# AI-Resume-Analyzer-
An end-to-end NLP web application that scores how well your resume matches a job description. Built with Flask, scikit-learn, and spaCy — features skill gap detection, keyword coverage mapping, and actionable suggestions.
## ✨ Features

| Feature | Description |
|---|---|
| 📄 **Resume Parsing** | Extracts text from PDF, TXT, and DOCX files |
| 🎯 **Match Score** | TF-IDF + cosine similarity, displayed as a percentage |
| 🧠 **Skill Extraction** | Identifies 500+ technical and soft skills |
| ⚠️ **Missing Skills** | Pinpoints skills in the JD absent from your resume |
| 💡 **Smart Suggestions** | Personalised, actionable improvement tips |
| 🔑 **Keyword Highlights** | Visual coverage map of JD keywords |
| 👥 **Multi-Resume** | Rank multiple candidates against one JD (API endpoint) |
| 📱 **Responsive UI** | Works on desktop and mobile |

---

## 🏗️ Project Structure

```
resume_analyzer/
├── app.py                    # Flask application & routes
├── requirements.txt          # Python dependencies
├── README.md
│
├── utils/
│   ├── __init__.py
│   ├── resume_parser.py      # PDF / TXT / DOCX text extraction
│   ├── text_processor.py     # NLP preprocessing pipeline
│   ├── skill_extractor.py    # 500+ skill keyword matching
│   ├── similarity_engine.py  # TF-IDF cosine similarity
│   └── suggestion_engine.py  # Improvement suggestions
│
├── templates/
│   └── index.html            # Jinja2 HTML template
│
├── static/
│   ├── css/
│   │   └── style.css         # Dark editorial UI design
│   └── js/
│       └── main.js           # Frontend logic
│
└── uploads/                  # Temporary upload folder (auto-created)
```

---

## 🚀 Quick Start

### 1. Clone / Download

```bash
git clone https://github.com/yourusername/resume-analyzer.git
cd resume_analyzer
```

### 2. Create a Virtual Environment

```bash
python -m venv venv

# Linux / macOS
source venv/bin/activate

# Windows
venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Download NLP Models (optional but recommended)

```bash
# spaCy English model for better lemmatisation
python -m spacy download en_core_web_sm

# NLTK stopwords
python -c "import nltk; nltk.download('stopwords')"
```

### 5. Run the App

```bash
python app.py
```

Open your browser at: **http://localhost:5000**

---

## 📡 API Reference

### `POST /analyze`

Analyse a single resume against a job description.

**Form Data:**

| Field | Type | Required | Description |
|---|---|---|---|
| `resume_file` | File | ✓ (or `resume_text`) | PDF, TXT, or DOCX file |
| `resume_text` | String | ✓ (or `resume_file`) | Plain-text resume |
| `job_description` | String | ✓ | Full job description text |

**Response:**

```json
{
  "success": true,
  "match_score": 72.4,
  "match_grade": { "letter": "B", "label": "Good Match", "color": "#3b82f6" },
  "matched_skills": ["python", "flask", "docker"],
  "missing_skills": ["kubernetes", "terraform"],
  "suggestions": ["Add a Docker-based project...", "..."],
  "keyword_highlights": { "python": true, "kubernetes": false },
  "stats": {
    "total_jd_skills": 12,
    "matched_count": 8,
    "missing_count": 4
  }
}
```

---

### `POST /analyze-multiple`

Rank multiple resumes against a single job description.

**Form Data:**

| Field | Type | Required | Description |
|---|---|---|---|
| `resumes` | File[] | ✓ | Multiple PDF/TXT/DOCX files |
| `job_description` | String | ✓ | Full job description text |

**Response:**

```json
{
  "success": true,
  "candidates": [
    { "filename": "alice.pdf", "match_score": 85.2, "match_grade": {...}, ... },
    { "filename": "bob.pdf",   "match_score": 62.1, "match_grade": {...}, ... }
  ]
}
```

---

## 🧪 How the Scoring Works

```
Match Score = (TF-IDF Cosine × 0.65) + (Token Overlap × 0.25) + (Length Ratio × 0.10)
```

1. **TF-IDF Vectorisation** — both texts are converted into numerical vectors using term frequency × inverse document frequency with 1–2 gram tokenisation.
2. **Cosine Similarity** — the angle between the two vectors measures semantic overlap (0 → no overlap, 1 → identical).
3. **Skill Keyword Matching** — over 500 curated skills across 12 categories are matched using regex word-boundary matching.
4. **Hybrid Score** — the TF-IDF score is blended with a token-overlap Jaccard coefficient and a length-ratio feature.

### Grade Scale

| Score | Grade | Label |
|---|---|---|
| 80–100 | A | Excellent Match |
| 65–79  | B | Good Match |
| 50–64  | C | Fair Match |
| 35–49  | D | Poor Match |
| 0–34   | F | Very Low Match |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Web Framework | Flask 2.3 |
| PDF Parsing | pdfplumber + PyPDF2 |
| DOCX Parsing | python-docx |
| NLP Preprocessing | spaCy / NLTK |
| ML / Similarity | scikit-learn (TF-IDF + cosine) |
| Frontend | Vanilla HTML/CSS/JS |
| Fonts | Syne + IBM Plex Mono |

---

## ☁️ Deployment

### Heroku

```bash
echo "web: gunicorn app:app" > Procfile
pip install gunicorn
heroku create resume-ai-app
git push heroku main
```

### Docker

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
RUN python -m spacy download en_core_web_sm
EXPOSE 5000
CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]
```

```bash
docker build -t resume-ai .
docker run -p 5000:5000 resume-ai
```

### Render / Railway

Set **Start Command** to: `gunicorn app:app`

---

## 📌 Roadmap

- [ ] Export results as PDF report
- [ ] Cover letter generator based on missing skills
- [ ] ATS (Applicant Tracking System) compatibility checker
- [ ] Integration with LinkedIn job listings
- [ ] User accounts and history

---

## 📄 License

MIT © 2024 — Free to use, modify, and distribute.

---

