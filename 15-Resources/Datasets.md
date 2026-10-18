# Data Science Datasets Resource Guide

A curated collection of datasets and data repositories for learning Python, data analysis, statistics, machine learning, deep learning, natural language processing, computer vision, time series forecasting, and real-world data science projects.

This guide helps learners discover suitable datasets, understand their formats, load data using Python, and build reproducible data science projects.

## Table of Contents

- [1. How to Choose a Dataset](#1-how-to-choose-a-dataset)
- [2. General Dataset Repositories](#2-general-dataset-repositories)
- [3. Indian Open Data Sources](#3-indian-open-data-sources)
- [4. Beginner-Friendly Datasets](#4-beginner-friendly-datasets)
- [5. Data Analysis and Visualization](#5-data-analysis-and-visualization)
- [6. Classification Datasets](#6-classification-datasets)
- [7. Regression Datasets](#7-regression-datasets)
- [8. Clustering and Anomaly Detection](#8-clustering-and-anomaly-detection)
- [9. Time Series Datasets](#9-time-series-datasets)
- [10. Natural Language Processing Datasets](#10-natural-language-processing-datasets)
- [11. Computer Vision Datasets](#11-computer-vision-datasets)
- [12. Deep Learning and AI Datasets](#12-deep-learning-and-ai-datasets)
- [13. Recommendation System Datasets](#13-recommendation-system-datasets)
- [14. Finance and Business Datasets](#14-finance-and-business-datasets)
- [15. Healthcare and Scientific Datasets](#15-healthcare-and-scientific-datasets)
- [16. Geospatial and Environmental Datasets](#16-geospatial-and-environmental-datasets)
- [17. Cybersecurity Datasets](#17-cybersecurity-datasets)
- [18. Common Dataset Formats](#18-common-dataset-formats)
- [19. Loading Datasets with Python](#19-loading-datasets-with-python)
- [20. Data Quality Checklist](#20-data-quality-checklist)
- [21. Dataset Licensing and Responsible Use](#21-dataset-licensing-and-responsible-use)
- [22. Dataset-Based Project Ideas](#22-dataset-based-project-ideas)
- [23. Recommended Learning Path](#23-recommended-learning-path)
- [24. Additional Resources](#24-additional-resources)

## 1. How to Choose a Dataset

Before downloading a dataset, consider the following factors.

| Factor | What to check |
|---|---|
| Objective | What question are you trying to answer? |
| Problem type | Classification, regression, clustering, forecasting, or NLP? |
| Size | Is it suitable for your computer and available memory? |
| Format | CSV, JSON, Parquet, images, text, or time series? |
| Quality | Are there missing values, duplicates, or inconsistent records? |
| Labels | Does the dataset include a suitable target variable? |
| Documentation | Are columns, units, and collection methods explained? |
| License | Are your intended uses permitted? |
| Representativeness | Does the dataset reflect the population or environment of interest? |
| Reproducibility | Can you record the exact version and retrieval method? |

**Beginner recommendation:** Start with a small, documented dataset. Complete an exploratory data analysis before moving to more complex machine learning tasks.

## 2. General Dataset Repositories

These platforms help you discover datasets across many domains.

| Platform | Best for | Link |
|---|---|---|
| Kaggle Datasets | Practice projects and competitions | [Visit Kaggle](https://www.kaggle.com/datasets) |
| UCI Machine Learning Repository | Classical machine learning benchmarks | [Visit UCI](https://archive.ics.uci.edu/datasets) |
| Google Dataset Search | Finding datasets across repositories | [Open Dataset Search](https://datasetsearch.research.google.com/) |
| Hugging Face Datasets | NLP, computer vision, audio, and AI | [Browse datasets](https://huggingface.co/datasets) |
| OpenML | Machine learning datasets and benchmarks | [Visit OpenML](https://www.openml.org/) |
| AWS Open Data | Large public datasets and cloud-based analysis | [Browse AWS Open Data](https://registry.opendata.aws/) |
| Google Cloud Datasets | Public datasets and analytical resources | [Explore Google Cloud](https://cloud.google.com/datasets) |
| GitHub Awesome Public Datasets | Curated links to public datasets | [Browse repository](https://github.com/awesomedata/awesome-public-datasets) |
| Zenodo | Research data and citable dataset releases | [Visit Zenodo](https://zenodo.org/) |
| Harvard Dataverse | Research datasets across disciplines | [Visit Dataverse](https://dataverse.harvard.edu/) |

The availability of a dataset does not automatically mean it is free for every use. Review its license, access requirements, and any restrictions before downloading or redistributing it.

## 3. Indian Open Data Sources

These resources are particularly useful for projects involving India.

| Platform | Dataset categories | Link |
|---|---|---|
| Open Government Data Platform India | Public administration, education, agriculture, health, and more | [data.gov.in](https://www.data.gov.in/) |
| Reserve Bank of India | Banking, monetary policy, financial statistics | [RBI Database](https://data.rbi.org.in/) |
| MOSPI | Official statistics and economic indicators | [MOSPI](https://www.mospi.gov.in/) |
| Census of India | Population, households, and demographic statistics | [Census India](https://censusindia.gov.in/) |
| India Meteorological Department | Weather and climate information | [IMD](https://mausam.imd.gov.in/) |
| National Family Health Survey | Population and health survey data | [NFHS](https://rchiips.org/nfhs/) |
| Indian Space Research Organisation | Earth observation and geospatial resources | [ISRO](https://www.isro.gov.in/) |
| Ministry of Agriculture and Farmers Welfare | Agricultural statistics and related resources | [Agriculture Ministry](https://agriwelfare.gov.in/) |

### Indian dataset project ideas

- Analyze rainfall and agricultural production across states.
- Explore population growth and urbanization.
- Study inflation and other economic indicators.
- Compare education indicators across regions.
- Analyze seasonal weather patterns.
- Explore public transport and urban mobility data where available.

**Important:** Verify publication dates, geographic coverage, units, revisions, and reuse conditions. Some portals may require registration or API access.

## 4. Beginner-Friendly Datasets

These datasets are suitable for learning data exploration, visualization, and introductory machine learning.

| Dataset | Learning objective | Source |
|---|---|---|
| Iris | Classification and feature visualization | [UCI Iris](https://archive.ics.uci.edu/dataset/53/iris) |
| Titanic | Data cleaning and classification | [Kaggle Titanic](https://www.kaggle.com/competitions/titanic) |
| Palmer Penguins | Exploratory data analysis and classification | [Palmer Penguins](https://allisonhorst.github.io/palmerpenguins/) |
| Wine Quality | Regression and classification | [UCI Wine Quality](https://archive.ics.uci.edu/dataset/186/wine+quality) |
| Adult Census Income | Classification and fairness analysis | [UCI Adult](https://archive.ics.uci.edu/dataset/2/adult) |
| California Housing | Regression | [Scikit-learn dataset guide](https://scikit-learn.org/stable/datasets/real_world.html) |
| Breast Cancer Wisconsin | Binary classification | [UCI Breast Cancer](https://archive.ics.uci.edu/dataset/17/breast+cancer+wisconsin+diagnostic) |
| Digits | Image classification | [Scikit-learn datasets](https://scikit-learn.org/stable/datasets/toy_dataset.html) |

### Suggested beginner exercises

1. Inspect rows, columns, and data types.
2. Calculate summary statistics.
3. Identify missing values and duplicates.
4. Create histograms and scatter plots.
5. Examine correlations between numerical features.
6. Build a baseline machine learning model.
7. Compare evaluation metrics.
8. Document findings and limitations.

## 5. Data Analysis and Visualization

Use these datasets to practice data cleaning, aggregation, visualization, and storytelling.

| Dataset or source | Suggested analysis | Link |
|---|---|---|
| NYC Taxi Trip Records | Trip duration, fares, and demand patterns | [NYC TLC](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page) |
| Our World in Data | Global health, energy, population, and economic trends | [Explore data](https://ourworldindata.org/) |
| World Bank Open Data | Development indicators and economic comparisons | [World Bank](https://data.worldbank.org/) |
| Gapminder | Population, income, and health comparisons | [Gapminder](https://www.gapminder.org/data/) |
| FiveThirtyEight datasets | Journalism-oriented statistical analyses | [GitHub repository](https://github.com/fivethirtyeight/data) |
| Google Trends | Search-interest patterns | [Google Trends](https://trends.google.com/trends/) |

### Recommended exercises

- Compare trends across countries or regions.
- Build an interactive or static dashboard.
- Identify outliers and seasonal patterns.
- Create a report supported by charts and statistical summaries.

## 6. Classification Datasets

Classification predicts a discrete category or label.

| Dataset | Problem | Source |
|---|---|---|
| Iris | Flower species classification | [UCI](https://archive.ics.uci.edu/dataset/53/iris) |
| Breast Cancer Wisconsin Diagnostic | Benign vs malignant classification | [UCI](https://archive.ics.uci.edu/dataset/17/breast+cancer+wisconsin+diagnostic) |
| Adult Census Income | Income category prediction | [UCI](https://archive.ics.uci.edu/dataset/2/adult) |
| Bank Marketing | Predicting subscription outcomes | [UCI](https://archive.ics.uci.edu/dataset/222/bank+marketing) |
| Dry Bean | Multiclass classification | [UCI](https://archive.ics.uci.edu/dataset/602/dry+bean+dataset) |
| Credit Card Fraud Detection | Fraud classification | [Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud) |
| SMS Spam Collection | Spam detection | [UCI](https://archive.ics.uci.edu/dataset/228/sms+spam+collection) |

### Models to practice

- Logistic Regression
- Decision Tree
- Random Forest
- Support Vector Machine
- K-Nearest Neighbors
- Naive Bayes
- Gradient Boosting

### Evaluation metrics

Use accuracy, precision, recall, F1-score, ROC-AUC, or average precision as appropriate to the dataset and business objective.

For imbalanced classification, avoid relying on accuracy alone.

## 7. Regression Datasets

Regression predicts continuous numerical values.

| Dataset | Prediction task | Source |
|---|---|---|
| California Housing | Housing values | [Scikit-learn](https://scikit-learn.org/stable/datasets/real_world.html) |
| Diabetes | Disease progression measurement | [Scikit-learn](https://scikit-learn.org/stable/datasets/toy_dataset.html) |
| Auto MPG | Vehicle fuel efficiency | [UCI](https://archive.ics.uci.edu/dataset/9/auto+mpg) |
| Concrete Compressive Strength | Material strength | [UCI](https://archive.ics.uci.edu/dataset/165/concrete+compressive+strength) |
| Energy Efficiency | Building energy demand | [UCI](https://archive.ics.uci.edu/dataset/242/energy+efficiency) |
| Appliances Energy Prediction | Energy consumption | [UCI](https://archive.ics.uci.edu/dataset/374/appliances+energy+prediction) |

### Models to practice

- Linear Regression
- Ridge and Lasso Regression
- Decision Tree Regressor
- Random Forest Regressor
- Gradient Boosting Regressor
- Support Vector Regression

### Evaluation metrics

- Mean Absolute Error (MAE)
- Mean Squared Error (MSE)
- Root Mean Squared Error (RMSE)
- R-squared

Always compare the model against a simple baseline and evaluate errors in the context of the target variable.

## 8. Clustering and Anomaly Detection

These tasks explore structure in data without requiring a conventional labeled target.

| Dataset or source | Learning objective | Link |
|---|---|---|
| Wholesale Customers | Customer segmentation | [UCI](https://archive.ics.uci.edu/dataset/292/wholesale+customers) |
| Online Retail | Customer behavior and transaction patterns | [UCI](https://archive.ics.uci.edu/dataset/352/online+retail) |
| Credit Card Fraud Detection | Anomaly detection and rare-event classification | [Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud) |
| KDD Cup 1999 | Network intrusion research and anomaly detection | [UCI](https://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html) |
| NAB | Anomaly detection in time series | [GitHub](https://github.com/numenta/NAB) |

### Algorithms to practice

- K-Means
- DBSCAN
- Agglomerative Clustering
- Isolation Forest
- Local Outlier Factor
- One-Class SVM

### Suggested exercises

- Standardize features before distance-based clustering when appropriate.
- Compare different cluster counts.
- Visualize clusters with PCA.
- Evaluate cluster quality with silhouette score where appropriate.
- Investigate potential anomalies rather than automatically assuming they are errors.

## 9. Time Series Datasets

Time series datasets contain observations ordered in time.

| Dataset or source | Application | Link |
|---|---|---|
| NOAA Climate Data | Weather and climate trends | [NOAA](https://www.ncei.noaa.gov/) |
| NASA Earth Data | Earth science and environmental observations | [NASA Earthdata](https://www.earthdata.nasa.gov/) |
| Electricity Load Diagrams | Energy consumption forecasting | [UCI](https://archive.ics.uci.edu/dataset/321/electricityloaddiagrams20112014) |
| Individual Household Electric Power Consumption | Household energy analysis | [UCI](https://archive.ics.uci.edu/dataset/235/individual+household+electric+power+consumption) |
| M4 Competition | Forecasting benchmarks | [M4 Competition](https://www.mcompetitions.unic.ac.cy/) |
| World Bank Indicators | Economic and development trends | [World Bank](https://data.worldbank.org/) |

### Common tasks

- Trend and seasonality analysis
- Short-term demand forecasting
- Anomaly detection
- Rolling statistics
- Forecast accuracy comparison

### Important practice

Do not randomly shuffle time series data for ordinary forecasting evaluation. Use chronological train-validation-test splits or time-aware cross-validation.

Avoid using future observations when constructing features for earlier prediction dates.

## 10. Natural Language Processing Datasets

NLP datasets contain text, labels, documents, or language pairs.

| Dataset | Application | Source |
|---|---|---|
| IMDb Reviews | Sentiment classification | [IMDb](https://ai.stanford.edu/~amaas/data/sentiment/) |
| SMS Spam Collection | Spam detection | [UCI](https://archive.ics.uci.edu/dataset/228/sms+spam+collection) |
| GLUE | Language understanding benchmarks | [GLUE](https://gluebenchmark.com/) |
| SQuAD | Reading comprehension and question answering | [SQuAD](https://rajpurkar.github.io/SQuAD-explorer/) |
| WikiText | Language modeling | [Salesforce Research](https://blog.salesforceairesearch.com/the-wikitext-long-term-dependency-language-modeling-dataset/) |
| Common Crawl | Large-scale web data | [Common Crawl](https://commoncrawl.org/) |
| Hugging Face text datasets | NLP and language model tasks | [Hugging Face](https://huggingface.co/datasets) |

### Projects

- Sentiment analysis
- Spam message detection
- News topic classification
- Document categorization
- Question answering
- Text summarization
- Named entity recognition

### Recommended tools

- NLTK
- spaCy
- Scikit-learn
- Hugging Face Datasets
- Transformers

Check dataset licenses, privacy restrictions, language coverage, and the reliability of labels before using text in a project.

## 11. Computer Vision Datasets

Computer vision datasets contain images or videos for classification, detection, segmentation, or other visual tasks.

| Dataset | Application | Source |
|---|---|---|
| MNIST | Handwritten digit classification | [Official site](http://yann.lecun.com/exdb/mnist/) |
| Fashion-MNIST | Clothing image classification | [GitHub](https://github.com/zalandoresearch/fashion-mnist) |
| CIFAR-10 | Small-image classification | [University of Toronto](https://www.cs.toronto.edu/~kriz/cifar.html) |
| ImageNet | Large-scale image recognition research | [ImageNet](https://www.image-net.org/) |
| COCO | Object detection and segmentation | [COCO](https://cocodataset.org/) |
| Oxford-IIIT Pet | Pet classification and segmentation | [Oxford Robotics Institute](https://www.robots.ox.ac.uk/~vgg/data/pets/) |
| Open Images | Image understanding and detection | [Open Images](https://storage.googleapis.com/openimages/web/index.html) |

### Projects

- Handwritten digit recognition
- Image classification
- Object detection
- Image segmentation
- Transfer learning
- Image similarity search

Review the license and any restrictions on image redistribution, commercial use, or biometric applications.

## 12. Deep Learning and AI Datasets

For modern deep learning, choose datasets based on the modality, model architecture, compute budget, and evaluation requirements.

| Dataset or platform | Application | Link |
|---|---|---|
| Hugging Face Datasets | Text, audio, image, and multimodal data | [Explore datasets](https://huggingface.co/datasets) |
| TensorFlow Datasets | Curated datasets for machine learning | [TFDS](https://www.tensorflow.org/datasets) |
| Torchvision Datasets | Computer vision | [Torchvision](https://pytorch.org/vision/stable/datasets.html) |
| LibriSpeech | Speech recognition | [OpenSLR](https://www.openslr.org/12) |
| Common Voice | Multilingual speech data | [Mozilla Common Voice](https://commonvoice.mozilla.org/) |
| AudioSet | Audio event classification | [AudioSet](https://research.google.com/audioset/) |
| WMT | Machine translation benchmarks | [WMT](https://www2.statmt.org/wmt/) |

### Projects

- Image classification with transfer learning
- Speech-to-text experiments
- Text classification using transformers
- Machine translation
- Audio event classification
- Multimodal classification

Large datasets may require substantial storage, memory, GPU resources, and specialized loading pipelines. Start with a small subset when learning.

## 13. Recommendation System Datasets

Recommendation systems use interactions, ratings, purchases, or other preference signals.

| Dataset | Application | Source |
|---|---|---|
| MovieLens | Movie recommendations and collaborative filtering | [GroupLens](https://grouplens.org/datasets/movielens/) |
| Amazon Reviews | Product reviews and recommendations | [UCSD Amazon Reviews](https://cseweb.ucsd.edu/~jmcauley/datasets.html) |
| Book-Crossing | Book ratings and recommendations | [Dataset page](http://www2.informatik.uni-freiburg.de/~cziegler/BX/) |
| Last.fm datasets | Music listening and recommendation | [Last.fm resources](https://grouplens.org/datasets/hetrec-2011/) |

### Projects

- Popularity-based recommendations
- User-user collaborative filtering
- Item-item collaborative filtering
- Content-based recommendations
- Implicit feedback ranking
- Recommendation evaluation

Split interaction data carefully. For realistic recommendations, chronological evaluation is often more useful than a random split because it avoids using future interactions to predict earlier ones.

## 14. Finance and Business Datasets

| Dataset or source | Application | Link |
|---|---|---|
| World Bank | Macroeconomic and development analysis | [World Bank Data](https://data.worldbank.org/) |
| RBI | Indian banking and monetary statistics | [RBI Database](https://data.rbi.org.in/) |
| FRED | Economic and financial time series | [FRED](https://fred.stlouisfed.org/) |
| UCI Online Retail | Transaction and customer analysis | [UCI](https://archive.ics.uci.edu/dataset/352/online+retail) |
| Rossmann Store Sales | Retail forecasting | [Kaggle](https://www.kaggle.com/competitions/rossmann-store-sales) |
| M5 Forecasting | Retail sales forecasting | [Kaggle](https://www.kaggle.com/competitions/m5-forecasting-accuracy) |

### Projects

- Customer segmentation
- Sales forecasting
- Revenue trend analysis
- Customer lifetime value estimation
- Churn prediction
- Fraud detection
- Inventory demand forecasting

For financial prediction projects, account for transaction timing, survivorship bias, historical revisions, and information that would not have been available at prediction time.

## 15. Healthcare and Scientific Datasets

| Dataset or source | Application | Link |
|---|---|---|
| WHO Data | Global health indicators | [WHO](https://data.who.int/) |
| CDC Data | Public health and epidemiology | [CDC](https://data.cdc.gov/) |
| PhysioNet | Clinical and physiological research data | [PhysioNet](https://physionet.org/) |
| MIMIC | Critical-care research, subject to access requirements | [MIMIC](https://physionet.org/content/mimiciv/) |
| NIH Chest X-ray resources | Medical imaging research | [NIH Clinical Center](https://nihcc.app.box.com/v/ChestXray-Nihcc) |
| NASA Earthdata | Earth and climate science | [NASA Earthdata](https://www.earthdata.nasa.gov/) |

### Projects

- Health indicator analysis
- Time series analysis of physiological signals
- Research-oriented medical image classification
- Environmental exposure analysis
- Scientific measurement forecasting

**Important:** Healthcare data may contain sensitive information and may require training, institutional approvals, data-use agreements, or access credentials. Do not upload private medical data to public repositories or treat a model's output as a clinical diagnosis.

## 16. Geospatial and Environmental Datasets

| Dataset or source | Application | Link |
|---|---|---|
| NASA Earthdata | Satellite observations and Earth science | [NASA](https://www.earthdata.nasa.gov/) |
| USGS | Geological and environmental data | [USGS](https://www.usgs.gov/) |
| OpenStreetMap | Roads, buildings, and geographic features | [OpenStreetMap](https://www.openstreetmap.org/) |
| Natural Earth | Global boundaries and map features | [Natural Earth](https://www.naturalearthdata.com/) |
| World Bank Climate Change Knowledge Portal | Climate indicators and projections | [Climate Portal](https://climateknowledgeportal.worldbank.org/) |
| India Open Government Data | Indian public data and geographic indicators | [data.gov.in](https://www.data.gov.in/) |

### Projects

- Geographic visualization of population or infrastructure
- Rainfall and temperature analysis
- Land-use classification
- Air-quality trend analysis
- Regional agricultural analysis
- Spatial clustering

Useful Python libraries include GeoPandas, Rasterio, Shapely, Xarray, and Folium.

## 17. Cybersecurity Datasets

These resources are useful for defensive security analytics, intrusion detection research, anomaly detection, and security operations projects.

| Dataset | Application | Source |
|---|---|---|
| CIC-IDS2017 | Network intrusion detection research | [Canadian Institute for Cybersecurity](https://www.unb.ca/cic/datasets/ids-2017.html) |
| CSE-CIC-IDS2018 | Intrusion detection research | [Dataset page](https://www.unb.ca/cic/datasets/ids-2018.html) |
| UNSW-NB15 | Network traffic classification | [UNSW Canberra](https://research.unsw.edu.au/projects/unsw-nb15-dataset) |
| NSL-KDD | Educational intrusion detection benchmarking | [UNB](https://www.unb.ca/cic/datasets/nsl.html) |
| CTU-13 | Botnet traffic research | [CTU University](https://www.stratosphereips.org/datasets-ctu13) |
| MalwareBazaar | Malware sample intelligence, subject to terms and safety precautions | [abuse.ch](https://bazaar.abuse.ch/) |

### Defensive project ideas

- Network intrusion classification
- Anomaly detection on network flow records
- Security alert prioritization
- Malware family classification using approved static features
- Log anomaly detection
- Evaluation of false-positive and false-negative rates

### Security precautions

- Prefer extracted, labeled metadata and flow records when learning.
- Do not execute unknown malware samples on a normal workstation.
- Use isolated, authorized research environments for malware analysis.
- Verify dataset provenance, usage restrictions, and safe handling procedures.
- Avoid publishing real credentials, personal information, or sensitive organizational logs.

## 18. Common Dataset Formats

| Format | Extension | Typical use | Python tools |
|---|---|---|---|
| Comma-separated values | `.csv` | Tabular data | Pandas |
| JSON | `.json` | Nested records and APIs | `json`, Pandas |
| Excel | `.xlsx` | Spreadsheets | Pandas, openpyxl |
| Parquet | `.parquet` | Efficient analytical storage | Pandas, PyArrow |
| NumPy binary | `.npy` | Numerical arrays | NumPy |
| NumPy archive | `.npz` | Multiple numerical arrays | NumPy |
| Text | `.txt` | Documents and logs | Python file handling |
| Images | `.jpg`, `.png` | Computer vision | Pillow, OpenCV |
| Audio | `.wav`, `.flac` | Speech and audio analysis | SoundFile, librosa |
| Geospatial | `.geojson`, `.shp`, `.tif` | Geographic and raster data | GeoPandas, Rasterio |

Choose a format based on the structure of the data, available tools, storage requirements, and interoperability needs.

## 19. Loading Datasets with Python

Install the common libraries:

```bash
python -m pip install numpy pandas scikit-learn openpyxl pyarrow
```

### Load CSV using Pandas

```python
import pandas as pd

df = pd.read_csv("data.csv")

print(df.head())
print(df.shape)
print(df.info())
print(df.describe())
```

### Load Excel

```python
df = pd.read_excel("data.xlsx")
```

### Load JSON

```python
df = pd.read_json("data.json")
```

For nested JSON, inspect the structure and consider `pd.json_normalize()`.

### Load Parquet

```python
df = pd.read_parquet("data.parquet")
```

Parquet support generally requires an appropriate engine, such as PyArrow.

### Load a built-in Scikit-learn dataset

```python
from sklearn.datasets import load_iris

iris = load_iris(as_frame=True)

df = iris.frame

print(df.head())
```

### Load a dataset from OpenML

```python
from sklearn.datasets import fetch_openml

dataset = fetch_openml(
    name="adult",
    version=2,
    as_frame=True
)

X = dataset.data
y = dataset.target
```

This requires network access and downloads the dataset if it is not already cached. Verify the dataset version and target definition before using it.

### Load a dataset from Hugging Face

Install the library:

```bash
python -m pip install datasets
```

Example:

```python
from datasets import load_dataset

dataset = load_dataset("imdb")

print(dataset)
print(dataset["train"][0])
```

Check the dataset card for the exact identifier, configuration, license, and required access. Large datasets can consume significant storage.

### Inspect a newly loaded dataset

```python
print(df.shape)
print(df.columns)
print(df.dtypes)
print(df.isna().sum())
print(df.duplicated().sum())
```

Always inspect the data before training a model.

## 20. Data Quality Checklist

Use this checklist before starting an analysis or machine learning project.

### Dataset inspection

- [ ] Record the source and retrieval date.
- [ ] Record the dataset version or release.
- [ ] Inspect the number of rows and columns.
- [ ] Review column names and data types.
- [ ] Read the dataset documentation and data dictionary.

### Cleaning and validation

- [ ] Check missing values.
- [ ] Check duplicate records.
- [ ] Validate numerical ranges.
- [ ] Check date and time formats.
- [ ] Inspect category spelling and consistency.
- [ ] Identify potential outliers.
- [ ] Confirm units of measurement.
- [ ] Investigate unusual or inconsistent labels.

### Machine learning preparation

- [ ] Define the target variable.
- [ ] Separate features and target.
- [ ] Identify target leakage.
- [ ] Split the data appropriately.
- [ ] Fit preprocessing steps on training data only.
- [ ] Establish a simple baseline.
- [ ] Select appropriate evaluation metrics.
- [ ] Document assumptions and limitations.

### Reproducibility

- [ ] Record the data retrieval method.
- [ ] Fix random seeds where relevant.
- [ ] Document preprocessing steps.
- [ ] Record package versions.
- [ ] Keep a consistent dataset version.
- [ ] Avoid committing sensitive or unnecessarily large raw datasets to Git.

## 21. Dataset Licensing and Responsible Use

Before using any dataset, review its documentation and license.

### Questions to ask

1. Is the dataset publicly accessible or access-controlled?
2. Does its license permit modification and redistribution?
3. Is commercial use permitted?
4. Is attribution or citation required?
5. Are there restrictions on sensitive, personal, or biometric data?
6. Does the dataset contain copyrighted or third-party material?
7. Are there requirements for anonymization or secure storage?
8. Does the intended use comply with applicable law and platform terms?

### Good practices

- Cite the original dataset and its publisher.
- Preserve license and attribution information.
- Do not assume that publicly downloadable means unrestricted.
- Avoid exposing personally identifiable information.
- Follow data-use agreements and research ethics requirements.
- Document known sampling biases and limitations.
- Assess whether the data is representative of the intended application.

For research and portfolio

