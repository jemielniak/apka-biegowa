# Specyfikacja ekranów grupy ATH - panel zawodnika, moduł zawodów

Grupa ATH to panel dorosłego zawodnika, który zapisuje się sam. Persona biega amatorsko, ma kilka startów w sezonie i zna swój numer buta lepiej niż swój numer startowy. Pracuje na dwa sposoby. Pierwszy to telefon w biegu - dosłownie, bo zapis często domyka się w tramwaju albo wieczorem na kanapie, jedną ręką. Drugi to desktop przy gadżetach, gdy zawodnik siada spokojnie z zegarkiem sportowym, czujnikiem tętna i kubkiem kawy, żeby obejrzeć wynik i zdjęcia. Z tego profilu wynika kilka decyzji przekrojowych dla całej grupy.

Po pierwsze, ciężar produktu leży nie w samym zapisie, lecz w tym, co dzieje się po nim. Zawodnik chce zmienić rozmiar koszulki, przepisać pakiet koledze, przełożyć start na przyszły rok albo wskoczyć z listy rezerwowej - i chce zrobić to sam, o dwudziestej trzeciej, bez pisania maila do organizatora. Samoobsługa zgłoszenia jest więc sercem grupy, a nie dodatkiem. Po drugie, numer startowy to rzecz święta i pojawia się dopiero po zaksięgowaniu wpłaty. Przed płatnością zawodnik ma zgłoszenie, nie ma numeru. Ta zasada przewija się przez połowę ekranów i porządkuje całą logikę galerii zdjęć, list startowych i klasyfikacji. Po trzecie, dosprzedaż gadżetów ma być uczciwa, nie nachalna. Proponujemy to, co pasuje do dystansu i pogody, pokazujemy stan magazynu zamiast sztucznej presji, i nigdy nie blokujemy zapisu sklepem.

Konto zawodnika jest tym samym kontem rodzinnym, które opisuje grupa PAR. Jeden login obsługuje oba moduły, ten sam portfel i ten sam paszport sportowy. Dorosły, który sam biega i jednocześnie wozi dziecko na trening, nie zakłada drugiego konta - przełącza kontekst. Dlatego część ekranów ATH to widok zawodnika na tych samych danych, które rodzic ogląda w PAR, tyle że z perspektywy własnego startu, nie startu dziecka.

Konwencje wspólne dla całej grupy, żeby nie powtarzać ich w każdym polu:

- Napisy widoczne dla użytkownika to klucze tłumaczeń ze znacznikami BCP 47, domyślnie pl-PL i en-GB. Daty, kwoty i numery telefonu formatuje się według ustawień regionalnych konta. Przełącznik języka stoi w nagłówku panelu, język zapasowy to en-GB przy braku tłumaczenia. Układ RTL włącza się automatycznie, jeśli aktywny język jest pisany od prawej do lewej.
- Tokeny marki pochodzą wyłącznie ze zmiennych CSS: `--brand-ink` i `--accent`, kolory semantyczne dla sukcesu, ostrzeżenia, błędu oraz informacji, para krojów (nagłówkowy wyróżniający i tekstowy czytelny, z dala od Inter i Arial), skala odstępów na jednej jednostce bazowej, jeden promień zaokrąglenia, jeden poziom cienia. Minimalny cel dotykowy i kontrast tekstu spełniają WCAG 2.1 na poziomie AA (W3C, 2018). Panel zawodnika jest brandowany barwami organizatora wydarzenia, którego dotyczy dany start, a przy startach u kilku organizatorów marka zmienia się wraz z wybranym wydarzeniem.
- Lista kontrolna heurystyk odnosi się do dziesięciu heurystyk użyteczności (Nielsen, 1994).
- Konto jest rodzinne i wspólne dla obu modułów, zgodnie z grupą PAR. Ten sam zawodnik bywa też rodzicem zapisującym dziecko, więc panel ATH dzieli z PAR portfel, paszport i ustawienia konta. Portfel gromadzi nadpłaty i zwroty jako kredyt u tego samego organizatora i nie wypłaca się ich na zewnątrz bez osobnej procedury organizatora.
- Numer startowy nadaje się dopiero po zaksięgowaniu wpłaty. Do tego czasu zawodnik ma zgłoszenie ze statusem płatności, nie numer. Galeria RunPixie, listy startowe i klasyfikacja opierają się na numerze, więc działają dla zawodnika dopiero po opłaceniu.
- Operacje na zgłoszeniu - transfer do innej osoby, odroczenie na kolejną edycję, wejście i zejście z listy rezerwowej oraz wymiana numeru lub pakietu - działają samoobsługowo w granicach reguł organizatora ustawionych w OEV-16 i OEV-17. Bywają darmowe albo płatne, a opłata za operację jest przychodem organizatora, nie opłatą platformy.
- Model płatności jest oderwany od liczby użytkowników. Cena, którą widzi zawodnik, to cena startu ustalona przez organizatora w progach cenowych OEV-10, a nie opłata od główki naliczana przez platformę.
- Urządzeniem podstawowym jest telefon, w układzie pionowym, do obsługi jedną ręką. Desktop jest wygodniejszym przypadkiem do spokojnego przeglądu wyniku, zdjęć i klasyfikacji przy gadżetach sportowych, a nie układem wiodącym.
- Część zadań zawodnik wykonuje na starcie, w strefie zawodów, gdzie zasięg bywa zerwany przez tłum. Pokazanie kodu odbioru pakietu, numeru startowego i podstawowych danych startu działa wtedy z ostatnio wczytanych danych, a zmiany czekają w kolejce lokalnej do powrotu sieci.

---

## Partia 1 (ATH-01 do ATH-06)

### ATH-01 - rejestracja i logowanie

Grupa i rola: zawodnik. Konto jest rodzinne i wspólne dla obu modułów, więc to samo logowanie obsługuje start dorosłego i, w roli rodzica, zapis dziecka.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę wejść na konto szybko, najlepiej bez wpisywania hasła, albo założyć je w trakcie zapisu na bieg, tak żeby zacząć wypełniać zgłoszenie i nie utracić wybranego dystansu.

Punkty wejścia: brandowana strona wydarzenia z GST-02, lista dystansów z GST-04, ekran rozpoczęcia zapisu z GST-05, link z zaproszenia organizatora, link z powiadomienia push lub e-mail o otwarciu zapisów albo o ofercie z listy rezerwowej, ikona aplikacji przy ponownym wejściu.

Warunki wstępne: dla rejestracji żadne, konto zakłada sam zawodnik. Dla logowania istniejące konto. Jeśli zawodnik trafił tu z trwającego zapisu, wybrany dystans i ewentualny próg cenowy czekają w koszyku przypiętym do sesji.

Dane wyświetlane: logo i marka organizatora kontekstu z tokenów, pole adresu e-mail lub telefonu, opcja logowania bez hasła linkiem lub kodem, logowanie przez dostawcę tożsamości, przełącznik języka, krótka nota, że jedno konto obsłuży starty i ewentualny zapis dziecka, informacja o ostatnim udanym logowaniu po wejściu.

Komponenty interfejsu: formularz z dużymi celami dotykowymi, przycisk logowania bez hasła, przyciski logowania przez dostawcę tożsamości, opcja zapamiętania urządzenia z biometrią, przełącznik języka w nagłówku, odnośnik do pomocy i do regulaminu.

Akcja główna: zaloguj się lub załóż konto. Na urządzeniu zapamiętanym dominującą akcją jest potwierdzenie biometryczne.

Akcje drugorzędne: zmień język, poproś o nowy link logowania, odzyskaj dostęp, otwórz pomoc.

Akcje niszczące: nie dotyczy. Usunięcie konta odbywa się z ustawień ATH-14, nie z ekranu wejścia.

Stany ekranu: ładowanie pokazuje wygaszony formularz; pusty nie dotyczy, formularz jest zawsze wypełnialny; wypełniony to gotowy formularz; błąd logowania tłumaczy, czy problem dotyczy adresu, wygasłego linku, czy zbyt wielu prób, i podaje następny krok; sukces przenosi do ATH-02 albo wraca do przerwanego zapisu w ATH-03, jeśli zawodnik logował się w jego trakcie; brak uprawnień nie dotyczy nowego konta; offline pokazuje, że pierwsze wejście wymaga sieci.

Walidacja i komunikaty: pusty adres prosi o uzupełnienie. Adres w złym formacie tłumaczy, czego brakuje. Wygasły link logowania proponuje wysłanie nowego i mówi, na jaki adres trafi. Zbyt wiele prób uruchamia chwilową blokadę z informacją, ile czasu pozostało. Próba rejestracji na adres już istniejący proponuje logowanie albo odzyskanie dostępu zamiast tworzenia duplikatu.

Przypadki brzegowe: zawodnik, który zaczął zapis jako gość i teraz zakłada konto, z przeniesieniem wybranego dystansu z koszyka; osoba mająca już konto jako rodzic w PAR, która pierwszy raz zapisuje się sama na bieg na tym samym koncie; zawodnik wracający po roku, którego dane kontaktowe się zmieniły; logowanie tuż przed zamknięciem progu cenowego, gdzie cena może wzrosnąć w trakcie wejścia.

Przejścia i następne ekrany: udane wejście prowadzi do ATH-02 albo do przerwanego zapisu w ATH-03. Rejestracja z poziomu zapisu wraca do tego zapisu z zachowanym dystansem. Pierwsze logowanie prowadzi przez krótką akceptację regulaminu i polityki prywatności z zapisaną wersją dokumentu.

Zdarzenia systemowe: wygaśnięcie sesji po czasie bezczynności, unieważnienie linku po użyciu, log udanych i nieudanych prób, zachowanie koszyka zapisu przypiętego do sesji do czasu zalogowania.

Wymagania prawne: dane logowania jako dane osobowe pod RODO, zgoda na regulamin i politykę prywatności z zapisaną wersją przy zakładaniu konta, minimalizacja danych na ekranie wejścia. Zawodnik zakłada konto sam, w odróżnieniu od instruktora i wolontariusza, których konta tworzy organizator.

Kontekst urządzenia: telefon jako podstawowy, biometria tam, gdzie urządzenie ją wspiera. Desktop jako wygodniejszy do pierwszej, spokojnej konfiguracji konta.

