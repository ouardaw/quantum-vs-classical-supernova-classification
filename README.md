# Quantum Transient Detector


## 🌌 Why This Project Exists


**From Childhood Wonder to Strategic Quantum Exploration**

My fascination with the cosmos began early—sparked by Hubert Reeves' *Poussière d'Étoiles* and Carl Sagan's *Cosmos*. Over the years, I've pursued this passion relentlessly: reading everything from quantum field theory and many-worlds interpretation to books on black holes, dark matter, and M-theory; watching countless documentaries; and learning from brilliant science communicators like Neil deGrasse Tyson, Janna Levin, Sean Carroll, and Brian Cox.

(One highlight: seeing Brian Cox's live show on black holes in Washington DC—witnessing physics presented with that kind of passion is unforgettable.)

**A New Obsession Emerges:**

In recent years, I became equally fascinated by quantum computing—not as a buzzword, but as a genuinely revolutionary technology with the potential to transform certain computational problems. After 10+ years in product management, I wanted to develop hands-on technical expertise alongside strategic understanding.

So I completed IBM's Basics of Quantum Information certification, deepened my ML knowledge, and decided to combine these new skills with my lifelong passion for astrophysics.

**The Real-World Context:**

Modern astronomical surveys generate unprecedented data volumes requiring sophisticated automated classification:

- **Zwicky Transient Facility (ZTF):** Currently produces ~10,000 transient alerts per night
- **ALeRCE & ANTARES:** Production ML systems (Random Forests, gradient boosting, deep learning) that classify these alerts in real-time
- **Vera Rubin Observatory (LSST):** Beginning operations in 2025, will generate **10 million alerts per night**
- **PLAsTiCC Challenge:** Created by LSST scientists specifically to develop ML algorithms for this scale

Astronomers have developed highly effective classical ML pipelines for these workflows. These systems work well—discovering thousands of supernovae, transient events, and astronomical phenomena.

**My Research Question:**

My goal was **NOT** to build a better astronomical classifier than existing systems—professional astronomers with domain expertise and computational resources have already solved that problem effectively.

Instead, I wanted to conduct a **systematic comparative study**: Given that classical ML works well for astronomical transient classification, what would it take for quantum ML to be competitive? What problem characteristics make quantum approaches advantageous versus where classical methods remain optimal?

This is fundamentally a **technology evaluation question**, not an astronomy optimization problem. I used real astronomical data (PLAsTiCC) as a concrete, realistic testbed to understand quantum ML's applicability.

**The Approach:**

I built rigorous implementations of both approaches:
- **Classical:** Ensemble of Logistic Regression, Random Forest, and CatBoost (similar architecture to production systems like ALeRCE)
- **Quantum:** 3-qubit variational quantum classifier using Qiskit EstimatorQNN

Both trained on identical data (1,072 PLAsTiCC supernovae) with proper evaluation methodology.

**The Finding:**

Classical ensemble methods achieved 74.4% accuracy (AUC 0.852) while quantum ML reached 52.1% accuracy (AUC 0.587)—indicating classical approaches were the more appropriate choice for this problem.

Through comprehensive feature analysis, I identified the root cause: available features showed 0.03-0.46σ class separation, below the >0.5σ threshold where quantum ML's unique properties (superposition, entanglement, quantum parallelism) can provide computational advantages.

**The Strategic Insight:**

This result isn't a limitation of quantum computing—it's a **problem-tool matching finding**. 

Quantum computing excels at specific problem types:
- Quantum simulation (molecular dynamics, materials science)
- Certain optimization problems with quantum-natural structure
- High-dimensional problems with strong feature correlations
- Problems where quantum superposition provides genuine parallelism advantages

Small astronomical classification datasets with weakly-separable features are better served by classical ensemble methods, which compensate for weak signals through high-dimensional feature combination—exactly what production systems like ALeRCE do successfully.

**The Valuable Learning:**

This project taught me something more important than any single benchmark: **how to evaluate which problems are good quantum candidates**. 

This assessment capability—knowing not just *how* to implement quantum algorithms but *when* they're the appropriate solution—is critical for organizations navigating quantum technology investments. Not every problem needs quantum computing, but the right problems can benefit tremendously.

**Looking Forward:**

My goal is to continue exploring quantum computing applications, specifically targeting problems where quantum's unique properties provide genuine computational advantages. 

Rather than forcing quantum onto problems already solved effectively by classical methods, I'm focused on identifying new problem classes where quantum computing can make transformational impact.

This project represents the convergence of:
- Childhood wonder (*Poussière d'Étoiles*, Carl Sagan, the cosmos)
- Professional product expertise (10+ years strategic PM)
- Newly acquired technical depth (quantum computing, ML)
- Scientific methodology (rigorous comparative analysis)

Sometimes the best learning comes from discovering when a powerful tool *isn't* the right fit—because then you know exactly what problems to look for where it *is*. 🌌⚛️

---

**Systematic Comparison: Classical vs Near-Term Quantum Machine Learning for Astronomical Classification**

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![Qiskit](https://img.shields.io/badge/Qiskit-1.x-purple.svg)](https://qiskit.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3+-orange.svg)](https://scikit-learn.org/)

An end-to-end, empirical comparison of classical ensemble methods and near-term quantum machine learning for binary supernova classification (Type Ia vs Type II) using real astronomical data from the PLAsTiCC challenge.

> **Key Finding:**  
> In this setting, classical models significantly outperform quantum ML (74.4% vs 52.1%) due to weak feature separability (0.03–0.46σ). This demonstrates that quantum ML requires high-quality features with strong class separation, not just adequate data volume.

---

## 📊 Results Summary

**Dataset:** 1,072 PLAsTiCC transients (523 SNIa, 549 SNII)

| Approach | Model | Features | Accuracy | AUC | Training Time |
|--------|------|----------|----------|-----|---------------|
| **Classical** | Ensemble (LR + RF + CatBoost) | 16 | **74.4%** | **85.2%** | ~2 min |
| **Quantum** | 3-qubit EstimatorQNN | 3 | **52.1%** | **58.7%** | ~10 min |
| Baseline | Random guessing | – | 50.0% | 50.0% | – |

**Classical Performance by Model**
- Random Forest: 75.8% accuracy  
- CatBoost: 74.4% accuracy  
- Logistic Regression: 71.2% accuracy  
- Ensemble: 74.4% accuracy, **85.2% AUC**

**Quantum Performance**
- Balanced accuracy: 52.9%  
- Sensitivity (SNIa recall): 85.7%  
- Specificity (SNII recall): 20.0%  
- **Interpretation:** The model learned a strong bias toward predicting SNIa rather than robust discriminative patterns.

---

## 🎯 Motivation

### Why This Project Matters

Time-domain astronomical surveys such as the Vera Rubin Observatory (LSST) will generate millions of transient alerts per night. Automated classification is essential for prioritizing follow-up observations and extracting scientific value at scale.

### The Quantum Computing Question

Quantum machine learning is frequently proposed as a solution for complex classification tasks — but **when does it actually help?**

This project provides a concrete, data-driven answer using:
- real scientific data  
- production-grade classical baselines  
- realistic NISQ-era quantum constraints  

### Objectives
1. Build a strong classical ensemble baseline  
2. Implement a comparable quantum ML pipeline  
3. Systematically evaluate performance and limitations  
4. Identify when quantum approaches are appropriate vs when classical methods are preferable  

---

## 📁 Dataset

**Source:** [PLAsTiCC Challenge](https://www.kaggle.com/c/PLAsTiCC-2018)  
(Photometric LSST Astronomical Time-Series Classification Challenge)

**Task:** Binary classification
- **Type Ia (SNIa):** Thermonuclear supernovae
- **Type II (SNII):** Core-collapse supernovae

**Data Characteristics**
- Full dataset: 7,848 objects, hundreds of millions of observations  
- Subset used: 1,072 objects (balanced)  
- ~115k multi-band photometric observations  
- Time coverage: ~750–1,100 days  
- Bands: ugrizy  
- Irregular cadence with realistic observational gaps  

---

## 🔧 Feature Engineering

**16 features extracted from raw light curves**

### Magnitude Features (5)
- `mag_min`, `mag_max`, `mag_mean`, `mag_std`, `mag_range`  
- Computed as: `mag = -2.5 × log10(flux)`

### Flux Features (3)
- `flux_max`, `flux_mean`, `flux_std`

### Temporal Features (4)
- `time_span`
- `rise_time`
- `decline_time`
- `rise_decline_ratio`

### Slope Features (3)
- `mean_rise_slope`
- `mean_decline_slope`
- `max_slope`

### Metadata
- `n_points`

**Data Quality Filters**
- Removed undetected observations (`detected = 0`)
- Filtered non-positive flux values
- Minimum 5 observations per object
- Removed edge-peaked light curves
- Outlier clipping at 1st / 99th percentiles

---

## 🤖 Classical ML Pipeline

### Models
1. **Logistic Regression**
   - L2 regularization (C=2.0)
   - StandardScaler
2. **Random Forest**
   - 600 trees, max depth 20
3. **CatBoost**
   - 800 iterations, depth 8
   - Learning rate 0.05
4. **Soft Voting Ensemble**
   - Weights: [1, 2, 3]
   - Best AUC performance

### Training
- Train/Test split: 80/20 (stratified)
- Random seed: 42
- Metrics: Accuracy, AUC, confusion matrix

---

## ⚛️ Quantum ML Pipeline

> **Note:** The quantum model is intentionally constrained to 3 qubits to reflect realistic NISQ-era limitations rather than idealized large-scale quantum systems.

### Circuit Architecture
Feature Map: ZZFeatureMap
	•	3 features
	•	2 repetitions
	•	Full entanglement

Variational Ansatz: TwoLocal
	•	3 qubits
	•	3 repetitions
	•	RY / RZ rotations
	•	CZ entanglement

Total parameters: 27
### Feature Selection
Selected via point-biserial correlation:
1. `time_span` (0.280)
2. `decline_time` (0.269)
3. `mag_max` (0.151)

### Preprocessing
- Outlier clipping
- Log transforms for high dynamic range
- Min-max scaling to [0, π] for angle encoding

### Training
- Optimizer: COBYLA
- Max iterations: 300
- Backend: Statevector Estimator (noiseless)
- Training time: ~10 minutes

---

## 🔍 Critical Finding: Feature Quality Limits Performance

| Feature | Correlation | Separation (σ) |
|-------|------------|----------------|
| time_span | 0.280 | 0.46 |
| decline_time | 0.269 | 0.44 |
| mag_max | 0.151 | 0.31 |
| rise_decline_ratio | **0.017** | **0.03** |

**Required for effective learning:** >0.5σ  
**Available:** 0.03–0.46σ

### Why Classical Outperforms Quantum
- Classical ensembles combine many weak signals across 16 dimensions
- Tree-based models form flexible non-linear boundaries
- Quantum models depend on strong individual feature signal due to qubit limits

### Key Insight
> **Quantum ML is not universally superior to classical ML.**  
> Understanding *when not to use quantum methods* is a critical practical skill.

---

## 💡 When to Use Quantum vs Classical ML

### Quantum ML is Promising When
- Large datasets (10k+ samples)
- Strong feature separation (>1σ)
- Quantum-native problem structure

### Classical ML is Preferable When
- Small to mid-sized datasets
- Weak individual features
- Well-understood domains

---

## 📂 Project Structure
quantum-transient-detector_Real_Data/
├── notebooks/
│   ├── 00_explore_plasticc.ipynb
│   ├── 01_feature_extraction.ipynb
│   ├── 02_classical_ml.ipynb
│   └── 03_quantum_classifier.ipynb
├── results/
│   ├── plasticc_classical_results.json
│   └── plasticc_quantum_results.json
├── README.md
├── requirements.txt
├── .gitignore
└── LICENSE

---

## 🚀 Setup & Installation

### Data Access (Required)

PLAsTiCC data **is not included** in this repository.

To access the dataset:
1. Create a Kaggle account
2. Accept the PLAsTiCC-2018 competition terms
3. Download the data directly from Kaggle

This repository complies fully with Kaggle’s data usage requirements.

### Installation
```bash
git clone https://github.com/yourusername/quantum-transient-detector.git
cd quantum-transient-detector
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

Run notebooks in order:
	1.	00_explore_plasticc.ipynb
	2.	01_feature_extraction.ipynb
	3.	02_classical_ml.ipynb
	4.	03_quantum_classifier.ipynb

📝 License

MIT License — see LICENSE file for details.

👤 Author

Ouarda Wilson
Senior Product Manager with hands-on experience in applied machine learning and quantum computing
	•	10+ years leading and shipping complex technical products
	•	Background in AI/ML systems, data-driven decision making, and SaaS
	•	Practical experience with quantum computing through Qiskit and quantum ML experimentation
	•	Interests: quantum-classical hybrid systems, real-world applicability of emerging technologies, scientific computing

Connect
	•	GitHub: [https://github.com/yourusername](https://github.com/ouardaw/quantum-vs-classical-supernova-classification)
	•	LinkedIn: https://www.linkedin.com/in/ouarda-jw/
## ⭐ Acknowledgments

- **IBM Quantum** — Qiskit framework, documentation, and educational resources  
- **Anthropic & OpenAI** — AI assistance used for reasoning support, code review, and documentation refinement  
- **PLAsTiCC Organizers** — Creation and curation of the astronomical dataset  
- **Kaggle** — Competition hosting and data infrastructure  
- **Kaggle Community** — Baseline solutions and shared insights  

---

*Built with quantum circuits, classical determination, and thoughtful AI assistance* ⚛️🔭

---

