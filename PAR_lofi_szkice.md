# Szkice lo-fi grupy PAR - panel rodzica, konto rodzinne

Szkice obejmują wszystkie szesnaście ekranów ze specyfikacji `PAR_spec.md` (PAR-01 do PAR-16). Każdy rysunek jest blokiem w czcionce o stałej szerokości, bez kolorów, w układzie telefonu, bo to urządzenie podstawowe tej persony. Pod każdym blokiem znajdują się trzy zdania: co dominuje, co może umknąć, jaki stan alternatywny warto pokazać w makiecie hi-fi.

Lista kontrolna heurystyk w specyfikacji odnosi się do dziesięciu heurystyk użyteczności (Nielsen, 1994), a minimalny cel dotykowy i kontrast tekstu do wytycznych dostępności (W3C, 2018). Komentarze poniżej korzystają z tych dwóch źródeł tam, gdzie mają realne zastosowanie.

## Legenda znaków

```
☰              menu                 ▾   rozwijane / przełącznik
profil▾        przełącznik dziecka  ⟳   znacznik ostatniej synchronizacji
[ ........ ]   pole wprowadzania    ●   wskaźnik stanu ekranu
( ) / (•)      wybór pojedynczy     ⚠   miejsce komunikatu walidacji
[x] / [ ]      pole wyboru          ▐ AKCJA ▌  akcja główna (dominująca)
· akcja        akcja drugorzędna    ‹akcja›    akcja niszcząca
```

---

## PAR-01 - rejestracja i logowanie z kontem rodzinnym

```
┌──────────────────────────────────
│ [LOGO ORGANIZATORA]          PL▾
├──────────────────────────────────
│  Jedno konto dla całej rodziny.
│  Obsłuży zajęcia i zawody.
│
│  Adres e-mail lub telefon
│  [ ............................ ]
│  ⚠ ‹walidacja adresu / blokady›
│
│  ▐  WYŚLIJ LINK BEZ HASŁA   ▌
│
│  ──────────  lub  ──────────
│  [ Zaloguj przez dostawcę ▾ ]
│
│  [x] Zapamiętaj to urządzenie
│      (odcisk / Face)
│
│  ● stan: gotowy do wypełnienia
│  ────────────────────────────
│  · Poproś o nowy link
│  · Odzyskaj dostęp · Pomoc · Regulamin
└──────────────────────────────────
```

Dominuje jedno wejście bez hasła, zgodne z heurystyką rozpoznawania zamiast przypominania (Nielsen, 1994), więc rodzic w pośpiechu nie szuka zapamiętanego hasła. Umknąć może to, że na urządzeniu już zapamiętanym akcją wiodącą jest potwierdzenie biometryczne, a nie pole adresu - to inny układ tego samego ekranu i trzeba go rozrysować osobno. W hi-fi pokażę stan błędu z wygasłym linkiem, bo wtedy komunikat musi wprost powiedzieć, na jaki adres trafi nowy link i ile prób zostało.

---

## PAR-02 - dashboard rodzica

```
┌──────────────────────────────────
│ ☰   PULPIT          (Z)(K)(+) PL▾
├──────────────────────────────────
│  ● ⟳ ostatnia synch. 18:42
│
│  NAJPILNIEJSZE
│  ┌────────────────────────────┐
│  │ ⚠ Zaległa płatność  89 zł  │
│  │ Zosia · pływanie  ▐ ZAPŁAĆ ▌│  ← akcja na kafelku
│  └────────────────────────────┘
│  ┌────────────────────────────┐
│  │ ⏱ Miejsce zwolnione 04:58  │
│  │ Lista rezerwowa · potwierdź│
│  └────────────────────────────┘
│  ┌────────────────────────────┐
│  │ ✎ Brakujący dokument       │
│  └────────────────────────────┘
│
│  DZIŚ I JUTRO
│  09:00 Zosia trening · 17:30 Kuba start
│  ⚠ kolizja: dwa starty w jednym bloku
│
│  Portfel: 40 zł kredytu
│  ────────────────────────────
│  · Zgłoś nieobecność · Grafik · Galeria
└──────────────────────────────────
```

