# Szkice lo-fi - grupa OCL (panel organizatora, tryb zajęć)

Osiemnaście ekranów z `OCL_spec.md`, każdy jako blok w czcionce o stałej szerokości. Bez kolorów. Pod każdym szkicem trzy zdania: co dominuje, co może umknąć, jaki stan alternatywny pokazać w makiecie hi-fi.

Lista kontrolna heurystyk w każdym ekranie odnosi się do dziesięciu heurystyk użyteczności (Nielsen, 1994), a wymóg kontrastu i celu dotykowego do WCAG 2.1 AA (W3C, 2018).

**Legenda znaków**

```
┌─┐ │ └─┘   ramka sekcji          [ Akcja ]      przycisk drugorzędny
[[ AKCJA ]] akcja główna          ( ........ )   pole tekstowe
[v]         lista rozwijana       [x] / [ ]      pole wyboru zaznaczone / puste
(•) / ( )   przełącznik radio     ≡              menu boczne
‹STAN: …›   wskaźnik stanu ekranu ⚠ WALIDACJA    miejsce komunikatu walidacji
░░░░        szkielet (ładowanie)  ▾              rozwinięcie
```

---

## OCL-01 - dashboard zajęć

```
┌──────────────────────────────────────────────────────────────┐
│ [LOGO placówki]   Tryb: Zajęcia[v]   Język: PL[v]   konto ▾   │
├──────────────────────────────────────────────────────────────┤
│ Placówka: Wszystkie[v]   Zakres: Dziś[v]      ‹STAN: wypełniony›│
├────────────┬────────────┬────────────┬───────────┬───────────┤
│ DZIŚ ZAJĘĆ │ FREKWENCJA │ NALEŻNOŚCI │ REZERWOWA │ ZGODY      │
│   8        │ 41 / 52    │ 1 240 zł   │  6 osób   │ 3 wygasają │
│            │            │ ⚠ 4 przeterm.│         │            │
└────────────┴────────────┴────────────┴───────────┴───────────┘
┌── OŚ CZASU DNIA ─────────────────┐ ┌── ALERTY (priorytet) ───────┐
│ 16:00 Żabki  sala A  [instr.: —] │ │ ! Brak instruktora 16:00     │
│ 17:00 Delfiny tor 2  Kowalska    │ │ ! 4 płatności przeterminowane│
│ 18:00 Rekiny  tor 1  Nowak       │ │ • 6 rodzin na liście rezerw. │
│ … (zwiń przy wielu grupach)      │ │                              │
└──────────────────────────────────┘ └──────────────────────────────┘
┌── FREKWENCJA TYGODNIOWA ─────────────────────────────────────┐
│  ▁▃▅▇▆▄▂   (wykres, dane zagregowane)                        │
└──────────────────────────────────────────────────────────────┘

        [[ PRZEJDŹ DO NAJPILNIEJSZEGO ALERTU ]]
  [ Filtr placówki ]  [ Zmień zakres dat ]  [ Grafik ]  [ Płatności ]

⚠ WALIDACJA: błąd dotyczy pojedynczego kafelka, nie całego ekranu
   (np. "Nie udało się pobrać należności - Ponów")
```

Dominuje pasek pięciu kafelków wskaźnikowych i jedna ścieżka „idź tam, gdzie pali się najmocniej”. Umknąć może to, że oś czasu i lista alertów konkurują o uwagę po prawej i lewej stronie, więc przy wielu grupach trzeba je zwijać, inaczej akcja główna ginie na dole. W hi-fi pokażę stan pusty dla świeżo założonego konta - powitanie organizatora bez grup i wyraźny skok do kreatora grupy (OCL-02), bo to moment, w którym pulpit decyduje, czy ktoś zostanie.

---

## OCL-02 - zarządzanie grupami i kursami z poziomami

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Grupy i kursy            Tryb: Zajęcia[v]  PL[v]   konto ▾  │
├──────────────────────────────────────────────────────────────┤
│ Szukaj ( ............ )   Filtr poziom[v]  ‹STAN: wypełniony› │
├──────────────────────────────────────────────────────────────┤
│ NAZWA       POZIOM    DZIEŃ/GODZ.  INSTRUKTOR  MIEJSCA  STATUS │
│ Delfiny     P2        Wt 17:00     Kowalska    8/10     publ.  │
│ Żabki       P1        Śr 16:00     —           5/12     ⚠ ukr. │
│ Rekiny      P3        Pt 18:00     Nowak       10/10    pełna  │
├──────────────────────────────────────────────────────────────┤
│ ┌── PANEL EDYCJI GRUPY (boczny) ──────────────────────────┐   │
│ │ Nazwa     ( Delfiny ............ )                       │   │
│ │ Poziom    [P2 ▾]  → otwiera drzewo (OCL-10)              │   │
│ │ Instruktor[Kowalska ▾]                                   │   │
│ │ Limit     [—|||||----] 10                                │   │
│ │ Zapisy    [x] widoczne dla rodzica                       │   │
│ │ ⚠ WALIDACJA: limit musi być dodatni; bez poziomu rodzic │   │
│ │   nie zobaczy grupy; kolizja terminu z grupą "Karpie"   │   │
│ └──────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘

   [[ DODAJ GRUPĘ ]]   (na karcie istniejącej: [[ ZAPISZ ZMIANY ]])
 [ Duplikuj na semestr ]  [ Archiwizuj ]  [ Zmień instruktora ]  [ Uczestnicy ]
 NISZCZĄCE: [ Usuń grupę ]  (tylko gdy 0 zapisanych, inaczej archiwizacja)
