# Szkice lo-fi grupy ATH - panel zawodnika (moduł zawodów)

Poniżej czternaście szkiców niskiej wierności dla ekranów ATH-01 do ATH-14, w układzie telefonu pionowego, bo to urządzenie podstawowe persony. Każdy blok pokazuje nagłówek, główne sekcje, pola, jedną akcję dominującą (podwójna ramka) i akcje drugorzędne. Miejsce komunikatu walidacji oznaczam jako `(!) WALIDACJA`, a wskaźnik stanu systemu jako `[STAN]` w nagłówku. Bez kolorów, zgodnie z zasadą, że kolor semantyczny wchodzi dopiero w hi-fi. Pod każdym szkicem trzy zdania: co dominuje, co może umknąć i jaki stan alternatywny warto narysować w makiecie wysokiej wierności.

## Legenda notacji

Znaczenia symboli: `[STAN]` to wskaźnik stanu systemu (synchronizacja, online/offline, status zgłoszenia); `(!) WALIDACJA` to zarezerwowane miejsce komunikatu błędu lub korekty przy danym polu; podwójna ramka `╔ ═ ╗` to akcja główna ekranu; `drug.:` zbiera akcje drugorzędne; `[ ... ]` to przycisk lub pole, a `( )` i `[x]` to przełączniki i pola wyboru. Strzałka `↗` prowadzi do pełnej treści dokumentu. Liczby i kwoty są przykładowe.

---

### ATH-01 - rejestracja i logowanie

```
┌────────────────────────────────────────────────┐
│ [logo organizatora]                  [PL ▾]    │
│ [STAN] ostatnie logowanie 12.05, 21:14         │
├────────────────────────────────────────────────┤
│ Zaloguj się lub załóż konto                    │
│ Jedno konto: Twoje starty + zapis dziecka      │
│                                                │
│ E-mail lub telefon                             │
│ [__________________________________]           │
│ (!) WALIDACJA: format / konto już istnieje     │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║     WYŚLIJ LINK LOGOWANIA (bez hasła)      ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ [ Zaloguj przez dostawcę tożsamości ]          │
│ ( ) zapamiętaj urządzenie + biometria          │
├────────────────────────────────────────────────┤
│ drug.: zmień język · nowy link · odzyskaj      │
│         dostęp · pomoc · regulamin             │
└────────────────────────────────────────────────┘
```

Dominuje logowanie bez hasła - jeden duży przycisk wysyłki linku przejmuje wzrok i skraca drogę do pulpitu. Łatwo przeoczyć krótką notę, że to samo konto obsłuży i własny start, i zapis dziecka, a właśnie ona chroni rodzica-zawodnika przed założeniem duplikatu (Nielsen, 1994). W hi-fi pokażę wariant urządzenia zapamiętanego, gdzie zamiast pola e-mail wchodzi monit biometryczny jako akcja dominująca.

---

### ATH-02 - dashboard z moimi zapisami

```
┌────────────────────────────────────────────────┐
│ Moje starty                          [PL ▾]    │
│ [STAN] sync 12.05 21:14 · online               │
├────────────────────────────────────────────────┤
│ ! ALERTY (wg pilności)                         │
│  > Zaległa płatność - dokończ  [ZAPŁAĆ]        │
│  > Oferta z listy rezerwowej  09:58 [...]      │
│  > Brakująca zgoda            [UZUPEŁNIJ]      │
├────────────────────────────────────────────────┤
│ Najbliższy start  Bieg X · 18.05 · 10 km       │
│   numer startowy: — (po opłaceniu)             │
│   [kafel: status / odliczanie]                 │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║      PRZEJDŹ DO NAJPILNIEJSZEJ SPRAWY      ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ (!) WALIDACJA: nie pobrano kafelka [ponów]     │
├────────────────────────────────────────────────┤
│ drug.: szczegóły · wydarzenia · portfel ·      │
│         galeria · klasyfikacja                 │
└────────────────────────────────────────────────┘
```

