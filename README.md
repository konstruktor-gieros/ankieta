# Ankieta GIEROS — Potwierdzenie wymiarów projektu

System slajdów do akceptacji rysunków technicznych przez inwestora.

## Struktura folderu

```
Ankieta/
├── index.html          # publiczna ankieta (klient klika "Dalej")
├── admin.html          # UKRYTY moduł do tworzenia folderów klientów
├── README.md
├── klienci/
│   ├── kowalski_ul-sloneczna-12-warszawa/
│   │   ├── manifest.json
│   │   ├── 01-rzut-parteru.pdf
│   │   ├── 02-elewacja-pn.pdf
│   │   └── 03-przekroj.pdf
│   └── …
└── potwierdzenie-wymiarow.html   # stara wersja (do usunięcia)
```

## Workflow tworzenia nowej ankiety

1. **Otwórz `admin.html` lokalnie** w Chrome lub Edge (File System Access API
   wymaga Chromium).
2. **Wpisz dane klienta** — inwestor (imię i nazwisko) + adres budowy.
   Slug folderu generuje się automatycznie:
   `Jan Kowalski` + `ul. Słoneczna 12, Warszawa`
   → `jan-kowalski_ul-sloneczna-12-warszawa`
3. **Dodaj slajdy** — przycisk „+ Dodaj slajd":
   - Tytuł rysunku (np. „Rzut parteru")
   - Opis dla klienta (co ma sprawdzić)
   - Plik PDF z dysku
4. **Kliknij „💾 Zapisz do folderu klienci/"** — przeglądarka pokaże
   selektor folderu. Wskaż `Ankieta/klienci/` w swoim lokalnym repo.
   Admin utworzy podfolder klienta z PDF-ami i manifestem.
5. **W terminalu wykonaj push:**
   ```bash
   git add Ankieta/klienci/<slug>
   git commit -m "ankieta: <slug>"
   git push
   ```
6. **Wyślij klientowi link**:
   ```
   https://<twoj-user>.github.io/<repo>/Ankieta/index.html?klient=<slug>
   ```
   Przycisk „🔗 Pokaż link" kopiuje go do schowka.

## Co widzi klient

1. Slajd powitalny z danymi projektu.
2. Slajd po slajdzie — każdy rysunek z opisem + checkbox „Akceptuję" +
   pole na uwagi.
3. Slajd końcowy — podsumowanie akceptacji + pola podpisu + przycisk
   **„📧 Wyślij potwierdzenie"**.
4. Mail leci na `FORMSUBMIT_EMAIL` (zmień w `index.html`) z pełnym
   raportem (kto akceptuje co, jakie uwagi).

## Konfiguracja

**W `index.html`** zmień jedną linijkę:
```js
var FORMSUBMIT_EMAIL = 'biuro@gieros.pl';
```

Pierwsza submisja wymaga jednorazowej aktywacji — FormSubmit przyśle
na ten adres link „Confirm your email". Klikasz raz i działa.

## Dlaczego admin.html jest „ukryty"

- Nie ma do niego linku z `index.html`.
- Klient nie zna URL `…/admin.html`.
- Możesz dodać `noindex` w meta lub `.htaccess` jeśli chcesz.

## Manifest format

`klienci/<slug>/manifest.json`:
```json
{
  "wersja": 1,
  "slug": "jan-kowalski_ul-sloneczna-12-warszawa",
  "klient": {
    "numerKlienta": "K-2026-014",
    "numerDokumentu": "PWB-2026-014/01",
    "inwestor": "Jan Kowalski",
    "adresBudowy": "ul. Słoneczna 12, Warszawa",
    "data": "2026-05-14",
    "osobaPrzygotowujaca": "Wojciech Wojtach"
  },
  "slajdy": [
    {
      "id": "slide-…",
      "tytul": "Rzut parteru",
      "opis": "Proszę zweryfikować rozkład pomieszczeń…",
      "plik": "01-rzut-parteru.pdf"
    }
  ],
  "utworzono": "2026-05-14T10:30:00Z"
}
```

## Wsparcie przeglądarek

- **index.html** (klient): wszystkie przeglądarki z PDF embed (Chrome,
  Firefox, Safari, Edge). Mobile (Android Chrome, iOS Safari) działa.
- **admin.html** (Ty): **tylko Chromium** (Chrome, Edge) — File System
  Access API. Fallback „Pobierz jako ZIP" dla innych (ale wymaga ręcznego
  wrzucania plików).
