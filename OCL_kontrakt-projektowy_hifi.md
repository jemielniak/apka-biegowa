# Kontrakt projektowy hi-fi grupy OCL (Blok startowy)

Wklej ten plik na początku nowego okna razem z `OCL_spec.md`, `OCL_lofi_szkice.md`
i aktualnym manifestem ekranów. Pilnuje, żeby kolejny ekran organizatora wyglądał
i działał spójnie, zamiast dryfować w estetykę rodzica (PAR), wolontariusza (VOL)
albo zawodnika (ATH).

OCL to panel administracyjny organizatora w trybie zajęć - właściciela szkółki. To
samo konto co organizator zawodów, inny tryb. Wszystkie ekrany działają w module
zajęć, rola dominująca to organizator, z odczytem dla instruktora tam, gdzie
zaznaczono. Z profilu pracy za biurkiem recepcji wynikają trzy decyzje przekrojowe
dla całej grupy. Pierwsza: to panel desktopowy, gęsty informacyjnie, z tabelami,
kafelkami wskaźników i wykresami - przeciwieństwo telefonowych, jednozadaniowych
ekranów VOL. Druga: organizator widzi finanse - należności, przeterminowane
płatności, faktury, cennik - więc kwoty formatuje się walutowo z cyframi tabelarycznymi,
a model płatności jest abonamentem placówki, nie opłatą od główki. Trzecia: dane
wrażliwe dziecka nie pojawiają się na widokach zbiorczych, tylko po wejściu w kartę
uczestnika - minimalizacja na pulpicie startowym.

## Kierunek wizualny

Pulpit recepcji szkółki. Spokojny, profesjonalny back-office o chłodnej, neutralnej
palecie, z wyraźną hierarchią danych i mocnym akcentem na liczbach. Gęstość informacji
obsłużona siatką: lewy pasek nawigacji, górny pasek z trybem i placówką, kafelki KPI,
tabele i wykresy. Charakter kompetentny i rzeczowy, nie konsumencki - świadomie z dala
od ciepłego, kremowego PAR i od jasnego, hi-vis VOL. Cyfry tabelaryczne wszędzie, gdzie
są kwoty, limity i frekwencja.

## Para krojów (Google Fonts, polskie diakrytyki)

- nagłówkowy: Space Grotesk (500-700), techniczny grotesk z charakterem do nagłówków,
  KPI i etykiet
- tekstowy: Source Sans 3 (400-700), profesjonalny grotesk roboczy z cyframi
  tabelarycznymi, z dala od Inter i Arial

```html
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500..700&family=Source+Sans+3:ital,wght@0,400;0,500;0,600;0,700;1,400&display=swap" rel="stylesheet">
```

## Tokeny marki - jedyne miejsce do przemotywowania

Skopiuj ten blok `:root` bez zmian. Przemotywowanie to podmiana `--brand-ink`
i `--accent` oraz kolorów semantycznych, nic poza tym. Branding placówki edytuje się
w OCL-18 i z tych samych zmiennych korzystają strony publiczne i paszport.

```css
:root{
  --brand-ink:#1a1f2e;
  --accent:#4f46e5;

  --color-success:#15803d;
  --color-warning:#b45309;
  --color-error:#dc2626;
  --color-info:#2563eb;

  --paper:#f6f8fc;
  --surface-1:#ffffff;
  --surface-2:#eef1f8;
  --surface-3:#e3e7f1;
  --hairline:#d5dae6;
  --text-strong:#1a1f2e;
  --text:#3b4252;
  --text-muted:#6b7385;
  --on-accent:#ffffff;

  --font-display:"Space Grotesk", system-ui, sans-serif;
  --font-body:"Source Sans 3", system-ui;

  --u:4px;
  --s1:calc(var(--u)*1); --s2:calc(var(--u)*2); --s3:calc(var(--u)*3);
  --s4:calc(var(--u)*4); --s5:calc(var(--u)*5); --s6:calc(var(--u)*6);
  --s8:calc(var(--u)*8); --s10:calc(var(--u)*10);

  --radius:12px;
  --shadow:0 8px 24px -16px rgba(26,31,46,.30);
  --tap:44px;
}
```

