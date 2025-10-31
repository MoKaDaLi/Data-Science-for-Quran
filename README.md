# Quranic Text Analysis

## Project Description
This project performs an exploratory data analysis on the MASAQ dataset, which contains morphological and syntactic annotations of the Holy Quran. The goal is to analyze word frequencies, identify and analyze proper nouns, and specifically track the occurrences and distribution of the name "Allah" throughout the Surahs.

## Setup and Installation
The analysis requires several Python libraries for data manipulation, analysis, and visualization. These include `pandas`, `numpy`, `matplotlib`, `scikit-learn`, `networkx`, `nltk`, `camel-tools`, `pyarabic`, `arabic-reshaper`, `python-bidi`, and `wordcloud`. The necessary libraries are installed at the beginning of the notebook.

## Data Loading
The MASAQ dataset, expected to be in a JSON format (`MASAQ_dataset.json`), is loaded into a pandas DataFrame. A custom function `load_masaq` is used to handle different JSON structures (list of dicts or NDJSON).

## Data Processing
The raw data is processed to select relevant columns (ID, Sura_No, Verse_No, Word_No, Without_Diacritics, Morph_Type). Duplicates are handled by prioritizing 'Stem' morphology type and keeping the first occurrence for each unique word position (Sura, Verse, Word, ID). The data types of numerical columns are also cleaned. A subset of the data containing only noun tags is created and further filtered to isolate proper nouns.

## Analysis
Various analyses are performed on the processed data:

*   **Overall Word Frequency:** Counting the occurrences of all unique words (without diacritics) to understand the most frequent terms in the Quran.
*   **Noun Word Frequency:** Counting the occurrences of unique words specifically tagged as nouns.
*   **Segmented Noun Frequency:** Analyzing the frequency of segmented noun forms.
*   **Proper Noun Analysis:** Filtering for words tagged as proper nouns (`NOUN_PROP`) and analyzing their frequencies and distribution.
*   **"Allah" Occurrences:** Specifically tracking the occurrences of the word "الله" (Allah), identifying its unique positions (Sura, Verse), and analyzing its distribution across Surahs.

## Key Findings and Visualizations
The analysis generates several outputs, including:

*   Ranked lists of overall word frequencies, noun word frequencies, and proper noun frequencies.
*   A distribution of unique "Allah" occurrences per Sura.
*   Visualizations such as histograms and scatter plots showing the distribution of data, and bar charts and word clouds illustrating word frequencies. For example, a bar chart shows the top 20 most frequent noun words, and a word cloud visualizes the overall word frequencies. Plots also show the distribution and occurrences of "Allah" across Surahs.

## Code Explanation

Here's a breakdown of the significant code sections in the notebook:

### 📦 Library Installation and Verification (Cells FZAJvW2tHmjo & yqDcjpRjHwwO)
- **Purpose:** Ensure all necessary Python libraries are installed and compatible versions are available.
- **Libraries:** `sys`, `importlib`, `subprocess`, `pandas`, `numpy`, `matplotlib`, `scikit-learn`, `networkx`.
- **Operations:** Checks for required packages (`pandas`, `numpy`, etc.). If a package is missing, it is installed using `pip` via `subprocess`. Basic imports and version checks are performed. A simple plot using `matplotlib.pyplot` is generated to confirm its functionality.

### ⬆️ Data Upload (Cell f4e3c491)
- **Purpose:** Upload the MASAQ dataset JSON file from the local machine to the Colab environment.
- **Libraries:** `google.colab.files`, `os`.
- **Operations:** Uses `files.upload()` to open a file picker dialog. Reads the uploaded file's content and potentially renames it to the expected filename (`MASAQ_dataset.json`). *Note: This cell failed during execution, likely due to user interruption during the file upload process.*

### 📖 Loading and Initial Processing of MASAQ Dataset (Cell crXu3Tnu3254)
- **Purpose:** Load the JSON dataset into a pandas DataFrame and perform initial cleaning and de-duplication.
- **Libraries:** `json`, `pandas`, `pathlib.Path`.
- **Operations:** Defines a path to the JSON file. A custom `load_masaq` function handles different JSON formats. Selects a subset of needed columns (`Sura_No`, `Verse_No`, etc.). Converts relevant columns to numeric types. Handles missing string values. Creates a categorical column `morph_priority` to prioritize 'Stem' when de-duplicating. Sorts data and drops duplicates based on word position (`Sura_No`, `Verse_No`, `Word_No`, `ID`), keeping the first entry based on morphology priority. Reorganizes columns and resets the index. Prints statistics about the processed data and displays the head.