Lokalizacja i języki: etykiety jako klucze tłumaczeń, lista języków zależna od organizatora kontekstu, język zapasowy en-GB.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania przez logowanie bez hasła i zapamiętane urządzenie; pomoc w diagnozie błędu przez komunikat wskazujący przyczynę i krok naprawczy; widoczność stanu systemu przez informację o ostatnim logowaniu; minimalistyczny projekt przez jedną dominującą akcję; zapobieganie błędom przez wykrycie istniejącego konta zamiast cichego duplikatu.

Metryki sukcesu: odsetek logowań bez hasła, czas od otwarcia do pulpitu lub powrotu do zapisu, odsetek zapisów dokończonych po założeniu konta w ich trakcie, liczba duplikatów kont wykrytych przy rejestracji.

---

### ATH-02 - dashboard z moimi zapisami

Grupa i rola: zawodnik.

Moduł: zawody. Pulpit pokazuje starty zawodnika; sprawy dziecka, jeśli konto pełni też rolę rodzica, żyją w panelu PAR.

Cel ekranu jako zadanie użytkownika: w kilkanaście sekund chcę wiedzieć, co mnie czeka - który start jest najbliżej, czy mam już numer, czy trzeba coś dopłacić albo uzupełnić zgodę - i jednym dotknięciem przejść do sprawy, która wymaga reakcji teraz.

Punkty wejścia: udane logowanie z ATH-01, link z powiadomienia push lub e-mail, powrót z dowolnego ekranu szczegółowego, ikona aplikacji.

Warunki wstępne: aktywne konto z przynajmniej jednym zgłoszeniem. Konto bez zgłoszeń pokazuje stan pusty z zaproszeniem do przeglądania wydarzeń.

Dane wyświetlane: lista moich startów z nazwą wydarzenia, datą, dystansem i statusem zgłoszenia (oczekuje na płatność, opłacone, na liście rezerwowej, przeniesione, odroczone), numer startowy widoczny dopiero po opłaceniu, alerty wymagające reakcji w kolejności pilności - zaległa płatność, brakująca zgoda, oferta z listy rezerwowej z odliczaniem, ostrzeżenie pogodowe z ATH-13, saldo portfela jako kredyt u organizatora, odliczanie do najbliższego startu. Kwoty według regionu konta.

Komponenty interfejsu: kafelki nadchodzących startów ze statusem i numerem, lista alertów z priorytetem i akcją wprost na kafelku, skrót do galerii zdjęć po ostatnim biegu, skrót do portfela, link do publicznej klasyfikacji serii, wskaźnik nieopłaconego zgłoszenia z czasem rezerwacji miejsca.

Akcja główna: przejdź do najpilniejszej sprawy. Najczęściej jest to dokończenie nieopłaconego zgłoszenia albo decyzja w ofercie z listy rezerwowej.

Akcje drugorzędne: otwórz szczegóły startu, przeglądaj inne wydarzenia, otwórz portfel, otwórz galerię, otwórz klasyfikację.

Akcje niszczące: nie dotyczy. Rezygnacja ze startu i zwrot prowadzą przez samoobsługę w ATH-08 z osobnym potwierdzeniem.

Stany ekranu: ładowanie pokazuje szkielety kafelków; pusty wita zawodnika bez startów i kieruje do katalogu wydarzeń z GST-01; wypełniony pokazuje pełną listę zgłoszeń; błąd ładowania danego kafelka dotyczy tylko jego, z przyciskiem ponów; sukces nie dotyczy osobno; brak uprawnień nie dotyczy własnych zgłoszeń; offline pokazuje ostatnio wczytany pulpit z banerem o wstrzymaniu zmian i datą ostatniej synchronizacji, z numerem startowym dostępnym lokalnie.

Walidacja i komunikaty: nieudane pobranie fragmentu danych mówi, że nie udało się pobrać tego kafelka, i proponuje ponowienie, bez blokowania całego ekranu. Nieopłacone zgłoszenie pokazuje, ile czasu zostało na płatność, zanim miejsce wróci do puli. Oferta z listy rezerwowej pokazuje okno decyzji z odliczaniem, żeby zawodnik nie przegapił szansy.

Przypadki brzegowe: zawodnik z kilkoma startami u różnych organizatorów, gdzie pulpit grupuje sprawy po pilności, nie po organizatorze; start opłacony, ale z brakującą zgodą blokującą wydanie pakietu; zgłoszenie przeniesione od innej osoby, które dopiero czeka na uzupełnienie danych; nakładające się starty w jeden weekend z ostrzeżeniem o kolizji.

Przejścia i następne ekrany: alert płatności prowadzi do ATH-06, oferta rezerwowa do ATH-08, brak zgody do ATH-05, kafelek startu do ATH-08, skrót zdjęć do ATH-11, klasyfikacja do ATH-12, portfel do ATH-10, ostrzeżenie pogodowe do ATH-13.

Zdarzenia systemowe: odświeżanie statusów po wejściu i co kilka minut, push o ofercie z listy rezerwowej, push o nadaniu numeru startowego po zaksięgowaniu wpłaty, sygnał o nowych zdjęciach z RunPixie, sygnał o ostrzeżeniu pogodowym.

Wymagania prawne: pulpit pokazuje dane zagregowane, dane wrażliwe i pełne dane startu pojawiają się po wejściu w szczegóły, zgodność z RODO przez minimalizację danych na widoku startowym i przez to, że zawodnik widzi wyłącznie własne zgłoszenia.

Kontekst urządzenia: telefon jako podstawowy, jedna kolumna kafelków uporządkowana po pilności. Desktop pokazuje szerszy przegląd kilku startów obok siebie.

Lokalizacja i języki: etykiety jako klucze tłumaczeń, kwoty i daty według regionu, nazwy wydarzeń jako treść organizatora z fallbackiem języka.

Lista kontrolna heurystyk: widoczność stanu systemu przez status każdego zgłoszenia i znacznik synchronizacji; rozpoznawanie zamiast przypominania, bo alerty mówią, co zrobić; minimalistyczny projekt przez ograniczenie startu do spraw pilnych; zgodność ze światem przez język zawodnika, nie żargon bazy; pomoc w diagnozie przez bezpośrednie linki do źródła sprawy.

Metryki sukcesu: czas od wejścia do pierwszej sensownej akcji, odsetek nieopłaconych zgłoszeń dokończonych tego samego dnia, odsetek przegapionych okien ofert rezerwowych, udział startów z kompletem zgód przed dniem zawodów.

---

### ATH-03 - formularz zapisu z polami warunkowymi

Grupa i rola: zawodnik.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę wypełnić zgłoszenie na wybrany dystans bez przeklikiwania się przez pola, które mnie nie dotyczą, i bez wpisywania danych, które system już o mnie wie, tak żeby przejść do płatności w jednym ciągu.

Punkty wejścia: lista dystansów z GST-04, ekran rozpoczęcia zapisu z GST-05, kafelek wydarzenia z ATH-02, powrót po zalogowaniu z ATH-01 z dystansem w koszyku, oferta z listy rezerwowej z ATH-08, link z powiadomienia o otwarciu zapisów.

Warunki wstępne: opublikowane wydarzenie z dystansami i kategoriami z OEV-03, aktywny próg cenowy z OEV-10, skonfigurowany formularz z konstruktora OEV-08, zalogowane konto albo zapis jako gość z możliwością założenia konta przy finalizacji.

Dane wyświetlane: wybrany dystans i kategoria, cena z aktywnego progu z OEV-10 z licznikiem miejsc lub czasu do zmiany ceny, pola formularza z OEV-08 z danymi już znanymi z konta wstępnie wypełnionymi, pola warunkowe pojawiające się zależnie od dystansu, kategorii i wcześniejszych odpowiedzi, wymagane dokumenty i oświadczenia, możliwość przypisania zgłoszenia do siebie lub do innej osoby w granicach reguł organizatora.

Komponenty interfejsu: formularz krokowy z dużymi celami dotykowymi i wyraźnym wskaźnikiem postępu, pola warunkowe odsłaniane progresywnie zamiast pokazywane i wyszarzane (Wroblewski, 2008), podsumowanie wyboru dystansu i ceny przypięte w widoku, wybór kategorii z automatyczną podpowiedzią z daty urodzenia, sekcja danych kontaktu alarmowego, walidacja w locie przy polach krytycznych.

Akcja główna: przejdź do dodatków i gadżetów. Formularz prowadzi liniowo do ATH-04, a stamtąd przez zgody do płatności.

Akcje drugorzędne: zmień dystans, zapisz zgłoszenie jako wersję roboczą, wróć do listy dystansów, zmień kategorię ręcznie.

Akcje niszczące: nie dotyczy na etapie wypełniania. Porzucenie formularza nie tworzy zobowiązania, a wersja robocza zachowuje wpisane dane.

Stany ekranu: ładowanie formularza ze szkieletem pól; pusty oznacza brak dostępnego dystansu albo zamknięte zapisy i tłumaczy przyczynę z propozycją listy rezerwowej; wypełniony to formularz w trakcie; błąd zapisu wersji roboczej tłumaczy przyczynę i zachowuje dane; sukces przenosi do ATH-04; brak uprawnień nie dotyczy; offline pozwala wypełniać formularz lokalnie, a wysłanie i rezerwacja miejsca wymagają sieci.

Walidacja i komunikaty: pole wymagane puste prosi o uzupełnienie i wskazuje, którego dotyczy. Data urodzenia spoza zakresu dystansu tłumaczy, że dystans ma ograniczenie wiekowe, i proponuje pasujący dystans, zamiast cicho odrzucać. Kategoria niespójna z płcią lub wiekiem pokazuje rozbieżność i pyta, co poprawić. Próg cenowy zmieniony w trakcie wypełniania pokazuje nową cenę przed przejściem dalej, żeby zawodnik nie zobaczył innej kwoty dopiero przy płatności. Limit miejsc wyczerpany w trakcie kieruje do listy rezerwowej w ATH-08 zamiast pozwalać dokończyć zapis, którego nie da się opłacić.

Przypadki brzegowe: zawodnik zapisujący się na sztafetę, gdzie formularz zbiera dane kilku osób; zapis przypisany do innej osoby w ramach reguł transferu, gdzie część pól wypełnia później osoba docelowa; pola warunkowe zależne od deklaracji niepełnosprawności lub klasy startowej; zawodnik wracający do wersji roboczej po zmianie progu cenowego; zapis tuż przed zamknięciem zapisów z krótkim oknem rezerwacji.

