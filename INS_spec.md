# Specyfikacja ekranów grupy INS - panel instruktora, moduł zajęć

Grupa INS to panel instruktora, czyli trenera prowadzącego grupę przy basenie lub na macie. Konto instruktora jest podrzędne wobec organizatora i ograniczone rolą. Wszystkie ekrany działają w module `zajecia`, rola dominująca to instruktor, miejscami z odczytem danych, których właścicielem jest organizator. Persona pracuje w pośpiechu, na telefonie lub tablecie, często mokrymi dłońmi, a zasięg sieci bywa zerwany. Z tego wynikają dwie decyzje przekrojowe dla całej grupy: priorytet ma dzisiejsza grupa i obecność liczona w sekundach, a obecność oraz aktualizacja umiejętności muszą działać bez sieci.

Konwencje wspólne dla całej grupy, żeby nie powtarzać ich w każdym polu:

- Napisy widoczne dla użytkownika to klucze tłumaczeń ze znacznikami BCP 47, domyślnie pl-PL i en-GB. Daty, godziny i numery telefonu formatuje się według ustawień regionalnych placówki. Przełącznik języka stoi w nagłówku panelu, język zapasowy to en-GB przy braku tłumaczenia. Układ RTL włącza się automatycznie, jeśli aktywny język jest pisany od prawej do lewej.
- Tokeny marki pochodzą wyłącznie ze zmiennych CSS: `--brand-ink` i `--accent`, kolory semantyczne dla sukcesu, ostrzeżenia, błędu oraz informacji, para krojów (nagłówkowy wyróżniający i tekstowy czytelny, z dala od Inter i Arial), skala odstępów na jednej jednostce bazowej, jeden promień zaokrąglenia, jeden poziom cienia. Minimalny cel dotykowy i kontrast tekstu spełniają WCAG 2.1 na poziomie AA (W3C, 2018). Persona obsługuje ekran mokrym palcem, więc cel dotykowy projektuje się powyżej minimum WCAG, a nie na jego granicy.
- Lista kontrolna heurystyk odnosi się do dziesięciu heurystyk użyteczności (Nielsen, 1994).
- Instruktor nie widzi płatności, sald ani faktur. To twarda granica roli, nie ustawienie. Każdy ekran, który po stronie organizatora pokazuje status finansowy, w panelu INS ten status pomija lub zastępuje znacznikiem neutralnym.
- Urządzeniem podstawowym jest telefon, drugim tablet trzymany pionowo. Desktop traktuje się jako przypadek brzegowy pracy zza biurka, nie jako układ wiodący. To odwrotnie niż w panelu organizatora.
- Tryb offline dotyczy obecności i aktualizacji umiejętności. Dane zebrane bez sieci trafiają do kolejki lokalnej i synchronizują się po powrocie łącza, z widocznym znacznikiem stanu synchronizacji.

---

## Partia 1 (INS-01 do INS-06)

### INS-01 - logowanie

Grupa i rola: instruktor.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę wejść do swojego panelu szybko, najlepiej jednym dotknięciem, także wtedy, gdy stoję przy wodzie i mam mokre ręce.

Punkty wejścia: ikona aplikacji lub adres panelu, link z zaproszenia od organizatora przy pierwszym logowaniu, link z powiadomienia push o dzisiejszej grupie, wylogowanie po stronie poprzedniej.

Warunki wstępne: konto instruktora utworzone i przypisane do placówki przez organizatora w OCL-12. Instruktor nie zakłada konta sam.

Dane wyświetlane: logo placówki z tokenów marki, pole identyfikatora, opcja logowania bez hasła linkiem lub kodem, przełącznik języka, informacja o ostatnim udanym logowaniu po wejściu.

Komponenty interfejsu: formularz logowania z dużymi celami dotykowymi, przycisk logowania bez hasła, opcja zapamiętania urządzenia, przełącznik języka w nagłówku, odnośnik do pomocy placówki.

Akcja główna: zaloguj się. Na urządzeniu zapamiętanym dominującą akcją jest potwierdzenie biometryczne lub kodem.

Akcje drugorzędne: zmień język, poproś o nowy link logowania, otwórz pomoc.

Akcje niszczące: nie dotyczy.

Stany ekranu: ładowanie pokazuje wygaszony formularz; pusty nie dotyczy, formularz jest zawsze wypełnialny; wypełniony to gotowy formularz; błąd logowania tłumaczy, czy problem dotyczy identyfikatora, wygasłego linku, czy braku przypisania do placówki, i podaje następny krok; sukces przenosi do INS-02; brak uprawnień informuje, że konto nie ma przypisanej placówki i kieruje do kontaktu z organizatorem; offline pokazuje, że logowanie wymaga sieci, ale dane zebrane wcześniej w trybie offline czekają w kolejce na tym urządzeniu.

Walidacja i komunikaty: pusty identyfikator prosi o uzupełnienie. Wygasły link logowania proponuje wysłanie nowego i mówi, na jaki adres trafi. Zbyt wiele prób uruchamia chwilową blokadę z wyjaśnieniem, ile czasu pozostało. Konto bez przypisanej placówki nie wpuszcza do panelu i tłumaczy, że dostęp nadaje organizator.

Przypadki brzegowe: instruktor pracujący w dwóch placówkach jednego operatora wybiera placówkę po zalogowaniu; konto dezaktywowane na koniec sezonu pokazuje komunikat o wygaśnięciu dostępu; logowanie na cudzym urządzeniu pomija zapamiętanie i wymusza wylogowanie po sesji.

Przejścia i następne ekrany: udane logowanie prowadzi do INS-02. Pierwsze logowanie z zaproszenia prowadzi przez krótki ekran zgody na regulamin i politykę prywatności, a potem do INS-02.

Zdarzenia systemowe: wygaśnięcie sesji po czasie bezczynności, unieważnienie linku po użyciu, log udanych i nieudanych prób do audytu organizatora.

Wymagania prawne: dane logowania jako dane osobowe pod RODO, zgoda na regulamin przy pierwszym logowaniu z zapisaną wersją dokumentu, minimalizacja danych na ekranie wejścia. Tworzenie konta i nadanie dostępu pozostają po stronie organizatora, nie instruktora.

Kontekst urządzenia: telefon jako podstawowy, biometria tam, gdzie urządzenie ją wspiera. Tablet recepcji jako drugi przypadek.

