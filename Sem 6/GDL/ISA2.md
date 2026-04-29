# Interdisciplinary Deep Learning on Graphs

### UE23AM342BA1 — Complete Exam Notes

**Dr. Bhaskarjyoti Das | Department of CS in AI & ML**

---

# TABLE OF CONTENTS

1. GNN in Recommender Systems
2. GNN in Traffic Prediction and Anomaly Detection
3. GNN in Cyber Security
4. Fake News Detection with GNN
5. GNN in Molecular Chemistry, Healthcare, and Bioinformatics
6. QA, KBQA, Graph2Seq, VQA, and Action Recognition
7. Adversarial Attack and Defense on GNN
8. Software Program Analysis with GNN
9. GNN and Large Language Models (LLM)

---

# UNIT 1: GNN IN RECOMMENDER SYSTEMS

---

## 1.1 Recommender Systems — Basics

### What is a Recommender System?

Formally, a recommender system takes:

- A set of **users** `U`
- A set of **items** `I`
- Learns a function `f : U × I → R` (a predicted rating/preference score)

The **User-Item Matrix** has most cells empty (unrated). The recommender system's job is to **predict the values of those empty cells**.

### Two Core Formulations

**1. Prediction Version (Matrix Completion)**

- Given `m` users and `n` items, the data is an incomplete `m × n` matrix.
- Goal: Fill in missing entries (predict ratings for un-rated items).
- This is called the **matrix completion problem**.

**2. Ranking Version (Top-K Recommendation)**

- Goal: Recommend the **top-K items** for a given user, or top-K users for a given item.
- This is the **top-K recommendation problem**.
- The 2nd formulation can be derived from the 1st formulation (rank by predicted score).

---

### Types of Ratings

|Rating Type|Description|Example|
|---|---|---|
|Continuous|Scale like [-10, 10]; burdensome for users|Jester Joke Recommender|
|Interval-based|Numerical, equidistant values|Scale of -5 to +5|
|Ordinal|Categorical values mapped to interval; no "neutral" → forced choice|Star ratings|
|Binary|Forced choice, no neutral|Pandora Radio (thumbs up/down)|
|Unary|Only positive response allowed|Facebook Like, News Click|
|Explicit/Implicit|Explicit = direct input; Implicit = derived from web activity|Clicks, dwell time|

---

### The Long Tail Property

- Real-world item ratings follow a **power law distribution**.
- **Few items are rated very frequently; most items are rated rarely** (the long tail).
- High-frequency items = volume-sale, low-margin (e.g., bestsellers on Amazon).
- Amazon's real profit comes from **low-frequency (long-tail) items**.
- Long-tail items are harder to provide robust ratings for.
- The consequence: **more empty spaces in the Utility Matrix** → Collaborative Filtering becomes less robust.

> **Key Insight:** The long-tail property is a primary motivation for why graph-based methods are needed — they can handle sparsity better.

---

## 1.2 Recommender Systems — Types

### 1. Collaborative Filtering (CF)

Based on the principle: "Users who agreed in the past tend to agree in the future."

Two sub-categories:

**a) Memory-Based (Neighborhood-Based) CF:** Predict ratings based on neighborhood relationships.

- **User-based CF:** Find like-minded users of target user A. Use their ratings to recommend items for A.
- **Item-based CF:** For target item B by user A, find set S of items most similar to B. Use ratings of those items by A to predict rating for B.

**b) Model-Based CF:** Machine learning/data mining methods are used to build a predictive model. Model parameters are learned via an optimization framework (e.g., matrix factorization).

---

### 2. Content-Based Recommender Systems

- Uses **descriptive attributes of items** (content/keywords) to make recommendations.
- Combines item content with user ratings and buying behavior.
- **Advantage:** Works well for new items (cold-start on items) since content is always available.
- **Disadvantage:**
    - Provides **obvious recommendations** (always recommends the same type).
    - Not effective for **new users** (cold-start on users problem).

---

### 3. Knowledge-Based Recommender Systems

- Useful for **rarely purchased, expensive, or complex items**: real estate, automobiles, financial services, luxury goods.
- Sufficient ratings often unavailable; relies on user-provided constraints.

Two types:

- **Constraint-based systems:** Users specify requirements/constraints (e.g., price range, year). System also uses rules like "Older investors don't invest in ultra-high-risk products."
- **Case-based systems:** User specifies specific cases as targets/anchors. Similarity metrics on item attributes retrieve similar items.

---

### 4. Utility-Based Recommender Systems

- Defines a **utility function** on product features to compute the probability of a user liking an item.
- Central challenge: Defining an appropriate utility function for each user.
- All recommender schemes implicitly rank items by perceived utility.

---

### 5. Hybrid and Ensemble-Based Recommender Systems

- Combines multiple recommendation methods.
- Motivation: Each type has strengths in different data conditions.
    - Knowledge-based: effective in **cold-start** (low data).
    - Collaborative filtering: effective when **lots of data** is available.
- Examples: Weighted hybrid, switching hybrid, cascade hybrid.

---

### 6. Context-Based (Context-Aware) Recommender Systems

- Incorporates **contextual information** into recommendations.
- Contexts include: **time, location, social data**.
- Example: Clothing recommendations depend on both season and customer location.
- Incredibly powerful because underlying ideas apply across many domains.

---

### 7. Time-Sensitive Recommender Systems

- Recommendations evolve over time (e.g., movie reviews change years after release).
- Temporal aspects:
    1. **Item rating evolves** as community attitudes change.
    2. **Rating depends on time of day, day of week, season.**

---

### 8. Location-Based Recommender Systems

- Driven by GPS-enabled mobile phones.
- Two types of spatial locality:
    1. **Preference locality (user-specific):** User's geographical location influences preferences.
    2. **Travel locality (item-specific):** Location of an item (e.g., restaurant) affects relevance based on user's current location.

---

### 9. Social Recommender Systems

- Based on **network structures, social cues, and tags**.
- Types:
    - Based purely on **structural aspects**: suggest nodes and links within the network itself.
    - Based on **social cues**: recommend various products using social signals.

---

## 1.3 Graph-Based Approach to Recommender Systems

### Why Graph-Based Answers to the Sparsity Problem?

Sparsity in the rating matrix is a major challenge in neighbor-based methods. Graph-based approaches circumvent this because:

- Two users **do not need to have rated many of the same items** to be considered neighbors — as long as many short paths exist between them.
- This allows **indirect connectivity** to build neighborhoods.

### Types of Graphs in RS

|Graph Type|Description|
|---|---|
|User–User graph|Direct similarity between users|
|User–Item graph|Bipartite graph of interactions|
|Item–Item graph|Similarity between items|

### Algorithms Used

- **Random Walk / PageRank** — used to compute top-K neighborhoods of an item in a directed correlation graph.
- **Shortest path-based approaches**

### Opportunity: Link Prediction

- The recommendation problem can be **mapped to a link prediction problem on a graph**.
- E.g., predicting whether an edge (user → item) will form.

---

### Indirect Neighbourhood via User-Item Graph

- The user-item graph is **bipartite**.
- A **2-hop user-user graph** can be derived from it.
- Edges are more informative because they capture the number and similarity of common items between two users.

**Example:** If none of John's immediate neighbors have rated _Terminator_, direct CF fails. But **structural transitivity** lets us check John's indirect neighbors. This gives better coverage.

---

### Rating Matrix and Correlation Graph with PageRank

- Once a directed correlation graph is built from the rating matrix, one can use **PageRank** (based on Random Walk) to identify the top-K neighbourhood of an item.
- Other variants of PageRank can also be used.

---

### Collaborative Filtering from a Graph Perspective: Encoder-Decoder Model

Collaborative filtering can be viewed as an **encoder-decoder model**:

|Component|Role|
|---|---|
|**Encoder**|Encodes each user/item into a vector representation (embedding)|
|**Decoder**|Reconstructs historical interactions from embeddings|

**Decoder Tasks:**

- **Regression Task:** Predict ratings (continuous output).
- **Binary Classification Task:** Predict whether an interaction exists (0 or 1).

### GNN in CF

- GNNs are used to **update (refine) representations** for users and items.
- **Spatial Graph Filtering:** Updates a user's/item's representation based on neighbors' information.
- Example: For user `u_i`, the representation is updated using all items `I_j` that `u_i` has interacted with.

---

## 1.4 Open Research Areas in GNN-Based Recommender Systems

### 1. Dynamic GNN

- Existing GNN-based RS models are almost entirely based on **static graphs**.
- In reality, recommender systems are dynamic:
    - Sequential/session-based recommendation collects data dynamically.
    - Modeling **dynamic user preferences** is a critical challenge.
    - New users, new products, and new features arrive continuously.
- **Future direction:** Dynamic GNN-based recommendation models.

---

### 2. Self-Supervised GNN

- Direct supervision from interaction data is sparse relative to the graph scale.
- Usual supervised GNN recommendation may be ineffective.
- Solution: Include more supervision signals from the **graph structure itself** using self-supervision.

**Steps:**

1. **Construct User-Item Graph:** Define nodes (users, items) and edges (interactions).
2. **Generate Augmented Views:** Create perturbed graphs (masking, dropout) for contrastive learning.
3. **Train with Self-Supervision:** Apply GNN encoders (GCN, LightGCN) with contrastive loss to align views.
4. **Fine-tune for Recommendation:** Use learned representations for the main task.

---

### 3. Conversational GNN (Conversational Recommendation Systems — CRS)

- In existing RS, the system estimates preferences from **historical data only** — information asymmetry issue.
- **CRS paradigm:** Users interact with the system through conversations; new data is dynamically collected.
- Users explicitly convey demands or give positive/negative feedback on recommended items.
- Future: Combine GNN representation learning with preference learning in conversational recommendation.

---

### 4. Knowledge Graph (KG) Enhanced GNN

- KG describes **relations between items**, providing information beyond historical interactions.

**KG Structure:**

- **Entities (nodes):** Items `V`
    
- **Relations:** `R`
    
- **Edges:** `Ek` (e.g., `e = (v_i, r, v_j)`)
    
- Can also be extended to a **user knowledge graph**.
    
- GNN models incorporate KG information to learn richer item representations.
    
- Enhances recommendation quality, especially for **diversity and fairness**.
    

---

### 5. LLM-Based + GNN Recommender Systems

- LLM-based RS face limitations: **hallucinations**, lack of up-to-date and domain-specific knowledge.
- **RAG (Retrieval-Augmented Generation)** addresses this by leveraging external knowledge.
- **Problem with vanilla RAG:** Introduces noise, neglects structural relationships.
- **Solution:** Use a GNN-driven RAG — retrieve high-quality, up-to-date **structural information from the KG** to augment recommendations.

---

## Q&A — Recommender Systems

**Q1: What is the matrix completion problem in recommender systems?** A: Given `m` users and `n` items with a partially filled rating matrix, matrix completion aims to predict the missing rating values. Each row represents a user and each column represents an item.

**Q2: What is the long-tail property and why does it matter?** A: It's the power-law distribution of item ratings where few items are rated very frequently and most items are rated rarely. This causes more empty cells in the utility matrix, making collaborative filtering less robust because there are fewer co-rated items between users.

**Q3: What is the difference between user-based and item-based CF?** A: User-based CF finds users similar to the target user and uses their ratings. Item-based CF finds items similar to the target item and uses the target user's ratings of those similar items to predict preference for the target item.

**Q4: Why are knowledge-based systems useful?** A: For rarely purchased items (cars, real estate, luxury goods) where sufficient ratings are unavailable. They use user-specified constraints or case-based similarity to make recommendations without relying on historical ratings.

