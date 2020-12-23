# Financial_Statements_Text_Analytics
Application of NLP on financial statements to predict corporate future success

## About
This work proposed to verify if the use of features derived from textual data, when combined with commonly used financial ratios, can help to make better predictions on corporate financial performance. We applied different machine learning techniques using 10-K fillings of US-based companies from 2012 to 2019.

## Data sets
Two sources of data were used:

### Textual data
#### Downloading and organizing SEC files
The SEC quarterly updates the Financial Statement and Notes Data Sets that reunites all textual and numerical data from financial statements, including notes sections (https://www.sec.gov/dera/data/financial-statement-and-notes-data-set.html). Download the data sets that covers the period relevant to the analysis. After creating a new folder to store the target data, unzip the dowloaded folders and extract only the sub.tsv and txt.tsv files for each quarter. Rename each file using the following scheme: **Year_Quarter_OriginalFileName.Extension** (i.e. 2012_Q1_sub.tsv).
```
2012_Q1_sub.tsv
2012_Q1_txt.tsv
2012_Q2_sub.tsv
2012_Q2_txt.tsv
2012_Q3_sub.tsv
2012_Q3_txt.tsv
2012_Q4_sub.tsv
2012_Q4_txt.tsv
2013_Q1_sub.tsv
2013_Q1_txt.tsv
.
.
.
``` 

#### Importing SEC files as pandas dataset and storing as a pickle file

Run [Import_Textual_Data.ipynb](./Import_Textual_Data.ipynb) notebook.
Depending on the time range of the analysis, this step can take a while to be completed.

#### Importing SEC files as pandas dataset and storing as a pickle file


### Financial data
The numerical data was acquired via the SimFin API (https://simfin.com/), that can be installed using the `!pip install simfin` command.
To download all Income Statements, Balance Sheets, and Cash Flow Statement from US-based companies available on this API, you can use the following command:

```
import simfin as sf
from simfin.names import *

sf.set_data_dir('~/simfin_data/')
sf.set_api_key(api_key='free')
df_income = sf.load_income(variant='annual', market='us')
df_balance = sf.load_balance(variant='annual', market='us')
df_cashflow = sf.load_cashflow(variant='annual', market='us')
```

You can also download a list of companies with their respective tickers and the companies' industry segments.

```
df_companies = sf.load_companies(index=TICKER, market='us')
df_industries = sf.load_industries()
```

#### Text pre-processing and merge with numerical data

Run [Merge_Financial_Textual_Data.ipynb](./Merge_Financial_Textual_Data.ipynb) notebook.
Text pre-processing can take a while, specially word2vec.
When running the notebook again, you can skip (comment) the following commands:
```
model = Word2Vec(token_list, size=300, window=5, min_count=2, sample=1e-3, sg=1, iter=5)
```
```
model.save("word2vec.model")
```


and uncomment the following line to import the word2vec previously saved model.

```
#model = Word2Vec.load("word2vec.model")
```
```
#word_vectors = model.wv
```

I you have any trouble using these notebooks, please e-mail me at alysson@usf.edu

