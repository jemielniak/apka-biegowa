# Kontrakt projektowy hi-fi grupy INS (Blok startowy)

Wklej ten plik na początku nowego okna razem z `INS_spec.md`, `INS_szkice_lo-fi.md`
i `manifest_ekranow_v3.csv`. Pilnuje, żeby kolejny ekran wyglądał i działał jak
INS-01, INS-02 i INS-04, zamiast dryfować w inną estetykę albo w estetykę grupy ATH.

INS to inny świat niż ATH. ATH to chronograf dnia startu dla zawodnika. INS to
narzędzie pracy instruktora przy basenie: mokra dłoń, pośpiech, zerwany zasięg,
twarda granica roli (zero finansów). Dlatego INS ma własny kierunek, własną parę
krojów i własne tokeny. Nie kopiujemy electric lime z ATH.

## Kierunek wizualny

Karta odprawy przy basenie. Głęboki teal-ink jako tło panelu, akcent w kolorze
wody (aqua), zaokrąglone, duże cele dotykowe pod mokry palec, cyfry tabelaryczne
w licznikach obecności, frekwencji, sumie godzin. Tryb offline jest obywatelem
pierwszej klasy na każdym ekranie, nie przypisem. Bez fioletowych gradientów na
bieli, bez krojów systemowych, bez krojów i akcentu z ATH.

## Para krojów (Google Fonts, polskie diakrytyki)

- nagłówkowy: Bricolage Grotesque (500–800)
- tekstowy: IBM Plex Sans (400–700, plus italic 500), z cyframi tabelarycznymi

```html
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,500..800&family=IBM+Plex+Sans:ital,wght@0,400;0,500;0,600;0,700;1,500&display=swap" rel="stylesheet">
```

## Tokeny marki - jedyne miejsce do przemotywowania

Skopiuj ten blok `:root` bez zmian. Przemotywowanie placówki to podmiana
`--brand-ink` i `--accent` oraz kolorów semantycznych, nic poza tym.

```css
:root{
  --brand-ink:#0d1a1e;
  --accent:#21d8c4;

  --color-success:#4ade80;
  --color-warning:#f6b73c;
  --color-error:#ff6b5e;
  --color-info:#5aa8ff;

  --surface-1:#13242a;
  --surface-2:#1a313a;
  --surface-3:#234049;
  --hairline:#2c4b55;
  --text-strong:#f0f6f5;
  --text:#c7d6d8;
  --text-muted:#83a0a5;
  --on-accent:#062420;
  --paper:#eef4f1;

  --font-display:"Bricolage Grotesque", system-ui;
  --font-body:"IBM Plex Sans", system-ui;

  --u:4px;
  --s1:calc(var(--u)*1); --s2:calc(var(--u)*2); --s3:calc(var(--u)*3);
  --s4:calc(var(--u)*4); --s5:calc(var(--u)*5); --s6:calc(var(--u)*6);
  --s8:calc(var(--u)*8); --s10:calc(var(--u)*10);

  --radius:16px;
  --shadow:0 12px 32px -14px rgba(0,0,0,.7);
  --tap:56px;
}
```

Uwaga na `--tap`: 56 px, nie 48. Spec INS mówi wprost, że persona obsługuje ekran
mokrym palcem, więc cel dotykowy projektujemy powyżej minimum WCAG, a nie na jego
granicy. To jedyna twarda różnica liczbowa wobec ATH.

## Stałe układu

- mobile-first, jedna kolumna, `max-width:480px`, środkowanie. Tablet trzymany
  pionowo to drugi przypadek, desktop to przypadek brzegowy pracy zza biurka.
- nagłówek przyklejony: logo placówki po lewej, przełącznik języka po prawej
  (zawsze w nagłówku), pod spodem pasek stanu systemu.
- pasek stanu systemu pokazuje sieć i synchronizację: online/offline, moment
  ostatniej synchronizacji oraz licznik zmian w kolejce lokalnej. Offline ma
  własny, wyraźny baner, nie szarą notkę.
