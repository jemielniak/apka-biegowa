# Specyfikacja ekranów grupy OCL - moduł zajęć

Grupa OCL to panel administracyjny organizatora pracującego w trybie zajęć, czyli właściciela szkółki. To samo konto co organizator zawodów, inny tryb. Wszystkie ekrany działają w module `zajecia`, rola dominująca to organizator, z odczytem dla instruktora tam, gdzie to zaznaczono.

Konwencje wspólne dla całej grupy, żeby nie powtarzać ich w każdym polu:

- Napisy widoczne dla użytkownika to klucze tłumaczeń ze znacznikami BCP 47, domyślnie pl-PL i en-GB. Daty, kwoty i numery telefonu formatuje się według ustawień regionalnych. Przełącznik języka stoi w nagłówku panelu, język zapasowy to en-GB przy braku tłumaczenia. Układ RTL włącza się automatycznie, jeśli aktywny język jest pisany od prawej do lewej.
- Tokeny marki pochodzą wyłącznie ze zmiennych CSS: `--brand-ink` i `--accent`, kolory semantyczne dla sukcesu, ostrzeżenia, błędu oraz informacji, para krojów (nagłówkowy wyróżniający i tekstowy czytelny, z dala od Inter i Arial), skala odstępów na jednej jednostce bazowej, jeden promień zaokrąglenia, jeden poziom cienia. Minimalny cel dotykowy i kontrast tekstu spełniają WCAG 2.1 na poziomie AA (W3C, 2018).
- Lista kontrolna heurystyk odnosi się do dziesięciu heurystyk użyteczności (Nielsen, 1994).
- Model płatności jest oderwany od liczby dzieci. Koszt operatora nie rośnie z liczbą użytkowników, więc cennik dla organizatora opiera się na abonamencie placówki, a nie na opłacie od główki.

---

## Partia 1 (OCL-01 do OCL-06)

### OCL-01 - dashboard zajęć

Grupa i rola: organizator (właściciel szkółki). Odczyt skrótowy dla instruktora przez osobny panel INS.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: w trzydzieści sekund po zalogowaniu chcę wiedzieć, co dziś wymaga mojej reakcji - które grupy mają zajęcia, gdzie brakuje instruktora, kto zalega z płatnością, ile dzieci czeka na liście rezerwowej.

Punkty wejścia: logowanie organizatora, przełącznik trybu z panelu zawodów, link z powiadomienia push lub e-mail.

Warunki wstępne: aktywne konto organizatora z włączonym modułem zajęć, przynajmniej jedna placówka skonfigurowana.

Dane wyświetlane: dzisiejsze zajęcia z godziną i przypisanym instruktorem, liczba obecnych względem zapisanych, alerty o brakach kadrowych, saldo należności i liczba przeterminowanych płatności, liczba zgłoszeń na listach rezerwowych, najbliższe wygasające zgody i dokumenty. Kwoty w walucie placówki według ustawień regionalnych.

Komponenty interfejsu: pasek nagłówka z przełącznikiem trybu i języka, kafelki wskaźników, oś czasu dzisiejszego dnia, lista alertów z priorytetem, wykres frekwencji tygodniowej.

Akcja główna: przejdź do najpilniejszego alertu. Jedna dominująca akcja prowadzi do listy spraw wymagających reakcji.

Akcje drugorzędne: filtr placówki, zmiana zakresu dat, skok do grafiku, skok do listy płatności.

Akcje niszczące: nie dotyczy.

Stany ekranu: ładowanie pokazuje szkielety kafelków; pusty wita organizatora bez grup i kieruje do kreatora grupy; wypełniony pokazuje pełen pulpit; błąd ładowania danych pokazuje komunikat z przyciskiem ponów; sukces nie dotyczy osobno; brak uprawnień ukrywa moduł i proponuje kontakt z operatorem; offline nie dotyczy, pulpit wymaga sieci.

Walidacja i komunikaty: przy nieudanym pobraniu wskaźników komunikat mówi, że nie udało się pobrać danych pulpitu i proponuje ponowienie. Liczby nie ładują się oddzielnie, więc częściowy błąd oznacza dany kafelek, a nie cały ekran.

Przypadki brzegowe: placówka bez zajęć w danym dniu; instruktor przypisany do dwóch grup w tym samym czasie, co wyzwala alert konfliktu; bardzo duża liczba grup wymaga zwijania osi czasu.

Przejścia i następne ekrany: alert płatności prowadzi do OCL-14, alert kadrowy do OCL-12, alert listy rezerwowej do OCL-05, kafelek grafiku do OCL-03.

Zdarzenia systemowe: odświeżanie wskaźników co kilka minut, push o konflikcie grafiku, sygnał o nowej płatności.

Wymagania prawne: pulpit pokazuje dane zagregowane, dane wrażliwe dzieci pojawiają się dopiero po wejściu w kartę. Zgodność z RODO przez minimalizację danych na widoku startowym.

Kontekst urządzenia: desktop jako podstawowy, responsywny tablet dla pracy zza biurka recepcji. Telefon pokazuje skróconą listę alertów.

Lokalizacja i języki: wszystkie etykiety jako klucze tłumaczeń, liczby i waluty według regionu placówki, przełącznik języka w nagłówku.

Lista kontrolna heurystyk: widoczność stanu systemu przez świeże wskaźniki i znacznik czasu odświeżenia; zgodność ze światem przez język recepcji, nie język bazy danych; rozpoznawanie zamiast przypominania, bo alerty wskazują, co zrobić; minimalistyczny projekt przez ograniczenie do spraw pilnych; pomoc w diagnozie przez bezpośrednie linki do źródła problemu.

Metryki sukcesu: czas od zalogowania do pierwszej sensownej akcji, odsetek alertów rozwiązanych tego samego dnia, spadek liczby przeterminowanych płatności tydzień do tygodnia.

---

### OCL-02 - zarządzanie grupami i kursami z poziomami

Grupa i rola: organizator.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę założyć i utrzymać strukturę grup z poziomami zaawansowania, tak żeby rodzic widział właściwy poziom dla dziecka, a instruktor wiedział, kogo uczy.

Punkty wejścia: kafelek grup z pulpitu, menu boczne, pusty stan pulpitu kierujący do założenia pierwszej grupy.

Warunki wstępne: skonfigurowana placówka, zdefiniowane sale lub tory.

Dane wyświetlane: lista grup z poziomem, dniem i godziną, przypisanym instruktorem, liczbą miejsc zajętych względem limitu, statusem zapisów. Każda grupa pokazuje powiązany poziom z drzewa umiejętności.

Komponenty interfejsu: tabela grup z sortowaniem, panel boczny edycji grupy, wybór poziomu z listy zdefiniowanej w OCL-10, selektor instruktora, suwak limitu miejsc, przełącznik widoczności zapisów.

Akcja główna: dodaj grupę. Na karcie istniejącej grupy dominującą akcją jest zapis zmian.

Akcje drugorzędne: duplikuj grupę na nowy semestr, archiwizuj, zmień instruktora, podejrzyj listę uczestników.

Akcje niszczące: usuń grupę. Dostępne tylko dla grup bez zapisanych uczestników, w innym wypadku proponuje się archiwizację z zachowaniem historii.

Stany ekranu: ładowanie z szkieletem tabeli; pusty proponuje utworzenie pierwszej grupy i pokazuje przykład poziomów; wypełniony to pełna tabela; błąd zapisu pokazuje komunikat przy formularzu; sukces potwierdza zapis tostem; brak uprawnień ukrywa edycję i pozostawia podgląd; offline nie dotyczy.

Walidacja i komunikaty: limit miejsc musi być liczbą dodatnią. Nakładanie się terminu z inną grupą tego samego instruktora pokazuje ostrzeżenie z nazwą kolidującej grupy i propozycją zmiany godziny. Brak przypisanego poziomu blokuje publikację zapisów i tłumaczy, że rodzic nie zobaczy grupy bez poziomu.

Przypadki brzegowe: grupa o zerowym limicie jako lista oczekujących; grupa łączona z dwóch poziomów; semestr przejściowy, gdzie ta sama grupa zmienia poziom w trakcie roku.

Przejścia i następne ekrany: wybór poziomu otwiera OCL-10, podgląd uczestników prowadzi do OCL-08, przypisanie instruktora łączy z OCL-12, publikacja zapisów uruchamia widok rodzica PAR-04.

Zdarzenia systemowe: aktualizacja licznika miejsc po nowym zapisie, sygnał o osiągnięciu limitu otwierający automatyczne dopisanie do listy rezerwowej w OCL-05.

Wymagania prawne: nie dotyczy bezpośrednio, dane uczestników nie są tu eksponowane poza liczbą.

Kontekst urządzenia: desktop i tablet. Telefon w trybie odczytu listy.

Lokalizacja i języki: nazwy grup jako treść organizatora, etykiety interfejsu jako klucze tłumaczeń, dni tygodnia i godziny według regionu.

