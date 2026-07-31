# 🔍 SQL Murder Mystery — My Investigation

My solution walkthrough for the [SQL Murder Mystery](https://mystery.knightlab.com/), a puzzle for practicing SQL by solving a fictional murder using nothing but database queries.

Dataset source: [SQL Murder Mystery Database on Kaggle](https://www.kaggle.com/datasets/johnp47/sql-murder-mystery-database).

## 🚀 Run it yourself

No installs needed — click below to run every query live in your browser via Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/devyur/SQL-murder-mystery/blob/main/investigation.ipynb)

Or run locally:

```bash
git clone https://github.com/devyur/SQL-murder-mystery.git
cd YOUR-REPO
pip install -r requirements.txt
jupyter notebook investigation.ipynb
```

The notebook connects directly to the included `sql-murder-mystery.db` SQLite file — that file travels with the repo, so there's no database server to set up. Just run the cells top to bottom.

## 📁 Contents

| File | Description |
|---|---|
| `investigation.ipynb` | The full walkthrough: clues, SQL queries, and results, step by step |
| `sql-murder-mystery.db` | The SQLite database (self-contained, no server required) |
| `requirements.txt` | Python packages needed to run the notebook |

## 🧩 The case

A murder took place on **2018-01-15** in **SQL City**. The investigation starts from a crime scene report and follows witness statements, gym check-ins, driver's license details, and other clues across the database to identify the culprit.

## ✅ Status

_(Update this once you finish)_ — e.g. "Solved — see the final cell of the notebook for the culprit and the evidence trail."
