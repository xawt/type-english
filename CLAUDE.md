# type-english

Quiz tłumaczenia PL → EN, styl terminala. Cała apka to jeden plik `index.html` (inline CSS + JS, bez frameworków i bez builda) + `questions.json`.

## Pliki
- `index.html` — UI i logika. Zawiera też `SAMPLE` (zapasowy zestaw, gdy nic innego się nie wczyta).
- `questions.json` — domyślny zestaw pytań.
- `.github/workflows/pages.yml` — deploy na GitHub Pages przy każdym pushu na `main` (kopiuje `index.html` i `*.json` do `_site`).

## Format pytań
`{ "group1": [...], "group2": [...] }`, element: `{ "pl": "...", "en": "..." | ["...", "..."] }`. Walidacja w `validate()`.

## Logika
- Runda: `N1 = 10` z group1 + `N2 = 1` bonus z group2 (2 pkt). Podpowiedź lub druga próba = połowa punktów.
- Odpowiedzi normalizowane w `norm()` (małe litery, apostrofy, końcowa interpunkcja, spacje).
- Kolejność źródeł pytań: `?set=nazwa` (`nazwa.json`) > zestaw w localStorage (`q`, nazwa w `qname`) > `questions.json` > `SAMPLE`.
- Log terminala: każde pytanie ma swoją grupę `.qgrp`; po zaliczeniu dostaje `.done` (przygaszenie).
- Historia wyników tylko w pamięci (`history`), od otwarcia strony — celowo nie w localStorage.

## Konwencje
- UI i komentarze po polsku, małymi literami w stylu terminala.
- Kolory jako tokeny w `:root`; czcionka JetBrains Mono.
- Mobile-first: 16px boczny margines, bez poziomego scrolla.
- Wszystkie dostępy do localStorage w try/catch.
- Commity atomowe (jedna logiczna zmiana na commit), bez linii `Co-Authored-By`.
- Workflow: gałąź → PR do `main` → merge. Bez commitów bezpośrednio na `main` (merge = deploy na Pages).

## Uruchomienie
`python3 -m http.server` w katalogu, potem http://localhost:8000.
