# Specyfikacja ekranów grupy PAR - panel rodzica, konto rodzinne

Grupa PAR to panel rodzica, czyli osoby zarządzającej całą rodziną z jednego konta. Persona ma kilkoro dzieci, działa najczęściej z telefonu, wieczorem po pracy albo w drodze, między jednym dowozem a drugim. Decyzję podejmuje w pośpiechu, jedną ręką, z dzieckiem ciągnącym za rękaw. Z tego profilu wynika kilka decyzji przekrojowych dla całej grupy. Przełączanie między profilami dzieci musi być natychmiastowe i widoczne, bo rodzic wykonuje tę czynność dziesiątki razy w tygodniu. Najczęstsze zadania - zgłoszenie nieobecności, zapis na odrabianie, sprawdzenie płatności - mają się domykać w jednym, najwyżej dwóch dotknięciach. Konto jest jedno na całą rodzinę i obejmuje oba moduły, zajęcia i zawody, więc to samo dziecko zapisuje się na trening i na bieg bez zakładania drugiego konta.

Konwencje wspólne dla całej grupy, żeby nie powtarzać ich w każdym polu:

- Napisy widoczne dla użytkownika to klucze tłumaczeń ze znacznikami BCP 47, domyślnie pl-PL i en-GB. Daty, kwoty i numery telefonu formatuje się według ustawień regionalnych konta. Przełącznik języka stoi w nagłówku panelu, język zapasowy to en-GB przy braku tłumaczenia. Układ RTL włącza się automatycznie, jeśli aktywny język jest pisany od prawej do lewej.
- Tokeny marki pochodzą wyłącznie ze zmiennych CSS: `--brand-ink` i `--accent`, kolory semantyczne dla sukcesu, ostrzeżenia, błędu oraz informacji, para krojów (nagłówkowy wyróżniający i tekstowy czytelny, z dala od Inter i Arial), skala odstępów na jednej jednostce bazowej, jeden promień zaokrąglenia, jeden poziom cienia. Minimalny cel dotykowy i kontrast tekstu spełniają WCAG 2.1 na poziomie AA (W3C, 2018). Panel rodzica jest brandowany barwami organizatora, którego dotyczy dany kontekst, a przy kilku organizatorach w rodzinie marka zmienia się wraz z wybranym wydarzeniem lub grupą.
- Lista kontrolna heurystyk odnosi się do dziesięciu heurystyk użyteczności (Nielsen, 1994).
- Konto jest rodzinne i wspólne dla obu modułów. Jeden login, wiele profili dzieci, jeden portfel, jeden paszport sportowy dziecka. Portfel gromadzi nadpłaty i zwroty jako kredyt u tego samego organizatora i nie wypłaca się ich na zewnątrz bez osobnej procedury organizatora. Zgodę opiekuna dla małoletniego realizuje się z tego konta, a ekran zgody opisuje grupa KID.
- Model płatności jest oderwany od liczby dzieci. Rodzic z trójką dzieci nie płaci potrójnej opłaty platformowej, bo koszt operatora nie rośnie z liczbą użytkowników. Cena, którą widzi rodzic, to cena oferty organizatora, nie opłata od główki.
- Urządzeniem podstawowym jest telefon, drugim tablet. Desktop traktuje się jako wygodniejszy przypadek do spokojnego przeglądu paszportu, dokumentów albo dłuższego formularza zapisu, a nie jako układ wiodący.
- Część zadań rodzic wykonuje na miejscu, przy basenie albo na starcie, gdzie zasięg bywa zerwany. Zgłoszenie nieobecności i pokazanie kodu wejścia działają wtedy z ostatnio wczytanych danych, a zmiany czekają w kolejce lokalnej do powrotu sieci.

---

## Partia 1 (PAR-01 do PAR-06)

### PAR-01 - rejestracja i logowanie z kontem rodzinnym

Grupa i rola: rodzic. Konto rodzinne jest nadrzędne wobec profili dzieci i wspólne dla modułów zajęć i zawodów.

Moduł: wspolny.

Cel ekranu jako zadanie użytkownika: chcę założyć jedno konto dla całej rodziny albo wejść na nie szybko, najlepiej bez wpisywania hasła, żeby od razu dostać się do spraw dzieci, niezależnie od tego, u ilu organizatorów jesteśmy zapisani.

Punkty wejścia: brandowana strona wydarzenia lub grupy z GST-02 i GST-03, ekran rozpoczęcia zapisu z GST-05, link z zaproszenia organizatora, link z powiadomienia push lub e-mail, ikona aplikacji przy ponownym wejściu, most wolontariusz-rodzic przy zgłoszeniu z PAR-14.

Warunki wstępne: dla rejestracji żadne, konto zakłada sam rodzic. Dla logowania istniejące konto rodzinne. Zakładanie konta dziecka nie istnieje osobno, dziecko jest profilem w koncie rodzica.

Dane wyświetlane: logo i marka organizatora kontekstu z tokenów, pole adresu e-mail lub telefonu, opcja logowania bez hasła linkiem lub kodem, logowanie przez dostawcę tożsamości, przełącznik języka, krótka nota o tym, że jedno konto obsłuży całą rodzinę i oba moduły, informacja o ostatnim udanym logowaniu po wejściu.

Komponenty interfejsu: formularz z dużymi celami dotykowymi, przycisk logowania bez hasła, przyciski logowania przez dostawcę tożsamości, opcja zapamiętania urządzenia z biometrią, przełącznik języka w nagłówku, odnośnik do pomocy i do regulaminu.

Akcja główna: zaloguj się lub załóż konto rodzinne. Na urządzeniu zapamiętanym dominującą akcją jest potwierdzenie biometryczne.

Akcje drugorzędne: zmień język, poproś o nowy link logowania, odzyskaj dostęp, otwórz pomoc.

Akcje niszczące: nie dotyczy. Usunięcie konta odbywa się z ustawień PAR-16, nie z ekranu wejścia.

Stany ekranu: ładowanie pokazuje wygaszony formularz; pusty nie dotyczy, formularz jest zawsze wypełnialny; wypełniony to gotowy formularz; błąd logowania tłumaczy, czy problem dotyczy adresu, wygasłego linku, czy zbyt wielu prób, i podaje następny krok; sukces przenosi do PAR-02 albo wraca do przerwanego zapisu, jeśli rodzic logował się w jego trakcie; brak uprawnień nie dotyczy nowego konta; offline pokazuje, że pierwsze wejście wymaga sieci.

Walidacja i komunikaty: pusty adres prosi o uzupełnienie. Adres w złym formacie tłumaczy, czego brakuje. Wygasły link logowania proponuje wysłanie nowego i mówi, na jaki adres trafi. Zbyt wiele prób uruchamia chwilową blokadę z informacją, ile czasu pozostało. Próba rejestracji na adres już istniejący proponuje logowanie albo odzyskanie dostępu zamiast tworzenia duplikatu rodziny.

Przypadki brzegowe: dwoje rodziców chcących wspólnego dostępu do jednej rodziny, obsłużone przez zaproszenie drugiego opiekuna z PAR-16, nie przez drugie konto na te same dzieci; rodzic zapisujący dziecko u kilku organizatorów na jednym koncie; rodzic, który zaczął zapis jako gość i teraz zakłada konto, z przeniesieniem koszyka; konto rodzinne wchodzące w rolę wolontariusza mostem z PAR-14.

Przejścia i następne ekrany: udane wejście prowadzi do PAR-02 albo do przerwanego zapisu dziecka w PAR-04 lub zawodnika w PAR-11. Rejestracja z poziomu zapisu wraca do tego zapisu. Pierwsze logowanie prowadzi przez krótką akceptację regulaminu i polityki prywatności z zapisaną wersją dokumentu.

Zdarzenia systemowe: wygaśnięcie sesji po czasie bezczynności, unieważnienie linku po użyciu, log udanych i nieudanych prób, sygnał o powiązaniu konta z rolą wolontariusza przy moście.

Wymagania prawne: dane logowania jako dane osobowe pod RODO, zgoda na regulamin i politykę prywatności z zapisaną wersją przy zakładaniu konta, minimalizacja danych na ekranie wejścia, jasna podstawa przetwarzania danych dzieci jako danych osób, za które odpowiada opiekun. Rodzic zakłada konto sam, w odróżnieniu od instruktora i wolontariusza, których konta tworzy organizator.

Kontekst urządzenia: telefon jako podstawowy, biometria tam, gdzie urządzenie ją wspiera. Tablet i desktop jako wygodniejsze do pierwszej, spokojnej konfiguracji rodziny.

Lokalizacja i języki: etykiety jako klucze tłumaczeń, lista języków zależna od organizatora kontekstu, język zapasowy en-GB.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania przez logowanie bez hasła i zapamiętane urządzenie; pomoc w diagnozie błędu przez komunikat wskazujący przyczynę i krok naprawczy; widoczność stanu systemu przez informację o ostatnim logowaniu; minimalistyczny projekt przez jedną dominującą akcję; zapobieganie błędom przez wykrycie istniejącego konta zamiast cichego tworzenia duplikatu.

Metryki sukcesu: odsetek logowań bez hasła, czas od otwarcia do pulpitu, odsetek zapisów dokończonych po założeniu konta w ich trakcie, liczba duplikatów rodzin wykrytych przy rejestracji.

---

### PAR-02 - dashboard rodzica

Grupa i rola: rodzic.

Moduł: wspolny. Pulpit zbiera sprawy z obu modułów w jednym widoku.

Cel ekranu jako zadanie użytkownika: w kilkanaście sekund chcę wiedzieć, co dziś i jutro dotyczy moich dzieci - kto ma zajęcia, czy trzeba coś zapłacić, czy zwolniło się miejsce z listy rezerwowej, czy dziecko ma start na zawodach - i jednym dotknięciem załatwić najpilniejszą sprawę.

Punkty wejścia: udane logowanie z PAR-01, link z powiadomienia push lub e-mail, powrót z dowolnego ekranu szczegółowego, ikona aplikacji.

Warunki wstępne: aktywne konto rodzinne z przynajmniej jednym profilem dziecka. Konto bez dzieci kieruje do dodania pierwszego profilu.

Dane wyświetlane: przełącznik profilu dziecka u góry, najbliższe zajęcia i starty z godziną i miejscem, alerty wymagające reakcji w kolejności pilności - zaległa płatność, brakujący dokument, wygasająca zgoda, oferta z listy rezerwowej z odliczaniem, postęp umiejętności od ostatniej wizyty, saldo portfela jako kredyt u organizatora. Kwoty według regionu konta.

Komponenty interfejsu: pasek profili dzieci z awatarami, kafelki wskaźników, oś najbliższych dni dla wszystkich dzieci łącznie, lista alertów z priorytetem i akcją wprost na kafelku, skrót zgłoszenia nieobecności, skrót do portfela.