Dominuje lista alertów uszeregowana po pilności, z akcją wprost na kafelku, więc oko trafia najpierw w to, co wymaga reakcji teraz. Umknąć może znacznik synchronizacji i baner offline, a bez nich zawodnik na starcie weźmie nieaktualne dane za świeże (Nielsen, 1994). Dla hi-fi narysuję stan offline z datą ostatniej synchronizacji i numerem startowym dostępnym lokalnie.

---

### ATH-03 - formularz zapisu (pola warunkowe)

```
┌────────────────────────────────────────────────┐
│ Zapis: Bieg X / 10 km          krok 2 z 4      │
│ [STAN] ●●○○  postęp · miejsca: 37              │
├────────────────────────────────────────────────┤
│ [przypięte] 10 km · 120,00 zł  zmiana ceny     │
│             za 02:11:40                        │
├────────────────────────────────────────────────┤
│ Imię i nazwisko  [wstępnie z konta]            │
│ Data urodzenia   [__.__.____]                  │
│ Kategoria        [auto: M40] (zmień)           │
│ Kontakt alarmowy [__________________]          │
│ + pole warunkowe (klasa startowa) ...          │
│ (!) WALIDACJA: wiek poza zakresem dystansu     │
│     -> proponowany dystans 5 km                │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║          DALEJ: DODATKI I GADŻETY          ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: zmień dystans · zapisz roboczą ·        │
│         wróć do listy                          │
└────────────────────────────────────────────────┘
```

Dominuje pojedynczy krok formularza z przypiętym podsumowaniem ceny i wskaźnikiem postępu, a kolejne pola odsłaniają się dopiero, gdy stają się istotne (Wroblewski, 2008). Cicho zniknąć może komunikat o zmianie progu cenowego w trakcie wypełniania, choć to on ratuje zawodnika przed inną kwotą przy płatności. Wariant alternatywny do makiety to stan wyczerpanych miejsc, który kieruje na listę rezerwową, zamiast pozwolić dokończyć zapis nie do opłacenia.

---

### ATH-04 - dodatki i gadżety

```
┌────────────────────────────────────────────────┐
│ Dodatki (opcjonalne)                 [PL ▾]    │
│ [STAN] magazyn na żywo                         │
├────────────────────────────────────────────────┤
│ [ Koszulka tech ]  rozmiar: [ M ▾]  79 zł      │
│    stan: zostały 3 szt.   tabela rozmiarów     │
│ [ Medal pamiątkowy ]               29 zł       │
├────────────────────────────────────────────────┤
│ Podpowiedź (pogoda 6°C): rękawki  +19 zł       │
│    to sugestia, nie domyślny wybór             │
├────────────────────────────────────────────────┤
│ (!) WALIDACJA: wybierz rozmiar / wariant       │
│     niedostępny -> najbliższy: L               │
│ [przypięte] Suma: 120 + 79 = 199,00 zł         │
│ ( ) pokryj z portfela (-50,00)                 │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║         DALEJ: ZGODY I REGULAMINY          ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: dodaj/usuń · zmień rozmiar · pomiń      │
└────────────────────────────────────────────────┘
```

Dominują karty produktów z realnym stanem magazynu i przypięta suma, która rośnie na oczach przy każdej zmianie. Sekcja sugestii bywa brana za domyślny wybór, choć ma pozostać podpowiedzią dopasowaną do dystansu i prognozy, więc to rozróżnienie trzeba utrzymać wizualnie. Hi-fi powinno pokazać wariant zniknięcia ostatniej sztuki z koszyka, gdzie pozycja wypada bez obciążenia i z czytelnym wyjaśnieniem.

---

### ATH-05 - zgody i regulaminy

