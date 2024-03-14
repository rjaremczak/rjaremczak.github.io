32 bity: CISC vs. RISC
Począwszy od pierwszej 4-bitowej konstrukcji, rozwój mikroprocesorów przebiegał zasadniczo dwiema niezależnymi ścieżkami. Pierwsza koncentruje się wokół hardware’u i zaimplementowaniu w nim jak największej liczby funkcji. Druga zorientowana jest na software, ponieważ oprogramowanie, w odróżnieniu od sprzętu, można zaktualizować. Dlatego sprzęt powinien realizować tylko to, co musi, oraz co – ze względu na wydajność – powinien.

W poprzednich odcinkach…

Wprowadzony w pierwszym cyklu współczynnik nazwany „efektywnością architektury” w przypadku konstrukcji 32-bitowych z przetwarzaniem potokowym (ang. pipelining) nie jest doskonałym wskaźnikiem pokazującym wydajność rdzenia. Szeroko dostępne, standaryzowane benchmarki sprawdzą się w tym przypadku lepiej, nawet standardowy, uśredniony współczynnik CPI (ang. cycles per instruction – liczba cykli na instrukcję) czy IPS (ang. instructions per second – liczba instrukcji na sekundę) pozwoli na lepsze porównanie wydajności procesorów. Zdaniem autora jednak współczynnik efektywności zdefiniowany następująco:

efektywność architektury =
ilość 64-bitowych dodawań na sekundę

ilość tranzystorów w tysiącach



pozwala na dokonanie oceny samej architektury w pewien specyficzny sposób. Pozwala na oszacowanie efektywności wykorzystania elementów składowych mikroprocesora do wykonywania prostego przetwarzania danych. Wszystkie mechanizmy i bloki funkcjonalne dostępne w złożonych mikroprocesorach niekoniecznie dają przewagę na tym polu, a niejednokrotnie bywają sporym balastem. Przykładem jest mechanizm stronicowania pamięci i przechowywania jej w pliku wymiany. Pochodzący z wielodostępnych systemów serwerowych sposób wirtualnego zwiększenia dostępnej pamięci nie ma praktycznego zastosowania w dzisiejszych, jednostanowiskowych stacjach roboczych, szczególnie w dobie stosunkowo tanich modułów pamięci DRAM.

CISC: Intel 80386

Mikroprocesor Intel 80386 (iAPX 386) jest modelowym przykładem konstrukcji typu CISC (ang. complex instruction set computer – komputer o złożonej liście rozkazów). Kategoria ta została zdefiniowana dużo później niż konstrukcje, które ostatecznie do niej zakwalifikowano. W pewnym momencie rozwoju technologii trend prący do wzrostu komplikacji rdzenia uległ odwróceniu, a w kontraście do architektur uproszczonych RISC zdefiniowano właśnie termin CISC. Najczęściej wymieniane cechy, klasyfikujące dany mikroprocesor do tej kategorii, są następujące:

rozbudowana lista rozkazów (nawet kilkaset),
instrukcje o zróżnicowanej długości i czasie wykonania,
instrukcje złożone, wymagające wykonania kilku mikrooperacji, realizujące więcej niż jedną czynność naraz,
konieczność stosowania mikrokodu do dekodowania instrukcji,
duża liczba trybów adresowania,
instrukcje mogą operować bezpośrednio na komórkach pamięci,
stosunkowo mała liczba rejestrów wewnętrznych.

Wiele z tych cech ma swoje źródło w architekturach komputerów mainframe z lat 60-tych XX wieku, na których bazowały wczesne architektury 16-bitowe. Były to głównie linie DEC VAX oraz PDP.

Chęć zachowania zgodności z 8-bitowymi poprzednikami oraz trend do oszczędzania pamięci programu wymuszał stosowanie rozkazów o zmiennej długości, operujących na różnych typach danych. Dawało to w efekcie krótki, oszczędny kod o dużej gęstości. W początkach technologii komputerowych było to bardzo ważne kryterium. Pamięć była droga, a dostęp do niej wolny, więc zmniejszenie ilości odwołań do pamięci dawało istotne zwiększenie prędkości wykonywania programu. Bardziej zwarty kod, z definicji, wymagał mniej pamięci, a tej wówczas zawsze brakowało. Zwiększenie listy rozkazów oraz zróżnicowanie rozmiarów operandów wymagało rozbudowy jednostki dekodującej. Wiązało się to najczęściej z wykorzystaniem mikrokodu. Dekoder oparty na mikrokodzie stawał się niejako interpreterem instrukcji maszynowych, pobieranych z pamięci, i realizował je za pomocą kodu niższego poziomu. Stosowano także, dla zaoszczędzenia liczby użytych tranzystorów, kod jeszcze niższego rzędu, tzw. nano-kod, którego sekwencje wywoływane były przez mikro-kod (patrz Motorola 68000). Istniały także rozwiązania hybrydowe, łączące wykorzystanie układów logicznych (ang. random logic) z mikro-kodem (np. rodzina Zilog Z8000). 

Konsekwencją wzrostu złożoności układów dekodujących było zwiększenie ilości cykli potrzebnych do zdekodowania rozkazu, co przekładało się na spadek prędkości wykonywania programu. Środkiem zaradczym, tak jak w przypadku mainframeów, było podzielenie jednostki centralnej na wiele jednostek funkcjonalnych, co miało umożliwić wykonywanie kilku operacji jednocześnie. W ten sposób np. proces obliczania adresu efektywnego mógł zachodzić równolegle z obliczaniem wyniku operacji arytmetycznej, której wynik miał być później zapisany w pamięci. W takiej architekturze mniej liczył się sam czas wykonania konkretnej instrukcji, a bardziej ilość instrukcji pobieranych w jednostce czasu. Przy przetwarzaniu potokowym wiele instrukcji mogło być wykonywanych, w całości lub częściowo, równolegle. Rozbijanie złożonych instrukcji na etapy skutkowało jeszcze większym rozbudowaniem jednostki dekodującej i logiki sterującej. Dodatkowym impulsem do rozbudowy listy rozkazów był fakt, że stosunkowo dużo kodu powstawało wówczas bezpośrednio w asemblerze. Kompilatory tamtych czasów były proste i często nie potrafiły efektywnie wykorzystać bardziej wyrafinowanych możliwości, które dawał mikroprocesor, więc z konieczności zadanie to spoczywało na programistach. Tego rodzaju błędne koło można było przerwać tylko przez radykalne uproszczenie listy rozkazów – oznaczało to jednak porzucenie zgodności z poprzednimi modelami mikroprocesorów danej rodziny. Na tak radykalny krok mało który producent mikroprocesorów mógł sobie pozwolić.