Akcja główna: przejdź do najpilniejszego alertu. Jedna dominująca akcja prowadzi prosto do sprawy, którą trzeba załatwić teraz.

Akcje drugorzędne: przełącz profil dziecka, zgłoś nieobecność, otwórz grafik, otwórz portfel, otwórz galerię zdjęć.

Akcje niszczące: nie dotyczy.

Stany ekranu: ładowanie pokazuje szkielety kafelków; pusty wita rodzica bez dzieci i kieruje do dodania pierwszego profilu w PAR-03; wypełniony pokazuje pełen pulpit; błąd ładowania danego kafelka dotyczy tylko jego, a nie całego ekranu, z przyciskiem ponów; sukces nie dotyczy osobno; brak uprawnień nie dotyczy, rodzic jest właścicielem swoich danych; offline pokazuje ostatnio wczytany pulpit z banerem o wstrzymaniu zmian i datą ostatniej synchronizacji.

Walidacja i komunikaty: przy nieudanym pobraniu wskaźników komunikat mówi, że nie udało się pobrać danego fragmentu, i proponuje ponowienie. Liczby ładują się niezależnie, więc częściowy błąd oznacza dany kafelek. Alert z odliczaniem pokazuje, ile czasu zostało na decyzję, żeby rodzic nie przegapił okna.

Przypadki brzegowe: rodzic z czworgiem dzieci u trzech organizatorów, gdzie pulpit grupuje sprawy po pilności, nie po organizatorze; dziecko zapisane jednocześnie na zajęcia i zawody w tym samym tygodniu; nakładające się starty dwójki dzieci w jednym bloku godzinowym z ostrzeżeniem o kolizji; rodzina z saldem portfela u jednego organizatora i długiem u innego.

Przejścia i następne ekrany: alert płatności prowadzi do PAR-10, alert dokumentu do PAR-09, oferta rezerwowa do PAR-07, skrót nieobecności do PAR-05, kafelek postępu do PAR-08, kafelek startu do PAR-11, profil dziecka do PAR-03.

Zdarzenia systemowe: odświeżanie wskaźników po wejściu i co kilka minut, push o ofercie z listy rezerwowej, push o odwołaniu zajęć, sygnał o nowej płatności do rozliczenia, sygnał o nowych zdjęciach z RunPixie.

Wymagania prawne: pulpit pokazuje dane zagregowane, dane wrażliwe dziecka pojawiają się dopiero po wejściu w kartę. Zgodność z RODO przez minimalizację danych na widoku startowym i przez to, że rodzic widzi wyłącznie własne dzieci.

Kontekst urządzenia: telefon jako podstawowy, układ pionowy z jedną kolumną alertów. Tablet i desktop pokazują szerszy przegląd kilku dzieci obok siebie.

Lokalizacja i języki: etykiety jako klucze tłumaczeń, kwoty i daty według regionu, nazwy zajęć i wydarzeń jako treść organizatora z fallbackiem języka.

Lista kontrolna heurystyk: widoczność stanu systemu przez świeże wskaźniki i znacznik ostatniej synchronizacji; rozpoznawanie zamiast przypominania, bo alerty mówią, co zrobić; minimalistyczny projekt przez ograniczenie startu do spraw pilnych; zgodność ze światem przez język rodzica, nie żargon bazy; pomoc w diagnozie przez bezpośrednie linki do źródła sprawy.

Metryki sukcesu: czas od wejścia do pierwszej sensownej akcji, odsetek alertów rozwiązanych tego samego dnia, odsetek przegapionych okien ofert rezerwowych, częstotliwość przełączania profili.

---

### PAR-03 - profile dzieci z przełączaniem

Grupa i rola: rodzic. Profile dzieci są podrzędne wobec konta rodzinnego.

Moduł: wspolny.

Cel ekranu jako zadanie użytkownika: chcę dodać dziecko do rodziny i błyskawicznie przełączać się między dziećmi, żeby każda kolejna czynność dotyczyła właściwego dziecka, bez pomyłki, którą trudno potem cofnąć.

Punkty wejścia: pasek profili z pulpitu PAR-02, pusty stan pulpitu kierujący do dodania pierwszego dziecka, link z zapisu w PAR-04 lub PAR-11, ustawienia konta PAR-16.

Warunki wstępne: aktywne konto rodzinne. Dla edycji danych zdrowotnych dodatkowa zgoda na przetwarzanie szczególnej kategorii danych.

Dane wyświetlane: lista profili dzieci z imieniem, awatarem, wiekiem i przypisanymi grupami oraz startami, aktywny profil wyróżniony, podstawowe dane dziecka, znacznik kompletności profilu, ostrzeżenia o brakujących dokumentach lub zgodach. Dane zdrowotne ukryte za osobnym wejściem.

Komponenty interfejsu: karty profili z dużym celem dotykowym, wyraźny wskaźnik aktywnego dziecka utrzymany na wszystkich kolejnych ekranach, formularz danych dziecka, sekcja dokumentów linkująca do PAR-09, przełącznik trybu odczytu dla nastolatka łączący z KID-02.

Akcja główna: przełącz na wybrane dziecko. Przy dodawaniu dominującą akcją jest zapis nowego profilu.

Akcje drugorzędne: dodaj dziecko, edytuj dane, otwórz dokumenty, zaproś drugiego opiekuna do współdzielenia profilu, włącz widok nastolatka.

Akcje niszczące: usuń profil dziecka. Dostępne tylko bez aktywnych zapisów i historii rozliczeń, w innym wypadku proponuje się archiwizację z zachowaniem paszportu i historii. Usunięcie wymaga wyraźnego potwierdzenia.

Stany ekranu: ładowanie z szkieletem kart; pusty proponuje dodanie pierwszego dziecka i tłumaczy, że profil zbierze udział w obu modułach; wypełniony to lista profili; błąd zapisu przy formularzu; sukces potwierdza zapis tostem i przełącza na nowy profil; brak uprawnień nie dotyczy własnych dzieci, dotyczy współdzielonych profili o ograniczonym zakresie; offline pozwala przełączać profile z danych lokalnych, dodanie dziecka wymaga sieci.

Walidacja i komunikaty: imię dziecka wymagane. Data urodzenia w przyszłości prosi o korektę. Wiek poza zakresem oferty organizatora tłumaczy, że dziecko może nie kwalifikować się do dostępnych grup, zamiast cicho ukrywać ofertę. Niespójna data między profilem a dokumentem pokazuje rozbieżność i pyta, która jest poprawna.

Przypadki brzegowe: rodzeństwo bliźniacze o tym samym nazwisku i podobnym imieniu, gdzie awatar i kolor profilu zmniejszają ryzyko pomyłki; dziecko przechodzące w wiek nastolatka z opcją własnego widoku odczytu; dwoje opiekunów współdzielących profil z różnym zakresem; dziecko przepisane między organizatorami z zachowaniem jednego profilu.

Przejścia i następne ekrany: wybór dziecka ustawia kontekst dla PAR-04, PAR-05, PAR-08, PAR-11 i pozostałych. Dokumenty prowadzą do PAR-09, zgoda opiekuna do KID-01, widok nastolatka do KID-02, zaproszenie drugiego opiekuna do PAR-16.

Zdarzenia systemowe: propagacja aktywnego profilu na kolejne ekrany, sygnał o niekompletnym profilu blokującym zapis, aktualizacja znaczników dokumentów po zmianie w PAR-09.

Wymagania prawne: dane dziecka jako dane osobowe pod RODO przetwarzane przez opiekuna, dane zdrowotne jako szczególna kategoria za osobną zgodą i osobnym wejściem, prawo do współdzielenia profilu między opiekunami z jasnym zakresem, usuwanie i archiwizacja zgodne z zasadą ograniczenia przechowywania.

Kontekst urządzenia: telefon jako podstawowy, przełącznik profili w stałym miejscu nagłówka. Tablet do spokojnej edycji danych.

Lokalizacja i języki: etykiety jako klucze, imiona jako dane, format daty według regionu, przełącznik języka w nagłówku.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania przez wyraźnie zaznaczony aktywny profil; zapobieganie błędom przez kolor i awatar odróżniające rodzeństwo; kontrola i swoboda przez archiwizację zamiast nieodwracalnego usuwania; widoczność stanu systemu przez znacznik kompletności profilu.

Metryki sukcesu: liczba czynności wykonanych na złym profilu, czas przełączenia między dziećmi, odsetek kompletnych profili, liczba profili współdzielonych przez dwoje opiekunów.

---

### PAR-04 - zapis dziecka na grupę z poziomami

Grupa i rola: rodzic.

Moduł: wspolny. Zapis dotyczy modułu zajęć, ale odbywa się z tego samego konta co zawody.

Cel ekranu jako zadanie użytkownika: chcę zapisać dziecko do właściwej grupy o właściwym poziomie i od razu zobaczyć, ile to kosztuje i czy są miejsca, najlepiej kończąc cały zapis i płatność w jednym przejściu.

Punkty wejścia: publikacja grupy przez organizatora z OCL-02, brandowana strona grupy z GST-03, lista grafiku z GST-04, kafelek z pulpitu PAR-02, oferta z listy rezerwowej PAR-07, profil dziecka z PAR-03.

Warunki wstępne: aktywny profil dziecka, opublikowana grupa z przypisanym poziomem, skonfigurowany plan cenowy w OCL-07, podpięte bramki płatności na poziomie operatora.

Dane wyświetlane: dostępne grupy filtrowane do poziomu i wieku dziecka, dzień i godzina, instruktor, sala lub tor, liczba wolnych miejsc, cena według planu z OCL-07 z uwzględnieniem zniżki rodzinnej, wymagane dokumenty i zgody, możliwość pokrycia części opłaty z portfela.

Komponenty interfejsu: lista grup z poziomem i dostępnością, podpowiedź dopasowania poziomu na podstawie postępów z PAR-08, podsumowanie kosztu z rozbiciem na cenę, zniżkę i kredyt z portfela, sekcja zgód i dokumentów, wybór metody płatności Przelewy24, PayU, Stripe, BLIK oraz karty.

Akcja główna: zapisz dziecko i przejdź do płatności. Gdy opłatę pokrywa portfel w całości, dominującą akcją jest potwierdź zapis.

Akcje drugorzędne: zmień grupę, zmień dziecko, zapisz zapis na później, zobacz szczegóły poziomu, dopłać z portfela.

Akcje niszczące: nie dotyczy na etapie zapisu. Rezygnacja z trwającego zapisu nie tworzy zobowiązania.

