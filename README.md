# 🛡️ Network Security ML Pipeline

A **production-ready, end-to-end Machine Learning system** for Network Security (phishing detection) with automated training, real-time predictions, and a modern web dashboard.

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-green.svg)
![MLflow](https://img.shields.io/badge/MLflow-Tracking-orange.svg)
![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-success.svg)

---

## ✨ Features

### **Core Pipeline**
- 🔄 **MongoDB Atlas Integration** - Cloud-based data storage
- ✅ **Data Validation** - Schema validation & drift detection
- 🔧 **Data Transformation** - KNN Imputer preprocessing
- 🤖 **Auto ML Training** - 5 algorithms with hyperparameter tuning
- 📊 **Experiment Tracking** - MLflow + DagsHub integration
- 🎯 **Best Model Selection** - Automatic by F1 score

### **Web Application**
- 🌐 **FastAPI Backend** - RESTful API endpoints
- 📈 **Modern Dashboard** - Single-page UI with real-time metrics
- 🚀 **One-Click Training** - Start training from the dashboard
- 🔮 **CSV Predictions** - Upload files and get instant predictions
- 📉 **Interactive Charts** - Performance visualization with Chart.js
- 🎨 **Professional Design** - Clean, responsive, mobile-friendly

---

## 🏗️ Architecture

```
┌──────────────┐
│ MongoDB Atlas│
└──────┬───────┘
       │
       ↓
┌──────────────────────────────────────┐
│   4-Stage ML Pipeline                │
│  ├─ Data Ingestion                   │
│  ├─ Data Validation                  │
│  ├─ Data Transformation              │
│  └─ Model Training (5 algorithms)    │
└──────────────┬───────────────────────┘
               │
               ↓
       ┌───────────────┐
       │ MLflow/DagsHub│
       └───────┬───────┘
               │
               ↓
       ┌───────────────┐
       │  FastAPI App  │
       └───────┬───────┘
               │
               ↓
     ┌─────────────────┐
     │  Web Dashboard  │
     │  - Train Models │
     │  - Predictions  │
     │  - Metrics View │
     └─────────────────┘
```

---

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- MongoDB Atlas account
- Git

### Installation

```bash
# Clone repository
git clone https://github.com/yourusername/NetworkSecurity.git
cd NetworkSecurity

# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate
# Linux/Mac:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
pip install -e .
```

### Configuration

1. **Create `.env` file** in the root directory:
```env
MONGO_DB_URL=mongodb+srv://username:password@cluster.mongodb.net/
```

2. **Update database constants** (if needed) in `networksecurity/constant/training_pipeline/__init__.py`:
```python
DATA_INGESTION_DATABASE_NAME: str = "YourDatabaseName"
DATA_INGESTION_COLLECTION_NAME: str = "YourCollectionName"
```

3. **Upload data to MongoDB**:
```bash
python push_data.py
```

### Run the Application

```bash
# Start the FastAPI server
python app.py

# Or use uvicorn directly
uvicorn app:app --host localhost --port 8000 --reload
```

**Access the dashboard**: Open your browser to **http://localhost:8000**

---

## 📊 ML Pipeline

### **Stage 1: Data Ingestion**
- Fetches data from MongoDB Atlas
- Creates feature store (CSV export)
- Splits into train/test (80/20)
- **Output**: `Artifacts/[timestamp]/data_ingestion/`

### **Stage 2: Data Validation**
- Schema validation (column count)
- Statistical drift detection (KS test)
- Generates drift report (YAML)
- **Output**: `Artifacts/[timestamp]/data_validation/`

### **Stage 3: Data Transformation**
- KNN Imputer for missing values
- Feature scaling and preprocessing
- Saves preprocessor object
- **Output**: `Artifacts/[timestamp]/data_transformation/`

### **Stage 4: Model Training**
- Tests 5 algorithms:
  - Random Forest
  - Gradient Boosting
  - Decision Tree
  - Logistic Regression
  - AdaBoost
- Hyperparameter tuning (GridSearchCV)
- Selects best model by F1 score
- Logs to MLflow/DagsHub
- **Output**: `final_model/` and `Artifacts/[timestamp]/model_trainer/`

---

## 🌐 Web Dashboard

### Features

**Overview Cards**
- Total Experiments
- Best F1 Score
- Best Precision
- Best Recall

**Performance Analytics**
- Performance Trends Chart (Line)
- Metrics Comparison Chart (Bar)

**Quick Actions**
- **Train Model**: Start training pipeline with one click
- **Make Predictions**: Upload CSV and get results
- **View Artifacts**: Access MLflow UI and API docs

**Experiments Table**
- Recent 10 training runs
- Full metrics (F1, Precision, Recall)
- Run status and timestamps

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Redirects to dashboard |
| GET | `/dashboard` | Main dashboard UI |
| GET | `/train` | Start training pipeline |
| POST | `/predict` | Upload CSV for predictions |
| GET | `/api/mlflow/runs` | Get MLflow experiment data (JSON) |
| GET | `/docs` | Swagger API documentation |

### Example: Make Predictions

**Using cURL:**
```bash
curl -X POST "http://localhost:8000/predict" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@your_data.csv"
```

**Using Python:**
```python
import requests

files = {'file': open('your_data.csv', 'rb')}
response = requests.post('http://localhost:8000/predict', files=files)
print(response.text)
```

---

## 📁 Project Structure

```
NetworkSecurity/
├── networksecurity/              # Main package
│   ├── components/              # Pipeline components
│   │   ├── data_ingestion.py
│   │   ├── data_validation.py
│   │   ├── data_transformation.py
│   │   └── model_trainer.py
│   ├── constant/                # Configuration constants
│   ├── entity/                  # Data classes (configs, artifacts)
│   ├── exception/               # Custom exceptions
│   ├── logging/                 # Logging setup
│   ├── pipeline/                # Pipeline orchestration
│   │   ├── training_pipeline.py
│   │   └── batch_prediction.py
│   └── utils/                   # Utility functions
│
├── static/                      # Frontend assets
│   ├── css/
│   │   └── dashboard.css        # Dashboard styling
│   └── js/
│       └── dashboard.js         # Interactive features
│
├── template/                    # HTML templates
│   ├── dashboard.html           # Main dashboard
│   └── table.html              # Prediction results
│
├── Artifacts/                   # Training outputs (timestamped)
├── final_model/                 # Production models
│   ├── model.pkl
│   └── preprocessor.pkl
├── output_prediction/           # Prediction results
├── mlruns/                      # MLflow local storage
├── logs/                        # Application logs
│
├── app.py                       # FastAPI application
├── main.py                      # Training entry point
├── push_data.py                 # MongoDB data upload
├── setup.py                     # Package setup
├── requirements.txt             # Dependencies
└── .env                         # Environment variables
```

---

## 💻 Tech Stack

### **Backend**
- FastAPI - Web framework
- Python 3.8+ - Programming language
- MongoDB Atlas - Cloud database
- PyMongo - MongoDB driver

### **Machine Learning**
- Scikit-learn - ML algorithms
- NumPy - Numerical computing
- Pandas - Data manipulation
- MLflow - Experiment tracking
- DagsHub - Remote tracking server

### **Frontend**
- Bootstrap 5 - UI framework
- Chart.js - Data visualization
- Font Awesome - Icons
- Vanilla JavaScript - Interactions

### **DevOps**
- Uvicorn - ASGI server
- Python-dotenv - Environment management
- Dill - Model serialization

---

## 📈 Model Performance

The system trains and evaluates 5 different algorithms:

| Algorithm | Hyperparameters Tuned | Selection Metric |
|-----------|----------------------|------------------|
| Random Forest | n_estimators | F1 Score |
| Gradient Boosting | learning_rate, subsample, n_estimators | F1 Score |
| Decision Tree | criterion | F1 Score |
| Logistic Regression | default | F1 Score |
| AdaBoost | learning_rate, n_estimators | F1 Score |

**Best model is automatically selected** based on highest F1 score and saved to `final_model/`.

---

## 🔧 Configuration

### Key Settings

Edit `networksecurity/constant/training_pipeline/__init__.py`:

```python
# Target column
TARGET_COLUMN: str = "Result"

# Train/test split ratio
DATA_INGESTION_TRAIN_TEST_SPLIT_RATION: float = 0.2

# KNN Imputer parameters
DATA_TRANSFORMATION_IMPUTER_PARAMS: dict = {
    "missing_values": np.nan,
    "n_neighbors": 3,
    "weights": "uniform",
}

# Model training threshold
MODEL_TRAINER_EXPECTED_SCORE: float = 0.6
MODEL_TRAINER_MODEL_CONFIG_FILE_PATH: str = "config/model.yaml"
```

---

## 📊 MLflow Tracking

### View Experiments

**Local MLflow UI:**
```bash
mlflow ui
# Access at http://localhost:5000
```

**DagsHub Remote:**
Visit: `https://dagshub.com/priyanshujaiz/networksecurity.mlflow`

### Tracked Metrics
- F1 Score (train & test)
- Precision Score (train & test)
- Recall Score (train & test)
- Model Parameters
- Training Duration

---

## 🎯 Usage Examples

### 1. Train a Model

**Via Dashboard:**
1. Open http://localhost:8000
2. Click "Start Training" button
3. Wait for completion
4. View updated metrics

**Via CLI:**
```bash
python main.py
```

**Via API:**
```bash
curl http://localhost:8000/train
```

### 2. Make Predictions

**Via Dashboard:**
1. Go to "Make Predictions" section
2. Upload your CSV file
3. Click "Upload & Predict"
4. Download results

**Via API:**
```bash
curl -X POST http://localhost:8000/predict \
  -F "file=@test_data.csv"
```

### 3. View Experiments

**Via Dashboard:**
- Scroll to "Recent Experiments" table
- View last 10 training runs with metrics

**Via MLflow:**
```bash
mlflow ui
# or visit DagsHub link
```

---

## 🐛 Troubleshooting

### Dashboard shows empty data
**Solution**: Run training first to generate experiments
```bash
curl http://localhost:8000/train
```

### MongoDB connection error
**Solution**: Check your `.env` file and MongoDB Atlas connection string
```bash
# Verify connection
python push_data.py
```

### Static files not loading
**Solution**: Ensure `static/` folder exists in project root with `css/` and `js/` subdirectories

### Model training fails
**Solution**: Check logs in `logs/` directory for detailed error messages

---

## 🚀 Deployment

### Docker (Recommended)

```dockerfile
FROM python:3.8-slim

WORKDIR /app
COPY . /app

RUN pip install --no-cache-dir -r requirements.txt
RUN pip install -e .

EXPOSE 8000

CMD ["python", "app.py"]
```

**Build and run:**
```bash
docker build -t network-security-ml .
docker run -p 8000:8000 --env-file .env network-security-ml
```

### Cloud Platforms

- **AWS**: EC2, Elastic Beanstalk, ECS
- **GCP**: Compute Engine, App Engine, Cloud Run
- **Azure**: App Service, Container Instances
- **Heroku/Render**: Direct Git deployment

---

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 👤 Author

**Priyanshu Ranjan**
- Email: priyanshujaiz341@gmail.com
- GitHub: [@priyanshujaiz](https://github.com/priyanshujaiz)
- LinkedIn: [Connect with me](https://linkedin.com/in/yourprofile)

---

## 🙏 Acknowledgments

- MLflow for experiment tracking
- DagsHub for remote tracking server
- FastAPI for the excellent web framework
- Scikit-learn for ML algorithms
- MongoDB for cloud database
- Bootstrap & Chart.js for beautiful UI

---

## ⭐ Star this repo if you find it helpful!

**Built with ❤️ using Python, FastAPI, MLflow & MongoDB**