Przejścia i następne ekrany: dystanse i kategorie pochodzą z OEV-03, układ pól z OEV-08, cena z OEV-10, sukces prowadzi do ATH-04, wyczerpanie miejsc do listy rezerwowej w ATH-08, wymagane dokumenty do sekcji zgód w ATH-05.

Zdarzenia systemowe: rezerwacja miejsca na czas finalizacji zapisu, zwolnienie rezerwacji po wygaśnięciu, zapis wersji roboczej, aktualizacja licznika miejsc w OEV-10 i listach startowych, sygnał o zmianie progu cenowego w trakcie sesji.

Wymagania prawne: ceny brutto widoczne dla konsumenta, jasne ograniczenia wiekowe i regulaminowe dystansu, dane kontaktu alarmowego i ewentualne dane zdrowotne jako szczególna kategoria za osobną zgodą i osobnym wejściem, minimalizacja pól do tych naprawdę potrzebnych do startu, prawo odstąpienia i jego ograniczenia komunikowane przy usłudze terminowej.

Kontekst urządzenia: telefon jako podstawowy, formularz krokowy do obsługi jedną ręką, pola odsłaniane progresywnie, żeby nie przytłaczać małego ekranu. Desktop do spokojnego wypełnienia dłuższego formularza sztafety.

Lokalizacja i języki: etykiety i pomoc kontekstowa jako klucze tłumaczeń, nazwy dystansów i kategorii jako treść organizatora, format daty i kwoty według regionu.

Lista kontrolna heurystyk: zapobieganie błędom przez progresywne odsłanianie i walidację w locie; zgodność ze światem przez język zawodnika i opis dystansu; rozpoznawanie zamiast przypominania przez wstępne wypełnienie danych z konta; widoczność stanu systemu przez wskaźnik postępu i licznik miejsc; pomoc w diagnozie przez komunikat wskazujący konkretne pole i poprawkę.

Metryki sukcesu: odsetek zgłoszeń ukończonych w jednym przejściu, odsetek porzuceń na poszczególnych krokach formularza, średnia liczba błędów walidacji na zgłoszenie, udział zgłoszeń z poprawnie dobraną kategorią bez ręcznej korekty.

---

### ATH-04 - dodatki i gadżety

Grupa i rola: zawodnik.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę dorzucić do zgłoszenia to, co rzeczywiście mi się przyda - koszulkę w moim rozmiarze, medal pamiątkowy, miejsce w depozycie, transport - bez poczucia, że ktoś mnie do tego przyciska, i widząc od razu, jak rośnie kwota do zapłaty.

Punkty wejścia: przejście z formularza zapisu ATH-03, powrót z podsumowania ATH-06 w celu zmiany dodatku, dosprzedaż po zapisie z ATH-09, kafelek startu z ATH-02.

Warunki wstępne: wybrany dystans z ATH-03, skonfigurowany magazyn gadżetów z OEV-11 z rozmiarami, wariantami i stanami, ceny dodatków przypisane przez organizatora.

Dane wyświetlane: lista dodatków z OEV-11 z ceną, wariantem i rozmiarem, dostępność każdego wariantu jako realny stan magazynu zamiast sztucznej presji, tabela rozmiarów z miarami, sugestie dopasowane do dystansu i prognozy pogody z ATH-13 opisane jako podpowiedź, a nie domyślny wybór, bieżąca suma zgłoszenia z rozbiciem na wpisowe i dodatki, możliwe pokrycie części kwoty z portfela.

Komponenty interfejsu: karty produktów z wyborem wariantu i rozmiaru, wskaźnik niskiego stanu pokazujący realną liczbę sztuk, tabela rozmiarów w modalu, sekcja sugestii oznaczona jako podpowiedź, podsumowanie kwoty przypięte w widoku, przełącznik pokrycia z portfela.

Akcja główna: przejdź do zgód i regulaminów. Dodatki są opcjonalne, więc akcja prowadzi dalej także przy pustym koszyku gadżetów.

Akcje drugorzędne: dodaj lub usuń gadżet, zmień rozmiar, otwórz tabelę rozmiarów, pomiń dodatki, dopłać z portfela.

Akcje niszczące: usuń dodatek z koszyka. Operacja odwracalna do czasu płatności, bez osobnego ostrzeżenia.

Stany ekranu: ładowanie listy gadżetów ze szkieletem kart; pusty oznacza brak dodatków w ofercie i prowadzi wprost do zgód; wypełniony to katalog z dostępnością; błąd pobrania stanu magazynu pokazuje, że dostępność może być nieaktualna, i proponuje odświeżenie; sukces przenosi do ATH-05; brak uprawnień nie dotyczy; offline pozwala przejrzeć katalog z danych lokalnych, a rezerwacja gadżetu i płatność wymagają sieci.

Walidacja i komunikaty: rozmiar niewybrany przy produkcie z rozmiarami prosi o wybór przed dodaniem. Wariant niedostępny pokazuje to wprost i proponuje najbliższy dostępny rozmiar zamiast pozwalać dodać brakujący towar. Ostatnia sztuka w koszyku innego zawodnika może zniknąć w trakcie, więc komunikat tłumaczy, że gadżet zniknął z dostępności, i nie obciąża za niego. Kwota dodatków przekraczająca saldo portfela pokazuje, ile trzeba dopłacić inną metodą.

Przypadki brzegowe: zawodnik dobierający rozmiar między dwoma wartościami, któremu pomaga tabela miar; gadżet limitowany z twardym limitem na osobę; pakiet łączony, gdzie koszulka jest w cenie wpisowego, a dodatkowa płatna; zmiana rozmiaru po opłaceniu obsługiwana później przez wymianę w ATH-08; wyprzedanie ostatniej sztuki dokładnie w momencie dodania.

Przejścia i następne ekrany: katalog i stany pochodzą z OEV-11, sugestie pogodowe z ATH-13, sukces prowadzi do zgód w ATH-05, dosprzedaż po zapisie do ATH-09, pokrycie z portfela rozlicza się w ATH-06 i ATH-10.

Zdarzenia systemowe: rezerwacja wybranego wariantu na czas finalizacji, zwolnienie po wygaśnięciu rezerwacji, aktualizacja stanu magazynu w OEV-11, przeliczenie sumy zgłoszenia po każdej zmianie.

Wymagania prawne: ceny brutto dodatków widoczne dla konsumenta, jasna informacja, co jest w cenie wpisowego, a co płatne osobno, prawo odstąpienia dla towaru oddzielone od usługi startu, brak ukrytych opłat i brak domyślnie zaznaczonych płatnych dodatków.

Kontekst urządzenia: telefon jako podstawowy, karty produktów w jednej kolumnie z dużym celem dotykowym. Desktop przy gadżetach do spokojnego porównania wariantów i tabeli rozmiarów.

Lokalizacja i języki: nazwy i opisy gadżetów jako treść organizatora, etykiety jako klucze, rozmiary i miary z jednostkami według regionu, kwoty i waluta lokalnie.

Lista kontrolna heurystyk: widoczność stanu systemu przez realny stan magazynu i bieżącą sumę; zapobieganie błędom przez wymóg wyboru rozmiaru i blokadę dodania niedostępnego wariantu; kontrola i swoboda przez łatwe usunięcie dodatku; zgodność ze światem przez tabelę rozmiarów z miarami; minimalistyczny projekt przez sugestie opisane jako podpowiedź, a nie narzucony wybór.

Metryki sukcesu: odsetek zgłoszeń z dodatkiem, udział poprawnie dobranych rozmiarów mierzony liczbą późniejszych wymian, średnia wartość dodatków na zgłoszenie, odsetek dodań przerwanych brakiem dostępności.

---

### ATH-05 - zgody i regulaminy

Grupa i rola: zawodnik.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę świadomie zaakceptować to, co jest naprawdę wymagane do startu, oddzielić to od zgód dobrowolnych, i nie utknąć przy długim regulaminie, którego nie da się przewinąć na telefonie.

Punkty wejścia: przejście z dodatków ATH-04, powrót z podsumowania ATH-06 w celu uzupełnienia zgody, alert braku zgody z ATH-02, ponowna akceptacja po zmianie wersji regulaminu.

Warunki wstępne: skonfigurowane oświadczenia i zgody wydarzenia z kreatora OEV-07, dostępne treści regulaminu, polityki prywatności i ewentualnego oświadczenia zdrowotnego z aktualną wersją.

Dane wyświetlane: lista zgód podzielona na wymagane do startu i dobrowolne, krótkie streszczenie każdej zgody z odnośnikiem do pełnej treści, oświadczenie o stanie zdrowia i zdolności do startu jeśli wymaga go organizator, zgoda na wykorzystanie wizerunku ze zdjęć i transmisji jako zgoda osobna i dobrowolna, wersja i data każdego dokumentu.

Komponenty interfejsu: lista zgód z przełącznikami i wyraźnym oznaczeniem wymaganych, odnośniki do pełnych treści w modalu z możliwością przewinięcia i pobrania, sekcja oświadczenia zdrowotnego za osobnym wejściem, wskaźnik, ile zgód wymaganych pozostało do zaznaczenia.

Akcja główna: zaakceptuj i przejdź do płatności. Akcja jest aktywna dopiero po zaznaczeniu wszystkich zgód wymaganych.

Akcje drugorzędne: otwórz pełną treść dokumentu, pobierz dokument, zmień zgodę dobrowolną, wróć do dodatków.

Akcje niszczące: nie dotyczy. Wycofanie zgody dobrowolnej po zapisie odbywa się w ustawieniach ATH-14.

Stany ekranu: ładowanie treści zgód ze szkieletem; pusty nie dotyczy, zawsze jest co najmniej regulamin i polityka prywatności; wypełniony to lista zgód do decyzji; błąd pobrania treści dokumentu pokazuje to i pozwala ponowić, nie udając, że dokumentu nie ma; sukces przenosi do ATH-06; brak uprawnień nie dotyczy; offline pozwala przeczytać wczytane wcześniej treści, a zapis akceptacji wymaga sieci.

Walidacja i komunikaty: próba przejścia dalej bez zgody wymaganej tłumaczy, której zgody brakuje, i przewija do niej, zamiast tylko wyszarzać przycisk bez wyjaśnienia. Odmowa zgody na wizerunek jest dozwolona i nie blokuje startu, a system tłumaczy skutek dla zdjęć z RunPixie. Zmiana wersji regulaminu między zapisem a startem prosi o ponowną akceptację z pokazaniem, co się zmieniło.