Stany ekranu: ładowanie listy grup ze szkieletem; pusty oznacza brak grup dla poziomu dziecka i proponuje listę rezerwową albo inny poziom; wypełniony to lista z dostępnością; błąd płatności wraca do podsumowania z zachowanym wyborem i tłumaczy przyczynę; sukces prowadzi do potwierdzenia i karnetu; brak uprawnień nie dotyczy; offline pozwala przejrzeć ofertę z danych lokalnych, sam zapis i płatność wymagają sieci.

Walidacja i komunikaty: grupa zapełniona w trakcie zapisu pokazuje to i proponuje listę rezerwową w PAR-07 zamiast cichej odmowy. Brak wymaganego dokumentu blokuje finalizację i tłumaczy, którego brakuje i gdzie go dodać. Poziom dziecka odbiegający od poziomu grupy ostrzega i proponuje konsultację z instruktorem, bez twardej blokady. Nieudana płatność nie zapisuje dziecka i wyjaśnia, że miejsce jest zarezerwowane na krótki czas.

Przypadki brzegowe: zapis rodzeństwa do tej samej grupy ze zniżką rodzinną; dziecko zmieniające poziom w trakcie semestru; zapis na grupę o zerowym limicie działającą jak lista oczekujących; pokrycie opłaty w części portfelem i w części kartą; zapis tuż przed startem semestru z proporcjonalnym rozliczeniem.

Przejścia i następne ekrany: dostępność i poziomy pochodzą z OCL-02 i drzewa OCL-10, plan cenowy z OCL-07, sukces prowadzi do potwierdzenia i do płatności rozliczanej w PAR-10, zapełniona grupa kieruje do PAR-07, dokumenty do PAR-09.

Zdarzenia systemowe: rezerwacja miejsca na czas płatności, zwolnienie rezerwacji po wygaśnięciu, aktualizacja licznika miejsc w OCL-02, naliczenie cyklicznej opłaty abonamentowej zgodnie z planem, zapis kredytu portfela po nadpłacie.

Wymagania prawne: ceny brutto widoczne dla konsumenta, jasna informacja o cyklu i sposobie rezygnacji z abonamentu, zgody i regulamin zaakceptowane przy zapisie z zapisaną wersją, dane zdrowotne dziecka jako szczególna kategoria, zgoda opiekuna dla małoletniego realizowana z konta przez KID-01.

Kontekst urządzenia: telefon jako podstawowy, formularz krokowy z dużymi celami dotykowymi. Tablet i desktop do spokojnego porównania grup.

Lokalizacja i języki: nazwy grup jako treść organizatora, etykiety jako klucze, kwoty, waluta i daty według regionu, dni tygodnia i godziny lokalnie.

Lista kontrolna heurystyk: zgodność ze światem przez opis grupy językiem rodzica; zapobieganie błędom przez kontrolę dokumentów i dostępności przed płatnością; widoczność stanu systemu przez jawne rozbicie kosztu i licznik miejsc; rozpoznawanie zamiast przypominania przez podpowiedź poziomu z postępów dziecka; pomoc w diagnozie przez czytelny komunikat nieudanej płatności.

Metryki sukcesu: odsetek zapisów ukończonych w jednym przejściu, odsetek zapisów z poprawnie dobranym poziomem, udział płatności pokrytych częściowo portfelem, liczba zapisów porzuconych na etapie płatności.

---

### PAR-05 - zgłoszenie nieobecności

Grupa i rola: rodzic.

Moduł: wspolny. Dotyczy zajęć, działa też przy obozach i lekcjach indywidualnych.

Cel ekranu jako zadanie użytkownika: chcę zgłosić, że dziecko nie przyjdzie na zajęcia, jednym dotknięciem, z wyprzedzeniem albo w ostatniej chwili po drodze, i od razu wiedzieć, czy należy mi się odrobienie.

Punkty wejścia: skrót nieobecności z pulpitu PAR-02, karta zajęć z grafiku dziecka, powiadomienie push przed zajęciami, profil dziecka z PAR-03.

Warunki wstępne: dziecko zapisane do grupy z zaplanowanym terminem, skonfigurowane reguły odrabiania w OCL-04.

Dane wyświetlane: najbliższe i kolejne terminy dziecka z możliwością zaznaczenia nieobecności, okno na zgłoszenie przed zajęciami wynikające z reguły, informacja, czy nieobecność uprawnia do odrobienia, liczba pozostałych odrobień w semestrze, krótki powód jako pole opcjonalne.

Komponenty interfejsu: lista terminów z dużym przełącznikiem obecności, znacznik prawa do odrobienia przy każdym terminie, pole powodu, potwierdzenie z informacją o skutku, skrót do zapisu na odrabianie po zgłoszeniu.

Akcja główna: zgłoś nieobecność na wybranym terminie. Jedno dotknięcie zaznacza nieobecność i pokazuje skutek.

Akcje drugorzędne: cofnij zgłoszenie przed upływem okna, dodaj powód, zgłoś nieobecność na serię terminów, przejdź do odrabiania.

Akcje niszczące: nie dotyczy w sensie kasowania danych. Cofnięcie zgłoszenia przywraca obecność i ewentualnie odbiera utworzone prawo do odrobienia, z jasnym komunikatem.

Stany ekranu: ładowanie listy terminów; pusty oznacza brak nadchodzących zajęć; wypełniony to lista terminów; błąd zapisu zgłoszenia z ponowieniem albo kolejką offline; sukces potwierdza zgłoszenie i pokazuje, czy powstało prawo do odrobienia; brak uprawnień nie dotyczy; offline pozwala zgłosić nieobecność z danych lokalnych, zgłoszenie czeka w kolejce i synchronizuje się po sieci, z zabezpieczeniem przed podwójnym zapisem.

Walidacja i komunikaty: zgłoszenie po zamknięciu okna tłumaczy, że nie uprawnia już do odrobienia, i proponuje prośbę o wyjątek do organizatora. Zgłoszenie na termin już rozpoczęty pokazuje, że jest spóźnione. Cofnięcie po terminie tłumaczy, że obecność została już rozliczona. Nieobecność na opłaconej lekcji indywidualnej kieruje do zasad zwrotu organizatora.

Przypadki brzegowe: nieobecność zgłoszona przy basenie bez zasięgu, trafiająca do kolejki lokalnej; rodzeństwo nieobecne na tym samym terminie zgłaszane łącznie; dziecko zmieniające poziom w trakcie semestru z niewykorzystanym prawem do odrobienia; nieobecność na obozie wielodniowym obejmująca kilka dni; reguła odrabiania zmieniona przez organizatora po zgłoszeniu.

Przejścia i następne ekrany: reguły pochodzą z OCL-04, prawo do odrobienia prowadzi do zapisu na odrabianie w PAR-06, kontekst dziecka z PAR-03, lekcja indywidualna łączy z PAR-12 i odwołaniem po stronie instruktora w INS-09.

Zdarzenia systemowe: utworzenie prawa do odrobienia po zgłoszeniu w oknie reguły, powiadomienie instruktora o nieobecności do listy obecności w INS-04, wygaśnięcie prawa po okresie ważności, synchronizacja kolejki offline po powrocie sieci.

Wymagania prawne: powód nieobecności jako dane opcjonalne, bez wymuszania danych zdrowotnych dziecka, zgłoszenie jako element regulaminu odrabiania w zaakceptowanej wersji, minimalizacja danych w polu powodu.

Kontekst urządzenia: telefon jako podstawowy, zgłoszenie jednym dotknięciem w ruchu. Tablet do przeglądu serii terminów.

Lokalizacja i języki: daty i godziny według regionu, etykiety jako klucze, powód jako treść rodzica z fallbackiem języka.

Lista kontrolna heurystyk: kontrola i swoboda przez cofnięcie zgłoszenia w oknie reguły; widoczność stanu systemu przez natychmiastową informację o prawie do odrobienia; zapobieganie błędom przez jasny komunikat o spóźnionym zgłoszeniu; zgodność ze światem przez prosty przełącznik obecności znany z życia.

Metryki sukcesu: czas zgłoszenia nieobecności, odsetek zgłoszeń w oknie uprawniającym do odrobienia, odsetek zgłoszeń obsłużonych offline bez błędu, udział zgłoszeń przekształconych w zapis na odrabianie.

---

### PAR-06 - zapis na odrabianie

Grupa i rola: rodzic.

Moduł: wspolny.

Cel ekranu jako zadanie użytkownika: mam prawo do odrobienia opuszczonych zajęć i chcę, żeby system sam pokazał mi wolne terminy w grupie o właściwym poziomie, a ja tylko wybieram i potwierdzam.

Punkty wejścia: skrót z potwierdzenia nieobecności w PAR-05, alert z pulpitu PAR-02, powiadomienie push o dostępnym terminie odrobienia, profil dziecka z PAR-03.

Warunki wstępne: aktywne prawo do odrobienia utworzone w PAR-05 zgodnie z regułą OCL-04, istnieją wolne miejsca w grupie zgodnej z regułą.

Dane wyświetlane: lista praw do odrobienia z datą nieobecności i terminem ważności, proponowane wolne terminy w grupach zgodnych poziomem, godzina, instruktor i miejsce, liczba wolnych miejsc, informacja o terminie wygaśnięcia prawa.

Komponenty interfejsu: lista dostępnych praw, lista proponowanych terminów z dużym celem dotykowym, filtr po dniu i godzinie, podgląd potwierdzenia, znacznik kończącego się terminu ważności.

Akcja główna: zapisz dziecko na wybrany termin odrobienia. Jedno potwierdzenie domyka zapis bez ponownej płatności, bo odrobienie wynika z opłaconych zajęć.

Akcje drugorzędne: zmień termin, odwołaj zapisane odrobienie przed jego datą, pokaż więcej terminów, przełącz dziecko.

Akcje niszczące: odwołanie zapisanego odrabiania. Zwalnia miejsce i przywraca prawo do odrobienia, jeśli mieści się w terminie ważności, z jasnym komunikatem.

Stany ekranu: ładowanie listy terminów; pusty oznacza brak wolnych terminów w zgodnej grupie i proponuje obserwowanie albo listę rezerwową; wypełniony to lista terminów; błąd zapisu z ponowieniem; sukces potwierdza zapis i pokazuje go w grafiku dziecka; brak uprawnień nie dotyczy; offline pokazuje ostatnio wczytane terminy z banerem, że potwierdzenie wymaga sieci ze względu na limit miejsc.

Walidacja i komunikaty: próba zapisu na termin, który właśnie się zapełnił, tłumaczy to i proponuje inny. Prawo po terminie ważności pokazuje, że wygasło, i proponuje prośbę o wyjątek. Termin kolidujący z innymi zajęciami dziecka ostrzega o nakładaniu. Brak terminów zgodnych poziomem tłumaczy, że odrabia się zwykle na tym samym poziomie, zgodnie z regułą organizatora.