Dominuje pojedynczy strumień alertów ułożony po pilności, nie po organizatorze, więc rodzic z dziećmi u kilku placówek dostaje jedną kolejkę spraw. Łatwo przeoczyć, że każdy kafelek ładuje się niezależnie - błąd jednego nie gasi pulpitu, lecz dotyczy tylko tej liczby, co trzeba oddać znacznikiem ponowienia na samym kafelku. W hi-fi rozrysuję stan offline z banerem wstrzymanych zmian i datą ostatniej synchronizacji, bo część rodziców otwiera pulpit przy basenie bez zasięgu.

---

## PAR-03 - profile dzieci z przełączaniem

```
┌──────────────────────────────────
│ ☰   PROFILE DZIECI            PL▾
├──────────────────────────────────
│  ● stan: wypełniony
│
│  ┌──────────┐  ┌──────────┐
│  │ (Z) ZOSIA│  │ (K) KUBA │
│  │ ● AKTYWNY│  │  7 lat   │
│  │ 9 lat    │  │ ⚠ brak   │
│  │ ✓ komplet│  │   zgody  │
│  └──────────┘  └──────────┘
│  ┌──────────┐
│  │    +     │  ← duży cel dotykowy
│  │  Dodaj   │
│  └──────────┐
│
│  DANE AKTYWNEGO DZIECKA
│  Imię   [ Zosia .............. ]
│  Ur.    [ 2016-04-11 ......... ]
│  ⚠ ‹walidacja: data, wiek, zgodność›
│
│  · Dokumenty · Dane zdrowotne (osobne wejście)
│  · Zaproś drugiego opiekuna · Widok nastolatka
│
│  ▐  PRZEŁĄCZ NA WYBRANE DZIECKO  ▌
│  ‹Usuń profil›  (lub archiwizuj)
└──────────────────────────────────
```

Dominuje wyraźnie zaznaczony aktywny profil, utrzymany potem na wszystkich kolejnych ekranach, co ogranicza ryzyko wykonania czynności na złym dziecku. Umknąć może rozróżnienie rodzeństwa o podobnym imieniu - awatar i kolor profilu robią tu więcej niż sama etykieta i muszą być duże, nie dekoracyjne. W hi-fi pokażę stan, w którym usunięcie profilu jest zablokowane przez aktywne zapisy i system proponuje archiwizację z zachowaniem paszportu, bo to częstszy przypadek niż czyste skasowanie.

---

## PAR-04 - zapis dziecka na grupę z poziomami

```
┌──────────────────────────────────
│ ☰   ZAPIS NA GRUPĘ      Zosia▾ PL▾
├──────────────────────────────────
│  ● stan: lista z dostępnością
│  Podpowiedź: poziom 3 (z postępów)
│
│  ┌────────────────────────────┐
│  │ Delfiny · pon/śr 17:00     │
│  │ poziom 3 · sala A          │
│  │ wolne: 2   ⚠ niezgodny wiek│
│  │ (•) wybierz                │
│  └────────────────────────────┘
│  ┌────────────────────────────┐
│  │ Delfiny · wt/czw 18:00     │
│  │ wolne: 0  → lista rezerwowa│
│  └────────────────────────────┘
│
│  KOSZT
│  Cena                   220 zł
│  Zniżka rodzinna       - 30 zł
│  Kredyt portfela       - 40 zł
│  Do zapłaty            150 zł
│
│  [ ] Akceptuję regulamin (wersja)
│  ⚠ ‹walidacja: brak dokumentu / miejsc›
│
│  ▐  ZAPISZ I PRZEJDŹ DO PŁATNOŚCI  ▌
│  · Zmień grupę · Zapisz na później · Dopłać z portfela
└──────────────────────────────────
```

Dominuje rozbicie kosztu na cenę, zniżkę i kredyt portfela, dzięki czemu rodzic widzi realną kwotę do dopłaty przed dotknięciem płatności (widoczność stanu systemu, Nielsen, 1994). Można przeoczyć, że poziom odbiegający od grupy tylko ostrzega i proponuje konsultację, a brak wymaganego dokumentu twardo blokuje finalizację - to dwa różne ciężary tego samego komunikatu. W hi-fi rozrysuję stan, w którym grupa zapełnia się w trakcie zapisu i ekran przekierowuje na listę rezerwową zamiast cicho odmawiać.

