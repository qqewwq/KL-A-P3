# KL-A – Pakiet P3: Obraz, ocena, pobudzenie

**Zespół:** Diana Kalinina - 164014; Oleksandr Shamak - 161578
**Data:** 2026-10-12  
**Repozytorium:** [link], tag `KL-A`

## Cel i zmienne zależne

W badaniu głównym uczestnik ogląda cztery obrazy (ekspozycja 4000 ms, potem szary ekran 1000 ms) i ocenia swoje wrażenia za pomocą skali SAM. Główne dane to subiektywna ocena uczestnika (odpowiedzi w SAM) i reakcja pobudzenia fizjologicznego (EDA/GSR). Eye-tracker może służyć również do kontroli jakości sygnału wzroku i przebiegu sesji, ale nie zastępuje ocen emocjonalnych. Dokładne zmienne zależne i liczbę prób należy potwierdzić z pełnym protokołem LBK.

## Tabela stanów i sygnałów

| Stan | Sygnał | Wymagany czas reakcji systemu | Możliwy błąd interpretacji |
|---|---|---|---|
| Podwyższone pobudzenie podczas oglądania obrazu | EDA/GSR – zmiana przewodnictwa skóry | Analiza sygnału w oknie 1–4 s po początku bodźca; nie zmieniać ekspozycji obrazu 4000 ms | Ruch ręki lub artefakt czujnika może zwiększyć sygnał EDA |
| Utrata śledzenia wzroku | Eye-tracker – `gaze_valid`, brak kolejnych ważnych próbek | Wykrywanie w ciągu 200–500 ms jako proponowany próg techniczny | Mrugnięcie lub chwilowy ruch głowy może zostać uznany za trwałą utratę śledzenia |
| Powrót EDA do poziomu bazowego | EDA/GSR – porównanie bieżącego sygnału z indywidualną linią bazową | Kontrola np. co 500 ms; maksymalny czas oczekiwania wymaga ustalenia z LBK | Naturalne wahania EDA mogą wyglądać jak powrót do poziomu bazowego, mimo że pobudzenie nadal występuje |

Czasy są propozycją do testów, nie wynikami pomiaru. Model procesora ludzkiego opisuje orientacyjnie czas przetwarzania percepcyjnego (ok. 100 ms), poznawczego (ok. 70 ms) i motorycznego (ok. 70 ms). Wartości te pomagają uzasadnić skalę czasu reakcji systemu, ale nie są bezpośrednimi czasami reakcji EDA ani eye-trackera. Zmiany EDA wymagają analizy w dłuższym oknie, dlatego proponuje się analizę 1–4 s po początku bodźca. Decyzje adaptacyjne nie mogą zmieniać czasu ekspozycji obrazu (4000 ms) ani kolejności bodźców. Podane czasy są propozycjami do testów, a nie oficjalnymi parametrami zatwierdzonymi przez LBK.

## Pola nagrania symulatora

| Pole | Znaczenie | Sygnał |
|---|---|---|
| `t` | Czas od początku symulacji | sekundy (s) |
| `hr` | Częstość akcji serca, zwykle uderzenia/min | HR |
| `eda_us` | Przewodnictwo skóry, µS | EDA/GSR |
| `rr_ms` | Odstęp między kolejnymi uderzeniami serca, ms (jeśli obecny) | HR/HRV |
| `focus` | Wskaźnik symulowany; definicja zależy od symulatora | Nie jest bezpośrednim pomiarem uwagi |
| `quality` | Wskaźnik jakości danych; dokładne kodowanie zależy od symulatora | Jakość |
| `marker` | Znacznik zdarzenia, jeśli obecny | Zdarzenie |

Według opisu zadania symulator nie udostępnia rzeczywistych próbek spojrzenia (`gaze_x`, `gaze_y`, `gaze_valid`), średnicy źrenicy (`pupil_mm`) ani oddechu (`resp`). Nazwy i rzeczywistą obecność pól należy zweryfikować po nagraniu; jeżeli nie ma jakiegoś pola z tabeli, trzeba to zaznaczyć.

## Plik danych i dokumentacja

- Nagranie: `sample_data/nagranie.jsonl` — **nagranie wykonano (241 wierszy, ok. 28 KB); plik należy jeszcze skopiować do tej lokalizacji**.
- Dokumentacja: `BUILD_BOOK.md`, rozdziały 0, 1 i 14.
- Repozytorium prywatne, z udostępnionym odczytem prowadzącemu i tagiem `KL-A`.
