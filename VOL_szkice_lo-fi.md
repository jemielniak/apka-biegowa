# Szkice lo-fi - grupa VOL (panel wolontariusza, moduł zawodów)

Makiety telefonowe (persona pracuje wyłącznie na telefonie; tablet stanowiska to przypadek brzegowy). Bez kolorów - kolor semantyczny zaznaczam słowem (OK, NIE, DO WYJASNIENIA). Listę kontrolną heurystyk i kontrast czytam za specyfikacją (Nielsen, 1994; W3C, 2018). Twarda granica roli trzyma się w każdej makiecie: zero kwot, sald i faktur, najwyżej neutralny znacznik statusu opłaty.

Legenda znaczników: `>>` akcja główna, `(X)` akcja niszcząca, `(!)` / `[walidacja]` miejsce komunikatu, `[stan]` / `[sync]` wskaźnik stanu i synchronizacji offline.


## VOL-01 - logowanie lub dołączenie z zaproszenia

```
┌──────────────────────────────────────────┐
│ [logo]  Bieg Wiosenny          [PL v]    │
├──────────────────────────────────────────┤
│ Nazwa:  Bieg Wiosenny 2026               │
│ Data:   12.04.2026  (12 kwietnia)        │
│ Rola:   wolontariusz                     │
│ Zakres: wydawanie pakietow, sektor B     │
├──────────────────────────────────────────┤
│ ┌──────────────────────────────────────┐ │
│ │                                      │ │
│ │           [  SKANER  QR  ]           │ │
│ │        skieruj telefon na kod        │ │
│ │            z zaproszenia             │ │
│ │                                      │ │
│ └──────────────────────────────────────┘ │
│ >> AKCJA GLOWNA: skan zaproszenia        │
├──────────────────────────────────────────┤
│ ----------   albo   ----------           │
│ Kod jednorazowy:                         │
│ [ _  _  _  _  _  _ ]                     │
│ [ Zaloguj bez hasla ]                    │
├──────────────────────────────────────────┤
│ (!) walidacja:                           │
│   wygasle / kod uzyty /                  │
│   inne wydarzenie / odwolany dostep      │
│ [stan] pierwsze wejscie wymaga sieci;    │
│        potem odprawa dziala offline      │
├──────────────────────────────────────────┤
│ Nowy kod . Zmien jezyk . Koordynator     │
│ Dostep wygasa po wydarzeniu.             │
└──────────────────────────────────────────┘
```

Dominuje skaner QR zaproszenia, postawiony jako pierwszy i największy element - całe wejście opiera się na jednym geście. Umknąć może nota o wygaśnięciu dostępu oraz rozróżnienie czterech przyczyn odmowy (wygasłe, użyte, inne wydarzenie, odwołane), bo siedzą nisko, a wolontariusz czyta ten ekran w biegu. W hi-fi pokaż wariant błędu *kod już użyty na innym urządzeniu* z kierowaniem do koordynatora - to najczęstszy realny problem przy zaproszeniu współdzielonym omyłkowo.


## VOL-02 - dashboard z zadaniami i zmianami

```
┌──────────────────────────────────────────┐
│ Bieg Wiosenny    [PL v]    [sync: 3]     │
├──────────────────────────────────────────┤
│ TERAZ . 09:00-11:00                      │
│ ┌──────────────────────────────────────┐ │
│ │          Wydawanie pakietow          │ │
│ │          Sektor B . Brama 2          │ │
│ │                                      │ │
│ │       Instrukcja: skanuj kod,        │ │
│ │    sprawdz rozmiar, wydaj pakiet.    │ │
│ │                                      │ │
│ │      [   ROZPOCZNIJ ZADANIE   ]      │ │
│ └──────────────────────────────────────┘ │
│ >> AKCJA GLOWNA: rozpocznij zadanie      │
├──────────────────────────────────────────┤
│ Dalej dzis:                              │
│   11:30  Odprawa . Brama 1               │
│   14:00  Lista startowa . sektor B       │
├──────────────────────────────────────────┤
│ (!) Alerty operacyjne:                   │
│   - Brama przesunieta o 15 min           │
│   - Brak rozmiaru S w magazynie          │
├──────────────────────────────────────────┤
│ [walidacja] nie udalo sie odswiezyc      │
│   planu - pracujesz na ost. danych       │
│ [stan] sync: 08:52, 3 w kolejce          │
├──────────────────────────────────────────┤
│ [Harmonogram] [Instrukcja] [tel.Koord]   │
└──────────────────────────────────────────┘
```

Dominuje karta bieżącego zadania z jednym przyciskiem startu; reszta pulpitu jest celowo cichsza, żeby kciuk trafiał od razu w to, co dzieje się teraz. Łatwo przeoczyć licznik kolejki offline i alerty operacyjne, które przy zerwanym zasięgu niosą całą wiedzę o stanie pracy. Zrób makietę stanu offline z banerem nieświeżych danych i niezerową kolejką synchronizacji, bo to codzienność pracy przy bramie, a nie wyjątek.


## VOL-03 - harmonogram zmian

