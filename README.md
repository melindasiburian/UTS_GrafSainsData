# Graph Data Science: User-Item Interaction Analysis and Recommendation System

## 📌 Project Overview

This project implements a **Graph Data Science (GDS)** workflow using Neo4j and Python to analyze user-item interactions and develop a recommendation system based on graph relationships and similarity measures.

The project uses the **RetailRocket e-commerce dataset**, which contains user interactions with items, including product views, add-to-cart events, and transactions. The interaction data is represented as a bipartite graph, where users and items are modeled as nodes connected by interaction relationships.

The project explores graph structure, Vector Space Model (VSM) representations, cosine similarity, K-Nearest Neighbors (KNN), and recommendation evaluation.

## 🎯 Objectives

- Analyze the structure and composition of a user-item interaction graph.
- Build and examine a bipartite graph using Neo4j.
- Construct User VSM and Item VSM representations.
- Analyze the sparsity and characteristics of interaction matrices.
- Calculate user similarity using cosine similarity.
- Identify similar users using KNN and Neo4j Graph Data Science.
- Generate item recommendations based on user interactions and similar users.
- Evaluate recommendation performance using Hit@K and NDCG@K.

## 📂 Dataset

This project uses the RetailRocket e-commerce dataset, particularly `events.csv`.

The dataset contains the following attributes:

| Attribute | Description |
|---|---|
| `visitorid` | Unique identifier of a visitor |
| `itemid` | Unique identifier of an item |
| `event` | Type of interaction: `view`, `addtocart`, or `transaction` |
| `timestamp` | Timestamp of the interaction |

### Temporal Data Split

The interaction data is divided into two periods:

- **P1 – Training period:** May 16, 2015 – August 4, 2015.
- **P2 – Testing period:** August 5, 2015 – August 25, 2015.

This temporal split allows the recommendation system to be evaluated using interactions occurring after the training period.

> Note: The dataset is not included in this repository. Users must obtain the dataset separately and configure the file paths in the notebook.

## 🛠️ Technologies

- **Python** – Data processing and recommendation analysis.
- **Jupyter Notebook** – Interactive development and experimentation.
- **Pandas and NumPy** – Data manipulation and numerical computation.
- **SciPy Sparse Matrices** – Efficient storage and processing of sparse VSM matrices.
- **Scikit-learn** – Cosine similarity computation.
- **NetworkX and Matplotlib** – Graph analysis and visualization.
- **Neo4j** – Graph database for storing user-item interactions and similarity relationships.
- **Neo4j Graph Data Science (GDS)** – Graph projection and KNN-based similarity analysis.
- **Py2neo or Neo4j Python Driver** – Python-to-Neo4j connectivity, depending on the notebook implementation.

## ⚙️ Methodology

### 1. Data Preprocessing

The raw interaction data is loaded from `events.csv`, cleaned, and divided into training and testing periods. Relevant fields are selected, missing identifiers are handled, and duplicate user-item interactions are addressed according to the preprocessing procedure.

### 2. Graph Construction

A bipartite graph is created in Neo4j with two node types:

- `Visitor`: represents users.
- `Item`: represents products.

The graph contains interaction relationships representing events such as `VIEWED`, `ADDED_TO_CART`, and `BOUGHT`.

### 3. Graph Structure Analysis

The graph is analyzed based on the number of nodes, relationships, active nodes, and interaction types. Graph projections are also examined to understand connectivity patterns between users and items.

### 4. Vector Space Model (VSM)

Two sparse matrices are constructed:

- **User VSM:** rows represent users, while columns represent items.
- **Item VSM:** rows represent items, while columns represent users.

The Item VSM is the transpose of the User VSM. These matrices provide numerical representations of interaction patterns for similarity analysis.

### 5. Cosine Similarity

Cosine similarity is used to measure the similarity between users based on their interaction vectors. A reference user is selected, and the similarity scores are ranked to identify the five most similar users.

The similarity results can also be stored in Neo4j as relationships between `Visitor` nodes.