---

## PAR-05 - zgłoszenie nieobecności

```
┌──────────────────────────────────
│ ☰   NIEOBECNOŚĆ         Zosia▾ PL▾
├──────────────────────────────────
│  ● stan: lista terminów
│  Pozostałe odrobienia: 2 / 4
│
│  śr 14.05  17:00  Delfiny
│   ( ) obecna   (•) NIEOBECNA
│   ✓ uprawnia do odrobienia
│
│  pon 19.05 17:00  Delfiny
│   (•) obecna   ( ) nieobecna
│   ⚠ okno zgłoszenia zamknięte
│     → możesz poprosić o wyjątek
│
│  Powód (opcjonalnie)
│  [ ............................ ]
│
│  ● offline: zgłoszenie czeka w kolejce
│
│  ▐  ZGŁOŚ NIEOBECNOŚĆ  ▌
│  · Cofnij zgłoszenie · Seria terminów · Przejdź do odrabiania
└──────────────────────────────────
```

Dominuje przełącznik obecności przy każdym terminie wraz z natychmiastową informacją, czy nieobecność daje prawo do odrobienia, więc skutek decyzji jest widoczny w tym samym dotknięciu. Umknąć może warstwa offline - zgłoszenie przy basenie trafia do kolejki lokalnej i musi być zabezpieczone przed podwójnym zapisem po powrocie sieci, co rzadko widać na szkicu, a jest sednem tego ekranu. W hi-fi pokażę stan zgłoszenia po zamkniętym oknie, gdzie komunikat tłumaczy utratę prawa do odrobienia i kieruje do prośby o wyjątek.

---

## PAR-06 - zapis na odrabianie

```
┌──────────────────────────────────
│ ☰   ODRABIANIE          Zosia▾ PL▾
├──────────────────────────────────
│  ● stan: lista praw i terminów
│
│  PRAWO DO ODROBIENIA
│  za śr 14.05 · ważne do 30.06
│  ⚠ wygasa za 9 dni
│
│  WOLNE TERMINY (poziom 3)
│  Filtr: [ dzień ▾ ] [ godzina ▾ ]
│
│  (•) sob 24.05 10:00 · sala B
│      instr. Nowak · wolne: 4
│  ( ) wt 27.05 18:00 · sala A
│      wolne: 1
│  ⚠ ‹walidacja: termin zajęty / kolizja›
│
│  Bez ponownej płatności
│  (odrobienie z opłaconych zajęć)
│
│  ▐  ZAPISZ NA WYBRANY TERMIN  ▌
│  · Zmień termin · Więcej terminów
│  ‹Odwołaj zapisane odrobienie›
└──────────────────────────────────
```

Dominuje gotowa lista terminów zgodnych poziomem dziecka, więc rodzic tylko wybiera i potwierdza, zamiast samodzielnie szukać pasującej grupy. Można przeoczyć znacznik kończącego się terminu ważności prawa - to on decyduje o pilności i powinien być mocniejszy niż sama lista godzin. W hi-fi rozrysuję stan pusty, w którym nie ma wolnych terminów tuż przed wygaśnięciem prawa, z propozycją obserwowania albo prośby o wyjątek.

---

## PAR-07 - lista rezerwowa z push

```
┌──────────────────────────────────
│ ☰   LISTA REZERWOWA     Kuba▾  PL▾
├──────────────────────────────────
│  ● stan: OFERTA (okno otwarte)
│
│     ┌──────────────────────┐
│     │  Twoja pozycja:  1   │
│     │  ⏱  04:58 do decyzji │
│     └──────────────────────┘
│
│  Grupa: Żabki · wt/czw 16:00
│  Reguła dopisania: kolejność zgłoszeń
│  ⚠ ‹oferta wygasła → przejdzie dalej›
│
│  ▐  POTWIERDŹ ZWOLNIONE MIEJSCE  ▌
│      (widoczne tylko w oknie oferty)
│
│  ALTERNATYWY (ten sam poziom)
│  · Żabki czw 17:00 · wolne: 1
│
│  ● offline: potwierdzenie wymaga sieci
│  ────────────────────────────
│  · Zmień dziecko · Kanał powiadomień
│  ‹Opuść kolejkę›
└──────────────────────────────────
```

