# 🌐 Suicidal Social Networks: Content Analysis & Detection

## 🎯 Project Purpose
**Suicidal Social Networks** is an AI-powered web platform designed to detect and analyze suicidal ideation in social media content. The primary goal is to provide a tool for researchers and mental health professionals to identify potentially at-risk individuals by analyzing the linguistic patterns in their posts (tweets, Reddit messages, etc.).

By leveraging **Deep Learning (CNN, LSTM)** and **Natural Language Processing (NLP)**, the system classifies text into "Suicidal" or "Non-suicidal" categories with high accuracy.

---

## 🛠 Technology Stack

### 💾 Core Language & Frameworks
*   **Language**: **Python 3.10** (The industry standard for ML and Data Science).
*   **Backend Framework**: **Django 5.2** (A high-level Python web framework that encourages rapid development and clean, pragmatic design).
*   **Database**: **SQLite3** (Lightweight, file-based database used for storing user profiles and analysis results).

### 🧠 Machine Learning & NLP Libraries
*   **TensorFlow & Keras**: Used for building and running the **Convolutional Neural Networks (CNN)** and **Long Short-Term Memory (LSTM)** models.
*   **Scikit-Learn**: Used for the **SGDClassifier** (Stochastic Gradient Descent) and text vectorization (**HashingVectorizer**).
*   **NLTK (Natural Language Toolkit)**: Handles text preprocessing like tokenization, stemming, and stopword removal.
*   **Pandas & NumPy**: For data manipulation and numerical computations.
*   **PRAW (Python Reddit API Wrapper)**: Used to fetch live data from Reddit for analysis.

### 🎨 Frontend & UI
*   **HTML5 / CSS3**: Structured with semantic tags and custom styling.
*   **Bootstrap 4**: For a responsive, "mobile-first" design.
*   **JavaScript / jQuery**: For interactive elements and animations.

---

## 🏗 Project Architecture & Components

### 1. Backend (Django Apps)
The project is split into two main Django applications:

*   **`users` App**: Handles the core logic for individual users.
    *   **Registration/Login**: Secure authentication for users.
    *   **Analysis Interface**: Where users input text or upload files for prediction.
    *   **History**: Stores and displays past analysis results using the `TweetResultModel`.
*   **`admins` App**: Provides administrative oversight.
    *   **User Management**: Admins can activate or deactivate accounts.
    *   **Global Monitoring**: View all analysis results across the entire platform.
    *   **Model Training**: Triggers the training of the CNN models from the web interface.

### 2. Machine Learning Utilities (`users/utility/`)
This is the "brain" of the project:

*   **`GetTweetTypes.py` (ProcesAndDetect class)**:
    *   **Purpose**: Fast, real-time prediction for individual tweets.
    *   **Logic**: Uses an **SGDClassifier** with a **HashingVectorizer**. It performs on-the-fly preprocessing (removing HTML, emoticons, stemming) and partial fitting to provide instant results.
*   **`SuicideAnalysisCnnModels.py` (MainCNNModel class)**:
    *   **Purpose**: Heavyweight, deep semantic analysis.
    *   **Logic**: Implements a hybrid **CNN + LSTM** architecture.
        *   **CNN** layer extracts local features (patterns of words).
        *   **LSTM** layer captures long-term dependencies (the "context" of the sentence).
    *   **Embeddings**: Uses pre-trained **GloVe** (Global Vectors for Word Representation) to understand word meanings.
*   **`BuildCNNMyModels.py`**:
    *   **Purpose**: A demonstration model built using the IMDB dataset to show the general sentiment analysis capabilities of CNNs.

---

## 💻 Code Explanation: How It Works

### The Analysis Pipeline
When you submit a text for analysis, the following happens:

1.  **Preprocessing**: The raw text is cleaned using Regex to remove HTML tags and special characters.
2.  **Tokenization**: The text is broken down into individual words (tokens).
3.  **Stemming**: Words are reduced to their root form (e.g., "running" → "run") using the **PorterStemmer**.
4.  **Vectorization**: The words are converted into numerical data. The CNN model uses **Embeddings**, while the faster model uses a **HashingVectorizer**.
5.  **Prediction**: The processed numbers are fed into the trained model, which outputs a probability.
6.  **Storage**: The result (Text, Prediction, Accuracy) is saved to the SQLite database.

---

## 📂 Database Schema
We use two main tables in **SQLite**:

1.  **`UserRegistrationModel`**: Stores user details (Name, Email, Password, Status).
2.  **`TweetResultModel`**: Stores the output of every analysis (Username, Tweet Text, Prediction, Accuracy Score, Date).

---

## 🚀 How to Run the Project

1.  **Activate Environment**: 
    ```bash
    .\venv310\Scripts\activate
    ```
2.  **Install Dependencies**:
    ```bash
    pip install -r requirements.txt
    ```
3.  **Start Server**:
    ```bash
    python manage.py runserver
    ```
4.  **Access App**: Open `http://127.0.0.1:8000/` in your browser.

---

## ⚠️ Important Note on Recent Fixes
The project has been updated to be compatible with **TensorFlow 2.12**. Old import paths (like `keras.layers.embeddings`) have been updated to modern standards (`keras.layers.Embedding`) to ensure the CNN models run correctly.
