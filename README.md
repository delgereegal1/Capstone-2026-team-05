# CHEERS

### Biomedical Knowledge Graph-Based Drug–Drug Interaction Prediction and Exploration Platform

**Pusan National University · Computer Science Capstone Design 2026**

CHEERS is a biomedical knowledge graph-based platform that combines **PrimeKG, Relational Graph Convolutional Networks (R-GCN), and web-based biomedical exploration tools** to support drug–drug interaction (DDI) prediction and knowledge graph exploration.

The project investigates how the composition of biomedical knowledge graphs affects DDI link prediction performance and provides an interactive web application for exploring drugs, diseases, relationships, and predicted drug–drug interactions.

> **Research and educational use only.**
> CHEERS is not a medical diagnosis, treatment recommendation, or clinical decision-support system.

---

## 1. Project Background

Drug–drug interactions (DDIs) can occur when two or more drugs influence each other's effects. Identifying potential DDIs is important for biomedical research and medication safety.

Biomedical knowledge graphs provide a way to represent complex relationships among drugs, diseases, genes, proteins, pathways, and other biomedical entities. However, the information available in a knowledge graph depends on the relationships and sources included in the graph.

This project focuses on the following research question:

> **How does biomedical knowledge graph composition affect R-GCN-based drug–drug interaction prediction?**

To investigate this question, we constructed multiple graph variants from **PrimeKG** and evaluated the same R-GCN architecture under controlled experimental conditions.

The project was subsequently extended into **CHEERS**, a web-based platform that allows users to explore biomedical relationships and interact with DDI prediction results.

---

## 2. Development Goals

The main goals of CHEERS are:

1. **Develop an R-GCN-based DDI prediction model**

   * Represent biomedical entities and relationships as a heterogeneous graph.
   * Predict potential drug–drug links using relational graph representation learning.

2. **Analyze the effect of graph composition**

   * Construct four graph variants: **G0, G1, G2, and G3**.
   * Keep the DDI split, model architecture, and evaluation procedure consistent.
   * Compare prediction performance across different graph compositions.

3. **Evaluate model stability**

   * Conduct multi-seed experiments.
   * Analyze ranking-based DDI prediction performance.
   * Perform relation-level ablation experiments.

4. **Develop an interactive biomedical exploration platform**

   * Explore drugs, diseases, and biomedical relationships.
   * Check known and predicted DDIs.
   * Visualize graph structures and subgraphs.
   * Provide evidence-grounded explanations for biomedical queries.

5. **Provide an accessible demonstration system**

   * Integrate the trained model with a web application.
   * Provide interactive biomedical exploration and DDI prediction features.

---

## 3. System Design

### 3.1 Overall Architecture

CHEERS consists of four major components:

```text
PrimeKG
   │
   ▼
Data Preprocessing
   │
   ├── Entity & Relation Processing
   ├── DDI Canonicalization
   ├── Train / Validation / Test Split
   └── Graph Construction
            │
            ▼
      R-GCN Model
            │
            ├── G0
            ├── G1
            ├── G2
            └── G3
            │
            ▼
      DDI Prediction
            │
            ▼
      CHEERS Platform
            │
            ├── DDI Checking
            ├── DDI Predictor
            ├── Graph Explorer
            ├── Subgraph Explorer
            ├── Medicine / Disease Exploration
            ├── My Health
            └── Ask CHEERS
```

### 3.2 Knowledge Graph

The project uses **PrimeKG** as the primary biomedical knowledge graph.

The graph contains heterogeneous biomedical entities and relationships, including:

* Drugs
* Diseases
* Genes
* Proteins
* Pathways
* Biological processes
* Molecular functions
* Other biomedical entities

The target prediction task is **drug–drug interaction link prediction**.

---

### 3.3 R-GCN Model

We use a **Relational Graph Convolutional Network (R-GCN)** to learn representations from the heterogeneous biomedical graph.

The final model uses:

* 2 R-GCN layers
* 128-dimensional node embeddings
* 128-dimensional hidden representation
* Dropout: 0.2
* Learning rate: 0.001
* Weight decay: 1e-5
* Maximum epochs: 500
* Early stopping based on validation performance

The same model architecture and evaluation protocol are used across the graph variants to enable controlled comparison.

---

### 3.4 Graph Composition Experiment

Four graph variants were constructed:

| Variant | Description                                        |
| ------- | -------------------------------------------------- |
| **G0**  | DDI-only graph                                     |
| **G1**  | DDI + selected biomedical relations                |
| **G2**  | DDI + another selected set of biomedical relations |
| **G3**  | DDI + broader biomedical relation composition      |

The purpose of the experiment is to investigate whether additional biomedical context contributes to DDI prediction.

---

### 3.5 Evaluation

The primary evaluation uses filtered ranking metrics:

* Mean Reciprocal Rank (MRR)
* Hits@1
* Hits@5
* Hits@10

The main five-seed MRR results are:

| Graph |                     MRR |
| ----- | ----------------------: |
| G0    |     0.527284 ± 0.006373 |
| G1    |     0.530969 ± 0.007414 |
| G2    |     0.526776 ± 0.007482 |
| G3    | **0.534209 ± 0.006288** |

Additional relation-level ablation experiments were conducted to examine the contribution of individual relation groups.

Detailed experimental procedures and results are available in [`README_research.md`](README_research.md).

---

## 4. Development Results

### 4.1 DDI Prediction