**Q5: How does a graph-based approach handle sparsity better than traditional CF?** A: Two users don't need to have rated the same items to be neighbors; short paths through the bipartite user-item graph establish indirect connectivity. This allows indirect neighbors to contribute to recommendations, greatly improving coverage.

**Q6: How is the recommendation problem reframed as a link prediction problem?** A: In the user-item bipartite graph, a recommendation is equivalent to predicting whether an edge (interaction) will form between a user node and an item node. GNNs trained for link prediction can then serve as recommender systems.

**Q7: In the encoder-decoder model for CF, what does the decoder do?** A: The decoder takes user and item embeddings from the encoder and reconstructs historical interactions — either as a regression (predict ratings) or binary classification (predict if interaction exists).

**Q8: What is self-supervised GNN in the context of recommender systems?** A: Instead of relying solely on sparse interaction labels, self-supervised GNNs create auxiliary tasks from the graph structure itself — e.g., contrastive learning with augmented graph views — to provide additional training signals and improve representation quality.

**Q9: What is the information asymmetry problem in traditional recommender systems?** A: The system can only estimate user preferences from historically collected behavior data; it doesn't know the user's current explicit intentions. CRS (Conversational RS) addresses this by letting users interact with the system in real time.

**Q10: Why does vanilla RAG fall short in KG-enhanced recommendation?** A: Vanilla RAG fetches text passages but ignores structural relationships in the knowledge graph, introducing noise. A GNN-driven RAG can retrieve structurally coherent subgraphs from the KG, providing higher-quality contextual information.

---

---

# UNIT 2: GNN IN TRAFFIC PREDICTION AND ANOMALY DETECTION

---

## 2.1 Background

- Analyzing and forecasting **dynamic traffic conditions** is critical for:
    - Planning and construction of new roads.
    - Transportation management in smart cities.
- Traffic flow data is treated as **time series**: includes traffic speed, volume, and density at multiple time steps.
- **Key insight:** Roads are NOT independent — they have spatial relations encoded in a **traffic network graph** (roads/road sections = nodes; spatial relations = edges).

### Combined Spatial + Temporal Modeling

|Aspect|Method Used|
|---|---|
|Spatial relations|Graph Neural Networks (GNN)|
|Temporal information|CNN, RNN, Transformers|

> **Core idea:** GNN handles the spatial/relational structure; sequential models handle the time dimension.

---

## 2.2 Traffic Forecasting: Data Formats

### 1. Time Series Data

- Oldest and most common format.
- Historical data points → predict future conditions.

**Sub-types:**

- **Univariate:** Only one traffic variable considered (e.g., traffic speed).
- **Multivariate:** Multiple variables considered simultaneously (e.g., traffic volume, speed, occupancy, weather).

**Prediction types:**

- **Single-step forecasting:** Predict one future data point.
- **Multi-step forecasting:** Predict multiple future data points.

**Limitation:** Does not capture spatial dependence of traffic activities.

---

### 2. Grid Data

- Traffic data is aggregated over **regularly divided regions** (grids) of the study area.
- Each grid = one region.
- Output: an **intensity map** displayed in image format.
- Useful when roads form a regular grid structure.

---

### 3. Graph Data

- Traffic data is aggregated at **specific locations/stations → nodes** in a traffic graph.
- **Node features:** Traffic flow, speed, etc.
- **Edges:** Road topological connections or spatial distances between nodes.
- **Single-step graph forecasting:** Historical graph data in a lookback window → predict next time step's graph.

This format is the most powerful as it captures both node attributes and network topology.

---

## 2.3 The Traffic Forecasting Problem

### Spatial Graph Filtering + Sequence Modeling Framework

Representations are refined **layer by layer**:

**Step 1: Spatial Graph Filtering**

- Aggregates information from a node's immediate neighbors.
- Applied to node representations at each time step.
- Captures spatial relations.

**Step 2: Sequence Modeling**

- The output of spatial filtering is a sequence.
- This sequence is fed to a sequence function to capture temporal relations.

**Formula:**

```
Output = Sequence( SpatialFilter(X, A) )
```

Where the output becomes input for the next spatial layer.

**Spatial filter choices:**

1. GCN filter
2. Attention mechanism (enhances graph filtering)

**Sequence model choices:**

1. 1-D Convolutional Neural Networks
2. GRU (Gated Recurrent Unit)
3. Transformer

---

## 2.4 Demand Forecasting vs. Lane Occupancy vs. Traffic Occupancy

- **Traffic occupancy** is often modeled as a **decision variable** rather than a continuous variable.
- Traffic occupancy can be detected efficiently with **computer vision methods** (CNNs, Transformers on video frames).
- **Demand prediction** and **anomaly detection** are typical use cases.

---

## 2.5 Datasets

### Most Famous Datasets

|Dataset|Description|
|---|---|
|METR-LA|Los Angeles traffic sensor data|
|PeMS|California Performance Measurement System|
|NYC Open Data|New York City traffic data|

### Why Develop New Datasets?

1. **Overfitting risk:** Deep learning models may overfit on small, old datasets compared to image or NLP corpora.
2. **Data drift:** Models trained on old datasets may fail on new data if traffic patterns have changed (data-shift problem).

---

## 2.6 Challenges in GNN Traffic Forecasting

1. **Black-box nature:** GNNs lack interpretability — predictions come without explanation.
2. **Anomalies/outliers:** Many anomalies are removed during preprocessing or absent from training data. When encountered at test time, model performance degrades significantly.
3. **Sparse/diverse graph structures:** Most studies focus on dense graphs (downtown, highways). Real-world city graphs may be sparse, with isolated nodes — this has received insufficient attention.

---

## 2.7 Research Opportunities

### 1. Traffic Simulation for Unseen Scenarios

Two approaches:

- **Model-driven (macroscopic/microscopic simulators):** Macroscopic tools focus on high-level flow/speed/density relationships; microscopic tools focus on individual vehicle behavior.
- **Data-driven:** Creates more training samples from existing data (no domain knowledge required).

### 2. New Learning Schemes

- **Transfer learning:** Transfer cross-city knowledge.
- **Meta-learning:** Helps address cold-start in new cities; useful for building new graph structures via structure-aware learning.
- **Federated learning:** Privacy-preserving distributed training.

### 3. GNN + Reinforcement Learning (RL)

- RL can be used for graph neural network structure search.
- RL itself is useful for traffic light control and autonomous driving.
- Significant research gap in applying RL to graph data.

### 4. Distributed Learning for Large-Scale Graphs

- When GNNs scale to larger city traffic graphs, **distributed training** is necessary.
- Improves training and runtime efficiency.

### 5. Joint Forecasting of Multimodal Data

- Joint prediction of multiple transport modes (e.g., taxi + bike) in dynamic settings.

---

## Q&A — Traffic Prediction

**Q1: Why can't traffic data be modeled as simple time series alone?** A: Time series ignores spatial dependence — roads interact with each other. Traffic conditions on one road affect neighboring roads. Graph data formats capture these spatial relationships explicitly via nodes (stations) and edges (connections/distances).

**Q2: What are the two main components of the GNN traffic forecasting framework?** A: (1) Spatial graph filtering — aggregates neighbor information to update node representations, capturing spatial relationships. (2) Sequence modeling (CNN/GRU/Transformer) — captures temporal evolution of node states over time steps.

**Q3: What is the data-shift problem in traffic datasets?** A: Traffic patterns from historical training data may differ from current patterns due to infrastructure changes (new roads, closures). Models trained on outdated data can fail significantly in production.

**Q4: What is the difference between model-driven and data-driven traffic simulation?** A: Model-driven uses macroscopic/microscopic traffic simulators based on known physics/equations of flow, speed, density. Data-driven creates more data samples from existing data without relying on domain knowledge.

**Q5: Why is GNN + RL a promising research direction for traffic?** A: GNNs model the relational/spatial structure of traffic networks, while RL makes sequential decisions with designed reward signals. Traffic light control and autonomous driving naturally benefit from this combination. However, applying RL specifically to graph data structures remains underexplored.

---

---

# UNIT 3: GNN IN CYBER SECURITY

---

## 3.1 Background

- Growing Internet use leads to new vulnerabilities daily.
- Cybersecurity data can be naturally modeled as **graphs**.
- GNNs facilitate cybersecurity tasks: spammer detection, fake news detection, fraud detection, intrusion detection, etc.

---

## 3.2 Cybersecurity: Definitions and Taxonomy

**Cybersecurity** is the collection of tools, policies, actions, and technologies to protect cyber assets.

**Cyber assets include:** users, network infrastructures, applications, systems, transmitted/stored information.

**Three pillars (CIA Triad):**

- **Confidentiality** — data access only by authorized parties.
- **Integrity** — data remains accurate and unaltered.
- **Availability** — systems remain accessible when needed.

### Two Main Cybersecurity Categories

```
Cybersecurity
├── Network Infrastructure Security
│   ├── Network Level (Botnet, Malicious Domains, Intrusion Detection)
│   └── System Level (Malware, System Vulnerability, Blockchain Security)
└── Application Security
    ├── Cognition Security (Fake News, Web Spam, Review Spam, Fake Accounts)
    └── Transaction Security (Financial Fraud, Underground Market)
```

**Most cybersecurity tasks are fundamentally anomaly detection** — detecting rare occurrences in samples.

---

## 3.3 Transaction Security

### Financial Fraud

Obtaining gains illegally by exploiting financial market rule vulnerabilities.

Four typical graph-modeled applications:

1. **Money laundering**
2. **Cash-out fraud**
3. **Loan default**
4. **Insurance fraud**

### Underground Market (Darknet)

- Illegal, untaxed, unregulated market on the **darknet**.
- Uses encrypted networks that hide IP addresses and untraceable cryptocurrency (e.g., Bitcoin).
- Facilitates illegal trades: drug trafficking, arms smuggling.

---

## 3.4 Cognition Security

|Task|Description|
|---|---|
|Web Spam|Misleading webpages to redirect users to compromised sites; spreads malware via drive-by download attacks|
|Fake News|Detecting false information on social media|
|Review Spam|Fraudulent reviews (individual or collective) to mislead buying decisions|
|Fake Accounts|Accounts created for fake likes/followers, political astroturfing, manipulating voting|

---

## 3.5 Network Security

### Intrusion Detection System (IDS)

- Proactively protects networks from illegal attacks.
- Collects/analyzes system/network information.
- Detects attempts to destroy integrity, confidentiality, and availability.

**Detection Methods:**

- **Signature-based:** Uses pre-defined patterns of known attacks. Effective against known threats but misses new ones.
- **Anomaly-based:** Establishes baseline of normal behavior; flags deviations. Can detect new threats but may generate false alarms.

**Network-IDS vs. Host-IDS:**

- **Network-IDS:** Monitors network traffic.
- **Host-IDS:** Monitors applications and processes on a single host (system calls, file activity, logs).

### Malicious Domains

- DNS maps domain names to IP addresses.
- Criminals exploit domains for phishing, spam, botnets.
- Detecting malicious domains = important network security task.

### Botnets

- An infected network of compromised devices (bots) controlled by a botmaster.
- **Three stages:**
    1. Criminals transmit malware to compromised devices.
    2. Botmasters supervise bots via **Command and Control (C&C) channels**.
    3. Bots launch **Distributed Denial of Service (DDoS)** and other attacks.

---

## 3.6 System Security