Przypadki brzegowe: dziecko z kilkoma prawami do odrobienia naraz; odrabianie w grupie innego instruktora niż macierzysta; dwie grupy o tym samym poziomie z dostępnymi terminami; odrabianie tuż przed wygaśnięciem prawa, gdy lista terminów jest pusta; dziecko, które zmieniło poziom po powstaniu prawa.

Przejścia i następne ekrany: prawa i reguły pochodzą z OCL-04 i ze zgłoszenia w PAR-05, dostępność terminów z grafiku OCL-03, zapis pojawia się w grafiku dziecka i na liście obecności instruktora w INS-04, kontekst dziecka z PAR-03.

Zdarzenia systemowe: rezerwacja miejsca na termin odrabiania, dopisanie do listy obecności grupy goszczącej, wygaśnięcie prawa po terminie, push o nowym wolnym terminie zgodnym z prawem.

Wymagania prawne: odrabianie jako realizacja opłaconej usługi bez dodatkowej opłaty, zasady jako element regulaminu w zaakceptowanej wersji, dane dziecka w grupie goszczącej w zakresie potrzebnym do obecności.

Kontekst urządzenia: telefon jako podstawowy, wybór terminu jednym dotknięciem. Tablet do przeglądu wielu terminów.

Lokalizacja i języki: daty, godziny i nazwy dni według regionu, etykiety jako klucze, nazwy grup jako treść organizatora.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania przez gotową listę zgodnych terminów; widoczność stanu systemu przez znacznik wygasającego prawa; zapobieganie błędom przez ostrzeżenie o kolizji terminów; kontrola i swoboda przez odwołanie odrabiania z przywróceniem prawa.

Metryki sukcesu: odsetek praw do odrobienia wykorzystanych przed wygaśnięciem, czas od zgłoszenia nieobecności do zapisu na odrobienie, odsetek odrobień obsłużonych bez kontaktu z recepcją, liczba kolizji terminów wykrytych przed zapisem.

---

## Partia 2 (PAR-07 do PAR-12)

### PAR-07 - lista rezerwowa z push

Grupa i rola: rodzic.

Moduł: wspolny.

Cel ekranu jako zadanie użytkownika: grupa, którą chcę, jest pełna, więc chcę stanąć w kolejce, znać swoją pozycję i dostać powiadomienie, gdy zwolni się miejsce, żeby zdążyć potwierdzić, zanim oferta przejdzie dalej.

Punkty wejścia: zapełniona grupa w zapisie PAR-04, alert i push z pulpitu PAR-02, brandowana strona grupy z GST-03, oferta automatyczna z OCL-05.

Warunki wstępne: grupa z osiągniętym limitem miejsc i włączoną listą rezerwową w OCL-05, aktywny profil dziecka.

Dane wyświetlane: pozycja w kolejce, data zgłoszenia, reguła dopisania, czas na potwierdzenie oferty z odliczaniem, status zgłoszenia, ewentualne inne grupy o tym samym poziomie z wolnymi miejscami jako alternatywa.

Komponenty interfejsu: karta pozycji w kolejce z dużym licznikiem, przycisk potwierdzenia oferty widoczny tylko w oknie oferty, odliczanie do wygaśnięcia oferty, lista alternatywnych grup, przełącznik dziecka.

Akcja główna: potwierdź zwolnione miejsce, gdy nadejdzie oferta. Poza oknem oferty dominującą akcją jest dołącz do listy rezerwowej.

Akcje drugorzędne: opuść kolejkę, zobacz alternatywne grupy, zmień dziecko, ustaw preferencję kanału powiadomień.

Akcje niszczące: opuszczenie listy rezerwowej. Zwalnia pozycję i informuje, że ponowne dołączenie zaczyna kolejkę od końca, z potwierdzeniem.

Stany ekranu: ładowanie pozycji; pusty oznacza brak kolejki, bo są wolne miejsca, i kieruje do zwykłego zapisu PAR-04; wypełniony pokazuje pozycję i status; oferta to wyróżniony stan z odliczaniem i przyciskiem potwierdzenia; błąd potwierdzenia z ponowieniem i jasnym komunikatem, że oferta jeszcze trwa; sukces prowadzi do zapisu i płatności; brak uprawnień nie dotyczy; offline pokazuje ostatnią pozycję z banerem, że potwierdzenie oferty wymaga sieci.

Walidacja i komunikaty: oferta wygasła tłumaczy, że miejsce przeszło do kolejnej osoby, i proponuje pozostanie w kolejce. Potwierdzenie tuż po wygaśnięciu wyjaśnia, że okno się zamknęło. Dołączenie do kolejki na grupę, gdzie dziecko jest już zapisane na inny poziom, pokazuje to. Dziecko poza zakresem wieku grupy ostrzega przed dołączeniem.

Przypadki brzegowe: rodzic w kolejce na dwie grupy o tym samym poziomie z jedną wspólną pulą; oferta przychodząca w nocy, gdy rodzic śpi, z odpowiednio dobranym oknem; potwierdzenie z urządzenia bez zasięgu; rodzeństwo w jednej kolejce na tę samą grupę; oferta tuż przed startem semestru ze skróconym oknem.

Przejścia i następne ekrany: kolejka i reguły pochodzą z OCL-05, oferta wynika ze zwolnienia miejsca po stronie organizatora, potwierdzenie prowadzi do zapisu PAR-04 i płatności PAR-10, alternatywy do innych grup w PAR-04, push i preferencje kanału łączą z PAR-16.

Zdarzenia systemowe: push o ofercie z odliczaniem, timer okna na potwierdzenie, eskalacja oferty do kolejnej osoby po wygaśnięciu, aktualizacja pozycji po zmianach w kolejce, dopisanie do grupy po potwierdzeniu.

Wymagania prawne: przejrzysta kolejność jako element regulaminu, zgoda na kanał powiadomień push lub e-mail, dane w zakresie potrzebnym do obsługi kolejki, jasna informacja o skutku opuszczenia kolejki.

Kontekst urządzenia: telefon jako podstawowy, push jako główny nośnik oferty, potwierdzenie jednym dotknięciem. Tablet do przeglądu alternatyw.

Lokalizacja i języki: etykiety jako klucze, daty i godziny według regionu, treść oferty tłumaczona z fallbackiem na en-GB.

Lista kontrolna heurystyk: widoczność stanu systemu przez jawną pozycję i odliczanie oferty; kontrola i swoboda przez opuszczenie kolejki z jasnym skutkiem; zapobieganie błędom przez ostrzeżenie o niezgodnym wieku lub poziomie; zgodność ze światem przez prosty obraz kolejki znany z życia.

Metryki sukcesu: odsetek ofert potwierdzonych w oknie, mediana czasu od oferty do potwierdzenia, odsetek miejsc obsadzonych z listy rezerwowej, liczba rezygnacji z kolejki.

---

### PAR-08 - postępy umiejętności dziecka

Grupa i rola: rodzic. Dane postępu tworzy instruktor w INS-05, rodzic widzi je w odczycie.

Moduł: wspolny. Postęp z zajęć łączy się z osiągnięciami z zawodów w paszporcie.

Cel ekranu jako zadanie użytkownika: chcę zobaczyć, czego moje dziecko już się nauczyło i co przed nim, w formie zrozumiałej dla rodzica, a nie żargonem trenerskim, żeby docenić postęp i wiedzieć, do jakiej grupy dziecko dojrzewa.

Punkty wejścia: kafelek postępu z pulpitu PAR-02, profil dziecka z PAR-03, link z potwierdzenia zajęć, paszport sportowy PAR-13.

Warunki wstępne: dziecko zapisane do grupy z przypisanym drzewem umiejętności z OCL-10, istnieją wpisy postępu od instruktora z INS-05.

Dane wyświetlane: drzewo umiejętności dziecka z poziomami i statusem każdej umiejętności, ostatnio zdobyte umiejętności, umiejętności w trakcie, opis każdej umiejętności językiem rodzica, opcjonalne krótkie filmy instruktażowe powiązane z umiejętnością, sugerowany kolejny poziom.

Komponenty interfejsu: wizualne drzewo lub ścieżka umiejętności z czytelnymi znacznikami statusu, karta umiejętności z opisem i filmem, oś postępu w czasie, znacznik gotowości do wyższej grupy, link do zapisu na wyższy poziom.

Akcja główna: otwórz szczegół umiejętności. Przy gotowości do wyższego poziomu dominującą akcją jest zobacz zapis na wyższą grupę.

Akcje drugorzędne: odtwórz film instruktażowy, przełącz dziecko, pokaż historię postępu, otwórz paszport.

Akcje niszczące: nie dotyczy. Rodzic nie edytuje postępu, to domena instruktora.

Stany ekranu: ładowanie drzewa ze szkieletem; pusty oznacza brak wpisów postępu i tłumaczy, że pojawią się po pierwszych zajęciach; wypełniony to drzewo z postępem; błąd ładowania z ponowieniem; sukces nie dotyczy osobno, ekran jest odczytowy; brak uprawnień nie dotyczy własnego dziecka; offline pokazuje ostatnio wczytany postęp z datą synchronizacji.

Walidacja i komunikaty: brak filmu dla umiejętności pokazuje sam opis bez pustego odtwarzacza. Drzewo zmienione przez organizatora po zdobyciu umiejętności pokazuje historyczny stan z notatką o zmianie definicji. Postęp w trakcie semestru przejściowego, gdy grupa zmienia poziom, tłumaczy kontekst zmiany.

Przypadki brzegowe: dziecko w dwóch grupach o różnych drzewach; umiejętność zdobyta w starym drzewie nieobecna w nowym; dziecko obecne tylko w module zawodów bez drzewa zajęć; rodzeństwo o bardzo różnym tempie postępu pokazywane obok siebie po przełączeniu profilu.

Przejścia i następne ekrany: definicja drzewa pochodzi z OCL-10, wpisy z INS-05, gotowość do wyższego poziomu prowadzi do zapisu PAR-04, dane zasilają paszport PAR-13, kontekst dziecka z PAR-03.

Zdarzenia systemowe: aktualizacja drzewa po nowym wpisie instruktora, push o zdobytej umiejętności lub gotowości do wyższej grupy, regeneracja sekcji paszportu po zmianie postępu.

Wymagania prawne: dane postępu dziecka jako dane osobowe pod RODO widoczne wyłącznie dla opiekuna, filmy bez wizerunku innych dzieci bez zgody, opis bez danych wrażliwych.

Kontekst urządzenia: telefon jako podstawowy, drzewo przewijane pionowo z dużymi znacznikami. Tablet i desktop do szerszego widoku ścieżki.

Lokalizacja i języki: nazwy i opisy umiejętności jako treść organizatora z możliwością tłumaczenia, etykiety jako klucze, daty według regionu.

