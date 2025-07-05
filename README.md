# Projekt-Koncowy-Analiza-danych
Projekt Analiza Danych mieszkania 
Analiza cen mieszkań — wynajem i zakup (2023–2024)


# Slajd 1: Tytuł  
**Projekt-Koncowy-Analiza-danych
Projekt Analiza Danych mieszkania 
Analiza cen mieszkań — wynajem i zakup (2023–2024)

---

# Slajd 2: Agenda  
1. Cel analizy  
2. Dane i wstępne przetwarzanie  
3. Eksploracyjna analiza danych  
4. Model regresji i predykcja  
5. Analiza ceny za m²  
6. Kluczowe wnioski  
7. Rekomendacje  

---

# Slajd 3: Cel analizy
- Cel analizy 
Celem projektu było zbadanie rynku mieszkaniowego w Polsce na podstawie danych o wynajmie oraz zakupie mieszkań w latach 2023–2024.
Założeniem było zrosumieć i porównać struktury ofert, poziomu cen i różnic międzymiastowych, a także ocena opłacalności inwestycji w najem względem zakupu. I jak kształtują się ceny mieszkań na rynku sprzedaży i wynajmu
-Do analizy wykorzystano zbiór ofert z kilkunastu miast (m.in. Warszawa, Kraków, Gdańsk, Wrocław) zawierający informacje o powierzchni, liczbie pokoi, cenie, odległościach od kluczowych punktów usługowych oraz cechach budynku.
- Weryfikować, które cechy (powierzchnia, lokalizacja) mają największy wpływ  
- Przygotować rekomendacje dla pośredników i inwestorów

---

# Slajd 4: Dane i preprocessing  
- **Źródła**:  
  - apartments_pl_2024_06.csv, apartments_pl_2023_12.csv (zakup)  
  - apartments_rent_pl_2024_06.csv (wynajem)  
- **Wczytywanie**: funkcja `read_csv_with_fallback()`  
  - Automatyczne wykrywanie kodowania (chardet)  
  - Separator `;` lub `,`  
- **Czyszczenie**:  
  - Standaryzacja nazw miast (`clean_city_column()`)
  - Przed dalszymi krokami wykonano:
    •	wykrycie i kwantyfikację braków danych,
    •	imputację brakujących wartości metodą hot-deck,
  - Identyfikacja braków i imputacja:  
    - Mediana dla liczb  
    - Hot-deck dla `area` w ramach miasta
- Wczytywanie:
- Kod:  
  ```python
  dfs[key] = read_csv_with_fallback(path)
- Czyszczenie miast:
- Kod:  
  ```python
  def clean_city_column(df):
    df['city'] = df['city'].str.strip().str.title()
- Imputacja:
- Kod:  
  ```python
  df[num_cols] = df[num_cols].fillna(df[num_cols].median())

---

# Slajd 5: Braki w danych  
| Zbiór danych      | Wizualizacja                   | Cel                                  |
|-------------------|--------------------------------|--------------------------------------|
| Wynajem           | heatmap(isnull)                | Lokalizacja kolumn z brakami         |
| Zakup 2024        | heatmap(isnull)                | Skala braków przed imputacją         |
| Zakup 2023        | heatmap(isnull)                | Wskaźnik jakości danych              |

- Heatmapy braków:
- Kod:  
  ```python
  sns.heatmap(df.isnull(), cbar=False, cmap="coolwarm")

**Notatka Braki w danych wynajmu:**  
- Opis wykresu braki w danych wynajmu
- Ten wykres jest tzw. mapą braków danych (missing values map), która pokazuje gdzie i w jakich zmiennych brakuje informacji w bazie dotyczącej mieszkań na wynajem. Wykres pozwala ocenić, czy problem brakujących wartości jest rozproszony czy skumulowany w konkretnych kolumnach.
- Opis do prezentacji 
Na tym wykresie widzimy, które cechy mieszkań zawierają braki danych. Każda kolumna to jedna zmienna, a każdy wiersz to jedna oferta. Czerwone kreski wskazują miejsca, gdzie brakuje informacji — na przykład, dla wielu mieszkań nie podano typu budynku czy formy własności. Z kolei dane o lokalizacji, czyli długość i szerokość geograficzna, są niemal kompletne. Dzięki temu możemy lepiej zaplanować dalszą analizę – czy należy uzupełniać brakujące dane, czy pomijać niektóre zmienne.”
- Wizualizacja:

**Notatka Braki w danych zakupu 2024:**  
- Opis wykresu braki w danych zakupu 2024
- To wizualizacja brakujących danych (missing values plot) w zbiorze dotyczącym ofert sprzedaży mieszkań w roku 2024. Pokazuje dokładnie, które cechy mieszkań zawierają braki danych i w których rekordach te braki występują.
- Opis do prezentacji
- Ten wykres przedstawia jakość danych o mieszkaniach na sprzedaż. Pozioma oś pokazuje różne cechy ofert, a pionowa to poszczególne mieszkania. Czerwone kreski wskazują, gdzie brakuje informacji — np. dla wielu ofert nie podano liczby pokoi czy formy własności. Dzięki temu możemy szybko ocenić, które kolumny wymagają uzupełnienia, a które są dobrze wypełnione, co pomoże nam poprawić jakość analiz.
-Wizualizacja:

