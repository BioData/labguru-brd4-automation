# 🚀 Predict BRD4 Binding Probabilities Using Labguru Workflows
## 🧠 Goal
Leverage Labguru’s **Scripter Docker** feature to automate the prediction of **BRD4 protein binding probabilities** for compounds using a pretrained machine learning model.

## 🧪 Training Script Overview
This project includes a basic training script that demonstrates how to train a model to predict protein binding probabilities. Although it supports multiple proteins, we’ll use a pretrained model to predict only for BRD4.

You can find the script in this repository.

## 🧩 Setup Guide

### 1. Create a Custom Field in Labguru

1. Go to **Inventory → Compounds**.
2. Click the three-dot menu (top-right) and select **Customize**.
3. Scroll to **Custom Fields** → click **Add Custom Field**.
4. Name it: `BRD4 Binding Probability`.
5. Set **Field Type** to: `Only Numbers`.
6. Click **Save**.

---

### 2. Create a Workflow in Labguru

1. Click your lab name (top-right) → select **Workflows**.
2. Click **+ New** to create a new workflow.
3. Give your workflow a title and set the **trigger** to `Compound`.
4. Click **Create**.
5. Once the workflow loads, click the **down arrow** in the bottom-right and select **Scripter**.
6. In the script editor popup, paste the contents of `scripter.py` from this repo.
7. Click **Save** in the Scripter window.

---


### 3. Build Your Custom Docker Environment
Now we’ll build a Docker environment that can run Python, load packages, and use the pretrained model.

#### 🧱 Local Setup Instructions

Step 1: Open a terminal and Create project directory
```bash
mkdir bring_your_own_docker
cd bring_your_own_docker
```


Step 2: Clone the basic worker template
```bash
git clone https://github.com/BioData/basic-worker
cd basic-worker
```

Step 3: Reset Git history
```bash
rm -rf .git
git init
```

Step 4: Create a Remote GitHub Repo
* Go to: https://github.com/new
* Name your repo (e.g., `basic-worker`)
* Click Create Repository

Step 5: Add your pretrained model files to this directory
 * `xgb_model.pkl`
 * `onehot_encoder.pkl`

Step 6: Update `requirements1.txt` with these dependencies
```bash
requests==2.31.0
requests-unixsocket==0.3.0
requests-toolbelt
rdkit
duckdb
xgboost
scikit-learn
pandas
numpy
joblib
```

Step 7: Save and Commit
```bash
git add .
git commit -m "Initial commit - added pretrained model and dependencies"
```

Step 8: Connect your GitHub remote
```bash
git remote add origin https://github.com/YOUR_USERNAME/basic-worker.git
git push -u origin main
```
Replace `YOUR_USERNAME` with your GitHub username.

Step 9: Tag the build and push
```bash
git tag -a basic-worker-test-$(date +%Y%m%d) -m "basic-worker test $(date +%Y%m%d)" -f
git push -f --tags
```

---

### 4.  Monitor Docker Build in Labguru

1. In Labguru, go to the **GitHub** tab (left menu).
2. Click the **tag** you just pushed (e.g., `basic-worker-test-YEARMONTHDAY`).
3. Watch the logs — once you see `end of build`, your Docker image is ready.

---

### 5.  Attach Docker Image to Your Workflow

1. Go to **Workflows → All Workflows**.
2. Click **Edit** on your workflow.
3. Scroll to the bottom of the Scripter block.
4. Select your Docker build from the **Custom Scripter** dropdown.
5. Click **Save**.

## ✅ Done!
From now on, every time a compound is added, the automation will:
* Run the prediction script.
* Calculate the BRD4 binding probability.
* Update the custom field BRD4 Binding Probability.