```
┌──────────────────────────────────────────┐
│ <- Harmonogram zmian           [PL v]    │
├──────────────────────────────────────────┤
│ Tylko Twoje zmiany . Bieg Wiosenny       │
├──────────────────────────────────────────┤
│ > TERAZ  09:00-11:00                     │
│ ┌──────────────────────────────────────┐ │
│ │          Wydawanie pakietow          │ │
│ │          Sektor B . Brama 2          │ │
│ │                                      │ │
│ │        [   OTWORZ ZMIANE   ]         │ │
│ └──────────────────────────────────────┘ │
│ >> AKCJA GLOWNA: otworz biezaca zmiane   │
├──────────────────────────────────────────┤
│   11:00-11:30   Przerwa                  │
│   11:30-14:00   Odprawa . Brama 1        │
│      [+ kalendarz]  [nie dam rady]       │
│   14:00-16:00   Lista start. [ODWOLANA]  │
├──────────────────────────────────────────┤
│ [walidacja] edycje zmian prowadzi        │
│   koordynator -> zglos niedostepnosc     │
│ [stan] offline: plan z 08:52,            │
│        moze byc nieaktualny              │
└──────────────────────────────────────────┘
```

Dominuje znacznik bieżącej zmiany i jej karta z przejściem do zadania - harmonogram służy jednej decyzji: gdzie być teraz. Pominąć można granicę roli, czyli to, że wolontariusz zmiany nie edytuje, a jedynie zgłasza niedostępność; bez wyraźnego komunikatu rodzi to próby edycji i telefon do koordynatora. W hi-fi pokaż zmianę odwołaną lub przesuniętą po synchronizacji, z notą co się zmieniło, bo to stan, w którym najłatwiej o nieporozumienie o godzinie.


## VOL-04 - ekran odprawy ze skanem QR

```
┌──────────────────────────────────────────┐
│ Odprawa . Brama 1   84/120  [sync:2]     │
├──────────────────────────────────────────┤
│ ┌──────────────────────────────────────┐ │
│ │                                      │ │
│ │           [  SKANER  QR  ]           │ │
│ │                                      │ │
│ └──────────────────────────────────────┘ │
│ Szukaj recznie: [ numer / nazwisko ]     │
├──────────────────────────────────────────┤
│ WYNIK SKANU:                             │
│ ┌──────────────────────────────────────┐ │
│ │        [ OK ]   wynik zielony        │ │
│ │        Anna Kowalska     #142        │ │
│ │        Zgoda do odbioru: TAK         │ │
│ │     Oplata: OPLACONE (bez kwoty)     │ │
│ │                                      │ │
│ │      [   POTWIERDZ ODPRAWE   ]       │ │
│ └──────────────────────────────────────┘ │
│ >> AKCJA GLOWNA: potwierdz odprawe       │
├──────────────────────────────────────────┤
│ [walidacja]                              │
│   juz odprawiony 09:12 / kod spoza       │
│   wydarzenia / kod uszkodzony            │
│ [stan] offline: odprawa lokalnie,        │
│        2 operacje w kolejce              │
├──────────────────────────────────────────┤
│ [Skanuj dalej] [Do wyjasnienia] [+not]   │
└──────────────────────────────────────────┘
```

Dominuje pole skanu, a po skanie - wielki, jednoznacznie zielony albo czerwony wynik, który ma być czytelny w słońcu i w rękawiczkach. Umyka neutralny status opłaty bez kwoty: musi wystarczyć do decyzji, ale nie wolno mu kusić wolontariusza do rozstrzygania sporu o pieniądze. Najważniejszy stan alternatywny do makiety to *uczestnik już odprawiony* z godziną pierwszej odprawy, bo to on zatrzymuje ciche dublowanie (zapobieganie błędom wg Nielsen, 1994).


## VOL-05 - wydawanie pakietów i gadżetów

```
┌──────────────────────────────────────────┐
│ Wydawanie . sekt.B  61/120  [sync:1]     │
├──────────────────────────────────────────┤
│ ┌──────────────────────────────────────┐ │
│ │                                      │ │
│ │           [  SKANER  QR  ]           │ │
│ │                                      │ │
│ └──────────────────────────────────────┘ │
├──────────────────────────────────────────┤
│ Anna Kowalska     #142                   │
│ ┌──────────────────────────────────────┐ │
│ │                                      │ │
│ │         R O Z M I A R :    M         │ │
│ │                                      │ │
│ │      (napis duzy i kontrastowy)      │ │
│ └──────────────────────────────────────┘ │
│ Zawartosc pakietu:                       │
│   [x] Koszulka M      [x] Numer          │
│   [ ] Medal           [ ] Chip           │
│ Magazyn M: 38 szt.    Wydany: NIE        │
├──────────────────────────────────────────┤
│ >> AKCJA GLOWNA:                         │
│ [   POTWIERDZ WYDANIE   ]                │
│ [ Zmien rozmiar ]  [ Brak rozmiaru? ]    │
│ (X) Cofnij wydanie  (z powodem)          │
├──────────────────────────────────────────┤
│ [walidacja]                              │
│   pakiet wydany 09:20 - potwierdz        │
│   ponowne / brak rozmiaru -> nastepny    │
│ [stan] offline: bez podwojnego           │
│        wydania na tym telefonie          │
└──────────────────────────────────────────┘
```

