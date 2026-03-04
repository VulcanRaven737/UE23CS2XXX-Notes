#  Complete Tutor Notes: Interdisciplinary Deep Learning on Graphs

### UE23AM342BA1 — Dr. Bhaskarjyoti Das (Based on Stanford CS224W by Prof. Leskovec)

---

> **How to use these notes:** Every major concept is explained from first principles, with intuition, formal definitions, real-world analogies, and exam-style Q&A at the end of each section. Topics outside the slides are added where they strengthen understanding.

---

# TABLE OF CONTENTS

1. [Euclidean vs. Non-Euclidean Data](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#1-euclidean-vs-non-euclidean-data)
2. [Geometric Deep Learning](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#2-geometric-deep-learning)
3. [Types of Graphs](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#3-types-of-graphs)
4. [Graph ML Downstream Tasks](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#4-graph-ml-downstream-tasks)
5. [Graph ML Datasets](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#5-graph-ml-datasets)
6. [Interdisciplinary Applications of GNNs](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#6-interdisciplinary-applications-of-gnns)
7. [Word2Vec & SkipGram](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#7-word2vec--skipgram)
8. [DeepWalk](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#8-deepwalk)
9. [Node2Vec & Encoder-Decoder Perspective](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#9-node2vec--encoder-decoder-perspective)
10. [Shallow Embedding Limitations](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#10-limitations-of-shallow-embedding)
11. [Graph Matrix Representations](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#11-graph-matrix-representations)
12. [Message Passing Framework in GNNs](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#12-message-passing-framework-in-gnns)
13. [Permutation Invariance & Equivariance](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#13-permutation-invariance--equivariance)
14. [Knowledge Graphs — Introduction](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#14-knowledge-graphs--introduction)
15. [Knowledge Graph Embedding (KGE)](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#15-knowledge-graph-embedding-kge)
16. [Translational KGE Models (TransE, TransH, TransR, PTransE)](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#16-translational-kge-models)
17. [Tensor Factorization & Neural KGE Models](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#17-tensor-factorization--neural-kge-models)
18. [Description-Based KGE (DKRL)](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#18-description-based-kge-dkrl)
19. [Loss Functions & Evaluation Metrics for KGE](https://claude.ai/chat/d095eb11-b982-4ed7-a8e2-0ca6b82c8701#19-loss-functions--evaluation-metrics-for-kge)

---

# 1. Euclidean vs. Non-Euclidean Data

## 1.1 What is Euclidean Data?

**Euclidean space** (ℝⁿ) is what we all intuitively understand as "regular" space — grids, coordinates, distances as we know them. It is named after the ancient Greek mathematician Euclid.

**Formal properties of Euclidean space:**

- **Identity of indiscernibles:** d(a, b) = 0 ⟺ a = b
- **Symmetry:** d(a, b) = d(b, a)
- **Triangle inequality:** d(a, b) + d(b, c) ≥ d(a, c)
- Follows the **Pythagorean theorem** for computing distances
- Sum of interior angles of any triangle = **180°**

**Examples:**

- An **image** → pixels on a 2D grid with (x, y) coordinates and z for colour/intensity
- **Audio/Text/Video** → sequences on a 1D timeline

**Why does this matter for ML?** CNNs and RNNs are specifically built to exploit the regularity of Euclidean domains:

- CNNs exploit the **grid structure** of images (left, right, up, down directions exist)
- CNNs show **shift invariance** — a cat detected in the top-left corner is still a cat in the bottom-right
- RNNs exploit the **sequential ordering** of text and audio

---

## 1.2 What is Non-Euclidean Data?

**Non-Euclidean data** lives on irregular, unstructured domains where the standard geometric rules break down.

**Key examples:**

- **Graphs** (social networks, molecular graphs, citation networks)
- **Manifolds** (curved surfaces like the Earth's surface; the triangle postulate fails — angles of a triangle on a sphere sum to MORE than 180°)

**Why graphs are non-Euclidean:**

- There is **no concept of left/right/up/down** for a node
- The **neighbourhood structure varies** across nodes (some nodes have 2 neighbours, others have 1000)
- **Isomorphism problem:** If you reorder the nodes of a graph, it is still the same graph — but the adjacency matrix looks completely different! Standard ML techniques like CNNs assume a fixed, consistent input structure
- **Convolution is hard to apply:** CNN filters rely on every position having the same neighbourhood structure. Graphs have no such guarantee

---

## 1.3 Manifolds (Extra Depth)

A **manifold** is a geometric surface that is locally flat but globally curved. Think of the Earth — locally (in your city) the ground seems flat (Euclidean), but globally it's a sphere (non-Euclidean).

In ML, datasets often live on a **low-dimensional manifold embedded in a very high-dimensional space**. Finding the decision boundary then becomes a **manifold learning** exercise.

---

## 🧠 Q&A — Section 1

**Q1: Why can't we apply CNN directly on a graph?**

> CNN requires that every location in the input has the same neighbourhood structure (e.g., every pixel has 8 neighbours in a 3×3 filter). In a graph, each node can have any number of neighbours. There's also no concept of spatial direction, so the idea of "sliding a filter" doesn't directly translate.

**Q2: What does "shift invariance" mean and why is it not applicable to graphs?**

> In CNNs, shift invariance means that if you slightly move an object in an image, the network still recognises it (because pooling summarises local features). In a graph, there is no notion of spatial translation — nodes don't have fixed positions — so this concept doesn't apply directly.

**Q3: What is the triangle postulate, and how does its failure distinguish Euclidean from non-Euclidean space?**

> The triangle postulate states that the sum of interior angles of a triangle is exactly 180°. On a sphere (non-Euclidean), the sum exceeds 180°. This violation signals that the space does not follow Euclidean geometry, meaning standard distance formulas and spatial assumptions break down.

**Q4: A molecule has atoms as nodes and bonds as edges. Why is it better to represent it as a graph rather than a SMILES string?**

> A SMILES string is a linear notation that loses local and global structural information. It cannot easily capture 3D spatial relationships, ring structures, and complex neighbourhood patterns. A graph representation preserves the full topology and allows Geometric Deep Learning models to exploit structural patterns.

**Q5: What is the role of "structural prior" in the success of CNNs?**

> Structural prior refers to an assumption baked into the model architecture about the data's structure. CNNs assume data has a regular grid structure with translation invariance. This inductive bias reduces the hypothesis space, making learning more efficient. Graphs lack this simple structural prior, which is what makes them harder to model.

---

# 2. Geometric Deep Learning

## 2.1 Definition

**Geometric Deep Learning (GDL)** is a subfield of deep learning that generalises neural network models to **non-Euclidean domains** such as graphs and manifolds.

> In traditional deep learning, "more dimensions = more features." In Geometric Deep Learning, "dimensions are related to the **type of data** itself."

For example, in a molecular graph, the dimensions correspond to atoms, bonds, and their properties — not arbitrary feature axes.

## 2.2 Why GDL?

Traditional deep learning was built for:

- Fixed-size, regular inputs (images, sequences)
- Euclidean geometry

But many real-world problems are inherently graph-structured:

- Drug discovery (molecules as graphs)
- Social network analysis
- Knowledge graph completion
- Traffic prediction
- Protein folding (AlphaFold!)

GDL provides the mathematical and architectural tools to handle these.

## 2.3 Key Insight: Symmetry as a Design Principle

GDL is fundamentally built on the idea that the model's architecture should **respect the symmetries of the underlying data**. For graphs, this means:

- **Permutation invariance:** Reordering nodes should not change graph-level predictions
- **Permutation equivariance:** Reordering nodes should consistently permute node-level predictions

---

## 🧠 Q&A — Section 2

**Q1: What distinguishes Geometric Deep Learning from classical deep learning?**

> Classical deep learning works on fixed-size, regularly structured inputs (grids, sequences) in Euclidean space. Geometric Deep Learning generalises neural networks to non-Euclidean domains like graphs and manifolds, where the structure is irregular and the standard operations like convolution don't directly apply.

**Q2: Why is symmetry important in Geometric Deep Learning?**

> Symmetry means that certain transformations to the input should leave the output unchanged (invariance) or transform the output consistently (equivariance). Respecting the symmetries of the data helps the model generalise better, requires fewer parameters, and ensures physically meaningful predictions.

---

# 3. Types of Graphs

## 3.1 Basic Graph Terminology

A **graph** G = (V, E) consists of:

- **V** = set of nodes (vertices)
- **E** = set of edges (connections between nodes)
- Edges can be **directed** (one-way) or **undirected** (two-way)
- Edges can be **weighted** (with numerical strength) or **unweighted**

## 3.2 Taxonomy of Graph Types

### Homogeneous Graph

- All nodes are of the **same type**
- All edges are of the **same type**
- Example: A social network where every node is a "person" and every edge is a "friendship"

### Heterogeneous Graph

- Nodes and edges can be of **different types**
- Example: A citation network with nodes for {authors, papers, venues} and edges for {wrote, cited, published-in}
- More practical for real-world applications

### Bipartite Graph

- Nodes are divided into **two disjoint sets**, and edges only connect nodes across sets
- Example: Recommender system — users on one side, items on the other; edges represent interactions

### Multi-relational / Multi-dimensional Graph

- Multiple **types of edges** between the same types of nodes
- Example: Twitter — edges can be {reply, retweet, mention, follow}

### Signed Graph

- Edges carry a **positive or negative sign**
- Example: Instagram follow/unfollow; Facebook follow/block

### Hypergraph

- Edges are replaced by **hyperedges** that can connect more than two nodes
- Example: A research paper (hyperedge) connecting multiple authors (nodes)

### Dynamic Graph (Temporal Graph)

- The graph changes **over time**
- Two types:
    - **DTDG (Discrete Time Dynamic Graph):** Snapshots of the graph at discrete time steps
    - **CTDG (Continuous Time Dynamic Graph):** Events happen continuously in time
- Models: LSTM + GNN, Temporal GCN

## 3.3 Explicit vs. Implicit Structure

- **Explicit structure:** The graph structure is given (e.g., a social network where friendships are recorded)
- **Implicit structure:** The graph must be constructed from raw data (e.g., build a graph from text documents by connecting co-occurring entities)

---

## 🧠 Q&A — Section 3

**Q1: What is the difference between a heterogeneous graph and a bipartite graph?**

> A bipartite graph has two types of nodes with edges only crossing between them. A heterogeneous graph is more general — it can have many types of nodes and many types of edges, without the restriction that edges only go between two groups.

**Q2: Why are hypergraphs useful?**

> Standard graphs only model pairwise relationships (between exactly 2 nodes). Hypergraphs model higher-order relationships (e.g., a paper co-authored by 5 people, which is one hyperedge connecting 5 nodes). This is more expressive for problems where groups of entities interact simultaneously.

**Q3: What is the advantage of modelling a system as a dynamic graph rather than a static graph?**

> Real-world systems evolve over time. Static graphs lose temporal information — they can't capture when edges form or dissolve, or how node properties change. Dynamic graphs preserve this temporal dimension, enabling predictions about future states or detecting anomalies over time.

**Q4: Give an example of a real-world system that can be modelled as a signed graph and explain its significance.**

> Political relationships: countries can have positive (ally) or negative (hostile) relations. Signed graphs allow the model to distinguish between trust and distrust, enabling applications like predicting political alliances, detecting online toxicity, or social influence analysis.

---

# 4. Graph ML Downstream Tasks

## 4.1 The Traditional ML Pipeline

In classical ML:

1. **Feature engineering** (manually design features)
2. **Model training** (on designed features)
3. **Prediction**

For graphs, the pipeline becomes:

1. **Graph representation / Embedding** (learn low-dimensional vectors for nodes, edges, or graphs)
2. **Downstream task** (use embeddings for prediction)

## 4.2 Node-Level Tasks

### Node Classification

- **Goal:** Predict a label for each node given partial labels
- **Setting:** Semi-supervised — some nodes have labels, many don't
- **Example:** Predict political affiliation of users in a social network based on a few labelled users and the network structure

### Node Clustering

- **Goal:** Group nodes into communities based on similarity (in features or connectivity)
- **Example:** Find interest communities in a social network; detect disease clusters in a patient graph
- **Evaluation:** Purity, Rand Index (if ground truth labels exist)

## 4.3 Edge-Level Tasks

### Link Prediction

- **Goal:** Predict whether an edge should exist between two nodes (i.e., predict missing edges)
- **Why needed:**
    - Knowledge graphs are always incomplete
    - Social graphs evolve (predict future friendships)
- **Extensions:** Predict edge sign; predict hyperedges
- **Example:** Predict future co-authorship on DBLP; predict drug-drug interactions

## 4.4 Graph-Level Tasks

### Graph Classification

- **Goal:** Predict a property of an **entire graph** (not individual nodes/edges)
- **Example:** Given a molecular graph, classify whether it is toxic or not; classify protein graphs as enzyme or non-enzyme

### Graph Generation

- **Goal:** Generate new graphs with desired properties
- **Example:** Drug discovery — generate novel molecular graphs with good binding properties

### Graph Evolution (Temporal Prediction)

- **Goal:** Predict how a graph will change over time
- **Example:** Predict the growth of a social network; predict traffic evolution on a road network

## 4.5 Other Tasks

### Visualisation

- Project high-dimensional graph data into 2D/3D for visual inspection
- Tools: t-SNE, PCA applied to node embeddings

### Reconstruction

- Use embedding similarity to reconstruct the original adjacency matrix
- Loss: Reconstruction loss between original and predicted adjacency matrix

## 4.6 Transductive vs. Inductive Learning

This is a crucial distinction in graph ML:

||**Transductive**|**Inductive**|
|---|---|---|
|**Definition**|Model only works on the graph it was trained on|Model generalises to unseen graphs/nodes|
|**Example**|DeepWalk, Node2Vec|GraphSAGE, GIN|
|**Limitation**|Cannot handle new nodes after training|More flexible|

## 4.7 Supervised / Unsupervised / Semi-supervised

- **Supervised:** Node/graph labels available for all training data
- **Unsupervised:** No labels (e.g., clustering, embedding without targets)
- **Semi-supervised:** Only some nodes/graphs are labelled (most common in graphs)

---

## 🧠 Q&A — Section 4

**Q1: What is the difference between link prediction and graph reconstruction?**

> Link prediction asks "will this specific pair of nodes be connected?" and typically concerns future or missing edges. Graph reconstruction asks "given node embeddings, can I recover the full adjacency matrix?" — it's more about evaluating the quality of the embeddings.

**Q2: Why is node classification in graphs typically semi-supervised?**

> Labelling every node in a large graph (e.g., millions of users in a social network) is expensive and impractical. In practice, only a small fraction of nodes have known labels. The graph structure itself carries implicit information, so we can propagate label information from labelled nodes to unlabelled ones through the edges.

**Q3: What is the difference between transductive and inductive graph learning?**

> Transductive learning trains embeddings for a fixed set of nodes — it cannot generalise to new nodes that arrive after training. Inductive learning learns a generalizable function (an encoder) that can produce embeddings for unseen nodes by aggregating their neighbourhood features. Inductive learning is required for dynamic graphs or large-scale settings where new data constantly arrives.

**Q4: Why is graph generation useful in drug discovery?**

> Drugs are molecules, which are graphs (atoms as nodes, bonds as edges). Instead of testing millions of candidate molecules in a lab, graph generation models can create novel molecular structures predicted to have desirable properties (e.g., high binding affinity to a target protein, low toxicity), dramatically accelerating drug discovery.

---

# 5. Graph ML Datasets

## 5.1 Citation Networks

|Dataset|Nodes|Edges|Classes|Task|
|---|---|---|---|---|
|**Cora**|Papers|Citations|7|Node classification|
|**Citeseer**|Papers|Citations|6|Node classification|
|**Pubmed**|Papers|Citations|3|Node classification|

- Nodes have **bag-of-words features** (TF-IDF of paper content)
- Networks are **unweighted and directed**

## 5.2 Biological Networks

|Dataset|Description|Graphs|Classes|
|---|---|---|---|
|**PPI**|Protein-Protein Interaction|1 large|40 node labels|
|**MUTAG**|Chemical compounds, mutagenic activity|188|2|
|**PROTEINS**|Protein graphs (amino acid sequences)|1113|2|
|**ENZYMES**|Protein structure graphs|600|6|

## 5.3 Social Networks

- **Single graph datasets:** YouTube, Flickr, BlogCatalog
- **Collection datasets:** IMDB-BINARY, IMDB-MULTI, COLLAB, REDDIT
- **Collaboration networks:** Based on ArXiv — co-authorship graphs (good for link prediction since node class labels are absent)

## 5.4 Other Networks

- Wikipedia language networks
- Enron email communication network
- Twitter network

---

## 🧠 Q&A — Section 5

**Q1: Why are citation networks like Cora popular benchmarks for node classification?**

> They have a clear ground truth (paper topics as labels), are publicly available, small enough for rapid experimentation, and have rich node features (bag-of-words). They enable fair comparison of different GNN models.

**Q2: Why are MUTAG and PROTEINS suitable for graph classification but not node classification?**

> In these datasets, each molecule or protein is a separate, independent graph. The task is to assign a label to the entire graph (e.g., toxic/non-toxic). There's no single large graph with nodes to classify. Node classification would only make sense within a single connected graph.

**Q3: Why is the ArXiv collaboration network suitable for link prediction but not node classification?**

> The ArXiv dataset only contains papers from a single field, so meaningful class labels distinguishing node types are absent. However, its co-authorship edges are perfect for link prediction — predicting future collaborations between researchers based on the existing network structure.

---

# 6. Interdisciplinary Applications of GNNs

## 6.1 Social Networks

- **Flickr:** Multi-label node classification — predict which interest groups a user will join
- **Twitter:** Predicting popularity of conversations; detecting propaganda/troll activity (bipartite graph of accounts and devices → project to account-account edges)
- **Crisis Detection:** Combine spatial + temporal + content features of tweet conversation graphs

## 6.2 Recommender Systems (PinSage @ Pinterest)

- Model as a **bipartite graph** (users + items) + implicit/explicit edges
- **Link prediction** → top-K item recommendation
- Matrix completion ≡ link prediction on a bipartite heterogeneous graph

## 6.3 Traffic Prediction (Google Maps)

- City road segments = nodes; spatial relations = edges
- Node features: traffic speed, volume, density as a function of time
- **Spatio-temporal graph** → predict travel time (sub-graph level regression)

## 6.4 Computer Vision

- **Visual Question Answering (VQA):** Objects in image as nodes (bounding boxes); edges based on feature similarity + relevance to question → GNN → answer selection
- **Action Recognition:** Human skeleton as graph (joints = nodes, bones = edges); classify actions (running, fighting) → graph classification with temporal dimension
- **Multi-label Image Classification:** Co-occurrence of objects → graph of objects; multi-label classification as n binary problems

## 6.5 Natural Language Processing

- **Dependency Trees:** Parse tree of a sentence → GNN over syntactic structure → classification (semantic roles, relation extraction, machine translation)
- **Entity GCN:** Entities as nodes; co-occurrence/co-reference as edges → multi-hop QA, summarisation
- **Graph2Seq:** GNN encoder + sequence decoder → abstractive summarisation, text generation

## 6.6 Knowledge Graphs

- **KG Completion:** Predict missing triples (h, r, t)
- **Knowledge-assisted QA:** Combine KG embeddings with text for richer question answering
- **Fake news detection:** Use KG as external knowledge source

## 6.7 Chemistry & Drug Discovery

- **Molecular fingerprints:** GNN provides better features than hand-crafted heuristics
- **Chemical reaction prediction:** Can molecules A and B react? Predict the product
- **Drug-target binding affinity:** Drug (graph) + protein (sequence) → regression on binding strength

## 6.8 Healthcare

- **Polypharmacy side effects:** Heterogeneous graph of drug-protein-drug interactions; multi-relational link prediction
- **Disease prediction:** Patients as nodes; MRI features as node features; phenotype similarity as implicit edges → node classification (diseased/healthy)
- **Protein classification:** Classify proteins as enzyme/non-enzyme using amino acid distance graphs (edge if distance < 6 Å)

## 6.9 Protein Folding (AlphaFold)

- Protein structure is fundamentally a graph problem
- AlphaFold2 (DeepMind) achieved breakthrough accuracy by treating protein structure prediction as a geometric deep learning problem
- Atoms/residues = nodes; spatial proximity = edges

## 6.10 Cyber Security

- Bipartite graph of accounts ↔ devices; burst of activity creates temporal graph snapshots
- **Node classification:** Classify malicious vs. legitimate accounts
- Same framework applies to **social media propaganda detection** (troll armies)

---

## 🧠 Q&A — Section 6

**Q1: How is traffic prediction a graph problem?**

> Road segments are nodes, spatial proximity defines edges, and traffic speed/volume are time-varying node features. The question "what will traffic be like in 30 minutes?" is a regression problem on a spatio-temporal graph — combining graph structure (road network) with temporal patterns (traffic flow over time).

**Q2: How does GNN help in polypharmacy side effect prediction?**

> When multiple drugs are administered together, they interact through shared protein targets. This creates a complex heterogeneous graph (drug-protein-drug). GNNs can model this multi-relational graph and predict which combinations will cause adverse side effects — a multi-relational link prediction task.

**Q3: Explain how image-based action recognition can be framed as a graph classification problem.**

> The human body is modelled as a skeleton graph where joints are nodes and bones are edges. A sequence of skeleton graphs over time captures motion. The task of identifying the action (running, attacking, embracing) is then a graph classification problem where the class label is the action. Complications include modelling inter-skeleton edges (multiple people interacting) and the temporal dimension.

**Q4: How is recommender system design related to graph link prediction?**

> A recommender system can be modelled as a bipartite graph with users and items as nodes. Interactions (clicks, purchases, ratings) are edges. Recommending items to a user means predicting which user-item edges are likely to exist but don't yet — exactly the link prediction task. GNNs aggregate information from user-user, item-item, and user-item edges to produce rich embeddings for this prediction.

---

# 7. Word2Vec & SkipGram

## 7.1 Why Word2Vec? (Motivation)

Before Word2Vec, words were represented as **one-hot vectors** — huge, sparse, and unable to capture semantic similarity (the vector for "king" is just as close to "banana" as it is to "queen").

Word2Vec (Mikolov et al., 2013) learns **dense, low-dimensional embeddings** that capture semantic and syntactic relationships. Famous example: **vec("king") - vec("man") + vec("woman") ≈ vec("queen")**

## 7.2 The Two Word2Vec Architectures

### SkipGram (what we focus on)

- **Given a center word, predict its surrounding context words**
- Example: Given "sat", predict {the, cat, on, the, mat}

### CBOW (Continuous Bag of Words)

- **Given context words, predict the center word**
- Opposite of SkipGram

## 7.3 SkipGram Architecture

```
Input: one-hot vector of center word (1 × V)
       ↓
Hidden layer: W_in (V × N) — no activation (identity function)
       ↓
Hidden representation: (1 × N) = WORD EMBEDDING
       ↓
Output layer: W_out (N × V)
       ↓
Softmax over V vocabulary → probability distribution over context words
```

- **V** = vocabulary size (e.g., 100,000 words)
- **N** = embedding dimension (chosen hyperparameter, e.g., 300)
- **After training:** The rows of W_in ARE the word embeddings (the hidden layer is just a lookup table)

## 7.4 Loss Function

**Objective:** Maximize the average log probability of context words given the center word:

```
L = (1/T) Σ_t Σ_{-c≤j≤c, j≠0} log P(w_{t+j} | w_t)
```

Where:

- T = length of training corpus
- c = context window size
- P(w_O | w_I) = softmax(v'_{w_O}^T · v_{w_I}) / Σ_w exp(v'_w^T · v_{w_I})

**In practice:** Minimize the **negative log likelihood** (NLL).

## 7.5 Training Optimisations

Computing softmax over the full vocabulary V is expensive (millions of words). Three key tricks:

### 1. Hierarchical Softmax

- Arrange vocabulary in a **binary Huffman tree** (frequent words closer to root)
- Probability of a word = product of probabilities along the path from root to leaf
- Computation reduces from O(V) to O(log₂ V)

### 2. Subsampling

- Frequent words like "the", "is", "a" add less information
- Probabilistically **discard frequent words** during training based on a formula:
    - P(discard wᵢ) = 1 - √(t / f(wᵢ)) where t is a threshold and f(wᵢ) is word frequency

### 3. Negative Sampling

- Instead of updating all V output weights for each training example, only update:
    - The **positive word** (actual context word)
    - A small number of **negative words** (randomly sampled non-context words, proportional to unigram frequency raised to 3/4 power)
- Drastically reduces computation

## 7.6 Is Word2Vec Supervised, Unsupervised, or Self-Supervised?

- **Supervised:** You train a network to predict context words (there is a defined prediction task)
- **Unsupervised:** No human-labelled data is needed — the training data is just raw text
- **Self-supervised:** The supervision signal is **automatically generated** from the structure of the data itself (the word co-occurrence in a sentence)

> Word2Vec is best described as **self-supervised learning** — it uses the inherent structure of language as a free source of supervision.

## 7.7 Theoretical Connection: Word2Vec ≡ PMI Matrix Factorization

It has been formally proven that **SkipGram with Negative Sampling (SGNS)** is implicitly factorising a **word-context PMI (Pointwise Mutual Information) matrix**, shifted by a global constant.

PMI(w, c) = log(P(w, c) / (P(w) · P(c)))

This means Word2Vec rediscovered a classical statistical concept (PMI) but with much greater efficiency using neural networks.

---

## 🧠 Q&A — Section 7

**Q1: What problem does Word2Vec solve that one-hot encoding cannot?**

> One-hot encoding represents every word as a completely orthogonal vector — there is zero notion of semantic similarity. Word2Vec learns dense embeddings where semantically similar words end up closer together in the vector space, enabling arithmetic operations like "king - man + woman = queen."

**Q2: Why is the hidden layer output (not the output layer) used as the word embedding?**

> The hidden layer weight matrix W_in (V × N) encodes the input word's context distribution in a compact N-dimensional space. The output layer is discarded after training because its purpose was only to define the prediction task during training. The embedding is the compact representation in the hidden layer, not the prediction.

**Q3: What is negative sampling, and why is it necessary?**

> Negative sampling is a training trick that updates only a small subset of weights per training step — the target (positive) word and a few randomly-selected non-target (negative) words — instead of all V words in the vocabulary. This is necessary because updating all V weights for every training example is computationally prohibitive when V is large (hundreds of thousands to millions).

**Q4: How does hierarchical softmax speed up training?**

> It replaces the flat softmax computation (O(V) operations) with a binary tree traversal (O(log V) operations). The vocabulary is arranged in a Huffman tree so that frequent words have short paths. The probability of a word is the product of sigmoid scores along the path from root to leaf, reducing computation dramatically.

**Q5: What does it mean that Word2Vec is "self-supervised"?**

> Self-supervised learning automatically generates labels from the data itself, without human annotation. In Word2Vec, the "label" for a center word is the set of surrounding context words — this is derived directly from the structure of the text. No human ever says "this context word goes with this center word." The supervision signal is implicit in the natural structure of language.

---

# 8. DeepWalk

## 8.1 Core Idea

**DeepWalk (Perozzi et al., 2014)** is the first major graph embedding method. Its insight is elegant:

> **"A random walk on a graph is like writing a sentence in a language."**
> 
> The sequence of nodes visited ≡ words in a sentence. Therefore, we can apply Word2Vec's SkipGram to learn node embeddings!

## 8.2 The Key Statistical Observation: Zipf's Law

**Zipf's Law** states that the frequency of any word in natural language is inversely proportional to its rank in the frequency table. So the 2nd most common word appears roughly half as often as the 1st, the 3rd about a third as often, etc.

Perozzi et al. showed that **the frequency statistics of nodes appearing in random walks on graphs also follow Zipf's Law** — just like words in language!

This is the mathematical justification for applying Word2Vec to graphs.

## 8.3 DeepWalk Algorithm (Two Phases)

### Phase 1: Generate Random Walks

```
For each epoch γ:
    Shuffle nodes V
    For each node u in V:
        For i = 1 to number_of_walks_per_node:
            Walk = RandomWalk(G, u, walk_length t)
            Collect walk
```

**A random walk:** From node u, randomly jump to one of u's neighbours, then from that node jump to one of its neighbours, and so on for t steps. This generates a sequence like: [3, 7, 2, 9, 4, ...].

### Phase 2: Apply SkipGram

```
For each random walk sequence:
    For each node vⱼ in the sequence:
        For each context node uₖ in window [-w, +w] around vⱼ:
            Maximise P(uₖ | φ(vⱼ))  // via gradient descent
```

**φ** is the embedding matrix (the hidden layer weights in SkipGram), which we're optimising.

## 8.4 DeepWalk Uses Hierarchical Softmax

Just like Word2Vec, DeepWalk uses hierarchical softmax to avoid the O(|V|) computation at each training step. The vocabulary here is the set of nodes V.

## 8.5 Analogy Table

|NLP (Word2Vec)|Graph (DeepWalk)|
|---|---|
|Corpus of sentences|Corpus of random walks|
|Words|Nodes|
|Vocabulary size|Number of nodes|
|Context window|Walk window|
|Word embedding|Node embedding|
|Word co-occurrence|Node co-occurrence in walks|

## 8.6 What DeepWalk Learns

DeepWalk learns embeddings such that **nodes that frequently co-occur in random walks are embedded close to each other** in the low-dimensional space. Nodes that are "close" in the graph tend to co-occur more in random walks, so nearby nodes get similar embeddings.

---

## 🧠 Q&A — Section 8

**Q1: Why is the analogy between random walks and sentences valid?**

> Both follow Zipf's Law — frequency statistics of nodes in random walks and words in sentences follow the same power-law distribution. This statistical similarity means the same training objective (maximise co-occurrence probability) is meaningful in both domains.

**Q2: What is the role of the context window in DeepWalk?**

> The context window w defines how many nodes before and after the current node in the walk are considered "context." A larger window captures longer-range dependencies, while a smaller window focuses on immediate neighbours. It controls how much of the walk neighbourhood is used as co-occurrence evidence.

**Q3: Is DeepWalk supervised or unsupervised? Is it transductive or inductive?**

> DeepWalk is **unsupervised** — it learns embeddings without any labels. It is **transductive** — it optimises embeddings for the specific nodes seen during training. New nodes arriving after training cannot get embeddings without re-running DeepWalk on the updated graph.

**Q4: What is the key limitation of DeepWalk that Node2Vec addresses?**

> DeepWalk uses purely **unbiased random walks** — each step is equally likely to go to any neighbour. This means the walk doesn't distinguish between exploring the local neighbourhood (BFS-like) or venturing far away (DFS-like). Node2Vec introduces biased walks that can interpolate between these two exploration strategies.

---

# 9. Node2Vec & Encoder-Decoder Perspective

## 9.1 The Problem Node2Vec Solves

DeepWalk's unbiased random walk creates one type of embedding — nodes that are in the same community are close together. But there's another important notion of similarity:

- **Homophily:** Nodes are similar if they belong to the same community (same social circle, same topic cluster)
- **Structural equivalence:** Nodes are similar if they play the same structural role in the graph (e.g., both are "hub" nodes even if in different communities)

**DeepWalk can only capture homophily.** Node2Vec can capture both by controlling the walk strategy.

## 9.2 BFS vs. DFS Random Walks

||**BFS (Breadth-First Search)**|**DFS (Depth-First Search)**|
|---|---|---|
|**Strategy**|Explore close neighbours first|Explore further nodes, venture away|
|**View**|Microscopic / local|Macroscopic / global|
|**Similarity captured**|**Structural equivalence**|**Homophily**|
|**p parameter**|Low p → BFS-like (return to origin)|—|
|**q parameter**|—|Low q → DFS-like (explore outward)|

## 9.3 First-Order vs. Second-Order Random Walk

### First-Order Random Walk

- The next step depends **only on the current node**
- Transition probability to neighbor x: P(x | v) = w(v, x) / Σᵤ w(v, u)
- This is what DeepWalk does

### Second-Order Random Walk (Node2Vec)

- The next step depends on **both the current node AND the previous node**
- This is called a **2nd-order Markov chain**
- The walker "remembers" where it came from and adjusts its next move accordingly
- PageRank is a 1st-order random walk; Node2Vec is 2nd-order

## 9.4 The Biased Walk in Node2Vec

Suppose the walk just traversed edge (z → u). Now we're at u, deciding where to go next. For each candidate next node s:

Compute **distance d(z, s)**:

- **d = 0** (s = z, going back): weight = **1/p** (p = return parameter)
- **d = 1** (s is also a neighbor of z): weight = **1** (neutral)
- **d = 2** (s is not a neighbor of z): weight = **1/q** (q = in-out parameter)

**Intuition of p and q:**

|Parameter|Effect|Walk Type|
|---|---|---|
|Low p (< 1)|Encourages returning to z|BFS-like, stays local|
|High p (> 1)|Discourages returning to z|DFS-like, ventures outward|
|Low q (< 1)|Encourages going to new territory|DFS-like (outward exploration)|
|High q (> 1)|Discourages going to new territory|BFS-like (local exploration)|

## 9.5 Concrete Example

Consider the walk arrived at node g from node e. Now at g, candidates for next step: {b, c, f, e, d}

Distances from e (previous node) to candidates:

- e → b: distance 2 → weight **1/q**
- e → c: distance 1 → weight **1**
- e → f: distance 1 → weight **1**
- e → e: distance 0 → weight **1/p** (returning)
- e → d: distance 2 → weight **1/q**

These weights are then normalised to get transition probabilities.

## 9.6 Node2Vec Algorithm

1. **Pre-compute transition probabilities** (second-order, for all edge triples)
2. **Simulate biased random walks** starting from each node (r walks of length l)
3. **Apply SkipGram** on the walk sequences (with negative sampling)
4. **Output:** Node embedding matrix Z

## 9.7 How to Use Node Embeddings

After learning node embeddings z_v for all nodes:

**For node classification:** z_v → linear layer / MLP → class label

**For link prediction:** Use a combination function on z_u, z_v:

- Hadamard product: z_u ⊙ z_v
- Average: (z_u + z_v) / 2
- L1 distance: |z_u - z_v|
- L2 distance: ||z_u - z_v||²

**For graph classification:**

- **Approach 1:** Add a virtual "super-node" connected to all nodes in the subgraph, run DeepWalk including the super-node, and use its embedding as the graph embedding
- **Approach 2:** Simply sum or average all node embeddings in the graph

## 9.8 Edge2Vec

Edge embeddings can be derived from node embeddings by combining the embeddings of the two endpoints:

- Average: (z_u + z_v) / 2
- Hadamard: z_u ⊙ z_v
- L1 norm: |z_u - z_v|
- L2 norm: ||z_u - z_v||²

## 9.9 Encoder-Decoder Perspective

This is a powerful unifying framework for understanding ALL graph embedding methods.

### The Framework

**Encoder:** Maps each node u to a low-dimensional embedding z_u

```
ENC: V → ℝᵈ
z_u = ENC(u)
```

**Decoder:** Reconstructs some graph property from the embeddings

```
DEC: ℝᵈ × ℝᵈ → ℝ  (for pairwise tasks)
dec(z_u, z_v) ≈ S[u, v]
```

Where S[u, v] is a **graph-based similarity measure** (e.g., whether u and v are neighbours, their number of shared neighbours, etc.)

### Loss Function

```
L = Σ_{(u,v) ∈ D} l(dec(z_u, z_v), S[u, v])
```

Where l is a loss function (MSE, cross-entropy, hinge loss, etc.)

### What Varies Across Methods?

|Method|Encoder|Decoder|Similarity S|Loss|
|---|---|---|---|---|
|GF|Lookup table|Inner product|Adjacency A|MSE|
|GraRep|Lookup table|Inner product|Powers of A|MSE|
|HOPE|Lookup table|Inner product|Neighbourhood overlap|MSE|
|DeepWalk|Lookup table|Log softmax|Random walk co-occurrence|NLL|
|Node2Vec|Lookup table|Log softmax|Biased walk co-occurrence|NLL|

### Shallow Encoding (the encoder in DeepWalk/Node2Vec)

The encoder in DeepWalk/Node2Vec is called **"shallow"** because:

- It is simply a **lookup table** — given a node ID, return its embedding row
- There is NO neural network processing
- The embedding matrix Z (|V| × d) is directly optimised
- This is equivalent to a single embedding layer with an identity activation

```
ENC(v) = Z · 1_v     (Z: embedding matrix, 1_v: one-hot indicator)
```

---

## 🧠 Q&A — Section 9

**Q1: How do the parameters p and q control the type of embedding learned?**

> p is the "return parameter" — low p makes the walk more likely to return to the previous node (BFS-like, capturing local structure). q is the "in-out parameter" — low q makes the walk more likely to explore distant nodes (DFS-like, capturing community structure). By tuning p and q, you can bias the embedding towards structural equivalence (low p) or homophily (low q).

**Q2: What is the difference between structural equivalence and homophily? Give a concrete example.**

> **Homophily:** Nodes are similar if they are in the same community. Two users in the same Twitter friend group have homophilic similarity — they're adjacent in the graph. **Structural equivalence:** Nodes are similar if they play the same role in the graph, even if they're far apart. The "admin" of Group A and the "admin" of Group B are structurally equivalent (both are hub nodes) even if they've never interacted.

**Q3: Why is Node2Vec called "second-order"?**

> Because the next step of the walk depends on both the current node AND the previous node — this is a 2nd-order Markov property (the state depends on the last 2 positions, not just the last 1). By contrast, DeepWalk is 1st-order (only depends on the current node), as is PageRank.

**Q4: In the Encoder-Decoder framework, what does the decoder do in the context of link prediction?**

> The decoder takes the embeddings of two nodes and predicts whether they are connected (or the strength of their connection). A simple pairwise decoder is the inner product: dec(z_u, z_v) = z_u^T · z_v. If this dot product is high, the nodes are predicted to be similar or likely connected.

**Q5: What is matrix factorization perspective of the Encoder-Decoder framework?**

> If we define S as a node-node similarity matrix (e.g., the adjacency matrix A), and the encoder as a lookup table Z, then training the encoder-decoder is equivalent to finding a low-rank factorisation of S such that S ≈ Z · Z^T. This connects graph embedding to classical dimensionality reduction and spectral methods.

---

# 10. Limitations of Shallow Embedding

## 10.1 Limitation 1: Inherently Transductive

**Problem:** Shallow embedding only generates embeddings for nodes seen during training. New nodes have no embedding.

**Why:** The encoder is just a lookup table — there is no function to compute an embedding from node features or graph structure for an unseen node.

**Impact:** Shallow embedding methods **cannot be used for inductive tasks** (generalising to new nodes or new graphs).

## 10.2 Limitation 2: Node Features NOT Used

**Problem:** The encoder ignores any node features (text, image, biological properties, etc.).

**Why:** The embedding is learned purely from graph structure (co-occurrence in random walks). Rich node-level attributes are discarded.

**Impact:** Many real-world graphs have highly informative node features that could dramatically improve downstream task performance.

## 10.3 Limitation 3: No Shared Parameters

**Problem:** Each node has its own independent embedding vector. There is **no parameter sharing** between nodes.

**Why this matters:**

- **Statistical inefficiency:** No regularisation through shared structure. Similar nodes can't benefit from each other's training signal.
- **Computational inefficiency:** The number of parameters grows as O(|V|) — millions of parameters for large graphs.
- With a 100-million-node graph and 128-dimensional embeddings, that's 12.8 billion parameters just for the embedding matrix!

## 10.4 The Solution: Deep Graph Encoders (GNNs)

GNNs address all three limitations:

1. **Inductive:** The encoder is a FUNCTION (neural network) that can process any node by aggregating its neighbourhood → generalises to new nodes ✓
2. **Uses node features:** Node features are the initial input to the GNN ✓
3. **Shares parameters:** All nodes use the same set of neural network weights ✓

---

## 🧠 Q&A — Section 10

**Q1: If a graph has 10 million nodes and we use shallow embedding with 128 dimensions, how many parameters does the embedding have?**

> 10,000,000 × 128 = **1.28 billion parameters** — just for the embedding matrix, before any downstream task. This is computationally and statistically impractical for large graphs.

**Q2: Why is lack of parameter sharing a problem from a statistical perspective?**

> Parameter sharing acts as a form of regularisation — it forces the model to learn a consistent, generalizable representation strategy rather than memorising every node individually. Without sharing, the model has no reason to produce consistent representations for nodes with similar structural roles or features.

**Q3: How do GNNs overcome the transductive limitation of shallow embeddings?**

> GNNs learn a parametric encoder function f(x_v, neighbourhood) that takes a node's features and its local neighbourhood as input and computes an embedding. This function can be applied to any node — including nodes not seen during training — as long as their features and neighbourhood structure are available.

---

# 11. Graph Matrix Representations

## 11.1 Adjacency Matrix (A)

For a graph with n nodes:

- A is an **n × n matrix**
- A[i][j] = 1 if there is an edge from node i to node j (0 otherwise)
- For undirected graphs, A is symmetric
- A is typically **sparse** (most pairs of nodes are NOT connected)

## 11.2 Degree Matrix (D)

- D is a **diagonal matrix**
- D[i][i] = degree of node i (number of edges connected to it)
- D[i][j] = 0 for i ≠ j

## 11.3 Laplacian Matrix (L)

The **Graph Laplacian** is defined as:

```
L = D - A
```

Properties:

- L is symmetric and positive semi-definite
- Every row and column sums to 0
- The number of zero eigenvalues equals the number of connected components
- Eigenvalues of L are used in **spectral graph theory** and **spectral clustering**

## 11.4 Feature Matrix (X)

- X is an **n × f matrix** where n = number of nodes, f = number of features per node
- Row i of X = feature vector of node i

## 11.5 The Key Operation: A · X

When we multiply the adjacency matrix A with the feature matrix X:

```
(A · X)[i] = Σⱼ A[i][j] · X[j] = SUM of feature vectors of all neighbours of i
```

**Interpretation:** For each node i, A · X computes the **sum of feature vectors of i's neighbours**.

### Problem with A · X

- Missing self-features: Node i's own features are not included in the sum

### Fix: Add Self-Loops (Ã = A + I)

```
Ã = A + λI   (typically λ = 1)
```

Now Ã · X computes: **(sum of neighbor features) + (own features)**

## 11.6 Normalisation

**Problem with raw sum:** High-degree nodes have larger feature sums, leading to numerical instability and gradient issues.

**Solution 1: Row normalisation (D⁻¹ · Ã)**

- Divides each node's aggregated features by its own degree
- Equivalent to **averaging** neighbour features

**Solution 2: Column normalisation (Ã · D⁻¹)**

- Divides by the degree of the source node
- Normalises by the degree of the node being aggregated FROM

**Solution 3: Symmetric normalisation (D⁻¹/² · Ã · D⁻¹/²)**

- Normalises by **both** the degree of the source and the target node
- **Intuition:** Low-degree nodes should have more impact on their neighbours (they scatter their influence less). High-degree nodes have less impact per edge.
- This is the normalisation used in **Graph Convolutional Networks (GCN)**

```
The (i,j) entry of D^{-1/2} Ã D^{-1/2} = 1 / sqrt(dᵢ · dⱼ)
```

---

## 🧠 Q&A — Section 11

**Q1: Why do we add a self-loop when doing graph convolution?**

> Without a self-loop, when we multiply A · X, each node aggregates its NEIGHBOURS' features but ignores its own. Self-loops (adding I to A) ensure each node includes its own features in the aggregation, which is crucial for preserving node identity during message passing.

**Q2: What happens mathematically when we compute A · X?**

> For node i, (A · X)[i] = Σⱼ A[i,j] · X[j]. Since A[i,j] = 1 only for neighbours j of i, this sum equals the sum of feature vectors of all neighbours of i. Effectively, each node's row in the result is the sum of its neighbours' feature vectors.

**Q3: What is the problem with row normalisation (D⁻¹Ã) and how does symmetric normalisation fix it?**

> Row normalisation (D⁻¹Ã) normalises by the degree of the target node (recipient). This makes a high-degree recipient dilute its received messages. Symmetric normalisation (D⁻¹/²ÃD⁻¹/²) normalises by BOTH the sender's and receiver's degree, ensuring that low-degree nodes (which are more "specialised") have higher per-edge influence while high-degree nodes (hubs) have lower per-edge influence.

**Q4: What does the Laplacian matrix tell us about the graph?**

> The eigenvalues of the Laplacian encode rich structural information. The number of zero eigenvalues equals the number of connected components. The smallest non-zero eigenvalue (algebraic connectivity / Fiedler value) measures how well-connected the graph is. Spectral clustering uses the eigenvectors of L to partition nodes into communities.

---

# 12. Message Passing Framework in GNNs

## 12.1 The Core Idea

Unlike shallow embeddings (which are just lookup tables), GNNs compute node embeddings by **iteratively aggregating information from neighbours**.

**Intuition:** Your identity is shaped by your neighbourhood. In a social network, who you are is partly defined by who your friends are, and who THEIR friends are.

## 12.2 Computation Graph

Each node u defines a **computation graph** based on its neighbourhood:

- At depth 0: just node u itself
- At depth 1: u + all direct neighbours of u
- At depth k: u + all nodes within k hops of u

A GNN with K layers computes embeddings using **k-hop neighbourhoods**.

## 12.3 The Two Core Operations

### AGGREGATE

- **Input:** The embeddings of all neighbouring nodes N(v) at the previous layer
- **Output:** A single aggregated "message" from the neighbourhood
- Must be a **permutation-invariant function** (e.g., sum, mean, max) since there's no natural ordering of neighbours

### UPDATE

- **Input:** The aggregated neighbourhood message + the node's own previous embedding
- **Output:** The node's new embedding for this layer
- Typically involves a **trainable neural network** (linear transformation + non-linearity)

## 12.4 The Basic GNN Equation

For node v at layer k:

```
h_v^(k) = σ( W_k · MEAN{ h_u^(k-1) : u ∈ N(v) ∪ {v} } )
```

Or more explicitly (separating AGGREGATE and UPDATE):

```
a_v^(k) = AGGREGATE({ h_u^(k-1) : u ∈ N(v) })    // neighbourhood message
h_v^(k) = UPDATE( h_v^(k-1), a_v^(k) )             // update own embedding
         = σ( W_k · CONCAT(h_v^(k-1), a_v^(k)) )
```

Where:

- **h_v^(k)** = embedding of node v at layer k
- **h_v^(0)** = initial features X_v (or one-hot if no features available)
- **W_k** = trainable weight matrix at layer k (SHARED across all nodes)
- **σ** = non-linear activation (ReLU, etc.)

## 12.5 Graph-Level Matrix Form

The full GNN computation can be written compactly as:

```
H^(k) = σ( D̃^{-1/2} Ã D̃^{-1/2} · H^(k-1) · W_k )
```

Where:

- **H^(k)** = matrix of all node embeddings at layer k (n × d)
- **Ã = A + I** (adjacency with self-loops)
- **D̃** = degree matrix of Ã
- **W_k** = weight matrix for layer k

This is the **Graph Convolutional Network (GCN)** formula (Kipf & Welling, 2017).

## 12.6 Key Improvements over Shallow Embedding

||Shallow Embedding|Deep GNN|
|---|---|---|
|Encoder|Lookup table (O(|V|
|Uses node features?|No|Yes|
|Inductive?|No|Yes|
|k-hop information?|Limited by walk|Exactly k-hop at layer k|
|Gradient flow?|Through embedding lookup only|Through full neural network|

## 12.7 What If No Node Features Are Available?

- Use **node statistics** as features: degree, clustering coefficient, etc.
- Use **one-hot encodings** (just a unique indicator per node)
- Use **constant features** (all nodes start with the same feature)

## 12.8 Self-Loops in Message Passing

When we add self-loops (Ã = A + I), the UPDATE step is implicitly merged into AGGREGATE:

```
h_v^(k) = σ( W_k · MEAN{ h_u^(k-1) : u ∈ N(v) ∪ {v} } )
```

**Benefit:** Simpler formulation, fewer equations.

**Drawback:** The node's own information from the previous layer cannot be **differentiated** from its neighbours' information. This limits expressivity — the GNN can't "know" whether a piece of information came from itself or a neighbour.

---

## 🧠 Q&A — Section 12

**Q1: Why must the AGGREGATE function be permutation-invariant?**

> Nodes have no natural ordering — the set of neighbours of node v is an unordered set. If we used an order-sensitive aggregation, the result would depend on how we arbitrarily ordered the neighbours, giving different embeddings for the same graph. Permutation-invariant functions (sum, mean, max) give the same result regardless of neighbour ordering.

**Q2: What does it mean that a GNN layer captures "k-hop neighbourhood" information?**

> After k layers of message passing, each node's embedding has aggregated information from all nodes within k hops (k edges away). Layer 1 uses immediate neighbours (1 hop), Layer 2 aggregates those to incorporate 2-hop neighbourhoods, and so on. This is the mechanism by which GNNs capture multi-hop structural context.

**Q3: Why is parameter sharing (shared W_k) important in GNNs?**

> Shared weights mean the same aggregation function is applied at every node. This is like the weight-sharing in CNNs (same filter applied everywhere). It dramatically reduces the parameter count from O(|V|·d) to O(d·d·K), makes the model inductive (can apply to new nodes), and acts as a regulariser (encourages consistent representations).

**Q4: What is the limitation of merging AGGREGATE and UPDATE via self-loops?**

> When the self-loop merges the node's own features into the neighbourhood average, the model cannot distinguish between information coming from the node itself and information coming from its neighbours. This limits the model's expressivity — in some cases, it's important to know whether a feature is "mine" or "my neighbour's."

**Q5: What information does h_v^(k) contain after k layers of message passing?**

> h_v^(k) encodes information about the entire **k-hop neighbourhood** of node v — a "summary" of all nodes reachable from v in k steps, aggregated through the learned weight matrices. This embedding reflects both the structural position of v in the graph and the features of all nodes in its k-hop neighbourhood.

---

# 13. Permutation Invariance & Equivariance

## 13.1 Why This Matters

A critical design question for GNNs: **should changing the ordering of nodes change the output?**

The mathematical answer is carefully handled through the concepts of permutation invariance and equivariance.

## 13.2 Permutation Invariance

**Definition:** A function f is permutation invariant if:

```
f(P(X)) = f(X)  for any permutation P
```

**Meaning:** Reordering the inputs doesn't change the output.

**Graph example:** Graph classification — the label of a molecular graph should be the same regardless of how we number its atoms.

**Analogy:** A bag of words approach in NLP — "The cat sat" and "cat sat The" and "sat The cat" all have the same bag-of-words representation (permutation invariant).

**Mathematical property:** A graph G has n! possible adjacency matrices (one for each ordering of nodes). Permutation invariance guarantees all are treated identically.

## 13.3 Permutation Equivariance

**Definition:** A function f is permutation equivariant if:

```
f(P(X)) = P(f(X))  for any permutation P
```

**Meaning:** If you permute the inputs, the output is permuted in the same way.

**Graph example:** Node classification — if we reorder nodes, the node embeddings should be reordered correspondingly (the same node should get the same embedding regardless of the ordering).

**Analogy:** A convolution operation on a sequence — if you shift the input sequence, the output shifts by the same amount (equivariant).

## 13.4 The Relationship Between the Two

```
Invariance: f(g(x)) = f(x)   [transformation is discarded]
Equivariance: f(g(x)) = g(f(x))  [transformation is preserved]
```

Invariance is a **special case** of equivariance where the output doesn't have the same structure as the input (e.g., scalar output vs. vector input).

## 13.5 In CNNs

- **Convolution layer:** Equivariant to translation (shift the input → output shifts)
- **Pooling layer:** Invariant (introduces invariance to small local translations)
- Lower layers: equivariant (preserve spatial information)
- Higher layers: invariant (produce translation-independent class predictions)

## 13.6 In GNNs

- **Hidden layers (message passing):** Permutation equivariant — if nodes are reordered, embeddings are reordered correspondingly
- **Final readout (graph classification):** Permutation invariant — sum/mean/max over all node embeddings is order-independent

**Why GNNs are naturally permutation equivariant:** The AGGREGATE function takes a **SET** (unordered collection) of neighbour embeddings as input. SET operations like sum, mean, max are by definition permutation invariant (the message for node v is the same regardless of how we order v's neighbours). This, combined with applying the same function to each node, makes the entire hidden layer permutation equivariant.

## 13.7 Why the Naive Approach Fails

The "naive approach" would be to flatten the adjacency matrix A into a vector and feed it to an MLP. This fails because:

- Different orderings of the same graph produce completely different input vectors
- The MLP would need to learn separate patterns for all n! orderings
- This is why we need architectures that are inherently permutation equivariant

---

## 🧠 Q&A — Section 13

**Q1: Why is permutation invariance essential for graph-level tasks but not for node-level tasks?**

> For graph classification, we want a single label for the whole graph — this label should be the same regardless of how we number the nodes (same graph, different node ordering). For node classification, we want a label per node — if we reorder nodes, the labels should be correspondingly reordered (equivariance). Graph-level tasks need invariance; node-level tasks need equivariance.

**Q2: Show mathematically why the mean aggregation in GNNs is permutation equivariant.**

> For node v with neighbours {u1, u2, u3} in any order: MEAN({h_u1, h_u2, h_u3}) = (h_u1 + h_u2 + h_u3)/3. This value is the same regardless of the order in which we list u1, u2, u3 (addition is commutative). Now if we apply a permutation P to all nodes, the set of neighbours changes correspondingly, and the mean changes in the same way P permutes the node labels. So f(P(X)) = P(f(X)) holds — equivariance is satisfied.

**Q3: What would go wrong if we used a permutation-sensitive aggregation in a GNN?**

> If aggregation depended on the order of neighbours (e.g., concatenation in a fixed order), then the same node would get different embeddings depending on how we happened to order its neighbours. This means the GNN would not be able to recognise the same graph under different node orderings, making it useless in practice.

**Q4: What is the relationship between permutation invariance/equivariance in CNNs and GNNs?**

> Both CNNs and GNNs are designed with the same principle: lower layers are equivariant (they preserve and transform spatial/structural information consistently), while the final output layer is invariant (it produces a single, stable prediction independent of the input ordering). In CNNs, the relevant transformation is spatial translation; in GNNs, it is node permutation.

---

# 14. Knowledge Graphs — Introduction

## 14.1 What is a Knowledge Graph?

A **Knowledge Graph (KG)** is a structured representation of world knowledge, where:

- **Nodes = entities** (real-world objects, concepts, places, people)
- **Directed edges = relations** between entities

Facts are stored as **triples** (h, r, t):

- **h** = head entity
- **r** = relation
- **t** = tail entity

Example: **(SpiderMan, stars_in, NoWayHome)**, **(Quebec, located_in, Canada)**

## 14.2 KG Terminology

|KG Term|Graph Term|Example|
|---|---|---|
|Entity|Node|"Tom Holland", "Canada"|
|Relation|Directed edge type|"nationality", "located_in"|
|Triple|Directed edge instance|(Tom Holland, nationality, American)|
|Triplet|(h, r, t)|(Quebec, located_in, Canada)|

## 14.3 Types of Relations

### By cardinality:

- **1-to-1:** Mary is the sister of Tom
- **1-to-N:** Amazon employs Mary, Tom, and Joe
- **N-to-1:** Mary, Tom, and Joe work at Amazon
- **N-to-N:** Joe, Mary, and Tom are colleagues

### By symmetry:

|Type|Definition|Example|
|---|---|---|
|**Symmetric**|(x, r, y) ⟹ (y, r, x)|"is a sibling of"|
|**Anti-symmetric**|(x, r, y) ⟹ ¬(y, r, x)|"is located in"|
|**Inverse**|(x, r1, y) ⟺ (y, r2, x)|"stars_in" ↔ "features"|
|**Composition**|(x, r1, y) ∧ (y, r2, z) ⟹ (x, r3, z)|"born_in" + "located_in" = "is_from"|

## 14.4 KGs are Always Incomplete

Real-world KGs (Freebase, Wikidata, Google's KG) are **inherently incomplete**. For example:

- Many people's nationalities are missing
- Many relationships between entities are not yet recorded

This is why **KG completion** (predicting missing triples) is such an important task.

## 14.5 Open World vs. Closed World Assumption

||**Closed World Assumption (CWA)**|**Open World Assumption (OWA)**|
|---|---|---|
|**Philosophy**|If a fact is not in the KG, it is FALSE|If a fact is not in the KG, it might be TRUE (just missing)|
|**Problem**|KGs are inherently incomplete → CWA is unrealistic|More complex to model|
|**Use**|Simple reasoning tasks|Real-world KG completion tasks|

**Example:** If the KG doesn't contain "(Poutine, is_a, Canadian_dish)", CWA says Poutine is NOT Canadian, while OWA says we just don't know.

## 14.6 KG Structure: Multigraph

Knowledge Graphs are **heterogeneous multigraphs**:

- **Heterogeneous:** Multiple types of nodes and edges
- **Multigraph:** Multiple directed edges between the same pair of nodes (e.g., both "collaborates_with" and "cites" can connect two researchers)

## 14.7 Types of KGs

### Standard KG

- Entities and relations
- Example: Freebase, Wikidata, Google Knowledge Graph

### Multimodal KG

- Entities have associated modalities: text descriptions, images, audio
- More expressive embeddings possible

### Temporal KG

- Relations have associated timestamps or time intervals
- (Macron, president_of, France, 2017-present)
- Important for reasoning about time-varying facts

---

## 🧠 Q&A — Section 14

**Q1: What is a triple in a knowledge graph? Give an example from each relation type (symmetric, anti-symmetric, composition).**

> A triple (h, r, t) represents a directed fact: head entity h is related to tail entity t via relation r.
> 
> - **Symmetric:** (Alice, is_sibling_of, Bob) → (Bob, is_sibling_of, Alice) must also hold
> - **Anti-symmetric:** (Quebec, is_in, Canada) → (Canada, is_in, Quebec) must NOT hold
> - **Composition:** (Joe, born_in, Quebec) + (Quebec, located_in, Canada) → (Joe, is_from, Canada)

**Q2: Why are knowledge graphs always incomplete in practice?**

> KGs are built by extracting information from text, databases, and human curation — all of which are inherently limited and error-prone. Not all real-world relationships are documented or easily extractable. Additionally, the world changes and KGs may not be updated in real-time. This is why KG completion is a critical task.

**Q3: What is the difference between the Open World Assumption and Closed World Assumption in KGs?**

> CWA treats all facts not in the KG as false (which is problematic since KGs are incomplete). OWA treats missing facts as unknown (they might be true, just unobserved). Real-world KG applications should use OWA because the absence of a fact from the KG doesn't mean that fact is false.

**Q4: Analyse the scenario: Joe plans to serve Poutine to vegetarian siblings Mary and Tom. What KG inference leads to the conclusion that Joe's plans will fail?**

> The inference chain (composition of relations):
> 
> 1. (Poutine, contains, potato) + (Poutine, contains, cheese) + (Poutine, contains, gravy)
> 2. (gravy, contains_form_of, meat)
> 3. (Poutine, is_not_vegetarian) — inferred via composition
> 4. (Mary, is_a, vegetarian) ∧ (Tom, is_a, vegetarian)
> 5. Inference: vegetarians don't eat non-vegetarian food → Mary and Tom won't eat Poutine This demonstrates how multi-hop reasoning over a KG can reveal complex implicit conclusions.

---

# 15. Knowledge Graph Embedding (KGE)

## 15.1 The Core Problem

Real-world KGs are **large, high-dimensional, and sparse**:

- Millions of entities → each entity as a one-hot vector would be millions of dimensions
- Most pairs of entities have no relationship → the adjacency tensor is extremely sparse

**Solution:** Learn **low-dimensional, dense embeddings** that capture the semantic structure of the KG.

## 15.2 KGE vs. GNN Embedding

||**KG Embedding**|**GNN Embedding**|
|---|---|---|
|**Focus**|RELATIONS (the triple structure)|NODE FEATURES + structure|
|**Input**|Triples (h, r, t) only|Node features + adjacency|
|**Node types**|Not explicitly differentiated|Often differentiated|
|**Depth**|Typically "shallow" (1-hop)|Multi-hop (K layers)|
|**Task**|Link prediction (KG completion)|Node classification, link prediction, graph classification|
|**Expressivity**|Good at direct relationships|Better at complex, multi-hop patterns|

**Hybrid approaches** combine both: use GNNs to produce richer entity embeddings, then use them in KGE scoring functions.

## 15.3 KG Completion Task

Given an incomplete KG, predict the **probability of missing triples**:

- Given (h, r, ?), predict the most likely tail entity
- Given (?, r, t), predict the most likely head entity

**Score function f(h, r, t):** Assigns high score to likely (true) triples, low score to unlikely (false) triples.

## 15.4 KG Completion vs. Link Prediction

||**KG Completion**|**Link Prediction**|
|---|---|---|
|**Objective**|Overall goal (complete the KG)|Specific technique|
|**Complexity**|Multi-relational, computationally expensive|Simpler (often single relation type)|
|**Node features**|Typically NOT used|Often used in GNN-based approaches|
|**Relations**|Multiple explicit relation types|Often a single binary relation|

---

## 🧠 Q&A — Section 15

**Q1: Why can't we just represent KG entities as one-hot vectors and do ML?**

> One-hot vectors are sparse and high-dimensional (one dimension per entity). For KGs with millions of entities, this is computationally intractable. More importantly, one-hot vectors don't capture semantic similarity — "Paris" and "London" would be just as different as "Paris" and "cheese." Dense embeddings in low-dimensional space capture the structure and meaning of entities.

**Q2: How does KGE differ from GNN embedding in terms of what it captures?**

> KGE focuses on learning representations that capture RELATION patterns (how entities are connected via specific relation types). GNN embedding focuses on aggregating neighbourhood information to capture the local graph structure and node features. KGE is typically shallower (captures direct 1-hop triples) while GNNs capture multi-hop context.

---

# 16. Translational KGE Models

## 16.1 The Translation Intuition

**Core idea of TransE (Bordes et al., 2013):** A relation r should act as a **translation vector** from head entity h to tail entity t in embedding space:

```
h + r ≈ t   if (h, r, t) is a true triple
```

**Visual analogy:** Think of navigation. If you're at "Paris" and you add the vector "capital_of_inverse", you arrive at "France." The relation vector is like a direction in geographic space.

## 16.2 TransE

### What it can model:

- **Anti-symmetric relations:** ✅ The relation vector points in a specific direction, not both ways
- **Composition relations:** ✅ r1 + r2 ≈ r3
- **Inverse relations:** ✅ r1 = -r2

### What it CANNOT model:

- **Symmetric relations:** ❌ If h + r ≈ t AND t + r ≈ h, then r ≈ 0, which forces h = t
- **1-to-N relations:** ❌ If Amazon employs Mary AND Tom, then h + r ≈ t₁ AND h + r ≈ t₂, forcing t₁ = t₂ (different employees forced to same embedding)

### Score Function:

```
f(h, r, t) = -||h + r - t||_L1/2
```

(Higher score = more likely to be a true triple; the negative distance is used since we want small distances for true triples)

### Training (Hinge Loss):

```
L = Σ_{(h,r,t)∈S} Σ_{(h',r,t')∈S'} max(0, γ + f(h,r,t) - f(h',r,t'))
```

Where S = positive (true) triples, S' = negative (corrupted) triples, γ = margin.

## 16.3 TransH

**Problem with TransE:** Every entity has a single embedding regardless of which relation it's involved in.

**TransH Fix:** Project entity embeddings onto **relation-specific hyperplanes** before doing translation.

```
h_⊥ = h - (w_r · h) w_r    // project h onto the hyperplane with normal w_r
t_⊥ = t - (w_r · t) w_r    // project t onto the same hyperplane
h_⊥ + d_r ≈ t_⊥
```

**What this enables:** The same entity can have different representations in different relation contexts. Mary in the context of "sibling" relation is different from Mary in the context of "colleague" relation.

**Can model:**

- Symmetric relations ✅
- 1-to-N relations ✅
- But still limited for highly complex patterns

## 16.4 TransR

**Problem with TransH:** Entities and relations share the same embedding space.

**TransR Fix:** Have **separate embedding spaces** for entities (ℝᵈ) and relations (ℝᵏ). Project entity embeddings into the relation space using a **relation-specific projection matrix M_r** (d × k):

```
h_r = h · M_r       // project head into relation space
t_r = t · M_r       // project tail into relation space
h_r + r ≈ t_r
```

**Can model:** Symmetric ✅, Anti-symmetric ✅, 1-to-N ✅, Inverse ✅, Composition ✅

**Drawback:** Complexity is O(k·d) parameters per relation vs. O(d) for TransE → much larger model.

## 16.5 PTransE (Path-based TransE)

**Problem with TransE/TransH/TransR:** Only considers **direct (1-hop) relations** between entities.

**PTransE Fix:** Considers **multi-hop relation paths** connecting head to tail.

```
f_path(h, t) = Σ_{p ∈ P(h,t)} R(p|h,t) · f_p(h, t)
```

Where:

- P(h, t) = set of relation paths from h to t
- R(p|h, t) = reliability/importance score of path p
- f_p(h, t) = scoring function based on the path embedding

**Path embedding:** The embedding of a path (r1 → r2 → r3) is composed by addition (or other operators):

```
path_embedding = r1 + r2 + r3
```

**Can model:** Long-range dependencies, inference chains over multiple hops.

## 16.6 Comparison Table

|Model|Symmetric|Anti-sym|1:N|Inverse|Composition|Parameters|
|---|---|---|---|---|---|---|
|**TransE**|❌|✅|❌|✅|✅|O(d) per relation|
|**TransH**|✅|✅|✅|✅|Limited|O(d) per relation|
|**TransR**|✅|✅|✅|✅|✅|O(k·d) per relation|
|**PTransE**|—|—|—|—|✅✅|Higher|

## 16.7 Other Translation Models

|Model|Innovation|
|---|---|
|**TransD**|Simplifies TransR: decomposes projection matrix into product of two vectors → O(d) per relation|
|**KG2E**|Represents entities/relations as **Gaussian distributions** (captures uncertainty)|
|**TransG**|Relation as **mixture of Gaussians** (captures multiple semantic meanings of a relation)|

---

## 🧠 Q&A — Section 16

**Q1: Why can TransE model anti-symmetric but not symmetric relations?**

> In TransE, a relation r is a fixed translation vector pointing in one direction. For anti-symmetric (A, r, B) → ¬(B, r, A): adding r to A points towards B, but adding r to B points in the same direction, away from A. This naturally prevents the symmetric case. For symmetric relations, we'd need r such that h + r ≈ t AND t + r ≈ h, which forces r ≈ 0 and h ≈ t, collapsing all entities in that relation to the same point.

**Q2: Explain why TransE cannot model 1-to-N relations and how TransH addresses this.**

> In TransE, h + r ≈ t means each head-relation pair maps to a unique point in space. If Amazon employs both Mary and Tom, we need Amazon + employs ≈ Mary AND Amazon + employs ≈ Tom, forcing Mary ≈ Tom (same embedding). TransH fixes this by projecting entities onto relation-specific hyperplanes before translation. Different employees can have different projections onto the "employs" hyperplane, allowing them to be distinct while still satisfying the translation constraint.

**Q3: What is the trade-off between TransE and TransR?**

> TransE uses a single embedding space for both entities and relations (O(d) parameters per relation) — simple and efficient but limited in expressivity. TransR uses separate embedding spaces (O(k·d) parameters per relation) — more expressive (can model all 5 relation pattern types) but significantly more parameters, higher computational cost, and harder to train at scale.

**Q4: What is the key advantage of PTransE over TransE?**

> PTransE considers multi-hop relation paths between entities, not just direct 1-hop relations. This allows the model to capture indirect relationships and perform multi-step inference (e.g., "born_in" + "located_in" = "is_from"). It assigns reliability scores to different paths, weighting more informative paths higher. This makes PTransE significantly better at capturing long-range dependencies in the KG.

**Q5: How does the hinge loss (margin-based loss) work in TransE training?**

> The hinge loss is: L = max(0, γ + f(h,r,t) - f(h',r,t')) where γ is a margin, f(h,r,t) is the score for a true triple (negative distance, so more negative = worse), and f(h',r,t') is the score for a corrupted triple. The loss is 0 when true triples score higher than corrupted ones by at least γ. If the margin is not satisfied, the loss is positive and gradients push true triples to higher scores and corrupted triples to lower scores.

---

# 17. Tensor Factorization & Neural KGE Models

## 17.1 Tensor Factorization Overview

The entire KG can be represented as a **3-dimensional tensor** 𝒳 ∈ {0,1}^{|E| × |R| × |E|}:

- Dimension 1: head entities
- Dimension 2: relations
- Dimension 3: tail entities
- 𝒳[i, k, j] = 1 if (eᵢ, rₖ, eⱼ) is a true triple

**Goal:** Decompose this large, sparse tensor into smaller, dense factor matrices (embeddings) that capture latent semantic structure.

## 17.2 RESCAL

**RESCAL** is the most general tensor factorization approach:

```
Score(h, r, t) = hᵀ · Mᵣ · t
```

Where:

- **h, t** = d-dimensional entity embedding vectors
- **Mᵣ** = d × d **full rank matrix** for relation r (captures all pairwise interactions between entity embedding dimensions)

This is a **bilinear model**: f(x, y) = xᵀ V y (outer product formulation).

**Advantage:** Maximum expressivity (full-rank matrix) **Disadvantage:** O(d²) parameters per relation → huge parameter count for many relations

## 17.3 DistMult

**DistMult** simplifies RESCAL by constraining M_r to be **diagonal**:

```
Score(h, r, t) = hᵀ · diag(r) · t = Σᵢ hᵢ · rᵢ · tᵢ
```

**Advantage:** O(d) parameters per relation — much more efficient **Disadvantage:** Diagonal matrix can only capture symmetric relations (hᵀ diag(r) t = tᵀ diag(r) h), so it **cannot model anti-symmetric relations**

## 17.4 ComplEx

**ComplEx** extends DistMult to **complex-valued embeddings** (ℂᵈ instead of ℝᵈ):

```
Score(h, r, t) = Re(hᵀ · diag(r) · t̄)     // t̄ = complex conjugate of t
```

**Key insight:** Complex numbers allow modelling of **asymmetry**: Re(h · r · t̄) ≠ Re(t · r · h̄) in general, so anti-symmetric relations CAN be modelled.

**Can model:** Symmetric, Anti-symmetric, Inverse relations ✅

**Advantage:** O(d) parameters per relation, handles asymmetry **Disadvantage:** Slightly more complex to implement than DistMult

## 17.5 Neural Network-Based: ConvKB

**ConvKB** applies a **convolutional neural network** to triples:

1. Represent triple (h, r, t) as a **3-column matrix** (each column is one of h, r, t ∈ ℝᵈ)
2. Apply **convolutional filters** across this matrix → multiple feature maps
3. **Concatenate** feature maps into a single vector
4. **Dot product** with a weight vector → scalar score
5. Score used for true/false triple prediction

**Key advantage:** Convolutional filters explore **global relationships** across the same-dimensional entries of h, r, and t — captures transitional characteristics more expressively than simple geometric operations.

## 17.6 Summary Comparison

|Model|Representation|Score Function|Symmetric|Anti-sym|1:N|Parameters/relation|
|---|---|---|---|---|---|---|
|RESCAL|ℝᵈ|hᵀMᵣt|✅|✅|✅|O(d²)|
|DistMult|ℝᵈ|Σ hᵢrᵢtᵢ|✅|❌|✅|O(d)|
|ComplEx|ℂᵈ|Re(hᵀdiag(r)t̄)|✅|✅|✅|O(d)|
|ConvKB|ℝᵈ|CNN(h,r,t)|✅|✅|✅|O(filter_size × channels)|

---

## 🧠 Q&A — Section 17

**Q1: What is the key difference between RESCAL and DistMult?**

> RESCAL uses a full d×d matrix M_r per relation, capturing all pairwise interactions between entity embedding dimensions. This is highly expressive but requires O(d²) parameters per relation. DistMult uses a diagonal matrix (just a d-dimensional vector r), reducing parameters to O(d) per relation but limiting expressivity — it can only model symmetric interactions.

**Q2: Why can't DistMult model anti-symmetric relations?**

> The score in DistMult is Score(h, r, t) = Σᵢ hᵢrᵢtᵢ = Score(t, r, h) (the formula is symmetric in h and t). This means DistMult scores (h, r, t) and (t, r, h) equally — it has no way to say one is true and the other is false, which is exactly what anti-symmetric relations require.

**Q3: How does ComplEx solve DistMult's limitation?**

> ComplEx uses complex-valued embeddings. The score Re(hᵀdiag(r)t̄) is NOT symmetric in h and t (taking the complex conjugate of t breaks the symmetry). This allows ComplEx to assign different scores to (h, r, t) and (t, r, h), enabling modelling of anti-symmetric relations, while still using only O(d) parameters per relation.

**Q4: Why does ConvKB use a convolutional approach rather than a simple geometric one like TransE?**

> Simple geometric models (TransE) use a single global operation (addition) to relate h, r, t. This misses fine-grained, dimension-level interactions. Convolutional filters in ConvKB examine patterns across the same-dimensional entries of h, r, t simultaneously, capturing global relationships that geometric models miss. CNNs also have the advantage of automatic feature extraction.

---

# 18. Description-Based KGE (DKRL)

## 18.1 Motivation

Traditional KGE models use only the **triple structure** (h, r, t). But entities in real KGs often have **rich textual descriptions** (Wikipedia articles, product descriptions, scientific abstracts).

**DKRL (Description-Embodied Knowledge Representation Learning)** leverages these textual descriptions to build richer entity embeddings.

## 18.2 Two Types of Representations

**DKRL learns TWO representations for each entity:**

1. **Structure-based representation (hₛ, tₛ):** Learned from the triple structure, just like TransE
2. **Description-based representation (h_d, t_d):** Learned by encoding the entity's textual description

The final scoring function considers all combinations: (hₛ, r, tₛ), (hₛ, r, t_d), (h_d, r, tₛ), (h_d, r, t_d).

## 18.3 Two Encoders for Textual Descriptions

### CBOW Encoder (Continuous Bag of Words)

- Average word embeddings: h_d = (1/|D|) Σ_{w∈D} v_w
- Fast, simple, but loses word order information

### CNN Encoder

- Apply convolutional filters over word embeddings → capture local patterns (phrases)
- Max-pooling → capture the most salient features
- Richer, more expressive than CBOW
- Better at capturing semantic patterns and contextual dependencies

## 18.4 Training

Margin-based ranking loss that considers all 4 combinations of structure-based and description-based representations. Goal: positive triples score higher than corrupted triples.

## 18.5 Advantages and Disadvantages

**Advantages:**

- Significantly improves performance for entities with rich descriptions
- Enables **zero-shot inference** for new entities (as long as they have textual descriptions)
- Combines structural and semantic information

**Disadvantages:**

- Higher computational complexity (need to encode text for every entity)
- Requires access to textual descriptions (not always available)

---

## 🧠 Q&A — Section 18

**Q1: What is the key advantage of DKRL over TransE?**

> DKRL can leverage the rich semantic information in entity descriptions, which TransE ignores. This allows DKRL to build more informative embeddings (especially for entities with limited structural information in the KG), and enables zero-shot inference for completely new entities that have text descriptions but no existing triples.

**Q2: Why does DKRL use a CNN encoder rather than just averaging word embeddings (CBOW)?**

> CBOW averages all word embeddings, losing word order and phrase structure. A CNN captures LOCAL PATTERNS (phrases like "Nobel Prize winner" or "side effect") through convolutional filters, then takes the most salient features through max-pooling. This captures the hierarchical structure and contextual dependencies of the text, resulting in richer, more informative entity embeddings.

---

# 19. Loss Functions & Evaluation Metrics for KGE

## 19.1 Negative Sampling in KGE

KGs only contain **positive** triples (known true facts). We must generate **negative triples** (likely false facts) for training.

**Method:** Corrupt a true triple (h, r, t) by replacing h or t with a randomly sampled entity:

- (h', r, t): corrupt the head → likely a false triple
- (h, r, t'): corrupt the tail → likely a false triple

**Intuition:** Most random entity substitutions will produce false triples (since most entity pairs are not connected by most relations).

## 19.2 Pointwise Loss Functions

Evaluate each triple **independently** against a static label.

### Pointwise SE (Squared Error) Loss

```
L = Σ (f(h,r,t) - l(h,r,t))²
```

Where l(h,r,t) = 1 for positive triples, 0 for negative.

- No configurable hyperparameters (nice property)
- Used in **RESCAL**

### Pointwise Hinge Loss

```
L = Σ max(0, λ - l·f(h,r,t))
```

- λ = configurable margin
- Used in **HolE**

### Pointwise Logistic Loss

```
L = Σ log(1 + exp(-l·f(h,r,t)))
```

- Smooth version of hinge loss, no margin parameter
- Used in **ComplEx**

## 19.3 Pairwise Loss Functions

Optimise the **relative ordering** of true vs. corrupted triples.

### Pairwise Hinge Loss

```
L = Σ_{(h,r,t)∈S+} Σ_{(h',r,t')∈S-} max(0, γ + f(h',r,t') - f(h,r,t))
```

- Maximises the difference between scores of true and false triples by margin γ
- Used in **TransE, DistMult**

**Intuition:** "True triples should score higher than corrupted triples by at least γ."

### Comparison

||**Pointwise**|**Pairwise**|
|---|---|---|
|**Focus**|Absolute score per triple|Relative ordering of true vs. false|
|**Analogy**|Binary classification (SVM)|Ranking (information retrieval)|
|**Best for**|Classification tasks|Ranking/recommendation tasks|

## 19.4 Evaluation Metrics for KG Completion

### Rank-Based Metrics

**Setting:** For each test triple (h, r, t), replace t with every possible entity e ∈ E and score all (h, r, e). Rank all entities by score and find where t appears.

#### Mean Rank (MR)

```
MR = (1/|T|) Σ rank_i
```

- Average rank of the correct entity across all test triples
- **Lower is better**
- Sensitive to outliers (one very bad prediction can heavily affect MR)

#### Mean Reciprocal Rank (MRR)

```
MRR = (1/|T|) Σ (1/rank_i)
```

- Average of the reciprocal of ranks
- **Higher is better** (maximum = 1.0)
- More robust than MR: gives less weight to predictions ranked very poorly
- If the correct entity is rank 1: contributes 1.0; rank 2: 0.5; rank 10: 0.1; rank 1000: 0.001

#### Hits@K

```
Hits@K = (1/|T|) Σ 𝟙[rank_i ≤ K]
```

- Fraction of test triples where the correct entity appears in the top K predictions
- Common values: K = 1, 3, 10
- **Higher is better** (maximum = 1.0)
- Hits@1 is the strictest (correct entity must be the top prediction)

### Classification-Based Metrics

- **Precision:** Fraction of predicted triples that are actually true
- **Recall:** Fraction of true triples that are correctly predicted
- **F1 score:** Harmonic mean of precision and recall

## 19.5 KGE Libraries

Key libraries for practical KGE implementation:

- **PyKEEN** — comprehensive KGE library with many models
- **AmpliGraph** — industrial-strength KGE library
- **DGL-KE** — GPU-accelerated KGE (by AWS/DGL)
- **OpenKE** — open-source KGE framework

---

## 🧠 Q&A — Section 19

**Q1: Why do we need negative sampling in KGE training?**

> KGs only contain positive facts (known true triples). A model trained only on positives would trivially learn to score everything highly. We need negative examples (corrupted triples) to teach the model to distinguish true from false facts. Without negatives, there's no contrastive signal for learning.

**Q2: What is the difference between MR and MRR as evaluation metrics?**

> MR (Mean Rank) is the average position of the correct entity in the ranked list — lower is better. MRR (Mean Reciprocal Rank) is the average of 1/rank — higher is better. MRR is preferred because it's more robust to outliers and gives more credit for high-ranked correct predictions. A model with MR=50 but one extremely bad prediction (rank 10,000) looks worse in MR than in MRR.

**Q3: When would you prefer Hits@1 over Hits@10 for evaluation?**

> Use Hits@1 when the application requires the TOP prediction to be correct (e.g., a question answering system that can only give one answer). Use Hits@10 when it's acceptable to present a ranked list and the correct answer just needs to be in the list (e.g., a recommendation system that shows 10 suggestions).

**Q4: What is the key difference between pointwise and pairwise loss functions in KGE?**

> Pointwise loss evaluates each triple independently and tries to predict an absolute score (1 for true, 0 for false) — similar to binary classification. Pairwise loss evaluates pairs of (positive, negative) triples and only requires the positive triple to score HIGHER than the negative by some margin — similar to learning-to-rank. Pairwise loss is generally preferred for KGE as it directly optimises the ranking objective used in evaluation (Hits@K, MRR).

**Q5: If a model achieves MRR = 0.4 and another achieves MRR = 0.35, what does this tell you?**

> The first model is better: on average, its correct predictions rank higher. MRR=0.4 means the average reciprocal rank is 0.4, corresponding to roughly an average rank of 2-3. MRR=0.35 corresponds to roughly rank 3. Both are reasonable models, but the first consistently ranks the correct entity slightly higher in the sorted list.

---

# 📎 Quick Reference Summary

## Embedding Methods Hierarchy

```
Graph Embedding Methods
├── Shallow (Transductive, no node features)
│   ├── Matrix Factorization based (Laplacian Eigenmap, GF, GraRep, HOPE)
│   └── Random Walk based
│       ├── DeepWalk (unbiased walk + SkipGram)
│       └── Node2Vec (biased walk with p, q + SkipGram)
│
└── Deep / GNN-based (Inductive, uses node features)
    ├── GCN (Graph Convolutional Network — spectral)
    ├── GraphSAGE (inductive, samples fixed neighbourhood)
    └── GAT (Graph Attention Network)
```

## KGE Methods Hierarchy

```
KG Embedding Methods
├── Triplet-based
│   ├── Translational: TransE, TransH, TransR, TransD, KG2E, TransG, PTransE
│   ├── Tensor Factorization: RESCAL, DistMult, ComplEx, HolE
│   └── Neural Network: ConvKB, ConvE
│
└── Description-based
    └── DKRL (uses textual descriptions)
```

## Parameter Cheat Sheet for Node2Vec

|Parameter|Name|Effect|Embedding Captures|
|---|---|---|---|
|Low p|Return parameter|Encourages returning to previous node|Structural equivalence (BFS-like)|
|High p|Return parameter|Discourages returning|Community-level homophily|
|Low q|In-out parameter|Explores outward|Homophily (DFS-like)|
|High q|In-out parameter|Stays local|Structural equivalence|

---

_These notes were compiled from UE23AM342BA1 — Interdisciplinary Deep Learning on Graphs, Dr. Bhaskarjyoti Das, Department of Computer Science Engineering (AI & ML), with additional depth from Stanford CS224W and supplementary resources._