Dominuje licznik pozycji i odliczanie okna oferty, bo cała wartość ekranu zależy od tego, czy rodzic zdąży potwierdzić, zanim miejsce przejdzie dalej. Umknąć może to, że przycisk potwierdzenia istnieje wyłącznie w oknie oferty - poza nim dominującą akcją jest zwykłe dołączenie do kolejki, czyli ten sam ekran ma dwa różne stany główne. W hi-fi pokażę stan po wygaśnięciu oferty, gdzie komunikat wyjaśnia, że okno się zamknęło, i proponuje pozostanie w kolejce, bo to moment największej frustracji.

---

## PAR-08 - postępy umiejętności dziecka

```
┌──────────────────────────────────
│ ☰   POSTĘPY             Zosia▾ PL▾
│                        (widok odczytu)
├──────────────────────────────────
│  ● stan: drzewo z postępem
│
│  ŚCIEŻKA UMIEJĘTNOŚCI
│   ✓ Oddech do wody
│   ✓ Pływanie na plecach
│   ◐ Kraul - w trakcie
│   ○ Nawrót koziołkowy
│   ○ Skok startowy
│
│  ┌────────────────────────────┐
│  │ Kraul                       │
│  │ Opis językiem rodzica...    │
│  │ ▷ film instruktażowy        │
│  └────────────────────────────┘
│
│  ★ Gotowa do wyższej grupy
│
│  ▐  OTWÓRZ SZCZEGÓŁ UMIEJĘTNOŚCI  ▌
│    (przy gotowości: ZOBACZ ZAPIS
│     NA WYŻSZĄ GRUPĘ)
│  · Odtwórz film · Historia postępu · Paszport
└──────────────────────────────────
```

Dominuje wizualna ścieżka ze statusem każdej umiejętności opisanej językiem rodzica, nie żargonem trenerskim, co realizuje heurystykę zgodności ze światem użytkownika (Nielsen, 1994). Łatwo przeoczyć, że ekran jest czysto odczytowy dla rodzica - postęp tworzy instruktor, więc żaden element nie może wyglądać na edytowalny. W hi-fi rozrysuję stan pusty dla dziecka bez wpisów, z komunikatem, że postęp pojawi się po pierwszych zajęciach, żeby nowy rodzic nie zobaczył martwego ekranu.

---

## PAR-09 - dokumenty dziecka

```
┌──────────────────────────────────
│ ☰   DOKUMENTY           Kuba▾  PL▾
├──────────────────────────────────
│  ● stan: lista ze statusami
│
│  Zgoda medyczna      [WYGASA 7 dni]
│   wymagana: obóz letni
│   · podgląd · zastąp
│  Orzeczenie          [ZAAKCEPTOWANY]
│  Karta pływacka      [BRAK]
│   ⚠ blokuje zapis na grupę
│   ▐ WGRAJ ▌  / 📷 zrób zdjęcie
│  ⚠ ‹walidacja: format / rozmiar / data›
│
│  ZGODY
│  [ ] Regulamin v.4  (nowa wersja)
│      → przejrzyj przed akceptacją
│  [x] Wizerunek dziecka
│
│  ● offline: plik czeka w kolejce
│  ────────────────────────────
│  ▐  WGRAJ BRAKUJĄCY DOKUMENT  ▌
│    (przy komplecie: ZAAKCEPTUJ ZGODY)
│  · Pobierz kopię  ‹Usuń dokument›
└──────────────────────────────────
```

Dominuje status każdego dokumentu w kolorze semantycznym, więc rodzic od razu widzi, czego brakuje i co wkrótce wygasa, zanim zapis utknie na braku papieru. Umknąć może wgranie dokumentu wrażliwego - orzeczenie czy zgoda medyczna to szczególna kategoria danych i ekran musi poprosić o świadome potwierdzenie, a nie traktować je jak zwykły plik. W hi-fi pokażę stan dokumentu odrzuconego przez organizatora z prośbą o poprawną wersję, bo to ścieżka, która najczęściej blokuje rodzica.

---

## PAR-10 - płatności, portfel i faktury