Pierwsze mikroprocesory były z konieczności proste i architektonicznie bliższe sterownikom przemysłowym niż współczesnym im minikomputerom. Inercja projektantów, pomimo rozwoju technologii, była trudna do przezwyciężenia. Dopiero konstrukcje 16-bitowe, a później kolejne, w pełni 32-bitowe, mogły zbliżyć się do swoich pierwowzorów – do procesorów trzeciej generacji. 

Intel 80386 jest przykładem dojrzałej konstrukcji, na której można było zaimplementować wielodostępne, „poważne” systemy operacyjne klasy UNIX. Inaczej niż w przypadku Motoroli 68000, projektanci zachowali zgodność na poziomie kodu maszynowego z poprzednimi modelami rodziny x86. W efekcie otrzymano mikroprocesor z listą rozkazów o zmiennej długości, mogący operować na danych różnych rozmiarach. Instrukcje, które pierwotnie operowały na dedykowanych dla nich rejestrach, w kolejnych modelach zyskiwały na elastyczności. Intel 80386 ma znacznie większą ortogonalność listy rozkazów niż model i8086. Bardziej skomplikowaną do dekodowania listę rozkazów zaimplementowano na poziomie mikro-kodu. Wewnętrzna architektura mikroprocesora została rozdzielona na wiele jednostek funkcjonalnych, a kluczowe dla wydajności elementy zostały powielone, by umożliwić równoległe wykonywanie niezależnych od siebie mikroinstrukcji.

Intel 80386 może wykonywać kod maszynowy i8086 w trybie rzeczywistym, znanym z i80286, oraz trybie wirtualnym i8086 (V86). Standardowo, podobnie jak poprzednik, po resecie pracuje w trybie rzeczywistym. W tym trybie może wykonywać bezpośrednio kod i8086 z większą wydajnością niż oryginał. Tryb V86 natomiast jest rozszerzeniem, dzięki któremu zadanie pracujące w trybie chronionym uzyskuje dostęp do pierwszego megabajta pamięci i umożliwia wykonywanie programów napisanych dla 8086 w środowisku maksymalnie zbliżonym do trybu rzeczywistego. Korzystając jednak z możliwości trybu chronionego, kod wykonywany w tym trybie może być całkowicie izolowany od innych zadań. Aby dodatkowo rozdzielić przestrzenie adresowe programów pracujących w trybie V86, które współdzielą obszar pierwszego megabajta pamięci, wykorzystać można mechanizm stronicowania. 

W dalszej części zostaną omówione tylko te elementy architektury i80386, które zostały dodane lub rozszerzone względem i80286. Poprzedni artykuł z tego cyklu zawiera opisy starszych modeli rodziny x86. Model dostępu do pamięci, oprócz standardowego mechanizmu segmentacji opartego na selektorach segmentów, został w i80386 wzbogacony o mechanizm stronicowania. Translację adresów logicznych na adresy fizyczne, wystawiane na szynę adresową mikroprocesora, dokonuje jego wewnętrzne MMU (ang. memory management unit – jednostka zarządzania pamięcią). Proces mapowania składa się z następujących kroków:

Adres logiczny składa się z 16-bitowego selektora segmentu oraz 32-bitowego przesunięcia (ang. offset) wewnątrz tego segmentu. W trybie rzeczywistym i V86 przesunięcie jest 16-bitowe. 20-bitowy adres fizyczny jest wyliczany przez pomnożenie 16-bitowego selektora przez 16 (przesunięcie o 4 bity w lewo) i dodanie do niego 16-bitowego przesunięcia.

W trybie chronionym selektor służy do wybrania odpowiedniego deskryptora segmentu z tablicy lokalnej (LDT) lub globalnej (GDT). W deskryptorze zdefiniowane są różne parametry segmentu, w tym jego adres bazowy oraz rozmiar. Korzystając z danych deskryptora, formowany jest 32-bitowy, płaski (liniowy) adres efektywny. Jeśli stronicowanie jest wyłączone, adres ten staje się bezpośrednio adresem fizycznym i jest podawany na szynę adresową mikroprocesora. Mechanizm stronicowania jest wykorzystywany do tworzenia logicznej przestrzeni adresowej dla zadania, wraz z określeniem praw dostępu do odpowiednich obszarów.


Jeśli mechanizm stronicowania jest włączony (ustawiony bit PE w rejestrze CR0), to uzyskany z modułu segmentacji adres liniowy jest poddawany dodatkowej translacji. Mechanizm stronicowania opiera się na podziale obszaru pamięci na bloki, tzw. strony (ang. page), o stałym rozmiarze 4 KiB. Podobnie jak w mechanizmie segmentacji, istnieją lokalne i globalne katalogi tablic stron. Każdy katalog tablic stron zawiera 1024 (210) elementów. Podobnie każda tablica stron zawiera 1024 (210) elementów wyznaczających fizyczny adres strony. Adres liniowy interpretowany jest więc jako 10-bitowy numer katalogu tablic stron, 10-bitowy indeks strony w tym katalogu oraz 12-bitowe przesunięcie w ramach strony. Daje to razem 32 bity. Mechanizm ten jest wykorzystywany głównie do implementacji pamięci wirtualnej połączonej z przenoszeniem niewykorzystywanych aktualnie stron na dysk (ang. page swapping). W przypadku działania wielu zadań w trybie V86 dzięki mechanizmowi stronicowania możliwa jest separacja ich przestrzeni adresowych.

Jak widać z powyższego, pobieżnego omówienia zintegrowane w strukturze MMU zapewnia wyrafinowane metody ochrony i wirtualizacji pamięci. Jest to wyraźny ukłon w stronę twórców wielodostępnych systemów operacyjnych. 

Kolejnym tego typu mechanizmem jest, wspomagająca wielozadaniowość, koncepcja kontekstu zadania (ang. task). Dla uproszczenia można myśleć o zadaniu jak o aktualnie wykonywanym procesie, który ma przydzieloną dla siebie, zdefiniowaną w oparciu o mechanizm segmentacji i stronicowania, pamięć logiczną. Pamięć logiczna procesu może być podzielona, w sposób przezroczysty dla programu, na obszary o różnym poziomie dostępu i ochrony, który sprawdzany jest w czasie rzeczywistym przez odpowiednie mechanizmy mikroprocesora. Przy próbie odwołania się do niedozwolonego obszaru wykonania niedozwolonej operacji itd. generowany jest sprzętowy wyjątek, który nadrzędny system operacyjny powinien przechwycić i odpowiednio obsłużyć. Mechanizm wielozadaniowości (ang. multitasking) oparty jest na przełączaniu procesora na wykonywanie kolejnych zadań co pewien okres czasu. W ten sposób uzyskuje się iluzję równoległej pracy kilku procesów, pomimo że fizycznie dostępny jest tylko jeden rdzeń. Każde z zadań opisane jest przez strukturę TSS (ang. task state segment – segment stanu zadania). Zawiera ona stan wszystkich rejestrów procesora, dostępnych dla zadania, oraz adres jego lokalnej tablicy deskryptorów oraz katalogu tablic stron. W ten sposób wykonywanie może zostać wstrzymane i wznowione bez zakłóceń. Część TSS jest przeznaczona na dane „swobodne”, które system operacyjny może wykorzystać zgodnie z własnymi potrzebami, np. dla zapisania aktualnego priorytetu zadania, jego zapotrzebowania na czas procesora, listę otwartych plików itd. Rejestr TR (ang. task register – rejestr zadania) zawiera deskryptor i selektor aktualnie wykonywanego zadania.

