# Circular Trade Detection

A team project completed as part of the **Fraud Analytics Using Predictive and Social Network Techniques** course during my B.Tech in Mathematics & Computing at **IIT Hyderabad**.

## Overview

Circular trading is a form of financial and tax fraud in which a group of businesses conduct transactions among themselves to create the appearance of legitimate business activity, potentially inflating sales or manipulating tax liabilities.

This project explores the detection of potential circular-trading patterns by representing transaction data as a **directed graph** and applying **Node2Vec** for node embeddings followed by **K-Means clustering**.

## Approach

The transaction data is transformed into a graph where:

* Each buyer or seller is represented as a **node**.
* Each transaction is represented as a **directed edge** from seller to buyer.
* The transaction/tax amount is stored as the **edge weight**.
* Multiple transactions between the same entities are represented using a **MultiDiGraph**.

The detection pipeline consists of:

```text
Transaction Data
       ↓
Directed Transaction Graph
       ↓
Node2Vec Embeddings
       ↓
K-Means Clustering
       ↓
Cluster Analysis
       ↓
Potential Circular Trade Detection
```

## Dataset

The project uses `Iron_dealers_data.csv`.

The dataset contains:

* **130,535 transactions**
* **3 columns**
* **799 unique buyer/seller IDs**

| Column      | Description           |
| ----------- | --------------------- |
| `Seller ID` | ID of the seller      |
| `Buyer ID`  | ID of the buyer       |
| `Value`     | Tax/transaction value |

## Methodology

### 1. Graph Construction

A directed multi-graph is constructed using NetworkX.

Each transaction creates an edge:

```text
Seller → Buyer
```

with the transaction value assigned as the edge weight.

### 2. Node2Vec

**Node2Vec** is used to generate vector representations of the traders based on their structural relationships in the transaction graph.

The configuration used in the project includes:

* Walk length: **30**
* Number of walks: **200**

The resulting node embeddings capture patterns in the trading relationships between entities.

### 3. K-Means Clustering

The Node2Vec embeddings are clustered using **K-Means with 10 clusters**.

The purpose is to identify groups of traders with similar patterns of connectivity within the transaction network.

### 4. Circular Trade Detection

Within the identified clusters, pairs of nodes are examined for transaction relationships.

For connected pairs, the project:

1. Calculates the total transaction value between the two traders.
2. Calculates the net sales-related value for each trader based on incoming and outgoing transaction weights.
3. Compares the transaction value between the pair against the calculated sales values.
4. Flags relationships satisfying the project's threshold condition as potential circular-trade activity.

## Technologies Used

* Python
* Pandas
* NumPy
* NetworkX
* Node2Vec
* Scikit-learn
* K-Means Clustering

## Results

The analysis identified transaction relationships satisfying the project's circular-trade detection criteria, indicating potential circular-trading activity among the traders in the dataset.