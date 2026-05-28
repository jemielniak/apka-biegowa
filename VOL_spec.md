# Specyfikacja ekranów grupy VOL - panel wolontariusza, moduł zawodów

Grupa VOL to panel wolontariusza, czyli osoby pomagającej w dniu zawodów. Persona pracuje na telefonie, ma dostęp jednorazowy, często nie przeszła żadnego szkolenia i bywa rodzicem, który zgłosił się przez swoje konto rodzinne. Z tego profilu wynikają trzy decyzje przekrojowe dla całej grupy. Logowanie ma być bezbolesne na starcie zmiany, najlepiej jednym skanem zaproszenia, bo nikt nie ma czasu na zakładanie konta przy bramie. Zakres danych jest minimalny i przypięty do jednego konkretnego wydarzenia - wolontariusz nie widzi listy wszystkich zawodów organizatora ani danych spoza swojego zadania. Skan kodu i wydanie właściwego pakietu w właściwym rozmiarze to operacja, w której pomyłka kosztuje najbardziej, więc projekt całej grupy podporządkowuje się jednemu celowi: zero pomyłek przy wydaniu.

Wszystkie ekrany działają w module `zawody`, rola dominująca to wolontariusz. Konto wolontariusza jest podrzędne wobec organizatora, tymczasowe i wygasa po wydarzeniu. Tworzy je organizator w OEV-25 albo powstaje przez most wolontariusz-rodzic z PAR-14. Wolontariusz nie zakłada konta sam.

Konwencje wspólne dla całej grupy, żeby nie powtarzać ich w każdym polu:

- Napisy widoczne dla użytkownika to klucze tłumaczeń ze znacznikami BCP 47, domyślnie pl-PL i en-GB. Daty, kwoty i numery telefonu formatuje się według ustawień regionalnych wydarzenia. Przełącznik języka stoi w nagłówku panelu, język zapasowy to en-GB przy braku tłumaczenia. Układ RTL włącza się automatycznie, jeśli aktywny język jest pisany od prawej do lewej.
- Tokeny marki pochodzą wyłącznie ze zmiennych CSS: `--brand-ink` i `--accent`, kolory semantyczne dla sukcesu, ostrzeżenia, błędu oraz informacji, para krojów (nagłówkowy wyróżniający i tekstowy czytelny, z dala od Inter i Arial), skala odstępów na jednej jednostce bazowej, jeden promień zaokrąglenia, jeden poziom cienia. Minimalny cel dotykowy i kontrast tekstu spełniają WCAG 2.1 na poziomie AA (W3C, 2018). Persona pracuje w słońcu, w rękawiczkach, w tłumie przy bramie, więc cel dotykowy projektuje się powyżej minimum WCAG, a nie na jego granicy, a kontrast podnosi się na pracę w pełnym słońcu.
- Lista kontrolna heurystyk odnosi się do dziesięciu heurystyk użyteczności (Nielsen, 1994).
- Wolontariusz nie widzi kwot, sald, faktur ani pełnych danych finansowych uczestnika. To twarda granica roli, nie ustawienie. Tam, gdzie zadanie wymaga wiedzy o opłacie - jak weryfikacja prawa do odbioru pakietu - panel pokazuje wyłącznie neutralny znacznik statusu (opłacone, nieopłacone, do wyjaśnienia), bez liczb. Rozstrzygnięcie sporu o płatność należy do koordynatora i warstwy organizatora w OEV-18.
- Urządzeniem jest telefon. Tablet jest przypadkiem brzegowym stanowiska stacjonarnego z dłuższą kolejką, desktop nie występuje. To zawężenie kontekstu mocniejsze niż w panelu instruktora.
- Tryb offline dotyczy odprawy i wydawania pakietów na starcie i przy bramie, gdzie zasięg bywa zerwany przez tłum. Dane wydania trafiają do kolejki lokalnej i synchronizują się po powrocie łącza, z widocznym znacznikiem stanu synchronizacji i zabezpieczeniem przed podwójnym wydaniem.
- Zakres każdego ekranu jest przypięty do jednego wydarzenia i jednego zadania. Wolontariusz przełączony na inne wydarzenie zaczyna od nowego zaproszenia, nie z menu.

---

## Partia 1 (VOL-01 do VOL-07)

### VOL-01 - logowanie lub dołączenie z zaproszenia

Grupa i rola: wolontariusz. Dostęp jednorazowy, tymczasowy, przypięty do jednego wydarzenia.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę wejść do panelu wolontariusza w kilka sekund na starcie zmiany, bez zakładania konta i bez wymyślania hasła, najlepiej skanując kod z zaproszenia, które dostałem od koordynatora.

Punkty wejścia: skan QR z zaproszenia wydrukowanego lub wyświetlonego przez koordynatora, link z zaproszenia w SMS lub e-mailu wysłanym z OEV-25, most wolontariusz-rodzic dla rodzica już zalogowanego na koncie rodzinnym po akceptacji zgłoszenia z PAR-14 i VOL-07, ikona aplikacji lub adres panelu przy ponownym wejściu tego samego dnia.

Warunki wstępne: organizator utworzył rolę wolontariusza i wysłał zaproszenie w OEV-25, albo rodzic zgłosił się w VOL-07 i zgłoszenie zostało zaakceptowane. Konto wolontariusza nie powstaje z inicjatywy osoby pomagającej.

Dane wyświetlane: logo wydarzenia i organizatora z tokenów marki, nazwa wydarzenia i jego data, rola i zakres dostępu opisane wprost (na przykład wydawanie pakietów, sektor B), opcja logowania bez hasła kodem lub linkiem, przełącznik języka, krótka nota, że dostęp wygasa po wydarzeniu. Bez jakichkolwiek danych finansowych i bez listy innych wydarzeń.