Lista kontrolna heurystyk: spójność i standardy przez jednolity sposób opisu grupy; zapobieganie błędom przez ostrzeżenie o konflikcie terminów; kontrola użytkownika przez archiwizację zamiast nieodwracalnego usuwania; rozpoznawanie zamiast przypominania, bo poziom wybiera się z gotowej listy.

Metryki sukcesu: czas założenia nowej grupy, odsetek grup z poprawnie przypisanym poziomem, liczba konfliktów terminów wykrytych przed publikacją.

---

### OCL-03 - grafik i kalendarz zajęć

Grupa i rola: organizator. Odczyt dla instruktora w INS-03.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę zobaczyć cały tydzień placówki w jednym widoku i szybko przesunąć, odwołać albo dodać zajęcia, bez kolizji sal i instruktorów.

Punkty wejścia: kafelek grafiku z pulpitu, menu boczne, link z karty grupy.

Warunki wstępne: istnieją grupy z przypisanymi terminami, zdefiniowane sale lub tory.

Dane wyświetlane: siatka tygodniowa z blokami zajęć, kolor bloku według poziomu, etykieta z instruktorem i salą, oznaczenia odwołań i zastępstw, dni świąteczne według kalendarza regionu.

Komponenty interfejsu: przełącznik widoku dzień, tydzień, miesiąc; siatka z przeciąganiem bloków; filtr po sali, instruktorze i poziomie; panel szczegółów wybranego bloku; wskaźnik kolizji.

Akcja główna: dodaj zajęcia do siatki. Na wybranym bloku dominującą akcją jest zapis przesunięcia.

Akcje drugorzędne: odwołaj pojedyncze zajęcia, ustaw zastępstwo, powiel termin na cały semestr, wydrukuj grafik.

Akcje niszczące: usuń termin z serii. Wymaga potwierdzenia z wyborem, czy dotyczy jednego wystąpienia, czy całej serii.

Stany ekranu: ładowanie z szarą siatką; pusty pokazuje tydzień bez bloków i podpowiada dodanie pierwszych zajęć; wypełniony to pełna siatka; błąd zapisu przesunięcia cofa blok na stare miejsce i tłumaczy przyczynę; sukces potwierdza zmianę; brak uprawnień daje widok tylko do odczytu; offline pokazuje ostatnio wczytany grafik z banerem, że zmiany są wstrzymane do powrotu sieci.

Walidacja i komunikaty: przesunięcie na zajęty termin sali pokazuje, która grupa już tam jest, i proponuje wolne okno. Zajęcia poza godzinami pracy placówki ostrzegają o naruszeniu harmonogramu. Odwołanie zajęć pyta, czy powiadomić rodziców i czy otworzyć odrabianie.

Przypadki brzegowe: zajęcia trwające przez północ; tydzień ze świętem przesuwającym wszystkie terminy; instruktor na zwolnieniu wymagający masowego zastępstwa.

Przejścia i następne ekrany: odwołanie zajęć łączy z regułami odrabiania OCL-04 i komunikacją OCL-15, blok prowadzi do listy obecności w INS-04, konflikt sali odsyła do ustawień placówki OCL-18.

Zdarzenia systemowe: synchronizacja z grafikiem instruktora, push do rodziców przy odwołaniu, aktualizacja API pogodowego dla zajęć na otwartym powietrzu.

Wymagania prawne: powiadomienie o odwołaniu jako obowiązek informacyjny wobec opiekuna, zapis zgody na kanał kontaktu.

Kontekst urządzenia: desktop jako główny, tablet do pracy przy recepcji. Telefon w widoku dnia.

Lokalizacja i języki: nazwy dni i miesięcy oraz format godziny według regionu, kalendarz świąt zależny od kraju placówki, etykiety jako klucze tłumaczeń.

Lista kontrolna heurystyk: widoczność stanu systemu przez natychmiastowy obraz tygodnia; zapobieganie błędom przez wykrywanie kolizji przed zapisem; kontrola i swoboda przez cofnięcie przesunięcia; zgodność ze światem przez kalendarz i nazwy dni z życia, nie z bazy.

Metryki sukcesu: liczba kolizji sal po wdrożeniu, czas reakcji na odwołanie, odsetek odwołań z powiadomieniem wysłanym automatycznie.

---

### OCL-04 - reguły odrabiania nieobecności

Grupa i rola: organizator.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę ustawić jasne zasady, kiedy i jak dziecko może odrobić opuszczone zajęcia, żeby system sam proponował terminy rodzicowi, a recepcja nie rozstrzygała tego ręcznie.

Punkty wejścia: menu ustawień zajęć, link z odwołania w grafiku, kafelek konfiguracji z pulpitu.

Warunki wstępne: zdefiniowane grupy i poziomy, bo odrabianie dopuszcza się zwykle w grupie o tym samym poziomie.

Dane wyświetlane: aktywne reguły z opisem warunku i limitem, okno zgłaszania nieobecności przed zajęciami, liczba dozwolonych odrobień na semestr, zasada zgodności poziomu, okres ważności prawa do odrobienia.

Komponenty interfejsu: lista reguł z przełącznikiem aktywności, formularz reguły z polami warunku i limitu, podgląd działania reguły na przykładzie, selektor poziomów objętych regułą.

Akcja główna: zapisz regułę. Na liście pustej dominującą akcją jest utworzenie pierwszej reguły z gotowego szablonu.

Akcje drugorzędne: dezaktywuj regułę, sklonuj na inny poziom, przetestuj na przykładowym uczestniku.

Akcje niszczące: usuń regułę. Z ostrzeżeniem, że aktywne prawa do odrobienia oparte na tej regule zostaną zachowane do wygaśnięcia.

Stany ekranu: ładowanie listy; pusty z propozycją szablonu i krótkim wyjaśnieniem mechaniki; wypełniony to lista reguł; błąd zapisu przy formularzu; sukces z tostem; brak uprawnień daje odczyt; offline nie dotyczy.

Walidacja i komunikaty: okno zgłaszania nieobecności nie może być ujemne. Limit odrobień jako liczba całkowita. Reguła bez wskazanego poziomu prosi o doprecyzowanie, czy obejmuje wszystkie poziomy. Sprzeczne reguły dla tego samego poziomu pokazują, która ma pierwszeństwo.

Przypadki brzegowe: dziecko zmieniające poziom w trakcie semestru z niewykorzystanym prawem do odrobienia; odrabianie między modułami, gdy rodzina ma też zapis na zawody; nieobecność zgłoszona po terminie z prośbą o wyjątek.

Przejścia i następne ekrany: reguła zasila zapis na odrabianie po stronie rodzica PAR-06, łączy się ze zgłoszeniem nieobecności PAR-05 i grafikiem OCL-03.

Zdarzenia systemowe: utworzenie prawa do odrobienia po zgłoszonej nieobecności, wygaśnięcie prawa po przekroczeniu okresu ważności, push do rodzica o dostępnym terminie odrobienia.

Wymagania prawne: zasady odrabiania jako element regulaminu placówki, wymagana wersja regulaminu zaakceptowana przy zapisie.

Kontekst urządzenia: desktop i tablet.

Lokalizacja i języki: opisy reguł jako treść organizatora z możliwością tłumaczenia, etykiety jako klucze, jednostki czasu według regionu.

Lista kontrolna heurystyk: zapobieganie błędom przez podgląd działania reguły przed zapisem; zgodność ze światem przez język rodzica, nie żargon systemu; pomoc i dokumentacja przez szablon i przykład; spójność przez jednolity format reguł.

Metryki sukcesu: odsetek odrobień obsłużonych automatycznie bez recepcji, liczba sporów o prawo do odrobienia, czas od nieobecności do zapisu na odrobienie.

---

### OCL-05 - listy rezerwowe z automatycznym dopisaniem

Grupa i rola: organizator.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę, żeby zwolnione miejsce w pełnej grupie samo trafiało do pierwszej oczekującej rodziny, na jasnych zasadach, bez ręcznego obdzwaniania.

Punkty wejścia: alert listy rezerwowej z pulpitu, karta grupy z OCL-02, menu zajęć.

Warunki wstępne: grupa z osiągniętym limitem miejsc i włączoną listą rezerwową.

Dane wyświetlane: kolejka oczekujących z pozycją, datą zgłoszenia i danymi kontaktu rodzica, reguła dopisania, czas na potwierdzenie miejsca, historia ofert i odmów.

Komponenty interfejsu: tabela kolejki z kolejnością, przełącznik trybu automatycznego i ręcznego, ustawienie okna na potwierdzenie, podgląd następnej osoby w kolejce.

Akcja główna: zaoferuj zwolnione miejsce następnej osobie. W trybie automatycznym ekran pokazuje przede wszystkim stan kolejki, a akcją główną jest włączenie automatu.

