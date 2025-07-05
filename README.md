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
- Zrozumieć, jak kształtują się ceny mieszkań na rynku sprzedaży i wynajmu  
- Identyfikować różnice cenowe pomiędzy miastami  
- Weryfikować, które cechy (powierzchnia, lokalizacja) mają największy wpływ  
- Zbudować prosty model predykcyjny ceny  
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
  - Identyfikacja braków i imputacja:  
    - Mediana dla liczb  
    - Hot-deck dla `area` w ramach miasta
Kod:
dfs[key] = read_csv_with_fallback(path)

---

# Slajd 5: Braki w danych  
| Zbiór danych      | Wizualizacja                   | Cel                                  |
|-------------------|--------------------------------|--------------------------------------|
| Wynajem           | heatmap(isnull)                | Lokalizacja kolumn z brakami         |
| Zakup 2024        | heatmap(isnull)                | Skala braków przed imputacją         |
| Zakup 2023        | heatmap(isnull)                | Wskaźnik jakości danych              |

**Speaker notes:**  
„Długie pasy wartości True to kolumny, które wymagają uzupełnienia.”  

---

# Slajd 6: Boxplot – ceny wg miast  
- **Zakup 2024** vs **Wynajem**  
- `sns.boxplot(x="city", y="price", ...)`  
- Cel:  
  - Pokazać rozrzut i outliery  
  - Porównać mediany i zakres cen  

**Speaker notes:**  
„Zwróćcie uwagę na szerokie wąsy i kilka ekstremalnych ofert w Warszawie i Krakowie.”  

---

# Slajd 7: Korelacje (zakup 2024)  
- Heatmapa korelacji zmiennych liczbowych  
- Kod:  
  ```python
  df_corr = numeric_df.corr()
  sns.heatmap(df_corr, annot=True, cmap="coolwarm")

# Slajd 8: Minimalne i maksymalne ceny
Porównanie minimalnych i maksymalnych cen zakupu mieszkań w 2024 roku

Wykres słupkowy barplot z adnotacjami

Wnioski:

Znaczne różnice w rozrzucie cen pomiędzy miastami

Warszawa i Kraków – najwyższe ceny maksymalne

#Slajd 9: Kurtoza cen wg miast
Kurtoza: miara „spiczastości” rozkładu

Obliczona dla price w danych zakupu 2024

Wnioski:

Miasta o wysokiej kurtozie mają więcej ekstremalnych cen

Pozwala identyfikować rynki niestabilne lub spekulacyjne

#Slajd 10: Statystyki opisowe (heatmapy)
Heatmapy statystyk describe()

Dla zbiorów: wynajem, zakup 2024 i 2023

Ułatwiają szybkie porównanie rozkładów cech liczbowych

Skala kolorów: min–max dla każdej cechy

#Slajd 11: ROI – opłacalność wynajmu
Mediana czynszu / Mediana ceny zakupu × 100%

ROI wyrażony procentowo – ile wynosi roczny zwrot z inwestycji

Najbardziej opłacalne miasta:

Gorzów Wlkp., Bydgoszcz, Rzeszów

Miasta z niskim ROI:

Warszawa, Gdańsk, Kraków

#Slajd 12: Cena za m² – statystyki i wykresy
Dodano kolumnę price_per_m2

Statystyki: średnia, mediana, min, max

Wykresy:

Barplot średniej ceny za m²

Boxplot rozkładu cen za m² wg miast

Wnioski:

Najwyższe stawki: Warszawa, Gdańsk

Duża zmienność w miastach turystycznych

#Slajd 13: Rozkład powierzchni mieszkań
Histogram squareMeters po imputacji

Główne wnioski:

Większość mieszkań ma powierzchnię 30–60 m²

Pojedyncze wartości skrajne (ponad 100 m²)

#Slajd 14: Korelacje liczbowych – Pearson & Spearman
Pearson – liniowa korelacja między zmiennymi liczbowymi

Spearman – korelacja rang (monotoniczna)

Wnioski:

Silna dodatnia korelacja: cena vs powierzchnia

Zmienne ilościowe mają niską korelację między sobą poza price

#Slajd 15: Cramér’s V – zmienne kategoryczne
Miernik siły asocjacji między cechami nominalnymi

Obliczony dla par kolumn kategorycznych

Przykłady: city, district, street

Ciepłe kolory = silniejsze powiązanie

#Slajd 16: Kluczowe wnioski
Dane wymagają czyszczenia i imputacji

Miasta różnią się znacząco pod względem cen

ROI może służyć jako wskaźnik inwestycyjny

Cena za m² – lepszy wskaźnik niż całkowita cena

Warto wykorzystywać metryki statystyczne (kurtoza, korelacja) do identyfikacji outlierów

#Slajd 17: Rekomendacje
Dla pośredników:

Skupienie na miastach o niskim ROI przy sprzedaży

Analiza outlierów dla nietypowych ofert

Dla inwestorów:

Priorytet dla miast o wysokim ROI

Uwzględnianie cen za m², nie tylko całkowitych cen


