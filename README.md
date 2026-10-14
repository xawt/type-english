# type-english

Quiz tłumaczenia PL → EN w stylu terminala. Statyczna strona, bez builda.

**Online:** https://xawt.github.io/type-english/

## Funkcje
- Runda: 10 losowych słów z `group1` + 1 zdanie bonusowe z `group2` (2 pkt).
- Zła odpowiedź podświetla pierwszą błędną literę; podpowiedź / pokazanie odpowiedzi daje połowę punktów.
- Zaliczone pytania przygasają w logu.
- Historia wyników od otwarcia strony (godzina, czas rundy, wynik, zestaw).
- Własny zestaw pytań: wgraj plik JSON lub wklej go w sekcji „zestaw pytań (JSON)” — zapamiętywany na tym urządzeniu.

## Format pliku pytań
```json
{
  "group1": [ { "pl": "pies", "en": "dog" }, { "pl": "przyjaciel", "en": ["friend", "mate"] } ],
  "group2": [ { "pl": "Lubię lody.", "en": "i like ice cream" } ]
}
```
`en` może być tekstem lub listą akceptowanych odpowiedzi. Wielkość liter i końcowa interpunkcja są ignorowane.

## Inne zestawy w repo
Dodaj `nazwa.json` obok `index.html` i otwórz `…/type-english/?set=nazwa`.

## Lokalnie
```sh
python3 -m http.server
```
