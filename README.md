# Financial_Statements_Text_Analytics
Application of NLP on financial statements to predict corporate future success

## About
This work proposed to verify if the use of features derived from textual data, when combined with commonly used financial ratios, can help to make better predictions on corporate financial performance. We applied different machine learning techniques using 10-K fillings of US-based companies from 2012 to 2019.

## Downloading the required data sets
Two sources of data were used:

### Textual data
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


