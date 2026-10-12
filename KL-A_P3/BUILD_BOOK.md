# BUILD BOOK – P3 „Obraz, ocena, pobudzenie”

**Etap:** KL-A · **Data aktualizacji:** 2026-10-12  
**Zakres:** rozdziały 0, 1 i 14. Dokumentacja wykonanych czynności.


## 0. Metryczka

- **Pakiet:** P3 – Obraz, ocena, pobudzenie (**do potwierdzenia przydziału na forum**).
- **Badanie główne:** „Qualia, barwa i subiektywne doświadczenie”.
- **Badanie drugie:** „Reklama spod ręki człowieka czy algorytmu?”.
- **Protokół badania głównego:** do uzupełnienia po uzyskaniu pliku od prowadzącego (nazwa, data, wersja). Na tym etapie wykorzystano opis pakietu na platformie AHE.
- **Zespół:** DK – Diana Kalinina - 164014; OS - Oleksandr Shamak - 161578
- **Podział pracy (KL-A):** DK – instalacja PsychoPy i uruchomienie demo; OS – uruchomienie symulatora i zapis 60-sekundowego nagrania, analiza pól i jakości danych, weryfikacja elementów środowiska (pylsl, LabRecorder, MouseGaze, EyeLogic/GSR-EDA) oraz przygotowanie pytań 14.2 i 14.4.
- **Repozytorium:** [adres prywatnego repozytorium po utworzeniu]. **Tag:** `KL-A` (do utworzenia).
- **Komputer do wykonania KL-A:** MacBook Air z Apple Silicon; **system:** macOS.
- **PsychoPy:** aplikacja Standalone pobrana jako `StandalonePsychoPy-2025.2.4-macOS-3.10.dmg`; w uruchomionej aplikacji widoczna była wersja **2025.2.4beta**. Wymaga potwierdzenia w oknie *About* i uzgodnienia tej samej wersji z drugą osobą zespołu.
- **Uruchomienie aplikacji:** przez Rosetta 2 (wersja instalacji dla procesorów Intel).
- **Python używany w terminalu:** `python3`; dokładna wersja: **[wynik `python3 --version`]**. Wersja Pythona wewnątrz PsychoPy może być inna.
- **Ekran:** rozdzielczość, odświeżanie i konfiguracja Monitor Center – **[do zmierzenia/zapisania przed budową eksperymentu]**.
- **Aparatura LBK:** EyeLogic (wzrok), GSR/EDA, ECG, oddech. Wersje sterowników i ustawienia stanowiska zostaną sprawdzone w LBK.

## 1. Instalacja od zera

### Krok 1.1 – Instalacja PsychoPy na Macu

**Autor:** DK · **Data:** 2026-10-12 · **Status:** wykonano

- **Cel:** przygotować aplikację do uruchamiania eksperymentów.
- **Gdzie:** instalator PsychoPy dla macOS, katalog `/Applications`.
- **Czynności:**
  1. Najpierw zainstalowano nowszą wersję PsychoPy 2026.2.4; przy uruchomieniu wystąpił błąd `wx._core.wxAssertionError` związany z locale. Próba uruchomienia z `LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8` nie pomogła.
  2. Następnie pobrano `StandalonePsychoPy-2025.2.4-macOS-3.10.dmg` i zainstalowano aplikację w `/Applications`.
  3. macOS zaproponował instalację Rosetta 2; zainstalowano ją.
  4. Uruchomiono PsychoPy. Otworzyły się okna Builder, Coder i Runner.
- **Wersja:** na ekranie widoczna `2025.2.4beta`; potwierdzić ją dodatkowo przez *About*.
- **Jak sprawdzić:** PsychoPy uruchamia się bez komunikatu o błędzie i pozwala otworzyć Coder.
- **Typowe problemy:** w przypadku błędu `Launch error (py2app)` sprawdzić plik `~/.psychopy3/last_app_load.log`. Nie przyjmować, że każda wersja instalatora zadziała na tym samym Macu.
- **Zrzut ekranu:** `docs/img/1-1-psychopy.png` (załączono).

### Krok 1.2 – Uruchomienie demo PsychoPy