```
┌──────────────────────────────────
│ ☰   PŁATNOŚCI           rodzina  PL▾
├──────────────────────────────────
│  ● stan: należności i portfel
│  Portfel: 40 zł kredytu (zwrot)
│
│  DO ZAPŁATY
│  [x] Pływanie · Zosia · 14.05 · 89 zł
│  [ ] Abonament · Kuba · 01.06 · 120 zł
│  ⚠ należność dotyczy: ZOSI
│
│  PODSUMOWANIE
│  Kwota                  89 zł
│  Kredyt portfela      - 40 zł
│  Do dopłaty            49 zł
│
│  Metoda: (•) BLIK ( ) karta
│          ( ) Przelewy24 ( ) PayU ( ) Stripe
│  ⚠ ‹walidacja: brak wyboru / odrzucenie›
│
│  ▐  ZAPŁAĆ ZAZNACZONE  ▌
│    (gdy portfel pokrywa: ROZLICZ Z PORTFELA)
│  · Pobierz fakturę · Płatność cykliczna
│  · Harmonogram abonamentu
└──────────────────────────────────
```

Dominuje rozbicie na kwotę, kredyt portfela i realną dopłatę przy jawnym wskazaniu, którego dziecka dotyczy należność, co zapobiega zapłaceniu nie tej pozycji. Można przeoczyć regułę, że kredyt portfela jest zobowiązaniem jednego organizatora i nie przechodzi do innego - rodzina z saldem u jednej placówki i długiem u drugiej zobaczy dwa osobne obrazy, nie jeden wspólny worek. W hi-fi rozrysuję stan odrzuconej płatności, gdzie komunikat tłumaczy najczęstsze przyczyny i proponuje inną metodę, bez zapisywania danych karty.

---

## PAR-11 - zapis dziecka na zawody między modułami

```
┌──────────────────────────────────
│ ☰   ZAPIS NA ZAWODY     Kuba▾  PL▾
├──────────────────────────────────
│  ● stan: lista dystansów
│  Bieg Wiosenny · 08.06 · Park Miejski
│
│  DYSTANS
│  (•) 1 km dzieci · kat. 7-9 lat
│      próg: 39 zł · wolne: 12
│  ( ) 3 km · ⚠ poza kategorią wieku
│
│  FORMULARZ (dane dziedziczone)
│  Imię i nazwisko  [ Kuba N. ] (z profilu)
│  Klub             [ ............ ]
│  Pole warunkowe   [ rozmiar koszulki ▾ ]
│
│  · Dodaj gadżet
│  [ ] Regulamin zawodów (wersja)
│  ⚠ ‹walidacja: próg wyczerpany → nowa cena›
│
│  Do zapłaty: 39 zł  (portfel: 0 zł)
│
│  ▐  ZAPISZ NA DYSTANS I ZAPŁAĆ  ▌
│  · Zmień dystans · Zapisz na później
│  · Zgłoś się jako wolontariusz
└──────────────────────────────────
```

Dominują dane dziedziczone z profilu dziecka, dzięki czemu ten sam zawodnik trafia na bieg bez zakładania drugiego konta i bez przepisywania danych (minimalizacja danych zamiast ponownego zbierania). Umknąć może moment zmiany progu cenowego między dodaniem do koszyka a płatnością - ekran musi pokazać nową kwotę przed potwierdzeniem, żeby rodzic nie zapłacił innej sumy niż widział. W hi-fi pokażę stan kolizji, w którym start dziecka nakłada się na zmianę rodzica jako wolontariusza, bo to typowy konflikt łączący PAR-11 z PAR-14.

---

## PAR-12 - rezerwacja lekcji indywidualnej

```
┌──────────────────────────────────
│ ☰   LEKCJA 1:1          Zosia▾ PL▾
├──────────────────────────────────
│  ● stan: kalendarz wolnych okien
│
│  Instruktor: [ Nowak ▾ ] poziom 3
│
│  PON  WT   ŚR   CZW  PT
│  ──   16:00 ──  17:00 ──
│  ──   17:00 ──  ──    18:00
│       (•)
│
│  Cel lekcji
│  [ ............................ ]
│  ⚠ ograniczaj do informacji dla instruktora
│
│  Koszt: 90 zł  (portfel: 40 zł → 50 zł)
│  Metoda: (•) BLIK ( ) karta
│  ⚠ ‹walidacja: okno zajęte / kolizja›
│
│  ▐  ZAREZERWUJ OKNO I ZAPŁAĆ  ▌
│  · Rezerwacja cykliczna · Zmień instruktora
│
│  MOJE REZERWACJE
│  czw 17:00 · [oczekująca]
│  ‹Odwołaj rezerwację›
└──────────────────────────────────
```

