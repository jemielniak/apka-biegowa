# Kontrakt projektowy hi-fi grupy VOL (Blok startowy)

Wklej ten plik na początku nowego okna razem z `VOL_spec.md`, `VOL_szkice_lo-fi.md`
i aktualnym manifestem ekranów. Pilnuje, żeby kolejny ekran wolontariusza wyglądał
i działał spójnie, zamiast dryfować w estetykę grupy rodzica (PAR), organizatora
(OCL) albo instruktora (INS).

VOL to panel wolontariusza w module zawodów. Persona pomaga w dniu zawodów: pracuje
wyłącznie na telefonie, w słońcu, często w rękawiczkach i w tłumie przy bramie, ma
dostęp jednorazowy przypięty do jednego wydarzenia i zwykle nie przeszła żadnego
szkolenia. Z tego profilu wynikają trzy decyzje przekrojowe dla całej grupy.
Pierwsza: wejście opiera się na jednym geście - skan zaproszenia jest pierwszym
i największym elementem ekranu logowania, bo nikt nie zakłada konta przy bramie.
Druga: twarda granica roli - wolontariusz nigdy nie widzi kwot, sald ani faktur,
najwyżej neutralny znacznik statusu opłaty (opłacone, nieopłacone, do wyjaśnienia)
bez żadnej liczby. Trzecia: zero pomyłek przy wydaniu - rozmiar pakietu i wynik
skanu to najjaśniejsze, najbardziej kontrastowe punkty ekranu, czytelne w pełnym
słońcu.

## Kierunek wizualny

Stanowisko przy bramie. Jasny, maksymalnie kontrastowy interfejs operacyjny w duchu
kamizelki odblaskowej i tablicy startowej. Czyste, chłodne tło, mocny akcent
operacyjny, ogromne werdykty skanu w kolorze semantycznym (zielony OK, czerwony
NIE) i wielkie cyfry numeru startowego oraz rozmiaru koszulki. Wszystko podporządkowane
czytelności w słońcu i obsłudze jedną ręką w rękawiczce. Świadomie z dala od ciepłego,
kremowego świata PAR i od ciemnych paneli ATH oraz INS - VOL jest jasny, twardy
i zadaniowy.

## Para krojów (Google Fonts, polskie diakrytyki)

- nagłówkowy: Archivo (600-800), geometryczny grotesk z mocnymi cyframi do numerów
  startowych i rozmiarów
- tekstowy: IBM Plex Sans (400-600), czytelny humanistyczny grotesk z cyframi
  tabelarycznymi, z dala od Inter i Arial

```html
<link href="https://fonts.googleapis.com/css2?family=Archivo:wght@600..800&family=IBM+Plex+Sans:wght@400;500;600&display=swap" rel="stylesheet">
```

## Tokeny marki - jedyne miejsce do przemotywowania

Skopiuj ten blok `:root` bez zmian. Przemotywowanie to podmiana `--brand-ink`
i `--accent` oraz kolorów semantycznych, nic poza tym.

```css
:root{
  --brand-ink:#11161c;
  --accent:#0b69d4;

  --color-success:#0a7d35;
  --color-warning:#b45309;
  --color-error:#c4181f;
  --color-info:#0b69d4;

  --paper:#f1f4f8;
  --surface-1:#ffffff;
  --surface-2:#e9eef4;
  --surface-3:#dbe3ec;
  --hairline:#c5cfdb;
  --text-strong:#11161c;
  --text:#33404e;
  --text-muted:#5d6b7c;
  --on-accent:#ffffff;

  --verdict-ok:#0a7d35;
  --verdict-no:#c4181f;
  --status-paid:#0a7d35;
  --status-unpaid:#b45309;
  --status-check:#5d6b7c;

  --font-display:"Archivo", system-ui, sans-serif;
  --font-body:"IBM Plex Sans", system-ui;

  --u:4px;
  --s1:calc(var(--u)*1); --s2:calc(var(--u)*2); --s3:calc(var(--u)*3);
  --s4:calc(var(--u)*4); --s5:calc(var(--u)*5); --s6:calc(var(--u)*6);
  --s8:calc(var(--u)*8); --s10:calc(var(--u)*10);

  --radius:14px;
  --shadow:0 10px 26px -14px rgba(17,22,28,.34);
  --tap:56px;
  --tap-primary:64px;
}
```