Akcje drugorzędne: przesuń ręcznie w kolejce z uzasadnieniem, wstrzymaj automat, przedłuż okno na potwierdzenie.

Akcje niszczące: usuń zgłoszenie z listy. Z potwierdzeniem i informacją, że rodzic dostanie powiadomienie o wypisaniu.

Stany ekranu: ładowanie kolejki; pusty oznacza brak oczekujących i wolne miejsca w grupie; wypełniony to pełna kolejka; błąd przy ofercie pokazuje, że nie udało się wysłać oferty i proponuje ponowienie; sukces potwierdza dopisanie; brak uprawnień daje odczyt; offline pokazuje ostatni stan z banerem o wstrzymaniu zmian.

Walidacja i komunikaty: okno na potwierdzenie nie może być zerowe. Próba ręcznego przesunięcia bez uzasadnienia prosi o powód, bo historia kolejki musi być audytowalna. Oferta wygasła wraca miejsce do puli i przechodzi do kolejnej osoby z notatką.

Przypadki brzegowe: dwie grupy o tym samym poziomie i wspólna kolejka; rodzic z dzieckiem już zapisanym na inny poziom; zwolnienie miejsca tuż przed startem semestru, gdy okno na potwierdzenie skraca się automatycznie.

Przejścia i następne ekrany: oferta wyzwala push do rodzica PAR-07, potwierdzenie prowadzi do zapisu PAR-04 i płatności PAR-10, dopisanie aktualizuje licznik miejsc w OCL-02.

Zdarzenia systemowe: zwolnienie miejsca po wypisie uruchamia automat, timer okna na potwierdzenie, eskalacja do kolejnej osoby po wygaśnięciu oferty.

Wymagania prawne: przejrzysta kolejność jako element regulaminu, zgoda na kontakt push lub e-mail, dane kontaktowe ograniczone do potrzeby obsługi kolejki.

Kontekst urządzenia: desktop i tablet recepcji.

Lokalizacja i języki: etykiety jako klucze, daty i godziny według regionu, treść oferty tłumaczona z fallbackiem na en-GB.

Lista kontrolna heurystyk: widoczność stanu systemu przez jawną kolejkę i status ofert; kontrola użytkownika przez tryb ręczny z uzasadnieniem; zapobieganie błędom przez wymóg powodu przy przesunięciu; pomoc w diagnozie przez historię ofert i odmów.

Metryki sukcesu: czas od zwolnienia miejsca do dopisania, odsetek miejsc obsadzonych automatycznie, liczba interwencji recepcji na sto ofert.

---

### OCL-06 - zapisy na obozy i półkolonie

Grupa i rola: organizator.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę utworzyć ofertę obozu lub półkolonii z turnusami, limitami i pakietem dokumentów, tak żeby rodzic zapisał dziecko i opłacił w jednym przejściu.

Punkty wejścia: menu zajęć, kafelek pulpitu w sezonie, duplikat zeszłorocznego obozu.

Warunki wstępne: skonfigurowana placówka, model płatności obozowy ustawiony w OCL-07, lista wymaganych dokumentów.

Dane wyświetlane: lista turnusów z datami i limitem, cena turnusu i ewentualne progi wczesne, wymagane dokumenty i zgody, dostępne warianty wyżywienia i transportu, liczba zapisanych względem limitu.

Komponenty interfejsu: kreator turnusu z krokami, kalendarz dat turnusu, definicja pakietu dokumentów, lista wariantów dodatkowych, podgląd oferty oczami rodzica.

Akcja główna: opublikuj turnus. W trakcie tworzenia dominującą akcją jest przejście do następnego kroku, na końcu publikacja.

Akcje drugorzędne: zapisz wersję roboczą, duplikuj turnus, ustaw wczesny próg cenowy, podejrzyj ofertę publiczną.

Akcje niszczące: usuń turnus. Dostępne tylko bez zapisanych dzieci, inaczej proponowana jest archiwizacja.

Stany ekranu: ładowanie; pusty z propozycją utworzenia pierwszego turnusu lub duplikatu zeszłorocznego; wypełniony to lista turnusów; błąd zapisu przy kroku kreatora; sukces po publikacji z linkiem do oferty; brak uprawnień daje odczyt; offline nie dotyczy.

Walidacja i komunikaty: data końca turnusu nie może poprzedzać początku. Limit miejsc jako liczba dodatnia. Publikacja bez kompletu wymaganych dokumentów ostrzega, których zgód brakuje i dlaczego są potrzebne. Cena niższa od zera blokuje zapis.

Przypadki brzegowe: turnus przez przełom miesiąca lub roku; rodzeństwo na różnych turnusach ze zniżką rodzinną; turnus z listą rezerwową dziedziczący zasady z OCL-05; obóz wyjazdowy wymagający dodatkowych zgód medycznych.

Przejścia i następne ekrany: publikacja otwiera zapis rodzica PAR-04 w wariancie obozowym, dokumenty łączą się z kartą uczestnika OCL-09, płatność z OCL-14, lista uczestników z OCL-08.

Zdarzenia systemowe: zamknięcie zapisów po osiągnięciu limitu, przypomnienie o brakujących dokumentach przed startem turnusu, sygnał o wczesnym progu cenowym wygasającym wkrótce.

Wymagania prawne: zgody medyczne i zgoda na udział w wyjeździe jako wymagane dokumenty, dane zdrowotne dziecka jako szczególna kategoria danych pod RODO, przechowywanie ograniczone czasowo i dostęp tylko dla uprawnionych.

Kontekst urządzenia: desktop do tworzenia oferty, tablet do podglądu, telefon do szybkiego sprawdzenia zapełnienia.

Lokalizacja i języki: nazwy turnusów jako treść organizatora, daty i ceny według regionu, etykiety jako klucze tłumaczeń.

Lista kontrolna heurystyk: zapobieganie błędom przez kontrolę dat i kompletu dokumentów; widoczność stanu systemu przez licznik zapełnienia; pomoc i dokumentacja przez podgląd oferty oczami rodzica; zgodność ze światem przez słownictwo turnusów i wyżywienia znane rodzicom.

Metryki sukcesu: odsetek zapisów ukończonych w jednym przejściu, liczba turnusów opublikowanych z kompletem dokumentów, czas tworzenia turnusu z duplikatu względem tworzenia od zera.

---

## Partia 2 (OCL-07 do OCL-12)

### OCL-07 - cennik i model płatności

Grupa i rola: organizator.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę ustawić ceny zajęć tak, żeby model był oderwany od liczby dzieci - abonament miesięczny, semestralny, karnet na wejścia, ze zniżkami rodzinnymi - i żeby rodzic od razu wiedział, ile płaci.

Punkty wejścia: menu zajęć, krok cenowy z kreatora obozu OCL-06, alert rozbieżności z pulpitu.

Warunki wstępne: skonfigurowane grupy lub turnusy, podpięte bramki płatności na poziomie operatora.

Dane wyświetlane: aktywne plany cenowe z typem rozliczenia, ceny netto i brutto według regionu podatkowego, zniżki rodzinne i progi, metody płatności dostępne dla placówki, przykładowa kalkulacja dla rodziny z dwójką dzieci.

Komponenty interfejsu: lista planów z przełącznikiem aktywności, formularz planu z typem rozliczenia, edytor zniżek, podgląd faktury przykładowej, lista metod płatności Przelewy24, PayU, Stripe, BLIK oraz karty.

Akcja główna: zapisz plan cenowy. Na pustej liście dominującą akcją jest dodanie pierwszego planu.

Akcje drugorzędne: sklonuj plan, ustaw zniżkę rodzinną, podejrzyj kalkulację, dezaktywuj plan.

Akcje niszczące: usuń plan. Tylko jeśli żaden aktywny zapis go nie używa, inaczej archiwizacja z zachowaniem historii rozliczeń.

Stany ekranu: ładowanie; pusty z wyjaśnieniem modeli rozliczenia i przykładem; wypełniony to lista planów; błąd zapisu przy formularzu; sukces z tostem; brak uprawnień daje odczyt; offline nie dotyczy.

Walidacja i komunikaty: cena nie może być ujemna. Suma zniżek przekraczająca cenę blokuje zapis i tłumaczy, że kwota końcowa byłaby poniżej zera. Plan bez przypisanej grupy lub turnusu ostrzega, że nie będzie widoczny przy zapisie. Zmiana ceny aktywnego planu pyta, czy dotyczy nowych zapisów, czy także trwających abonamentów.

Przypadki brzegowe: rodzina z trójką dzieci na różnych planach; zmiana stawki podatku w trakcie semestru; przejście z karnetu na abonament w środku miesiąca z proporcjonalnym rozliczeniem; rodzic z saldem w portfelu pokrywającym część opłaty.

Przejścia i następne ekrany: plan zasila zapis rodzica PAR-04 i podsumowanie płatności PAR-10, łączy się z portfelem i fakturami OCL-14, ceny obozów wracają do OCL-06.