Przypadki brzegowe: zawodnik, który zapisuje inną osobę w ramach transferu i nie może w jej imieniu złożyć oświadczenia zdrowotnego, więc system odracza tę zgodę do osoby docelowej; odmowa zgody na wizerunek wpływająca na widoczność w galerii; aktualizacja regulaminu organizatora po opłaceniu startu; oświadczenie zdrowotne wymagane tylko dla wybranych dystansów.

Przejścia i następne ekrany: treści i wymagalność pochodzą z konfiguracji OEV-07, sukces prowadzi do podsumowania i płatności w ATH-06, zgoda na wizerunek rzutuje na galerię ATH-11, ponowna akceptacja wraca z ATH-02, odroczona zgoda osoby docelowej łączy się z transferem w ATH-08.

Zdarzenia systemowe: zapis akceptacji z wersją i datą dokumentu do rozliczalności, sygnał o nowej wersji regulaminu wymagającej ponownej zgody, propagacja zgody na wizerunek do warstwy galerii.

Wymagania prawne: zgody RODO z wersją, datą i możliwością wycofania zgód dobrowolnych, rozdzielenie zgód wymaganych od dobrowolnych bez wymuszania pakietu, zgoda na wizerunek jako osobna i dobrowolna, oświadczenie zdrowotne jako szczególna kategoria danych za osobną zgodą, jasna podstawa przetwarzania danych przetwarzanych do realizacji startu.

Kontekst urządzenia: telefon jako podstawowy, treści w modalu z wygodnym przewijaniem i pobraniem. Desktop do spokojnego przeczytania pełnego regulaminu.

Lokalizacja i języki: treść zgód i polityk jako treść organizatora z fallbackiem języka, etykiety jako klucze, data i wersja dokumentu według regionu.

Lista kontrolna heurystyk: kontrola i swoboda przez rozdzielenie zgód wymaganych od dobrowolnych; pomoc i dokumentacja przez odnośniki do pełnych treści i ich pobranie; pomoc w diagnozie przez wskazanie brakującej zgody zamiast niemego wyszarzenia; widoczność stanu systemu przez wersję i datę każdej zgody; zgodność ze światem przez streszczenie zgody językiem zawodnika.

Metryki sukcesu: odsetek zgłoszeń z kompletem zgód za pierwszym podejściem, udział zgód dobrowolnych udzielonych świadomie, liczba ponownych akceptacji po zmianie wersji, odsetek otwarć pełnej treści dokumentów.

---

### ATH-06 - podsumowanie i płatność

Grupa i rola: zawodnik.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę zobaczyć dokładnie, za co płacę i ile, wybrać wygodną metodę płatności i sfinalizować start w jednym przejściu, z pewnością, że dopiero teraz, po wpłacie, dostanę numer startowy.

Punkty wejścia: przejście ze zgód ATH-05, alert nieopłaconego zgłoszenia z ATH-02, powrót po nieudanej płatności z zachowanym wyborem, dokończenie wersji roboczej zapisu.

Warunki wstępne: kompletne zgłoszenie z ATH-03, zaznaczone zgody wymagane z ATH-05, podpięte bramki płatności na poziomie operatora w OPR-06, dostępne saldo portfela jeśli zawodnik chce nim pokryć część kwoty.

Dane wyświetlane: rozbicie kwoty na wpisowe według progu z OEV-10, dodatki z ATH-04, opłatę za ewentualną operację samoobsługową, naliczony kredyt z portfela, kwotę pozostałą do zapłaty, wybór metody płatności Przelewy24, PayU, Stripe, BLIK oraz karty, czas rezerwacji miejsca pozostały do wygaśnięcia, wyraźna informacja, że numer startowy zostanie nadany po zaksięgowaniu wpłaty.

Komponenty interfejsu: podsumowanie pozycji z możliwością cofnięcia się do edycji każdej z nich, przełącznik pokrycia z portfela z przeliczeniem reszty, wybór metody płatności, licznik rezerwacji miejsca, przycisk finalizacji z jasną kwotą.

Akcja główna: zapłać i sfinalizuj zgłoszenie. Gdy portfel pokrywa całość, dominującą akcją jest potwierdź zgłoszenie bez przechodzenia do bramki.

Akcje drugorzędne: zmień metodę płatności, wróć do dodatków lub zgód, użyj kredytu z portfela, pobierz wstępne podsumowanie.

Akcje niszczące: nie dotyczy. Porzucenie płatności zwalnia rezerwację po czasie i nie tworzy zobowiązania.

Stany ekranu: ładowanie podsumowania ze szkieletem pozycji; pusty nie dotyczy, podsumowanie zawsze ma co najmniej wpisowe; wypełniony to gotowe podsumowanie z wyborem metody; błąd płatności wraca tutaj z zachowanym wyborem i tłumaczy przyczynę odrzucenia oraz następny krok; sukces przenosi do potwierdzenia ATH-07 z nadanym numerem; brak uprawnień nie dotyczy; offline pokazuje, że finalizacja wymaga sieci, i zachowuje gotowe podsumowanie do powrotu łącza.

Walidacja i komunikaty: wygaśnięcie rezerwacji miejsca w trakcie płatności tłumaczy, że trzeba potwierdzić dostępność ponownie, i sprawdza, czy miejsce nadal jest. Odrzucenie płatności przez bramkę podaje powód w języku zawodnika i proponuje inną metodę zamiast surowego kodu błędu. Zmiana progu cenowego, która zaszła po dłuższej bezczynności, pokazuje nową kwotę przed obciążeniem. Kwota zero po pokryciu z portfela zamienia płatność na proste potwierdzenie.

Przypadki brzegowe: płatność BLIK z kodem wygasającym w trakcie; częściowe pokrycie portfelem i reszta kartą; podwójne kliknięcie finalizacji zabezpieczone przed dwukrotnym obciążeniem; płatność zaksięgowana z opóźnieniem, gdzie zgłoszenie czeka w stanie oczekuje na płatność, a numer pojawia się po księgowaniu; zwrot środków trafiający do portfela jako kredyt po anulowaniu przez zawodnika.

Przejścia i następne ekrany: progi i ceny z OEV-10, dodatki z OEV-11, bramki z OPR-06, sukces prowadzi do ATH-07, rozliczenie i faktura widoczne w ATH-10 i po stronie organizatora w OEV-18, nieudana płatność wraca tutaj, kredyt portfela do ATH-10.

Zdarzenia systemowe: utworzenie transakcji w bramce, oczekiwanie na potwierdzenie księgowania, nadanie numeru startowego po zaksięgowaniu zgodnie z OEV-14, wystawienie potwierdzenia płatności, zapis pozycji do portfela przy nadpłacie lub zwrocie, zabezpieczenie idempotencji przy ponownym wysłaniu.

Wymagania prawne: ceny brutto i pełne rozbicie kosztu przed płatnością, jasna informacja o tym, kto jest sprzedawcą usługi i kto wystawia dokument, zgodność z wymogami płatności elektronicznych, prawo odstąpienia i jego ograniczenia dla usługi terminowej komunikowane przed finalizacją, brak przechowywania danych karty po stronie panelu, dane płatności obsługiwane przez bramkę.

Kontekst urządzenia: telefon jako podstawowy, BLIK i płatność mobilna jako naturalna ścieżka, licznik rezerwacji widoczny. Desktop do płatności kartą przy spokojnym przeglądzie podsumowania.

Lokalizacja i języki: kwoty, waluta i metody płatności według regionu i dostępności w danym kraju, etykiety jako klucze, nazwy pozycji jako treść organizatora.

Lista kontrolna heurystyk: widoczność stanu systemu przez pełne rozbicie kwoty, licznik rezerwacji i jasny status płatności; pomoc w diagnozie przez czytelny komunikat odrzucenia z propozycją innej metody; zapobieganie błędom przez zabezpieczenie przed podwójnym obciążeniem; kontrola i swoboda przez powrót do edycji każdej pozycji; zgodność ze światem przez regułę numeru po wpłacie wyłożoną wprost.

Metryki sukcesu: odsetek finalizacji bez porzucenia na płatności, udział płatności udanych za pierwszym podejściem, średni czas od zgód do zaksięgowania, odsetek zgłoszeń utkniętych w stanie oczekuje na płatność, udział płatności pokrytych częściowo portfelem.

---

## Partia 2 (ATH-07 do ATH-12)

### ATH-07 - potwierdzenie zapisu

Grupa i rola: zawodnik.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę natychmiast po wpłacie zobaczyć czarno na białym, że jestem zapisany, jaki mam numer startowy i co dalej, oraz mieć to gdzie wrócić, gdy będę tego szukać na starcie.

Punkty wejścia: udana finalizacja z ATH-06, link z e-maila potwierdzającego, kafelek startu z ATH-02 po opłaceniu, powrót po zaksięgowaniu opóźnionej płatności.

Warunki wstępne: zaksięgowana wpłata albo pełne pokrycie z portfela, nadany numer startowy zgodnie z regułami OEV-14, kompletne zgłoszenie ze zgodami.

Dane wyświetlane: status opłacone i nadany numer startowy, nazwa wydarzenia, dystans, kategoria i data, kod odbioru pakietu, najważniejsze następne kroki - kiedy i gdzie odbiór, godzina startu, lista rzeczy do zabrania, podsumowanie zamówionych gadżetów, link do potwierdzenia płatności, zaproszenie do galerii i klasyfikacji dostępne po biegu.

Komponenty interfejsu: wyraźny baner sukcesu z numerem startowym, kod QR odbioru pakietu, karta startu z kluczowymi danymi, przyciski dodania do kalendarza i pobrania potwierdzenia, sekcja następnych kroków, skrót do samoobsługi zgłoszenia.

Akcja główna: zapisz start do telefonu - dodaj do kalendarza i pobierz kartę z numerem oraz kodem odbioru, dostępną później offline.

Akcje drugorzędne: pobierz potwierdzenie płatności, otwórz szczegóły startu, zamów dodatkowe gadżety, udostępnij informację o starcie, otwórz samoobsługę.

Akcje niszczące: nie dotyczy na tym ekranie.

Stany ekranu: ładowanie krótkie, do czasu potwierdzenia numeru; pusty nie dotyczy; wypełniony to pełne potwierdzenie z numerem; błąd dotyczy sytuacji, gdy płatność zaksięgowała się, ale numer jeszcze się nadaje, i pokazuje stan przejściowy z informacją, kiedy numer się pojawi, zamiast udawać porażkę; sukces jest stanem domyślnym ekranu; brak uprawnień nie dotyczy; offline pokazuje zachowaną kartę startu z numerem i kodem z ostatniej synchronizacji.