```

Dominuje tabela grup, a edycja wjeżdża z boku, żeby nie tracić kontekstu listy. Łatwo przeoczyć powiązanie poziomu z drzewem umiejętności - bez przypisanego poziomu publikacja zapisów jest zablokowana, a komunikat o tym żyje wewnątrz panelu bocznego, gdzie bywa niewidoczny. W hi-fi pokażę stan ostrzeżenia o konflikcie terminów: ten sam instruktor w dwóch grupach o tej samej godzinie, z nazwą kolidującej grupy i podpowiedzią wolnego okna.

---

## OCL-03 - grafik i kalendarz zajęć

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Grafik    [Dzień][Tydzień•][Miesiąc]   PL[v]   ‹wypełniony›│
├──────────────────────────────────────────────────────────────┤
│ Filtr: Sala[v]  Instruktor[v]  Poziom[v]   ⚠ KOLIZJA: tor 2   │
├──────┬──────┬──────┬──────┬──────┬──────┬──────┬──────────────┤
│ godz │ Pon  │ Wt   │ Śr   │ Czw  │ Pt   │ Sob  │              │
│ 16:00│      │      │ Żabki│      │      │ obóz │  ← przeciągaj │
│ 17:00│Delfny│Delfny│      │      │      │      │     bloki     │
│ 18:00│      │      │ ⚠kol.│      │Rekiny│      │              │
│ święto: czw (kalendarz regionu)                               │
├──────────────────────────────────────────────────────────────┤
│ ┌ PANEL BLOKU: Delfiny, Wt 17:00, tor 2, Kowalska ──────────┐ │
│ │ [ Odwołaj ] [ Zastępstwo ] [ Powiel na semestr ] [ Drukuj ]│ │
│ └────────────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────┘

   [[ DODAJ ZAJĘCIA ]]   (na bloku: [[ ZAPISZ PRZESUNIĘCIE ]])
 NISZCZĄCE: [ Usuń termin z serii ]  → pyta: to wystąpienie / cała seria
⚠ WALIDACJA: przesunięcie na zajętą salę cofa blok i wskazuje wolne okno;
   odwołanie pyta: powiadomić rodziców? otworzyć odrabianie?
‹STAN OFFLINE: baner "Pokazuję ostatni grafik, zmiany wstrzymane do sieci"›
```

Dominuje siatka tygodnia z przeciąganiem bloków - cały plan placówki widać od razu. Umknąć może zachowanie offline, bo grafik bywa otwierany przy recepcji ze słabym łączem, a baner o wstrzymaniu zmian trzeba pokazać zanim ktoś zacznie przeciągać i zdziwi się, że nic się nie zapisuje. W hi-fi pokażę stan kolizji sali w trakcie przeciągania: blok zatrzymany, czerwony obrys zastąpiony wzorem kreskowania (bez koloru w lo-fi) i dymek „tor 2 zajęty przez Rekiny - wolne okno 18:00”.

---