Lista kontrolna heurystyk: zgodność ze światem przez opis umiejętności językiem rodzica; widoczność stanu systemu przez czytelny status każdej umiejętności; rozpoznawanie zamiast przypominania przez wizualne drzewo zamiast listy kodów; pomoc i dokumentacja przez filmy instruktażowe przy umiejętności.

Metryki sukcesu: częstotliwość otwierania postępu przez rodzica, odsetek rodziców przechodzących z postępu do zapisu na wyższy poziom, czas spędzony na karcie umiejętności, udział umiejętności z obejrzanym filmem.

---

### PAR-09 - dokumenty dziecka

Grupa i rola: rodzic.

Moduł: wspolny. Dokumenty obsługują zarówno zajęcia, jak i zawody oraz obozy.

Cel ekranu jako zadanie użytkownika: chcę mieć w jednym miejscu wszystkie dokumenty i zgody dziecka - orzeczenia, zgody medyczne, regulaminy - i od razu widzieć, czego brakuje albo co wkrótce wygasa, żeby zapis nie utknął na braku papieru.

Punkty wejścia: alert dokumentu z pulpitu PAR-02, sekcja dokumentów w profilu PAR-03, blokada w zapisie PAR-04 lub PAR-11, krok dokumentów w zapisie na obóz z OCL-06.

Warunki wstępne: aktywny profil dziecka, lista wymaganych dokumentów zdefiniowana przez organizatora dla danej oferty.

Dane wyświetlane: lista dokumentów z statusem - brak, wgrany, zaakceptowany, wygasa, wygasły - data ważności, do której oferty dokument jest wymagany, podgląd wgranego pliku, wymagane zgody z wersją regulaminu, znacznik dokumentów wrażliwych.

Komponenty interfejsu: lista dokumentów z kolorem semantycznym statusu, przycisk wgrania pliku lub zrobienia zdjęcia dokumentu, podgląd, sekcja zgód z polem akceptacji, znacznik zbliżającej się daty wygaśnięcia.

Akcja główna: wgraj brakujący dokument. Przy komplecie dokumentów dominującą akcją jest przejrzyj i zaakceptuj wymagane zgody.

Akcje drugorzędne: zrób zdjęcie dokumentu, zastąp wygasający dokument, pobierz kopię, przełącz dziecko.

Akcje niszczące: usuń wgrany dokument. Z potwierdzeniem i ostrzeżeniem, że brak dokumentu może zablokować aktywny zapis dziecka.

Stany ekranu: ładowanie listy; pusty oznacza brak wymaganych dokumentów dla bieżących zapisów; wypełniony to lista z statusami; błąd wgrania z ponowieniem i informacją o dozwolonym formacie i rozmiarze; sukces potwierdza wgranie i zmianę statusu; brak uprawnień nie dotyczy własnego dziecka; offline pozwala przygotować dokument do wgrania w kolejce, samo wgranie i akceptacja zgód wymagają sieci.

Walidacja i komunikaty: plik w niewspieranym formacie tłumaczy, jakie formaty są dozwolone. Plik za duży podaje limit rozmiaru. Dokument wygasły przed datą oferty ostrzega, że trzeba go odnowić. Akceptacja nieaktualnej wersji regulaminu prosi o przejrzenie nowej wersji. Wgranie dokumentu wrażliwego prosi o potwierdzenie świadomości, że to szczególna kategoria danych.

Przypadki brzegowe: jeden dokument ważny dla kilku ofert i organizatorów jednocześnie; dokument wygasający w trakcie semestru z przypomnieniem przed wygaśnięciem; zgoda medyczna wymagana tylko dla obozu wyjazdowego; rodzeństwo dzielące ten sam typ dokumentu rodzinnego; dokument odrzucony przez organizatora z prośbą o poprawny.

Przejścia i następne ekrany: wymagania dokumentów pochodzą z ofert OCL-06, OEV i zapisów, status zasila zapis PAR-04 i PAR-11, dane wrażliwe łączą z profilem PAR-03, zgody z KID-01 dla małoletniego, karta uczestnika po stronie organizatora widzi status w OCL-09.

Zdarzenia systemowe: zmiana statusu po akceptacji organizatora, przypomnienie push o wygasającym dokumencie, blokada zapisu przy braku wymaganego dokumentu, log akceptacji wersji regulaminu.

Wymagania prawne: dokumenty medyczne i orzeczenia jako szczególna kategoria danych pod RODO, dostęp tylko dla opiekuna i uprawnionego organizatora, przechowywanie ograniczone czasowo, akceptacja regulaminu z zapisaną wersją i datą, prawo do pobrania własnej kopii.

Kontekst urządzenia: telefon jako podstawowy, zdjęcie dokumentu aparatem jako najszybsza ścieżka. Tablet i desktop do przeglądu kompletu.

Lokalizacja i języki: etykiety i statusy jako klucze tłumaczeń w kolorach semantycznych, nazwy typów dokumentów jako treść organizatora, daty według regionu.

Lista kontrolna heurystyk: widoczność stanu systemu przez czytelny status każdego dokumentu; zapobieganie błędom przez kontrolę formatu i daty ważności; pomoc w diagnozie przez komunikat o powodzie odrzucenia; zgodność ze światem przez nazwy dokumentów znane rodzicowi.

Metryki sukcesu: odsetek kompletnych zestawów dokumentów przed startem oferty, czas od żądania dokumentu do akceptacji, liczba zapisów zablokowanych brakiem dokumentu, odsetek dokumentów odnowionych przed wygaśnięciem.

---

### PAR-10 - płatności, portfel i faktury

Grupa i rola: rodzic.

Moduł: wspolny. Jeden portfel obsługuje zajęcia i zawody u danego organizatora.

Cel ekranu jako zadanie użytkownika: chcę zapłacić zaległe i nadchodzące opłaty, wykorzystać kredyt z portfela i pobrać faktury, widząc jasno, ile, za co i za które dziecko płacę.

Punkty wejścia: alert płatności z pulpitu PAR-02, podsumowanie zapisu z PAR-04 i PAR-11, oferta z listy rezerwowej PAR-07, profil dziecka, link z faktury w e-mailu.

Warunki wstępne: aktywne konto rodzinne, podpięte bramki płatności na poziomie operatora, istnieją należności lub saldo portfela.

Dane wyświetlane: lista należności z kwotą, terminem, dzieckiem i tytułem, saldo portfela jako kredyt u danego organizatora ze źródłem - nadpłata lub zwrot, harmonogram opłat cyklicznych abonamentu, historia płatności, lista faktur do pobrania, dostępne metody płatności Przelewy24, PayU, Stripe, BLIK oraz karty.

Komponenty interfejsu: lista należności z zaznaczaniem do opłaty, podsumowanie z rozbiciem na kwotę, kredyt portfela i do dopłaty, wybór metody płatności, sekcja portfela z historią ruchów, lista faktur z przyciskiem pobrania, harmonogram abonamentu.

Akcja główna: zapłać zaznaczone należności. Gdy portfel pokrywa całość, dominującą akcją jest rozlicz z portfela.

Akcje drugorzędne: pobierz fakturę, ustaw płatność cykliczną, użyj kredytu portfela do części opłaty, zmień metodę płatności, przełącz dziecko.

Akcje niszczące: nie dotyczy w sensie kasowania. Rezygnacja z abonamentu cyklicznego jest osobną, potwierdzaną czynnością z informacją o terminie obowiązywania do końca opłaconego okresu.

Stany ekranu: ładowanie listy; pusty oznacza brak należności i pokazuje saldo portfela; wypełniony to lista należności i portfel; błąd płatności wraca do podsumowania z zachowanym wyborem i tłumaczy przyczynę odrzucenia; sukces potwierdza opłatę i aktualizuje salda; brak uprawnień nie dotyczy; offline pokazuje ostatni stan z banerem, że płatność wymaga sieci.

Walidacja i komunikaty: brak zaznaczonej należności blokuje płatność i prosi o wybór. Kwota dopłaty po użyciu portfela pokazuje się przed potwierdzeniem. Odrzucona płatność tłumaczy najczęstsze przyczyny i proponuje inną metodę, bez zapisywania danych karty. Próba opłacenia należności innego dziecka pokazuje, którego dziecka dotyczy, żeby uniknąć pomyłki. Nadpłata trafia do portfela jako kredyt z jasną informacją o źródle.

Przypadki brzegowe: rodzina z saldem portfela u jednego organizatora i należnością u innego, gdzie kredyt nie przechodzi między organizatorami; zwrot za odwołane zajęcia trafiający do portfela; proporcjonalne rozliczenie przy zmianie planu w środku miesiąca; częściowa płatność portfelem i kartą; faktura korygująca po zwrocie.

Przejścia i następne ekrany: plany cenowe pochodzą z OCL-07, należności z zapisów PAR-04 i PAR-11, portfel i faktury po stronie organizatora widoczne w OCL-14 i OEV-19, zwroty i transfery z operacji masowych OEV-16, kontekst dziecka z PAR-03.

Zdarzenia systemowe: potwierdzenie płatności z bramki, naliczenie opłaty cyklicznej, zapis kredytu portfela po nadpłacie lub zwrocie, wystawienie faktury, push o nieudanej płatności cyklicznej z prośbą o aktualizację metody.

Wymagania prawne: ceny brutto dla konsumenta, faktura zgodna z lokalnymi przepisami podatkowymi, jasna informacja o cyklu i rezygnacji z abonamentu, kredyt portfela jako zobowiązanie organizatora wobec rodziny z przejrzystą historią, dane karty wprowadzane przez rodzica w bramce, nie przechowywane w panelu.

Kontekst urządzenia: telefon jako podstawowy, płatność BLIK i kartą zoptymalizowana pod jedną rękę. Tablet i desktop do przeglądu faktur i historii.

Lokalizacja i języki: kwoty, waluta i stawki podatku według regionu, etykiety jako klucze, tytuły należności jako treść organizatora z fallbackiem języka.

Lista kontrolna heurystyk: widoczność stanu systemu przez jawne salda i rozbicie kosztu; zapobieganie błędom przez wskazanie dziecka i tytułu należności; pomoc w diagnozie przez czytelny komunikat o odrzuconej płatności; zgodność ze światem przez prosty obraz portfela jako kredytu; kontrola użytkownika przez świadomą rezygnację z abonamentu.

Metryki sukcesu: odsetek należności opłaconych przed terminem, udział płatności z wykorzystaniem portfela, spadek liczby przeterminowanych płatności tydzień do tygodnia, odsetek nieudanych płatności cyklicznych odzyskanych po aktualizacji metody.

---

### PAR-11 - zapis dziecka na zawody między modułami

Grupa i rola: rodzic.

Moduł: wspolny. Ekran łączy moduł zawodów z tym samym kontem rodzinnym, co zajęcia.