```
┌────────────────────────────────────────────────┐
│ Zgody                                [PL ▾]    │
│ [STAN] wymagane do uzupełnienia: 1             │
├────────────────────────────────────────────────┤
│ WYMAGANE                                       │
│ [x] Regulamin wydarzenia   v3 · 02.05  ↗       │
│ [ ] Polityka prywatności   v2 · 10.04  ↗       │
│ [ ] Oświadczenie zdrowotne (osobne wejście)    │
├────────────────────────────────────────────────┤
│ DOBROWOLNE                                     │
│ [ ] Zgoda na wizerunek (galeria RunPixie)      │
│     odmowa NIE blokuje startu                  │
│                                                │
│ (!) WALIDACJA: brak zgody -> przewiń do niej   │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║     ZAAKCEPTUJ I PRZEJDŹ DO PŁATNOŚCI      ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: otwórz treść · pobierz · wróć           │
└────────────────────────────────────────────────┘
```

Dominuje podział na zgody wymagane i dobrowolne wraz z licznikiem, ile wymaganych jeszcze brakuje do odblokowania płatności. Najłatwiej przeoczyć, że odmowa zgody na wizerunek jest dozwolona i nie blokuje startu, a jedynie ogranicza galerię RunPixie. W makiecie hi-fi pokażę stan ponownej akceptacji po zmianie wersji regulaminu, z zaznaczeniem tego, co dokładnie się zmieniło.

---

### ATH-06 - podsumowanie i płatność

```
┌────────────────────────────────────────────────┐
│ Podsumowanie i płatność              [PL ▾]    │
│ [STAN] rezerwacja miejsca: 04:59               │
├────────────────────────────────────────────────┤
│ Wpisowe (próg II)              120,00 zł       │
│ Koszulka tech (M)               79,00 zł       │
│ Kredyt z portfela              -50,00 zł       │
│ ------------------------------------------     │
│ Do zapłaty                     149,00 zł       │
├────────────────────────────────────────────────┤
│ Numer startowy: nadany PO zaksięgowaniu        │
│ Metoda: ( )P24 ( )PayU ( )Stripe               │
│         ( )BLIK ( )karta                       │
│ (!) WALIDACJA: odrzucenie bramki + powód       │
│     -> zaproponuj inną metodę                  │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║            ZAPŁAĆ I SFINALIZUJ             ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: zmień metodę · wróć do edycji pozycji   │
└────────────────────────────────────────────────┘
```

Dominuje rozbicie kwoty i jeden przycisk finalizacji z konkretną sumą, a tuż obok tyka licznik rezerwacji miejsca. Drobny, lecz nośny szczegół - zdanie, że numer nadaje się dopiero po zaksięgowaniu wpłaty - bywa pomijany, mimo że porządkuje całą późniejszą logikę galerii, list i klasyfikacji. Alternatywny stan do narysowania to odrzucenie płatności przez bramkę z powodem wyłożonym po ludzku i propozycją innej metody.

---

### ATH-07 - potwierdzenie zapisu

```
┌────────────────────────────────────────────────┐
│ [STAN] OPŁACONE · numer nadany                 │
├────────────────────────────────────────────────┤
│   ✓ Jesteś zapisany                            │
│                                                │
│         TWÓJ NUMER STARTOWY: 1287              │
│                                                │
│    ┌───────────┐                               │
│    │  [ QR ]   │  kod odbioru pakietu          │
│    └───────────┘                               │
├────────────────────────────────────────────────┤
│ Bieg X · 18.05 · 10 km · kat. M40              │
│ Następne kroki: odbiór 17.05 16-20, hala A     │
│                 start 10:00, zabierz: ...      │
│ (!) WALIDACJA: wpłata OK, numer w nadawaniu    │
│     -> powiadomimy, gdy będzie                 │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║     ZAPISZ START DO TELEFONU (offline)     ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: potwierdzenie płatności · samoobsługa   │
└────────────────────────────────────────────────┘
```