Zdarzenia systemowe: aktualizacja kursu metody płatności, sygnał o wygasającym progu wczesnym, naliczenie cyklicznej opłaty abonamentowej.

Wymagania prawne: ceny brutto widoczne dla konsumenta, faktura zgodna z lokalnymi przepisami podatkowymi, jasna informacja o cyklu i sposobie rezygnacji z abonamentu.

Kontekst urządzenia: desktop i tablet.

Lokalizacja i języki: kwoty, waluta i stawki podatku według regionu, nazwy planów tłumaczone, etykiety jako klucze.

Lista kontrolna heurystyk: zgodność ze światem przez modele rozliczenia znane rodzicom; zapobieganie błędom przez kontrolę zniżek i ceny; widoczność stanu systemu przez kalkulację przykładową; pomoc i dokumentacja przez wyjaśnienie modeli na pustym stanie.

Metryki sukcesu: odsetek rodzin korzystających ze zniżki rodzinnej, liczba błędnych konfiguracji wykrytych przed publikacją, udział płatności cyklicznych w przychodzie placówki.

---

### OCL-08 - lista uczestników i rodzin

Grupa i rola: organizator. Odczyt ograniczony dla instruktora w INS-06.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę szybko znaleźć dziecko lub rodzinę, zobaczyć status płatności i dokumentów, i przejść do działania, bez przeklikiwania kilku list.

Punkty wejścia: kafelek uczestników z pulpitu, karta grupy OCL-02, wyszukiwarka globalna panelu.

Warunki wstępne: istnieją zapisani uczestnicy w placówce.

Dane wyświetlane: lista dzieci z przypisaną grupą i poziomem, powiązany rodzic i konto rodzinne, status płatności, status wymaganych dokumentów, frekwencja skrócona. Konto rodzinne łączy rodzeństwo i wspólny portfel.

Komponenty interfejsu: tabela z filtrami po grupie, poziomie, statusie płatności i dokumentów; pole wyszukiwania; przełącznik widoku dzieci albo rodziny; znaczniki statusu kolorami semantycznymi; akcje masowe na zaznaczeniu.

Akcja główna: otwórz kartę uczestnika. Akcja wiodąca prowadzi do szczegółu wybranego dziecka.

Akcje drugorzędne: filtruj, eksportuj zaznaczenie do arkusza, wyślij komunikat do segmentu, przenieś dziecko do innej grupy.

Akcje niszczące: wypisz dziecko z grupy. Z potwierdzeniem i wyborem, czy zachować historię i paszport sportowy, oraz informacją o ewentualnym zwrocie do portfela.

Stany ekranu: ładowanie z szkieletem tabeli; pusty z propozycją zaproszenia pierwszej rodziny lub otwarcia zapisów; wypełniony to pełna lista; błąd ładowania z ponowieniem; sukces przy akcji masowej z podsumowaniem; brak uprawnień ogranicza kolumny wrażliwe; offline daje ostatnio wczytaną listę w odczycie z banerem.

Walidacja i komunikaty: filtr bez wyników tłumaczy, że żadne dziecko nie spełnia kryteriów, i proponuje wyczyszczenie filtrów. Akcja masowa na rekordzie bez uprawnień pomija go i wyjaśnia, ile rekordów pominięto i dlaczego. Przeniesienie do grupy o innym poziomie ostrzega o niezgodności poziomu.

Przypadki brzegowe: rodzina z dziećmi w obu modułach, zajęcia oraz zawody; dziecko bez przypisanej grupy oczekujące na listę rezerwową; duplikat konta rodzica wykryty po e-mailu lub telefonie; rodzina z saldem ujemnym w portfelu.

Przejścia i następne ekrany: rekord prowadzi do karty uczestnika OCL-09, segment do komunikacji OCL-15, eksport do raportów OCL-16, przeniesienie do OCL-02.

Zdarzenia systemowe: aktualizacja statusu płatności po rozliczeniu, oznaczenie wygasającego dokumentu, sygnał o nowym zapisie pojawiającym się na liście.

Wymagania prawne: dane dzieci jako dane osobowe pod RODO, dane medyczne jako kategoria szczególna z dostępem wyłącznie dla uprawnionych, eksport z odnotowaniem celu i zakresu.

Kontekst urządzenia: desktop jako podstawowy, tablet recepcji, telefon do szybkiego wyszukania.

Lokalizacja i języki: etykiety jako klucze, daty i kwoty według regionu, imiona i nazwiska jako dane użytkownika bez tłumaczenia.

Lista kontrolna heurystyk: elastyczność i efektywność przez filtry i akcje masowe; widoczność stanu systemu przez kolorowe statusy; zapobieganie błędom przez ostrzeżenie o niezgodności poziomu; pomoc w diagnozie przez wyjaśnienie pustego wyniku filtra.

Metryki sukcesu: czas znalezienia konkretnego dziecka, odsetek rodzin z kompletem dokumentów, liczba duplikatów kont wykrytych i scalonych.

---

### OCL-09 - karta uczestnika z dokumentami

Grupa i rola: organizator. Odczyt dla instruktora w INS-06.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę mieć jedno miejsce z pełnym obrazem dziecka - grupa, postępy, dokumenty, płatności, historia obecności - żeby obsłużyć każdą sprawę rodzica bez szukania w innych ekranach.

Punkty wejścia: rekord z listy OCL-08, wynik wyszukiwania globalnego, link z karty grupy.

Warunki wstępne: istnieje uczestnik powiązany z kontem rodzinnym.

Dane wyświetlane: dane dziecka i opiekuna, przypisana grupa i poziom, postęp na drzewie umiejętności, komplet dokumentów ze statusem i datą ważności, historia płatności i saldo portfela, frekwencja, paszport sportowy dziecka wspólny dla obu modułów.

Komponenty interfejsu: nagłówek profilu z przełącznikiem między dziećmi tej samej rodziny, zakładki danych, lista dokumentów z podglądem i statusem, oś czasu obecności, panel umiejętności w odczycie, skrót do portfela.

Akcja główna: edytuj dane lub status dziecka. Akcja wiodąca zależy od kontekstu wejścia, domyślnie zapis zmian w profilu.

Akcje drugorzędne: dodaj dokument, wyślij wiadomość do rodzica, przenieś do innej grupy, zarejestruj płatność ręczną.

Akcje niszczące: usuń dokument lub wypisz dziecko. Z potwierdzeniem i informacją o skutkach, dokumentów nie usuwa się trwale, tylko archiwizuje z zachowaniem śladu audytu.

Stany ekranu: ładowanie z szkieletem profilu; pusty nie dotyczy, bo karta istnieje tylko dla istniejącego dziecka; wypełniony to pełny profil; błąd ładowania sekcji pokazuje komunikat lokalnie przy zakładce; sukces po zapisie z tostem; brak uprawnień ukrywa dane medyczne i finansowe; offline daje odczyt ostatnich danych z banerem.

Walidacja i komunikaty: data ważności dokumentu w przeszłości oznacza go jako wygasły z czytelnym ostrzeżeniem. Brak wymaganej zgody blokuje udział w zajęciach i wyjaśnia, której zgody brakuje. Edycja danych medycznych wymaga potwierdzenia ze względu na wrażliwość.

Przypadki brzegowe: dziecko zapisane w obu modułach z jednym paszportem; opiekun rozdzielony, dwa konta dorosłych przy jednym dziecku; dokument przesłany w formacie nieobsługiwanym; saldo portfela współdzielone z rodzeństwem.

Przejścia i następne ekrany: drzewo umiejętności łączy z definicją OCL-10 i aktualizacją instruktora INS-05, dokumenty z eksportem paszportu OCL-17, płatności z OCL-14, komunikat z OCL-15.

Zdarzenia systemowe: ostrzeżenie o zbliżającym się wygaśnięciu dokumentu, aktualizacja postępu po wpisie instruktora, sygnał o zaległej płatności.

Wymagania prawne: dane zdrowotne jako kategoria szczególna RODO z dostępem ograniczonym rolą, prawo opiekuna do wglądu i sprostowania, retencja dokumentów zgodna z polityką placówki, ślad audytu dla zmian danych wrażliwych.

Kontekst urządzenia: desktop i tablet recepcji, telefon do szybkiego wglądu w odczycie.

Lokalizacja i języki: etykiety jako klucze, daty i kwoty według regionu, nazwy dokumentów tłumaczone z fallbackiem.

Lista kontrolna heurystyk: rozpoznawanie zamiast przypominania przez pełny obraz w jednym miejscu; widoczność stanu systemu przez statusy dokumentów; zapobieganie błędom przez blokadę udziału bez zgody; pomoc w diagnozie przez czytelne ostrzeżenia o wygaśnięciu.

Metryki sukcesu: odsetek spraw rodzica obsłużonych z jednego ekranu, liczba dokumentów wygasłych niezauważonych, czas obsługi typowego zgłoszenia rodzica.