|Threat|Description|
|---|---|
|Malware|Malicious software: viruses, worms, trojans, ransomware, rootkits, spyware, bots|
|System Vulnerability|Defects in software/OS design exploited to implant trojans/viruses and control systems|
|Blockchain Security|Smart contracts on Ethereum have vulnerabilities due to newly emerged programming languages; exploit can compromise digital assets|

---

## 3.7 Malicious Account Detection — Case Study

### Context

- Cyber attackers create malicious accounts on email, social networks, e-commerce platforms.
- They propagate spam and can cause financial loss.

### Two Key Observations

1. **Malicious accounts from the same attacker share devices** (limited attacker resources → same IP, MAC, phone number).
2. **Malicious accounts behave in batches** — sign up or log in in a burst of time.

### Graph Construction

- **Two types of nodes:** accounts, devices (IP, MAC, phone numbers, etc.).
- **Edges:** An edge exists between an account and a device if the account has activity (signup/login) on that device.
- **Graph:** `G = (V, E)` where `V` = accounts + devices, `E` = account-device interactions.
- **Adjacency matrix A** represents relations.

**Visualization from paper:** A subgraph of 210 vertices: 20 normal accounts (blue), 7 malicious accounts (yellow), rest are 6 types of devices. Edge thickness = attention coefficients.

### The GNN Model

A dedicated graph filter:

```
Z = GNNFilter(A, X)   [L layers of filtering]
ŷ = sigmoid(Z * W)    [binary classification output logits]
```

- `X` = input activity feature matrix for accounts.
- After `L` filtering layers, binary classification (malicious or not) is performed.
- Since malicious accounts from the same attacker connect to the same device nodes, **graph filtering enforces similar features** among them.
- The input features `X` capture activity patterns; graph structure captures shared device usage patterns.
- **Both patterns are captured jointly by the GNN.**

---

## 3.8 Intrusion Detection with GNN

### Why GNN for IDS?

Three advantages:

1. **Structural property of data:** Network traffic naturally forms graphs.
2. **Higher-order interaction:** GNNs capture complex multi-hop interactions between hosts.
3. **Behavioral supervisory signal:** GNNs mitigate the scarcity of attack instances by leveraging behavioral signals within the graph structure.

### Approaches

#### Node-Level Detection on Static Graph

- GCN learns node/edge representations.
- Classifier (FCN/MLP) assigns anomaly scores.
- **Top-K data points are marked as anomalies.**

#### Intrusion Detection on Dynamic Graph

For each graph snapshot:

1. **GCN** derives node embeddings from the temporal graph at discrete time intervals.
2. **GRU (Gated Recurrent Unit)** synthesizes the current hidden state by integrating node embeddings with preceding hidden states.
3. **Edge scoring function** allocates anomaly scores; top-K are flagged.

#### Graph-Level Detection on Static Graph

- Node features aggregated through GCN layers.
- **Global graph embedding** derived via pooling layer.
- **MLP or one-class classification** detects anomalous graphs.

#### Graph-Level Detection on Dynamic Graph

- **Node2Vec** computes the k-discriminative shingle `S_k^t` for each graph sketch.
- Anomalous graphs are detected using one-class classification.
- **A graph is anomalous if it exhibits a sudden change.**

---

## Q&A — Cyber Security

**Q1: What is the CIA Triad in cybersecurity?** A: Confidentiality (data accessible only to authorized parties), Integrity (data remains accurate and unaltered), and Availability (systems remain accessible when needed). Cybersecurity aims to ensure all three.

**Q2: What are the two major cybersecurity categories and their sub-divisions?** A: (1) Network Infrastructure Security, subdivided into Network level (botnet, malicious domains, IDS) and System level (malware, vulnerabilities, blockchain). (2) Application Security, subdivided into Cognition Security (fake news, web spam, review spam, fake accounts) and Transaction Security (financial fraud, underground market).

**Q3: How does the malicious account detection approach model the problem?** A: It builds a bipartite graph between accounts and devices (IP, MAC, phone numbers). An edge exists if an account used a device. The GNN is then used for binary semi-supervised classification to label each account as malicious or not. This captures the two key patterns: device sharing and batch behavior.

**Q4: Why does graph filtering cause malicious accounts to have similar feature representations?** A: Because malicious accounts from the same attacker are connected to the same set of device nodes. During graph filtering (message passing), nodes connected to similar neighbors receive similar aggregated messages, enforcing feature similarity.

**Q5: What is the difference between signature-based and anomaly-based IDS?** A: Signature-based uses known patterns of attacks and is effective only against known threats. Anomaly-based establishes a baseline of normal behavior and flags deviations — can detect new threats but may produce false positives.

**Q6: How does a GRU help in dynamic graph intrusion detection?** A: At each time step, the GCN produces node embeddings from the current graph snapshot. The GRU then combines these new embeddings with the previous hidden state (memory of past behavior) to capture temporal evolution, enabling detection of behavioral anomalies over time.

**Q7: What are the three stages of a botnet attack?** A: (1) Criminals transmit malware to compromised devices. (2) Criminals supervise botmasters who manipulate bots via C&C channels. (3) Bots launch distributed attacks (e.g., DDoS) on the network.

---

---

# UNIT 4: FAKE NEWS DETECTION WITH GNN

---

## 4.1 Background

- Online social media = major news source due to easy access and instant dissemination.
- Fake news leads to negative consequences, societal issues, and financial loss.
- Detecting fake news on platforms like Twitter is critical.

---

## 4.2 Approach: Every Post is a Graph

**Key insight:** Fake news has **different propagation patterns** from real news in social media.

- Model each **story/post as a graph** characterizing its diffusion process and social relations on Twitter.
- Task = **binary graph classification** (fake vs. real).
- GNNs are used to improve performance.

---

## 4.3 Fake News Detection as Graph Classification

### Problem Formulation

Given: A set of news posts `P = {p1, ..., pn}` with textual/visual content.

**Steps:**

1. **Pre-process content:** Extract key concepts/features. Map to:
    
    - Textual embeddings `Rt` (e.g., using GloVE)
    - Visual embeddings `Rv` (e.g., using VGG-19)
2. **Create the graph:**
    
    - Nodes `u ∈ V`: Key concepts/features.
    - Edges `e ∈ E`: Relations between concepts (e.g., co-occurrence in posts).
3. **Input matrix:** `X ∈ R^{n×d}` using embeddings `R = [Rt || Rv]` (concatenated).
    
4. **Adjacency matrix:** `A ∈ R^{n×n}` represents edges `E`.
    

### GCN Layers

Each Graph Convolutional (GC) layer:

```
Z^(j+1) = GCLayer(Z^(j), A)
```

- Takes feature matrix from previous layer and outputs a higher-level representation.
- Captures the effect of neighboring nodes (concepts).

### Final Steps

5. **Global mean pooling:** Aggregates all node representations into one post-level vector.
6. **Binary classifier:** Uses cross-entropy loss to distinguish fake vs. real news.

---

## 4.4 Rumor Detection with Bidirectional GCN on Rumor Tree

### Key Idea

- Rumors spread both **far (spread)** and **wide (dispersion)**.
- A **Bidirectional GCN** attempts to capture both directions: Top-Down (TD-GCN) and Bottom-Up (BU-GCN).

### Node Features

Node features in the rumor tree are enriched using:

- **Empath (Stanford, Ethan Fast et al.):** Captures 200 topic categories — affect, cognition, and others. Captures rumour characteristics well.

### Observations

- **Bidirectional GCN did NOT exactly beat TD-GCN**, because rumor dispersion was rather small in the dataset used.
- This shows dataset characteristics significantly impact model performance.

### Bidirectional GCN with Co-Attention

- Co-attention mechanism allows the model to jointly attend to both the rumor spread tree and the content.

---

## 4.5 Emerging Area: Continual Graph Learning for Fake News

### The Problem: Catastrophic Forgetting

- Each new event requires a different dataset as graph characteristics change.
- Incremental learning on new tasks causes **catastrophic forgetting** of old tasks.

### Continual Graph Learning (CGL)

- Aims to resolve catastrophic forgetting while adapting to newly emerged graph tasks.
- In 2022 (NeurIPS), the **CGL Benchmark** was made public.

### Two Popular Algorithms

#### 1. Gradient Episodic Memory (GEM)

- Stores a number of **episodic memory samples** from previous tasks.
- When learning new task `t`, GEM ensures the loss over stored memory **does not increase** compared to when task `t-1` was finished.
- Prevents forgetting by constraining gradient updates.

#### 2. Elastic Weight Consolidation (EWC)

- Loss function includes a **quadratic penalty term** on the change of parameters.
- Prevents drastic updates to parameters important to old tasks.

**EWC Loss:**

```
L_total = L_new_task + λ * Σ_i  F_i * (θ_i - θ*_i)^2
```

Where:

- `F` = **Fisher Information Matrix (FIM)** — measures how much information observable data carries about unknown model parameters. Defines the curvature of the log-likelihood function.
- `θ*_i` = optimal parameters from the previous task.
- `λ` = regularization strength.
- `F_i` identifies which parameters are critical (high curvature/information).
- **FIM protects important weights** — high FIM value → high penalty for changing that weight.

**Method:** Tasks on datasets D1 and D2 are two sequential tasks. GEM and EWC are applied when moving from D1 to D2.

---

## Q&A — Fake News Detection

**Q1: Why are graph-based models effective for fake news detection?** A: Fake news and real news have different propagation patterns in social networks. Modeling each post as a graph captures the structural diffusion pattern, allowing GNNs to use both content features and structural propagation characteristics to discriminate.

**Q2: What are the node features and edges in the fake news detection graph?** A: Nodes are key concepts/features extracted from the post, represented using text embeddings (GloVE) and visual embeddings (VGG-19). Edges represent relations between concepts (e.g., co-occurrence in posts). The adjacency matrix A encodes these edges.

**Q3: What is the role of global mean pooling in fake news classification?** A: After GCN layers produce node-level representations, global mean pooling aggregates all node representations into a single fixed-size vector representing the entire post/graph. This graph-level embedding is then fed to the binary classifier.

**Q4: Why does a Bidirectional GCN capture rumor characteristics better?** A: Rumors spread in two directions — top-down (spread: from root to leaves as it propagates) and bottom-up (dispersion: engagement and reactions). Bidirectional GCN captures both spreading and dispersion patterns simultaneously.

**Q5: What is catastrophic forgetting in the context of continual graph learning?** A: When a model is trained incrementally on new graph tasks (e.g., a new fake news dataset from a new event), it "forgets" its learned capabilities on old tasks. This is catastrophic forgetting — the model overwrites previous knowledge with new training.

**Q6: How does EWC prevent catastrophic forgetting?** A: EWC adds a quadratic penalty term to the loss function that penalizes large changes to parameters that were important (high Fisher Information) for previous tasks. The Fisher Information Matrix identifies which weights are critical, and changes to those weights are strongly penalized.

**Q7: What is the Fisher Information Matrix and why is it used in EWC?** A: The FIM measures how much information the training data provides about model parameters — it defines the curvature of the log-likelihood function. High curvature (high FIM) means a parameter is critical to model performance. EWC uses FIM as a per-parameter importance weight in its regularization penalty.

---

---

# UNIT 5: GNN IN MOLECULAR CHEMISTRY, HEALTHCARE, AND BIOINFORMATICS

---

## 5.1 Molecular Chemistry

### Basics of Molecular Modeling

