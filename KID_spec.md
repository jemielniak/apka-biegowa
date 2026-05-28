# Specyfikacja ekranów grupy KID - małoletni uczestnik

Grupa KID obejmuje ekrany dotyczące małoletniego uczestnika, który domyślnie nie ma własnego logowania i jest profilem w koncie rodzica opisanym w grupie PAR. Persona to dziecko zapisane na zajęcia albo zawody, którego dane przetwarza opiekun. Priorytetem grupy jest poprawna obsługa zgody opiekuna realizowanej z konta dorosłego, bo to ona warunkuje legalność przetwarzania danych dziecka w usłudze świadczonej drogą elektroniczną. Drugi ekran to opcjonalny, ograniczony widok dla nastolatka, wyłącznie do odczytu paszportu, postępów i zdjęć, bez płatności, zgód i zapisów.

Grupa KID ma odrębny status wobec pozostałych. Każdy jej ekran wymaga osobnej decyzji prawnej co do wieku zgody i sposobu przetwarzania danych dziecka, zanim trafi do wdrożenia. W polskim porządku prawnym granica wieku dla samodzielnej zgody na usługi społeczeństwa informacyjnego wynosi 16 lat, bo ustawodawca nie skorzystał z możliwości obniżenia jej do 13 lat, którą daje RODO (Ustawa o ochronie danych osobowych, 2018; RODO, 2016). Poniżej tej granicy zgodę wyraża albo autoryzuje opiekun. Z tego powodu pole "decyzja prawna przed wdrożeniem" jest w tej grupie polem obowiązkowym, a nie formalnością.

Konwencje wspólne dla całej grupy, żeby nie powtarzać ich w każdym polu:

- Napisy widoczne dla użytkownika to klucze tłumaczeń ze znacznikami BCP 47, domyślnie pl-PL i en-GB. Daty, kwoty i numery telefonu formatuje się według ustawień regionalnych konta. Przełącznik języka stoi w nagłówku, język zapasowy to en-GB przy braku tłumaczenia. Układ RTL włącza się automatycznie, jeśli aktywny język jest pisany od prawej do lewej.
- Tokeny marki pochodzą wyłącznie ze zmiennych CSS: `--brand-ink` i `--accent`, kolory semantyczne dla sukcesu, ostrzeżenia, błędu oraz informacji, para krojów (nagłówkowy wyróżniający i tekstowy czytelny, z dala od Inter i Arial), skala odstępów na jednej jednostce bazowej, jeden promień zaokrąglenia, jeden poziom cienia. Minimalny cel dotykowy i kontrast tekstu spełniają WCAG 2.1 na poziomie AA (W3C, 2018).
- Lista kontrolna heurystyk odnosi się do dziesięciu heurystyk użyteczności (Nielsen, 1994).
- Małoletni jest profilem w koncie rodzinnym opisanym w PAR, nie osobnym kontem. Konta dziecka nie zakłada się jako bytu samodzielnego. Ograniczony dostęp nastolatka z KID-02 włącza opiekun z PAR-03, a poświadczenia ustawia opiekun, nie dziecko, bo samodzielna rejestracja małoletniego nie istnieje w systemie.
- Zgodę opiekuna rejestruje się w sposób rozliczalny, z wersją treści, datą i wskazaniem osoby wyrażającej zgodę, tak aby administrator mógł ją wykazać (RODO, 2016, art. 7 ust. 1). Wycofanie zgody musi być równie proste jak jej udzielenie (RODO, 2016, art. 7 ust. 3).
- Treść kierowana do dziecka jest formułowana jasnym i prostym językiem, dostosowanym do odbiorcy małoletniego (RODO, 2016, art. 12 ust. 1; motyw 58).
- Urządzeniem podstawowym jest telefon, drugim tablet. Desktop traktuje się jako wygodniejszy przypadek do spokojnego przeglądu treści zgód lub paszportu, nie jako układ wiodący.

---

## Partia 1 (KID-01 do KID-02)

### KID-01 - ekran zgody opiekuna realizowany z konta dorosłego

Grupa i rola: małoletni uczestnik jako podmiot danych. Ekran obsługuje opiekun zalogowany na konto rodzinne, który sprawuje władzę rodzicielską nad dzieckiem. Dziecko nie wykonuje tu żadnej czynności.

Moduł: wspolny. Zgoda dotyczy tak samo zajęć, jak i zawodów, bo paszport, portfel i dane dziecka są wspólne dla obu modułów.