Lokalizacja i języki: etykiety jako klucze tłumaczeń, lista języków dziedziczona z ustawień placówki w OCL-18, język zapasowy en-GB.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania przez logowanie bez hasła i zapamiętane urządzenie; pomoc w diagnozie błędu przez komunikat wskazujący przyczynę i krok naprawczy; widoczność stanu systemu przez informację o ostatnim logowaniu; minimalistyczny projekt przez jedną dominującą akcję na ekranie.

Metryki sukcesu: czas od otwarcia aplikacji do pulpitu, odsetek logowań bez hasła, liczba zgłoszeń pomocy dotyczących wejścia do panelu.

---

### INS-02 - dashboard z dzisiejszymi zajęciami

Grupa i rola: instruktor.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: po zalogowaniu chcę od razu widzieć, co prowadzę dziś i o której, i jednym dotknięciem zacząć sprawdzanie obecności, bez szukania w grafiku całej placówki.

Punkty wejścia: udane logowanie z INS-01, link z powiadomienia push o najbliższych zajęciach, powrót z ekranu obecności lub umiejętności.

Warunki wstępne: instruktor przypisany do przynajmniej jednej grupy z terminem w grafiku placówki.

Dane wyświetlane: lista dzisiejszych zajęć z godziną, salą lub torem, poziomem grupy i liczbą zapisanych dzieci, znacznik, czy obecność za dane zajęcia jest już rozpoczęta lub zamknięta, najbliższe zajęcia wyróżnione na górze, krótka informacja o zastępstwie, jeśli instruktor wchodzi za kogoś. Bez jakichkolwiek danych finansowych.

Komponenty interfejsu: oś dnia z kartami zajęć, duży przycisk startu obecności na najbliższej karcie, znacznik stanu synchronizacji offline, skrót do własnego grafiku tygodniowego, lista krótkich alertów operacyjnych, na przykład zmiana sali albo odwołanie.

Akcja główna: rozpocznij obecność dla najbliższych zajęć. Jedna dominująca akcja prowadzi prosto do INS-04.

Akcje drugorzędne: otwórz własny grafik, podejrzyj listę uczestników grupy, zgłoś dostępność, otwórz aktualizację umiejętności dla zakończonych zajęć.

Akcje niszczące: nie dotyczy.

Stany ekranu: ładowanie pokazuje szkielety kart; pusty wita instruktora bez zajęć dziś i pokazuje najbliższy dzień z zajęciami; wypełniony to lista dnia; błąd ładowania pokazuje komunikat z ponowieniem, a jeśli są dane lokalne, pokazuje je z banerem nieświeżości; sukces nie dotyczy osobno; brak uprawnień informuje o braku przypisanych grup; offline pokazuje ostatnio zsynchronizowany plan dnia z banerem, że obecność i tak działa lokalnie.

Walidacja i komunikaty: nieudane pobranie listy dnia mówi, że nie udało się odświeżyć planu, i proponuje pracę na ostatnich danych. Konflikt grafiku, gdy instruktor jest przypisany do dwóch grup w tej samej godzinie, pokazuje ostrzeżenie z obiema grupami i kieruje do organizatora, bo rozstrzygnięcie należy do niego w OCL-03.

Przypadki brzegowe: dzień bez zajęć; zajęcia odwołane przez organizatora widoczne jako przekreślone z powodem; zastępstwo dodane w ostatniej chwili pojawiające się po odświeżeniu; instruktor z grupami w dwóch placówkach widzi przełącznik placówki.

Przejścia i następne ekrany: karta zajęć prowadzi do obecności INS-04, skrót grafiku do INS-03, podgląd uczestnika do INS-06, aktualizacja po zajęciach do INS-05, zgłoszenie dostępności do INS-07.

Zdarzenia systemowe: odświeżanie planu dnia przy wejściu i co kilka minut, push o zmianie sali lub odwołaniu z OCL-03, sygnał o zakończeniu synchronizacji danych offline.

Wymagania prawne: pulpit pokazuje dane zagregowane i liczbę dzieci, dane wrażliwe pojawiają się dopiero po wejściu w obecność lub profil. Minimalizacja danych pod RODO na widoku startowym.

Kontekst urządzenia: telefon jako podstawowy, duży przycisk startu obecności dostępny kciukiem jednej ręki. Tablet pokazuje oś dnia szerzej.

Lokalizacja i języki: godziny i nazwy dni według regionu, nazwy grup jako treść organizatora, etykiety jako klucze tłumaczeń.

Lista kontrolna heurystyk: widoczność stanu systemu przez znacznik rozpoczętej obecności i stan synchronizacji; zgodność ze światem przez język grafiku trenera, nie język bazy; rozpoznawanie zamiast przypominania, bo następne zajęcia są na wierzchu; minimalistyczny projekt przez ograniczenie pulpitu do dziś.

Metryki sukcesu: czas od logowania do rozpoczęcia obecności, odsetek zajęć z obecnością rozpoczętą z poziomu pulpitu, liczba wejść w niewłaściwą grupę.

---

### INS-03 - własny grafik

Grupa i rola: instruktor. Odczytowe odbicie grafiku placówki, którego właścicielem jest organizator w OCL-03.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę zobaczyć swój tydzień, tylko swoje zajęcia i zastępstwa, żeby zaplanować dojazdy i wiedzieć, gdzie i kiedy pracuję.

Punkty wejścia: skrót grafiku z pulpitu INS-02, menu panelu, link z powiadomienia o zmianie terminu.

Warunki wstępne: instruktor przypisany do grup z terminami w grafiku placówki.

Dane wyświetlane: siatka tygodniowa wyłącznie z zajęciami instruktora, kolor bloku według poziomu grupy, etykieta z salą lub torem, oznaczenia zastępstw i odwołań, dni świąteczne według kalendarza regionu. Brak zajęć innych instruktorów, bo to nie jest panel organizatora.

Komponenty interfejsu: przełącznik widoku dzień i tydzień, siatka bloków bez przeciągania, panel szczegółów wybranego bloku z przyciskiem przejścia do obecności, filtr po placówce dla instruktora z dwóch placówek.

Akcja główna: otwórz wybrany blok zajęć. Akcja wiodąca prowadzi do obecności lub szczegółu zajęć.