|Concept|Description|
|---|---|
|Atoms|Nodes in the molecular graph|
|Bonds|Edges in the molecular graph|
|Covalent Bonds|Share electrons between atoms; both atoms keep shared electrons|
|Non-Covalent Bonds|Weaker electromagnetic interactions (H-bonds, salt bridges, pi-stacking); critical in drug design as most drug-biomolecule interactions use these|
|Molecular Conformations|How atoms are positioned relative to each other in 3D space|
|Chirality|Some molecules (including drugs) come in two mirror-image forms identical as graphs but different functionally; graph-only models CANNOT distinguish them|

### Applications

- **Predicting molecular properties** (e.g., solubility)
- **De novo molecular design:** "From-scratch" generation of novel chemical structures designed for specific biological targets or physio-chemical properties.

---

### SMILES (Simplified Molecular Input Line Entry System)

A compact, ASCII text format to represent chemical structures.

|Feature|Description|
|---|---|
|Atoms|Written with atomic symbols; H is usually implied|
|Single bonds|Implied or `-`|
|Double bonds|`=`|
|Triple bonds|`#`|
|Aromatic bonds|Lowercase letters|
|Branches|Inside parentheses: `CC(C)C` = isobutane|
|Rings|Numbered connection points: `C1CCCCC1` = cyclohexane|

- Multiple SMILES can represent the same molecule → **canonical SMILES** creates a unique, standardized string.

---

### RDKit

An open-source cheminformatics toolkit for handling molecular data.

|Feature|Description|
|---|---|
|Molecule Handling|Create, visualize, manipulate 2D and 3D structures from SMILES, SDF, Mol formats|
|Cheminformatics|Substructure matching, similarity searching (Tanimoto, Dice), reaction modeling|
|ML & Data Analysis|Generate molecular descriptors and fingerprints (Morgan/circular) for predictive models|
|Versatility|Core in C++, Python 3.x wrappers for data science pipelines|

---

### ECFP (Extended Connectivity Fingerprints)

- Also called **Morgan fingerprints**.
- Topological molecular descriptors representing molecular structures as **binary or count vectors**.
- Used for similarity searching, virtual screening, compound clustering.

**Mechanism:**

- Iteratively updates atomic identifiers based on neighbors (Morgan algorithm).
- Hashes atom neighborhoods to produce a **fixed-length binary vector** (e.g., 1024, 2048, 4096 bits).

**Limitation:** 2D topological — does NOT encode 3D conformational information.

---

### Molecule Representation Learning

**Traditional (non-differentiable) approach:**

1. Extract molecular fingerprint using off-the-shelf software.
2. Use fingerprint as input to a deep learning model.

- Problem: Fingerprint extraction is not guided by the downstream task → may not be optimal.

**Graph Deep Learning Approach:**

- A molecule = graph `G = {V, E}` where nodes = atoms, edges = bonds.
- These graph representations are called **molecular fingerprints** in context.
- Task = **graph classification or graph regression** (predict molecular property).
- GNN model = graph filtering layers + graph pooling layers.

---

## 5.2 Healthcare — Adverse Drug Reaction (ADR) from Clinical Data

### Background

- **ADR = Adverse Drug Reaction:** A significant public health concern worldwide.
- Graph-based methods are applied to biomedical graphs for predicting ADRs.
- Post-market surveillance ADR detection is as important as pre-marketing assessment.
- Data source: **SIDER (Side Effect Resource) Dataset**.

### Method

**Step 1: Node Embeddings for Drugs and Diseases**

- **Word2Vec SkipGram** on clinical dataset → embeddings for drug and disease nodes.
- Additional **one-hot vectors** describing the category (existing categorization).
- Both features combined → node embedding.

**Step 2: Edge Creation**

For homogeneous nodes (drug-drug or disease-disease):

```
Edge(u, v) = 1   if similarity(u, v) > θ (threshold)
```

Threshold θ controls sparsity of the graph.

For heterogeneous nodes (drug-disease):

```
Edge(drug, disease) = based on co-occurrence in clinical records
```

**Step 3: ADR Prediction**

- **Bilinear approach:** Models interactions between two inputs (drug embedding and disease embedding) using outer product.
- **Outer product of vectors u (m×1) and v (n×1) = UV^T (m×n).**
- Score matrix is generated, flattened, and normalized.
- Not a dot product — produces a full matrix capturing all pairwise interactions.

---

## 5.3 Healthcare — Rare Disease Prediction from Phenotypes

### The Problem

- Rare diseases are difficult to predict accurately.
- Limited EMR data per rare disease → insufficient to distinguish similar clinical symptoms.
- Existing methods rely on sequential EMRs → fail for new patients with no history.

### Human Phenotype Ontology (HPO)

- **Phenotype:** An individual's observable traits (height, eye color, blood type), determined by genotype + environment.
- **Ontology:** A data model representing concepts, attributes, and relationships as a **Directed Acyclic Graph (DAG)**.
- **HPO:** Covers all phenotypic abnormalities in human monogenic diseases.
- Both diseases and symptoms can be mapped to HPO nodes.

### Method

**Graph Creation:**

1. **EMR graph (patient-symptom):** Nodes = patients + symptoms; edges from EMR data.
2. **HPO graph (disease-symptom):** Nodes = diseases + symptoms from HPO knowledge base.

**GNN Processing:**

- GNN processes both graphs to produce:
    - **Patient embeddings** (from EMR graph)
    - **Disease embeddings** (from HPO graph)
- Measure closeness of embedding pairs → rare disease prediction.

---

## 5.4 Healthcare — RNA-Disease Association Prediction

### Biological Background

|Molecule|Role|
|---|---|
|DNA|Stores and transmits genetic information|
|RNA|Involved in protein synthesis and cellular processes; carries genetic info from DNA to ribosomes|
|Protein-Coding Gene (PCG)|Segment of DNA with instructions to build a specific protein|
|Non-Coding RNA (ncRNA)|RNAs that don't code for proteins but regulate gene expression|

**Types of non-coding RNA associated with disease:**

- **miRNA** (microRNA)
- **lncRNA** (long non-coding RNA)
- **circRNA** (circular RNA)
- **Piwi-interacting RNA**

**Associated diseases:** Cancer, neurodegenerative diseases, Alzheimer's, COVID-19, cardiovascular disease.

### Method

- Integrates heterogeneous data: RNA-disease associations, protein-coding gene-disease associations, gene interaction networks.
- GNN builds associations between RNA molecules and diseases.

**Applications:**

- **Disease-Gene Associations:** Known disease-related PCGs help identify risk lncRNAs (even with no known lncRNA-disease association).
- **Path Inference:** Models predict associations by finding pathways where an lncRNA regulates a PCG already known to cause a specific disease.

---

## 5.5 Bioinformatics — Drug Discovery

### The Scale of the Problem

- Drug discovery = a **difficult search problem** in an astronomically large space.
- Estimated **10^63 drug-like molecules** (vs. 10^24 grains of sand on Earth!).
- **~10^5 known human proteins** as potential targets.
- Industry can test **10^5 to 10^7 compounds per day**.
- To test all 10^63 compounds: would require **2.7 × 10^53 years** (universe is ~1.37 × 10^10 years old).

> **ML's primary value:** Automate drug discovery with computation, providing a feasible approach to this intractable search problem.

### Virtual Screening vs. De Novo Design

|Approach|Description|
|---|---|
|Virtual Screening (VS)|Predict and prioritize compounds as drug candidates by simulating target interactions. Reduces compounds needing experimental testing. **Chemical space → Functional space**|
|De Novo Design|Start with desired property criteria; use a generative model to generate structure. **Functional space → Chemical space**|

**ML Benefits in Drug Discovery:**

- Broaden the number of candidate molecules considered.
- Identify failures earlier (when they are inexpensive).
- Accelerate delivery of novel therapeutics to clinics.

---

## 5.6 Bioinformatics — Drug-Target Binding Affinity Prediction

### Background

- Drug development is time-consuming and costly.
- **Drug-Target Interaction (DTI)** identification is vital in early drug development to narrow the search space.
- Also used for **drug repurposing**: finding new targets for existing/abandoned drugs.
- Task: **Regression** — infer the strength of binding between a drug-target pair.

**Four target types:** Protein, disease, gene, side effect.

### Method

- **Drug:** Represented as molecular graph `Gd` (atoms = nodes, bonds = edges) → fed into GNN → **graph-level drug representation**.
- **Protein:** Represented as sequence of amino acids → fed into a **sequence model** → **protein representation**.
- **Combined representation:** Drug + protein representations are **concatenated**.
- **Prediction:** Combined representation fed to a regression layer for binding affinity score.

---

## 5.7 Bioinformatics — Polypharmacy Side Effect Prediction

### Background

- Many complex diseases require **polypharmacy** (combination of several drugs).
- Major risk: Drug-Drug Interactions (DDI) cause side effects.
- Task: Predict not just **if** a side effect exists, but **what type** it is.

**Key insight from exploratory analysis:**

- Co-prescribed drugs have more common target proteins than random drug pairs.
- → Drug-protein and protein-protein interactions are important for polypharmacy modeling.

### The Multi-Modal Graph

A graph `G = {V, E, R}` with:

- **Nodes `V`:** Drugs + proteins.
- **Edges `E`:** Each edge `e = (v_i, r, v_j)` with relation `r ∈ R`.
- **Three relation types `R`:**
    1. Protein-protein interactions
    2. Drug-protein target relationships
    3. Various types of drug-drug side effects

**Example:** Drug Doxycycline (D) and Drug Ciprofloxacin (C) are connected by relation r2 (bradycardia side effect) → taking them together likely causes bradycardia.

### Task: Multi-Relational Link Prediction

- Given a pair of drugs `{v_i, v_j}`, predict if edge `e_ij = (v_i, r, v_j)` of type `r ∈ R` exists.
- GNN graph filtering learns node representations → relational link prediction.

---

## 5.8 Bioinformatics — Protein-Protein Interaction (PPI)

### Basics

- **PPI network:** Proteins = nodes, interactions = edges.
- **Interactions:** Can be direct (physical contact) or indirect (via a third protein).
- **Interactome:** Complete set of PPIs in a cell/organism.

### Proteins and Residues

|Term|Definition|
|---|---|
|Protein|Chain of amino acids with biochemical functions|
|Amino Acid|Contains amine (-NH2), carboxyl (-COOH) groups, and a side chain (R-group)|
|Side chain (R-group)|Unique group that determines amino acid properties: size, charge, polarity, reactivity, and drives protein folding|
|Residue|An amino acid incorporated into a polypeptide chain (after losing a water molecule)|
|Interface|Two residues from different proteins are at the interface if any non-hydrogen atom in one is within **6 Å** of any non-hydrogen atom in the other|

**Protein folding rule:**

- Hydrophobic side chains → turn inward (away from water).
- Hydrophilic (polar/charged) side chains → face outward.

### Protein Interface Prediction

- **Task:** Predict whether a pair of amino acid residues (one from each protein) are in the protein interface.
- **Formulation:** Binary classification problem.

### Graph Modeling

- Protein = graph `G = {V, E}`.
- Nodes = amino acid residues.
- Edges = spatial relations (node `v_i` connected to k closest residues based on mean inter-atom distance).
- Node features = `x_i`; edge features = `e_ij`.
- Given ligand protein `Gl` and receptor protein `Gr`, classify each residue pair `(v_l, v_r)` as interface or not.

### PPI Datasets

|Dataset|Description|
|---|---|
|STRING (v11+)|Covers 5000+ organisms; subsets SHS27k, SHS148k widely used|
|HPRD (Human Protein Reference DB)|Human protein info: sequences, structures, functions, disease associations|
|DIP (Database of Interacting Proteins)|Curated by manual + algorithmic methods|
|Others|HIPPIE, Negatome, IntAct|