---

### OCL-10 - definicja drzewa umiejętności

Grupa i rola: organizator.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę zdefiniować drzewo umiejętności dla każdego poziomu - od podstaw pływania do zaawansowanych technik - tak żeby instruktor zaznaczał postęp, a rodzic widział, czego dziecko się uczy. Drzewo da się aktualizować z prostego arkusza, nie tylko z tabletu.

Punkty wejścia: menu zajęć, wybór poziomu z karty grupy OCL-02, link z karty uczestnika OCL-09.

Warunki wstępne: zdefiniowane poziomy, do których przypina się gałęzie umiejętności.

Dane wyświetlane: struktura drzewa z gałęziami i pojedynczymi umiejętnościami, powiązany poziom, opcjonalne filmy instruktażowe, kryteria zaliczenia umiejętności, wersja drzewa i historia zmian.

Komponenty interfejsu: edytor drzewa z hierarchią, formularz umiejętności z opisem i kryterium, miejsce na film instruktażowy, import z arkusza, podgląd drzewa oczami rodzica i instruktora.

Akcja główna: zapisz drzewo. Na pustym stanie dominującą akcją jest utworzenie pierwszej gałęzi lub import z arkusza.

Akcje drugorzędne: dodaj gałąź, dołącz film, importuj z arkusza, podejrzyj wersję rodzica, sklonuj drzewo na inny poziom.

Akcje niszczące: usuń umiejętność lub gałąź. Z ostrzeżeniem, że historia postępu dzieci oparta na tej umiejętności zostanie zarchiwizowana, nie skasowana.

Stany ekranu: ładowanie; pusty z wyborem między ręcznym tworzeniem a importem z arkusza i przykładem struktury; wypełniony to edytor drzewa; błąd importu pokazuje, który wiersz arkusza jest niepoprawny i czego brakuje; sukces po zapisie; brak uprawnień daje odczyt; offline nie dotyczy.

Walidacja i komunikaty: umiejętność bez nazwy blokuje zapis. Import z arkusza waliduje nagłówki kolumn i wskazuje wiersz z błędem oraz oczekiwany format. Cykliczna zależność gałęzi pokazuje, gdzie struktura zapętla się. Film w nieobsługiwanym formacie tłumaczy, jakie formaty są dozwolone.

Przypadki brzegowe: bardzo rozbudowane drzewo wymagające zwijania; import nadpisujący istniejące umiejętności z trwającym postępem dzieci; dwa poziomy współdzielące część gałęzi; arkusz z polskimi i angielskimi nazwami w jednym pliku.

Przejścia i następne ekrany: drzewo zasila aktualizację instruktora INS-05, podgląd postępu rodzica PAR-08, kartę uczestnika OCL-09 i eksport paszportu OCL-17.

Zdarzenia systemowe: wersjonowanie drzewa po zapisie, przeliczenie postępów po zmianie kryteriów, sygnał o zakończeniu importu z arkusza.

Wymagania prawne: nie dotyczy bezpośrednio, drzewo nie zawiera danych osobowych, dopiero powiązanie z dzieckiem podlega RODO.

Kontekst urządzenia: desktop do projektowania drzewa, tablet do podglądu, import z arkusza niezależny od urządzenia.

Lokalizacja i języki: nazwy umiejętności jako treść tłumaczona z kluczami, opisy z fallbackiem na en-GB, import obsługuje kolumny w kilku językach.

Lista kontrolna heurystyk: elastyczność przez dwie drogi edycji, ręczną i arkuszową; pomoc w diagnozie przez wskazanie błędnego wiersza importu; zapobieganie błędom przez wykrycie cyklu w strukturze; spójność przez jednolity format umiejętności.

Metryki sukcesu: odsetek aktualizacji drzewa zrobionych przez import z arkusza, liczba błędów importu na sto wierszy, czas zbudowania drzewa dla nowego poziomu.

---

### OCL-11 - przegląd obecności i frekwencji

Grupa i rola: organizator. Wpis pochodzi od instruktora w INS-04.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę zobaczyć frekwencję w grupach i u poszczególnych dzieci, wychwycić systematyczne nieobecności i powiązać je z prawem do odrabiania oraz płatnościami.

Punkty wejścia: kafelek frekwencji z pulpitu, karta grupy OCL-02, karta uczestnika OCL-09.

Warunki wstępne: istnieją zajęcia z odnotowaną obecnością.

Dane wyświetlane: frekwencja grupowa w czasie, frekwencja dzieci z trendem, oznaczenia nieobecności usprawiedliwionych i nie, powiązane prawa do odrobienia, wskaźnik ryzyka rezygnacji oparty na trendzie obecności.

Komponenty interfejsu: wykres frekwencji, tabela dzieci z trendem i liczbą nieobecności, filtr po grupie i okresie, znacznik nieobecności wymagających reakcji, eksport do arkusza.

Akcja główna: otwórz przypadek wymagający reakcji. Akcja wiodąca prowadzi do dziecka z niepokojącym trendem.

Akcje drugorzędne: filtruj okres i grupę, eksportuj frekwencję, wyślij przypomnienie do rodzica, oznacz nieobecność jako usprawiedliwioną.

Akcje niszczące: nie dotyczy, korekta wpisu obecności odbywa się jako edycja z zapisem zmiany, nie kasowanie.

Stany ekranu: ładowanie z szkieletem wykresu; pusty oznacza brak odnotowanych zajęć i tłumaczy, że dane pojawią się po pierwszych listach obecności; wypełniony to pełny przegląd; błąd ładowania z ponowieniem; sukces przy wysłanym przypomnieniu; brak uprawnień daje odczyt zagregowany; offline daje ostatni stan z banerem.

Walidacja i komunikaty: zakres dat z końcem przed początkiem prosi o korektę. Eksport bez zaznaczonego zakresu domyślnie obejmuje bieżący semestr i o tym informuje. Korekta obecności po długim czasie ostrzega, że wpłynie na statystyki i prawa do odrobienia.

Przypadki brzegowe: dziecko przeniesione między grupami w trakcie okresu; zajęcia odwołane przez placówkę liczone inaczej niż nieobecność dziecka; instruktor, który nie odprawił listy, zostawiający lukę w danych; sezon z wieloma świętami zniekształcający trend.

Przejścia i następne ekrany: nieobecność łączy z regułami odrabiania OCL-04, dziecko z kartą OCL-09, eksport z raportami OCL-16, przypomnienie z komunikacją OCL-15.

Zdarzenia systemowe: aktualizacja po odprawie listy obecności, przeliczenie wskaźnika ryzyka, sygnał o przekroczeniu progu nieobecności.

Wymagania prawne: dane o obecności jako dane osobowe dziecka, dostęp ograniczony rolą, eksport z odnotowaniem celu.

Kontekst urządzenia: desktop i tablet, telefon do szybkiego wglądu w trend.

Lokalizacja i języki: etykiety jako klucze, daty i okresy według regionu, kalendarz świąt zależny od kraju.

Lista kontrolna heurystyk: widoczność stanu systemu przez trend i wskaźnik ryzyka; zgodność ze światem przez rozróżnienie odwołania placówki i nieobecności dziecka; pomoc w diagnozie przez powiązanie nieobecności z prawem do odrobienia; zapobieganie błędom przez ostrzeżenie przy późnej korekcie.

Metryki sukcesu: odsetek wychwyconych dzieci zagrożonych rezygnacją, czas reakcji na niepokojący trend, kompletność odprawionych list obecności.

---

### OCL-12 - zarządzanie instruktorami

Grupa i rola: organizator.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę zarządzać kadrą instruktorów - przypisania do grup, dostępność, kwalifikacje, ewidencja godzin - tak żeby grafik nie miał luk, a wypłaty zgadzały się z faktycznymi zajęciami.

Punkty wejścia: alert kadrowy z pulpitu, przypisanie z karty grupy OCL-02, menu zajęć.

Warunki wstępne: istnieją zaproszeni lub dodani instruktorzy.

Dane wyświetlane: lista instruktorów ze statusem zaproszenia, przypisane grupy, deklarowana dostępność, kwalifikacje i ich ważność, ewidencja godzin z bieżącego okresu, konflikty grafiku.

Komponenty interfejsu: tabela instruktorów, panel przypisań do grup, podgląd dostępności z INS-07, lista kwalifikacji z datą ważności, podsumowanie godzin z INS-08.

Akcja główna: zaproś instruktora albo przypisz do grupy. Akcja wiodąca zależy od stanu, pusty stan prowadzi do zaproszenia.

Akcje drugorzędne: edytuj kwalifikacje, ustaw zastępstwo, podejrzyj godziny, dezaktywuj konto instruktora.

Akcje niszczące: usuń przypisanie albo dezaktywuj instruktora. Konta nie kasuje się trwale, dezaktywacja zachowuje historię zajęć i godzin.

