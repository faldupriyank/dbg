# A Closer Look at Lightweight Graph Reordering
Source code for the graph reordering technique, DBG, published in [Faldu et al., IISWC'19].

This repo integrates skew-aware lightweight reordering techniques with a widely popular shared-memory graph processing framework, Ligra. To make this repo standalone, it retains large chunk of the code from the original Ligra framework along with the changes from Balaji et al. [2]. For detailed information on the Ligra framework, please consult their original [repository](https://github.com/jshun/ligra).

Specifically, we provide multi-threaded implementations of DBG[1], Hub Clustering[2], Hub Sorting[3], Sort and Random (in ligra/dbg.h), in addition to the original implementations of Hub Clustering and Hub Sorting provided by Balaji et al. (in ligra/IO.h). We also support aribtrary reordering based on an input MAP file (see LoadMappingFromFile() in ligra/dbg.h for more detail).

# How to build
```
export DBG_ROOT='directory where this repo is cloned'
cd ${DBG_ROOT}/apps
make clean; make cleansrc; make -j
```

# Preparing input datasets

## Graph Format
In addition to the Ligra's built-in support of graphs in adjacency format from the Problem Based Benchmark Suite, we also support graphs in binary CSR format used in Galois (enabled by setting VGR=1 in the Makefile, which is ON by default).

## Download and prepare graph datasets
Workloads used in the paper can be downloaded from the links specified in `${DBG_ROOT}/graph-convert-utils/workload_links.info`.

Directory `${DBG_ROOT}/datasets` contains a small dataset called `web-Google` to get you started. The following set of commands shows how you could download and prepare it by yourself.
```
wget http://snap.stanford.edu/data/web-Google.txt.gz
gunzip web-Google.txt.gz
${DBG_ROOT}/graph-convert-utils/clean_edgelist.py ${DBG_ROOT}/datasets/web-Google.txt ${DBG_ROOT}/datasets/web-Google.el
${DBG_ROOT}/graph-convert-utils/convert.sh ${DBG_ROOT}/datasets/web-Google
```

# Run graph applications
## Run individual application
The following command runs the `PageRank` application on the `web-Google` dataset with `DBG` reordering (specified by 5).
```
cd ${DBG_ROOT}/apps
make REORDERING_ALGO=5 DEGREE_USED_FOR_REORDERING=0 DATASET=web-Google run-PageRank
```
Total reordering time is printed as `DBG Total Map Time:  0.032493` and Application runtime is printed as `PageRank Run Time(sec) :0.437708`. Addition of both would provide a net runtime of an application. 

## Run all applications at once
The following script runs all five graph applications on the `web-Google` dataset.
```
cd ${DBG_ROOT}/apps
./run.sh
```



# Target system
This version is tested on x86 system with gcc 6.4.0 on Ubuntu 14.04.1 booted with Linux 4.4.0-96-lowlatency.

# License and copyright of the code used from external repositories
The repo also contains code from multiple other repositories (e.g., [Ligra](https://github.com/jshun/ligra), [Graph-Reordering-IISWC18](https://github.com/CMUAbstract/Graph-Reordering-IISWC18), [GAP](https://github.com/sbeamer/gapbs)) and the original copyright and license constraints apply to their code.

# References
**Please cite the following if you use the source code from this repository in your research.**
```
@inproceedings{DBGFalduIISWC19,  
  author={Priyank Faldu and Jeff Diamond and Boris Grot},  
  booktitle={International Symposium on Workload Characterization (IISWC)},  
  title="{A Closer Look at Lightweight Graph Reordering}",  
  year={2019},  
  month=nov,  
}
```

[1] P. Faldu and J. Diamond and B. Grot, "A Closer Look at Lightweight Graph Reordering," in *Proceedings of the International Symposium on Workload Characterization (IISWC)*, November 2019.

[2] V. Balaji and B. Lucia, "When is Graph Reordering an Optimization? Studying the Effect of Lightweight Graph Reordering Across Applications and Input Graphs," in *Proceedings of the International Symposium on Workload Characterization (IISWC)*, September 2018.

[3] Y. Zhang, V. Kiriansky, C. Mendis, S. Amarasinghe and M. Zaharia, "Making Caches Work for Graph Analytics," in *Proceedings of the International Conference on Big Data (Big Data)*, January 2018.
