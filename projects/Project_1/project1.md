Poniżej masz **ten sam tekst**, bez zmiany treści — dodałem jedynie uporządkowaną strukturę Markdown (spis treści + kotwice + logiczne nagłówki), żebyś mógł łatwo nawigować w swojej Wiki.

---

# Projekt: Platforma aplikacyjna na Kubernetes (Lab LAN)

## 📑 Spis treści

* [1. Cel projektu](#1-cel-projektu)
* [2. Architektura środowiska](#2-architektura-środowiska)
* [3. Etap 1 – Kontenery i prywatny registry](#3-etap-1--kontenery-i-prywatny-registry)
* [4. Etap 2 – Deployment (Desired State)](#4-etap-2--deployment-desired-state)
* [5. Etap 3 – Service (L4)](#5-etap-3--service-l4)
* [6. Etap 4 – MetalLB (LoadBalancer w LAN)](#6-etap-4--metallb-loadbalancer-w-lan)
* [7. Etap 5 – Ingress (L7)](#7-etap-5--ingress-l7)
* [8. Pełny przepływ ruchu](#8-pełny-przepływ-ruchu)
* [9. Kubernetes Networking – teoria w skrócie](#9-kubernetes-networking--teoria-w-skrócie)
* [10. Decyzje inżynierskie](#10-decyzje-inżynierskie)
* [11. Co osiągnąłeś](#11-co-osiągnąłeś)
* [12. CV Summary](#12-cv-summary)

---

## 1. Cel projektu

Celem projektu było zbudowanie kompletnej platformy do uruchamiania aplikacji web w Kubernetes — symulującej środowisko chmurowe w warunkach on-prem (LAN).

Projekt obejmuje:

* budowę obrazu aplikacji
* Deployment z replikacją
* Service (L4 load balancing)
* LoadBalancer (MetalLB)
* Ingress (L7 routing)

Nie był to projekt „Hello World”, lecz budowa pełnego runtime dla aplikacji.

---

## 2. Architektura środowiska

### Finalny układ

* rocky-ansible → control-plane + Ansible
* ubuntu-node → worker
* ubuntu-node1 → worker
* rocky-registry → prywatny registry

Sieć LAN: `192.168.0.0/24`
MetalLB IP pool: `192.168.0.240–250`

---

## 3. Etap 1 – Kontenery i prywatny registry

### Co zrobiliśmy

Zbudowaliśmy własny obraz nginx:

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

Zbudowanie i push:

```bash
docker build -t 192.168.0.75:5000/k8s-runtime:2.3 .
docker push 192.168.0.75:5000/k8s-runtime:2.3
```

### Teoria

Kubernetes nie uruchamia aplikacji — uruchamia kontenery.

Obraz:

* jest niezmienny (immutable)
* zawiera kod aplikacji
* jest deployowany wielokrotnie

Zasada:

> Build once, run many times.

---

## 4. Etap 2 – Deployment (Desired State)

### deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: k8s-runtime
spec:
  replicas: 2
  selector:
    matchLabels:
      app: k8s-runtime
  template:
    metadata:
      labels:
        app: k8s-runtime
    spec:
      containers:
      - name: web
        image: 192.168.0.75:5000/k8s-runtime:2.3
        ports:
        - containerPort: 80
```

### Teoria

Deployment:

* utrzymuje określoną liczbę replik
* zarządza ReplicaSet
* restartuje pody
* pozwala robić rolling update

Kubernetes działa deklaratywnie:

> „Chcę 2 instancje aplikacji”

System sam utrzymuje ten stan.

---

## 5. Etap 3 – Service (L4)

### service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: k8s-runtime-svc
spec:
  type: LoadBalancer
  selector:
    app: k8s-runtime
  ports:
  - port: 80
    targetPort: 80
```

### Teoria

Problem:

* Pody mają dynamiczne IP
* Mogą być usuwane

Service:

* daje stały adres (ClusterIP)
* load-balansuje ruch
* działa na L4 (TCP)

Service nie wie nic o HTTP.
Przekazuje ruch na poziomie portów.

---

## 6. Etap 4 – MetalLB (LoadBalancer w LAN)

### Problem

W chmurze:

* Service typu LoadBalancer dostaje publiczne IP

W on-prem:

* Kubernetes nie ma integracji z load balancerem

### Rozwiązanie

MetalLB:

* ogłasza IP w sieci LAN (ARP)
* symuluje cloud LoadBalancer
* przydziela External IP dla Service

Konfiguracja IP pool:

```yaml
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: default-pool
  namespace: metallb-system
spec:
  addresses:
  - 192.168.0.240-192.168.0.250
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: default
  namespace: metallb-system
```

Efekt:
Service otrzymał IP: `192.168.0.241`

To jest warstwa L4.

---

## 7. Etap 5 – Ingress (L7)

### Instalacja ingress-nginx

Ingress Controller to zwykły pod nginx, który:

* czyta obiekty Ingress
* generuje konfigurację
* wykonuje routing HTTP

### ingress.yaml

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: k8s-runtime-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: app.lab.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: k8s-runtime-svc
            port:
              number: 80
```

Efekt:
Wejście przez nazwę hosta:
`http://app.lab.local`

---

## 8. Pełny przepływ ruchu

### Diagram

```
[Laptop]
    |
    v
192.168.0.241  (MetalLB IP)
    |
    v
Ingress Controller (nginx L7)
    |
    v
Service (ClusterIP L4)
    |
    +----> Pod 1
    |
    +----> Pod 2
```

---

## 9. Kubernetes Networking – teoria w skrócie

### 1. Pod Network

Każdy pod:

* ma własne IP
* jest routowalny w klastrze

CNI (u Ciebie Calico):

* zapewnia overlay network
* łączy pody między node'ami

---

### 2. Service Network

Service:

* dostaje ClusterIP
* używa kube-proxy
* load-balansuje ruch między podami

Service działa w oparciu o iptables / IPVS.

---

### 3. LoadBalancer (MetalLB)

* działa poza klastrem
* ogłasza IP w LAN
* kieruje ruch do Service

---

### 4. Ingress (L7)

Ingress:

* analizuje nagłówki HTTP
* może routować po:

  * host
  * ścieżce
* umożliwia TLS termination

---

### Warstwy OSI

* MetalLB → L2/L4
* Service → L4
* Ingress → L7

Masz pełny stack sieciowy.

---

## 10. Decyzje inżynierskie

Podczas projektu:

* Rocky workers były niestabilne
* uprościliśmy architekturę
* zostawiliśmy 2 Ubuntu workerów
* ograniczyliśmy zmienne

Wniosek:

> Stabilność środowiska jest ważniejsza niż różnorodność systemów.

---

## 11. Co osiągnąłeś

Po tym projekcie rozumiesz:

* Desired state w Kubernetes
* Replikację i self-healing
* L4 vs L7
* Service abstraction
* LoadBalancer w on-prem
* Host-based routing
* Pod-to-pod networking

To jest poziom junior platform engineer / DevOps.

---

## 12. CV Summary

### Wersja techniczna

> Designed and implemented a multi-node Kubernetes lab environment with private container registry, MetalLB-based LoadBalancer and nginx Ingress Controller.
> Deployed containerized application using Deployment with replicas and exposed it via L4 (Service + MetalLB) and L7 (Ingress) networking layers.
> Implemented host-based routing and validated full request flow from client to pod across cluster nodes.

### Wersja skrócona

> Built a Kubernetes-based application runtime platform including Deployment, Service, MetalLB LoadBalancer and nginx Ingress with host-based routing in a multi-node cluster.

---

Jeśli chcesz, mogę teraz:

* zrobić wersję „enterprise-style wiki” (bardziej formalna dokumentacja),
* rozbić to na osobne pliki `.md`,
* albo przygotować wersję pod GitHub README.

