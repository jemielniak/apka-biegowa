# Szkice lo-fi - panel instruktora (grupa INS, moduł zajęć)

Dziesięć ekranów z `INS_spec.md`, ujęcie mobile-first. Każdy blok pokazuje nagłówek, sekcje, pola, akcję główną i drugorzędne, miejsce walidacji oraz wskaźnik stanu. Bez kolorów.

**Legenda.** `═ ║ ╔╗╚╝` = akcja główna (jedna na ekran) · `─ │ ┌┐└┘` = akcja drugorzędna lub pole · `‹ … ›` = miejsce komunikatu walidacji · `◍` = wskaźnik stanu/synchronizacji · `!` = stan offline lub ostrzeżenie · `▾` = lista rozwijana · `[PL ▾]` = przełącznik języka w nagłówku.

## INS-01 - logowanie

```
┌──────────────────────────────────────────────┐
│ [LOGO placówki]                      [PL ▾]  │
├──────────────────────────────────────────────┤
│                                              │
│              Panel instruktora               │
│                                              │
│ Identyfikator / e-mail                       │
│ ┌──────────────────────────────────────────┐ │
│ │                                          │ │
│ └──────────────────────────────────────────┘ │
│ ‹walidacja: „uzupełnij identyfikator         │
│   lub poproś o nowy link                     │
│                                              │
│  ╔════════════════════════════════════════╗  │
│  ║          Zaloguj się linkiem           ║  │
│  ╚════════════════════════════════════════╝  │
│   na zapamiętanym urządzeniu →               │
│   odcisk / Face ID / kod                     │
│                                              │
│ [ ] Zapamiętaj to urządzenie                 │
│                                              │
│ Poproś o nowy link · Zmień język · Pomoc     │
├──────────────────────────────────────────────┤
│ ◍ ostatnie udane logowanie: wczoraj 18:04    │
│ ! offline → logowanie wymaga sieci;          │
│   zmiany w kolejce na tym urządzeniu: 3      │
└──────────────────────────────────────────────┘
```

*Co dominuje.* Dominuje logowanie bez hasła, a na zapamiętanym urządzeniu skrót biometryczny - jeden duży cel dla mokrego palca.

*Co może umknąć.* Łatwo zlać w jeden ogólnik trzy różne przyczyny odmowy: zły identyfikator, wygasły link i brak przypisania do placówki.

*Stan alternatywny do hi-fi.* W hi-fi pokaż stan „konto bez przypisanej placówki” z jasnym kierunkiem do organizatora zamiast wpuszczania do panelu.

---

## INS-02 - dashboard z dzisiejszymi zajęciami

```
┌──────────────────────────────────────────────┐
│ Dziś, wtorek 27 maja            [PL ▾]       │
│ ◍ sync: 2 min temu · 0 w kolejce             │
├──────────────────────────────────────────────┤
│ ══ NAJBLIŻSZE ══════════════════════         │
│  16:00 · Tor 2 · Delfinki (poz. 3)           │
│  zapisanych: 11   obecność: nierozp.         │
│  ╔════════════════════════════════════════╗  │
│  ║          Rozpocznij obecność           ║  │
│  ╚════════════════════════════════════════╝  │
│                                              │
│ ── Później dziś ────────────────────         │
│  17:00 · Sala A · Żabki   [zastępstwo]       │
│  18:00 · Tor 1 · Rekiny   [zamknięta]        │
│                                              │
│ ‹alert: zmiana sali 17:00 → Sala B›          │
│                                              │
│ Mój grafik · Lista grupy · Dostępność        │
├──────────────────────────────────────────────┤
│ ! offline → plan z ostatniej sync;           │
│   obecność i tak działa lokalnie             │
└──────────────────────────────────────────────┘
```

*Co dominuje.* Dominuje najbliższa karta zajęć z dużym przyciskiem startu obecności prowadzącym prosto do INS-04.

*Co może umknąć.* Znacznik zastępstwa i alert o zmianie sali mogą zniknąć pod osią dnia, jeśli kart jest dużo.

*Stan alternatywny do hi-fi.* W hi-fi pokaż stan offline z banerem na ostatnio zsynchronizowanym planie i wyraźnym „obecność działa lokalnie”.

---

## INS-03 - własny grafik