Komponenty interfejsu: duży skaner QR zaproszenia jako pierwszy element, pole kodu jednorazowego jako alternatywa, przycisk logowania bez hasła, przełącznik języka w nagłówku, odnośnik do kontaktu z koordynatorem. Brak formularza rejestracji - zakładanie konta jest po stronie organizatora.

Akcja główna: dołącz do wydarzenia przez skan zaproszenia lub kod jednorazowy. W moście rodzica dominującą akcją jest kontynuuj jako zalogowany rodzic, bez ponownego logowania.

Akcje drugorzędne: zmień język, poproś o nowy link lub kod, otwórz kontakt z koordynatorem.

Akcje niszczące: nie dotyczy. Wolontariusz nie usuwa konta ani dostępu, dostęp wygasa po wydarzeniu lub odwołuje go organizator.

Stany ekranu: ładowanie pokazuje wygaszony ekran wejścia; pusty nie dotyczy, ekran jest zawsze gotowy do skanu; wypełniony to gotowy skaner z polem kodu; błąd tłumaczy, czy problem dotyczy wygasłego zaproszenia, kodu już użytego, zaproszenia do innego wydarzenia, czy odwołanego dostępu, i podaje następny krok; sukces przenosi do VOL-02; brak uprawnień informuje, że zaproszenie zostało wycofane, i kieruje do koordynatora; offline pokazuje, że pierwsze wejście wymaga sieci, a po jednorazowym zalogowaniu odprawa i wydawanie działają lokalnie.

Walidacja i komunikaty: wygasłe zaproszenie proponuje poproszenie koordynatora o nowe i mówi, że stary kod już nie działa. Kod jednorazowy użyty na innym urządzeniu tłumaczy, że zaproszenie jest jednorazowe, i kieruje do koordynatora. Zaproszenie na inne wydarzenie albo inną datę pokazuje, do czego dotyczy, i nie wpuszcza do bieżącego. Zbyt wiele prób uruchamia chwilową blokadę z informacją, ile czasu pozostało.

Przypadki brzegowe: rodzic już zalogowany na koncie rodzinnym wchodzący mostem bez nowego logowania; wolontariusz przypisany do dwóch stanowisk tego samego wydarzenia wybierający stanowisko po wejściu; jedno zaproszenie współdzielone omyłkowo przez kilka osób, gdzie tylko pierwsze użycie jest ważne; dostęp wygasły po zakończeniu wydarzenia pokazujący komunikat o końcu zmiany; telefon bez aparatu lub bez zgody na kamerę przełączający się na ręczne wpisanie kodu.

Przejścia i następne ekrany: udane wejście prowadzi do VOL-02. Pierwsze wejście z zaproszenia prowadzi przez krótki ekran zgody na regulamin wolontariusza i politykę prywatności z zapisaną wersją dokumentu, a potem do VOL-02. Most rodzica łączy tożsamość z PAR-14 i prowadzi do VOL-02 z zakresem przypiętym do wydarzenia.

Zdarzenia systemowe: unieważnienie kodu po pierwszym użyciu, wygaśnięcie dostępu po zakończeniu wydarzenia, log udanych i nieudanych wejść do audytu organizatora, sygnał o powiązaniu konta rodzica z rolą wolontariusza przy moście.

Wymagania prawne: dane wejścia jako dane osobowe pod RODO w zakresie minimalnym do odprawy zmiany, zgoda na regulamin wolontariusza przy pierwszym wejściu z zapisaną wersją, dostęp czasowy wygasający po wydarzeniu jako realizacja zasady ograniczenia przechowywania. Tworzenie konta i nadanie dostępu pozostają po stronie organizatora.

Kontekst urządzenia: telefon wyłącznie, obsługa jedną ręką, skaner QR jako pierwszy sposób wejścia. Tablet stanowiska stacjonarnego jako przypadek brzegowy.

Lokalizacja i języki: etykiety jako klucze tłumaczeń, lista języków dziedziczona z ustawień wydarzenia w OEV-28, język zapasowy en-GB, nazwa wydarzenia jako treść organizatora.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania przez skan zaproszenia w miejsce hasła; pomoc w diagnozie błędu przez komunikat wskazujący przyczynę i krok naprawczy; widoczność stanu systemu przez pokazanie roli i zakresu od razu na wejściu; minimalistyczny projekt przez jedną dominującą akcję i brak rejestracji.

Metryki sukcesu: czas od skanu zaproszenia do pulpitu, odsetek wejść bez hasła, liczba zgłoszeń do koordynatora dotyczących logowania, odsetek wejść mostem rodzica względem zaproszeń bezpośrednich.

---

### VOL-02 - dashboard z zadaniami i zmianami

Grupa i rola: wolontariusz.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: po wejściu chcę od razu wiedzieć, co mam robić teraz i gdzie stać, i jednym dotknięciem zacząć bieżące zadanie, bez czytania instrukcji i bez szkolenia.

Punkty wejścia: udane wejście z VOL-01, link z powiadomienia push o rozpoczęciu zmiany, powrót z ekranu odprawy, wydawania pakietów albo listy startowej.

Warunki wstępne: wolontariusz ma przypisaną przynajmniej jedną zmianę lub zadanie na bieżące wydarzenie w OEV-25.

Dane wyświetlane: bieżące lub najbliższe zadanie wyróżnione na górze z godziną, stanowiskiem i sektorem, krótka instrukcja stanowiska napisana prostym językiem, lista pozostałych zadań na dziś, kontakt do koordynatora jednym dotknięciem, znacznik stanu synchronizacji offline, krótkie alerty operacyjne takie jak przesunięcie bramy albo brak rozmiaru w magazynie. Bez danych finansowych, bez pełnej listy uczestników, bez danych spoza zadania.