Akcje drugorzędne: przełącz widok dnia i tygodnia, przejdź do następnego tygodnia, zgłoś niedostępność dla wskazanego terminu, dodaj termin do kalendarza urządzenia.

Akcje niszczące: nie dotyczy. Instruktor nie usuwa ani nie przesuwa zajęć, robi to organizator.

Stany ekranu: ładowanie z szarą siatką; pusty pokazuje tydzień bez zajęć i kieruje do zgłoszenia dostępności; wypełniony to siatka z blokami; błąd ładowania z ponowieniem lub pracą na danych lokalnych; sukces nie dotyczy osobno; brak uprawnień nie dotyczy, bo widok jest własny; offline pokazuje ostatnio zsynchronizowany tydzień z banerem o wstrzymanej aktualizacji.

Walidacja i komunikaty: nieudane odświeżenie tłumaczy, że plan może być nieaktualny, i proponuje ponowienie. Próba edycji bloku wyjaśnia, że zmiany terminu należą do organizatora, i proponuje zgłoszenie niedostępności jako właściwą ścieżkę.

Przypadki brzegowe: tydzień ze świętem przesuwającym terminy; zastępstwo nakładające się na własne zajęcia pokazane jako konflikt do zgłoszenia; zajęcia trwające przez północ; instruktor bez żadnych przypisań w danym tygodniu.

Przejścia i następne ekrany: blok prowadzi do obecności INS-04, zgłoszenie niedostępności do INS-07, zmiana terminu po stronie organizatora odbija się tu po synchronizacji z OCL-03.

Zdarzenia systemowe: synchronizacja z grafikiem placówki, push o odwołaniu lub zastępstwie, aktualizacja po zatwierdzeniu zgłoszonej niedostępności w OCL-12.

Wymagania prawne: grafik zawiera dane organizacyjne instruktora, bez danych dzieci poza liczbą, eksport terminu do kalendarza urządzenia tylko na działanie instruktora.

Kontekst urządzenia: telefon w widoku dnia, tablet w widoku tygodnia, desktop jako przypadek brzegowy.

Lokalizacja i języki: nazwy dni i miesięcy oraz format godziny według regionu, kalendarz świąt zależny od kraju placówki, etykiety jako klucze.

Lista kontrolna heurystyk: widoczność stanu systemu przez natychmiastowy obraz tygodnia; kontrola i swoboda przez jasny podział na to, co instruktor może, a co zgłasza; zgodność ze światem przez kalendarz i nazwy dni z życia; spójność z grafikiem organizatora przez te same oznaczenia odwołań i zastępstw.

Metryki sukcesu: odsetek instruktorów otwierających grafik przed pierwszymi zajęciami tygodnia, liczba zgłoszeń niedostępności złożonych z tego ekranu, liczba nieporozumień o terminie zgłoszonych do recepcji.

---

### INS-04 - lista obecności z odprawą, skanem QR i trybem offline

Grupa i rola: instruktor.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę odhaczyć obecność całej dzisiejszej grupy w kilka sekund, mokrym palcem, także bez zasięgu, i mieć pewność, że nic nie przepadnie.

Punkty wejścia: przycisk startu obecności z pulpitu INS-02, blok zajęć z grafiku INS-03, link z bloku grafiku organizatora w OCL-03, powrót z profilu uczestnika.

Warunki wstępne: zajęcia z listą zapisanych dzieci pobraną na urządzenie przed wejściem na halę, żeby tryb offline miał na czym pracować.

Dane wyświetlane: lista dzieci grupy z imieniem i czytelnym oznaczeniem, duże pola obecny i nieobecny, znacznik zgłoszonej wcześniej nieobecności od rodzica, znacznik gościa odrabiającego z innej grupy, licznik obecnych względem zapisanych, stan synchronizacji offline. Bez statusu płatności i bez danych finansowych, zgodnie z granicą roli.

Komponenty interfejsu: lista z dużymi celami dotykowymi na całą szerokość wiersza, przełącznik obecny i nieobecny jednym dotknięciem, skaner QR z karnetu lub paszportu dziecka, pole szybkiego wyszukania, baner trybu offline z liczbą zmian w kolejce, przycisk zamknięcia obecności.

Akcja główna: oznacz obecność dziecka. Akcją zamykającą sesję jest zamknij i zsynchronizuj.

Akcje drugorzędne: skanuj QR, dopisz gościa odrabiającego z listy uprawnionych, oznacz spóźnienie, dodaj krótką notatkę do zajęć.

Akcje niszczące: nie dotyczy w sensie kasowania danych. Cofnięcie błędnego odhaczenia jest zwykłym przełączeniem stanu, nie operacją nieodwracalną.

Stany ekranu: ładowanie listy z szkieletem wierszy; pusty oznacza grupę bez zapisanych dzieci i tłumaczy, że obecności nie ma na czym zrobić; wypełniony to lista do odhaczania; błąd synchronizacji nie blokuje pracy, dane zostają w kolejce i ekran pokazuje, ile pozycji czeka; sukces potwierdza zamknięcie i zsynchronizowanie obecności; brak uprawnień nie dotyczy, bo to własna grupa instruktora; offline jest stanem pierwszej klasy, lista działa w całości lokalnie, a baner pokazuje tryb i moment ostatniej synchronizacji.

Walidacja i komunikaty: zamknięcie obecności z dzieckiem bez oznaczenia pyta, czy pozostawić je jako nieobecne, zamiast cicho zakładać stan. Skan QR niepasujący do grupy tłumaczy, że dziecko nie należy do tych zajęć, i proponuje dopisanie jako gość odrabiający, jeśli reguły na to pozwalają. Próba synchronizacji bez sieci informuje, że dane są bezpieczne lokalnie i pójdą po powrocie łącza. Konflikt dwóch urządzeń odhaczających tę samą grupę pokazuje, które zmiany weszły, i prosi o przegląd rozbieżności.

Przypadki brzegowe: dziecko zapisane, ale z wcześniej zgłoszoną nieobecnością przez rodzica w PAR-05; gość odrabiający z innej grupy o tym samym poziomie; grupa licząca kilkadziesiąt dzieci wymagająca szybkiego wyszukania; przerwana sesja, gdy telefon się wyłączył, wznawiana z danych lokalnych; dwoje rodzeństwa o tym samym nazwisku wymagających dodatkowego rozróżnienia.