### 📊 Overall Word Frequency Analysis (Cell OO_aD14PWzed)
- **Purpose:** Calculate and display the frequency of all unique words (without diacritics) in the processed dataset.
- **Libraries:** `pandas`.
- **Operations:** Uses `result["Without_Diacritics"].value_counts()` to count unique words. Resets the index and renames columns to 'Word' and 'Occurrences'. Sorts the results by frequency in descending order. Calculates and prints the total number of tokens, unique words, and lexical diversity ratio. Displays the top 20 most frequent words. Exports the full frequency list to a CSV file.

### ✨ Identifying and Displaying Morphological Tags (Cells BrVd4BsqyZkF & 1YpqqajPy3za)
- **Purpose:** Inspect the different morphological tags present in the raw data and specifically identify those related to nouns.
- **Libraries:** `pandas`.
- **Operations:** Accesses the `Morph_Tag` column in `df_raw`. Prints and displays all unique tags. Filters this list to create `noun_tags`, containing tags that include "NOUN" (case-insensitive). Prints and displays the identified noun tags.

### 🗂️ Filtering for Nouns (Cell ac65519b)
- **Purpose:** Create a new DataFrame containing only the rows that correspond to noun tags.
- **Libraries:** `pandas`.
- **Operations:** Filters the `df_raw` DataFrame using the `noun_tags` list created previously. Creates a new DataFrame `df_nouns`. Prints the number of rows before and after filtering to show the count of noun entries. Displays the head of the `df_nouns` DataFrame.

### 📊 Noun Word Frequency Analysis (Cell 333a2c9c)
- **Purpose:** Calculate and display the frequency of unique noun words (without diacritics).
- **Libraries:** `pandas`.
- **Operations:** Similar to overall word frequency analysis, uses `df_nouns["Without_Diacritics"].value_counts()`. Renames columns and sorts by frequency. Displays the top 20 most frequent noun words. Exports the noun word frequencies to a CSV file.

### 📊 Segmented Noun Frequency Analysis (Cell 5a8239f1)
- **Purpose:** Calculate and display the frequency of unique *segmented* noun words.
- **Libraries:** `pandas`.
- **Operations:** Uses `df_nouns["Segmented_Word"].value_counts()`. Renames columns and sorts by frequency. Displays the top 20 most frequent segmented noun words. Exports the segmented noun frequencies to a CSV file.

### 📊 Noun DataFrame Summary (Cell 557ecdab)
- **Purpose:** Display summary statistics and information about the `df_nouns` DataFrame.
- **Libraries:** `pandas`.
- **Operations:** Uses `df_nouns.describe()` to show descriptive statistics for numerical columns. Uses `df_nouns.info()` to display the column names, non-null counts, and data types.

### 🗂️ Filtering for Proper Nouns (Cell 1bF8OejjaJQZ)
- **Purpose:** Create a new DataFrame specifically for proper nouns.
- **Libraries:** `pandas`.
- **Operations:** Filters `df_raw` to include only rows where `Morph_Tag` is exactly "NOUN_PROP". Selects specific columns (`ID`, `Sura_No`, `Verse_No`, `Word_No`, `Without_Diacritics`). Cleans and sorts the data. Prints the total count of proper nouns and the number of Surahs they appear in. Displays the head of the `df_nouns_prop` DataFrame. Exports the proper noun data to CSV and Parquet files.

### 📊 Proper Noun Frequency Analysis (Cells grWPW3rGrQsz & Yz3FS-RWucxU)
- **Purpose:** Calculate and rank the frequency of proper nouns and identify the most/least frequent.
- **Libraries:** `pandas`.
- **Operations:** Uses `df_nouns_prop["Without_Diacritics"].value_counts()`. Renames columns to 'Word' and 'Occurrences'. Calculates both ascending and descending ranks based on occurrences using `.rank(method="dense")`. Sorts the DataFrame by ascending occurrences for viewing the rarest words. Displays the top 20 rarest and top 20 most frequent proper nouns. Exports the ranked list to a CSV file.

