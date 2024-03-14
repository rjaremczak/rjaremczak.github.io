Mikroprocesor w stylu „retro”
Używając wysokopoziomowych języków programowania i wyszukanych abstrakcji, często tracimy z oczu procesor, a to on ostatecznie wykonuje napisany przez nas program. Bohaterem tego artykułu jest MOS 6510, mikroprocesor o architekturze, której prostotę i piękno oddaje następujący cytat:

„Wydaje się, że doskonałość osiąga się nie wtedy, kiedy nie można już nic dodać, ale raczej wtedy, gdy nie można nic ująć.”
Antoine de Saint-Exupéry


Wprowadzenie, czyli motywacja

Dzisiejsze komercyjne mikroprocesory są złożonymi, wielordzeniowymi konstrukcjami, których architektura wewnętrzna jest trudna do zrozumienia we wszystkich szczegółach przez pojedynczego człowieka. Dlatego programiści, szczególnie ci używający języków wysokiego poziomu, traktują procesor jak tzw. „czarną skrzynkę”, zdając się całkowicie na kompilator i ewentualnie maszynę wirtualną, ignorując tym samym sprzęt podczas pracy z kodem. Daje to złudne poczucie braku ograniczeń, co niekiedy prowadzi do powstawania kodu stwarzającego tylko pozory „dobrego rzemiosła”, a będącego w rzeczywistości zlepkiem modnych wzorców projektowych połączonych ze sobą za pomocą „spaghetti code”. Tworzenie oprogramowania ma obecnie charakter masowy, a więc kładzie się nacisk na standaryzację, zwiększenie wydajności programisty i obniżenie ceny dostarczonej funkcjonalności. Bazowanie głównie na frameworkach i gotowych “stockowych” rozwiązaniach ma jednak taką wadę, że może prowadzić do suboptymalnych rozwiązań, należy być świadomym istnienia takiego ryzyka.

W początkach mojej kariery programistycznej miałem przyjemność tworzenia oprogramowania na komputery 8-bitowe, było to we wczesnych latach 90-tych XX wieku. W tamtych czasach system operacyjny był zaledwie biblioteką procedur, a sprzęt był dostępny bezpośrednio i w pełnym zakresie. Zaawansowany programista wiedział prawie wszystko o docelowym systemie, na który pisał program. Obecnie taki poziom zrozumienia sprzętu spotyka się praktycznie tylko w środowisku systemów wbudowanych. Systemy o ograniczonych, jak na dzisiejsze standardy, zasobach wymuszały na programiście, oprócz konieczności ich gruntownego poznania, swoistą dyscyplinę pracy i oszczędność w wykorzystywaniu pamięci oraz optymalizację kodu pod każdym względem. Kosztowało to dużo pracy, ale w efekcie częściej powstawał kod elegancki, wydajny i zwięzły niż ma to miejsce obecnie. Z konieczności musiał taki być.

Proponuję swoisty powrót do źródeł poprzez zapoznanie się ze znanym i niegdyś bardzo popularnym mikroprocesorem MOS 6502, a ściślej jego wariantem MOS 6510, który został użyty w komputerze Commodore C64. Korzystając z postępu technologii i wzrostu wydajności, zamiast korzystać z oryginału, możemy, pisząc emulator, uzyskać pogłębioną wiedzę o jego funkcjonowaniu, co pozwoli poznać logiczną strukturę tego mikroprocesora niejako „od środka”.

Projekt mo65x, czyli emulator

Repozytorium z kodem emulatora znajduje się pod adresem: https://github.com/rjaremczak/mo65x. Na przedstawionych poniżej Rysunkach 1, 2 oraz 3 zaprezentowano jego trzy podstawowe tryby pracy:


Rysunek 1. Widok w trybie assemblera


Rysunek 2. Widok w trybie podglądu pamięci


Rysunek 3. Widok w trybie disassemblera

Pierwsza część tego artykułu poświęcona jest architekturze samego procesora z odwołaniami do kodu źródłowego niniejszego projektu. Projekt jest oparty na bibliotece Qt 5.13.x i kompilowany kompilatorem GCC w wersji wspierającej co najmniej standard języka C++17. W chwili pisania artykułu przetestowana została konfiguracja pracująca pod systemem Ubuntu 19.10, Windows 10/MinGW oraz macOS Catalina 10.15.1. W celu skonfigurowania lokalnego środowiska Qt należy:

Pobrać i zainstalować kompilator wspierany przez Qt , najlepiej GCC, czy MinGW/MSVC dla systemu Windows. Szczegółowy opis zawiera dokumentacja na stronach Qt.

Pobrać i zainstalować darmową edycję Qt SDK w wersji 5.13.1 lub nowszej (jeśli jest zgodna z projektem) ze strony https://qt.io. Wygodne jest użycie instalatora online. W procesie instalacji, zależnie od systemu operacyjnego, należy zwrócić uwagę na możliwość instalacji kompilatora i modułów pomocniczych, integrujących go z biblioteką Qt.


Sklonować lub ściągnąć repozytorium z projektem mo65x.


Otworzyć plik mo65x.pro z poziomu QtCreatora, który jest zintegrowanym środowiskiem programistycznym dla projektów Qt. QtCreator powinien wykryć istniejący w systemie kompilator i zaproponować utworzenie kompletnej konfiguracji do budowania oraz uruchamiania projektu.


W ramach jednej konfiguracji może istnieć kilka gotowych ustawień, standardowo są tworzone ustawienia Debug, Profile i Release. W projekcie zawarty został zestaw testów jednostkowych, dla których trzeba utworzyć osobne ustawienia. W tym celu w sekcji Build Settings wybieramy konfigurację Debug, a następnie klikamy Clone. Nowo utworzonym ustawieniom nadajemy nazwę Test, zmieniamy również ścieżkę wynikową Build directory, podmieniając w niej Debug na Test. Następnie rozwijamy sekcję Build Steps i w polu Additional arguments wpisujemy: CONFIG+=test. Zestaw testów uruchamiamy przez wybranie opcji Tools→Tests→Run All Tests.

W tym momencie w trybie podstawowej, domyślnie utworzonej konfiguracji powinniśmy mieć możliwość lokalnego skompilowania i uruchomienia emulatora. W związku z tym, że jest to projekt o charakterze edukacyjnym, nie jest wyposażony w automatyczny instalator i nie zawiera gotowych, skompilowanych plików wykonywalnych. 

Krótka biografia bohatera

Okresem, w którym powstawała architektura procesora MOS 6502, jest połowa lat 70-tych XX wieku. Okres ten to świt klasycznych konstrukcji 8-bitowych, jak Intel 8080 i Motorola MC6800. Wkrótce powstały oparte na nich konkurencyjne produkty, jak Z80 firmy Zilog, zgodny z Intel 8080, oraz wreszcie MOS 6502 wzorowany na Motoroli 6800, jednak bez zachowania kompatybilności.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 

Mikroprocesor MOS 6502 zaprojektowali Chuck Peddle i Bill Mensch z MOS Technology, a swoją premierę miał w 1975 roku. Głównym założeniem była prostota i niska cena, ostatecznie kosztował on 25 USD, co stanowiło ułamek ceny produktów konkurencji w tamtym czasie. W celu redukcji kosztów MOS 6502 był z konieczności uproszczony względem MC6800 i miał okrojony zestaw rozkazów. Jedną z niezaimplementowanych instrukcji była słynna HCF (ang. halt and catch fire – zatrzymaj się i stań w ogniu), która uruchamiała wewnętrzny mechanizm self-testu. Ciekawostką jest, że pełna nazwa tej instrukcji została wykorzystana jako tytuł popularnego serialu produkcji AMC opowiadającego o początkach ery komputerów IBM PC. 

Prezes firmy Commodore, Jack Tramiel, czyli Jacek Trzmiel, biznesmen polskiego pochodzenia, był znany z zamiłowania do oszczędności. Na potrzeby projektu Commodore C64 zakupił firmę MOS Technology i zlecił zaprojektowanie zmodyfikowanej wersji procesora w celu obniżenia kosztów całego systemu. W ten sposób powstał MOS 6510, w którym niewykorzystane wyprowadzenia obudowy DIL40 przeznaczono na linie portu wejścia-wyjścia, który zintegrowano z procesorem. Tak MOS 6510 został swojego rodzaju 8-bitowym mikrokontrolerem, a wyprowadzenia portu I/O wykorzystano np. do sterowania bankami pamięci czy komunikacji z magnetofonem kasetowym, który stanowił pamięć masową w C64.


Linia procesorów zgodnych z 6502 została użyta w innych popularnych konstrukcjach lat 70-tych i 80-tych, jak np. Apple I, Apple II i Apple III, konsole oraz 8-bitowe komputery Atari oraz kultowa konsola Nintendo NES. W tym ostatnim przypadku wykorzystano sam rdzeń procesora 6502, pozbawiony trybu arytmetyki BCD (o czym dalej), który zintegrowano w jednym układzie wraz z innymi komponentami systemu. Był on produkowany jako Ricoh 2A03/2A07 i stanowił przykład wczesnych rozwiązań znanych dziś jako SoC (ang. System On a Chip).


Firma
Model 
Liczba instrukcji
Liczba tranzystorów (przybliżona)
Rok premiery
Cena*
(przybliżona)
MOS Technology
MOS 6502
56
3500
1975
$25 (1975 r.)
Motorola
MC6800
72
6800
1975
$120 (1975 r.)
Intel
8080
78
6000
1974
$120 (1975 r.)
Zilog
Z80
158
8500
1976
$65 (1977 r.)

* dane oparte na historycznych materiałach producenta, folderach reklamowych i informacjach zaczerpniętych z Wikipedii oraz for internetowych.