```
┌──────────────────────────────────────────────┐
│ Mój grafik          [Dzień|Tydzień] [PL ▾]   │
│ Tydz. 22 · ‹plac. A ▾›   ◍ sync ok           │
├──────────────────────────────────────────────┤
│ Pon  16:00 Delfinki   Tor 2                  │
│ Wt   16:00 Delfinki   Tor 2                  │
│ Wt   17:00 Żabki      Sala A  [zast.]        │
│ Śr   —                                       │
│ Czw  18:00 Rekiny     Tor 1  [odwoł.]        │
│ Pt   16:00 Delfinki   Tor 2                  │
│                                              │
│ ‹info: zmiany terminu ustawia organizator›   │
│  ╔════════════════════════════════════════╗  │
│  ║          Otwórz wybrany blok           ║  │
│  ╚════════════════════════════════════════╝  │
│                                              │
│ Zgłoś niedostępność · Dodaj do kalendarza    │
│ Następny tydzień →                           │
├──────────────────────────────────────────────┤
│ ! offline → tydzień z ostatniej sync,        │
│   aktualizacja wstrzymana                    │
└──────────────────────────────────────────────┘
```

*Co dominuje.* Dominuje siatka tygodnia złożona wyłącznie z własnych bloków instruktora, bez zajęć innych trenerów.

*Co może umknąć.* Bez komunikatu, że edycja terminu należy do organizatora, instruktor będzie szukał przycisku, którego nie ma.

*Stan alternatywny do hi-fi.* W hi-fi pokaż tydzień pusty - zero przypisań - z kierunkiem do zgłoszenia dostępności jako właściwej ścieżki.

---

## INS-04 - lista obecności (odprawa, QR, offline)

```
┌──────────────────────────────────────────────┐
│ Delfinki · 16:00 · Tor 2     [PL ▾]          │
│ Obecni 7 / 11      ! OFFLINE · kolejka 4     │
├──────────────────────────────────────────────┤
│ szukaj dziecka                               │
│ ┌──────────────────────────────────────────┐ │
│ │ wpisz imię…                              │ │
│ └──────────────────────────────────────────┘ │
│ ┌─────────────────────────────┬───────┐      │
│ │ Kowalska, Zofia             │[OB][N]│      │
│ │ Nowak, Jan        [spóźn.]  │[OB][N]│      │
│ │ Nowak, Jan B.   [rodzeństwo]│[OB][N]│      │
│ │ Wiśniewski, A.  [zgł. nieob]│[ ][ ●]│      │
│ │ Gość: Lewy (odrabia)        │[OB][N]│      │
│ └─────────────────────────────┴───────┘      │
│ ‹walidacja: 4 dzieci bez oznaczenia ‹        │
│   → zostawić jako nieobecne?›                │
│  ╔════════════════════════════════════════╗  │
│  ║        Zamknij i zsynchronizuj         ║  │
│  ╚════════════════════════════════════════╝  │
│ Skanuj QR · Dopisz gościa · Notatka          │
├──────────────────────────────────────────────┤
│ ! dane bezpieczne lokalnie, pójdą po sieci   │
└──────────────────────────────────────────────┘
```

*Co dominuje.* Dominuje lista dużych wierszy obecny/nieobecny z licznikiem - cała szerokość wiersza jest celem dotykowym.

*Co może umknąć.* Stan kolejki offline i ostrzeżenie o konflikcie dwóch urządzeń odhaczających tę samą grupę bywają najsłabiej widoczne.

*Stan alternatywny do hi-fi.* W hi-fi pokaż offline jako stan pierwszej klasy: stały baner trybu, licznik zmian w kolejce i moment ostatniej synchronizacji.

---

## INS-05 - aktualizacja umiejętności (drzewo, filmy, offline)

```
┌──────────────────────────────────────────────┐
│ Umiejętności · ‹Kowalska, Z. ▾›  [PL ▾]      │
│ Poziom 3 · ◍ sync ok · film: w tle           │
├──────────────────────────────────────────────┤
│ Gałąź: Pływanie na piersiach ▾               │
│  ● Wydech do wody       [zaliczone]          │
│  ◐ Praca nóg            [w toku ▾]           │
│  ○ Koordynacja          [ustaw ▾]            │
│    miniatura filmu: [> 0:08]                 │
│                                              │
│ ‹walidacja: zaliczenie bez etapu „w toku” ›  │
│   → pominąć etap pośredni?›                  │
│  ╔════════════════════════════════════════╗  │
│  ║           Zaznacz opanowanie           ║  │
│  ╚════════════════════════════════════════╝  │
│ + Dołącz film · Cofnij status · Notatka      │
│ Zastosuj do podgrupy · Zmień dziecko         │
├──────────────────────────────────────────────┤
│ ! offline → statusy zapisane lokalnie,       │
│   nagrania czekają na lżejsze łącze          │
└──────────────────────────────────────────────┘
```

*Co dominuje.* Dominuje drzewo z dużymi przełącznikami stanu dla wybranego dziecka, gotowe do obsługi mokrym palcem.