Komponenty interfejsu: duża karta bieżącego zadania z jednym przyciskiem startu, oś dnia z kolejnymi zadaniami, przycisk połączenia lub wiadomości do koordynatora, znacznik trybu offline z liczbą operacji w kolejce, lista krótkich alertów z priorytetem.

Akcja główna: rozpocznij bieżące zadanie. Jedna dominująca akcja prowadzi prosto do właściwego ekranu roboczego, najczęściej do odprawy VOL-04 albo wydawania pakietów VOL-05.

Akcje drugorzędne: otwórz harmonogram zmian, zadzwoń lub napisz do koordynatora, podejrzyj instrukcję stanowiska, przełącz na inne przypisane zadanie.

Akcje niszczące: nie dotyczy.

Stany ekranu: ładowanie pokazuje szkielet karty zadania; pusty wita wolontariusza bez przypisanych zadań i kieruje do koordynatora; wypełniony to karta bieżącego zadania z osią dnia; błąd ładowania pokazuje komunikat z ponowieniem, a jeśli są dane lokalne, pokazuje je z banerem nieświeżości; sukces nie dotyczy osobno; brak uprawnień informuje o braku przypisanych zadań na to wydarzenie; offline pokazuje ostatnio zsynchronizowany plan zmiany z banerem, że odprawa i wydawanie i tak działają lokalnie.

Walidacja i komunikaty: nieudane pobranie planu mówi, że nie udało się odświeżyć zadań, i proponuje pracę na ostatnich danych. Brak przypisanego zadania w danej chwili pokazuje najbliższą zmianę i godzinę jej startu. Zmiana stanowiska wprowadzona przez koordynatora w trakcie zmiany pojawia się po odświeżeniu z krótką notą, co się zmieniło.

Przypadki brzegowe: wolontariusz między zmianami z przerwą; zadanie odwołane przez koordynatora widoczne jako przekreślone z powodem; wolontariusz na dwóch stanowiskach w tym samym czasie z ostrzeżeniem i kierowaniem do koordynatora; rodzic wolontariusz, który w tym samym czasie chce wejść w panel rodzica, przełączający kontekst bez utraty roli wolontariusza na zmianie.

Przejścia i następne ekrany: karta zadania prowadzi do odprawy VOL-04 albo wydawania pakietów VOL-05, skrót harmonogramu do VOL-03, kontekst listy startowej do VOL-06, kontakt do koordynatora poza panelem kanałem zdefiniowanym w OEV-25.

Zdarzenia systemowe: odświeżanie planu zmiany przy wejściu i co kilka minut, push o przesunięciu bramy lub odwołaniu zadania, sygnał o zakończeniu synchronizacji danych offline, log wejścia w zadanie.

Wymagania prawne: pulpit pokazuje dane organizacyjne zmiany i instrukcję stanowiska, dane uczestników pojawiają się dopiero po wejściu w odprawę lub wydawanie. Minimalizacja danych pod RODO na widoku startowym, zakres przypięty do jednego wydarzenia.

Kontekst urządzenia: telefon jako jedyne urządzenie, duży przycisk startu zadania dostępny kciukiem jednej ręki, kontrast podniesiony na pracę w słońcu. Tablet stanowiska jako przypadek brzegowy.

Lokalizacja i języki: godziny i nazwy stanowisk według regionu i treści organizatora, instrukcja stanowiska jako treść organizatora z fallbackiem języka, etykiety jako klucze tłumaczeń.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania, bo bieżące zadanie i jego instrukcja są na wierzchu; widoczność stanu systemu przez stan synchronizacji i alerty operacyjne; zgodność ze światem przez język zadania zrozumiały dla osoby bez szkolenia; minimalistyczny projekt przez ograniczenie pulpitu do tego, co dzieje się teraz.

Metryki sukcesu: czas od wejścia do rozpoczęcia bieżącego zadania, odsetek zadań startowanych z poziomu pulpitu, liczba zgłoszeń do koordynatora o niejasne zadanie, odsetek wolontariuszy korzystających z instrukcji stanowiska.

---

### VOL-03 - harmonogram zmian

Grupa i rola: wolontariusz. Odczytowe odbicie przydziału zmian, którego właścicielem jest organizator w OEV-25.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę zobaczyć swoje zmiany na to wydarzenie, tylko swoje, żeby wiedzieć, kiedy przyjść, gdzie stać i kiedy mam przerwę.

Punkty wejścia: skrót harmonogramu z pulpitu VOL-02, menu panelu, link z powiadomienia o zmianie przydziału.

Warunki wstępne: wolontariusz ma przypisane zmiany na bieżące wydarzenie w OEV-25.

Dane wyświetlane: lista własnych zmian z godziną początku i końca, stanowiskiem i sektorem, oznaczenie przerw, oznaczenie zmian odwołanych lub przesuniętych, kontakt do koordynatora danej zmiany. Bez zmian innych wolontariuszy, bo to nie panel organizatora, i bez danych finansowych.

Komponenty interfejsu: lista zmian uporządkowana po godzinie, karta szczegółu zmiany z przyciskiem przejścia do zadania, znacznik bieżącej zmiany, przycisk zgłoszenia, że nie dam rady na zmianę, dodanie zmiany do kalendarza urządzenia.

Akcja główna: otwórz bieżącą lub najbliższą zmianę. Akcja wiodąca prowadzi do zadania zmiany, najczęściej odprawy lub wydawania pakietów.

