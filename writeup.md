# China's Tourism: Project Process

Table of Contents
1. [Data Collection](#collection)
2. [Data Cleaning](#cleaning)
3. [Analysis & Insights](#analysis)
4. [Dataset Schema & Info](#info)

## Data Collection <a name="collection"></a> #collection

Data was collected from the National Bureau of Statistics of China's (NBS) [English language site](https://data.stats.gov.cn/english/).

The NBS data was originally 4 separate CSV files that were imported and combined in Microsoft Excel.

The English titles were cross referenced with the original titles from the Bureau's [Chinese language site](https://data.stats.gov.cn/staticreq.htm). The titles were renamed to be more concise before cleaning in Jupyter Notebook.

## Data Cleaning <a name="cleaning"></a>

All code can be seen in [.ipynb file](china.ipynb).

### 1. Translation Issues

First was to determine what was going on with the domestic tourism yuan values. The English titles were "Tourism Expenditure(100 million yuan)" and " Earnings from Domestic Tourism(100 million yuan)", but both shared the same Chinese title「国内旅游总花费(亿元)」. The Chinese translates to "Total Domestic Tourism Expenditure (100 million yuan)." (All translations can be found in the [.xlsx file](data.xlsx).)

9/20 values were not the same between the two datasets with only a few million yuan difference (a few hundred-thousand USD). Further investigation did not clarify the differences, so both were kept following the English titles.

### 2. Nulls

There were several nulls within the table.

Most null values were for subcategories of international tourists (ie tourists from Hong Kong, Taiwan, single-day entry, etc.[^1]) Those fields had too many nulls and were dropped since there were complete records for the total international tourists count. 

`inbound_tourists_10k` and `intl_tourism_revenue_million_usd` have null values during COVID Lockdown[^2] in China, so these values were replaced with 0.

`travel_agencies` and `star_hotels` have missing values for 2023. They were backwards filled as every other field 2023 values to be analyzed and it marks a whole year after China lifted their COVID restrictions.

### 3. Continuity

The raw data was all in Chinese Yuan (CNY) except for international tourist earnings which was in US Dollars (USD).

Annual currency conversions were taken from [FRED (Federal Reserve Economic Data)](https://fred.stlouisfed.org/series/AEXCHUS) for all relevant years.

Two tables were made, one with all currency converted to CNY and the other to USD.

The analysis uses currency set to USD.

## Analysis & Insights <a name="analysis"></a>

## Dataset Info <a name="info"></a>

Note from NBS:
- Population data for 2010 and 2020 are projections from the population census of that year and data for the remaining years are projections from the Annual Population Sample Survey. 
- The growth in per capita disposable income (%) and per capita consumption expenditure (%) are real growth (%) net of price factors, while the rest of the growth (%) is nominal growth (%)
- Median per capita income refers to the per capita income of all surveyed households ranked in order of per capita income level from low to high (or high to low), with the per capita income of the surveyed households in the middle most position


| Field | Description
| --- | ---
| year | year
| pop_total_10k | total year-end population of Chinese citizens within Mainland China, including active-duty military personnel (10,000 persons)
| pop_male_10k | total male population, including active-duty military personnel (10,000 persons)
| pop_female_10k | total female population, including active-duty military personnel (10,000 persons)
| pop_urban_10k | total population of citizens living in urban areas, including active-duty military personnel (10,000 persons)
| pop_rural_10k | total population of citizens not living in urban areas (10,000 persons)
| income_per_capita_yuan | disposable income per capita from all income sources (CNY)
| income_per_capita_yoy_pct | disposable income per capita from all income sources growth rate (%)
| income_median_yuan | median disposable income per capita from all income sources (CNY)
| income_median_yoy_pct | median disposable income per capita from all income sources growth rate (%)
| wage_income_yuan | disposable income per capita from wage income (CNY)
| wage_income_yoy_pct | disposable income per capita from wage income growth rate (%)
| net_op_income_yuan | disposable income per capita from net business operating income (CNY)
| net_op_income_yoy_pct | disposable income per capita from net business operating income growth rate (%)
| net_prop_income_yuan | disposable income per capita from net property income (CNY)
| net_prop_income_yoy_pct | disposable income per capita from net property income growth rate (%)
| net_trans_income_yuan | disposable income per capita from net transfer income (CNY)
| net_trans_income_yoy_pct | disposable income per capita from net transfer income growth rate (%)
| travel_agencies | total travel agencies
| star_hotels | total hotels that are inline with the  "Classification and Evaluation of Tourist Hotel Star Grade"[^3]
| inbound_tourists_10k | total international tourists[^1], one person per trip (10,000 persons)
| foreign_tourists_10k | international tourists (excluding those from Hong Kong, Macau, and Taiwan), one person per trip (10,000 persons)
| hk_macau_tourists_10k | tourists from Hong Kong and Macau, one person per trip (10,000 persons)
| taiwan_tourists_10k | tourists from Taiwan, one person per trip (10,000 persons)
| overnight_tourists_10k | overnight tourists, one person per trip (10,000 persons)
| domestic_departures_10k | outbound Mainland tourists, one person per trip (10,000 persons)
| private_departures_10k | outbound Mainland citizens for personal purposes, one person per trip (10,000 persons)
| domestic_tourists_10k | domestic visitors, one person per trip (10,000 persons)
| intl_tourism_revenue_million_usd | all expenses made by international tourists (transportation, sightseeing, lodging, catering, shopping, entertainment, etc.) during their travels and excursions in Mainland China (million USD)
| domestic_tourism_expenditure_100million_yuan | earnings from domestic tourists (100 million CNY)
| dom_tourists_million | total domestic tourists, one person per trip (million persons)
| urban_dom_tourists_million | domestic tourists from urban residents, one person per trip (million persons)
| rural_dom_tourists_million | domestic tourists from non-urban residents, one person per trip (million persons)
| total_dom_tourism_exp_100million_yuan | total domestic tourism expenditures (100 million CNY)
| urban_dom_tourism_exp_100million_yuan | domestic tourism expenditures by urban residents (100 million CNY)
| rural_dom_tourism_exp_100million_yuan | domestic tourism expenditures by non-urban residents (100 million CNY)
| per_capita_dom_tourism_exp_yuan | domestic tourism expenditures per capita (CNY)
|urban_per_capita_tourism_exp_yuan | domestic tourism expenditures per capita by urban residents (100 million CNY)
| rural_per_capita_tourism_exp_yuan | domestic tourism expenditures per capita by non-urban residents (100 million CNY)


---
[^1]: NBS [Definitions of Foreign Tourists](https://www.stats.gov.cn/sj/ndsj/2021/html/zb17.htm).

[^2]: Zero COVID Policy 动态清零 (January 2020 to December 2022)

[^3]: Classification and Evaluation of Tourist Hotel Star Grade aka [GB/T14308-2003](https://baike.baidu.com/item/%E6%97%85%E6%B8%B8%E9%A5%AD%E5%BA%97%E6%98%9F%E7%BA%A7%E7%9A%84%E5%88%92%E5%88%86%E4%B8%8E%E8%AF%84%E5%AE%9A/381657).