Przejścia i następne ekrany: zamknięta obecność zasila przegląd frekwencji organizatora w OCL-11, otwiera aktualizację umiejętności INS-05 dla tych samych zajęć, a wiersz dziecka prowadzi do profilu w trybie odczytu INS-06.

Zdarzenia systemowe: zapis lokalny przy każdej zmianie, synchronizacja w tle po wykryciu sieci, sygnał konfliktu przy równoległej edycji, aktualizacja licznika obecnych po stronie organizatora.

Wymagania prawne: obecność dziecka jako dana osobowa pod RODO, dane przechowywane lokalnie na czas offline w postaci ograniczonej do potrzeby odhaczenia, notatka do zajęć bez danych wrażliwych, dostęp tylko dla przypisanego instruktora i organizatora.

Kontekst urządzenia: telefon jako podstawowy, cele dotykowe powiększone na obsługę mokrym palcem, kontrast podniesiony na pracę w słońcu przy basenie. Tablet dla grup liczniejszych.

Lokalizacja i języki: imiona dzieci jako dane, etykiety obecny i nieobecny jako klucze tłumaczeń, format godziny spóźnienia według regionu.

Lista kontrolna heurystyk: widoczność stanu systemu przez licznik obecnych i znacznik synchronizacji; zapobieganie błędom przez pytanie o dzieci bez oznaczenia przed zamknięciem; kontrola i swoboda przez łatwe cofnięcie odhaczenia; zgodność ze światem przez prosty podział obecny i nieobecny; elastyczność przez skan QR obok ręcznego odhaczania; pomoc w diagnozie przez czytelny komunikat trybu offline.

Metryki sukcesu: mediana czasu odhaczenia całej grupy, odsetek sesji obecności ukończonych offline i poprawnie zsynchronizowanych, liczba korekt obecności po zamknięciu, udział zajęć z obecnością domkniętą tego samego dnia.

---

### INS-05 - aktualizacja umiejętności na drzewie skilli z filmami

Grupa i rola: instruktor. Korzysta z drzewa umiejętności zdefiniowanego przez organizatora w OCL-10.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: po zajęciach chcę szybko zaznaczyć, które dziecko opanowało którą umiejętność, czasem dołączyć krótki film jako dowód postępu, i zrobić to nawet bez zasięgu na hali.

Punkty wejścia: przycisk aktualizacji po zamknięciu obecności w INS-04, karta zajęć z pulpitu, profil uczestnika INS-06, menu panelu.

Warunki wstępne: zdefiniowane drzewo umiejętności dla poziomu grupy w OCL-10, lista dzieci grupy pobrana na urządzenie.

Dane wyświetlane: drzewo umiejętności dla poziomu grupy z gałęziami i kamieniami milowymi, status każdej umiejętności na dziecko, na przykład w toku albo zaliczone, miniatury załączonych filmów, data ostatniej zmiany statusu, znacznik synchronizacji offline.

Komponenty interfejsu: lista dzieci z szybkim przełączaniem, drzewo umiejętności z dużymi przełącznikami stanu, nagrywarka lub wybór krótkiego filmu, pole notatki do umiejętności, filtr po gałęzi drzewa, baner trybu offline z liczbą zmian w kolejce.

Akcja główna: zaznacz opanowanie umiejętności dla dziecka. Przy filmie akcją uzupełniającą jest dołączenie krótkiego nagrania.

Akcje drugorzędne: cofnij status, dodaj notatkę, przełącz dziecko, nagraj lub wybierz film, zastosuj ten sam postęp do kilku dzieci naraz.

Akcje niszczące: usuń załączony film. Z potwierdzeniem, bo nagranie dziecka to dana wymagająca ostrożności, oraz z informacją, że status umiejętności pozostaje.

Stany ekranu: ładowanie drzewa; pusty oznacza poziom bez zdefiniowanego drzewa i kieruje do organizatora, bo definicja należy do OCL-10; wypełniony to drzewo z dziećmi; błąd zapisu nie gubi zmiany, trzyma ją lokalnie i pokazuje stan kolejki; sukces potwierdza zapis; brak uprawnień nie dotyczy własnej grupy; offline działa w całości lokalnie, film czeka na lżejszą synchronizację po sieci.

Walidacja i komunikaty: oznaczenie umiejętności spoza poziomu grupy tłumaczy, że gałąź nie należy do tego poziomu, i proponuje właściwą. Film przekraczający limit długości lub rozmiaru prosi o krótsze nagranie i podaje granicę. Zaznaczenie zaliczenia bez wcześniejszego stanu w toku pyta, czy pominąć etap pośredni. Synchronizacja filmów na słabym łączu informuje, że nagrania pójdą w tle, a statusy są już zapisane.

Przypadki brzegowe: dziecko zmieniające poziom w trakcie semestru z częściowo opanowanym drzewem; zmiana definicji drzewa po wcześniejszych ocenach wymagająca mapowania starych statusów; film nakręcony offline na urządzeniu o małej pamięci; nadanie tej samej umiejętności całej podgrupie naraz; dziecko obecne w dwóch grupach o różnych poziomach.

Przejścia i następne ekrany: zapisany postęp zasila widok rodzica PAR-08 i paszport sportowy OCL-17, definicja drzewa pochodzi z OCL-10, kontekst dziecka łączy z profilem INS-06.

Zdarzenia systemowe: zapis lokalny przy każdej zmianie statusu, synchronizacja statusów natychmiast po sieci i filmów w tle, push do rodzica o nowym kamieniu milowym, aktualizacja paszportu po nowych danych.

Wymagania prawne: nagranie dziecka jako dana osobowa, a często szczególna, pod RODO, zgoda opiekuna na nagrywanie pobrana wcześniej i sprawdzana przed udostępnieniem filmu, przechowywanie ograniczone czasowo, dostęp tylko dla uprawnionych, brak udostępniania nagrań poza konto rodzinne bez zgody.

Kontekst urządzenia: telefon jako podstawowy, nagrywanie z kamery urządzenia, duże przełączniki stanu na obsługę mokrym palcem. Tablet do przeglądu drzewa szerzej.

