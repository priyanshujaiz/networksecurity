# Network Security ML Pipeline

A production-ready Machine Learning pipeline for Network Security analysis with MongoDB Atlas integration. This project implements a modular, scalable architecture following ML engineering best practices.

## 📋 Prerequisites

- Python 3.8+
- MongoDB Atlas account (or local MongoDB instance)
- Git

## 🛠️ Installation

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd NetworkSecurity
```

### 2. Create Virtual Environment

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux/Mac
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Install Package in Development Mode

```bash
pip install -e .
```

## ⚙️ Configuration

### MongoDB Atlas Setup

1. Create a MongoDB Atlas account at [mongodb.com](https://www.mongodb.com/cloud/atlas)
2. Create a new cluster and database
3. Get your connection string (MongoDB URI)
4. Create a `.env` file in the root directory:

```env
MONGO_DB_URL=your_mongodb_atlas_connection_string_here
```

**Example:**
```env
MONGO_DB_URL=mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
```

### Database Configuration

Update the database and collection names in `networksecurity/constant/training_pipeline/__init__.py`:

```python
DATA_INGESTION_DATABASE_NAME: str = "YourDatabaseName"
DATA_INGESTION_COLLECTION_NAME: str = "YourCollectionName"
```

## 📊 Data Setup

### Upload Data to MongoDB Atlas

1. Place your CSV file in `Network_Data/` directory (e.g., `phisingData.csv`)
2. Run the data upload script:

```bash
python push_data.py
```

This will:
- Read the CSV file
- Convert it to JSON format
- Insert records into MongoDB Atlas

**Note:** Update `DATABASE` and `Collection` variables in `push_data.py` to match your MongoDB setup.

## 🎯 Usage

### Run the Data Ingestion Pipeline

```bash
python main.py
```

This will:
1. Connect to MongoDB Atlas
2. Export data as DataFrame
3. Save to feature store (CSV)
4. Split data into train/test sets (80/20)
5. Save train.csv and test.csv in artifact directory
6. Return artifact with file paths

### Output Structure

```
Artifacts/
└── MM_DD_YYYY_HH_MM_SS/
    └── data_ingestion/
        ├── feature_store/
        │   └── phisingData.csv
        └── ingested/
            ├── train.csv
            └── test.csv
```

## 📁 Project Structure

```
NetworkSecurity/
├── networksecurity/              # Main package
│   ├── components/               # Pipeline components
│   │   └── data_ingestion.py    # Data ingestion logic
│   ├── constant/                # Configuration constants
│   │   └── training_pipeline/
│   │       └── __init__.py      # Pipeline constants
│   ├── entity/                  # Data classes
│   │   ├── artifact_entity.py   # Artifact data classes
│   │   └── config_entity.py     # Configuration entities
│   ├── exception/               # Custom exceptions
│   │   └── exception.py
│   ├── logging/                 # Logging configuration
│   │   └── logger.py
│   ├── pipeline/                # Pipeline orchestration
│   └── utils/                   # Utility functions
├── Network_Data/                 # Data directory
│   └── phisingData.csv
├── logs/                        # Log files
├── Artifacts/                   # Generated artifacts
├── main.py                      # Entry point
├── push_data.py                 # Data upload script
├── requirements.txt             # Dependencies
├── setup.py                     # Package setup
└── .env                         # Environment variables (not in repo)
```

## 🛠️ Technologies Used

- **Python 3.8+**
- **pandas** - Data manipulation
- **numpy** - Numerical operations
- **pymongo** - MongoDB driver
- **scikit-learn** - Machine learning utilities
- **python-dotenv** - Environment variable management
- **certifi** - SSL certificate verification

## 📝 Configuration Constants

Key constants can be modified in `networksecurity/constant/training_pipeline/__init__.py`:

- `TARGET_COLUMN`: Target variable name
- `DATA_INGESTION_TRAIN_TEST_SPLIT_RATION`: Train/test split ratio (default: 0.2)
- `DATA_INGESTION_DATABASE_NAME`: MongoDB database name
- `DATA_INGESTION_COLLECTION_NAME`: MongoDB collection name

## 🔍 Current Status

✅ **Completed:**
- MongoDB Atlas integration
- Data ingestion pipeline
- Feature store management
- Train/test data splitting
- Logging and exception handling
- Artifact management

🚧 **In Progress:**
- Data validation component
- Data transformation component

## 📝 Logging

Logs are automatically generated in the `logs/` directory with timestamp format:
```
logs/MM_DD_YYYY_HH_MM_SS.log/MM_DD_YYYY_HH_MM_SS.log
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 👤 Author

**Priyanshu Ranjan**
- Email: priyanshujaiz341@gmail.com

**Note:** Make sure to add `.env` to your `.gitignore` file to keep your MongoDB credentials secure!


## Quick Start

1. Clone repo → `git clone <repo-url>`
2. Create venv → `python -m venv venv` → `venv\Scripts\activate`
3. Install deps → `pip install -r requirements.txt` → `pip install -e .`
4. Add `.env` → `MONGO_DB_URL=your_connection_string`
5. Upload data → `python push_data.py`
6. Run pipeline → `python main.py`