Dominuje baner sukcesu z numerem startowym i kod QR odbioru pakietu, bo to one są celem całej ścieżki. Pod progiem uwagi ląduje sekcja następnych kroków, czyli godzina i miejsce odbioru, którą zawodnik docenia dopiero na starcie. Hi-fi powinno objąć stan przejściowy wpłaty zaksięgowanej z numerem w trakcie nadawania, żeby opóźnienie nie wyglądało jak porażka.

---

### ATH-08 - samoobsługa zgłoszenia

```
┌────────────────────────────────────────────────┐
│ Moje zgłoszenie: Bieg X / 10 km                │
│ [STAN] numer 1287 · status: opłacone           │
├────────────────────────────────────────────────┤
│ Dostępne operacje (reguły organizatora):       │
│  > Transfer do innej osoby   płatne 20 zł      │
│      termin do 10.05                           │
│  > Odroczenie na 2027        darmowe           │
│  > Wymiana numeru / pakietu  darmowe           │
│  > Zejście z listy rezerw.   darmowe           │
├────────────────────────────────────────────────┤
│ NISZCZĄCE (wyraźne potwierdzenie):             │
│  ! Rezygnacja + zwrot   zwrot: 90 zł -> portfe │
│ (!) WALIDACJA: po terminie -> kontakt z org.   │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║          WYKONAJ WYBRANĄ OPERACJĘ          ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: porównaj · reguły wydarzenia · anuluj   │
└────────────────────────────────────────────────┘
```

Dominuje lista operacji z wyraźnym oznaczeniem darmowych i płatnych oraz terminem, do którego każda jest możliwa. Skutek nieodwracalny, jak utrata prawa do startu przy transferze albo kwota zwrotu przy rezygnacji, może zginąć, jeśli potwierdzenie nie wyłoży go wprost. Wariant alternatywny do hi-fi: oferta z listy rezerwowej z odliczaniem jako akcja dominująca, zamiast zwykłej listy operacji.

---

### ATH-09 - dosprzedaż gadżetów

```
┌────────────────────────────────────────────────┐
│ Dokup do startu: Bieg X              [PL ▾]    │
│ [STAN] okno zamówień do 12.05 · magazyn live   │
├────────────────────────────────────────────────┤
│ MOJE ZAMÓWIENIA                                │
│ [ Koszulka tech ]  M -> zmień rozmiar          │
├────────────────────────────────────────────────┤
│ KATALOG                                        │
│ [ Czapka ]            zostały 5 szt.  39 zł    │
│ [ Medal dla osoby tow.]               29 zł    │
│ Podpowiedź (pogoda): buff           19 zł      │
├────────────────────────────────────────────────┤
│ (!) WALIDACJA: wariant niedostępny -> L        │
│     ostatnia sztuka zniknęła -> bez obciążenia │
│ Suma nowego zamówienia:            68,00 zł    │
│ ( ) pokryj z portfela                          │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║               DOKUP I ZAPŁAĆ               ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: zmień rozmiar · usuń · tabela · pomiń   │
└────────────────────────────────────────────────┘
```

Dominuje katalog po zapisie z sekcją moich zamówień i wymianą rozmiaru w jednym miejscu. Łatwo przeoczyć termin zamknięcia okna zamówień oraz sposób odbioru dokupionych rzeczy, a od tego zależy, czy zawodnik w ogóle zdąży. Dla hi-fi narysuję stan zamkniętego okna zamówień, z informacją, do kiedy było otwarte i czy odbiór na miejscu wchodzi w grę.

---

### ATH-10 - portfel i kredyty