Walidacja i komunikaty: opóźnione nadanie numeru tłumaczy, że wpłata jest zaksięgowana, a numer pojawi się po przetworzeniu, i obiecuje powiadomienie. Brak zamówionego gadżetu w magazynie po finalizacji informuje o tym i kieruje do rozwiązania przez organizatora, nie chowając problemu. Nieudane pobranie karty proponuje ponowienie i wysyłkę na e-mail.

Przypadki brzegowe: numer nadany z opóźnieniem przy ręcznej weryfikacji płatności; zawodnik przepisany od innej osoby, który widzi potwierdzenie dopiero po uzupełnieniu swoich danych; start w sztafecie z jednym numerem dla zespołu; zawodnik bez dostępu do drukarki, dla którego kod QR na telefonie jest jedyną formą odbioru; potwierdzenie dla startu odroczonego na kolejną edycję bez aktywnego numeru.

Przejścia i następne ekrany: nadanie numeru z OEV-14, dane startu z listy startowej OEV-15, samoobsługa do ATH-08, dosprzedaż do ATH-09, galeria do ATH-11 po biegu, klasyfikacja do ATH-12, potwierdzenie płatności powiązane z ATH-10.

Zdarzenia systemowe: wysyłka e-maila potwierdzającego z kartą startu, dodanie startu do osi na pulpicie ATH-02, sygnał o nadaniu numeru, przygotowanie pliku karty do pobrania, push o gotowości numeru przy opóźnieniu.

Wymagania prawne: potwierdzenie zawarcia umowy o usługę z istotnymi warunkami, dokument płatności zgodny z wymogami, dane na karcie ograniczone do potrzebnych do startu, zgoda na wizerunek odzwierciedlona w dostępności galerii, jasne warunki odbioru pakietu.

Kontekst urządzenia: telefon jako podstawowy, karta startu i kod QR zaprojektowane do pokazania na starcie offline. Desktop do pobrania i wydruku potwierdzenia.

Lokalizacja i języki: etykiety jako klucze, data, godzina i kwoty według regionu, nazwa wydarzenia i instrukcje organizatora jako treść z fallbackiem języka.

Lista kontrolna heurystyk: widoczność stanu systemu przez wyraźny status opłacone i nadany numer; rozpoznawanie zamiast przypominania przez następne kroki wyłożone wprost; pomoc i dokumentacja przez kartę startu do zabrania; pomoc w diagnozie przez czytelny stan przejściowy przy opóźnionym numerze; zgodność ze światem przez język zawodnika i kod QR znany ze startów.

Metryki sukcesu: odsetek zawodników, którzy zapisali kartę startu lub dodali start do kalendarza, udział numerów nadanych natychmiast po wpłacie, liczba zgłoszeń pomocy o numer mimo zaksięgowanej wpłaty, odsetek odbiorów pakietu kodem z telefonu.

---

### ATH-08 - samoobsługa zgłoszenia

Grupa i rola: zawodnik.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę sam, bez maila do organizatora, przepisać start koledze, przełożyć go na przyszły rok, wejść lub zejść z listy rezerwowej albo wymienić numer lub pakiet, widząc jasno, co wolno w regułach tego wydarzenia i ile to kosztuje.

Punkty wejścia: kafelek startu z ATH-02, potwierdzenie zapisu ATH-07, oferta z listy rezerwowej z powiadomienia push, alert o możliwej operacji z pulpitu, link z e-maila o otwarciu okna transferów.

Warunki wstępne: aktywne zgłoszenie zawodnika, reguły operacji ustawione przez organizatora w OEV-16 dla transferów i odroczeń oraz w OEV-17 dla list rezerwowych, ewentualna opłata za operację zdefiniowana jako przychód organizatora, otwarte okno czasowe danej operacji.

Dane wyświetlane: lista dostępnych operacji z krótkim opisem skutku - transfer do innej osoby, odroczenie na kolejną edycję, wymiana numeru lub pakietu, zejście z listy rezerwowej, wejście na listę rezerwową gdy brak miejsc, warunki i opłata każdej operacji, termin do którego operacja jest możliwa, status zgłoszenia i bieżący numer jeśli już nadany.

Komponenty interfejsu: lista operacji z wyraźnym oznaczeniem darmowych i płatnych, kreator wybranej operacji krok po kroku, pole danych osoby docelowej przy transferze, wybór nowej edycji przy odroczeniu, podsumowanie skutku i kosztu przed potwierdzeniem, sekcja reguł organizatora w czytelnej formie.

Akcja główna: wykonaj wybraną operację. Dominująca akcja zmienia się zależnie od kontekstu - przy ofercie z listy rezerwowej jest to potwierdź miejsce z odliczaniem.

Akcje drugorzędne: porównaj operacje, zobacz reguły wydarzenia, anuluj operację w toku, skontaktuj się z organizatorem gdy operacja jest poza regułami.

Akcje niszczące: zrezygnuj ze startu i poproś o zwrot, przepisz start tracąc do niego prawo, zejdź z listy rezerwowej. Każda z tych operacji wymaga wyraźnego potwierdzenia z pokazaniem nieodwracalnego skutku i kwoty zwrotu lub opłaty.

Stany ekranu: ładowanie listy operacji ze szkieletem; pusty oznacza, że dla tego zgłoszenia żadna operacja nie jest teraz dostępna, i tłumaczy dlaczego oraz kiedy się to zmieni; wypełniony to lista możliwych operacji; błąd wykonania tłumaczy przyczynę i nie zostawia zgłoszenia w stanie pośrednim; sukces potwierdza zmianę i aktualizuje status; brak uprawnień dotyczy operacji zablokowanej regułą organizatora z wyjaśnieniem; offline pozwala przejrzeć reguły z danych lokalnych, a wykonanie operacji wymaga sieci.

Walidacja i komunikaty: transfer na adres bez konta wysyła zaproszenie do założenia konta i tłumaczy, że osoba docelowa musi dokończyć dane i zgody. Operacja po terminie pokazuje, że okno się zamknęło, i kieruje do kontaktu z organizatorem zamiast cicho jej odmawiać. Wymiana numeru gdy numer jeszcze nie nadany tłumaczy, że numer pojawi się dopiero po wpłacie. Oferta z listy rezerwowej z wygasłym odliczaniem informuje, że miejsce trafiło do kolejnej osoby. Opłata za operację pokazuje pełną kwotę przed potwierdzeniem.

Przypadki brzegowe: transfer do osoby, która sama ma już start na tym dystansie; odroczenie z różnicą ceny między edycjami rozliczaną przez portfel; zawodnik na liście rezerwowej, który dostał ofertę, ale w tym czasie zapisał się gdzie indziej; wymiana pakietu na inny rozmiar zależna od stanu magazynu OEV-11; rezygnacja z częściowym zwrotem zgodnie z polityką organizatora i resztą jako kredyt w portfelu.

Przejścia i następne ekrany: reguły transferów i odroczeń z OEV-16, listy rezerwowe z OEV-17, wymiana pakietu sprzęga się z magazynem OEV-11, zwrot i opłata rozliczają się w ATH-10 i po stronie organizatora w OEV-18, transfer do nowej osoby prowadzi przez ATH-01 i uzupełnienie zgód w ATH-05, nadanie lub zmiana numeru przez OEV-14.

Zdarzenia systemowe: zapis zmiany statusu zgłoszenia, zwolnienie miejsca przy odroczeniu lub rezygnacji i zasilenie listy rezerwowej, wysyłka zaproszenia przy transferze, naliczenie opłaty za operację jako przychód organizatora, zapis zwrotu do portfela, log operacji do audytu organizatora.

Wymagania prawne: jasne warunki i koszty operacji przed jej wykonaniem, polityka zwrotów zgodna z prawem konsumenckim i z regulaminem wydarzenia, transfer danych nowej osobie zgodny z RODO z jej własną zgodą i oświadczeniami, ślad rozliczalności każdej operacji, opłata za operację jako przychód organizatora wyłożona przejrzyście.

Kontekst urządzenia: telefon jako podstawowy, operacje wykonalne jedną ręką wieczorem. Desktop do spokojnego porównania reguł przy droższych decyzjach jak odroczenie.

Lokalizacja i języki: reguły i opisy operacji jako treść organizatora z fallbackiem języka, etykiety jako klucze, kwoty i terminy według regionu.

Lista kontrolna heurystyk: kontrola i swoboda przez samoobsługę operacji w granicach reguł; zapobieganie błędom przez podsumowanie skutku i kosztu przed potwierdzeniem; widoczność stanu systemu przez jasny status zgłoszenia i terminy okien; pomoc w diagnozie przez wyjaśnienie, czemu operacja jest niedostępna; zgodność ze światem przez nazwy operacji znane ze startów.

Metryki sukcesu: udział spraw załatwionych samoobsługowo bez kontaktu z organizatorem, odsetek transferów dokończonych przez osobę docelową, czas od oferty z listy rezerwowej do potwierdzenia, udział operacji płatnych w przychodzie organizatora, liczba operacji nieudanych zostawiających zgłoszenie w stanie pośrednim.

---

### ATH-09 - dosprzedaż gadżetów

Grupa i rola: zawodnik.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: po zapisie chcę spokojnie dokupić to, co mi się przyda na ten konkretny start, albo zmienić rozmiar zamówionej koszulki, widząc realną dostępność i nie tracąc czasu na ponowne wpisywanie danych.

Punkty wejścia: potwierdzenie zapisu ATH-07, kafelek startu z ATH-02, baner dosprzedaży na pulpicie, link z e-maila przypominającego o terminie zamówień, powrót z koszyka dodatków ATH-04.

Warunki wstępne: opłacone zgłoszenie zawodnika, otwarte okno zamówień gadżetów ustawione przez organizatora, skonfigurowany magazyn z OEV-11 ze stanami i wariantami, podpięte bramki płatności.

Dane wyświetlane: katalog gadżetów dostępnych po zapisie z ceną, wariantem i rozmiarem, realny stan magazynu, pozycje już zamówione z możliwością zmiany rozmiaru w granicach reguł, sugestie dopasowane do dystansu i pogody opisane jako podpowiedź, suma nowego zamówienia, możliwe pokrycie z portfela, termin do którego zamówienie jest możliwe.

