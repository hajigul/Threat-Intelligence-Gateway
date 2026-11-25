# 🛡️ Threat Intelligence Gateway: Phishing Detection & XAI Engine

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Gradio](https://img.shields.io/badge/Gradio-UI-orange)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-ML-yellow)
![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)

An enterprise-grade, web-based application designed to detect phishing emails using a hybrid approach: **Machine Learning (Random Forest)** combined with a **Rule-Based Heuristic Engine**. 

Unlike standard "black-box" AI models, this tool features an **Advanced Explainable AI (XAI)** dashboard. It doesn't just tell you *if* an email is malicious—it physically highlights *why* the model made its decision, providing complete transparency to the user.

---

##  Key Features

* ** Advanced Explainable AI (XAI):** Extracts actual feature importances directly from the TF-IDF vectorizer and Random Forest model to show you which words pushed the probability toward "Phishing."
* ** Hybrid Detection Engine:** Combines the ML model's confidence score with a rule-based engine that detects urgency language, suspicious URLs (e.g., missing HTTPS, bit.ly), and requests for PII (Passwords, SSNs).
* ** Interactive Payload Inspector:** 
  * <span style="color:#ef4444; font-weight:bold;">Red Highlights</span>: Words mathematically proven by the ML model to be strong phishing indicators.
  * <span style="color:#f59e0b; font-weight:bold;">Yellow Highlights</span>: Words/URLs flagged by the heuristic rule engine.
* ** Cyber-Analyst Dark UI:** Built with Gradio and custom CSS to mimic real-world Security Operations Center (SOC) terminals. Features glowing risk meters and terminal-style inputs.
* * Real-Time Latency Tracking:** Displays inference time in milliseconds for every scan.

---

##  Quick Start (Local Setup)

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/threat-intel-gateway.git
cd threat-intel-gateway
```


## 2. Install Dependencies

Ensure you have Python 3.8+ installed. Then, install the required packages:

```pip install gradio scikit-learn pandas numpy```

3. Train the Model (Optional)

- Note: The application expects pre-trained objects (vectorizer and rf_model). If you are running this fresh, you need to train the model on your dataset first.

- Run the provided data training script (or your own Jupyter notebook) to generate the model. Ensure you have cleaned_phishing_emails.csv in the same directory.

  # Example training snippet

```
import pandas as pd
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.ensemble import RandomForestClassifier
import joblib

# Load data
df = pd.read_csv('cleaned_phishing_emails.csv')
df = df.dropna()

# Preprocess & Vectorize
vectorizer = TfidfVectorizer(stop_words='english', max_features=5000)
X_vec = vectorizer.fit_transform(df['Email Text'])
y = df['Email Type'].map({'Phishing Email': 1, 'Safe Email': 0})

# Train
rf_model = RandomForestClassifier(n_estimators=100, random_state=42)
rf_model.fit(X_vec, y)

# Save
joblib.dump(vectorizer, 'vectorizer.pkl')
joblib.dump(rf_model, 'rf_model.pkl')
```



## 4. Run the Application

Save the provided Python script as app.py. Ensure it loads the saved .pkl files at the top of the script:

```
import joblib
vectorizer = joblib.load('vectorizer.pkl')
rf_model = joblib.load('rf_model.pkl')
```

Then, launch the Gradio app:

```
python app.py
```

