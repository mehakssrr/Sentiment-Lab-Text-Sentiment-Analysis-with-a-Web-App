# 🎭 Sentiment Lab — Text Sentiment Analysis with a Web App

A simple machine learning project that reads a piece of text (like a review or a comment) and tells you whether it sounds **Positive 🙂** or **Negative 🙁** — with a confidence score.

It comes with a small web app, so you don't need to touch code to try it. Type a sentence, click a button, see the result.

> Built for learning: the code is short, commented, and split into clear steps so you can follow (and change) every part of the pipeline.

---

## 🧠 What this project actually does

In plain words:

1. We collect a bunch of text examples that are already labeled **Positive** or **Negative**.
2. We clean the text (lowercase it, remove punctuation/numbers).
3. We turn the text into numbers the computer can understand, using a technique called **TF-IDF**.
4. We train a simple, fast model called **Logistic Regression** on those numbers.
5. We save the trained model, then use it inside a small **Flask web app** to predict sentiment for anything you type.

No deep learning, no GPU needed — this runs on any laptop in seconds. It's a great first "real" ML project because you can see every step.

---

## 📁 Project structure

```
sentiment-analysis-nlp/
├── data/
│   ├── generate_dataset.py     # Creates the training data (reviews.csv)
│   └── reviews.csv             # ~1,900 labeled example reviews
├── model/
│   ├── sentiment_model.pkl     # The trained model (already included)
│   └── vectorizer.pkl          # The fitted TF-IDF vectorizer (already included)
├── src/
│   ├── preprocess.py           # Text cleaning function
│   ├── train.py                # Trains the model and saves it
│   └── predict.py              # Command-line tool to test predictions
├── app/
│   ├── app.py                  # Flask web app
│   ├── templates/
│   │   └── index.html          # The web page
│   └── static/
│       └── style.css           # Styling for the web page
├── requirements.txt            # Python packages this project needs
├── .gitignore
├── LICENSE
└── README.md
```

---

## 🚀 Getting started

### 1. Clone the repo and go into the folder

```bash
git clone https://github.com/your-username/sentiment-analysis-nlp.git
cd sentiment-analysis-nlp
```

### 2. Create a virtual environment (recommended, not required)

```bash
python -m venv venv
source venv/bin/activate      # on Windows: venv\Scripts\activate
```

### 3. Install the required packages

```bash
pip install -r requirements.txt
```

### 4. (Optional) Re-train the model yourself

A trained model is already included in `model/`, so you can skip this step if you just want to try the app. But if you want to see training happen:

```bash
# Regenerate the dataset (optional — reviews.csv is already included)
python data/generate_dataset.py

# Train the model
python src/train.py
```

You'll see output like this:

```
Loading data...
Loaded 1924 rows.
Vectorizing text (TF-IDF)...
Training Logistic Regression model...
Evaluating...
Accuracy: 1.0000
...
Saved model to: model/sentiment_model.pkl
Saved vectorizer to: model/vectorizer.pkl
```

### 5. Try it from the command line

```bash
python src/predict.py "The delivery was fast and the packaging was excellent!"
```

Output:

```
Text: The delivery was fast and the packaging was excellent!
Prediction: Positive 🙂  (confidence: 81.42%)
```

### 6. Run the web app

```bash
python app/app.py
```

Then open your browser and go to:

```
http://127.0.0.1:5000
```

Type any sentence in the box, click **Analyze sentiment**, and see the result.

---

## 🖼️ How the web app looks

A clean single page: a text box, an "Analyze sentiment" button, and the result shown right below it with a confidence percentage.

---

## ⚙️ How it works under the hood (a bit more detail)

| Step | File | What happens |
|---|---|---|
| 1. Clean text | `src/preprocess.py` | Lowercases the text, strips out URLs, numbers, and punctuation |
| 2. Vectorize | `src/train.py` | Turns cleaned text into numeric features using `TfidfVectorizer` (word + 2-word combinations) |
| 3. Train | `src/train.py` | Fits a `LogisticRegression` classifier on those features |
| 4. Save | `src/train.py` | Saves the model and vectorizer with `joblib` so we don't have to retrain every time |
| 5. Predict | `src/predict.py` | Loads the saved model/vectorizer and scores new text |
| 6. Serve | `app/app.py` | A Flask route takes text from a form, calls `predict.py`, and shows the result |

**Why Logistic Regression instead of a big neural network?**
Because for a small, learning-focused project, it trains in seconds, is easy to explain, and works surprisingly well on text classification. It's the right tool for this job — you don't always need a huge model.

---

## ⚠️ Limitations & honest notes

This project uses a **synthetically generated dataset** (`data/generate_dataset.py` builds it from sentence templates) so that the whole project works offline, with no downloads and no dataset licensing issues. That means:

- The model performs extremely well (100% accuracy) on its own test set, because the sentences follow similar patterns.
- On real-world, unusual, or sarcastic sentences, it may be less confident or occasionally wrong — this is expected and normal for a small template-based dataset.

**To make this a stronger, production-style project**, swap in a real dataset such as:
- [IMDB Movie Reviews](https://ai.stanford.edu/~amaas/data/sentiment/) (50,000 labeled reviews)
- [Amazon Product Reviews](https://cseweb.ucsd.edu/~jmcauley/datasets/amazon_v2/)
- [Sentiment140 (Twitter)](http://help.sentiment140.com/for-students)

Just replace `data/reviews.csv` with a file that has `review` and `sentiment` columns, and re-run `src/train.py`. Everything else works the same.

---

## 🌱 Ideas to extend this project

Good next steps if you want to keep building:

- [ ] Swap in a real-world dataset (see above) for stronger generalization
- [ ] Add a neutral class (Positive / Negative / Neutral)
- [ ] Try a different model (Naive Bayes, SVM, or a small transformer like DistilBERT)
- [ ] Add a `/api/predict` JSON endpoint so other apps can call your model
- [ ] Deploy the Flask app for free on Render, Railway, or PythonAnywhere
- [ ] Add unit tests for `preprocess.py` and `predict.py`
- [ ] Track experiments with different models using a simple CSV log or MLflow

---

## 🛠️ Tech stack

- **Python 3.9+**
- **scikit-learn** – TF-IDF vectorizer + Logistic Regression model
- **pandas** – loading and handling the dataset
- **joblib** – saving/loading the trained model
- **Flask** – the web app
- **HTML/CSS** – simple front end (no JavaScript frameworks needed)

---

## 📄 License

This project is open source under the [MIT License](LICENSE) — free to use, modify, and share.

---

## 🙌 Contributing

Found a bug or want to add a feature? Pull requests are welcome. For big changes, please open an issue first to discuss what you'd like to change.

---

If this project helped you learn something, consider giving the repo a ⭐ — it helps others find it too.