Lokalizacja i języki: nazwy umiejętności jako treść organizatora z możliwością tłumaczenia, etykiety stanów jako klucze, data ostatniej zmiany według regionu.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania, bo umiejętności wybiera się z gotowego drzewa; zapobieganie błędom przez kontrolę zgodności umiejętności z poziomem; widoczność stanu systemu przez znacznik synchronizacji i datę zmiany; kontrola i swoboda przez cofnięcie statusu; spójność z definicją organizatora przez to samo drzewo dla wszystkich ról.

Metryki sukcesu: odsetek zajęć z aktualizacją umiejętności tego samego dnia, czas aktualizacji na dziecko, udział aktualizacji wykonanych offline i poprawnie zsynchronizowanych, odsetek kamieni milowych z dołączonym filmem.

---

### INS-06 - profil uczestnika w trybie odczytu

Grupa i rola: instruktor. Ograniczony odczyt danych, których właścicielem jest organizator w OCL-08 i OCL-09.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę szybko sprawdzić to, co potrzebne do bezpiecznego prowadzenia zajęć z konkretnym dzieckiem - poziom, postępy, istotne uwagi zdrowotne dopuszczone do mojego wglądu - bez dostępu do spraw rodziny, których prowadzić nie muszę.

Punkty wejścia: wiersz dziecka z listy obecności INS-04, lista dzieci z aktualizacji umiejętności INS-05, wyszukiwarka panelu instruktora, link z komunikatu w INS-10.

Warunki wstępne: dziecko zapisane do grupy prowadzonej przez tego instruktora. Instruktor nie widzi dzieci spoza swoich grup.

Dane wyświetlane: imię i oznaczenie dziecka, przypisana grupa i poziom, postęp na drzewie umiejętności w odczycie, frekwencja skrócona, uwagi zdrowotne lub bezpieczeństwa dopuszczone przez organizatora do wglądu instruktora, na przykład alergia istotna przy zajęciach, kontakt do opiekuna na czas zajęć. Bez statusu płatności, bez danych finansowych, bez pełnej dokumentacji rodziny.

Komponenty interfejsu: nagłówek z imieniem i poziomem, sekcja postępu w odczycie z linkiem do aktualizacji, sekcja frekwencji, sekcja uwag bezpieczeństwa wyraźnie wyróżniona kolorem informacyjnym, przycisk kontaktu z opiekunem w nagłych sytuacjach.

Akcja główna: przejdź do aktualizacji umiejętności tego dziecka. To jedyna akcja zapisująca dostępna z profilu, reszta jest odczytem.

Akcje drugorzędne: zadzwoń lub napisz do opiekuna w sprawie zajęć, otwórz frekwencję, wróć do listy grupy.

Akcje niszczące: nie dotyczy. Instruktor nie edytuje ani nie usuwa danych profilu.

Stany ekranu: ładowanie z szkieletem sekcji; pusty nie dotyczy, profil istnieje, jeśli dziecko jest w grupie; wypełniony to pełny odczyt; błąd ładowania z ponowieniem lub danymi lokalnymi; sukces nie dotyczy osobno; brak uprawnień ukrywa sekcje poza zakresem instruktora i wyjaśnia, że pełna karta jest po stronie organizatora; offline pokazuje ostatnio pobrane dane profilu z banerem nieświeżości.

Walidacja i komunikaty: próba wejścia w dziecko spoza swoich grup tłumaczy, że instruktor widzi tylko uczestników prowadzonych grup. Brak uwag zdrowotnych pokazuje neutralny komunikat, że nic istotnego nie odnotowano, zamiast pustego pola budzącego niepewność. Kontakt z opiekunem otwiera kanał ustawiony przez placówkę, nie odsłania prywatnego numeru, jeśli reguły placówki tego nie przewidują.

Przypadki brzegowe: dziecko w dwóch grupach tego instruktora; rodzeństwo w jednym koncie rodzinnym widoczne osobno; uwaga zdrowotna oznaczona jako poufna, której organizator nie dopuścił do wglądu instruktora i która pozostaje ukryta; dziecko świeżo przeniesione między grupami z niespójnym jeszcze poziomem.

Przejścia i następne ekrany: link postępu prowadzi do INS-05, frekwencja do widoku obecności, kontakt do kanału komunikacji zgodnego z INS-10, pełna karta pozostaje w OCL-09 poza zasięgiem instruktora.

Zdarzenia systemowe: odświeżenie postępu po nowej aktualizacji umiejętności, sygnał o nowej uwadze bezpieczeństwa dodanej przez organizatora, aktualizacja frekwencji po zamknięciu obecności.

Wymagania prawne: dane dziecka jako dane osobowe pod RODO, uwagi zdrowotne jako kategoria szczególna z dostępem zawężonym do potrzeby prowadzenia zajęć i do zakresu dopuszczonego przez organizatora, zasada minimalizacji przez ukrycie wszystkiego, co nie jest potrzebne instruktorowi, log wglądu do audytu.

Kontekst urządzenia: telefon jako podstawowy, sekcja uwag bezpieczeństwa widoczna bez przewijania. Tablet do spokojnego przeglądu.

Lokalizacja i języki: imię jako dana, etykiety sekcji jako klucze tłumaczeń, format daty frekwencji i numeru kontaktowego według regionu.

Lista kontrolna heurystyk: minimalizacja danych jako spójna realizacja minimalistycznego projektu i wymogu prawnego; zgodność ze światem przez język trenera, nie kartotekę; widoczność stanu systemu przez świeżość danych i wyróżnioną sekcję bezpieczeństwa; zapobieganie błędom przez jasne ograniczenie do własnych grup.

Metryki sukcesu: odsetek instruktorów sprawdzających uwagi bezpieczeństwa przed zajęciami z nowym dzieckiem, czas dotarcia do kontaktu z opiekunem w sytuacji nagłej, liczba prób wejścia w dziecko spoza grup zatrzymanych przez ograniczenie roli.

---

## Partia 2 (INS-07 do INS-10)

### INS-07 - zgłaszanie dostępności

Grupa i rola: instruktor. Zgłoszenie zasila zarządzanie instruktorami i grafik po stronie organizatora w OCL-12 i OCL-03.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę z wyprzedzeniem powiedzieć, kiedy mogę pracować, a kiedy nie, i szybko zgłosić niedostępność na konkretny termin, żeby organizator ułożył grafik bez dopytywania.

