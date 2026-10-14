# type-english

Quiz tłumaczenia PL → EN w stylu terminala. Statyczna strona, bez builda.

**Online:** https://xawt.github.io/type-english/

## Funkcje
- Runda: losowe pytania z `group1` i bonusowe z `group2` — liczbę pytań i punkty ustawiasz w pliku pytań (domyślnie 10 × 1 pkt + 1 × 2 pkt).
- Zła odpowiedź podświetla pierwszą błędną literę; każda zła próba zmniejsza punkty za pytanie (100% → 50% → 25% → 0).
- Zaliczone pytania przygasają w logu.
- Timer (przełącznik, domyślnie włączony): 20 s na pytanie. Na pierwszym pytaniu rundy odliczanie rusza od pierwszej wpisanej litery, na kolejnych od razu. Po czasie można dalej odpowiadać, ale za 50% punktów. Ustawienie zapamiętywane na tym urządzeniu.
- Historia wyników od otwarcia strony (godzina, czas rundy, wynik, zestaw).
- Własny zestaw pytań: wgraj plik JSON lub wklej go w sekcji „zestaw pytań (JSON)” — zapamiętywany na tym urządzeniu.

## Format pliku pytań
Plik JSON z dwiema grupami: `group1` (zwykłe pytania) i `group2` (pytania bonusowe, oznaczone ★).
Dla każdej grupy ustawiasz, ile pytań losować do rundy i ile punktów daje odpowiedź.

```json
{
  "group1": {
    "count": 10,
    "points": 1,
    "questions": [
      { "pl": "pies", "en": "dog" },
      { "pl": "przyjaciel", "en": ["friend", "mate"] }
    ]
  },
  "group2": {
    "count": 1,
    "points": 2,
    "questions": [
      { "pl": "Mam na imię Ola.", "en": ["my name is ola", "i am ola", "i'm ola"] },
      { "pl": "Lubię lody.", "en": "i like ice cream" }
    ]
  }
}
```

| Pole | Znaczenie | Domyślnie |
|---|---|---|
| `count` | ile pytań z grupy losować do jednej rundy (liczba całkowita ≥ 0; gdy pytań jest mniej, bierze wszystkie) | `group1`: 10, `group2`: 1 |
| `points` | punkty za poprawną odpowiedź za pierwszym razem, bez podpowiedzi (liczba > 0, może być ułamkowa) | `group1`: 1, `group2`: 2 |
| `questions` | lista pytań, co najmniej 1 | — |
| `pl` | tekst pytania (po polsku) | — |
| `en` | poprawna odpowiedź: tekst albo lista akceptowanych wariantów | — |

- Tekst w nawiasach w `en` jest opcjonalny: `"enter (your PIN number)"` zalicza `enter your pin number` i `enter`, a `"(a) broken-down (machine)"` każdą kombinację z nawiasami i bez. Podpowiedź pokazuje pełną wersję.
- `count` i `points` można pominąć — wtedy obowiązują wartości domyślne.
- Grupa może być też samą listą pytań (starszy format): `"group1": [ { "pl": "pies", "en": "dog" } ]`.
- `count: 0` wyłącza grupę z rundy, ale łącznie runda musi mieć co najmniej 1 pytanie.

### Punktacja
Punkty za pytanie zależą od liczby złych prób przed poprawną odpowiedzią:

| Złe próby | Punkty | Przykład dla `points: 1` |
|---|---|---|
| 0 | 100% `points` | 1 |
| 1 | 50% `points` | 0.5 |
| 2 | 25% `points` | 0.25 |
| 3 i więcej | 0 | 0 |

- Podpowiedź ogranicza wynik pytania do najwyżej 50% `points`.
- „Pokaż odpowiedź” daje 0 pkt.
- Przy włączonym timerze odpowiedź po upływie 20 s daje połowę tego, co wynika z powyższych zasad (np. 1 zła próba + koniec czasu = 25% `points`).
- Maksymalny wynik rundy to suma `points` wylosowanych pytań.

### Sprawdzanie odpowiedzi
Wielkość liter, końcowe `. ! ? ,`, nadmiarowe spacje są ignorowane, a apostrofy `’` i `` ` `` traktowane jak `'`.

## Inne zestawy w repo
Dodaj `nazwa.json` obok `index.html` i otwórz `…/type-english/?set=nazwa`.

## Lokalnie
```sh
python3 -m http.server
```