Cel ekranu jako zadanie użytkownika: chcę zapisać to samo dziecko na zawody, korzystając z konta, profilu i dokumentów, które już mam z zajęć, bez zakładania drugiego konta i bez wpisywania danych dziecka od nowa.

Punkty wejścia: kafelek startu z pulpitu PAR-02, brandowana strona wydarzenia z GST-02, lista dystansów z GST-04, profil dziecka z PAR-03, link z komunikacji organizatora zawodów.

Warunki wstępne: aktywny profil dziecka z danymi i dokumentami, opublikowane wydarzenie z dystansami i kategoriami z OEV, podpięte bramki płatności.

Dane wyświetlane: dostępne dystanse i kategorie dopasowane do wieku i płci dziecka, data i miejsce startu, cena według progu z OEV-10 z licznikiem miejsc, pola warunkowe formularza z OEV-08, dziedziczone dane dziecka i dokumenty z profilu w odczycie, możliwość pokrycia części opłaty z portfela.

Komponenty interfejsu: lista dystansów z dostępnością i progiem cenowym, formularz zapisu z polami warunkowymi i danymi dziedziczonymi, sekcja gadżetów i dodatków, sekcja zgód i regulaminów, podsumowanie kosztu, wybór metody płatności.

Akcja główna: zapisz dziecko na dystans i przejdź do płatności. Gdy portfel pokrywa całość, dominującą akcją jest potwierdź zapis.

Akcje drugorzędne: zmień dystans, dokup gadżet, zmień dziecko, zapisz zapis na później, zgłoś się jako wolontariusz na to wydarzenie.

Akcje niszczące: nie dotyczy na etapie zapisu. Samoobsługowa rezygnacja po opłacie odbywa się zgodnie z polityką organizatora i kieruje do portfela na zwrot w formie kredytu.

Stany ekranu: ładowanie listy dystansów; pusty oznacza brak otwartych zapisów na wydarzenie; wypełniony to lista dystansów; błąd płatności wraca do podsumowania z zachowanym wyborem; sukces prowadzi do potwierdzenia z numerem zgłoszenia; brak uprawnień nie dotyczy; offline pozwala przejrzeć ofertę z danych lokalnych, zapis i płatność wymagają sieci.

Walidacja i komunikaty: dystans poza kategorią wiekową dziecka tłumaczy, że dziecko nie kwalifikuje się, i pokazuje właściwą kategorię. Próg cenowy wyczerpany w trakcie zapisu pokazuje nową cenę przed płatnością, żeby rodzic nie zapłacił innej kwoty niż widział. Brak wymaganego dokumentu kieruje do PAR-09. Limit miejsc osiągnięty proponuje listę rezerwową wydarzenia. Zgoda opiekuna dla małoletniego wymagana przed startem.

Przypadki brzegowe: dziecko zapisane jednocześnie na zajęcia i na zawody w tym samym tygodniu; rodzeństwo na różnych dystansach jednego wydarzenia ze wspólną płatnością; dziecko startujące u organizatora innego niż ten od zajęć, z osobnym portfelem; kolizja startu z udziałem rodzica jako wolontariusza z PAR-14; zmiana progu cenowego między dodaniem do koszyka a płatnością.

Przejścia i następne ekrany: dystanse i kategorie pochodzą z OEV, pola warunkowe z OEV-08, progi z OEV-10, gadżety z OEV-11, dane i dokumenty z profilu PAR-03 i PAR-09, płatność rozliczana w PAR-10, numer startowy nadawany w OEV-14, zgłoszenie wolontariusza w PAR-14, potwierdzenie i lista startowa po stronie organizatora w OEV-15.

Zdarzenia systemowe: rezerwacja miejsca i progu cenowego na czas płatności, nadanie numeru zgłoszenia, aktualizacja licznika miejsc i progu w OEV-10, zapis kredytu portfela po rezygnacji, push o potwierdzeniu zapisu.

Wymagania prawne: ceny brutto dla konsumenta, zgody i regulamin zawodów w zaakceptowanej wersji, zgoda opiekuna dla małoletniego z konta przez KID-01, dane zdrowotne dziecka jako szczególna kategoria, dziedziczenie danych z profilu jako realizacja minimalizacji zamiast ponownego zbierania.

Kontekst urządzenia: telefon jako podstawowy, formularz krokowy z danymi dziedziczonymi skracającymi wpisywanie. Tablet i desktop do spokojnego wyboru dystansu.

Lokalizacja i języki: nazwy dystansów i kategorii jako treść organizatora, etykiety jako klucze, kwoty, daty i miejsce według regionu wydarzenia.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania przez dane dziedziczone z profilu; zapobieganie błędom przez kontrolę kategorii wiekowej i progu cenowego przed płatnością; widoczność stanu systemu przez licznik miejsc i jawny próg; zgodność ze światem przez słownictwo zawodów znane rodzicowi; spójność przez ten sam profil dziecka w obu modułach.

Metryki sukcesu: odsetek zapisów na zawody korzystających z istniejącego profilu zamiast nowego, odsetek zapisów ukończonych w jednym przejściu, udział płatności pokrytych częściowo portfelem, liczba zapisów porzuconych po zmianie progu cenowego.

---

### PAR-12 - rezerwacja lekcji indywidualnej

Grupa i rola: rodzic.

Moduł: wspolny. Rezerwacja dotyczy modułu zajęć, rozliczana z konta rodzinnego.

Cel ekranu jako zadanie użytkownika: chcę zarezerwować dziecku lekcję jeden na jeden u konkretnego instruktora w dogodnym terminie, podać cel lekcji i zapłacić, widząc wolne okna bez dzwonienia na recepcję.

Punkty wejścia: profil dziecka z PAR-03, postępy umiejętności z PAR-08, brandowana strona placówki, link z komunikacji instruktora z INS-10, kafelek z pulpitu PAR-02.

Warunki wstępne: organizator włączył lekcje indywidualne w OCL-13, instruktor otworzył wolne okna w INS-09, aktywny profil dziecka.

Dane wyświetlane: kalendarz wolnych okien instruktorów z godziną i miejscem, dostępni instruktorzy z poziomem i specjalizacją, cena lekcji według planu z OCL-07, pole celu lekcji, status rezerwacji - oczekująca, potwierdzona - możliwość pokrycia opłaty z portfela.

Komponenty interfejsu: kalendarz dnia i tygodnia z wolnymi oknami, wybór instruktora, pole celu lekcji, podsumowanie kosztu, wybór metody płatności, lista własnych rezerwacji ze statusem.

Akcja główna: zarezerwuj wybrane okno i przejdź do płatności. Gdy portfel pokrywa całość, dominującą akcją jest potwierdź rezerwację.

Akcje drugorzędne: zmień termin lub instruktora, dodaj cel lekcji, ustaw rezerwację cykliczną, przełącz dziecko, odwołaj rezerwację przed terminem.

Akcje niszczące: odwołanie potwierdzonej rezerwacji. Z potwierdzeniem, wyborem powodu i informacją, że zasady zwrotu rozstrzyga polityka organizatora, a środki mogą wrócić do portfela jako kredyt.

Stany ekranu: ładowanie kalendarza; pusty oznacza brak wolnych okien i proponuje obserwowanie albo prośbę o termin; wypełniony to kalendarz z oknami; błąd płatności lub rezerwacji wraca z zachowanym wyborem; sukces potwierdza rezerwację i pokazuje ją w grafiku dziecka; brak uprawnień informuje, że organizator nie włączył lekcji indywidualnych; offline pokazuje ostatnio wczytane okna z banerem, że rezerwacja wymaga sieci.

Walidacja i komunikaty: okno zajęte w trakcie rezerwacji pokazuje to i proponuje inne. Rezerwacja kolidująca z innymi zajęciami dziecka ostrzega o nakładaniu. Odwołanie z bliskim terminem ostrzega o krótkim wyprzedzeniu i skutku dla zwrotu. Cel lekcji z danymi wrażliwymi prosi o ograniczenie do informacji potrzebnej instruktorowi. Nieudana płatność nie rezerwuje okna i wyjaśnia, że okno jest wolne dla innych.

Przypadki brzegowe: rezerwacja w ostatniej chwili tuż przed terminem; lekcja indywidualna nachodząca na zajęcia grupowe dziecka; tor współdzielony przez grupy i lekcje; rezerwacja cykliczna na cały semestr z jedną płatnością albo płatnościami w cyklu; instruktor odwołujący lekcję z poziomu INS-09 z propozycją nowego terminu.

Przejścia i następne ekrany: konfiguracja i ceny pochodzą z OCL-13 i OCL-07, wolne okna z INS-09, płatność rozliczana w PAR-10, cel lekcji widzi instruktor w odczycie w INS-06 i INS-09, kontekst dziecka z PAR-03, postęp po lekcji wraca do PAR-08.

Zdarzenia systemowe: rezerwacja okna na czas płatności, push do instruktora o nowej rezerwacji, synchronizacja z grafikiem grupowym dla wykrycia kolizji, sygnał o odwołaniu przez instruktora z propozycją terminu, zapis kredytu portfela po zwrocie.

Wymagania prawne: cena brutto dla konsumenta, zasady zwrotu w regulaminie organizatora, cel lekcji bez wymuszania danych wrażliwych, dane dziecka u instruktora w zakresie potrzebnym do lekcji pod RODO, zgoda opiekuna dla małoletniego.

Kontekst urządzenia: telefon jako podstawowy, wybór okna jednym dotknięciem. Tablet i desktop do układania rezerwacji cyklicznych.

Lokalizacja i języki: godziny, daty i nazwy dni według regionu, etykiety jako klucze, cel lekcji jako treść rodzica z fallbackiem języka.

Lista kontrolna heurystyk: widoczność stanu systemu przez jasny status rezerwacji; zapobieganie błędom przez wykrycie kolizji z grafikiem dziecka; kontrola i swoboda przez odwołanie z jasnym skutkiem dla zwrotu; zgodność ze światem przez kalendarz wolnych okien znany z rezerwacji wizyt.

Metryki sukcesu: odsetek wolnych okien zamienionych w rezerwacje przez rodziców, odsetek rezerwacji opłaconych w jednym przejściu, liczba kolizji z grafikiem dziecka wykrytych przed potwierdzeniem, udział rezerwacji cyklicznych.

---

## Partia 3 (PAR-13 do PAR-16)

### PAR-13 - paszport sportowy z eksportem PDF

Grupa i rola: rodzic. Konfigurację paszportu ustawia organizator w OCL-17, rodzic eksportuje gotowy dokument.

Moduł: wspolny. Paszport łączy dane z zajęć i z zawodów w jednym dokumencie dziecka.