*Co może umknąć.* Można pomylić dwa różne stany synchronizacji: status umiejętności jest już zapisany, a film dopiero idzie w tle.

*Stan alternatywny do hi-fi.* W hi-fi pokaż akcję zbiorczą „zastosuj ten sam postęp do podgrupy” - to realny skrót po typowych zajęciach.

---

## INS-06 - profil uczestnika (tryb odczytu)

```
┌──────────────────────────────────────────────┐
│ ← Grupa     Kowalska, Zofia      [PL ▾]      │
│ Delfinki · Poziom 3 · ◍ dane świeże          │
├──────────────────────────────────────────────┤
│ ┌────────────────────────────────────┐       │
│ │ ! UWAGI BEZPIECZEŃSTWA               │     │
│ │ Astma wysiłkowa - inhalator w torbie │     │
│ └────────────────────────────────────┘       │
│ Postęp (odczyt)            5/12 ›            │
│ Frekwencja (30 dni)        92% ›             │
│                                              │
│ Kontakt na czas zajęć: opiekun               │
│  ╔════════════════════════════════════════╗  │
│  ║        Aktualizuj umiejętności         ║  │
│  ╚════════════════════════════════════════╝  │
│ tel: Zadzwoń do opiekuna · Otwórz frekwencję │
│ ‹info: pełna karta dziecka jest u organizator│
├──────────────────────────────────────────────┤
│ ! offline → ostatnio pobrane dane profilu    │
└──────────────────────────────────────────────┘
```

*Co dominuje.* Dominuje nagłówek z imieniem i poziomem oraz wyróżniona ramka uwag bezpieczeństwa widoczna bez przewijania.

*Co może umknąć.* Gdy uwag brak, łatwo zostawić puste pole - a ono budzi niepewność zamiast neutralnego „nic nie odnotowano”.

*Stan alternatywny do hi-fi.* W hi-fi pokaż stan braku uprawnień: sekcje poza zakresem instruktora ukryte z wyjaśnieniem, gdzie jest pełna karta.

---

## INS-07 - zgłaszanie dostępności

```
┌──────────────────────────────────────────────┐
│ Moja dostępność                  [PL ▾]      │
│ Tydz. 23 · ‹cykliczna|jednorazowa›           │
├──────────────────────────────────────────────┤
│      Pn Wt Śr Cz Pt So Nd                    │
│ rano  ▓  ▓  ░  ▓  ▓  ░  ░                    │
│ popoł ▓  ▓  ▓  ▓  ▓  ▓  ░                    │
│ wiecz ░  ▓  ░  ▓  ░  ░  ░                    │
│  ▓ dostępny  ░ niedostępny                   │
│                                              │
│ Zgłoszenia: Czw 18:00 — oczekuje ⏳           │
│ ‹walidacja: termin ma przypisane zajęcia ‹   │
│   → wymaga zastępstwa, decyduje organizator› │
│  ╔════════════════════════════════════════╗  │
│  ║           Zapisz dostępność            ║  │
│  ╚════════════════════════════════════════╝  │
│  ┌────────────────────────────────────────┐  │
│  │      Zgłoś niedostępność (termin)      │  │
│  └────────────────────────────────────────┘  │
│ Edytuj okno cykliczne · Wycofaj zgłoszenie   │
├──────────────────────────────────────────────┤
│ ! offline → zgłoszenie wyśle się po sieci    │
└──────────────────────────────────────────────┘
```

*Co dominuje.* Dominuje siatka okien tygodnia z dwoma akcjami: zapis dostępności ogólnej i zgłoszenie niedostępności na konkretny termin.

*Co może umknąć.* Okno wyprzedzenia placówki i ścieżka prośby pilnej poniżej tego okna mogą zostać przeoczone.

*Stan alternatywny do hi-fi.* W hi-fi pokaż zgłoszenie na termin z już przypisanymi zajęciami, z ostrzeżeniem o konieczności zastępstwa.

---

## INS-08 - ewidencja godzin

```
┌──────────────────────────────────────────────┐
│ Ewidencja godzin    ‹Maj 2026 ▾› [PL ▾]      │
│ Status okresu: OTWARTY · ◍ sync ok           │
├──────────────────────────────────────────────┤
│ ╔══════════════════════════════════╗         │
│ ║ SUMA: 38,5 h                      ║        │
│ ╚══════════════════════════════════╝         │
│ własne 30 · zastępstwa 5 · 1:1 3,5           │
│ Data   Grupa       Czas   Δ plan             │
│ 06.05  Delfinki    1,0 h   =     ›           │
│ 06.05  Żabki       1,0 h  +0,25  ›           │
│ 08.05  Rekiny      —      brak ob.›          │
│ ‹info: zajęcia bez zamkniętej obecności      │
│   nie wliczają się, dopóki nie domkniesz›    │
│  ╔════════════════════════════════════════╗  │
│  ║        Potwierdź godziny okresu        ║  │
│  ╚════════════════════════════════════════╝  │
│ Zgłoś korektę wpisu · Eksport · Zmień okres  │
├──────────────────────────────────────────────┤
│ ! offline → odczyt; potwierdzenie wymaga siec│
└──────────────────────────────────────────────┘
```

