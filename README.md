## Nov 18 2019 CSCI 5702 Big Data Mining 
# Programming Assignment 3: PageRank 

In this problem, you will learn how to implement the PageRank algorithm in Spark. You will be experimenting with a small randomly generated graph (assume graph has no dead-ends) provided in the file graph-full.txt.

There are 100 nodes (n = 100) in the small graph and 1000 nodes (n = 1000) in the full graph, and m = 8192 edges, 1000 of which form a directed cycle (through all the nodes) which ensures that the graph is connected. It is easy to see that the existence of such a cycle ensures that there are no dead ends in the graph. There may be multiple directed edges between a pair of nodes, and your solution should treat them as the same edge. The first column in graph-full.txt refers to the source node, and the second column refers to the destination node. 
 
## Implementation
Assume the directed graph G = (V,E) has n nodes (numbered 1,2,...,n) and m edges, all nodes have positive out-degree, and M = [M<sub>ji</sub>]<sub>n×n</sub> is a 𝑛 × 𝑛 matrix as defined in class such that for any 𝑖,𝑗 within the range of 0 to n.  
<img src="https://i.postimg.cc/BvHXPbxG/page-Rank-Equation.png" width="40%"></img>

Here, deg(i) is the number of outgoing edges of node i in G. If there are multiple edges in the same direction between two nodes, treat them as a single edge. By the definition of PageRank, assuming 1−β to be the teleport probability, and denoting the PageRank vector by the column vector r, we have the following equation: 
<img src="https://i.postimg.cc/fR6VbmSK/columnR.png" width="40%"></img>

where 1 is the n × 1 vector with all entries equal to 1. 

Based on this equation, the iterative procedure to compute PageRank works as follows: 
1. Initialize: 
<img src="https://i.postimg.cc/8PXFqq1x/column-Init.png" width="10%"></img>
2. For i from 1 to k, iterate: 
<img src="https://i.postimg.cc/4xKKZByy/r-iteration.png" width="25%"></img>

Run the aforementioned iterative process in Spark for 40 iterations (assuming β = 0.8) and obtain the PageRank vector r. You do not have to implement the block-based algorithms mentioned in the lectures to develop the PageRank algorithm; the vanilla implementation would work. Matrix M can be large and should be processed as an RDD in your solution. 

Compute the followings for the larger dataset:
* List the top 5 node ids with the highest PageRank scores. 
* List the bottom 5 node ids with the lowest PageRank scores.

### Implementation hints:  
* You may choose to store the PageRank vector r either in memory or as an RDD. However, matrix of links and M must not be stored in memory.
* The page ranks should sum up to ~1 at each iteration. 
* For a sanity check, we have provided a smaller dataset (graph-small.txt). In that dataset, the top node has id 53 with value ~0.036 (after 40 iterations). 

Your need to declare the following variables on top of your code:
```python
dataset_path= ‘’ # path to the dataset 
iterations = 40 # number of iterations 
beta = 0.8
```

Not that you must not hardcode the number of nodes in your program, as it may be tested with a different dataset. 

The driver function (which is the only function directly called for grading) should have the following signature: 
`page_rank(dataset_path, iterations, beta)`
