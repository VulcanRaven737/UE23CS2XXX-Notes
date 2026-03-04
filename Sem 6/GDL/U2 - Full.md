# 📘 Deep Learning on Graphs — Complete Tutor Notebook

### Course: UE23AM342BA1 | Interdisciplinary Deep Learning on Graphs

### Based on: Dr. Bhaskarjyoti Das | CS Engineering (AI & ML), PES University

---

> **How to use this notebook:** Every major concept is explained deeply from first principles, followed by a rich set of Q&A pairs to test and consolidate your understanding. Read actively — don't skip the Q&A sections!

---

# Table of Contents

1. [Message Passing Framework & Aggregation Rules](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#1-message-passing-framework--aggregation-rules)
2. [Transductive vs. Inductive Embedding](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#2-transductive-vs-inductive-embedding)
3. [Permutation Invariance & Equivariance](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#3-permutation-invariance--equivariance)
4. [Graph Convolutional Network (GCN)](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#4-graph-convolutional-network-gcn)
5. [GraphSAGE](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#5-graphsage)
6. [Graph Attention Network (GAT)](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#6-graph-attention-network-gat)
7. [GNN Loss Functions & Training Settings](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#7-gnn-loss-functions--training-settings)
8. [Graph Isomorphism & GIN](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#8-graph-isomorphism--gin)
9. [Graph Auto-Encoder (GAE) & Variational GAE (VGAE)](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#9-graph-auto-encoder-gae--variational-gae-vgae)
10. [Spatio-Temporal GNNs (Dynamic Graphs)](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#10-spatio-temporal-gnns-dynamic-graphs)
11. [Heterogeneous Graphs & R-GCN](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#11-heterogeneous-graphs--r-gcn)
12. [Graph Transformer Networks](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#12-graph-transformer-networks)
13. [GNN Interpretability & GNNExplainer](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#13-gnn-interpretability--gnnexplainer)
14. [GNN Modelling: Complexity, Pooling, Layers, Augmentation](https://claude.ai/chat/03a9d347-8d7b-477b-95dd-7216216ed181#14-gnn-modelling-complexity-pooling-layers-augmentation)

---

# 1. Message Passing Framework & Aggregation Rules

## 1.1 The Core Idea

In a traditional neural network, data has a natural linear or grid structure — words in a sentence, pixels in an image. Graphs don't have this luxury. A node's neighbourhood can be any size, in any order.

The **Message Passing Framework** solves this by defining a universal recipe: at each layer, every node collects ("aggregates") information from its direct neighbours, then "updates" its own representation using that collected information plus its own state.

Formally, for node $v$ at layer $l$:

$$\mathbf{m}_v^{(l)} = \text{AGGREGATE}\left(\left{\mathbf{h}_u^{(l-1)} : u \in \mathcal{N}(v)\right}\right)$$

$$\mathbf{h}_v^{(l)} = \text{UPDATE}\left(\mathbf{h}_v^{(l-1)},\ \mathbf{m}_v^{(l)}\right)$$

After $L$ layers, node $v$ has a representation that encodes information from all nodes within $L$ hops.

---

## 1.2 Aggregation Rules

Three classical aggregation strategies exist, each with a distinct philosophy:

### Sum Aggregation

$$f(\mathbf{H}^l, A) = \sigma\left(A \mathbf{H}^l \mathbf{W}^l\right)$$

- Adds all neighbour feature vectors.
- **Advantage:** Preserves multi-set structure — 10 blue + 10 red nodes give a different result than 1 blue + 1 red. It is therefore more **expressive**.
- **Risk:** For high-degree nodes, the aggregated vector's magnitude scales with degree, potentially causing numerical instability.

### Mean Aggregation

$$f(\mathbf{H}^l, A) = \sigma\left(\mathbf{D}^{-1}\hat{A}\mathbf{H}^l \mathbf{W}^l\right) \quad \text{where } \hat{A} = A + I$$

- Normalises by the degree matrix $\mathbf{D}^{-1}$, so the magnitude doesn't blow up.
- Good general-purpose baseline; works well in most scenarios.
- **Limitation:** Cannot distinguish between neighbourhoods of different sizes that happen to have the same average.

### Spectral (Symmetric Normalisation) Aggregation — used by GCN

$$f(\mathbf{H}^l, A) = \sigma\left(\hat{\mathbf{D}}^{-1/2}\hat{A}\hat{\mathbf{D}}^{-1/2}\mathbf{H}^l \mathbf{W}^l\right)$$

- Both the source and target node degrees are used for normalisation.
- Proposed by Kipf & Welling.
- Weighs low-degree neighbours more than high-degree ones — beneficial when low-degree neighbours are more informative.

### Max Aggregation

- Takes the element-wise maximum across all neighbour feature vectors.
- Useful when identifying the **single most influential** neighbour or feature.
- Not sensitive to the number of neighbours; less affected by dilution from averaging.
- But loses information about less dominant features.

---

## 1.3 The UPDATE Function

After aggregation, the UPDATE step combines the aggregated message $\mathbf{m}_v$ with the node's own current state:

$$\mathbf{h}_v^{(l)} = \sigma\left(\mathbf{W} \cdot \text{CONCAT}\left[\mathbf{h}_v^{(l-1)},\ \mathbf{m}_v^{(l)}\right]\right)$$

**Why is a non-linear activation needed?**

Without a non-linearity, every GNN layer is just a linear transformation. Stacking multiple linear layers is mathematically equivalent to a single linear transformation. The entire network collapses to one layer, losing the power to learn complex, non-linear patterns. Activations like ReLU break this collapse.

---

## 📝 Q&A — Section 1

**Q1. What problem does the message passing framework solve that traditional neural networks can't handle directly?**

A: Traditional NNs require fixed-size, ordered inputs. Graphs have variable-size neighbourhoods with no natural ordering. Message passing sidesteps this by defining aggregation over sets of neighbours (which are unordered), enabling learning on arbitrarily structured graphs.

---

**Q2. Why does sum aggregation have better expressive power than mean aggregation in discriminating graph structures?**

A: Mean aggregation loses information about the size of the neighbourhood. For example, a node with 10 blue and 10 red neighbours gives the same mean vector as a node with 1 blue and 1 red neighbour. Sum aggregation preserves this size information — 10+10 ≠ 1+1 in the summed vector space.

---

**Q3. What is the mathematical issue with using only sum aggregation on high-degree nodes?**

A: The magnitude of the aggregated vector scales linearly with degree. For a node connected to thousands of neighbours, the sum can become extremely large, causing numerical instability (exploding gradients) during backpropagation.

---

**Q4. In the mean aggregation formula $f(\mathbf{H}^l, A) = \sigma(\mathbf{D}^{-1}\hat{A}\mathbf{H}^l \mathbf{W}^l)$, what is $\hat{A}$ and why is it used?**

A: $\hat{A} = A + I$, where $I$ is the identity matrix. Adding the identity matrix incorporates self-loops into the adjacency matrix, ensuring that each node's own feature is included in the aggregation. Without it, the node would only aggregate from its neighbours, ignoring its own current representation.

---

**Q5. Why is a non-linear activation function necessary between GNN layers?**

A: Without non-linearity, a composition of $L$ linear GNN layers reduces to a single linear transformation. No amount of depth adds representational power. Non-linear activations (e.g., ReLU) make it possible to learn complex, non-linear decision boundaries in the graph feature space.

---

**Q6. In what use case would max aggregation be preferred over mean aggregation?**

A: Max aggregation is preferred when the goal is to detect the **presence** of the most prominent feature, not the average feature. For instance, in fraud detection on a transaction graph, if even one connected account has suspicious behaviour (a high-value feature), max aggregation will surface it, whereas mean aggregation might dilute it across many legitimate transactions.

---

**Q7. What is the role of the weight matrix $\mathbf{W}$ in each GNN layer?**

A: The weight matrix $\mathbf{W}$ serves two purposes: (1) it enables **learning** by being updated during backpropagation — it transforms features to encode what information is most useful for the task; (2) it performs **dimensionality reduction** of the feature embedding from one layer to the next.

---

**Q8. After $L$ GNN layers, what does a node's embedding encode?**

A: After $L$ layers, a node's embedding encodes information from all nodes within $L$ hops (its $L$-hop neighbourhood). Each layer expands the "receptive field" by one hop.

---

# 2. Transductive vs. Inductive Embedding

## 2.1 Transductive Embedding

Transductive methods learn **embeddings directly** for a fixed set of nodes in a single, known, static graph.

- The model directly optimises individual embedding vectors for each node.
- The **entire graph structure must be available during training**.
- Examples: DeepWalk, Node2Vec, matrix factorisation.
- **Cannot** generalise to unseen nodes or new graphs without full retraining.
- Adding one new node means retraining the entire model.
- Computationally expensive for large data.

**When is transductive better?** Community detection in a stable social network — the model exploits the full global network structure of all nodes (labelled and unlabelled), which is critical for identifying communities correctly.

---

## 2.2 Inductive Embedding

Inductive methods learn a **function** (a set of aggregator/transformation weights) to **generate** embeddings. This function can be applied to unseen nodes.

- Learns how to aggregate node features and local graph structure.
- Can generate embeddings for **previously unseen nodes** without retraining.
- More flexible and scalable for dynamic or expanding graphs.

**When is inductive better?** Email spam detection — new emails and accounts appear constantly. A transductive model would require retraining with every new batch, which is computationally prohibitive.

**Key insight:** In GCN, the output is node-specific embeddings. In GraphSAGE, the output is a trained **aggregation function** that can generate embeddings on demand for any node.

---

## 📝 Q&A — Section 2

**Q1. What is the fundamental conceptual difference between transductive and inductive learning?**

A: Transductive learning memorises a specific embedding for each specific node in a fixed graph. Inductive learning trains a general function (a mapping) that takes any node's features + local structure and produces its embedding. Transductive = per-node memory; Inductive = universal function.

---

**Q2. Why can't a standard GCN handle a new node appearing in the graph?**

A: GCN's formulation relies on the global adjacency matrix $\hat{A}$. When a new node is added, the dimensions of $\hat{A}$ change, invalidating the entire learned model. GCN learns node-specific embeddings rather than a universal function for generating them.

---

**Q3. What is the practical output of training a GraphSAGE model vs. training a GCN model?**

A: Training GCN produces a set of learned node embeddings (one per node). Training GraphSAGE produces trained aggregator functions — weights that define how to combine neighbourhood information. These functions can then generate embeddings for any node, even ones never seen during training.

---

**Q4. In semi-supervised learning on graphs, what is the difference between transductive test nodes and inductive test nodes?**

A: Transductive test nodes ($V_{trans}$) are present during training message passing (their edges and features are visible) but their labels are withheld from the loss. Inductive test nodes ($V_{ind}$) are completely invisible during training — neither their features, nor edges, nor labels are used. The model must generalise to them from scratch.

---

**Q5. Give two real-world scenarios where the choice between transductive and inductive matters.**

A: (1) **Drug interaction network** — When predicting interactions between known drugs in a fixed, well-studied graph, transductive is fine. But when new drugs are continuously being discovered and added, inductive GraphSAGE is essential to generate their embeddings instantly. (2) **Social network** — For community detection in a stable graph like a closed company network, transductive is better because it leverages all structural relationships. For a public platform like Twitter where users join every second, inductive is required.

---

# 3. Permutation Invariance & Equivariance

## 3.1 Why This Matters

Unlike pixels and words, **nodes have no natural ordering**. If you relabel all the nodes in a graph, the graph itself hasn't changed — it's the same structure. A well-designed GNN must respect this.

There are two key properties a GNN must satisfy:

---

## 3.2 Permutation Invariance

A function $f$ is **permutation invariant** if reordering its inputs doesn't change the output:

$$f(x_1, x_2, \ldots, x_n) = f(x_{\pi(1)}, x_{\pi(2)}, \ldots, x_{\pi(n)})$$

for **any** permutation $\pi$.

- This applies to the **aggregation function** over a single node's neighbourhood.
- SUM, MEAN, and element-wise MAX all satisfy this — they operate on **sets**, not sequences.
- Why needed: The set of a node's neighbours is the same regardless of how they are ordered/indexed.

---

## 3.3 Permutation Equivariance

A function $f$ is **permutation equivariant** if permuting the inputs results in the **same permutation** being applied to the output:

$$f(\pi(X)) = \pi(f(X))$$

- This applies to the entire GNN computation over all nodes.
- Since node embedding computation is performed for every node in parallel, if you relabel node A as node B, the embedding for that entity should simply be relabelled too — the underlying representation stays intact.
- Formally: if you permute the rows of the input feature matrix $\mathbf{H}$ and the rows/columns of the adjacency matrix $A$, the output embeddings $\mathbf{H}'$ will be permuted in the exact same way.

---

## 📝 Q&A — Section 3

**Q1. Why is it essential for the aggregation function in a GNN to be permutation invariant?**

A: A node's neighbours have no canonical ordering. If we listed them in a different order, we'd want the same aggregate message. If the aggregation function were not permutation invariant, two representations of the exact same neighbourhood (in different orders) would produce different embeddings — which is mathematically incorrect.

---

**Q2. What is the difference between permutation invariance and permutation equivariance?**

A: Invariance means the output doesn't change at all when inputs are permuted (used for graph-level outputs like graph classification). Equivariance means the output transforms in the same way as the input (used for node-level outputs — if node A's features are moved to position B in the input matrix, node A's embedding will also move to position B in the output).

---

**Q3. Give an example of an aggregation function that is NOT permutation invariant and explain why.**

A: Using an LSTM directly over a node's neighbours (without special handling) is not permutation invariant, because LSTMs process sequences and their output depends on the order in which elements are fed. The same set of neighbours in a different order produces a different hidden state, violating invariance. GraphSAGE uses LSTM aggregation with random shuffling to approximate invariance.

---

**Q4. Why is permutation equivariance the right property for node-level tasks while permutation invariance is appropriate for graph-level tasks?**

A: Node-level tasks (classification, embedding) need to produce a distinct output for each node. If nodes are relabelled, the outputs should relabel consistently — this is equivariance. Graph-level tasks (classifying the entire graph) need a single output for the entire graph; this output shouldn't change just because the nodes are renumbered — this is invariance.

---

# 4. Graph Convolutional Network (GCN)

## 4.1 Core Idea

GCN (Kipf & Welling, 2017) simplifies graph convolutions inspired by spectral graph theory. The key propagation rule is:

$$\mathbf{H}^{(l+1)} = \sigma\left(\hat{\mathbf{D}}^{-1/2}\hat{A}\hat{\mathbf{D}}^{-1/2}\mathbf{H}^{(l)}\mathbf{W}^{(l)}\right)$$

Where:

- $\hat{A} = A + I$ (adjacency matrix with added self-loops)
- $\hat{\mathbf{D}}$ is the degree matrix of $\hat{A}$
- $\hat{\mathbf{D}}^{-1/2}\hat{A}\hat{\mathbf{D}}^{-1/2}$ is the **symmetric normalisation**
- $\mathbf{W}^{(l)}$ is the learnable weight matrix at layer $l$
- $\sigma$ is a non-linear activation (typically ReLU)

---

## 4.2 Symmetric Normalisation — Why Not Just Mean?

The mean rule uses $\mathbf{D}^{-1}A$ (only the target node's degree for normalisation — **asymmetric**).

GCN uses $\hat{\mathbf{D}}^{-1/2}\hat{A}\hat{\mathbf{D}}^{-1/2}$ — both source and target node degrees normalise the contribution. This means:

- A message from a node $u$ to node $v$ is scaled by $\frac{1}{\sqrt{d_u \cdot d_v}}$
- Low-degree nodes receive more weight than high-degree hub nodes in both directions
- Empirically performs better than simple mean normalisation

---

## 4.3 Dimensionality Through Layers

GCN combines AGGREGATION and UPDATE into a single matrix multiplication, making it elegant and efficient. Here's how dimensions flow:

- Layer 0: $(N \times N)(N \times C)(C \times H) \Rightarrow N \times H$ (reduces from $C$ to $H$ features)
- Layer 1: $(N \times N)(N \times H)(H \times F) \Rightarrow N \times F$ (reduces from $H$ to $F$)

Final embedding dimension $F \ll C$ (original feature dimension). The weight matrices perform learnable dimensionality reduction.

---

## 4.4 Strengths and Weaknesses

**Strengths:**

- **Efficiency:** Relies on sparse matrix multiplications, computationally fast.
- **Simplicity:** Straightforward to implement; relatively few parameters.
- **Strong baseline:** Performs well on node classification benchmarks like Cora, PubMed.

**Weaknesses:**

- **Transductive:** Requires the full graph adjacency matrix for every forward pass. Cannot handle new nodes without retraining.
- **Isotropic aggregation:** Treats all neighbours equally (after normalisation) — cannot differentiate between important and unimportant neighbours. GAT addresses this.
- **Over-smoothing:** Stacking many layers causes node representations to converge to identical values (all nodes become indistinguishable).

---

## 4.5 GCN vs. Simple Message Passing

The elegant insight: GCN compresses the separate AGGREGATE and UPDATE steps into a **single matrix multiplication**. The symmetric normalisation makes it distinct from vanilla message passing where all neighbours contribute equally without any structural weighting.

---

## 4.6 GCN in PyTorch Geometric

Key function: `gcn_norm()` — adds self-loops and applies symmetric normalisation.

Arguments:

- `edge_index`: Node representations as a tensor
- `edge_weight`: Optional 1D edge weights
- `num_nodes`: Dimension of input graph
- `improved`: Alternative self-loop method from Graph U-Nets
- `add_self_loops`: Default is True

`edge_index` is a long tensor of shape `[2, num_edges]` — first row is source nodes, second is destination nodes.

---

## 📝 Q&A — Section 4

**Q1. Write the GCN propagation rule and explain every symbol.**

A: $\mathbf{H}^{(l+1)} = \sigma(\hat{\mathbf{D}}^{-1/2}\hat{A}\hat{\mathbf{D}}^{-1/2}\mathbf{H}^{(l)}\mathbf{W}^{(l)})$. Here: $\hat{A} = A + I$ is the adjacency matrix with self-loops; $\hat{\mathbf{D}}$ is its degree matrix; $\hat{\mathbf{D}}^{-1/2}\hat{A}\hat{\mathbf{D}}^{-1/2}$ is the symmetric normalisation; $\mathbf{H}^{(l)}$ is the node feature matrix at layer $l$; $\mathbf{W}^{(l)}$ is the learnable weight matrix; $\sigma$ is non-linear activation.

---

**Q2. Why does GCN add the identity matrix to A (i.e., use $\hat{A} = A + I$)?**

A: Adding $I$ introduces self-loops. Without them, each node only aggregates from its neighbours, completely ignoring its own current features. Self-loops ensure the node's own representation is included in the aggregation, which is critical for the node to maintain its identity.

---

**Q3. What is the difference between asymmetric normalisation (mean rule) and symmetric normalisation (GCN)?**

A: Mean rule: $\mathbf{D}^{-1}A$ — divides only by the degree of the target node (the node receiving the message). GCN: $\hat{\mathbf{D}}^{-1/2}\hat{A}\hat{\mathbf{D}}^{-1/2}$ — divides by the square root of both source and target degrees. Symmetric normalisation accounts for the degree of the sending node too, giving lower weight to messages from high-degree hub nodes.

---

**Q4. What is over-smoothing in GCN and why does it happen?**

A: Over-smoothing occurs when too many GCN layers are stacked. At each layer, a node's representation becomes the average of its neighbours' representations. After many layers, every node's representation converges to the same global average, making all nodes indistinguishable from each other. The receptive field grows exponentially with layers (covering most of the graph), erasing local distinctions.

---

**Q5. Why is GCN considered transductive, and what limitation does this create?**

A: GCN's forward pass requires multiplying by the full normalised adjacency matrix $\hat{\mathbf{D}}^{-1/2}\hat{A}\hat{\mathbf{D}}^{-1/2}$, which encodes the entire graph's structure. When a new node is added, this matrix changes dimensions entirely, invalidating the learned model. The GCN cannot produce an embedding for the new node without a full retraining.

---

**Q6. Describe how dimensionality reduction happens across GCN layers.**

A: At each layer, the weight matrix $\mathbf{W}^{(l)}$ has dimensions $(d_{in} \times d_{out})$ where $d_{out} < d_{in}$. So Layer 0 transforms from $C$ features (original) to $H$ (hidden), and Layer 1 from $H$ to $F$ (final embedding), where $F \ll C$. The weight matrices progressively compress the representation into a compact, task-relevant embedding.

---

**Q7. In what scenario would you choose GCN over GraphSAGE?**

A: Choose GCN when: (1) the graph is static and fits in memory, (2) you don't need to generalise to unseen nodes, (3) you want a simple, fast baseline for node classification on benchmark datasets like Cora or PubMed, (4) the graph is medium-sized and you don't expect new nodes to appear.

---

**Q8. What does the GCN model depth (number of layers) control in terms of information aggregation?**

A: Each GCN layer increases the receptive field by exactly one hop. A $k$-layer GCN allows each node to aggregate information from nodes up to $k$ hops away. Too few layers: limited context captured. Too many layers: over-smoothing, where all representations converge.

---

# 5. GraphSAGE

## 5.1 Motivation — Fixing GCN's Shortcomings

GraphSAGE (Graph **SA**mpling and a**G**gr**E**gating — Hamilton et al., 2017) directly addresses two GCN shortcomings:

1. **GCN is transductive** — cannot handle unseen nodes.
2. **GCN is expensive on large graphs** — requires the full adjacency matrix.

---

## 5.2 Two Key Innovations

### Innovation 1: Neighbourhood Sampling

Instead of using **all** neighbours of a node (as GCN does), GraphSAGE samples a **fixed number** $k$ of neighbours at each layer. This has two benefits:

- Bounds computational complexity regardless of actual degree
- Reduces dependence on the fixed graph structure → adds inductive capability

### Innovation 2: Generalised Aggregators (Learnable)

GCN has one specific (non-trainable, except via $\mathbf{W}$) aggregation mechanism. GraphSAGE offers multiple learnable aggregator choices:

- **Mean:** Average of sampled neighbours' features (like GCN but sampled)
- **Max:** Element-wise maximum across sampled neighbours
- **LSTM:** Processes an ordered sequence of sampled neighbours (less permutation-invariant but powerful)
- **Sum:** Sum of sampled neighbours

---

## 5.3 The UPDATE in GraphSAGE — Concatenation

This is a crucial difference from GCN. GraphSAGE's UPDATE step **concatenates** the node's own previous embedding with the aggregated neighbourhood message:

$$\mathbf{h}_v^{(l)} = \sigma\left(\mathbf{W}^{(l)} \cdot \text{CONCAT}\left[\mathbf{h}_v^{(l-1)},\ \text{AGGREGATE}\left(\left{\mathbf{h}_u^{(l-1)}: u \in S_{\mathcal{N}(v)}\right}\right)\right]\right)$$

Where $S_{\mathcal{N}(v)}$ is the **sampled** neighbourhood. This gives the node explicit memory of its own identity while incorporating neighbourhood information.

---

## 5.4 Inductive Capability

The critical insight: GraphSAGE **doesn't learn node embeddings** — it learns **aggregator functions**.

The trained aggregator knows: "given any node and a sample of its neighbours, here is how to produce a meaningful embedding." This function can immediately be applied to brand new nodes that appear after training, without any retraining.

---

## 5.5 L2 Normalisation

After each iteration, GraphSAGE normalises the embedding vectors by their L2 norm:

$$\mathbf{h}_v^{(l)} \leftarrow \frac{\mathbf{h}_v^{(l)}}{|\mathbf{h}_v^{(l)}|_2}$$

This prevents the magnitude of embeddings from growing uncontrollably across layers.

---

## 5.6 GraphSAGE vs. GCN — Summary of Differences

|Feature|GCN|GraphSAGE|
|---|---|---|
|UPDATE method|No concatenation (only aggregated)|CONCATENATION of self + aggregated|
|Expressiveness|Fixed, non-learnable aggregation|Learnable aggregator with W matrix|
|Neighbourhood|Full neighbourhood|Fixed-size sampled neighbourhood|
|Setting|Transductive|Inductive|
|L2 normalisation|No|Yes|
|Complexity|Full graph in memory|Mini-batch, bounded|

---

## 5.7 Advanced Aggregation Options

**Multiple aggregations per layer:** You can combine different strategies simultaneously in PyG:

```python
self.conv1 = SAGEConv(in_channels, hidden, aggr=['max', 'sum', 'mean'])
```

**Jumping Knowledge (JK) Networks:** Address over-smoothing by allowing each node to selectively use information from different depths. Instead of only using the last layer's output, JK networks combine outputs from all intermediate layers using cat, max, or LSTM combination modes.

**Layer-specific aggregation:** Use mean aggregation in early layers (smooth features) and max in later layers (identify dominant signals). Useful when the graph has varying sparsity at different structural levels.

---

## 5.8 Real-World Applications

- **Dynamic social networks (Twitter, LinkedIn):** New users appear every second. Inductive GraphSAGE generates their embeddings immediately.
- **Biomedical knowledge graphs:** New proteins/drugs are added as research progresses. GraphSAGE updates or generates embeddings without full retraining.
- **Large-scale recommendation systems (Amazon):** With millions of products, full GCN retraining is impractical. GraphSAGE trains on a subgraph and generalises to new products/users.

---

## 📝 Q&A — Section 5

**Q1. What are the two key innovations of GraphSAGE over GCN?**

A: (1) **Neighbourhood sampling** — instead of using all neighbours, GraphSAGE samples a fixed number $k$ at each layer, bounding computation regardless of node degree. (2) **Generalised learnable aggregators** — replaces GCN's fixed mean with a choice of learnable aggregation functions (mean, max, LSTM, sum), each with a dedicated learnable weight matrix.

---

**Q2. Why does the concatenation in GraphSAGE's UPDATE step improve performance over simple averaging?**

A: Concatenation preserves the node's own identity explicitly in the embedding. A node's previous representation is not diluted into the neighbourhood aggregate — it's kept as a distinct component. This allows the model to separately learn "what this node is" vs. "what its neighbourhood looks like," leading to more expressive representations.

---

**Q3. How does neighbourhood sampling in GraphSAGE contribute to its inductive capability?**

A: By using only a sampled, fixed-size subset of neighbours (not the full fixed neighbourhood), the aggregation function is less tied to the specific structural pattern of any one node. This generalisation means the learned aggregator can work on any node's sampled neighbourhood, including nodes it has never seen before.

---

**Q4. Explain why GraphSAGE's computational complexity is $O(Lbd^2k^L)$ and what each term represents.**

A: $L$ = number of layers (cost scales linearly); $b$ = batch size (number of target nodes); $d^2$ = cost of matrix multiplication at each node (if embedding dimension is $d$); $k^L$ = exponential factor from nested sampling — for $L$ layers each sampling $k$ neighbours, the number of nodes in the computation tree is $k^L$.

---

**Q5. What problem does Jumping Knowledge (JK) Networks solve, and how?**

A: JK Networks solve over-smoothing. As GNN depth increases, node representations become overly similar (smoothed). JK Networks allow each node to "jump" its representations from intermediate layers directly to the final output, then aggregate them. This lets each node selectively use whichever depth best characterises its neighbourhood — some nodes may benefit from 1-hop information, others from 3-hop.

---

**Q6. In a financial transaction network where you need to detect an overall tendency of suspicious activity, which aggregation would you choose and why?**

A: **Mean aggregation.** You want an overall summary of the neighbourhood's tendency. Max would focus too narrowly on the most extreme single transaction. Mean gives you an averaged signal across all connections, appropriate for detecting systemic patterns rather than isolated events.

---

**Q7. What does "the trained outcome of GraphSAGE is a trained AGGREGATOR function" mean in practice?**

A: It means after training, you have weight matrices that define how to combine neighbourhood features — not a lookup table of node embeddings. Given any new node and any sample of its neighbours, you pass their features through these learned weight matrices + activation functions to get the node's embedding. It's a function, not a memory.

---

**Q8. Why does GraphSAGE use L2 normalisation, and what would happen without it?**

A: Without L2 normalisation, the norms of embedding vectors can grow across layers as weighted sums accumulate. This creates scale inconsistency between layers and can cause numerical instability or poor gradient flow. L2 normalisation constrains all embeddings to lie on the unit sphere, ensuring consistent scale.

---

# 6. Graph Attention Network (GAT)

## 6.1 Motivation — Why Attention?

Both GCN and GraphSAGE use **implicit attention** — they weight neighbours based on structural properties (degree) rather than learning which neighbours are actually important for a task.

GAT introduces **explicit, learned attention** — the model learns directly which neighbours deserve more weight for each specific task.

---

## 6.2 The Attention Mechanism

### Step 1: Linear Transformation

Apply a shared weight matrix $\mathbf{W}$ to each node's features: $$\mathbf{h}'_i = \mathbf{W}\mathbf{h}_i$$

### Step 2: Attention Coefficient Computation

Compute a raw attention score for each edge $(i, j)$ using a learnable attention vector $\mathbf{a}$:

$$e_{ij} = \text{LeakyReLU}\left(\mathbf{a}^T \left[\mathbf{W}\mathbf{h}_i | \mathbf{W}\mathbf{h}_j\right]\right)$$

The $|$ denotes concatenation of transformed source and target features.

### Step 3: Normalisation with Softmax

$$\alpha_{ij} = \text{softmax}_j(e_{ij}) = \frac{\exp(e_{ij})}{\sum_{k \in \mathcal{N}(i)} \exp(e_{ik})}$$

This ensures attention scores for all neighbours of $i$ sum to 1.

### Step 4: Weighted Aggregation

$$\mathbf{h}'_i = \sigma\left(\sum_{j \in \mathcal{N}(i)} \alpha_{ij} \mathbf{W}\mathbf{h}_j\right)$$

### Step 5: Multi-Head Attention

To stabilise and diversify the learned representations, run $K$ independent attention heads:

$$\mathbf{h}'_i = |_{k=1}^{K} \sigma\left(\sum_{j \in \mathcal{N}(i)} \alpha_{ij}^{(k)} \mathbf{W}^{(k)}\mathbf{h}_j\right)$$

Concatenate (or average) outputs from all heads.

---

## 6.3 Learnable Parameters in GAT

GAT has **two sets** of learnable parameters:

1. **Weight matrix $\mathbf{W}$** — transforms node features, performs dimensionality reduction (same purpose as in GCN)
2. **Attention vector $\mathbf{a}$** — learned parameters that compute the importance score for each edge

Compare with GCN (only $\mathbf{W}$) and GraphSAGE (only $\mathbf{W}$ for the aggregator function).

---

## 6.4 GAT as a GCN Variant

GATs extend GCNs by making the aggregation **anisotropic** (direction-sensitive) instead of isotropic:

- GCN: $\alpha_{ij} = \frac{1}{\sqrt{d_i \cdot d_j}}$ — determined by structure alone
- GAT: $\alpha_{ij}$ — learned from node features, different for each edge

GAT is still **permutation invariant** by design (the softmax normalisation ensures attention scores are computed independently for each node's neighbourhood set).

---

## 6.5 Advantages

- **Computational efficiency:** Self-attention can be parallelised across all edges; output computation parallelised across all nodes.
- **Inductive capability:** The attention mechanism depends only on local neighbourhood features, not global graph structure. A trained GAT can be applied to different graphs.
- **Expressiveness:** By assigning different importance to neighbours, GAT is more expressive than GCN, often achieving better performance in tasks with complex relational structures (protein interaction networks, knowledge graphs).

---

## 6.6 Trade-offs

- **Computational cost:** Calculating attention coefficients for every edge adds overhead over GCN or mean-aggregation GraphSAGE — especially in very dense graphs.
- However, performance gains often justify the cost.

---

## 6.7 GATConv vs. GATv2Conv

||GATConv|GATv2Conv|
|---|---|---|
|Paper|Veličković et al. (2018)|Brody et al. (2021)|
|Attention type|Static attention|Dynamic attention|
|Attention recomputation|Per graph|Node-specific across layers|
|Expressiveness|Lower|Higher|
|Performance|Good|Better|

GATv2Conv introduces "dynamic attention" — attention scores are recalculated in a node-specific context at each layer, making the model more expressive in how it weights representations during message passing.

---

## 6.8 GCN vs. GraphSAGE vs. GAT — Full Comparison

|Feature|GCN|GraphSAGE|GAT|
|---|---|---|---|
|Aggregation|Fixed Mean (Isotropic)|Flexible Aggregators|Learned Weighted Mean (Anisotropic)|
|Learning Setting|Transductive|Inductive|Inductive|
|Scalability|Limited (full graph)|High (sampling)|Moderate (costly for dense graphs)|
|Expressiveness|Lower|Moderate|Higher|
|Main Strength|Efficient, simple|Scalable, inductive|High performance, adaptive|
|Main Weakness|Not inductive, over-smoothing|Sampling adds noise|Higher compute cost|

---

## 📝 Q&A — Section 6

**Q1. What is the fundamental difference between implicit and explicit attention in GNNs?**

A: Implicit attention (GCN, GraphSAGE) weights neighbours based on pre-determined structural properties like degree ($\frac{1}{\sqrt{d_i d_j}}$) — the weights are computed from graph topology, not learned for the task. Explicit attention (GAT) learns attention coefficients from node features during training, allowing the model to discover which neighbours matter for the specific task.

---

**Q2. Write and explain the attention coefficient formula in GAT.**

A: $e_{ij} = \text{LeakyReLU}(\mathbf{a}^T[\mathbf{W}\mathbf{h}_i | \mathbf{W}\mathbf{h}_j])$. Here: $\mathbf{W}\mathbf{h}_i$ and $\mathbf{W}\mathbf{h}_j$ are the linearly transformed features of source and target nodes; $|$ is concatenation; $\mathbf{a}$ is a learned attention vector (the attention mechanism's parameters); LeakyReLU introduces non-linearity. This score measures how relevant node $j$'s features are to node $i$.

---

**Q3. Why is softmax normalisation applied to attention scores?**

A: Softmax converts raw attention scores $e_{ij}$ into a proper probability distribution summing to 1 over all neighbours of node $i$. This ensures the aggregation is a proper weighted average and provides numerical stability. It also makes the attention scores interpretable as probabilities of importance.

---

**Q4. What is the purpose of multi-head attention in GAT?**

A: Running multiple independent attention heads allows the model to simultaneously attend to different aspects of the neighbourhood. Each head may focus on different feature patterns. The outputs are concatenated (or averaged), giving a richer final representation than any single attention head could produce. It also stabilises training.

---

**Q5. How many sets of learnable parameters does GAT have, and what does each one do?**

A: GAT has two: (1) The weight matrix $\mathbf{W}$ — linearly transforms node features to a new space and performs dimensionality reduction (same role as in GCN). (2) The attention vector $\mathbf{a}$ — learns to compute edge-specific importance scores by combining transformed source and target features.

---

**Q6. Is GAT permutation invariant? Justify your answer.**

A: Yes. The attention scores $\alpha_{ij}$ are computed separately for each individual pair $(i, j)$, then normalised via softmax over all of $i$'s neighbours. This is a set operation — the order in which neighbours are processed doesn't affect the final weighted sum. The softmax ensures that regardless of the enumeration order of neighbours, the output is the same.

---

**Q7. When would you choose GAT over GCN or GraphSAGE?**

A: Choose GAT when: (1) you believe some neighbours are significantly more important than others for the task (heterogeneous relationships), (2) you have sufficient computational resources, (3) you're working with complex relational data like protein-protein interaction networks or knowledge graphs where not all relationships are equally informative.

---

**Q8. What is the key improvement GATv2Conv introduces over GATConv?**

A: GATv2Conv introduces dynamic attention — attention scores are recomputed in a node-specific context at each layer rather than being static. This makes the attention mechanism more expressive because it can adapt its weighting scheme based on the evolving representations of nodes through layers, rather than computing a fixed attention score from the initial features.

---

# 7. GNN Loss Functions & Training Settings

## 7.1 How to Train a GNN

Training a GNN means learning the weight matrices $\mathbf{W}$ by minimising a loss function. This requires:

**Supervised setting:** Minimise the difference between the true label and the label predicted from the node embedding.

**Unsupervised setting:** Make the embeddings structurally meaningful — nearby nodes in the graph should have similar embeddings, distant nodes should have dissimilar embeddings.

---

## 7.2 Supervised Setting — Labelled Nodes

For node classification, the predicted class is:

$$\hat{y} = \text{softmax}(\mathbf{h}_v)$$

Loss: **Cross-Entropy**:

$$\mathcal{L} = -\sum_i \sum_c y_{ic} \log \hat{y}_{ic}$$

where $y_{ic}$ is the true label and $\hat{y}_{ic}$ is the predicted probability for class $c$ of node $i$.

---

## 7.3 Unsupervised Setting — GraphSAGE Loss

The goal: nearby nodes have similar embeddings; far-apart nodes have different embeddings.

$$\mathcal{L} = -\log\sigma(\mathbf{z}_u^T \mathbf{z}_v) - Q \cdot \mathbb{E}_{v_n \sim P_n} \log\sigma(-\mathbf{z}_u^T \mathbf{z}_{v_n})$$

Where:

- $\mathbf{z}_u, \mathbf{z}_v$ are embeddings of a co-occurring pair (positive sample)
- $v_n$ are **negative samples** drawn from distribution $P_n$
- $Q$ is the number of negative samples
- $\sigma$ is the sigmoid function

This is essentially a binary cross-entropy loss encouraging positive pairs to have high dot products and negative pairs to have low dot products.

The trained model produces embeddings stored in a repository, serving as a feature extraction service for downstream tasks. In supervised settings, the unsupervised loss is replaced by cross-entropy.

---

## 7.4 Transductive vs. Inductive Node Classification — Formal Definitions

Three types of nodes in training:

|Type|In Loss|In Message Passing|
|---|---|---|
|Training nodes $V_{train}$|✓|✓|
|Transductive test nodes $V_{trans}$|✗|✓ (edges/features visible)|
|Inductive test nodes $V_{ind}$|✗|✗ (completely hidden)|

**Semi-supervised** = GNN uses transductive test nodes in message passing (their structure is observed, not their labels). The term is applicable because the model "sees" the test nodes during training.

**Inductive node classification** = test nodes and all incident edges are completely unobserved during training (e.g., train on one subgraph of a citation network, test on a disjoint subgraph).

---

## 7.5 Encoder-Decoder Framework

The general network embedding problem can be framed as:

- **Encoder:** Maps nodes (features + structure) to a low-dimensional embedding $Z$
    - Input: Node features $X \in \mathbb{R}^{|V| \times d}$ and/or adjacency matrix $W$
    - Output: Embedding $Z$
- **Decoder:** Reconstructs the original data or makes predictions from $Z$
    - Unsupervised: Reconstruct graph structure (link prediction)
    - Supervised: Predict labels (node classification)

Model parameters $\theta$ are learned through optimising a loss function.

---

## 7.6 Types of Graph Embedding

|Type|Method|Supervised?|Features?|
|---|---|---|---|
|Shallow (Node2Vec, DeepWalk)|Skip-gram, lookup|No|No|
|Neighbour aggregation (GCN, GAT)|Message passing|Both|Yes|
|Graph auto-encoding (GAE, VGAE)|Encoder-decoder|No|Optional|
|Non-Euclidean (HGCN)|Hyperbolic space|Both|Yes|

---

## 7.7 Positional vs. Structural Embeddings

- **Positional embedding:** Preserves relative position of nodes. Random walk-based approaches tend to preserve this. Useful for link prediction and clustering (unsupervised tasks).
- **Structural embedding:** Preserves local structure/roles. GNN-learned representations are structure-aware. Used for node classification and graph classification.

---

## 📝 Q&A — Section 7

**Q1. What is the difference between supervised and unsupervised GNN training?**

A: Supervised training uses node/graph labels to directly optimise a task-specific loss (e.g., cross-entropy for classification). The model learns what makes nodes belong to different classes. Unsupervised training uses structural information only — nearby nodes should have similar embeddings, distant nodes should have dissimilar ones. The output is general-purpose embeddings usable for any downstream task.

---

**Q2. Explain the unsupervised GraphSAGE loss function and why both positive and negative samples are needed.**

A: The loss encourages pairs of nodes that co-occur in random walks (positive pairs — structurally close) to have high dot-product similarity, and pushes randomly sampled node pairs (negative samples — structurally far) to have low similarity. Both are needed: without positive pairs, the model doesn't learn what closeness means; without negative samples, the trivial solution of all embeddings being equal (maximising all dot products) would minimise the loss trivially.

---

**Q3. What is the difference between transductive test nodes and inductive test nodes in GNN training?**

A: Transductive test nodes participate in message passing during training (their structural information propagates through the graph) but their labels are withheld from the loss. The model can use their local context to improve its understanding of the graph. Inductive test nodes are completely hidden — no edges, features, or labels are visible. The model must generalise to them purely from the learned function.

---

**Q4. In the generalised encoder-decoder framework, when would you choose an unsupervised decoder over a supervised one?**

A: Choose unsupervised when: (1) labels are scarce or expensive to obtain, (2) you want general-purpose embeddings usable for multiple downstream tasks, (3) the task is link prediction or graph reconstruction. Choose supervised when you have labels and a specific downstream task like node or graph classification.

---

**Q5. What is the difference between positional and structural graph embeddings? Give a use case for each.**

A: Positional embeddings capture a node's relative position in the graph (like coordinates in space) — useful for link prediction (predict if two nodes should be connected based on proximity) and clustering. Structural embeddings capture the local structural role of a node (is it a hub? A leaf? Part of a triangle?) — useful for node classification (e.g., classifying paper categories by their citation pattern).

---

# 8. Graph Isomorphism & GIN

## 8.1 The Expressiveness Problem

GNNs aim to produce **distinct embeddings for distinct nodes** and **similar embeddings for similar nodes**. But how expressive can a GNN actually be? When do two different graph structures get the same embedding — causing the GNN to fail?

To answer this, we need the concept of **graph isomorphism** and the **Weisfeiler-Lehman (WL) test**.

---

## 8.2 Graph Isomorphism

Two graphs are **isomorphic** (equivalent) if they have the same structure — the only difference is how the nodes are labelled. Formally: there exists a **bijection** (one-to-one and onto mapping) between the vertex sets of the two graphs that preserves all adjacency relationships.

Applications of graph isomorphism:

- **Computational chemistry:** Determine if two molecular graphs are the same compound (critical for drug discovery)
- **Circuit design:** Check if two differently laid-out circuits are functionally identical
- **Pattern recognition:** Match shapes and structures in images
- **Network analysis:** Identify structurally identical networks

---

## 8.3 Injective, Surjective, Bijective — A Quick Recap

- **Injective (one-to-one):** Each input maps to a unique output. No two inputs share the same output.
- **Surjective (onto):** Every output has at least one input mapping to it.
- **Bijective (both):** One-to-one AND onto. Every output has exactly one input.

A graph isomorphism is a bijection between vertex sets preserving adjacency.

---

## 8.4 Weisfeiler-Lehman (WL) Test

Proposed by Weisfeiler & Lehman (1968). It's a **graph colouring algorithm** used to test if two graphs might be isomorphic:

**Steps:**

1. Assign all nodes the same initial colour (label).
2. At each iteration:
    - For each node, collect the multiset of its own colour + its neighbours' colours.
    - Hash this multiset into a new unique colour.
3. Repeat until colours stabilise.
4. Compare the final colour histograms of both graphs:
    - Different histograms → graphs are **definitely NOT isomorphic**
    - Same histograms → graphs are **possibly** isomorphic (but not guaranteed)

**Key limitation:** The WL test is a necessary but NOT sufficient condition. Non-isomorphic graphs can sometimes share the same canonical form.

---

## 8.5 WL Test and GNNs — The Deep Connection

The steps of the WL test are strikingly similar to what a GNN does!

|WL Test|GNN|
|---|---|
|Node colours|Node embeddings|
|Hash function|Aggregation function|
|Iterative recolouring|Message passing layers|
|Comparing colour histograms|Comparing embedding distributions|

This means: **a GNN can be at most as powerful as the WL test** in distinguishing graph structures.

**Now, which aggregation function is best for expressiveness?**

- **Mean:** 10 blue + 10 red nodes → same embedding as 1 blue + 1 red (loses multiset info)
- **Max:** Ignores half the nodes (only keeps the maximum)
- **Sum:** 10 blue + 10 red ≠ 1 blue + 1 red ✓ — preserves full multiset information

**Conclusion: SUM aggregation can discriminate more graph structures than mean or max.**

---

## 8.6 Graph Isomorphism Network (GIN)

Xu et al. (2018) designed GIN specifically to match the power of the 1-WL test.

**Key insight:** Both the AGGREGATE and UPDATE functions must be **injective** (map distinct inputs to distinct outputs). If they're not injective, different neighbourhoods could produce the same embedding — losing discriminative power.

**GIN's formulation:**

$$\mathbf{h}_v^{(l+1)} = \text{MLP}^{(l)}\left(\left(1 + \epsilon^{(l)}\right) \cdot \mathbf{h}_v^{(l)} + \sum_{u \in \mathcal{N}(v)} \mathbf{h}_u^{(l)}\right)$$

Where:

- $\epsilon$ is a learnable parameter (or fixed scalar) controlling the importance of the target node's own embedding vs. its neighbours
- The MLP must have **more than one layer** to distinguish specific graph structures
- The SUM aggregation preserves the multiset structure of the neighbourhood
- The Universal Approximation Theorem guarantees that a sufficiently large MLP can approximate any injective function

---

## 8.7 Why is GIN the Most Expressive Standard GNN?

- **Matches 1-WL:** Proven to be at least as powerful as the 1-WL test in distinguishing non-isomorphic graphs.
- **Overcomes GCN/GraphSAGE limitations:** GCN and GraphSAGE fail to distinguish simple graph structures (e.g., a triangle vs. a cycle of 6 nodes with the same local degree sequence). GIN's injective SUM + MLP solves this.
- **Theoretical guarantee:** With sufficient layers and injective aggregation/readout functions, GIN can distinguish any graphs the 1-WL test can.

---

## 8.8 Limitations of GIN

- **1-WL Ceiling:** GIN cannot distinguish graphs that the 1-WL test itself fails on (e.g., certain regular graphs where all nodes have the same local neighbourhood structure).
- **Beyond 1-WL:** To be more expressive than GIN, you need higher-order methods (k-WL, subgraph GNNs, or structural features like cycle counts/spectral features).
- GIN is the most expressive among standard message-passing GNNs, but not among all GNN architectures overall.

---

## 📝 Q&A — Section 8

**Q1. What is graph isomorphism and why is it important for GNNs?**

A: Two graphs are isomorphic if they have the same structure (same connections) up to node relabelling — there exists a bijection between their vertex sets preserving all edges. For GNNs, this is important because a GNN should assign different embeddings to structurally different nodes/graphs. The GNN's ability to distinguish non-isomorphic graphs determines its expressiveness.

---

**Q2. Describe the WL test algorithm step by step.**

A: (1) Assign all nodes the same initial colour. (2) At each iteration, for every node, collect the multiset of its own colour + all neighbours' colours. Hash this multiset into a new unique colour. (3) Repeat until no colour changes (stable colouring). (4) Compare colour histograms of both graphs — different histograms → definitely not isomorphic; same histograms → possibly isomorphic. The test provides a necessary but not sufficient condition.

---

**Q3. Why does sum aggregation have more discriminative power than mean or max in the context of the WL test?**

A: Sum aggregation preserves the full multiset structure of the neighbourhood. 10 blue neighbours + 10 red neighbours will produce a different sum than 1 blue + 1 red. Mean aggregation loses count information (both give 0.5 blue + 0.5 red). Max aggregation ignores most neighbours (only the element-wise maximum survives). Only sum can encode both the identity and multiplicity of neighbours.

---

**Q4. What makes GIN's aggregation and update functions injective, and why is injectivity important?**

A: GIN uses SUM aggregation (preserves multisets) and a multi-layer MLP for transformation (by the Universal Approximation Theorem, can approximate any injective function). Injectivity is critical because non-injective functions map distinct inputs to the same output — different neighbourhood structures produce identical embeddings, destroying the GNN's ability to distinguish non-isomorphic graphs.

---

**Q5. What is the role of the $\epsilon$ parameter in GIN?**

A: $\epsilon$ balances the contribution of the target node's own embedding versus its neighbours' sum. It scales the self-feature term: $(1 + \epsilon) \cdot \mathbf{h}_v$. If $\epsilon = 0$, the node's own embedding is treated equally with those of its neighbours. If $\epsilon$ is large, the node's own identity dominates. It can be a fixed scalar or a learnable parameter, giving flexibility in how much self-information is weighted.

---

**Q6. What is the 1-WL ceiling, and what approaches can break it?**

A: The 1-WL ceiling means GIN (and all standard message-passing GNNs) cannot distinguish certain regular graphs where all nodes have identical local neighbourhoods at every depth. To break this ceiling: (1) **k-WL test** — generalised Weisfeiler-Lehman using $k$-tuples of nodes; (2) **Subgraph GNNs** — compute embeddings over subgraphs; (3) **Structural features** — inject hand-crafted features like cycle counts or spectral information.

---

**Q7. In what way is the message passing process in a GNN analogous to the WL test?**

A: In the WL test, each node's colour is updated by hashing its own colour + the multiset of neighbour colours. In a GNN, each node's embedding is updated by transforming its own embedding + the aggregate of neighbour embeddings. Both processes are iterative, both use neighbourhood information to refine node representations, and both converge to a stable state. Node colours ↔ node embeddings; hash function ↔ aggregation + MLP.

---

# 9. Graph Auto-Encoder (GAE) & Variational GAE (VGAE)

## 9.1 Generative vs. Discriminative Models — Recap

||Discriminative|Generative|
|---|---|---|
|Goal|Learn $P(Y|X)$|
|Output|Class boundary|Data distribution|
|Example|GNN classifier|VAE|
|Strength|Efficient, accurate|Can generate new data|

Generative models are useful when labelling data is expensive but generating new data is easy (e.g., drug discovery — generating candidate molecules with desired properties).

---

## 9.2 Autoencoders — Recap

An autoencoder has two components:

- **Encoder:** Compresses data $X$ → **latent space** $Z$ (low-dimensional representation)
- **Decoder:** Reconstructs $X$ from $Z$

Limitation: Regular autoencoders map data to **discrete points** in latent space. Sampling between points may hit meaningless regions, causing poor generative performance. The latent space is not structured.

---

## 9.3 Variational Autoencoders (VAE) — Recap

VAE improves this by making the latent space **continuous and structured**:

- The encoder outputs a **probability distribution** (mean $\mu$ and variance $\sigma^2$) for each latent dimension, not a fixed point.
- The decoder samples from this distribution to reconstruct input.
- **Regularity:** Ensures continuity (nearby points → similar reconstructions) and compactness (any latent point → meaningful output).
- **KL Divergence Loss:** Regularises the learned distribution toward a standard Gaussian prior. Forces the latent space to be smooth and well-organised.

$$\mathcal{L} = \underbrace{\mathcal{L}_{\text{reconstruction}}}_{\text{how well we reconstruct}} + \underbrace{D_{KL}(q(Z|X) | p(Z))}_{\text{how close latent dist. is to Gaussian}}$$

---

## 9.4 Graph Autoencoder (GAE) for Link Prediction

**Task:** Given a graph, predict which edges are missing (link prediction).

**Training Setup:**

1. Hide a subset of edges from the graph (these become the "missing" links to predict).
2. Encoder: GCN layers compress node features + graph structure into embedding $Z$.
3. Decoder: Inner product of node embeddings predicts edge existence: $\hat{A}_{ij} = \sigma(\mathbf{z}_i^T \mathbf{z}_j)$
4. Loss: Binary cross-entropy between predicted and actual adjacency (positive + negative samples).
5. Optimise until reconstruction loss is minimised.

**PyG Implementation Steps:**

1. Define encoder (two GCNConv layers)
2. Decoder (inner product, built into base GAE class)
3. Prepare data with positive/negative edge splits
4. Train with Adam optimiser + BCE loss
5. Evaluate on test edge set

---

## 9.5 Variational Graph Autoencoder (VGAE)

VGAE extends GAE to produce **probabilistic embeddings**:

- Encoder outputs mean vector $\mu_i$ and log-variance $\log\sigma_i^2$ for each node
- Final embedding is sampled: $\mathbf{z}_i \sim \mathcal{N}(\mu_i, \sigma_i^2 I)$
- Loss = Reconstruction Loss + KL Divergence

In PyG, `VGAE` class directly implements this with `encode()`, `decode()` methods. The final node embedding used downstream is $\mu$ (the mean).

---

## 9.6 GAE vs. VGAE — When to Use Which

||GAE|VGAE|
|---|---|---|
|Latent space|Deterministic|Probabilistic|
|Best for|Specific reconstruction tasks (link prediction, node classification)|Generating diverse new samples|
|Overfitting|More susceptible|Less susceptible (variational regularisation)|
|Speed|Faster|Slower|
|Noisy data|Less robust|More robust|
|Use case|Clear graph structure|Noisy data; drug molecule generation|

**Drug molecule generation:** VGAE is preferred because the structured latent space allows interpolation and sampling of novel molecules with desired properties (QED score, LogP, SAS).

---

## 9.7 Case Study: Protein-Protein Interaction (PPI) Prediction

**Problem:** Predict which proteins physically interact.

**Pipeline:**

1. **Sequence features:** Extract Quasi-Sequence-Order descriptors from protein sequences; apply SNLF to get sparse latent factors; reduce with PCA → feature matrix $X$
2. **Network topology:** Construct higher-order adjacency matrix $Q$ using Personalised PageRank (transition probabilities) + topological similarity matrix
3. **VGAE:** Use $X$ (features) and $Q$ (structure) as input to VGAE encoder (two GCN layers). Encoder produces $\mu$ and $\sigma$ for each protein. Decoder reconstructs interactions.
4. **Prediction:** Use trained VGAE to get protein embeddings, then apply classical ML classifiers (RF, SVM, KNN, etc.)

---

## 📝 Q&A — Section 9

**Q1. What is the key limitation of a regular autoencoder that the VAE solves?**

A: A regular autoencoder maps inputs to discrete points in latent space. The space between these points is unstructured and may contain meaningless regions. VAE solves this by mapping inputs to probability distributions (Gaussians), making the latent space continuous, smooth, and regularised toward a standard Gaussian. Any point in the VAE latent space decodes to a meaningful output.

---

**Q2. Explain how a GAE is used for link prediction. What is the encoder-decoder structure?**

A: The encoder uses GCN layers to transform node features and graph structure into low-dimensional embeddings $Z$. The decoder computes the probability that two nodes share an edge using the inner product of their embeddings: $\hat{A}_{ij} = \sigma(\mathbf{z}_i^T \mathbf{z}_j)$. Missing edges (negative samples) are created artificially. The model is trained to reconstruct the original adjacency matrix, forcing the embeddings to preserve link structure.

---

**Q3. What are the two loss terms in VGAE and what does each one enforce?**

A: (1) **Reconstruction loss** — measures how well the decoder reconstructs the original adjacency matrix from the sampled latent embeddings (binary cross-entropy or MSE). Forces the encoder to preserve graph structure. (2) **KL divergence loss** — measures how far the learned latent distribution $q(Z|X)$ is from a standard Gaussian prior $p(Z) = \mathcal{N}(0, I)$. Forces the latent space to be smooth and regular, enabling generation.

---

**Q4. Why is VGAE preferred for drug molecule generation over GAE?**

A: Drug generation requires sampling novel molecules from the latent space. GAE's latent space is deterministic — there's no principled way to sample new points. VGAE's probabilistic, regularised latent space has a smooth structure where you can sample any point and decode it to a valid (or at least meaningful) molecular graph. This continuity is essential for exploring the chemical space.

---

**Q5. In the PPI case study, why was a higher-order adjacency matrix used instead of the standard adjacency matrix?**

A: The standard adjacency matrix only captures direct (1-hop) connections. In protein interaction networks, functionally related proteins may not be directly connected but share similar network positions (topological similarity) or have high visiting probabilities (Personalised PageRank). The higher-order adjacency matrix captures these indirect but meaningful relationships, providing richer structural information to the VGAE encoder.

---

**Q6. What is the difference in what the encoder outputs between GAE and VGAE?**

A: GAE encoder outputs a single fixed embedding vector $\mathbf{z}_i$ for each node (deterministic). VGAE encoder outputs two vectors per node — a mean vector $\mu_i$ and a log-variance vector $\log\sigma_i^2$ — parametrising a Gaussian distribution. The actual embedding is sampled from this distribution during training; at inference time, $\mu_i$ is used as the point embedding.

---

# 10. Spatio-Temporal GNNs (Dynamic Graphs)

## 10.1 Why Dynamic Graphs?

Standard GNNs assume **static graphs** — structure that doesn't change over time. But most real-world graphs are dynamic:

- Social networks: friends added/removed
- Financial networks: new transactions every second
- Road networks: traffic congestion changes minute by minute
- Body skeleton: joints move continuously

When dynamic behaviour is ignored, important temporal insights are lost.

---

## 10.2 DTDG vs. CTDG

||DTDG (Discrete Time Dynamic Graph)|CTDG (Continuous Time Dynamic Graph)|
|---|---|---|
|Representation|Snapshots at fixed time intervals|Stream of timestamped events|
|Detail loss|Yes (misses events between snapshots)|No (every event recorded)|
|Computational cost|Lower|Higher|
|Best for|Slowly changing graphs|Real-time, event-driven graphs|

---

## 10.3 Spatio-Temporal Graph Architecture

Three modules:

1. **Data Processing Module:** Constructs the spatio-temporal graph from raw data.
    
2. **Spatio-Temporal Graph Learning Module:** Combines:
    
    - **Spatial learning:** GCN, Spatial GCN, or GAT for node-level relationships
    - **Temporal learning:** RNN/LSTM, **TCN (Temporal Convolution Network)** — 1D convolution with dilated filters (shown to outperform RNN), or **TSAN (Temporal Self-Attention Network)** for long-range temporal dependencies
3. **Task-Aware Prediction Module:** Maps spatio-temporal representations to downstream predictions.
    

---

## 10.4 Graph Construction for Urban Computing

When no predefined topology exists, graphs must be constructed from data:

- **Distance-based:** Use Gaussian Radial Basis Function (RBF): $w_{ij} = \exp(-d_{ij}^2 / \sigma^2)$. Small $\sigma$ → only close nodes connected; large $\sigma$ → broader connectivity.
- **Similarity-based:** Use Pearson Correlation Coefficient (PCC) or Dynamic Time Warping (DTW) between time series of different locations.
- **Interaction-based:** Use actual flow (mobility) between locations as edge weights.

---

## 10.5 STGNN for Traffic Prediction

**Graph structure:** Intersections = nodes; roads = edges. **Spatial component (GNN):** Captures how traffic at connected intersections influences each other. **Temporal component (LSTM/TCN):** Captures how traffic at each intersection evolves over time.

Together, STGNNs predict future traffic conditions accounting for both where you are (spatial) and when it is (temporal).

---

## 10.6 ST-GCN for Human Action Recognition

**Problem:** Recognise human actions (walking, jumping) from motion capture data. **Graph structure:** Body joints = nodes; bones = edges. **Spatial:** GCN captures which joints are connected (skeleton topology). **Temporal:** Captures how joints move over frames.

Pure sequence models (LSTM without graph) miss relational information between joints — e.g., the wrist can't move without the elbow and shoulder. ST-GCN (Yan et al., 2018) formalises this as a spatio-temporal graph.

**CMU Motion Capture Dataset:** 41 sensors from toe to neck per subject, recording xyz coordinates + velocity at each timestep for various activities (walking, running, sports).

---

## 10.7 Applications of Spatio-Temporal GNNs

**Transportation:** Traffic incident prediction, travel time prediction, trajectory prediction (autonomous driving)

**Environment:** Air quality prediction, meteorological prediction

**Public Safety:** Crime frequency prediction, disaster situation prediction

**Public Health:** Ambulance demand prediction, epidemic prediction

---

## 📝 Q&A — Section 10

**Q1. Why are standard static GNNs insufficient for modelling social networks?**

A: Social networks continuously evolve — users join and leave, connections form and dissolve, content propagates. A static GNN assumes a fixed snapshot, missing the temporal dynamics entirely. Whether someone is an influencer depends not just on who they know but when and how fast they gained connections. Static models lose this temporal signal.

---

**Q2. What is the difference between DTDG and CTDG?**

A: DTDG takes snapshots at fixed intervals (e.g., daily graph states) — simple to process but loses events between snapshots. CTDG records every graph event with its exact timestamp as a continuous stream — accurate to every interaction but more computationally demanding. CTDG is more realistic; DTDG is a practical approximation for slowly changing systems.

---

**Q3. Why is a TCN (Temporal Convolution Network) often preferred over RNN/LSTM for temporal modelling in STGNNs?**

A: TCNs apply 1D dilated convolutions over the temporal dimension. Dilated convolutions exponentially expand the receptive field without proportionally increasing depth, allowing efficient capture of long-range temporal dependencies. Empirically, TCNs have been shown to outperform RNNs/LSTMs on many sequence tasks while being more parallelisable (unlike sequential RNNs) and avoiding vanishing gradient issues.

---

**Q4. Describe how a Gaussian RBF kernel is used to construct a spatial graph for traffic prediction.**

A: For $N$ road sensors, compute pairwise distances $d_{ij}$ between all pairs. Apply the Gaussian kernel: $w_{ij} = \exp(-d_{ij}^2 / \sigma^2)$. The bandwidth $\sigma$ controls connectivity — small $\sigma$ means only very close sensors are strongly connected; large $\sigma$ gives broader, smoother connectivity. The resulting weighted adjacency matrix $W$ captures how physically close locations influence each other.

---

**Q5. Why is human action recognition treated as a dynamic graph problem rather than a simple classification problem?**

A: Action recognition is inherently temporal — you need to observe the sequence of movements, not just a single frame. It also has a relational structure — joints are physically connected (wrist depends on elbow which depends on shoulder). A static graph CNN handles spatial dependencies; a classification model handles spatial patterns in one frame. Only a dynamic GNN (ST-GCN) can simultaneously model both the skeletal connectivity and the temporal evolution of movement.

---

**Q6. In the encoder-decoder perspective for dynamic graphs, what does the encoder produce at each time step?**

A: The encoder maps the dynamic graph (its state including nodes, edges, features, and all prior events) to node embeddings $Z_i(t)$ for each time $t$. These embeddings encode both the spatial structure of the graph and the temporal context of how the graph has evolved up to time $t$.

---

# 11. Heterogeneous Graphs & R-GCN

## 11.1 Homogeneous vs. Heterogeneous Graphs

||Homogeneous|Heterogeneous|
|---|---|---|
|Node types|One|Multiple (e.g., User, Movie, Genre)|
|Edge types|One|Multiple (e.g., "watches", "belongs to")|
|Message passing|Same for all|Different per type|
|Challenge|Simple|Features have different meaning & dimensionality|

**Key challenge of heterogeneous graphs:** Features of different node/edge types are not comparable — merging them directly destroys information.

---

## 11.2 Relational Graph Convolutional Network (R-GCN)

R-GCN handles graphs with **multiple edge (relation) types** within a potentially homogeneous node set.

**Key idea:** Use different weight matrices for different relation types.

$$\mathbf{h}_v^{(l+1)} = \sigma\left(\mathbf{W}_0^{(l)}\mathbf{h}_v^{(l)} + \sum_{r \in \mathcal{R}} \sum_{u \in \mathcal{N}_r(v)} \frac{1}{c_{v,r}} \mathbf{W}_r^{(l)}\mathbf{h}_u^{(l)}\right)$$

Where:

- $\mathcal{R}$ is the set of relation types
- $\mathcal{N}_r(v)$ is the set of neighbours connected to $v$ via relation $r$
- $c_{v,r}$ is a normalisation constant (degree w.r.t. relation $r$)
- $\mathbf{W}_r^{(l)}$ is a separate learnable weight matrix per relation type per layer
- $\mathbf{W}_0^{(l)}$ handles the self-loop

**Distinction:** RGCN is NOT necessarily heterogeneous — it can have one node type but multiple edge types.

---

## 11.3 Heterogeneous Information Network (HIN)

Formally: $G = (V, E, A, R, \theta, \phi)$ where $A$ is the set of node types, $R$ is the set of relation types, $\theta$ maps nodes to types, $\phi$ maps edges to types.

---

## 11.4 Metapaths

A **metapath** is a sequence of node types defining a semantically meaningful composite relationship:

$$A_1 \xrightarrow{R_1} A_2 \xrightarrow{R_2} A_3 \cdots \xrightarrow{R_l} A_{l+1}$$

**Example (gaming graph):**

- UGU = User → Game → User (users who play the same game)
- UGDGU = User → Game → Developer → Game → User (users who play games by the same developer)

Metapaths are used to:

- Define semantically meaningful neighbourhoods
- Decompose heterogeneous graphs into multiple homogeneous subgraphs

---

## 11.5 MetaPath2Vec

Adapts Node2Vec/DeepWalk to heterogeneous graphs:

1. Perform **metapath-guided random walks** (follow predefined type sequences instead of random hops)
2. Train a **heterogeneous Skip-gram** model on these walks
3. Output: type-aware node embeddings

---

## 11.6 Heterogeneous GNN with Metapaths

Approach:

1. Split the heterogeneous graph into multiple **simple homogeneous subgraphs** — one per metapath schema.
2. Apply graph filtering (GCN/GAT) independently on each subgraph.
3. Combine the resulting representations through an **attention mechanism** — learns which metapath is most informative for each node.

---

## 11.7 Application: Review Sentiment Analysis with R-GCN

- Nodes: words + documents
- Edges:
    - Word-Word: PMI (Pointwise Mutual Information) from co-occurrence
    - Word-Document: TF-IDF score
    - Document-Document: Jaccard similarity
- R-GCN uses separate weight matrices for each edge type, allowing it to treat these fundamentally different relationships separately.

---

## 📝 Q&A — Section 11

**Q1. Why can't you simply apply a standard GCN directly to a heterogeneous graph?**

A: In a heterogeneous graph, different node types have features with different meanings and potentially different dimensions. A single weight matrix $\mathbf{W}$ assumes all features are in the same semantic space and can be combined. Merging features of different types (e.g., user profile features with movie genre features) with the same $\mathbf{W}$ destroys the type-specific semantics. Also, different edge types carry different relational information that shouldn't be aggregated identically.

---

**Q2. How does R-GCN handle multiple relation types?**

A: R-GCN assigns a separate learnable weight matrix $\mathbf{W}_r$ to each relation type $r$. When aggregating messages for a node, it applies the appropriate relation-specific matrix to each incoming message based on which relation type the edge belongs to, then sums across all relation types. This allows the model to distinguish how information flows differently through different types of relationships.

---

**Q3. What is a metapath and why is it important in heterogeneous GNNs?**

A: A metapath is a sequence of node types and edge types defining a composite semantic relationship (e.g., User → Movie → Genre → Movie → User). It's important because in a heterogeneous graph, direct connections often don't capture all meaningful relationships. Metapaths define higher-level, semantically interpretable pathways — "users who watch movies of the same genre" captures a different relationship than direct user-user edges. They provide a way to decompose heterogeneous graphs into homogeneous subgraphs for standard GNN processing.

---

**Q4. What is the difference between RGCN and a heterogeneous GCN?**

A: RGCN handles graphs with multiple edge types but can have a single node type — the heterogeneity is only in the relations. A full heterogeneous GCN handles graphs where both node types AND edge types are multiple. RGCN is a special case; a proper heterogeneous GNN must handle the dimensional incompatibility of features from different node types through type-specific projection layers.

---

**Q5. How does MetaPath2Vec extend Node2Vec for heterogeneous graphs?**

A: Node2Vec uses random walks on homogeneous graphs, treating all nodes and edges identically. MetaPath2Vec uses metapath-guided random walks that follow predefined type sequences, ensuring the walk visits nodes in a semantically meaningful order (e.g., always following User → Movie → Genre sequences). A heterogeneous Skip-gram model trained on these walks produces type-aware embeddings that capture the complex relational structure of the HIN.

---

# 12. Graph Transformer Networks

## 12.1 Recap: Self-Attention in Standard Transformers

The Transformer architecture (Vaswani et al., 2017) processes sequences using self-attention:

1. **Positional encoding:** Adds position information to tokens using sinusoidal functions (low frequencies → global relationships; high frequencies → local relationships).
2. **Q, K, V transformation:** Input features are projected into Query, Key, and Value spaces via learned weight matrices.
3. **Attention scores:** $\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$ — every token attends to every other token.
4. **Multi-head attention:** $K$ parallel attention heads, concatenated, capturing diverse aspects.
5. **Feed-forward network (FFN):** Two FC layers with non-linearity applied after attention.
6. **Residual connections + Layer normalisation:** Ensure training stability.

---

## 12.2 Transformer vs. Graph Transformer

|Aspect|NLP Transformer|Graph Transformer|
|---|---|---|
|Tokens/nodes|Words (tokens)|Graph nodes|
|Data structure|Ordered sequence|Unordered graph|
|Attention scope|Fully connected (all pairs)|Graph topology constrained|
|Positional encoding|Sinusoidal (absolute position)|Graph-aware (Laplacian eigenvectors)|
|Edge awareness|None|Can use edge features|
|Complexity|$O(N^2)$|Can be reduced via constraints|

---

## 12.3 Graph Transformer Layer

1. **Input graph:** Nodes ${X_1, X_2, \ldots, X_n}$ with edges.
2. **Linear transformations:** $Q = XW_Q$, $K = XW_K$, $V = XW_V$
3. **Positional encoding:** Graph-aware positional encoding (Laplacian eigenvectors) added to encode structural position, unlike NLP where it encodes sequential position.
4. **Attention calculation:** Same as standard Transformer, but potentially restricted to neighbourhood edges.
5. **Output:** Node representations used for classification, regression, or generation.

---

## 12.4 Edge Features in Graph Transformers

Unlike NLP, graphs have explicit edge information (bond types in molecules, interaction strengths in social networks). Different Graph Transformer variants incorporate edge information:

- **Key-query similarity:** Edge features bias the attention score computation
- **Edge-aware message passing:** Edge features transform the value/message

Ignoring edge features would mean losing critical structural context (e.g., in molecular graphs, whether a bond is single, double, or aromatic completely changes the molecule's properties).

---

## 12.5 Laplacian Eigenvector Positional Encoding (LPE)

The Laplacian matrix: $L = D - A$ (degree matrix minus adjacency matrix).

Properties:

- Positive semi-definite: all eigenvalues $\lambda \geq 0$
- Solutions to $Lu = \lambda u$ give eigenvectors $u$
- **Small eigenvalues → eigenvectors vary slowly** → encode global relationships
- **Large eigenvalues → eigenvectors vary rapidly** → encode local relationships

**Insight:** Representing a sentence as a linear chain graph and computing its Laplacian eigenvectors recovers the same sine/cosine positional encodings as the original Transformer. LPE is a generalisation of sinusoidal encoding to arbitrary graph topologies.

---

## 12.6 Computational Complexity Challenge

- Standard Transformer attention: $O(N^2)$ — every node attends to every other
- Laplacian eigenvector computation: $O(N^3)$ — prohibitively expensive for large graphs

**Solutions being explored:**

- **Sparse/localised attention:** Nodes attend only to their neighbours, reducing $O(N^2)$ toward $O(N \cdot d)$
- **Linear attention (NodeFormer):** Kernelised Gumbel-Softmax approximates attention in linear time
- **Graph coarsening (GapFormer, ASN-GT):** Merge nodes to reduce total count
- **Sampling strategies:** Process subgraphs
- **NAGphormer/VCR-graphormer:** Create a per-node sequence (from neighbourhood hops) and feed it to a Transformer

---

## 12.7 GNN vs. Graph Transformer

|Aspect|GNN|Graph Transformer|
|---|---|---|
|Information flow|Local (layer by layer, 1 hop per layer)|Global (all pairs attend to each other)|
|Long-range dependencies|Poor (requires many layers)|Strong (directly)|
|Over-smoothing|Susceptible (features converge with depth)|Less susceptible (nodes retain individual characteristics)|
|Over-squashing|Yes (information compressed in bottleneck nodes)|Less (attention maintains fine-grained structure)|
|Scalability|Generally better|More challenging (O(N²))|
|Best for|Large, dense graphs|Small, sparse graphs needing long-range relationships|

---

## 📝 Q&A — Section 12

**Q1. What is the key distinction between how a standard Transformer and a Graph Transformer use attention?**

A: A standard Transformer assumes a fully connected graph — every token attends to every other token equally (before masking). A Graph Transformer constrains or biases attention using the graph's actual topology — nodes may only attend to their neighbours or attention scores are modulated by graph structure. This makes attention topology-aware rather than uniform.

---

**Q2. Why can't you use sinusoidal positional encoding directly for graph nodes?**

A: Sinusoidal encoding assigns positions based on sequential order (1st, 2nd, 3rd token in a sequence). Graph nodes have no natural ordering — there's no "position" in a linear sense. Nodes need a positional encoding derived from the graph's structure itself. Laplacian eigenvectors serve this purpose, encoding each node's position relative to the graph's connectivity pattern.

---

**Q3. Explain why Laplacian eigenvectors encode both local and global graph structure.**

A: Laplacian eigenvectors with smaller eigenvalues (near 0) vary slowly across the graph — adjacent nodes have similar values. These encode global structure, like how far a node is from the "center" of the graph. Eigenvectors with larger eigenvalues vary rapidly — adjacent nodes may have very different values. These encode local structure, distinguishing nodes with different immediate neighbourhoods.

---

**Q4. What is over-squashing in GNNs, and how do Graph Transformers address it?**

A: Over-squashing occurs when a node's embedding must encode information from an exponentially growing neighbourhood as depth increases, but the fixed embedding dimension can't hold all this information — crucial details are "squashed" out. Graph Transformers address this through self-attention, which can directly attend to any node regardless of distance, without routing information through potentially bottleneck intermediate nodes.

---

**Q5. Why is the $O(N^2)$ complexity of Graph Transformers a practical problem, and what is one approach to mitigate it?**

A: For graphs with millions of nodes (social networks, citation graphs), $O(N^2)$ attention means computing $N^2$ attention scores — for $N = 10^6$, that's $10^{12}$ computations, completely infeasible. Sparse localised attention mitigates this by restricting each node to attend only to its graph neighbours, reducing complexity to $O(N \cdot \bar{d})$ where $\bar{d}$ is average degree.

---

**Q6. When would you choose a Graph Transformer over a standard GNN?**

A: Choose Graph Transformer when: (1) the graph is small or medium-sized, (2) long-range dependencies between distant nodes are critical (molecular graphs where atoms interact across the whole molecule), (3) you have sufficient computation, (4) you need to model complex global patterns that can't be captured by sequential local message passing.

---

# 13. GNN Interpretability & GNNExplainer

## 13.1 Interpretability vs. Explainability

**Interpretability:** Understanding cause and effect within the AI model. Enables experts to estimate model predictions, understand how inputs affect outputs, and detect errors. Mostly for technical experts.

**Explainability:** Goes beyond interpretability — communicates in human-readable form WHY a model made a decision, bridging AI to law, ethics, and human-computer interaction. Requires interpretability as a foundation.

---

## 13.2 Types of Interpretability

|Dimension|Options|
|---|---|
|Scope|**Local** (single instance) vs. **Global** (entire model)|
|Model dependency|**Model-specific** (e.g., GNNExplainer) vs. **Model-agnostic** (LIME, SHAP)|
|Timing|**Intrinsic** (white-box: decision tree, linear regression) vs. **Post-hoc** (applied after training: Grad-CAM, GNNExplainer)|

---

## 13.3 Unique Challenges of GNN Interpretability

1. **Contextual understanding:** Explanations must account for relationships between nodes, not just individual feature values.
2. **Dynamic relationships:** Graph edges change over time — explanations may not be stable.
3. **Heterogeneous data:** Multiple node/edge types require unified but differentiated explanations.
4. **Dual granularity:** Explanations must identify BOTH important structure (which nodes/edges/subgraphs matter) AND important features.

---

## 13.4 GNNExplainer

For a trained GNN making prediction $\hat{y}$ for node $v$, GNNExplainer identifies:

- A small **subgraph** $G_s$ of the computation graph
- A **subset of node features** $X_s$

Such that $G_s$ and $X_s$ together are the most responsible for the prediction.

**Mathematical objective:** Maximise mutual information (MI) between the explanation subgraph and the full model prediction:

$$\max_{G_s, X_s} MI(Y, (G_s, X_s)) = H(Y) - H(Y | G_s, X_s)$$

Where $H(Y|G_s, X_s)$ should be low (having $G_s$ should make the prediction as certain as the full graph $G$).

**Type:** Post-hoc, local interpretability technique.

---

## 13.5 Limitations of GNNExplainer

1. **Local only:** Explains single instances — hard to generalise explanations to other nodes.
2. **No global understanding:** Cannot explain the model's overall behaviour.
3. **Must be retrained per instance:** Computationally expensive and impractical for large graphs.

---

## 13.6 Explainability Metrics

|Metric|Description|
|---|---|
|Accuracy / F1 / ROC-AUC|For datasets with ground-truth explanations|
|**Fidelity**|How much does removing the explanatory subgraph change the prediction? High fidelity = subgraph is truly important|
|**Contrastivity**|Hamming distance between explanations for different classes — good explainers highlight different features for different classes|
|**Unfaithfulness (GEF)**|KL divergence between predictions from full graph vs. masked subgraph — measures how faithful the explanation is to the underlying model|

---

## 📝 Q&A — Section 13

**Q1. What is the difference between interpretability and explainability in the context of GNNs?**

A: Interpretability is about understanding the model's internal mechanics — which inputs affect which outputs and how, at a technical level. Explainability translates this into human-readable, contextualised justifications understandable by non-experts, incorporating considerations from law, ethics, and HCI. Interpretability is a prerequisite for explainability.

---

**Q2. What does GNNExplainer output for a given node prediction, and how is this useful?**

A: GNNExplainer outputs a small subgraph $G_s$ of the computation graph and a subset of important node features $X_s$. Together, these capture the minimal structural and feature information most responsible for the prediction $\hat{y}$. This helps a user understand: "This node was classified as X because it's connected to these specific nodes (with these specific features)."

---

**Q3. Why is GNNExplainer formulated as a mutual information maximisation problem?**

A: Mutual information measures how much knowing the explanatory subgraph reduces uncertainty about the prediction. If the explanation $G_s$ is good, knowing $G_s$ should let us predict $\hat{y}$ as confidently as knowing the entire graph $G$. Maximising MI = finding the smallest subgraph that contains all the information the GNN actually used to make its decision.

---

**Q4. What is fidelity as an explainability metric, and why is it important?**

A: Fidelity measures how critical the explanatory subgraph is to the model's prediction. It comes in two flavours: fidelity- (give only the subgraph to the model — prediction should stay the same) and fidelity+ (remove the subgraph from the full graph — prediction should change significantly). High fidelity means the explanation truly captures what the model relied on, not just a correlation. Low fidelity means the explanation is misleading.

---

**Q5. What is contrastivity, and what does a high contrastivity score indicate?**

A: Contrastivity uses Hamming distance to compare explanations generated for the same instance but for different predicted classes. A high contrastivity score means the model highlights genuinely different features/subgraphs when predicting different classes — the explanation is discriminative. Low contrastivity indicates the model's explanations look the same regardless of class, suggesting the explainer is not class-sensitive and therefore less useful.

---

# 14. GNN Modelling: Complexity, Pooling, Layers, Augmentation

## 14.1 Complexity Analysis

### GCN Complexity

**Time (forward pass):** $O(L \cdot N \cdot d^2)$

- $L$ = layers
- $N$ = nodes (scales linearly)
- $d^2$ = quadratic in feature dimension (from $d \times d$ weight matrix)

**Space:** $O(L \cdot N \cdot d + L \cdot d^2)$

- $LNd$ = storing all node embeddings at all layers
- $Ld^2$ = storing all weight matrices

### GraphSAGE Complexity

**Time:** $O(L \cdot b \cdot d^2 \cdot k^L)$

- $b$ = batch size
- $k^L$ = exponential neighbourhood expansion from $k$ sampled neighbours per layer

**Space:** $O(b \cdot k^L)$ — crucially **independent of total graph size $N$**. This is the key advantage: memory usage is bounded by sampling parameters, not graph size.

**GCN vs. GraphSAGE:** GCN's complexity depends on $N$ (full graph); GraphSAGE's space complexity is independent of $N$. For massive graphs, this difference is enormous.

---

## 14.2 Graph Pooling

Graph pooling reduces graph size to learn **graph-level** (not node-level) representations.

**Not needed for:** Node-level tasks (node classification, link prediction) — pooling destroys node-specific information.

**Needed for:** Graph-level tasks:

- Graph classification (is this molecule toxic?)
- Hierarchical representation learning
- Graph community detection

**Analogy:** Max-pooling in CNNs reduces spatial resolution while preserving essential features. Graph pooling reduces the number of nodes while preserving structural patterns.

---

### 14.2.1 Set Pooling (Global Readout)

Simplest approach: aggregate all node embeddings into one graph-level representation:

- **Global Mean:** Average all node embeddings
- **Global Sum:** Sum all node embeddings
- **Global Max:** Element-wise max across all embeddings

Does not exploit graph structure in the pooling step.

---

### 14.2.2 Hierarchical Graph Coarsening (DiffPool)

**DiffPool** (Ying et al., 2018): Learns to cluster nodes hierarchically.

Two GNNs per layer:

1. **Embedding GNN:** Learns node representations $Z = \text{GNN}(A, X)$
2. **Pooling GNN:** Learns cluster assignments $S = \text{GNN}(A, X)$ — a soft assignment matrix

New coarsened graph: $$A_{new} = S^T A S \quad (\text{new adjacency})$$ $$X_{new} = S^T Z \quad (\text{new features})$$

Repeat until graph is small enough, then apply global readout.

DiffPool is end-to-end differentiable (using soft assignments), making it trainable via backpropagation.

---

## 14.3 GNN Layers in Practice

A modern GNN layer combines three key operations:

**1. Transformation:** Apply learnable $\mathbf{W}$ to node features (message preparation)

**2. Aggregation:** Collect neighbourhood information (AGGREGATE function)

**3. Update:** Combine aggregated info with node's own state (UPDATE function)

Plus practical training components:

- **Batch Normalisation:** Normalises activations within a batch, stabilising training and accelerating convergence. Applied after aggregation.
- **Dropout:** Randomly zeroes activations during training to prevent overfitting. Only neural network activations are dropped — the graph topology (nodes, edges) is preserved.
- **Activation functions:** ReLU (most common), PReLU (learnable slope), Swish/GELU (smoother variants).

---

## 14.4 Model Depth, Over-Smoothing & Skip Connections

**Receptive field:** A $k$-layer GNN sees each node's $k$-hop neighbourhood. Deeper = broader context.

**Over-smoothing:** With many layers, all node representations converge toward the same value — all nodes become indistinguishable. The receptive field grows to encompass most of the graph, causing representations to average out.

**Skip connections (residual connections):** Add the input of a layer directly to its output:

$$\mathbf{h}_v^{(l+1)} = \text{GNN}(\mathbf{h}_v^{(l)}) + \mathbf{h}_v^{(l)}$$

- Preserves each node's original (or earlier) representation
- Equivalent to a mixture of shallow and deep GNNs
- Prevents over-smoothing by retaining early-layer individuality
- Borrowed from ResNet in computer vision

---

## 14.5 Graph Augmentation

Sometimes the raw graph is not ideal for GNN training:

**Feature Augmentation:**

- **Approach 1:** When node features are absent or uninformative, assign structural features as node features (e.g., degree, clustering coefficient, pagerank score).
- **Approach 2:** Augment with constant features or one-hot identity encodings.

**Structure Augmentation:**

- Add virtual edges to connect distant nodes (improve information flow)
- Add virtual nodes connected to all others (enable global communication)
- Drop edges/nodes (data augmentation for regularisation)

---

## 📝 Q&A — Section 14

**Q1. Why does GraphSAGE's space complexity not depend on the total number of graph nodes, while GCN's does?**

A: GCN must store the full $N \times N$ normalised adjacency matrix and $N \times d$ feature matrix for all nodes at every layer. GraphSAGE uses mini-batching and neighbourhood sampling — only the $b$ target nodes and their sampled $k^L$-node computation trees are loaded at once. The computation is bounded by $bk^L$, independent of the graph's total node count $N$.

---

**Q2. When is graph pooling necessary, and when is it harmful?**

A: Necessary: graph-level tasks (classifying entire molecules as toxic/non-toxic, classifying social network types). Here, you need to compress the entire graph into a single fixed-size representation. Harmful: node-level tasks (node classification, link prediction). Pooling coarsens the graph by merging/removing nodes, destroying the node-specific information needed to classify individual nodes.

---

**Q3. How does DiffPool perform hierarchical pooling differently from simple global pooling?**

A: DiffPool learns to cluster nodes into groups at each pooling step using a dedicated pooling GNN that outputs soft cluster assignments. Nodes are merged into clusters, creating a smaller graph at each step, preserving local graph topology through the coarsening matrices $A_{new} = S^TAS$ and $X_{new} = S^TZ$. Global pooling simply aggregates all nodes at once without any hierarchical structure, losing topological information.

---

**Q4. What is the receptive field of a GNN, and how does it relate to the number of layers?**

A: The receptive field of a node in a $k$-layer GNN is its $k$-hop neighbourhood — all nodes reachable in exactly $k$ steps. Each layer extends the receptive field by one hop. A 1-layer GNN sees only direct neighbours; a 3-layer GNN sees all nodes within 3 hops. This is directly analogous to increasing the kernel size in CNNs.

---

**Q5. Explain skip connections in GNNs and why they help with over-smoothing.**

A: Skip connections add the input of a GNN layer directly to its output: $\mathbf{h}^{(l+1)} = \text{GNN}(\mathbf{h}^{(l)}) + \mathbf{h}^{(l)}$. They preserve each node's earlier representation, preventing it from being entirely overwritten by neighbourhood aggregation. As a result, even in deep networks, nodes retain some of their original, individual characteristics. It's equivalent to mixing outputs from multiple depths, combining shallow (local) and deep (global) perspectives.

---

**Q6. What is batch normalisation and why is it used in GNN layers?**

A: Batch normalisation computes the mean and standard deviation of activations within a mini-batch, then normalises and rescales them. This reduces internal covariate shift — the changing distribution of activations during training — stabilising the gradient flow and often allowing higher learning rates. In GNN layers, it's applied after aggregation to keep activation scales consistent across nodes and layers.

---

**Q7. Why is dropout in GCNConv/SAGEConv different from dropping nodes or edges in PyG?**

A: Dropout in GCNConv/SAGEConv zeroes random activation values in the neural network's weight matrices — it is classic deep learning regularisation that prevents overfitting. The graph structure (nodes, edges, adjacency) is completely unchanged. PyG separately offers functions like DropNode and DropEdge that actually remove nodes or edges from the graph topology during training — a completely different operation used for graph-level data augmentation and robustness.

---

**Q8. Why should you not simply increase GNN depth indefinitely to capture more graph context?**

A: Two problems arise: (1) **Over-smoothing** — deeper GNNs aggregate from exponentially growing neighbourhoods. Features from distant nodes with different structural roles get mixed, making all representations similar and reducing discriminative power. (2) **Over-squashing** — information from large neighbourhoods must pass through narrow bottleneck nodes to reach the target, compressing and losing structural details. Both degrade performance beyond an optimal depth.

---

# 🔑 Final Summary: The Big Picture

```
Message Passing Framework
├── Aggregation: SUM (most expressive), MEAN, MAX
├── Update: Concatenation + Linear Layer + Non-linearity
└── Depth: k layers → k-hop receptive field

Foundational Models
├── GCN → Symmetric normalisation, transductive, isotropic
├── GraphSAGE → Sampling + learnable aggregator, inductive, concat update
└── GAT → Learned attention weights, inductive, anisotropic

Expressiveness
├── WL Test ≡ GNN upper bound
├── SUM > MEAN/MAX for discrimination
└── GIN = most expressive standard MPNN

Generative Models
├── GAE → deterministic embeddings, link prediction
└── VGAE → probabilistic embeddings, drug discovery, generation

Advanced Architectures
├── Spatio-Temporal GNN → DTDG/CTDG, TCN/LSTM for time
├── Heterogeneous GNN → R-GCN, metapaths, HIN
└── Graph Transformer → global attention, LPE, O(N²)

Practical Training
├── Supervised: cross-entropy; Unsupervised: contrastive loss
├── Over-smoothing → skip connections, shallow networks
├── Scalability → sampling (GraphSAGE), mini-batching
└── Explainability → GNNExplainer (subgraph + features)
```

---

> **End of Notebook** — All topics from Dr. Bhaskarjyoti Das's course UE23AM342BA1 are covered. Revisit each Q&A section regularly for exam preparation!