Emulacja czy symulacja

Projektując system, który ma za zadanie „udawanie” jakiegoś innego systemu, stajemy przed wyborem zakresu funkcji i atrybutów oryginału, które chcemy wymodelować. Termin „symulacja” jest zwyczajowo zarezerwowany dla takich przypadków, w których odwzorowywana jest wewnętrzna struktura modelowanych obiektów. W naszym przypadku oznaczałoby to konieczność modelowania wewnętrznej mikro-architektury procesora MOS 6510 ze wszystkimi komponentami koniecznymi do jego poprawnego funkcjonowania. Można pójść dalej i modelować również pojedyncze bramki logiczne, a nawet pojedyncze tranzystory – przykładem jest projekt „visual6502” dostępny pod adresem http://visual6502.org. 

Wszystko zależy od tego, z jaką dokładnością chcemy odwzorować modelowany obiekt – w tym miejscu przebiega granica między emulacją i symulacją. Emulacja to odwzorowanie funkcjonalne danego obiektu, bez konieczności odwzorowania jego struktury wewnętrznej, która tę funkcjonalność realizuje. Emulator traktuje podmiot jako „czarną skrzynkę”, która realizuje pewne funkcje, ale nic o tym, jak ona to robi, nie wiemy, a nawet nie chcemy wiedzieć. W przypadku symulacji konieczność odwzorowania struktury wewnętrznej jest kluczowa, choć jak widać, jest to kwestia określenia granicy szczegółowości, co czasem bywa arbitralne. W przypadku omawianego emulatora tym, co chcemy emulować, jest tzw. model programowy znany jako ISA (ang. Instruction Set Architecture). Tylko komponenty i właściwości, które są istotne z punktu widzenia możliwości wykonywania kodu maszynowego, będą odwzorowane. 

Model programowy jako podzbiór mikroarchitektury pokazano na Rysunku 4.:


Rysunek 4. Diagram blokowy procesora MOS 6510


ISA – Model programowy procesora

Skrót ISA jest zazwyczaj używany jako synonim zestawu instrukcji procesora, co jest może nie do końca ścisłe, ale dostatecznie bliskie prawdy. Jest to abstrakcyjny model procesora i jego komponentów przedstawiony tak, jak jest on widziany przez instrukcje kodu maszynowego. Emulator będący przedmiotem tego artykułu ma być w stanie wykonywać binarny kod maszynowy, tak jakby był on wykonywany przez fizyczny procesor. 

	Zanim przejdziemy do bardziej usystematyzowanego opisu architektury, popatrzmy na przykładowy prosty fragment kodu:

		LDX #$00     ; zerowanie indeksu transferu
pętla:	LDA $1000,X  ; ładuj akumulator z adresu 0x1000 + x
		STA $2000,X  ; zapisz akumulator pod adresem 0x2000 + x
		INX          ; zwiększ indeks transferu o 1
		CPX #200     ; sprawdź czy przesłano już 200 bajtów
		BNE pętla    ; jeśli jeszcze nie, kontynuuj pętlę
                      
Jest to procedura przepisująca zawartość np. bufora pamięci zaczynającego się od adresu 0x1000 do bufora zaczynającego się od adresu 0x2000. Liczba bajtów do przesłania wynosi 200. Zgodnie z klasyczną konwencją asemblerów 6502 liczby szesnastkowe zapisujemy z prefiksem $, dwójkowe z prefiksem %, a bez prefiksu dziesiętne. Umieszczenie znaku # przed operandem oznacza, że jest on wartością bezpośrednio podaną w kodzie instrukcji czyli tzw. literałem (ang. literal), w przeciwnym razie operand jest traktowany jako adres komórki pamięci.

Powyższy kod jest przykładem, zachętą dla czytelnika do samodzielnego eksperymentowania. W tym celu w emulator wbudowany został prosty assembler, disassembler oraz monitor pamięci. Program można wykonywać w trybie ciągłym „Start” lub w trybie krokowym „Step”. Bardziej szczegółowy opis będzie podany w kolejnej części artykułu.

Każda instrukcja procesora ma długość od jednego do trzech bajtów. Pierwszy bajt to kod operacji (ang. op-code), który określa jej typ oraz tryb adresowania. Dodatkowe bajty zawierają wartość operandu bądź jego adres w pamięci.

	Wspomniany model programowy procesora MOS 6510 jest bardzo prosty. Zawiera tylko kilka rejestrów:

A – 8-bitowy akumulator oraz rejestr ogólnego przeznaczenia. W operacjach arytmetyczno-logicznych jest jednocześnie jednym z operandów i miejscem zapisania rezultatu operacji. Może być domyślnym argumentem dla niektórych instrukcji.


X,Y – 8-bitowe rejestry indeksowe oraz ogólnego przeznaczenia. W niektórych trybach adresowania wykorzystywane jako przesunięcie względem adresu bazowego. Rejestr X jest używany do transferu danych z/do rejestru stanu P.


P – 8-bitowy (logicznie) rejestr stanu procesora. Zawiera flagi zależne od ostatnio wykonanej operacji arytmetyczno-logicznej oraz flagi sterujące pracą procesora. Zostanie szczegółowo opisany w osobnej sekcji.


SP – 16-bitowy rejestr wskaźnika stosu. W rzeczywistości stos jest sprzętowo ograniczony do 256 bajtów w obszarze 0x0100 – 0x01FF, a SP jest de facto rejestrem 8-bitowym zawierającym offset w zakresie 0x00-0xFF.


PC – 16 bitowy rejestr licznika programu. Zawiera adres aktualnie wykonywanej instrukcji.	


 Typ Registers (registers.h) zawiera deklaracje powyższego zestawu rejestrów, typy StackPointer (stackpointer.h) i ProcessorStatus (processorstatus.h) definiują semantykę właściwą rejestrom SP i P. Ostatni z nich wymaga osobnego omówienia. Rejestr P składa się z następujących pól bitowych (flag):

C – przeniesienie (ang. carry), bit 0. Ustawiony, jeśli ostatnia operacja arytmetyczna lub logicznego przesunięcia wywołała przeniesienie z bitu 7 lub pożyczkę z bitu 8 (czyli bitu 0 bardziej znaczącego bajtu). 


Z – zero, bit 1. Ustawiony jeśli wynik ostatniej operacji wynosi zero.


I – maska przerwań, bit 2. Ustawiony powoduje, że przychodzące żądania przerwania IRQ/BRK nie są obsługiwane.


D – tryb dziesiętny (ang. decimal mode), bit 3. Ustawiony powoduje, że operacje dodawania i odejmowania traktują operandy i wynik jak liczby w formacie BCD. Tryb ten jest nieco „egzotyczny”, w praktyce mało używany, a w wersji procesora dla konsoli NES został w ogóle usunięty z rdzenia.


B – przerwanie programowe (ang. break), bit 4. W rzeczywistości nie istnieje w rejestrze P, jego odczyt daje wartość niezdefiniowaną, najczęściej 1 (zależnie od wersji procesora i technologii). Istnieje jedynie w kopii rejestru P odkładanej na stos podczas obsługi przerwania, w przypadku wykonania instrukcji BRK jest tam zapisywana wartość 1, w przypadku przerwania sprzętowego – wartość 0.


bit 5 nie jest używany.


V – przepełnienie (ang. overflow), bit 6. W arytmetyce ze znakiem (kod U2 – uzupełnień do dwóch – ang. two’s complement) bit V sygnalizuje, że 8-bitów nie jest w stanie przechować wartości działania, np.: 70 + 70 = 140, co jest poprawne bez znaku, ale w kodzie U2 jest już poza zakresem [-128,127]. 
W dodatku 14010 = 100011002 , co w kodzie U2 ma wartość -116 i jest błędnym wynikiem.


N – wartość ujemna (ang. negative), bit 7. Ustawiana kiedy wartość ostatniej operacji jest w kodzie U2 ujemna, czyli ma ustawiony najstarszy bit wyniku.


Typ ProcessorStatus zawiera metody compute...(…) obliczające wartości flag, często kilku jednocześnie dla podniesienia czytelności kodu, bazując na rezultacie operacji i/lub pozostałych operandach, jeśli jest to konieczne. Przeglądając implementacje możemy prześledzić algorytmy obliczania wartości odpowiednich flag. Same flagi zostały zaimplementowane jako indywidualne pola typu bool, co jest prostsze z punktu widzenia programisty i bardziej wydajne. Dla potrzeb składowania rejestru w formie bajtu zaimplementowano odpowiednie operatory konwertujące.


Pamięć jest jednolitą tablicą bajtów o wielkości 64 KiB zaimplementowaną jako typ Memory (memory.h). Z punktu widzenia procesora jednak pewne adresy i obszary mają specjalne znaczenie:

Obszar 0x0000 – 0x00FF – tzw. strona zerowa (ang. zero-page). Logicznie pamięć podzielona jest w procesorze MOS 6510 na strony po 256 B każda. Mamy więc 256 stron po 256 B, co daje 64 KiB. Do określenia adresu na stronie zerowej wymagany jest tylko jeden bajt, dlatego instrukcje operujące na niej są o jeden bajt krótsze. Daje to również oszczędność jednego cyklu dostępu do pamięci oraz w przypadku adresowania z indeksem brak konieczności dodawania 16-bitowego. Dostępne w strukturze procesora ALU (ang. Arithmetic-Logic Unit - jednostka arytmetyczno-logiczna) jest 8-bitowe, więc przy obliczaniu adresu z przesunięciem wykraczającym poza granicę strony dodawanie musi być wykonane w dwóch krokach. Koncepcja strony zerowej w 8-bitowym mikroprocesorze jest naprawdę błyskotliwym rozwiązaniem architektonicznym, pozwalającym na redukcję rozmiaru instrukcji i jednocześnie skracającym czas jej wykonania.