CHEERS successfully implements an end-to-end DDI prediction pipeline:

```text
PrimeKG
   ↓
Preprocessing
   ↓
Graph Construction
   ↓
R-GCN Training
   ↓
DDI Link Prediction
   ↓
Ranking Evaluation
```

The controlled experiments demonstrate measurable differences between graph compositions while keeping the prediction architecture and evaluation protocol consistent.

---

### 4.2 CHEERS Web Platform

The trained model and biomedical knowledge graph are integrated into a web-based platform.

Major features include:

#### DDI Checking

Users can explore drug pairs and examine available interaction information.

#### DDI Predictor

The system provides model-based ranking scores for potential drug–drug interactions.

> The model score is a ranking value. It should not be interpreted as a probability, clinical risk score, severity score, or confirmation that an interaction is clinically safe or dangerous.

#### Medicine & Disease Exploration

Users can search and explore biomedical entities and their relationships within the knowledge graph.

#### Graph Explorer

Provides an interactive view of biomedical entities and their relationships.

#### Subgraph Explorer

Allows users to examine local graph structures surrounding selected biomedical entities.

#### Ask CHEERS

Provides a natural-language interface for biomedical questions.

The system combines retrieved biomedical information with **grounded Gemini-based explanations** to make graph-based information easier to understand.

#### My Health

Provides a user-oriented interface for organizing and exploring relevant health-related information within the application.

---

### 4.3 External Evidence

CHEERS also integrates external biomedical sources to provide supporting information.

The platform uses:

* **openFDA** for drug-related regulatory information
* **PubMed** for biomedical literature
* PrimeKG for biomedical knowledge graph relationships
* Grounded Gemini explanations for natural-language interpretation

External evidence is presented as supporting information and does not replace professional medical judgment.

---

### 4.4 Deployment

The CHEERS web platform consists of:

* **Frontend:** React / Vite
* **Backend:** FastAPI
* **Model inference:** lightweight NumPy-based runtime
* **Deployment:** Vercel / Render

**Live Demo:** [ADD LIVE DEMO URL]

---

## 5. Installation and Execution

### 5.1 Requirements

Recommended environment:

* Python 3.12+
* Node.js
* npm
* Git

For model training, a separate GPU environment may be required.

---

### 5.2 Backend

```bash
cd api

python -m venv .venv
source .venv/bin/activate
```

For Windows:

```bash
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Run the API server:

```bash
uvicorn app:app --reload
```

The API will be available at:

```text
http://127.0.0.1:8000
```

---

### 5.3 Frontend

```bash
cd frontend
npm install
npm run dev
```

The development server will be available at the URL shown by Vite.

---

### 5.4 Training Environment

The original training environment used:

* Python 3.10.20
* PyTorch 2.2.1 + CUDA 12.1
* PyTorch Geometric 2.5.3
* NVIDIA RTX 2080 Ti GPUs

Training artifacts and detailed experimental procedures are documented in:

[`README_research.md`](README_research.md)

---

## 6. Introduction Materials and Demonstration Video

### 6.1 Project Introduction

**Project:** CHEERS
**Team:** CHEERS
**Institution:** Pusan National University
**Department:** Computer Science

CHEERS integrates biomedical knowledge graph analysis, graph neural networks, and an interactive web application to explore and predict drug–drug interactions.

### 6.2 Demonstration Video

**YouTube:** [ADD PNU CSE YOUTUBE VIDEO URL]

The demonstration video introduces the CHEERS platform and shows the main features, including:

* DDI checking
* DDI prediction
* Medicine exploration
* Disease exploration
* Graph Explorer
* Subgraph Explorer
* Ask CHEERS
* My Health

---

## 7. Team

### CHEERS

| Member     | Role                    |
| ---------- | ----------------------- |
| **[Name]** | Project Leader / [Role] |
| **[Name]** | [Role]                  |
| **[Name]** | [Role]                  |
| **[Name]** | [Role]                  |
| **[Name]** | [Role]                  |

### Responsibilities

The team collaborated on:

* Project planning
* Biomedical data preprocessing
* Knowledge graph construction
* R-GCN model development
* Experimental evaluation
* Web application development
* API development
* UI/UX design
* Deployment
* Documentation and presentation

---

## 8. References and Sources

### Biomedical Knowledge Graph

* PrimeKG
  Chandak, P. et al. *PrimeKG: a multi-modal knowledge graph for precision medicine.*

### Drug–Drug Interaction Information

* PrimeKG biomedical relationships
* DDI-related relationships included in the processed PrimeKG graph

### Graph Neural Network

* Schlichtkrull, M. et al. *Modeling Relational Data with Graph Convolutional Networks.* ESWC, 2018.

### External Biomedical Sources

* **openFDA** — U.S. Food and Drug Administration open data
* **PubMed** — National Library of Medicine biomedical literature database

### AI Explanation

* **Google Gemini** — used for grounded natural-language explanations within the CHEERS application.

### Detailed Project Documentation

For complete research methodology, data preprocessing, experimental settings, ablation studies, reproducibility information, and detailed results:

**[`README_research.md`](README_research.md)**

---

## Disclaimer

CHEERS is a graduation project developed for research and educational purposes.

The DDI prediction results are computational predictions and should not be interpreted as medical advice, clinical recommendations, or confirmed evidence of drug safety or drug interaction severity.

Users should consult qualified healthcare professionals and authoritative medical information when making decisions regarding medications.