Stany ekranu: ładowanie; pusty z propozycją zaproszenia pierwszego instruktora i wyjaśnieniem roli; wypełniony to lista kadry; błąd zapisu przy formularzu; sukces po przypisaniu z tostem; brak uprawnień daje odczyt; offline nie dotyczy.

Walidacja i komunikaty: przypisanie do grupy w terminie kolidującym z innym zajęciem pokazuje konflikt i proponuje wolne okno. Wygasła kwalifikacja blokuje przypisanie do grupy jej wymagającej i tłumaczy, czego brakuje. Zaproszenie na zajęty e-mail informuje, że osoba ma już konto, i proponuje powiązanie.

Przypadki brzegowe: instruktor pracujący w dwóch placówkach tego samego operatora; instruktor będący też rodzicem z osobnym kontem rodzinnym; zastępstwo łańcuchowe, gdy zastępca też jest niedostępny; kwalifikacja wygasająca w środku semestru.

Przejścia i następne ekrany: przypisanie aktualizuje grafik OCL-03 i kartę grupy OCL-02, dostępność łączy z INS-07, godziny z INS-08 i raportami OCL-16, zaproszenie uruchamia logowanie instruktora INS-01.

Zdarzenia systemowe: powiadomienie instruktora o przypisaniu, ostrzeżenie o wygasającej kwalifikacji, sygnał o luce w grafiku po dezaktywacji.

Wymagania prawne: dane kadrowe i kwalifikacje jako dane osobowe, ewidencja godzin zgodna z prawem pracy lub umową, dostęp do danych płacowych ograniczony rolą.

Kontekst urządzenia: desktop jako podstawowy, tablet do przeglądu.

Lokalizacja i języki: etykiety jako klucze, daty i godziny według regionu, nazwy kwalifikacji tłumaczone z fallbackiem.

Lista kontrolna heurystyk: zapobieganie błędom przez wykrycie konfliktu grafiku i wygasłej kwalifikacji; widoczność stanu systemu przez status zaproszeń i godzin; kontrola użytkownika przez dezaktywację zamiast kasowania; spójność przez jednolitą kartę instruktora.

Metryki sukcesu: liczba luk w grafiku spowodowanych brakiem kadry, odsetek instruktorów z aktualnymi kwalifikacjami, rozbieżność między ewidencją godzin a grafikiem.

---

## Partia 3 (OCL-13 do OCL-18)

### OCL-13 - konfiguracja rezerwacji lekcji indywidualnych

Grupa i rola: organizator.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę ustawić zasady rezerwacji lekcji jeden na jeden, tak żeby rodzic widział tylko terminy, w których dany trener i obiekt są naprawdę wolne, i mógł zarezerwować bez kontaktu z recepcją.

Punkty wejścia: menu zajęć, link z karty instruktora OCL-12, alert konfliktu rezerwacji z pulpitu.

Warunki wstępne: zdefiniowani instruktorzy z deklarowaną dostępnością, zdefiniowane sale lub tory z godzinami pracy.

Dane wyświetlane: reguły rezerwacji z oknem wyprzedzenia i oknem odwołania, powiązanie dostępności trenera z dostępnością obiektu, czas trwania i cena lekcji, bufor między lekcjami, podgląd dostępnych okien dla przykładowego trenera.

Komponenty interfejsu: formularz reguły rezerwacji, mapowanie trener plus obiekt, ustawienie buforów i okien czasowych, podgląd kalendarza dostępności wynikowej, przełącznik wymagania przedpłaty.

Akcja główna: zapisz regułę rezerwacji. Na pustym stanie dominującą akcją jest utworzenie pierwszej reguły.

Akcje drugorzędne: sklonuj regułę dla innego trenera, ustaw bufor, włącz przedpłatę, podejrzyj wynikowe okna.

Akcje niszczące: usuń regułę. Z ostrzeżeniem, że istniejące rezerwacje pozostają ważne, a usunięcie blokuje tylko nowe.

Stany ekranu: ładowanie; pusty z wyjaśnieniem mechaniki rezerwacji i przykładem; wypełniony to lista reguł; błąd zapisu przy formularzu; sukces z podglądem wynikowych okien; brak uprawnień daje odczyt; offline nie dotyczy.

Walidacja i komunikaty: okno wyprzedzenia i okno odwołania nie mogą być ujemne. Reguła wskazująca trenera bez dostępności pokazuje, że nie wygeneruje żadnych wolnych okien, i kieruje do INS-07. Kolizja obiektu z zajęciami grupowymi tłumaczy, że obiekt jest zajęty w danym oknie. Bufor dłuższy niż przerwa między dostępnościami ostrzega, że okna znikną.

Przypadki brzegowe: trener dostępny, ale obiekt zajęty zajęciami grupowymi; rezerwacja na styku dwóch okien dostępności; rodzic z saldem portfela pokrywającym lekcję; lekcja wymagająca konkretnego toru niedostępnego w danym dniu.

Przejścia i następne ekrany: reguła zasila rezerwację rodzica PAR-12 i kalendarz lekcji instruktora INS-09, dostępność łączy z INS-07, płatność z OCL-14, obiekty z ustawieniami placówki OCL-18.

Zdarzenia systemowe: przeliczenie wolnych okien po zmianie dostępności trenera, blokada okna po rezerwacji, zwolnienie okna po odwołaniu w dozwolonym czasie.

Wymagania prawne: zasady odwołania i zwrotu jako element regulaminu, jasna informacja o warunkach przedpłaty, dane rezerwacji ograniczone do potrzeby obsługi.

Kontekst urządzenia: desktop do konfiguracji, tablet do podglądu.

Lokalizacja i języki: etykiety jako klucze, czas i ceny według regionu, opisy reguł tłumaczone.

Lista kontrolna heurystyk: zapobieganie błędom przez sprzężenie dostępności trenera i obiektu; widoczność stanu systemu przez podgląd wynikowych okien; zgodność ze światem przez pojęcia okien i buforów znane z rezerwacji; pomoc w diagnozie przez wyjaśnienie, dlaczego okien brakuje.

Metryki sukcesu: odsetek rezerwacji bez interwencji recepcji, liczba kolizji obiektu po wdrożeniu, czas konfiguracji rezerwacji dla nowego trenera.

---

### OCL-14 - płatności, portfel i faktury

Grupa i rola: organizator.

Moduł: zajecia. Portfel i konto rodzinne wspólne z modułem zawodów.

Cel ekranu jako zadanie użytkownika: chcę zobaczyć wszystkie płatności placówki, obsłużyć zwroty i korekty, wystawić faktury, i zarządzać saldami portfeli rodzin, które są wspólne dla zajęć i zawodów.

Punkty wejścia: alert płatności z pulpitu, karta uczestnika OCL-09, menu zajęć.

Warunki wstępne: skonfigurowane plany cenowe OCL-07, podpięte bramki płatności operatora.

Dane wyświetlane: lista transakcji ze statusem i metodą, należności bieżące i przeterminowane, salda portfeli rodzin, faktury i ich status, zwroty i korekty z powodem. Metody obejmują Przelewy24, PayU, Stripe, BLIK oraz karty.

Komponenty interfejsu: tabela transakcji z filtrami, panel portfela rodziny, generator faktury, formularz zwrotu z powodem, podsumowanie należności.

Akcja główna: rozlicz wybraną należność albo wystaw fakturę, zależnie od kontekstu. Domyślnie akcja wiodąca prowadzi do obsługi najpilniejszej należności.

Akcje drugorzędne: filtruj transakcje, doładuj portfel rodziny, pobierz fakturę, oznacz płatność ręczną gotówką.

Akcje niszczące: zwrot środków. To operacja finansowa wymagająca wyraźnego potwierdzenia, z podaniem kwoty i powodu, bez kasowania historii transakcji.

Stany ekranu: ładowanie z szkieletem tabeli; pusty oznacza brak transakcji i tłumaczy, że pojawią się po pierwszych zapisach; wypełniony to pełna lista; błąd komunikacji z bramką pokazuje, że płatność jest w toku, i odradza ponawianie do czasu potwierdzenia; sukces po rozliczeniu z potwierdzeniem; brak uprawnień ukrywa dane finansowe; offline daje odczyt z banerem o wstrzymaniu operacji.

Walidacja i komunikaty: zwrot przekraczający wpłaconą kwotę blokuje się z wyjaśnieniem dostępnego limitu. Faktura bez kompletnych danych nabywcy prosi o ich uzupełnienie i wskazuje brakujące pole. Doładowanie portfela kwotą ujemną odrzuca się. Płatność w toku jest oznaczona, żeby nie obciążyć rodziny podwójnie.

Przypadki brzegowe: rodzina z saldem portfela rozliczającym część zajęć i część zawodów; zwrot za turnus odwołany przez placówkę; rozbieżność między płatnością bramki a zapisem po zerwanej sesji; korekta faktury po zmianie danych nabywcy.