- przełącznik stanu jako segmented control tuż pod paskiem stanu: stan domyślny
  plus jeden istotny stan alternatywny dla danego ekranu (z ostatniego zdania
  szkicu lo-fi).
- dolny pasek akcji przyklejony: linia statusu zapisu lub kolejki, akcje
  drugorzędne jako linki, jedna dominująca akcja główna na akcencie, z cyframi
  tabelarycznymi tam, gdzie są liczby.
- jeden promień (`--radius`), jeden cień (`--shadow`).
- animacje wejścia kart z `animation-delay`, wyłączane przez
  `prefers-reduced-motion`.

## Reguły zachowania (te same na każdym ekranie)

- Wszystkie napisy widoczne dla użytkownika to klucze tłumaczeń. Obiekt `I18N` z
  `pl-PL` i `en-GB`, język zapasowy en-GB przy braku klucza. Treści placówki
  (nazwy grup, poziomy, umiejętności) też przez klucze z fallbackiem.
- Przełącznik języka realnie przerysowuje teksty i formaty.
- Daty, godziny i numer telefonu przez `Intl` według `DATA.locale`
  (`Intl.DateTimeFormat`, `Intl.NumberFormat`, własny formater telefonu). Liczby
  godzin formatuj lokalnie (przecinek dziesiętny w pl-PL).
- RTL: `document.documentElement.dir` ustawiany automatycznie, gdy język jest
  pisany od prawej (`RTL = new Set(["ar","he","fa","ur"])`).
- Stan systemu zawsze widoczny: online/offline z notą o kolejce lokalnej, moment
  synchronizacji, status zapisu lub potwierdzenia.
- Tryb offline dotyczy obecności (INS-04) i aktualizacji umiejętności (INS-05):
  dane lecą do kolejki lokalnej i synchronizują się po sieci, z widocznym
  licznikiem. Reszta ekranów w offline pokazuje dane z ostatniej synchronizacji
  z banerem nieświeżości.
- Granica roli jest twarda: instruktor nie widzi płatności, sald ani faktur.
  Tam, gdzie organizator pokazałby status finansowy, INS go pomija lub zastępuje
  znacznikiem neutralnym. Żadnych kwot na ekranach INS.
- Akcja główna wizualnie dominuje i bywa kontekstowa (zmienia się zależnie od
  stanu, np. na zapamiętanym urządzeniu logowanie biometryczne).
- Komunikaty walidacji to dokładnie te z opisu ekranu w `INS_spec.md`, mówią co
  poszło nie tak i jak to naprawić.
- WCAG 2.1 AA: cel dotykowy min `--tap` (56 px), kontrast tekstu AA, widoczny
  `:focus-visible` na akcencie. Heurystyki według Nielsena (1994).
- Motywowanie wyłącznie przez zmienne CSS. Żadnego `localStorage` ani
  `sessionStorage`.

## Konwencja nazw plików

`{ID}_{slug-bez-polskich-znaków}_hifi.html`, np.
`INS-04_lista-obecnosci_hifi.html`.

## Wzorzec generacji jednego ekranu

1. Doczytaj opis ekranu w `INS_spec.md` i odpowiedni szkic w `INS_szkice_lo-fi.md`.
2. Weź z opisu: cel, akcję główną, akcje drugorzędne i niszczące, stany,
  walidacje, przypadki brzegowe, lokalizację, granicę roli (zero finansów).
3. Stan alternatywny do przełącznika bierz z ostatniego zdania szkicu lo-fi
  ("Stan alternatywny do hi-fi ...").
4. Realne treści po polsku, bez lorem ipsum. Placówka domyślna:
  Klub Pływacki Delfin, druga placówka do przykładów: Szkoła Pływania Nurek.
  Grupy: Delfinki, Żabki, Rekiny. Instruktor wzorcowy: A. Mazur.

## Stan prac

Gotowe hi-fi: INS-01 (logowanie), INS-02 (dashboard dnia), INS-04 (obecność
offline). Następne w kolejce partii: INS-03 (grafik), INS-05 (umiejętności),
INS-06 (profil uczestnika), dalej INS-07 do INS-10.
