---

# Monitoring Kubernetes – Prometheus i Grafana

## 📑 Spis treści

* [1. Instalacja](#1-instalacja)
* [2. Architektura monitoringu](#2-architektura-monitoringu)
* [3. Metryki](#3-metryki)
* [4. PromQL](#4-promql)
* [5. Alerty](#5-alerty)
* [6. Co monitorujemy](#6-co-monitorujemy)
* [7. Najczęstsze pytania na rozmowie](#7-najczęstsze-pytania-na-rozmowie)
* [8. Przykładowe scenariusze](#8-przykładowe-scenariusze)
* [9. Podsumowanie](#9-podsumowanie)

---

# 1. Instalacja

Monitoring w Kubernetes najczęściej instaluje się przez **Helm** przy użyciu **kube-prometheus-stack**, który instaluje:

* **Prometheus**
* **Grafana**
* **Alertmanager**
* **Prometheus Operator**
* exporters i dashboardy.

---

## 1.1 Dodanie repo Helm

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

---

## 1.2 Utworzenie namespace

```bash
kubectl create namespace monitoring
```

---

## 1.3 Instalacja stacka

```bash
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring
```

Po instalacji powstają komponenty:

```bash
kubectl get pods -n monitoring
```

np.

```
prometheus
grafana
alertmanager
kube-state-metrics
node-exporter
operator
```

---

## 1.4 Sprawdzenie serwisów

```bash
kubectl get svc -n monitoring
```

---

## 1.5 Dostęp do Grafany

```bash
kubectl port-forward svc/monitoring-grafana -n monitoring 3000:80
```

login:

```
admin
```

hasło:

```bash
kubectl get secret monitoring-grafana -n monitoring -o jsonpath="{.data.admin-password}" | base64 --decode
```

---

## 1.6 Dostęp do Prometheusa

```bash
kubectl port-forward svc/monitoring-kube-prometheus-prometheus -n monitoring 9090
```

---

# 2. Architektura monitoringu

Typowy stack wygląda tak:

```
node-exporter
kube-state-metrics
        ↓
Prometheus
        ↓
Alertmanager
        ↓
Grafana
```

---

## 2.1 Prometheus

Prometheus:

* zbiera metryki
* zapisuje je w bazie time-series
* wykonuje zapytania PromQL
* generuje alerty.

Prometheus używa **pull model**:

```
Prometheus → HTTP request → /metrics
```

---

## 2.2 Grafana

Grafana:

* wizualizuje dane
* tworzy dashboardy
* wykonuje zapytania do Prometheus.

Grafana **nie zbiera metryk**.

---

## 2.3 Alertmanager

Alertmanager:

* odbiera alerty z Prometheus
* grupuje je
* deduplikuje
* wysyła powiadomienia.

np.

```
Slack
Email
PagerDuty
Teams
```

---

## 2.4 Exportery

Exportery udostępniają metryki dla Prometheus.

Przykłady:

* node-exporter
* mysql-exporter
* redis-exporter
* blackbox-exporter.

---

# 3. Metryki

Metryka to wartość liczbowa opisująca stan systemu.

Przykład:

```
node_cpu_seconds_total{instance="192.168.0.100",mode="idle"} 92341
```

Elementy metryki:

* nazwa
* labels
* wartość
* timestamp.

---

## 3.1 Typy metryk

### Counter

licznik rosnący

```
http_requests_total
```

---

### Gauge

wartość rosnąca i malejąca

```
memory_usage_bytes
```

---

### Histogram

rozkład wartości

np. latency.

---

### Summary

percentyle.

---

# 4. PromQL

PromQL to język zapytań Prometheus.

---

## 4.1 Sprawdzenie statusu targetów

```
up
```

wynik:

```
1 → działa
0 → nie działa
```

---

## 4.2 CPU node

```
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

---

## 4.3 restart podów

```
kube_pod_container_status_restarts_total
```

---

## 4.4 CPU podów

```
sum(rate(container_cpu_usage_seconds_total[5m])) by (pod)
```

---

# 5. Alerty

Alerty definiuje się jako reguły.

W Kubernetes używa się obiektu:

```
PrometheusRule
```

---

## 5.1 Przykład alertu

```
NodeDown
```

```
up{job="node-exporter"} == 0
```

---

## 5.2 Przepływ alertu

```
Prometheus
     ↓
Alertmanager
     ↓
Slack / email / Teams
```

---

# 6. Co monitorujemy

### infrastruktura

```
CPU
RAM
disk
network
```

---

### Kubernetes

```
node status
pod status
replicas
restarts
```

---

### aplikacje

```
request rate
latency
error rate
```

---

# 7. Najczęstsze pytania na rozmowie

## Co to jest Prometheus

System monitoringu zbierający metryki w modelu time-series.

---

## Co to jest Grafana

Narzędzie wizualizacji danych.

---

## Jak Prometheus zbiera metryki

Pull model:

```
Prometheus → /metrics endpoint
```

---

## Co to jest exporter

Program udostępniający metryki Prometheus.

---

## Jak wygląda stack monitoringu Kubernetes

```
node-exporter
kube-state-metrics
Prometheus
Alertmanager
Grafana
```

---

## Co robi Alertmanager

* routing alertów
* deduplikacja
* powiadomienia.

---

## Co to jest PromQL

Język zapytań Prometheus.

---

## Jak sprawdzić czy node działa

```
up
```

---

## Co oznacza up=0

Prometheus nie może pobrać metryk z targetu.

---

# 8. Przykładowe scenariusze

### Node ma wysokie CPU

sprawdzam:

```
node_cpu_seconds_total
```

---

### Pod restartuje się

sprawdzam:

```
kube_pod_container_status_restarts_total
```

---

### Node nie odpowiada

alert:

```
NodeDown
```

---

# 9. Podsumowanie

Prometheus:

```
zbiera metryki
przechowuje dane
wykonuje zapytania
generuje alerty
```

Grafana:

```
tworzy dashboardy
wizualizuje dane
```

Alertmanager:

```
wysyła alerty
```

---

