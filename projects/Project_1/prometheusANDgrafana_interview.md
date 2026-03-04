
# Pytania rekrutacyjne – Prometheus i Grafana

## 📑 Spis treści

* [1. Podstawowe pytania](#1-podstawowe-pytania-bardzo-częste)
* [2. Pytania o architekturę](#2-pytania-o-architekturę)
* [3. Pytania o Kubernetes](#3-pytania-o-kubernetes)
* [4. Pytania o alerty](#4-pytania-o-alerty)
* [5. Pytania praktyczne](#5-pytania-praktyczne-częste)
* [6. Pytania scenariuszowe](#6-pytania-scenariuszowe)
* [7. Pytania o różnice](#7-pytania-o-różnice)
* [8. Pull vs Push model](#8-pytanie-które-często-pada)

---

# 1. Podstawowe pytania (bardzo częste)

**Co to jest Prometheus?**
System monitoringu zbierający metryki w modelu time-series.

**Jak działa Prometheus?**

Mechanizm:

```
targets → /metrics endpoint → scrape → TSDB → PromQL → alerty
```

czyli:

* Prometheus cyklicznie pobiera metryki
* zapisuje je w bazie danych
* pozwala wykonywać zapytania PromQL.

---

**Co to jest Grafana?**

Narzędzie wizualizacji danych, które pobiera dane z Prometheusa i tworzy dashboardy.

---

# 2. Pytania o architekturę

Często pojawia się pytanie:

**Jak wygląda stack monitoringu Kubernetes?**

Typowa odpowiedź:

```
node-exporter        → metryki systemu
kube-state-metrics   → metryki obiektów K8s
Prometheus           → zbieranie metryk
Alertmanager         → obsługa alertów
Grafana              → dashboardy
```

---

**Jak Prometheus zbiera metryki?**

Metoda:

```
HTTP pull model
```

czyli Prometheus sam odpytuje endpoint:

```
/metrics
```

---

# 3. Pytania o Kubernetes

To bardzo częsty temat.

**Jak Prometheus znajduje targety w Kubernetes?**

Mechanizmy discovery:

```
ServiceMonitor
PodMonitor
Endpoints
```

Te obiekty są zarządzane przez **Prometheus Operator**.

---

**Co to jest exporter?**

Program eksportujący metryki w formacie Prometheus.

Przykłady:

```
node-exporter
mysql-exporter
redis-exporter
blackbox-exporter
```

---

# 4. Pytania o alerty

**Jak tworzy się alert w Prometheus?**

Przez regułę:

```
PrometheusRule
```

z wyrażeniem PromQL.

Przykład:

```
up == 0
```

Alerty są wysyłane do:

**Alertmanager**

---

**Co robi Alertmanager?**

* deduplikacja alertów
* grupowanie
* routing
* wysyłanie powiadomień.

---

# 5. Pytania praktyczne (częste)

**Jak sprawdzić czy node działa w Prometheus?**

Zapytanie:

```
up
```

---

**Jak sprawdzić użycie CPU node?**

PromQL:

```
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

---

**Jak sprawdzić restart podów?**

```
kube_pod_container_status_restarts_total
```

---

# 6. Pytania scenariuszowe

Rekruter może zapytać:

**Pod jest w CrashLoopBackOff – jak to sprawdzisz w monitoringu?**

Odpowiedź:

* sprawdzam metrykę restartów
* sprawdzam alert `KubePodCrashLooping`
* analizuję dashboard w Grafanie.

---

**Node ma wysokie CPU – co robisz?**

* sprawdzam CPU node
* sprawdzam CPU podów
* identyfikuję aplikację generującą load.

---

# 7. Pytania o różnice

Częste pytanie:

**Prometheus vs Grafana**

Prometheus:

```
zbiera dane
przechowuje dane
wykonuje zapytania
generuje alerty
```

Grafana:

```
wizualizuje dane
tworzy dashboardy
```

---

# 8. Pytanie które często pada

**Pull vs Push model**

Prometheus używa:

```
pull
```

czyli sam pobiera metryki.

Wyjątek:

```
Pushgateway
```

dla krótkich jobów.

---