Cel ekranu jako zadanie użytkownika: chcę zobaczyć i pobrać spójny dokument pokazujący drogę sportową mojego dziecka - umiejętności, frekwencję, osiągnięcia z zawodów - żeby mieć go na pamiątkę, do nowej szkółki albo do trenera.

Punkty wejścia: kafelek paszportu z pulpitu PAR-02, profil dziecka z PAR-03, postępy umiejętności z PAR-08, link z komunikacji organizatora.

Warunki wstępne: skonfigurowany paszport w OCL-17, istnieją dane postępu z INS-05, frekwencji z OCL-11 albo wyniki z modułu zawodów.

Dane wyświetlane: podgląd paszportu z sekcjami włączonymi przez organizatora - umiejętności z drzewa, frekwencja, osiągnięcia i wyniki z zawodów, branding placówki z tokenów marki, zakres dat, wybór języka dokumentu, znacznik kompletności danych.

Komponenty interfejsu: podgląd dokumentu strona po stronie, wybór zakresu dat, wybór języka eksportu, przycisk eksportu PDF, lista wcześniejszych eksportów, przełącznik dziecka.

Akcja główna: eksportuj paszport do PDF. Przed eksportem dominującą akcją jest przejrzyj podgląd.

Akcje drugorzędne: zmień zakres dat, zmień język dokumentu, pobierz wcześniejszy eksport, przełącz dziecko, udostępnij kopię.

Akcje niszczące: nie dotyczy. Eksport tworzy kopię i nie zmienia danych źródłowych.

Stany ekranu: ładowanie podglądu; pusty oznacza brak danych do paszportu i tłumaczy, że pojawią się po pierwszych zajęciach lub starcie; wypełniony to podgląd gotowy do eksportu; błąd generowania PDF pokazuje przyczynę i proponuje ponowienie; sukces potwierdza wygenerowanie i udostępnia plik; brak uprawnień nie dotyczy własnego dziecka; offline pozwala obejrzeć ostatni wygenerowany dokument, świeży eksport wymaga sieci.

Walidacja i komunikaty: paszport bez żadnej sekcji z danymi tłumaczy, że dokument byłby pusty. Sekcja zawodów dla dziecka bez startów wyjaśnia, że pojawi się po pierwszym starcie. Eksport z danymi wrażliwymi prosi o potwierdzenie, że dokument zostaje u opiekuna. Zakres dat bez danych pokazuje, że w tym okresie nie ma wpisów.

Przypadki brzegowe: dziecko obecne tylko w jednym module; zmiana drzewa umiejętności po wcześniejszym eksporcie z notatką o wersji; rodzeństwo z osobnymi paszportami w jednym koncie; eksport w języku innym niż roboczy placówki; paszport łączący dane od dwóch organizatorów z jasnym oznaczeniem źródła.

Przejścia i następne ekrany: konfiguracja pochodzi z OCL-17, dane z drzewa OCL-10 i wpisów INS-05, frekwencja z OCL-11, wyniki z modułu zawodów i klasyfikacji serii, branding z OCL-18, kontekst dziecka z PAR-03, postęp z PAR-08.

Zdarzenia systemowe: generowanie PDF na żądanie, regeneracja podglądu po nowych danych, sygnał o gotowym dokumencie, log eksportu do rozliczalności.

Wymagania prawne: paszport zawiera dane osobowe dziecka i ewentualnie dane wrażliwe, eksport tylko do opiekuna, realizacja prawa do przenoszenia danych pod RODO, branding bez naruszenia praw osób trzecich, dokument od dwóch organizatorów z jasnym wskazaniem źródła każdej sekcji.

Kontekst urządzenia: telefon do szybkiego eksportu i udostępnienia, tablet i desktop do spokojnego przeglądu wielostronicowego dokumentu, podgląd PDF niezależny od urządzenia.

Lokalizacja i języki: dokument generowany w języku wybranym przez rodzica z fallbackiem, daty i jednostki według regionu, etykiety sekcji jako klucze tłumaczeń.

Lista kontrolna heurystyk: spójność i standardy przez jeden paszport dla obu modułów; widoczność stanu systemu przez podgląd przed eksportem; zgodność ze światem przez sekcje czytelne dla rodzica; zapobieganie błędom przez ostrzeżenie o pustym dokumencie.

Metryki sukcesu: odsetek rodzin korzystających z eksportu, kompletność danych w paszporcie, spójność dokumentu między modułami, udział eksportów udostępnionych dalej.

---

### PAR-14 - zgłoszenie się jako wolontariusz

Grupa i rola: rodzic występujący jako kandydat na wolontariusza. Ekran łączy konto rodzinne z rolą wolontariusza, zgłoszenie zatwierdza organizator w OEV-25.

Moduł: wspolny. Most łączy konto rodzinne z modułem zawodów.

Cel ekranu jako zadanie użytkownika: jestem rodzicem na koncie rodzinnym i chcę zgłosić się jako wolontariusz na zawody mojego dziecka, bez zakładania osobnego konta, żeby po akceptacji wejść w panel wolontariusza tym samym logowaniem.

Punkty wejścia: kafelek wolontariatu z pulpitu PAR-02 przy wydarzeniu, na które dziecko jest zapisane, brandowana strona wydarzenia z zaproszeniem dla wolontariuszy z GST-02, link z komunikacji organizatora, skrót z zapisu dziecka na zawody PAR-11.

Warunki wstępne: aktywne konto rodzinne, wydarzenie z otwartym naborem wolontariuszy w OEV-25, zdefiniowane stanowiska i zmiany.

Dane wyświetlane: nazwa wydarzenia i data, opis poszukiwanych stanowisk i zmian z OEV-25, dostępność zmian, zakres roli wolontariusza i informacja o jednorazowym, wygasającym dostępie, dane rodzica dziedziczone z konta w odczycie, zgoda na regulamin wolontariusza, status własnego zgłoszenia.

Komponenty interfejsu: lista stanowisk i zmian z wyborem, pole preferencji i uwag, podgląd zakresu roli, dziedziczone dane kontaktowe rodzica z możliwością korekty, pole zgody na regulamin wolontariusza, przycisk wysłania zgłoszenia, znacznik statusu po wysłaniu.

Akcja główna: wyślij zgłoszenie na wybrane stanowisko i zmianę. Po akceptacji organizatora rola dopina się do konta rodzinnego, a wejście odbywa się mostem w VOL-01.

Akcje drugorzędne: zmień stanowisko lub zmianę, dodaj uwagę dla koordynatora, zapisz wersję roboczą, wycofaj zgłoszenie przed akceptacją.

Akcje niszczące: wycofanie wysłanego zgłoszenia przed akceptacją. Z potwierdzeniem, bo zwalnia zarezerwowane miejsce na zmianie i informuje organizatora.

Stany ekranu: ładowanie listy stanowisk; pusty oznacza brak otwartego naboru i proponuje obserwowanie wydarzenia; wypełniony to formularz wyboru zmiany; błąd wysyłki tłumaczy przyczynę i proponuje ponowienie; sukces potwierdza przyjęcie i wyjaśnia, że zgłoszenie czeka na akceptację; brak uprawnień informuje, że nabór jest zamknięty albo wymaga zaproszenia; offline pozwala przygotować zgłoszenie w kolejce, wysyłka rusza po sieci.

Walidacja i komunikaty: brak wyboru zmiany blokuje wysłanie. Zmiana zapełniona w trakcie pokazuje to i proponuje inną. Brak zgody na regulamin blokuje wysłanie z wyjaśnieniem. Zmiana kolidująca z udziałem dziecka w starcie ostrzega o kolizji, żeby rodzic nie przegapił biegu dziecka. Powtórne zgłoszenie na to samo wydarzenie pokazuje status poprzedniego.

Przypadki brzegowe: rodzic dwojga dzieci startujących w różnych blokach wybierający zmianę między startami; rodzic zgłaszający się na zawody, na które dziecko nie jest zapisane; nabór zamknięty w trakcie wypełniania; rodzic kierowany najpierw do rejestracji w PAR-01, jeśli nie ma konta; akceptacja dopinająca rolę bez tworzenia osobnego konta.

Przejścia i następne ekrany: zgłoszenie trafia do zarządzania wolontariuszami w OEV-25 i odpowiada zgłoszeniu opisanemu w VOL-07, akceptacja dopina rolę i otwiera wejście mostem w VOL-01, status widoczny tu i na pulpicie PAR-02, kolizja z udziałem dziecka odsyła do zapisów w PAR-11.

Zdarzenia systemowe: wysłanie zgłoszenia do organizatora, rezerwacja miejsca na zmianie na czas rozpatrzenia, push lub e-mail o akceptacji albo odrzuceniu, dopięcie roli wolontariusza do konta po akceptacji, log zgłoszeń do audytu organizatora.

Wymagania prawne: dane rodzica dziedziczone z konta pod RODO w zakresie potrzebnym do roli, zgoda na regulamin wolontariusza z zapisaną wersją, jasna informacja o tymczasowości i zakresie dostępu, brak zbierania danych innych kandydatów, powiązanie roli z istniejącym kontem zamiast tworzenia nowego jako realizacja minimalizacji danych.

Kontekst urządzenia: telefon jako podstawowy przy zgłoszeniu w ruchu, tablet i desktop do spokojnego wyboru zmian w domu, bo to ekran planowania, a nie pracy przy bramie.

Lokalizacja i języki: nazwy stanowisk i opis naboru jako treść organizatora z fallbackiem języka, etykiety jako klucze, daty i godziny zmian według regionu wydarzenia.

Lista kontrolna heurystyk: zgodność ze światem przez opis stanowisk zrozumiały dla rodzica bez doświadczenia w wolontariacie; zapobieganie błędom przez ostrzeżenie o kolizji zmiany ze startem dziecka; rozpoznawanie zamiast przypominania przez dziedziczone dane konta; kontrola i swoboda przez wycofanie zgłoszenia przed akceptacją; widoczność stanu systemu przez jasny status zgłoszenia.

Metryki sukcesu: odsetek zgłoszeń zakończonych wysłaniem, odsetek zgłoszeń zaakceptowanych, udział wejść mostem rodzica względem zaproszeń bezpośrednich w VOL-01, liczba kolizji ze startem dziecka wykrytych przed wysłaniem.

---

### PAR-15 - galeria zdjęć z RunPixie

Grupa i rola: rodzic.

Moduł: wspolny. Zdjęcia pochodzą z zawodów, oglądane są z konta rodzinnego.

Cel ekranu jako zadanie użytkownika: chcę szybko znaleźć zdjęcia mojego dziecka z zawodów, rozpoznane po numerze startowym, obejrzeć je i pobrać albo udostępnić, bez przeglądania tysięcy cudzych kadrów.