Dominuje rozmiar koszulki, podany największym i najbardziej kontrastowym napisem na ekranie - to punkt, w którym pomyłka kosztuje najwięcej, więc wszystko inne mu ustępuje. Łatwo przegapić, że potwierdzenie wydania jest poprzedzone pokazaniem rozmiaru, a cofnięcie wymaga powodu; bez tego wydanie staje się odruchem, a nie decyzją. Pokaż w hi-fi stan *pakiet już wydany 09:20* z prośbą o potwierdzenie ponownego wydania, bo zatrzymanie podwójnego wydania jest tu metryką sukcesu.


## VOL-06 - lista startowa z weryfikacją opłaty

```
┌──────────────────────────────────────────┐
│ Lista startowa   Gotowi 84/120 [sync]    │
├──────────────────────────────────────────┤
│ Szukaj: [ numer / nazwisko ]             │
│ Filtr: [Wszyscy] [Oplac.] [Do wyjasn.]   │
├──────────────────────────────────────────┤
│ #142  Anna Kowalska                      │
│    Oplata OK | Odprawa OK | Pakiet OK    │
│ ......................................   │
│ #143  Jan Nowak                          │
│    Oplata NIE | Odprawa - | Pakiet -     │
│    -> Skieruj do koordynatora            │
│ ......................................   │
│ #144  Ewa Lis                            │
│    Oplata: DO WYJASNIENIA                │
│    -> Sprawe prowadzi koordynator        │
├──────────────────────────────────────────┤
│ >> AKCJA GLOWNA: dotknij wiersz ->       │
│    odprawa (VOL-04) / wydanie (VOL-05)   │
├──────────────────────────────────────────┤
│ [walidacja] status mogl sie zmienic      │
│   od ost. sync - potwierdz u koord.      │
│ [stan] offline: lista z 08:52            │
└──────────────────────────────────────────┘
```

Dominuje lista ze statusami sprowadzonymi do trzech czytelnych znaczników; numer i nazwisko wystarczają do odnalezienia, a kolor semantyczny do szybkiej decyzji. Umknąć może to, że status *do wyjaśnienia* nie jest zadaniem wolontariusza, tylko skierowaniem do koordynatora - i że nigdzie nie pada kwota. W makiecie hi-fi pokaż baner pracy offline z ostrzeżeniem, że status opłaty mógł się zmienić od ostatniej synchronizacji, bo uczestnik opłacony tuż przed startem to klasyczny przypadek brzegowy.


## VOL-07 - zgłoszenie się na wolontariat (most rodzic)

```
┌──────────────────────────────────────────┐
│ Zglos sie na wolontariat       [PL v]    │
├──────────────────────────────────────────┤
│ Bieg Wiosenny 2026 . 12.04               │
│ Szukamy: brama, depozyt, punkt wody      │
│ Rola tymczasowa - wygasa po imprezie     │
├──────────────────────────────────────────┤
│ Wybierz stanowisko i zmiane:             │
│   ( ) Brama    09:00-11:00  (3 wolne)    │
│   (o) Depozyt  11:00-14:00  (1 wolne)    │
│   ( ) Woda     14:00-16:00  PELNE        │
├──────────────────────────────────────────┤
│ Uwagi / preferencje:                     │
│   [____________________________]         │
│ Twoje dane (z konta rodzinnego):         │
│   Maria K.  tel 600...     [Edytuj]      │
│ [ ] Akceptuje regulamin wolontariusza    │
├──────────────────────────────────────────┤
│ >> AKCJA GLOWNA:                         │
│ [   WYSLIJ ZGLOSZENIE   ]                │
│ [ Zapisz robocza ]   (X)[ Wycofaj ]      │
├──────────────────────────────────────────┤
│ (!) walidacja: zmiana koliduje ze        │
│   startem Twojego dziecka o 13:40        │
│ [stan] po wyslaniu: czeka na             │
│        akceptacje organizatora           │
└──────────────────────────────────────────┘
```

Dominuje wybór stanowiska i zmiany z widoczną dostępnością miejsc, bo to ekran planowania robiony spokojnie w domu, a nie pracy przy bramie. Pominąć można ostrzeżenie o kolizji zmiany ze startem własnego dziecka oraz dziedziczenie danych z konta rodzinnego, które odróżnia ten most od zwykłej rejestracji. Zrób wariant z aktywnym ostrzeżeniem o kolizji godzinowej i ze zmianą oznaczoną jako PEŁNE, żeby pokazać, jak formularz prowadzi rodzica do dostępnego terminu zamiast blokować go bez wyjaśnienia.


## Literatura

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

W3C. (2018). *Web Content Accessibility Guidelines (WCAG) 2.1*. World Wide Web Consortium. https://www.w3.org/TR/WCAG21/