### 6. K-Nearest Neighbors (KNN)

The KNN approach identifies neighboring users with similar interaction patterns. Neo4j GDS can be used to compute user similarity from projected graph data and node embeddings, depending on the configured algorithm.

The resulting similarity relationships are used to identify potential neighbors for recommendation generation.

### 7. Recommendation Generation

Items associated with similar users are considered as recommendation candidates. The recommendation procedure ranks candidate items and selects the top-K items for the reference user, with previously interacted items excluded when required by the implementation.

### 8. Evaluation

Recommendation performance is evaluated by comparing recommended items against relevant items in the testing period.

The evaluation metrics include:

- **Hit@K:** measures whether at least one relevant item appears among the top-K recommendations.
- **NDCG@K:** evaluates the ranking quality of relevant items within the top-K recommendations.

A score of zero indicates that no relevant testing item was retrieved within the evaluated recommendation list.

## 📊 Key Analysis

The notebook covers the following analyses:

- User-item graph composition and interaction distribution.
- User VSM and Item VSM characteristics, including matrix dimensions, non-zero elements, density, sparsity, and memory usage.
- Structural comparison of User-User and Item-Item graph projections.
- Top-five user similarity results.
- Graph visualization of similarity relationships in Neo4j.
- KNN-based recommendation results.
- Recommendation evaluation using Hit@5 and NDCG@5.

The notebook is intended to support experimentation and comparison between graph-based similarity analysis and recommendation strategies.

## 🚀 Getting Started

### Prerequisites

Ensure that the following software and libraries are available:

- Python 3.x
- Jupyter Notebook
- Neo4j Database
- Neo4j Graph Data Science Library
- Required Python libraries

### Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
```

Install the main Python dependencies:

```bash
pip install pandas numpy scipy scikit-learn networkx matplotlib neo4j jupyter
```

If the notebook uses additional Neo4j integration libraries, install those dependencies as well.

### Configuration

1. Start your Neo4j database.
2. Install and enable the Neo4j Graph Data Science library if required by the notebook.
3. Update the dataset path to the location of `events.csv`.
4. Configure the Neo4j connection URI, username, password, and database name.
5. Open the notebook and execute the cells in order.

Example Neo4j connection configuration:

```python
from neo4j import GraphDatabase

URI = "neo4j://127.0.0.1:7687"
DATABASE = "neo4j"
USERNAME = "neo4j"
PASSWORD = "YOUR_PASSWORD"

driver = GraphDatabase.driver(
    URI,
    auth=(USERNAME, PASSWORD)
)

driver.verify_connectivity()
```

Replace the example credentials with your own local Neo4j configuration. Do not upload passwords, API keys, or other credentials to GitHub.

## 📁 Repository Structure

```text
.
├── README.md
├── notebook.ipynb
└── data/
    └── events.csv  # Not included in the repository
```

The filenames above are illustrative. Update the structure to match the actual files in your repository.

## ⚠️ Limitations

- The interaction matrix is highly sparse, meaning many users interact with only a small subset of available items.
- Similar users do not necessarily share relevant future interactions.
- Recommendation performance depends on the training/testing split, neighbor selection, candidate generation, and ranking strategy.
- Graph projections and KNN results depend on the selected node properties, similarity configuration, and available graph relationships.
- The absence of relevant items in a top-K list can produce zero Hit@K and NDCG@K scores.

## 🔮 Future Improvements

- Incorporate interaction weights to distinguish views, cart additions, and transactions.
- Improve candidate generation and recommendation ranking.
- Compare cosine similarity, Jaccard similarity, and alternative graph-based methods.
- Experiment with different K values and similarity thresholds.
- Evaluate Precision@K, Recall@K, Hit@K, and NDCG@K across multiple users.
- Optimize graph projections and sparse matrix operations for large-scale datasets.

## 👩‍💻 Author

**Melinda Siburian**

Data Science Student | Universitas Negeri Surabaya

## 📄 License

A license has not yet been specified. Add a `LICENSE` file if you intend to distribute this project under an open-source license.
