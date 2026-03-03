# 4️⃣ Projekt 1 — plan krok po kroku

---

## 4️⃣0️⃣ ETAP 0 — środowisko (start projektu)

Cel

Mieć lokalny klaster i narzędzia.

Cel

1. Zainstaluj:

   * kubectl
   * helm
   * kind **lub** k3d

2. Utwórz klaster:

```
kind create cluster
kubectl get nodes
```

Umiejętności

* czym jest cluster
* node
* kubectl

👉 To jest moment „start projektu”.

---

## 4️⃣1️⃣ ETAP 1 — aplikacja w kontenerze

Cel

Mieć prostą aplikację web.

Cel

1. Prosta aplikacja:

   * nginx **lub**
   * python flask hello

2. Dockerfile

3. docker build

4. docker run

5. test w przeglądarce

Umiejętności

* obraz
* port
* kontener runtime

---

## 4️⃣2️⃣ ETAP 2 — Deployment (pierwszy K8s runtime)

Cel

Uruchomić aplikację w K8s.

Cel

1. deployment.yaml
2. kubectl apply
3. kubectl get pods
4. kubectl logs
5. scale replicas

Nauka K8s

* pod
* deployment
* replica
* scheduler

---

## 4️⃣3️⃣ ETAP 3 — Service (sieć w klastrze)

Cel

Umożliwić dostęp do aplikacji.

Cel

1. service.yaml
2. ClusterIP
3. port mapping
4. test curl z pod

Nauka

* service
* cluster networking
* DNS w K8s

---

## 4️⃣4️⃣ ETAP 4 — Ingress (dostęp z zewnątrz)

Cel

Wejście HTTP do aplikacji.

Cel

1. install ingress controller
2. ingress.yaml
3. host/path routing
4. test w przeglądarce

Nauka

* ingress
* L7 routing
* exposure

---

## 4️⃣5️⃣ ETAP 5 — Config + Secret

Cel

Konfiguracja runtime.

Cel

1. configmap.yaml
2. secret.yaml
3. env w pod
4. restart deploy

Nauka

* config externalization
* secrets

---

## 4️⃣6️⃣ ETAP 6 — TLS (cert-manager)

Cel

HTTPS.

Cel

1. install cert-manager
2. clusterissuer
3. annotate ingress
4. cert status

Nauka

* cert lifecycle
* TLS w K8s

---

## 4️⃣7️⃣ ETAP 7 — Scaling (HPA)

Cel

Autoscaling aplikacji.

Cel

1. metrics-server
2. hpa.yaml
3. load test
4. observe scale

Nauka

* CPU metrics
* autoscaling

---

## 4️⃣8️⃣ ETAP 8 — Helm (final platform)

Cel

Zamienić runtime w chart.

Cel

1. helm create
2. przenieś manifests
3. values.yaml
4. helm install
5. helm upgrade

Nauka

* templating
* packaging
* platform reuse

---

## 4️⃣9️⃣ ETAP 9 — Zakończenie projektu

Gotowe repo:

```
app/
Dockerfile

k8s/
raw-manifests/

helm/app-runtime/

docs/
architecture.md
```

README:

* architektura
* diagram
* jak deploy
* features

---

# 5️⃣ Co osiągasz po projekcie

Realnie umiesz:

* deploy app na K8s
* exposure HTTP
* TLS
* config
* scaling
* Helm

To = junior Kubernetes engineer.