Dominuje kalendarz wolnych okien znany z rezerwacji wizyt, więc rodzic wybiera termin bez dzwonienia na recepcję. Można przeoczyć wykrywanie kolizji z grafikiem grupowym dziecka - lekcja jeden na jeden potrafi nałożyć się na zwykłe zajęcia i ekran powinien ostrzec przed potwierdzeniem, a nie po nim. W hi-fi rozrysuję stan odwołania potwierdzonej rezerwacji z bliskim terminem, gdzie komunikat tłumaczy skutek dla zwrotu i kieruje środki do portfela jako kredyt.

---

## PAR-13 - paszport sportowy z eksportem PDF

```
┌──────────────────────────────────
│ ☰   PASZPORT SPORTOWY   Zosia▾ PL▾
├──────────────────────────────────
│  ● stan: podgląd gotowy
│
│  Zakres dat: [ wrzesień–maj ▾ ]
│  Język dokumentu: [ pl-PL ▾ ]
│
│  ┌──────── PODGLĄD ─────────┐
│  │ [marka placówki]         │
│  │ ZOSIA N. · 9 lat         │
│  │ Umiejętności: 4/8        │
│  │ Frekwencja: 92%          │
│  │ Zawody: 1 start, 3. m-ce │
│  │       strona 1 / 3       │
│  └──────────────────────────┘
│  ⚠ ‹walidacja: pusty dokument / brak startów›
│
│  ▐  EKSPORTUJ DO PDF  ▌
│    (przed eksportem: PRZEJRZYJ PODGLĄD)
│  · Zmień zakres dat · Udostępnij kopię
│
│  WCZEŚNIEJSZE EKSPORTY
│  paszport_2024-12.pdf
└──────────────────────────────────
```

Dominuje podgląd dokumentu strona po stronie przed eksportem, co daje rodzicowi pewność, co znajdzie się w pliku, zanim go wygeneruje. Umknąć może oznaczenie źródła sekcji - paszport łączący dane od dwóch organizatorów musi jasno wskazywać, skąd pochodzi każda część, inaczej dokument myli pochodzenie osiągnięć. W hi-fi pokażę stan pusty dla dziecka obecnego tylko w jednym module, z wyjaśnieniem, że sekcja zawodów pojawi się po pierwszym starcie.

---

## PAR-14 - zgłoszenie się jako wolontariusz

```
┌──────────────────────────────────
│ ☰   WOLONTARIAT         rodzic  PL▾
├──────────────────────────────────
│  ● stan: formularz wyboru zmiany
│  Bieg Wiosenny · 08.06
│
│  STANOWISKO I ZMIANA
│  (•) Punkt wody · 08:00–11:00 · wolne 2
│  ( ) Bramka start · 07:00–09:00 · wolne 0
│  ⚠ kolizja: start Kuby o 09:30
│
│  Uwagi dla koordynatora
│  [ ............................ ]
│
│  Dane kontaktowe (z konta)
│  [ Anna N. · 600... ]  · popraw
│
│  [ ] Regulamin wolontariusza (wersja)
│  ⚠ ‹walidacja: brak zmiany / brak zgody›
│
│  Dostęp jednorazowy, wygasający
│
│  ▐  WYŚLIJ ZGŁOSZENIE  ▌
│  · Zapisz wersję roboczą
│  Status: — ‹Wycofaj zgłoszenie›
└──────────────────────────────────
```

