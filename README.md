# Country Development & Economic Performance Dashboard

An interactive, multi-page **Power BI** dashboard that compares development and economic indicators across countries. Built as Mini-Project 3 to support strategic policy and investment decisions.

## Table of Contents

- [Business Scenario](#business-scenario)
- [Dataset](#dataset)
- [Dashboard Pages](#dashboard-pages)
- [Data Preparation](#data-preparation)
- [Data Model](#data-model)
- [DAX Highlights](#dax-highlights)
- [Key Insights](#key-insights)
- [Strategic Recommendations](#strategic-recommendations)
- [Repository Structure](#repository-structure)
- [How to Use](#how-to-use)
- [Limitations](#limitations)
- [Tools](#tools)
- [Author](#author)

## Business Scenario

An international economic research organization needs a centralized dashboard to monitor and compare development indicators across countries and regions. Senior management uses it to:

- Compare economic performance between countries
- Monitor development indicators
- Identify high-growth and low-growth regions
- Analyze the relationship between health, economic, and social factors
- Support strategic policy and investment decisions

## Dataset

**Source file:** `data/Country Data Comparison.csv`

Country-level socio-economic indicators: population, GDP, GDP per capita, GDP growth, life expectancy, birth and death rates, infant mortality, inflation, imports and exports, unemployment, oil production and consumption, electricity, internet and mobile usage, and more.

## Dashboard Pages

| Page | Purpose | Main visuals |
| --- | --- | --- |
| **1. Executive Overview** | High-level snapshot for management | KPI cards, GDP comparison by country, population distribution, regional development comparison, country ranking table, map |
| **2. Health & Education Analysis** | Link between income and health | Life expectancy comparison, birth rate vs death rate, health indicators, GDP vs life expectancy scatter plot |
| **3. Economic Performance Analysis** | Trade, inflation, and growth | Imports vs exports, inflation analysis, growth indicators, top and bottom performers, regional economic comparison |

**Presentation:** see [`docs/Country_Dashboard_Presentation.pptx`](docs/Country_Dashboard_Presentation.pptx).

**Interactivity:** Region, Country, and GDP Category slicers, drill-through pages, tooltips, conditional formatting, dynamic page titles, and navigation buttons.

**KPI cards:** Average GDP, Average Life Expectancy, Total Population, Highest GDP Country, Lowest Inflation Country.

## Data Preparation

All cleaning was done in **Power Query**. Full details are in [`docs/Data_Cleaning_Documentation.docx`](docs/Data_Cleaning_Documentation.docx).

- Kept **26 of 45** columns. The other 19 were mostly empty, not needed, or duplicated a stronger indicator.
- Standardized **17 country names** (for example `Korea South` to `South Korea`, `Burma` to `Myanmar`) so the map and slicers recognize them.
- Removed 1 non-country row (European Union) and 25 rows with no data except area (dataset reduced to **237 rows**).
- Replaced unrealistic zeros with null (for example Area, Unemployment, Mobile Phones, External Debt).
- Kept missing values as null instead of imputing them, so averages are not distorted.
- Renamed columns to short names (for example `GDP - real growth rate(%)` to `GDP Growth %`) and set correct data types.

## Data Model

- **Fact table:** `Country Data Comparison`, one row per country or entity
- **Dimension table:** `Dim_Region` (`data/Dim_Region.csv`), related by region
- **Measures table:** `_Measures`, which holds all DAX measures

**Calculated columns:** GDP Category, Population Category, Development Level, Growth Category, Population Density, Internet Penetration %, Trade Balance, Trade Position, Natural Increase Rate, Inflation Category, Life Expectancy Category, Oil Status.

## DAX Highlights

Full documentation with every formula is in [`docs/DAX_Documentation.docx`](docs/DAX_Documentation.docx).

```DAX
Highest GDP Country =
MAXX(
    TOPN(1, ALLSELECTED('Country Data Comparison'), 'Country Data Comparison'[GDP], DESC),
    'Country Data Comparison'[Country]
)
```

```DAX
GDP Rank =
IF(
    ISBLANK(SUM('Country Data Comparison'[GDP])),
    BLANK(),
    RANKX(
        ALLSELECTED('Country Data Comparison'[Country]),
        CALCULATE(SUM('Country Data Comparison'[GDP])),
        , DESC, DENSE
    )
)
```

Other measures: Average GDP, Average Life Expectancy, Total Population, Lowest Inflation Country, GDP Growth Indicator, Population Density Indicator, Average Birth Rate, Average Death Rate, Trade Balance Indicator, Weighted Life Expectancy, dynamic titles, and color measures for conditional formatting.

## Key Insights

Full analysis is in [`docs/Business_Insights_Report.docx`](docs/Business_Insights_Report.docx).

1. **Income and health move together.** High Income countries average 79.3 years of life expectancy versus 54.1 in Low Income countries. Infant mortality has a correlation of -0.89 with life expectancy.
2. **Lower-income countries grow faster.** Median GDP growth is 2.5% in High Income countries versus about 4.3% to 4.5% in the other groups.
3. **Lower inflation goes with higher life expectancy.** Countries with inflation below 3% average 72.0 years, while countries with 10% or more average 58.2 years.
4. **World output is highly concentrated.** The five largest economies produce about 51% of total GDP, and 154 of 223 countries run a trade deficit.
5. **Population growth and the digital gap are concentrated in poorer countries.** Internet penetration is 43.8% in High Income countries versus 1.5% in Low Income countries.

## Strategic Recommendations

1. **Prioritize healthcare in countries with low life expectancy.** Fifty countries are below 60 years, covering about 855 million people.
2. **Build economic support for high-inflation countries.** Thirty-one countries have inflation of 10% or more, and they should be monitored with the median.
3. **Direct investment to fast-growing, lower-middle income economies.** They average 5.3% growth but have very low internet access.

## Repository Structure

```
country-development-powerbi-dashboard/
├── README.md
├── data/
│   ├── Country Data Comparison.csv
│   └── Dim_Region.csv
├── dashboard/
│   └── MiniProject-3.pbix
└── docs/
    ├── Business_Insights_Report.docx
    ├── Country_Dashboard_Presentation.pptx
    ├── Data_Cleaning_Documentation.docx
    └── DAX_Documentation.docx
```

## How to Use

1. Download or clone this repository.
2. Open `dashboard/MiniProject-3.pbix` in **Power BI Desktop**.
3. If Power BI asks for the data source, go to **Home > Transform data > Data source settings** and point it to `data/Country Data Comparison.csv` (and `data/Dim_Region.csv` for the region table).
4. Use the slicers and the navigation buttons to explore the three pages.

## Limitations

- The dataset is a single snapshot, so it shows relationships between indicators but not change over time.
- Some indicators have missing values, which are excluded from the related averages.
- Correlations show association, not causation.
- The dataset has no education indicators, so internet penetration is used as a proxy for access to knowledge and technology.

## Tools

Microsoft Power BI Desktop, Power Query, DAX

## Author

**Mahmoud Essam Salah**
[LinkedIn](https://www.linkedin.com/in/mahmoud-essam-salah/) | [GitHub](https://github.com/moudssam2004-lang)