Zadanie ma przypisany do siebie poziom uprzywilejowania, jeden z czterech dostępnych w i80386. Jest on wykorzystywany do weryfikacji uprawnień dostępu do obszarów pamięci, czy też procedur, które kod zadania może chcieć wywołać. W ten sposób kod użytkownika, pracujący z niskim poziomem uprzywilejowania, może uzyskać dostęp do zasobów innego zadania lub zasobów systemu operacyjnego wyłącznie w kontrolowany sposób. Segment stanu zadania (TSS) jest zdefiniowany jako deskryptor, podlega więc pod ogólne zasady ochrony dostępu, jakie daje system segmentacji. Osobny, uproszczony system ochrony oferuje także system stronicowania, dostępne są tam jednak tylko dwa poziomy dostępu: użytkownika i nadzorcy.

W celu wywołania przez proces użytkownika funkcji systemu operacyjnego, czyli funkcji pracujących z wyższym poziomem uprzywilejowania niż on sam, zachodzi konieczność zmiany tego poziomu na wyższy. Mechanizm pozwalający na takie przełączenie w sposób kontrolowany to tzw. bramka wywołania (ang. call gate) pozwalająca na wywołanie procedury uprzywilejowanej w standardowy sposób, pozwalając procesorowi na wykonanie wszystkich dodatkowych operacji związanych z przełączeniem kontekstu. Struktura definiująca bramkę zawiera, oprócz selektora i offsetu procedury, również minimalny poziom uprzywilejowania kodu, który może z niej skorzystać. W podobny sposób realizowane jest przełączenie kontekstu w przypadku przerwania czy wyjątku. Wykorzystywana jest wtedy odpowiednio bramka przerwania (ang. interrupt gate) lub pułapki (ang. trap gate), które definiowane są w tablicy deskryptorów przerwania IDT (ang. interrupt descriptor table). Adres IDT przechowywany jest w rejestrze IDTR.

W celu ułatwienia analizy pracy programów czy debugowania wprowadzono sprzętowe wsparcie dla obsługi pułapek instrukcji (ang. breakpoints), trybu pracy krokowej oraz pułapek wychwytujących dostęp do obszarów pamięci. Procesor może generować pułapkę po wykonaniu każdej instrukcji, po przełączeniu do określonego zadania lub po wystąpieniu warunku określonego w jednym z rejestrów sterujących debugowaniem DR0..7. 



Rysunek 1. Schemat blokowy mikroprocesora Intel 80386

Jak widać, mikroprocesor Intel 80386 jest nie tylko złożony pod względem zestawu instrukcji, ale również, a może przede wszystkim, ze względu na zintegrowany układ zarządzania pamięcią i dodatkowe moduły znajdujące się w jednej strukturze z rdzeniem.

Intel 80386
rok wprowadzenia do produkcji
1985
ilość tranzystorów
275000
częstotliwość taktowania
16 MHz (cykl zegarowy:  62.5 ns)
najkrótszy cykl instrukcji
125 ns (2 cykle zegarowe)
indeks prędkości
8 MIPS
dodawanie 64-bitowe
250000/s
efektywność architektury
909
typ architektury
32-bitowa, CISC
kolejność bajtów
little-endian
licznik programu
32-bitowy licznik programu
16-bitowy selektor segmentu kodu
stos
32-bitowy wskaźnik stosu
16-bitowy selektor segmentu stosu
rejestry
8 x 32-bitowe ogólnego przeznaczenia
6 x 16-bitowe selektory segmentów
sprzętowe moduły obliczeniowe
32-bitowe ALU
mnożenie/dzielenie
przesuwnik bitowy
obliczanie adresu efektywnego
znaczniki
O – przepełnienie
D – kierunek dla operacji łańcuchowych
I – maska przerwań
T – pułapka przy pracy krokowej
S – znak
Z – zero
A – przeniesienie pomocnicze
P – parzystość
C – przeniesienie
IOPL – priorytet operacji we/wy
NT – zadanie zagnieżdżone
R – używane w obsłudze punktów wstrzymania
VM – tryb wirtualny V86

oraz w rejestrze kontrolnym CR0:

PG – stronicowanie włączone
ET – tryb pracy z koprocesorem
EM – tryb emulacji instrukcji koprocesora
MP – monitorowanie instrukcji koprocesora
PE – tryb chroniony
adresowanie pamięci
4 GiB (32-bitowy adres), adresowanie segmentowane, stronicowanie pamięci
adresowanie portów
64 KiB portów wejścia/wyjścia
przerwania
sprzętowe niemaskowalne
sprzętowe maskowalne
programowe maskowalne

obsługa w/g priorytetów, razem 256 rodzajów (typów) przerwań identyfikowanych kodem
tryby adresowania
domyślne
natychmiastowe
rejestrowe
bezpośrednie
pośrednie rejestrowe (z przesunięciem)
bazowe (z przesunięciem)
indeksowane (z przesunięciem)
bazowe indeksowane (z przesunięciem)
blok danych
względne
tryby pracy
rzeczywisty
wirtualny 8086 (V86)
chroniony (4 poziomy)
praca z koprocesorem
tak
systemy wieloprocesorowe
tak


Lista instrukcji została omówiona w ostatnim odcinku cyklu, przy prezentacji poprzednich mikroprocesorów rodziny x86. W i80386 rejestry ogólnego przeznaczenia zostały rozszerzone do 32-bitów oraz wprowadzono nowe, pomocnicze rejestry segmentowe. W istotny sposób zmniejszono ilość cykli potrzebnych do wykonania większości instrukcji. Sam kod procedury dodawania liczb 64-bitowych przedstawiono poniżej.

; Intel 80386 (iAPX 386)
; dodawanie dwóch liczb 64-bitowych
; ARG0, ARG1 - etykiety buforów zawierających argumenty
; wynik zapisywany jest w ARG1, dane zorganizowane są w 32-bitowe słowa
; kolejność bajtów w słowie zgodna z wymaganą przez architekturę procesora


LEA ESI, ARG0
; ładuj adres argumentu 0 do ESI
2


LEA EDI, ARG1
; ładuj adres argumentu 1 do EDI
2


MOV CX, 2
; ustaw licznik słów
2


CLD
; ustaw znacznik kierunku na inkrementację
2


