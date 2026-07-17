# Master promptbook - platforma zapisów na biegi

Wersja 1.0 z 17 lipca 2026. Zakres: faza 1, wyłącznie moduł zawodów (grupy ekranów GST, OEV, ATH, VOL, OPR z manifestu v7). Moduł zajęć i szkółek (OCL, INS, PAR, KID) celowo poza zakresem - wraca w fazie 2, a model danych z tej fazy jest na to przygotowany.

Dokument służy jako scenariusz wdrożenia prowadzonego agentem kodującym (Claude Code lub podobnym). Składa się z jednego promptu-matki (M0), którą wkleja się do każdej sesji, oraz z sekwencji promptów roboczych A1-H2, z których każdy domyka jeden pakiet pracy. Aneksy na końcu zbierają model danych, analizę konkurencji z propozycją cennika, reguły przenośności Vercel - własny serwer, mapę wszystkich ekranów manifestu na prompty oraz checklisty RODO.

---

## 1. Jak korzystać z promptbooka

Zasada podstawowa: jedna sesja agenta = jeden prompt roboczy. Każdą sesję zaczynasz od wklejenia promptu M0 (sekcja 4), potem wklejasz właściwy prompt roboczy. Materiały projektowe (specyfikacje, kontrakty, manifest) commitujesz do repozytorium w `docs/design/` już w kroku A1 - od tego momentu agent czyta je sam z dysku i nie trzeba ich wklejać.

Prompt roboczy ma zawsze pięć części: cel, pliki kontekstu (co agent ma przeczytać przed pracą), treść do wklejenia, kryteria akceptacji i weryfikację ręczną (co sam klikasz po zakończeniu). Nie przechodź do następnego promptu, dopóki kryteria akceptacji poprzedniego nie są spełnione - kolejne prompty zakładają, że poprzednie działają.

Gdy agent zgłosi konflikt między promptem a specyfikacją ekranu w `docs/design/`, specyfikacja wygrywa w sprawach zachowania produktu, a prompt wygrywa w sprawach zakresu (prompty MVP świadomie wycinają część funkcji ze specyfikacji na później).

Definicja ukończenia pakietu (obowiązuje w każdym prompcie, M0 ją powtarza): kod przechodzi `pnpm lint`, `pnpm typecheck`, `pnpm test`, migracje odpalają się na czystej bazie, seed działa, a agent kończy raportem: co zrobił, czego nie zrobił, jakie decyzje podjął sam.

Konwencja wersjonowania: po każdym ukończonym pakiecie commit z prefiksem pakietu, np. `B5: zapis zawodnika - formularz krokowy i zgody`. Gałąź `main` ma zawsze działać.

---

## 2. Decyzje zamrożone (stan na 17 lipca 2026)

Poniższe decyzje zapadły przed powstaniem promptbooka i wszystkie prompty je zakładają. Zmiana którejkolwiek wymaga przejrzenia promptów, które na niej stoją.

1. Płatności: model marketplace na Stripe Connect (destination charges z `application_fee_amount`). Platforma przyjmuje płatność (BLIK, karty, Przelewy24 jako metody płatności w Stripe), Stripe wypłaca organizatorowi, prowizja platformy potrąca się automatycznie. Licencja płatnicza jest po stronie Stripe - platforma nie przechowuje środków na własnych rachunkach. Warstwa płatności powstaje za interfejsem `PaymentProvider`, a w etapie I dochodzą dwie krajowe bramki w tym samym modelu marketplace: PayU Marketplace i Paynow (mElements/mBank) - obie z boardingiem sprzedawców i automatycznym potrącaniem prowizji platformy, obie wyraźnie tańsze od Stripe na wolumenie krajowym (analiza w aneksie 2).
2. Zakres pilotażu: wąski MVP - kreator wydarzenia, landing w subdomenie z brandingiem, formularz zapisu z polami warunkowymi, płatność, numer startowy po wpłacie, lista uczestników, eksporty, e-maile transakcyjne, zgody RODO. Samoobsługa, portfel, gadżety, wolontariusze, serie i API publiczne wchodzą etapami D-G po pilotażu.
3. Stack: Next.js (App Router, TypeScript strict), Tailwind CSS, PostgreSQL (w PoC Neon, docelowo własny Postgres), Drizzle ORM z migracjami SQL, Auth.js (magic link + Google), S3-kompatybilny storage (w PoC Cloudflare R2 albo AWS S3, docelowo MinIO), Resend za interfejsem `EmailProvider` (docelowo SMTP). Monorepo pnpm: `apps/web` (aplikacja), `packages/embed` (widget), `packages/shared` (typy wspólne).
4. Hosting: PoC na Vercel, produkcja docelowo na własnym serwerze w Dockerze. Konsekwencja: zakaz używania czegokolwiek, co istnieje tylko na Vercel (aneks 3 zawiera pełną listę zakazów). Vercel ma być wyłącznie wygodnym miejscem uruchomienia standardowej aplikacji Next.js.
5. Makiety: grupy GST, OEV i OPR nie dostają osobnych makiet hi-fi - implementacja powstaje wprost ze specyfikacji lo-fi i kontraktów projektowych, a wzorcem estetyki i mechaniki (tokeny, i18n, stany ekranu) są gotowe makiety ATH i VOL.
6. Języki: interfejs pl-PL z zapasowym en-GB, wszystkie napisy jako klucze tłumaczeń od pierwszego dnia (dokładnie tak, jak robią to makiety ATH). Kod, identyfikatory i komentarze po angielsku.
7. Waluta: PLN. Model danych trzyma kwoty w groszach (integer) z polem waluty, więc EUR nie wymaga zmian schematu.
8. Pomiar czasu: zewnętrzny partner. Platforma wystawia kontrakt integracyjny (eksport listy startowej, import wyników, webhooki) i adapter na partnera - etap F.

Placeholder `{DOMENA}` w promptach oznacza produkcyjną domenę platformy (np. `zapisy.example.pl`) - podstaw własną, gdy będzie kupiona. Subdomeny organizatorów mają postać `{slug-organizatora}.{DOMENA}`.

---

## 3. Mapa etapów i kolejność pracy

Etapy idą sekwencyjnie, prompty wewnątrz etapu przeważnie też, wyjątki zaznaczono. Szacunki sesji zakładają agenta klasy Claude Code na średnim projekcie - traktuj je jako rząd wielkości, nie zobowiązanie.

| Etap | Treść | Prompty | Szacunek sesji | Efekt końcowy |
|---|---|---|---|---|
| A | Fundamenty: repo, baza, tenancy, auth, storage, e-mail, design system | A1-A6 | 8-12 | działający szkielet z logowaniem na subdomenach |
| B | Ścieżka pieniądza (MVP pilotażu) | B1-B8 | 14-20 | pierwszy prawdziwy zapis z płatnością i numerem |
| C | Twardnienie przed pilotażem: testy, monitoring, backupy, checklista | C1-C3 | 4-6 | pilotaż na żywym biegu |
| D | Samoobsługa i przewagi: transfery, listy rezerwowe, portfel, gadżety, komunikacja, raporty, serie, wolontariusze | D1-D6 | 12-18 | pełny standard rynkowy + wyróżniki |
| E | API publiczne i widget do osadzania | E1-E3 | 5-8 | organizator osadza zapisy u siebie |
| F | Pomiar czasu: kontrakt integracyjny i wyniki | F1-F2 | 3-5 | partner pobiera listy, wyniki wracają na landing |
| G | Panel operatora platformy i rozliczenia prowizji | G1-G2 | 4-6 | obsługa wielu organizatorów bez ręcznej roboty |
| H | Migracja na własny serwer | H1-H2 | 2-4 | `docker compose up` na VPS, runbook przejścia |
| I | Rozszerzenie płatności: PayU Marketplace i Paynow | I1-I3 | 6-9 | wolumen krajowy na tańszej bramce, Stripe jako uzupełnienie |
| J | Zdjęcia z biegu: moduł RunPixie | J1-J2 | 4-6 | organizator wgrywa zdjęcia, zawodnik dostaje swoje - darmo albo w sprzedaży |

Kolejność między etapami D-G jest częściowo elastyczna: E (API i widget) można zacząć zaraz po C, jeśli pierwszy klient tego potrzebuje; F zależy tylko od B; G może iść równolegle z D. H jest ostatnie z natury, ale prompt H1 (dockeryzacja) warto uruchomić wcześniej choćby po to, żeby sprawdzić, że zakazy przenośności są przestrzegane - to test, nie tylko migracja. Etap I zależy technicznie tylko od B6 (i zyskuje na gotowym G2), ale jego prawdziwym warunkiem są podpisane warunki marketplace z PayU i mElements - zapytania ofertowe wyślij dużo wcześniej (sekcja 16), a prompty I1-I3 uruchamiaj, gdy umowy są na stole. Etap J (zdjęcia) wymaga B5, B6 i D4, a rozliczeniowo G2 - naturalny moment to okolice etapu F, żeby wyniki i zdjęcia spięły razem doświadczenie dnia zawodów; kontrakt integracyjny z zespołem RunPixie uzgodnij wcześniej (sekcja 16, punkt 8).

Ścieżka krytyczna do pierwszego przychodu: A1 → A2 → A3 → A4 → A5 → A6 → B1 → B2 → B3 → B4 → B5 → B6 → B7 → B8 → C1 → C2 → C3.

---

## 4. Prompt M0 - kontekst-matka (wklejany na początku każdej sesji)

Poniższy blok kopiujesz w całości jako pierwszą wiadomość każdej sesji agenta. Jest rozmyślnie zwięzły: szczegóły behawioralne żyją w `docs/design/`, a M0 mówi agentowi, kim jest, co buduje i jakich zasad nie wolno mu łamać.

```text
KONTEKST PROJEKTU (M0, wersja 1.0)

Budujesz platformę internetową dla organizatorów imprez biegowych - zapisy,
płatności, obsługa uczestników. Konkurencja: RunSignup.com, datasport.pl.
Platforma należy do RunPixie (serwis zdjęć z biegów z rozpoznawaniem
zawodników) - moduł zdjęć (etap J) jest więc integracją pierwszostronną
wewnątrz jednej firmy, nie adapterem do obcego dostawcy.
Faza 1 obejmuje wyłącznie moduł zawodów. Moduł zajęć sportowych (szkółki)
wróci w fazie 2 - projektuj dane tak, żeby go nie zablokować, ale nie buduj
go.

ROLE W SYSTEMIE
- Gość: przegląda wydarzenia, może zapisać się bez konta (konto powstaje
  w trakcie zapisu).
- Zawodnik (athlete): zapisuje się, płaci, zarządza swoim zgłoszeniem.
- Organizator (organizer): tworzy wydarzenia, konfiguruje zapisy i ceny,
  zarządza uczestnikami. Ma członków zespołu z rolami.
- Wolontariusz (volunteer): pomaga w dniu zawodów (odprawa, wydawanie
  pakietów) - etap D/VOL.
- Operator platformy (operator): my; zarządza organizatorami, prowizjami
  i rozliczeniami.

ARCHITEKTURA (ZAMROŻONA)
- Monorepo pnpm: apps/web (Next.js App Router, TypeScript strict,
  Tailwind), packages/embed (widget), packages/shared (typy wspólne).
- PostgreSQL + Drizzle ORM, migracje generowane do SQL i commitowane.
- Auth.js: magic link (e-mail) + Google OAuth. Sesje w bazie.
- Storage plików: S3 API przez jeden moduł src/lib/storage.ts (podpisane
  URL-e). Zakaz bezpośrednich zależności od konkretnego dostawcy poza tym
  modułem.
- E-mail: interfejs EmailProvider w src/lib/email/, implementacja Resend,
  szablony react-email. Zakaz wysyłki poza tym interfejsem.
- Płatności: Stripe Connect, destination charges z application_fee_amount,
  za interfejsem PaymentProvider w src/lib/payments/. Konto Connect
  organizatora typu Standard lub Express (decyzja w B6). Interfejs od
  początku projektowany pod wielu dostawców: w etapie I dochodzą PayU
  Marketplace i Paynow Marketplace, z routingiem aktywnej bramki per
  organizator - każda transakcja pamięta swojego dostawcę.
- Multi-tenancy: organizator = tenant. Subdomena {slug}.{DOMENA} przez
  middleware przepisywana na trasy /o/[organizerSlug]/... Dev lokalny:
  {slug}.lvh.me:3000.
- Harmonogram zadań: wyłącznie endpointy POST /api/cron/{nazwa}
  zabezpieczone sekretem, wołane z zewnątrz (Vercel Cron w PoC, systemowy
  cron po migracji). Zakaz bibliotek trzymających stan schedulera w
  procesie.
- Kolejka zadań: tabela jobs w Postgresie + tick z crona. Bez Redis w MVP.

PRZENOŚNOŚĆ (TWARDE ZAKAZY - PoC stoi na Vercel, produkcja na własnym
serwerze w Dockerze)
- Zakaz: Vercel KV, Vercel Blob, Vercel Postgres SDK, Edge Runtime w
  trasach (runtime = 'nodejs' wszędzie, gdzie jest wybór), @vercel/og
  poza wariantem działającym w node, funkcje serverless zależne od
  vendora, localStorage/sessionStorage jako magazyn stanu aplikacji.
- next.config: output 'standalone' od pierwszego dnia.
- Wszystkie sekrety i konfiguracja przez zmienne środowiskowe walidowane
  zodem w src/env.ts. Żadnych odwołań do process.env poza tym plikiem.

DOMENA - REGUŁY NIENARUSZALNE
- Numer startowy nadaje się dopiero po zaksięgowaniu wpłaty. Wcześniej
  istnieje zgłoszenie ze statusem płatności.
- Cykl życia zgłoszenia: draft -> pending_payment -> paid oraz stany
  końcowe/boczne: expired, cancelled, refunded, transferred, deferred,
  waitlisted. Przejścia wyłącznie przez funkcje domenowe z logiem audytu.
- Cena wynika z progu cenowego (price tier) aktywnego w chwili wejścia do
  koszyka; zmiana progu w trakcie sesji zapisu musi być jawnie pokazana
  przed płatnością.
- Miejsce na dystansie rezerwuje się na czas finalizacji (hold z TTL);
  wygasły hold zwalnia miejsce.
- Kwoty w groszach (integer), pole currency, formatowanie przez Intl.
- Wszystkie widoczne napisy to klucze tłumaczeń (pl-PL domyślny, en-GB
  zapasowy), daty i kwoty przez Intl według locale konta.
- Dostępność: WCAG 2.1 AA, cele dotykowe min. 48 px, focus-visible,
  prefers-reduced-motion. Mobile-first.
- Branding organizatora wyłącznie przez tokeny CSS (--brand-ink, --accent,
  kolory semantyczne) - wzorzec w docs/design/ATH_kontrakt-projektowy_hifi.md.
- RODO: minimalizacja danych, zgody wersjonowane (dokument + wersja + data
  + IP), dane zdrowotne tylko za osobną zgodą, prawo do wglądu i usunięcia
  przewidziane w modelu danych.
- Zdjęcia (etap J): rozpoznawanie działa po numerze startowym, więc
  obejmuje wszystkie opłacone zgłoszenia i wszystkie rozpoznane numery
  liczą się do rozliczenia 1 zł od osoby - organizator deklaruje to
  z góry, włączając tryb darmowy; nie potwierdza niczego per partia.
  Zgody nie wpływają na koszt: sterują wyłącznie tym, które numery są
  widoczne w wyszukiwarce i widokach publicznych.

KONWENCJE KODU
- Identyfikatory, komentarze, commity: angielski. UI: klucze tłumaczeń.
- Słownik domenowy EN <-> PL: event = wydarzenie, edition = edycja,
  distance = dystans, category = kategoria wiekowa/płciowa, price tier =
  próg cenowy, registration = zgłoszenie, participant = uczestnik, bib =
  numer startowy, addon = dodatek/gadżet, variant = wariant (rozmiar),
  waitlist = lista rezerwowa, transfer = przepisanie pakietu, deferral =
  odroczenie na kolejną edycję, wallet = portfel (kredyt u organizatora),
  consent = zgoda, series = cykl/seria biegów, hold = rezerwacja miejsca.
- Logika domenowa w src/modules/{moduł}/ (czyste funkcje + repozytoria),
  trasy App Routera cienkie. Walidacja wejścia zodem na granicy.
- Testy: Vitest dla domeny (money flow, przejścia statusów, holdy
  obowiązkowo), Playwright dla ścieżek krytycznych.
- Migracje: drizzle-kit generate, pliki SQL commitowane, bez edycji
  wykonanych migracji.

MATERIAŁY PROJEKTOWE
W docs/design/ leżą: manifest_ekranow_v7.csv (spis wszystkich ekranów),
specyfikacje grup (ATH_spec.md, VOL_spec.md - pełne opisy zachowań ekranów),
kontrakty projektowe hi-fi (tokeny, typografia, wzorce) i makiety HTML grup
ATH i VOL. Grupy GST/OEV/OPR mają wiersze w manifeście, ale nie mają makiet -
implementujesz je zgodnie z konwencjami z kontraktów i spójnie z ATH/VOL.
Zanim zbudujesz ekran, przeczytaj jego wiersz w manifeście i sekcję w
specyfikacji, jeśli istnieje.

DEFINICJA UKOŃCZENIA (KAŻDY PAKIET)
pnpm lint && pnpm typecheck && pnpm test przechodzą; migracje działają na
czystej bazie; pnpm db:seed odtwarza dane demo; krótki raport końcowy: co
zrobione, co pominięte, jakie decyzje podjąłeś samodzielnie i dlaczego.
```

---
## 5. Etap A - fundamenty

Etap A kończy się szkieletem aplikacji: monorepo z CI, pełny schemat bazy, subdomeny organizatorów, logowanie magic linkiem, storage plików, wysyłka e-maili i design system przeniesiony z kontraktów projektowych. Nic tu jeszcze nie sprzedaje - ale wszystko poniżej etapu B na tym stoi.

### Prompt A1 - monorepo, narzędzia, CI, materiały projektowe

Cel: powstaje repozytorium, w którym każda kolejna sesja agenta ma identyczne warunki pracy.

Pliki kontekstu: zawartość paczki projektowej (cały katalog z manifestem, specyfikacjami i makietami) dostępna lokalnie do skopiowania.

Prompt do wklejenia:

```text
Zainicjalizuj monorepo platformy zapisów zgodnie z M0.

1. pnpm workspace: apps/web (create-next-app: App Router, TypeScript,
   Tailwind, ESLint), packages/shared (tsconfig + pusty pakiet typów),
   packages/embed (pusty pakiet z vite w trybie library - wypełnimy w E3).
2. TypeScript strict we wszystkich pakietach; wspólny tsconfig bazowy.
3. Prettier + ESLint (flat config) + skrypty root: lint, typecheck, test,
   dev, build. Vitest skonfigurowany w apps/web i packages/shared.
4. next.config.ts: output 'standalone'. Utwórz src/env.ts z walidacją zod
   (na razie DATABASE_URL, APP_URL, NODE_ENV) i wepnij wywołanie walidacji
   do startu aplikacji.
5. docker-compose.dev.yml: postgres:16 (port 5433, wolumen) + minio
   (konsola na 9001) do pracy lokalnej.
6. Skopiuj materiały projektowe do docs/design/ (manifest_ekranow_v7.csv,
   wszystkie *_spec.md, *_kontrakt-projektowy*.md, *_szkice*.md, makiety
   *.html, index.html). Dodaj docs/design/README.md z jednym akapitem: co
   tu leży i że GST/OEV/OPR nie mają makiet celowo.
7. GitHub Actions: workflow ci.yml uruchamiający lint, typecheck, test,
   build na push i PR.
8. README repo: jak postawić dev (pnpm i docker compose), struktura
   katalogów, odnośnik do docs/design/.
Nie dodawaj bibliotek ponad wymienione. Nie konfiguruj jeszcze bazy w
aplikacji - to A2.
```

Kryteria akceptacji: świeży klon + `pnpm install` + `docker compose -f docker-compose.dev.yml up -d` + `pnpm dev` daje działającą stronę startową; CI zielone na pierwszym pushu; `docs/design/` zawiera komplet materiałów; `pnpm build` przechodzi z output standalone.

Weryfikacja ręczna: otwórz repo w przeglądarce GitHuba i sprawdź, że makiety HTML w `docs/design/` otwierają się lokalnie w przeglądarce.

### Prompt A2 - schemat bazy danych

Cel: pełny schemat fazy 1 w Drizzle - również tabele używane dopiero w etapach D-G, żeby uniknąć bolesnych przeróbek w połowie drogi. Szczegółowa lista encji i pól jest w aneksie 1 - prompt odsyła do niego wprost.

Pliki kontekstu: aneks 1 tego promptbooka (wklej go pod promptem), `docs/design/manifest_ekranow_v7.csv`.

Prompt do wklejenia:

```text
Zaimplementuj schemat bazy w apps/web/src/db/schema/ (Drizzle, Postgres)
dokładnie według załączonej listy encji (aneks 1). Wymagania:

1. Podział na pliki tematyczne: identity.ts (users, sessions, accounts,
   verification_tokens), organizers.ts, events.ts, registrations.ts,
   payments.ts, addons.ts, consents.ts, waitlist.ts, wallet.ts, series.ts,
   volunteers.ts, results.ts, api.ts (api_keys, webhook_endpoints,
   webhook_deliveries), platform.ts (fee_schedules, platform_invoices,
   audit_log, jobs, email_log).
2. Klucze główne: uuid v7 generowane w aplikacji (funkcja w
   packages/shared). Timestampy created_at/updated_at wszędzie, timestamptz.
3. Kwoty: integer w groszach, kolumna currency char(3) z default 'PLN'.
4. Enumy Postgresa dla statusów wymienionych w aneksie 1 (m.in.
   registration_status, payment_status, event_status, addon_order_status,
   waitlist_status, job_status).
5. Indeksy pod realne zapytania: registrations po (event_id, status),
   (distance_id, status), unikalny bib w ramach event_id (partial index
   gdzie bib_number is not null), waitlist po (distance_id, position),
   holdy po expires_at, jobs po (status, run_at), audit_log po
   (organizer_id, created_at).
6. Relacje drizzle relations() dla najczęstszych złączeń.
7. drizzle-kit: konfiguracja + pierwsza migracja wygenerowana i
   commitowana. Skrypt pnpm db:migrate i pnpm db:seed.
8. Seed: operator platformy, organizator demo "Stołeczny Klub Biegacza"
   (slug skb) z brandingiem, drugi organizator "Górski Klub Trailowy"
   (slug gkt), wydarzenie demo z trzema dystansami (5 km, 10 km,
   półmaraton), progi cenowe (early/regular/late), kategorie wiekowe
   M/K 18-29/30-39/40+, definicja formularza zapisu, dokumenty zgód
   (regulamin, polityka prywatności, zgoda wizerunkowa), 30 sztucznych
   zgłoszeń w różnych statusach.
Modelujesz też tabele etapów D-G (portfel, gadżety, serie, wolontariusze,
api, wyniki) - mają istnieć od razu, nawet jeśli aplikacja ich jeszcze nie
używa. Nie pisz logiki domenowej - wyłącznie schemat, migracja, seed.
```

Kryteria akceptacji: `pnpm db:migrate` na czystym kontenerze przechodzi; `pnpm db:seed` odtwarza świat demo; typy z Drizzle eksportowane do `packages/shared`; partial unique index na bib działa (test wstawienia duplikatu).

Weryfikacja ręczna: przejrzyj schemat w narzędziu typu drizzle studio, sprawdź, że zgłoszenia seedowe mają sensowne statusy i kwoty.

### Prompt A3 - multi-tenancy i subdomeny

Cel: żądanie na `skb.{DOMENA}` trafia do kontekstu organizatora `skb`; ta sama aplikacja obsługuje panel platformy na domenie głównej.

Prompt do wklejenia:

```text
Zaimplementuj tenancy po subdomenie zgodnie z M0.

1. middleware.ts: rozpoznanie hosta; {slug}.{DOMENA} przepisuje na
   /o/[organizerSlug]/... zachowując ścieżkę; domena główna obsługuje
   katalog wydarzeń i panele. Obsłuż dev: {slug}.lvh.me:3000 oraz
   nagłówek x-forwarded-host (za proxy po migracji). Zarezerwuj listę
   subdomen systemowych (www, api, app, admin, static, mail).
2. src/lib/tenant.ts: getOrganizerFromHost() z cache na czas requestu;
   404 dla nieistniejącego sluga; organizator w statusie suspended
   dostaje stronę "zapisy wstrzymane".
3. Layout /o/[organizerSlug]: wstrzykuje tokeny CSS brandingu organizatora
   (--brand-ink, --accent, logo, obrazek nagłówka) z danych z bazy jako
   inline style na <html> - wzorzec tokenów z
   docs/design/ATH_kontrakt-projektowy_hifi.md.
4. Strona placeholder organizatora (lista opublikowanych wydarzeń -
   na razie surowa, ładny landing powstaje w B2).
5. Testy: jednostkowe parsowania hosta (z portem, z www, host spoza
   domeny, subdomena systemowa) i integracyjny happy path.
Nie buduj jeszcze custom domen organizatorów - odnotuj TODO w kodzie,
wróci przy G1.
```

Kryteria akceptacji: `skb.lvh.me:3000` pokazuje stronę SKB z jego kolorami, `gkt.lvh.me:3000` - GKT, `zla.lvh.me:3000` - 404; domena główna działa niezależnie; testy parsera hosta zielone.

Weryfikacja ręczna: podmień kolor akcentu SKB w bazie i odśwież - branding ma się zmienić bez deployu.

### Prompt A4 - uwierzytelnianie i role

Cel: logowanie magic linkiem i Googlem, sesje w bazie, role wielopoziomowe; konto jest od razu gotowe pod przyszłe konto rodzinne (faza 2), ale nie buduje jego interfejsu.

Pliki kontekstu: `docs/design/ATH_spec.md` sekcja ATH-01.

Prompt do wklejenia:

```text
Zaimplementuj auth zgodnie z M0 i zachowaniem ekranu ATH-01 ze
specyfikacji.

1. Auth.js z adapterem Drizzle: provider e-mail (magic link przez
   EmailProvider - na razie zaloguj link do konsoli, realna wysyłka
   wchodzi w A5) i Google OAuth. Sesje database, cookie httpOnly,
   secure na produkcji, SameSite=Lax działające między subdomenami
   ({DOMENA} jako cookie domain).
2. Model ról: users.platform_role (user | operator) oraz tabela
   organizer_members (organizer_id, user_id, role: owner | admin |
   staff). Helpery requireUser(), requireOrganizerRole(slug, role),
   requireOperator() do użycia w trasach i server actions.
3. Ekran logowania wspólny (na subdomenie brandowany tokenami
   organizatora): pole e-mail, przycisk magic link, przycisk Google,
   przełącznik języka. Komunikaty błędów według walidacji z ATH-01
   (zły format, wygasły link, zbyt wiele prób - rate limit 5 prób na
   15 minut na adres+IP, licznik w Postgresie).
4. Przepływ gościa: funkcja domenowa claimGuestRegistration() -
   zgłoszenie utworzone bez konta zostaje przypisane po pierwszym
   logowaniu na ten sam e-mail (użyjemy w B5; tu sama funkcja + test).
5. Wylogowanie, strona konta z minimalnymi danymi (imię, nazwisko,
   e-mail, język) i zapisem locale.
6. Testy: happy path magic link, wygasły token, rate limit, RBAC
   helperów (403 dla staff tam, gdzie trzeba owner).
Nie implementuj biometrii ani zapamiętywania urządzenia z ATH-01 -
zapisz jako TODO. Nie buduj jeszcze paneli - tylko auth i strona konta.
```

Kryteria akceptacji: pełny cykl magic link działa na dev (link z konsoli); sesja przeżywa przejście między domeną główną a subdomeną; testy RBAC i rate limitu zielone.

Weryfikacja ręczna: zaloguj się na `skb.lvh.me`, przejdź na `gkt.lvh.me` - sesja ma być ta sama, a branding inny.

### Prompt A5 - storage plików i e-maile transakcyjne

Cel: dwa porty infrastrukturalne, przez które przechodzi wszystko w kolejnych etapach: pliki (logo, zdjęcia nagłówka, eksporty) i poczta.

Prompt do wklejenia:

```text
Zaimplementuj warstwę plików i poczty zgodnie z M0.

1. src/lib/storage.ts: klient S3 (AWS SDK v3) skonfigurowany przez env
   (endpoint, region, bucket, klucze) tak, żeby działał z MinIO (dev),
   R2/S3 (PoC) i MinIO (produkcja). Operacje: uploadPrivate,
   uploadPublic, getSignedUrl, delete. Klucze obiektów:
   {organizerId}/{kategoria}/{uuid}.{ext}.
2. Upload obrazów brandingu: server action z walidacją (typ, wymiary,
   rozmiar do 5 MB), przetwarzanie sharp - logo do 512 px, obraz
   nagłówka do 2000 px szerokości, zapis webp + fallback jpeg.
3. src/lib/email/: interfejs EmailProvider (send z szablonem i danymi),
   implementacje ResendProvider i SmtpProvider (nodemailer) wybierane
   env-em, DevProvider logujący do konsoli i zapisujący .eml do /tmp.
4. Szablony react-email w apps/web/src/emails/: baza z brandingiem
   organizatora (logo, kolory, stopka z danymi organizatora jako
   administratora danych) + szablony: magic link, potwierdzenie
   zgłoszenia (użyjemy w B6), potwierdzenie płatności.
5. Tabela email_log już istnieje (A2): każda wysyłka zapisuje adresata,
   szablon, status, message_id dostawcy; nieudane wysyłki trafiają do
   tabeli jobs do ponowienia (wykorzystamy mechanizm jobs z A2; tick
   crona podłączymy w C2).
6. Podepnij realną wysyłkę magic linków z A4 przez EmailProvider.
7. Testy: storage przeciwko MinIO z docker-compose (integracyjny),
   renderowanie szablonów (snapshot), fallback SMTP.
```

Kryteria akceptacji: upload logo z formularza ląduje w MinIO i wraca podpisanym URL-em; magic link przychodzi naprawdę (Resend w trybie testowym lub konsola na dev); `email_log` rośnie przy każdej wysyłce.

Weryfikacja ręczna: wgraj logo SKB, podmień je, sprawdź stare i nowe URL-e; obejrzyj szablon e-maila w podglądzie react-email.

### Prompt A6 - design system w kodzie

Cel: przełożenie kontraktów projektowych na komponenty - raz, zamiast osobno przy każdym ekranie. Panel zawodnika dziedziczy estetykę makiet ATH (ciemny "chronograf dnia startu"), panele robocze OEV/OPR dostają jasny wariant tych samych tokenów, a strony publiczne GST - wariant brandowany organizatorem.

Pliki kontekstu: `docs/design/ATH_kontrakt-projektowy_hifi.md`, `docs/design/VOL_kontrakt-projektowy_hifi.md`, dwie-trzy makiety ATH jako wzorzec (np. ATH-08, ATH-10).

Prompt do wklejenia:

```text
Zbuduj design system w apps/web na bazie kontraktów projektowych z
docs/design/.

1. Tokeny: przenieś zmienne :root z kontraktu ATH do globals.css jako
   trzy warstwy motywu: theme-athlete (ciemny, jak w kontrakcie),
   theme-console (jasny wariant dla paneli OEV/OPR - te same zmienne,
   jasne powierzchnie, zachowany akcent), theme-public (dla stron GST -
   neutralny jasny, nadpisywany tokenami organizatora z A3). Fonty:
   Saira Condensed + Barlow przez next/font z subsetem latin-ext.
2. Tailwind: zmapuj kolory/odstępy/promień/cień na zmienne CSS (żadnych
   twardych hexów w komponentach).
3. Komponenty bazowe w src/components/ui/: Button (warianty primary na
   akcencie, secondary, ghost, destructive; stan processing ze
   spinnerem), Input/Select/Checkbox/Radio z etykietą i błędem, Card,
   Badge statusu (mapa statusów zgłoszenia na kolory semantyczne),
   Dialog/modal, Toast, Tabs, SegmentedControl, Skeleton, EmptyState,
   Table z sortowaniem (dla paneli), StickyActionBar (dolny pasek akcji
   z makiet ATH), AppShell dla paneli (nagłówek z marką, nawigacja,
   przełącznik języka), PublicShell dla stron GST.
4. i18n: skonfiguruj next-intl (albo równoważny, uzasadnij) z plikami
   messages/pl-PL.json i messages/en-GB.json, fallback en-GB, negocjacja
   z locale konta; formatery kwot (grosze -> Intl.NumberFormat currency)
   i dat w packages/shared.
5. Dostępność jak w M0: cele dotykowe, focus-visible, kontrast AA,
   prefers-reduced-motion wyłącza animacje wejścia.
6. Storybook albo prosta strona /dev/ui (wybierz mniejszy koszt) z
   przeglądem wszystkich komponentów w trzech motywach.
7. Testy: snapshot komponentów krytycznych, test formatera kwot
   (1234 -> "12,34 zł" dla pl-PL).
Trzymaj się wizualnie makiet ATH (obejrzyj ATH-08 i ATH-10 w
docs/design/) - to wzorzec gęstości, typografii i tonu.
```

Kryteria akceptacji: `/dev/ui` pokazuje komplet komponentów w trzech motywach; zmiana `--accent` organizatora barwi komponenty bez zmian w kodzie; lighthouse a11y strony `/dev/ui` bez błędów kontrastu; formatery przechodzą testy.

Weryfikacja ręczna: porównaj `/dev/ui` w motywie athlete z makietą ATH-10 obok siebie - mają wyglądać jak jedna rodzina.

---
## 6. Etap B - ścieżka pieniądza

Etap B buduje jedno: kompletną drogę od wejścia gościa na landing wydarzenia do zaksięgowanej wpłaty, numeru startowego i wiersza na liście uczestników w panelu organizatora. Wszystko inne czeka. Po B8 platforma umie obsłużyć prawdziwy bieg w wąskim zakresie i to jest materiał na pilotaż etapu C.

### Prompt B1 - encje wydarzenia i kreator organizatora

Cel: organizator zakłada wydarzenie w kreatorze pięciu kroków - odchudzona wersja ekranów OEV-02..07 (pełny kreator ze specyfikacji, z gadżetami i zaawansowanym konstruktorem, rozrasta się w etapie D).

Pliki kontekstu: `docs/design/manifest_ekranow_v7.csv` wiersze OEV-01..10, aneks 1.

Prompt do wklejenia:

```text
Zaimplementuj kreator wydarzenia w panelu organizatora
(/o/[slug]/panel/wydarzenia/nowe, motyw theme-console, dostęp od roli
staff wzwyż, publikacja od admin).

Kreator w pięciu krokach z zapisem wersji roboczej po każdym kroku:
1. Dane podstawowe: nazwa, slug (z walidacją unikalności w ramach
   organizatora), miejscowość i adres startu, data i godzina, strefa
   czasowa (default Europe/Warsaw), opis (markdown, sanityzowany),
   limit łączny uczestników (opcjonalny).
2. Dystanse: lista dystansów (nazwa, dystans w metrach, godzina startu,
   limit miejsc, wiek minimalny/maksymalny). Kategorie: automatyczne
   przedziały wiekowe M/K konfigurowalne szablonem (np. co 10 lat) albo
   ręczna lista; kategoria przypisuje się później z daty urodzenia.
3. Ceny: progi cenowe per dystans (nazwa, kwota brutto, początek/koniec
   obowiązywania po dacie LUB po liczbie zgłoszeń, limit sztuk progu).
   Walidacja: progi nie mogą się nakładać w tym samym wymiarze; wykres
   podglądowy ceny w czasie.
4. Formularz zapisu: wybór pól z biblioteki standardowej (dane osobowe,
   data urodzenia, płeć, klub, telefon, kontakt alarmowy, rozmiar
   koszulki, miasto, narodowość) + własne pola (tekst, wybór, checkbox)
   + reguły warunkowe "pokaż pole, gdy" (prosty builder; silnik
   warunków w packages/shared, bo użyje go też widget w E3). Zapis jako
   form_definitions z wersją.
5. Zgody i publikacja: edytor dokumentów zgód (regulamin - wymagany,
   polityka prywatności - wymagana, zgoda wizerunkowa - dobrowolna,
   oświadczenie zdrowotne - przełącznik), każda zmiana treści tworzy
   nową wersję dokumentu; podsumowanie całości i przycisk Opublikuj.
   Publikacja waliduje kompletność (min. 1 dystans, min. 1 próg
   aktywny, regulamin i polityka istnieją) i zmienia status wydarzenia
   draft -> published.

Do tego: lista wydarzeń organizatora (draft/published/archived) z
akcjami edytuj/duplikuj/archiwizuj; edycja opublikowanego wydarzenia
dozwolona z ostrzeżeniem przy polach wrażliwych (ceny, limity).
Wszystkie mutacje przez server actions z zod i wpisem do audit_log.
Testy domenowe: walidacja progów, wersjonowanie formularza i zgód,
przejścia statusu wydarzenia.
```