## OCL-04 - reguły odrabiania nieobecności

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Reguły odrabiania              PL[v]      ‹STAN: wypełniony›│
├───────────────────────────────────┬──────────────────────────┤
│ AKTYWNE REGUŁY                     │ FORMULARZ REGUŁY          │
│ [x] Zgłoszenie >24h przed zajęc.   │ Nazwa ( ............... ) │
│ [x] Max 3 odrobienia / semestr     │ Okno zgłosz. ( 24 ) godz. │
│ [ ] Tylko ten sam poziom           │ Limit/semestr ( 3 )       │
│                                    │ Poziomy [P1][x][P2][x]    │
│ (pusty stan: "Zacznij od gotowego  │ Ważność prawa ( 30 ) dni  │
│  szablonu" + krótkie wyjaśnienie)  │ ┌ PODGLĄD NA PRZYKŁADZIE ┐│
│                                    │ │ Dziecko X, nieob. 5.05  ││
│                                    │ │ → może odrobić do 4.06  ││
│                                    │ └─────────────────────────┘│
│                                    │ ⚠ WALIDACJA: okno ≥ 0;    │
│                                    │   limit całkowity; reguła │
│                                    │   bez poziomu → "wszystkie│
│                                    │   poziomy?"; sprzeczne    │
│                                    │   reguły → która ma       │
│                                    │   pierwszeństwo           │
└───────────────────────────────────┴──────────────────────────┘

   [[ ZAPISZ REGUŁĘ ]]   (pusta lista: [[ UTWÓRZ Z SZABLONU ]])
 [ Dezaktywuj ]  [ Sklonuj na inny poziom ]  [ Przetestuj na uczestniku ]
 NISZCZĄCE: [ Usuń regułę ]  (aktywne prawa zostają do wygaśnięcia)
```

Dominuje podział na listę aktywnych reguł i formularz z podglądem działania na konkretnym przykładzie. Umknąć może okno zgłaszania nieobecności mylone z oknem ważności prawa do odrobienia - dwa różne czasy, które przy pośpiechu łatwo pomylić, więc opis przy polu musi mówić językiem rodzica, nie systemu. W hi-fi pokażę stan sprzecznych reguł dla tego samego poziomu, z jawnym wskazaniem, która wygrywa i dlaczego.

---

## OCL-05 - listy rezerwowe z automatycznym dopisaniem

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Lista rezerwowa: Rekiny P3     PL[v]   ‹STAN: wypełniony›  │
├──────────────────────────────────────────────────────────────┤
│ Tryb dopisania:  (•) Automatyczny   ( ) Ręczny                │
│ Okno na potwierdzenie ( 24 ) godz.   Następny: rodzina Nowak  │
├──────────────────────────────────────────────────────────────┤
│ # │ DZIECKO     │ ZGŁOSZ.  │ KONTAKT       │ STATUS OFERTY     │
│ 1 │ Anna N.     │ 02.05    │ tel/mail      │ oferta wysłana ⏳ │
│ 2 │ Piotr K.    │ 04.05    │ tel/mail      │ oczekuje          │
│ 3 │ Maja W.     │ 05.05    │ tel/mail      │ odmowa 03.05      │
│ HISTORIA OFERT I ODMÓW ▾                                       │
└──────────────────────────────────────────────────────────────┘

   [[ ZAOFERUJ ZWOLNIONE MIEJSCE NASTĘPNEJ OSOBIE ]]
   (tryb automatyczny: [[ WŁĄCZ AUTOMAT ]])
 [ Przesuń ręcznie (z powodem) ]  [ Wstrzymaj automat ]  [ Przedłuż okno ]
 NISZCZĄCE: [ Usuń zgłoszenie ]  (rodzic dostanie powiadomienie o wypisaniu)
⚠ WALIDACJA: okno na potwierdzenie ≠ 0; ręczne przesunięcie wymaga powodu
   (kolejka musi być audytowalna); wygasła oferta → miejsce wraca do puli
‹STAN OFFLINE: baner o wstrzymaniu zmian; STAN PUSTY: wolne miejsca, brak kolejki›
```

Dominuje kolejka z numerami pozycji i statusem oferty, a przełącznik trybu auto/ręczny stoi nad nią jako pierwsza decyzja. Łatwo przeoczyć wymóg uzasadnienia przy ręcznym przesunięciu - bez tego historia kolejki przestaje być audytowalna, a spór z rodzicem „dlaczego mnie wyprzedzono” nie ma rozstrzygnięcia. W hi-fi pokażę stan oferty z odliczającym czasem (timer okna na potwierdzenie) i moment eskalacji do kolejnej osoby po wygaśnięciu.

---

## OCL-06 - zapisy na obozy i półkolonie

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Obozy i półkolonie - kreator   PL[v]   ‹STAN: wypełniony›  │
├──────────────────────────────────────────────────────────────┤
│ Kroki:  Dane ▸ Daty ▸ Limit ▸ Dokumenty ▸ Warianty ▸ Publik.  │
├──────────────────────────────────────────────────────────────┤
│ TURNUS:  Nazwa ( Lato I ........ )                            │
│ Daty     od [01.07] do [05.07]    Limit ( 24 )                │
│ Cena     ( 850 zł )  Próg wczesny [x] do [15.05] ( 750 zł )   │
│ DOKUMENTY:  [x] Zgoda udziału  [x] Zgoda medyczna  [ ] RODO   │
│ WARIANTY:   wyżywienie [v]   transport [v]                    │
│ Zapisani: 0 / 24                                              │
│ ┌ PODGLĄD OFERTY OCZAMI RODZICA ▾ ─────────────────────────┐ │
│ └────────────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────┘

   [[ DALEJ ]]  →  na końcu kreatora: [[ OPUBLIKUJ TURNUS ]]
 [ Zapisz wersję roboczą ]  [ Duplikuj turnus ]  [ Podgląd publiczny ]
 NISZCZĄCE: [ Usuń turnus ]  (tylko bez zapisanych dzieci, inaczej archiwum)
⚠ WALIDACJA: data końca ≥ początek; limit dodatni; cena ≥ 0;
   publikacja bez kompletu zgód wskazuje, których brakuje i po co
```

Dominuje pasek kroków kreatora i jedna akcja „dalej”, która na końcu zmienia się w publikację. Umknąć mogą zgody medyczne przy obozie wyjazdowym - to dane szczególnej kategorii pod RODO, a kreator musi zablokować publikację, dopóki pakiet dokumentów nie jest kompletny, i wytłumaczyć po co. W hi-fi pokażę stan startu z duplikatu zeszłorocznego turnusu, bo to najczęstsza droga organizatora i skraca tworzenie oferty o większość kroków.

---

## OCL-07 - cennik i model płatności

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Cennik i model płatności       PL[v]   ‹STAN: wypełniony›  │
├───────────────────────────────────┬──────────────────────────┤
│ PLANY CENOWE                       │ FORMULARZ PLANU           │
│ [x] Abonament miesięczny  120 zł   │ Typ [Abonament mies. ▾]   │
│ [x] Karnet 8 wejść        160 zł   │ Cena netto ( ..... )      │
│ [ ] Semestralny           550 zł   │ Cena brutto ( ..... )     │
│                                    │ ┌ ZNIŻKI RODZINNE ───────┐ │
│ METODY: Przelewy24 PayU Stripe     │ │ 2. dziecko  -10%        ││
│         BLIK  karty                │ │ 3. dziecko  -20%        ││
│ ┌ KALKULACJA: rodzina 2 dzieci ──┐ │ └─────────────────────────┘│
│ │ 120 + 108 = 228 zł / mies.     │ │ ⚠ WALIDACJA: cena ≥ 0;    │
│ └────────────────────────────────┘ │   suma zniżek < cena;     │
│                                    │   plan bez grupy → "nie   │
│                                    │   będzie widoczny przy    │
│                                    │   zapisie"; zmiana ceny   │
│                                    │   aktywnego → nowe czy też│
│                                    │   trwające abonamenty?    │
└───────────────────────────────────┴──────────────────────────┘

   [[ ZAPISZ PLAN CENOWY ]]   (pusta lista: [[ DODAJ PIERWSZY PLAN ]])
 [ Sklonuj plan ]  [ Ustaw zniżkę rodzinną ]  [ Podgląd kalkulacji ]  [ Dezaktywuj ]
 NISZCZĄCE: [ Usuń plan ]  (tylko gdy żaden aktywny zapis go nie używa)
```

Dominuje lista planów obok formularza, a żywa kalkulacja dla rodziny z dwójką dzieci tłumaczy model „oderwany od liczby główek” na konkretnej liczbie. Umknąć może pytanie przy zmianie ceny aktywnego planu - czy dotyczy tylko nowych zapisów, czy też trwających abonamentów - bo zła odpowiedź uderza w portfele rodzin w trakcie cyklu. W hi-fi pokażę stan walidacji, gdzie suma zniżek przekracza cenę i kwota końcowa spadłaby poniżej zera, z jasnym komunikatem blokującym zapis.

---

## OCL-08 - lista uczestników i rodzin

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Uczestnicy i rodziny    Widok: (•)Dzieci ( )Rodziny  PL[v] │
├──────────────────────────────────────────────────────────────┤
│ Szukaj ( ........ )  Grupa[v] Poziom[v] Płatn.[v] Dok.[v]     │
│ ‹STAN: wypełniony›                                             │
├───┬─────────────┬───────┬──────────┬─────────┬───────┬────────┤
│[ ]│ DZIECKO     │ GRUPA │ RODZIC    │ PŁATN.  │ DOK.  │ FREKW. │
│[x]│ Anna Nowak  │ Delfny│ J. Nowak  │ ● ok    │ ● ok  │ 92%    │
│[x]│ Piotr Nowak │ Żabki │ J. Nowak  │ ⚠ zaleg.│ ○ brak│ 70%    │
│[ ]│ Maja Wójcik │ Rekiny│ A. Wójcik │ ● ok    │ ⚠ wyg.│ 85%    │
│   (status kolorami semantycznymi - tu znaczniki ● ○ ⚠)        │
├──────────────────────────────────────────────────────────────┤
│ Zaznaczono 2 → AKCJE MASOWE: [ Eksport ] [ Komunikat ] [ Przenieś ]│
└──────────────────────────────────────────────────────────────┘

   [[ OTWÓRZ KARTĘ UCZESTNIKA ]]
 [ Filtruj ]  [ Eksport zaznaczenia ]  [ Komunikat do segmentu ]  [ Przenieś do grupy ]
 NISZCZĄCE: [ Wypisz z grupy ]  → zachować historię i paszport? zwrot do portfela?
⚠ WALIDACJA: filtr bez wyników → "wyczyść filtry"; akcja masowa pomija rekordy
   bez uprawnień i mówi ile i dlaczego; przeniesienie na inny poziom ostrzega
‹STAN OFFLINE: ostatnia lista w odczycie z banerem; BRAK UPRAWNIEŃ: ukryte kolumny wrażliwe›
```

Dominuje filtrowalna tabela z przełącznikiem widoku dzieci kontra rodziny - jeden ekran zamiast przeklikiwania kilku list. Umknąć może łączenie rodzeństwa pod jednym kontem rodzinnym i wspólnym portfelem, bo akcja masowa na „rodzinie z dziećmi w obu modułach” musi scalać, a nie dublować rekordy. W hi-fi pokażę stan wykrytego duplikatu konta rodzica po e-mailu lub telefonie, z propozycją scalenia, bo to typowe źródło bałaganu w danych recepcji.

---

## OCL-09 - karta uczestnika z dokumentami

```
┌──────────────────────────────────────────────────────────────┐
│ ‹ Wróć    Anna Nowak  [◂ rodzeństwo ▸]   PL[v]  ‹wypełniony›  │
├──────────────────────────────────────────────────────────────┤
│ [Dane][Postęp][Dokumenty•][Płatności][Obecność][Paszport]     │
├──────────────────────────────────────────────────────────────┤
│ DOKUMENTY                                                      │
│  • Zgoda RODO        ważna do 2026-09-01    [podgląd]         │
│  • Orzeczenie lek.   ⚠ WYGASŁ 2026-04-10    [podgląd]         │
│  • Zgoda na wizerunek ○ brak                                  │
│ ┌ SKRÓT: grupa Delfiny P2 · saldo portfela 40 zł · frekw. 92% ┐│
│ └──────────────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────┘

   [[ EDYTUJ DANE / STATUS ]]  (domyślnie zapis zmian w profilu)
 [ Dodaj dokument ]  [ Wiadomość do rodzica ]  [ Przenieś do grupy ]  [ Płatność ręczna ]
 NISZCZĄCE: [ Usuń dokument ] [ Wypisz dziecko ]  (archiwizacja + ślad audytu)
⚠ WALIDACJA: data ważności w przeszłości → "wygasły"; brak wymaganej zgody
   blokuje udział w zajęciach; edycja danych medycznych wymaga potwierdzenia
‹BRAK UPRAWNIEŃ: ukryte dane medyczne i finansowe; OFFLINE: odczyt z banerem›
```

Dominuje nagłówek profilu z przełącznikiem między rodzeństwem i zakładki, które trzymają pełny obraz dziecka w jednym miejscu. Umknąć mogą dane wrażliwe - orzeczenie medyczne to szczególna kategoria pod RODO, więc zakładka musi być ukryta przy braku uprawnień, a jej edycja osłonięta dodatkowym potwierdzeniem. W hi-fi pokażę stan wygasłego dokumentu blokującego udział w zajęciach, z czytelnym ostrzeżeniem i skrótem „poproś rodzica o nowy”.

---

## OCL-10 - definicja drzewa umiejętności

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Drzewo umiejętności: P2        PL[v]   ‹STAN: wypełniony›  │
├───────────────────────────────────┬──────────────────────────┤
│ STRUKTURA (hierarchia)             │ UMIEJĘTNOŚĆ               │
│ ▾ Pływanie - podstawy              │ Nazwa ( Oddychanie ... )  │
│   • Zanurzenie twarzy              │ Opis  ( ............... ) │
│   • Wydech do wody                 │ Kryterium ( 5 wydechów )  │
│ ▾ Technika kraul                   │ Film [ + dołącz ]         │
│   • Praca nóg                      │ ┌ PODGLĄD ───────────────┐│
│   • Oddychanie  ◂ edytowane        │ │ ( ) rodzic  (•) instr.  ││
│ wersja 4 · historia zmian ▾        │ └─────────────────────────┘│
│                                    │ ⚠ WALIDACJA: nazwa wymagana│
│ IMPORT Z ARKUSZA [ wgraj plik ]    │   import: błędny wiersz 7 │
│                                    │   (brak kolumny "poziom");│
│                                    │   cykl w gałęziach; format│
│                                    │   filmu nieobsługiwany    │
└───────────────────────────────────┴──────────────────────────┘

   [[ ZAPISZ DRZEWO ]]   (pusty stan: [[ UTWÓRZ GAŁĄŹ ]] lub [[ IMPORTUJ ]])
 [ Dodaj gałąź ]  [ Dołącz film ]  [ Importuj z arkusza ]  [ Podgląd rodzica ]  [ Sklonuj na poziom ]
 NISZCZĄCE: [ Usuń umiejętność/gałąź ]  (postęp dzieci archiwizowany, nie kasowany)
```

Dominuje edytor hierarchii obok formularza pojedynczej umiejętności, a obok niego równorzędna droga importu z arkusza. Umknąć może walidacja importu - przy pliku z polskimi i angielskimi nazwami w jednej kolumnie komunikat musi wskazać konkretny wiersz i oczekiwany format, inaczej organizator porzuci arkusz i wróci do ręcznego klikania. W hi-fi pokażę stan błędu importu z podświetlonym wierszem 7 i instrukcją naprawy, bo to ekran, który obiecuje „aktualizację z prostego arkusza” i na tej obietnicy stoi.

---

## OCL-11 - przegląd obecności i frekwencji

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Obecność i frekwencja          PL[v]   ‹STAN: wypełniony›  │
├──────────────────────────────────────────────────────────────┤
│ Grupa[v]  Okres: Semestr[v]   [ Eksport ]                     │
├──────────────────────────────────────────────────────────────┤
│ FREKWENCJA GRUPOWA   ▇▆▇▅▆▇▄▆  (trend w czasie)               │
├───┬─────────────┬─────────┬──────────┬───────────────────────┤
│   │ DZIECKO     │ TREND   │ NIEOBEC. │ PRAWO DO ODROBIENIA    │
│ ! │ Piotr K.    │ ↘ spada │ 4 (2 us.)│ 1 dostępne do 04.06    │
│   │ Anna N.     │ → stała │ 1        │ —                      │
│ ! │ Maja W.     │ ↘ ryzyko│ 5        │ 2 dostępne             │
│   wskaźnik ryzyka rezygnacji oparty na trendzie               │
└──────────────────────────────────────────────────────────────┘

   [[ OTWÓRZ PRZYPADEK WYMAGAJĄCY REAKCJI ]]
 [ Filtruj okres/grupę ]  [ Eksportuj ]  [ Przypomnienie do rodzica ]  [ Oznacz usprawiedliwioną ]
 NISZCZĄCE: nie dotyczy (korekta wpisu = edycja z zapisem zmiany)
⚠ WALIDACJA: zakres dat - koniec ≥ początek; eksport bez zakresu → bieżący
   semestr (informuje); późna korekta ostrzega o wpływie na statystyki i odrabiania
‹STAN PUSTY: brak odnotowanych zajęć - "dane po pierwszych listach obecności"›
```

Dominuje tabela dzieci z trendem i kolumną wskaźnika ryzyka, która zamienia surowe liczby nieobecności w sygnał „zadzwoń, zanim odejdą”. Umknąć może rozróżnienie nieobecności dziecka od zajęć odwołanych przez placówkę - liczone inaczej, a zlane w jeden słupek fałszują trend i podnoszą fałszywe alarmy. W hi-fi pokażę stan ostrzeżenia przy korekcie obecności po długim czasie, bo taka zmiana cofa się do statystyk i praw do odrobienia i powinna wymagać świadomej decyzji.

---

## OCL-12 - zarządzanie instruktorami

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Instruktorzy                   PL[v]   ‹STAN: wypełniony›  │
├──────────────────────────────────────────────────────────────┤
│ INSTRUKTOR    STATUS      GRUPY        KWALIF.      GODZINY    │
│ Kowalska      aktywna     Delfiny,P2   ważne        32 h       │
│ Nowak         aktywny     Rekiny       ⚠ wygasa 1.06 28 h      │
│ Lis           zaproszony  —            —            —          │
├───────────────────────────────────┬──────────────────────────┤
│ PRZYPISANIA DO GRUP                │ DOSTĘPNOŚĆ (z INS-07) ▾   │
│ Kowalska → Delfiny Wt 17:00        │ KWALIFIKACJE + ważność ▾  │
│ ⚠ konflikt: 17:00 także Żabki?     │ GODZINY (z INS-08) ▾      │
└───────────────────────────────────┴──────────────────────────┘

   [[ ZAPROŚ INSTRUKTORA ]]  /  [[ PRZYPISZ DO GRUPY ]]  (zależnie od stanu)
 [ Edytuj kwalifikacje ]  [ Ustaw zastępstwo ]  [ Podejrzyj godziny ]  [ Dezaktywuj ]
 NISZCZĄCE: [ Usuń przypisanie ] [ Dezaktywuj konto ]  (historia zachowana)
⚠ WALIDACJA: kolizja terminu → wolne okno; wygasła kwalifikacja blokuje
   przypisanie do grupy jej wymagającej; zajęty e-mail → "osoba ma już konto, powiąż?"
```

Dominuje tabela kadry ze statusem zaproszenia, przypisaniami i godzinami w jednym rzędzie. Umknąć może wygasająca kwalifikacja w środku semestru - jeśli ekran nie zablokuje przypisania do grupy wymagającej tej kwalifikacji, organizator dowie się o problemie dopiero przy kontroli. W hi-fi pokażę stan zastępstwa łańcuchowego, gdy wskazany zastępca też jest niedostępny, bo to przypadek brzegowy, który rozsadza naiwny formularz „wybierz zastępcę”.

---

## OCL-13 - konfiguracja rezerwacji lekcji indywidualnych

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Rezerwacje 1:1 - reguły        PL[v]   ‹STAN: wypełniony›  │
├───────────────────────────────────┬──────────────────────────┤
│ REGUŁA REZERWACJI                  │ PODGLĄD WYNIKOWYCH OKIEN  │
│ Trener   [Kowalska ▾]              │ Pon  10:00 11:00 (wolne)  │
│ Obiekt   [tor 2 ▾]                 │ Śr   16:00 (zajęte grupą) │
│ Czas lekcji ( 45 ) min             │ Pt   09:00 10:00          │
│ Okno wyprzedzenia ( 12 ) godz.     │                           │
│ Okno odwołania ( 24 ) godz.        │ ⚠ WALIDACJA: okna ≥ 0;    │
│ Bufor między lekcjami ( 15 ) min   │   trener bez dostępności  │
│ Przedpłata  [x] wymagana           │   → 0 okien, idź do INS-07│
│                                    │   kolizja obiektu z grupą;│
│                                    │   bufor > przerwa → okna  │
│                                    │   znikną                  │
└───────────────────────────────────┴──────────────────────────┘

   [[ ZAPISZ REGUŁĘ REZERWACJI ]]   (pusty stan: [[ UTWÓRZ PIERWSZĄ ]])
 [ Sklonuj dla innego trenera ]  [ Ustaw bufor ]  [ Włącz przedpłatę ]  [ Podgląd okien ]
 NISZCZĄCE: [ Usuń regułę ]  (istniejące rezerwacje zostają, blokuje tylko nowe)
```

Dominuje formularz reguły obok żywego podglądu wynikowych okien - widać natychmiast, co rodzic zobaczy po zapisaniu. Umknąć może sprzężenie dostępności trenera z dostępnością obiektu, bo trener wolny przy zajętym torze daje zero okien, a bez wyjaśnienia „dlaczego pusto” konfiguracja wygląda na zepsutą. W hi-fi pokażę stan, w którym bufor jest dłuższy niż przerwa między blokami dostępności i wszystkie okna znikają, z ostrzeżeniem prowadzącym do korekty.

---

## OCL-14 - płatności, portfel i faktury

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Płatności, portfel, faktury    PL[v]   ‹STAN: wypełniony›  │
├──────────────────────────────────────────────────────────────┤
│ Filtr: status[v] metoda[v] okres[v]    Należności: 1 240 zł   │
├──────────────────────────────────────┬───────────────────────┤
│ TRANSAKCJE                            │ PORTFEL RODZINY Nowak  │
│ 12.05 Nowak  120 zł  P24   ● opłac.   │ saldo: 40 zł           │
│ 11.05 Wójcik 160 zł  BLIK  ⏳ w toku  │ wspólny: zajęcia+zawody│
│ 10.05 Lis     85 zł  Stripe⚠ zaległ.  │ [ Doładuj ]            │
│ metody: Przelewy24 PayU Stripe BLIK karty                      │
├──────────────────────────────────────┴───────────────────────┤
│ FAKTURA: nabywca [v]  ⚠ brak NIP   [ Generuj ]                │
└──────────────────────────────────────────────────────────────┘

   [[ ROZLICZ NAJPILNIEJSZĄ NALEŻNOŚĆ ]]  /  [[ WYSTAW FAKTURĘ ]]
 [ Filtruj ]  [ Doładuj portfel ]  [ Pobierz fakturę ]  [ Płatność gotówką ]
 NISZCZĄCE: [ Zwrot środków ]  (wyraźne potwierdzenie: kwota + powód, bez kasowania historii)
⚠ WALIDACJA: zwrot > wpłaty blokuje z limitem; faktura bez danych nabywcy →
   brakujące pole; doładowanie kwotą ujemną odrzucone; płatność w toku oznaczona
   (nie obciążaj podwójnie)
‹STAN: błąd bramki → "płatność w toku, nie ponawiaj do potwierdzenia"; OFFLINE: odczyt›
```

Dominuje tabela transakcji obok panelu portfela rodziny, a portfel jest jawnie wspólny dla zajęć i zawodów - to oś całego modelu rozliczeń. Umknąć może stan „płatność w toku” po zerwanej sesji bramki, bo bez wyraźnego oznaczenia recepcja ponawia operację i obciąża rodzinę podwójnie. W hi-fi pokażę stan zwrotu jako operacji finansowej: modal z kwotą, powodem i potwierdzeniem, z blokadą zwrotu przekraczającego wpłaconą kwotę.

---

## OCL-15 - komunikacja

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Komunikacja                    PL[v]   ‹STAN: wypełniony›  │
├───────────────────────────────────┬──────────────────────────┤
│ WYSŁANE / ZAPLANOWANE             │ EDYTOR WIADOMOŚCI         │
│ • Odwołanie Żabki  dostarcz. 12/12│ Segment [Rodzice zaleg.▾] │
│ • Przypomnienie    otw. 60%       │ Kanał  [x]e-mail [x]push  │
│ SZABLONY ▾                        │        [ ]SMS (limit zn.) │
│                                   │ Temat ( ............... ) │
│                                   │ Treść ( {imię}, ........ )│
│                                   │ ┌ PODGLĄD RODZICA ▾ ─────┐│
│                                   │ └─────────────────────────┘│
│                                   │ Wyślij: (•)teraz ( )plan  │
│                                   │ ⚠ WALIDACJA: kanał bez   │
│                                   │   zgody → pomija i mówi   │
│                                   │   ilu; pusta treść blokuje│
│                                   │   {pole} bez wartości →   │
│                                   │   wartość zapasowa;       │
│                                   │   duży segment → potwierdź│
└───────────────────────────────────┴──────────────────────────┘

   [[ WYŚLIJ WIADOMOŚĆ ]]   (tryb planowania: [[ ZAPLANUJ WYSYŁKĘ ]])
 [ Zapisz szablon ]  [ Zaplanuj ]  [ Test do siebie ]  [ Sklonuj wiadomość ]
 NISZCZĄCE: [ Anuluj zaplanowaną ]  (wysłanej nie da się cofnąć - ekran uprzedza)
‹STAN OFFLINE: wersja robocza z banerem o wstrzymaniu wysyłki›
```

Dominuje edytor z wyborem segmentu i kanału, gdzie status zgody jest widoczny przy samym kanale, nie schowany w ustawieniach. Umknąć może odbiorca, który wycofał zgodę już po zaplanowaniu wysyłki - system musi go pominąć w momencie nadania, nie w momencie planowania, i powiedzieć ilu pominął. W hi-fi pokażę stan błędu częściowej wysyłki: „11 z 12 dostarczone, 1 pominięty z powodu braku zgody”, bo to najczęstszy realny wynik, nie czysty sukces.

---

## OCL-16 - raporty

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Raporty                        PL[v]   ‹STAN: wypełniony›  │
├──────────────────────────────────────────────────────────────┤
│ GALERIA: [Przychód] [Frekwencja] [Retencja] [Obłożenie grup]  │
├───────────────────────────────────┬──────────────────────────┤
│ FILTRY                            │ WYKRES + TREND            │
│ Okres [Semestr ▾]                 │  ▁▃▅▇▆  ↗ +8% r/r         │
│ Grupa [Wszystkie ▾]               │ porównanie z poprz. okr.  │
│ [x] Porównaj z poprzednim okresem │ ┌ TABELA DANYCH ŹRÓDŁ. ▾ ┐│
│                                   │ └─────────────────────────┘│
│                                   │ ⚠ WALIDACJA: koniec ≥     │
│                                   │   początek; okresy różnej │
│                                   │   długości → "nie wprost  │
│                                   │   porównywalne"; duży      │
│                                   │   eksport → "potrwa, zawęź"│
└───────────────────────────────────┴──────────────────────────┘

   [[ OTWÓRZ RAPORT ]]   (po otwarciu: dostosuj zakres + [ Eksport ])
 [ Zmień okres ]  [ Porównaj z poprzednim ]  [ Raport cykliczny na e-mail ]  [ Dane źródłowe ]
 NISZCZĄCE: nie dotyczy (tylko odczyt i eksport)
‹STAN PUSTY: za mało danych - "potrzeba X tygodni"; BRAK UPRAWNIEŃ: bez raportów finansowych›
```

Dominuje galeria gotowych raportów z jedną akcją „otwórz”, a porównanie okresów stoi jako pole wyboru przy filtrach. Umknąć może pułapka porównywania okresów o różnej długości - semestr ze świętami kontra pełny - bo bez ostrzeżenia trend wygląda na spadek, którego nie ma. W hi-fi pokażę stan pusty nowej placówki bez danych historycznych do porównania, z komunikatem ile tygodni danych potrzeba, zamiast pustego wykresu sugerującego awarię.

---

## OCL-17 - konfiguracja i eksport paszportu sportowego

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Paszport sportowy - konfiguracja  PL[v]  ‹STAN: wypełniony›│
├───────────────────────────────────┬──────────────────────────┤
│ SEKCJE PASZPORTU                   │ PODGLĄD PDF               │
│ [x] Postępy umiejętności (OCL-10)  │ ┌─────────────────────┐   │
│ [x] Frekwencja (OCL-11)            │ │ [branding placówki] │   │
│ [ ] Osiągnięcia z zawodów          │ │ Imię dziecka        │   │
│ Widoczność dla rodzica  [x]        │ │ ▸ Umiejętności      │   │
│ Zakres danych [cały rok ▾]         │ │ ▸ Frekwencja        │   │
│ Nagłówek/stopka: tokeny marki      │ │ (sekcja zawodów —)  │   │
│                                    │ └─────────────────────┘   │
│ ⚠ WALIDACJA: brak włączonej sekcji → "dokument byłby pusty";  │
│   sekcja zawodów dla dziecka bez startów → "pojawi się po     │
│   pierwszym starcie"; eksport danych wrażliwych → potwierdź,   │
│   że trafia do opiekuna                                        │
└───────────────────────────────────┴──────────────────────────┘

   [[ ZAPISZ KONFIGURACJĘ PASZPORTU ]]  (alt.: [[ WYGENERUJ PRZYKŁAD ]])
 [ Podgląd PDF ]  [ Włącz sekcję zawodów ]  [ Ustaw branding ]  [ Ogranicz zakres dat ]
 NISZCZĄCE: nie dotyczy (rodzic eksportuje kopię bez wpływu na dane źródłowe)
‹STAN PUSTY: propozycja podstawowych sekcji + przykład; BŁĄD PODGLĄDU: przyczyna + ponów›
```

Dominuje lista włączanych sekcji obok żywego podglądu PDF, co czyni abstrakcyjny „dokument” namacalnym jeszcze przed eksportem. Umknąć może dziecko obecne tylko w jednym module - włączona sekcja zawodów dla kogoś bez startów dałaby pustą stronę, więc trzeba to wytłumaczyć, nie po cichu pominąć. W hi-fi pokażę stan eksportu w języku innym niż roboczy placówki, z fallbackiem na en-GB, bo paszport ma być spójny dla obu modułów niezależnie od ustawień rodziny.

---

## OCL-18 - ustawienia placówki oraz role i uprawnienia

```
┌──────────────────────────────────────────────────────────────┐
│ ≡  Ustawienia placówki            PL[v]   ‹STAN: wypełniony›  │
├──────────────────────────────────────────────────────────────┤
│ [Dane placówki][Sale/tory][Branding][Role i uprawnienia•]     │
├───────────────────────────────────┬──────────────────────────┤
│ OSOBY I ROLE                       │ MACIERZ UPRAWNIEŃ         │
│ J. Nowak    organizator            │            podgl edyt usuń│
│ Kowalska    instruktor             │ Grupy       [x]  [x]  [ ] │
│ Lis         wolontariusz           │ Płatności   [x]  [ ]  [ ] │
│ [ + zaproś osobę ]                 │ Dane medycz.[x]  [ ]  [ ] │
│                                    │ (role: oper/org/instr/    │
│ TOKENY MARKI                       │  wolont/rodzic/uczestnik) │
│ --brand-ink [■]  --accent [■]      │ ⚠ WALIDACJA: pojemność    │
│ podgląd: [ przykładowy ekran ]     │   sali > 0; koniec godz.  │
│                                    │   przed początkiem; usun. │
│                                    │   sali z zajęciami → które│
│                                    │   grupy; kontrast WCAG    │
│                                    │   złamany → które kolory  │
└───────────────────────────────────┴──────────────────────────┘

   [[ ZAPISZ USTAWIENIA ]]   (sekcja ról: [[ PRZYPISZ ROLĘ OSOBIE ]])
 [ Dodaj salę ]  [ Zmień tokeny marki ]  [ Zaproś osobę ]  [ Podgląd brandingu ]
 NISZCZĄCE: [ Usuń salę ] [ Odbierz rolę ]  (ostatnia rola organizatora → wskaż następcę;
   zmiana uprawnień = wyraźne potwierdzenie, ślad audytu)
```

Dominuje rozdział na listę osób z rolami i macierz uprawnień, gdzie sześć ról spotyka się z konkretnymi prawami w jednej tabeli. Umknąć może kontrola kontrastu tokenów marki - kolory łamiące WCAG 2.1 AA (W3C, 2018) muszą ostrzegać i wskazywać które zestawienie nie spełnia minimum, zanim branding rozejdzie się po wszystkich stronach publicznych. W hi-fi pokażę stan odbierania ostatniej roli organizatora, który wymusza wskazanie następcy, bo bez tego placówka mogłaby zostać bez nikogo z prawem do zarządzania.

---

## Literatura

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

W3C. (2018). *Web Content Accessibility Guidelines (WCAG) 2.1*. World Wide Web Consortium. https://www.w3.org/TR/WCAG21/