Akcje drugorzędne: dodaj zmianę do kalendarza urządzenia, zgłoś niedostępność na wskazaną zmianę, skontaktuj się z koordynatorem, przełącz na inną własną zmianę.

Akcje niszczące: nie dotyczy. Wolontariusz nie usuwa ani nie przesuwa zmian, robi to organizator w OEV-25.

Stany ekranu: ładowanie z szarą listą; pusty pokazuje brak przypisanych zmian i kieruje do koordynatora; wypełniony to lista zmian; błąd ładowania z ponowieniem lub pracą na danych lokalnych; sukces nie dotyczy osobno; brak uprawnień nie dotyczy, bo widok jest własny; offline pokazuje ostatnio zsynchronizowany harmonogram z banerem o wstrzymanej aktualizacji.

Walidacja i komunikaty: nieudane odświeżenie tłumaczy, że plan może być nieaktualny, i proponuje ponowienie. Próba edycji zmiany wyjaśnia, że przydział należy do koordynatora, i proponuje zgłoszenie niedostępności jako właściwą ścieżkę. Zgłoszenie niedostępności blisko startu zmiany ostrzega o krótkim wyprzedzeniu i prosi o kontakt telefoniczny z koordynatorem.

Przypadki brzegowe: dwie zmiany nakładające się po zmianie przydziału pokazane jako konflikt do zgłoszenia; zmiana przez północ przy nocnej części wydarzenia; wolontariusz bez żadnej zmiany w danym dniu wydarzenia wielodniowego; przesunięcie zmiany przez organizatora widoczne dopiero po synchronizacji.

Przejścia i następne ekrany: karta zmiany prowadzi do zadania w VOL-04 albo VOL-05, zgłoszenie niedostępności trafia do organizatora w OEV-25, przesunięcie zmiany po stronie organizatora odbija się tu po synchronizacji.

Zdarzenia systemowe: synchronizacja z przydziałem zmian z OEV-25, push o odwołaniu lub przesunięciu zmiany, aktualizacja po zatwierdzeniu zgłoszonej niedostępności.

Wymagania prawne: harmonogram zawiera dane organizacyjne wolontariusza bez danych uczestników, eksport zmiany do kalendarza urządzenia tylko na działanie wolontariusza, dane przypięte do jednego wydarzenia pod RODO.

Kontekst urządzenia: telefon jako jedyne urządzenie, lista czytelna jedną ręką. Tablet jako przypadek brzegowy.

Lokalizacja i języki: godziny i nazwy dni według regionu, nazwy stanowisk jako treść organizatora, etykiety jako klucze tłumaczeń.

Lista kontrolna heurystyk: widoczność stanu systemu przez wyraźne oznaczenie bieżącej zmiany; kontrola i swoboda przez jasny podział na to, co wolontariusz może, a co zgłasza; zgodność ze światem przez godziny i nazwy z życia wydarzenia; spójność z przydziałem organizatora przez te same oznaczenia odwołań i przesunięć.

Metryki sukcesu: odsetek wolontariuszy otwierających harmonogram przed pierwszą zmianą, liczba zgłoszeń niedostępności z tego ekranu, liczba nieporozumień o godzinie lub stanowisku zgłoszonych do koordynatora, odsetek zmian dodanych do kalendarza urządzenia.

---

### VOL-04 - ekran odprawy ze skanem QR

Grupa i rola: wolontariusz.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę szybko odprawić uczestnika skanem jego kodu, w sekundę zobaczyć, czy wszystko się zgadza, i puścić go dalej, także bez zasięgu przy bramie.

Punkty wejścia: przycisk startu odprawy z pulpitu VOL-02, karta zmiany z harmonogramu VOL-03, przejście z listy startowej VOL-06, powrót z wydawania pakietów VOL-05.

Warunki wstępne: lista uczestników odprawy pobrana na urządzenie przed wejściem na stanowisko, żeby tryb offline miał na czym pracować, numery startowe nadane w OEV-14.

Dane wyświetlane: pole skanu jako element wiodący, wynik skanu w dużym formacie z czytelnym oznaczeniem zielonym lub czerwonym, imię uczestnika i numer startowy, znacznik zgody do odebrania, neutralny znacznik statusu opłaty bez kwoty, licznik odprawionych względem oczekiwanych, stan synchronizacji offline. Bez kwot, sald i faktur, zgodnie z granicą roli.

Komponenty interfejsu: duży skaner QR z numeru startowego, paszportu lub potwierdzenia zapisu, pole ręcznego wyszukania po numerze lub nazwisku na wypadek uszkodzonego kodu, wynik skanu zajmujący większość ekranu, przycisk potwierdzenia odprawy, baner trybu offline z liczbą operacji w kolejce.

Akcja główna: odpraw uczestnika po udanym skanie. Wynik prowadzi wprost do potwierdzenia, a przy wydarzeniach z pakietem do przejścia na wydawanie w VOL-05.

Akcje drugorzędne: wyszukaj ręcznie, skanuj kolejnego, oznacz do wyjaśnienia i skieruj do koordynatora, dodaj krótką notatkę do odprawy.

Akcje niszczące: nie dotyczy w sensie kasowania danych. Cofnięcie błędnej odprawy jest zwykłym przełączeniem stanu, nie operacją nieodwracalną.

Stany ekranu: ładowanie listy odprawy ze szkieletem; pusty oznacza stanowisko bez pobranej listy i tłumaczy, że bez listy nie ma czego odprawiać; wypełniony to gotowy skaner z licznikiem; błąd synchronizacji nie blokuje pracy, dane zostają w kolejce i ekran pokazuje, ile pozycji czeka; sukces potwierdza odprawę dużym znacznikiem; brak uprawnień nie dotyczy, bo to przypisane stanowisko; offline jest stanem pierwszej klasy, skan i odprawa działają w całości lokalnie, a baner pokazuje tryb i moment ostatniej synchronizacji.