### Transfer Learning for Proteins

|Model|Description|
|---|---|
|SeqVec|Uses ELMo language model on protein sequences; models the "language of life" — principles underlying protein sequences|
|ProteinBERT|Inspired by BERT; pretrained on ~106M proteins from UniRef90; fine-tuneable in minutes; state-of-the-art on wide range of benchmarks; built on Keras/TensorFlow|

### PPI Case Study

- GNN models interactions in PPI network.
- Node features extracted using **ProteinBERT** and **SeqVec** pre-trained language models.
- Protein graphs built from **PDB (Protein Data Bank) files** (contain 3D atom coordinates).
- Two nodes connected if any atom pair is within **6 Å** threshold.
- Dataset used: **Pan's Human Dataset**.

---

## 5.9 Bioinformatics — MultiOmics

### The "Omes"

|Ome|What it contains|
|---|---|
|Proteome|All proteins in a cell/organism|
|Genome|All genes and DNA sequences|
|Transcriptome|All RNA molecules|
|Epigenome|All DNA/protein modifications affecting gene expression|
|Metabolome|All small molecules|

### Why MultiOmics?

- Each "ome" reveals a different layer of biology.
- Combined analysis reveals previously hidden facets.
- Example: A protein upregulated in cancer may only make sense when combined with metabolomics (how it changes cell state) and transcriptomics (how gene expression is affected).
- Better understanding → more options for diagnosis, treatment, and cure.

### GNN in MultiOmics

- GNNs model complex, nonlinear, heterogeneous relationships between biological entities (genes, proteins, miRNA, methylation sites, patients).

|Aspect|Description|
|---|---|
|Integration and Fusion|Combine multiple omics layers into a unified GNN framework|
|Applications|Cancer subtyping, tumor classification, prognosis prediction, identifying cancer driver genes, biomarker discovery|
|Interpretability|GNNExplainer and similar tools identify key biomarkers, genes, and functional pathways|

---

## Q&A — Molecular Chemistry, Healthcare, Bioinformatics

**Q1: Why can't a graph-only molecular model distinguish between chiral molecules?** A: Chiral molecules are mirror images of each other but have identical graph structures (same atoms and bonds). A model that only uses graph topology cannot differentiate them — it requires 3D spatial information (conformation).

**Q2: What is de novo molecular design?** A: It's the from-scratch generation of novel chemical structures using a generative model, starting from desired property criteria. The model maps from functional space (desired properties) to chemical space (molecule structure). Contrast with virtual screening, which maps from chemical space to functional space.

**Q3: Why is drug discovery described as an intractable problem?** A: There are an estimated 10^63 drug-like molecules and ~10^5 human protein targets. Even at 10^7 experiments per day, testing all compounds would take 2.7 × 10^53 years — vastly exceeding the age of the universe. ML is used to intelligently prioritize candidates.

**Q4: In drug-target binding affinity prediction, how are the drug and protein represented differently?** A: The drug is represented as a molecular graph (atoms = nodes, bonds = edges) processed by a GNN. The protein is represented as a sequence of amino acids processed by a sequence model. Their output representations are concatenated and fed to a regression layer.

**Q5: What is polypharmacy and why does it increase the risk of side effects?** A: Polypharmacy is using combinations of drugs to treat complex diseases. The risk increases because different drugs can interact with each other via shared protein targets — these Drug-Drug Interactions (DDI) can cause unexpected side effects. The task is to predict both the existence and type of these side effects.

**Q6: What is the 6 Å threshold in PPI modeling?** A: Two amino acid residues from different proteins are considered to be at the protein-protein interface if any non-hydrogen atom in one residue is within 6 Ångströms of any non-hydrogen atom in the other residue. This threshold defines the spatial closeness required for an interaction.

**Q7: What is ProteinBERT and how is it used in PPI tasks?** A: ProteinBERT is a protein language model pretrained on ~106M protein sequences from UniRef90, inspired by the BERT architecture. It generates rich protein embeddings that encode sequence context and properties. These embeddings serve as node features (residue representations) in GNN-based PPI models.

**Q8: What is the advantage of multiomics over single-omics analysis?** A: Each omics layer (genomics, transcriptomics, proteomics, etc.) captures only one aspect of cellular biology. Combining them reveals interactions that are invisible in single-omics analysis — e.g., understanding why a protein is elevated in cancer requires knowing how genes are expressed (transcriptomics), what metabolic changes occur (metabolomics), and what epigenetic modifications are present.

---

---

# UNIT 6: QA, KBQA, GRAPH2SEQ, IMAGE, VQA, AND ACTION RECOGNITION

---

## 6.1 Question Answering (QA) and GNN

### What is Machine Reading Comprehension / QA?

- **Goal:** Generate the correct answer for a given question by consuming and comprehending documents.
- GNNs enhance QA performance, especially for **multi-hop QA** — where reasoning across multiple documents is needed.

### Why GNN for Multi-Hop QA?

- Early multi-hop QA used **semantic parsing** (convert question to SPARQL query for knowledge bases).
- Multi-hop QA requires **reasoning paths**: start from topic entity → follow relations through intermediate entities → reach answer.

**GNN advantage:**

- GNN retains a state representing information from its neighborhood with **arbitrary depth**.
- In one-hop message passing, neighboring node information reaches the current node.
- After L hops of propagation, nodes outside L hops receive information from the reasoning path starting from the topic entity.
- **If the path and the question are highly matched → the node is very likely the answer.**

---

## 6.2 WikiHop Dataset and Multi-Hop QA

### Dataset Structure

Each sample is a tuple: `(q, Sq, Cq, a*)`

|Element|Description|
|---|---|
|`q`|Query, given as tuple `(s, r, ?)` where `s` = subject, `r` = relation, `?` = unknown answer|
|`Sq`|Set of supporting documents|
|`Cq`|Set of candidate answers (all entities in Sq)|
|`a* ∈ Cq`|Correct answer|

**Goal:** Learn a model that identifies `a*` from `Cq` using supporting documents `Sq`.

---

### Entity Graph Construction

For a sample `(q, Sq, Cq, a*)`:

1. Identify mentions of entities in `Cq ∪ {s}` in supporting documents.
2. Each mention = a **node** in the graph.
3. Mentions include exact matches and co-reference chains.

**Four edge types:**

|Edge Type|Condition|
|---|---|
|MATCH|Two mentions are identical (within or across documents)|
|DOC-BASED|Two mentions co-occur in the same support document|
|COREF|Two mentions are in the same co-reference chain|
|COMPLEMENT|Any pair of nodes not already connected (makes graph complete, avoids disconnected components)|

Note: Edge features are **not used** in this formulation.

---

### Multi-Step Reasoning with Entity GCN (MPNN Framework)

- **MPNN (Message Passing Neural Network)** is a framework, not a specific algorithm.
- In a single layer: RGCN-style implementation with a common learning matrix per relation type.
- **Node embedding initialization:** ELMO embedding on the concatenation of the query and node entity text.
- **Gating parameter** is used for final aggregation of new embedding and original embedding.
- Implementation: **RGATConv** from PyTorch Geometric (supports GAT + multi-relational graph + edge features).

**Answer Selection:**

- Node with the highest score (max operator or softmax) on the candidate set is selected as the answer.

---

## 6.3 Knowledge Base QA (KBQA) and Multi-Hop QA

### What is KBQA?

- **Task:** Find entity answers for a natural language question from a Knowledge Base (KB).
- More challenging with multi-hop questions (requires traversing the KB through multiple relation hops).

### 3-Hop Example

Question: "What languages are the movies that share directors with Dick Tracy in?"

Multi-hop path:

```
Dick Tracy --[directed_by]--> Warren Beatty --[directed]--> Reds --[in_language]--> Russian
```

Answer: Russian.

---

### Forming the KG and Query Graph

- **Predicate:** The directed link (relation/property) between two entities in a KG.
- **Predicate information on edges must be used** — this requires edge-aware GNNs.
- A **query graph `Gq`** is constructed:
    - The question is treated as a **node `q`**.
    - Connected to the **topic entity `s1`** via new predicate type `rq`.
    - This allows question information to flow to candidate entities during propagation.
- **Node embedding for `q`:** Initialized using LSTM to encode query context.
- **Other nodes and edges:** Initialized using pre-trained word vectors.

### Overall Steps in KBQA

1. **Topic Entity Linking:** Identify subject entities in the user's question using Named Entity Recognition (NER) or entity linkers.
2. **Subgraph Retrieval:** Extract a smaller, relevant subgraph around the topic entity (N-hop neighborhood) from the full KG.
3. **GNN-Based Pruning and Scoring (Refinement):** GNN evaluates semantic and structural relevance of candidate entities to the question.
4. **Selection of Top Candidates:** Final candidates ranked by updated embeddings or PageRank scores.

### Message Passing in KBQA

- Uses **RGATConv** from PyTorch Geometric (relational GAT).
- Note: Standard RGATConv does NOT update edge features during message passing. For edge feature updates, custom implementation is needed.

---

### Combining GNN + LLM for QA (GraphRAG)

Workflow:

1. A GNN reasons over a dense KG subgraph → retrieves answer candidates.
2. Shortest paths in the KG connecting question entities and answer candidates are extracted (KG reasoning paths).
3. Paths are **verbalized** (converted to text) and given to an LLM for final reasoning (RAG style).

---

## 6.4 Attention-Based GNN to Predict Country GDP

- Structured as a **regression problem**.
- **Nodes:** Countries.
- **Node features:** Country-level statistics (population, unemployment, CPI).
- **Edges:** Between trading partners.
- **Edge features:** Quantities of particular traded items.
- Implementation: **GATConv** (Graph Attention Convolution).

---

## 6.5 Graph2Seq: SQL-to-Text Generator

### Seq2Seq Limitation

- Traditional **Seq2Seq** models (encoder-decoder) process sequential inputs.
- **Limitation:** Less effective for graph-structured data where complex relationships are present — converting graphs to sequences loses critical information.

### WikiSQL Dataset

- 80,654 hand-annotated examples of questions and SQL queries over 24,241 Wikipedia tables.
- Contains: question (string), table (string), sql (string).
- **Task: Generate the question (text) from the SQL query (Graph2Seq direction).**

### Graph Representation of SQL Query

**For SELECT clause:**

- Create a SELECT node.
- Connect to column nodes with column name as text attribute.
- For aggregation functions (COUNT, MAX): add aggregation node connected to column nodes.

**For WHERE clause:**

- Each condition creates nodes similarly to SELECT.

**Extension:** Handles JOIN, ORDER BY, and other complex SQL syntax.

### Graph-to-Sequence Model Architecture

- SQL query → Graph representation.
- Graph → GNN encoding → graph-level representation.
- Graph representation → Sequence decoder (RNN/Transformer) → Natural language question.

---

## 6.6 GNN for Image-Guided Disease Diagnosis

### Background

- Medical imaging is critical for disease diagnosis.
- Traditional CNN methods work in the regular image domain.
- **Problem:** Some crucial disease indicators are **irregular spatial patterns** in medical images.
- **Solution:** Graphs naturally represent irregular spatial structures → GNNs capture hidden spatial patterns.

### Workflow

1. **Graph Construction:**
    
    - Brain regions and lesions localized as **ROI (Region of Interest)** → nodes.
    - Relationships between ROIs → edges.
2. **Modeling:**
    
    - GNN algorithms decode pairwise relations between subjects, brain regions, or lesions.
3. **Population Graph:**
    
    - All subjects construct one population graph → **node classification** task.
    - Aggregates features between subjects; uses spatial relations among subjects.