Kryteria akceptacji: organizator seedowy przechodzi kreator end-to-end i publikuje wydarzenie; wersja robocza przeżywa wylogowanie; nieprawidłowe progi zatrzymane walidacją z czytelnym komunikatem; audit_log zapisuje publikację.

Weryfikacja ręczna: załóż testowy bieg z trzema dystansami i progiem early bird kończącym się jutro - sprawdź podgląd wykresu cen.

### Prompt B2 - landing wydarzenia i katalog (GST)

Cel: publiczna twarz platformy - ekrany GST-01, GST-02, GST-04, GST-05 i lekki katalog na domenie głównej. To jest to, co widzi biegacz z linku od organizatora, oraz domyślny "ładny landing w subdomenie" z założeń produktu.

Pliki kontekstu: manifest wiersze GST-01..05, `docs/design/ATH_kontrakt-projektowy_hifi.md` (tokeny), zrzut dowolnego landingu RunSignup jako odniesienie jakościowe (opcjonalnie).

Prompt do wklejenia:

```text
Zbuduj publiczne strony wydarzeń (motyw theme-public + tokeny
organizatora z A3, rendering serwerowy, cache z rewalidacją po zmianie
danych wydarzenia).

1. Strona organizatora {slug}.{DOMENA} (GST-01): obrazek nagłówka,
   logo, nazwa, opis, lista nadchodzących wydarzeń (karta: data, miejsce,
   dystanse, cena od, licznik wolnych miejsc), sekcja minionych wydarzeń.
2. Landing wydarzenia {slug}.{DOMENA}/[eventSlug] (GST-02): hero z
   obrazkiem, nazwą, datą, miejscem i licznikiem dni do startu; sekcje:
   dystanse z cenami aktualnego progu i informacją "cena rośnie od..."
   (GST-04), opis/program, mapa dojazdu (link, bez SDK map w MVP), FAQ
   organizatora, stopka z danymi organizatora. Wyraźne CTA "Zapisz się".
3. Ekran startu zapisu (GST-05): wybór dystansu z ceną i dostępnością,
   przejście do formularza zapisu (B5). Dystans wyprzedany pokazuje
   stan i zapowiedź listy rezerwowej (funkcja wchodzi w D2 - na razie
   komunikat).
4. Licznik miejsc: liczony z zgłoszeń paid + pending_payment + aktywnych
   holdów (funkcja domenowa z B4; do czasu B4 przygotuj interfejs i
   policz z paid).
5. SEO: metadane OG per wydarzenie, obrazek OG generowany w node
   (satori/resvg - bez @vercel/og edge), sitemap.xml per subdomena,
   dane strukturalne schema.org/SportsEvent.
6. Wydajność: obrazy przez next/image, LCP na landingu < 2,5 s na
   symulowanym 4G (sprawdź lighthouse).
7. Strony prawne (GST-08) jako layout: /regulamin, /prywatnosc,
   /cookies renderujące opublikowane wersje dokumentów organizatora
   (treści z B1; strony prawne platformy na domenie głównej dostaną
   treść w B8).
Realne treści demo po polsku dla SKB i GKT (bez lorem ipsum), zgodnie z
konwencją z kontraktów projektowych.
```

Kryteria akceptacji: `skb.lvh.me:3000/polmaraton-demo` wygląda jak produkt, nie jak szkielet - hero, ceny, licznik, CTA; lighthouse: performance i SEO > 90 na landingu; OG image renderuje się z nazwą i datą biegu; zmiana opisu wydarzenia w panelu pojawia się na landingu bez redeployu.

Weryfikacja ręczna: obejrzyj landing na telefonie (tryb responsywny), wyślij link w komunikatorze i sprawdź podgląd OG.

### Prompt B3 - silnik formularza zapisu

Cel: definicja formularza z kreatora (B1 krok 4) renderuje się gościowi jako formularz krokowy z polami warunkowymi - ekran ATH-03 w wersji MVP. Silnik jest współdzielony, bo w E3 użyje go widget.

Pliki kontekstu: `docs/design/ATH_spec.md` sekcja ATH-03, manifest wiersz OEV-08.

Prompt do wklejenia:

```text
Zaimplementuj silnik formularza zapisu.

1. packages/shared/form-engine: typy definicji (pola, typy pól, reguły
   warunkowe, walidacje), czysta funkcja evaluateVisibility(definition,
   answers), generator schematu zod z definicji, wersjonowanie (odpowiedzi
   zapisują form_version).
2. Renderer w apps/web: formularz krokowy (dane osobowe -> pola
   wydarzenia -> podsumowanie kroku) na komponentach z A6, mobile-first,
   wskaźnik postępu, pola warunkowe odsłaniane progresywnie (bez
   wyszarzania), walidacja w locie pól krytycznych (data urodzenia
   vs. limity wieku dystansu - komunikat z propozycją pasującego
   dystansu, jak w ATH-03), zapis wersji roboczej do localStorage NIE -
   zapis draftu do bazy powiązany z sesją/hold-em (zgodnie z M0 zakaz
   localStorage jako magazynu stanu).
3. Wstępne wypełnienie danymi konta zalogowanego zawodnika (imię,
   nazwisko, data urodzenia, klub z ostatniego zgłoszenia).
4. Automatyczne przypisanie kategorii z daty urodzenia i płci po
   regułach dystansu, z możliwością ręcznej korekty tam, gdzie
   organizator na to pozwolił.
5. Testy: silnik warunków (tabela przypadków), generator zod, przypisanie
   kategorii (granice wieku - rocznikowo vs. w dniu biegu; wybierz
   rocznik i odnotuj decyzję w kodzie, organizatorzy w PL tak liczą
   najczęściej).
Formularz jeszcze nie tworzy zgłoszenia w bazie - wpinamy go w przepływ
w B5. Zrób stronę /dev/form-preview renderującą definicję demo.
```

Kryteria akceptacji: definicja z seedu renderuje się z polami warunkowymi działającymi bez przeładowania; walidacja wieku zatrzymuje 15-latka na półmaratonie z czytelnym komunikatem; testy silnika zielone.

Weryfikacja ręczna: przeklikaj formularz na telefonie jedną ręką - kolejność pól i focus mają prowadzić same.

### Prompt B4 - progi cenowe, limity i rezerwacja miejsca

Cel: serce współbieżności zapisów - licznik miejsc, który nie kłamie przy szpicy tysiąca osób w minutę otwarcia, holdy z TTL i cena z progu potwierdzana przy płatności (mechanika z OEV-10 i reguł M0).

Prompt do wklejenia:

```text
Zaimplementuj logikę miejsc i cen w src/modules/capacity/.

1. Funkcja domenowa openRegistration(distanceId, sessionKey):
   transakcyjnie sprawdza dostępność (limit dystansu i limit progu minus
   zgłoszenia paid/pending_payment minus aktywne holdy), tworzy
   zgłoszenie draft z holdem (expires_at = now() + 20 min, konfigurowalne
   per wydarzenie) i przypina cenę aktywnego progu (price_tier_id +
   kwota zamrożona w zgłoszeniu). Użyj SELECT ... FOR UPDATE na wierszu
   licznika albo advisory lock per dystans - uzasadnij wybór i pokryj
   testem współbieżności (dwa równoległe wejścia na ostatnie miejsce:
   dokładnie jedno dostaje hold).
2. Wygasanie holdów: job w tabeli jobs uruchamiany tickiem crona
   (endpoint /api/cron/expire-holds z A2/M0): draft z minionym
   expires_at -> status expired, miejsce wraca do puli. Przedłużenie
   holdu raz o 10 min, gdy użytkownik jest na kroku płatności.
3. Zmiana progu w trakcie: przy przejściu do płatności porównaj cenę
   zamrożoną z aktualną; jeśli próg się zmienił, pokaż jawnie nową cenę
   i wymagaj potwierdzenia (reguła z M0), zaktualizuj zgłoszenie.
4. Licznik publiczny: getAvailability(distanceId) z krótkim cachem
   (10 s) dla landingu; wersja bez cache dla przepływu zapisu.
5. Testy: współbieżność (najważniejszy test etapu B - napisz go
   porządnie, z realnymi równoległymi transakcjami na testowym
   Postgresie), wygasanie, przywracanie miejsca, zamrożenie ceny,
   przejście progu po dacie i po liczbie sztuk.
```

Kryteria akceptacji: test współbieżności przechodzi powtarzalnie (uruchom 20 razy); po wygaśnięciu holdu licznik na landingu wraca w górę; cena w zgłoszeniu nie zmienia się cicho nigdy.

Weryfikacja ręczna: otwórz zapis w dwóch przeglądarkach na dystans z limitem 1 - druga ma dostać uczciwy komunikat.

### Prompt B5 - przepływ zapisu zawodnika

Cel: sklejenie B2+B3+B4 w pełny przepływ ekranów ATH-01..05: wejście z landingu, formularz, zgody, aż do progu płatności. Zapis gościa tworzy konto w trakcie.

Pliki kontekstu: `docs/design/ATH_spec.md` sekcje ATH-01..05, makiety `ATH-03_formularz-zapisu_hifi.html`, `ATH-05_zgody-regulaminy_hifi.html`.

Prompt do wklejenia:

```text
Zepnij przepływ zapisu (motyw theme-athlete, branding organizatora,
mobile-first, zgodnie ze specyfikacjami ATH-01..05 w docs/design/ -
przeczytaj je przed pracą).

1. Wejście z GST-05: wybór dystansu wywołuje openRegistration (B4);
   koszyk (dystans, cena, licznik ważności holdu) widoczny przez cały
   przepływ jako przypięte podsumowanie.
2. Krok konta: zalogowany przechodzi dalej od razu; gość podaje e-mail -
   jeśli konto istnieje, proponuj logowanie (bez ujawniania, że konto
   istnieje ponad potrzebę - komunikat neutralny); jeśli nie, kontynuuje
   jako gość, konto tworzy się przy finalizacji i wysyłamy magic link
   (claimGuestRegistration z A4).
3. Formularz (B3) zapisuje odpowiedzi do zgłoszenia draft na bieżąco.
4. Zgody (ATH-05): lista z podziałem na wymagane i dobrowolne, treści z
   wersjonowanych dokumentów B1, akceptacja zapisuje consent_acceptances
   (dokument, wersja, czas, IP); zgoda wizerunkowa jawnie dobrowolna;
   oświadczenie zdrowotne za osobnym wejściem, jeśli włączone.
5. Przejście do podsumowania płatności (ekran powstaje w B6 - na razie
   strona podsumowania z kwotą i przyciskiem-zaślepką).
6. Stany offline/błędów zgodnie ze specyfikacją (przynajmniej: utrata
   sieci nie gubi wpisanych danych; wygaśnięcie holdu w trakcie pokazuje
   uczciwy komunikat i ścieżkę powrotu).
7. Testy Playwright: gość kończy zapis do progu płatności; zalogowany
   z wstępnie wypełnionymi danymi; próba przejścia bez wymaganej zgody
   wskazuje brakującą zgodę i przewija do niej.
```

Kryteria akceptacji: pełny przepływ od landingu do podsumowania działa na telefonie; consent_acceptances zapisuje wersję dokumentu; e2e zielone; hold widoczny jako licznik czasu w koszyku.

Weryfikacja ręczna: zapisz się jako gość z nowym adresem, kliknij magic link z e-maila i sprawdź, że zgłoszenie jest na koncie.

### Prompt B6 - płatności Stripe Connect i numer startowy

Cel: pieniądz płynie - onboarding Connect organizatora, checkout z BLIK/kartą/P24, webhooki, idempotencja, nadanie numeru po zaksięgowaniu (reguła nienaruszalna z M0, ekrany ATH-06/07, mechanika OEV-14, konfiguracja OPR-06 w wersji minimalnej).

Pliki kontekstu: `docs/design/ATH_spec.md` sekcje ATH-06, ATH-07; dokumentacja Stripe destination charges (agent zna, ale wskaż wersję API w prompcie po sprawdzeniu aktualnej).

Prompt do wklejenia:

```text
Zaimplementuj płatności zgodnie z M0 (Stripe Connect, destination
charges) w src/lib/payments/ za interfejsem PaymentProvider (metody:
createCheckout, refund, getPaymentStatus, verifyWebhook; typy w
packages/shared).

1. Onboarding organizatora: sekcja w panelu (dojdzie do ustawień B7)
   "Płatności" z przyciskiem podłączenia konta Stripe (Connect
   onboarding hosted, typ konta Express; zapisz account_id i status
   weryfikacji; webhook account.updated aktualizuje status). Wydarzenia
   organizatora bez aktywnego konta Connect nie mogą przyjmować
   płatności (publikacja dozwolona, zapis zablokowany z komunikatem
   dla organizatora).
2. Checkout (ATH-06): podsumowanie z rozbiciem kwoty (wpisowe wg progu;
   miejsce na dodatki - etap D; prowizja platformy jeśli model
   participant_pays), metody: BLIK, karta, Przelewy24 (przez Stripe
   Payment Element w trybie embedded na naszej stronie). Kwota z
   application_fee_amount liczoną z fee_schedules organizatora (default
   platformy: aneks 2; na razie wartość z bazy per organizator,
   edytowalna tylko przez operatora ręcznie w bazie - panel wchodzi
   w G1). Licznik holdu widoczny; przedłużenie holdu z B4 przy wejściu
   na krok płatności.
3. Webhooki /api/webhooks/stripe: payment_intent.succeeded/failed,
   charge.refunded, account.updated. Weryfikacja podpisu, idempotencja
   po event.id (tabela webhook_events_processed), obsługa retry.
   Zaksięgowanie: registration pending_payment -> paid W TRANSAKCJI z
   nadaniem numeru startowego.
4. Numery startowe (OEV-14 minimum): pula per dystans (zakres od-do,
   konfigurowalna w kreatorze B1 z sensownym defaultem: kolejne liczby
   od 1 per wydarzenie), nadawanie sekwencyjne bez dziur i bez
   duplikatów pod współbieżnością (advisory lock; test równoległy jak
   w B4), zapis bib_assigned_at, generacja pickup_code (krótki kod +
   payload QR).
5. Potwierdzenie (ATH-07): strona sukcesu z numerem (lub uczciwym
   stanem przejściowym "wpłata zaksięgowana, numer za chwilę" gdy
   webhook jeszcze nie doszedł - polling statusu), kod QR odbioru
   pakietu, przycisk pobrania karty startu (strona do druku), e-mail
   potwierdzający przez szablony z A5.
6. Płatność nieudana/porzucona: powrót do ATH-06 z zachowanym wyborem,
   komunikat przyczyny po ludzku; hold obowiązuje do wygaśnięcia,
   zgłoszenie expired sprząta się jobem z B4.
7. Refund pojedynczy (na potrzeby B7): funkcja domenowa refund z
   application_fee refund proporcjonalnym, status refunded, wpis audytu.
8. Tryb testowy: całość działa na kluczach testowych Stripe + stripe
   CLI do webhooków lokalnie; docs/dev/payments.md z instrukcją
   uruchomienia.
9. Testy: jednostkowe naliczania prowizji (participant_pays vs
   organizer_pays vs split 50/50 - trzy modele z aneksu 2), idempotencja
   webhooka (dwukrotne dostarczenie -> jeden numer), współbieżne
   księgowania -> numery bez kolizji, e2e happy path na kartach
   testowych.
```

Kryteria akceptacji: pełny zapis z płatnością testową BLIK/kartą kończy się numerem i e-mailem; podwójne dostarczenie webhooka nie psuje niczego; refund cofa application_fee proporcjonalnie; numer nigdy przed wpłatą (grep po kodzie: jedyna ścieżka nadania w transakcji webhooka i pokrycie testem).

Weryfikacja ręczna: przejdź płatność kartą testową 4242..., potem odmową 4000...0002; obejrzyj payment w dashboardzie Stripe z rozbiciem prowizji.

### Prompt B7 - panel organizatora: uczestnicy i operacje MVP

Cel: minimalny panel roboczy, bez którego organizator nie przyjmie prawdziwego biegu: dashboard (OEV-01 w wersji skromnej), lista uczestników (OEV-12), karta uczestnika (OEV-13), eksporty (OEV-22), refund i ręczne operacje, ustawienia wydarzenia (OEV-28 minimum).

Pliki kontekstu: manifest wiersze OEV-01, OEV-12, OEV-13, OEV-22, OEV-28.

Prompt do wklejenia:

```text
Zbuduj panel organizatora w motywie theme-console (desktop-first, ale
używalny na telefonie).

1. Dashboard wydarzenia (OEV-01): liczba zgłoszeń wg statusu, przychód
   brutto (suma paid), wykres zapisów dziennie (ostatnie 30 dni,
   komponent z dataviz bez ciężkich bibliotek - prosty SVG), wolne
   miejsca per dystans, skróty do listy i eksportów, stan konta Stripe.
2. Lista uczestników (OEV-12): tabela server-side (paginacja, sortowanie,
   filtry: dystans, status, płeć, kategoria, fraza po nazwisku/e-mailu/
   numerze), kolumny konfigurowalne, odznaki statusów. Do 10 tys.
   wierszy ma być żwawo - zapytania z indeksów A2, bez ładowania
   wszystkiego do przeglądarki.
3. Karta uczestnika (OEV-13): dane zgłoszenia i odpowiedzi formularza
   (w wersji z chwili zapisu), historia płatności, zgody z wersjami,
   oś zdarzeń z audit_log. Operacje: popraw dane (z wpisem audytu),
   zmień dystans (przelicz cenę: różnica jako dopłata linkiem
   płatności albo zwrot - MVP: tylko gdy ceny równe, resztę odnotuj
   jako TODO etapu D), refund (funkcja z B6, z polem powodu), anuluj
   zgłoszenie, wyślij ponownie e-mail potwierdzenia, ręczne dodanie
   uczestnika (zapis za uczestnika: bez płatności online, status paid
   ze źródłem manual i notatką - np. zapis gotówkowy w biurze zawodów).
4. Eksporty (OEV-22): CSV i XLSX listy uczestników z aktualnymi
   filtrami (strumieniowo, nie w pamięci), gotowy szablon "lista
   startowa dla pomiaru czasu" (kolumny: bib, nazwisko, imię, płeć,
   data urodzenia, kategoria, dystans, klub, miejscowość) - ten sam
   format wystawi API w F1. Eksport z danymi osobowymi zapisuje wpis
   w audit_log (kto, co, kiedy, ile wierszy).
5. Ustawienia wydarzenia (OEV-28 minimum): edycja danych z kreatora,
   zamknięcie/otwarcie zapisów ręczne niezależnie od dat, tekst
   e-maila potwierdzającego (dopisek organizatora), podłączenie
   Stripe (sekcja z B6).
6. Testy: uprawnienia (staff widzi, admin operuje, obcy organizator
   dostaje 404), eksport CSV poprawnie koduje polskie znaki (UTF-8
   z BOM dla Excela), refund przez panel działa e2e na Stripe testowym.
```

