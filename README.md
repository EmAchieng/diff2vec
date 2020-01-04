# Diff2Vec
![GitHub stars](https://img.shields.io/github/stars/benedekrozemberczki/diff2vec.svg?style=plastic)
![GitHub forks](https://img.shields.io/github/forks/benedekrozemberczki/diff2vec.svg?color=blue&style=plastic)
![License](https://img.shields.io/github/license/benedekrozemberczki/diff2vec.svg?color=blue&style=plastic)

<p align="justify">
Diffusion to vector is a transductive topological graph embedding procedure inspired by other sequence based graph embedding procedures. Sequences are generated by a linearized branching process. It is robust to graph densification and growth. The created embeddings are competitive with other sequence based embedding methods (e.g. DeepWalk/Node2Vec). It can be used to learn embeddings of networks with millions of nodes and it allows for parallel processing in the sequence generation and learning phases.
</p>

<p align="center">
  <img width="500" src="diff2vec.jpeg">
</p>
This repository provides a reference implementation for Diff2Vec as described in the paper:

> Fast Sequence Based Embedding with Diffusion Graphs.
> [Benedek Rozemberczki](http://homepages.inf.ed.ac.uk/s1668259/) and  [Rik Sarkar](https://homepages.inf.ed.ac.uk/rsarkar/).
> International Conference on Complex Networks, 2018.
> http://homepages.inf.ed.ac.uk/s1668259/papers/sequence.pdf


### Citing

If you find Diff2Vec useful in your research, please consider citing the following paper:

>@inproceedings{rozemberczki2018fastsequence,  
  title={Fast Sequence Based Embedding with Diffusion Graphs},  
  author={Rozemberczki, Benedek and Sarkar, Rik},  
  booktitle={International Conference on Complex Networks},  
  year={2018}
 }

### Requirements

The codebase is implemented in Python 3.5.2 | Anaconda 4.2.0 (64-bit).

```
tqdm              4.28.1
numpy             1.15.4
pandas            0.23.4
texttable         1.5.0
gensim            3.6.0
networkx          2.4
joblib            0.13.0
logging           0.4.9.6  
```

### Datasets
<p align="justify">
The code takes an input graph in a csv file. Every row indicates an edge between two nodes separated by a comma. The first row is a header. A sample graph for the `Facebook Restaurants` dataset is included in the  `data/` directory.</p>

### Options

<p align="justify">
Learning of the embedding is handled by the `src/diffusion_2_vec.py` script which provides the following command line arguments.</p>
#### Input and output options
```
  --input    STR     Path to the edge list csv.            Default is `data/restaurant_edges.csv`
  --output   STR     Path to the embedding features.       Default is `emb/restaurant.csv`
```

#### Model options
```
  --model                    STR      Embedding procedure.                      Default is `non-pooled`
  --dimensions               INT      Number of embedding dimensions.           Default is 128.
  --vertex-set-cardinality   INT      Number of nodes per diffusion tree.       Default is 80.
  --num-diffusions           INT      Number of diffusions per source node.     Default is 10.
  --window-size              INT      Context size for optimization.            Default is 10.
  --iter                     INT      Number of ASGD iterations.                Default is 1.
  --workers                  INT      Number of cores.                          Default is 4.
  --alpha                    FLOAT    Initial learning rate.                    Default is 0.025.
```

### Examples

<p align="justify">
The following commands learns a graph embedding and writes it to disk. The first column in the embedding file is the node ID.
</p>
Creating an embedding of the default dataset with the default hyperparameter settings.

```
python src/diffusion_2_vec.py
```
Creating an embedding of an other dataset the `Facebook Politicians`.

```
python src/diffusion_2_vec.py --input data/politician_edges.csv --output output/politician.csv
```
<p align="justify">
Creating an embedding of the default dataset in 32 dimensions, 5 sequences per source node with maximal vertex set cardinality of 40.</p>

```
python src/diffusion_2_vec.py --dimensions 32 --num-diffusions 5 --vertex-set-cardinality 40
```