Cel ekranu jako zadanie użytkownika: jako opiekun chcę świadomie udzielić albo cofnąć zgody wymagane do udziału mojego dziecka i przetwarzania jego danych, widząc dokładnie, na co się zgadzam, w jakim celu i jaki skutek ma każda decyzja, żeby zapis dziecka mógł się domknąć zgodnie z prawem.

Punkty wejścia: blokada zgody w zapisie dziecka na grupę z PAR-04, blokada w zapisie dziecka na zawody z PAR-11, sekcja zgód w profilu dziecka z PAR-03, sekcja zgód i dokumentów z PAR-09, ustawienia konta i zgód RODO z PAR-16, link z e-maila o zmianie wersji regulaminu lub polityki.

Warunki wstępne: aktywne konto rodzinne, istniejący profil dziecka, ustalona osoba sprawująca władzę rodzicielską, oferta organizatora wskazująca, które zgody są wymagane i w jakim celu. Wiek dziecka poniżej progu samodzielnej zgody kieruje decyzję do opiekuna, wiek powyżej progu zmienia układ ekranu i jest przedmiotem decyzji prawnej opisanej niżej.

Dane wyświetlane: kontekst dziecka, którego dotyczy zgoda, lista zgód z osobnym celem każdej z nich, podstawa prawna przetwarzania, kategorie danych z wyróżnieniem danych szczególnej kategorii jak dane o zdrowiu, administrator danych, czyli organizator, okres przechowywania, skutek udzielenia i skutek wycofania, wersja i data treści każdej zgody, nota o progu wieku samodzielnej zgody i o tym, że poniżej niego decyzję podejmuje opiekun.

Komponenty interfejsu: lista zgód z osobnym, jednoznacznym przełącznikiem dla każdego celu i odnośnikiem do pełnej treści, brak pól zaznaczonych z góry, znacznik wersji przy każdej zgodzie, wyraźny wskaźnik dziecka, którego dotyczy ekran, potwierdzenie tożsamości opiekuna dziedziczone z zalogowanego konta, oddzielenie zgody na dane szczególnej kategorii od zgód zwykłych.

Akcja główna: udziel wymaganej zgody świadomym i jednoznacznym działaniem. Treść zgody opisuje konkretny cel, a zaznaczenie jest aktywną decyzją opiekuna, nie domyślnym stanem pola.

Akcje drugorzędne: przeczytaj pełną treść zgody, zmień dziecko, zapisz decyzję na później, poproś o wyjaśnienie celu przetwarzania, otwórz powiązaną politykę prywatności ze stron prawnych GST-08.

Akcje niszczące: wycofanie udzielonej zgody. Wycofanie nie usuwa danych samo z siebie, ale ekran tłumaczy, że cofnięcie zgody wymaganej do usługi może uniemożliwić udział dziecka, i prosi o potwierdzenie. Wycofanie jest tak samo dostępne jak udzielenie i nie chowa się za dodatkowymi barierami (RODO, 2016, art. 7 ust. 3).

Stany ekranu: ładowanie pokazuje szkielet listy zgód; pusty oznacza, że bieżąca oferta nie wymaga żadnej zgody, i tłumaczy, że zapis może iść dalej; wypełniony to lista zgód ze stanem każdej z nich; błąd zapisu zgody tłumaczy przyczynę i zachowuje już podjęte decyzje; sukces potwierdza zarejestrowanie zgody i odblokowuje przerwany zapis; brak uprawnień dotyczy drugiego opiekuna o zawężonym zakresie albo osoby bez władzy rodzicielskiej, która nie może wyrazić zgody za dziecko; offline tłumaczy, że rejestracja zgody jest czynnością wymagającą sieci, bo zapisuje rozliczalny ślad, i wstrzymuje finalizację do powrotu połączenia.

Walidacja i komunikaty: pole zgody zaznaczone z góry jest niedozwolone, zgoda wymaga aktywnego działania opiekuna (EROD, 2020). Brak wymaganej zgody blokuje finalizację zapisu i wskazuje, której zgody brakuje i czego dotyczy. Zgody na różne cele nie łączy się w jedną zbiorczą zgodę, każdy cel ma osobną decyzję (EROD, 2020). Zgoda na dane szczególnej kategorii, jak dane o zdrowiu dziecka, wymaga osobnego, wyraźnego potwierdzenia (RODO, 2016, art. 9). Zmiana wersji treści zgody prosi o przejrzenie nowej wersji i ponowne wyrażenie zgody. Próba wyrażenia zgody przez osobę bez władzy rodzicielskiej tłumaczy, kto może to zrobić.

