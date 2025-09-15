# 📊 Yelp Reviews Analysis with AWS & Snowflake



## 📌 Project Overview

This project demonstrates how to work with the **Yelp Open Dataset**, upload it to **AWS S3**, transform JSON into structured tables in **Snowflake**, and run queries to extract business insights.
Additionally, a **Python UDF** was created in Snowflake using **TextBlob** to perform **sentiment analysis** on Yelp reviews.

---

## 🛠️ Tech Stack

* **AWS S3** – Data storage for raw Yelp JSON files
* **Snowflake** – Data warehousing, SQL queries, and Python UDFs
* **Python (TextBlob)** – Sentiment analysis integration within Snowflake
* **SQL** – Data transformation, exploration, and analysis

---

## ⚙️ Project Workflow

1. **Upload Data to AWS S3**

   * Yelp JSON datasets uploaded to an S3 bucket.

2. **Ingest Data into Snowflake**

   * Used `COPY INTO` to load JSON data from S3 into Snowflake tables.
   * Converted semi-structured JSON into structured tables (`tbl_yelp_reviews`, `tbl_yelp_businesses`).

3. **Sentiment Analysis**

   * Defined a **Snowflake Python UDF** using TextBlob:

     ```sql
     CREATE OR REPLACE FUNCTION analyze_sentiment(text STRING)
     RETURNS STRING
     LANGUAGE PYTHON
     RUNTIME_VERSION = '3.9'
     PACKAGES = ('textblob') 
     HANDLER = 'sentiment_analyzer'
     AS $$
     from textblob import TextBlob
     def sentiment_analyzer(text):
         analysis = TextBlob(text)
         if analysis.sentiment.polarity > 0:
             return 'Positive'
         elif analysis.sentiment.polarity == 0:
             return 'Neutral'
         else:
             return 'Negative'
     $$;
     ```



4. **Analysis Queries**

   * Wrote SQL queries to extract insights such as:

     * Most popular categories
     * Top users by review count
     * Businesses with highest 5-star review %
     * Recent reviews per business
     * Top businesses by **positive sentiment**

---

## 📂 Repository Structure

```
├── split_files.py          # Python script for preprocessing Yelp JSON
├── Functions&Tables.sql    # Snowflake UDFs and table creation
├── yelpqueries.sql         # Analytical SQL queries
└── README.md               # Project documentation
```

---

## 🚀 How to Run

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/yelp-snowflake-aws.git
   cd yelp-snowflake-aws
   ```
2. Update AWS credentials in `Functions&Tables.sql` (replace with your own).
3. Run `split_files.py` if needed to preprocess the dataset.
4. Execute `Functions&Tables.sql` in Snowflake to create tables and UDFs.
5. Run queries from `yelpqueries.sql` to explore insights.

---

## 🔮 Future Improvements

* Build interactive dashboards with **Tableau / Power BI**
* Implement advanced NLP models for sentiment (e.g., VADER, HuggingFace Transformers)
* Automate data pipeline with **Airflow / dbt**


