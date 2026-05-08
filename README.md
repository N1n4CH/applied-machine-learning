# AI Role Archetypes — DACH Skill Clustering

This project applies unsupervised machine learning to discover natural role archetypes in the DACH AI job market (Germany, Austria and Switzerland). Rather than relying on official job titles, K-Means clustering groups job postings by their skill profiles - revealing how the market actually organises AI roles versus how employers label them. The project directly extends Projects 1 and 2: Project 1 mapped where AI jobs are, Project 2 analysed what they pay and this project asks what skills they demand.

**Dataset:** Custom collection via the Adzuna Jobs API — 485 unique job postings, enriched to 304 modeling-ready entries with skill profiles  
**Source:** Collected using `fetch_ai_jobs_and_skills.py` and enriched using `fetch_ai_jobs_and_skills_enriched.py`

---

## How to Run the Project

**1. Clone the repository**

    git clone https://github.com/n1n4ch/applied-machine-learning.git
    cd applied-machine-learning

**2. Install dependencies**

    pip install -r requirements.txt

**3. Collect the dataset** *(optional — CSVs already included)*

    python fetch_ai_jobs_and_skills.py
    python fetch_ai_jobs_and_skills_enriched.py

**4. Open and run the notebook**

    jupyter notebook modeling.ipynb

Run all cells in order via **Kernel → Restart & Run All**

---

## Project Structure

| File | Description |
|------|-------------|
| `modeling.ipynb` | Main ML notebook — preprocessing, clustering, evaluation |
| `fetch_ai_jobs_and_skills.py` | Adzuna API collection script |
| `fetch_ai_jobs_and_skills_enriched.py` | Description enrichment and skill extraction script |
| `adzuna_ai_jobs_dach.csv` | Raw job postings (485 rows) |
| `adzuna_ai_skills_dach.csv` | Raw skill extractions |
| `adzuna_ai_jobs_dach_enriched.csv` | Enriched job postings with full descriptions (485 rows, 17 columns) |
| `adzuna_ai_skills_dach_enriched.csv` | Enriched skill table (304 jobs, 1,470 skill mentions) |
| `Machine_Learning_Analysis_Report.pdf` | Written report with citations |
| `requirements.txt` | Python dependencies (generated via `pip freeze`) |

---

## Approach

**Data collection** — The Adzuna API was queried across 31 search terms spanning five role categories: AI Engineering, Data Analytics, AI Product and Project Management, AI Integration and AI Consulting. Because the API caps descriptions at ~500 characters, a secondary enrichment step fetches full job pages from source URLs and re-extracts skills, increasing mean description length from 500 to 3,402 characters.

**Skill extraction** — A 50-skill taxonomy covering programming languages, ML frameworks, cloud platforms, AI domains, and tooling practices is matched against each job description. Skills are stored as a separate CSV with one row per job-skill pair.

**Modeling** — Each job is encoded as a binary skill vector using MultiLabelBinarizer. K-Means clustering (k=5, selected via the elbow method) is applied to group postings by skill profile. The optimal k is validated using the silhouette score.

---

## Key Findings

Five skill-based role archetypes were identified:

| Cluster | Archetype | Defining Skills |
|---------|-----------|-----------------|
| 0 | Data & Analytics Generalist | agile, machine learning, python, sql, git |
| 1 | Cloud ML Engineer | azure, python, gcp, aws, docker |
| 2 | MLOps & GenAI Engineer | python, machine learning, generative ai, kubernetes, mlops |
| 3 | AI Automation & Integration | scala, rpa, python, generative ai, aws |
| 4 | Deep Learning & AI Research | machine learning, python, computer vision, deep learning, generative ai |

AI Engineering dominates three of five clusters. Consulting and PM roles do not form distinct skill-based clusters — suggesting these roles require broadly similar technical skills to engineering roles in the DACH market.

---

## Data Bias and Responsible Use

Germany accounts for 85% of postings (410 of 485), meaning results primarily reflect the German AI market rather than the full DACH region. Austria and Switzerland are underrepresented and should not be treated as equivalent in any regional comparison. The skill taxonomy was defined in English with selected German variants — skills expressed in German terminology outside the taxonomy will have been missed, potentially undercounting skill diversity in German-language postings. Cluster assignments are one possible partitioning of the data and should not be used to make hiring decisions or draw conclusions about individual employers.

---

## Requirements

Regenerate with:

    pip freeze > requirements.txt