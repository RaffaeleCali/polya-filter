
---

# Data Mining Introduction Project

## Raffaele Calì

### Polya Filter Porting

## Problem Description

The Polya Filter function implements a new method for filtering information within complex networks. Complex networks are networks composed of a large number of interconnected nodes, such as social networks or communication networks. The authors of the function propose a filtering methodology inspired by the Polya urn, which represents a combinatorial model driven by a self-reinforcement mechanism. This model is based on a family of null hypotheses that can be calibrated to assess which edges are statistically significant compared to the network's inherent heterogeneity.

The Polya urn is a probabilistic model where a series of balls (or "urns") of different colors are drawn and replaced in other urns containing balls of the same color. In this way, the contents of the urn change over time, reflecting the evolution of color frequencies.

In the context of information propagation, the Polya urn can be used to model how an idea or information spreads within a network. For example, each node in the network can be imagined to have an associated color (based on the content of the information), and nodes exchange information with each other, thus changing the frequencies of the colors. The authors of the function use the Polya urn to model the propagation of information within a social network, where each node represents an individual, and each edge represents a social connection. Specifically, the model is used to study the process of "filtering" information within the network, i.e., how information is selected, modified, and transmitted by the nodes in the network.

The study demonstrates the effectiveness of the method through a series of experiments on real and artificial complex networks. It highlights how the proposed method significantly improves the precision of node selection for information dissemination compared to traditional methods. The authors show that this approach can be used to predict network evolution and identify influential nodes in the spread of information. Additionally, the model can be extended to consider the influence of nodes external to the network and to predict the effect of different information dissemination strategies.

In practice, the method involves assigning a score to each node in the network based on its relative importance and using these scores to select the nodes from which to disseminate information. Moreover, the method accounts for the fact that the importance of nodes can vary over time, for instance, due to new information emerging in the network.

## Code Description

The function takes as input an adjacency matrix "W" representing the network, a free parameter "a" of the filter, an approximation level "apr_lvl," and a "parallel" flag to enable or disable parallel processing.

- **Parameter "a"**: Refers to the probability that a node in the network decides to follow another source of information instead of continuing to follow its current source.
- **Adjacency matrix "W"**: Represents the network.
- **Approximation level "apr_lvl"**: Specifies the level of approximation.
- **Parallel flag "parallel"**: Enables or disables parallel processing.

The code performs various operations to calculate p-values. Firstly, it checks if the network is symmetric, extracts the edges (i, j, w) from the adjacency matrix W based on symmetry, and obtains the list of edges. Additionally, it calculates the degrees (k_in, k_out) and strengths (s_in, s_out) of each node using the asymptotic form if non-integer weights are present.

Finally, the function calculates the p-values for each edge (i, j, w) using the polya_cdf function and returns the minimum p-value. The function implements the cumulative distribution of Polya. It calculates p-values that can be approximated and those that cannot be approximated using two different forms depending on the value of a: if a = 0, it uses the binomial distribution; otherwise, it uses an asymptotic approximation.

The function returns a vector of values "P" representing the p-values prescribed by the Polya filter for each link in the network.

## How to Use

1. Open the "idmdefinitivo" folder and start the project in R.

### Libraries Used

- `igraph`
- `mvtnorm`
- `Matrix`

### Example with Manually Created Graph

```R
# Create an empty graph
g <- make_empty_graph()
# Add vertices and edges
g <- add_vertices(g, 5)
g <- add_edges(g, c(1,2, 1,3, 2,4, 3,4, 4,5), attr = list(weight = c(0.5, 0.8, 0.2, 1.0, 0.6)))
# Call the function
PF(adj_matrix, 4.5, 2, FALSE)
```

### Example with Graph from Network Repository

```R
# Graph contained in the file bio-CE-CX.edges
# Read from file and create graph structure
edge_list <- read.csv("bio-CE-CX.edges", header = FALSE, sep = " ")
graph <- graph_from_data_frame(d = edge_list, directed = FALSE, vertices = NULL)
# Create adjacency matrix
adj_matrix <- as_adjacency_matrix(graph, type = "both", sparse = FALSE)
# Call the PF function
PF(adj_matrix, 4.5, 2, FALSE)
```

## Author

Raffaele Calì

---