Adres 0x0000 – rejestr konfiguracji portu I/O (specyfika MOS 6510). Każdy bit odpowiada za określenie kierunku działania zintegrowanego w procesorze portu.


Adres 0x0001 – rejestr danych zintegrowanego portu I/O (specyfika MOS 6510), kierunek pracy poszczególnych bitów jest określony przez rejestr pod adresem 0x0000.


Obszar 0x0100 – 0x01FF – stos rosnący w dół, sprzętowo ograniczony do 256 B.


Adres 0xFFFA – 16-bitowy adres procedury obsługi przerwania niemaskowalnego (NMI).


Adres 0xFFFC – 16 bitowy adres procedury uruchamianej po włączeniu zasilania lub sprzętowym resecie.


Adres 0xFFFE – 16 bitowy adres obsługi przerwania maskowalnego (IRQ).


Niektóre z powyższych adresów są zdefiniowane w pliku cpudefs.h. Należy dodać, że MOS 6510 jest architekturą Little Endian, czyli kolejność zapisu bajtów w słowie (ang. endianness) jest od mniej do bardziej znaczącego, czyli tak samo, jak w procesorach Intela x86, a odwrotnie niż np. w PowerPC czy Intel Itanium, gdzie obowiązuje konwencja Big Endian. Ma to znaczenie w przypadku zapisywania np. wektorów przerwań i resetu w obszarze 0xFFFA – 0xFFFF.

	Instrukcje procesora można podzielić na kilka funkcjonalnych grup, po omówieniu ich przejdziemy do opisu trybów adresowania, czyli sposobów, w jaki instrukcja uzyskuje dostęp do swojego operandu (czy też argumentu). Metody klasy Cpu mają nazwy typu Cpu::exec<INS>(),  gdzie <INS> to trzyliterowy mnemonik. Analizując implementację można poznać działanie każdej z instrukcji:

NOP – nie wykonuj żadnej operacji. Instrukcja wykorzystywana głównie do generowania precyzyjnych opóźnień czasowych.


BRK – wywołaj przerwanie IRQ w sposób programowy. Na stos odkładany jest adres powrotu oraz rejestr P z ustawioną flagą B. Następnie ustawiana jest flaga I, co blokuje przyjmowanie kolejnych przerwań i wykonywany jest skok do adresu zapisanego w wektorze pod adresem 0xFFFE.


RTI – powrót z procedury obsługi przerwania. Ze stosu zdejmowany jest rejestr P oraz rejestr PC, flaga I jest kasowana i wykonywanie jest wznawiane od adresu zapisanego w liczniku programu PC.


JMP – skok bezwarunkowy. Rejestr PC jest ustawiany na adres podany w operandzie i wykonywanie jest kontynuowane od tego adresu.


JSR – skok do podprogramu. Na stos odkładany jest adres powrotu i wykonywany jest skok do adresu podanego w operandzie.


RTS – powrót z podprogramu. Adres powrotu jest zdejmowany ze stosu, następnie wykonywany jest skok pod ten adres.


BCC, BCS – skok, jeśli flaga C jest odpowiednio skasowana lub ustawiona.


BVC, BVS – skok, jeśli flaga V jest odpowiednio skasowana lub ustawiona.


BNE, BEQ – skok jeśli flaga Z jest odpowiednio skasowana lub ustawiona. Operacja porównania CMP dla argumentów równych sobie ustawia flagę Z. BEQ to akronim od słów Branch if Equal, czyli „skocz jeśli równe”, i oddaje sens tej instrukcji.


BPL, BMI – skok, jeśli flaga N jest odpowiednio skasowana lub ustawiona. Flaga N jest czasem określana jako S, czyli znak (ang. sign) – wtedy nazwa tej instrukcji staje się bardziej zrozumiała. BPL to akronim od Branch if Plus, czyli „skocz jeśli dodatnie”, ma to miejsce np. wtedy, gdy przy porównaniu wartość operandu nie jest większa od wartości akumulatora.


PHA, PHP – odłóż na stos odpowiednio rejestr A lub P. Zapisuje wartość rejestru pod adresem wskazanym przez SP, następnie zmniejsza SP o 1.


PLA, PLP – pobierz ze stosu rejestr A lub P. Zwiększa SP o 1, następnie ładuje rejestr wartością spod adresu wskazanego przez SP.


TXS, TSX – prześlij dane między mniej znaczącym bajtem rejestru SP a rejestrem X. Kierunek transferu jest zgodny z kolejnością rejestrów w nazwie instrukcji.


SEI, SED, SEC – ustaw flagę I, D lub C w rejestrze P.


CLI, CLD, CLC, CLV – skasuj flagę I, D, C lub V w rejestrze P.


LDA, LDX, LDY – wpisz wartość operandu do rejestru A, X lub Y.

	LDA $20,Y ;wpisz do rejestru A wartość spod adresu 0x20 + Y
	LDX #$8F  ;wpisz do rejestru X wartość 0x8F.


STA, STX, STY – zapisz wartość rejestru do pamięci.

	STX $7A,Y ;zapisz rejestr X pod adres 0x7A + Y
	STA $8000 ;zapisz rejestr A pod adres 0x8000


TAX, TAY, TXA, TYA – transfer pomiędzy rejestrami.


TAY ;prześlij wartości akumulatora do rejestru Y.


AND – oblicz iloczyn logiczny akumulatora i operandu. Wynik wpisz do akumulatora.

	AND $102A ;iloczyn logiczny akumulatora i wartości pod
;adresem 0x102A


ORA – oblicz sumę logiczną akumulatora i operandu. Wynik wpisz do akumulatora.

	ORA #$2F ;suma logiczna akumulatora i wartości 0x2F.


EOR – oblicz sumę modulo dwa (czyli wykonaj XOR) akumulatora i operandu. Wynik wpisz do akumulatora.

	EOR $8A ;suma mod. 2 akumulatora i wartości pod adresem 0x8A.


BIT – testuj bity operandu. Instrukcja działa tak jak iloczyn logiczny akumulatora i operandu, przy czym wynik nie jest wpisywany do akumulatora, lecz ustawia flagę Z. Dodatkowo flaga V przyjmuje wartość bitu 6 operandu, a flaga N wartość bitu 7 operandu. Tak ustawione flagi mogą być łatwo użyte w skokach warunkowych.


ROL, ROR – przesuń cyklicznie (obróć) operand w lewo lub w prawo.
Przy przesunięciu cyklicznym skrajny bit, wchodzący na puste miejsce, jest pobierany z flagi C, analogicznie skrajny bit wysuwany poza bajt jest zapamiętywany w fladze C.


ASL – przesuń arytmetycznie operand w lewo. Bity operandu są przesuwane w lewo, bit 0 ustawiany jest na wartość 0. Bit z pozycji 7 jest wpisywany do flagi C. Efektywnie operacja ASL jest mnożeniem operandu przez 2.


LSR – logiczne przesuń operand w prawo. Bity operandu są przesuwane w prawo, bit z pozycji 0 ustawia stan flagi C, bit 7 ustawiany jest na wartość 0. Efektywnie operacja LSR jest dzieleniem operandu przez 2, reszta z dzielenia (czyli pierwsza pozycja po przecinku) jest zapisywana w rejestrze C.


ADC – dodaj akumulator i operand z przeniesieniem. Wynik to suma akumulatora, operandu i wartości bitu C. Nie istnieje operacja dodawania bez przeniesienia, w razie potrzeby flagę C należy skasować przed wykonaniem dodawania.


SBC – odejmij operand od akumulatora z przeniesieniem (pożyczką). Wynik to różnica wartości akumulatora i operandu pomniejszona o zanegowaną wartość bitu C. MOS 6510 nie ma osobnej flagi pożyczki B (ang. borrow) i wykorzystuje negację flagi C jako wartość flagi B. Upraszcza to układ ALU i czyni operacje ADC i SBC w pełni symetrycznymi. Proste zanegowanie operandu powoduje, że instrukcja ADC funkcjonuje jak SBC, nie trzeba wyliczać wartości ujemnej w kodzie U2 – zapewnia to automatycznie zmiana interpretacji flagi C.


CMP – porównaj akumulator z operandem. Operacja wpływa na flagi Z,C i N tak samo jak odjęcie operandu od akumulatora, tylko w tym wypadku wartość odejmowania nie jest w nim zapisywana.


CPX, CPY – porównanaj rejestr X lub Y z operandem. Operacja działa jak CMP, lecz pierwszym argumentem nie jest akumulator, a odpowiednio rejestr X lub Y.


INC, INX, INY – zwiększ odpowiednio operand, rejestr X lub Y o 1


DEC, DEX, DEY – zmniejsz odpowiednio operand, rejestr X lub Y o 1


KIL – instrukcja niedozwolona. W konwencji emulatora mo65x każdy nie udokumentowany kod operacji jest uznawany za niedozwolony i powoduje wstrzymanie wykonywania (metoda Cpu::execKIL()). W praktyce jest wiele kodów, które – choć nieudokumentowane – wywołują jednak pewne, niekiedy pożyteczne, skutki. Nie będziemy się jednak nimi zajmować w tym artykule :-)

Kod instrukcji (ang. operation code – w skrócie opcode) oprócz jej typu określa również tryb adresowania i bazową liczbę cykli. Kody te są zdefiniowane jako InstructionTable (instructiontable.h), czyli tablica obiektów typu Instruction (instruction.h).


