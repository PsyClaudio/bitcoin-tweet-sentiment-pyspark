# Bitcoin Tweet Sentiment Analysis with PySpark

An exploratory analysis of Bitcoin-related tweets using Python, PySpark, Spark SQL and VADER. The project examines estimated sentiment over time, compares engagement across sentiment classes and explores the relationship between sentiment changes and BTC/USD price movements.

This educational project was developed by **Claudio Lombardi** as part of a Master’s course in Data Analytics. The business scenario described in the notebook is simulated.

## Project questions

1. How are the analyzed tweets distributed across positive, neutral and negative sentiment classes?
2. How does estimated sentiment vary over time?
3. Do tweets classified as negative receive more likes or replies on average than those classified as positive?
4. Are daily changes in estimated sentiment associated with BTC/USD returns on the same, previous or following day?

## Data

The analysis uses two external sources:

- [Bitcoin tweets dataset](https://proai-datasets.s3.eu-west-3.amazonaws.com/bitcoin_tweets.csv)
- [BTC/USD historical prices](https://github.com/Profession-AI/progetti-big-data/blob/main/Analisi%20del%20consenso%20sul%20Bitcoin/BTC-USD.csv)

The tweet file used in the notebook contains **100,001 records**. Language detection identified **67,974 records as English**—approximately 67.97% of the file—and the main sentiment analysis focuses on that subset.

The archive is historical and unevenly distributed over time: approximately **92.9% of the selected English-language records** fall on May 10 and 11, 2019. It should not be treated as a representative sample of all public discussion about Bitcoin.

## Method

The notebook develops the analysis in the following stages:

1. Download the source files and record their origin and SHA-256 checksums.
2. Inspect the schema, dates, missing values, duplicates and engagement fields.
3. Detect language and select records identified as English.
4. Estimate positive, neutral or negative sentiment using **VADER**, a lexicon- and rule-based tool.
5. Compare VADER classifications with a manually annotated evaluation sample.
6. Aggregate estimated sentiment by day and compare likes and replies across sentiment classes.
7. Explore correlations between changes in daily sentiment balance and BTC/USD daily returns.

PySpark is used for data preparation, sentiment application and aggregations, with Spark SQL where appropriate. The notebook runs Spark in **local mode with two threads** (`local[2]`) in Google Colab.

VADER is used as an existing sentiment tool; **no supervised MLlib model is trained** on the tweet dataset.

## Evaluation of sentiment classification

The final evaluation sample contains **100 annotated records**, of which **94 were considered evaluable**. The annotations were prepared by the author, with documented AI assistance; they are not independent annotations from multiple human reviewers.

| Metric | Result |
|---|---:|
| Accuracy | 52.13% |
| Macro-F1 | 0.504 |
| Weighted F1 | 0.525 |
| Majority-class baseline accuracy | 48.94% |

The sample is small and contains only 11 evaluable negative examples. Some texts are ambiguous, and the tone of a tweet does not necessarily express an opinion specifically about Bitcoin.

These results support an **exploratory use** of the sentiment labels, with caution and human review.

## Selected results

### Estimated sentiment

Among the records identified as English, VADER classified:

| Estimated class | Records | Share |
|---|---:|---:|
| Positive | 32,187 | 47.35% |
| Neutral | 24,446 | 35.96% |
| Negative | 11,341 | 16.68% |

These percentages describe VADER’s labels for the selected records; they do not measure overall public opinion.

### Engagement

| Estimated sentiment | Mean likes | Mean replies |
|---|---:|---:|
| Negative | 12.118 | 1.289 |
| Positive | 9.285 | 1.356 |

In this dataset, tweets classified as negative received more likes on average, but fewer replies on average, than those classified as positive. The median number of likes and replies was **zero** in both groups, and a small proportion of tweets accounted for a large share of interactions.

The comparison is descriptive. It does not establish that sentiment causes engagement differences.

### Sentiment and BTC/USD

The Pearson correlation between the daily change in the sentiment balance and the BTC/USD return on the **same day** was **0.1252**, based on **386 available pairs**.

A separate comparison using a minimum of 10 tweets on both days involved in the sentiment change retained **28 pairs** and covered a shorter period. The findings do not provide convincing evidence that sentiment predicts price movements or vice versa.

No causal conclusion or trading strategy is derived from these correlations.

## Limitations

- Language identification is automatic and may contain errors.
- VADER measures the estimated tone of a text, which may differ from the opinion expressed specifically about Bitcoin.
- The manually annotated evaluation sample is small and includes ambiguous or similar texts.
- Tweet coverage is highly concentrated in two days, while many other days have few records.
- Bots, promotional content and coordinated activity were not systematically identified.
- Engagement is not adjusted for factors such as author popularity or time since publication.
- The price file’s closing-time convention could not be verified beyond its supplied information.
- The time-series analysis is exploratory; it does not include a future-data prediction test or establish causality.

## Run the notebook

1. Open [Analisi_del_sentiment_Bitcoin_con_PySpark.ipynb](Analisi_del_sentiment_Bitcoin_con_PySpark.ipynb) in Google Colab.
2. Run the cells in order. The notebook installs the required versions of PySpark, `langdetect` and `vaderSentiment` and downloads the source files.
3. The notebook checks the source-file and annotation-sample fingerprints. If a source file changes, execution may stop so the existing annotations and conclusions can be reviewed before reuse.

The notebook and its explanations are written in Italian. Internet access is required to install dependencies and download the datasets. Files created in Colab’s runtime storage are temporary.

## Tools

- Python and Google Colab
- PySpark and Spark SQL
- VADER and `langdetect`
- Data visualization and exploratory statistical analysis

## Author

**Claudio Lombardi**
