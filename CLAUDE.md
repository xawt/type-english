# type-english

Quiz tłumaczenia PL → EN, styl terminala. Cała apka to jeden plik `index.html` (inline CSS + JS, bez frameworków i bez builda) + `questions.json`.

## Pliki
- `index.html` — UI i logika. Zawiera też `SAMPLE` (zapasowy zestaw, gdy nic innego się nie wczyta).
- `questions.json` — domyślny zestaw pytań.
- `.github/workflows/pages.yml` — deploy na GitHub Pages przy każdym pushu na `main` (kopiuje `index.html` i `*.json` do `_site`).

## Format pytań
`{ "group1": { "count", "points", "questions": [...] }, "group2": {...} }`, element: `{ "pl": "...", "en": "..." | ["...", "..."] }`. Grupa może być też samą tablicą (stary format) — wtedy `DEFAULTS`. `validate()` normalizuje do formy obiektowej. Pełny opis formatu w README.

## Logika
- Runda: `count` pytań z każdej grupy, każde warte `points` grupy (domyślnie `DEFAULTS`: 10 × 1 + 1 × 2). Mnożnik wg złych prób w `SCALE` (1 / 0.5 / 0.25 / 0 od 3 złych), podpowiedź (przycisk albo Tab) ucina wpis na pierwszym błędzie i dopisuje 3 kolejne znaki, najdalej do 2/3 odpowiedzi; każda mnoży punkty × 0.5 (`HINT_*`), „pokaż odpowiedź” = 0. Maks. = suma `points` wylosowanych pytań.
- Timer: `T_LIMIT = 20` s na pytanie, po czasie mnożnik `T_MULT = 0.5` (mnożony z `SCALE`). Pierwsze pytanie rundy czeka na pierwszą literę (`waitStart`), kolejne liczą od `show()`. Przełącznik `#timerOn`, stan w localStorage `timer` (domyślnie włączony).
- `variants()` rozwija nawiasy w `en` na warianty (z tekstem i bez; pierwszy = pełny, używany w podpowiedzi). `accepted()` = unikalne warianty po `norm()`.
- Odpowiedzi normalizowane w `norm()` (małe litery, apostrofy, końcowa interpunkcja, spacje).
- Kolejność źródeł pytań: `?set=nazwa` (`nazwa.json`) > zestaw w localStorage (`q`, nazwa w `qname`) > `questions.json` > `SAMPLE`.
- Log terminala: każde pytanie ma swoją grupę `.qgrp`; po zaliczeniu dostaje `.done` (przygaszenie).
- Historia wyników tylko w pamięci (`history`), od otwarcia strony — celowo nie w localStorage.

## Konwencje
- UI i komentarze po polsku, małymi literami w stylu terminala.
- Kolory jako tokeny w `:root`; czcionka JetBrains Mono.
- Mobile-first: 16px boczny margines, bez poziomego scrolla.
- Okno terminala (`.body`) ma stałą wysokość — nie może zmieniać rozmiaru w trakcie gry; nowe linie przewijają się w `.log`.
- Wszystkie dostępy do localStorage w try/catch.
- Commity atomowe (jedna logiczna zmiana na commit), bez linii `Co-Authored-By`.
- Workflow: gałąź → PR do `main` → merge. Bez commitów bezpośrednio na `main` (merge = deploy na Pages).

## Uruchomienie
`python3 -m http.server` w katalogu, potem http://localhost:8000.

## Testy
Każdą zmianę funkcjonalności sprawdzaj w headless Chromium (z Playwrighta: `~/.cache/ms-playwright/chromium-*/chrome-linux*/chrome`), nie tylko `node --check`.
- Skopiuj `index.html` do katalogu tymczasowego, dopisz przed `</body>` `<script src="t.js"></script>` ze scenariuszem testu.
- Globalne `let`/funkcje z `index.html` są widoczne w `t.js`: ustaw `round`/`i`, wywołaj `show()`, `hint()`, `check({preventDefault(){}})`, `$("skip").click()` itd. Odczekaj start (`setTimeout` ~500 ms), a po poprawnej odpowiedzi 700 ms do następnego pytania.
- Wyniki zapisz do `document.body` i odczytaj z DOM:
  `chrome --headless --no-sandbox --virtual-time-budget=4000 --dump-dom file:///…/i.html`
- Z `file://` nie wczyta się `questions.json` — startuje `SAMPLE`, co do testów wystarcza.