Przypadki brzegowe: dwoje opiekunów ze wspólną władzą rodzicielską i pytanie, czyja zgoda wystarcza, rozstrzygane decyzją prawną i odzwierciedlone w zakresie dostępu z PAR-16; dziecko przekraczające próg wieku samodzielnej zgody w trakcie członkostwa, gdzie układ zgód się zmienia; dziecko w przedziale wieku, w którym zgodę nadal wyraża opiekun, bo polski próg to 16 lat, nie 13; oferta wymagająca danych o zdrowiu tylko dla wyjazdu lub obozu; jeden zestaw zgód obejmujący dziecko zapisane u dwóch organizatorów z osobnym administratorem dla każdego; rozbieżność danych dziecka między profilem a dokumentem wpływająca na zakres zgody.

Przejścia i następne ekrany: blokady zgody pochodzą z zapisu dziecka w PAR-04 i PAR-11, kontekst dziecka z PAR-03, dokumenty z PAR-09, zarządzanie zgodami i zakresem drugiego opiekuna z PAR-16, pełne treści polityk ze stron prawnych GST-08, opcjonalny widok nastolatka z KID-02. Udzielenie zgody odblokowuje przerwany zapis i wraca do niego.

Zdarzenia systemowe: zapis zgody z wersją treści, datą i wskazaniem opiekuna do rozliczalności (RODO, 2016, art. 7 ust. 1), odblokowanie finalizacji zapisu po komplecie zgód, zapis wycofania zgody z datą, sygnał o zmianie wersji treści wywołujący prośbę o ponowną zgodę, log decyzji zgody do audytu organizatora.

Wymagania prawne: zgoda na przetwarzanie danych dziecka w usłudze świadczonej drogą elektroniczną poniżej progu wieku wyrażana albo autoryzowana przez osobę sprawującą władzę rodzicielską (RODO, 2016, art. 8 ust. 1), z polskim progiem 16 lat (Ustawa o ochronie danych osobowych, 2018); rozsądne starania o weryfikację, że zgodę wyraził opiekun (RODO, 2016, art. 8 ust. 2); warunki ważnej zgody, czyli dobrowolność, konkretność, świadomość i jednoznaczność, bez pól zaznaczonych z góry i bez łączenia celów (EROD, 2020); osobna podstawa dla danych szczególnej kategorii (RODO, 2016, art. 9); rozliczalność i możliwość wykazania zgody (RODO, 2016, art. 7 ust. 1); wycofanie zgody równie proste jak jej udzielenie (RODO, 2016, art. 7 ust. 3); jasny i prosty język treści (RODO, 2016, art. 12 ust. 1).

Decyzja prawna przed wdrożeniem: TAK. Ekran nie może trafić do wdrożenia bez rozstrzygnięcia, które przetwarzanie faktycznie opiera się na zgodzie, a które na innej podstawie, na przykład na wykonaniu umowy o usługę, bo zgoda zebrana tam, gdzie podstawą jest umowa, jest zgodą pozorną. Dodatkowo trzeba ustalić próg wieku dla danego organizatora i regionu, sposób weryfikacji władzy rodzicielskiej, regułę przy dwojgu opiekunach oraz odrębną podstawę dla danych o zdrowiu. To pierwszy ekran do uzgodnienia z prawnikiem przed implementacją.

Kontekst urządzenia: telefon jako podstawowy, czytelne cele dotykowe przy liście zgód, połączenie z siecią wymagane do rejestracji rozliczalnego śladu. Tablet i desktop do spokojnego przeczytania pełnej treści zgód.

Lokalizacja i języki: treść każdej zgody jako treść organizatora z wersją i znacznikiem języka, fallback en-GB, etykiety jako klucze tłumaczeń, daty wersji i akceptacji według regionu konta.

Lista kontrolna heurystyk: widoczność stanu systemu przez wersję i datę każdej zgody oraz jawny skutek decyzji; zgodność ze światem przez opis celu zrozumiały dla rodzica zamiast żargonu prawnego bazy; zapobieganie błędom przez brak pól zaznaczonych z góry i rozdzielenie celów; kontrola i swoboda przez wycofanie zgody równie proste jak udzielenie; pomoc i dokumentacja przez odnośnik do pełnej treści i polityki.