Różnice liczbowe wobec PAR i VOL są celowe. Motyw jasny, chłodny i neutralny, promień
najmniejszy (12 px) dla rzeczowego, administracyjnego charakteru, cień subtelny. Cel
dotykowy schodzi do minimum WCAG 44 px, bo urządzeniem wiodącym jest desktop z myszą,
a nie palec w rękawiczce - na widokach dotykowych (tablet recepcji) zachowaj 44 px
jako dolną granicę. Akcent indygo odróżnia panel od koralowego PAR i niebieskiego VOL;
zieleń pozostaje kolorem sukcesu, nie marki, żeby nie kolidowała ze statusami płatności.

## Stałe układu

- desktop-first, szeroki kontener `max-width:1280px`. Tablet recepcji zwija lewy pasek
  do ikon; telefon pokazuje wyłącznie skróconą listę alertów w trybie odczytu.
- górny pasek przyklejony: logo placówki, przełącznik trybu zawody/zajęcia, filtr
  placówki, przełącznik języka, menu konta. Tryb i placówka są stałym kontekstem
  całego panelu.
- lewy pasek nawigacji ze skokami do sekcji (pulpit, grupy, grafik, zapisy, uczestnicy,
  płatności, raporty, ustawienia).
- kafelki KPI w rzędzie u góry pulpitu, każdy ładowany niezależnie, żeby częściowy
  błąd dotyczył jednego kafelka, a nie całego ekranu.
