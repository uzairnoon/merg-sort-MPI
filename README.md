# merg-sort-MPI
This project presents the implementation of a Parallel Merge Sort algorithm, an optimized version of the classical merge sort designed to utilize the power of parallel computing. Merge sort is a comparison-based sorting algorithm that follows the divide-and-conquer paradigm.
# Parallel Merge Sort Using MPI

## Implementing Merge Sort Using MPI



## Abstract

While merge sort is well-understood in parallel algorithms theory, relatively little is known of how to implement parallel merge sort with mainstream parallel programming platforms such as MPI, and run it efficiently. Hence, better understanding of merge sort parallelization can contribute to a better understanding of divide-and-conquer parallelization in general. In this work, a serial and a parallel merge sort are investigated using a message-passing approach that runs on computer clusters with MPI. Experimental results comparing the execution time of serial and parallel versions of merge sort show that the parallel version achieves the best speedup.



## 1. Merge Sort Algorithm

Merge Sort follows the rule of Divide and Conquer. It does not directly divide the list into two equal halves. Instead, the unsorted list is divided into N sublists, each having one element, because a list containing one element is already considered sorted. Then, it repeatedly merges these sublists to produce new sorted sublists, and finally one completely sorted list is produced. Merge Sort is quite fast and has a time complexity of O(n log n). It is also a stable sort, which means that equal elements preserve their original order in the sorted list.

The merge sort function recursively sorts the subarray array[p..r]. After calling merge sort (array, p, r), the elements from index p to index r of the array are sorted in ascending order. If the subarray has size zero or one, it is already sorted and no further action is required. Otherwise, merge sort uses divide-and-conquer to split the subarray and then uses the merge (array, p, q, r) function to merge the sorted subarrays array[p..q] and array[q+1..r].

The merge function merges two adjacent sorted subarrays into a single sorted subarray. To achieve this, temporary arrays are created to store the elements of both subarrays. This ensures that original values are preserved during the merge process. Since each element must be examined once, the merging operation takes Θ(n) time.



## 2. Serial Implementation of Merge Sort Using MPI

Merge sort is based on the divide-and-conquer paradigm. It divides the problem into smaller subproblems that are simpler instances of the same problem. These subproblems are solved recursively, and their results are combined to obtain the final solution.

### 2.1 Sort Function

The sort function implements the core sorting algorithm. It receives an array along with its starting and ending indices. The function computes the middle index and divides the array into two smaller subarrays. The sort function is then called recursively on each subarray until arrays of size one are obtained. Such arrays are considered sorted. Once both subarrays are sorted, the merge function is called to combine them into a single sorted array.

### 2.2 Merge Function

The merge function handles the merging stage of the algorithm. It takes two sorted arrays and their respective sizes as input and returns a merged sorted array. The size of the merged array is equal to the sum of the sizes of the two input arrays. Iterators are used to traverse the arrays, compare elements, and insert them into the merged array in sorted order. Once one array is exhausted, the remaining elements of the other array are appended while preserving their order.

### 2.3 Main Function

The main function defines the required data structures and initializes the data array with size N. The clock function is used to measure the execution time of the program.


## 3. Parallel Implementation of Merge Sort Algorithm

The parallel version follows the Single Instruction Multiple Data (SIMD) model. The same instructions are executed on different subsets of data across multiple processors, making merge sort suitable for parallelization.

### 3.1 Partitioning the Data Equally

Initially, the data is partitioned equally among the available processors. When the total number of elements cannot be evenly divided, additional elements are added and initialized to zero to ensure uniform local data sizes. Random three-digit numbers are generated to populate the dataset.

### 3.2 Sorting the Data

The MPI_Broadcast function is used to distribute the size of the local data to all processors. Memory is allocated accordingly, and MPI_Scatter is used to distribute portions of the data to each processor. Each processor then independently calls the sort function on its local data.

### 3.3 Merging the Data

A tree-based global merging strategy is used to combine sorted data from all processors. Processors communicate based on their ranks and merge their data in steps that are multiples of two. This continues until the final sorted array is produced.



## 4. Performance Measurement

The execution time of both serial and parallel implementations is measured using the clock function. CPU time comparisons are used to evaluate performance improvements achieved through parallelization.