Metryki sukcesu: odsetek zapisów z kompletem ważnych zgód, odsetek ponownych zgód udzielonych po zmianie wersji treści, częstotliwość wycofań zgody, liczba zgłoszeń pomocy dotyczących zrozumienia celu przetwarzania, udział zapisów zablokowanych brakiem zgody na dane szczególnej kategorii.

---

### KID-02 - opcjonalny widok tylko do odczytu dla nastolatka

Grupa i rola: małoletni uczestnik w wieku nastoletnim, z własnym ograniczonym logowaniem nadanym przez opiekuna, działający wyłącznie w trybie odczytu. Opiekun pozostaje właścicielem konta rodzinnego i administruje dostępem nastolatka.

Moduł: wspolny. Widok pokazuje dane z obu modułów, postęp z zajęć i osiągnięcia z zawodów w jednym paszporcie.

Cel ekranu jako zadanie użytkownika: jako nastolatek chcę zobaczyć swoje postępy, paszport i zdjęcia ze startów, bez dostępu do płatności, zgód ani zapisów, żeby śledzić własny rozwój bez podejmowania decyzji zastrzeżonych dla opiekuna.

Punkty wejścia: włączenie widoku nastolatka przez opiekuna w profilu dziecka PAR-03, link logowania lub poświadczenia ustawione przez opiekuna, ikona aplikacji przy ponownym wejściu nastolatka.

Warunki wstępne: opiekun włączył widok nastolatka w PAR-03, poświadczenia dostępu ustawił opiekun, a nie dziecko, wiek dziecka mieści się w przedziale uzgodnionym decyzją prawną dla tego widoku, istnieją dane do pokazania, czyli postęp z INS-05, paszport z konfiguracji OCL-17 albo zdjęcia z RunPixie.

Dane wyświetlane: paszport sportowy w odczycie z danymi z PAR-13, drzewo umiejętności i postęp z PAR-08, galeria zdjęć powiązanych ze startem z PAR-15, osiągnięcia i wyniki z modułu zawodów, wyraźny znacznik trybu tylko do odczytu. Płatności, portfel, faktury, zgody i zapisy nie pojawiają się w tym widoku w ogóle, a nie jako wyszarzone przyciski.

Komponenty interfejsu: widoki odczytowe paszportu, postępu i galerii, brak jakiejkolwiek powierzchni akcji prowadzącej do płatności, zgody albo zapisu, przełącznik języka w nagłówku, trwały znacznik trybu odczytu, data ostatniej synchronizacji danych.

Akcja główna: przeglądaj swoje postępy. Widok jest odczytowy, więc dominującą czynnością jest oglądanie, a nie zmiana stanu.

Akcje drugorzędne: zmień język, otwórz paszport, odtwórz film instruktażowy przy umiejętności, obejrzyj zdjęcia ze startu.

Akcje niszczące: nie dotyczy. Nastolatek nie edytuje, nie usuwa i nie zatwierdza niczego w tym widoku.

Stany ekranu: ładowanie ze szkieletem sekcji; pusty oznacza brak danych i tłumaczy, że pojawią się po pierwszych zajęciach lub starcie; wypełniony to paszport, postęp i zdjęcia w odczycie; błąd ładowania z ponowieniem; sukces nie dotyczy osobno, bo widok nie zmienia danych; brak uprawnień to stała granica widoku, sekcje płatności, zgód i zapisów są niedostępne z założenia; offline pokazuje ostatnio zsynchronizowane dane z datą synchronizacji.

Walidacja i komunikaty: próba dotarcia do płatności, zgody albo zapisu nie jest możliwa, bo te ścieżki nie istnieją w widoku nastolatka, zamiast być widoczne i blokowane komunikatem. Cofnięcie dostępu przez opiekuna zamyka widok i kieruje do informacji, że dostęp wyłączył opiekun. Brak danych w danej sekcji tłumaczy, że pojawią się z czasem, bez pustego ekranu.

Przypadki brzegowe: nastolatek przekraczający próg wieku samodzielnej zgody, gdzie widok pozostaje odczytowy do czasu osobnej decyzji prawnej rozszerzającej jego zakres; nastolatek chcący sam się zapisać, kierowany do opiekuna, bo zapis i płatność są poza tym widokiem; opiekun cofający dostęp w trakcie sesji nastolatka; zdjęcie z wizerunkiem innych dzieci pokazane z poszanowaniem ich wizerunku; dziecko obecne tylko w jednym module, gdzie druga sekcja paszportu jest pusta.

