# Kontrakt projektowy hi-fi grupy KID (Blok startowy)

Wklej ten plik na początku nowego okna razem z `KID_spec.md`, `KID_szkice_lo-fi.md`
i aktualnym manifestem ekranów. Pilnuje, żeby ekran KID wyglądał i działał spójnie
ze światem rodziny (PAR), a zarazem trzymał odrębny, prawnie ostrożny charakter tej
grupy.

KID obejmuje małoletniego uczestnika, który domyślnie nie ma własnego logowania i jest
profilem w koncie rodzica opisanym w PAR. Grupa ma dwa ekrany: KID-01, ekran zgody
opiekuna realizowany z konta dorosłego, oraz KID-02, opcjonalny widok tylko do odczytu
dla nastolatka. Z prawnego charakteru grupy wynikają trzy decyzje przekrojowe.
Pierwsza, najważniejsza: każdy ekran KID ma w manifeście pole „decyzja prawna przed
wdrożeniem: TAK" - makieta rezerwuje miejsce na rozstrzygnięcia (próg wieku samodzielnej
zgody, reguła przy dwojgu opiekunów, zakres danych nastolatka), ale ich nie przesądza.
Druga: zgoda jest rozdzielona na cele, nigdy zbiorcza, bez pól zaznaczonych z góry,
z wersją, datą i wskazaniem osoby wyrażającej, a jej wycofanie jest równie proste jak
udzielenie. Trzecia: dane szczególnej kategorii (zdrowie, art. 9 RODO) są wizualnie
oddzielone od zgód zwykłych i wymagają osobnego, wyraźnego potwierdzenia.

W polskim porządku prawnym próg samodzielnej zgody na usługi społeczeństwa informacyjnego
wynosi 16 lat, bo ustawodawca nie obniżył go do 13 lat (Ustawa o ochronie danych
osobowych, 2018; RODO, 2016). Poniżej tej granicy zgodę wyraża albo autoryzuje opiekun.
Kontrakt nie rozstrzyga progu ani reguły przy dwojgu opiekunów - to przedmiot decyzji
prawnej przed wdrożeniem.

## Kierunek wizualny

Bezpieczna karta zgód i mój paszport. Łagodny, ciepły, wzbudzający zaufanie interfejs
o miękkich kształtach i spokojnej palecie, czytelny dla dwóch odbiorców: opiekuna
przeglądającego zgody i nastolatka oglądającego swoje postępy. Blisko ciepła rodziny
z PAR, ale z własnym, bardziej miętowo-zaufaniowym akcentem zamiast koralu, żeby grupa
miała odrębną tożsamość. Sekcja danych szczególnej kategorii jest wyraźnie wyodrębniona
osobnym znacznikiem, a widok nastolatka komunikuje oglądanie, nie zmianę stanu.

## Para krojów (Google Fonts, polskie diakrytyki)

- nagłówkowy: Nunito (600-800), zaokrąglony, przyjazny i budzący zaufanie
- tekstowy: Nunito Sans (400-700), miękki, bardzo czytelny grotesk, z dala od Inter
  i Arial

```html
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@600..800&family=Nunito+Sans:ital,opsz,wght@0,6..12,400;0,6..12,500;0,6..12,600;0,6..12,700;1,6..12,400&display=swap" rel="stylesheet">
```

## Tokeny marki - jedyne miejsce do przemotywowania

Skopiuj ten blok `:root` bez zmian. Przemotywowanie to podmiana `--brand-ink`
i `--accent` oraz kolorów semantycznych, nic poza tym. Token `--sensitive` służy
wyłącznie do oznaczania sekcji danych szczególnej kategorii i nie jest kolorem marki.

