
# PROJEKTY

Czyli:
👉 platforma do uruchamiania aplikacji
👉 monitoring
👉 CI/CD
👉 deploy pipeline
👉 dostęp do usług

---

# 1️⃣ 3 przykłady projektów platformowych (dla Ciebie idealne)

## 1️⃣ Projekt 1 — Platforma aplikacyjna na K8s

### Co to:

* aplikacja w kontenerze
* deployment
* service
* ingress
* TLS
* config
* scaling

Czyli pełny runtime.

### Technicznie:

* Dockerfile
* Kubernetes manifests
* Helm chart
* Ingress + cert-manager

### Efekt:

„Platforma do uruchamiania aplikacji web na Kubernetes”

---

## 2️⃣ Projekt 2 — Monitoring klastra

### Co to:

* Prometheus
* Grafana
* metryki node/pod
* dashboard
* alert

### Technicznie:

* kube-prometheus-stack (Helm)
* dashboard
* alert rule

### Efekt:

„Monitoring Kubernetes cluster z alertami”

---

## 3️⃣ Projekt 3 — CI/CD do K8s

### Co to:

* repo aplikacji
* build image
* push registry
* deploy do K8s

### Technicznie:

* GitHub Actions / GitLab CI
* docker build
* kubectl apply / helm upgrade

### Efekt:

„Pipeline deployujący aplikację do Kubernetes”

---

# 2️⃣ Dlaczego to działa w CV

Rekruter widzi:

✔ Docker
✔ Kubernetes
✔ Helm
✔ CI/CD
✔ monitoring

Czyli DevOps.

---

Krótka odpowiedź: **zaczynamy już projekt 1 i uczymy się K8s w jego trakcie.**
To najefektywniejsza ścieżka przy Twoim profilu (Linux/Ansible/containers).
Projekt 1 pokryje ~70% podstaw Kubernetes wymaganych na start.

Poniżej masz **pełny plan: start → realizacja → zakończenie**, w kolejności nauki.

---

# 3️⃣ Czy najpierw nauka K8s czy projekt?

Dla Ciebie właściwa kolejność:

1. minimalne podstawy (2–3 dni)
2. projekt runtime (główna nauka)
3. utrwalenie + CV

Nie robimy „teorii K8s miesiącami”.
Uczysz się przez budowę platformy.

---

# 4️⃣ Projekt 1 — plan krok po kroku

---

## 4️⃣ ETAP 0 — środowisko (start projektu)

Cel

Mieć lokalny klaster i narzędzia.

Kroki

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

## 5️⃣ ETAP 1 — aplikacja w kontenerze

Cel

Mieć prostą aplikację web.

Kroki

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

## 6️⃣ ETAP 2 — Deployment (pierwszy K8s runtime)

Cel

Uruchomić aplikację w K8s.

Kroki

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

## 7️⃣ ETAP 3 — Service (sieć w klastrze)

Cel

Umożliwić dostęp do aplikacji.

Kroki

1. service.yaml
2. ClusterIP
3. port mapping
4. test curl z pod

Nauka

* service
* cluster networking
* DNS w K8s

---

## 8️⃣ ETAP 4 — Ingress (dostęp z zewnątrz)

Cel

Wejście HTTP do aplikacji.

Kroki

1. install ingress controller
2. ingress.yaml
3. host/path routing
4. test w przeglądarce

Nauka

* ingress
* L7 routing
* exposure

---

## 9️⃣ ETAP 5 — Config + Secret

Cel

Konfiguracja runtime.

Kroki

1. configmap.yaml
2. secret.yaml
3. env w pod
4. restart deploy

Nauka

* config externalization
* secrets

---

## 🔟 ETAP 6 — TLS (cert-manager)

Cel

HTTPS.

Kroki

1. install cert-manager
2. clusterissuer
3. annotate ingress
4. cert status

Nauka

* cert lifecycle
* TLS w K8s

---

## 1️⃣1️⃣ ETAP 7 — Scaling (HPA)

Cel

Autoscaling aplikacji.

Kroki

1. metrics-server
2. hpa.yaml
3. load test
4. observe scale

Nauka

* CPU metrics
* autoscaling

---

## 1️⃣2️⃣ ETAP 8 — Helm (final platform)

Cel

Zamienić runtime w chart.

Kroki

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

## 1️⃣3️⃣ ETAP 9 — Zakończenie projektu

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

# 1️⃣4️⃣ Co osiągasz po projekcie

Realnie umiesz:

* deploy app na K8s
* exposure HTTP
* TLS
* config
* scaling
* Helm

To = junior Kubernetes engineer.

---

# 1️⃣5️⃣ Odpowiedź na Twoje pytanie decyzyjne

Twoja sytuacja:

* Linux ✔
* Docker ✔
* infra ✔
* automatyzacja ✔

Brakuje tylko runtime K8s.

👉 więc: **wchodzimy w projekt 1 teraz**
i uczymy się K8s w jego trakcie.

To dokładnie ścieżka platform engineer.