Dominuje wybór stanowiska i zmiany na koncie, które rodzic już ma, więc rola wolontariusza dopina się do istniejącego logowania zamiast tworzyć osobne konto. Można przeoczyć ostrzeżenie o kolizji wybranej zmiany ze startem własnego dziecka - bez niego rodzic obsadzi punkt wody dokładnie wtedy, gdy jego dziecko biegnie. W hi-fi rozrysuję stan po wysłaniu, gdzie zgłoszenie czeka na akceptację organizatora, a znacznik statusu jasno mówi, że rola jeszcze nie jest aktywna.

---

## PAR-15 - galeria zdjęć z RunPixie

```
┌──────────────────────────────────
│ ☰   GALERIA             Kuba▾  PL▾
├──────────────────────────────────
│  ● stan: siatka zdjęć
│  Filtr: [ Bieg Wiosenny ▾ ]
│  Dopasowane po numerze startowym 142
│
│  ┌────┐ ┌────┐ ┌────┐
│  │ ▣  │ │ ▣  │ │ ▣? │ ← niepewne
│  └────┘ └────┘ └────┘
│  ┌────┐ ┌────┐ ┌────┐
│  │ ▣  │ │ ▣$ │ │ ▣  │ ← płatne
│  └────┘ └────┘ └────┘
│  ⚠ ‹walidacja: niepewne dopasowanie - potwierdź›
│
│  PODGLĄD
│  [ pełne zdjęcie · data · wydarzenie ]
│  Licencja dostawcy: warunki
│
│  ▐  OTWÓRZ I POBIERZ  ▌
│    (płatne: DODAJ DO KOSZYKA → PŁATNOŚĆ)
│  · Udostępnij · Pobierz komplet
│  · Zgłoś błędne dopasowanie
└──────────────────────────────────
```

Dominuje dopasowanie po numerze startowym, więc rodzic ogląda kadry własnego dziecka, a nie tysiące cudzych, co jest rdzeniem tego ekranu. Umknąć może rozróżnienie zdjęć darmowych od płatnych oraz znacznik niepewnego dopasowania - przy słabym oznaczeniu rodzic kupi zdjęcie nie tego dziecka. W hi-fi pokażę stan potwierdzenia niepewnego dopasowania przed pobraniem, bo to zabezpieczenie przed pobraniem cudzego wizerunku i częsty przypadek przy nieczytelnym numerze na kadrze.

---

## PAR-16 - ustawienia konta, zgody RODO i powiadomienia

```
┌──────────────────────────────────
│ ☰   USTAWIENIA          rodzina  PL▾
├──────────────────────────────────
│  ● stan: pełne ustawienia
│
│  DANE KONTAKTOWE
│  E-mail [ anna@... ] Tel [ 600... ]
│  ⚠ ‹walidacja: format adresu / telefonu›
│
│  ZGODY RODO            wersja · data
│  Wizerunek      [x]   v.3 · 12.01
│  Marketing      [ ]   —
│  ⚠ cofnięcie zgody wymaganej blokuje usługę
│
│  POWIADOMIENIA   push  e-mail  SMS
│  Oferta rezerw.  [x]   [x]    [ ]
│  Płatności       [x]   [x]    [ ]
│  ⚠ wyłączasz kanał krytyczny
│
│  Język i region [ pl-PL ▾ ]
│  Drugi opiekun  [ zaproś · zakres ▾ ]
│  Historia logowań · Pobierz moje dane
│
│  ▐  ZAPISZ USTAWIENIA  ▌
│  ‹Usuń konto rodzinne›  ‹Odbierz dostęp opiekunowi›
└──────────────────────────────────
```

Dominuje świadome zarządzanie zgodami z widoczną wersją i datą każdej z nich, co daje rodzicowi kontrolę nad tym, co system przetwarza (heurystyka kontroli i swobody, Nielsen, 1994). Można przeoczyć ostrzeżenie przy wyłączaniu kanału krytycznego, na przykład oferty z listy rezerwowej - bez niego rodzic wycisza powiadomienie, które potem przegapi. W hi-fi rozrysuję stan usuwania konta przy aktywnych zapisach i dodatnim saldzie portfela, gdzie ekran wyjaśnia skutek i proponuje najpierw rozliczenie, bo to decyzja nieodwracalna.

---

## Literatura

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

W3C. (2018). *Web Content Accessibility Guidelines (WCAG) 2.1*. World Wide Web Consortium. https://www.w3.org/TR/WCAG21/