Różnice liczbowe wobec PAR są celowe. Motyw jasny i chłodny, promień mniejszy
(14 px) dla utylitarnego, operacyjnego charakteru, cień ostrzejszy i chłodniejszy.
Cel dotykowy idzie powyżej minimum WCAG (56 px bazowo, 64 px na akcji głównej),
bo persona pracuje w rękawiczce, w słońcu i w tłumie - tu nie projektuje się na
granicy normy. Kolory semantyczne są nasycone i ciemne, żeby trzymać kontrast AA
w pełnym słońcu, a werdykt skanu jest największym kolorowym elementem ekranu.

## Stałe układu

- mobile-first, jedna kolumna, `max-width:480px`, środkowanie. Telefon jest jedynym
  urządzeniem; tablet stanowiska stacjonarnego to przypadek brzegowy dłuższej kolejki.
- nagłówek przyklejony: nazwa wydarzenia po lewej, przełącznik języka po prawej
  (zawsze w nagłówku). Obok języka stały znacznik synchronizacji offline: liczba
  operacji w kolejce i godzina ostatniej synchronizacji.
- brak przełącznika dziecka i brak menu wydarzeń - zakres jest przypięty do jednego
  wydarzenia i jednego zadania. Przełączenie na inne wydarzenie zaczyna się od nowego
  zaproszenia, nie z menu.
- moduł jest stały (zawody) i nie wymaga znacznika przełączania; identyfikuje go
  nazwa wydarzenia w nagłówku.
- pole skanu QR jako element wiodący na ekranach wejścia, odprawy i wydawania;
  ręczne wyszukiwanie po numerze lub nazwisku to zawsze jawna alternatywa pod skanem.
- strefa werdyktu po skanie: wielki, jednokolorowy wynik (zielony OK albo czerwony
  NIE) z numerem startowym wielką cyfrą; rozmiar pakietu największym napisem ekranu.
- dolny pasek akcji przyklejony: jedna dominująca akcja na akcencie o wysokości
  `--tap-primary`, akcje drugorzędne jako linki, linia statusu (synchronizacja,
  licznik kolejki) zawsze widoczna.
- jeden promień (`--radius`), jeden cień (`--shadow`).
- animacje wejścia kart z `animation-delay`, wyłączane przez `prefers-reduced-motion`.

## Reguły zachowania (te same na każdym ekranie)

- Wszystkie napisy widoczne dla użytkownika to klucze tłumaczeń. Obiekt `I18N` z
  `pl-PL` i `en-GB`, język zapasowy en-GB przy braku klucza. Nazwa wydarzenia i nazwy
  stanowisk to treść organizatora, też przez klucze z fallbackiem.
- Przełącznik języka realnie przerysowuje teksty i formaty. Lista języków dziedziczona
  z ustawień wydarzenia.
- Daty, godziny zmian i numer telefonu przez `Intl` według regionu wydarzenia
  (`Intl.DateTimeFormat`, własny formater telefonu). Kwot nie formatuje się, bo
  kwot się nie pokazuje.
- RTL: `document.documentElement.dir` ustawiany automatycznie, gdy język jest pisany
  od prawej (`RTL = new Set(["ar","he","fa","ur"])`).
- Twarda granica roli: zero kwot, sald i faktur na każdym ekranie. Gdzie zadanie
  wymaga wiedzy o opłacie (weryfikacja prawa do odbioru), pokazuje się wyłącznie
  neutralny znacznik statusu w kolorze semantycznym z etykietą słowną (opłacone,
  nieopłacone, do wyjaśnienia), nigdy liczbę. Status „do wyjaśnienia" nie jest
  zadaniem wolontariusza - kieruje do koordynatora.
- Skan przed wszystkim: QR jest pierwszym sposobem wejścia i pierwszym sposobem
  odprawy oraz wydania; ręczne wpisanie kodu, numeru lub nazwiska to fallback dla
  telefonu bez aparatu albo bez zgody na kamerę.
- Werdykt po skanie jest wielki i jednoznaczny: jeden kolor (zielony albo czerwony),
  czytelny w słońcu i w rękawiczkach.
- Offline jest codziennością przy bramie, nie wyjątkiem: pierwsze wejście wymaga
  sieci, potem odprawa i wydawanie działają lokalnie. Operacje trafiają do kolejki
  z widocznym licznikiem i godziną synchronizacji; zabezpieczenie przed podwójną
  odprawą i podwójnym wydaniem na tym samym telefonie jest twarde.
- Potwierdzenie przed czynnością nieodwracalną: wydanie poprzedza pokazanie rozmiaru,
  cofnięcie wydania wymaga powodu. Stan „już odprawiony / już wydany HH:MM" prosi
  o świadome potwierdzenie ponowienia zamiast cichego dublowania.