**Autor:** DK · **Data:** 2026-10-12 · **Status:** wykonano

- **Cel:** sprawdzić, czy działa uruchamianie skryptu i wyświetlanie bodźców.
- **Gdzie:** PsychoPy → `Demos` → `basic` → `hello_world.py` (otwiera się w Coder).
- **Czynności:** wybrano demo `hello_world.py`, a następnie zielony przycisk **Run**.
- **Wynik:** uruchomiło się osobne okno zawierające napisy `Hello world!` i `¡Hola mundo!`.
- **Jak sprawdzić:** ponownie uruchomić demo i sprawdzić, czy wyświetla oba napisy.
- **Zrzut ekranu:** `docs/img/1-2-demo.png` (załączono).

### Krok 1.3 – Symulator biosygnałów: test i nagranie

**Odpowiedzialny:** OS · **Data:** 2026-10-12 · **Status:** nagranie wykonane; analiza do uzupełnienia

- **Cel:** sprawdzić symulator i zapisać 60 s danych w formacie JSONL.
- **Pliki wejściowe:** `symulator_biosygnalow.py` z archiwum „Kod do laboratorium – symulator i klient startowy”. W pakiecie jest również `klient_startowy.py`, ale do nagrywania w trybie `file` nie był potrzebny.
- **Gdzie:** Terminal, w rozpakowanym katalogu zawierającym `symulator_biosygnalow.py`.
- **Wymagania:** Python 3.8+; skrypt nie wymaga dodatkowych bibliotek.
- **Test działania:**

```bash
python3 symulator_biosygnalow.py --mode stdout --scenario stres --duration 5
```

- **Wynik testu:** na ekranie pojawiły się kolejne obiekty JSON zawierające m.in. `t`, `hr`, `rr_ms`, `eda_us`, `focus`, `quality`, `scenario`. Jedna próbka (`t=4.75`) miała `quality="lost"` i wartości `null` dla części sygnałów, co pokazuje symulowaną utratę danych.
- **Nagranie do pliku:**

```bash
python3 symulator_biosygnalow.py --mode file --duration 60 --out nagranie.jsonl
```

- **Wynik:** komunikat `Zapisano nagranie.jsonl`; rozmiar według `ls -lh`: około **28 KB**. Polecenie `wc -l nagranie.jsonl` pokazało **241** wierszy. Przy domyślnych 4 Hz są to próbki od `t=0` do `t=60` co 0,25 s.
- **Sprawdzenie:**

```bash
ls -lh nagranie.jsonl
wc -l nagranie.jsonl
head -n 2 nagranie.jsonl
python3 --version
```

- **Potwierdzenie wyniku w Terminalu:** `docs/symulator_wynik_terminala.txt` (zapis tekstowy, bez zrzutu ekranu).
- **Dalsze działanie:** OS kopiuje **oryginalny** plik `nagranie.jsonl` do `sample_data/nagranie.jsonl`, sprawdza liczbę rekordów, zakres `t`, wartości `quality` i występowanie `null`, a wyniki zapisuje w `docs/analiza_nagrania.md`. Ten Build Book **nie zawiera** samego nagrania. Nie zastępować go danymi ręcznie utworzonymi.

#### Opis pól nagrania

| Pole | Znaczenie | Jednostka / uwaga |
| --- | --- | --- |
| `t` | Czas od początku symulacji | s |
| `seq` | Numer kolejnej próbki | liczba całkowita |
| `hr` | Symulowane tętno | uderzenia/min |
| `rr_ms` | Symulowany odstęp między kolejnymi uderzeniami serca | ms |
| `eda_us` | Symulowane przewodnictwo skóry (EDA/GSR) | µS |
| `focus` | Umowny, symulowany wskaźnik skupienia | wartość bez jednostki; nie jest pomiarem eye-trackera |
| `quality` | Status jakości próbki | np. `ok` lub `lost` |
| `scenario` | Etykieta scenariusza generującego sygnał | wyłącznie do oceny rozwiązania, **nie** do sterowania interfejsem |