Punkty wejścia: kafelek galerii z pulpitu PAR-02 po zawodach, potwierdzenie startu, profil dziecka z PAR-03, link z powiadomienia o gotowych zdjęciach, klasyfikacja z wynikiem dziecka.

Warunki wstępne: dziecko miało start z nadanym numerem startowym z OEV-14, integracja RunPixie aktywna dla wydarzenia, zdjęcia otagowane numerem startowym dostępne.

Dane wyświetlane: zdjęcia dziecka dopasowane po numerze startowym, miniatury z datą i wydarzeniem, znacznik jakości dopasowania, opcje pobrania i udostępnienia, informacja o warunkach licencji zdjęć od dostawcy, ewentualne zdjęcia płatne z ceną.

Komponenty interfejsu: siatka miniatur z dużymi celami dotykowymi, podgląd pełnego zdjęcia, przycisk pobrania, przycisk udostępnienia, filtr po wydarzeniu i dziecku, oznaczenie zdjęć z niepewnym dopasowaniem do weryfikacji.

Akcja główna: otwórz zdjęcie i pobierz. Przy zdjęciach płatnych dominującą akcją jest dodaj do koszyka i przejdź do płatności.

Akcje drugorzędne: udostępnij zdjęcie, zgłoś błędne dopasowanie, filtruj po wydarzeniu, przełącz dziecko, pobierz komplet.

Akcje niszczące: nie dotyczy po stronie rodzica. Rodzic nie usuwa zdjęć dostawcy, może zgłosić błędne dopasowanie albo prośbę o usunięcie wizerunku.

Stany ekranu: ładowanie siatki ze szkieletem; pusty oznacza brak zdjęć dla numeru dziecka i tłumaczy, że mogą pojawić się po obróbce przez dostawcę; wypełniony to siatka zdjęć; błąd ładowania z ponowieniem; sukces po pobraniu lub zakupie; brak uprawnień nie dotyczy własnego dziecka; offline pokazuje wcześniej wczytane miniatury, pobranie pełnych plików wymaga sieci.

Walidacja i komunikaty: brak zdjęć tłumaczy, że dopasowanie po numerze trwa albo numer nie został wykryty na kadrach, i proponuje przegląd po czasie startu. Zdjęcie z niepewnym dopasowaniem prosi o potwierdzenie, że to właściwe dziecko, przed pobraniem. Pobranie wymaga akceptacji warunków licencji dostawcy. Zakup zdjęcia płatnego prowadzi przez płatność z jasną ceną.

Przypadki brzegowe: dwoje dzieci rodziny z osobnymi numerami na jednym wydarzeniu; zdjęcie z dwójką dzieci z różnych rodzin z poszanowaniem wizerunku osób trzecich; błędne dopasowanie numeru z możliwością zgłoszenia; numer startowy nieczytelny na kadrze; prośba o usunięcie wizerunku dziecka kierowana do dostawcy i organizatora.

Przejścia i następne ekrany: numery startowe pochodzą z OEV-14, integracja z ustawień organizatora, zakup rozliczany w PAR-10, kontekst dziecka z PAR-03, zdjęcia powiązane ze startem z PAR-11 i klasyfikacją serii.

Zdarzenia systemowe: synchronizacja nowych zdjęć po obróbce dostawcy, push o gotowych zdjęciach dziecka, aktualizacja dopasowań po korekcie numeru, log pobrań i zgłoszeń błędnego dopasowania.

Wymagania prawne: wizerunek dziecka jako dane osobowe pod RODO, zgoda na publikację i udostępnianie zdjęć, poszanowanie wizerunku osób trzecich na wspólnych kadrach, warunki licencji dostawcy RunPixie, prawo do żądania usunięcia wizerunku, zakup zdjęć jako transakcja z jasną ceną i licencją.

Kontekst urządzenia: telefon jako podstawowy, siatka i podgląd zoptymalizowane pod dotyk, pobranie i udostępnienie jednym gestem. Tablet i desktop do przeglądu większej liczby kadrów.

Lokalizacja i języki: etykiety jako klucze, daty i nazwy wydarzeń jako treść z fallbackiem języka, ceny zdjęć płatnych według regionu.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania przez dopasowanie po numerze zamiast ręcznego szukania; widoczność stanu systemu przez znacznik jakości dopasowania; zapobieganie błędom przez potwierdzenie niepewnego dopasowania przed pobraniem; kontrola i swoboda przez zgłoszenie błędnego dopasowania i prośbę o usunięcie wizerunku.

Metryki sukcesu: odsetek rodzin znajdujących zdjęcia dziecka po numerze, czas od startu do dostępnych zdjęć, odsetek zdjęć z poprawnym dopasowaniem, udział zdjęć pobranych lub zakupionych, liczba zgłoszeń błędnego dopasowania.

---

### PAR-16 - ustawienia konta, zgody RODO i powiadomienia

Grupa i rola: rodzic. Właściciel konta rodzinnego zarządza danymi rodziny, zgodami i dostępem drugiego opiekuna.

Moduł: wspolny.

Cel ekranu jako zadanie użytkownika: chcę w jednym miejscu zarządzać kontem rodziny - danymi, zgodami RODO, kanałami powiadomień, językiem, dostępem drugiego opiekuna - i mieć kontrolę nad tym, co i jak system o nas przetwarza.

Punkty wejścia: menu konta z dowolnego ekranu, link z pulpitu PAR-02, prośba o zgodę przy zapisie, link z e-maila o zmianie polityki, profil dziecka z PAR-03.

Warunki wstępne: aktywne konto rodzinne.

Dane wyświetlane: dane kontaktowe rodzica, lista profili dzieci z dostępem, zgody RODO z wersją i datą akceptacji, ustawienia kanałów powiadomień push, e-mail i SMS dla różnych typów zdarzeń, język i region konta, dostęp drugiego opiekuna z zakresem, historia logowań, sekcja pobrania danych i usunięcia konta.

Komponenty interfejsu: formularz danych kontaktowych, lista zgód z przełącznikami i odnośnikiem do treści, macierz powiadomień typ zdarzenia względem kanału, wybór języka i regionu, zarządzanie zaproszeniem drugiego opiekuna, przyciski eksportu danych i usunięcia konta z osobnym potwierdzeniem.

Akcja główna: zapisz ustawienia. W sekcji zgód akcją wiodącą jest świadoma zmiana zgody z jasnym skutkiem.

Akcje drugorzędne: zmień język i region, ustaw kanały powiadomień, zaproś drugiego opiekuna, pobierz swoje dane, przejrzyj historię logowań.

Akcje niszczące: usuń konto rodzinne oraz odbierz dostęp drugiemu opiekunowi. Usunięcie konta wymaga wyraźnego potwierdzenia i wyjaśnia skutek dla aktywnych zapisów, salda portfela i paszportów dzieci, z informacją o danych zachowywanych przez organizatora dla rozliczeń zgodnie z prawem. Cofnięcie zgody wymaganej do usługi tłumaczy, że może uniemożliwić udział dziecka.

Stany ekranu: ładowanie; pusty nie dotyczy, konto zawsze ma dane; wypełniony to pełne ustawienia; błąd zapisu przy danej sekcji; sukces z tostem; brak uprawnień dotyczy drugiego opiekuna o ograniczonym zakresie, który nie zmienia ustawień właściciela; offline pozwala przejrzeć ustawienia z danych lokalnych, zmiany wymagają sieci.

Walidacja i komunikaty: adres lub telefon w złym formacie prosi o korektę. Wyłączenie kanału powiadomień dla zdarzeń krytycznych, jak oferta z listy rezerwowej, ostrzega, że rodzic może przegapić ważne sprawy. Cofnięcie zgody wymaganej do usługi pokazuje, na co wpłynie. Usunięcie konta z aktywnymi zapisami albo dodatnim saldem portfela ostrzega i proponuje najpierw rozliczenie. Odebranie dostępu jedynemu drugiemu opiekunowi pyta o potwierdzenie.

Przypadki brzegowe: dwoje rozdzielonych opiekunów współdzielących dzieci z różnym zakresem dostępu; rodzic chcący usunąć konto przy trwającym abonamencie i niewykorzystanym kredycie portfela; zmiana regionu zmieniająca format kwot i dat w całym koncie; rodzic z preferencją języka innego niż domyślny organizatora; eksport danych obejmujący dane kilkorga dzieci.

Przejścia i następne ekrany: profile dzieci łączą z PAR-03, zgody dla małoletniego z KID-01, kanały powiadomień rzutują na push w PAR-07 i całym panelu, dane i faktury z PAR-10, dostęp drugiego opiekuna wpływa na widoczność profili, polityki prawne ze stron prawnych GST-08.

Zdarzenia systemowe: zapis zmiany zgody z wersją i datą do rozliczalności, propagacja preferencji powiadomień do systemu wysyłki, zaproszenie drugiego opiekuna e-mailem, przygotowanie paczki danych do pobrania, log zmian ustawień i dostępu.

Wymagania prawne: zgody RODO z wersją, datą i możliwością wycofania, prawo dostępu do danych i prawo do przenoszenia przez eksport, prawo do usunięcia z wyjaśnieniem danych zachowywanych dla obowiązków prawnych organizatora, dane dzieci przetwarzane przez opiekuna, kontakt tylko kanałem objętym zgodą, dostęp drugiego opiekuna jako kontrolowane współdzielenie danych rodziny. Zmiana dostępu i usunięcie konta pozostają świadomą decyzją rodzica wykonywaną w panelu, nie automatem.

Kontekst urządzenia: telefon do szybkiej zmiany powiadomień i zgód, tablet i desktop do spokojnego przeglądu danych, eksportu i zarządzania dostępem.

Lokalizacja i języki: etykiety jako klucze, treść zgód i polityk z fallbackiem języka, język i region konta ustawiane tutaj wraz z językiem zapasowym, format kwot, dat i telefonu według regionu.

Lista kontrolna heurystyk: kontrola i swoboda przez świadome zarządzanie zgodami i dostępem; widoczność stanu systemu przez wersję i datę każdej zgody oraz historię logowań; zapobieganie błędom przez ostrzeżenie o wyłączeniu krytycznych powiadomień i o skutkach usunięcia konta; pomoc i dokumentacja przez odnośniki do treści zgód i polityk; zgodność ze światem przez język praw rodzica, nie żargon prawny bazy.

Metryki sukcesu: odsetek kont z aktualnymi zgodami, udział rodziców korzystających z eksportu danych, liczba przegapionych zdarzeń krytycznych po wyłączeniu kanału, odsetek rodzin z drugim opiekunem o poprawnie ustawionym zakresie, liczba zgłoszeń pomocy dotyczących ustawień.

---

## Literatura

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

W3C. (2018). *Web Content Accessibility Guidelines (WCAG) 2.1*. World Wide Web Consortium. https://www.w3.org/TR/WCAG21/