```
┌────────────────────────────────────────────────┐
│ Portfel (wspólny z PAR-10)           [PL ▾]    │
│ [STAN] sync 12.05 21:14                        │
├────────────────────────────────────────────────┤
│ SALDO WG ORGANIZATORA (nieprzenoszalne)        │
│   Organizator A           120,00 zł            │
│   Organizator B             0,00 zł            │
├────────────────────────────────────────────────┤
│ HISTORIA                                       │
│   10.05 zwrot za rezygnację   +90,00           │
│   02.05 nadpłata              +30,00           │
├────────────────────────────────────────────────┤
│ DOKUMENTY PŁATNOŚCI                            │
│   faktura 04/2026            [pobierz]         │
│ (!) WALIDACJA: kredyt org. A != start org. B   │
│     dokument w przygotowaniu -> powiadomimy    │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║   WYKORZYSTAJ KREDYT PRZY NAJBL. PŁATN.    ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: filtruj · pobierz · wniosek o wypłatę   │
└────────────────────────────────────────────────┘
```

Dominuje saldo rozbite po organizatorach, bo kredyt nie przenosi się między nimi i to jest pierwsza rzecz do zrozumienia. Zasada nieprzenoszalności kredytu bywa pomijana, dopóki zawodnik nie spróbuje zapłacić nim u innego organizatora i nie zobaczy odmowy. Hi-fi powinno pokazać stan pusty konta bez operacji, z wyjaśnieniem, że kredyt pojawi się dopiero po nadpłacie albo zwrocie.

---

### ATH-11 - galeria zdjęć z RunPixie

```
┌────────────────────────────────────────────────┐
│ Moje zdjęcia (numer 1287)            [PL ▾]    │
│ [STAN] dopasowano po numerze · przetwarzanie   │
├────────────────────────────────────────────────┤
│ Filtr punktu trasy: [ meta ▾]                  │
├────────────────────────────────────────────────┤
│  [img] [img] [img]    pewność: wysoka          │
│  [img] [img] [img]                             │
│  [img?] niepewne -> [to ja] / [nie ja]         │
├────────────────────────────────────────────────┤
│ (!) WALIDACJA: brak numeru (nieopłacone)       │
│     -> galeria działa po nadaniu numeru        │
│     zdjęcia w przetwarzaniu: komplet ~2h       │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║          POBIERZ WYBRANE ZDJĘCIA           ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: udostępnij (wg zgody) · filtruj ·       │
│         ulubione · podgląd                     │
└────────────────────────────────────────────────┘
```

Dominuje siatka miniatur dopasowanych po numerze startowym, dzięki czemu zawodnik nie przegląda tysięcy cudzych zdjęć (Nielsen, 1994). Znacznik pewności dopasowania i prośba o potwierdzenie niepewnych przypadków łatwo umykają, a chronią przed pomieszaniem zdjęć dwóch osób. W makiecie pokażę stan pusty dla biegu jeszcze przetwarzanego, z informacją, kiedy spodziewać się kompletu.

---

### ATH-12 - klasyfikacja serii z moim wynikiem

```
┌────────────────────────────────────────────────┐
│ Klasyfikacja serii          [generalna|kat.]   │
│ [STAN] aktual. 18.05 · WYNIK WSTĘPNY           │
├────────────────────────────────────────────────┤
│ Filtr: [ edycja ▾] [ dystans ▾]                │
├────────────────────────────────────────────────┤
│  18.  Kowalski A.     00:48:11   120 pkt       │
│  19. >> TY            00:48:39   118 pkt <<    │
│  20.  Nowak P.        00:49:02   115 pkt       │
│    strata do 18.: 28 s / 2 pkt                 │
├────────────────────────────────────────────────┤
│ (!) WALIDACJA: brak Ciebie mimo ukończenia     │
│     -> możliwe przyczyny + reklamacja          │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║       ZOBACZ MÓJ WYNIK W KONTEKŚCIE        ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: kategoria · zasady punktacji ·          │
│         udostępnij · pobierz                   │
└────────────────────────────────────────────────┘
```

Dominuje tabela z przypiętym i wyróżnionym wierszem zawodnika, do którego widok przewija się sam. Znacznik wyniku wstępnego przed weryfikacją bywa niezauważony, przez co zawodnik bierze liczbę za ostateczną. Alternatywny stan do hi-fi to brak zawodnika w klasyfikacji mimo ukończonego biegu, z wyjaśnieniem przyczyn i ścieżką reklamacji.