*Co dominuje.* Dominuje suma godzin na górze i akcja potwierdzenia całego okresu - bez stawek i kwot, zgodnie z granicą roli.

*Co może umknąć.* Łatwo przeoczyć, że zajęcia bez zamkniętej obecności w ogóle nie wchodzą do sumy, dopóki instruktor ich nie domknie w INS-04.

*Stan alternatywny do hi-fi.* W hi-fi pokaż różnicę plan kontra wykonanie wyświetloną przed zatwierdzeniem, gdy pojawia się niezgodność.

---

## INS-09 - kalendarz lekcji jeden na jeden

```
┌──────────────────────────────────────────────┐
│ Lekcje 1:1   [Dzień|Tydzień]     [PL ▾]      │
│ Czw 29.05 · ‹Tor 1 ▾› · ◍ sync ok            │
├──────────────────────────────────────────────┤
│ 15:00 ┌ wolne okno ───────── [otwarte]       │
│ 15:30 │ Malinowski K.  [potwierdz.] ›        │
│ 16:00 │ — wolne —                            │
│ 16:30 │ Zieliński O.   [oczekuje ⏳] ›        │
│       cel: „praca nad startem” (rodzic)      │
│                                              │
│ ‹walidacja: okno koliduje z grupą 16:00 ‹    │
│   → proponowany wolny czas: 17:00›           │
│  ╔════════════════════════════════════════╗  │
│  ║         Otwórz / zablokuj okno         ║  │
│  ╚════════════════════════════════════════╝  │
│ Na karcie rezerwacji → „Obecność lekcji”     │
│ Zaproponuj inny termin · Notatka · Profil    │
├──────────────────────────────────────────────┤
│ ! offline → obecność lekcji lokalnie;        │
│   otwieranie okien wymaga sieci              │
└──────────────────────────────────────────────┘
```

*Co dominuje.* Dominuje kalendarz dnia lub tygodnia z rezerwacjami i akcją otwierania albo blokowania wolnych okien.

*Co może umknąć.* Kolizja z grafikiem grupowym oraz status rezerwacji (oczekuje kontra potwierdzona) bywają trudne do odczytania na małym ekranie.

*Stan alternatywny do hi-fi.* W hi-fi pokaż widok karty pojedynczej rezerwacji, gdzie akcją dominującą jest oznaczenie obecności po lekcji.

---

## INS-10 - komunikaty do grupy lub rodziców

```
┌──────────────────────────────────────────────┐
│ Nowy komunikat                   [PL ▾]      │
│ ◍ ostatnia wysyłka: dziś 9:12                │
├──────────────────────────────────────────────┤
│ Odbiorca:  (•) Cała grupa  ( ) Rodzina       │
│ Kanał:     [push ▾]  (wg zgód rodziców)      │
│ Szablon:   ‹Przypomnienie o stroju ▾›        │
│ Treść                                        │
│ ┌──────────────────────────────────────────┐ │
│ │ Jutro proszę o klapki i czepek…          │ │
│ └──────────────────────────────────────────┘ │
│ ‹walidacja: 2 rodziny bez zgody na push ‹    │
│   → pominięte; zaproponuj inny kanał›        │
│ ‹ostrzeżenie: dane wrażliwe → wyślij 1:1›    │
│  ╔════════════════════════════════════════╗  │
│  ║            Wyślij komunikat            ║  │
│  ╚════════════════════════════════════════╝  │
│ Podgląd oczami rodzica · Zapisz roboczy      │
│ Ogranicz odbiorców · Zmień kanał             │
├──────────────────────────────────────────────┤
│ ! offline → komunikat w kolejce, ruszy po sie│
└──────────────────────────────────────────────┘
```

*Co dominuje.* Dominuje selektor odbiorcy, pole treści z podpowiedzią szablonu i przycisk wysyłki w zasięgu kciuka.

*Co może umknąć.* Ostrzeżenie o odbiorcach bez zgody na dany kanał i o danych wrażliwych w komunikacie grupowym łatwo przeoczyć przed wysłaniem.

*Stan alternatywny do hi-fi.* W hi-fi pokaż potwierdzenie liczby odbiorców przed masową wysyłką, żeby uniknąć przypadkowej wiadomości do całej grupy.

---