Kryteria akceptacji: organizator obsługuje cały cykl bez dewelopera: widzi zapisy, poprawia literówkę w nazwisku, robi refund, eksportuje listę startową; eksport 5 tys. wierszy nie zabija procesu; każda operacja na danych osobowych zostawia ślad w audit_log.

Weryfikacja ręczna: wyeksportuj XLSX i otwórz w Excelu - polskie znaki i daty mają być poprawne; sprawdź kartę uczestnika po refundzie.

### Prompt B8 - RODO, strony prawne i zgodność konsumencka

Cel: domknięcie prawne MVP: strony prawne platformy (GST-08), cookies, rejestr czynności na danych, retencja, eksport danych na żądanie. Treści prawne przygotowuje prawnik - prompt buduje mechanikę i wstawia robocze szkice oznaczone jako DO WERYFIKACJI PRAWNEJ.

Prompt do wklejenia:

```text
Zaimplementuj warstwę zgodności.

1. Strony prawne platformy na domenie głównej (GST-08): regulamin
   serwisu, polityka prywatności, polityka cookies - treści robocze
   po polsku oznaczone w repo jako wymagające weryfikacji prawnej,
   wersjonowane tym samym mechanizmem co dokumenty organizatorów (B1).
2. Baner cookies: tylko niezbędne domyślnie, zgoda na analityczne
   osobno, zapis decyzji bez localStorage (cookie techniczne),
   respektowany przez ewentualne skrypty analityczne (w MVP brak
   analityki zewnętrznej - przygotuj hak).
3. Rejestr czynności przetwarzania w praktyce: audit_log obejmuje już
   operacje na danych (B7) - dodaj widok operatora TODO(G1) i upewnij
   się, że logowane są: eksporty, odczyty kart uczestników przez
   organizatora, zmiany danych, refundy.
4. Prawa podmiotu danych: w ustawieniach konta zawodnika (rozszerz
   stronę z A4): pobierz moje dane (JSON + czytelny HTML: konto,
   zgłoszenia, zgody, płatności), usuń konto (soft delete z
   anonimizacją zgłoszeń: dane osobowe zastąpione, agregaty i
   rozliczenia zostają; blokada gdy aktywne zgłoszenie na przyszły
   bieg - komunikat z wyjaśnieniem).
5. Retencja: zadanie cron szkic (wyłączone flagą) anonimizujące dane
   uczestników po N latach od wydarzenia (N per organizator, default
   5) - sama mechanika + test, włączenie po decyzji prawnej.
6. Role przetwarzania w stopkach i politykach: organizator jako
   administrator danych uczestników swojego biegu, platforma jako
   podmiot przetwarzający oraz administrator danych kont - odzwierciedl
   to w szablonach e-maili (stopka z A5) i politykach roboczych.
7. docs/legal/README.md: lista dokumentów do zamówienia u prawnika
   (regulamin platformy, umowa powierzenia dla organizatorów, wzór
   regulaminu wydarzenia, polityka prywatności, zasady zwrotów przy
   usłudze terminowej) z krótkim opisem kontekstu każdego.
```

Kryteria akceptacji: pobranie własnych danych zwraca komplet; usunięcie konta anonimizuje zgłoszenia historyczne i nie psuje list startowych (bib zostaje, dane osobowe znikają); baner cookies nie używa localStorage; strony prawne wersjonowane.

Weryfikacja ręczna: usuń konto testowe z historycznym zgłoszeniem i obejrzyj listę uczestników w panelu - wiersz ma być zanonimizowany, licznik się zgadzać.

---
## 7. Etap C - twardnienie i pilotaż

Krótki etap o nieproporcjonalnym znaczeniu: między "działa u mnie" a "przyjęło prawdziwe pieniądze pięciuset osób w godzinę". Pilotaż robisz na jednym zaprzyjaźnionym biegu, z ręcznym nadzorem i planem odwrotu.

### Prompt C1 - testy krytyczne i test szpicy

Prompt do wklejenia:

```text
Uzupełnij siatkę testów przed pilotażem.

1. Playwright - komplet ścieżek krytycznych jako osobny projekt e2e
   uruchamiany w CI na zbudowanej aplikacji z testowym Stripe:
   zapis gościa z płatnością do numeru; zapis zalogowanego; odmowa
   płatności i powrót; wygaśnięcie holdu; refund z panelu; eksport CSV;
   zapis na ostatnie miejsce przy dwóch równoległych sesjach.
2. Test szpicy k6 (docs/dev/load-test.md + skrypt): scenariusz otwarcia
   zapisów - 300 wirtualnych użytkowników w 60 s na openRegistration +
   odczyt landingu; próg zaliczenia: p95 < 800 ms na API zapisu przy
   liczniku miejsc bez przekłamań (sprawdź sumy po teście zapytaniem
   kontrolnym). Uruchamiany ręcznie na stagingu, nie w CI.
3. Testy webhooków: symulacja opóźnionego webhooka (numer po odświeżeniu
   strony sukcesu), zdublowanego i osieroconego (payment bez zgłoszenia
   - alarm, nie wyjątek).
4. Chaos drobny: wyłącz Postgres na 10 s pod ruchem - aplikacja ma
   wrócić bez restartu; wpisz wynik do docs/dev/resilience.md.
```

Kryteria akceptacji: CI z e2e zielone i powtarzalne (trzy przebiegi bez flake); raport k6 w repo z wynikiem spełniającym próg; suma miejsc po teście szpicy zgadza się co do sztuki.

### Prompt C2 - obserwowalność, backupy, staging

Prompt do wklejenia:

```text
Przygotuj eksploatację.

1. Sentry (błędy + trace podstawowy) w apps/web, z tagiem organizatora
   i wydarzenia; PII maskowane. Alternatywnie self-hostowalny
   GlitchTip - wybierz i uzasadnij (pamiętaj o migracji na własny
   serwer).
2. Logi strukturalne (pino) z request id; log płatności i webhooków
   osobnym loggerem; bez danych osobowych w logach.
3. Health: /api/health (baza, storage, kolejka jobs - głębokość i wiek
   najstarszego joba), monitoring zewnętrzny (better uptime / uptime
   kuma - wybór z myślą o self-host) na health i na landing demo.
4. Backupy: na PoC Neon PITR - opisz w docs/ops/backup.md; do tego
   nocny pg_dump do S3 (job crona) jako backup niezależny od dostawcy,
   z testem odtworzenia opisanym krok po kroku (i wykonanym raz -
   zapisz datę wykonania w docs).
5. Staging: drugie środowisko na Vercel (gałąź staging) z osobną bazą
   i Stripe testowym; seed demo automatyczny.
6. Uruchom tick kolejki jobs (retry e-maili z A5, expire-holds z B4,
   szkic retencji z B8) przez Vercel Cron co minutę na /api/cron/tick
   z sekretem; dokumentacja w docs/ops/cron.md wraz z odpowiednikiem
   crontab dla self-host.
```

Kryteria akceptacji: wymuszony błąd testowy widoczny w Sentry ze zdrowym maskowaniem; `docs/ops/backup.md` zawiera datę udanego odtworzenia; staging dostępny pod osobną domeną z wildcardem.

### Prompt C3 - checklista pilotażu i runbook

Prompt do wklejenia:

```text
Przygotuj pilotaż na żywym wydarzeniu.

1. docs/ops/pilot-checklist.md: konfiguracja organizatora pilotażowego
   (Connect zweryfikowany, przelew testowy 1 zł wykonany i zwrócony),
   dry-run pełnego zapisu na produkcji za 1 zł, plan komunikacji
   z organizatorem (kto odbiera telefon w dniu otwarcia zapisów),
   limity Stripe sprawdzone, monitoring włączony, backup wykonany
   ręcznie przed otwarciem.
2. docs/ops/runbook.md: co robić gdy - płatności przechodzą a webhooki
   nie (ręczne przetworzenie z dashboardu + skrypt pnpm ops:reprocess),
   podwójne obciążenie zgłoszone przez uczestnika, baza niedostępna,
   trzeba awaryjnie zamknąć zapisy (przełącznik z B7), trzeba oddać
   wszystkim pieniądze (skrypt zbiorczego refundu z potwierdzeniem).
3. Skrypty operacyjne w apps/web/scripts/: reprocess-webhook,
   close-registrations, bulk-refund (z trybem dry-run domyślnie).
4. Strona statusu awaryjnego: statyczna informacja "trwa przerwa
   techniczna" możliwa do włączenia env-em bez deployu logiki.
```

Kryteria akceptacji: obie checklisty przećwiczone na stagingu w całości; skrypty operacyjne mają tryb dry-run i test; osoba nietechniczna rozumie runbook (przeczytaj na głos - serio).

---

## 8. Etap D - samoobsługa i przewagi rynkowe

Po udanym pilotażu dokładamy to, co w specyfikacji jest sercem produktu (samoobsługa ATH-08 nazwana wprost "sercem grupy"), oraz funkcje odróżniające od prostych systemów zapisów. Kolejność promptów D1-D6 można przestawiać poza jednym wyjątkiem: D3 (portfel) przed D1 tylko wtedy, gdy chcesz zwroty do portfela od pierwszego dnia - w praktyce D1 z refundem na kartę wystarcza na start.

### Prompt D1 - samoobsługa zgłoszenia i reguły operacji

Cel: ekran ATH-08 z regułami organizatora z OEV-16: transfer do innej osoby, odroczenie na kolejną edycję, zmiana dystansu, rezygnacja ze zwrotem wg polityki, opłaty za operacje jako przychód organizatora.

Pliki kontekstu: `docs/design/ATH_spec.md` sekcja ATH-08 (przeczytaj w całości - stany i przypadki brzegowe są tam wprost), makieta `ATH-08_samoobsluga-zgloszenia_hifi.html`, manifest OEV-16.

Prompt do wklejenia:

```text
Zaimplementuj samoobsługę zgłoszenia.

1. Konfiguracja organizatora (rozszerzenie ustawień wydarzenia):
   per operacja (transfer / odroczenie / zmiana dystansu / rezygnacja):
   włączona?, okno czasowe (od-do względem daty biegu), opłata w zł
   (przychód organizatora, doliczana do płatności operacji), polityka
   zwrotu przy rezygnacji (procent malejący progami do daty biegu).
2. Panel zawodnika: dashboard ATH-02 (wersja pełna: kafelki startów,
   alerty pilności) i ekran ATH-08 z listą dostępnych operacji, opisem
   skutku i kosztu przed potwierdzeniem, zgodnie ze specyfikacją i
   makietą (motyw theme-athlete, mechanika stanów z kontraktu).
3. Transfer: podanie e-maila osoby docelowej -> zaproszenie (szablon
   e-mail) -> osoba docelowa przechodzi skrócony zapis (dane + zgody,
   bo oświadczeń nie składa się za kogoś - patrz spec) -> zgłoszenie
   przepisane, numer startowy zostaje przy zgłoszeniu, płatność
   opłaty operacyjnej przez checkout jak w B6. Stany pośrednie jawne
   (transfer_pending z TTL 72 h).
4. Odroczenie: przeniesienie na kolejną edycję (wydarzenie wskazane
   przez organizatora jako następca) ze statusem deferred i kuponem
   100% na przyszły zapis (mechanizm kuponów wewnętrznych - zaprojektuj
   minimalny, użyje go też portfel w D3).
5. Zmiana dystansu: dopłata różnicy przez checkout albo zwrot różnicy
   (refund częściowy z B6); przelicz kategorię i numer (pula numerów
   per dystans: nowy numer, stary wraca do puli).
6. Rezygnacja: wyliczenie kwoty zwrotu wg polityki, refund na metodę
   płatności, miejsce wraca do puli (i do listy rezerwowej po D2).
7. Wszystkie operacje jako funkcje domenowe z audit_log, idempotentne,
   z blokadą równoległych operacji na jednym zgłoszeniu (jedna
   operacja w toku na raz).
8. Testy: macierz reguł (okno zamknięte, opłata, polityka zwrotu),
   transfer e2e z dwoma kontami, współbieżna próba dwóch operacji.
```

Kryteria akceptacji: zawodnik wykonuje każdą z czterech operacji bez udziału organizatora, w granicach reguł; opłaty operacyjne trafiają do organizatora przez Connect z prowizją platformy wg cennika; zgłoszenie nigdy nie ląduje w stanie pośrednim bez wyjścia.

Weryfikacja ręczna: przejdź transfer między dwoma własnymi kontami na stagingu, w tym płatność opłaty 10 zł.

### Prompt D2 - listy rezerwowe

Cel: OEV-17 + oferta z odliczaniem z ATH-02/ATH-08: wyprzedany dystans zbiera chętnych, zwolnione miejsce uruchamia ofertę z oknem czasowym, kaskadowo w dół listy.

Prompt do wklejenia:

```text
Zaimplementuj listy rezerwowe.

1. Konfiguracja per dystans: lista włączona?, limit długości, okno
   ważności oferty (default 24 h, konfigurowalne), tryb kolejności
   (kolejność zgłoszeń - jedyny w tym etapie).
2. Zapis na listę z GST-05/B5 gdy brak miejsc: skrócony formularz
   (dane kontaktowe + dystans), pozycja widoczna dla zawodnika,
   rezygnacja z listy samoobsługowo.
3. Zwolnienie miejsca (rezygnacja D1, wygaśnięcie holdu B4, anulowanie
   B7, podniesienie limitu przez organizatora) -> job tworzy ofertę dla
   pierwszej osoby: e-mail + kafelek z odliczaniem na dashboardzie;
   przyjęcie oferty otwiera pełny zapis z holdem i ceną AKTUALNEGO
   progu (jawnie pokazane); wygaśnięcie oferty przesuwa ją kaskadowo.
   Uważaj na wyścig: miejsce zarezerwowane dla oferty nie może być
   sprzedane z ulicy (hold systemowy powiązany z ofertą).
4. Panel organizatora (OEV-17): podgląd listy, ręczne zaproszenie
   poza kolejnością (z audytem), statystyka konwersji ofert.
5. Testy: kaskada trzech ofert po kolei, wyścig oferta vs. zapis
   z ulicy, rezygnacja z listy w trakcie aktywnej oferty.
```

Kryteria akceptacji: wyprzedany dystans z listą działa w pętli: rezygnacja -> oferta -> przyjęcie -> paid, bez ręcznej roboty; miejsce nigdy nie jest sprzedane podwójnie; konwersja ofert widoczna w panelu.

### Prompt D3 - portfel i kredyty u organizatora

Cel: ATH-10/OEV-19 - nadpłaty i zwroty jako kredyt u organizatora (nie w platformie), użycie kredytu przy kolejnym zapisie, księga wpisów bez możliwości edycji.

Prompt do wklejenia:

```text
Zaimplementuj portfel zawodnika per organizator.

1. Ledger: wallet_accounts (user x organizer) + wallet_entries
   (append-only: credit/debit, źródło: refund_to_wallet, deferral,
   manual_adjustment przez organizatora z powodem, spend_on_checkout),
   saldo wyliczane, nigdy przechowywane bez pokrycia wpisami; test
   spójności sum.
2. Zwroty z D1 dostają opcję "zwrot do portfela" (szybciej, bez
   prowizji zwrotu) obok zwrotu na metodę płatności - wybór zawodnika,
   organizator może ograniczyć do portfela w polityce (jawnie
   komunikowane przy zapisie).
3. Checkout B6: przełącznik "pokryj z portfela" - częściowe pokrycie,
   reszta przez Stripe; kwota zero w całości z portfela = potwierdzenie
   bez bramki (przypadek ze spec ATH-06).
4. Ekran ATH-10 wg makiety: saldo per organizator, historia wpisów,
   wyjaśnienie czym jest kredyt (nie są to pieniądze wypłacalne na
   konto - zgodnie ze spec, wypłata tylko przez procedurę organizatora).
5. Rozliczenie Connect przy pokryciu z portfela: kwota z portfela nie
   przechodzi przez Stripe - odnotuj w danych rozliczeniowych
   organizatora (raport w G2 to pokaże) i przemyśl prowizję platformy
   od części portfelowej wg cennika (aneks 2: prowizja od wartości
   zgłoszenia, nie od transakcji Stripe - policz i zapisz w
   platform_usage do fakturowania w G2).
6. Testy: ledger (suma wpisów = saldo, brak ujemnych bez limitu
   debetowego), checkout mieszany, zero-checkout.
```

Kryteria akceptacji: zwrot do portfela i wydanie kredytu przy kolejnym zapisie działają e2e; ledger audytowalny (żadnych UPDATE na wpisach); przypadek "portfel pokrywa całość" omija bramkę i nadaje numer.

### Prompt D4 - gadżety: magazyn, dodatki w zapisie, dosprzedaż

Cel: OEV-11 (magazyn z wariantami i stanami), ATH-04 (dodatki w przepływie zapisu), ATH-09 (dosprzedaż po zapisie) - uczciwa sprzedaż bez dark patterns, zgodnie z tonem specyfikacji.

Pliki kontekstu: `docs/design/ATH_spec.md` sekcje ATH-04, ATH-09; makieta `ATH-09_dosprzedaz-gadzetow_hifi.html`.

Prompt do wklejenia:

```text
Zaimplementuj gadżety.

1. Magazyn (OEV-11): produkty per wydarzenie (nazwa, opis, zdjęcie,
   cena, limit na osobę, okno sprzedaży), warianty (rozmiar/kolor) ze
   stanem magazynowym i rezerwacją (reserved_qty przy dodaniu do
   koszyka z TTL jak holdy w B4 - użyj tej samej mechaniki), tabela
   rozmiarów (markdown organizatora), flaga "w cenie wpisowego"
   (wybór wariantu bez ceny).
2. ATH-04 w przepływie zapisu: karty produktów, realny stan ("zostały
   3 szt." tylko gdy naprawdę mało), sugestie jako podpowiedź nie
   preselekcja, suma rośnie jawnie; krok pomijalny.
3. ATH-09 dosprzedaż: po opłaceniu, w oknie sprzedaży - osobny checkout
   na dodatki (B6 przyjmuje koszyk pozycji, nie tylko wpisowe -
   zrefaktoruj checkout na orders/order_items, jeśli B6 tego nie
   zrobił), wymiana rozmiaru w granicach stanów magazynu bez dopłaty,
   anulowanie gadżetu wg reguł (zwrot do portfela D3).
4. Panel: stany, edycja, prosty raport sprzedaży, eksport zamówień
   do odbioru pakietów (kolumny: bib, nazwisko, pozycje).
5. Testy: rezerwacja wariantu z TTL, ostatnia sztuka przy dwóch
   koszykach, wymiana rozmiaru, zamówienie w cenie wpisowego.
```

Kryteria akceptacji: zapis z koszulką i dosprzedaż medalu po zapisie działają e2e; stan magazynu nie schodzi poniżej zera pod współbieżnością; brak jakiejkolwiek preselekcji płatnych dodatków (sprawdź w teście).

### Prompt D5 - komunikacja z uczestnikami

Cel: OEV-20 w wersji użytecznej: wiadomości do segmentów (dystans, status, kategoria), szablony, harmonogram wysyłki, bez ambicji marketing automation.

Prompt do wklejenia:

```text
Zaimplementuj komunikację organizatora.

1. Kompozytor wiadomości: segment (filtry jak na liście B7), temat,
   treść (markdown z podstawianiem pól: imię, numer, dystans, link do
   zgłoszenia), podgląd na próbce 3 odbiorców, wysyłka teraz albo
   zaplanowana (job), throttling przez kolejkę jobs (partie po 50/min
   - limity Resend sprawdź i ustaw env-em).
2. Wymagane stopki: nadawca-organizator, powód otrzymania, link do
   preferencji powiadomień (rozszerz ustawienia konta zawodnika:
   transakcyjne zawsze, organizacyjne per wydarzenie opt-out,
   marketingowe opt-in - trzy klasy, klasę wiadomości wybiera
   kompozytor i wysyłka respektuje preferencje).
3. Historia wysyłek z licznikami (dostarczone/odbite z webhooka
   dostawcy), podgląd wysłanej treści.
4. Automaty transakcyjne przeniesione na szablony zarządzalne:
   dopisek organizatora do potwierdzenia (z B7) + przypomnienie przed
   biegiem (T-3 dni, konfigurowalne, z kodem QR odbioru) jako job.
5. Testy: segmentacja (liczność segmentu = liczba jobów), respektowanie
   opt-out, podstawianie pól, throttling.
```

Kryteria akceptacji: wiadomość do "wszyscy paid na 10 km" dochodzi tylko do nich, z poprawnym podstawieniem imienia; opt-out organizacyjnych wyklucza z wysyłki niekrytycznej, ale nie z potwierdzenia płatności; przypomnienie T-3 wysyła się samo na stagingu.

### Prompt D6 - raporty, serie biegów, wolontariusze

Cel: trzy mniejsze pakiety domykające fazę przewag: raporty OEV-21, serie OEV-23 z klasyfikacją GST-07/ATH-12, wolontariusze VOL-01..07 z OEV-25. Można zlecić jako trzy osobne sesje - poniżej trzy bloki promptów.

Prompt do wklejenia (D6a raporty):

```text
Zaimplementuj raporty wydarzenia (OEV-21): sprzedaż dzienna i
skumulowana (wpisowe vs dodatki), struktura uczestników (płeć, kategorie,
kluby, miasta), konwersja progu cenowego, porównanie z poprzednią edycją
(gdy wydarzenie ma poprzednika), eksport każdego raportu do CSV.
Wykresy prostym SVG jak w B7, dane agregowane zapytaniami (bez ETL).
```

Prompt do wklejenia (D6b serie):

```text
Zaimplementuj serie biegów (OEV-23, GST-07, ATH-12): definicja serii
(wydarzenia członkowskie, zasady punktacji: miejsce -> punkty tabelą,
liczba najlepszych startów liczonych do klasyfikacji), klasyfikacja
generalna liczona z wyników (etap F; do czasu importu wyników seria
pokazuje uczestnictwo i frekwencję), strona publiczna klasyfikacji na
subdomenie organizatora + widok "moja pozycja" w panelu zawodnika
zgodnie ze spec ATH-12. Przelicznik punktów jako czysta funkcja z
testami tabelarycznymi.
```

Prompt do wklejenia (D6c wolontariusze):

```text
Zaimplementuj moduł wolontariuszy wg specyfikacji VOL_spec.md i makiet
VOL-01..07 (przeczytaj je - są kompletne): zaproszenia od organizatora
(OEV-25: zmiany/zadania, sloty), dołączenie z linku zaproszenia
(VOL-01, konto tworzy organizator - inna reguła niż zawodnik!),
dashboard zadań i zmian (VOL-02/03), odprawa ze skanem QR kodu odbioru
pakietu (VOL-04, kamera przez BarcodeDetector z fallbackiem wpisania
kodu, tryb offline: ostatnia lista lokalnie + kolejka potwierdzeń),
wydawanie pakietów (VOL-05: wyszukiwarka po numerze/nazwisku,
odnotowanie wydania, blokada gdy brak wymaganej zgody - przypadek ze
spec ATH-02), lista startowa z weryfikacją opłaty (VOL-06).
Uprawnienia wolontariusza ograniczone do wydarzenia i zadań, dane
uczestników w zakresie minimalnym do zadania.
```

Kryteria akceptacji (łącznie): raporty zgadzają się z eksportami co do wiersza; klasyfikacja serii przelicza się z tabeli punktów deterministycznie; wolontariusz skanuje QR z ATH-07 i wydaje pakiet w trybie samolotowym z synchronizacją po powrocie sieci.

Weryfikacja ręczna: zrób odprawę testową dwoma telefonami - jeden zawodnik, drugi wolontariusz.

---
## 9. Etap E - API publiczne i widget do osadzania

Wymóg założycielski produktu: organizator ma móc wpiąć zapisy we własną stronę, a nie tylko dostać landing w subdomenie. Robimy to porządnie - API najpierw, widget jako jego klient. Ekrany: OEV-29 (klucze i webhooki organizatora), częściowo OPR-10.

### Prompt E1 - publiczne API v1

Prompt do wklejenia:

```text
Zaimplementuj publiczne API REST /api/v1 z dokumentacją OpenAPI.

1. Uwierzytelnianie: klucze API organizatora (OEV-29): para
   publiczny/sekretny (sekretny hashowany argon2, pokazywany raz),
   scopes: read:events, read:registrations, write:registrations,
   read:results, webhooks. Panel: tworzenie, nazwa, ostatnie użycie,
   unieważnienie. Rate limit per klucz (bucket w Postgresie, 600/min
   default, nagłówki X-RateLimit-*).
2. Endpointy (wersjonowane, JSON, cursor pagination, błędy RFC 7807):
   GET /events (opublikowane organizatora), GET /events/{id} (dystanse,
   progi, dostępność), GET /events/{id}/registrations (dane wg scope,
   filtry jak w panelu, PII tylko z read:registrations),
   POST /events/{id}/registrations (utworzenie zgłoszenia w imieniu
   uczestnika: tworzy draft + hold i zwraca checkout_url do dokończenia
   płatności u nas - przepływ dla stron organizatorów bez własnej
   obsługi kart), GET /registrations/{id}, GET /events/{id}/start-list
   (format listy startowej z B7, dla partnera pomiaru czasu).
3. Idempotencja zapisu: nagłówek Idempotency-Key obowiązkowy dla POST.
4. OpenAPI 3.1 generowane ze schematów zod (jedna prawda), publikowane
   na /api/v1/openapi.json + strona dokumentacji (scalar/redoc) na
   {DOMENA}/dev z przykładami curl.
5. CORS: publiczne odczyty dozwolone z origin listy per klucz
   (konfigurowane w panelu), mutacje tylko server-to-server (sekretny
   klucz nie działa z przeglądarki - blokuj po nagłówku Origin).
6. Testy: autoryzacja scope po scope, rate limit, idempotencja POST,
   kontrakt (przykłady z dokumentacji uruchamiane jako testy).
```

Kryteria akceptacji: `curl` z kluczem demo przechodzi cały cykl: lista wydarzeń -> utworzenie zgłoszenia -> checkout_url -> po opłaceniu zgłoszenie widoczne w GET; dokumentacja na /dev czytelna dla zewnętrznego programisty bez naszej pomocy.

### Prompt E2 - webhooki wychodzące organizatora

Prompt do wklejenia:

```text
Zaimplementuj webhooki organizatora (OEV-29): endpointy per organizator
(url, sekret do podpisu HMAC-SHA256 w nagłówku, wybór zdarzeń),
zdarzenia: registration.created, registration.paid,
registration.cancelled, registration.transferred, event.published,
results.published (etap F). Dostarczanie przez kolejkę jobs: retry
z backoff (1 min / 10 min / 1 h / 6 h / 24 h), dead letter z alertem
w panelu, podgląd ostatnich 100 dostaw z payloadem i odpowiedzią,
przycisk "wyślij ponownie". Payload wersjonowany, bez pełnych danych
osobowych (id + link do API). Test: odbiornik testowy w repo +
symulacja 500 -> retry -> sukces.
```

Kryteria akceptacji: zdarzenie paid dociera do testowego odbiornika z poprawnym podpisem; wyłączony odbiornik przechodzi ścieżkę retry i ląduje w dead letter z widocznym alertem.

### Prompt E3 - widget zapisów do osadzania

Prompt do wklejenia:

```text
Zaimplementuj widget osadzalny w packages/embed.

1. Skrypt embed.js (vanilla TS, build vite, < 15 kB gz): organizator
   wkleja <script src="https://{DOMENA}/embed.js" data-event="..."
   data-mode="inline|button" data-locale="pl-PL"></script>. Skrypt
   renderuje iframe z przepływem zapisu (trasa /embed/event/{id}:
   ten sam przepływ B5/B6 w wariancie bez nagłówka platformy,
   brandowany tokenami organizatora), auto-wysokość przez postMessage,
   tryb button otwiera modal-overlay.
2. Bezpieczeństwo: frame-ancestors z listy domen organizatora
   (konfigurowana w panelu przy kluczu API), reszta tras platformy
   z frame-ancestors 'none'; cookies w iframe: przepływ gościa musi
   działać bez cookies third-party (stan po stronie serwera powiązany
   z tokenem w URL iframe - przetestuj w Safari).
3. Strona w panelu organizatora "Osadź na swojej stronie": generator
   snippetu, podgląd na żywo, instrukcje WordPress/Wix/własny HTML.
4. Strona demo w repo (docs/dev/embed-demo.html) udająca stronę klubu.
5. Testy Playwright: zapis przez widget na stronie demo end-to-end,
   w tym płatność testowa; snapshot rozmiaru bundla w CI (budżet
   15 kB gz - fail przy przekroczeniu).
```

Kryteria akceptacji: zapis z płatnością przechodzi w całości wewnątrz iframe na obcej domenie (test na deployu preview), także w Safari; bundle mieści się w budżecie; snippet z panelu działa po wklejeniu bez edycji.

Weryfikacja ręczna: osadź widget na dowolnym darmowym hostingu statycznym i przejdź zapis z telefonu.

---

## 10. Etap F - pomiar czasu

Pomiar robi zewnętrzny partner - platforma ma mu oddać listę startową i przyjąć wyniki, resztę widzą zawodnicy i landing. Kontrakt integracyjny projektujemy generycznie (adapter na partnera), bo partnerów będzie więcej niż jeden.

### Prompt F1 - kontrakt integracyjny pomiaru czasu

Prompt do wklejenia:

```text
Zaimplementuj integrację pomiaru czasu (OEV-24).

1. Konto partnera: operator (na razie ręcznie w bazie, panel w G1)
   tworzy partnera pomiaru; organizator w ustawieniach wydarzenia
   przypina partnera do wydarzenia -> generuje się klucz API partnera
   ograniczony do tego wydarzenia (scopes: read:start-list,
   write:results).
2. Start lista dla partnera: GET /api/v1/events/{id}/start-list w
   dwóch formatach (JSON i CSV zgodny z szablonem eksportu B7),
   z parametrem since do pobierania przyrostowego (zmiany po
   transferach D1!), oraz webhook start_list.updated.
3. Import wyników: POST /api/v1/events/{id}/results batch JSON
   (bib, czas brutto, czas netto, splity opcjonalne, status:
   finished/dnf/dns/dq), idempotentny po (event, bib, wersja),
   walidacja bibów spoza listy (raport odrzutów, nie cichy drop),
   status wyników: provisional -> official (osobne wywołanie
   "publikacja oficjalna" przez partnera albo organizatora w panelu).
   Fallback: ręczny import CSV w panelu organizatora z mapowaniem
   kolumn i podglądem błędów przed zatwierdzeniem.
4. Po publikacji: zdarzenie results.published (webhooki E2),
   przeliczenie klasyfikacji serii (D6b), e-mail do uczestników
   "Twój wynik" (klasa organizacyjna, respektuje preferencje D5).
5. docs/partners/timing-integration.md: dokument dla partnera po
   polsku i angielsku - endpointy, formaty, przykłady curl, kolejność
   operacji w dniu zawodów. To dokument, który wyślesz partnerowi
   przed pierwszą wspólną imprezą.
6. Testy: pobranie przyrostowe po transferze, idempotencja importu,
   odrzuty bibów, przejście provisional -> official.
```

Kryteria akceptacji: symulowany partner (skrypt w repo) pobiera listę, wrzuca wyniki 500 osób w partiach i publikuje - klasyfikacja serii i e-maile ruszają same; ręczny import CSV wychwytuje błędne biby z czytelnym raportem.

### Prompt F2 - wyniki dla zawodników i na landingu

Prompt do wklejenia:

```text
Zaimplementuj prezentację wyników: strona wyników wydarzenia na
subdomenie (tabela z filtrami dystans/kategoria/płeć, wyszukiwarka
po nazwisku i numerze, oznaczenie provisional/official, miejsca
open i w kategorii liczone przy publikacji, nie w locie), widok
"mój wynik" w panelu zawodnika (czasy, miejsca, link do klasyfikacji
serii ATH-12, udostępnianie - obrazek OG z czasem generowany jak
w B2), anonimizacja: wynik osoby, która usunęła konto/odmówiła
publikacji, pokazuje numer bez nazwiska zgodnie z polityką z B8.
Wydajność: strona wyników 5 tys. osób renderuje się < 1 s (paginacja
+ cache po publikacji oficjalnej).
```

Kryteria akceptacji: wyniki demo wyświetlają się z miejscami zgodnymi z czasami (test na danych kontrolnych); "mój wynik" pokazuje się zawodnikowi po publikacji; obrazek OG wyniku renderuje czas i dystans.

---

## 11. Etap G - panel operatora platformy

Do tej pory operator (czyli Ty) robił rzeczy ręcznie w bazie. Ten etap zamyka pętlę biznesową: onboarding organizatorów, cenniki prowizji, rozliczenia, nadzór. Ekrany OPR-01..12 w wersji dopasowanej do skali kilkudziesięciu organizatorów.

### Prompt G1 - organizatorzy, onboarding, cenniki

Prompt do wklejenia:

```text
Zbuduj panel operatora na app.{DOMENA} (dostęp: platform_role operator,
motyw theme-console).

1. Lista i karta organizatora (OPR-02/03): dane, status (active/
   suspended), wydarzenia, wolumen zapisów i obrotu (agregaty),
   status Connect, członkowie.
2. Onboarding (OPR-04): zaproszenie organizatora e-mailem, kreator
   pierwszego logowania (dane podmiotu, NIP, dane rozliczeniowe,
   slug subdomeny z walidacją, zgoda na regulamin platformy i umowę
   powierzenia - wersjonowane jak wszystko), checklist do startu
   (Connect podłączony, pierwsze wydarzenie, branding).
3. Cennik prowizji (OPR-05): fee_schedules per organizator: procent +
   kwota stała od zgłoszenia, minimum, model (participant_pays /
   organizer_pays / split50), indywidualne nadpisania per wydarzenie;
   zmiana działa od nowych transakcji, historia zmian. Default
   platformy konfigurowany globalnie (wartości: aneks 2).
4. Branding i domeny (OPR-07): rezerwacja subdomen, custom domena
   organizatora (CNAME + weryfikacja TXT; na Vercel przez API domen,
   po migracji przez Caddy on-demand TLS - zaimplementuj za
   interfejsem DomainProvider z dwiema implementacjami, wybór env-em).
5. Impersonacja (OPR-11): operator wchodzi w panel organizatora w trybie
   podglądu z banerem i pełnym audytem każdej akcji; akcje mutujące
   w impersonacji domyślnie zablokowane, odblokowanie per sesja z
   powodem zapisanym w audit_log.
6. Widok audytu (OPR-09): przegląd audit_log z filtrami, w tym wpisy
   RODO z B8.
```

Kryteria akceptacji: nowy organizator przechodzi od zaproszenia do opublikowanego wydarzenia bez dotykania bazy ręcznie; zmiana cennika obowiązuje tylko nowe transakcje; impersonacja zostawia kompletny ślad.

### Prompt G2 - rozliczenia i faktury prowizji

Prompt do wklejenia:

```text
Zaimplementuj rozliczenia platformy (OPR-12, OPR-06 w części
raportowej).

1. Księga przychodów platformy: application_fee z wpłat Stripe
   (webhooki application_fee.created / refunded -> tabela
   platform_revenue) + prowizje od części portfelowej (platform_usage
   z D3) + ewentualne opłaty ręczne.
2. Raport miesięczny per organizator: wolumen, liczba zgłoszeń,
   prowizje pobrane przez Stripe, prowizje do dofakturowania (portfel),
   eksport CSV; zamknięcie miesiąca zamraża raport.
3. Faktury: MVP bez integracji księgowej - generowanie danych do
   faktury (pozycje, kwoty, VAT 23% od prowizji) jako eksport dla
   księgowości + miejsce na wpięcie API (interfejs InvoiceProvider,
   implementacja stub; Fakturownia/wFirma jako TODO z komentarzem).
4. Uzgodnienie (reconciliation): job miesięczny porównujący sumy
   platform_revenue z raportem payout Stripe (API balance
   transactions), różnice na listę rozbieżności do ręcznego wyjaśnienia.
5. Dashboard operatora (OPR-02 rozszerzenie): MRR z prowizji, wykres
   wolumenu, top organizatorzy, zdarzenia wymagające uwagi (dead
   letter webhooków, rozbieżności uzgodnienia, konta Connect z
   problemami weryfikacji).
```

Kryteria akceptacji: po miesiącu testowych transakcji raport zgadza się z dashboardem Stripe co do grosza; zamknięty miesiąc jest niezmienialny; rozbieżność wykryta w teście (sztucznie wstrzyknięta) ląduje na liście.

---

## 12. Etap H - migracja na własny serwer

Jeśli zakazy przenośności z M0 były przestrzegane, ten etap jest krótki. Prompt H1 warto odpalić dużo wcześniej (choćby po etapie C) jako test szczelności architektury - wykryje każdy przemyt vendor lock-inu.

### Prompt H1 - dockeryzacja i parytet środowisk

Prompt do wklejenia:

```text
Przygotuj wdrożenie self-host.

1. Dockerfile apps/web: multi-stage, next build z output standalone,
   obraz produkcyjny node:22-slim, non-root, healthcheck.
2. docker-compose.prod.yml: app (repliki x2), postgres:16 z wolumenem
   i pg_dump w cronie sidecar, minio z init bucketów, caddy (reverse
   proxy, automatyczne TLS, wildcard *.{DOMENA} przez DNS challenge -
   dokumentacja dla Cloudflare i OVH), watchtower opcjonalnie
   (wyłączony default).
3. Crontab hosta (docs/ops/crontab.example): tick co minutę na
   /api/cron/tick z sekretem, backup nocny, odnowienie certyfikatów
   robi caddy sam.
4. Zmienne środowiskowe: .env.production.example z KAŻDĄ zmienną
   z src/env.ts i komentarzem; skrypt pnpm ops:check-env porównujący
   env z wymaganiami zoda bez uruchamiania aplikacji.
5. CI: build obrazu i smoke test compose w GitHub Actions (up,
   health, zapis testowy na SQLite? NIE - na kontenerowym Postgresie,
   pełny stack).
6. docs/ops/self-host.md: wymagania VPS (2 vCPU / 4 GB na start),
   instalacja od zera krok po kroku, aktualizacje (pull obrazu,
   migracje, restart z maintenance page z C3).
```

Kryteria akceptacji: `docker compose -f docker-compose.prod.yml up` na czystym VPS-ie (przetestuj na najtańszym dropletcie) daje działającą platformę z TLS i subdomenami; smoke test w CI zielony; żadnej zmiennej środowiskowej poza .env.

### Prompt H2 - runbook migracji produkcyjnej

Prompt do wklejenia:

```text
Napisz i przećwicz runbook migracji Vercel -> self-host
(docs/ops/migration-runbook.md):

1. Przygotowanie: VPS postawiony (H1), DNS TTL obniżone do 300 s dzień
   wcześniej, pełny backup, staging odtworzony z backupu produkcji
   i przetestowany (zapis + płatność testowa).
2. Okno migracji (wybierz noc bez otwarć zapisów): maintenance page
   na Vercel, finalny pg_dump -> restore na VPS, rsync/rclone bucketów
   S3 -> MinIO, weryfikacja sum (skrypt liczący rekordy i obiekty),
   podmiana DNS (A/AAAA + wildcard), webhooki Stripe przepięte na
   nową domenę (endpoint stały, jeśli domena bez zmian - tylko test),
   smoke test produkcji, zdjęcie maintenance.
3. Rollback: kryteria decyzji (co musi nie działać), powrót DNS,
   scenariusz rozjazdu danych (zapisy przyjęte na nowym środowisku
   przed rollbackiem - eksport i ręczne uzgodnienie).
4. Po migracji: monitoring z C2 przepięty, Vercel w trybie readonly
   przez 2 tygodnie, potem wyłączony.
Przećwicz całość na stagingu i wpisz do runbooka rzeczywiste czasy
każdego kroku.
```

Kryteria akceptacji: migracja próbna staging -> VPS wykonana w całości z czasami wpisanymi do runbooka; różnica sum rekordów zero; rollback próbny przećwiczony.

---
## 13. Etap I - rozszerzenie płatności: PayU Marketplace i Paynow

Stripe Connect jest właściwym wyborem na start (najlepsze narzędzia deweloperskie, tryb testowy, natychmiastowy onboarding), ale na polskim wolumenie jest drogi: do prowizji od płatności (BLIK 1,6% + 1 zł) dochodzi 9 zł miesięcznie za każde aktywne konto połączone organizatora oraz 0,25% + 1,35 zł od każdej wypłaty. Obie krajowe bramki mają dziś pełnoprawne produkty marketplace, które utrzymują tę samą konstrukcję prawną (środki przechodzą przez licencjonowaną instytucję płatniczą, nie przez rachunki platformy) przy koszcie niższym o połowę lub więcej: Paynow rozlicza transfery przez tablicę `transfers[]` z prowizją platformy potrącaną polem `feeAmount` i boardingiem sprzedawców po NIP z weryfikacją tożsamości i rachunku po stronie mElements; PayU Marketplace dzieli koszyk między subsprzedawców z weryfikacją AML po stronie PayU. Cenniki standardowe (Paynow 0,95% bez opłaty stałej, PayU 1,1% + 0,30-0,32 zł) są punktem odniesienia - warunki marketplace w obu przypadkach negocjuje się indywidualnie i to jest przedkroczek biznesowy tego etapu, nie techniczny.

Kolejność I2/I3 ustaw według tego, która umowa będzie pierwsza na stole - adaptery są niezależne. Uwaga praktyczna do rozmów handlowych: boarding wymaga NIP organizatora (kluby i fundacje go mają; osoba fizyczna bez działalności nie przejdzie - to samo ograniczenie ma zresztą Stripe Connect na podmiot gospodarczy).

### Prompt I1 - fundament wielu dostawców płatności

Cel: uogólnienie warstwy z B6 tak, żeby dostawca był cechą transakcji, nie założeniem globalnym - bez zmiany zachowania działającego Stripe.

Prompt do wklejenia:

```text
Uogólnij warstwę płatności do wielu dostawców (bez zmiany zachowania
produkcyjnego Stripe - to refaktoryzacja z rozszerzeniem).

1. Tabela provider_accounts: organizer_id, provider (stripe|payu|paynow),
   status onboardingu (none|pending|active|restricted), zewnętrzne
   identyfikatory (stripe_account_id / payu submerchant id / paynow
   sellerId), config jsonb (szyfrowane sekrety per organizator, jeśli
   dostawca ich wymaga), timestamps weryfikacji. Przenieś dane Stripe
   z organizers do tej tabeli (migracja z backfill).
2. Rejestr dostawców: PaymentProvider rozszerzony o onboardSeller,
   getSellerStatus, capability map metod płatności (blik|card|p24|
   transfer) i walut; implementacja StripeProvider przeniesiona do
   rejestru bez zmian funkcjonalnych.
3. Routing: aktywna bramka per organizator (default) z możliwością
   nadpisania per wydarzenie; checkout tworzy payment z polem provider
   i od tej chwili wszystkie operacje (refund, status, webhook) idą
   przez dostawcę zapisany w transakcji. Przełączenie aktywnej bramki
   nie dotyka transakcji otwartych.
4. Webhooki: /api/webhooks/{provider} z weryfikacją podpisu wg
   dostawcy, wspólna tabela idempotencji (provider, event_id).
5. UI: sekcja Płatności w panelu organizatora pokazuje bramki, status
   onboardingu każdej i przełącznik aktywnej ze strażnikiem (ostrzeżenie
   przy otwartych zapisach); checkout pokazuje metody płatności z
   capability map aktywnego dostawcy.
6. Rozliczenia: platform_revenue.source rozszerzone o prowizje payu
   i paynow; uzgodnienie G2 za interfejsem ReconciliationSource
   (implementacja Stripe istnieje, payu/paynow dojdą w I2/I3).
7. Testy: routing per transakcja, capability map, pełna regresja e2e
   B6 na Stripe (nic nie miało prawa się zmienić).
```

Kryteria akceptacji: regresja Stripe zielona co do testu; transakcja rozpoczęta na Stripe kończy się na Stripe także po przełączeniu organizatora na inną bramkę; panel pokazuje trzy sloty dostawców z uczciwymi statusami.

### Prompt I2 - adapter PayU Marketplace

Prompt do wklejenia:

```text
Zaimplementuj PayuProvider w modelu PayU Marketplace (sandbox).

1. Boarding subsprzedawcy: inicjacja rejestracji organizatora (API lub
   formularz PayU wg dokumentacji marketplace), śledzenie statusu
   weryfikacji AML/KYC (polling + notyfikacje), blokada przyjmowania
   płatności do statusu pozytywnego - spójnie z zachowaniem B6 dla
   Stripe (zapisy zablokowane z komunikatem dla organizatora).
2. Płatności: REST API orders w wariancie marketplace - podział na
   część organizatora i prowizję platformy zgodnie z mechanizmem
   submerchant/shopping cart z dokumentacji; metody BLIK i karty
   (3DS); weryfikacja podpisu notyfikacji (nagłówek OpenPayu-Signature);
   mapowanie statusów na payments.status.
3. Refundy ze splitem: zwrot proporcjonalny z cofnięciem prowizji
   platformy; respektuj ograniczenia dokumentacji; idempotencja.
4. Wypłaty i salda: jeśli API udostępnia saldo subsprzedawcy, wystaw
   je do raportów G2; ReconciliationSource dla PayU (zestawienie
   prowizji pobranych vs platform_revenue).
5. Konfiguracja sandbox przez env (pos_id, klucze, oznaczenie testów
   integracyjnych wymagających sekretów - pomijane w publicznym CI),
   docs/dev/payments-payu.md z instrukcją uruchomienia.
6. E2e na sandboxie: boarding testowego organizatora -> zapis ->
   płatność BLIK testowa -> paid + numer -> refund częściowy.
```

Kryteria akceptacji: pełny cykl na sandboxie PayU przechodzi; prowizja platformy widoczna w platform_revenue i zgodna z fee_schedules; odrzucona płatność wraca czytelnym komunikatem jak w B6.

### Prompt I3 - adapter Paynow Marketplace

Prompt do wklejenia:

```text
Zaimplementuj PaynowProvider w modelu Marketplace API v3 (sandbox).

1. Seller boarding: POST /v3/boarding/merchants/sellers/init z NIP
   organizatora, obsługa asynchronicznych notyfikacji COMPLETED/
   CANCELED, zapis sellerId w provider_accounts, ekran statusu jak
   w I2.
2. Płatności: POST /v3/payments z transfers[] - grossAmount
   organizatora i feeAmount jako prowizja platformy (suma transfers
   musi równać się amount - walidacja po naszej stronie przed
   wywołaniem), Idempotency-Key na każdym wywołaniu, weryfikacja
   podpisu notyfikacji, mapowanie statusów.
3. Refundy: POST /v3/payments/{id}/refunds ze splitem; obsłuż minimum
   1 zł dla zwrotów kartowych (walidacja z czytelnym komunikatem
   i furtką: zwrot do portfela D3 zamiast zwrotu kartowego poniżej
   minimum).
4. Buyer Protection Program: wyłączony dla wpisowego (usługa
   terminowa, potwierdzenie natychmiastowe) - udokumentuj decyzję
   w kodzie; salda sellera z GET /v3/balances/sellers/{sellerId}
   do raportów G2 + ReconciliationSource dla Paynow.
5. Sandbox przez env, docs/dev/payments-paynow.md, testy jak w I2.
6. E2e na sandboxie: boarding -> zapis -> BLIK -> paid + numer ->
   refund; test walidacji sumy transfers i minimum zwrotu.
```

Kryteria akceptacji: pełny cykl na sandboxie Paynow przechodzi; suma transfers waliduje się przed wysyłką (test jednostkowy); zwrot 0,50 zł na kartę proponuje portfel zamiast twardego błędu.

---

## 14. Etap J - zdjęcia z biegu: moduł RunPixie

Platforma należy do RunPixie, więc zdjęcia z rozpoznawaniem zawodników to nie dodatek, tylko rodzinna przewaga wpisana w produkt: każdy bieg obsługiwany przez platformę może mieć galerię z automatycznym dopasowaniem zdjęć do numerów startowych. Kontrakt integracyjny definiujemy sami, po obu stronach jednej firmy - bez negocjacji, bez adaptera do cudzych ograniczeń.

Warunki handlowe modułu (ustalone, wpisane do konfiguracji, nie do kodu): w trybie darmowym dla biegaczy organizator płaci 1 zł od rozpoznanej osoby, niezależnie od liczby zdjęć tej osoby; w trybie sprzedaży prowizja RunPixie wynosi 33% ceny, a 67% trafia do organizatora. Platforma nie dokłada własnej marży: 1 zł/os przechodzi 1:1 w rozliczeniu miesięcznym, a sprzedaż zdjęć nie nalicza dodatkowo standardowej prowizji transakcyjnej platformy - 33% to już przychód grupy (w kodzie zostaje flaga konfiguracyjna na wypadek zmiany tej decyzji).

Reguły nienaruszalne (są też w M0): rozpoznawanie działa po numerze startowym, więc obejmuje wszystkie opłacone zgłoszenia - i wszystkie rozpoznane numery liczą się do rozliczenia trybu darmowego, bo to rozpoznanie jest elementem kosztowym. Organizator akceptuje to z góry, jednorazową deklaracją przy włączaniu trybu, i nie potwierdza już żadnej partii ani prognozy. Sprawą osobną jest ekspozycja: zgody decydują o tym, które numery da się znaleźć w wyszukiwarce i zobaczyć publicznie - nie zmieniają kosztu. Zależności: B5 (zgody), B6 (numery startowe), D4 (orders wielopozycyjne), G2 (rozliczenia miesięczne). Najlepszy moment: okolice etapu F.

### Prompt J1 - wgrywanie zdjęć przez organizatora i rozpoznawanie

Cel: organizator (albo jego fotograf) wrzuca zdjęcia z biegu jednym przeciągnięciem, pipeline robi resztę, a przed publikacją organizator świadomie akceptuje koszt trybu darmowego.

Pliki kontekstu: `docs/design/ATH_spec.md` sekcja ATH-11 (kontekst galerii), aneks 1 blok "Zdjęcia".

Prompt do wklejenia:

```text
Zaimplementuj moduł zdjęć po stronie organizatora (nowy ekran poza
manifestem v7, proponowane ID: OEV-30 - dopisz do docs/design/README.md).

1. Konfiguracja per wydarzenie (event_photo_settings): tryb galerii
   (free_for_runners | paid), przy paid cennik (cena pojedynczego
   zdjęcia i pakietu wszystkich zdjęć osoby - kwoty ustala organizator),
   włączenie publicznej wyszukiwarki po numerze (default wyłączone),
   retencja zdjęć w miesiącach (default 12, spina się z retencją B8).
   Włączenie trybu free_for_runners wymaga jednorazowej deklaracji
   kosztowej: organizator akceptuje, że płaci 1 zł za KAŻDĄ rozpoznaną
   osobę z numerem, niezależnie od zgód i liczby zdjęć (checkbox z
   pełną treścią; zapis kto, kiedy i którą wersję treści zaakceptował,
   tym samym mechanizmem co dokumenty B1). Bez deklaracji trybu
   darmowego nie da się włączyć; deklaracji nie powtarza się per partia.
2. Upload: drag&drop wielu plików (jpg/png/heic do 30 MB, do 2000
   zdjęć na partię), bezpośrednio do S3 podpisanymi URL-ami
   (multipart, wznowienie przerwanej partii), photo_batches ze
   statusami: uploading -> uploaded -> processing -> recognized ->
   published; joby: miniatury, strip EXIF GPS (czas wykonania
   zdjęcia zostaje - porządkuje galerię chronologicznie).
3. Kontrakt integracyjny z RunPixie (pierwszostronny - spisujemy go
   my): docs/partners/photos-runpixie.md ze specyfikacją obu stron:
   przekazanie partii (wspólny bucket + manifest JSON: klucze zdjęć,
   wydarzenie, dystanse, pełna mapa numerów startowych opłaconych
   zgłoszeń), odbiór rozpoznań webhookiem
   POST /api/webhooks/runpixie (photo_key, bib_number, confidence,
   bbox opcjonalnie; podpis HMAC, idempotencja po id zdarzenia),
   sygnał zakończenia partii. Adapter za interfejsem
   PhotoRecognitionProvider z implementacją runpixie i dev (fake
   rozpoznająca numer z nazwy pliku) do pracy lokalnej i testów.
4. Zgody a widoczność: rozpoznawane i mapowane są wszystkie numery -
   to element kosztowy usługi. Zgody sterują wyłącznie ekspozycją:
   numer osoby bez zgody image_public nie jest wyszukiwalny publicznie,
   a widoki publiczne go pomijają. Wycofanie zgody po publikacji:
   job ukrywa osobę z wyszukiwarki i widoków publicznych; mapowania
   i naliczenie zostają (koszt rozpoznania jest poniesiony), własna
   galeria osoby dalej działa.
5. Rozliczenie trybu darmowego: photo_usage_charges - 1 zł od każdej
   rozpoznanej osoby (unikalny numer z co najmniej jednym dopasowaniem,
   niezależnie od liczby zdjęć i od zgód), naliczane automatycznie po
   zakończeniu rozpoznawania partii (przejście w status recognized),
   pozycja w rozliczeniu miesięcznym G2 (platform_invoices),
   pass-through 1:1 bez marży platformy. Żadnych bramek potwierdzenia -
   podstawą naliczenia jest deklaracja z punktu 1. Panel pokazuje
   licznik informacyjnie: szacunek przed wysłaniem partii (liczba
   opłaconych numerów jako górna granica) i stan faktyczny po
   rozpoznaniu ("rozpoznano N osób = N zł").
6. Panel partii: statusy, liczba rozpoznań per dystans, zdjęcia bez
   dopasowania, ręczna korekta (dopnij/odepnij numer z audytem;
   korekta zmieniająca zbiór rozpoznanych osób tworzy wpisy
   korygujące w photo_usage_charges, w plus i w minus), przycisk
   publikacji galerii; publikacja wysyła e-mail "Twoje zdjęcia są
   gotowe" (klasa organizacyjna, respektuje preferencje D5).
7. Testy: włączenie trybu darmowego wymaga zapisanej deklaracji;
   naliczenie liczy osoby, nie zdjęcia, i obejmuje także osoby bez
   zgód; korekta mapowań koryguje naliczenie w obie strony;
   idempotencja webhooka rozpoznań; wycofanie zgody ukrywa z
   wyszukiwarki, ale nie zmienia naliczenia; wznowienie przerwanego
   uploadu.
```

