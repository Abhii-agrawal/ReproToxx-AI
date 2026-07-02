# 🧬 ReproToxx AI

### Machine Learning Platform for Predicting Reproductive Toxicity from Chemical Structures

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10+-blue?style=for-the-badge&logo=python" />
  <img src="https://img.shields.io/badge/scikit--learn-ML-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/RDKit-Cheminformatics-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/FastAPI-Backend-009688?style=for-the-badge&logo=fastapi" />
  <img src="https://img.shields.io/badge/Next.js-Frontend-black?style=for-the-badge&logo=next.js" />
  <img src="https://img.shields.io/badge/Docker-Deploy-2496ED?style=for-the-badge&logo=docker" />
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" />
  <img src="https://img.shields.io/badge/IIT%20BHU-Research-red?style=for-the-badge" />
</p>

<p align="center">
  <strong>83.50% Internal Accuracy &nbsp;|&nbsp; 89.67% External Validation &nbsp;|&nbsp; 4,503 Compounds &nbsp;|&nbsp; 2,068 Features</strong>
</p>

---

## 📖 Project Overview

**ReproToxx AI** is an end-to-end machine learning platform developed to predict the reproductive toxicity of chemical compounds directly from their molecular SMILES strings.

Reproductive toxicity — the ability of a substance to impair fertility, sexual function, or cause developmental harm to offspring — is a critical safety concern in drug discovery and chemical regulation. Experimental testing (animal studies, in vitro assays) is expensive ($50,000–$500,000 per compound), time-consuming (6–12 months), and raises ethical concerns.

This project implements **QSAR (Quantitative Structure–Activity Relationship)** modeling using **Morgan Fingerprints + Molecular Descriptors** and **ensemble machine learning** to provide rapid, interpretable, regulatory-grade toxicity predictions from SMILES strings alone.

> **Internship Research Project — IIT BHU**  
> Dataset: DARTQSAR (Lee et al., *Nature Scientific Reports*, May 2025)

---

## 🏆 Key Results

| Validation Method | Accuracy | ROC-AUC | Details |
|:---|:---:|:---:|:---|
| **Internal Test Set** (80-20 stratified) | **83.50%** | **0.8952** | 901 held-out compounds |
| **Scaffold Split** (Murcko) | 70.98% | 0.7730 | Generalization to novel scaffolds |
| **5-Fold Cross-Validation** | 79.28% ± 1.42% | 0.8797 ± 0.0076 | Robust consistency check |
| **External Validation** (overlap-cleaned) | **89.67%** | **0.9458** | 1,414 independent compounds |

### Benchmark Comparison

| Method | Accuracy | Source |
|:---|:---:|:---|
| **ReproToxx AI (RF Improved)** | **83.50%** | This work |
| Paper GCN (Lee et al., 2025) | 81.19% | Reference |
| P&G DART | 70.10% | Regulatory tool |
| VEGA CAESAR | 59.57% | Regulatory tool |

> Our model **exceeds the published GCN paper by 2.31%** while being fully interpretable and deployable without GPU.

---

## 🗂️ Repository Structure

```
ReproToxx-AI/
│
├── 📓 ReproToxx_Final.ipynb          # Complete ML pipeline (main notebook)
│
├── 🌐 backend/                        # FastAPI prediction server
│   ├── main.py                        # API entry point
│   ├── predict.py                     # Prediction + SHAP endpoint
│   ├── response.py                    # Response schemas
│   ├── models/
│   │   └── rf_model.pkl               # Trained RF model (place here)
│   └── requirements.txt
│
├── 🖥️ frontend/                       # Next.js web application
│   ├── app/
│   │   ├── predict/page.tsx           # Single + batch prediction
│   │   ├── performance/page.tsx       # Model metrics dashboard
│   │   ├── dataset/page.tsx           # Dataset documentation
│   │   └── ...
│   └── package.json
│
├── 🐳 docker-compose.yml              # One-command deployment
├── 📊 model_metadata.pkl              # Model configuration metadata
├── 📋 README.md
├── 📄 LICENSE
└── .gitignore
```

---

## 🔬 Dataset

**DARTQSAR — Reproductive Toxicity Dataset**

