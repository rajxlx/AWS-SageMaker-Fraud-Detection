# 📊 Credit Card Fraud Detection with AWS & XGBoost

An end-to-end Machine Learning project focused on identifying fraudulent financial transactions. This repository demonstrates how to build an optimized classification pipeline, bypass AWS cloud limits using Python, and solve severe data imbalance to protect business assets.

---

## 🛠️ Tech Stack & Libraries (Beginner-Friendly Guide)

Here is a breakdown of the infrastructure and tools used to build this project, explained in plain English:

### ☁️ Cloud Infrastructure (The Workspace)
*   **AWS SageMaker:** *A super-powerful computer in the cloud.* Instead of slowing down a personal laptop to train heavy AI models, this project used SageMaker's remote cloud compute space.
*   **AWS S3 (Simple Storage Service):** *A massive, digital cloud filing cabinet.* Corporate datasets are too big for a desktop. The raw transaction data was stored safely here, allowing the code to pull it in automatically.

### 🐍 Python Libraries (The Tools)
*   **XGBoost (`xgboost`):** *The brain of the project.* It stands for *eXtreme Gradient Boosting*. Imagine 100 detectives solving a crime: the second detective fixes the first one's mistakes, the third fixes the second's, and so on. It is the industry standard for spotting complex patterns in rows and columns.
*   **Boto3 (`boto3`):** *The remote control for AWS.* Python cannot talk to Amazon's servers natively. `boto3` was used as a bridge code to check our cloud account logs when our servers got stuck.
*   **Scikit-Learn (`sklearn`):** *The AI grading scale.* This library provided the mathematical report cards (`classification_report` and `confusion_matrix`) used to measure the model's performance.
*   **Pandas & NumPy:** *The Excel of Python.* `pandas` organizes messy data into clean rows and columns, while `numpy` handles the heavy math in the background.
*   **Pickle (`pickle`):** *The freezer for the AI.* When a cloud computer is turned off, its memory wipes. `pickle` was used to freeze the trained model weights into a permanent file (`model.pkl`) so it can be reused forever for free.

---

## 📈 System Metrics & Business Optimization

The baseline model was heavily biased due to a severe **Class Imbalance** ($95\%$ legitimate transactions vs. only $5\%$ fraud). By tweaking the algorithm's internal settings, the model's real-world business value was completely transformed:

| Metric | Baseline XGBoost Model | Balanced XGBoost Model (Optimized) |
| :--- | :--- | :--- |
| **Overall Accuracy** | 95% | 92% |
| **Fraud Recall (Detection Rate)** | 0.05 *(Missed 95% of fraud)* | **1.00 (Caught 100% of fraud)** |
| **Fraud Precision (Alert Certainty)** | 1.00 | 0.38 |

### 💡 Core Business Takeaway
While overall accuracy dropped by 3%, **Fraud Recall jumped from 5% to 100%**. In banking operations, the cost of a false alarm (asking an innocent customer to verify a text) is significantly cheaper than a false negative (letting a criminal steal thousands of dollars undetected). Minimizing false negatives was the top business priority.

---

## 🎯 Interview Q&A Cheat Sheet

These are the exact technical scenarios and questions an interviewer might ask about this repository:

### 💬 Q1: Your training run hit an AWS Service Quota limit. How did you troubleshoot and bypass that bottleneck?
*   **Plain English Answer:** "When I tried to spin up a heavy `ml.m5.xlarge` server inside SageMaker, AWS blocked it because my account budget limit for that server was set to zero. Instead of stopping the project to wait days for AWS support, I used `boto3` to inspect my cloud settings. I built a smart workaround: I streamed our data directly out of our remote Amazon S3 storage bucket and ran the XGBoost training loop inside my local container memory instead. This bypassed the server block completely with zero extra costs."

### 💬 Q2: The baseline model achieved 95% accuracy. Why wasn't that good enough to launch?
*   **Plain English Answer:** "The dataset was completely uneven: 4,744 transactions were clean and only 256 were fraud. Because of this, the model realized it could cheat the system. By simply guessing 'Legitimate' for every single transaction, it scored a deceptive 95% accuracy on paper. However, looking deeper at the classification report revealed a Fraud Recall of 0.05—meaning it missed 95% of the thieves. Relying on accuracy alone here would have caused massive financial losses."

### 💬 Q3: How did you fix that class imbalance issue inside the algorithm?
*   **Plain English Answer:** "I used an XGBoost setting called `scale_pos_weight`. I calculated the exact math ratio between our clean data and fraud data ($4744 / 256 \approx 18.5$). By passing `18.5` into the model, I essentially told the algorithm: *'Every time you miss a single fraud case, punish yourself 18.5 times harder than missing a normal transaction.'* This forced the AI to focus on the minority fraud signals, bringing our final Fraud Recall up to a perfect 1.00."

### 💬 Q4: How did you wrap up and finalize this project?
*   **Plain English Answer:** "Once the model was catching 100% of the fraud, I used the `pickle` library to save its trained weights into a permanent `model.pkl` file on my desktop. With the brain of the project safe, I went back into the AWS console and deleted the SageMaker domain resources. This completely stops all hourly cloud charges while leaving the raw data safe inside S3 storage."