Ostatnim elementem modelu emulowanego procesora są tryby adresowania. W celu uproszczenia architektury emulatora mo65x wykonanie instrukcji zostało podzielone na dwie fazy. Pierwszą fazą wykonania każdej instrukcji jest ustawienia pól Cpu::effectiveAddress i Cpu::effectiveOperandPtr zgodnie z wybranym trybem adresowania. To daje instrukcji dostęp do faktycznego operandu, bez rozróżnienia czy jest nim rejestr, stała liczbowa czy komórka pamięci. Następujące w drugiej fazie wykonanie odpowiedniej metody Cpu::exec...(), właściwej dla danego rozkazu, wykorzystuje wartości tych pól i nie jest zależne od trybu adresowania.


W celu przyspieszenia dekodowania podczas kompilacji tworzona jest tablica DecodeTable (plik decodetable.h) , która jest tzw. look-up-table – w tym wypadku pozwalającą na uniknięcie instrukcji if/switch w czasie wykonania. Tablica DecodeTable jest zadeklarowana jako constexpr i inicjalizowana przez wyrażenie lambda wykorzystujące również tylko metody constexpr, dzięki czemu translacja z InstructionTable jest wykonana w czasie kompilacji. DecodeTable zawiera już wskaźniki do odpowiednich dla każdego kodu operacji metod przygotowania operandu Cpu::prep...Mode() oraz wykonania operacji Cpu::exec...(). Fragment metody Cpu::execute(…) pokazuje tę koncepcję w praktyce:

void Cpu::execute(bool continuous, ...) {
  state = CpuState::Running;
  while (state == CpuState::Running) {

    // pcPtr to wskaźnik na aktualnie wykonywaną instrukcję


    const auto& entry = DecodeTable[*pcPtr];
    const auto ins = entry.instruction;

    regs.pc += ins->size;
    cycles += ins->cycles;

    (this->*entry.prepareOperands)();
    (this->*entry.executeInstruction)();

     // ...

    }
    if (!continuous) break;
  }
  // ...
}

Nie dla każdej instrukcji dostępne są wszystkie tryby adresowania, prawidłowe kombinacje są zakodowane w tablicy InstructionTable, gdzie każdemu kodowi operacji przyporządkowany jest rodzaj instrukcji oraz tryb adresowania, a także bazowa liczba cykli. Bazowa, ponieważ może zostać zwiększona o dodatkowy cykl w pewnych, opisanych poniżej, przypadkach. Lista trybów adresowania przedstawia się następująco:

Tryb domyślny (ang. implied mode), metoda Cpu::prepImpliedMode(). Operand jest określony przez samą instrukcję, np. CLC oznacza kasowanie flagi C, która jest w tym momencie operandem domyślnym.


Tryb akumulatora (ang. accumulator mode), metoda Cpu::prepAccumulatorMode(). Operandem jest akumulator, w niektórych assemblerach wymagane jest dodanie litery “A” (np. ROL A) po mnemoniku, lecz jest to nadmiarowe i w assemblerze zintegrowanym w projekcie mo65x nie jest dozwolone. Przykładem może być instrukcja PLA – zdjęcie bajtu ze stosu i wpisanie do akumulatora. W tym przypadku operandem jest akumulator.