CLC
; zeruj znacznik przeniesienia
2
NST:
LODSD
; ładuj blokowo kolejne słowo ARG0 do EAX
5


ADC EAX,[EDI]
; dodaj kolejne słowo z ARG1 do AX
6


STOSD
; zapisz blokowo AX do kolejnego słowa ARG1
5


LOOP NST
; kontynuuj dla następnego słowa
11


Sumując czasy wykonania, dostajemy: 10 + 2 x 27 = 64 cykle zegara. Czas wykonania wynosi więc 4 μs, co daje 250000 dodawań na sekundę.

Reasumując, należy przyznać, że Intel 80386 był jak na owe czasy konstrukcją bardzo zaawansowaną. Jednak przez wzgląd na zachowanie zgodności z poprzednikami jego model programowy i mikroarchitektura były nadmiernie rozbudowane i niekonsekwentne. Nowoczesne rozwiązania sąsiadowały ze starymi, a nawet przestarzałymi koncepcjami rodem z mikroprocesorów 8-bitowych. Model segmentacji, przez wymóg zgodności z 16-bitowym i80286, jest niespójny, np. aby umożliwić definiowanie segmentów o większych rozmiarach, musiano dodać specjalny bit ziarnistości (ang. granularity). Cała funkcjonalność związana z implementacją trybu rzeczywistego, zgodnego z i8086 oraz tryb V86 są reliktami, obciążającymi architekturę i rdzeń niepotrzebnym balastem. Oczywiście niepotrzebnym z punktu widzenia efektywności mikroprocesora jako takiego – względy marketingowe związane z zapewnieniem kompatybilności z poprzednimi modelami bez wątpienia miały tutaj pierwszorzędne znaczenie. Zastosowanie mikroprocesora Intel 80386 w prostszych konstrukcjach, które miały być po prostu wydajne i oferować dobre parametry w jednostanowiskowych zastosowaniach, było marnowaniem jego potencjału. Trend aby oferować rozwiązania prostsze, lecz bardziej uniwersalne, zyskał na popularności, a przykład takiego właśnie podejścia zostanie zaprezentowany w dalszej części artykułu.

RISC: VTI VL86C010

Jako przykład mikroprocesora zrealizowanego w architekturze RISC (ang. reduced instruction set computer – komputer o zredukowanej liczbie rozkazów) wybrany został VLSI Technology Inc. (w skrócie VTI) model VL86C010. Jest to procesor w pełni 32-bitowy, którego struktura zawiera 10 razy mniej tranzystorów niż, omawianego powyżej, mikroprocesora Intel 80386. VL86C010 został zaprojektowany przez firmę Acorn jako układ implementujący model programowy ARMv2 oraz mikro-architekturę ARM2. Warto tutaj wspomnieć, że ARM to skrót od Acorn RISC Machine, a później Advanced RISC Machine. Początki architektury ARM to projekt brytyjskiej firmy Acorn Computers Ltd. zainspirowany architekturą 8-bitowego MOS 6502, a mający na celu stworzenie bardziej wydajnego, lecz prostego mikroprocesora o przejrzystej architekturze i uniwersalnym modelu programowym. Ścieżka ewolucji, zaprezentowana w poprzednich odcinkach cyklu, znajduje tutaj swoją kontynuację w postaci konsekwentnego dążenia do efektywnego wykorzystania zasobów.

Sama koncepcja RISC ma swoje źródło w badaniach efektywności wykorzystania listy rozkazów w poszczególnych architekturach i modelach programowych. W latach 80-tych było już jasne, że tylko niewielka część listy rozkazów, często poniżej 20%, jest najczęściej wykorzystywana w kodzie programów. W tym czasie coraz częściej programy były pisane w językach wysokiego poziomu, a coraz rzadziej w asemblerze. Kompilatory nie były jednak wystarczająco wyrafinowane, by wykorzystać złożone instrukcje oferowane przez architektury typu CISC. W efekcie najczęściej wykorzystywane były instrukcje proste i uniwersalne, które kompilator mógł łatwo wykorzystać. Projektanci architektur siłą bezwładu starali się dostarczyć jak najbardziej rozbudowany model programowy, by uczynić go komfortowym dla programisty. Pochłaniało to tysiące godzin pracy projektantów, skutkowało rozbudowaniem jednostki dekodującej rozkazy i w efekcie wymuszało wykorzystanie mikrokodu. Racjonalnym krokiem wydawało się więc dostosowanie sprzętu do rzeczywistych potrzeb programistów, szczególnie gdy stało się jasne, jak mały podzbiór listy rozkazów jest faktycznie wykorzystywany. W przypadku mikroprocesorów na które istniała już duża baza oprogramowania taki krok byłby jednak bardzo brzemienny w skutki. W takich przypadkach nie można było po prostu zerwać z kompatybilnością. Firma Intel, nie pierwszy już raz zresztą, wpadła w taką właśnie pułapkę. 

Aby nie zmuszać klientów do płacenia za nieużywaną funkcjonalność, która dodatkowo pobierała sporą ilość prądu i emitowała ciepło, co dodatkowo ograniczało maksymalną częstotliwość taktowania, wiele firm zaczęło oferować uproszczone konstrukcje typu RISC. Można wyróżnić następujące, charakterystyczne cechy tego typu architektury:

Jednostka dekodująca powinna być realizowana sprzętowo, za pomocą układów logicznych zamiast mikrokodu. Powoduje to skrócenie czasu dekodowania i znaczne uproszczenie układu.


Sprzętowa jednostka dekodująca wymusza ograniczenie listy rozkazów oraz ujednolicenie formatu instrukcji. W praktyce każdy rozkaz powinien mieć jednakową długość (najczęściej pojedyncze słowo) oraz zunifikowany cykl wykonywania. Dzięki temu pobieranie z wyprzedzeniem nie wymaga dekodowania, a czas wykonywania poszczególnych instrukcji jest w miarę jednolity.


Operacje na danych są ograniczone tylko do rejestrów wewnętrznych.


Liczba rejestrów wewnętrznych powinna być duża (kilkadziesiąt).


Dostęp do pamięci jest realizowany tylko za pośrednictwem rejestrów wewnętrznych (architektura typu load-store).


Tryby adresowania pamięci są uniwersalne, najczęściej z pre/post inkrementacją/dekrementacją.

Powyższy zestaw cech jest konsekwencją faktu, że rozbicie bardziej złożonego zadania na więcej prostych instrukcji jest najczęściej i tak szybsze niż wykorzystanie skomplikowanych, dedykowanych instrukcji zaimplementowanych wewnątrz procesora. Prostszy procesor wykona je szybciej dzięki wydajniejszej jednostce dekodującej i elastyczniejszej architekturze, co zapewnia mu większą przepustowość w dostępie do pamięci i większą liczbę instrukcji wykonywanych na sekundę. Przyjęte założenia okazały się słuszne, dodatkowo okazało się, że prostszy rdzeń pobiera mniej mocy w przeliczeniu na megaherc, w związku z tym może być taktowany szybszym zegarem lub może być realizowany w tańszej technologii, co również przekłada się na wzrost efektywności.