Walidacja i komunikaty: skan kodu spoza tego wydarzenia tłumaczy, że uczestnik nie należy do tej odprawy. Uczestnik już odprawiony pokazuje, że to powtórny skan, i podaje godzinę pierwszej odprawy, żeby uniknąć cichego dublowania. Status opłaty wymagający wyjaśnienia kieruje do koordynatora bez pokazywania kwoty. Skan kodu uszkodzonego proponuje wyszukanie ręczne. Konflikt dwóch urządzeń odprawiających tę samą bramę pokazuje, które odprawy weszły, i prosi o przegląd rozbieżności.

Przypadki brzegowe: kod startowy uszkodzony lub zalany wodą wymagający wpisania numeru ręcznie; uczestnik bez kodu z potwierdzeniem na inny dokument kierowany do koordynatora; rodzeństwo o tym samym nazwisku wymagające rozróżnienia po numerze; uczestnik dopisany z listy rezerwowej tuż przed startem, którego nie ma w pobranej liście, pojawiający się po synchronizacji; przerwana sesja po wyłączeniu telefonu wznawiana z danych lokalnych.

Przejścia i następne ekrany: udana odprawa otwiera wydawanie pakietu w VOL-05 dla wydarzeń z pakietem, status uczestnika zasila listy startowe organizatora w OEV-15, wiersz uczestnika łączy z listą startową w VOL-06, przypadek do wyjaśnienia kieruje do kontaktu z koordynatorem z OEV-25.

Zdarzenia systemowe: zapis lokalny przy każdej odprawie, synchronizacja w tle po wykryciu sieci, sygnał konfliktu przy równoległej odprawie z dwóch urządzeń, aktualizacja licznika odprawionych po stronie organizatora.

Wymagania prawne: imię i numer startowy jako dane osobowe pod RODO w zakresie potrzebnym do odprawy, dane przechowywane lokalnie na czas offline w postaci ograniczonej do potrzeby odprawy, brak danych finansowych poza neutralnym znacznikiem statusu, dostęp tylko dla przypisanego wolontariusza i organizatora, dane usuwane po wygaśnięciu dostępu.

Kontekst urządzenia: telefon jako jedyne urządzenie, skaner i wynik powiększone na obsługę jedną ręką w tłumie, kontrast podniesiony na pracę w pełnym słońcu, cele dotykowe powyżej minimum WCAG na obsługę w rękawiczkach. Tablet stanowiska z kolejką jako przypadek brzegowy.

Lokalizacja i języki: imiona uczestników jako dane, etykiety wyniku skanu jako klucze tłumaczeń, format godziny odprawy według regionu.

Lista kontrolna heurystyk: widoczność stanu systemu przez wynik skanu, licznik i znacznik synchronizacji; zapobieganie błędom przez ostrzeżenie o powtórnym skanie i jasny wynik zielony lub czerwony; kontrola i swoboda przez łatwe cofnięcie błędnej odprawy; elastyczność przez wyszukanie ręczne obok skanu; pomoc w diagnozie przez czytelny komunikat trybu offline i przypadku do wyjaśnienia.

Metryki sukcesu: mediana czasu jednej odprawy, odsetek odpraw wykonanych offline i poprawnie zsynchronizowanych, liczba powtórnych skanów zatrzymanych przed dublowaniem, liczba przypadków skierowanych do koordynatora, odsetek odpraw przez skan względem wyszukania ręcznego.

---

### VOL-05 - wydawanie pakietów i gadżetów

Grupa i rola: wolontariusz. Korzysta z magazynu gadżetów i kalkulatora zdefiniowanego przez organizatora w OEV-11.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę wydać właściwy pakiet startowy z właściwym rozmiarem koszulki, bez pomyłki w rozmiarze i bez wydania dwa razy temu samemu uczestnikowi, możliwie szybko, także bez zasięgu.

Punkty wejścia: przejście z udanej odprawy w VOL-04, przycisk startu wydawania z pulpitu VOL-02, karta zmiany z harmonogramu VOL-03.

Warunki wstępne: pakiety i ich zawartość zdefiniowane w OEV-06, magazyn gadżetów z rozmiarami i stanami w OEV-11, lista uprawnionych do odbioru pobrana na urządzenie przed wejściem na stanowisko.

Dane wyświetlane: po skanie duży, jednoznaczny opis pakietu należnego uczestnikowi z wyróżnionym rozmiarem koszulki, zawartość pakietu jako lista pozycji, znacznik, czy pakiet został już wydany, stan magazynu rozmiaru w skrócie, licznik wydanych pakietów, stan synchronizacji offline. Bez kwot i danych finansowych.

Komponenty interfejsu: skaner QR jako wejście, karta pakietu z wielkim, kontrastowym oznaczeniem rozmiaru, lista zawartości z polami do odhaczenia, przycisk potwierdzenia wydania, opcja zmiany rozmiaru w granicach reguł organizatora, baner trybu offline z liczbą wydania w kolejce, skrót do zgłoszenia braku rozmiaru.

Akcja główna: potwierdź wydanie pakietu. To jedna dominująca akcja, poprzedzona wyraźnym pokazaniem rozmiaru, żeby zminimalizować pomyłkę.

Akcje drugorzędne: zmień rozmiar w granicach reguł, zgłoś brak rozmiaru w magazynie, dodaj notatkę do wydania, skanuj kolejnego uczestnika.

Akcje niszczące: cofnięcie błędnego wydania. Z potwierdzeniem i powodem, bo cofnięcie zwraca pozycję do magazynu i zmienia licznik, więc nie jest zwykłym przełącznikiem jak przy odprawie.