Przejścia i następne ekrany: portfel łączy z PAR-10, faktura z eksportem OCL-22 w analogii do zawodów oraz raportami OCL-16, transakcja z kartą uczestnika OCL-09, ceny z OCL-07.

Zdarzenia systemowe: potwierdzenie płatności z bramki przez webhook, naliczenie cyklicznej opłaty, ostrzeżenie o przeterminowanej należności.

Wymagania prawne: faktura zgodna z lokalnymi przepisami podatkowymi, dane płatnicze obsługiwane przez bramki, a nie przechowywane w panelu, prawo do otrzymania dokumentu rozliczeniowego, ślad audytu dla zwrotów i korekt.

Kontekst urządzenia: desktop jako podstawowy ze względu na wagę operacji finansowych, tablet do przeglądu.

Lokalizacja i języki: kwoty, waluta i format faktury według regionu podatkowego, etykiety jako klucze, opisy transakcji tłumaczone.

Lista kontrolna heurystyk: zapobieganie błędom przez blokadę zwrotu ponad limit i oznaczenie płatności w toku; widoczność stanu systemu przez jawny status transakcji; pomoc w diagnozie przez powód zwrotu i historię; kontrola użytkownika przez wyraźne potwierdzenie operacji finansowych.

Metryki sukcesu: odsetek należności rozliczonych w terminie, liczba podwójnych obciążeń, czas wystawienia faktury, udział płatności obsłużonych bezobsługowo przez bramkę.

---

### OCL-15 - komunikacja

Grupa i rola: organizator. Komunikaty instruktora do grupy w INS-10.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę wysyłać wiadomości do właściwych odbiorców - jednej grupy, rodziców z zaległą płatnością, listy rezerwowej - przez kanał, na który rodzina wyraziła zgodę, z gotowych szablonów.

Punkty wejścia: akcja z listy uczestników OCL-08, alert z pulpitu, odwołanie zajęć z grafiku OCL-03, menu zajęć.

Warunki wstępne: istnieją odbiorcy z danymi kontaktu i zgodami na kanał.

Dane wyświetlane: lista wysłanych i zaplanowanych wiadomości, szablony komunikatów, segmenty odbiorców, kanały e-mail, push i SMS ze statusem zgody, statystyki dostarczenia i otwarcia.

Komponenty interfejsu: edytor wiadomości z polami personalizacji, wybór segmentu, wybór kanału z informacją o zgodach, podgląd wiadomości oczami rodzica, planowanie wysyłki.

Akcja główna: wyślij wiadomość. W trybie planowania akcją wiodącą jest zaplanowanie wysyłki.

Akcje drugorzędne: zapisz szablon, zaplanuj wysyłkę, wyślij test do siebie, sklonuj wcześniejszą wiadomość.

Akcje niszczące: anuluj zaplanowaną wysyłkę. Wysłanej wiadomości nie da się cofnąć, o czym ekran uprzedza przed wysyłką.

Stany ekranu: ładowanie; pusty z propozycją wyboru szablonu i wyjaśnieniem segmentów; wypełniony to historia i edytor; błąd wysyłki pokazuje, ilu odbiorców nie dostało wiadomości i dlaczego; sukces z podsumowaniem dostarczeń; brak uprawnień daje odczyt; offline pozwala przygotować wersję roboczą z banerem o wstrzymaniu wysyłki.

Walidacja i komunikaty: wysyłka na kanał bez zgody odbiorcy pomija go i jawnie informuje, ilu odbiorców pominięto z powodu braku zgody. Pusta treść blokuje wysyłkę. Pole personalizacji bez wartości dla części odbiorców ostrzega i proponuje wartość zapasową. Wysyłka do dużego segmentu prosi o potwierdzenie liczby odbiorców.

Przypadki brzegowe: rodzina z dziećmi w dwóch grupach trafiająca podwójnie do segmentu, scalana do jednej wiadomości; odbiorca, który wycofał zgodę po zaplanowaniu wysyłki; wiadomość wielojęzyczna dla rodzin o różnych ustawieniach języka; SMS przekraczający limit znaków.

Przejścia i następne ekrany: segment pochodzi z listy uczestników OCL-08, statystyki łączą z raportami OCL-16, odwołanie zajęć z grafiku OCL-03 wstępnie wypełnia szablon, zgody z ustawieniami OCL-18.

Zdarzenia systemowe: potwierdzenia dostarczenia z bramek wiadomości, wyzwalacze automatyczne przy odwołaniu zajęć lub ofercie z listy rezerwowej, raport otwarć.

Wymagania prawne: zgoda na kanał komunikacji jako warunek wysyłki marketingowej, rozdzielenie komunikatów obsługowych od marketingowych, łatwa rezygnacja z subskrypcji, dane kontaktowe pod RODO.

Kontekst urządzenia: desktop i tablet, telefon do szybkiej wysyłki krótkiego komunikatu.

Lokalizacja i języki: treść wiadomości w języku odbiorcy z fallbackiem na en-GB, etykiety interfejsu jako klucze, daty i godziny planowania według regionu.

Lista kontrolna heurystyk: zapobieganie błędom przez pomijanie odbiorców bez zgody i ostrzeżenie przy dużym segmencie; widoczność stanu systemu przez statystyki dostarczeń; kontrola użytkownika przez test do siebie i anulowanie planu; pomoc i dokumentacja przez gotowe szablony.

Metryki sukcesu: wskaźnik dostarczenia i otwarcia, liczba wiadomości wysłanych z naruszeniem zgody, odsetek komunikatów wysłanych z szablonu.

---

### OCL-16 - raporty

Grupa i rola: organizator.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę dostać gotowe raporty o przychodach, frekwencji, retencji i obłożeniu grup, żeby podejmować decyzje o ofercie i kadrze na podstawie liczb, nie przeczuć.

Punkty wejścia: kafelek raportów z pulpitu, link z frekwencji OCL-11 lub płatności OCL-14, menu zajęć.

Warunki wstępne: istnieją dane operacyjne z zajęć, płatności i obecności.

Dane wyświetlane: zestawy raportów przychodu, frekwencji, retencji i obłożenia, filtry okresu i grupy, wykresy z trendem, porównanie okresów, wskaźnik utrzymania uczestników między semestrami.

Komponenty interfejsu: galeria gotowych raportów, panel filtrów, wykresy, tabela z danymi źródłowymi, przycisk eksportu, planowanie raportu cyklicznego na e-mail.

Akcja główna: otwórz raport. Po otwarciu akcją wiodącą jest dostosowanie zakresu i eksport.

Akcje drugorzędne: zmień okres, porównaj z poprzednim okresem, zaplanuj raport cykliczny, przejdź do danych źródłowych.

Akcje niszczące: nie dotyczy, raporty są tylko do odczytu i eksportu.

Stany ekranu: ładowanie z szkieletem wykresów; pusty oznacza za mało danych do raportu i tłumaczy, ile danych potrzeba; wypełniony to gotowy raport; błąd liczenia pokazuje, który zestaw nie policzył się i proponuje ponowienie; sukces przy eksporcie; brak uprawnień ogranicza raporty finansowe; offline daje ostatnio policzony raport z datą.

Walidacja i komunikaty: zakres dat z końcem przed początkiem prosi o korektę. Porównanie okresów o różnej długości ostrzega, że dane nie są wprost porównywalne. Eksport bardzo dużego zakresu informuje, że może potrwać, i proponuje zawężenie.

Przypadki brzegowe: semestr z wieloma odwołaniami zniekształcający frekwencję; nowa placówka bez danych historycznych do porównania; raport łączący zajęcia i zawody dla rodzin obecnych w obu modułach; zmiana cennika w środku okresu wpływająca na przychód.

Przejścia i następne ekrany: dane źródłowe prowadzą do frekwencji OCL-11 i płatności OCL-14, eksport do formatu arkusza analogicznie do OCL-22 w module zawodów, raport cykliczny do komunikacji OCL-15.

Zdarzenia systemowe: nocne przeliczenie raportów, wysyłka raportu cyklicznego, sygnał o gotowości dużego eksportu.

Wymagania prawne: raporty zagregowane bez ekspozycji danych wrażliwych dzieci, eksport z danymi osobowymi z odnotowaniem celu i zakresu, dostęp do raportów finansowych ograniczony rolą.

Kontekst urządzenia: desktop jako podstawowy, tablet do przeglądu wykresów.

Lokalizacja i języki: liczby, waluty i daty według regionu, etykiety i nazwy raportów jako klucze tłumaczeń.

Lista kontrolna heurystyk: zgodność ze światem przez raporty nazwane językiem decyzji biznesowej; zapobieganie błędom przez ostrzeżenie o nieporównywalnych okresach; widoczność stanu systemu przez datę ostatniego przeliczenia; elastyczność przez gotowe raporty i własne zakresy.