Na Rysunku 2 przedstawiono poglądowy schemat modelu programowego mikroprocesora VL86C010. Całkowita liczba 32-bitowych rejestrów to 27, jednak nie wszystkie mogą być jednocześnie wykorzystywane. Rejestry R0..R15 są w każdym trybie pracy dostępne jednocześnie i są, poza pewnymi wyjątkami, rejestrami ogólnego przeznaczenia.



Rysunek 2. Schemat blokowy mikroprocesora VLSI Technology Inc. (VTI) VL86C010 reprezentującego przykład mikroarchitektury ARM2

Istnieje konwencja przypisująca niektórym rejestrom specjalne funkcje, co nie wyklucza ich użycia w dowolnym, innym celu. Tak więc:

R0..R11 – rejestry ogólnego przeznaczenia bez przypisanych specjalnych funkcji.
R12 (FP) – wskaźnik ramki danych (ang. frame pointer).
R13 (SP) – wskaźnik stosu (ang. stack pointer).
R14 (LK) – wskaźnik adresu powrotu z podprogramu (ang. link).
R15 (PC, PSR) – licznik programu (ang. program counter) i rejestr stanu procesora (ang. processor status register).

Każdy z dostępnych trybów pracy ma swój własny podzbiór rejestrów, które zastępują niektóre rejestry trybu użytkownika. Dostępne są następujące tryby pracy:

Użytkownika (ang. user) – standardowy tryb wykonywania programu pracujący na najniższym poziomie uprzywilejowania.


FIRQ (ang. fast interrupt request) – tryb szybkiej obsługi przerwania, wykorzystywany do obsługi przerwań wymagających niskiej latencji. Ma własne rejestry R8..R14, aby zminimalizować konieczność przechowywania modyfikowanych rejestrów na stosie. Rejestr R14 zawiera adres powrotu z przerwania.


IRQ (ang. interrupt request) – tryb normalnej obsługi przerwania. Ma własne kopie rejestru R14, gdzie przechowywany jest adres powrotu z procedury obsługi przerwania, oraz R13, zwyczajowo wykorzystywany jako wskaźnik stosu.


Nadzorcy (ang. supervisor) – tryb pracy uprzywilejowanej, wyzwalany przez pułapki (ang. traps) programowe oraz wyjątki takie jak np. próba wykonania instrukcji niedozwolonej czy dostępu do pamięci poza dozwolonym zakresem 0x0000000 – 0x3FFFFFF.

Koncepcja zapisywania adresu powrotu w rejestrze zamiast na stosie jest swego rodzaju powrotem do koncepcji stosu sprzętowego, znanego już z 4-bitowego mikroprocesora Intel 4004. W przypadku procedur obsługi przerwania daje to bardzo znaczącą redukcję czasu obsługi. Do przechowywania adresów powrotu wykorzystywany jest rejestr R14. Jako wskaźnik stosu wykorzystywany jest rejestr R13, natomiast R12 jako wskaźnik tzw. ramki stosu (ang. stack frame). W skrócie, R13 zawiera aktualny, a R12 poprzedni adres szczytu stosu. W ten sposób obszar pomiędzy R12 i R13 może być wykorzystany do alokowania tzw. zmiennych automatycznych. Są to zmienne dealokowane automatycznie po powrocie z podprogramu, ponieważ ich obszar pamięci umieszczony jest na stosie. Tego typu mechanizm alokacji jest bardzo powszechny i występuje w praktycznie każdym rodzaju procesora.

Rejestr R15 jest jedynym, który nie ma kopii i jest bezpośrednio dostępny w każdym trybie pracy. Ten 32-bitowy rejestr jest funkcjonalnym złożeniem rejestru licznika programu PC i rejestru stanu procesora PSR:

Tryb pracy procesora (ang. processor mode) – bity 0..1, określają aktualny tryb pracy: użytkownika, FIRQ, IRQ lub nadzorcy.


Licznik programu – bity 2..25, adres instrukcji musi być wyrównany do pełnych czterech bajtów, dlatego wystarczą 24 najbardziej znaczące bity pełnego, 26-bitowego adresu.


Znaczniki – bity 26..31, zawierają znaczniki (flagi) procesora, odpowiednio: F, I, O, C, Z, N.

Takie połączenie w jednym, 32-bitowym rejestrze dwóch niezależnych struktur danych ma, pomimo oczywistych wad, jedną zaletę. Pozwala na zapisanie stanu i adresu powrotu mikroprocesora w jednym słowie, za pomocą jednej, standardowej instrukcji przesłania danych.

VLSI Technology Inc. (VTI) VL86C010
rok wprowadzenia do produkcji
1986
ilość tranzystorów
27000
częstotliwość taktowania
10 MHz (cykl zegarowy:  100 ns)
najkrótszy cykl instrukcji
100 ns (1 cykl zegarowy)
indeks prędkości
10 MIPS
dodawanie 64-bitowe
 277 777/s
efektywność architektury
10288
typ architektury
32-bitowa, RISC, load-store
kolejność bajtów
little-endian
licznik programu
24-bitowy licznik 32-bitowych słów
stos
32-bitowy wskaźnik, którym może być dowolny rejestr, zwyczajowo jest to R13
rejestry
27 rejestrów 32-bitowych
sprzętowe moduły obliczeniowe
32-bitowe ALU
mnożenie/dzielenie
przesuwnik bitowy
obliczanie adresu efektywnego
znaczniki
M0,M1 – tryb pracy procesora
F – maska przerwania szybkiego
I – maska przerwania normalnego
O – przepełnienie
C – przeniesienie
Z – zero
N – znak
adresowanie pamięci
64 MiB (24-bitowy adres słowa 32-bitowego), tryb adresowania bezpośredniego(fizycznego) lub logicznego w zależności od trybu pracy i obecności MMU w systemie.
adresowanie portów
mapowane w obszarze pamięci
przerwania
sprzętowe maskowalne szybkie
sprzętowe maskowalne
programowe maskowalne
wyjątek/pułapka

obsługa wg stałych priorytetów
tryby adresowania
domyślne
względne (względem PC)
bazowe z przesunięciem i postinkrementacją/postdekrementacją
bazowe z przesunięciem i preinkrementacją/predekrementacją
bazowe z indeksowaniem i postinkrementacją/postdekrementacją
bazowe z indeksowaniem i preinkrementacją/predekrementacją
tryby pracy
nadzorcy
użytkownika
obsługi przerwania szybkiego (FIRQ)
obsługi przerwania (IRQ)
praca z koprocesorem
tak
systemy wieloprocesorowe
nie