4. **Individual Graph:**
    
    - Each subject constructs their own graph → **graph classification** task.
    - GNN learns node representations; graph pooling/readout layers cluster nodes.

---

## 6.7 Visual Question Answering (VQA) and GNN

### What is VQA?

- Given an **image** and a **natural language question**, answer the question using information from the image.
- Requires both **NLP** (understanding the question) and **computer vision** (understanding the image).
- Standard approach: CNN for image representation + combination with question representation.

### Why GNNs for VQA?

- **Object relations** in the image are important for answering questions correctly.
- Graphs model explicit object connections; GNNs learn representations capturing semantic and spatial relations.

### VQA as Classification

Each sample: `(q, I)` where q = question, I = image.

- Task = classification where each class = one of the most common answers in the training set.
- Representations of `q` and `I` are learned and combined → fed to a prediction layer.

### Image as a Fully Connected Graph

1. **Object detector** generates bounding boxes → each bounding box = node.
2. Initial node representation `x_i` = average of convolutional feature maps in the bounding box.
3. **Question-dependent representation:**
    
    ```
    e_i = h([x_i, q])
    ```
    
    Where `h()` is a non-linear function combining node features and question.
4. **Adjacency matrix:** `A = E * E^T` (question-dependent similarity between nodes).

### Image as a Sparsely Connected Graph

- Fully connected graph is suboptimal (efficiency and performance).
- Keep only **top-m values** in each row of A; set others to 0.
- Resulting sparse graph `G_I` is then processed by GCN.
- **Max-pooling** → graph representation `F_I`.
- `F_I ⊙ q` (element-wise product) → prediction layer.

### Image AND Question as Graphs

- **Question `q`:** Modeled as a **syntactic dependency tree** (words = nodes, dependency relations = edges). Graph `Gq = {Vq, Eq, Rq}`.
- **Image `I`:** Modeled as a **fully connected graph** (extracted objects = nodes, pair-wise connected). Each edge `(v_i, v_j)` associated with vector `x_ij` encoding relative spatial relations.
- Both processed by GNNs → node representations combined to generate `(q, I)` pair representation.

### Generations of VQA Systems

|Generation|Description|
|---|---|
|1st: Language Joint Embedding|Early methods; only seen answers during training|
|2nd: Attention Mechanism|Focuses on specific image region relevant to question|
|3rd: Compositional Models|Multi-step reasoning for questions like "what is beside the cup?"|
|4th: Graph-Based Approaches|GNN captures object relations and question word interactions; handles complex reasoning|

### Challenges in VQA

- Low-quality images or very small objects.
- Answers may vary by technique used.
- Common sense and knowledge base reasoning (e.g., "Why is the boy running?") — requires external knowledge.
- Descriptive/explanatory answers are hard to generate.
- Numerical answer questions.

---

## 6.8 Skeleton-Based Action Recognition with GNN

### Background

- Action recognition applied in: **video surveillance, human-computer interaction**.
- Human skeleton joints can naturally form a graph.
- Earliest milestone GNN approach: **ST-GCN (Spatial-Temporal Graph Convolutional Network)**.

### Skeleton Graph Types

#### 1. Spatial Graph (Per-Frame)

- Each frame's skeleton = independent graph.
- Nodes `V` = skeleton joints (features: 3D/2D coordinates, confidence scores).
- Edges `E` = physical bone connections.
- Adjacency matrix `A`: `A_ij = 1` if joint `i` and joint `j` are connected, else `0`.

#### 2. Spatial-Temporal Graph (ST-GCN)

- Extends spatial graph by connecting joints across time frames.
- Combines spatial (physical connections, blue) and temporal (inter-frame links, green) information.
- **Heterogeneous graph:** Two edge types (spatial physical + temporal connections).

#### 3. Interaction Graph

- Involves two or more skeletons (for multi-person interaction).
- **Heterogeneous:** Nodes from different subjects; edges within-skeleton (physical) and between-skeleton (interaction).

#### 4. Generated Graph

- Adds automatically generated nodes or edges beyond physical/temporal connections.
- **Action-specific edges** added (in green in figures) for specific actions that require non-obvious joint connections.

### Graph for Action Classification

Graph `G = {V, E}`:

- Node set `V = {v_t_i | t = 1, ..., T; i = 1, ..., N}` (T = time steps, N = joints per frame).
- **Intra-skeleton edges (green):** Bone connections within the same skeleton.
- **Inter-skeleton edges (blue):** Same joint in consecutive skeletons.

**Task:** Graph classification where classes are the actions (running, walking, etc.).

**Process:**

1. Graph filtering → node representations.
2. Global pooling layer → graph representation.
3. Feedforward network → prediction (action class).

### Approaches for Spatial-Temporal Processing

|Approach|Description|
|---|---|
|Separate Strategy|GCN extracts spatial features; RNN/LSTM handles temporal separately; then combined|
|Bidirectional Strategy|Bidirectional LSTM for both forward and backward temporal information|
|Spatial-Based|GNN = spatial feature extractor; temporal evolution by CRF or RNN|

### Problem Definitions per Graph Type

|Graph Type|Main Challenge|
|---|---|
|Spatial graph|How to extract spatial info + perform temporal aggregation|
|Spatial-temporal graph|Process spatial and temporal simultaneously|
|Interaction graph|Preserve inter-skeleton interactions|
|Generated graph|Add relevant information effectively|
|Directed graph|Modify undirected GCN kernels to respect message direction|

---

## Q&A — QA, KBQA, Graph2Seq, VQA, Action Recognition

**Q1: Why is GNN well-suited for multi-hop QA?** A: Multi-hop QA requires following reasoning paths through multiple entities and relations. GNN's message passing over L layers naturally propagates information from the topic entity through L hops to candidate answer nodes. If a candidate's L-hop neighborhood path matches the question, it's likely the answer.

**Q2: What are the four edge types in the WikiHop entity graph?** A: MATCH (identical mentions), DOC-BASED (co-occur in same document), COREF (same co-reference chain), and COMPLEMENT (pairs not otherwise connected, making the graph complete to prevent disconnected components).

**Q3: What is the difference between Virtual Screening and De Novo Design?** A: Virtual Screening maps chemical space to functional space (predicts properties of existing molecules to find candidates). De Novo Design maps functional space to chemical space (uses generative models to create new molecular structures meeting desired property criteria from scratch).

**Q4: How is a SQL query represented as a graph for Graph2Seq?** A: SELECT and WHERE clauses each generate nodes. A SELECT node connects to column nodes; aggregation functions (COUNT, MAX) add aggregation nodes connected to column nodes. Conditions in WHERE are processed similarly. This preserves SQL structure and semantics for GNN processing.

**Q5: In VQA, why is a fully connected graph for the image converted to a sparse one?** A: A fully connected adjacency matrix `A = EE^T` captures all pairwise object similarities, but this is computationally expensive and includes many weak connections. Sparsifying by keeping only the top-m connections per node improves both efficiency and model performance.

**Q6: What makes the ST-GCN graph heterogeneous?** A: It contains two different types of edges: spatial edges (physical bone connections within the same frame/skeleton) and temporal edges (connecting the same joint across consecutive time frames). Having multiple edge types with different semantics makes it heterogeneous.

**Q7: What is the role of COMPLEMENT edges in the WikiHop entity graph?** A: COMPLEMENT edges connect any pair of nodes not otherwise connected by MATCH, DOC-BASED, or COREF edges. They ensure the graph is fully connected (no isolated components), allowing information to flow across all entity mentions and facilitating multi-hop reasoning.

**Q8: Why is population graph modeling vs. individual graph modeling used for different disease diagnosis tasks?** A: Population graphs aggregate features across subjects and use spatial relations among subjects → suited for node classification (each patient = a node). Individual graphs model one patient's own brain/lesion structure → suited for graph classification (the patient's graph is classified into a disease category).

---

---

# UNIT 7: ADVERSARIAL ATTACK AND DEFENSE ON GNN

---

## 7.1 Introduction

- Adversarial attack and defense on graphs is a recent, rapidly growing area.
- Almost all work on graph adversarial attacks uses GNNs.
- An important area for future research.

---

## 7.2 Core Definitions

**Adversarial Attack:** Any method that exploits the weakness of the targeted system to cause it to work in an unintended or undesirable way.

**Adversarial Example:** An input to an ML model **purposely designed to cause a mistake** despite resembling a valid input to a human.

- Most studied in **image recognition** (imperceptible pixel modifications cause misclassification).
- Also applicable to GNNs (graph modifications cause incorrect node/graph classification).

---

## 7.3 Types of Attacks

### By Knowledge Level

|Attack Type|Attacker's Knowledge|
|---|---|
|White-box Attack|Full access to model parameters (weights, gradients)|
|Black-box Attack|No access to model parameters; only observes inputs/outputs|

---

### By What is Perturbed

**Perturbation on graph `G^(0) = (A^(0), X^(0))` → `G' = (A', X')`:**

|Attack Type|What is Perturbed|Description|
|---|---|---|
|Structure Attack|Adjacency matrix `A`|Adding/deleting edges|
|Feature Attack|Feature matrix `X`|Modifying node attributes|

---

### By Target

**Direct Attack vs. Influencer Attack:**

|Attack Type|Target|Mechanism|
|---|---|---|
|Direct Attack|The target node `v_0` itself|Directly modify features or connections of `v_0`|
|Influencer Attack|Neighbors of `v_0` (NOT `v_0` itself)|Modify neighboring nodes' features/connections to indirectly mislead GNN about `v_0`|

**Why Influencer Attacks work:** Due to GNN's message passing, node `v_0`'s prediction depends not only on itself but on all its neighbors (non-i.i.d. nature of graph data). Changing neighbors changes what information flows to `v_0`.

---

### By Timing

#### 1. Poisoning Attack (Training Phase)

- Attacker **influences training data or labels** to cause model to underperform during deployment.
- As ML systems are retrained using operational data, an attacker can inject malicious samples during operation.

**Data Poisoning:**

- **Label Flipping:** Change correct labels to incorrect ones.
- **Backdoor Attack:** Embed specific triggers (e.g., pixel pattern) that cause malicious behavior only when trigger is present.
- **Data Injection:** Insert fake data into training pipeline.
- **Examples:** Poisoning spam filters to accept malicious emails; manipulating autonomous driving models to ignore stop signs.
- **High risk** for systems using automated, unverified data feeds (LLM pre-training, recommender systems).

**Model Poisoning:**

- Directly modifies model parameters (weights) after the model is created.
- Common in **Federated Learning:** Attacker poisons model updates (gradients/weights) sent from a compromised node to the central server.
- More effective than data poisoning — directly manipulates model parameters.
- **Defense:** Privacy Preserving Machine Learning; stringent data sanitization; robust aggregation in collaborative training.

---

#### 2. Evasion Attack (Inference/Deployment Phase)

- The model is already trained; attacker manipulates **input data during deployment** to deceive the classifier.
- **Most researched type of attack.**
- Subtle, imperceptible perturbations to graph structure (add/delete edges) or node features.
- Can severely degrade GNN performance in node classification, link prediction, and graph classification.
- Works even in **black-box scenarios**.

**Common approaches:**

- Graph Manipulation (add/delete edges)
- Feature Manipulation (modify node features)
- Gradient-based attacks (compute gradient to find optimal perturbation)

**Gradient-based Evasion (Inverse Goal):**

- Standard training: Use gradients to minimize loss by adjusting **model weights**.
- Evasion attack: Use gradients to **maximize loss** by adjusting **input data**.

