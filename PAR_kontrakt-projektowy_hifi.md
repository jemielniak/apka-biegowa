# Kontrakt projektowy hi-fi grupy PAR (Blok startowy)

Wklej ten plik na początku nowego okna razem z `PAR_spec.md`, `PAR_szkice_lo-fi.md`
i `manifest_ekranow_v3.csv`. Pilnuje, żeby kolejny ekran rodzica wyglądał i działał
spójnie, zamiast dryfować w estetykę grupy ATH (zawodnik) albo INS (instruktor).

PAR to konto rodziny i moduł wspólny. Rodzic zarządza kilkorgiem dzieci, jednym
portfelem i jednym paszportem sportowym, a zapisy prowadzi i do zawodów, i do
zajęć. Dwie decyzje przekrojowe wynikają z tego dla całej grupy. Pierwsza:
przełącznik dziecka jest elementem stałym, bo prawie każdy ekran dotyczy
konkretnego dziecka albo całej rodziny. Druga: rodzic widzi finanse - portfel,
płatności, faktury - więc granica „zero kwot" z INS tu nie obowiązuje, a kwoty
formatuje się walutowo.

## Kierunek wizualny

Rodzinny dziennik sportowy. Ciepły kremowy papier jako tło, akcent w kolorze
koralu, miękki krój nagłówkowy z charakterem i czytelny grotesk w treści z cyframi
tabelarycznymi w kwotach, saldach i wynikach. Każde dziecko dostaje swój kolorowy
żeton tożsamości. Motyw jasny - świadomy kontrast wobec ciemnych ATH i INS. Bez
fioletowych gradientów na bieli, bez krojów systemowych, z dala od estetyki tamtych
dwóch grup.

## Para krojów (Google Fonts, polskie diakrytyki)

- nagłówkowy: Fraunces (500–800), miękki serif z osobowością
- tekstowy: Hanken Grotesk (400–700, plus italic 500), z cyframi tabelarycznymi

```html
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,500..800;1,9..144,500&family=Hanken+Grotesk:ital,wght@0,400;0,500;0,600;0,700;1,500&display=swap" rel="stylesheet">
```

## Tokeny marki - jedyne miejsce do przemotywowania

Skopiuj ten blok `:root` bez zmian. Przemotywowanie to podmiana `--brand-ink`
i `--accent` oraz kolorów semantycznych, nic poza tym. Paleta dzieci `--kid-*`
służy tylko do żetonów tożsamości i nie jest kolorem marki.

```css
:root{
  --brand-ink:#241c17;
  --accent:#ef5a3c;

  --color-success:#1f9d57;
  --color-warning:#c47d0a;
  --color-error:#d6402a;
  --color-info:#2f72d6;

  --paper:#faf5ee;
  --surface-1:#ffffff;
  --surface-2:#fff9f2;
  --surface-3:#f3ebe0;
  --hairline:#e7ddcf;
  --text-strong:#241c17;
  --text:#4a4039;
  --text-muted:#8a7d70;
  --on-accent:#fff7f3;

  --kid-1:#ef5a3c;
  --kid-2:#1f9d8f;
  --kid-3:#7a5cd6;
  --kid-4:#c47d0a;

  --font-display:"Fraunces", Georgia, serif;
  --font-body:"Hanken Grotesk", system-ui;

  --u:4px;
  --s1:calc(var(--u)*1); --s2:calc(var(--u)*2); --s3:calc(var(--u)*3);
  --s4:calc(var(--u)*4); --s5:calc(var(--u)*5); --s6:calc(var(--u)*6);
  --s8:calc(var(--u)*8); --s10:calc(var(--u)*10);

  --radius:20px;
  --shadow:0 14px 34px -16px rgba(60,40,30,.28);
  --tap:48px;
}
```

Różnice liczbowe wobec ATH i INS są celowe. Motyw jest jasny, promień większy
(20 px) dla cieplejszego, konsumenckiego charakteru, cień miękki i ciepły. Cel
dotykowy wraca do minimum WCAG 48 px - rodzic nie obsługuje ekranu mokrym palcem
przy basenie, więc nie ma powodu na 56 px z INS. Kolory semantyczne są ciemniejsze
niż w grupach ciemnych, żeby trzymać kontrast AA na jasnym tle.

## Stałe układu

- mobile-first, jedna kolumna, `max-width:480px`, środkowanie. Telefon jest
  urządzeniem wiodącym rodzica.
- nagłówek przyklejony: znak i nazwa konta rodziny po lewej, przełącznik języka po
  prawej (zawsze w nagłówku). Pod znakiem przełącznik dziecka: rząd żetonów z
  inicjałami w kolorach `--kid-*` plus opcja „cała rodzina" tam, gdzie ekran tego
  dotyczy.
- pasek modułu pod przełącznikiem dziecka tam, gdzie ekran wisi w jednym module:
  neutralny znacznik „zawody" albo „zajęcia", bo PAR łączy oba.
- dolny pasek akcji przyklejony: linia statusu (zapis, saldo, termin), akcje
  drugorzędne jako linki, jedna dominująca akcja główna na akcencie z cyframi
  tabelarycznymi tam, gdzie są liczby.
- jeden promień (`--radius`), jeden cień (`--shadow`).
- animacje wejścia kart z `animation-delay`, wyłączane przez
  `prefers-reduced-motion`.