Stany ekranu: ładowanie listy uprawnionych ze szkieletem; pusty oznacza stanowisko bez pobranej listy lub bez zdefiniowanych pakietów i tłumaczy, czego brakuje; wypełniony to gotowy skaner z kartą pakietu po skanie; błąd synchronizacji nie blokuje pracy, wydanie trafia do kolejki, a ekran pokazuje, ile pozycji czeka; sukces potwierdza wydanie dużym znacznikiem z rozmiarem; brak uprawnień nie dotyczy, bo to przypisane stanowisko; offline jest stanem pierwszej klasy, wydawanie działa lokalnie z zabezpieczeniem przed podwójnym wydaniem na tym urządzeniu.

Walidacja i komunikaty: pakiet już wydany pokazuje to wyraźnie z godziną pierwszego wydania i prosi o potwierdzenie przed ewentualnym ponownym wydaniem, żeby zatrzymać dublowanie. Brak rozmiaru w magazynie proponuje najbliższy dostępny rozmiar zgodnie z regułami organizatora albo zgłoszenie braku, zamiast cichego wydania złego rozmiaru. Zmiana rozmiaru poza regułami tłumaczy granicę i kieruje do koordynatora. Próba wydania uczestnikowi nieodprawionemu przypomina o odprawie w VOL-04. Konflikt dwóch urządzeń wydających temu samemu uczestnikowi pokazuje rozbieżność do przeglądu.

Przypadki brzegowe: uczestnik proszący o inny rozmiar niż zamówiony przy zapisie; brak rozmiaru w magazynie w szczycie kolejki; pakiet z pozycją limitowaną wydawaną tylko części uczestników; odbiór pakietu przez osobę upoważnioną w imieniu uczestnika za zgodą reguł organizatora; cofnięcie wydania po pomyłce w rozmiarze z zwrotem pozycji do magazynu; rodzeństwo z różnymi pakietami wymagające rozróżnienia.

Przejścia i następne ekrany: wydanie zmniejsza stan magazynu w OEV-11 i zasila statystyki wydań, status odbioru łączy się z listą startową w VOL-06 i z listami startowymi organizatora w OEV-15, zgłoszenie braku rozmiaru kieruje do koordynatora z OEV-25, wejście z odprawy wraca do VOL-04 dla kolejnego uczestnika.

Zdarzenia systemowe: zapis lokalny przy każdym wydaniu, synchronizacja w tle po wykryciu sieci, dekrementacja magazynu po stronie organizatora po synchronizacji, sygnał konfliktu przy równoległym wydaniu, alert o niskim stanie rozmiaru z OEV-11.

Wymagania prawne: dane potrzebne do wydania w zakresie minimalnym pod RODO, brak danych finansowych na ekranie, notatka do wydania bez danych wrażliwych, log wydań do rozliczalności magazynu, dane usuwane po wygaśnięciu dostępu.

Kontekst urządzenia: telefon jako jedyne urządzenie, oznaczenie rozmiaru w największym możliwym formacie i kontraście, bo to punkt, w którym pomyłka kosztuje najwięcej, cele dotykowe powyżej minimum WCAG. Tablet stanowiska z kolejką jako przypadek brzegowy.

Lokalizacja i języki: nazwy pakietów i pozycji jako treść organizatora z fallbackiem języka, oznaczenia rozmiarów spójne z OEV-11, etykiety jako klucze tłumaczeń.

Lista kontrolna heurystyk: zapobieganie błędom przez wyróżnienie rozmiaru przed potwierdzeniem i ostrzeżenie o ponownym wydaniu; widoczność stanu systemu przez licznik wydań i stan magazynu; kontrola i swoboda przez możliwość cofnięcia z powodem; zgodność ze światem przez prosty opis pakietu zrozumiały bez szkolenia; pomoc w diagnozie przez jasny komunikat o braku rozmiaru i propozycję następnego kroku.

Metryki sukcesu: odsetek wydań z poprawnym rozmiarem mierzony liczbą cofnięć z powodu rozmiaru, mediana czasu jednego wydania, liczba zatrzymanych podwójnych wydań, odsetek wydań offline poprawnie zsynchronizowanych, liczba zgłoszeń braku rozmiaru w szczycie.

---

### VOL-06 - lista startowa z weryfikacją opłaty

Grupa i rola: wolontariusz. Odczytowe odbicie listy startowej, której właścicielem jest organizator w OEV-15, ze statusem opłaty sprowadzonym do neutralnego znacznika zgodnie z granicą roli.

Moduł: zawody.

Cel ekranu jako zadanie użytkownika: chcę sprawdzić, czy uczestnik ma prawo odebrać pakiet i wystartować, czyli czy jest opłacony i odprawiony, bez wglądu w kwoty i bez rozstrzygania sporów o pieniądze.

Punkty wejścia: przejście z odprawy VOL-04, przejście z wydawania pakietów VOL-05, przycisk listy startowej z pulpitu VOL-02.

Warunki wstępne: lista startowa wygenerowana w OEV-15, numery startowe nadane w OEV-14, status opłaty sprowadzony do neutralnego znacznika po stronie systemu, lista pobrana na urządzenie na czas offline.

Dane wyświetlane: lista uczestników z imieniem i numerem startowym, neutralny znacznik statusu opłaty jako opłacone, nieopłacone lub do wyjaśnienia, znacznik odprawy, znacznik odbioru pakietu, pole szybkiego wyszukania, licznik gotowych do startu. Bez kwot, sald, metod płatności i faktur.

