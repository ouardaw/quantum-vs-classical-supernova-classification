

Systematic Comparison of Classical vs Near-Term Quantum Machine Learning for Astronomical Classification

⸻

🌌 Why This Project Exists

From Childhood Wonder to Strategic Quantum Exploration

My fascination with the cosmos began early—sparked by Hubert Reeves’ Poussière d’Étoiles and Carl Sagan’s Cosmos. Over the years, I pursued this curiosity relentlessly: reading about quantum field theory, many-worlds interpretation, black holes, dark matter, and M-theory; watching countless space documentaries; and learning from science communicators like Neil deGrasse Tyson, Janna Levin, Sean Carroll, and Brian Cox.

A New Obsession Emerges

In recent years, I became equally fascinated by quantum computing—not as a buzzword, but as a fundamentally new computational paradigm with real (and very specific) potential. After more than 10 years in product management, I wanted to deepen my hands-on technical understanding of emerging technologies, not just their strategic implications.

I completed IBM’s Basics of Quantum Information certification, strengthened my machine-learning foundations, and decided to combine these skills with my lifelong interest in astrophysics.

The Real-World Context

Modern astronomical surveys operate at extreme scale:
	•	Zwicky Transient Facility (ZTF): ~10,000 alerts per night
	•	ALeRCE & ANTARES: Production ML systems using Random Forests, gradient boosting, and deep learning
	•	Vera Rubin Observatory (LSST): Expected to generate ~10 million alerts per night
	•	PLAsTiCC Challenge: Created by LSST scientists to benchmark ML approaches at this scale

Classical ML already works extremely well here.
This project does not attempt to outperform professional astronomy pipelines.

⸻

❓ The Research Question

Instead, I asked a more fundamental question:

Given that classical ML works well for astronomical transient classification, what would it take for quantum ML to be competitive?

This is a technology-fit evaluation, not an astronomy optimization task.
I used real PLAsTiCC data as a realistic testbed to understand where near-term quantum ML helps — and where it does not.

⸻

🧪 Methodology Overview

I implemented parallel, production-quality pipelines:
	•	Classical ML
	•	Logistic Regression
	•	Random Forest
	•	CatBoost
	•	Soft-voting ensemble
	•	Quantum ML
	•	3-qubit variational quantum classifier
	•	Qiskit EstimatorQNN
	•	COBYLA optimizer

Both approaches were trained and evaluated on the same dataset, using consistent splits and metrics.

⸻

📊 Results Summary (Updated)

Dataset: 1,072 PLAsTiCC transients
	•	523 Type Ia (SNIa)
	•	549 Type II (SNII)

Approach	Model	Features	Accuracy	AUC	Training Time
Classical	Ensemble (LR + RF + CatBoost)	16	74.4%	0.852	~2 min
Quantum	3-qubit EstimatorQNN	3	50.2%	0.576	~10–11 min
Baseline	Random guessing	–	50.0%	0.500	–

Classical Performance
	•	Random Forest: 75.8% accuracy
	•	CatBoost: 74.4% accuracy
	•	Logistic Regression: 71.2% accuracy
	•	Ensemble: 74.4% accuracy, best AUC

Quantum Performance
	•	Accuracy: 50.2%
	•	AUC: 0.576
	•	Sensitivity (SNIa recall): 87.6%
	•	Specificity (SNII recall): 14.5%
	•	Balanced accuracy: 51.1%

Interpretation:
The quantum model learned a strong bias toward predicting SNIa rather than a balanced discriminative boundary — a clear symptom of weak feature separability under tight qubit constraints.

⸻

🔬 Experimental Iteration & Model Diagnostics

<details>
<summary><b>Systematic experimentation, diagnostics, and limits analysis (click to expand)</b></summary>


This project evolved through controlled, hypothesis-driven iterations to understand how near-term quantum ML behaves on real scientific data.

Initial Baseline (600 samples)
	•	Classical: ~75.0% accuracy
	•	Quantum: ~47.5% accuracy
	•	Gap: −27.5 pp

