# World Development Indicators — A Visual Analysis

An interactive Tableau dashboard exploring how **wealth, health, and connectivity** relate across 207 countries between 2000 and 2012. Built as a portfolio project to demonstrate calculated fields, geographic and time-series visualisation, and dashboard design.

> **🔗 Live interactive version:** [View on Tableau Public]([https://public.tableau.com/PLACEHOLDER-your-link-here](https://public.tableau.com/app/profile/junning.peng/viz/DataVisulisationonWorldIndicators/GlobalDevelopment))

---

## The question

Global averages hide enormous variation. This project asks a single guiding question and answers it four ways:

**How are national income, life expectancy, and internet access related across countries and regions — and how did those relationships change over 2000–2012?**

---

## Key findings

- **Wealth buys longevity, but with steep diminishing returns.** Life expectancy rises sharply with income among poorer countries, then flattens — the classic Preston-curve shape. Beyond roughly middle-income levels, extra GDP per capita adds little to life expectancy.
- **Every region improved, but the ranking barely moved.** Life expectancy rose across all six regions from 2000 to 2012. Africa climbed fastest from the lowest base, yet the gap between regions persisted throughout the period.
- **Internet access remained deeply uneven.** Connectivity spread rapidly over the period, but a stark divide between high- and low-access countries was still clearly visible by the end of the window.
- **The gender gap in life expectancy is widest across the former Soviet bloc.** Russia, Belarus, Lithuania, Ukraine, Estonia, and Latvia show the largest female–male life-expectancy gaps (roughly 10–13 years), a well-documented regional pattern.

---

## Inside the dashboard

The workbook contains four worksheets and an assembled dashboard.

| View | Chart type | What it shows |
|------|-----------|---------------|
| **Wealth vs. Longevity** | Animated bubble scatter | GDP per capita (log scale) vs. life expectancy, sized by population, coloured by region, with a logarithmic trend line. Annotated comparison of China, the UK, and Nigeria. |
| **Digital Divide Map** | Choropleth world map | Internet usage by country, with an interactive year control. |
| **Regional Trends** | Multi-line time series | Average life expectancy by region, 2000–2012, with a forecast band and an actual/estimate divider. |
| **Gender Gap** | Ranked horizontal bar | Top 15 countries by female–male life-expectancy gap. |

---

## Data

- **Source:** *World Indicators* dataset (originally distributed as a Tableau sample), covering common World Bank–style development indicators.
- **Scope:** 207 countries across 6 regions, annual data for **2000–2012** (2,691 rows).
- **Indicators used:** GDP, Population (total + age bands + urban), Life Expectancy (female/male), Internet Usage, Mobile Phone Usage, Birth Rate, Infant Mortality, Health Expenditure, and others.

### Data preparation

The raw file needed cleaning before analysis — documenting this is part of the point:

- The source file was **UTF-16 encoded**; re-saved as UTF-8 for portability.
- `Year` was stored as a full date string (e.g. `01/12/2000`); the year component was extracted for a clean time axis.
- **`Ease of Business` was dropped** — it was ~93% missing and effectively unusable.
- Several business-climate fields (`Business Tax Rate`, `Hours to do Tax`, `Days to Start Business`) are 36–47% missing and were **excluded from the core analysis** to avoid misleading conclusions.
- Core measures used in the visuals (GDP, Population, Life Expectancy, Internet Usage) are 93–100% complete.
- Countries with **no data for a given year are shown in grey** on the map rather than imputed as zero — "no data" is not the same as "0%."

### Calculated fields

| Field | Formula (conceptual) | Why |
|-------|---------------------|-----|
| GDP per capita | `GDP / Population Total` | Comparable across countries of different sizes |
| Internet Usage % | `Internet Usage × 100` | Source stores a 0–1 fraction |
| Avg. Life Expectancy | `(Life Exp Female + Life Exp Male) / 2` | Single headline health measure |
| Gender Gap | `Life Exp Female − Life Exp Male` | Basis for the ranking view |

---

## Methodology notes & caveats

These choices are deliberate and worth stating explicitly:

- **Log-scaled income axis.** GDP per capita spans roughly \$100 to \$100,000, so it is plotted on a **logarithmic axis**. Paired with a logarithmic trend model, the diminishing-returns relationship appears as a straight line — this is the standard way to render a Preston curve.
- **Regional averages are unweighted.** The regional trend lines average each region's *countries equally* (a small nation counts the same as a large one). A population-weighted figure (`Σ(life expectancy × population) / Σ population`) would be the "correct" regional value; the unweighted version is used here for simplicity and is flagged for transparency.
- **The 2013 figures are a forecast, not observed data.** The data ends in 2012. The shaded band and dashed divider on the trend chart mark a model-based projection — it should not be read as measured 2013 values.
- **The gender-gap ranking is a period average.** With the year filter set to "All," each bar is the country's mean gap across 2000–2012, which smooths single-year noise. It is not a single-year snapshot.
- **Non-zero y-axis baselines.** Life-expectancy charts do not start at zero, which is standard for a bounded rate but worth noting.

---
