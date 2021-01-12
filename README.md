# Financial_Statements_Text_Analytics
## About
Notebooks to import, pre-process and merge financial data from SimFin API and textual data from 10-k statements downloaded from the SEC website.

## Data sets
Two sources of data were used:

### Textual data
#### Downloading and organizing SEC files
The SEC quarterly updates the Financial Statement and Notes Data Sets that reunites all textual and numerical data from financial statements, including notes sections (https://www.sec.gov/dera/data/financial-statement-and-notes-data-set.html). Download the data sets that covers the period relevant to the analysis. After creating a new folder to store the target data, unzip the dowloaded folders and extract only the sub.tsv and txt.tsv files for each quarter. Rename each file using the following scheme: **Year_Quarter_OriginalFileName.Extension** (i.e. 2012_q1_sub.tsv).
```
2012_q1_sub.tsv
2012_q1_txt.tsv
2012_q2_sub.tsv
2012_q2_txt.tsv
2012_q3_sub.tsv
2012_q3_txt.tsv
2012_q4_sub.tsv
2012_q4_txt.tsv
2013_q1_sub.tsv
2013_q1_txt.tsv
.
.
.
``` 

#### Importing SEC files as pandas data sets and storing as a pickle file

Run [Import_Textual_Data.ipynb](./Import_Textual_Data.ipynb) notebook.
Depending on the time range of the analysis, this step can take a while to be completed.


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

I you face any trouble using these notebooks, please e-mail me at alysson@usf.edu

#### Financial Ratios Formulas

| Name	| Formula |
| ----- | ------- |
| Total Debt |	Short Term Debt + Long Term Debt |
| Size	| Log(Total Assets) |
| Leverage | Total Debt / Total Assets |
| EBIT | Revenue - Cost of Revenue - Selling, General & Administrative |
| EBITDA | EBIT + Depreciation & Amortization |
| ROA |	Income (Loss) from Continuing Operations/ Total Assets |
| ROE |	Income (Loss) from Continuing Operations/ Total Equity |
| ROIC | Income (Loss) from Continuing Operations / (Total Assets - Cash & Equivalents - LT Investments & Receivables) |
| Gross Profit Margin |	Gross Profit / Revenue |
| EBIT Margin |	EBIT / Revenue |
| Net Profit Margin |	Income (Loss) from Continuing Operations/ Revenue |
| Asset Turnover |	Revenue / Property, Plant & Equipment, Net |
| Financial Leverage |	Income Continuing Oper. * Total Assets / Total Equity / (Income Continuing Oper. - Non-Oper. Income (Loss)) |
| Operating Leverage |	Gross Profit / (Gross Profit - Selling, General & Administrative) |
| Days of Sales Outstanding |	(Accounts & Notes Receivable * 360) / Revenue |
| Days of Payments Outstanding |	(Payables & Accruals * 360) / Revenue |
| Days of Inventory Outstanding |	(Inventories * 360) / Cost of Revenue |