Metryki sukcesu: częstotliwość korzystania z raportów przez właściciela, odsetek decyzji o ofercie popartych raportem, czas od pytania biznesowego do gotowej liczby.

---

### OCL-17 - konfiguracja i eksport paszportu sportowego

Grupa i rola: organizator.

Moduł: zajecia. Paszport wspólny dla zajęć i zawódów w ramach konta rodzinnego.

Cel ekranu jako zadanie użytkownika: chcę ustawić, co znajdzie się w paszporcie sportowym dziecka - postępy umiejętności, frekwencja, osiągnięcia z zawodów - i umożliwić rodzicowi eksport spójnego dokumentu PDF dla obu modułów.

Punkty wejścia: menu zajęć, link z karty uczestnika OCL-09, ustawienia placówki OCL-18.

Warunki wstępne: zdefiniowane drzewo umiejętności OCL-10, istnieją dane postępu i obecności, opcjonalnie wyniki z modułu zawodów.

Dane wyświetlane: szablon paszportu z sekcjami do włączenia, mapowanie danych z zajęć i zawodów, podgląd wygenerowanego dokumentu, ustawienia widoczności sekcji dla rodzica, branding placówki na dokumencie z tokenów marki.

Komponenty interfejsu: edytor sekcji paszportu, przełączniki widoczności, podgląd PDF, wybór zakresu danych, ustawienie nagłówka i stopki z tokenów marki.

Akcja główna: zapisz konfigurację paszportu. Akcją alternatywną przy podglądzie jest wygenerowanie przykładowego dokumentu.

Akcje drugorzędne: podejrzyj PDF, włącz sekcję zawodów, ustaw branding, ogranicz zakres dat.

Akcje niszczące: nie dotyczy na poziomie konfiguracji, rodzic eksportuje kopię bez wpływu na dane źródłowe.

Stany ekranu: ładowanie; pusty z propozycją włączenia podstawowych sekcji i przykładem; wypełniony to edytor konfiguracji; błąd generowania podglądu pokazuje przyczynę i proponuje ponowienie; sukces po zapisie; brak uprawnień daje odczyt; offline nie dotyczy dla konfiguracji.

Walidacja i komunikaty: paszport bez włączonej żadnej sekcji ostrzega, że dokument byłby pusty. Sekcja zawodów włączona dla dziecka bez danych z zawodów tłumaczy, że pojawi się dopiero po pierwszym starcie. Eksport z danymi wrażliwymi prosi o potwierdzenie, że dokument trafia do opiekuna.

Przypadki brzegowe: dziecko obecne tylko w jednym module; zmiana drzewa umiejętności po wygenerowaniu wcześniejszego paszportu; rodzeństwo z osobnymi paszportami w jednym koncie rodzinnym; eksport w języku innym niż roboczy placówki.

Przejścia i następne ekrany: konfiguracja zasila eksport rodzica PAR-13, dane pochodzą z drzewa OCL-10, frekwencji OCL-11 i karty uczestnika OCL-09, branding z OCL-18.

Zdarzenia systemowe: regeneracja podglądu po zmianie konfiguracji, sygnał o gotowym dokumencie, aktualizacja paszportu po nowych danych postępu.

Wymagania prawne: paszport zawiera dane osobowe dziecka i ewentualnie dane wrażliwe, eksport tylko do opiekuna, zgodność z prawem do przenoszenia danych pod RODO, branding bez naruszenia praw osób trzecich.

Kontekst urządzenia: desktop do konfiguracji, podgląd PDF niezależny od urządzenia.

Lokalizacja i języki: dokument generowany w języku wybranym przez rodzica z fallbackiem, daty i jednostki według regionu, etykiety sekcji jako klucze tłumaczeń.

Lista kontrolna heurystyk: spójność i standardy przez jeden paszport dla obu modułów; widoczność stanu systemu przez podgląd przed eksportem; zgodność ze światem przez sekcje czytelne dla rodzica; zapobieganie błędom przez ostrzeżenie o pustym dokumencie.

Metryki sukcesu: odsetek rodzin korzystających z eksportu paszportu, kompletność danych w paszporcie, spójność dokumentu między modułami zajęć i zawodów.

---

### OCL-18 - ustawienia placówki oraz role i uprawnienia

Grupa i rola: organizator.

Moduł: zajecia.

Cel ekranu jako zadanie użytkownika: chcę skonfigurować placówkę - sale i tory, godziny pracy, branding, role i uprawnienia zespołu - tak żeby reszta panelu działała na poprawnych danych, a każdy widział tylko to, do czego ma prawo.

Punkty wejścia: menu ustawień, pusty stan pulpitu przy pierwszym uruchomieniu, link z grafiku OCL-03 przy konflikcie sali.

Warunki wstępne: aktywne konto organizatora z włączonym modułem zajęć.

Dane wyświetlane: dane placówki i godziny pracy, lista sal lub torów z pojemnością, tokeny marki jako zmienne CSS, lista osób z przypisanymi rolami, macierz uprawnień ról operator, organizator, instruktor, wolontariusz, rodzic, uczestnik.

Komponenty interfejsu: formularz danych placówki, edytor sal i torów, edytor tokenów marki z podglądem, tabela osób z rolami, macierz uprawnień z przełącznikami.

Akcja główna: zapisz ustawienia. W sekcji ról akcją wiodącą jest przypisanie roli osobie.

Akcje drugorzędne: dodaj salę, zmień tokeny marki, zaproś osobę do zespołu, podejrzyj branding na przykładowym ekranie.

Akcje niszczące: usuń salę lub odbierz rolę. Usunięcie sali z przypisanymi zajęciami jest blokowane, a odebranie ostatniej roli organizatora wymaga wskazania następcy. Zmiana uprawnień jako modyfikacja kontroli dostępu wymaga wyraźnego potwierdzenia.

Stany ekranu: ładowanie; pusty przy pierwszym uruchomieniu prowadzi krok po kroku przez dane placówki, sale i branding; wypełniony to pełne ustawienia; błąd zapisu przy sekcji; sukces z tostem; brak uprawnień ukrywa sekcję ról przed osobami bez prawa do zarządzania zespołem; offline nie dotyczy.

Walidacja i komunikaty: pojemność sali jako liczba dodatnia. Godziny pracy z końcem przed początkiem proszą o korektę. Usunięcie sali z zajęciami pokazuje, które grupy jej używają. Tokeny marki łamiące kontrast WCAG ostrzegają i pokazują, które zestawienie kolorów nie spełnia minimum. Odebranie roli sobie samemu pyta o potwierdzenie z ostrzeżeniem o utracie dostępu.

Przypadki brzegowe: placówka z salami w dwóch lokalizacjach; osoba z rolą instruktora w zajęciach i wolontariusza w zawodach; zmiana brandingu w trakcie sezonu odbijająca się na wszystkich brandowanych stronach; tor współdzielony przez zajęcia grupowe i lekcje indywidualne.

Przejścia i następne ekrany: sale zasilają grafik OCL-03 i rezerwacje OCL-13, role łączą z zarządzaniem instruktorami OCL-12, branding wpływa na strony publiczne grupy GST-03 i paszport OCL-17, uprawnienia rzutują na widoczność wszystkich ekranów panelu.

Zdarzenia systemowe: propagacja brandingu po zapisie tokenów, przeliczenie dostępności obiektów po zmianie godzin pracy, log zmian uprawnień do audytu.

Wymagania prawne: zmiany uprawnień i ról jako operacje istotne dla bezpieczeństwa danych, ślad audytu dla modyfikacji dostępu, dane zespołu pod RODO, branding bez naruszenia znaków towarowych osób trzecich. Modyfikacja kontroli dostępu pozostaje świadomą decyzją organizatora wykonywaną w panelu, nie automatem.

Kontekst urządzenia: desktop jako podstawowy ze względu na wagę ustawień, tablet do przeglądu.

Lokalizacja i języki: nazwy sal jako treść organizatora, etykiety jako klucze, godziny i daty według regionu, przełącznik języka i lista języków placówki konfigurowane tutaj wraz z językiem zapasowym.

Lista kontrolna heurystyk: zapobieganie błędom przez blokadę usunięcia sali w użyciu i kontrolę kontrastu marki; widoczność stanu systemu przez podgląd brandingu; kontrola i swoboda przez wskazanie następcy przed odebraniem roli; spójność przez jedną macierz uprawnień dla wszystkich ról.

Metryki sukcesu: czas konfiguracji nowej placówki, liczba ekranów z naruszonym kontrastem marki, liczba błędów dostępu zgłoszonych przez zespół, kompletność przypisania ról.

---

## Literatura

Nielsen, J. (1994). *10 usability heuristics for user interface design*. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

W3C. (2018). *Web Content Accessibility Guidelines (WCAG) 2.1*. World Wide Web Consortium. https://www.w3.org/TR/WCAG21/
