# Artykuły
Artykuły publikowane w prasie.

## Intel 4004 -  początek czwartej generacji
Z perspektywy współczesnych 64-bitowych, wielordzeniowych mikroprocesorów nawet konstrukcje 16-bitowe wydają się archaiczne. Komputery 8-bitowe lat 70-tych i 80-tych XX wieku wyglądają jak zabawki, mające tylko wartość sentymentalną. Przy nich Intel 4004, konstrukcja 4-bitowa, prezentuje się już tylko groteskowo i prymitywnie. Jednak przy przetwarzaniu liczb dziesiętnych podstawową porcją danych jest pojedyncza cyfra, a ta daje się doskonale zapisać właśnie na 4 bitach.

## 8-bitów - Intel 8080, Motorola i inni...
Wprowadzenie mikroprocesorów na rynek komercyjny na początku lat 70-tych XX wieku  zaowocowało powstaniem najrozmaitszych systemów komputerowych, oferowanych przez wielkie firmy, jak i „garażowych”, niszowych hobbystów. Szczególnie konstrukcje 8-bitowe, jako stosunkowo proste i tanie, wyzwoliły twórczy potencjał konstruktorów. Rozgrzały także rywalizację o rynek wśród producentów mikroprocesorów.

## Okolice 16-bitów - Intel 8086 i konkurencja
Po sukcesie mikrokomputerów 8-bitowych przyszła pora na przejmowanie przez mikroprocesory obszarów wymagających większej mocy obliczeniowej, zarezerwowanych do tej pory dla tzw. minikomputerów. Do tego potrzebna była, oprócz szybkości, bardziej elastyczna architektura, możliwość obsługi większej pamięci i współpraca z dodatkowymi jednostkami obliczeniowymi. Kolejny krok w ewolucji stał się nieuchronny.

## 32-bity - CISC vs RISC
Począwszy od pierwszej 4-bitowej konstrukcji, rozwój mikroprocesorów przebiegał zasadniczo dwiema niezależnymi ścieżkami. Pierwsza koncentruje się wokół hardware’u i zaimplementowaniu w nim jak największej liczby funkcji. Druga zorientowana jest na software, ponieważ oprogramowanie, w odróżnieniu od sprzętu, można zaktualizować. Dlatego sprzęt powinien realizować tylko to, co musi, oraz co – ze względu na wydajność – powinien.

## Mikroprocesor w stylu "retro"
Używając wysokopoziomowych języków programowania i wyszukanych abstrakcji, często tracimy z oczu procesor, a to on ostatecznie wykonuje napisany przez nas program. Bohaterem tego artykułu jest MOS 6510, mikroprocesor o architekturze, której prostotę i piękno oddaje następujący cytat:

*Wydaje się, że doskonałość osiąga się nie wtedy, kiedy nie można już nic dodać, ale raczej wtedy, gdy nie można nic ująć.”*

<p style="text-align: right">Antoine de Saint-Exupéry</p>

## Rust - nowoczesny minimalizm
Języki programowania wywodzące się od C mają w dalszym ciągu wielu zwolenników. Po części jest to spowodowane ogromną bazą kodu i szerokim wyborem narzędzi, które przez lata zostały stworzone na potrzeby tego języka, a po części przez niemal pełną kontrolę nad kodem maszynowym, który powstaje w wyniku kompilacji. Zaproponowanie  nowocześniejszej, a jednak w dalszym ciągu niskopoziomowej alternatywy, która przyjęłaby się w środowisku programistycznym, wydawało się niemożliwe, a przynajmniej bardzo trudne. Tak było, dopóki nie pojawił się Rust ...

## Sprzęgacz kierunkowy - jak i dlaczego to działa

# Projekty
Projekty, w większości open-source.

## mo65x - emulator 6502/6510 napisany w C++ z wykorzystaniem Qt
Emulator opisywany w artykule *"Mikroprocesor w stylu retro"* z GUI zrealizowanym za pomocą biblioteki Qt.

## mo65x-rs - emulator procesora 6502/6510 napisany w języku Rust
Emulator o podobnej funkcjonalności jak **mo65x** zaimplementowany w języku Rust z interface-em tekstowym.

## Radiotester - uniwersalne urządzenie pomiarowe
Uniwersalne urządzenie pomiarowe typu wobuloskop z kilkoma sondami pomiarowymi wraz z oprogramowaniem na komputer osobisty.