```css
:root{
  --brand-ink:#23303a;
  --accent:#2a9d8f;

  --color-success:#1f9d57;
  --color-warning:#c47d0a;
  --color-error:#d6402a;
  --color-info:#2f72d6;

  --sensitive:#7a5cd6;

  --paper:#f4f8f6;
  --surface-1:#ffffff;
  --surface-2:#eef4f1;
  --surface-3:#e1ebe7;
  --hairline:#d3e0da;
  --text-strong:#23303a;
  --text:#46555c;
  --text-muted:#7a8a8f;
  --on-accent:#ffffff;

  --font-display:"Nunito", system-ui, sans-serif;
  --font-body:"Nunito Sans", system-ui;

  --u:4px;
  --s1:calc(var(--u)*1); --s2:calc(var(--u)*2); --s3:calc(var(--u)*3);
  --s4:calc(var(--u)*4); --s5:calc(var(--u)*5); --s6:calc(var(--u)*6);
  --s8:calc(var(--u)*8); --s10:calc(var(--u)*10);

  --radius:22px;
  --shadow:0 14px 34px -16px rgba(35,48,58,.26);
  --tap:48px;
}
```

Różnice wobec PAR są celowe. Motyw jasny, promień największy (22 px) dla najłagodniejszego,
najbardziej przyjaznego charakteru, akcent miętowo-zaufaniowy zamiast koralu. Token
`--sensitive` (fiolet) wyodrębnia sekcję danych o zdrowiu i pojawia się tylko tam.
Cel dotykowy WCAG 48 px, kontrast AA na jasnym tle. Kolory semantyczne dzielone z PAR,
bo oba światy są jasne i konsumenckie.

## Stałe układu

- mobile-first, jedna kolumna, `max-width:480px`, środkowanie. Telefon jest urządzeniem
  wiodącym, tablet i desktop to wygodniejszy przegląd treści zgód lub paszportu.
- nagłówek przyklejony: tytuł ekranu i powrót po lewej, przełącznik języka po prawej
  (zawsze w nagłówku).