---

#### 3. Model Extraction Attack

- **Goal:** Obtain a copy of the target model `~f` by querying it (black-box).
- Reconstructs the model or extracts training data.
- Significant risk when training data or model is sensitive/confidential.
- **Example:** Stealing a stock market prediction model for financial benefit.

---

## 7.4 Norm Bound of Evasion Attacks

**Mathematical formulation:**

```
x_adv = x + δ   such that   f(x + δ) ≠ y
```

- `x` = original input; `δ` = perturbation; `y` = true label; `f` = model.
- **Constraint:** `L(δ) < T`
    - `L` = norm function measuring the size of perturbation `δ`.
    - `T` = upper bound (the "budget").

**Purpose of Norm Bound:**

- Ensures the adversarial example remains close to the original (imperceptible to humans).
- The attack is a subtle change crossing the model's decision boundary, not a random large distortion.

---

## 7.5 Fast Gradient Sign Method (FGSM)

**Type:** Single-step, gradient-based **evasion attack**.

**Purpose:** Create perturbations on input data to cause a trained model to misclassify.

**How it works:**

- Leverages the **gradient of the model's loss function** to determine the direction that **maximizes loss** in a single step.
- Operates during the **inference/testing stage** (not training) → it's an evasion attack, NOT poisoning.

**Mathematical formula:**

```
x' = x + ε * sign(∇_x J(θ, x, y))
```

|Symbol|Meaning|
|---|---|
|`ε`|Perturbation magnitude (how much noise to add)|
|`θ`|Model parameters (weights and biases)|
|`x`|Input data (testing data)|
|`y`|Actual label|
|`J(θ, x, y)`|Loss function|
|`∇_x J(θ, x, y)`|Gradient of loss with respect to input `x`|
|`sign()`|Takes only the sign (+1 or -1) of the gradient|

FGSM is a **baseline benchmark** for measuring neural network robustness.

---

### FGSM on GNNs

- FGSM can be adapted to GNNs by computing the gradient with respect to:
    - **Adjacency matrix** (structure attack): identifies which edges to add/delete.
    - **Node features** (feature attack): identifies which features to modify.
- Graph data is often **discrete** — gradient methods are adapted for discrete graph modifications.

---

## 7.6 PGD (Projected Gradient Descent) Attack

**Type:** Iterative, stronger version of FGSM.

**Key properties:**

|Property|Description|
|---|---|
|Iterative|Applies multiple small steps (unlike single-step FGSM)|
|Projected|After each step, perturbed input is projected back onto a constraint set (ε-ball), keeping perturbation small|
|Random Restart|Can start from random points in constraint set to find stronger adversarial examples|
|Benchmark|"De facto" standard for evaluating adversarial robustness|

**Mathematical formula:**

```
x^(t+1) = Π_{x + ε}(x^(t) + α * sign(∇_x J(θ, x^(t), y)))
```

|Symbol|Meaning|
|---|---|
|`Π`|Projection operator (ensures input stays in ε-ball)|
|`x + ε`|The ε-ball constraint around original input `x`|
|`α`|Step size per iteration|
|`t`|Current iteration number|

---

## 7.7 Defense Methods

### 1. Adversarial Training

- **Retrain** the model using adversarial examples to make it more robust.
- **Limitation:** Slow and expensive — every training example must be tested for adversarial weaknesses, and the model must be retrained with all these examples.

### 2. Mixed Training

- Use training datasets that mix **clean data and perturbed data**.
- Allows the model to make functional predictions for both clean and perturbed inputs.
- **Trade-off:** Lowers overall accuracy compared to using only clean data.

---

## Q&A — Adversarial Attacks

**Q1: What is the difference between a poisoning attack and an evasion attack?** A: Poisoning attacks occur at training time — the attacker contaminates the training data to degrade model performance during deployment. Evasion attacks occur at inference time — the model is already trained and the attacker modifies inputs to cause misclassifications during deployment.

**Q2: What is an adversarial example?** A: An input to an ML model purposely designed to cause it to make a mistake despite appearing as a valid input to a human. The perturbation is imperceptible or inconspicuous to humans but crosses the model's decision boundary.

**Q3: What is the difference between direct and influencer attacks in GNNs?** A: A direct attack modifies the target node's own features or connections. An influencer attack modifies neighboring nodes (not the target itself) to indirectly cause the target's prediction to change — exploiting GNN's message passing where a node's prediction depends on its neighborhood.

**Q4: Why is model poisoning more effective than data poisoning?** A: Data poisoning corrupts the training data indirectly — the model learns from it but doesn't guarantee specific parameter changes. Model poisoning directly modifies model parameters (gradients/weights), enabling more targeted and precise manipulation of the model's behavior.

**Q5: What is the norm bound in an evasion attack and why is it important?** A: The norm bound `L(δ) < T` is a constraint on the magnitude of perturbation `δ`. It ensures the adversarial example remains close to the original (imperceptible to humans). Without it, a trivially large perturbation could always fool the model, but it wouldn't be a meaningful attack.

**Q6: What is the key difference between FGSM and PGD?** A: FGSM takes a single large step in the gradient direction — fast but weaker. PGD takes multiple small steps iteratively, projecting back onto the epsilon-ball constraint at each step, finding a stronger adversarial example. PGD is the stronger, more reliable attack.

**Q7: Why does the `sign()` function appear in the FGSM formula?** A: The `sign()` function normalizes the gradient direction to ±1, ensuring the perturbation magnitude is exactly ε regardless of the gradient's actual magnitude. This creates a uniform, bounded perturbation in the direction that maximally increases the loss.

**Q8: What is the `Π` projection operator in PGD?** A: The projection operator `Π_{x+ε}` maps the perturbed input back onto the ε-ball (the allowed perturbation ball around the original input x) after each gradient step. This ensures the cumulative perturbation across all iterations remains bounded and imperceptible.

**Q9: How is FGSM adapted for GNNs?** A: Instead of computing gradients with respect to pixel values (images), FGSM for GNNs computes gradients with respect to the adjacency matrix (for structure attacks — finding which edges to add/delete) or the node feature matrix (for feature attacks). The discrete nature of graph data requires special handling.

---

---

# UNIT 8: SOFTWARE PROGRAM ANALYSIS WITH GNN

---

## 8.1 Overview

**Research Paper:** "An integrated graph neural network model for joint software defect prediction and code quality assessment."

**Goal:** Use multiple graph representations of source code as inputs to a GNN for joint defect prediction and code quality assessment.

---

## 8.2 Code as Graphs — Three Key Representations

### 1. Abstract Syntax Tree (AST)

**Definition:** A tree representation of the syntactic structure of source code.

- Abstracts away unnecessary details (parentheses, semicolons).
- **Nodes:** Program constructs (statements, expressions, declarations).
- **Edges:** Grammatical hierarchy and relationships.

**Uses:**

- Compiler parsing and semantic analysis.
- Code formatting, linting (static analysis), automated refactoring.

**Formal definition of AST nodes:**

```
NAST = {n_method, n_variable, n_operator, n_statement, ...}
```

**Graph:** `GAST` preserves the **hierarchical nature** of code (parent-child relationships) — critical for understanding code structure and identifying defect patterns.

**Construction:** Recursive parsing of code statements and expressions.

---

### 2. Control Flow Graph (CFG)

**Definition:** A directed graph modeling all possible execution paths through a function or program.

- **Nodes:** **Basic blocks** — maximal sequences of straight-line, branch-free code with single entry and exit points.
- **Edges:** Potential control transfers (jumps, branches, function calls) between blocks.

**Formal definition:**

```
GCFG = (VCFG, ECFG)
```

- `VCFG` = set of basic blocks.
- `ECFG` = directed edges representing control flow transitions.

```
NCFG = {basicblock_1, basicblock_2, ..., basicblock_n}
```

**Uses:**

- Control flow analysis.
- Compiler optimizations (dead code elimination).
- Software testing (path coverage).

---

### 3. Data Flow Graph (DFG)

**Definition:** A directed graph representing the flow of data and variable dependencies within a program.

- **Nodes:** Operations or functions.
- **Edges:** Flow of data values (variables, operands) from where they are **defined** to where they are **used**.

**Formal edge definition:**

```
Edge (u → v) exists if:
    def(u) ∩ use(v) ≠ ∅
```

- `def(u)` = set of variables defined at node `u`.
- `use(v)` = set of variables used at node `v`.

**Uses:**

- Compiler optimizations (parallelism, instruction reordering).
- Hardware synthesis.
- Identifying data dependencies.

**Key property:** DFGs expose parallelism opportunities — operations without data dependencies can execute in parallel.

---

### Comparison of Three Code Graphs

|Graph|Focus|Nodes|Edges|Primary Use|
|---|---|---|---|---|
|AST|Syntax structure|Program constructs|Grammatical hierarchy|Parsing, linting, refactoring|
|CFG|Execution flow|Basic blocks|Control transfers|Testing, compiler optimization|
|DFG|Data dependencies|Operations|Variable def-use relationships|Parallelism, data flow analysis|

---

## 8.3 Graph Embedding Generation

### AST-Based Embedding

- Recursive parsing → nodes for method declarations, variable assignments, control structures.
- Preserves hierarchical code organization.
- Captures parent-child relationships for understanding code structure.

### CFG-Based Embedding

- Identifies control flow relationships: sequential execution, conditional branching, loop structures.
- Creates directed edges for possible execution paths.

### DFG-Based Embedding

- Analyzes variable definitions, uses, and modifications.
- Creates edges representing data dependencies between statements.

### Multilevel Code-Graph Embedding

- Combines embeddings from AST, CFG, and DFG into a **unified multi-level representation**.
- This combined graph embedding can be used to indicate **code quality** and predict **software defects**.

---

## Q&A — Software Program Analysis

**Q1: What is an Abstract Syntax Tree and what does it capture?** A: An AST is a tree representation of source code's syntactic structure. Nodes represent program constructs (statements, expressions, declarations) and edges represent grammatical hierarchy. It abstracts away syntactic details like parentheses and semicolons, capturing the logical structure of code for tasks like linting, formatting, and defect analysis.

**Q2: What are basic blocks in a CFG?** A: Basic blocks are maximal sequences of consecutive statements with a single entry point and single exit point — no jumps into or out of the middle of the block. They are the nodes of a Control Flow Graph, connected by directed edges representing possible control transfers.

**Q3: What does the DFG edge condition `def(u) ∩ use(v) ≠ ∅` mean?** A: An edge exists from node u to node v in the DFG if the set of variables defined (written) at u overlaps with the set of variables used (read) at v. This captures data dependency — v needs the output of u.

**Q4: Why do we need all three representations (AST, CFG, DFG) rather than just one?** A: Each graph captures different aspects of code: AST captures syntactic structure and hierarchy, CFG captures execution paths and branching behavior, DFG captures data dependencies and variable flow. Defects can manifest in any of these dimensions. A multilevel embedding combining all three provides a comprehensive code representation.

**Q5: What is a Data Flow Graph used for in compiler optimization?** A: DFGs expose data dependency relationships between operations. Operations without data dependencies (no shared variables) can be safely reordered or executed in parallel, enabling instruction-level parallelism. This is essential for modern compiler optimizations and hardware synthesis.

---

---

# UNIT 9: GNN AND LARGE LANGUAGE MODELS (LLM)

---

## 9.1 Motivation: Complementary Strengths

### GNN Strengths and Limitations

**Strengths:**

- Detect patterns of connections (fraud rings, user-product networks).
- Relational reasoning over graph structure.
- Pattern recognition in networks.