Tym, co jest w przypadku tego mikroprocesora najciekawsze, a co jest charakterystyczne dla całej rodziny RISC, jest prostota i regularność listy rozkazów. Prostota i regularność umożliwia implementację dekodera instrukcji w formie sprzętowej, jako układu logicznego, zamiast dekodera opartego na mikrokodzie.

Każda instrukcja mikroprocesora VL86C010 ma 32-bity; pozwala to uprościć układ pobierania i kolejkowania instrukcji oraz pobierania z wyprzedzeniem. Nie jest potrzebna informacja z układu dekodującego o rozmiarze instrukcji do pobrania, ponieważ jest on stały. Kolejną generalizacją jest umieszczenie w kodzie każdej instrukcji 4-bitowego kodu warunkowego. Bity 28..31 każdej instrukcji określają warunek, który musi wystąpić, aby była ona wykonana przez procesor. Jeśli warunek nie wystąpi, instrukcja jest ignorowana, a przetwarzanie przechodzi do następnej. Kody binarne warunków wraz z opisami podane są poniżej:


kod
skrót
opis
znaczniki
0000
EQ (ang. equal)
równe
Z=1
0001 
NE (ang. not equal)
różne
Z=0
0010 
CS (ang. carry set)
przeniesienie
C=1
0011 
CC (ang. carry clear)
brak przeniesienia
C=0
0100 
MI (ang. minus)
mniejsze od zera
N=1
0101 
PL (ang. plus)
nie mniejsze od zera
N=0
0110 
VS (ang. overflow set)
przepełnienie
V=1
0111 
VC (ang. overflow clear)
brak przepełnienia
V=0
1000 
HI (ang. unsigned higher)
większe (bez znaku)
C=1,Z=0
1001 
LS (ang. unsigned less)
nie większe (bez znaku)
C=0,Z=1
1010 
GE (ang. greater or equal)
większe lub równe
N=V
1011 
LT (ang. less than)
mniejsze
N≠V
1100 
GT (ang. greater than)
większe
Z=0 i N=V
1101 
LE (ang. less or equal)
mniejsze lub równe
Z=0 lub N≠V
1110 
AL (ang. always)
zawsze


1111 
NV (ang. never)
nigdy




Podając warunek NV, każda instrukcja staje się instrukcją NOP znaną z innych modeli programowych.

Zestaw instrukcji zasadniczo można podzielić na następujące kategorie:

Przetwarzające dane (ang. data processing) – operują wyłącznie na rejestrach wewnętrznych. W każdej z nich określone są dwa operandy źródłowe (argumenty) oraz miejsce zapisania wyniku. Przy wybraniu R15 jako operandu wynikowego nie wszystkie jego bity mogą zostać ustawione w trybie użytkownika. Jednym z operandów źródłowych może być 8-bitowa wartość natychmiastowa (literał). Również do jednego z argumentów może zostać zastosowane przesunięcie realizowane przez sprzętowy przesuwnik bitowy (ang. barrel shifter). Do tej kategorii danych, oprócz instrukcji transferów między rejestrami, zalicza się również wszystkie operacje realizowane przez jednostkę arytmetyczno-logiczną (ALU).


Przesyłające dane (ang. data transfer) – realizują ładowanie danych z pamięci do rejestru (ang. load) oraz zapisywanie wartości rejestru do pamięci (ang. store). Adres efektywny jest obliczany jako suma wartości rejestru bazowego i 12-bitowego przesunięcia, które może być wartością natychmiastową lub zapisaną w innym rejestrze. W przypadku gdy przesunięciem jest wartość rejestru, może ona być dodatkowo przesunięta bitowo o określoną w instrukcji wartość. Rejestr przesunięcia może być modyfikowany przed lub po obliczeniu efektywnego adresu, może też nie być modyfikowany wcale. Przykłady trybów adresowania zostaną pokazane w dalszej części artykułu. Instrukcja wykonywana w trybie uprzywilejowanym może określać, czy adres efektywny ma być adresem fizycznym czy logicznym, podlegającym dalszej translacji przez zewnętrzne MMU. Odpowiedni bit kodu instrukcji jest podawany na pin TRAN mikroprocesora do wykorzystania przez układy zewnętrzne.


Transferu blokowego (ang. block transfer) – umożliwiają przesłanie grupy rejestrów do/z pamięci operacyjnej. Każdy z rejestrów od R0 do R15 ma jeden bit w kodzie instrukcji. Mechanizm transferu blokowego umożliwia efektywne składowanie rejestrów na stosie oraz przyspieszenie przesyłania danych. Dostępne są tryby adresowania z post/pre inkrementacją lub dekrementacją.


Skoków (ang. branch) – instrukcje skoków realizowane są z zachowaniem adresu powrotu (ang. link) lub bez. Adres docelowy podany jest jako 24-bitowe przesunięcie względem licznika rozkazów. Przesunięcie jest określone w 32-bitowych słowach, więc efektywnie jest 26-bitowe. Adres powrotu razem z aktualną wartością PSR jest zachowywany w rejestrze R14. Powrót z tak wywołanego podprogramu realizowany jest przez instrukcję MOV PC,R14, która po prostu ładuje do PC wartość zapisaną uprzednio w R14. Nie ma dedykowanej instrukcji powrotu z podprogramu. W celu odtworzenia również znaczników należy dodać “S” do mnemonika instrukcji (konwencja ta zostanie omówiona na przykładach w dalszej części artykułu).


Przerwań programowych (ang. software interrupt) – stosowane głównie do realizowania wywołań funkcji systemowych, które muszą być wykonane w trybie uprzywilejowanym. Stan PC i PSR, czyli wartość rejestru R15, jest zapisywana w rejestrze R14 trybu nadzorcy, następnie PC jest ładowane wartością odpowiednią dla wywoływanego przerwania, a procesor przechodzi w tryb nadzorcy.


Jak widać na przykładzie umieszczenia warunku wykonania w kodzie każdej instrukcji, programowanie mikroprocesora typu RISC jest trochę podobne do programowania w mikrokodzie. Każda instrukcja ma 32-bity, co jest w wielu przypadkach wielkością o wiele za dużą, jednak dzięki temu układ dekodujący może być szybki i zaimplementowany sprzętowo. VL86C010 jest mikroprocesorem o architekturze load-store, w którym istotną rolę pełnią dostępne przy transferach między rejestrami i pamięcią tryby adresowania. Lista instrukcji przesyłania danych została zebrana w poniższej tabeli.


instrukcja
opis
LDR
załaduj do rejestru wartość z pamięci
STR
zapisz zawartość rejestru do pamięci
LDM
załaduj do listy rejestrów wartości z kolejnych komórek pamięci
SRM
zapisz listę rejestrów do kolejnych komórek pamięci