- KID-01: pod nagłówkiem wskaźnik dziecka, którego dotyczy zgoda (imię, wiek), oraz
  dziedziczona tożsamość opiekuna; pasek postępu zgód (np. 2 z 4); lista zgód, każda
  z osobnym przełącznikiem, celem, administratorem, kategorią danych, okresem
  przechowywania, skutkiem udzielenia i wycofania oraz znacznikiem wersji i daty;
  sekcja danych szczególnej kategorii wyraźnie oddzielona znacznikiem `--sensitive`;
  dolny pasek z jedną dominującą akcją („Udziel wymaganych zgód") i równie dostępnym
  „Wycofaj udzieloną zgodę".
- KID-02: trwały znacznik „tylko odczyt" i data ostatniej synchronizacji; trzy sekcje
  odczytowe (paszport, drzewo postępu, galeria); akcja główna pasywna („Przeglądaj").
  Powierzchnie płatności, zgód i zapisów nie istnieją w tym widoku - nie ma ich wcale,
  nie są to wyszarzone przyciski.
- jeden promień (`--radius`), jeden cień (`--shadow`).
- animacje wejścia łagodne, wyłączane przez `prefers-reduced-motion`.

## Reguły zachowania (te same na każdym ekranie)

- Wszystkie napisy widoczne dla użytkownika to klucze tłumaczeń. Obiekt `I18N` z
  `pl-PL` i `en-GB`, język zapasowy en-GB przy braku klucza. Treści dokumentów
  i opisy umiejętności to treść organizatora z fallbackiem.
- Przełącznik języka realnie przerysowuje teksty i formaty.
- Daty i numer telefonu przez `Intl` według regionu konta (`Intl.DateTimeFormat`,
  własny formater telefonu). Kwoty nie występują na ekranach KID.
- RTL: `document.documentElement.dir` ustawiany automatycznie, gdy język jest pisany
  od prawej (`RTL = new Set(["ar","he","fa","ur"])`).
- Zgoda rozdzielona na cele: każdy cel ma osobną, jednoznaczną decyzję, nigdy zgody
  zbiorczej (EROD, 2020). Żadne pole nie jest zaznaczone z góry - zgoda wymaga aktywnego
  działania opiekuna.
- Rozliczalność: przy każdej zgodzie znacznik wersji treści i daty; rejestracja zapisuje
  wersję, datę i wskazanie osoby wyrażającej (RODO, 2016, art. 7 ust. 1).
- Wycofanie równie proste jak udzielenie: akcja wycofania jest tak samo widoczna i
  dostępna jak udzielenie, bez dodatkowych barier (RODO, 2016, art. 7 ust. 3). Ekran
  tłumaczy, że cofnięcie zgody wymaganej do usługi może uniemożliwić udział dziecka,
  i prosi o potwierdzenie, ale nie utrudnia samego wycofania.
- Dane szczególnej kategorii oddzielone: zgoda na dane o zdrowiu (art. 9 RODO) siedzi
  w osobnej sekcji oznaczonej `--sensitive`, z osobnym, wyraźnym potwierdzeniem; nigdy
  nie łączy się jej ze zgodami zwykłymi i nie upodabnia wizualnie.
- Prosty język: treść kierowana do dziecka formułowana jasnym, prostym językiem
  dostosowanym do małoletniego (RODO, 2016, art. 12 ust. 1; motyw 58).
- Małoletni jest profilem w koncie rodzinnym, nie osobnym kontem. Widok nastolatka
  (KID-02) włącza opiekun w PAR-03, a poświadczenia ustawia opiekun, nie dziecko -
  samodzielnej rejestracji małoletniego system nie prowadzi.
- W KID-02 brak powierzchni zastrzeżonych dla opiekuna jest realizowany przez ich
  nieobecność, a nie przez widoczne, zablokowane przyciski - to chroni przed błędem
  i nie przywraca dziecku widoku funkcji opiekuna.
- Offline: rejestracja zgody wymaga sieci, bo zapisuje rozliczalny ślad - akcja główna
  KID-01 jest wtedy wstrzymana z wyjaśnieniem. KID-02 offline pokazuje ostatnio
  zsynchronizowane dane z datą synchronizacji.
- Brak uprawnień: drugi opiekun o zawężonym zakresie albo osoba bez władzy rodzicielskiej
  nie może wyrazić zgody za dziecko - ekran tłumaczy, kto może to zrobić. Cofnięcie
  dostępu nastolatka przez opiekuna zamyka KID-02 i kieruje do informacji, kto go wyłączył.
- Stan systemu zawsze widoczny: postęp zgód, status rejestracji, znacznik trybu odczytu
  i data synchronizacji w KID-02.
- Komunikaty walidacji to dokładnie te z opisu ekranu w `KID_spec.md`, mówią co poszło
  nie tak i jak to naprawić (brak wymaganej zgody z nazwą celu, zgoda zaznaczona z góry
  niedozwolona, zmiana wersji treści prosząca o ponowne przejrzenie, zgoda art. 9
  wymagająca osobnego potwierdzenia).
- Decyzja prawna przed wdrożeniem jest świadomie nierozstrzygnięta w makiecie: próg
  wieku samodzielnej zgody, reguła przy dwojgu opiekunów oraz zakres i samo istnienie
  widoku nastolatka zostają jako miejsce do wypełnienia, nie jako przesądzenie
  (Ustawa o ochronie danych osobowych, 2018).
- WCAG 2.1 AA: cel dotykowy min `--tap` (48 px), kontrast tekstu AA na jasnym tle,
  widoczny `:focus-visible` na akcencie. Heurystyki według Nielsena (1994), WCAG
  według W3C (2018).
- Motywowanie wyłącznie przez zmienne CSS. Żadnego `localStorage` ani `sessionStorage`.

## Konwencja nazw plików

`{ID}_{slug-bez-polskich-znaków}_hifi.html`, np.
`KID-01_zgody-opiekuna_hifi.html`.

## Wzorzec generacji jednego ekranu

1. Doczytaj opis ekranu w `KID_spec.md` i odpowiedni szkic w `KID_szkice_lo-fi.md`.
2. Weź z opisu: cel, akcję główną, akcje drugorzędne i niszczące, stany, walidacje,
   przypadki brzegowe, podstawę prawną, lokalizację oraz to, czy ekran obsługuje
   opiekun (KID-01) czy nastolatek (KID-02).
3. Stan alternatywny do przełącznika bierz z komentarza pod szkicem lo-fi. Dla KID-01
   to offline ze wstrzymaną akcją główną albo „brak uprawnień" dla drugiego opiekuna
   bez władzy rodzicielskiej; dla KID-02 to offline z datą synchronizacji albo moment,
   w którym opiekun cofa dostęp i widok się zamyka.
4. Realne treści po polsku, bez lorem ipsum. Dziecko wzorcowe: Zofia Kowalska
   (pływanie, Klub Pływacki Delfin). Opiekun wzorcowy: Anna Kowalska, zalogowana,
   władza rodzicielska. Zgody wzorcowe: udział w zajęciach (wymagana, art. 6),
   wizerunek w galeriach (dobrowolna), dane o zdrowiu na wyjazd lub obóz (osobna,
   art. 9). Wersje i daty zgód jak w szkicu lo-fi.

## Powiązania między grupami

KID jest cienką warstwą nad światem rodziny. Zgody (KID-01) wyzwalają się z blokad
zapisu dziecka w PAR-04 i PAR-11, kontekst dziecka pochodzi z PAR-03, dokumenty
z PAR-09, zarządzanie zgodami i zakresem drugiego opiekuna z PAR-16, pełne treści
polityk ze stron prawnych GST-08. Widok nastolatka (KID-02) włącza i wyłącza opiekun
w PAR-03; dane czerpie z postępu (INS-05, PAR-08), paszportu (OCL-17, PAR-13) i zdjęć
(PAR-15). Widok nastolatka nie prowadzi do żadnego ekranu płatności, zgody ani zapisu.

## Uwaga prawna do całej grupy

Oba ekrany mają w manifeście „decyzja prawna przed wdrożeniem: TAK". KID-01 świadomie
nie rozstrzyga reguły przy dwojgu opiekunów ani progu wieku dla danego organizatora -
zostawia na to miejsce. KID-02 nie przesądza, czy widok w ogóle udostępniać, od jakiego
wieku, w jakim zakresie danych, kto kontroluje poświadczenia ani czy wyłączyć dane
o zdrowiu z widoku dziecka. Bez tych rozstrzygnięć ekrany nie wchodzą do wdrożenia -
makieta hi-fi ma je zilustrować, nie zadekretować.

## Czego brakuje do startu generacji

Do tego okna trzeba wkleić `KID_spec.md` (opisy KID-01 i KID-02) oraz
`KID_szkice_lo-fi.md`. Bez nich można policzyć zakres z manifestu, ale nie da się
wyciągnąć celów, walidacji, podstaw prawnych i stanów alternatywnych.

## Stan prac

Grupa KID: lo-fi gotowe dla KID-01 i KID-02, hi-fi do zrobienia. Sugerowana kolejność:
KID-01 (zgody opiekuna) jako ekran ustalający język wizualny grupy i wzorzec sekcji
danych szczególnej kategorii; potem KID-02 (widok nastolatka tylko do odczytu).
Obie makiety przed wdrożeniem wymagają rozstrzygnięcia decyzji prawnej opisanej wyżej.

## Literatura

Europejska Rada Ochrony Danych. (2020). *Wytyczne 05/2020 dotyczące zgody na mocy
rozporządzenia 2016/679* (wersja 1.1). European Data Protection Board.
https://www.edpb.europa.eu/our-work-tools/our-documents/guidelines/guidelines-052020-consent-under-regulation-2016679_pl

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman
Group. https://www.nngroup.com/articles/ten-usability-heuristics/

Rozporządzenie Parlamentu Europejskiego i Rady (UE) 2016/679 z dnia 27 kwietnia 2016 r.
w sprawie ochrony osób fizycznych w związku z przetwarzaniem danych osobowych [RODO].
(2016). Dziennik Urzędowy Unii Europejskiej L 119/1.
https://eur-lex.europa.eu/legal-content/PL/TXT/?uri=CELEX%3A32016R0679

Ustawa z dnia 10 maja 2018 r. o ochronie danych osobowych. (2018). Dziennik Ustaw 2018
poz. 1000. https://isap.sejm.gov.pl/isap.nsf/DocDetails.xsp?id=WDU20180001000

W3C. (2018). *Web Content Accessibility Guidelines (WCAG) 2.1*. World Wide Web
Consortium. https://www.w3.org/TR/WCAG21/