**Notatka Braki w danych 2023:**  
- Opis wykresu braki w danych zakupu 2023
- Wizualizacja pokazuje rozmieszczenie brakujących danych w zbiorze ofert sprzedaży mieszkań. Dzięki niej można szybko zobaczyć, w których zmiennych (kolumnach) najczęściej występują braki, i w jakich wierszach one się pojawiają.
- Opis do prezentacji
- Na tym wykresie widzimy, gdzie w danych o mieszkaniach brakuje informacji. Każda kolumna to cecha, a każda kreska to brakująca wartość dla jednej oferty. Widać, że dane techniczne takie jak metraż czy liczba pokoi są często nieuzupełnione, natomiast lokalizacja jest prawie kompletna. Taka analiza pomaga nam zdecydować, które zmienne możemy wykorzystać dalej, a które wymagają uzupełnienia lub odfiltrowania.
- Wizualizacja:

---

# Slajd 6: Boxplot – ceny wg miast  
- **Zakup 2024** vs **Wynajem**  
- `sns.boxplot(x="city", y="price", ...)`  
- Cel:  
  - Pokazać rozrzut i outliery  
  - Porównać mediany i zakres cen  

-  Boxplot – ceny wg miast:
- Kod:  
  ```python
  sns.boxplot(x="city", y="price", data=df_buy_2024)
- Wizualizacje:

**Notatka Boxplot – ceny wg miast:**
- Opis histogramu
- Histogram który ilustruje porównanie cen zakupu mieszkań w różnych miastach Polski. Każde „pudełko” reprezentuje jedno miasto i pokazuje, jak bardzo zróżnicowane są ceny mieszkań w jego obrębie — od najniższych do najwyższych.
- Opis do prezentacji
- Tutaj widzimy wykres pudełkowy, który porównuje rozkład cen mieszkań w 15 miastach Polski. Każde pudełko obrazuje ceny od najtańszych do najdroższych – środkowa linia to mediana, czyli najczęściej spotykana cena. Im dłuższe pudełko, tym większe zróżnicowanie w ofertach. Warto tutaj zwrócić uwagę na Warszawę – jest nie tylko najdroższa, ale też bardzo zmienna, co widać po licznych kropkach z bardzo wysokimi cenami. Dla porównania – w Radomiu ceny są bardziej spójne i znacznie niższe.


---
# Slajd 7: Korelacje (zakup 2024)  
- Heatmapa korelacji zmiennych liczbowych  
- Kod:  
  ```python
  df_corr = numeric_df.corr()
  sns.heatmap(df_corr, annot=True, cmap="coolwarm")
-----

# Slajd 8: Minimalne i maksymalne ceny
Porównanie minimalnych i maksymalnych cen zakupu mieszkań w 2024 roku

Wykres słupkowy barplot z adnotacjami

Wnioski:

Znaczne różnice w rozrzucie cen pomiędzy miastami

Warszawa i Kraków – najwyższe ceny maksymalne
----

#Slajd 9: Kurtoza cen wg miast
Kurtoza: miara „spiczastości” rozkładu

Obliczona dla price w danych zakupu 2024

Wnioski:

Miasta o wysokiej kurtozie mają więcej ekstremalnych cen

Pozwala identyfikować rynki niestabilne lub spekulacyjne
----

#Slajd 10: Statystyki opisowe (heatmapy)
Heatmapy statystyk describe()

Dla zbiorów: wynajem, zakup 2024 i 2023

Ułatwiają szybkie porównanie rozkładów cech liczbowych

Skala kolorów: min–max dla każdej cechy
-----
#Slajd 11: ROI – opłacalność wynajmu
Mediana czynszu / Mediana ceny zakupu × 100%

ROI wyrażony procentowo – ile wynosi roczny zwrot z inwestycji

Najbardziej opłacalne miasta:

Gorzów Wlkp., Bydgoszcz, Rzeszów

Miasta z niskim ROI:

Warszawa, Gdańsk, Kraków
-----
#Slajd 12: Cena za m² – statystyki i wykresy
Dodano kolumnę price_per_m2

Statystyki: średnia, mediana, min, max

Wykresy:

Barplot średniej ceny za m²

Boxplot rozkładu cen za m² wg miast

Wnioski:

Najwyższe stawki: Warszawa, Gdańsk

Duża zmienność w miastach turystycznych
-----

#Slajd 13: Rozkład powierzchni mieszkań
Histogram squareMeters po imputacji

Główne wnioski:

Większość mieszkań ma powierzchnię 30–60 m²

Pojedyncze wartości skrajne (ponad 100 m²)
-----

#Slajd 14: Korelacje liczbowych – Pearson & Spearman
Pearson – liniowa korelacja między zmiennymi liczbowymi

Spearman – korelacja rang (monotoniczna)

Wnioski:

Silna dodatnia korelacja: cena vs powierzchnia

Zmienne ilościowe mają niską korelację między sobą poza price
------

#Slajd 15: Cramér’s V – zmienne kategoryczne
Miernik siły asocjacji między cechami nominalnymi

Obliczony dla par kolumn kategorycznych

Przykłady: city, district, street

Ciepłe kolory = silniejsze powiązanie
-----

#Slajd 16: Kluczowe wnioski
Dane wymagają czyszczenia i imputacji

Miasta różnią się znacząco pod względem cen

ROI może służyć jako wskaźnik inwestycyjny

Cena za m² – lepszy wskaźnik niż całkowita cena

Warto wykorzystywać metryki statystyczne (kurtoza, korelacja) do identyfikacji outlierów
------

#Slajd 17: Rekomendacje
Dla pośredników:

Skupienie na miastach o niskim ROI przy sprzedaży

Analiza outlierów dla nietypowych ofert

Dla inwestorów:

Priorytet dla miast o wysokim ROI

Uwzględnianie cen za m², nie tylko całkowitych cen

----