- Dostęp jest tymczasowy i wygasa po wydarzeniu. Wolontariusz nie zakłada konta
  i nie usuwa dostępu - tworzy je organizator albo most rodzica.
- Stan systemu zawsze widoczny: synchronizacja, licznik kolejki, świeżość danych,
  wynik skanu, ostrzeżenie o możliwej nieświeżości statusu offline.
- Komunikaty walidacji to dokładnie te z opisu ekranu w `VOL_spec.md`, mówią co
  poszło nie tak i jaki jest następny krok (wygasłe zaproszenie, kod już użyty,
  zaproszenie do innego wydarzenia, odwołany dostęp, już odprawiony, brak rozmiaru).
- WCAG 2.1 AA, z celem dotykowym powyżej minimum (`--tap` 56 px, akcja główna
  `--tap-primary` 64 px) i kontrastem podniesionym na pracę w słońcu; widoczny
  `:focus-visible` na akcencie. Heurystyki według Nielsena (1994), WCAG według
  W3C (2018).
- Motywowanie wyłącznie przez zmienne CSS. Żadnego `localStorage` ani `sessionStorage`.

## Konwencja nazw plików

`{ID}_{slug-bez-polskich-znaków}_hifi.html`, np.
`VOL-04_odprawa-skan-qr_hifi.html`.

## Wzorzec generacji jednego ekranu

1. Doczytaj opis ekranu w `VOL_spec.md` i odpowiedni szkic w `VOL_szkice_lo-fi.md`.
2. Weź z opisu: cel, akcję główną, akcje drugorzędne i niszczące, stany, walidacje,
   przypadki brzegowe, lokalizację oraz to, czy ekran jest skanem, listą czy formularzem.
3. Stan alternatywny do przełącznika bierz z ostatniego zdania szkicu lo-fi
   („W hi-fi pokaż ..."). Dla VOL najczęściej jest to stan offline z niezerową kolejką
   albo konkretny błąd skanu (kod już użyty, uczestnik już odprawiony, pakiet już
   wydany HH:MM).
4. Realne treści po polsku, bez lorem ipsum. Wydarzenie wzorcowe: Bieg Wiosenny 2026,
   organizator Stołeczny Klub Biegacza. Uczestnik wzorcowy do skanu: Jan Kowalski,
   numer startowy #142, status opłaty „opłacone" (bez kwoty). Rodzic-wolontariusz
   wzorcowy w VOL-07: Anna Kowalska, zgłoszenie z konta rodzinnego. Stanowiska
   wzorcowe: wydawanie pakietów (sektor B, brama 2), odprawa (brama 1), lista startowa.

## Powiązania między grupami

VOL jest panelem operacyjnym w dniu zawodów, podrzędnym wobec organizatora. Konto
wolontariusza tworzy organizator w OEV-25 albo powstaje przez most wolontariusz-rodzic
z PAR-14 i VOL-07, łącząc tożsamość z konta rodzinnego PAR-01 z rolą przypiętą do
wydarzenia. Statusy opłaty, odprawy i odbioru mają być spójne z warstwą organizatora;
rozstrzyganie sporów o płatność należy do koordynatora i do OEV-18, nie do panelu
wolontariusza. Publiczne zaproszenie dla wolontariuszy pochodzi z GST-02, ustawienia
języków wydarzenia z OEV-28. Kolizja zmiany ze startem dziecka odsyła do zapisów
dziecka w PAR-11.

## Czego brakuje do startu generacji

Do tego okna trzeba wkleić `VOL_spec.md` (opisy VOL-01 do VOL-07) oraz
`VOL_szkice_lo-fi.md`. Bez nich można policzyć zakres z manifestu, ale nie da się
wyciągnąć celów, walidacji i stanów alternatywnych dla poszczególnych ekranów.

## Stan prac

Grupa VOL: lo-fi gotowe dla VOL-01 do VOL-07, hi-fi do zrobienia. Sugerowana kolejność
partii: VOL-01 (logowanie z zaproszenia) i VOL-02 (dashboard zadań) jako blok
ustalający język wizualny; potem VOL-04 i VOL-05 (odprawa i wydawanie - rdzeń
operacyjny ze skanem, werdyktem i kolejką offline); na końcu VOL-03 (harmonogram),
VOL-06 (lista startowa ze statusami) i VOL-07 (most rodzica, jedyny ekran planowania
robiony spokojnie, nie przy bramie).
