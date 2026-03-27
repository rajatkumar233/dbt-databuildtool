# 🎬 Netflix Movie Data Pipeline — dbt (Data Build Tool) & Snowflake

An end-to-end data transformation pipeline built with **dbt (Data Build Tool)** and **Snowflake** to turn raw Netflix movie data into clean, analytics-ready tables. The project follows a layered data modeling approach — from raw data ingestion to final reporting tables.

---

## 📌 About the Project

This project loads raw Netflix/MovieLens data from **AWS S3** into **Snowflake**, and then uses **dbt** to transform it step by step:

1. **Raw Layer** — Raw CSV files (movies, ratings, tags, genome scores, genome tags, links) are loaded into Snowflake from an S3 bucket.
2. **Staging Layer** — Source data is cleaned and standardized using SQL views.
3. **Dimension Layer** — Organized tables for movies, users, and genome tags with proper formatting and deduplication.
4. **Fact Layer** — Core business tables for ratings and genome scores, including incremental loading support.
5. **Mart Layer** — Final reporting table combining fact data with seed data for movie release information.

---

## 🛠️ Tech Stack & Tools

| Tool / Technology | Purpose |
|---|---|
| **dbt (Data Build Tool)** | Data transformation, modeling, testing |
| **Snowflake** | Cloud data warehouse |
| **SQL** | Data querying and transformation logic |
| **AWS S3** | Raw data storage (CSV files) |
| **Jinja** | Templating for dynamic SQL in dbt |
| **dbt_utils** | Helper package for surrogate keys, macros |
| **Git & GitHub** | Version control and code hosting |

---

## 📁 Project Structure

```
netflix/
├── models/
│   ├── staging/          # 6 source views (movies, ratings, tags, links, genome scores, genome tags)
│   ├── dim/              # 4 dimension tables (movies, users, genome tags, movies with tags)
│   ├── fct/              # 2 fact tables (ratings with incremental load, genome scores)
│   ├── mart/             # 1 mart table (movie releases report)
│   ├── sources.yml       # Source definitions pointing to raw Snowflake tables
│   └── schema.yml        # Column-level tests and documentation
├── macros/               # Custom macros (e.g., no_nulls_in_columns)
├── snapshots/            # SCD Type-2 snapshot for tags table
├── seeds/                # Static CSV data (movie release dates)
├── tests/                # Custom data quality tests
├── packages.yml          # dbt packages (dbt_utils)
└── dbt_project.yml       # Project configuration
```

---

## 📊 Data Sources (Raw Tables)

| Table | Description |
|---|---|
| `raw_movies` | Movie ID, title, and genres |
| `raw_ratings` | User ratings for movies |
| `raw_tags` | User-generated tags for movies |
| `raw_genome_scores` | Relevance scores for movie-tag pairs |
| `raw_genome_tags` | Genome tag labels |
| `raw_links` | External links (IMDb, TMDb IDs) |

All raw data is loaded from an **AWS S3 bucket** using Snowflake's `COPY INTO` command.

---

## ⚡ Key Features

- **Layered Architecture** — Clean separation of staging, dimension, fact, and mart layers
- **Incremental Models** — `fct_ratings` uses incremental loading to process only new data
- **Snapshots (SCD Type-2)** — `snap_tags` tracks historical changes in user tags over time
- **Custom Macros** — `no_nulls_in_columns` macro checks for NULL values across all columns
- **Schema Testing** — Column-level tests (`not_null`, `relationships`) defined in `schema.yml`
- **Custom Tests** — Additional data quality tests for relevance scores
- **Seed Data** — Static movie release dates loaded as a seed file
- **dbt_utils Package** — Used for generating surrogate keys in snapshots

---

## 🚀 How to Run

### 1. Setup Snowflake
Run `instructions.sql` in your Snowflake console to:
- Create roles, users, database, and schema
- Create an S3 stage and load all raw CSV data into tables

### 2. Install dbt
```bash
pip install dbt-snowflake==1.9.0
```

### 3. Configure dbt Profile
Create a `profiles.yml` file in your `~/.dbt/` directory with your Snowflake credentials.

### 4. Run dbt Commands
```bash
dbt deps          # Install packages (dbt_utils)
dbt seed          # Load seed data
dbt run           # Run all models
dbt test          # Run all tests
dbt snapshot      # Run snapshots
```

---

## 🔮 Future Scope

- Add more mart tables for genre-wise and user-wise analytics
- Build a **Power BI / Tableau dashboard** on top of the mart layer
- Schedule dbt runs using **dbt Cloud** or **Airflow** for automation
- Add more data quality tests and documentation

---

## 👤 Author

**Rajat Kumar**
- 📧 rajatkumar39008@gmail.com
- 🔗 [LinkedIn](https://www.linkedin.com/in/rajatkumar96)
- 💻 [GitHub](https://github.com/rajatkumar233)

---

> Built with ❤️ using dbt and Snowflake
