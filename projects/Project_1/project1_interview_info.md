
# Jak opowiadać o projekcie Kubernetes na rozmowie

## 📑 Spis treści

* [1️⃣ Wersja 60 sekund](#1️⃣-wersja-60-sekund-zwięzła-konkretna)
* [2️⃣ Wersja techniczna (2–3 minuty)](#2️⃣-wersja-techniczna-2–3-minuty)
* [3️⃣ Jak dokładnie działa ruch?](#3️⃣-jeśli-padnie-pytanie-jak-dokładnie-działa-ruch)
* [4️⃣ Dlaczego Ingress, skoro był LoadBalancer?](#4️⃣-jeśli-zapytają-dlaczego-ingress-skoro-był-loadbalancer)
* [5️⃣ Co było najtrudniejsze?](#5️⃣-jeśli-zapytają-co-było-najtrudniejsze)
* [6️⃣ Czego się Pan nauczył?](#6️⃣-jeśli-zapytają-czego-się-pan-nauczył)
* [7️⃣ Networking – pytania głębsze](#7️⃣-jeśli-zapytają-o-networking-głębiej)
* [8️⃣ Czego NIE mówić](#8️⃣-czego-nie-mówić)

---

Poniżej masz gotowy, uporządkowany sposób tłumaczenia projektu na rozmowie — w 3 poziomach:

1. **wersja 60 sekund**
2. **wersja techniczna (głębsza)**
3. **wersja pod pytania pogłębiające**

Możesz to trenować jak odpowiedź na pytanie:

> „Proszę opowiedzieć o projekcie Kubernetes, który Pan zrobił.”

---

## 1️⃣ Wersja 60 sekund (zwięzła, konkretna)

> Zbudowałem w labie wielowęzłowy klaster Kubernetes z prywatnym registry.
> Wdrożyłem aplikację web jako Deployment z replikami i wystawiłem ją przez Service typu LoadBalancer przy użyciu MetalLB w sieci LAN.
> Następnie skonfigurowałem ingress-nginx, aby umożliwić routing po nazwie hosta.
> Zweryfikowałem pełny przepływ ruchu od klienta przez MetalLB, Ingress, Service aż do poda.
> Projekt pozwolił mi zrozumieć różnicę między L4 a L7 oraz sposób działania sieci w Kubernetes.

To jest wersja „bez lania wody”.

---

## 2️⃣ Wersja techniczna (2–3 minuty)

> Zbudowałem klaster kubeadm z control-plane i workerami.
> Własną aplikację nginx spakowałem jako obraz i wypchnąłem do prywatnego registry.
> W Kubernetes użyłem Deployment z dwiema replikami, aby zapewnić wysoką dostępność i self-healing.
>
> Następnie stworzyłem Service, który zapewnił stały punkt dostępu do dynamicznych podów oraz load balancing na poziomie L4.
>
> Ponieważ działałem w środowisku on-prem, użyłem MetalLB, aby zasymulować cloud LoadBalancer i przydzielić zewnętrzny adres IP w sieci LAN.
>
> Na końcu zainstalowałem ingress-nginx, który działa jako reverse proxy L7 i umożliwia routing HTTP po nazwie hosta.
>
> Dzięki temu uzyskałem pełny stack:
> klient → LoadBalancer → Ingress → Service → Pod.
>
> Projekt pozwolił mi praktycznie zrozumieć architekturę aplikacyjną Kubernetes i warstwy sieciowe.

---

## 3️⃣ Jeśli padnie pytanie: „Jak dokładnie działa ruch?”

Możesz odpowiedzieć:

> Gdy użytkownik wchodzi na app.lab.local, DNS wskazuje na IP przydzielone przez MetalLB.
> MetalLB ogłasza to IP w LAN przez ARP i przekazuje ruch do Service.
>
> Service używa kube-proxy i iptables, aby load-balansować ruch do jednego z podów.
>
> Jeśli używamy Ingress, ruch trafia najpierw do ingress-nginx, który analizuje nagłówek Host i kieruje żądanie do odpowiedniego Service.

To pokazuje, że rozumiesz mechanizm, nie tylko YAML.

---

## 4️⃣ Jeśli zapytają: „Dlaczego Ingress, skoro był LoadBalancer?”

Odpowiedź:

> LoadBalancer działa na L4 i kieruje ruch na port.
> Ingress działa na L7 i umożliwia routing po nazwie hosta oraz ścieżce URL.
>
> Dzięki Ingress mogę mieć wiele aplikacji pod jednym adresem IP i jednym punktem wejścia.

To jest bardzo dobre pytanie rekrutacyjne.

---

## 5️⃣ Jeśli zapytają: „Co było najtrudniejsze?”

Możesz powiedzieć:

> Najwięcej nauczyłem się podczas problemów z networkingiem i stabilnością workerów.
> Musiałem uprościć architekturę i zredukować zmienne środowiskowe.
>
> To pokazało mi, że w inżynierii czasem kluczowe jest uproszczenie systemu, a nie dokładanie kolejnych komponentów.

To pokazuje dojrzałość.

---

## 6️⃣ Jeśli zapytają: „Czego się Pan nauczył?”

Możesz odpowiedzieć:

> Zrozumiałem różnicę między:
>
> * Deployment a Service
> * L4 a L7
> * Pod IP a ClusterIP
> * LoadBalancer cloud vs on-prem
>
> I zobaczyłem w praktyce, jak Kubernetes utrzymuje desired state i samoleczy aplikację.

---

## 7️⃣ Jeśli zapytają o networking głębiej

Możesz powiedzieć:

> Każdy pod ma własne IP w sieci overlay zapewnianej przez CNI (w moim przypadku Calico).
> Service działa poprzez kube-proxy i iptables, tworząc reguły NAT.
>
> MetalLB ogłasza adres IP w warstwie 2 w LAN.
>
> Ingress działa jako reverse proxy L7.

To już brzmi jak ktoś, kto rozumie platformę.

---

## 8️⃣ Czego NIE mówić

Nie mów:

* „zainstalowałem YAML i działa”
* „to było z tutoriala”
* „nie wiem dokładnie jak działa networking”

Zamiast tego:

* skup się na przepływie ruchu
* skup się na warstwach
* skup się na decyzjach

---