Komponenty interfejsu: karty produktów z wyborem wariantu, sekcja moje zamówienia z opcją wymiany rozmiaru, wskaźnik niskiego stanu z realną liczbą, podsumowanie kwoty, przełącznik pokrycia z portfela, informacja o terminie i sposobie odbioru dokupionych rzeczy.

Akcja główna: dokup i zapłać. Przy pokryciu z portfela w całości dominującą akcją jest potwierdź zamówienie.

Akcje drugorzędne: zmień rozmiar zamówionej rzeczy, usuń pozycję z nowego koszyka, otwórz tabelę rozmiarów, dopłać z portfela, pomiń.

Akcje niszczące: anuluj wcześniej zamówiony gadżet jeśli reguły organizatora na to pozwalają, z wyraźnym potwierdzeniem i informacją o zwrocie do portfela.

Stany ekranu: ładowanie katalogu ze szkieletem; pusty oznacza zamknięte okno zamówień albo brak gadżetów i tłumaczy, do kiedy było otwarte; wypełniony to katalog z dostępnością i moimi zamówieniami; błąd pobrania stanu magazynu ostrzega o możliwej nieaktualności i proponuje odświeżenie; sukces potwierdza zamówienie tostem; brak uprawnień nie dotyczy własnego zgłoszenia; offline pozwala przejrzeć katalog z danych lokalnych, a zamówienie i płatność wymagają sieci.

Walidacja i komunikaty: wymiana rozmiaru na wariant niedostępny pokazuje to i proponuje najbliższy dostępny. Wyczerpanie ostatniej sztuki w trakcie tłumaczy, że gadżet zniknął z dostępności, i nie obciąża. Zamówienie po terminie informuje, że okno się zamknęło, i podaje, czy odbiór na miejscu będzie możliwy. Kwota powyżej salda portfela pokazuje dopłatę inną metodą.

Przypadki brzegowe: wymiana rozmiaru koszulki zamówionej w pierwotnym zapisie; dokupienie medalu pamiątkowego dla osoby towarzyszącej; limit sztuk na osobę liczony łącznie z pierwszym zamówieniem; dosprzedaż tuż przed zamknięciem okna z krótką rezerwacją; gadżet dostępny tylko w odbiorze na miejscu, bez wysyłki.

Przejścia i następne ekrany: katalog i stany z OEV-11, sugestie pogodowe z ATH-13, płatność rozlicza się jak w ATH-06 i widnieje w ATH-10, zmiana rozmiaru sprzęga się z magazynem OEV-11 i może łączyć z wymianą pakietu w ATH-08.

Zdarzenia systemowe: rezerwacja wariantu na czas płatności, aktualizacja stanu w OEV-11, zapis zamówienia do rozliczenia organizatora w OEV-18, zwrot do portfela przy anulowaniu, przeliczenie sumy po każdej zmianie.

Wymagania prawne: ceny brutto widoczne, prawo odstąpienia dla towaru, jasna informacja o terminie i sposobie odbioru, brak domyślnie zaznaczonych płatnych pozycji, brak presji sztucznymi licznikami zamiast realnego stanu.

Kontekst urządzenia: telefon jako podstawowy, jedna kolumna kart. Desktop przy gadżetach do spokojnego porównania i tabeli rozmiarów.

Lokalizacja i języki: nazwy i opisy jako treść organizatora, etykiety jako klucze, rozmiary i miary według regionu, kwoty i waluta lokalnie.

Lista kontrolna heurystyk: widoczność stanu systemu przez realny stan magazynu i termin okna; zapobieganie błędom przez blokadę zamówienia niedostępnego wariantu; kontrola i swoboda przez wymianę rozmiaru i anulowanie w granicach reguł; minimalistyczny projekt przez sugestie jako podpowiedź, nie narzucony wybór; zgodność ze światem przez tabelę rozmiarów z miarami.

Metryki sukcesu: udział zawodników dokupujących po zapisie, odsetek wymian rozmiaru rozwiązanych samoobsługowo, średnia wartość dosprzedaży na zawodnika, odsetek zamówień przerwanych brakiem dostępności.

---

### ATH-10 - portfel i kredyty

Grupa i rola: zawodnik. Portfel jest wspólny dla konta rodzinnego i dla obu modułów, więc to ten sam portfel, który rodzic widzi w PAR-10.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę zobaczyć, ile mam kredytu u danego organizatora, skąd się wziął i na co mogę go wykorzystać, oraz pobrać dokument płatności, kiedy go potrzebuję do rozliczenia.

Punkty wejścia: kafelek portfela z pulpitu ATH-02, link z potwierdzenia zapisu ATH-07, rozliczenie zwrotu po operacji w ATH-08, skrót z podsumowania płatności ATH-06, alert o dostępnym kredycie.

Warunki wstępne: aktywne konto z historią płatności lub zwrotów u co najmniej jednego organizatora. Konto bez operacji pokazuje stan pusty.

Dane wyświetlane: saldo kredytu rozbite po organizatorach, bo kredyt jest u danego organizatora i nie przenosi się między nimi, historia operacji z datą, kwotą i tytułem, lista płatności z dostępnymi dokumentami, oznaczenie, na co kredyt można wykorzystać, informacja o tym, że wypłata na zewnątrz wymaga osobnej procedury organizatora.

Komponenty interfejsu: karta salda per organizator, lista transakcji z filtrem, sekcja dokumentów płatności z pobraniem, wyjaśnienie zasad kredytu, skrót do wykorzystania kredytu przy najbliższym zapisie lub dosprzedaży.

Akcja główna: wykorzystaj kredyt przy najbliższej płatności. Gdy nie ma czego opłacić, dominującą akcją jest pobierz dokument płatności.

Akcje drugorzędne: filtruj historię, pobierz dokument, zobacz szczegóły transakcji, poproś organizatora o wypłatę kredytu.

Akcje niszczące: nie dotyczy. Wypłata kredytu na zewnątrz przechodzi przez procedurę organizatora, nie przez nieodwracalną akcję w panelu.

Stany ekranu: ładowanie ze szkieletem listy; pusty wita konto bez operacji i tłumaczy, że kredyt pojawi się po nadpłacie lub zwrocie; wypełniony to saldo i historia; błąd pobrania dokumentu proponuje ponowienie i wysyłkę na e-mail; sukces dotyczy potwierdzenia użycia kredytu; brak uprawnień dotyczy ograniczonego zakresu drugiego opiekuna na koncie współdzielonym; offline pokazuje ostatnio wczytane saldo z banerem o wstrzymaniu zmian.

Walidacja i komunikaty: próba użycia kredytu jednego organizatora na start u innego tłumaczy, że kredyt jest przypisany do organizatora i nie przenosi się. Kredyt mniejszy niż kwota startu pokazuje, ile trzeba dopłacić. Wniosek o wypłatę informuje, że decyzję i termin ustala organizator. Niedostępny dokument płatności pokazuje, że jest w przygotowaniu, i obiecuje powiadomienie.

Przypadki brzegowe: zawodnik z kredytem u jednego organizatora i zerem u innego, między którymi nie ma przepływu; konto, które jest jednocześnie kontem rodzica z saldem zbieranym ze startów dziecka; zwrot częściowy po rezygnacji rozbity na kredyt i zwrot na źródło płatności; kredyt z odroczenia pokrywający różnicę ceny między edycjami; wygasający kredyt z regułą terminu ustaloną przez organizatora.

Przejścia i następne ekrany: wykorzystanie kredytu sprzęga się z płatnością w ATH-06 i dosprzedażą w ATH-09, zwroty pochodzą z operacji w ATH-08, dokumenty i rozliczenie po stronie organizatora w OEV-18 i OEV-19, ten sam portfel widnieje w PAR-10 dla roli rodzica.

Zdarzenia systemowe: zaksięgowanie kredytu po nadpłacie lub zwrocie, naliczenie wykorzystania przy płatności, przygotowanie dokumentu do pobrania, sygnał o dostępnym kredycie na pulpicie, log operacji portfela.

Wymagania prawne: jasne zasady kredytu i jego nieprzenoszalności między organizatorami, dokumenty płatności zgodne z wymogami, prawo do informacji o saldzie i historii, procedura wypłaty zgodna z polityką organizatora i prawem, przechowywanie danych rozliczeniowych zgodnie z obowiązkiem podatkowym organizatora.

Kontekst urządzenia: telefon do szybkiego sprawdzenia salda, desktop do pobrania i archiwizacji dokumentów płatności.

Lokalizacja i języki: kwoty, waluta i daty według regionu, etykiety jako klucze, tytuły transakcji jako treść organizatora z fallbackiem języka.

Lista kontrolna heurystyk: widoczność stanu systemu przez saldo per organizator i czytelną historię; zgodność ze światem przez wyjaśnienie zasad kredytu językiem zawodnika; zapobieganie błędom przez blokadę użycia kredytu poza organizatorem; pomoc w diagnozie przez wskazanie dopłaty i statusu dokumentu; pomoc i dokumentacja przez dostęp do dokumentów płatności.

Metryki sukcesu: udział kredytu wykorzystanego na kolejne starty, odsetek dokumentów pobranych samoobsługowo, liczba zgłoszeń pomocy o saldo lub dokument, czas od zwrotu do widocznego kredytu.

---

### ATH-11 - galeria zdjęć z RunPixie

Grupa i rola: zawodnik. Ta sama integracja RunPixie zasila galerię rodzica w PAR-15 dla zdjęć dziecka.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę po biegu szybko znaleźć swoje zdjęcia bez przeglądania tysięcy cudzych, bo system zna mój numer startowy, i pobrać albo udostępnić te, które lubię.

Punkty wejścia: skrót zdjęć z pulpitu ATH-02, potwierdzenie zapisu ATH-07 po biegu, push o nowych zdjęciach z RunPixie, link z e-maila po wydarzeniu, kafelek zakończonego startu.

Warunki wstępne: opłacone zgłoszenie z nadanym numerem startowym, bo RunPixie taguje zdjęcia po numerze, zakończony bieg z wgranymi zdjęciami, zgoda na wizerunek z ATH-05 wpływająca na widoczność i udostępnianie.

Dane wyświetlane: zdjęcia dopasowane do numeru startowego zawodnika, znaczniki pewności dopasowania, zdjęcia z różnych punktów trasy i mety, metadane jak punkt na trasie i przybliżony czas, opcje pobrania i udostępnienia zgodne z udzieloną zgodą, informacja, gdy zdjęcia jeszcze się przetwarzają.