Punkty wejścia: skrót z pulpitu INS-02, blok grafiku INS-03, menu panelu, link z prośby organizatora o uzupełnienie dostępności.

Warunki wstępne: aktywne konto instruktora przypisane do placówki.

Dane wyświetlane: siatka tygodniowych okien dostępności, zgłoszone niedostępności z datą i statusem zatwierdzenia przez organizatora, zbliskie terminy własnych zajęć dla kontekstu, okno wyprzedzenia wymagane przez placówkę na zgłoszenie zmiany.

Komponenty interfejsu: edytor okien dostępności w siatce tygodnia, formularz zgłoszenia niedostępności na termin z polem powodu opcjonalnego, lista zgłoszeń ze statusem, przełącznik dostępności cyklicznej i jednorazowej.

Akcja główna: zapisz dostępność. Przy konkretnym terminie dominującą akcją jest zgłoś niedostępność.

Akcje drugorzędne: edytuj okno cykliczne, wycofaj zgłoszenie oczekujące, dodaj powód, ustaw dostępność na kolejny okres.

Akcje niszczące: usuń okno dostępności. Z informacją, że nie wpływa to na już przypisane zajęcia, które rozstrzyga organizator.

Stany ekranu: ładowanie siatki; pusty zachęca do ustawienia pierwszej dostępności i tłumaczy, po co służy; wypełniony to siatka z oknami i lista zgłoszeń; błąd zapisu przy formularzu z możliwością ponowienia; sukces potwierdza wysłanie zgłoszenia z informacją, że czeka na decyzję organizatora; brak uprawnień nie dotyczy; offline pozwala przygotować zgłoszenie, które wyśle się po sieci, z widocznym stanem oczekiwania na wysyłkę.

Walidacja i komunikaty: zgłoszenie niedostępności na termin z już przypisanymi zajęciami ostrzega, że wymaga zastępstwa, i tłumaczy, że decyzję podejmuje organizator. Zgłoszenie poniżej okna wyprzedzenia placówki informuje o wymaganym terminie i pozwala mimo to wysłać prośbę pilną z oznaczeniem. Pusty zakres dostępności pyta, czy instruktor jest niedostępny w całym tygodniu.

Przypadki brzegowe: instruktor pracujący w dwóch placówkach ustawiający rozłączne okna; choroba wymagająca masowego zgłoszenia kilku terminów naraz; cofnięcie zgłoszenia już zatwierdzonego przez organizatora wymagające ponownej prośby; dostępność sezonowa zmieniająca się między semestrami.

Przejścia i następne ekrany: zgłoszenie trafia do zarządzania instruktorami OCL-12, wpływa na grafik OCL-03 i odbija się w INS-03, zatwierdzone zastępstwo pojawia się na pulpicie INS-02.

Zdarzenia systemowe: powiadomienie organizatora o nowym zgłoszeniu, push do instruktora o decyzji, aktualizacja grafiku po zatwierdzeniu, przypomnienie o uzupełnieniu dostępności przed nowym okresem.

Wymagania prawne: powód niedostępności jako dana opcjonalna, bez wymogu podawania danych zdrowotnych, zgłoszenie traktowane jako dana pracownicza lub współpracownicza zgodnie z relacją placówki, dostęp dla organizatora i instruktora.

Kontekst urządzenia: telefon jako podstawowy, zgłoszenie jednego terminu w kilka dotknięć. Tablet do ustawienia okien cyklicznych.

Lokalizacja i języki: dni i godziny według regionu, etykiety jako klucze tłumaczeń, status zgłoszenia tłumaczony.

Lista kontrolna heurystyk: widoczność stanu systemu przez status zatwierdzenia zgłoszeń; zapobieganie błędom przez ostrzeżenie o terminie z przypisanymi zajęciami; kontrola i swoboda przez wycofanie zgłoszenia oczekującego; zgodność ze światem przez podział na dostępność cykliczną i jednorazową znany z grafików pracy.

Metryki sukcesu: wyprzedzenie zgłoszeń niedostępności względem terminu zajęć, odsetek terminów obsadzonych bez ręcznego dopytywania, liczba zgłoszeń pilnych poniżej okna wyprzedzenia.

---

### INS-08 - ewidencja godzin

Grupa i rola: instruktor. Zestawienie zasila zarządzanie instruktorami i raporty organizatora w OCL-12 i OCL-16.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę widzieć, ile godzin przepracowałem w danym okresie, na podstawie przeprowadzonych zajęć, i zgłosić ewentualną korektę, żeby rozliczenie z placówką było zgodne z rzeczywistością.

Punkty wejścia: menu panelu, skrót z pulpitu po zamknięciu okresu, link z prośby organizatora o potwierdzenie godzin.

Warunki wstępne: instruktor prowadził zajęcia z zamkniętą obecnością w wybranym okresie.

Dane wyświetlane: lista przeprowadzonych zajęć w okresie z datą, godziną, grupą i czasem trwania, suma godzin, podział na zajęcia własne, zastępstwa i lekcje indywidualne, status okresu na przykład otwarty albo potwierdzony. Bez stawek i kwot, bo wynagrodzenie należy do warstwy finansowej zamkniętej dla instruktora.

Komponenty interfejsu: selektor okresu, tabela zajęć z czasem trwania, suma godzin na górze, formularz korekty pojedynczego wpisu, przycisk potwierdzenia okresu, znacznik różnicy między planem a wykonaniem.

Akcja główna: potwierdź godziny okresu. Przy pojedynczym wpisie dominującą akcją jest zgłoszenie korekty.

Akcje drugorzędne: zmień okres, zgłoś korektę wpisu z powodem, eksportuj własne zestawienie godzin do pliku, rozwiń podział na typy zajęć.

Akcje niszczące: nie dotyczy. Instruktor nie usuwa wpisów, zgłasza korektę do akceptacji organizatora.

Stany ekranu: ładowanie tabeli; pusty oznacza okres bez przeprowadzonych zajęć; wypełniony to zestawienie z sumą; błąd ładowania z ponowieniem; sukces potwierdza zatwierdzenie okresu lub wysłanie korekty; brak uprawnień nie dotyczy własnych godzin; offline pokazuje ostatnio pobrane zestawienie w odczycie z banerem, potwierdzenie wymaga sieci.