### 📍 Tracking "Allah" Occurrences (Cell d4iOp01sDnik)
- **Purpose:** Find all occurrences of the word "الله" (Allah) and list their unique positions in the Quran.
- **Libraries:** `pandas`.
- **Operations:** Filters `df_raw` where `Without_Diacritics` is "الله". Removes duplicate occurrences based on `ID` to get unique positions. Selects the `ID`, `Sura_No`, and `Verse_No` columns. Displays the first 10 unique occurrences and prints the total count. Exports the unique occurrences and their positions to a CSV file.

### 📈 Analyzing "Allah" Distribution by Sura (Cell e9407c8d)
- **Purpose:** Calculate how many unique occurrences of "Allah" are found in each Sura.
- **Libraries:** `pandas`.
- **Operations:** Uses `allah_positions["Sura_No"].value_counts()` to count unique occurrences per Sura. Resets index, renames columns, and sorts by Sura number. Displays the distribution table. Exports the distribution data to a CSV file.

### 🥇 Identifying Surahs with Most "Allah" Occurrences (Cell b952c329)
- **Purpose:** Find and list the Surahs that contain the highest number of unique "Allah" occurrences.
- **Libraries:** `pandas`.
- **Operations:** Sorts the `allah_sura_distribution` DataFrame by 'Unique_Occurrences' in descending order. Displays the top 10 Surahs with the most occurrences. Exports this list to a CSV file.

### 📊 Visualizations of "Allah" Distribution (Cells YiHlDdQuIZ-N & ZwGfk_qhIT1j)
- **Purpose:** Visualize the distribution and unique occurrences of "Allah" across Surahs.
- **Libraries:** `matplotlib.pyplot`.
- **Operations:** Generates a line plot showing the trend of unique occurrences across Sura numbers. Generates a scatter plot showing the relationship between Sura number and unique occurrences.

### Arabic Text Handling Setup (Cells hTP0JnFTYYyL, X77ZO1KkZa0z, QxmIxw58ZhzC, & ui7FZfSRXyDw)
- **Purpose:** Install necessary libraries for handling Arabic text display (reshaping, bidi algorithm) and configure Matplotlib to use an Arabic font.
- **Libraries:** `arabic-reshaper`, `python-bidi`, `wordcloud`, `matplotlib.pyplot`, `matplotlib.font_manager`, `wget`, `fc-cache`, `IPython.display`.
- **Operations:** Installs `arabic-reshaper`, `python-bidi`, and `wordcloud` using `pip`. Downloads an Arabic font (`NotoNaskhArabic-Regular.ttf`). Configures Matplotlib's font properties to use the Arabic font. Defines an `ar_display` function using `arabic_reshaper` and `bidi.algorithm.get_display` to correctly render Arabic text. *Note: This setup is consolidated and demonstrated in the final `ui7FZfSRXyDw` cell.*

### 📊 Visualizations with Arabic Text (Cell ui7FZfSRXyDw - Part 2: Histogram, Wordcloud, RTL Test)
- **Purpose:** Generate visualizations (histogram, wordcloud) that correctly display Arabic text, and include a test for RTL display.
- **Libraries:** `matplotlib.pyplot`, `matplotlib.font_manager`, `wordcloud`, `arabic_reshaper`, `bidi.algorithm`.
- **Operations:** Prepares data for visualization by applying the `ar_display` function to Arabic words. Generates a bar chart of the top 20 overall most frequent words, ensuring Arabic labels on the x-axis are displayed correctly using the configured font. Creates a word cloud from the overall word frequencies, also using the Arabic font and `ar_display`. Includes a simple text plot to demonstrate correct Right-to-Left (RTL) rendering of an Arabic phrase.


## Key Findings and Visualizations

The analysis of the MASAQ dataset revealed several interesting insights into the Quranic text, particularly concerning word frequencies, noun usage, proper nouns, and the distribution of the name "Allah".

### Overall Word Frequencies