**Czego symulator nie wysyła:** współrzędnych wzroku `gaze_x`, `gaze_y`, ważności śledzenia `gaze_valid`, średnicy źrenicy `pupil_mm` ani danych oddechowych `resp`. Wartość `focus` nie zastępuje żadnego z tych pomiarów. Nie można na podstawie tych danych oceniać rzeczywistych emocji uczestnika.

### Krok 1.4 – Elementy środowiska wymagane w dalszych etapach

**Odpowiedzialny:** OS · **Data przydzielenia:** 2026-10-12 · **Status:** do sprawdzenia przez OS

- **pylsl:** do zainstalowania w środowisku PsychoPy przez menedżer pakietów/wtyczek; zanotować wersję i przeprowadzić test w KL-B.
- **LabRecorder:** do pobrania z wydań `labstreaminglayer/App-LabRecorder`; zanotować wersję, sprawdzić nagrywanie LSL do XDF.
- **MouseGaze:** do skonfigurowania w eksperymencie PsychoPy (Eyetracking) do testów bez aparatury LBK.
- **EyeLogic, EyeLogic Server, sterownik GSR/EDA:** potrzebne na stanowisku LBK; instalację i wersje potwierdzić z prowadzącym.
- **Monitor Center:** zapisać rzeczywistą rozdzielczość, odświeżanie monitora, wielkość i odległość oglądania przed przygotowaniem bodźców.
- **Nakładka analityczna:** w późniejszym etapie osobne środowisko Python oraz `requirements.txt` dla bibliotek, np. pandas, pyxdf, h5py, matplotlib.
- **Jak sprawdzić:** podczas KL-B lub wizyty w LBK wykonać osobny test każdej części i dopisać wynik. Brak tych testów nie oznacza, że komponenty działają.

## 14. Odstępstwa i pytania do LBK

W opisie pakietu P3 podano, że obraz jest wyświetlany przez **4000 ms**, a następnie szary ekran przez **1000 ms**; przewidziano oceny SAM i pomiar GSR/EDA. Warstwa adaptacyjna może wydłużać przerwę lub wprowadzać ekran neutralny, ale **nie może zmieniać czasu ekspozycji obrazu ani kolejności bodźców**. Poniższe kwestie wymagają uzgodnienia z LBK i nie zostały samodzielnie rozstrzygnięte.

### Pytanie 14.1 – Liczba prób i kontrbalansowanie

**Autor:** DK · **Data:** 2026-10-12 · **Status:** otwarte  
W opisie pojawiają się cztery obrazy, ale nie ma ostatecznej liczby prób. Ile powtórzeń należy zastosować i jak dokładnie ustalić kolejność?  


### Pytanie 14.2 – Powrót EDA do linii bazowej

**Odpowiedzialny za pytanie:** OS · **Data przydzielenia:** 2026-10-12 · **Status:** propozycja pytania, do sprawdzenia przez OS  
Jak wyznaczać linię bazową EDA, jaki przyjąć próg powrotu do niej i maksymalny czas czekania przed kolejnym bodźcem?  


### Pytanie 14.3 – Utrata śledzenia wzroku

**Autor:** DK · **Data:** 2026-10-12 · **Status:** otwarte  
Jaki czas utraty sygnału powinien wywoływać pauzę? Czy przerwę wolno wprowadzać wyłącznie między próbami, żeby zachować 4000 ms ekspozycji obrazu?  


### Pytanie 14.4 – Aparatura i synchronizacja

**Odpowiedzialny za pytanie:** OS · **Data przydzielenia:** 2026-10-12 · **Status:** propozycja pytania, do sprawdzenia przez OS  
Jakie są nazwy, typy, częstotliwości strumieni LSL dla EDA i EyeLogic na stanowisku LBK? Jakie są wymagania dotyczące kalibracji?  


### Pytanie 14.5 – Wersja PsychoPy

**Autor:** DK · **Data:** 2026-10-12 · **Status:** otwarte  
Na Macu uruchomiono instalator 2025.2.4, a aplikacja pokazała oznaczenie 2025.2.4beta. Jaką dokładną wspólną wersję należy przyjąć dla zespołu i stanowiska LBK?  