Tryb natychmiastowy (ang. immediate mode), metoda Cpu::prepImmediateMode(). Operandem jest wartość podana bezpośrednio w instrukcji (poprzedzona znakiem „#”), a w kodzie instrukcji zawarta jest w bajcie następującym po kodzie operacji. 

LDA #$3F ;wpisz do akumulatora wartość 0x3F


Tryb względny (ang. relative mode), metoda: Cpu::prepRelativeMode(). Operandem jest 8-bitowa wartość ze znakiem określająca przemieszczenie względem aktualnego adresu PC. Ten tryb adresowania jest wykorzystywany tylko przez rozkazy skoków warunkowych (ang. branch). Jeśli podczas dodawania przekroczona zostanie granica strony (czyli bardziej znaczący bajt adresu wskutek dodawania ulega zmianie) instrukcja trwa o jeden cykl dłużej. Jest to spowodowane koniecznością wykonania dodatkowego dodawania spowodowanego przeniesieniem z mniej znaczącego bajtu wyniku. Procesor ma tylko 8-bitowe ALU, dlatego obliczenie 16-bitowej sumy musi zostać rozbite na dwa kroki. Funkcjonalność metody Cpu::prepRelativeMode()została przeniesiona do procedur wykonujących skok warunkowy, sama metoda jest pusta. Faktyczne obliczenie adresu wykonywane jest tylko wtedy gdy odpowiedni warunek jest spełniony.


Tryb strony zerowej (ang. zero-page mode), metoda: Cpu::prepZeroPageMode(). Operandem jest 8-bitowy adres na stronie zerowej, czyli z zakresu 0x0000-0x00FF. Daje to zysk na skróceniu kodu rozkazu (tylko jeden bajt operandu) i tym samym na skróceniu czasu pobierania rozkazu z pamięci o jeden cykl dostępu.

ORA $20 	;oblicz sumę logiczną akumulatora i komórki
;pamięci pod adresem 0x20.


Tryb strony zerowej indeksowany (ang. zero-page mode indexed), metody odpowiednio: Cpu::prepZeroPageXMode() i Cpu::prepZeroPageYMode(). Operandem jest komórka pamięci pod adresem równym sumie 8-bitowego argumentu i wartości rejestru X lub Y. Wynik dodawania jest ograniczony do 8-bitów, reszta jest ignorowana, więc efektywny adres operandu leży również na stronie zerowej. 

LDX #$20 	;wpisz do rej. X wartość 0x20
LDA $F0,X ;zapisz akumulator pod adres 0x10


Tryb adresowania absolutnego (ang. absolute mode), metoda: Cpu::prepAbsoluteMode(). Operandem jest komórka w pamięci o podanym w instrukcji 16-bitowym adresie.

STA $80FA ;wpisz wartość akumulatora do pamięci 
;pod adres 0x80FA.


Tryb adresowania absolutnego, indeksowanego (ang. absolute mode indexed), metody to odpowiednio: Cpu::prepAbsoluteXMode() i Cpu::prepAbsoluteYMode(). Operandem jest komórka pamięci pod podanym 16-bitowym adresem powiększonym o wartość rejestru X lub Y. Jeśli podczas dodawania występuje zmiana strony adresowej, instrukcja zajmuje dodatkowy cykl procesora (jw.). 

ROL $2000,X 	;obróć logicznie zawartość komórki pamięci
;o adresie $2000 + X.


Tryb pośredni (ang. indirect mode), metoda Cpu::prepIndirectMode(). Operandem jest komórka pamięci o adresie zawartym pod adresem podanym w instrukcji. Taki adres, pod którym znajduje się inny adres, jest nazywany wektorem. W MOS 6510 adresowanie pośrednie jest wykorzystywane tylko w instrukcji skoku JMP.


tryb indeksowany pośredni (ang. indexed indirect), metoda: Cpu::prepIndexedIndirectXMode(). Operandem jest komórka pamięci o adresie podanym w wektorze na stronie zerowej. Adres wektora jest 8-bitową sumą wartości podanej w instrukcji i wartości rejestru X. Praktyczne zastosowanie to odwołanie do tablicy wektorów, która jest na stronie zerowej, a konkretny wektor jest wybierany przez podanie odpowiedniej wartości rejestru X.


; pod adresem 0x20 znajduje się 16-bitowa wartość 0x802F

	LDX #$08 		;wpisz 0x08 do rejestru X
	STA (0x18,X) 	;zapisz akumulator pod adres 0x802F


tryb pośredni indeksowany (ang. indirect indexed), metoda: Cpu::prepIndirectIndexedYMode(). Operandem jest komórka pamięci o adresie będącym sumą wektora o podanym adresie i rejestru Y. Typowym zastosowaniem jest dostęp do kolejnych danych bufora, którego adres jest zapisany gdzieś na stronie zerowej.

; pod adresem 0x30 znajduje się adres bufora źródłowego
; pod adresem 0x50 znajduje się adres bufora docelowego

       LDY #$00
pętla: LDA (0x30),Y ;pobierz bajt z bufora źródłowego
       STA (0x50),Y ;zapisz bajt do bufora docelowego
       INY
       CPY #$30  ;sprawdź czy przesłano już całe 0x30 bajtów
       BNE pętla ;jeśli nie kontynuuj pętlę



Na tym kończymy omówienie modelu programowego procesora MOS 6510. Zachęcam zainteresowanych do samodzielnego eksperymentowania i pogłębiania wiedzy z zakresu assemblera opisywanego procesora. W internecie znajduje się wiele materiałów na ten temat, głównie jednak w języku angielskim. Podstawowym źródłem informacji jest nieodmiennie http://www.6502.org.


W kolejnym odcinku cyklu zostanie opisany sam emulator, zarówno od strony programowej, jak i od strony użytkownika.
![alt text](rysunek-1.png) ![alt text](rysunek-2.png) ![alt text](rysunek-3.png) ![alt text](rysunek-4.png) ![alt text](zdjecie.jpeg)


Mikroprocesor w stylu „retro” (część 2)
W poprzedniej części artykułu omówiony został model programowy procesora MOS 6510 bez wnikania w szczegóły architektury emulatora. Dziś opisany zostanie bardziej szczegółowo sam emulator mo65x, jego wewnętrzna budowa i działanie. Repozytorium z kodem jest dostępne pod adresem: https://github.com/rjaremczak/mo65x. 

Maszyna rzeczywista czy wirtualna

Pierwszą decyzją przed rozpoczęciem prac nad projektem był wybór języka i bibliotek pomocniczych. Wybór został oparty na założeniu, które bywa podważane i jest przyczyną wielu sporów, a mianowicie na stwierdzeniu, że do napisania emulatora najbardziej odpowiedni będzie język kompilowany do kodu maszynowego i wykonywany bezpośrednio przez procesor. Wykluczone zostały języki tworzące kod uruchamiany pod kontrolą  jakichkolwiek maszyn wirtualnych. Oczywiście napisanie emulatora MOS 6510 w Javie czy nawet JavaScript jest możliwe, czego dowodem jest emulator ze strony http://www.6502asm.com, jednak pewne kluczowe dla emulacji aspekty są w takich językach trudniejsze do zaimplementowania. Te aspekty to wydajność, niska latencja i kontrola nad opóźnieniami czasowymi. Niedeterministyczna działalność Garbage Collectora generuje losowe opóźnienia czy wręcz wstrzymania wykonywania kodu (pełny cykl GC), a sama natura języków tego typu zazwyczaj skutkuje niższą wydajnością, a na pewno statystycznie większą latencją, występującą niezależnie od intencji programisty.

Jako język programowania wybrany został C++ 17, a ponadto, ze względu na wymóg wieloplatformowości, w projekcie wykorzystano bibliotekę Qt (obecnie w wersji 5.14.x). Za wyborem tej biblioteki przemawia jej dostępność na licencji LGPL, fakt, że dostarcza w miarę wygodne IDE (Qt Creator) wraz ze zintegrowanym edytorem UI (ang. User Interface – interfejs użytkownika), a także narzędzia do internacjonalizacji. Zawiera również klasy wspomagające pisanie  testów jednostkowych. Klasy biblioteki Qt zawierają wiele potrzebnych rozwiązań i zwiększają wygodę pisania programu, co w połączeniu z wykorzystaniem STL (ang. Standard Template Library – standardowa biblioteka wzorców) daje już całkiem wygodny zestaw narzędzi. 

Struktura biblioteki Qt, nazewnictwo klas i metod nawiązuje do biblioteki dostarczanej z Java SDK (np. QString - String, QVector - Vector, itd.), co może  być dodatkowym atutem dla programistów znających ten język. W kontraście do biblioteki Qt, która ma strukturę obiektową, gdzie głównie metody obiektów realizują jej funkcjonalność, stoi „generyczne” podejście stosowane w bibliotece STL. Łączenie tych dwóch bibliotek w jednym kodzie może być w związku z tym utrudnione, jednak projektanci Qt dołożyli starań, by tam, gdzie to tylko możliwe, dodać metody ułatwiające współdziałanie tych dwóch rozwiązań. Przykładem jest klasa QVector i jej metody begin(), cbegin(), end(), cend()zapewniające kompatybilność z metodami STL wymagającymi iteratorów.

Przy omawianiu kodu emulatora będą opisywane niektóre aspekty biblioteki Qt, zachęcam jednak czytelnika do samodzielnego eksplorowania dokumentacji na stronie https://doc.qt.io. Dokumentacja ta jest naprawdę dobra, zawiera wiele przykładów i była jednym z powodów wybrania tej właśnie biblioteki. Opis funkcjonowania i budowy emulatora jest z konieczności fragmentaryczny i ograniczony do wybranych obszarów. Aby poznać szczegóły implementacji poszczególnych rozwiązań, konieczna jest samodzielna analiza kodu źródłowego, a najlepiej próby jego modyfikacji i obserwowania rezultatów.

Architektura emulatora 

Emulator obsługuje interfejs użytkownika i jednocześnie intensywnie korzysta z procesora, dlatego do każdego z tych zadań wykorzystuje osobny wątek. Jeden to wątek główny, który obsługuje operacje związane z GUI (ang. Graphical User Interface – graficzny interfejs użytkownika), gdzie dla komfortu pracy liczy się responsywność. Drugi wątek realizuje obsługę samego emulatora, który w trybie egzekucji ciągłej nie opuszcza pętli pobierania i wykonywania rozkazów kodu maszynowego procesora MOS 6510. Taki podział, oprócz zapewnienia płynnego działania UI podczas emulacji, umożliwia uruchomienie wątku emulatora przez system operacyjny na osobnym rdzeniu CPU, co poprawia wydajność w wielordzeniowych systemach. Uproszczony schemat architektury emulatora pokazany jest na Rysunku 1 – dla czytelności pominięto nazwy zmiennych i pozostawiono tylko nazwy klas (typów).



Rysunek 1. Uproszczony schemat architektury z uwidocznionym podziałem na wątek główny oraz dodatkowy wątek obsługujący emulację

Cała interakcja z użytkownikiem i wszystkie operacje graficzne muszą być wykonane w wątku głównym programu. Takie ograniczenie narzucają wszystkie wspierane systemy operacyjne. Wymusza to całkowitą separację warstwy prezentacji od warstwy emulacji.


Rysunek 2. Przykładowy plik źródłowy w trybie asemblera, uruchomiony i zastopowany. Po lewej stronie widać wygenerowany obraz

Zaznaczone i ponumerowane obszary interfejsu użytkownika, pokazane na Rysunku 2,  mają następujące funkcje:


Obszar pamięci interpretowany jako obraz o wymiarach 32x32 piksele.
CentralWidget zawierający mechanizm wyboru widoku.
Aktualnie wybrany widok, tutaj AssemblerWidget.
CpuWidget, obszar pokazujący stan procesora i kontrolujący wykonywanie.

Podstawowe koncepcje biblioteki Qt

Podstawową klasą w modelu obiektowym biblioteki Qt jest klasa QObject. Łączy ona w sobie kilka kluczowych funkcjonalności, z których najważniejszymi w kontekście tego projektu są:

możliwość deklarowania tzw. sygnałów i slotów – jest to charakterystyczny dla biblioteki Qt mechanizm komunikacji między obiektami. Metoda zadeklarowana jako signal może być za pomocą metody QObject::connect podłączona do innej metody, zadeklarowanej jako slot. Konkretny przykład takiego rozwiązania zostanie pokazany przy okazji omawiania klasy Emulator. Każdorazowe wywołanie sygnału, czyli zwykłe wywołanie odpowiedniej metody, zwyczajowo poprzedzone makrem emit, powoduje wywołanie podłączonego slotu (lub slotów) wraz z przekazaniem parametrów. W przypadku gdy obiekt wysyłający sygnał jest przyporządkowany do tego samego wątku, co odbierający go slot, wywołanie standardowo jest synchroniczne i natychmiastowe. Jeśli komunikacja zachodzi między różnymi wątkami, połączenie jest domyślnie realizowane poprzez kolejkę komunikatów wątku docelowego. Metoda realizacji połączenia może zostać określona jako bezpośrednia, poprzez kolejkę komunikatów, z blokowaniem (synchronicznie) lub bez (asynchronicznie). Jest to definiowane indywidualnie dla każdej pary sygnał-slot podczas wywołania metody QObject::connect. Parametry połączenia są sprawdzane w czasie wykonania, więc mogą być dynamicznie zmieniane.


obiekt dziedziczący po QObject może być obsługiwany przez kolejkę komunikatów (ang. message loop) wątku, do którego jest przypisany, i odbierać komunikaty i zdarzenia, które mogą pochodzić od obiektów przypisanych do innych wątków. Najprostszym sposobem na „zlecenie” wykonania jakiegoś zadania w wątku roboczym (ang. worker thread) jest wysłanie sygnału do obiektu, który jest do niego przypisany, co spowoduje obsługę tego sygnału przez połączony z nim slot. Zapewnia to jednocześnie synchronizację i szeregowanie zadań po stronie wykonawczej (odbiorczej), ponieważ żądania są obsługiwane przez kolejkę, jedno po drugim w kolejności zgłoszeń. W ten sposób na przykład jest realizowana komunikacja między obiektami UI (ang. User Interface – interfejs użytkownika) i obiektem emulatora.


podczas emitowania sygnału poprzez kolejkowane połączenie sygnał-slot między różnymi wątkami, Qt dokonuje serializacji i deserializacji argumentów wywołania. Jeśli argument nie jest jednym z typów, które biblioteka Qt potrafi domyślnie serializować, jego typ musi zostać odpowiednio zarejestrowany. Służą do tego makra specjalnego preprocesora, tak zwanego MOC (ang. Meta Object Compiler). Odpowiada on między innymi za generowanie kodu realizującego komunikację sygnał-slot i umożliwia introspekcję obiektów dziedziczących po QObject. Deklaracja i rejestracja typu, który ma być serializowany, musi znaleźć się przed jego pierwszym użyciem. Bardziej szczegółowy opis znajduje się poniżej, w opisie kodu startowego.


obiekty dziedziczące po QObject tworzą hierarchię, w której „rodzic” jest odpowiedzialny za kontrolę cyklu życia swoich „dzieci”. Przypisując obiekt do innego obiektu-rodzica, nie musimy usuwać go z pamięci instrukcją delete, rodzic to zrobi, kiedy sam będzie usuwany. Jest to funkcjonalność podobna do wzorca projektowego RAII (ang. Resource Acquisition Is Initialisation – inicjalizacja zasobu przy jego uzyskaniu), którą Qt stworzyło samodzielnie, jeszcze przed wprowadzeniem standardu C++11 i inteligentnych wskaźników. W przypadku obiektów biblioteki Qt związanych z UI hierarchia obiektów odzwierciedla również ich hierarchię na ekranie. Biblioteka wymusza, żeby obiekt i jego dzieci były przypisane do tego samego wątku. Dodatkowo obiekt, który ma już rodzica, nie może zmienić wątku, do którego jest przypisany. Ma to wpływ na sposób inicjalizacji obiektu klasy Emulator i jego skonfigurowania do pracy w wątku roboczym.

Połączenia sygnał-slot mogą być blokowane i odblokowywane w dowolnym momencie. Ta właściwość jest wykorzystana podczas pracy emulatora w trybie wykonywania ciągłego. Dla wygody wykorzystano do tego celu biblioteczną klasę QSignalBlocker.

Kod startowy aplikacji, plik main.cpp

Standardowo punktem startowym aplikacji C++ jest instrukcja main w pliku main.cpp. Typy danych, które nie są typami fundamentalnymi ani standardowymi typami biblioteki Qt, a biorą udział w komunikacji za pomocą sygnałów i slotów, należy zadeklarować za pomocą makra Q_DECLARE_METATYPE. Poniżej przykładowa deklaracja, umieszczona tuż po deklaracjach #include

Q_DECLARE_METATYPE(Address)
Q_DECLARE_METATYPE(AddressRange)

Address jest aliasem (inną nazwą) typu uint16_t. Został zadeklarowany w pliku commondefs.h jako jeden ze specyficznych dla projektu i używanych w wielu miejscach typów. Należy zauważyć, że o ile typy fundamentalne (ang. primitive) są domyślnie obsługiwane przez Qt, to typ Address, który de facto jest aliasem do typu fundamentalnego unsigned short int, pomimo to musi zostać zarejestrowany.

// wymagane podanie tekstowego id

qRegisterMetaType<Address>("Address");

// w tym przypadku nie ma takiej potrzeby

qRegisterMetaType<AddressRange>(); 

Dodatkowo podczas rejestracji typów będących aliasami typów fundamentalnych wymagane jest explicite podanie ich identyfikatorów tekstowych. Nie mogą one zostać wydedukowane automatycznie przez preprocesor na podstawie ich nazw. Dla pozostałych typów taka dedukcja, jak widać na przykładzie kodu powyżej, działa bez problemu.
Ta niespójność bywa powodem frustracji początkujących użytkowników biblioteki i była zgłaszana do Qt jako błąd.

Kolejnym krokiem jest ustalenie wyglądu UI i załadowanie pliku ze stylami, modyfikującymi standardowy look & feel:

QApplication::setStyle(QStyleFactory::create("Fusion"));
QFile qss(":/main.qss");
qss.open(QFile::ReadOnly);
app.setStyleSheet(qss.readAll());

Plik main.qss zawiera modyfikacje oryginalnego stylu "Fusion" w języku bardzo podobnym do CSS. Składnia definicji stylów jest dosyć dobrze udokumentowana z podaniem przykładów. Po bardziej szczegółowy opis dobrze jest zajrzeć pod adres: https://doc.qt.io/qt-5/stylesheet.html.  Sam plik main.qss jest umieszczony w swojego rodzaju wewnętrznym systemie plików, osadzonym w aplikacji, który pozwala na dostęp do zasobów w sposób niezależny od platformy. Ścieżka do pliku z definicją zasobów (ang. resources), w naszym przypadku resources.qrc, jest zdefiniowana w głównym pliku projektu mo65x.pro. Jest to plik wykorzystywany przez narzędzie qmake do generowania skryptów i konfiguracji, które faktycznie realizują kompilację i konsolidację projektu na każdej ze wspieranych przez Qt SDK platform.

Kolejnym istotnym krokiem jest zdefiniowanie obsługi zdarzenia związanego z zamykaniem aplikacji. W tym celu definiujemy połączenie sygnału aplikacji ze slotem okna głównego:

QObject::connect(&app, &QCoreApplication::aboutToQuit, &mainWindow, &MainWindow::prepareToQuit);

Zawsze przed zamknięciem aplikacji zostanie wywołany slot MainWindow::prepareToQuit, który ma za zadanie zakończenie w sposób kontrolowany dodatkowego wątku klasy Emulator. Bez takiego rozwiązania wątek dodatkowy mógłby nie zawsze zostać poprawnie zakończony, co skutkowałoby „zawieszeniem się” aplikacji i koniecznością jej siłowego zamknięcia z poziomu systemu operacyjnego.

Okno główne, klasy MainWindow i CentralWidget

Podczas uruchamiania okna głównego aplikacji (konstruktor klasy MainWindow), tuż po zainicjalizowaniu UI, odczytywany jest plik konfiguracji, za co odpowiada metoda MainWindow::initConfigStorage. Konfiguracja jest odczytywana z pliku ~/.mo65x/config.json lub jeśli plik nie istnieje, tworzony jest z domyślnymi wartościami. Aktualnie zapisywana jest w nim tylko ścieżka do ostatnio otwartego pliku z kodem źródłowym, wyświetlanym w oknie asemblera. Obiektem DTO (ang. Data Transfer Object – obiekt przenoszący dane) jest obiekt klasy Config (plik config.h). Zaimplementowane są w nim metody serializacji i deserializacji:

void read(const QJsonObject& json);
void write(QJsonObject& json) const;

wymagane przez klasę szablonową FileDataStorage, która odpowiada za przechowywanie zawartości obiektu w pliku.

Kolejnym krokiem jest utworzenie instancji klasy Emulator i przygotowanie jej do pracy, za co odpowiada metoda MainWindow::startEmulator. Obiekt emulatora jest tworzony bez rodzica i następnie przypisany do osobnego wątku:

  emulator = new Emulator();
  emulator->moveToThread(&emulatorThread);

Sygnał finished wątku roboczego zostaje połączony ze slotem deleteLater, który jest zaimplementowany w klasie bazowej QObject. Takie połączenie zapewnia poprawne zniszczenie obiektu podczas zakończenia wątku, do którego został przypisany.

    connect(&emulatorThread, &QThread::finished, emulator, &Emulator::deleteLater);

Następnie wątek jest alokowany na poziomie systemu operacyjnego i domyślnie uruchamiana jest jego pętla obsługi komunikatów:

  emulatorThread.start();

Wątkowi roboczemu zostaje nadana krótka nazwa ułatwiająca identyfikację podczas debugowania:

  emulatorThread.setObjectName("emulator");

Aplikacja wykorzystuje jeden z najbardziej popularnych szablonów UI, składający się z okna centralnego, otoczonego przez strefy, w których mogą się znajdować okna dokowalne (rozszerzające QDockWidget), a także paska statusu u dołu okna głównego. Opcjonalny pasek narzędzi oraz pasek menu nie jest w tym przypadku wykorzystywany.

Po uruchomieniu aplikacji definiowane są połączenia między sygnałami i slotami różnorodnych obiektów UI i obiektem emulatora. Należy zwrócić uwagę na wywołania metody QObject::connect z argumentem type równym Qt::DirectConnection. Ma to na celu ustanowienie połączenia bezpośredniego, synchronicznego, realizowanego za pomocą prostego wywołania slotu. Istotne jest to, że takie połączenie jest ustanawiane pomiędzy obiektami przypisanymi do różnych wątków, standardowo w takim przypadku stosuje się połączenie poprzez kolejkę komunikatów. W tym przypadku jednak ominięcie kolejki komunikatów obiektu docelowego, która podczas emulacji ciągłej nie jest aktywna, jest konieczne, żeby w ogóle jakikolwiek sygnał został obsłużony.

Następnie wczytywany jest ostatnio otwarty plik z kodem źródłowym, jego ścieżka pobierana jest z danych konfiguracji. Dalej inicjalizowany jest adres pamięci obrazu. Na końcu wywołana jest metoda MainWindow::propagateState, co wymusza odświeżenie stanu interfejsu użytkownika i aplikacja jest gotowa do działania.

Klasa Emulator

Główną funkcjonalność aplikacji realizuje klasa Emulator i powiązane z nią klasy Memory oraz Cpu. Klasa Emulator dziedziczy po QObject i jest uruchomiona na osobnym wątku roboczym. Jest ona swojego rodzaju pośrednikiem między interfejsem użytkownika a realizującymi emulację klasami Cpu i Memory. 

Sygnały, które może generować klasa Emulator, są następujące:

void stateChanged(EmulatorState)
stan emulatora został zmieniony.

void memoryContentChanged(AddressRange)
komórki pamięci w podanym zakresie mogły zostać zmienione.


void operationCompleted(const QString& m, bool ok)
zlecona operacja została zakończona,
m – komunikat tekstowy, status zakończonej operacji,
ok – flaga określająca, czy operacja zakończyła się sukcesem.

Sloty przeznaczone do wywoływania z wątku głównego poprzez kolejkę komunikatów, czyli w sposób domyślny:

void execute(bool continuous, Frequency clock)
rozpocznij wykonywanie programu od aktualnego adresu PC,
continuous – true – tryb ciągły, false – pojedynczy rozkaz,
clock – szybkość symulowanego zegara w cyklach na sekundę.

void changeProgramCounter(Address)
zmień wartość licznika programu PC.


void changeStackPointer(Address)
zmień wartość wskaźnika stosu SP.

void changeAccumulator(uint8_t)
zmień wartość rejestru A czyli akumulatora.

void changeRegisterX(uint8_t)
zmień wartość rejestru X.

void changeRegisterY(uint8_t)
zmień wartość rejestru Y.

void changeMemory(Address a, uint8_t v)
wpisz wartość v do komórki o adresie a.

void loadMemory(Address a, const Data& data)
załaduj dane binarne data do pamięci, począwszy od adresu a.

void loadMemoryFromFile(Address a, const QString& fn)
załaduj zawartość pliku binarnego fn do pamięci, począwszy od adresu a.

void saveMemoryToFile(AddressRange range, const QString& fn)
zapisz zawartość pamięci z zakresu range do pliku fn w postaci binarnej.

Sloty przeznaczone do wywoływania w trybie połączenia bezpośredniego, ponieważ mają wpływ na proces emulacji, który w trybie ciągłym blokuje obsługę kolejki komunikatów:

void triggerIrq()
żądanie obsługi przerwania maskowalnego.

void triggerNmi()
żądanie obsługi przerwania niemaskowalnego.

void triggerReset()
żądanie symulowanego resetu sprzętowego.

void stopExecution()
żądanie wstrzymania trybu wykonywania ciągłego.

void clearStatistics()
wyczyszczenie statystyk czasu wykonania i ilości cykli maszynowych.

Jeśli w wyniku wywołania slotu zostaje zmieniony stan emulatora, czyli wartości rejestrów procesora (typ Registers), jego stan (typ CpuState) czy tryb pracy (typ CpuRunLevel), zostanie wyemitowany sygnał Emulator::stateChanged i/lub Emulator::memoryContentChanged. Daje to możliwość aktualizacji interfejsu użytkownika.

W trybie emulacji ciągłej odświeżenie interfejsu użytkownika nie odbywa się po wykonaniu każdego rozkazu. W celu uniknięcia generowania zbyt dużej ilości wywołań sygnał-slot aktualizacja UI jest dokonywana ze stałą częstotliwością, dużo mniejszą niż po każdej wykonanej instrukcji, co z przyczyn praktycznych i tak nie miałoby sensu. Do tego celu służy timer MainWindow::pollTimer. Wywołuje on co 40 ms (czyli 25 razy na sekundę) metodę MainWindow::polling, która z kolei w trybie wykonywania ciągłego wywołuje metodę MainWindow::propagateState.

Klasa Cpu

Kluczową klasą, realizującą właściwą emulację, jest klasa Cpu. Aktualny stan pracy procesora trzymany jest w prywatnej zmiennej Cpu::state (typ CpuState), która może przyjąć jedną z następujących wartości:

Idle – stan bezczynności, np. po wykonaniu pojedynczej instrukcji.
Stopped – zatrzymany, poprzednio w trybie ciągłym Running.
Halted – zawieszony, próba wykonania instrukcji niedozwolonej.
Running – pracuje w trybie ciągłym.
Stopping – zlecono zatrzymanie pracy ciągłej, przechodzi w stan Stopped po wykonaniu bieżącej instrukcji.

Zmienna Cpu::state może zostać zmodyfikowana przez więcej niż jeden wątek, z tego powodu, dla zapewnienia atomowego dostępu, została zdefiniowana jako typ wyliczany, bazujący na typie uint8_t, czyli pojedynczym bajcie. Bajt to najmniejsza porcja danych, jaka może być zapisana w pamięci, włączając w to pamięć cache. W związku z tym dostęp do danych o rozmiarze bajtu musi być atomowy. Alternatywą byłoby wykorzystanie wrappera gwarantującego taki dostęp, czyli np. deklarując zmienną Cpu::state jako std::atomic<CpuState>, wydaje się to jednak w tym przypadku niepotrzebną komplikacją.

Kolejną ważną zmienną kontrolującą wykonywanie jest Cpu::runLevel (typ CpuRunLevel). Określa ona aktualny tryb wykonywania i przyjmuje następujące wartości:

Normal – wykonywanie kodu maszynowego.
PendingIrq – zgłoszone zostało przerwanie maskowalne.
PendingNmi – zgłoszone zostało przerwanie niemaskowalne.
PendingReset – zgłoszone zostało żądanie wykonania resetu sprzętowego.

Wylistowane wartości są uszeregowane narastająco, według priorytetu. Priorytet może zostać tylko podniesiony, co daje pewność, że tylko zgłoszenie o większej ważności może przerwać zgłoszone wcześniej, mniej ważne.

Najważniejsze zmienne i metody publicznego interfejsu klasy Cpu to:

Registers regs
wszystkie rejestry procesora łącznie z rejestrem stanu P.


bool running() const
zwraca true, jeśli procesor jest w trakcie wykonywania instrukcji programu.


void reset()
wykonuje symulowany reset sprzętowy.


void resetExecutionState()
przywraca domyślny stan procesora, o ile nie jest aktualnie w trakcie wstrzymywania egzekucji lub nie jest „zawieszony” jak przy próbie wykonania instrukcji niedozwolonej.


void resetStatistics()
zeruje licznik wykonanych instrukcji i czasu spędzonego na ich wykonywaniu.


void stopExecution()
rozpocznij wstrzymywanie, jeśli procesor aktualnie wykonuje instrukcje.


void execute(bool cont, Duration period = Duration(1000))
rozpocznij wykonywanie w sposób ciągły (cont = true) lub w trybie krokowym (cont = false). Jeśli wykonywanie pojedynczej instrukcji będzie krótsze niż period (w nanosekundach), to wątek jest wstrzymywany przez odpowiedni czas. Domyślnie period = 1000 ns, czyli 1 μs, co odpowiada zegarowi o częstotliwości 1 MHz. Aby wyjść z pętli w trybie ciągłym, inny wątek musi wywołać metodę Cpu::stopExecution, która zmieniając wartość prywatnej zmiennej Cpu::state, wymusi zakończenie pętli.


void triggerReset()
w trybie ciągłym zgłoszone jest żądanie wykonania resetu, które zostanie zrealizowane w pętli emulacji. W trybie krokowym reset wykonywany jest natychmiast.

void triggerNmi()
o ile nie zlecono już resetu, w trybie ciągłym zgłoszone jest żądanie wykonania przerwania niemaskowalnego, które zostanie wykonane przez pętlę emulacji. W trybie krokowym NMI wykonywane jest natychmiast.

void triggerIrq()
o ile nie zlecono już żądania resetu lub NMI, w trybie ciągłym zgłoszone jest żądanie wykonania przerwania maskowalnego, które zostanie wykonane przez pętlę emulacji. W trybie krokowym IRQ wykonywane jest natychmiast.

CpuInfo info() const
zwraca chwilową kopię (ang. snapshot) struktury CpuInfo zawierającej statystyki emulowanego CPU oraz aktualny stan i tryb pracy.

Klasa Memory

Obiekt klasy Memory, który jest zmienną w klasie Emulator, reprezentuje pamięć operacyjną procesora o wielkości 64 KiB. Wewnętrzna reprezentacja pamięci to prosta tablica bajtów o rozmiarze 65536 elementów. Oprócz operatorów indeksowych dających bezpośredni dostęp do elementów tablicy zostały zdefiniowane następujące metody ułatwiające korzystanie z obiektu:

begin(), end(), cbegin(), cend()
metody pozwalające wygodnie używać obiektów Memory z funkcjami STL operującymi na iteratorach, zwracają odpowiednie iteratory wewnętrznej tablicy bajtów.


size_t size() 
zwraca rozmiar tablicy w bajtach.


uint16_t word(Address), void setWord(Address, uint16_t)
odczyt i zapis 16-bitowego słowa z zachowaniem konwencji Little-Endian.

Panel kontrolny procesora, klasa CpuWidget

Jest to dokowalne okno prezentujące pełny stan emulowanego procesora, wartości wszystkich jego rejestrów, które za wyjątkiem rejestru stanu P można również modyfikować. Dodatkowo udostępnione są aktualne wartości komórek pamięci przechowujących wektory przerwań NMI (ang. Non Maskable Interrupt – przerwanie niemaskowalne), IRQ (ang. Interrupt ReQuest – żądanie przerwania), procedury obsługującej reset oraz rejestry dodanego w MOS 6510 portu wejścia/wyjścia.

Za pomocą przycisków „NMI”, „RST” i „IRQ” można wywołać odpowiednie przerwanie lub zasymulować sprzętowy reset. Pole „Clock Frequency” umożliwia wprowadzenie prędkości zegara, którą emulator ma za zadanie utrzymać. Przy podaniu zbyt dużej prędkości może nie udać się jej uzyskać z powodu zbyt niskiej wydajności systemu.

Poniżej umieszczone jest pole tekstowe wyświetlające statystyki procesora. W kolorze szarym dla ostatnio wykonanej instrukcji w trybie krokowym, lub sesji wykonywania ciągłego. W kolorze białym wartości skumulowane od ostatniego wyzerowania statystyk.

U dołu umieszczony jest widok monitora kodu (disassemblera) pokazujący aktualnie wykonywane instrukcje, a pod nim następujące przyciski:

Step – tryb krokowy, wykonanie pojedynczej instrukcji.
↷ – przeskok do następnej instrukcji bez jej wykonywania.
Start – rozpoczęcie wykonywania ciągłego.
Stop – przerwanie wykonywania ciągłego.
Clear – wyzerowanie statystyk procesora.

Widok asemblera, klasa Assembler i AssemblerWidget

Dla wygody użytkownika z emulatorem został zintegrowany prosty asembler wraz z edytorem. Za parsowanie linii kodu źródłowego oraz generowanie kodu maszynowego odpowiada klasa Assembler. Do analizy pojedynczych linii wykorzystywane są wyrażenia regularne (regex), klasa QRegularExpression jest odpowiedzialna za kompilowanie tekstowej reprezentacji wyrażenia regularnego oraz właściwe dopasowywanie wzorca do podanego tekstu, natomiast klasa QRegularExpressionMatch reprezentuje wynik procesu dopasowywania. Obie te klasy stanowią część biblioteki Qt.

Lista wyrażeń regularnych reprezentujących prawidłowe linijki kodu, wraz z przyporządkowanymi im procedurami obsługi, jest zapisana w statycznej tablicy Assembler::Patterns[]. Każda prawidłowa linia kodu składa się z opcjonalnej etykiety, opcjonalnego kodu rozkazu lub instrukcji oraz opcjonalnej listy argumentów. Poszczególne komponenty są dla czytelności zdefiniowane jako odpowiednie stałe: Symbol, Label, Comment itd. Analizując kod umieszczony w pliku assembler.cpp, możemy poznać szczegóły składni wspieranej przez tę klasę. Przykładowe kody źródłowe można znaleźć w repozytorium projektu, w katalogu /asm lub np. na stronie http://www.6502asm.com.


Oprócz instrukcji procesora i możliwości deklarowania symboli za pomocą etykiety dostępne są następujące komendy:

.ORG lub =*
ustaw licznik lokacji generowanego kodu.


.BYTE lub DCB
wpisz bezpośrednio bajty podane po rozkazie.


.WORD
wpisz bezpośrednio słowa (konwencja Little-Endian) podane po rozkazie.

Separatorem wartości podawanych jako listy jest przecinek. Klasa Assembler jest konstruowana z podaniem referencji do obiektu Memory, który jest wykorzystywany jako miejsce docelowe generowanego kodu. Standardowa sekwencja składa się z fazy definiowania symboli oraz, następującej po niej, fazy generowania kodu:

// inicjalizacja, przygotowanie asemblera do pracy
// licznik lokacji ustawiany na 0x0000
// czyszczenie tablicy symboli


Assembler::init()


// przejście w tryb wypełniania tablicy symboli
// kod nie jest generowany

Assembler::changeMode(Assembler::ProcessingMode::ScanForSymbols)


// przetwarzanie linii kodu
// zwracany status jest wartością typu AssemblyResult
// należy powtórzyć dla wszystkich linii kodu


Assembler::processLine(const QString&)


// inicjalizacja, bez czyszczenia tablicy symboli

Assembler::initPreserveSymbols()


// zmiana trybu pracy na generowanie kodu
// w przypadku gdy używany symbol nie jest zdefiniowany,
// generowany jest błąd AssemblyResult::SymbolNotDefined


assembler.changeMode(Assembler::ProcessingMode::EmitCode)


// ponowne przetwarzanie wszystkich linii kodu
// generowany kod jest bezpośrednio zapisywany do pamięci
// należy powtórzyć dla wszystkich linii kodu

Assembler::processLine(const QString&)


Klasa AssemblerWidget jest wizualnym interfejsem dla klasy Assembler, udostępniającym następujące funkcje:

New – kasowanie zawartości edytora.
Load – odczyt z pliku.
Save – zapis lub aktualizacja otwartego pliku.
SaveAs – zapis do pliku z możliwością podania nazwy.
Assemble – kompilacja całego pliku.
→ – ustawienie PC na początek wygenerowanego kodu.

Monitor kodu, klasy Disassembler, DisassemblerView,  DisassemblerWidget

Disassembler przeprowadza proces odwrotny do asemblera, z kodu maszynowego tworzy tekstową reprezentację kodu, zrozumiałą dla człowieka. Przetwarzanie kodu maszynowego na kod asemblera zaimplementowana jest w klasie Disassembler, której konstruktor wymaga podania referencji do obiektu Memory. Główne metody klasy  Disassembler są następujące:

void setOrigin(Address)
ustawia adres startowy kodu.


QString disassemble()
zwraca tekst odpowiednio sformatowany, gotowy do prezentacji. Zawiera heksadecymalną reprezentację kodu maszynowego, a po nim mnemonik wraz z argumentami w odpowiednim dla rozkazu i trybu adresowania formacie.


void nextInstruction()
bieżący adres kodu jest zwiększany o odpowiednią wartość, tak by wskazywał kolejną instrukcję.


Address currentAddress() const
zwraca aktualny adres kodu.


Powyższy proces jest powtarzany aż do osiągnięcia założonego adresu końcowego. 

Widok ten, zwany również monitorem kodu maszynowego, jest wykorzystany w projekcie dwukrotnie, raz jako niezależny element UI (ang. widget) oraz jako część widoku CpuWidget. W tym celu został wydzielony jako osobny komponent – klasę DisassemblerView dziedziczącą po QWidget. Umożliwia to swobodne osadzenie monitora kodu w dowolnej hierarchii okien. 

Klasa DisassemblerWidget jest nadrzędnym elementem UI, który ma na celu opakowanie widoku DisassemblerView i udostępnienie go wraz z następującymi funkcjami:

Start – kontrolka pozwalająca na ustawienie adresu początkowego.
☉ – ustawia adres początkowy na aktualną wartość licznika rozkazów PC.
→ – ustawia licznik rozkazów PC na aktualną wartość adresu początkowego.


Linia prezentująca adres równy aktualnej wartości PC jest podświetlona, co pozwala  na bardziej wygodne śledzenie pracy emulatora, zarówno w trybie krokowym, jak i ciągłym.

Podgląd pamięci, klasa MemoryWidget

Klasa ta realizuje klasyczny, heksadecymalny podgląd pamięci z możliwością podania adresu startowego, zapisu obszaru od „Start” do „End” do pliku, a także odczyt z pliku pod podany adres startowy „Start”. 

Widok jest odświeżany przez wywołanie slotu  MemoryWidget::updateOnChange(AddressRange), który sprawdza, czy podany zakres występuje w aktualnie wyświetlanym widoku. Tylko wtedy odświeżanie jest rzeczywiście wykonywane, za co odpowiada wywołanie metody MemoryWidget::updateView().

Podgląd pamięci obrazu, klasa VideoWidget

Dla podniesienia atrakcyjności emulatora oraz zachowania kompatybilności z przykładowymi programami pochodzącymi ze strony http://www.6502asm.com do projektu został dodany prosty komponent interpretujący blok pamięci o wielkości 1024 bajtów jako pamięć obrazu o rozdzielczości 32 x 32 piksele. Jeden piksel jest reprezentowany przez jeden bajt w trybie indeksowanego koloru, przy założeniu, że tylko 4 mniej znaczące bity są wykorzystywane. Dostępna jest więc paleta 16 kolorów. 

Właściwe renderowanie obrazu realizuje klasa ScreenWidget, a konkretnie jej implementacja metody paintEvent(QPaintEvent*). Paleta jest zapisana w zmiennej QVector<QRgb> ScreenWidget::colorTable i jest zainicjalizowana wartościami kolorów używanymi w Commodore C64. Realizuje to metoda ScreenWidget::setColorTableToC64Palette(). Jeden bajt na piksel definiuje 256 różnych kolorów, dlatego metoda ta dodatkowo kopiuje pierwsze 16 definicji do kolejnych elementów tabeli kolorów, powielając je. W efekcie efektywny wybór ogranicza się do 4 mniej znaczących bitów.

Do rysowania mapy bitowej na docelowym obszarze okna został wykorzystany obiekt QImage z biblioteki Qt. Pozwala on na zinterpretowanie podanego obszaru pamięci jako obrazu z indeksowanym kolorem o zadanej rozdzielczości. Fragment metody renderującej obraz ScreenWidget::paintEvent wygląda następująco:

// utwórz obiekt QPainter dla bieżącego kontekstu okna

QPainter painter(this); 

// utwórz obiekt QImage dla danych 
// wskazywanych przez frameBuffer
// o rozdzielczości resolutionX na resolutionY
// oraz o 8-mio bitowym, indeksowanym kolorze

QImage image(frameBuffer, resolutionX, resolutionY, QImage::Format_Indexed8);

// ustaw aktualną tablicę kolorów

image.setColorTable(colorTable);

// rysuj obraz w oknie bieżącym
// jeśli rect() jest większe niż rozdzielczość obrazu
// źródłowego obiekt painter dokona odpowiedniego skalowania

painter.drawImage(rect(), image);

Rozmiar okna klasy ScreenWidget został ustawiony w edytorze UI na stałą wartość: 128 x 128 pikseli, co skutkuje czterokrotnym powiększeniem każdego piksela obrazu źródłowego.

Testy jednostkowe

W celu uproszczenia pliku konfiguracji mo65x.pro i uniknięcia generowania dwóch osobnych plików wynikowych zintegrowano kod testujący z testowanym kodem projektu. Różnica między tworzeniem aplikacji projektu a aplikacji testującej sprowadza się tylko do wyboru odpowiedniej funkcji main, z pliku /main.cpp lub test/main.cpp, co realizowane jest  przez następujący fragment kodu konfiguracji z pliku mo65x.pro:

test {
  TARGET = $${TARGET}_tests
  SOURCES -= main.cpp
  SOURCES += test/main.cpp
}

Aby przełączyć się w tryb uruchamiania testów jednostkowych, wystarczy dodać do zmiennej CONFIG wartość test, co można zrobić w polu Additional Arguments w sekcji Build Steps konfiguracji projektu. 

Do testów wykorzystana została biblioteka QTest, a właściwy kod uruchamiający zestaw klas testujących wygląda następująco:

int main(int argc, char** argv) {
  QApplication app(argc, argv);

  AssemblerTest assemblerTest;
  InstructionsTest opCodesTest;
  FlagsTest flagsTest;

  return QTest::qExec(&opCodesTest, argc, argv) | 
    QTest::qExec(&assemblerTest, argc, argv) | 
    QTest::qExec(&flagsTest, argc, argv);
}

Jak widać, sprowadza się do utworzenia instancji obiektów zawierających zestawy testów, czyli AssemblerTest, InstructionsTest i FlagsTest, a następnie wywołanie ich po kolei. Zastosowanie operatora |, czyli binarnego OR, zapobiega optymalizacji stosowanej przez kompilator, która mogłaby wyeliminować ewaluacje kolejnych testów po sukcesie pierwszego z nich.

Mam nadzieję, że zaprezentowany projekt okaże się pomocny w tworzeniu własnych, wieloplatformowych aplikacji desktopowych, w nowoczesnym języku C++ i w oparciu o bibliotekę Qt. A przede wszystkim pomoże pokonać barierę w przejściu od  języka tworzącego kod dla maszyny wirtualnej do języka kompilowanego do kodu maszynowego. Co moim zdaniem nie jest tak trudne, a otwiera zupełnie nowe możliwości :-)