Komponenty interfejsu: lista z dużymi wierszami i czytelnymi znacznikami statusu w kolorach semantycznych, pole wyszukania po numerze lub nazwisku, filtr po statusie, karta uczestnika w trybie odczytu z neutralnym statusem opłaty, przycisk skierowania przypadku do koordynatora.

Akcja główna: znajdź uczestnika i potwierdź jego status do startu. Akcja wiodąca prowadzi do odprawy w VOL-04 albo wydania pakietu w VOL-05, jeśli status na to pozwala.

Akcje drugorzędne: filtruj po statusie, wyszukaj uczestnika, skieruj przypadek do wyjaśnienia do koordynatora, podejrzyj kartę uczestnika w odczycie.

Akcje niszczące: nie dotyczy. Wolontariusz nie zmienia statusu opłaty, robi to warstwa organizatora w OEV-18.

Stany ekranu: ładowanie listy ze szkieletem; pusty oznacza brak pobranej listy i tłumaczy, że bez niej nie ma czego weryfikować; wypełniony to lista ze znacznikami; błąd ładowania z ponowieniem lub pracą na danych lokalnych z banerem nieświeżości; sukces nie dotyczy osobno; brak uprawnień nie dotyczy poza zawężeniem do własnego stanowiska; offline pokazuje ostatnio zsynchronizowaną listę z banerem, że status mógł się zmienić po stronie organizatora.

Walidacja i komunikaty: uczestnik ze statusem nieopłacone nie blokuje wprost, tylko pokazuje czerwony znacznik i kieruje do koordynatora, bo wolontariusz nie rozstrzyga płatności. Status do wyjaśnienia tłumaczy, że sprawę prowadzi koordynator, i nie pokazuje kwoty. Praca na danych offline ostrzega, że status opłaty mógł się zmienić od ostatniej synchronizacji, i proponuje potwierdzenie u koordynatora w razie wątpliwości. Wyszukanie bez wyniku proponuje sprawdzenie pisowni nazwiska albo numeru.

Przypadki brzegowe: uczestnik opłacony tuż przed startem, którego status nie zdążył się zsynchronizować offline; uczestnik z listy rezerwowej dopisany w ostatniej chwili w OEV-17; spór o płatność przy bramie kierowany w całości do koordynatora; uczestnik z numerem startowym, ale bez odprawy; dwoje uczestników o tym samym nazwisku rozróżnianych po numerze.

Przejścia i następne ekrany: status uczestnika wynika z list startowych OEV-15 i z warstwy płatności OEV-18, wiersz prowadzi do odprawy VOL-04 albo wydania pakietu VOL-05, przypadek do wyjaśnienia kieruje do koordynatora z OEV-25, dopisani z listy rezerwowej pochodzą z OEV-17.

Zdarzenia systemowe: synchronizacja listy i statusów po wykryciu sieci, aktualizacja znacznika opłaty po zmianie w warstwie organizatora, sygnał o dopisaniu z listy rezerwowej, log skierowań do koordynatora.

Wymagania prawne: imię, numer startowy i znacznik statusu jako dane osobowe pod RODO w zakresie minimalnym do weryfikacji prawa do startu, twardy zakaz pokazywania kwot, sald i faktur wolontariuszowi, dane przypięte do jednego wydarzenia i usuwane po wygaśnięciu dostępu, status opłaty jako informacja niezbędna do zadania, nie jako pełny obraz finansowy uczestnika.

Kontekst urządzenia: telefon jako jedyne urządzenie, znaczniki statusu w kolorach semantycznych czytelne w słońcu, lista obsługiwana jedną ręką. Tablet stanowiska jako przypadek brzegowy.

Lokalizacja i języki: imiona uczestników jako dane, etykiety statusu jako klucze tłumaczeń w kolorach semantycznych, format numeru i daty według regionu.

Lista kontrolna heurystyk: widoczność stanu systemu przez czytelne znaczniki opłaty, odprawy i odbioru; zapobieganie błędom przez kierowanie sporów o płatność do koordynatora zamiast rozstrzygania ich przez wolontariusza; zgodność ze światem przez prosty podział opłacone, nieopłacone, do wyjaśnienia; minimalistyczny projekt przez ukrycie wszystkich danych finansowych poza jednym znacznikiem; pomoc w diagnozie przez ostrzeżenie o możliwej nieświeżości statusu offline.

Metryki sukcesu: odsetek uczestników zweryfikowanych bez kierowania do koordynatora, mediana czasu weryfikacji jednego uczestnika, liczba sporów o płatność rozwiązanych poza panelem wolontariusza, liczba przypadków pracy na nieświeżym statusie offline.

---

### VOL-07 - zgłoszenie się na wolontariat przez most wolontariusz-rodzic

Grupa i rola: rodzic występujący jako kandydat na wolontariusza. Ekran łączy konto rodzinne z PAR-01 z rolą wolontariusza, zgłoszenie zatwierdza organizator w OEV-25.

Moduł: zawody. Most łączy moduł zawodów z kontem rodzinnym wspólnym dla obu modułów.

Cel ekranu jako zadanie użytkownika: jestem rodzicem zalogowanym na koncie rodzinnym i chcę zgłosić się jako wolontariusz na konkretne zawody mojego dziecka, bez zakładania osobnego konta, żeby po akceptacji wejść w panel wolontariusza tym samym logowaniem.

Punkty wejścia: skrót zgłoszenia z panelu rodzica PAR-14, brandowana strona wydarzenia z zaproszeniem dla wolontariuszy z GST-02, link z komunikacji organizatora, kafel na dashboardzie rodzica PAR-02 przy wydarzeniu, na które dziecko jest zapisane.