Przejścia i następne ekrany: dane postępu pochodzą z INS-05 i PAR-08, paszport z OCL-17 i PAR-13, zdjęcia z PAR-15, włączenie i wyłączenie dostępu z PAR-03. Widok nie prowadzi do żadnego ekranu płatności, zgody ani zapisu.

Zdarzenia systemowe: logowanie w ograniczonej roli nastolatka, egzekwowanie zakresu dostępu po stronie systemu, powiadomienie opiekuna o włączeniu i wyłączeniu dostępu nastolatka, log logowań nastolatka do wglądu opiekuna, synchronizacja danych odczytowych.

Wymagania prawne: widok przetwarza dane dziecka pokazywane samemu dziecku, co wymaga decyzji o minimalnym wieku oferowania takiego dostępu i o zakresie danych; informacja w widoku formułowana prostym językiem dostosowanym do małoletniego (RODO, 2016, art. 12 ust. 1; motyw 58); poświadczenia dostępu małoletniego ustawia opiekun, bo samodzielnej rejestracji małoletniego system nie prowadzi; dane szczególnej kategorii, jak dane o zdrowiu, nie są pokazywane nastolatkowi bez osobnego rozstrzygnięcia; brak płatności, zgód i zapisów w widoku jako realizacja zasady minimalizacji dostępu do funkcji zastrzeżonych dla opiekuna.

Decyzja prawna przed wdrożeniem: TAK. Trzeba rozstrzygnąć, czy widok w ogóle udostępniać, od jakiego wieku, w jakim zakresie danych, kto kontroluje poświadczenia oraz czy i jak wyłączyć dane o zdrowiu z widoku dziecka. Trzeba też ustalić sposób przedstawienia informacji o przetwarzaniu w formie zrozumiałej dla nastolatka. Bez tych rozstrzygnięć widok nie wchodzi do wdrożenia.

Kontekst urządzenia: telefon jako podstawowy, sekcje przewijane pionowo z dużymi znacznikami. Tablet i desktop do szerszego przeglądu paszportu i galerii.

Lokalizacja i języki: nazwy i opisy umiejętności jako treść organizatora z możliwością tłumaczenia, etykiety jako klucze, daty według regionu, fallback en-GB.

Lista kontrolna heurystyk: minimalistyczny projekt przez pokazanie tylko tego, co nastolatek może oglądać; zapobieganie błędom przez brak powierzchni akcji, których małoletni nie może użyć, zamiast widocznych i blokowanych przycisków; widoczność stanu systemu przez znacznik trybu odczytu i datę synchronizacji; zgodność ze światem przez język dostosowany do nastolatka; kontrola i swoboda po stronie opiekuna przez włączanie i wyłączanie dostępu z PAR-03.

Metryki sukcesu: odsetek rodzin włączających widok nastolatka, częstotliwość logowań nastolatka, liczba prób dotarcia do funkcji zastrzeżonych, która z założenia powinna być zerowa, liczba wyłączeń dostępu przez opiekuna, udział nastolatków otwierających paszport i galerię.

---

## Literatura

Europejska Rada Ochrony Danych. (2020). *Wytyczne 05/2020 dotyczące zgody na mocy rozporządzenia 2016/679* (wersja 1.1). European Data Protection Board. https://www.edpb.europa.eu/our-work-tools/our-documents/guidelines/guidelines-052020-consent-under-regulation-2016679_pl

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

Rozporządzenie Parlamentu Europejskiego i Rady (UE) 2016/679 z dnia 27 kwietnia 2016 r. w sprawie ochrony osób fizycznych w związku z przetwarzaniem danych osobowych i w sprawie swobodnego przepływu takich danych oraz uchylenia dyrektywy 95/46/WE (ogólne rozporządzenie o ochronie danych) [RODO]. (2016). Dziennik Urzędowy Unii Europejskiej L 119/1. https://eur-lex.europa.eu/legal-content/PL/TXT/?uri=CELEX%3A32016R0679

Ustawa z dnia 10 maja 2018 r. o ochronie danych osobowych. (2018). Dziennik Ustaw 2018 poz. 1000. https://isap.sejm.gov.pl/isap.nsf/DocDetails.xsp?id=WDU20180001000

W3C. (2018). *Web Content Accessibility Guidelines (WCAG) 2.1*. World Wide Web Consortium. https://www.w3.org/TR/WCAG21/