## Reguły zachowania (te same na każdym ekranie)

- Wszystkie napisy widoczne dla użytkownika to klucze tłumaczeń. Obiekt `I18N` z
  `pl-PL` i `en-GB`, język zapasowy en-GB przy braku klucza. Treści organizatorów
  (nazwy biegów, grup, transakcji) też przez klucze z fallbackiem.
- Przełącznik języka realnie przerysowuje teksty i formaty.
- Daty, kwoty i numer telefonu przez `Intl` według `DATA.locale`
  (`Intl.NumberFormat` z `currency:"PLN"`, `Intl.DateTimeFormat`, własny formater
  telefonu). Salda i ruchy portfela ze znakiem przez `signDisplay:"exceptZero"`.
- RTL: `document.documentElement.dir` ustawiany automatycznie, gdy język jest
  pisany od prawej (`RTL = new Set(["ar","he","fa","ur"])`).
- Przełącznik dziecka realnie przełącza kontekst ekranu: dane, postępy, salda i
  zapisy zmieniają się na wybrane dziecko, a „cała rodzina" agreguje.
- Moduł jest jawny: ekran z modułu zawody albo zajęcia pokazuje swój znacznik,
  ekrany wspólne (portfel, paszport, ustawienia) mówią, że obejmują oba.
- Finanse są widoczne, bo to rola rodzica: portfel ze stanem i historią, ceny i
  progi, płatności, faktury. Wrażliwych danych płatniczych nie zbiera makieta -
  numery kart i kont zostają po stronie bramki, ekran prowadzi tylko do niej.
- Stan systemu zawsze widoczny: status zapisu, termin lub okno zapisu, status
  płatności, potwierdzenie operacji na portfelu.
- Akcja główna wizualnie dominuje i bywa kontekstowa (zmienia się zależnie od
  stanu, np. zapis kontra dopłata kontra dołączenie do listy rezerwowej).
- Komunikaty walidacji to dokładnie te z opisu ekranu w `PAR_spec.md`, mówią co
  poszło nie tak i jak to naprawić.
- Zgody RODO i regulaminy traktuje się wprost: wersja dokumentu, data zgody,
  rozdział zgód marketingowych od niezbędnych. Zgody za dziecko realizuje rodzic
  z konta dorosłego (powiązanie z KID-01).
- WCAG 2.1 AA: cel dotykowy min `--tap` (48 px), kontrast tekstu AA na jasnym tle,
  widoczny `:focus-visible` na akcencie. Heurystyki według Nielsena (1994).
- Motywowanie wyłącznie przez zmienne CSS. Żadnego `localStorage` ani
  `sessionStorage`.

## Konwencja nazw plików

`{ID}_{slug-bez-polskich-znaków}_hifi.html`, np.
`PAR-02_dashboard-rodzica_hifi.html`.

## Wzorzec generacji jednego ekranu

1. Doczytaj opis ekranu w `PAR_spec.md` i odpowiedni szkic w `PAR_szkice_lo-fi.md`.
2. Weź z opisu: cel, akcję główną, akcje drugorzędne i niszczące, stany, walidacje,
  przypadki brzegowe, lokalizację, moduł oraz to, czy ekran dotyczy dziecka,
  rodziny, czy obu.
3. Stan alternatywny do przełącznika bierz z ostatniego zdania szkicu lo-fi
  ("Stan alternatywny do hi-fi ...").
4. Realne treści po polsku, bez lorem ipsum. Konto domyślne: rodzina Kowalskich.
  Dzieci wzorcowe: Zofia (zajęcia, pływanie, Klub Pływacki Delfin) i Jan (zawody,
  bieganie, Stołeczny Klub Biegacza) - parę modułów pokazuj na tych dwojgu.
  Drugi organizator do przykładów zajęć: Szkoła Pływania Nurek.

## Powiązania między grupami

PAR jest warstwą wspólną nad światem zawodów i zajęć. Łączy się z ATH przez zapis
dziecka na zawody (PAR-11), z OCL i INS przez zapisy na grupy, postępy i obecności,
z KID przez zgody opiekuna i widok nastolatka. Statusy zapisów, portfela i paszportu
mają być spójne z tym, co po swojej stronie pokazują organizator i zawodnik.

## Czego brakuje do startu generacji

Do tego okna trzeba jeszcze wkleić `PAR_spec.md` (opisy PAR-01 do PAR-16) oraz
`PAR_szkice_lo-fi.md`. Bez nich można policzyć zakres z manifestu, ale nie da się
wyciągnąć celów, walidacji i stanów alternatywnych dla poszczególnych ekranów.

## Stan prac

Grupa PAR: lo-fi gotowe dla PAR-01 do PAR-16, hi-fi do zrobienia. Sugerowana
kolejność partii: PAR-01 (logowanie z kontem rodzinnym), PAR-02 (dashboard rodzica),
PAR-03 (profile dzieci z przełączaniem) jako blok ustalający język wizualny; potem
PAR-04 do PAR-10 (zapisy, nieobecności, postępy, dokumenty, portfel); na końcu
PAR-11 do PAR-16 (zapis na zawody między modułami, lekcje 1:1, paszport, wolontariat,
galeria, ustawienia i zgody).