Komponenty interfejsu: siatka miniatur z lazy loading, podgląd pełnoekranowy, filtr po punkcie trasy, przyciski pobrania i udostępnienia, oznaczenie zdjęć o niepewnym dopasowaniu z możliwością potwierdzenia lub odrzucenia, baner stanu przetwarzania.

Akcja główna: pobierz wybrane zdjęcia. Alternatywnie udostępnij, jeśli zgoda i ustawienia na to pozwalają.

Akcje drugorzędne: filtruj po punkcie trasy, potwierdź lub odrzuć niepewne dopasowanie, otwórz podgląd, oznacz zdjęcie jako ulubione.

Akcje niszczące: nie dotyczy zdjęć źródłowych. Zawodnik może ukryć dopasowanie błędne, co nie usuwa zdjęcia z systemu RunPixie.

Stany ekranu: ładowanie siatki ze szkieletem miniatur; pusty oznacza brak dopasowanych zdjęć i tłumaczy, czy bieg się zakończył i czy zdjęcia są jeszcze przetwarzane; wypełniony to galeria zdjęć zawodnika; błąd pobrania pojedynczego zdjęcia dotyczy tylko jego; sukces potwierdza pobranie; brak uprawnień dotyczy zawodnika bez zgody na wizerunek, gdzie widoczność jest ograniczona zgodnie z jego wyborem; offline pokazuje wcześniej wczytane miniatury bez nowego pobierania.

Walidacja i komunikaty: brak numeru startowego, bo zgłoszenie nieopłacone, tłumaczy, że galeria działa po nadaniu numeru, i kieruje do dokończenia płatności. Zdjęcia w trakcie przetwarzania pokazują, kiedy spodziewać się kompletu, zamiast pustego ekranu. Niepewne dopasowanie prosi o potwierdzenie, że to zawodnik, żeby nie mieszać zdjęć różnych osób. Odmowa zgody na wizerunek ogranicza udostępnianie i tłumaczy skutek.

Przypadki brzegowe: dwóch zawodników z podobnie wyglądającym numerem przy zabrudzonym numerze na zdjęciu; zawodnik w sztafecie dzielący numer z zespołem; zdjęcia z kilku punktów trasy wgrywane w różnym czasie; zawodnik, który cofnął zgodę na wizerunek po biegu; brak dopasowań mimo ukończonego biegu z powodu zasłoniętego numeru.

Przejścia i następne ekrany: numer startowy z OEV-14, integracja zdjęć z RunPixie, zgoda na wizerunek z ATH-05, ta sama integracja zasila PAR-15, skrót do galerii z pulpitu ATH-02 i potwierdzenia ATH-07.

Zdarzenia systemowe: synchronizacja nowych zdjęć z RunPixie po wydarzeniu, dopasowanie po numerze startowym, push o gotowych zdjęciach, zapis potwierdzeń i odrzuceń dopasowania, respektowanie zgody na wizerunek przy udostępnianiu.

Wymagania prawne: zgoda na wykorzystanie wizerunku jako podstawa widoczności i udostępniania, dane wizerunkowe pod RODO, prawo do ograniczenia i sprzeciwu wpływające na galerię, brak gromadzenia i analizy danych biometrycznych twarzy ponad dopasowanie po numerze, jasna informacja o źródle zdjęć i podmiocie przetwarzającym.

Kontekst urządzenia: telefon do szybkiego przeglądu i udostępnienia, desktop przy gadżetach do pobrania zdjęć w pełnej rozdzielczości.

Lokalizacja i języki: etykiety jako klucze, daty i czasy według regionu, nazwy punktów trasy jako treść organizatora z fallbackiem języka.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania przez automatyczne dopasowanie po numerze; widoczność stanu systemu przez znacznik przetwarzania i pewności dopasowania; zapobieganie błędom przez potwierdzenie niepewnych dopasowań; kontrola i swoboda przez respektowanie zgody na wizerunek; pomoc w diagnozie przez wyjaśnienie braku zdjęć.

Metryki sukcesu: odsetek zawodników, którzy znaleźli swoje zdjęcia, udział poprawnych dopasowań po numerze, liczba korekt niepewnych dopasowań, odsetek pobrań i udostępnień zgodnych ze zgodą.

---

### ATH-12 - klasyfikacja generalna serii z moim wynikiem

Grupa i rola: zawodnik. Publiczną wersję klasyfikacji bez kontekstu zawodnika opisuje GST-07.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę zobaczyć swój wynik i miejsce w klasyfikacji serii, porównać się w mojej kategorii i sprawdzić, ile punktów dzieli mnie od osób przede mną, bez przeszukiwania długiej listy.

Punkty wejścia: skrót klasyfikacji z pulpitu ATH-02, potwierdzenie zapisu ATH-07 po biegu, push o opublikowaniu wyników, link z publicznej klasyfikacji GST-07, kafelek zakończonego startu.

Warunki wstępne: zawodnik z numerem startowym i ukończonym biegiem, opublikowane wyniki z pomiaru czasu z OEV-24, skonfigurowana klasyfikacja serii z OEV-23 z zasadami punktacji i kategoriami.

Dane wyświetlane: pozycja zawodnika w klasyfikacji generalnej i w kategorii wyróżniona w tabeli, jego czas i punkty, różnica do osób przed i za nim, historia jego startów w serii, zasady punktacji opisane czytelnie, stan klasyfikacji z datą ostatniej aktualizacji, oznaczenie wyników wstępnych przed weryfikacją.

Komponenty interfejsu: tabela klasyfikacji z przypiętym wierszem zawodnika, przełącznik generalna albo kategoria, filtr po edycji i dystansie, karta osobistego wyniku z czasem i punktami, sekcja zasad punktacji, znacznik wyniku wstępnego.

Akcja główna: zobacz mój wynik w kontekście. Tabela domyślnie przewija się do wiersza zawodnika i go wyróżnia.

Akcje drugorzędne: przełącz na kategorię, filtruj po edycji lub dystansie, otwórz zasady punktacji, udostępnij swój wynik, pobierz wynik.

Akcje niszczące: nie dotyczy. Wynik jest danymi pomiaru, nie zmienia go zawodnik; reklamacja wyniku idzie do organizatora.

Stany ekranu: ładowanie tabeli ze szkieletem wierszy; pusty oznacza brak opublikowanych wyników i tłumaczy, kiedy się pojawią; wypełniony to klasyfikacja z wyróżnionym zawodnikiem; błąd pobrania fragmentu tabeli dotyczy tylko jego; sukces nie dotyczy osobno; brak uprawnień nie dotyczy wyników publicznych; offline pokazuje ostatnio wczytaną klasyfikację z datą synchronizacji.

Walidacja i komunikaty: wynik wstępny przed weryfikacją oznacza się jasno, żeby zawodnik nie traktował go jako ostatecznego. Brak zawodnika w klasyfikacji mimo ukończonego biegu tłumaczy możliwe przyczyny, jak brak odczytu na mecie, i kieruje do reklamacji u organizatora. Niespójność czasu między punktami pomiaru pokazuje status w weryfikacji zamiast błędnej liczby.

Przypadki brzegowe: zawodnik startujący w kilku edycjach serii z sumowaniem punktów; zmiana kategorii wiekowej między edycjami; dyskwalifikacja lub korekta wyniku zmieniająca pozycję po publikacji; remis punktowy rozstrzygany regułą serii; zawodnik z wynikiem wstępnym, który zmienił się po weryfikacji.

Przejścia i następne ekrany: wyniki z pomiaru czasu OEV-24, zasady i kategorie z konfiguracji serii OEV-23, publiczna wersja w GST-07, skrót z pulpitu ATH-02, zdjęcia powiązane z tym startem w ATH-11.

Zdarzenia systemowe: import wyników z pomiaru czasu, przeliczenie punktów serii po każdej edycji, publikacja wyników i push do zawodników, oznaczenie wyników wstępnych i ich aktualizacja po weryfikacji.

Wymagania prawne: publikacja danych wynikowych w zakresie zgodnym z regulaminem i zgodą zawodnika, możliwość ograniczenia widoczności danych osobowych w publicznej tabeli, rozróżnienie wyniku wstępnego od oficjalnego, ścieżka reklamacji wyniku, dane przetwarzane do celu klasyfikacji.

Kontekst urządzenia: telefon do szybkiego sprawdzenia miejsca, desktop przy gadżetach do spokojnej analizy wyniku i historii startów.

Lokalizacja i języki: etykiety jako klucze, czasy, daty i format liczb według regionu, nazwy kategorii i serii jako treść organizatora z fallbackiem języka.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania przez przypięty i wyróżniony wiersz zawodnika; widoczność stanu systemu przez datę aktualizacji i znacznik wyniku wstępnego; zgodność ze światem przez zasady punktacji opisane czytelnie; pomoc w diagnozie przez wyjaśnienie braku w klasyfikacji; minimalistyczny projekt przez domyślne przewinięcie do własnego wyniku.

Metryki sukcesu: odsetek zawodników sprawdzających wynik po biegu, czas do odnalezienia własnej pozycji, liczba reklamacji wyniku, udział wyników wstępnych skorygowanych po weryfikacji.

---

## Partia 3 (ATH-13 do ATH-14)

### ATH-13 - powiadomienia pogodowe

Grupa i rola: zawodnik. Po stronie organizatora powiadomienia pogodowe konfiguruje OEV-26.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę z wyprzedzeniem wiedzieć, jaka pogoda będzie na moim starcie i czy organizator wydał ostrzeżenie albo zmienił plan, żeby dobrać ubiór i nie dać się zaskoczyć burzy ani upałowi.

Punkty wejścia: ostrzeżenie pogodowe z pulpitu ATH-02, push o zmianie warunków, link z e-maila przed startem, kafelek startu, sugestia gadżetów z ATH-04 i ATH-09 powiązana z pogodą.

Warunki wstępne: opłacone zgłoszenie z datą i miejscem startu, integracja z API pogodowym, ewentualne komunikaty i ostrzeżenia wydane przez organizatora w OEV-26.

Dane wyświetlane: prognoza dla miejsca i godziny startu z temperaturą, opadami, wiatrem i odczuwalną temperaturą, ostrzeżenia organizatora z OEV-26 jeśli wydane, oznaczenie powagi ostrzeżenia kolorem semantycznym, podpowiedzi ubioru i dodatków dopasowane do prognozy, data i godzina ostatniej aktualizacji prognozy, informacja o ewentualnej zmianie planu wydarzenia.