Improvements Applied
	•	Dataset scaling: 600 → 1,072 samples
	•	Auto-selection of top 3 features via correlation analysis
	•	Outlier-robust preprocessing (clipping + log transforms)
	•	Quantum-specific scaling to [0, \pi]
	•	Deeper circuit and more training iterations

Final Outcome (1,072 samples)
	•	Quantum accuracy improved slightly to ~50.2%
	•	Classical performance remained stable
	•	Performance plateaued despite tuning

Why Performance Plateaued

Feature analysis revealed a fundamental data limitation:

Feature	Separation (σ)
Best features	0.31–0.46σ
Typical requirement for quantum ML	>0.5σ

No amount of circuit depth or optimizer tuning can compensate for insufficient feature separability.

Key takeaway:

Quantum ML performance is primarily constrained by feature quality, not model complexity or dataset size alone.

</details>



⸻

🔍 Root Cause Analysis: Feature Quality

Feature	Correlation	Separation (σ)
time_span	0.280	0.46
decline_time	0.269	0.44
mag_max	0.151	0.31
rise_decline_ratio	0.017	0.03

Effective quantum learning typically requires >0.5σ separation per encoded feature.
This dataset provides 0.03–0.46σ, explaining the observed performance ceiling.

⸻

⚛️ Why Classical Outperformed Quantum Here

Classical ensemble methods succeed because they:
	•	Combine many weak signals across 16 dimensions
	•	Learn flexible, non-linear decision boundaries
	•	Compensate for poor individual features via ensemble voting

Quantum ML struggled because it:
	•	Is constrained to 3 features (3 qubits)
	•	Requires strong individual feature signal
	•	Suffers from weak gradients when features overlap

This is not a failure of quantum computing — it is a problem–tool mismatch.

⸻

💡 Strategic Insight

Quantum ML is not universally superior to classical ML.
Knowing when not to use it is just as important as knowing how to implement it.

Quantum ML is most promising when:
	•	Feature separation is strong (>1σ)
	•	Datasets are large (10k+ samples)
	•	Problem structure is quantum-native

Classical ML remains optimal when:
	•	Datasets are small to mid-sized
	•	Features are weakly separable
	•	The domain is mature and well-understood

⸻

📂 Project Structure

quantum-transient-detector_Real_Data/
├── data/
│   └── plasticc/                 # User-created directory
│                                # Place raw PLAsTiCC data here
│                                # Engineered features are also saved here
├── notebooks/
│   ├── 00_explore_plasticc.ipynb
│   ├── 01_feature_extraction.ipynb
│   ├── 02_classical_ml.ipynb
│   └── 03_quantum_classifier.ipynb
├── results/
│   ├── plasticc_classical_results.json
│   └── plasticc_quantum_results_final.json
├── README.md
├── requirements.txt
└── LICENSE


⸻

🚀 Setup & Data Access

PLAsTiCC data is not included.

To run the notebooks:
	1.	Create a Kaggle account
	2.	Accept the PLAsTiCC-2018 competition terms
	3.	Download the dataset from Kaggle
	4.	Create data/plasticc/ and place the files there

This repository fully complies with Kaggle’s data-usage requirements.

⸻

👤 Author

Ouarda Wilson
Senior Product Manager with hands-on experience in applied ML and quantum computing
	•	10+ years delivering complex technical products
	•	Background in AI/ML systems and data-driven decision making
	•	Practical quantum computing experience with Qiskit
	•	Interests: quantum-classical hybrids, real-world applicability of emerging tech, scientific computing

🔗 GitHub: https://github.com/ouardaw/quantum-vs-classical-supernova-classification
🔗 LinkedIn: https://www.linkedin.com/in/ouarda-jw/

⸻

⭐ Acknowledgments
	•	IBM Quantum — Qiskit framework and educational resources
	•	Anthropic & OpenAI — AI assistance for reasoning support, code review, and documentation
	•	PLAsTiCC Organizers — Dataset creation
	•	Kaggle — Hosting and infrastructure
	•	Kaggle Community — Baselines and shared insights

⸻

Built with quantum curiosity, classical rigor, and thoughtful AI assistance ⚛️🔭

⸻