Walidacja i komunikaty: korekta przekraczająca rozsądny zakres względem planu prosi o powód i ostrzega, że organizator ją zweryfikuje. Potwierdzenie okresu z niezgodnością planu i wykonania pokazuje różnicę przed zatwierdzeniem. Zajęcia bez zamkniętej obecności tłumaczą, że nie wliczają się do sumy, dopóki obecność nie zostanie domknięta w INS-04.

Przypadki brzegowe: zajęcia odwołane w trakcie z częściowym czasem; zastępstwo nachodzące na własne zajęcia; lekcja indywidualna krótsza niż grupowa; okres na przełomie miesiąca; instruktor z godzinami w dwóch placówkach widzący osobne zestawienia.

Przejścia i następne ekrany: potwierdzone godziny trafiają do OCL-12, zasilają raporty OCL-16, korekta czeka na akceptację organizatora, źródłem danych są zamknięte obecności z INS-04 i lekcje z INS-09.

Zdarzenia systemowe: domknięcie wpisu godzin po zamknięciu obecności, powiadomienie o otwarciu nowego okresu do potwierdzenia, sygnał o akceptacji lub odrzuceniu korekty.

Wymagania prawne: ewidencja czasu jako dana istotna dla rozliczenia, przechowywanie zgodne z wymogami właściwymi dla relacji placówki z instruktorem, eksport własnych danych dostępny dla instruktora, brak ekspozycji danych finansowych w panelu instruktora.

Kontekst urządzenia: telefon do szybkiego sprawdzenia sumy, tablet i desktop do przeglądu pełnego okresu i potwierdzenia.

Lokalizacja i języki: daty, godziny i czas trwania według regionu, etykiety jako klucze tłumaczeń, status okresu tłumaczony.

Lista kontrolna heurystyk: widoczność stanu systemu przez status okresu i sumę godzin; zgodność ze światem przez język godzin i zajęć, nie rekordów bazy; zapobieganie błędom przez pokazanie różnicy planu i wykonania przed potwierdzeniem; kontrola i swoboda przez korektę zamiast cichej akceptacji.

Metryki sukcesu: odsetek okresów potwierdzonych w terminie, liczba korekt i ich akceptacji, zgodność godzin potwierdzonych z planem grafiku.

---

### INS-09 - kalendarz lekcji jeden na jeden

Grupa i rola: instruktor. Działa w ramach konfiguracji rezerwacji lekcji indywidualnych ustawionej przez organizatora w OCL-13, z zapisami od rodzica w PAR-12.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę zarządzać swoimi lekcjami indywidualnymi - widzieć rezerwacje, otwierać i blokować terminy, prowadzić obecność lekcji jeden na jeden - bez mieszania ich z grafikiem grupowym.

Punkty wejścia: skrót lekcji indywidualnych z pulpitu INS-02, menu panelu, link z powiadomienia o nowej rezerwacji od rodzica.

Warunki wstępne: organizator włączył lekcje indywidualne dla instruktora w OCL-13, zdefiniowane są tory lub sale dostępne na lekcje.

Dane wyświetlane: kalendarz własnych lekcji indywidualnych z godziną, dzieckiem i miejscem, wolne okna otwarte na rezerwację, rezerwacje oczekujące i potwierdzone, krótka informacja o celu lekcji, jeśli rodzic ją podał. Bez ceny i płatności, bo to warstwa finansowa po stronie organizatora i rodzica.

Komponenty interfejsu: kalendarz dnia i tygodnia dla lekcji indywidualnych, edytor wolnych okien, karta rezerwacji ze szczegółem dziecka w odczycie, przycisk obecności lekcji, filtr po miejscu.

Akcja główna: otwórz lub zablokuj okno na lekcje. Na karcie rezerwacji dominującą akcją jest oznaczenie obecności po przeprowadzonej lekcji.

Akcje drugorzędne: zaproponuj inny termin rezerwacji, dodaj notatkę do lekcji, przejdź do profilu dziecka, ustaw okno cykliczne na lekcje.

Akcje niszczące: odwołaj zaplanowaną lekcję. Z potwierdzeniem, wyborem powodu i informacją, że rodzic dostanie powiadomienie, a zasady zwrotu rozstrzyga warstwa organizatora poza panelem instruktora.

Stany ekranu: ładowanie kalendarza; pusty z propozycją otwarcia pierwszych wolnych okien; wypełniony to kalendarz z rezerwacjami; błąd zapisu okna lub obecności z ponowieniem albo kolejką offline; sukces potwierdza zmianę; brak uprawnień informuje, że organizator nie włączył lekcji indywidualnych; offline pozwala prowadzić obecność lekcji lokalnie, otwieranie okien wymaga sieci ze względu na rezerwacje rodziców.

Walidacja i komunikaty: otwarcie okna kolidującego z zajęciami grupowymi pokazuje konflikt i proponuje wolny czas. Odwołanie lekcji z bliskim terminem ostrzega o krótkim wyprzedzeniu dla rodzica. Obecność lekcji bez oznaczenia stawienia się pyta, czy dziecko było obecne, zanim zamknie wpis. Próba zablokowania okna z już istniejącą rezerwacją tłumaczy, że trzeba najpierw obsłużyć rezerwację.

Przypadki brzegowe: rodzic rezerwujący w ostatniej chwili tuż przed terminem; lekcja indywidualna nachodząca na okno grupowe po zmianie grafiku; dziecko spoza grup instruktora rezerwujące lekcję indywidualną, widoczne w zawężonym odczycie; tor współdzielony przez zajęcia grupowe i lekcje; brak stawienia się dziecka na opłaconą lekcję.

Przejścia i następne ekrany: wolne okna i rezerwacje wynikają z konfiguracji OCL-13 i zapisów PAR-12, obecność lekcji zasila ewidencję godzin INS-08, karta dziecka prowadzi do odczytu INS-06, odwołanie łączy z komunikacją w INS-10.

Zdarzenia systemowe: push o nowej rezerwacji od rodzica, synchronizacja kalendarza z grafikiem grupowym dla wykrycia kolizji, sygnał o odwołaniu przez rodzica, domknięcie godziny lekcji po oznaczeniu obecności.

