# 🏡 Boston House Price Prediction App

A Flask web application that predicts housing prices using a Linear Regression model trained on the Boston Housing dataset.

## 🔧 Tech Stack
- Python
- Flask
- Scikit-learn
- HTML + CSS (custom styled)
- Numpy

## 📂 Folder Structure
BostonHousingFlaskApp/
├── model.pkl              ← from trained model
├── app.py
├── templates/
│   └── index.html
├── BostonHousingModel.ipynb
├── README.md

The dataset CSV (BostonHousing.csv) is used only during model training

Project Workflow Breakdown:
Model Training (Jupyter Notebook) load BostonHousing.csv, train the LinearRegression model using scikit-learn, then save the model to model.pkl

Web App (Flask) no longer need the CSV. The Flask app loads model.pkl, and uses form input to make predictions.

# Flow of work
use BostonHousing.csv to train the model:
df = pd.read_csv("BostonHousing.csv")
# Model training...
pickle.dump(model, open("model.pkl", "wb"))

You save the trained model to model.pkl.
The Flask app uses the .pkl model:
model = pickle.load(open("model.pkl", "rb"))

# app.py (Flask backend)
from flask import Flask, render_template, request
import pickle
import numpy as np

app = Flask(__name__)
model = pickle.load(open("model.pkl", "rb"))

@app.route("/")
def home():
    return render_template("index.html")

@app.route("/predict", methods=["POST"])
def predict():
    try:
        features = [float(x) for x in request.form.values()]
        prediction = model.predict([features])[0]
        return render_template("index.html", prediction_text=f"Estimated House Price: ${prediction:.2f}")
    except:
        return render_template("index.html", prediction_text="Please enter valid inputs.")

if __name__ == "__main__":
    app.run(debug=True)

# templates/index.html (Form + CSS)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Boston House Price Prediction</title>
    <style>
        body {
            font-family: 'Segoe UI', sans-serif;
            background-color: #f0f2f5;
            display: flex;
            justify-content: center;
            padding: 30px;
        }
        .container {
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0px 4px 12px rgba(0,0,0,0.1);
            width: 600px;
        }
        h2 {
            text-align: center;
            margin-bottom: 20px;
            color: #333;
        }
        label, input {
            display: block;
            width: 100%;
            margin-bottom: 10px;
        }
        input[type="text"] {
            padding: 8px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        input[type="submit"] {
            background-color: #28a745;
            color: white;
            padding: 10px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        input[type="submit"]:hover {
            background-color: #218838;
        }
        .result {
            margin-top: 20px;
            font-size: 18px;
            color: #111;
            text-align: center;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Boston House Price Prediction</h2>
        <form action="/predict" method="post">
            {% for feature in ['CRIM','ZN','INDUS','CHAS','NOX','RM','AGE','DIS','RAD','TAX','PTRATIO','B','LSTAT'] %}
                <label for="{{ feature }}">{{ feature }}</label>
                <input type="text" name="{{ feature }}" required>
            {% endfor %}
            <input type="submit" value="Predict">
        </form>
        {% if prediction_text %}
            <p class="result">{{ prediction_text }}</p>
        {% endif %}
    </div>
</body>
</html>

# BostonHousingModel.ipynb (Training Notebook Summary)
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
import pickle

# Load dataset
df = pd.read_csv("BostonHousing.csv")
X = df.drop("price", axis=1)
y = df["price"]

# Split and train
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
model = LinearRegression()
model.fit(X_train, y_train)

# Save model
pickle.dump(model, open("model.pkl", "wb"))

# README.md (for GitHub)
This file

# Install packages:
pip install flask scikit-learn pandas numpy

# Fix 1: Install pandas numpy scikit-learn in Jupyter's environment
!pip install pandas numpy scikit-learn

# Fix 2: Add your Python environment to Jupyter
First, check which Python environment you're using in terminal:
where python 
!pip install ipykernel

# Add your environment to Jupyter:
python -m ipykernel install --user --name=boston_env --display-name "Python (Boston Housing)"

Then in your notebook, choose this kernel:
Click Kernel > Change Kernel > Python (Boston Housing)

Fix 3: VS Code-specific Issue?
If you're using VS Code Jupyter extension, follow this:

Press Ctrl+Shift+P → Type: Python: Select Interpreter
Choose the interpreter where pandas is installed.
Restart the kernel.

## 🚀 How to Run Locally

1. Clone this repo:
   ```bash
   git clone https://github.com/mmsec2016/BostonHousingFlaskApp.git
   cd BostonHousingFlaskApp

2. (Optional) Create and activate a virtual environment:
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

3. Install Packages Flask scikit-learn pandas numpy:
pip install flask scikit-learn pandas numpy

4. Run the app:
python app.py

5. Open your browser and go to:
http://127.0.0.1:5000

6. Modules Requirements:
flask
scikit-learn
numpy

1. Push to GitHub
Prerequisites:
You must have a GitHub account: https://github.com 
Download GitHub for windows and install it
Git should be installed on your system. 
To verify: git --version

1. Create a GitHub Repository
Go to https://github.com 
Login to your account
Click ➕ (top-right) → New repository
Name it: BostonHousingFlaskApp
Keep it Public or Private
Do not initialize with a README
Click Create repository
Leave that page open — you’ll need the remote URL

2. Initialize Git Locally (In VS Code or CMD)
Open your project folder (e.g., BostonHousingFlaskApp) in VS Code or Terminal, then run:
git init
git add .
git commit -m "Initial commit: Boston Housing Flask App"

3. Connect to GitHub Remote Repo
Replace YOUR_USERNAME with your GitHub username:
git remote add origin https://github.com/mmsec2016/BostonHousingFlaskApp.git

4. Push the Code
If it's a new repo:
git branch -M main
git push -u origin main

5. Add Screenshot of Localhost App
Open your Flask app in browser (http://127.0.0.1:5000)
Take a screenshot
Save it in your project folder (e.g., screenshot.png)
Add it to Git:
git add screenshot.png
git commit -m "Added localhost screenshot"
git push

