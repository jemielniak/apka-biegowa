# Szkice lo-fi - grupa KID (małoletni uczestnik)

Szkice ekranów KID-01 i KID-02 w stałej szerokości. Etykiety wewnątrz bloków są bez polskich znaków - to celowa konwencja makiety roboczej, dla pewnego wyrównania. Komentarz pod każdym szkicem jest pełną polszczyzną.

Źródło opisu: `KID_spec.md`. Status w manifeście: `lo_fi: todo` dla obu ekranów.

---

## KID-01 - ekran zgody opiekuna realizowany z konta dorosłego

```
+--------------------------------------------------+
| < Zgody opiekuna                          [PL v] |
+--------------------------------------------------+
| Dziecko: Zofia K., 8 lat                 [Zmien] |
| Opiekun: Anna K. (zalogowana, wladza rodzic.)    |
+--------------------------------------------------+
| (i) STAN: 2 z 4 wymaganych zgod udzielone        |
|     [#######...............]                     |
+--------------------------------------------------+
| Ponizej 16 lat zgode wyraza opiekun.             |
| [Dowiedz sie wiecej]                             |
+--------------------------------------------------+
| ZGODY ZWYKLE (kazdy cel osobno)                  |
|                                                  |
| Udzial w zajeciach                     [ ] Zgoda |
|   Cel: zapis i realizacja umowy                  |
|   Administrator: KS Delfin                       |
|   Dane: imie, wiek | Przech.: czas czlonk.       |
|   Skutek braku zgody: brak zapisu                |
|   Wersja 2.1 - 2026-04-10          [Pelna tresc] |
|..................................................|
| Wizerunek - zdjecia ze startu          [x] Zgoda |
|   Cel: publikacja galerii (RunPixie)             |
|   Wersja 1.3 - 2026-03-01          [Pelna tresc] |
+--------------------------------------------------+
| /!\ DANE SZCZEGOLNEJ KATEGORII (oddzielone)      |
|                                                  |
|   Dane o zdrowiu (wyjazd / oboz)                 |
|   Podstawa: art. 9 RODO                          |
|   [ ] Wyrazam OSOBNA, wyrazna zgode na           |
|       przetwarzanie danych o zdrowiu             |
|   Wersja 1.0 - 2026-02-15          [Pelna tresc] |
+--------------------------------------------------+
| (!) WALIDACJA  <- tu komunikat bledu:            |
| "Brak wymaganej zgody: Udzial w                  |
|  zajeciach. Zaznacz, aby domknac zapis."         |
+--------------------------------------------------+
|           [  UDZIEL WYMAGANYCH ZGOD  ]           |
|                                                  |
| [Zapisz na pozniej]  [Otworz polityke]           |
| [Wycofaj udzielona zgode]  (tak samo latwo)      |
+--------------------------------------------------+
```

Dominuje lista zgód z osobnym przełącznikiem dla każdego celu i jedna akcja główna na dole - ekran zbudowany od decyzji opiekuna, nie od pól w bazie, więc dominującym gestem jest aktywne, rozdzielone na cele zaznaczenie (EROD, 2020). Umknąć może to, że żadne pole nie jest zaznaczone z góry oraz że dane o zdrowiu siedzą w osobnej sekcji - jeśli karty będą wyglądać identycznie, znika rozróżnienie podstawy z art. 9 RODO, a razem z nim chowa się znacznik wersji i równie łatwe wycofanie zgody (RODO, 2016, art. 7 ust. 3). W makiecie hi-fi pokazałbym stan offline, w którym akcja główna jest wstrzymana, bo rejestracja rozliczalnego śladu zgody wymaga sieci, a obok alternatywnie stan "brak uprawnień" dla drugiego opiekuna bez władzy rodzicielskiej.

---

## KID-02 - opcjonalny widok tylko do odczytu dla nastolatka

```
+--------------------------------------------------+
| < Moj paszport             [TYLKO ODCZYT] [PL v] |
+--------------------------------------------------+
| (i) STAN: dane z 2026-05-25 14:02 (synch.)       |
+--------------------------------------------------+
| PASZPORT SPORTOWY                                |
|   Zofia K. - plywanie + biegi                    |
|   Poziom: Delfin 3                      [Otworz] |
+--------------------------------------------------+
| POSTEP UMIEJETNOSCI (drzewo skilli)              |
|   [x] Slizg na piersi                            |
|   [x] Oddychanie bokiem                          |
|   [ ] Nawrot koziolkowy                   [Film] |
+--------------------------------------------------+
| ZDJECIA ZE STARTU (RunPixie)                     |
|   [img] [img] [img]                       nr 142 |
+--------------------------------------------------+
| OSIAGNIECIA I WYNIKI (modul zawodow)             |
|   5 km - 28:14 (rekord zyciowy)                  |
+--------------------------------------------------+
| KOMUNIKAT  <- np. "Dostep wylaczyl opiekun"      |
| lub "Dane pojawia sie po pierwszych zajeciach"   |
+--------------------------------------------------+
| BRAK platnosci / portfela / zgod / zapisow       |
| - te powierzchnie NIE istnieja (nie wyszarzone)  |
+--------------------------------------------------+
|        Akcja glowna: PRZEGLADAJ (odczyt)         |
| [Zmien jezyk] [Otworz paszport] [Galeria]        |
+--------------------------------------------------+
```

Dominują trzy sekcje odczytowe - paszport, drzewo postępu i galeria - spięte trwałym znacznikiem "tylko odczyt" i datą synchronizacji, więc cały ekran komunikuje oglądanie zamiast zmiany stanu. Najłatwiej umknąć temu, że brak płatności, zgód i zapisów to nieobecność powierzchni akcji, a nie wyszarzone przyciski; w hi-fi kusi, żeby narysować disabled buttons, co łamie heurystykę zapobiegania błędom i przywraca dziecku widok funkcji zastrzeżonych dla opiekuna (Nielsen, 1994). Jako stan alternatywny w makiecie pokazałbym offline z wyraźną datą ostatniej synchronizacji albo moment, w którym opiekun cofa dostęp i widok się zamyka, kierując do informacji, kto go wyłączył.

---

Uwaga sprawdzająca względem specyfikacji: oba ekrany mają w manifeście pole "decyzja prawna przed wdrożeniem: TAK", więc szkic KID-01 świadomie nie rozstrzyga reguły przy dwojgu opiekunach ani progu wieku dla danego organizatora - zostawia na to miejsce, ale ich nie przesądza (Ustawa o ochronie danych osobowych, 2018).

---

## Literatura

Europejska Rada Ochrony Danych. (2020). *Wytyczne 05/2020 dotyczące zgody na mocy rozporządzenia 2016/679* (wersja 1.1). European Data Protection Board. https://www.edpb.europa.eu/our-work-tools/our-documents/guidelines/guidelines-052020-consent-under-regulation-2016679_pl

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

Rozporządzenie Parlamentu Europejskiego i Rady (UE) 2016/679 z dnia 27 kwietnia 2016 r. w sprawie ochrony osób fizycznych w związku z przetwarzaniem danych osobowych [RODO]. (2016). Dziennik Urzędowy Unii Europejskiej L 119/1. https://eur-lex.europa.eu/legal-content/PL/TXT/?uri=CELEX%3A32016R0679

Ustawa z dnia 10 maja 2018 r. o ochronie danych osobowych. (2018). Dziennik Ustaw 2018 poz. 1000. https://isap.sejm.gov.pl/isap.nsf/DocDetails.xsp?id=WDU20180001000