Wymagania prawne: dane dziecka na karcie rezerwacji w zakresie potrzebnym do lekcji pod RODO, notatka bez danych wrażliwych, powiadomienie o odwołaniu jako obowiązek informacyjny wobec opiekuna, warstwa zwrotów i płatności poza zasięgiem instruktora.

Kontekst urządzenia: telefon do obsługi pojedynczej rezerwacji i obecności lekcji, tablet do układania okien tygodniowych.

Lokalizacja i języki: godziny i daty według regionu, etykiety jako klucze tłumaczeń, cel lekcji jako treść rodzica z fallbackiem języka.

Lista kontrolna heurystyk: zapobieganie błędom przez wykrycie kolizji z grafikiem grupowym; widoczność stanu systemu przez jasny status rezerwacji; kontrola i swoboda przez propozycję innego terminu zamiast twardej odmowy; spójność z lekcjami po stronie organizatora przez te same statusy rezerwacji.

Metryki sukcesu: odsetek wolnych okien zamienionych w rezerwacje, liczba kolizji z grafikiem grupowym wykrytych przed potwierdzeniem, odsetek lekcji z domkniętą obecnością, wyprzedzenie odwołań lekcji.

---

### INS-10 - komunikaty do grupy lub rodziców

Grupa i rola: instruktor. Działa w ramach komunikacji placówki, której szablony i zasady ustawia organizator w OCL-15.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę szybko wysłać krótki komunikat do rodziców mojej grupy albo do jednej rodziny - przypomnienie o stroju, informację o postępie, prośbę o sprzęt - kanałem zgodnym z zasadami placówki i bez wglądu w dane, których nie potrzebuję.

Punkty wejścia: skrót komunikatu z pulpitu INS-02, karta zajęć po obecności, profil uczestnika INS-06, karta odwołanej lekcji z INS-09.

Warunki wstępne: instruktor prowadzi grupę z rodzicami mającymi zgodę na wybrany kanał kontaktu, komunikacja placówki włączona w OCL-15.

Dane wyświetlane: wybór odbiorcy jako cała grupa albo pojedyncza rodzina, dostępne kanały zgodne ze zgodami rodziców, szablony przygotowane przez organizatora, historia własnych komunikatów instruktora, znacznik dostarczenia. Lista odbiorców pokazuje dzieci grupy bez danych spoza zakresu instruktora.

Komponenty interfejsu: selektor odbiorcy, wybór kanału, pole treści z podpowiedzią szablonu, podgląd komunikatu, lista wysłanych z ich statusem, ostrzeżenie o odbiorcach bez zgody na kanał.

Akcja główna: wyślij komunikat. Przy gotowym szablonie dominującą akcją jest wybór szablonu i wysyłka.

Akcje drugorzędne: zapisz wersję roboczą, zmień kanał, ogranicz odbiorców do podgrupy, podejrzyj komunikat oczami rodzica.

Akcje niszczące: nie dotyczy w sensie kasowania. Wycofanie wersji roboczej to zwykłe odrzucenie nicwysłanego tekstu.

Stany ekranu: ładowanie; pusty z propozycją pierwszego komunikatu z szablonu; wypełniony to gotowy formularz z historią; błąd wysyłki tłumaczy, do których odbiorców komunikat nie dotarł i dlaczego, i proponuje ponowienie dla nich; sukces potwierdza wysyłkę z liczbą odbiorców; brak uprawnień ogranicza odbiorców do własnych grup; offline pozwala przygotować i zakolejkować komunikat, wysyłka rusza po sieci.

Walidacja i komunikaty: pusta treść blokuje wysyłkę. Odbiorcy bez zgody na wybrany kanał są pomijani z jasną informacją, ilu i dlaczego, oraz propozycją innego kanału. Komunikat do całej grupy prosi o potwierdzenie liczby odbiorców przed wysłaniem, żeby uniknąć przypadkowej masowej wiadomości. Treść z danymi wrażliwymi dziecka ostrzega, że komunikat grupowy widzą wszyscy rodzice, i proponuje wysłanie do jednej rodziny.

Przypadki brzegowe: rodzina bez zgody na żaden kanał, do której komunikat nie dotrze; rodzeństwo w dwóch grupach tego instruktora otrzymujące podwójną wiadomość, z opcją scalenia; komunikat pilny o odwołaniu lekcji powiązany z INS-09; rodzic z preferencją języka innego niż roboczy placówki.

Przejścia i następne ekrany: komunikat korzysta z szablonów i zasad OCL-15, odbiorcy wynikają z grup prowadzonych przez instruktora, kontekst dziecka łączy z INS-06, komunikat o odwołaniu lekcji startuje z INS-09.

Zdarzenia systemowe: wysyłka kanałem placówki, status dostarczenia od bramki komunikacyjnej, push lub e-mail do rodzica zależnie od zgody, log wysłanych komunikatów do audytu organizatora.

Wymagania prawne: kontakt z opiekunem tylko kanałem objętym zgodą pod RODO, treść bez danych wrażliwych w komunikacji grupowej, dane kontaktowe rodziców niewidoczne dla instruktora poza tym, co potrzebne do wysyłki, log komunikatów dla rozliczalności.

Kontekst urządzenia: telefon jako podstawowy, wysłanie krótkiego komunikatu z szablonu w kilka dotknięć. Tablet do dłuższej treści.

Lokalizacja i języki: treść jako tekst instruktora z możliwością tłumaczenia, etykiety jako klucze, komunikat dostarczany w języku preferowanym przez rodzica z fallbackiem, format daty i godziny w treści według regionu.

Lista kontrolna heurystyk: zapobieganie błędom przez potwierdzenie liczby odbiorców i ostrzeżenie o danych wrażliwych w grupie; zgodność ze światem przez język rodzica i gotowe szablony; widoczność stanu systemu przez status dostarczenia; pomoc i dokumentacja przez podgląd oczami rodzica; spójność z komunikacją placówki przez wspólne szablony z OCL-15.

Metryki sukcesu: odsetek komunikatów dostarczonych do zamierzonych odbiorców, czas przygotowania komunikatu z szablonu, liczba komunikatów wymagających ponowienia, udział kontaktu jeden na jeden względem grupowego.

---

## Literatura

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

W3C. (2018). *Web Content Accessibility Guidelines (WCAG) 2.1*. World Wide Web Consortium. https://www.w3.org/TR/WCAG21/