Jednym z argumentów jest zawsze rejestr (lub rejestry) wewnętrzny, drugim zaś adres efektywny. Sposób wyliczania adresu efektywnego zależy od wybranego trybu adresowania:

Adresowanie względne (względem PC) – suma wartość licznika rozkazów i  12-bitowego przesunięcia.


Adresowanie bazowe rejestrowe z postinkrementacją – wartość rejestru bazowego. Po wykonaniu rozkazu do rejestru bazowego dodawane jest 12-bitowe przesunięcie.


Adresowanie bazowe rejestrowe z preinkrementacją – suma rejestru bazowego i 12-bitowego przesunięcia. Obliczona wartość może zostać wpisana do rejestru bazowego.


Adresowanie bazowe rejestrowe, indeksowane z postinkrementacją – wartość rejestru bazowego. Po wykonaniu rozkazu do rejestru bazowego dodawana jest wartość rejestru indeksowego.


Adresowanie pośrednie rejestrowe, indeksowane z preinkrementacją – suma rejestru bazowego i rejestru indeksowego. Obliczona wartość może zostać wpisana do rejestru bazowego.


Wartość offsetu lub rejestru indeksowego może być przesunięta bitowo i dopiero potem wykorzystana do wyliczenia adresu. Daje to możliwość skalowania kroku np. przy dostępie do tablic w trybie adresowania indeksowego. Przesunięcie może być wartością dodatnią lub ujemną, steruje tym osobny bit w kodzie instrukcji. Instrukcje przesyłające dane mogą operować na pojedynczych bajtach lub na 32-bitowych słowach, jest to również definiowane przez osobny bit. W przypadku przesyłania danych o rozmiarze bajtu możliwe jest wykorzystanie adresów niewyrównanych do pełnych 4-bajtów. Transfery blokowe, obejmujące kilka rejestrów, mają nieco okrojony zbiór dostępnych trybów adresowania. W trakcie takiego transferu mogą również wystąpić wyjątki związane z dostępem do zabronionego obszaru pamięci.

Kolejną grupą rozkazów są rozkazy przetwarzające dane. W przypadku tego mikroprocesora zalicza się do nich także instrukcje transferu między rejestrami. Wszystkie instrukcje tej kategorii zebrane zostały w poniższej tabeli.

instrukcja
opis
ADC
dodawanie z przeniesieniem
ADD
dodawanie
AND
iloczyn logiczny
BIC
skasuj bit
CMN
porównanie oparte na sumie argumentów
CMP
porównanie oparte na różnicy argumentów (standardowe)
EOR
suma modulo 2, czyli XOR
MLA
mnożenie z akumulacją wyniku
MUL
mnożenie
MOV
transfer danych między rejestrami
MVN
transfer danych między rejestrami z negacją
ORR
suma logiczna, OR
RSB
odejmowanie z odwróceniem kolejności argumentów
SUB
odejmowanie
RSC
odejmowanie z odwróceniem kolejności argumentów i przeniesieniem
SBC
odejmowanie z przeniesieniem
TEQ
test na równość argumentów (XOR bez zapisania wyniku)
TST
test z wykorzystaniem maski bitowej


Zestaw instrukcji jest prosty, lecz w połączeniu z elastycznością w definiowaniu drugiego argumentu oraz możliwością zastosowania do niego przesunięcia daje wszechstronne i elastyczne narzędzie dla programisty czy kompilatora. Do drugiego argumentu instrukcji przetwarzającej dane można zastosować przesunięcie o określoną ilość bitów. Wartość przesunięcia może być zdefiniowana jako literał lub może być pobrana z innego rejestru. Do wyboru są tryby przesunięcia w prawo, w lewo, z wykorzystaniem znacznika C lub wartości 1. Możliwy jest również obrót z wykorzystaniem znacznika C jako dodatkowego bitu.

W niniejszym artykule nie ma miejsca na systematyczny wykład assemblera mikroprocesora VL86C010, w tym miejscu autor odsyła do stosownej literatury. Ogólny styl i możliwości, jakie daje architektura ARMv2, zaprezentowane zostały na poniższych, wybranych fragmentach kodu. 

Przykład 1:

TEQ		R1, 0
RSBMI	R1, R1, 0

Sprawdź (TEQ), czy R1 ma wartość 0. Instrukcja ustawia również znacznik znaku N. Następnie wykonaj odejmowanie z odwróceniem kolejności operandów (RSB), tylko jeśli testowanie wykryło wartość ujemną (MI). Wynik operacji 0 - R1 wpisz do rejestru R1. Biorąc pod uwagę, że przy odejmowaniu R1 musi być wartością ujemną, cały powyższy kod przekształca liczbę zapisaną w R1 na jej wartość absolutną. Możliwość zadeklarowania dowolnej instrukcji jako wykonywanej warunkowo, dodając do niej odpowiedni postfix, pokazuje w tym przykładzie swoją użyteczność.

Przykład 2:

ADD		R1, R1, R1 LSL 1
MOV		R1, R1 LSL 1

Pierwsza instrukcja dodaje (ADD) wartość rejestru R1 do wartości rejestru R1 przesuniętej w lewo (LSL) o jeden bit. W efekcie mamy R1 + 2*R1 = 3*R1 i taka wartość zostaje wpisana do R1. Następnie prześlij (MOV) do rejestru R1 wartość R1 przesuniętą w lewo (LSL) o jeden bit, czyli pomnożoną przez 2. Wartość w rejestrze R1 zostaje pomnożona najpierw przez 3, a potem przez 2, czyli w efekcie przez 6. Za pomocą dwóch instrukcji, z wykorzystaniem sprzętowego przesuwnika bitowego można zrealizować błyskawiczne mnożenie przez 6.

Przykład 3:
LDR		R1, [R0, 8]
STR		R1, [R2, R3 LSL 2]!

Załaduj rejestr (LDR), w tym przypadku R1 wartością z komórki pamięci o adresie zapisanym w rejestrze R0 z przesunięciem 8. Nie modyfikuj wartości rejestru R0. Następnie zapisz tak pobraną wartość rejestru (STR) R1 do komórki pamięci o adresie zapisanym w rejestrze R2 powiększoną o wartość rejestru R3 przesuniętą uprzednio w lewo (LSL) o 2 bity. Po wykonaniu instrukcji zapisz (!) tak wyliczony adres efektywny do rejestru bazowego R2. Powyższe instrukcje pokazują elastyczność i siłę wyrazu modelu programowego opartego na łączeniu funkcji podstawowych bloków mikroarchitektury w jednej instrukcji. 