| Property | Value |
|:---|:---|
| **Source Paper** | Lee et al., *Nature Scientific Reports*, May 2025 |
| **DOI** | [10.1038/s41598-025-02590-y](https://doi.org/10.1038/s41598-025-02590-y) |
| **Original Size** | 4,514 compounds |
| **After Deduplication** | **4,503 compounds** (11 duplicates removed, 2 with contradictory labels) |
| **Toxic (Positive)** | 2,463 compounds (54.7%) |
| **Non-toxic (Negative)** | 2,040 compounds (45.3%) |
| **Missing Values** | None |
| **Valid SMILES** | 100% |

**Regulatory Sources:**
- 🇪🇺 ECHA — European Chemicals Agency
- 🇺🇸 EPA CompTox — US Environmental Protection Agency  
- 🇯🇵 NITE — National Institute of Technology, Japan
- 🇦🇺 HCIS — Hazardous Chemical Information System, Australia
- 🇰🇷 NIER — National Institute of Environmental Research, Korea

**External Validation Dataset:**
- Source: *Frontiers in Toxicology*, 2025
- Original: 2,154 compounds → **1,414 after overlap removal** (739 training duplicates removed)

> ⚠️ **Data Quality Note:** 34.31% of the external set overlapped with training data (739 compounds). These were identified and removed before external evaluation to prevent data leakage.

---

## ⚙️ Feature Engineering

Each molecule is converted from SMILES to a **2,068-dimensional numerical vector**:

### Part 1: Morgan Fingerprints (2,048 bits) — ECFP4
```
Algorithm : Extended Connectivity Fingerprints
Radius    : 2  (captures 2-hop atomic neighborhood)
Bits      : 2048
Each bit  : Presence (1) or absence (0) of a chemical substructure
Also known as ECFP4 (diameter = 2 × radius = 4)
```

### Part 2: Molecular Descriptors (20 properties)

| Descriptor | What it measures |
|:---|:---|
| Molecular Weight | Molecular size |
| LogP | Lipophilicity / membrane penetration |
| TPSA | Topological Polar Surface Area |
| H-Bond Donors | Protein binding potential |
| H-Bond Acceptors | Protein binding potential |
| Rotatable Bonds | Molecular flexibility |
| Ring Count | Structural complexity |
| Aromatic Rings | Aromaticity (toxicity predictor) |
| FractionCSP3 | Aliphatic character |
| Heavy Atom Count | Molecular complexity |
| Max/Min Partial Charge | Electronic properties |
| Molar Refractivity | Polarizability |
| + 8 more | Structural and ring properties |

**Why combine both?**
- Fingerprints answer: *What structural patterns are present?*
- Descriptors answer: *How does the molecule physically behave?*
- Together: complete chemical information → better predictions

---

## 🤖 Models & Training

### Models Trained

| Model | Accuracy | ROC-AUC | Notes |
|:---|:---:|:---:|:---|
| Random Forest (Original) | 79.07% | 0.8590 | Baseline, 100 trees |
| **Random Forest (Improved)** | **83.50%** | **0.8952** | **Best model**, 500 trees, balanced |
| XGBoost | 81.28% | 0.8738 | Gradient boosting |
| LightGBM | 79.43% | 0.8774 | Fast gradient boosting |
| Gradient Boosting | 79.96% | 0.8752 | sklearn GBM |
| Ensemble (XGB+LGBM+RF) | 80.36% | 0.8855 | Soft voting |

### Best Model Configuration
```python
RandomForestClassifier(
    n_estimators   = 500,
    max_depth      = None,       # Unlimited depth
    class_weight   = 'balanced', # Handles class imbalance
    random_state   = 42,         # Reproducible
    n_jobs         = -1          # Parallel training
)
```

### Training Strategy

**1. Data Split:** Stratified 80-20 (preserves 54.7%/45.3% class ratio)
```
Training : 3,602 compounds (80%)
Test     :   901 compounds (20%)  ← never seen during training
```

**2. Scaffold Split (rigorous test):**
```
Method   : Murcko scaffold grouping
Result   : 70.98% accuracy (vs 83.50% stratified)
Gap      : -9.60% → confirms ~10% of accuracy came from 
           structural similarity in standard split
```

**3. 5-Fold Cross-Validation:**
```
Fold 1: 78.14%  |  Fold 4: 78.56%
Fold 2: 77.91%  |  Fold 5: 81.67%
Fold 3: 80.13%
Mean  : 79.28% ± 1.42%  → consistent, not luck-dependent
```

---

## 📊 Evaluation Metrics

### Internal Test Set (901 compounds)

```
Accuracy  : 83.50%   Overall correctness
Precision : 83.24%   When model says TOXIC → correct 83.24%
Recall    : 87.45%   Catches 87.45% of actual toxic compounds ★
F1-Score  : 0.8529   Harmonic mean of precision & recall
ROC-AUC   : 0.8952   Discrimination ability (excellent)
MCC       : 0.6663   Matthews Correlation Coefficient
```

**★ Recall is most critical for toxicity prediction:**  
False Negatives (missed toxics) → dangerous exposure risk  
False Positives (false alarms) → only extra testing needed

### Confusion Matrix (Internal Test)

```
                 Predicted
              Non-toxic  Toxic
Actual  Non-toxic  322     87
Actual  Toxic       62    430
```

### External Validation (1,414 compounds)

```
Accuracy  : 89.67%
Precision : 90.65%
Recall    : 92.16%
F1-Score  : 0.9140
ROC-AUC   : 0.9458
MCC       : 0.7850
```

---

## 🔍 Explainability (SHAP)

Model predictions are explained using **SHAP (SHapley Additive exPlanations)**:

```
Global Importance  → Which features matter most across all compounds?
Local Explanation  → Why was THIS specific compound predicted toxic?
Waterfall Plot     → Step-by-step feature contribution breakdown
Beeswarm Plot      → Feature direction and magnitude
```

**Top Features Identified:**
- `Morgan_1114` — specific toxic substructure pattern
- `RotatableBonds` — molecular flexibility → toxicity risk
- `HBA` — H-bond acceptors → protein binding
- `AromaticRings` — aromatic character → toxicity signal
- `MW` — molecular weight (higher → more toxic generally)

**Toxicophore Insights:**
- Phthalate patterns + ester groups → strong TOXIC signal
- Simple carboxylic acids + low MW → NON-TOXIC signal
- Halogenated aromatics → increased toxicity risk

---

## 🌐 Web Application (ReproToxx AI)

A full-stack deployment with professional UI:

```
Frontend : Next.js (localhost:3000)
Backend  : FastAPI (localhost:8000)
Deploy   : Docker Compose
```

### Features
| Page | What it does |
|:---|:---|
| **Home** | Stats overview, key metrics |
| **Predict** | Single SMILES → toxic/non-toxic + confidence + top-5 features |
| **Batch** | Upload CSV → bulk screening + summary statistics |
| **Performance** | Model comparison, ROC curves, confusion matrix |
| **Dataset** | DARTQSAR documentation and citations |
| **Model** | Architecture details |
| **Docs** | API documentation |

### Uncertainty Estimation
```
Confidence > 85%  → Reliable prediction, trust it
Confidence 70-85% → Moderate confidence
Confidence < 70%  → ⚠️ Low confidence — manual review recommended
```

---

## 🚀 Quick Start

### Option 1: Docker (Recommended — one command)

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/ReproToxx-AI.git
cd ReproToxx-AI

# Place your trained model file
cp path/to/rf_model.pkl backend/models/rf_model.pkl

# Launch everything
docker-compose up --build

# Access:
# Frontend → http://localhost:3000
# Backend  → http://localhost:8000
# API Docs → http://localhost:8000/docs
```

### Option 2: Manual Setup

**Backend (FastAPI):**
```bash
cd backend
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp path/to/rf_model.pkl models/
python main.py
# → Running at http://localhost:8000
```

**Frontend (Next.js):**
```bash
cd frontend
npm install
npm run dev
# → Running at http://localhost:3000
```

### Option 3: Jupyter Notebook (ML pipeline only)

```bash
pip install rdkit xgboost lightgbm scikit-learn shap pandas numpy \
            matplotlib seaborn openpyxl jupyter

jupyter notebook ReproToxx_Final.ipynb
# Run all cells top-to-bottom
```

---

## 📡 API Usage

**Single Prediction:**
```bash
curl -X POST http://localhost:8000/predict \
  -H "Content-Type: application/json" \
  -d '{"smiles": "CC(C)Cc1ccc(cc1)C(C)C(=O)O"}'
```

**Response:**
```json
{
  "smiles": "CC(C)Cc1ccc(cc1)C(C)C(=O)O",
  "prediction": "NON-TOXIC",
  "probability": 0.234,
  "confidence": 76.6,
  "risk_level": "Low Risk",
  "feature_contributions": {
    "MolLogP": 0.312,
    "Morgan_1114": 0.187,
    "TPSA": 0.143,
    "RotatableBonds": 0.098,
    "AromaticRings": 0.072
  }
}
```

**Batch Prediction:**
```bash
curl -X POST http://localhost:8000/predict/batch \
  -F "file=@compounds.csv"
```

---

## 📦 Requirements

### Python (ML + Backend)
```
python>=3.10
rdkit>=2023.9
scikit-learn>=1.3
xgboost>=2.0
lightgbm>=4.0
shap>=0.44
fastapi>=0.104
uvicorn>=0.24
pandas>=2.0
numpy>=1.24
matplotlib>=3.7
seaborn>=0.12
openpyxl>=3.1
```

### Node.js (Frontend)
```
node>=18.0
next>=14.0
react>=18.0
```

---

## ⚠️ Applicability Domain & Limitations

**Most reliable for:**
- ✅ Drug-like organic molecules (MW 100–600 g/mol)
- ✅ Compounds structurally similar to DARTQSAR training set
- ✅ Common pharmaceutical and industrial chemicals

**Less reliable for:**
- ⚠️ Inorganic / organometallic compounds
- ⚠️ Very large molecules (MW > 1000 g/mol, polymers)
- ⚠️ Completely novel chemical scaffolds (scaffold accuracy: 70.98%)

**Known Limitations:**
1. Structure-only prediction — cannot account for metabolism or bioavailability
2. Binary output only — does not predict toxicity severity or dose-response
3. 11 duplicates found in DARTQSAR (2 with contradictory labels) — dataset noise
4. External dataset had 34.31% overlap with training (cleaned before evaluation)
5. SHAP Morgan bit names (e.g., `Morgan_1114`) lack direct chemical nomenclature

---

## 🔮 Future Work

- [ ] Larger, more diverse training datasets (Tox21, ChEMBL integration)
- [ ] Transfer learning from general toxicity pre-trained models
- [ ] Multi-task learning (reproductive + developmental + genetic toxicity)
- [ ] Graph Neural Network implementation (matching paper's GCN architecture)
- [ ] Complexity-matched external validation dataset
- [ ] Active learning for smart experimental compound selection
- [ ] Applicability domain formalization (Tanimoto similarity threshold)
- [ ] SMARTS-based toxicophore identification from SHAP analysis

---

## 📚 References

**Primary Dataset:**
> Lee et al. "Prediction of reproductive and developmental toxicity using an attention and gate augmented graph convolutional network." *Nature Scientific Reports* 15, 18186 (May 2025).  
> DOI: [10.1038/s41598-025-02590-y](https://doi.org/10.1038/s41598-025-02590-y)

**External Validation Dataset:**
> *Frontiers in Toxicology* (2025) — Reproductive toxicity screening compounds

**Key Methods:**
> Morgan, H.L. "The generation of a unique machine description for chemical structures." *J. Chem. Doc.* 5(2), 107–113 (1965). — Morgan fingerprints

> Lundberg, S.M. & Lee, S.I. "A unified approach to interpreting model predictions." *NeurIPS* (2017). — SHAP

> Bemis, G.W. & Murcko, M.A. "The properties of known drugs." *J. Med. Chem.* 39(15), 2887–2893 (1996). — Murcko scaffold split

---

## 📄 License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.

---

## 👤 Author

**IIT BHU Research Internship Project**  
Reproductive Toxicity Prediction using QSAR & Machine Learning

---

## ⚕️ Disclaimer

> **For research and screening purposes only.**  
> ReproToxx AI is a probabilistic QSAR screening tool. It is **not** intended to replace laboratory-based toxicological assays (in vivo/in vitro), regulatory submissions (EU REACH, US EPA), or clinical decision-making. All predictions must be validated by qualified toxicologists before any regulatory or clinical use.

---

<p align="center">
  Made with 🧬 for safer chemical discovery
</p>