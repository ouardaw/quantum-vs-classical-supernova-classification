
Systematic Comparison of Classical vs Near-Term Quantum Machine Learning for Astronomical Classification

🌌 Why This Project Exists

From Childhood Wonder to Strategic Quantum Exploration

My fascination with the cosmos began early—sparked by Hubert Reeves’ Poussière d’Étoiles and Carl Sagan’s Cosmos. Over the years, I pursued this curiosity relentlessly: reading about quantum field theory, many-worlds interpretation, black holes, dark matter, and M-theory; watching countless space documentaries; and learning from science communicators like Neil deGrasse Tyson, Janna Levin, Sean Carroll, and Brian Cox.

A New Obsession Emerges

In recent years, I became equally fascinated by quantum computing—not as a buzzword, but as a fundamentally new computational paradigm with real (and very specific) potential. After more than 10 years in product management, I wanted to deepen my hands-on technical understanding of emerging technologies, not just their strategic implications.

I completed IBM’s Basics of Quantum Information certification, strengthened my machine-learning foundations, and decided to combine these skills with my lifelong interest in astrophysics.

The Real-World Context

Modern astronomical surveys operate at extreme scale:
	•	Zwicky Transient Facility (ZTF): ~10,000 alerts/night
	•	ALeRCE & ANTARES: production ML systems using Random Forests, gradient boosting, and deep learning
	•	Vera Rubin Observatory (LSST): expected to generate ~10 million alerts/night
	•	PLAsTiCC Challenge: created by LSST scientists to benchmark ML approaches at this scale

Classical ML already works extremely well here. This project does not attempt to outperform professional astronomy pipelines.

⸻

❓ The Research Question

Instead, I asked a more fundamental question:

Given that classical ML works well for astronomical transient classification, what would it take for quantum ML to be competitive?

This is a technology-fit evaluation, not an astronomy optimization task. I used real PLAsTiCC data as a realistic testbed to understand where near-term quantum ML helps—and where it does not.

⸻

🧪 Methodology Overview (What We Implemented Today)

I implemented parallel, consistent pipelines with leakage-safe evaluation:

Classical ML (Baselines)
	•	Logistic Regression (scaled)
	•	Random Forest (raw)
	•	CatBoost (raw)
	•	Soft-voting ensemble (LR + RF + CatBoost)

Quantum ML (Near-term QML)
	•	3-qubit variational quantum classifier (VQC)
	•	Qiskit EstimatorQNN
	•	COBYLA optimizer (gradient-free)
	•	StatevectorEstimator (V2 primitive) for noiseless simulation (no deprecation warnings)

✅ Leakage-Safe Design (Key Fix)

To avoid unintentionally “peeking” at the test set, the final pipelines follow this order:

split → select features (train only) → fit preprocessing (train only) → apply to test → train → evaluate

This applies to:
	•	correlation-based feature selection
	•	outlier clipping (p1/p99)
	•	conditional log transform
	•	scaling (standardization or angle mapping)
	•	threshold selection (done on train only for the quantum model)

⸻

📊 Results Summary (Updated)

Dataset: 1,072 PLAsTiCC transients
	•	523 Type Ia (SNIa)
	•	549 Type II (SNII)

Main Comparison (Realistic QML Constraint: 3 features / 3 qubits)

Approach	Model	Features	Accuracy	AUC	Notes
Classical	Ensemble (LR+RF+CatBoost)	16	0.744	0.853	Best overall baseline
Classical	Ensemble (LR+RF+CatBoost)	3	0.609	0.638	Apples-to-apples vs quantum
Quantum	3-qubit EstimatorQNN	3	0.591	0.602	Threshold chosen on TRAIN

Classical (16 features) breakdown
	•	Logistic Regression: Accuracy 0.712, AUC 0.770
	•	Random Forest: Accuracy 0.758, AUC 0.845
	•	CatBoost: Accuracy 0.744, AUC 0.846
	•	Ensemble: Accuracy 0.744, AUC 0.853

Quantum (3 qubits / 3 features) diagnostic metrics
	•	Accuracy: 0.591
	•	AUC: 0.602
	•	Balanced accuracy: 0.594
	•	Sensitivity (SNIa recall): 0.714
	•	Specificity (SNII recall): 0.473
	•	Decision threshold: 0.20 (selected from TRAIN to maximize balanced accuracy)

Confusion matrix (SNII, SNIa):

[[52 58]
 [30 75]]


⸻

🔍 Root Cause Insight (Today’s Main Takeaway)

The most important result wasn’t “quantum lost” or “quantum won.”

It was this:

When both classical and quantum models are restricted to the same 3 features, their performance becomes very close.
The biggest drop happens when you compress 16 features → 3 features, not when you switch classical → quantum.

	•	Classical ensemble: 0.744 → 0.609 accuracy when forced to use only 3 features
	•	Quantum QNN on the same 3 features: 0.591 accuracy

Conclusion: the dominant bottleneck is information loss from feature compression (a practical NISQ constraint), not necessarily “quantum vs classical” in isolation.

⸻

🧠 Feature Selection Used for Quantum (Train-Only)

Because 3 qubits ≈ 3 input dimensions, features were selected automatically using point-biserial correlation computed on the training split only:

Top 3 features (train-only):
	•	time_span (|corr| ≈ 0.279)
	•	decline_time (|corr| ≈ 0.266)
	•	mag_max (|corr| ≈ 0.161)

The same selected features were used for:
	•	the quantum classifier
	•	the classical Top-3 baseline (apples-to-apples)

⸻

⚛️ What This Suggests About Near-Term QML

Quantum ML is not universally superior to classical ML.
Knowing when not to use it is just as important as knowing how to implement it.

Quantum ML is most promising when:
	•	the problem naturally has a small number of high-signal features
	•	feature separation is strong (often > 0.5σ per feature)
	•	you can scale to more qubits / richer embeddings without collapsing trainability
	•	there’s structure that might benefit from quantum representations (kernels / embeddings / physics structure)

Classical ML remains optimal when:
	•	performance comes from combining many weak signals across higher dimensions
	•	ensembles and boosting can exploit feature interactions easily
	•	you want mature, interpretable, production-ready workflows

⸻

📂 Project Structure

quantum-vs-classical-supernova-classification/
├── data/
│   └── plasticc/ User-created directory │ # Place raw PLAsTiCC data her
│       
├── notebooks/
│   ├── 00_explore_plasticc.ipynb
│   ├── 01_feature_extraction.ipynb
│   ├── 02_classical_ml.ipynb
│   └── 03_quantum_classifier.ipynb
├── results/
│   
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
	5.	Run the feature extraction notebook to generate transient_features.csv

This repository complies with Kaggle’s data-usage requirements.

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
	•	PLAsTiCC Organizers — dataset creation
	•	Kaggle — hosting and infrastructure
	•	Kaggle Community — baselines and shared insights

⸻

Built with quantum curiosity, classical rigor, and thoughtful AI assistance ⚛️🔭

⸻