Kryteria akceptacji: partia 500 zdjęć przechodzi cały pipeline na dev z fake providerem; tryb darmowy nie daje się włączyć bez zapisanej deklaracji kosztowej; naliczenie równa się liczbie unikalnych rozpoznanych numerów, niezależnie od zgód; wycofanie zgody znika osobę z wyszukiwarki w ciągu minut, a naliczenie pozostaje bez zmian.

Weryfikacja ręczna: wgraj 30 własnych zdjęć testowych z telefonu, w tym HEIC i jedno z GPS w EXIF - sprawdź miniatury, brak GPS po przetworzeniu i korektę błędnego dopasowania.

### Prompt J2 - galeria zawodnika i sprzedaż zdjęć

Cel: ekran ATH-11 ze specyfikacji - zawodnik dostaje swoje zdjęcia po numerze, darmo albo przez koszyk, zawsze w granicach zgody wizerunkowej.

Pliki kontekstu: `docs/design/ATH_spec.md` sekcja ATH-11, makieta `ATH-11_galeria-runpixie_hifi.html` (jest w docs/design/).

Prompt do wklejenia:

```text
Zaimplementuj galerię zawodnika (ATH-11 - przeczytaj specyfikację
i makietę w docs/design/, motyw theme-athlete).

1. Moje zdjęcia w panelu zawodnika: dostęp po opłaceniu startu (numer),
   niezależnie od zgód - własne zdjęcia to prywatne doręczenie osobie,
   której wizerunek dotyczy, nie publikacja; siatka miniatur z podglądem,
   stany: zapowiedź przed publikacją, pusta po publikacji (uczciwy
   komunikat + zgłoszenie "widzę siebie na nieoznaczonym zdjęciu" ->
   trafia do korekty w panelu organizatora J1), opublikowana.
2. Tryb free_for_runners: pobieranie oryginałów pojedynczo i całości
   jako zip (job + link czasowy), bez płatności.
3. Tryb paid: miniatury i podgląd z watermarkiem (nakładanym w jobie,
   oryginały prywatne w S3), koszyk zdjęć -> checkout przez orders
   (kind: photos) na aktywnej bramce organizatora; podział: 33%
   RunPixie (platform_revenue.source = runpixie_commission), 67%
   organizator; standardowa prowizja transakcyjna platformy NIE jest
   doliczana - flaga photo_platform_fee_enabled (default false)
   zostaje w konfiguracji. Po opłaceniu odblokowanie pobrań bez
   watermarku i faktura pozycji jak przy gadżetach.
4. Publiczna wyszukiwarka po numerze (tylko gdy organizator włączył
   w J1): pole numeru na stronie wydarzenia; wyniki wyłącznie dla
   osób, które udzieliły osobnej, dobrowolnej zgody na publiczną
   widoczność galerii (nowy typ dokumentu image_public w B1 - dodaj
   do kreatora zgód i ekranu ATH-05). Bez tej zgody zdjęcia widzi
   tylko zalogowany właściciel numeru.
5. Prawo konsumenckie: przed pobraniem kupionych plików checkbox
   zgody na utratę prawa odstąpienia od treści cyfrowej, z zapisem;
   sformułowania do weryfikacji prawnej - dopisz pozycję do
   docs/legal/README.md.
6. Testy: własna galeria bramkowana opłatą (nie zgodą), wyszukiwarka
   bramkowana zgodą image_public, watermark tylko w paid, split
   33/67 w platform_revenue, flaga prowizji, zip dużej galerii jako
   job.
```

Kryteria akceptacji: każdy opłacony numer widzi po zalogowaniu własną galerię, także bez zgód; publiczna wyszukiwarka nie zwraca osób bez zgody image_public; zakup w trybie paid odblokowuje oryginały, a rozliczenie pokazuje dokładnie 33/67; w trybie free pobranie działa natychmiast po publikacji.

Weryfikacja ręczna: przejdź oba tryby na stagingu dwoma kontami (ze zgodą i bez) i kup jedno zdjęcie płatnością testową.

---

## 15. Backlog świadomie poza fazą 1

Rzeczy z manifestu i specyfikacji, które nie mają promptu, bo czekają na swój moment - żeby było jasne, że to decyzja, nie przeoczenie:

- Galeria zdjęć w panelu rodzica (PAR-15): widok zdjęć dziecka wraca z całym modułem rodzinnym w fazie 2 - silnik galerii z etapu J jest wspólny, dojdzie tylko warstwa uprawnień opiekuna.
- Powiadomienia pogodowe (ATH-13, OEV-26) i wskaźnik ryzyka nieobecności (OEV-27): wymagają źródła danych pogodowych i danych historycznych - wróć po kilku sezonach danych.
- Push notifications: D5 obsługuje e-mail; web push jako rozszerzenie D5 po pilotażu.
- Aplikacje natywne: nie planujemy - PWA wystarcza (makiety zakładają telefon w przeglądarce).
- Custom domeny organizatorów: zaprojektowane w G1, włączenie po migracji H (na Vercel działa, ale sens ma dopiero na docelowej infrastrukturze).
- Sztafety (przypadek brzegowy w ATH-03): formularz wieloosobowy - po fazie 1, model orders/order_items z D4 jest na to gotowy.
- KID-01/02, PAR, OCL, INS: cała warstwa szkółek i kont rodzinnych to faza 2. Model konta (A4) i portfel (D3) są z nią zgodne - specyfikacja PAR zakłada wspólne konto i wspólny portfel, dlatego portfel jest per organizator, a nie per moduł.

## 16. Pierwszy tydzień - zanim wklejisz A1

Sprawy, których agent za Ciebie nie załatwi:

1. Domena produkcyjna (i decyzja o nazwie marki) - potrzebna od B2, wildcard DNS od A3 (na dev wystarczy lvh.me).
2. Konto Stripe z włączonym Connect (tryb testowy wystarczy do etapu D włącznie; aktywacja produkcyjna wymaga danych firmy - załóż działalność/spółkę, jeśli jeszcze nie istnieje, bo Connect na osobę prywatną nie ruszy).
3. Konta: GitHub (repo prywatne), Vercel (hobby wystarczy na dev, Pro przy pilotażu), Neon (baza PoC), Cloudflare (DNS + R2) albo AWS (S3), Resend (domena nadawcza zweryfikowana DKIM - zrób wcześnie, propagacja bywa powolna).
4. Rozmowa z kandydatem na partnera pomiaru czasu - pokaż mu docs/partners/timing-integration.md z etapu F zanim powstanie kod; jego uwagi są tańsze przed implementacją.
5. Zaprzyjaźniony organizator na pilotaż etapu C - najlepiej mały bieg (200-500 osób) z terminem za 3-4 miesiące.
6. Prawnik od dokumentów z docs/legal/README.md (B8) - zamów wcześnie, terminy u prawników bywają dłuższe niż sprinty.
7. Zapytania ofertowe o warunki marketplace do PayU i do mElements (Paynow) - wdrożenie czeka dopiero w etapie I, ale negocjacje, umowy i aktywacja trybu marketplace potrafią trwać tygodniami, więc uruchom je równolegle z pilotażem, nie po nim.
8. Uzgodnienie z zespołem RunPixie kontraktu integracyjnego zdjęć (specyfikacja z J1: wspólny bucket, manifest, webhook rozpoznań) - to rozmowa wewnątrz firmy, ale im wcześniej obie strony znają format, tym mniej przeróbek w etapie J.

---

## Aneks 1 - model danych: encje i pola

Lista dla promptu A2. Konwencje: wszędzie id (uuid v7), created_at, updated_at; kwoty integer w groszach + currency; nazwy tabel w liczbie mnogiej.

Tożsamość i dostęp:

- users: email (unikalny, citext), email_verified_at, name, phone, date_of_birth, gender, locale (default pl-PL), platform_role (user|operator), deleted_at (soft delete), anonymized_at.
- sessions, accounts, verification_tokens: wg adaptera Auth.js.
- organizer_members: organizer_id, user_id, role (owner|admin|staff), invited_by, accepted_at.

Organizatorzy i platforma:

- organizers: slug (unikalny, subdomena), name, legal_name, nip, contact_email, contact_phone, status (active|suspended), branding jsonb (ink, accent, logo_key, header_image_key), locale_default, stripe_account_id, stripe_status (none|pending|active|restricted), custom_domain, custom_domain_verified_at, data_retention_years (default 5).
- fee_schedules: organizer_id (null = default platformy), event_id (null = całość organizatora), percent_bps (punkty bazowe, np. 390), fixed_fee, min_fee, max_fee (nullable), payer_mode (participant|organizer|split50), valid_from, valid_to.
- platform_revenue: source (stripe_application_fee|wallet_usage|manual), organizer_id, event_id, payment_id, amount, occurred_at, stripe_ref.
- platform_invoices: organizer_id, period (YYYY-MM), status (open|closed|exported), totals jsonb, closed_at.
- audit_log: actor_user_id, acting_as (impersonation), organizer_id, event_id, action, entity_type, entity_id, before jsonb, after jsonb, ip, created_at.
- jobs: type, payload jsonb, status (pending|running|done|failed|dead), run_at, attempts, last_error, locked_until, locked_by.
- email_log: to_email, template, class (transactional|organizational|marketing), status, provider_message_id, registration_id, error.

Wydarzenia:

- events: organizer_id, slug (unikalny per organizator), name, status (draft|published|archived), starts_at, timezone, venue_name, venue_address, city, description_md, header_image_key, total_limit, registration_opens_at, registration_closes_at, registrations_closed_manually bool, hold_ttl_minutes (default 20), predecessor_event_id (poprzednia edycja), series_id, settings jsonb (teksty e-maili, FAQ, reguły operacji z D1: transfer/deferral/distance_change/cancellation - enabled, window, fee, refund_policy jsonb).
- distances: event_id, name, distance_m, starts_at, limit, min_age, max_age, bib_range_from, bib_range_to, sort_order.
- categories: distance_id, code, name, gender (M|F|X|null), age_from, age_to, auto_assign bool.
- price_tiers: distance_id, name, amount, currency, active_from, active_to, quantity_limit, sort_order.
- form_definitions: event_id, version, schema jsonb (pola, typy, warunki, walidacje), published_at.
- consent_documents: scope (platform|organizer|event), organizer_id, event_id, type (terms|privacy|image|health|custom), title, content_md, version, required bool, published_at.

Zgłoszenia i płatności:

- registrations: event_id, distance_id, user_id (nullable do czasu przejęcia przez gościa), guest_email, status (draft|pending_payment|paid|expired|cancelled|refunded|transferred|deferred), form_version, answers jsonb, first_name, last_name, date_of_birth, gender, club, city, phone, emergency_contact jsonb (osobno od answers - dane częste w eksportach), category_id, category_locked bool, price_tier_id, entry_fee_amount (zamrożona), platform_fee_amount, payer_mode (kopia z chwili zapisu), hold_expires_at, bib_number, bib_assigned_at, pickup_code, picked_up_at, picked_up_by (wolontariusz), source (online|manual|api|import), transferred_from_registration_id, transfer_state (null|pending|completed), deferred_to_event_id, notes_internal, anonymized_at.
- consent_acceptances: registration_id, user_id, document_id, document_version, accepted_at, ip.
- orders: organizer_id, event_id, user_id/guest_email, status (draft|pending|paid|cancelled|refunded), total_amount, wallet_amount_used, provider_amount, kind (registration|addons_upsell|operation_fee).
- order_items: order_id, type (entry_fee|addon|operation_fee|discount), registration_id, addon_variant_id, description, quantity, unit_amount, total_amount.
- payments: order_id, provider (stripe), provider_payment_intent_id, status (pending|succeeded|failed|refunded|partially_refunded), amount, application_fee_amount, method (blik|card|p24), paid_at, failure_reason.
- refunds: payment_id, amount, application_fee_refund_amount, reason, initiated_by, to_wallet bool, status, provider_refund_id.
- webhook_events_processed: provider, event_id (unikalny), processed_at.
- coupons: organizer_id, event_id (nullable), code, kind (percent|amount|full), value, source (deferral|manual), max_uses, used_count, valid_to, registration_id_origin.

Gadżety, portfel, listy rezerwowe:

- addons: event_id, name, description_md, image_key, price, included_in_entry bool, per_person_limit, sale_opens_at, sale_closes_at, size_chart_md, sort_order.
- addon_variants: addon_id, name (np. M, L), stock_qty, reserved_qty, sold_qty, sort_order.
- registration_addons: registration_id, order_item_id, addon_variant_id, status (reserved|paid|cancelled|fulfilled), reserved_until.
- wallet_accounts: user_id, organizer_id, unikalne razem.
- wallet_entries: wallet_account_id, direction (credit|debit), amount, source (refund|deferral|manual_adjustment|checkout_spend), reference_type/reference_id, note, created_by.
- waitlist_entries: distance_id, user_id/guest_email, contact jsonb, position, status (waiting|offered|accepted|expired|withdrawn), offered_at, offer_expires_at, offer_hold_registration_id.

Serie, wyniki, wolontariusze:

- series: organizer_id, name, slug, scoring jsonb (tabela punktów, liczba startów liczonych), status.
- series_events: series_id, event_id, sort_order.
- results: event_id, distance_id, bib_number, registration_id (dopięte po bibie), status (finished|dnf|dns|dq), gun_time_ms, chip_time_ms, splits jsonb, place_open, place_gender, place_category, version, state (provisional|official), imported_at, source (api|csv).
- volunteer_shifts: event_id, name, task_type (checkin|packet_pickup|route|other), starts_at, ends_at, capacity, instructions_md.
- volunteer_assignments: shift_id, user_id, status (invited|accepted|declined|checked_in), invited_by.

API i integracje:

- api_keys: organizer_id, name, public_id, secret_hash, scopes text[], event_id (nullable - klucze partnera pomiaru ograniczone do wydarzenia), allowed_origins text[], last_used_at, revoked_at.
- webhook_endpoints: organizer_id, url, secret, events text[], active bool.
- webhook_deliveries: endpoint_id, event_type, payload jsonb, attempt, status, response_code, response_body_snippet, next_retry_at, delivered_at.
- timing_partners: name, contact, status (operator zarządza; klucz przez api_keys z event_id).

Zdjęcia (etap J):

- event_photo_settings: event_id (unikalne), mode (free_for_runners|paid), price_single, price_all, public_search_enabled bool, retention_months (default 12), photo_platform_fee_enabled bool default false, free_mode_declared_by, free_mode_declared_at, free_mode_declaration_version (deklaracja kosztowa trybu darmowego - wymagana, zanim mode przyjmie free_for_runners).
- photo_batches: event_id, status (uploading|uploaded|processing|recognized|published), photo_count, recognized_people_count, published_at, cost_confirmed_by, cost_confirmed_at.
- photos: batch_id, event_id, storage_key, thumb_key, watermark_key, taken_at (z EXIF), width, height, status (ok|deleted).
- photo_recognitions: photo_id, bib_number, registration_id, confidence, source (runpixie|manual), corrected_by, unikalność (photo_id, registration_id).
- photo_usage_charges: event_id, batch_id, registration_id, amount (100 = 1 zł, wartości ujemne dla stornowań po korekcie mapowań), charged_at, platform_invoice_id - jedna pozycja na rozpoznaną osobę, naliczana po zakończeniu rozpoznania partii, niezależnie od zgód.
- zakupy zdjęć przez orders/order_items (kind: photos) + platform_revenue.source = runpixie_commission; osobna tabela nie jest potrzebna.

## Aneks 2 - konkurencja i propozycja oferty

Stan cenników na 17 lipca 2026, sprawdzony na stronach dostawców (pełne odesłania w bibliografii; cenniki bywają zmieniane, więc przed publikacją własnego cennika odśwież dane).

RunSignup nie pobiera abonamentu, a prowizję nalicza od koszyka, nie od osoby: 6% + 1 USD do 249,99 USD, 5% + 1 USD od 250 USD, 4% + 1 USD od 1000 USD; darowizny zawsze 4%; o tym, kto płaci prowizję (uczestnik, organizator albo po połowie), decyduje organizator; strona wydarzenia, e-maile, aplikacja RaceDay i platforma zdjęć są w cenie, a wydarzenia darmowe nie płacą nic (RunSignup, b.d.). Datasport liczy 6% brutto od płatności elektronicznych za pakiet zapisów z wynikami live, publikacją zdjęć i płatnościami; pomiar czasu wycenia osobno - 8 zł brutto za numer startowy z chipem i od 4400 zł brutto za usługę pomiaru podstawowej imprezy (Datasport, b.d.). ZapisyOnline pobiera 0,99 zł brutto od uczestnika wyłącznie przy wydarzeniach płatnych, przy czym koszty szybkich płatności nalicza osobno zewnętrzny operator (ZapisyOnline, b.d.).

Po stronie kosztów własnych: Stripe w Polsce liczy 1,5% + 1 zł za karty krajowe EEA, 1,6% + 1 zł za BLIK i 1,9% + 1 zł za Przelewy24; karty spoza EEA 3,25% + 1 zł (Stripe, b.d.-a). W modelu Connect, w którym to platforma ustala ceny, dochodzi 9 zł miesięcznie za każde aktywne konto połączone oraz 0,25% + 1,35 zł od każdej wypłaty do organizatora (Stripe, b.d.-b) - przy małych organizatorach z częstymi wypłatami to zauważalna pozycja.

