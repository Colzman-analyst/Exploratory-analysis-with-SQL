# Exploratory-analysis-with-SQL
Exploring and taking insights of the world layoffs datasets to deepening the understanding of the data 
Thanks! Based on your uploaded SQL script (`exploratory_data-db.sql`), here’s a tailored `README.md` using your enhanced GitHub-style template — focused on **Exploratory Data Analysis (EDA)** using SQL.

---

# 📊 Exploratory Data Analysis: Layoffs Dataset

## 📂 About The Project

![EDA SQL Screenshot](#)<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/359acb0c-92f1-497b-a24a-5702c2d0c2c7" />

This project performs structured **Exploratory Data Analysis (EDA)** using SQL to uncover key patterns, distributions, and trends in global layoffs data. It's an extension of the earlier data cleaning process and focuses on slicing the data across time, geography, industry, and company.

Why this is important:

* It helps uncover insights before visualization or modeling.
* It uses SQL window functions and aggregations to surface real business questions.
* It serves as a reusable SQL template for any tabular dataset that needs high-level exploration.

Whether you're a data analyst or BI developer, this script gives you a quick lay of the land.

[(back to top)](#)

---

## 🔧 Built With

* **MySQL 8.0+**
* **SQL** — Aggregations, `GROUP BY`, `DENSE_RANK`, CTEs, and date functions
* **Cleaned Table:** `layoffs_staging2`

[(back to top)](#)

---

## 🚀 Getting Started

### 📦 Prerequisites

* A SQL-compatible database like **MySQL**, **PostgreSQL**, or **BigQuery**
* Load the cleaned `layoffs_staging2` table (output of your data cleaning pipeline)


## 📈 Usage

### 🧪 Key EDA Queries Included

* 🔹 **Peak Layoffs**

  ```sql
  SELECT MAX(total_laid_off), MAX(percentage_laid_off)
  FROM layoffs_staging2;
  ```

* 🔹 **Companies With Highest Layoffs**

  ```sql
  SELECT company, SUM(total_laid_off)
  FROM layoffs_staging2
  GROUP BY company
  ORDER BY 2 DESC;
  ```

* 🔹 **Layoffs Over Time (Yearly)**

  ```sql
  SELECT YEAR(date), SUM(total_laid_off)
  FROM layoffs_staging2
  GROUP BY YEAR(date);
  ```

* 🔹 **Layoffs by Country and Industry**

  ```sql
  SELECT country, SUM(total_laid_off)
  FROM layoffs_staging2
  GROUP BY country;
  ```

* 🔹 **Rolling Monthly Total**

  ```sql
  WITH rolling_total AS (
    SELECT SUBSTRING(date,1,7) AS month, SUM(total_laid_off) AS total_off
    FROM layoffs_staging2
    GROUP BY month
  )
  SELECT month, total_off,
    SUM(total_off) OVER (ORDER BY month) AS Rolling_total
  FROM rolling_total;
  ```

* 🔹 **Top 5 Companies by Layoffs Per Year**

  ```sql
  WITH Company_year (company, years, total_laid_off) AS (
    SELECT company,  YEAR(date), SUM(total_laid_off)
    FROM layoffs_staging2
    GROUP BY company, YEAR(date)
  ), Company_year_Rank AS (
    SELECT *, 
      DENSE_RANK() OVER (PARTITION BY years ORDER BY total_laid_off DESC) AS Ranking
    FROM Company_year
  )
  SELECT *
  FROM Company_year_Rank
  WHERE Ranking <= 5;
  ```

> These queries can be used directly for dashboards, reporting, or feature engineering for ML.

[(back to top)](#)

---

## 🛣️ Roadmap

* [ ] Create materialized views for recurring EDA queries
* [ ] Export results into Power BI
* [ ] Add Python notebook version for comparison
* [ ] Include visual summary of top companies, industries, and regions

[(back to top)](#)

---



## 🙏 Acknowledgments

* [SQL for Data Analysis – Mode](https://mode.com/sql-tutorial/)
* [SQLZoo.net](https://sqlzoo.net/)
* [Leetcode SQL Challenges](https://leetcode.com/problemset/database/)
* [GitHub Pages](https://pages.github.com/)
* [contrib.rocks](https://contrib.rocks/)


