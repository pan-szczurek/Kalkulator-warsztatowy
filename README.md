# Kalkulator warsztatowy

Aplikacja mobilna (Flutter, Android) dla operatorów tokarek, frezarek i maszyn CNC oraz dla każdego, kto pracuje w warsztacie. W jednym miejscu zbiera kalkulatory, tabele i ściągawki, które przydają się przy maszynie: od liczenia masy detalu i doboru metody spawania, przez obroty, posuwy, moc i gwinty, po pasowania ISO, zębatki wymienne i kody G i M. Do aplikacji loguje się adresem e-mail, a operator może publikować na ekranie głównym aktualizacje i wskazówki, także osobno dla wybranych grup.

Aplikacja ma ciemny, "szklany" wygląd: tło z kręcącymi się kołami zębatymi i sylwetką tokarki, półprzezroczyste przyciski o zaokrąglonych końcach i wysuwane z prawej strony menu z trzema kreskami.

---

## Spis treści

1. [Dla kogo jest aplikacja](#dla-kogo-jest-aplikacja)
2. [Ekran główny i nawigacja](#ekran-główny-i-nawigacja)
3. [Konta i logowanie](#konta-i-logowanie)
4. [Role i uprawnienia](#role-i-uprawnienia)
5. [Wpisy operatora i grup](#wpisy-operatora-i-grup)
6. [Grupy](#grupy)
7. [Panel operatora: użytkownicy i role](#panel-operatora-użytkownicy-i-role)
8. [Kalkulatory podstawowe: pole, obwód, masa](#kalkulatory-podstawowe-pole-obwód-masa)
9. [Spawanie: dobór metody](#spawanie-dobór-metody)
10. [Tokarka](#tokarka)
11. [Frezarka](#frezarka)
12. [Wiercenie i gwinty](#wiercenie-i-gwinty)
13. [CNC i ściągawki](#cnc-i-ściągawki)
14. [Pomiary i tolerancje](#pomiary-i-tolerancje)
15. [Narzędzia warsztatowe](#narzędzia-warsztatowe)
16. [Wyszukiwarka funkcji](#wyszukiwarka-funkcji)
17. [Bezpieczeństwo i dane](#bezpieczeństwo-i-dane)
18. [Technologia](#technologia)
19. [Konfiguracja projektu](#konfiguracja-projektu)
20. [Zastrzeżenia](#zastrzeżenia)

---

## Dla kogo jest aplikacja

- Operatorzy i ustawiacze tokarek, frezarek, wiertarek oraz obrabiarek CNC.
- Spawacze i ślusarze, którzy chcą szybko dobrać metodę, drut, elektrodę i gaz.
- Technolodzy i osoby wyceniające obróbkę.
- Zespoły warsztatowe, w których jedna osoba (operator) publikuje aktualizacje, informacje i wskazówki, a administratorzy zarządzają grupami ludzi.

---

## Ekran główny i nawigacja

Ekran główny jest celowo prosty: pokazuje tło i wpisy. Na górze po prawej jest ikona menu (trzy kreski), z której otwiera się wysuwane menu ze wszystkimi funkcjami.

**Tło** to ciemny, stalowy gradient z delikatną siatką, dwoma kręcącymi się kołami zębatymi (kręcą się w przeciwne strony, z jednym zegarem, więc obrót jest płynny także przy zmianie ekranu) i sylwetką tokarki na dole.

**Menu (po prawej)** zawiera:

| Pozycja | Zawartość |
|---|---|
| Okrągła ikona konta (obok napisu "Menu") | logowanie, rejestracja, profil, wylogowanie |
| Szukaj funkcji | wyszukiwarka wszystkich narzędzi |
| Oblicz pole | 10 figur |
| Oblicz obwód | 10 figur |
| Oblicz masę | 10 brył, 28 materiałów |
| Spawanie - dobór metody | doradca spawalniczy |
| Tokarka | 10 narzędzi |
| Frezarka | 6 narzędzi |
| Wiercenie i gwinty | 4 narzędzia |
| CNC i ściągawki | 4 narzędzia |
| Pomiary i tolerancje | 5 narzędzi |
| Narzędzia warsztatowe | 7 narzędzi |
| Grupy | grupy, do których należysz lub którymi zarządzasz |
| Dodaj wpis na ekran główny | tylko operator |
| Użytkownicy i role | tylko operator |
| Odśwież konto | odświeża uprawnienia po potwierdzeniu adresu e-mail |

Pod napisem "Menu" widać, kto jest zalogowany i jaką ma rolę. Jeśli adres e-mail nie jest jeszcze potwierdzony, menu o tym przypomina.

---

## Konta i logowanie

Logowanie działa przez Firebase Authentication (adres e-mail i hasło).

- **Rejestracja** wymaga podania imienia i nazwiska (po minimum 2 litery każde), adresu e-mail i hasła (minimum 6 znaków, z powtórzeniem).
- Po założeniu konta Firebase wysyła wiadomość z linkiem potwierdzającym adres e-mail. Uprawnienia (role, grupy) działają dopiero po potwierdzeniu adresu i wybraniu w menu pozycji **Odśwież konto**.
- **Odzyskiwanie hasła**: przycisk "Nie pamiętam hasła" wysyła wiadomość z linkiem do zmiany hasła.
- **Profil**: po zalogowaniu widać imię i nazwisko, adres e-mail i rolę. Imię i nazwisko można zmienić.
- Zalogowanie jest pamiętane po zamknięciu aplikacji.
- Błędy (zły e-mail lub hasło, za słabe hasło, brak internetu, zbyt wiele prób) są pokazywane po polsku.
- Opcja w kodzie (`kRequireLogin`) pozwala wymagać zalogowania przed wejściem do funkcji z menu. Domyślnie narzędzia są dostępne bez logowania.

---

## Role i uprawnienia

| Rola | Co może |
|---|---|
| **Użytkownik** | używa wszystkich narzędzi, widzi wpisy operatora oraz wpisy i listę grup, do których został dodany |
| **Administrator grupy** | w swojej grupie dodaje i usuwa zwykłych członków oraz dodaje wpisy dla grupy; widzi tylko tę grupę i nie ma funkcji operatora |
| **Administrator** | tworzy grupy, dodaje do nich osoby, nadaje i odbiera rolę administratora grupy, dodaje wpisy w swoich grupach i usuwa swoje grupy |
| **Operator** | wszystko powyżej we wszystkich grupach oraz wpisy na ekranie głównym, nadawanie roli administratora, wyszukiwanie i usuwanie użytkowników |
| **Główny operator** | to samo co operator, a dodatkowo jako jedyny nadaje i odbiera rolę operatora; nie można go usunąć |

Główny operator jest wskazany adresem e-mail w kodzie i w regułach bazy. Rolę dostaje tylko po potwierdzeniu adresu, więc nikt nie przejmie jej, rejestrując się na cudzy adres. Pozostałe role są zapisane w bazie.

Uprawnienia są egzekwowane po stronie serwera przez reguły Firestore, a nie tylko ukrywaniem przycisków w aplikacji.

---

## Wpisy operatora i grup

Wpisy to krótkie informacje wyświetlane na ekranie głównym.

- **Rodzaje wpisu:** Aktualizacja, Wskazówka spawalnicza, Informacja.
- **Pola:** tytuł i treść. Wpis ma datę dodania.
- **Wpisy operatora** są widoczne dla wszystkich (także niezalogowanych). Jeśli mają być tylko dla zalogowanych, wystarczy zmienić jedną linijkę w regułach bazy.
- **Wpisy grup** widzą tylko członkowie danej grupy oraz osoby nią zarządzające. Wyświetlają się na ekranie głównym **jako pierwsze**, pogrupowane pod nagłówkiem z nazwą grupy, a pod nimi pojawiają się wpisy operatora.
- Wpisy można edytować i usuwać z menu z trzema kropkami przy wpisie. Operator zarządza wpisami ogólnymi, a wpisami grupy zarządzają: operator, administrator, który utworzył grupę, i administrator tej grupy.
- Operator dodaje wpis okrągłym przyciskiem "plus" na ekranie głównym, a wpisy grupy dodaje się na ekranie grupy.

---

## Grupy

- Grupy tworzy operator lub administrator. Twórca grupy jest automatycznie jej członkiem.
- Członków dodaje się po adresie e-mail (także osoby, które jeszcze nie mają konta; zobaczą grupę po założeniu konta i potwierdzeniu adresu).
- **Administrator grupy**: właściciel grupy (operator lub administrator, który ją utworzył) klika ikonę tarczy przy osobie w grupie, aby nadać lub odebrać jej rolę administratora grupy. Taka osoba widzi w "Grupach" tylko swoją grupę, nie ma przycisku tworzenia grup ani panelu użytkowników, a w swojej grupie może dodawać i usuwać zwykłych członków oraz publikować wpisy. Nie może nadawać ról, usuwać grupy ani usuwać innych administratorów grupy.
- Ekran grupy pokazuje listę osób, rolę każdej z nich i wpisy grupy.
- Zwykły użytkownik widzi grupy, do których został dodany, i ich wpisy.

---

## Panel operatora: użytkownicy i role

Dostępny w menu jako "Użytkownicy i role" (tylko operator).

- **Wyszukiwarka** po imieniu, nazwisku lub adresie e-mail. Wyniki pojawiają się na bieżąco i działają także bez polskich liter.
- **Lista użytkowników** z imieniem i nazwiskiem, adresem e-mail i rolą. Osoba pojawia się na liście po pierwszym zalogowaniu.
- **Nadawanie i odbieranie roli administratora** jednym przyciskiem.
- **Nadawanie i odbieranie roli operatora** (tylko główny operator).
- **Usuwanie użytkownika** z potwierdzeniem: osoba traci role i członkostwo w grupach, znika z listy, a przy próbie wejścia do aplikacji jest wylogowywana z komunikatem. Reguły bazy odcinają jej dostęp do wpisów i grup.
- **Przywracanie**: przełącznik "Pokaż usuniętych użytkowników" pokazuje usuniętych i pozwala ich przywrócić.
- Głównego operatora nie da się usunąć ani zmienić, a zwykły operator nie może zmieniać ani usuwać innych operatorów.

Uwaga: usunięcie w aplikacji blokuje użytkownika, ale nie kasuje jego konta logowania w Firebase (wymagałoby to serwera). Konto można całkowicie usunąć w konsoli Firebase w sekcji Authentication.

---

## Kalkulatory podstawowe: pole, obwód, masa

### Pole i obwód

Osobne działy "Oblicz pole" i "Oblicz obwód". Do wyboru 10 figur: kwadrat, prostokąt, trójkąt, trójkąt równoboczny, koło, trapez, romb, równoległobok, pięciokąt foremny i sześciokąt foremny. Każda figura ma własne pola na wymiary (np. trójkąt: podstawa i wysokość dla pola, trzy boki dla obwodu). Liczby można wpisywać z kropką lub przecinkiem.

### Masa

Wybierasz bryłę, materiał i wymiary w milimetrach, wpisujesz ilość sztuk. Aplikacja podaje objętość (cm3), masę jednej sztuki (kg i g) oraz masę razem.

**Bryły (10):** klocek (prostopadłościan), sześcian, blacha/płyta, wałek (walec), rura okrągła/tuleja, pręt kwadratowy, pręt sześciokątny (rozmiar klucza), rura kwadratowa/prostokątna (dwa boki zewnętrzne A i B, grubość ścianki, długość), kula i stożek.

**Materiały (28), z gęstością w g/cm3:**
- Stale i żeliwa: stal zwykła (czarna), stal nierdzewna 304 i 316, stal szybkotnąca HSS, żeliwo szare, żeliwo sferoidalne.
- Metale nieżelazne: aluminium (6061 i 7075), miedź, mosiądz, brąz cynowy, brąz aluminiowy, tytan, cynk, ołów, cyna, nikiel, magnez, wolfram, Inconel 718.
- Tworzywa: teflon (PTFE), poliamid PA6, Delrin (POM), polietylen PE-HD, PVC, pleksi (PMMA), poliwęglan (PC), PEEK.

Obliczenie rur profilowych pomija zaokrąglenia narożników, więc dla profili ze zaokrąglonymi rogami wynik jest o kilka procent zawyżony. Gęstości są tablicowe, przybliżone.

---

## Spawanie: dobór metody

Doradca podpowiada, której metody użyć i czym spawać. Dane wejściowe:

- **Materiał (11):** stal zwykła (konstrukcyjna), stal ocynkowana, stal ulepszana (np. C45), stal narzędziowa, stal na ścieranie (Hardox), stal nierdzewna (304 / 316), aluminium, miedź, mosiądz i brąz, tytan, żeliwo.
- **Rodzaj złącza:** blacha lub płaskownik (czołowo), blacha (teowo, spoina pachwinowa), rura lub profil zamknięty (czołowo), pręt lub wałek pełny (czołowo).
- **Grubość materiału** (dla rur grubość ścianki, dla prętów średnica).
- **Miejsce pracy:** warsztat (bez wiatru) lub na zewnątrz.
- **Priorytet:** jakość i estetyka albo szybkość i koszt.

Wynik zawiera:

- **Zalecaną metodę** (TIG, MIG/MAG lub elektroda otulona MMA) wraz z uzasadnieniem, np. dlaczego na zewnątrz wybrana jest elektroda (wiatr rozwiewa osłonę gazową).
- **Porównanie metod** dla podanej grubości: wady i zalety TIG, MIG/MAG i elektrody. Przykład: przy 20 mm wychodzi MIG/MAG wielościegowo, a TIG tylko na przetop korzenia.
- **Materiał dodatkowy** dla każdej metody: typ i średnica drutu lub elektrody, gaz osłonowy z orientacyjnym przepływem, elektroda wolframowa, orientacyjny prąd, a dla elektrod zasadowych także suszenie.
- **Przygotowanie złącza:** rodzaj skosu (I, V, X, K), kąt, stopka, szczelina, orientacyjna liczba ściegów, kolejność spawania.
- **Uwagi materiałowe:** podgrzew i temperatura międzywarstwowa, czyszczenie, odkształcenia oraz zagrożenia, np. opary cynku przy stali ocynkowanej, tlenek i podgrzew w aluminium, formowanie grani w nierdzewce, pełna osłona argonu przy tytanie, spawanie żeliwa na zimno elektrodą niklową.

Wskazówki są orientacyjne. Parametry (prąd, napięcie, prędkość) należy dobrać na próbce, a konstrukcje nośne, zbiorniki i elementy pod ciśnieniem wymagają technologii wg WPS i spawacza z uprawnieniami (EN ISO 9606).

---

## Tokarka

Dział zawiera 10 narzędzi. Kalkulatory z doborem prędkości pozwalają wybrać **materiał obrabiany** (stal konstrukcyjna, stal ulepszana, stal nierdzewna, żeliwo szare, aluminium, mosiądz, miedź, tytan, tworzywa) oraz **narzędzie** (węglik spiekany lub HSS). Aplikacja podpowiada zakres zalecanej prędkości skrawania Vc, a jeśli pole Vc zostanie puste, bierze wartość średnią.

| Narzędzie | Co liczy |
|---|---|
| Obroty i prędkość skrawania | obroty n z średnicy i Vc, zalecany zakres Vc |
| Prędkość skrawania z obrotów | Vc z średnicy i obrotów |
| Czas toczenia | czas maszynowy z drogi, posuwu, obrotów i liczby przejść |
| Chropowatość Ra z posuwu | teoretyczne Ra i Rz z posuwu i promienia naroża płytki |
| Posuw dla wymaganego Ra | maksymalny posuw dla zadanej chropowatości |
| Moc skrawania | siła skrawania Fc, moc skrawania i wymagana moc silnika (sprawność 80%) |
| Toczenie stożka | kąt nastawy suportu, pełny kąt, zbieżność 1:x i przesunięcie konika |
| Zębatki wymienne (gwint) | kombinacje kół do gwintowania na tokarce konwencjonalnej |
| Moment na wrzecionie | moment obrotowy z mocy i obrotów |
| Zalecane prędkości skrawania | tabela Vc dla 9 materiałów i dwóch rodzajów narzędzi |

**Zębatki wymienne:** program przeszukuje komplet kół 20 do 120 co 5 oraz 127 i podaje trzy najlepsze układy Z1, Z2, Z3, Z4 wraz z błędem w procentach względem szukanego skoku.

---

## Frezarka

| Narzędzie | Co liczy |
|---|---|
| Obroty i posuw frezowania | obroty, posuw na ząb fz i posuw stołu Vf dla średnicy freza, liczby zębów, materiału i narzędzia |
| Moc skrawania | moc, wymagana moc silnika i wydajność skrawania w cm3/min |
| Czas frezowania | czas maszynowy z drogi, posuwu i liczby przejść |
| Głowica podziałowa | obroty korbki i otwory na tarczy dla N podziałów (przełożenie 40:1, tarcze 15 do 49) |
| Moment na wrzecionie | moment obrotowy z mocy i obrotów |
| Zalecane prędkości skrawania | tabela Vc |

Posuw na ząb jest podpowiadany zależnie od średnicy freza, materiału i rodzaju narzędzia, a można wpisać własną wartość.

---

## Wiercenie i gwinty

| Narzędzie | Co liczy |
|---|---|
| Obroty i posuw wiercenia | obroty, zakres posuwu w mm/obr i posuw Vf dla średnicy wiertła i materiału |
| Wiertło pod gwint | 25 gwintów metrycznych (zgrubne i drobnozwojne): średnica wiertła dla gwintu ciętego i formowanego, średnica podziałowa, głębokość profilu |
| Gwint rurowy G | G1/8, G1/4, G3/8, G1/2, G3/4 i G1: średnica zewnętrzna, zwoje na cal, wiertło |
| Głębokość z końcówką | długość stożkowej końcówki i głębokość Z do programu CNC (domyślnie kąt 118 st.) |

---

## CNC i ściągawki

- **Kody G i M (ściągawka)**: kody G (ruchy, płaszczyzny, jednostki, korekcje, układy współrzędnych, cykle stałe, tryby programowania), cykle tokarskie Fanuc (G70, G71, G72, G76), kody M oraz adresy (X Y Z, F, S, T, D, H, I J K, R). Przy ściągawce jest przypomnienie, że kody zależą od sterowania (Fanuc, Siemens, Heidenhain, Mazak) i że trzeba sprawdzić instrukcję maszyny.
- **Otwory na okręgu**: współrzędne X i Y każdego otworu na okręgu (2 do 60 otworów) z możliwością podania kąta pierwszego otworu, gotowe do wpisania do programu.
- **Przeliczniki jednostek** (12 przeliczeń): mm i cale, m/min i SFM, kW i KM, bar i psi, Nm i funto-stopy oraz zwoje na cal (TPI) i skok w mm.
- **Zalecane prędkości skrawania**: tabela Vc.

---

## Pomiary i tolerancje

- **Pasowania i tolerancje ISO**: według ISO 286 dla wymiarów do 500 mm. Otwory H6, H7, H8, H9, H11 oraz wałki h6, h7, g6, f7, e8, d9, k6, m6, n6, p6. Wynik: odchyłki ES i EI oraz es i ei w mikrometrach, wymiary graniczne otworu i wałka, rodzaj pasowania (luźne, mieszane, wciskowe) z luzem lub wciskiem.
- **Trójkąt prostokątny**: po wpisaniu dwóch wartości (boków lub boku i kąta) liczy pozostałe boki, przeciwprostokątną i kąty. Przydatne do fazowań i nachyleń.
- **Sinusówka (stos płytek)**: wysokość stosu płytek wzorcowych dla zadanego kąta i rozstawu rolek (domyślnie 100 mm).
- **Rozszerzalność cieplna**: zmiana długości detalu po nagrzaniu ponad 20 st. C dla stali, nierdzewki, żeliwa, aluminium, miedzi, mosiądzu, tytanu oraz tworzyw (POM, PTFE).
- **Stożki Morse i 7:24**: tabele stożków MT0 do MT6 i ISO 30, 40 i 50 ze średnicą nominalną, zbieżnością i kątem nastawy.

---

## Narzędzia warsztatowe

- **Wycena obróbki**: koszt maszynowy, koszt przygotowania rozłożony na sztuki, materiał i narzut, czyli cena za sztukę i razem.
- **Stoper i licznik sztuk**: stoper z przyciskiem zapisu cyklu. Pokazuje liczbę sztuk, ostatni i średni czas cyklu oraz wydajność w sztukach na godzinę.
- **Rozkrój pręta**: ile sztuk wyjdzie z pręta przy danej długości detalu, szerokości cięcia piły, naddatku na planowanie i odpadzie końcowym. Podaje też resztę i procent odpadu.
- **Moment dokręcania śrub**: M3 do M24, klasy wytrzymałości 8.8, 10.9 i 12.9 (wartości orientacyjne dla tarcia 0,14).
- **Mieszanie chłodziwa**: ilość koncentratu i wody dla zadanej objętości zbiornika i stężenia.
- **Prędkość obwodowa tarczy**: prędkość w m/s, maksymalne obroty tarczy i ostrzeżenie o przekroczeniu prędkości dopuszczalnej z etykiety.
- **Przeliczniki jednostek**.

---

## Wyszukiwarka funkcji

Pozycja "Szukaj funkcji" na górze menu. Wpisujesz fragment nazwy lub działu, np. "gwint", "moc" albo "pasowanie", a aplikacja na bieżąco pokazuje pasujące narzędzia wraz z nazwą działu. Wyszukiwanie działa także bez polskich liter. Dotknięcie wyniku otwiera narzędzie.

---

## Bezpieczeństwo i dane

- Konta, hasła i dane użytkowników przechowuje Firebase (Authentication i Cloud Firestore). Aplikacja nie przechowuje haseł.
- Reguły bazy (`firestore.rules`) egzekwują uprawnienia po stronie serwera: kto czyta, tworzy, edytuje i usuwa wpisy, grupy i profile użytkowników.
- Role operatora, administratora i członkostwa w grupach działają tylko dla potwierdzonych adresów e-mail.
- Użytkownik nie może samodzielnie zmienić swojej roli ani usunąć blokady; może zmienić tylko własne imię i nazwisko.
- Usunięty użytkownik jest blokowany także w regułach bazy.
- Wpisy ogólne są domyślnie publiczne do odczytu. Można to ograniczyć do zalogowanych jedną zmianą w regułach.

---

## Technologia

- **Flutter / Dart** (Material 3), cały kod aplikacji w jednym pliku `main.dart`.
- **Firebase Authentication** (e-mail i hasło) oraz **Cloud Firestore** (kolekcje: `users`, `posts`, `groups` z podkolekcją `posts`).
- Pakiety: `firebase_core`, `firebase_auth`, `cloud_firestore`.
- Własny rysunek tła (`CustomPainter`) z animacją kół zębatych, uniwersalny silnik kalkulatorów (jedna definicja narzędzia: pola, listy wyboru i wzór) oraz wspólne elementy interfejsu w stylu szkła.
- Docelowa platforma: Android. Konfiguracja dla iOS jest możliwa, ale zbudowanie wersji na iPhone'a wymaga komputera Mac.

---

## Konfiguracja projektu

1. Utwórz projekt w konsoli Firebase i włącz **Authentication** z logowaniem **Email/Password**.
2. Utwórz bazę **Cloud Firestore** (tryb produkcyjny) i w zakładce Rules wklej zawartość `firestore.rules`, a następnie opublikuj.
3. Dodaj paczki: `flutter pub add firebase_core firebase_auth cloud_firestore`.
4. Połącz aplikację z projektem narzędziem FlutterFire: `flutterfire configure` (powstaje plik `lib/firebase_options.dart`).
5. Wpisz adres e-mail głównego operatora (małymi literami) w dwóch miejscach: w `main.dart` przy stałej `kOperatorEmail` oraz w `firestore.rules` w funkcji `operatorEmail`.
6. Jeśli build na Androidzie zgłosi zbyt niski `minSdk`, ustaw w `android/app/build.gradle` wartość `23`.
7. Uruchom aplikację (`flutter run`) lub zbuduj plik instalacyjny (`flutter build apk --release`).

Po pierwszym zalogowaniu na adres operatora trzeba potwierdzić adres e-mail i wybrać w menu "Odśwież konto". Wtedy rola operatora zaczyna działać..

---

## Zastrzeżenia

Prędkości skrawania, posuwy, moce skrawania, momenty dokręcania, gęstości materiałów, parametry spawania i dobór metod są wartościami **orientacyjnymi**, opartymi na danych tablicowych i ogólnie przyjętych zasadach. Zależą od narzędzia, maszyny, chłodzenia, sztywności i warunków pracy. Przy ważnych, nośnych lub bezpieczeństwo-krytycznych elementach zawsze porównaj wyniki z katalogiem producenta narzędzia, normami i dokumentacją maszyny. Spawanie konstrukcji nośnych, zbiorników i elementów pod ciśnieniem wymaga technologii wg WPS i spawacza z uprawnieniami. Kody G i M różnią się zależnie od sterowania. Autor nie ponosi odpowiedzialności za skutki użycia wyników bez weryfikacji.**

**
