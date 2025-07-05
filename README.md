# Projekt-Koncowy-Analiza-danych
Projekt Analiza Danych mieszkania 
Analiza cen mieszkań — wynajem i zakup (2023–2024)

Ten projekt służy do analizy danych dotyczących wynajmu i zakupu mieszkań w Polsce w latach 2023–2024. Analiza obejmuje czyszczenie danych, imputację braków, generowanie wizualizacji oraz wyliczenia wskaźników takich jak ROI czy korelacje między zmiennymi.

Zawartość

Automatyczne wykrywanie kodowania i separatorów plików CSV

Czyszczenie i standaryzacja danych (kolumna city, brakujące wartości)

Statystyki opisowe i wizualizacje (heatmapy, boxploty, słupki)

Obliczanie opłacalności inwestycji (ROI)

Korelacje (Pearson, Spearman, Cramér's V)

Obsługa danych z Google Drive (Google Colab) lub lokalnie

Wymagania

Aby uruchomić projekt, potrzebujesz:

Python 3.7+

Biblioteki:

pandas

numpy

matplotlib

seaborn

chardet

scipy

google.colab (opcjonalnie, jeśli korzystasz z Colab)
# Slajd 1: Tytuł  
**Analiza cen mieszkań w Polsce**  
Adam, Gdynia • 27.06.2025  

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