---

### ATH-13 - powiadomienia pogodowe

```
┌────────────────────────────────────────────────┐
│ Pogoda na start: Bieg X · 18.05 10:00          │
│ [STAN] aktual. 12.05 21:14 (strefa startu)     │
├────────────────────────────────────────────────┤
│ ! OSTRZEŻENIE ORGANIZATORA (ma pierwszeństwo)  │
│   Burze 9-11, możliwa zmiana godziny  ↗        │
├────────────────────────────────────────────────┤
│ Prognoza 10:00   6°C (odczuw. 3°C)             │
│    wiatr 25 km/h · opady 40%                   │
│    [ oś godzinowa: 8 9 10 11 12 ]              │
├────────────────────────────────────────────────┤
│ Podpowiedź ubioru: rękawki, buff  -> gadżety   │
│ (!) WALIDACJA: brak danych -> bez zmyślania    │
│     offline: prognoza może być nieaktualna     │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║   USTAW POWIADOMIENIE O ZMIANIE WARUNKÓW   ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: odśwież · komunikat org. · wyłącz       │
└────────────────────────────────────────────────┘
```

Dominuje karta prognozy dla godziny startu, a nad nią, gdy istnieje, baner ostrzeżenia organizatora w kolorze semantycznym. Znacznik czasu ostatniej aktualizacji łatwo pominąć, lecz to on odróżnia świeżą prognozę od starej, zassanej offline. Hi-fi powinno objąć stan błędu pobrania, w którym zamiast zmyślonych liczb pojawia się czytelny komunikat o chwilowej niedostępności danych.

---

### ATH-14 - ustawienia konta i zgody

```
┌────────────────────────────────────────────────┐
│ Ustawienia (wspólne z PAR-16)        [PL ▾]    │
│ [STAN] zgody aktualne · 1 do odnowienia        │
├────────────────────────────────────────────────┤
│ Dane kontaktowe / startowe   [edytuj]          │
├────────────────────────────────────────────────┤
│ ZGODY (wersja · data · wycofanie)              │
│   Regulamin v3 · 02.05        [x] ↗            │
│   Wizerunek                   [ ] ↗            │
├────────────────────────────────────────────────┤
│ POWIADOMIENia (typ × kanał)                    │
│           push  e-mail  SMS                    │
│   rezerw.  [x]   [x]    [ ]                    │
│   pogoda   [x]   [ ]    [ ]                    │
│ (!) WALIDACJA: wyłączasz kanał krytyczny?      │
├────────────────────────────────────────────────┤
│ Dane: [pobierz]    Konto: [USUŃ KONTO]         │
│                                                │
│ ╔════════════════════════════════════════════╗ │
│ ║             ZAPISZ USTAWIENIA              ║ │
│ ╚════════════════════════════════════════════╝ │
│                                                │
│ drug.: język/region · dane zdrowotne · logi    │
└────────────────────────────────────────────────┘
```

Dominuje lista zgód z wersją i datą oraz macierz powiadomień w układzie typ zdarzenia względem kanału. Skutki akcji niszczącej, czyli usunięcia konta przy aktywnym starcie i dodatnim saldzie portfela, mogą umknąć, jeśli potwierdzenie ich nie rozpisze. W makiecie hi-fi pokażę ostrzeżenie przy wyłączaniu kanału dla zdarzeń krytycznych, takich jak oferta z listy rezerwowej albo ostrzeżenie pogodowe.

---

## Literatura

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

Wroblewski, L. (2008). *Web form design: Filling in the blanks*. Rosenfeld Media. https://rosenfeldmedia.com/books/web-form-design/

W3C. (2018). *Web Content Accessibility Guidelines (WCAG) 2.1*. World Wide Web Consortium. https://www.w3.org/TR/WCAG21/