Warunki wstępne: aktywne konto rodzinne z PAR-01, wydarzenie z otwartym naborem wolontariuszy w OEV-25, zdefiniowane stanowiska i zmiany do wyboru.

Dane wyświetlane: nazwa wydarzenia i data, opis poszukiwanych stanowisk i zmian z OEV-25, dostępność zmian, krótka informacja o zakresie roli wolontariusza i o tym, że dostęp jest jednorazowy i wygasa po wydarzeniu, dane rodzica dziedziczone z konta rodzinnego w odczycie, zgoda na regulamin wolontariusza. Bez danych innych kandydatów.

Komponenty interfejsu: lista stanowisk i zmian z możliwością wyboru, pole preferencji i uwag, podgląd zakresu roli, dziedziczone dane kontaktowe rodzica w odczycie z możliwością korekty, pole zgody na regulamin wolontariusza, przycisk wysłania zgłoszenia.

Akcja główna: wyślij zgłoszenie na wybrane stanowisko i zmianę. Po akceptacji organizatora rola wolontariusza dopina się do konta rodzinnego, a wejście odbywa się przez most w VOL-01.

Akcje drugorzędne: zmień wybór stanowiska lub zmiany, dodaj uwagę dla koordynatora, zapisz wersję roboczą zgłoszenia, wycofaj wysłane zgłoszenie przed akceptacją.

Akcje niszczące: wycofanie wysłanego zgłoszenia przed akceptacją. Z potwierdzeniem, bo zwalnia zarezerwowane miejsce na zmianie i informuje organizatora.

Stany ekranu: ładowanie listy stanowisk ze szkieletem; pusty oznacza brak otwartego naboru i proponuje obserwowanie wydarzenia; wypełniony to formularz wyboru zmiany; błąd wysyłki tłumaczy przyczynę i proponuje ponowienie; sukces potwierdza przyjęcie zgłoszenia i wyjaśnia, że czeka na akceptację organizatora; brak uprawnień informuje, że nabór jest zamknięty albo wymaga zaproszenia; offline pozwala przygotować zgłoszenie i zakolejkować je, wysyłka rusza po sieci.

Walidacja i komunikaty: brak wyboru zmiany blokuje wysłanie i prosi o wskazanie terminu. Zmiana, która właśnie się zapełniła, pokazuje to i proponuje inną dostępną. Brak zgody na regulamin wolontariusza blokuje wysłanie z wyjaśnieniem. Zgłoszenie na zmianę kolidującą z udziałem dziecka w starcie ostrzega o kolizji, żeby rodzic nie przegapił biegu dziecka. Powtórne zgłoszenie na to samo wydarzenie pokazuje status poprzedniego.

Przypadki brzegowe: rodzic dwojga dzieci startujących w różnych blokach godzinowych wybierający zmianę między startami; rodzic zgłaszający się na zawody, na które dziecko nie jest zapisane; nabór zamknięty w trakcie wypełniania formularza; rodzic bez konta rodzinnego kierowany najpierw do rejestracji w PAR-01; akceptacja zgłoszenia po stronie organizatora dopinająca rolę bez tworzenia osobnego konta.

Przejścia i następne ekrany: zgłoszenie trafia do zarządzania wolontariuszami organizatora w OEV-25, akceptacja dopina rolę do konta rodzinnego i otwiera wejście mostem w VOL-01, status zgłoszenia jest widoczny w panelu rodzica PAR-14, kolizja z udziałem dziecka odsyła do zapisów dziecka w PAR-11.

Zdarzenia systemowe: wysłanie zgłoszenia do organizatora, rezerwacja miejsca na zmianie na czas rozpatrzenia, push lub e-mail o akceptacji albo odrzuceniu, dopięcie roli wolontariusza do konta rodzinnego po akceptacji, log zgłoszeń do audytu organizatora.

Wymagania prawne: dane rodzica dziedziczone z konta rodzinnego pod RODO w zakresie potrzebnym do roli wolontariusza, zgoda na regulamin wolontariusza z zapisaną wersją dokumentu, jasna informacja o tymczasowości i zakresie dostępu, brak zbierania danych innych kandydatów, powiązanie roli z istniejącym kontem zamiast tworzenia nowego jako realizacja minimalizacji danych.

Kontekst urządzenia: telefon jako podstawowy przy zgłoszeniu w ruchu, tablet lub większy ekran do spokojnego wyboru zmian w domu jako przypadek wygodniejszy, bo to ekran planowania, a nie pracy przy bramie.

Lokalizacja i języki: nazwy stanowisk i opis naboru jako treść organizatora z fallbackiem języka, etykiety jako klucze tłumaczeń, daty i godziny zmian według regionu wydarzenia.

Lista kontrolna heurystyk: zgodność ze światem przez opis stanowisk zrozumiały dla rodzica bez doświadczenia w wolontariacie; zapobieganie błędom przez ostrzeżenie o kolizji zmiany ze startem dziecka; rozpoznawanie zamiast przypominania przez dziedziczone dane konta rodzinnego; kontrola i swoboda przez możliwość wycofania zgłoszenia przed akceptacją; widoczność stanu systemu przez jasny status zgłoszenia po wysłaniu.

Metryki sukcesu: odsetek zgłoszeń zakończonych wysłaniem, odsetek zgłoszeń zaakceptowanych przez organizatora, udział wejść mostem rodzica względem zaproszeń bezpośrednich w VOL-01, liczba kolizji zmiany ze startem dziecka wykrytych przed wysłaniem, czas od akceptacji do pierwszego wejścia w panel wolontariusza.

---

## Literatura

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

W3C. (2018). *Web Content Accessibility Guidelines (WCAG) 2.1*. World Wide Web Consortium. https://www.w3.org/TR/WCAG21/
