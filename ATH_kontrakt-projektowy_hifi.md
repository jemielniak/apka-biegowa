# Kontrakt projektowy hi-fi grupy ATH (Blok startowy)

Wklej ten plik na początku nowego okna razem z `ATH_spec.md`, `ATH_szkice_lo-fi.md`
i `manifest_ekranow_v3.csv`. Pilnuje, żeby kolejny ekran wyglądał i działał jak
ATH-08, ATH-09 i ATH-10, zamiast dryfować w inną estetykę.

## Kierunek wizualny

Chronograf dnia startu. Ciemny ink jako tło panelu, akcent w kolorze electric lime,
kondensowany krój nagłówkowy i cyfry tabelaryczne w numerach, licznikach i kwotach.
Numer startowy ma formę tabliczki biegowej. Bez fioletowych gradientów na bieli,
bez krojów systemowych.

## Para krojów (Google Fonts, polskie diakrytyki)

- nagłówkowy: Saira Condensed (500–800)
- tekstowy: Barlow (400–700, plus italic 500)

```html
<link href="https://fonts.googleapis.com/css2?family=Saira+Condensed:wght@500;600;700;800&family=Barlow:ital,wght@0,400;0,500;0,600;0,700;1,500&display=swap" rel="stylesheet">
```

## Tokeny marki - jedyne miejsce do przemotywowania

Skopiuj ten blok `:root` bez zmian. Przemotywowanie marki organizatora to podmiana
`--brand-ink` i `--accent` oraz kolorów semantycznych, nic poza tym.

```css
:root{
  --brand-ink:#10141b;
  --accent:#d6f622;

  --color-success:#3ddc84;
  --color-warning:#ffb84d;
  --color-error:#ff6b5e;
  --color-info:#6aa9ff;

  --surface-1:#181e28;
  --surface-2:#1f2733;
  --surface-3:#283241;
  --hairline:#313c4c;
  --text-strong:#f2f5f7;
  --text:#cdd5df;
  --text-muted:#8c97a6;
  --on-accent:#10141b;
  --paper:#f4f5ef;

  --font-display:"Saira Condensed", system-ui;
  --font-body:"Barlow", system-ui;

  --u:4px;
  --s1:calc(var(--u)*1); --s2:calc(var(--u)*2); --s3:calc(var(--u)*3);
  --s4:calc(var(--u)*4); --s5:calc(var(--u)*5); --s6:calc(var(--u)*6);
  --s8:calc(var(--u)*8); --s10:calc(var(--u)*10);

  --radius:14px;
  --shadow:0 10px 30px -12px rgba(0,0,0,.65);
  --tap:48px;
}
```

## Stałe układu

- mobile-first, jedna kolumna, `max-width:480px`, środkowanie.
- nagłówek przyklejony: znak i marka organizatora po lewej, przełącznik języka po
  prawej (zawsze w nagłówku), pod spodem pasek stanu systemu.
- przełącznik stanu jako segmented control tuż pod nagłówkiem: stan domyślny plus
  jeden istotny stan alternatywny dla danego ekranu.
- dolny pasek akcji przyklejony: linia statusu zapisu, akcje drugorzędne jako linki,
  jedna dominująca akcja główna na akcencie z cyframi tabelarycznymi.
- jeden promień (`--radius`), jeden cień (`--shadow`).
- animacje wejścia kart z `animation-delay`, wyłączane przez
  `prefers-reduced-motion`.

## Reguły zachowania (te same na każdym ekranie)

- Wszystkie napisy widoczne dla użytkownika to klucze tłumaczeń. Obiekt `I18N` z
  `pl-PL` i `en-GB`, język zapasowy en-GB przy braku klucza. Treści organizatora
  (nazwy biegów, tytuły transakcji) też przez klucze z fallbackiem.
- Przełącznik języka realnie przerysowuje teksty i formaty.
- Daty, kwoty i numer telefonu przez `Intl` według `DATA.locale`
  (`Intl.NumberFormat` z `currency:"PLN"`, `Intl.DateTimeFormat`, własny formater
  telefonu). Kwoty ze znakiem przez `signDisplay:"exceptZero"`.
- RTL: `document.documentElement.dir` ustawiany automatycznie, gdy język jest pisany
  od prawej (`RTL = new Set(["ar","he","fa","ur"])`).
- Stan systemu zawsze widoczny: online/offline z notą o wstrzymaniu zmian,
  czas synchronizacji lub status okna, spinner przetwarzania, potwierdzenie zapisu.
- Akcja główna wizualnie dominuje i bywa kontekstowa (zmienia się zależnie od stanu).
- Komunikaty walidacji to dokładnie te z opisu ekranu w `ATH_spec.md`, mówią co
  poszło nie tak i jak to naprawić.
- WCAG: cel dotykowy min 48 px (`--tap`), kontrast tekstu AA, widoczny `:focus-visible`
  na akcencie.
- Motywowanie wyłącznie przez zmienne CSS. Żadnego `localStorage` ani
  `sessionStorage`.

## Zakres makiety statycznej

Makieta hi-fi reprezentuje stan wypełniony jako domyślny oraz dokładnie jeden
istotny stan alternatywny, przełączany segmented controlem. To celowe
ograniczenie, nie przeoczenie.

- Stan ładowania jako szkielet (`skeleton`, `aria-busy`) jest poza zakresem
  statycznej makiety. Fazę pierwszego wczytania zastępuje animacja wejścia kart
  (`animation-delay` na wzorcu `rise`, wyłączana przez `prefers-reduced-motion`).
  Szkielet ładowania należy do implementacji, nie do makiety. Spinner
  przetwarzania przy akcji (`.processing` na akcji głównej) zostaje, bo dotyczy
  działania użytkownika, nie ładowania ekranu.
- Stany opisane w `ATH_spec.md`, które nie są ani domyślnym, ani wybranym stanem
  alternatywnym, makieta dokumentuje w specyfikacji, ale nie musi czynić ich
  osiągalnymi z przełącznika. Dotyczy to na przykład stanu pustego na ekranie,
  którego przełącznik niesie już wariant offline (ATH-02) albo zniknięcia
  ostatniej sztuki (ATH-04). Realizuje się je w implementacji. Wybór jedynego
  stanu alternatywnego do przełącznika bierze się z ostatniego zdania szkicu
  lo-fi, zgodnie z wzorcem generacji niżej.
- Referencyjne ekrany ATH-08, ATH-09 i ATH-10 trzymają tę zasadę i są wzorcem.

## Konwencja nazw plików

`{ID}_{slug-bez-polskich-znaków}_hifi.html`, np.
`ATH-11_galeria-runpixie_hifi.html`.

## Wzorzec generacji jednego ekranu

1. Doczytaj opis ekranu w `ATH_spec.md` i odpowiedni szkic w `ATH_szkice_lo-fi.md`.
2. Weź z opisu: cel, akcję główną, akcje drugorzędne i niszczące, stany, walidacje,
  przypadki brzegowe, lokalizację.
3. Stan alternatywny do przełącznika bierz z ostatniego zdania szkicu lo-fi
  ("Dla hi-fi / Wariant alternatywny ...").
4. Realne treści po polsku, bez lorem ipsum. Organizator domyślny:
  Stołeczny Klub Biegacza, drugi organizator do przykładów: Górski Klub Trailowy.

## Stan prac

Gotowe hi-fi: ATH-08 (samoobsługa), ATH-09 (dosprzedaż), ATH-10 (portfel).
Następny w kolejności: ATH-11 (galeria zdjęć z RunPixie).