- **Findings:** The analysis of overall word frequencies (Cell OO_aD14PWzed and ui7FZfSRXyDw) identified the most common words in the Quran (without diacritics). The top 20 list is headed by prepositions, conjunctions, and common particles (e.g., من, الله, في, ما, إن). This provides a foundational understanding of the most frequently used linguistic elements. The total number of tokens and unique words were calculated, along with the lexical diversity ratio.
- **Visualizations:**
    - **Histogram of Word Frequencies (Cell ui7FZfSRXyDw):** A bar chart displays the top 20 most frequent words. The x-axis shows the Arabic words (correctly rendered using the configured Arabic font and `ar_display`), and the y-axis shows their occurrence counts. This visualization makes it easy to compare the relative frequencies of the most common words.
    - **Word Cloud of Word Frequencies (Cell ui7FZfSRXyDw):** A word cloud visualizes the overall word frequencies, where the size of each word is proportional to its frequency. This provides a visually appealing summary of the most prominent terms in the dataset. Arabic text is correctly rendered.

### Noun Analysis

- **Findings:** By filtering for entries tagged as nouns (Cell ac65519b), the analysis focused specifically on nominal terms (Cell 333a2c9c). The most frequent noun words (without diacritics) were identified, with "الله" (Allah) being by far the most frequent, followed by terms like "الأرض" (the earth), "كل" (all), and "الكتاب" (the book). The analysis of segmented noun frequencies (Cell 5a8239f1) showed that even segmented forms of common nouns appear very frequently (e.g., "له", "رب", "أرض"). Summary statistics and DataFrame information for the noun subset (`df_nouns`) were also provided (Cell 557ecdab), giving an overview of the data's structure and numerical ranges for relevant columns.
- **Visualizations:**
    - **Histogram of Top Noun Words (Cell 4e99bca3):** A bar chart visualizes the top 20 most frequent noun words. Similar to the overall word frequency chart, it uses correctly rendered Arabic labels on the x-axis to show the noun words and the y-axis for their occurrence counts. This highlights the most prominent noun concepts.

### Proper Noun Analysis

- **Findings:** The analysis specifically extracted and analyzed words tagged as proper nouns (Cell 1bF8OejjaJQZ, grWPW3rGrQsz, Yz3FS-RWucxU). The count of proper nouns and the number of Surahs they appear in were reported. The most frequent proper noun is overwhelmingly "الله", reinforcing its significance. Other frequent proper nouns include names and attributes like "الرحمن", "الرحيم", "إبراهيم", "موسى", and places or concepts like "القيامة". Ranked lists of proper nouns by frequency (both ascending and descending) were generated, allowing identification of both the most common and the rarest proper nouns.
- **Visualizations:**
    - **Quick Charts (from cells 1bF8OejjaJQZ, grWPW3rGrQsz, Yz3FS-RWucxU outputs):** Various quick charts are automatically generated by Colab after displaying DataFrames, including histograms of numerical columns (ID, Sura_No, Verse_No, Word_No, Occurrences, Ranks) and scatter plots showing relationships between these columns. Violin plots provide insights into the distribution of numerical features across different proper nouns. These visualizations help in understanding the distribution and relationships within the proper noun dataset.

### "Allah" Occurrences and Distribution

- **Findings:** A detailed analysis tracked every unique occurrence of the name "الله" (Allah) by its position (Sura, Verse) (Cell d4iOp01sDnik). The total count of unique occurrences was reported (2153). The distribution of these unique occurrences across different Surahs was calculated (Cell e9407c8d), showing which Surahs contain "Allah" and how many times. The top 10 Surahs with the highest number of "Allah" occurrences were identified (Cell b952c329), with Surah Al-Baqarah (Sura 2) having the highest count.
- **Visualizations:**
    - **Line Plot of Unique Occurrences by Sura (Cell YiHlDdQuIZ-N and quick charts):** A line plot shows the number of unique "Allah" occurrences for each Sura number. This helps visualize the trend of occurrences across the sequential order of Surahs.
    - **Scatter Plot of Unique Occurrences by Sura (Cell ZwGfk_qhIT1j and quick charts):** A scatter plot displays each Sura's number on the x-axis and the count of unique "Allah" occurrences on the y-axis. This allows for visual identification of Surahs with particularly high or low occurrences and observation of any patterns in the distribution.
    - **Histograms of Sura_No and Unique_Occurrences (quick charts):** Histograms show the distribution of Surah numbers present in the `allah_sura_distribution` DataFrame and the distribution of the counts of unique "Allah" occurrences.

Overall, the analysis provides a quantitative perspective on the vocabulary and structure of the Quran based on the MASAQ dataset, highlighting the central presence and distribution of the name "Allah" and identifying the most frequently used terms and noun concepts.
