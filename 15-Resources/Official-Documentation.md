# Official Documentation for Data Science

A curated directory of official documentation for Python, data analysis, statistics, machine learning, deep learning, artificial intelligence, visualization, MLOps, cloud platforms, and data engineering.

This resource is designed to help learners find reliable technical references, understand library APIs, follow best practices, and build real-world data science projects.

## Table of Contents

- [1. Python and Development Environment](#1-python-and-development-environment)
- [2. Numerical Computing](#2-numerical-computing)
- [3. Data Analysis and Manipulation](#3-data-analysis-and-manipulation)
- [4. Data Visualization](#4-data-visualization)
- [5. Statistics and Scientific Computing](#5-statistics-and-scientific-computing)
- [6. Machine Learning](#6-machine-learning)
- [7. Deep Learning and Neural Networks](#7-deep-learning-and-neural-networks)
- [8. Natural Language Processing](#8-natural-language-processing)
- [9. Generative AI and Large Language Models](#9-generative-ai-and-large-language-models)
- [10. Computer Vision](#10-computer-vision)
- [11. Data Engineering and Big Data](#11-data-engineering-and-big-data)
- [12. Databases and Query Languages](#12-databases-and-query-languages)
- [13. APIs and Data Collection](#13-apis-and-data-collection)
- [14. MLOps and Model Deployment](#14-mlops-and-model-deployment)
- [15. Cloud Platforms](#15-cloud-platforms)
- [16. Experiment Tracking and Model Management](#16-experiment-tracking-and-model-management)
- [17. Development Tools](#17-development-tools)
- [18. Data Ethics, Privacy, and Responsible AI](#18-data-ethics-privacy-and-responsible-ai)
- [19. Learning and Reference Resources](#19-learning-and-reference-resources)
- [20. How to Use Official Documentation](#20-how-to-use-official-documentation)
- [21. Recommended Learning Sequence](#21-recommended-learning-sequence)

## 1. Python and Development Environment

Python is the primary programming language used throughout this roadmap.

| Resource | Purpose | Official link |
|---|---|---|
| Python | Language documentation and tutorials | [Python Documentation](https://docs.python.org/3/) |
| Python Tutorial | Core language fundamentals | [Official Tutorial](https://docs.python.org/3/tutorial/) |
| Python Standard Library | Built-in modules and utilities | [Library Reference](https://docs.python.org/3/library/) |
| Python Language Reference | Language syntax and semantics | [Language Reference](https://docs.python.org/3/reference/) |
| Python Packaging | Package distribution and management | [Packaging Guide](https://packaging.python.org/) |
| pip | Python package installation | [pip Documentation](https://pip.pypa.io/en/stable/) |
| venv | Virtual environment management | [venv Documentation](https://docs.python.org/3/library/venv.html) |
| Jupyter | Interactive computing notebooks | [Jupyter Documentation](https://docs.jupyter.org/en/latest/) |
| IPython | Interactive Python shell | [IPython Documentation](https://ipython.readthedocs.io/en/stable/) |

### Recommended topics

- Variables, data types, operators, and control flow
- Functions, classes, and modules
- Lists, dictionaries, sets, and tuples
- Exception handling and file operations
- Iterators, generators, and decorators
- Virtual environments and dependency management
- Jupyter Notebook workflows

## 2. Numerical Computing

These libraries support numerical arrays, matrix operations, scientific calculations, and efficient computation.

| Library | Purpose | Official documentation |
|---|---|---|
| NumPy | Multidimensional arrays and numerical computing | [NumPy Docs](https://numpy.org/doc/) |
| SciPy | Scientific algorithms and numerical methods | [SciPy Docs](https://docs.scipy.org/doc/scipy/) |
| SymPy | Symbolic mathematics | [SymPy Docs](https://docs.sympy.org/latest/index.html) |
| mpmath | Arbitrary-precision arithmetic | [mpmath Docs](https://mpmath.org/doc/current/) |
| Numba | JIT compilation for numerical Python | [Numba Docs](https://numba.readthedocs.io/en/stable/) |

### Recommended topics

- Array creation and indexing
- Broadcasting and vectorization
- Linear algebra
- Random number generation
- Optimization and numerical integration
- Scientific computing performance

Start with NumPy, then explore SciPy for specialized scientific algorithms.

## 3. Data Analysis and Manipulation

Use these libraries to load, clean, transform, aggregate, and analyze structured datasets.

| Library | Purpose | Official documentation |
|---|---|---|
| Pandas | DataFrames and tabular analysis | [Pandas Docs](https://pandas.pydata.org/docs/) |
| Polars | Fast DataFrame processing | [Polars Docs](https://docs.pola.rs/) |
| Dask | Parallel and out-of-core data processing | [Dask Docs](https://docs.dask.org/en/stable/) |
| Xarray | Labeled multidimensional scientific data | [Xarray Docs](https://docs.xarray.dev/en/stable/) |
| PyArrow | Columnar data and Apache Arrow | [PyArrow Docs](https://arrow.apache.org/docs/python/) |
| DuckDB | Analytical SQL database | [DuckDB Docs](https://duckdb.org/docs/stable/) |

### Recommended topics

- Loading CSV, Excel, JSON, and Parquet
- Handling missing values and duplicates
- Filtering, grouping, and joining datasets
- Date and time processing
- Data reshaping and aggregation
- Memory-efficient data processing

## 4. Data Visualization

Visualization helps communicate patterns, distributions, trends, and relationships.

| Library | Purpose | Official documentation |
|---|---|---|
| Matplotlib | General-purpose plotting | [Matplotlib Docs](https://matplotlib.org/stable/) |
| Seaborn | Statistical visualization | [Seaborn Docs](https://seaborn.pydata.org/) |
| Plotly | Interactive visualizations | [Plotly Python Docs](https://plotly.com/python/) |
| Bokeh | Interactive browser-based charts | [Bokeh Docs](https://docs.bokeh.org/en/latest/) |
| Altair | Declarative statistical visualization | [Altair Docs](https://altair-viz.github.io/) |
| Streamlit | Interactive data applications | [Streamlit Docs](https://docs.streamlit.io/) |
| Dash | Analytical web applications | [Dash Docs](https://dash.plotly.com/) |

### Recommended topics

- Line, bar, scatter, and histogram plots
- Distribution and correlation analysis
- Subplots, legends, and annotations
- Interactive dashboards
- Visualization design and accessibility
- Communicating results to nontechnical audiences

## 5. Statistics and Scientific Computing

These resources help build a foundation in probability, inference, experimental design, and statistical modeling.

| Resource | Purpose | Official documentation |
|---|---|---|
| SciPy Statistics | Probability distributions and statistical tests | [SciPy Stats](https://docs.scipy.org/doc/scipy/reference/stats.html) |
| Statsmodels | Statistical models and econometrics | [Statsmodels Docs](https://www.statsmodels.org/stable/) |
| PyMC | Bayesian statistical modeling | [PyMC Docs](https://www.pymc.io/projects/docs/en/stable/) |
| ArviZ | Bayesian model diagnostics and visualization | [ArviZ Docs](https://python.arviz.org/en/stable/) |
| Pingouin | Statistical testing and analysis | [Pingouin Docs](https://pingouin-stats.org/build/html/index.html) |

### Recommended topics

- Descriptive statistics
- Probability distributions
- Sampling and confidence intervals
- Hypothesis testing
- Correlation and regression
- Experimental design
- Bayesian inference
- Statistical assumptions and limitations

## 6. Machine Learning

Scikit-learn is a central library for classical machine learning. Its official documentation covers supervised learning, unsupervised learning, preprocessing, model selection, and evaluation. [Scikit-learn Documentation](https://scikit-learn.org/stable/documentation)

| Resource | Purpose | Official documentation |
|---|---|---|
| Scikit-learn | Classical machine learning | [Scikit-learn Docs](https://scikit-learn.org/stable/) |
| Getting Started | Core estimator workflow | [Getting Started](https://scikit-learn.org/stable/getting_started.html) |
| User Guide | Detailed algorithms and techniques | [User Guide](https://scikit-learn.org/stable/user_guide.html) |
| API Reference | Classes, methods, and parameters | [API Reference](https://scikit-learn.org/stable/api/index.html) |
| Imbalanced-learn | Resampling for imbalanced datasets | [imbalanced-learn Docs](https://imbalanced-learn.org/stable/) |
| XGBoost | Gradient-boosted trees | [XGBoost Docs](https://xgboost.readthedocs.io/en/stable/) |
| LightGBM | Gradient boosting framework | [LightGBM Docs](https://lightgbm.readthedocs.io/en/stable/) |
| CatBoost | Gradient boosting with categorical features | [CatBoost Docs](https://catboost.ai/en/docs/) |
| H2O-3 | Distributed machine learning | [H2O Docs](https://docs.h2o.ai/h2o/latest-stable/h2o-docs/index.html) |
| InterpretML | Model interpretation | [InterpretML Docs](https://interpret.ml/docs/) |

### Recommended topics

- Classification and regression
- Clustering and anomaly detection
- Feature engineering and selection
- Preprocessing and pipelines
- Cross-validation and hyperparameter tuning
- Model evaluation and calibration
- Imbalanced classification
- Model interpretability

## 7. Deep Learning and Neural Networks

These frameworks support neural networks, GPU acceleration, and deep learning research.

| Framework | Purpose | Official documentation |
|---|---|---|
| PyTorch | Deep learning and tensor computation | [PyTorch Docs](https://docs.pytorch.org/docs/stable/) |
| TensorFlow | Machine learning and deep learning | [TensorFlow Docs](https://www.tensorflow.org/learn) |
| Keras | High-level neural network API | [Keras Docs](https://keras.io/) |
| JAX | High-performance numerical computing and automatic differentiation | [JAX Docs](https://docs.jax.dev/en/latest/) |
| PyTorch Lightning | Structured PyTorch training workflows | [Lightning Docs](https://lightning.ai/docs/pytorch/stable/) |
| TensorBoard | Training visualization and experiment monitoring | [TensorBoard Docs](https://www.tensorflow.org/tensorboard) |

### Recommended topics

- Perceptrons and neural networks
- Forward propagation and backpropagation
- Loss functions and optimizers
- Regularization and dropout
- CNNs, RNNs, and attention
- GPU training
- Model checkpoints
- Reproducibility and numerical precision

## 8. Natural Language Processing

NLP resources support text processing, language understanding, information extraction, and text classification.

| Library | Purpose | Official documentation |
|---|---|---|
| NLTK | Tokenization and traditional NLP | [NLTK Docs](https://www.nltk.org/) |
| spaCy | Production-oriented NLP pipelines | [spaCy Docs](https://spacy.io/usage) |
| Hugging Face Tokenizers | Fast text tokenization | [Tokenizers Docs](https://huggingface.co/docs/tokenizers/) |
| Gensim | Topic modeling and document similarity | [Gensim Docs](https://radimrehurek.com/gensim/) |
| Stanza | Multilingual NLP | [Stanza Docs](https://stanfordnlp.github.io/stanza/) |
| TextBlob | Basic text processing | [TextBlob Docs](https://textblob.readthedocs.io/en/dev/) |

### Recommended topics

- Text normalization and tokenization
- Stop words and stemming
- Lemmatization
- Bag of Words and TF-IDF
- Text classification
- Named entity recognition
- Topic modeling
- Evaluation of language processing systems

## 9. Generative AI and Large Language Models

These resources support transformer models, generative AI applications, embeddings, and retrieval-augmented generation.

| Resource | Purpose | Official documentation |
|---|---|---|
| Hugging Face Transformers | Pretrained transformer models | [Transformers Docs](https://huggingface.co/docs/transformers/) |
| Hugging Face Datasets | Loading and processing AI datasets | [Datasets Docs](https://huggingface.co/docs/datasets/) |
| Hugging Face PEFT | Parameter-efficient fine-tuning | [PEFT Docs](https://huggingface.co/docs/peft/) |
| Hugging Face Accelerate | Distributed and accelerated training | [Accelerate Docs](https://huggingface.co/docs/accelerate/) |
| OpenAI API | Building applications with OpenAI models | [OpenAI API Docs](https://developers.openai.com/api/docs/) |
| vLLM | High-throughput LLM inference | [vLLM Docs](https://docs.vllm.ai/en/latest/) |
| Ollama | Running supported models locally | [Ollama Docs](https://docs.ollama.com/) |
| Sentence Transformers | Embeddings and semantic similarity | [Sentence Transformers Docs](https://sbert.net/) |
| LangChain | LLM application development | [LangChain Docs](https://docs.langchain.com/) |
| LlamaIndex | Data-centric LLM applications | [LlamaIndex Docs](https://developers.llamaindex.ai/python/framework/) |
| FAISS | Similarity search and vector indexing | [FAISS Wiki](https://github.com/facebookresearch/faiss/wiki) |

### Recommended topics

- Transformer architecture
- Tokenization and embeddings
- Prompt engineering
- Fine-tuning and parameter-efficient tuning
- Vector similarity search
- Retrieval-augmented generation (RAG)
- Evaluation and hallucination analysis
- Safety, privacy, and responsible deployment

## 10. Computer Vision

These libraries provide tools for image processing, object detection, segmentation, and visual learning.

| Library | Purpose | Official documentation |
|---|---|---|
| OpenCV | Computer vision and image processing | [OpenCV Docs](https://docs.opencv.org/) |
| Torchvision | Vision datasets, models, and transforms | [Torchvision Docs](https://pytorch.org/vision/stable/) |
| Ultralytics | Supported YOLO workflows | [Ultralytics Docs](https://docs.ultralytics.com/) |
| Pillow | Image loading and manipulation | [Pillow Docs](https://pillow.readthedocs.io/en/stable/) |
| Albumentations | Image augmentation | [Albumentations Docs](https://albumentations.ai/docs/) |
| Kornia | Differentiable computer vision | [Kornia Docs](https://kornia.readthedocs.io/en/latest/) |

### Recommended topics

- Image representation and color spaces
- Filtering and edge detection
- Image classification
- Object detection
- Semantic and instance segmentation
- Image augmentation
- Transfer learning
- Computer vision evaluation metrics

## 11. Data Engineering and Big Data

These tools support data ingestion, transformation, orchestration, distributed processing, and analytical storage.

| Technology | Purpose | Official documentation |
|---|---|---|
| Apache Spark | Distributed data processing | [Spark Docs](https://spark.apache.org/docs/latest/) |
| PySpark | Python API for Spark | [PySpark Docs](https://spark.apache.org/docs/latest/api/python/) |
| Apache Airflow | Workflow orchestration | [Airflow Docs](https://airflow.apache.org/docs/) |
| Apache Kafka | Event streaming | [Kafka Docs](https://kafka.apache.org/documentation/) |
| Apache Flink | Stream processing | [Flink Docs](https://flink.apache.org/) |
| dbt | Data transformation workflows | [dbt Docs](https://docs.getdbt.com/) |
| Apache Iceberg | Table format for large analytical datasets | [Iceberg Docs](https://iceberg.apache.org/docs/latest/) |
| Great Expectations | Data quality validation | [GX Docs](https://docs.greatexpectations.io/) |
| Prefect | Workflow orchestration | [Prefect Docs](https://docs.prefect.io/) |

### Recommended topics

- ETL and ELT
- Batch and stream processing
- Data pipelines and orchestration
- Data validation
- Distributed computing
- Data lakes and lakehouses
- Partitioning and file formats
- Pipeline monitoring and recovery

## 12. Databases and Query Languages

SQL and database fundamentals are essential for accessing, analyzing, and preparing data.

| Technology | Purpose | Official documentation |
|---|---|---|
| PostgreSQL | Relational database | [PostgreSQL Docs](https://www.postgresql.org/docs/) |
| MySQL | Relational database | [MySQL Docs](https://dev.mysql.com/doc/) |
| SQLite | Lightweight embedded database | [SQLite Docs](https://www.sqlite.org/docs.html) |
| MongoDB | Document database | [MongoDB Docs](https://www.mongodb.com/docs/) |
| SQLAlchemy | Python SQL toolkit and ORM | [SQLAlchemy Docs](https://docs.sqlalchemy.org/en/20/) |
| SQL Server | Relational database platform | [Microsoft SQL Docs](https://learn.microsoft.com/en-us/sql/) |
| BigQuery | Cloud data warehouse | [BigQuery Docs](https://cloud.google.com/bigquery/docs) |
| Snowflake | Cloud data platform | [Snowflake Docs](https://docs.snowflake.com/) |
| Redis | In-memory data store | [Redis Docs](https://redis.io/docs/latest/) |

### Recommended topics

- SELECT, WHERE, GROUP BY, and ORDER BY
- JOIN operations
- Subqueries and common table expressions
- Window functions
- Aggregations
- Indexing and query optimization
- Transactions and data integrity
- Python database connections

## 13. APIs and Data Collection

These resources help collect data from services and websites while following applicable access policies.

| Library or resource | Purpose | Official documentation |
|---|---|---|
| Requests | HTTP requests | [Requests Docs](https://requests.readthedocs.io/en/latest/) |
| HTTPX | Synchronous and asynchronous HTTP | [HTTPX Docs](https://www.python-httpx.org/) |
| Beautiful Soup | HTML parsing | [Beautiful Soup Docs](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) |
| Scrapy | Web crawling and extraction | [Scrapy Docs](https://docs.scrapy.org/en/latest/) |
| Playwright | Browser automation and testing | [Playwright Python Docs](https://playwright.dev/python/docs/intro) |
| FastAPI | Building APIs | [FastAPI Docs](https://fastapi.tiangolo.com/) |
| Pydantic | Data validation and settings | [Pydantic Docs](https://docs.pydantic.dev/latest/) |

### Recommended topics

- HTTP methods and status codes
- JSON and REST APIs
- Authentication and API keys
- Pagination and rate limits
- Retries and error handling
- Data validation
- Responsible web scraping
- API development and documentation

Respect website terms, access restrictions, rate limits, and privacy requirements when collecting data.

## 14. MLOps and Model Deployment

MLOps connects model development with reliable production systems.

| Technology | Purpose | Official documentation |
|---|---|---|
| MLflow | Experiment tracking and model lifecycle | [MLflow Docs](https://mlflow.org/docs/latest/) |
| BentoML | Packaging and serving models | [BentoML Docs](https://docs.bentoml.com/) |
| FastAPI | Model inference APIs | [FastAPI Docs](https://fastapi.tiangolo.com/) |
| Docker | Containerization | [Docker Docs](https://docs.docker.com/) |
| Kubernetes | Container orchestration | [Kubernetes Docs](https://kubernetes.io/docs/home/) |
| KServe | Kubernetes model serving | [KServe Docs](https://kserve.github.io/website/) |
| Evidently | Data and ML monitoring | [Evidently Docs](https://docs.evidentlyai.com/) |
| Feast | Feature store | [Feast Docs](https://docs.feast.dev/) |

### Recommended topics

- Experiment tracking
- Model versioning
- Model serialization
- Inference APIs
- Containerized deployments
- CI/CD for machine learning
- Data and concept drift
- Monitoring latency and model quality

## 15. Cloud Platforms

Cloud platforms provide storage, computing, managed machine learning, data engineering, and deployment services.

| Platform | Purpose | Official documentation |
|---|---|---|
| Amazon Web Services | Cloud computing and data services | [AWS Docs](https://docs.aws.amazon.com/) |
| Amazon SageMaker AI | Managed machine learning | [SageMaker Docs](https://docs.aws.amazon.com/sagemaker/) |
| Google Cloud | Cloud infrastructure and data analytics | [Google Cloud Docs](https://cloud.google.com/docs) |
| Vertex AI | Managed AI and machine learning | [Vertex AI Docs](https://cloud.google.com/vertex-ai/docs) |
| Microsoft Azure | Cloud computing and analytics | [Azure Docs](https://learn.microsoft.com/en-us/azure/) |
| Azure Machine Learning | Model training and lifecycle management | [Azure ML Docs](https://learn.microsoft.com/en-us/azure/machine-learning/) |
| Databricks | Data engineering and machine learning | [Databricks Docs](https://docs.databricks.com/) |

### Recommended topics

- Cloud storage
- Virtual machines and compute
- Managed notebooks
- Data warehouses
- Distributed processing
- Model training and deployment
- Identity and access management
- Cost management and security

## 16. Experiment Tracking and Model Management

These resources help organize experiments, metrics, artifacts, and model versions.

| Tool | Purpose | Official documentation |
|---|---|---|
| MLflow Tracking | Log experiments and metrics | [MLflow](https://mlflow.org/docs/latest/ml/tracking/) |
| Weights & Biases | Experiment tracking and visualization | [W&B Docs](https://docs.wandb.ai/) |
| TensorBoard | Visualize training metrics | [TensorBoard](https://www.tensorflow.org/tensorboard) |
| DVC | Data and model versioning | [DVC Docs](https://dvc.org/doc) |
| Neptune | Experiment tracking resources | [Neptune Documentation](https://docs.neptune.ai/) |

### Recommended topics

- Experiment names and configuration
- Training and validation metrics
- Artifact storage
- Dataset and model versioning
- Reproducibility
- Comparing experiments
- Model registry workflows

Check current product availability, service lifecycle, and licensing before selecting a tool for a new project.

## 17. Development Tools

These tools improve coding, testing, collaboration, and project quality.

| Tool | Purpose | Official documentation |
|---|---|---|
| Git | Version control | [Git Docs](https://git-scm.com/doc) |
| GitHub | Source control and collaboration | [GitHub Docs](https://docs.github.com/) |
| VS Code | Code editor | [VS Code Docs](https://code.visualstudio.com/docs) |
| PyCharm | Python IDE | [PyCharm Docs](https://www.jetbrains.com/help/pycharm/) |
| pytest | Automated testing | [pytest Docs](https://docs.pytest.org/en/stable/) |
| Ruff | Python linting and formatting | [Ruff Docs](https://docs.astral.sh/ruff/) |
| Black | Python code formatting | [Black Docs](https://black.readthedocs.io/en/stable/) |
| mypy | Static type checking | [mypy Docs](https://mypy.readthedocs.io/en/stable/) |
| pre-commit | Automated code-quality hooks | [pre-commit Docs](https://pre-commit.com/) |
| Poetry | Python dependency and project management | [Poetry Docs](https://python-poetry.org/docs/) |

### Recommended topics

- Git branches and pull requests
- Virtual environments
- Code formatting and linting
- Unit and integration testing
- Dependency pinning
- Type checking
- Continuous integration
- Project documentation

## 18. Data Ethics, Privacy, and Responsible AI

Responsible data science requires understanding how datasets and models can affect people and organizations.

| Resource | Purpose | Official link |
|---|---|---|
| NIST AI Risk Management Framework | AI risk management | [NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework) |
| NIST Privacy Framework | Privacy risk management | [NIST Privacy Framework](https://www.nist.gov/privacy-framework) |
| OECD AI Principles | Principles for trustworthy AI | [OECD AI](https://oecd.ai/en/ai-principles) |
| Google Responsible AI | Responsible AI practices | [Google AI](https://ai.google/responsibility/) |
| Microsoft Responsible AI | Responsible AI guidance | [Microsoft Learn](https://learn.microsoft.com/en-us/azure/machine-learning/concept-responsible-ai) |
| IBM AI Fairness 360 | Bias and fairness assessment toolkit | [AI Fairness 360](https://aif360.res.ibm.com/) |
| SHAP | Model explanation methods | [SHAP Docs](https://shap.readthedocs.io/en/latest/) |

### Recommended topics

- Data privacy and consent
- Bias and representativeness
- Fairness metrics
- Explainability and interpretability
- Security of data and models
- Documentation of limitations
- Human oversight
- Monitoring for unintended consequences

These resources are educational references, not substitutes for applicable legal advice or formal risk assessments.

## 19. Learning and Reference Resources

Use these resources alongside the documentation for structured learning and practical exercises.

| Resource | Purpose | Link |
|---|---|---|
| Kaggle Learn | Short practical data science lessons | [Kaggle Learn](https://www.kaggle.com/learn) |
| Google Machine Learning Crash Course | Introductory machine learning | [Google MLCC](https://developers.google.com/machine-learning/crash-course) |
| DeepLearning.AI | Machine learning and AI courses | [DeepLearning.AI](https://www.deeplearning.ai/) |
| fast.ai | Practical deep learning education | [fast.ai](https://course.fast.ai/) |
| Hugging Face Course | NLP and transformer learning | [Hugging Face Course](https://huggingface.co/learn) |
| MIT OpenCourseWare | University-level learning materials | [MIT OCW](https://ocw.mit.edu/) |
| Stanford Online | Academic courses and resources | [Stanford Online](https://online.stanford.edu/) |
| UCI Machine Learning Repository | Machine learning datasets | [UCI Repository](https://archive.ics.uci.edu/) |

Prefer the official documentation when checking exact API behavior, supported parameters, compatibility, and deprecation notices. Use courses and tutorials to develop conceptual understanding.

## 20. How to Use Official Documentation

Follow this workflow when learning a new library or API.

### Step 1: Read the introduction

Understand what the library does and which problems it solves.

### Step 2: Complete the getting-started tutorial

Run the examples in a notebook or isolated development environment.

### Step 3: Learn the API

Review the functions, classes, parameters, return values, and exceptions relevant to your task.

### Step 4: Study a complete example

Follow an example from data preparation through execution and evaluation.

### Step 5: Build a small project

Apply the library to a real dataset and record the steps taken.

### Step 6: Verify compatibility

Check the installed library version and the documentation for that release when APIs differ.

### Step 7: Review common pitfalls

Look for guidance on performance, data leakage, numerical precision, security, and limitations.

### Step 8: Record useful references

Keep a short note with the relevant documentation URL, example, package version, and any important caveats.

## 21. Recommended Learning Sequence

Use the following sequence to navigate the CubicNova Data Science Roadmap.

1. **Python fundamentals:** Python, Jupyter, Git, and virtual environments.
2. **Numerical computing:** NumPy and SciPy.
3. **Data analysis:** Pandas, SQL, and data quality.
4. **Visualization:** Matplotlib, Seaborn, and Plotly.
5. **Statistics:** Probability, inference, and statistical modeling.
6. **Machine learning:** Scikit-learn, preprocessing, and evaluation.
7. **Advanced machine learning:** Boosting, clustering, feature selection, and interpretability.
8. **Deep learning:** PyTorch or TensorFlow and Keras.
9. **Specialization:** NLP, computer vision, time series, or generative AI.
10. **Data engineering:** SQL, Spark, orchestration, and analytical storage.
11. **MLOps:** Experiment tracking, pipelines, testing, deployment, and monitoring.
12. **Cloud deployment:** Choose AWS, Google Cloud, or Azure based on project requirements.

---

## Final Recommendations

- Use official documentation as the source of truth for API behavior.
- Start with tutorials before exploring advanced reference pages.
- Verify the documentation version against your installed package.
- Prefer reproducible examples and tested code.
- Check licensing, privacy, and security requirements before using external datasets.
- Link relevant documentation directly from the learning modules and project READMEs in this roadmap.

**Goal:** Make this file the central reference directory for learners who need reliable, maintained documentation while progressing through data science and machine learning.