**Limitations:**

- Rely on simple initial features (one-hot encodings, basic attributes).
- Struggle when **rich semantic information** (e.g., text) is present but not properly encoded.

---

### LLM Strengths and Limitations

**Strengths:**

- Powerful **semantic understanding** of text.
- High-dimensional vector representations encoding meaning.
- Example: Read product descriptions and user reviews → condense into embeddings capturing features and sentiment.

**Limitations:**

- No innate ability to handle **structured relational information**.
- Don't naturally understand that two records are linked unless described in text.
- **Hallucination:** Rely on statistical associations rather than explicit relationships.
- Miss the full picture when structure and relationships matter.

---

### The Complementarity

> GNNs excel at relational reasoning and pattern recognition in networks. LLMs excel at language understanding and rich contextual knowledge.

**A combined approach** can understand **both structure AND semantics**.

---

## 9.2 Three Configuration Patterns

---

### Configuration 1: LLM as Enhancer

**Concept:** GNN is the primary reasoning model. LLM supplies additional information (feature generation or label assistance) to enhance GNN inputs.

**How it works:**

- LLM generates rich node embeddings from text associated with nodes (user profiles, product descriptions, document links).
- These embeddings provide stronger descriptive features.
- GNN refines them in light of graph connections.

**When to use:** When the graph alone is information-poor — connectivity is important but doesn't fully characterize nodes. Works best when plentiful text data is associated with graph nodes.

**Advantage:** Flexible; plug-and-play.

**Disadvantage:** Significant overhead at scale. Requires querying LLM API **N times** for a graph with N nodes — substantial cost for large graphs.

---

### Configuration 2: LLM as Predictor (Graph-Assisted LLM Reasoning)

**Concept:** LLM is the primary reasoning engine. GNN acts as a **structured knowledge provider** for the LLM.

**How it works:**

- GNN reasons over a dense KG subgraph → retrieves structured facts/entity chains.
- These are converted to text and provided as context for the LLM (RAG style).
- LLM uses this structured, verifiable information to produce grounded answers.

**Example — GraphRAG:**

- Normal RAG uses vector similarity search for text passages → fails for multi-hop reasoning.
- GraphRAG: KG-based graph algorithms retrieve structured chains of facts → LLM consumes them.

**When to use:** When the problem requires explicit relational reasoning hard for LLMs to learn implicitly (tracing money flow, connecting clues across large datasets).

**Benefits:**

- Reduces hallucination.
- Improves factuality by grounding LLM in verifiable relations.

**Two sub-approaches:**

1. **Flatten-based:** Convert the graph to textual descriptions; LLM processes directly.
2. **GNN-based:** GNN captures structural characteristics; cross-modal projections fuse GNN output with LLM context.

---

### Configuration 3: Unified GNN-LLM Architecture

**Concept:** A single model jointly learning from graph data and language data end-to-end.

**Goal:** Seamlessly blend GNN and Transformer (LLM backbone) into one framework.

**Two practical approaches:**

**Approach 1: Graph Transformers**

- Graph Transformers (Graphormer, Graph Attention Networks) serve as the "graph part."
- A language encoder provides initial embeddings or processes text queries.
- Both components trained together on a multi-modal objective.

**Approach 2: Knowledge Distillation**

- A large LLM (teacher) teaches a GNN (student) by:
    - Labeling large amounts of graph data.
    - Providing enriched node embeddings for the GNN to mimic.
- OR: Train GNN on graph task; fine-tune LLM to consume GNN output for downstream tasks (e.g., explanation generation).

**Practical strategy:** Pre-train or freeze one component. Example: Use pre-trained LLM to generate node embeddings (frozen), then train GNN on the task.

---

## 9.3 GNN + LLM Use Cases

### 1. Intelligent Engineering Assistant

- **GNN:** Models full system architecture (e.g., aircraft environmental control system).
- **LLM:** Reads requirements, design rationale, and certification rules.
- **Fusion enables:** "If we change this compressor, what downstream systems are impacted?" → answer grounded in real system structure and documentation.
- **Value:** Moves AI from chatbots to engineering copilots.

---

### 2. Manufacturing Planning and Optimization

- **GNN:** Models factory production line as a graph (machines, workflows, materials, dependencies).
- **LLM:** Understands process documentation, work instructions, historical reports.
- **Fusion enables:** Predict bottlenecks before they happen; explain why in natural language; recommend corrective actions.

---

### 3. Supply Chain Resilience

- **GNN:** Supplier networks are graphs (multi-tier supplier relationships).
- **LLM:** Contracts, risk reports, and communications are text.
- **Fusion enables:** Early detection of supply chain risk; impact analysis of supplier disruption; natural-language explanation of mitigation strategies.

---

### 4. Recommendation Systems

- **LLM:** Provides item embeddings (product descriptions, reviews) and user embeddings (biography, past reviews).
- **GNN:** Captures the interaction graph (who clicked/purchased what, who follows whom).
- **Benefits:**
    - Handles sparse data better: when interactions are few, content fills the gap; when content is ambiguous, interactions clarify.
    - Incorporates multi-hop reasoning (friend-of-friend, item-item similarity via graph).
    - Covers both collaborative (graph) and content-based (LLM) dimensions.

---

### 5. Social Networks

**Community characterization:**

- GNN detects communities.
- LLM summarizes what each community is about by reading representative posts.
- Ensures communities are not just graph-cohesive but also topically coherent.

**Influence and information spread:**

- Graph algorithms identify central or bridge users.
- LLM analyzes the content those central users produce (persuasive language, misinformation level).
- Together: forecast how content will spread — graph structure gives "who connects to whom"; language gives "why content might resonate."
- **Applications:** Viral marketing; countering fake news.

---

## Q&A — GNN and LLM

**Q1: Why do GNNs struggle with rich text information?** A: Traditional GNNs rely on simple initial node features (one-hot encodings, basic attributes). When rich semantic text is associated with nodes, GNNs don't naturally encode the full meaning of that text. This is why LLMs are needed as enhancers to convert text to semantically rich embeddings first.

**Q2: Why do LLMs hallucinate, and how do GNNs help?** A: LLMs generate text based on learned statistical associations rather than explicit structured facts. They can "hallucinate" facts that seem plausible but are wrong. GNNs, by reasoning over explicit knowledge graph edges and nodes, provide verifiable relational facts that ground the LLM's generation, reducing hallucination.

**Q3: What is the main cost disadvantage of the LLM-as-enhancer approach?** A: For a graph with N nodes, LLM-as-enhancer requires querying the LLM N times (once per node) to generate embeddings. For large-scale graphs (millions of nodes), this is prohibitively expensive in both cost and latency.

**Q4: What is GraphRAG and how does it improve over vanilla RAG?** A: Vanilla RAG fetches text passages using vector similarity, which fails for multi-hop reasoning. GraphRAG uses GNN/graph algorithms to traverse a knowledge graph, extracting structured chains of related facts. These chains are verbalized and provided to the LLM as context, enabling accurate multi-hop question answering grounded in explicit relational knowledge.

**Q5: How does knowledge distillation work in unified GNN-LLM architectures?** A: A large LLM (teacher model) is used to label large amounts of graph data or provide enriched node embeddings. The GNN (student model) is trained to match or mimic the LLM's outputs. This transfers the LLM's semantic knowledge into the GNN without requiring the GNN to directly process raw text.

**Q6: In a GNN+LLM recommender system, what does each component contribute?** A: The LLM contributes semantic embeddings for items (from product descriptions, reviews) and users (from biography, past reviews). The GNN contributes structural reasoning over the interaction graph (who purchased what, who follows whom). Together they handle both content-based and collaborative filtering aspects, with each compensating for the other's weaknesses.

**Q7: How can GNN+LLM help with fake news on social networks?** A: The GNN can detect communities and identify central/bridge users based on graph structure. The LLM can analyze the content those users post to assess persuasiveness, misinformation, or emotional appeal. Together, they can predict how content will spread (graph structure) and why it might resonate (language analysis), enabling targeted intervention.

**Q8: What is the flatten-based vs. GNN-based approach in LLM-as-predictor configuration?** A: Flatten-based converts the graph into a sequential text description, allowing standard LLM text processing but potentially losing structural information. GNN-based uses a GNN to capture structural patterns and dependencies, then fuses this with LLM context via cross-modal projections, preserving graph structure in the model's reasoning.

---

---

# APPENDIX: KEY FORMULAS QUICK REFERENCE

|Formula|Context|
|---|---|
|`x' = x + ε * sign(∇_x J(θ, x, y))`|FGSM evasion attack|
|`x^(t+1) = Π_{x+ε}(x^(t) + α * sign(∇_x J))`|PGD iterative evasion attack|
|`L_total = L_new + λ * Σ F_i(θ_i - θ*_i)²`|EWC continual learning loss|
|`A = E * E^T`|VQA image graph adjacency matrix|
|`e_i = h([x_i, q])`|VQA question-dependent node representation|
|`Edge(u→v) if def(u) ∩ use(v) ≠ ∅`|DFG edge condition|
|`G = {V, E, R}` where `e = (v_i, r, v_j)`|Multi-relational graph for polypharmacy|
|`Z^(j+1) = GCLayer(Z^(j), A)`|GCN layer for fake news detection|

---

# APPENDIX: KEY TERMINOLOGY GLOSSARY

|Term|Definition|
|---|---|
|Long Tail Property|Power-law distribution of item ratings; most items are rarely rated|
|Matrix Completion|Predicting missing entries in the user-item rating matrix|
|Cold-Start Problem|Difficulty making recommendations for new users/items with no history|
|Bipartite Graph|Graph with two disjoint node sets (e.g., users and items)|
|Molecular Fingerprint|Vector representation of a molecule's structural information|
|De Novo Design|From-scratch generation of novel chemical structures|
|SMILES|ASCII text format representing chemical structures|
|Polypharmacy|Using combinations of drugs to treat complex diseases|
|Residue|An amino acid incorporated into a protein chain|
|Interfacial Residues|Residues at the binding interface between two proteins|
|Adversarial Example|Purposely perturbed input causing an ML model to make a mistake|
|Evasion Attack|Attack during inference that manipulates test inputs|
|Poisoning Attack|Attack during training that corrupts training data|
|Norm Bound|Maximum allowed perturbation budget for adversarial examples|
|Catastrophic Forgetting|Loss of old task knowledge when learning new tasks sequentially|
|Fisher Information Matrix|Measures curvature of log-likelihood; identifies critical model parameters|
|GraphRAG|Retrieval-Augmented Generation using knowledge graph structure|
|KBQA|Knowledge Base Question Answering|
|Multi-Hop QA|QA requiring reasoning across multiple intermediate entities|
|Chirality|Molecular property where two mirror-image structures exist|
|Interactome|Complete set of protein-protein interactions in an organism|
|CIA Triad|Confidentiality, Integrity, Availability — pillars of cybersecurity|
|EWC|Elastic Weight Consolidation — continual learning algorithm using FIM|
|GEM|Gradient Episodic Memory — continual learning using memory of old tasks|
|C&C Channel|Command and Control channel used by botmasters to control bots|
|AST|Abstract Syntax Tree — hierarchical syntactic representation of code|
|CFG|Control Flow Graph — models all possible execution paths in code|
|DFG|Data Flow Graph — models variable definition and use dependencies|

---

_End of Notes — UE23AM342BA1: Interdisciplinary Deep Learning on Graphs_ _Dr. Bhaskarjyoti Das | Department of CS in AI & ML and Engineering_