- tabele z sortowaniem i panelem bocznym edycji; jedna dominująca akcja główna na
  ekran (np. „Dodaj grupę"), zapis zmian potwierdzany tostem.
- kwoty, limity i frekwencja zawsze cyframi tabelarycznymi; przeterminowania
  wyróżnione kolorem ostrzeżenia lub błędu.
- jeden promień (`--radius`), jeden cień (`--shadow`).
- animacje wejścia subtelne, wyłączane przez `prefers-reduced-motion`.

## Reguły zachowania (te same na każdym ekranie)

- Wszystkie napisy widoczne dla użytkownika to klucze tłumaczeń. Obiekt `I18N` z
  `pl-PL` i `en-GB`, język zapasowy en-GB przy braku klucza. Nazwy grup, sal i poziomów
  to treść organizatora, też przez klucze z fallbackiem.
- Przełącznik języka realnie przerysowuje teksty i formaty. Lista języków placówki
  i język zapasowy konfigurowane w OCL-18.
- Daty, kwoty i numer telefonu przez `Intl` według regionu placówki
  (`Intl.NumberFormat` z walutą placówki, `Intl.DateTimeFormat`, własny formater
  telefonu). Salda i ruchy ze znakiem przez `signDisplay:"exceptZero"`.
- RTL: `document.documentElement.dir` ustawiany automatycznie, gdy język jest pisany
  od prawej (`RTL = new Set(["ar","he","fa","ur"])`).
- Tryb jawny: panel działa w trybie zajęć, przełącznik trybu w nagłówku prowadzi do
  bliźniaczego świata zawodów (OEV). Filtr placówki zawęża wszystkie widoki.
- Finanse są widoczne, bo to rola organizatora: należności, przeterminowane płatności,
  portfel, faktury, cennik. Cennik opiera się na abonamencie placówki, nie na opłacie
  od liczby dzieci - koszt operatora nie rośnie z liczbą użytkowników. Wrażliwych
  danych płatniczych makieta nie zbiera; numery kart i kont zostają po stronie bramki.
- Minimalizacja na widokach zbiorczych: pulpit i tabele pokazują dane zagregowane
  i liczby; dane wrażliwe dziecka pojawiają się dopiero w karcie uczestnika (OCL-09).
- Zapobieganie konfliktom: nakładanie się terminów tego samego instruktora oraz
  kolizje sal pokazują nazwę kolidującego elementu i propozycję naprawy zamiast cichej
  odmowy.
- Akcje niszczące z zabezpieczeniem: usunięcie grupy tylko bez zapisanych uczestników,
  w innym wypadku archiwizacja z zachowaniem historii; usunięcie sali w użyciu
  zablokowane z listą grup, które jej używają; zmiana uprawnień i ról wymaga wyraźnego
  potwierdzenia i zostawia ślad audytu; odebranie ostatniej roli organizatora wymaga
  wskazania następcy.
- Branding wyłącznie przez zmienne CSS edytowane w OCL-18; kontroler kontrastu
  ostrzega, gdy tokeny marki łamią WCAG, i wskazuje, które zestawienie nie spełnia
  minimum.
- Stan systemu zawsze widoczny: znacznik czasu odświeżenia wskaźników, status zapisu
  (tost), komunikat przy nieudanym pobraniu z przyciskiem ponowienia. Offline zwykle
  nie dotyczy - panel wymaga sieci.
- Komunikaty walidacji to dokładnie te z opisu ekranu w `OCL_spec.md`, mówią co poszło
  nie tak i jak to naprawić (limit dodatni, koniec godzin pracy po początku, brak
  przypisanego poziomu blokujący publikację, konflikt terminu z nazwą kolidującej grupy).
- WCAG 2.1 AA: cel dotykowy min `--tap` (44 px) na widokach dotykowych, kontrast tekstu
  AA, widoczny `:focus-visible` na akcencie, pełna obsługa klawiaturą w tabelach
  i formularzach. Heurystyki według Nielsena (1994), WCAG według W3C (2018).
- Motywowanie wyłącznie przez zmienne CSS. Żadnego `localStorage` ani `sessionStorage`.

## Konwencja nazw plików

`{ID}_{slug-bez-polskich-znaków}_hifi.html`, np.
`OCL-01_dashboard-zajec_hifi.html`.

## Wzorzec generacji jednego ekranu

1. Doczytaj opis ekranu w `OCL_spec.md` i odpowiedni szkic w `OCL_lofi_szkice.md`.
2. Weź z opisu: cel, akcję główną, akcje drugorzędne i niszczące, stany, walidacje,
   przypadki brzegowe, lokalizację oraz to, czy ekran jest pulpitem, tabelą, formularzem
   czy raportem.
3. Stan alternatywny do przełącznika bierz z ostatniego zdania szkicu lo-fi. Dla OCL
   typowe stany to pusty (placówka bez grup, kreator), błąd jednego kafelka KPI,
   konflikt terminu lub sali, blokada usunięcia z propozycją archiwizacji.
4. Realne treści po polsku, bez lorem ipsum. Placówka wzorcowa: Klub Pływacki Delfin.
   Grupy wzorcowe: Żabki, Delfiny, Rekiny z poziomami; instruktorzy Nowak i Mazur;
   uczestnicy z rodziny Kowalskich (Zofia, pływanie). Kwoty w PLN. Drugi organizator
   do przykładów: Szkoła Pływania Nurek.

## Powiązania między grupami

OCL jest warstwą organizatora w trybie zajęć, bliźniaczą wobec świata zawodów (OEV).
Pulpit (OCL-01) kieruje alertami do płatności (OCL-14), kadry (OCL-12), list rezerwowych
(OCL-05) i grafiku (OCL-03). Drzewo umiejętności (OCL-10) zasila grupy (OCL-02), kartę
uczestnika (OCL-09) i postęp widziany przez rodzica (PAR-08) oraz instruktora (INS).
Publikacja grup uruchamia widok zapisu rodzica (PAR-04). Konfiguracja paszportu
(OCL-17) zasila eksport rodzica (PAR-13). Ustawienia placówki i branding (OCL-18)
wpływają na strony publiczne grupy (GST-03) i na paszport. Statusy zapisów, list
rezerwowych i płatności mają być spójne z tym, co po swojej stronie widzą rodzic,
instruktor i zawodnik.

## Czego brakuje do startu generacji

Do tego okna trzeba wkleić `OCL_spec.md` (opisy OCL-01 do OCL-18) oraz
`OCL_lofi_szkice.md`. Bez nich można policzyć zakres z manifestu, ale nie da się
wyciągnąć celów, walidacji i stanów alternatywnych dla poszczególnych ekranów.

## Stan prac

Grupa OCL: lo-fi gotowe dla OCL-01 do OCL-18, hi-fi do zrobienia. Sugerowana kolejność
partii: OCL-01 (dashboard zajęć) i OCL-02 (grupy z poziomami) jako blok ustalający
język wizualny i wzorzec tabeli oraz pulpitu; potem OCL-03 do OCL-09 (grafik,
odrabianie, listy rezerwowe, obozy, cennik, uczestnicy, karta uczestnika); dalej
OCL-10 do OCL-14 (drzewo umiejętności, frekwencja, instruktorzy, rezerwacje lekcji,
płatności i faktury); na końcu OCL-15 do OCL-18 (komunikacja, raporty, paszport,
ustawienia placówki z rolami i uprawnieniami). Ze względu na desktopową gęstość
i finanse to najcięższa grupa - planuj mniejsze partie niż w PAR.
