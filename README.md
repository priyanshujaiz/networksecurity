# Network Security ML Pipeline

A production-ready Machine Learning pipeline for Network Security analysis with MongoDB Atlas integration, implementing modular architecture and ML engineering best practices.

## ✨ Features

- 🔄 **MongoDB Atlas Integration** - Cloud-based data ingestion
- 📊 **Data Validation** - Schema validation & statistical drift detection
- 🔧 **Data Transformation** - Automated preprocessing with KNN Imputer
- 📁 **Artifact Management** - Timestamped experiment tracking
- 🏗️ **Modular Architecture** - Scalable, maintainable codebase
- 📝 **Production-Grade** - Comprehensive logging & error handling

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- MongoDB Atlas account

### Installation

```bash
# Clone repository
git clone <your-repo-url>
cd NetworkSecurity

# Create virtual environment
python -m venv venv
venv\Scripts\activate  # Windows
# source venv/bin/activate  # Linux/Mac

# Install dependencies
pip install -r requirements.txt
pip install -e .
```

### Configuration

1. Create `.env` file:
```env
MONGO_DB_URL=your_mongodb_atlas_connection_string
```

2. Update constants in `networksecurity/constant/training_pipeline/__init__.py`:
```python
DATA_INGESTION_DATABASE_NAME: str = "YourDatabaseName"
DATA_INGESTION_COLLECTION_NAME: str = "YourCollectionName"
```

3. Upload data to MongoDB:
```bash
python push_data.py
```

### Run Pipeline

```bash
python main.py
```

## 📋 Pipeline Components

### 1. Data Ingestion
- Reads from MongoDB Atlas
- Exports to feature store
- Splits train/test (80/20)

### 2. Data Validation
- Schema validation (column count)
- Drift detection (Kolmogorov-Smirnov test)
- Generates drift report (YAML)

### 3. Data Transformation
- KNN Imputer for missing values
- Feature preprocessing
- Saves transformed NumPy arrays & preprocessing object

## 📁 Project Structure
NetworkSecurity/
├── networksecurity/
│   ├── components/          # Pipeline components
│   ├── constant/           # Configuration constants
│   ├── entity/            # Data classes (configs, artifacts)
│   ├── utils/             # Utility functions
│   ├── exception/         # Custom exceptions
│   └── logging/           # Logging setup
├── data_schema/           # Schema definitions
├── Network_Data/          # Raw data
├── Artifacts/             # Generated outputs
├── main.py               # Entry point
└── push_data.py          # Data upload utility


 📊 Output Structure
 Artifacts/MM_DD_YYYY_HH_MM_SS/
├── data_ingestion/
│   ├── feature_store/phisingData.csv
│   └── ingested/{train,test}.csv
├── data_validation/
│   ├── validated/{train,test}.csv
│   └── drift_report/report.yaml
└── data_transformation/
    ├── transformed_data/{train,test}.npy
    └── transformed_object/preprocessing.pkl

📝 Configuration
Key settings in networksecurity/constant/training_pipeline/__init__.py:
TARGET_COLUMN: Target variable name
DATA_INGESTION_TRAIN_TEST_SPLIT_RATION: Split ratio (default: 0.2)
DATA_TRANSFORMATION_IMPUTER_PARAMS: KNN Imputer parameters


🤝 Contributing
Contributions welcome! Please open an issue or submit a PR.


👤 Author
Priyanshu Ranjan
Email: priyanshujaiz341@gmail.com