# EverythingGraph_tmp


README

Provided is the code for BFS, PR, SSSP, SpMV and WCC using different data layouts and compute variations.

This code was designed to evaluate existing techniques. Therefore, some files are reused from existing systems. More specifically: transpose_ligra, parallel_ligra, radixSort_ligra, utils_ligra from <a href="https://github.com/jshun/ligra/tree/master/ligra">Ligra</a>.
Bitmap from <a href="https://github.com/thu-pacman/GeminiGraph/tree/master/core"> Gemini </a>.

<h1>Organization</h1>

The driver of the program is <b>random.c</b>. Pre-processing code is located in <b>init_all.c</b>.  Files starting with algorithm names implement different variations of the same algorithm, used to evaluate programming techniques. 

<h1>Compilation</h1>

To compile the code you need compiler support for Cilk Plus. The code is then compiled simply by running <strong> make </strong>. 

<h1>Input</h1>

The input file is a binary edge array of the for <strong>[src][dst].</strong>
If the edges have weights, the <b>–w</b> flag needs to be added on running the program and the WEIGHTED constant changed to 1 in <b>random.h .</b>
If the algorithm requires weights and the input file does not have them, random weights are generated by setting <b>CREATE_FLAG to 1 in random.h</b>.

<h1>Running the code</h1>

To run the code you need to call the corresponding algorithm binary with a number of required options:

<b>./algorithm_binary –n #_of_vertices –m load_mode –f path_to_graph [optional parameters].</b>

When not running the NUMA-aware version, it is recommended to use <b>numactl --interlave=all</b> before the command line to improve performance. <br/>
To change the number of threads running, it is necessary to edit the constants ALGO_NB_THREADS and CONCURRENCY_THREADS in random.h.


<h1>Pre-processing</h1>

The graph can be represented as:

1.	An edge array
2.	Adjacency lists
3.	2D grid

The way these data layouts are created can be varied: Using a radix sort, a dynamic allocation and reallocation on load or count sort. 
The implementation of all of these options is in <b>init_all.c</b>. The data layout can be selected by setting the <b>load_mode</b> flag. 

If the algorithm needs to be run an undirected graph, pass the <b>–u</b> flag when running the algorithm. If the graph is already undirected and the algorithm requires creating incoming adjacency lists, passing <b>–s 1</b> will disable this. 


<h1>NUMA-Aware data placement</h1>

The additional pre-processing step needed to place data in a NUMA-Aware manner is implemented in the actual files that run the algorithms with NUMA-Awareness: <b>pr_numa</b> and <b>bfs_numa</b>. The expected data layout is an adjacency list. 

Most files contain the currently pushed data layouts for the algorithm except BFS and PR over a grid which are stored in separate files (bfsgrid, prgrid).