Komponenty interfejsu: karta prognozy dla godziny startu, oś prognozy godzinowej wokół startu, baner ostrzeżenia organizatora z kolorem semantycznym, sekcja podpowiedzi ubioru z odnośnikiem do gadżetów, znacznik czasu aktualizacji.

Akcja główna: ustaw powiadomienie o zmianie warunków dla tego startu. Gdy ostrzeżenie już jest, dominującą akcją jest przeczytaj komunikat organizatora.

Akcje drugorzędne: odśwież prognozę, otwórz podpowiedzi gadżetów, zobacz komunikat organizatora, wyłącz powiadomienia pogodowe dla tego startu.

Akcje niszczące: nie dotyczy.

Stany ekranu: ładowanie prognozy ze szkieletem; pusty oznacza, że prognoza dla tak odległej daty nie jest jeszcze dostępna, i mówi, od kiedy będzie; wypełniony to prognoza i ewentualne ostrzeżenie; błąd pobrania prognozy tłumaczy, że dane pogodowe są chwilowo niedostępne, i proponuje ponowienie, nie pokazując zmyślonych liczb; sukces dotyczy ustawienia powiadomienia; brak uprawnień nie dotyczy; offline pokazuje ostatnio pobraną prognozę z wyraźnym znacznikiem czasu i ostrzeżeniem, że może być nieaktualna.

Walidacja i komunikaty: nieaktualna prognoza z powodu braku łącza oznacza się czasem ostatniej aktualizacji, żeby zawodnik nie ufał starym danym jak świeżym. Ostrzeżenie organizatora ma pierwszeństwo nad samą prognozą i wyświetla się na górze. Sprzeczność między prognozą API a komunikatem organizatora rozstrzyga na rzecz komunikatu organizatora jako wiążącego dla wydarzenia.

Przypadki brzegowe: zmiana godziny lub trasy startu z powodu pogody ogłoszona przez organizatora; ostrzeżenie burzowe tuż przed startem wymagające szybkiej decyzji; start wieloetapowy z różną pogodą na etapach; zawodnik w innej strefie czasowej niż miejsce startu; prognoza znacząco zmieniona w ostatniej dobie.

Przejścia i następne ekrany: ostrzeżenia i komunikaty z OEV-26, podpowiedzi gadżetów do ATH-04 i ATH-09, dane startu z listy startowej OEV-15, alert pogodowy na pulpicie ATH-02, preferencje powiadomień z ustawień ATH-14.

Zdarzenia systemowe: cykliczne odświeżanie prognozy z API, push o ostrzeżeniu organizatora i o istotnej zmianie warunków, propagacja preferencji powiadomień, zapis ostatniej aktualizacji do widoku offline.

Wymagania prawne: jasne źródło danych pogodowych i ich charakter prognozy, komunikaty organizatora jako wiążąca informacja o wydarzeniu, kontakt powiadomieniami tylko kanałem objętym zgodą, brak gwarancji co do prognozy zewnętrznej.

Kontekst urządzenia: telefon jako podstawowy, w tym dostęp na starcie offline z ostatniej prognozy. Desktop przy gadżetach do spokojnego planowania ubioru przed wyjazdem.

Lokalizacja i języki: jednostki temperatury, prędkości wiatru i opadów według regionu, etykiety jako klucze, komunikaty organizatora jako treść z fallbackiem języka, czas w strefie miejsca startu z oznaczeniem.

Lista kontrolna heurystyk: widoczność stanu systemu przez czas ostatniej aktualizacji prognozy; pomoc w diagnozie przez czytelny komunikat o niedostępnych danych zamiast zmyślonych liczb; zgodność ze światem przez jednostki i język zawodnika; zapobieganie błędom przez pierwszeństwo wiążącego komunikatu organizatora; kontrola i swoboda przez sterowanie powiadomieniami dla startu.

Metryki sukcesu: odsetek zawodników, którzy włączyli powiadomienia pogodowe, czas od wydania ostrzeżenia do jego odczytania, udział kliknięć w podpowiedzi gadżetów z pogody, liczba zgłoszeń o nieaktualnej prognozie offline.

---

### ATH-14 - ustawienia konta i zgody

Grupa i rola: zawodnik. Konto jest wspólne z rolą rodzica, więc ustawienia pokrywają się z PAR-16 i działają na tym samym koncie.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę zarządzić swoimi danymi, zgodami i powiadomieniami w jednym miejscu, świadomie włączyć lub wyłączyć to, na co się godzę, i wiedzieć, co się stanie, gdy coś wycofam albo usunę konto.

Punkty wejścia: menu konta z dowolnego ekranu panelu, alert brakującej lub wygasającej zgody z pulpitu ATH-02, link z ustawień powiadomień pogodowych ATH-13, sekcja zgód po zmianie wersji regulaminu, ustawienia wspólne z PAR-16.

Warunki wstępne: aktywne konto. Dla edycji danych zdrowotnych dodatkowa zgoda na przetwarzanie szczególnej kategorii danych.

Dane wyświetlane: dane kontaktowe zawodnika, dane do startu jak kategoria i kontakt alarmowy, zgody RODO z wersją i datą akceptacji, zgoda na wizerunek z ATH-05, ustawienia kanałów powiadomień push, e-mail i SMS dla różnych typów zdarzeń, język i region konta, historia logowań, sekcja pobrania danych i usunięcia konta.

Komponenty interfejsu: formularz danych kontaktowych i startowych, lista zgód z przełącznikami i odnośnikiem do treści, macierz powiadomień typ zdarzenia względem kanału, wybór języka i regionu, przyciski eksportu danych i usunięcia konta z osobnym potwierdzeniem, sekcja danych zdrowotnych za osobnym wejściem.

Akcja główna: zapisz ustawienia. W sekcji zgód akcją wiodącą jest świadoma zmiana zgody z jasnym skutkiem.

Akcje drugorzędne: zmień język i region, ustaw kanały powiadomień, otwórz dane zdrowotne, pobierz swoje dane, przejrzyj historię logowań.

Akcje niszczące: usuń konto. Usunięcie wymaga wyraźnego potwierdzenia i wyjaśnia skutek dla aktywnych startów, salda portfela i paszportu, z informacją o danych zachowywanych przez organizatora dla rozliczeń zgodnie z prawem. Cofnięcie zgody wymaganej do startu tłumaczy, że może uniemożliwić udział.

Stany ekranu: ładowanie; pusty nie dotyczy, konto zawsze ma dane; wypełniony to pełne ustawienia; błąd zapisu przy danej sekcji tłumaczy przyczynę i zachowuje pozostałe; sukces z tostem; brak uprawnień dotyczy ograniczonego zakresu na koncie współdzielonym; offline pozwala przejrzeć ustawienia z danych lokalnych, a zmiany wymagają sieci.

Walidacja i komunikaty: adres lub telefon w złym formacie prosi o korektę. Wyłączenie kanału powiadomień dla zdarzeń krytycznych, jak oferta z listy rezerwowej albo ostrzeżenie pogodowe, ostrzega, że zawodnik może przegapić ważne sprawy. Cofnięcie zgody wymaganej do startu pokazuje, na co wpłynie. Usunięcie konta z aktywnym startem albo dodatnim saldem portfela ostrzega i proponuje najpierw rozliczenie i decyzję o startach.

Przypadki brzegowe: konto pełniące jednocześnie rolę zawodnika i rodzica, gdzie ustawienia obejmują oba moduły; zawodnik chcący usunąć konto przy nadchodzącym starcie i niewykorzystanym kredycie; zmiana regionu zmieniająca format kwot, dat i jednostek pogodowych w całym koncie; cofnięcie zgody na wizerunek wpływające na galerię ATH-11; eksport danych obejmujący starty u kilku organizatorów.

Przejścia i następne ekrany: zgody startowe łączą z ATH-05, zgoda na wizerunek rzutuje na ATH-11, kanały powiadomień rzutują na pulpit ATH-02 i powiadomienia pogodowe ATH-13, dane i dokumenty z portfela ATH-10, polityki prawne ze stron prawnych GST-08, ustawienia wspólne z PAR-16 na tym samym koncie.

Zdarzenia systemowe: zapis zmiany zgody z wersją i datą do rozliczalności, propagacja preferencji powiadomień do systemu wysyłki, przygotowanie paczki danych do pobrania, log zmian ustawień, sygnał o cofnięciu zgody wymaganej do startu.

Wymagania prawne: zgody RODO z wersją, datą i możliwością wycofania, prawo dostępu do danych i prawo do przenoszenia przez eksport, prawo do usunięcia z wyjaśnieniem danych zachowywanych dla obowiązków prawnych organizatora, dane zdrowotne jako szczególna kategoria za osobną zgodą, kontakt tylko kanałem objętym zgodą. Zmiana zgód i usunięcie konta pozostają świadomą decyzją zawodnika wykonywaną w panelu, nie automatem.

Kontekst urządzenia: telefon do szybkiej zmiany powiadomień i zgód, desktop do spokojnego przeglądu danych, eksportu i decyzji o usunięciu konta.

Lokalizacja i języki: etykiety jako klucze, treść zgód i polityk z fallbackiem języka, język i region konta ustawiane tutaj wraz z językiem zapasowym, format kwot, dat, jednostek i telefonu według regionu.

Lista kontrolna heurystyk: kontrola i swoboda przez świadome zarządzanie zgodami i powiadomieniami; widoczność stanu systemu przez wersję i datę każdej zgody oraz historię logowań; zapobieganie błędom przez ostrzeżenie o wyłączeniu krytycznych powiadomień i o skutkach usunięcia konta; pomoc i dokumentacja przez odnośniki do treści zgód i polityk; zgodność ze światem przez język praw zawodnika, nie żargon prawny bazy.

Metryki sukcesu: odsetek kont z aktualnymi zgodami, udział zawodników korzystających z eksportu danych, liczba przegapionych zdarzeń krytycznych po wyłączeniu kanału, odsetek poprawnie ustawionych kanałów powiadomień, liczba zgłoszeń pomocy dotyczących ustawień.

---

## Literatura

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

Wroblewski, L. (2008). *Web form design: Filling in the blanks*. Rosenfeld Media. https://rosenfeldmedia.com/books/web-form-design/

W3C. (2018). *Web Content Accessibility Guidelines (WCAG) 2.1*. World Wide Web Consortium. https://www.w3.org/TR/WCAG21/