Struktura asemblera ARMv2 jest nieco podobna w swojej regularności do asemblera mikroprocesora Motorola 68000, który został opisany w poprzednim odcinku cyklu. Separacja instrukcji przetwarzających dane od instrukcji przesyłających daje czysty i elegancki kod. Pomimo że często musi zawierać więcej operacji, jest dzięki swojej prostocie wykonywany szybciej. Widać to na przykładzie podanego niżej, standardowego programu testowego. Widoczna niedogodność to konieczność zapisywania i odtwarzania znacznika przeniesienia między kolejnymi operacjami dodawania z powodu braku np. dedykowanej instrukcji skoku po odliczeniu do zera.

; VLSI Technology Inc. VL86C010 (ARMv2)
; dodawanie dwóch liczb 64-bitowych
; ARG0, ARG1 - etykiety buforów zawierających argumenty
; wynik zapisywany jest w ARG1, dane zorganizowane są w 32-bitowe słowa
; kolejność bajtów w słowie zgodna z wymaganą przez architekturę procesora


LEA  R0, ARG0
; ładuj adres argumentu 0 do R0
2


LEA  R1, ARG1
; ładuj adres argumentu 1 do R1
2


ADD  R2, R1, 8
; ładuj adres po końcu bufora argumentu 1
1


MOV  R5, 0
; zeruj bufor znacznika C (rejestr R5)
1
NST:
LDR  R3, [R0],4
; ładuj kolejne słowo ARG0 do R3, R0 = R0+4
3


LDR  R4, [R1]
; ładuj kolejne słowo ARG1 do R4
3


MOVS R5, R5 LSL 1
; odtwórz znacznik C z MSB rejestru R5
1


ADCS R4, R3, R4
; dodaj słowa ARG0 i ARG1 wynik zapisz w R4
1


STR  R4, [R1],4
; zapisz R4 do słowa ARG1, R1 = R1+4
2


MOVS R5, R5 RRX 1
; zapisz znacznik C w MSB rejestru R5
1


CMP  R1, R2
; sprawdź, czy już dodano wszystkie słowa
1


BNE  NST
; kontynuuj, jeśli nie
3


Sumując czasy wykonania, dostajemy: 6 + 2 x 15 = 36 cykli zegara. Czas wykonania wynosi więc 3.6 μs, co daje 277 777 dodawań na sekundę. Obliczając, zgodnie z przyjętą metodologią, wskaźnik efektywności, dostajemy wartość ponad 10-krotnie większą niż uzyskał Intel 80386. Tak miażdżąca przewaga wynika z faktu, że w przypadku VL86C010 płacimy tylko za to, czego potrzebujemy w tak prostym teście. Wskaźnik efektywności dla prostych zadań oddaje ten fakt z bezlitosną precyzją.

Kolejną, nie omówioną wcześniej klasą instrukcji są operacje komunikacji z koprocesorem. Do sterowania komunikacją służa wyprowadzone na zewnątrz linie:

Wejście CPA (ang. co-processor absent – koprocesor nieobecny) – jeśli koprocesor nie jest fizycznie zainstalowany, nie będzie w stanie zasygnalizować swojej obecności po wykryciu przeznaczonej dla niego instrukcji. W takim przypadku wymiana potwierdzeń (ang. handshake) między procesorem a koprocesorem jest przerywana i wywoływany jest wyjątek/pułapka związany z niedozwoloną instrukcją. W przeciwnym razie procesor główny aktywuje linię CPI i oczekuje na zakończenie wykonywania instrukcji, testując linię CPB.


Wejście CPB (ang. co-processor busy – koprocesor zajęty) – koprocesor aktywuje tę linię podczas przetwarzania przeznaczonej dla niego instrukcji. Daje to sygnał procesorowi do wstrzymania wykonywania aż do jej zakończenia. Koprocesor powinien aktywować sygnał dopiero w momencie, gdy jest gotów na odbieranie danych związanych z konkretną instrukcją. Procesor główny jest wstrzymywany, by dać czas koprocesorowi na przygotowanie.


Wyjście CPI (ang. co-processor instruction – instrukcja koprocesora) – procesor główny aktywuje tę linię podczas przetwarzania instrukcji przez koprocesor. W tym czasie program jest wstrzymywany, a dalsze przetwarzanie uzależnione od stanu linii CPA i CPB.


Instrukcje służące do komunikacji z koprocesorem zostały zebrane w poniższej tabeli.

instrukcja
opis
CDO
instrukcja zleca wykonanie operacji przez koprocesor na danych, które zawarte są w jego wewnętrznych rejestrach. Procesor główny nie oczekuje na zakończenie takiej operacji ani żaden wynik nie jest automatycznie do niego przesyłany z koprocesora
LDC/STC
instrukcje przesyłają pojedyncze bajty lub słowa 32-bitowe z/do rejestrów wewnętrznych koprocesora
MCR/MRC
instrukcje realizują transfer danych między rejestrami procesora głównego a rejestrami koprocesora. W efekcie wykonania takiej instrukcji rejestry procesora głównego mogą zostać zmodyfikowane


Jak widać, architektura load-store ma swoje odzwierciedlenie również w obszarze komunikacji z koprocesorem.

Podsumowanie

Przez wszystkie poprzednie odcinki cyklu o ewolucji mikroprocesorów obserwowaliśmy zmaganie się ze sobą dwóch paradygmatów. Terminy RISC i CISC oddają podejście projektantów poszczególnych architektur i modeli programowych na tyle dobrze, że możemy w końcu w miarę precyzyjnie nazwać te dwa odmienne trendy. Mikroprocesory RISC, jako prostsze, a więc tańsze i bardziej energooszczędne zdominowały dosyć szybko segment urządzeń mobilnych, czyli ten obszar, w którym wydajność nie mogła być osiągana kosztem dużego poboru mocy. Zdobywanie doświadczenia w tworzeniu efektywnych konstrukcji opłaciło się, gdy pojawiła się technologiczna bariera dla zwiększania częstotliwości taktowania. Mikroprocesory RISC, pozbawione bagażu kompatybilności wstecznej i przerośniętej listy rozkazów, pokazały swoją przewagę w sytuacji, gdy „surowa moc” nie była już dla architektur CISC dostępna bez ograniczeń. W chwili pisania tego artykułu znajdujemy się w przededniu pierwszego szturmu na bastion CISC, w którym nowy mikroprocesor RISC firmy Apple ma wyprzeć z komputerów tej firmy zadomowione tam od lat mikroprocesory firmy Intel. 

Źródła i bibliografia

W powstaniu niniejszego artykułu wykorzystane zostały następujące publikacje i źródła:


Introduction to the 80386. Including the 80386 Data Sheet, Intel Corporation 1986.
80386 Hardware Reference Manual, Intel Corporation 1987.
Acorn RISC Machine (ARM) Family Data Manual, VLSI Technology Inc. 1990.
Wikipedia – wikipedia.org
Wikichip – wikichip.org
![alt text](<wzór na efektywność architektury.png>) ![alt text](<VL86C010 - schemat blokowy.png>) ![alt text](<intel 80386 - schemat blokowy.png>)