Krajowe alternatywy na etap I wypadają wyraźnie taniej. Paynow (bramka mBanku, operacyjnie mElements) liczy w ofercie standardowej 0,95% od transakcji bez opłaty stałej, bez opłaty za rejestrację i bez negocjowania deklarowanych obrotów, a środki z BLIK-a i szybkich przelewów są dostępne od razu (Paynow, b.d.-a); jego API v3 zawiera pełny tryb Marketplace: boarding sprzedawców po NIP z weryfikacją tożsamości i rachunku, podział płatności tablicą transfers[] z prowizją platformy potrącaną polem feeAmount, zwroty ze splitem i salda sprzedawców (Paynow, b.d.-b). PayU w ofercie standardowej liczy 1,1% + 0,30 zł (BLIK 1,1% + 0,32 zł) przy jednorazowej rejestracji 29 zł i indywidualnych warunkach powyżej 800 tys. zł obrotu (PayU, b.d.-a); odrębny produkt PayU Marketplace obsługuje wielu sprzedawców w jednym koszyku z automatycznym podziałem środków i weryfikacją AML subsprzedawców po stronie PayU (PayU, b.d.-b). W obu przypadkach warunki trybu marketplace ustala się indywidualnie - cenniki standardowe traktuj jako punkt odniesienia do negocjacji, nie jako obietnicę.

Propozycja cennika platformy (do wpisania w fee_schedules jako default i na publiczną stronę cennika):

1. Jedna stawka all-in: 3,9% + 1 zł od zgłoszenia, minimum 1,99 zł, maksimum 14,99 zł. W cenie płatności BLIK/karta/P24, landing w subdomenie, e-maile, eksporty, API i widget, samoobsługa uczestników. Bez abonamentu, bez opłat wdrożeniowych, bez opłat za wydarzenia darmowe.
2. Kto płaci - wybiera organizator (uczestnik / organizator / 50-50), dokładnie jak u RunSignup; domyślnie uczestnik, bo tak wygląda standard rynkowy w PL.
3. Opłaty za operacje samoobsługowe (transfer, odroczenie) ustala organizator i są jego przychodem - platforma bierze od nich tylko standardową prowizję transakcyjną. To wprost zasada ze specyfikacji ATH i mocny argument sprzedażowy: u nas zarabiasz na operacjach, które gdzie indziej robisz ręcznie mailem.
4. Rachunek kontrolny na wpisowym 100 zł płaconym BLIK-iem, przy naszej cenie 4,90 zł od zgłoszenia: przez Stripe koszt płatności 2,60 zł plus udział opłat od wypłaty (0,25% + rozłożone 1,35 zł) i 9 zł miesięcznie za aktywne konto - realna marża w okolicach 2 zł. Przez PayU po stawce standardowej koszt 1,42 zł, marża około 3,5 zł. Przez Paynow po stawce standardowej koszt 0,95 zł, marża blisko 4 zł. Przeniesienie wolumenu krajowego na bramkę polską po pilotażu niemal podwaja marżę przy niezmienionej cenie dla organizatora - to jest cały biznesowy sens etapu I, a Stripe zostaje dla kart zagranicznych i jako plan awaryjny. Dla porównania: ten sam bieg u Datasportu kosztuje organizatora 6,00 zł (w pakiecie z wynikami live), w RunSignup - w USD i bez BLIK-a. ZapisyOnline wyjdzie taniej (0,99 zł plus koszt bramki), ale bez marketplace'u płatności, widgetu, API i samoobsługi - to inna półka produktu. Pozycjonowanie: nowocześniejszy i tańszy od zasiedziałego kombajnu, poważniejszy od minimalistów.
5. Wskaźnik do pilnowania od pilotażu: efektywny koszt płatności jako procent obrotu, per dostawca i per metoda (mix metod realnie decyduje o marży; jeśli karty spoza EEA przekroczą kilka procent wolumenu, wróć do kalkulacji). Po etapie I dashboard G2 powinien pokazywać tę wartość osobno dla Stripe, PayU i Paynow.
6. Zdjęcia jako rodzinny wyróżnik oferty: platforma należy do RunPixie, więc każdy bieg dostaje galerię z automatycznym rozpoznawaniem w standardzie - organizator wybiera, czy zdjęcia są darmowe dla biegaczy (1 zł od rozpoznanej osoby, niezależnie od liczby zdjęć, przenoszone 1:1 bez marży platformy), czy sprzedawane (33% prowizji RunPixie, 67% dla organizatora, bez doliczania prowizji transakcyjnej platformy). RunSignup gra darmową platformą zdjęć jako argumentem pakietu (RunSignup, b.d.), Datasport wiąże zdjęcia z pakietem zapisów i pomiaru (Datasport, b.d.) - nasza wersja jest natywna i daje organizatorowi wybór modelu, z którego może wprost zarabiać. Na stronie cennika pokaż oba tryby z przykładem: bieg na 400 rozpoznanych osób = 400 zł w trybie darmowym albo przychód z pakietów przy sprzedaży.

Mini-prompt na stronę cennika (uruchom przy okazji G1 albo wcześniej, gdy zaczniesz sprzedawać):

```text
Zbuduj publiczną stronę /cennik na domenie głównej: stawka 3,9% + 1 zł
(min 1,99 zł, max 14,99 zł, all-in, zero abonamentu, darmowe wydarzenia
0 zł), interaktywny kalkulator (wpisowe x liczba uczestników -> koszt
u nas vs 6% konkurencji, z uczciwym przypisem czym różnią się pakiety),
tabela co jest w cenie, FAQ (kto płaci prowizję, kiedy wypłaty, faktury),
CTA do formularza kontaktowego onboardingu. Liczby stawek z konfiguracji
(fee_schedules default), nie zahardkodowane w komponencie.
```

## Aneks 3 - przenośność Vercel - własny serwer: reguły szczegółowe

Zasada nadrzędna: Vercel jest tylko wygodną maszyną do uruchamiania standardowej aplikacji Next.js. Każda funkcja platformy musi mieć odpowiednik w docker-compose z etapu H. Tabela odpowiedników:

| Zdolność | PoC (Vercel) | Docelowo (VPS) | Reguła w kodzie |
|---|---|---|---|
| Baza danych | Neon (Postgres) | postgres:16 w kontenerze | tylko DATABASE_URL, bez SDK Neona |
| Pliki | R2 albo S3 | MinIO | tylko S3 API przez src/lib/storage.ts |
| E-mail | Resend | SMTP (nodemailer) | tylko przez EmailProvider |
| Cron | Vercel Cron -> endpoint | crontab -> ten sam endpoint | logika wyłącznie w endpointach /api/cron/* |
| Kolejka | tabela jobs + tick | identycznie | bez Redis/QStash w fazie 1 |
| TLS i subdomeny | wildcard domain Vercel | Caddy + DNS challenge | brak założeń o terminacji TLS w kodzie |
| Custom domeny | Vercel Domains API | Caddy on-demand TLS | interfejs DomainProvider (G1) |
| Obrazki OG | satori + resvg w Node | identycznie | zakaz @vercel/og w edge |
| Optymalizacja obrazów | next/image | next/image + sharp | bez loaderów vendorowych |
| Rate limiting | licznik w Postgresie | identycznie | zakaz Upstash/Vercel KV |
| Błędy i logi | Sentry (SaaS) | Sentry albo GlitchTip | DSN z env, bez integracji vendorowej |
| Sekrety | env Vercela | .env na hoście | wyłącznie src/env.ts z zodem |

Twarde zakazy (agent ma odmówić i zaproponować zamiennik): Vercel KV, Vercel Blob, Vercel Postgres SDK, Edge Runtime/edge middleware z logiką biznesową, @vercel/analytics, localStorage/sessionStorage jako magazyn stanu, biblioteki cron trzymające harmonogram w pamięci procesu (node-cron), zapisy na lokalny dysk poza /tmp.

Sygnały ostrzegawcze przy przeglądzie PR: import z @vercel/*, odwołanie do process.env poza env.ts, użycie fetch do własnych tras zamiast wywołania funkcji, "runtime: 'edge'" w jakiejkolwiek trasie.

## Aneks 4 - mapa ekranów manifestu na prompty

Wszystkie ekrany fazy 1 z manifest_ekranow_v7.csv i miejsce, w którym powstają. Ekrany szkółek (OCL, INS, PAR) i KID pominięte zgodnie z zakresem - wracają w fazie 2.

| Ekran | Nazwa (skrót) | Prompt |
|---|---|---|
| GST-01 | strona operatora / katalog wydarzeń | B2 |
| GST-02 | brandowana strona wydarzenia | B2 |
| GST-03 | strona grupy lub kursu | faza 2 (szkółki) |
| GST-04 | lista dystansów | B2 |
| GST-05 | ekran rozpoczęcia zapisu | B2 (+B5) |
| GST-06 | rejestracja i logowanie | A4 |
| GST-07 | publiczna klasyfikacja serii | D6b |
| GST-08 | strony prawne | B2 + B8 |
| OPR-01 | logowanie operatora | A4 (+G1) |
| OPR-02 | dashboard operatora | G1 + G2 |
| OPR-03 | lista i karta organizatora | G1 |
| OPR-04 | onboarding organizatora | G1 |
| OPR-05 | konfiguracja modelu rozliczeń | G1 |
| OPR-06 | bramki płatności platformy | B6 + I1-I3 (+G2 raporty) |
| OPR-07 | branding i domeny | G1 |
| OPR-08 | integracje | F1 (partnerzy) + G1 |
| OPR-09 | logi i audyt | G1 (fundament w A2/B7) |
| OPR-10 | klucze API i webhooki platformy | E1 + F1 |
| OPR-11 | impersonacja z audytem | G1 |
| OPR-12 | rozliczenia i faktury | G2 |
| OEV-01 | dashboard zawodów | B7 |
| OEV-02..07 | kreator wydarzenia (6 kroków) | B1 (krok gadżetów: D4) |
| OEV-08 | konstruktor formularza | B1 + B3 |
| OEV-09 | zarządzanie dystansami | B1 + B7 |
| OEV-10 | progi cenowe z licznikiem | B1 + B4 |
| OEV-11 | magazyn gadżetów | D4 |
| OEV-12 | lista uczestników | B7 |
| OEV-13 | karta uczestnika | B7 |
| OEV-14 | numery startowe | B6 |
| OEV-15 | listy startowe | B7 + F1 |
| OEV-16 | operacje zwrotów/transferów/odroczeń | D1 (masowe: C3 skrypty) |
| OEV-17 | listy rezerwowe | D2 |
| OEV-18 | płatności i faktury wydarzenia | B6 + B7 |
| OEV-19 | portfel i kredyty uczestników | D3 |
| OEV-20 | komunikacja | D5 |
| OEV-21 | raporty i statystyki | D6a |
| OEV-22 | eksporty | B7 |
| OEV-23 | konfiguracja klasyfikacji serii | D6b |
| OEV-24 | integracja pomiaru czasu | F1 |
| OEV-25 | zarządzanie wolontariuszami | D6c |
| OEV-26 | powiadomienia pogodowe | backlog |
| OEV-27 | wskaźnik ryzyka nieobecności | backlog |
| OEV-28 | ustawienia wydarzenia | B7 (+D1 reguły) |
| OEV-29 | webhooki i API organizatora | E1 + E2 |
| OEV-30 | wgrywanie i publikacja zdjęć (nowy, poza manifestem v7) | J1 |
| ATH-01 | rejestracja i logowanie | A4 + B5 |
| ATH-02 | dashboard moje zapisy | D1 (minimum w B6) |
| ATH-03 | formularz zapisu | B3 + B5 |
| ATH-04 | dodatki i gadżety | D4 |
| ATH-05 | zgody i regulaminy | B5 |
| ATH-06 | podsumowanie i płatność | B6 |
| ATH-07 | potwierdzenie zapisu | B6 |
| ATH-08 | samoobsługa zgłoszenia | D1 |
| ATH-09 | dosprzedaż gadżetów | D4 |
| ATH-10 | portfel i kredyty | D3 |
| ATH-11 | galeria RunPixie | J2 |
| ATH-12 | klasyfikacja serii | D6b |
| ATH-13 | powiadomienia pogodowe | backlog |
| ATH-14 | ustawienia konta i zgody | A4 + B8 |
| VOL-01..07 | cały moduł wolontariusza | D6c |
| KID-01..02 | zgody opiekuna, paszport | faza 2 |

## Aneks 5 - checklisty zgodności (RODO i okolice)

Zastrzeżenie: to lista kontrolna do pracy z prawnikiem, nie porada prawna.

Role przetwarzania: organizator jest administratorem danych uczestników swojego biegu; platforma - podmiotem przetwarzającym dla organizatora oraz administratorem danych kont użytkowników i danych rozliczeniowych organizatorów. Konsekwencje w produkcie: umowa powierzenia w onboardingu (G1), stopki e-maili wskazujące administratora (A5), polityki na landing wydarzenia per organizator (B1/B2).

Do zamówienia u prawnika (docs/legal/README.md z B8): regulamin platformy dla organizatorów (w tym model prowizji i wypłat przez Stripe Connect), regulamin serwisu dla uczestników, umowa powierzenia przetwarzania (załącznik do onboardingu), wzór regulaminu wydarzenia dla organizatorów (wartość dodana!), polityka prywatności i cookies, zasady odstąpienia od umowy przy usłudze związanej z wydarzeniem w określonym terminie (art. 38 ustawy o prawach konsumenta - wyłączenie prawa odstąpienia dla usług związanych z wydarzeniami rozrywkowymi/sportowymi z oznaczoną datą; jak to się ma do wpisowego i do gadżetów osobno - do rozstrzygnięcia z prawnikiem, produktowo B8 rozdziela usługę startu od towarów).

Model płatności do potwierdzenia z prawnikiem: w konstrukcji Stripe Connect z destination charges środki uczestników przechodzą przez licencjonowaną instytucję (Stripe), nie przez rachunki platformy - intencją jest uniknięcie statusu dostawcy usług płatniczych po stronie platformy. Poproś prawnika o potwierdzenie tej kwalifikacji na gruncie ustawy o usługach płatniczych, zanim ruszy produkcyjny pilotaż.

Dane szczególne: oświadczenie zdrowotne tylko wtedy, gdy organizator go wymaga, za osobną zgodą (B5), przechowywane oddzielnie od answers i objęte krótszą retencją - dopisz do przeglądu retencji w B8.

Zdjęcia (etap J): wizerunek na zdjęciach to dane osobowe - łańcuch ról do ułożenia z prawnikiem obejmuje organizatora (administrator danych uczestników), platformę i RunPixie; choć platforma i RunPixie należą do jednej grupy, odrębne podmioty prawne wymagają umowy powierzenia albo ustalenia współadministrowania, a klauzule informacyjne uczestnika muszą wymieniać odbiorcę danych. Mechanika w produkcie: rozpoznawane są wszystkie numery startowe - to element kosztowy usługi, rozliczany z organizatorem na podstawie jego uprzedniej deklaracji (J1) - natomiast ekspozycja jest bramkowana zgodami: publiczna wyszukiwalność wymaga osobnej zgody image_public (J2), wycofanie zgody ukrywa osobę z widoków publicznych, retencja zdjęć jest konfigurowalna, a utrata prawa odstąpienia przy pobraniu treści cyfrowej jest odnotowywana przed pobraniem. Dwie kwalifikacje do potwierdzenia z prawnikiem: prywatne doręczenie zawodnikowi jego własnych zdjęć bez zgody wizerunkowej (udostępnienie osobie, której dane dotyczą, a nie rozpowszechnianie wizerunku) oraz rozpoznawanie numerów osób bez zgód jako techniczny etap przetwarzania w ramach usługi organizatora.

Praktyki, które już są w promptach i mają takie zostać: minimalizacja pól formularza (B3), wersjonowanie zgód z IP i datą (B5), log dostępu do danych (B7), eksport i usunięcie danych na żądanie (B8), anonimizacja zamiast twardego kasowania tam, gdzie rozliczenia wymagają śladu (B8), maskowanie PII w logach i monitoringu (C2).

---

## Bibliografia

Datasport. (b.d.). Oferta. Pobrano 17 lipca 2026, z https://online.datasport.pl/page/oferta.php

Nielsen, J. (1994). Ten usability heuristics for user interface design. Nielsen Norman Group. https://www.nngroup.com/articles/ten-usability-heuristics/

Paynow. (b.d.-a). Bramka płatności online. Pobrano 17 lipca 2026, z https://www.paynow.pl/

Paynow. (b.d.-b). Marketplace. Dokumentacja Paynow API v3. Pobrano 17 lipca 2026, z https://docs.paynow.pl/pl/docs/v3/marketplace

PayU. (b.d.-a). Oferta handlowa. Pobrano 17 lipca 2026, z https://poland.payu.com/oferta-handlowa/

PayU. (b.d.-b). Marketplace (dokumentacja dla integratorów). Pobrano 17 lipca 2026, z https://developers.payu.com/en/marketplace.html

RunPixie. (b.d.). Strona główna. Pobrano 17 lipca 2026, z https://runpixie.com/ (warunki modułu zdjęć w tym dokumencie - 1 zł od rozpoznanej osoby w trybie darmowym, 33% prowizji przy sprzedaży - pochodzą z ustaleń właścicielskich, nie z publicznego cennika serwisu).

RunSignup. (b.d.). Pricing. Pobrano 17 lipca 2026, z https://info.runsignup.com/pricing

Stripe. (b.d.-a). Pricing (Polska). Pobrano 17 lipca 2026, z https://stripe.com/pl/pricing

Stripe. (b.d.-b). Connect pricing. Pobrano 17 lipca 2026, z https://stripe.com/pl/connect/pricing

W3C. (2018). Web Content Accessibility Guidelines (WCAG) 2.1 (W3C Recommendation). https://www.w3.org/TR/WCAG21/

Wroblewski, L. (2008). Web form design: Filling in the blanks. Rosenfeld Media.

ZapisyOnline. (b.d.). Cennik. Pobrano 17 lipca 2026, z https://zapisyonline.pl/cennik

Odesłania do Nielsena, W3C i Wroblewskiego pochodzą ze specyfikacji ekranów w docs/design/ (heurystyki użyteczności, WCAG 2.1 AA, progresywne odsłanianie pól formularzy) i obowiązują implementację tak samo, jak obowiązywały makiety.
