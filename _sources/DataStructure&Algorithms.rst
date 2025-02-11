===========================
Data Structure & Algorithms
===========================

1. `Complexity`_
2. `Data Structures`_
3. `Algorithms`_
4. `Maths`_

`back to top <#data-structure--algorithms>`_

Complexity
==========

* `Time Complexity`_, `Space Complexity`_, `Big O`_
* to determine how much time and space a program/algorithm take

Time Complexity
---------------
    - measure the growth rate in steps instead of time, as time can change depending on the
      hardware

Space Complexity
----------------
    - measure how many units of extra/auxiliary memory will be consumed as data increases
    - only consider generated new data
    - algorithm can consume extra space even when not generating additional data, e.g
      call stacks of recursive functions such as quicksort
* has Theta, Omega and O notations, which allow to easily categorize efficiency
* notations are originally a concept from mathematics
* concerned with algorithm's performance as input data, "n", increases
* algorithms that aren't affected by increased input data are said to be constant, e.g O(1)
* constants in the functions are ignored and only highest order is used, e.g O(2n<sup>2</sup>+3n+1)
becomes O(n<sup>2</sup>)
* when two algorithms have same classification, further analysis is required to determine which
is faster
* determining the current efficiency is the first step in optimizing the program
* most only consider time complexity
* for devices with limited memory, space complexity matter a lot
* should analyze whether to prioritize speed or memory and check all pros and cons of each
algorithm

Big O
-----
    * describe the upper bound, worst case scenario
    * generally most used notation
    * growth function cannot get larger than this
    * e.g linear search best case is O(1) and worst case O(n)
    * knowing the worst case have strong impact on choosing algorithms
    * O(1)
    * O(log n)
    * O(n)
    * O(n log n)
    * O(n * m)
        - for two distinct datasets interact through multiplication
        - usually assign smaller number as "m"
        - can be considered as a range between O(n) and O(n<sup>2</sup>)
        - same as O(n<sup>2</sup>) when n and m are same
    * O(n<sup>2</sup>)
    * O(2<sup>n</sup>), doubles in steps each time one element is added

`back to top <#data-structure--algorithms>`_

Data Structures
===============

* `Array`_, `Linked List`_, `Circular Array`_, `Hash Table`_, `Queue`_, `Stack`_, `Set`_
* `Tree`_, `Binary Tree`_, `Binary Search Tree`_, `Heap`_, `Prefix Tree`_
* `Spanning Tree`_, `Minimum Spanning Tree`_, `Graph`_
* can operate read, search, insert and delete on data structures
* use a data structure that is consistently fast, rather than the one that is sometimes
extremely fast and sometimes slow

Array
-----
    * a list of fixed length, used when number of data items is known
    * can be used to implement any type of binary tree
    * in most languages, array can be passed through a call stack as it remains the same object
    after modifying it
    * **Pros**
        - good for storing similar contiguous data
        - O(1) accessing power
        - very basic and easy to learn
    * **Cons**
        - size cannot be changed once initialized
        - inserting and deleting are not efficient
        - can be wasting storage space
    * **Time Complexity**
        - Accessing: O(1), array is instantiated with fixed size and the memory is already
          reserved
        - Searching: O(n), most of the time the array is unsorted list, so it must use linear
          search
        - Inserting: O(1) from the end, (O(n) for inserting an element at the front or middle,
          as it requires to shift every element)
        - Deleting: O(1) from the end, (O(n) for deleting an element at the front or middle,
          as it requires to shift every element)
    * **Ordered array**
        - like normal array, but values are always in order
        - slower insertion than normal array, but faster searching
        - Accessing: O(1)
        - Searching: O(log n), can use binary search
        - Inserting: O(n), need to search for right place first
        - Deleting: O(n)

Linked List
-----------
    * contains data and a pointer/link to another data node
    * data can be scattered across different cells of memory
    * used when number of items is unknown
    * the pointer at the end, the tail, is set to null
    * each node contains two memory cells, one for the actual data and other for the link
    * a deleted node will still exist in memory, different languages handle the deleted nodes
    in various ways
    * **Doubly Linked List**
        - root node has a pointer to both the first and last nodes
        - each node has a pointer to the previous and the next node
        - can traverse in both direction
        - always know the first and last nodes
        - perfect data structure for a queue
        - Accessing: O(n)
        - Searching: O(n)
        - Inserting: O(n), (O(1) for inserting element at the head or end)
        - Deleting: O(n), (O(1) for deleting element at the head or end)
    * **Pros**
        - can easily change size to add or remove
        - insertion can be O(1) if have access to the right node
        - efficient for moving through and insert or delete many elements
    * **Cons**
        - must traverse every node to find the target or insert a new node at specific
          location
        - only have access to the first node
    * **Time Complexity**
        - Accessing: O(n), must start from the beginning
        - Searching: O(n), must search from the beginning
        - Inserting: O(n), (O(1) for inserting element at the head)
        - Deleting: O(n), (O(1) for deleting element at the head)

Circular Array
--------------
    * also called circular buffer or ring buffer
    * uses a single, fixed-size array as if it were connected end-to-end
    * allows efficient and continuous writing and reading of data without the need for shifting
    elements
    * useful where data is continuously produced and consume at different rates
    * fixed size is determined at creation, has read and write pointers
    * when the end is reached, read and write continues from the beginning using modulo
    arithmetic, e.g. `write_idx = (write_idx + 1) % SIZE`
    * **Pros**
        - efficient use of memory
        - constant time complexity due to wrap-around behaviour
        - well-suited for real-time applications and concurrent programming
    * **Cons**
        - fixed size, sequential access can be less efficient compared to linear arrays
        - complex to implement, maintain and debug
    * **Time Complexity**
        - Accessing: O(1)
        - Searching: O(n), most of the time the array is unsorted list, so it must use linear
          search
        - Inserting: O(1)
        - Deleting: O(1)

Hash Table
----------
    * also called hashes, maps, hash maps, dictionaries, associative arrays
    * built on top of array, unordered data structure
    * contain a set of keys, each with associated value, key-value pairs
    * used to implement dictionary interface
    * each key must be unique, but can contain multiple values
    * storing existing key will overwrite the value, but keep the same key
    * hash value can be used for organizing and sorting the data
    * efficiency depends on data amount to be stored, number of available cells in a table and
    hash function
    * **Hash Function**
        - process of converting characters to numbers
        - must convert the same string to same number every time
        - used to compute an index to store the value
        - a good hash function must distribute data across all available cells
    * **Collision**
        - adding data to a cell that is already filled
        - hash bucket with more than one value
        - storing keys next to the values can help with collisions
        - separate chaining: placing a reference to an array instead of placing single value
          when collisions occur
        - searching can take extra steps when a value is a reference to an array, worst case
          O(n)
        - hash tables need to be designed to have few collisions
        - have larger number of cells to avoid collisions, without consuming lots of memory
        - load factor: ratio of data to cells
        - use ideal load factor of 0.7 (7 elements / 10 cells)
    * **Pros**
        - constant time complexity for all operations
        - can be used to make code faster, by trading some memory
    * **Cons**
        - unordered, no beginning or end
        - cannot find key using the value
    * **Time Complexity**
        - Accessing: O(1)
        - Searching: O(1)
        - Inserting: O(1)
        - Deleting: O(1)

Queue
-----
    * First in, First out
    * abstract data type, can be implemented using Array, Linked List or Doubly Linked List
    * data can be inserted only at the end, deleted from the front and only read first element
    * e.g printing jobs, background workers in web apps, handle asynchronous requests
    * **Priority Queue**
        - accept states and store in a method to get the least cost state
        - data is sorted in specific order
        - can implement using ordered array
    * **Deque/Double Ended Queue**
        - can push and pop from both front and back
    * **Pros**
        - insertion and deletion from ends are fast as it uses FIFO rule
        - useful for handling temporary data
    * **Cons**
        - some programming languages don't have as built-in data type
        - can only read the first element
        - searching for an element will need to remove elements until the desired one is found
    * **Time Complexity**
        - Accessing: O(1)
        - Searching: O(n), must dequeue every element
        - Inserting: O(1), also called enqueue
        - Deleting: O(1), also called dequeue

Stack
-----
    * Last in, First out data structure
    * abstract data type, can be implemented using Array or Linked List
    * data can be inserted and deleted only from the end, and only read last element
    * e.g undo function in a word processor
    * **Pros**
        - useful for handling temporary data
        - forced to remove only items from the top, give new mental model for solving problems
    * **Cons**
        - can only read the last element
        - some programming languages don't have as built-in data type
        - searching for an element will need to remove elements until the desired one is found
    * **Time Complexity**
        - Accessing: O(1), only topmost/last element
        - Searching: O(n), must pop every element
        - Inserting: O(1), also called pushing onto the stack
        - Deleting: O(1), also called popping from the stack

Set
---
    * do not allow duplicate values
    * has different implementations, e.g array-based set
    * **Time Complexity**
        - Accessing: O(1)
        - Searching: O(n)
        - Inserting: O(n), need to search first not to add duplicates
        - Deleting: O(n)

Tree
----
    * node-based data structure, contains one or more data nodes
    * first/uppermost node is the root, each node can have links to zero or more child nodes
    * node with child nodes is called parent node
    * a node can have descendants and ancestors
    * each level of a tree is a row
    * traversing a tree is always O(n)
    * searching in a node is same as traversing the tree, except the method stops when target
    is found
    * **Balanced Tree**
        - nodes' subtrees have the same number of nodes
    * **Complete Tree**
        - a tree completely filled with nodes
        - bottom row can have empty positions, as long as no nodes to the right of them
    * [**Traversal/Search Methods**](#tree-traversals)
        - DFS (Depth-first Search): In-Order, Pre-Order, Post-Order
        - BFS (Breadth-first Search): Level-Order
    * **Pros**
    * **Cons**
    * **Time Complexity**
        - Accessing
        - Searching
        - Inserting
        - Deleting

Binary Tree
-----------
    * special type of tree, each node has at most two children
    * used to implement other tree data structures such as binary search tree
    * **Balanced Binary Tree**
        - has about "log n" levels/rows for "n" nodes
        - each new level doubles the size of the tree
    * **Pros**
    * **Cons**
    * **Time Complexity**
        - Accessing
        - Searching: O(n)
        - Inserting: O(n)
        - Deleting: O(n)

Binary Search Tree
------------------
    * binary tree with specific order
    * values in left subtree are less than that of the node
    * values in right subtree are greater than that of the node
    * **Pros**
        - easy to determine which child node to traverse
        - values are ordered, use binary search to find a node
        - faster insertion over ordered arrays
    * **Cons**
        - must use randomly sorted data to create a tree to be well-balanced
        - inserting sorted data can create imbalanced tree, make searching O(n)
        - need to randomize an ordered array before converting it to a bst
    * **Searching**
          1\. set a node as current node, usually the root node is set as when start
                    2\. check value of the current node
                    3\. search left subtree if current node value is less than the target value or search
          right subtree if current node value is greater
                    4\. repeat steps 1 to 3 until target is found or no nodes left to search for
    * **Deleting**
        - just delete the node if it doesn't have children
        - if the node has one child, place the child node in the deleted node position
        - if the node has two children, replace the node with successor node, whose value is
          the least of all values that are greater than the deleted node
        - finding successor node: visit the right child and visit the left child until there
          are no more left children, bottom value is the successor node
        - if successor node has a right child, turn it into the left child of the former
          parent of the successor node
    * **Time Complexity**
        - Accessing: O(log n), (O(n) for unbalanced)
        - Searching: O(log n), (O(n) for unbalanced)
        - Inserting: O(log n), (O(n) for unbalanced)
        - Deleting: O(log n), (O(n) for unbalanced)

Heap
----
    * binary tree with heap condition, each node must be greater or less than each of its
    descendants, and the tree must be complete, to ensure it remains well-balanced
    * visualized as a tree but usually implemented using Array, to find the last node
    efficiently
    * can also use linked-list to implement
    * last node is the rightmost node in the bottom level
    * trickling: process of moving the node up/down the heap
    * only delete the root node
    * **Binary Heap**
        - specific kind of binary tree
        - min heap: minimum value is the root
        - max heap: maximum value is the root
    * **Pros**
        - elements are always in order
        - efficient data structure for inserting and deleting
        - useful for implementing priority queues
        - weak ordering allow insertions of O(log n)
    * **Cons**
        - ordering is useless when searching for a value, but search operation is usually not
          implemented
        - weakly ordered compared to binary search trees
        - cannot efficiently find the last node or a place to hold a new last node without
          inspecting every node
        - imbalanced heap will have operations of O(n)
    * **Inserting**
          1\. create a node and insert at the rightmost spot in the bottom level
                    2\. compare the new node with its parent node
                    3\. swap the node with the parent if it is greater than the parent's value
                    4\. repeat step 3 and move the node up the heap until parent with greater value is met
    * **Deleting**
          1\. move the last node to the root node position
                    2\. trickle the root node down to proper place
    * **Trickling Down**
          1\. check both children of the trickle node
                    2\. swap the node with the larger one of the two child nodes
                    3\. repeat steps 1 and 2 until the trickle node has no greater children
    * **Array-based Heap**
        - root node is always stored at index 0 and last node will be last element
        - accessing left-child: (2*i)+1, accessing right-child: (2*i)+2
        - finding a node's parent: (i-1)/2
    * **Time Complexity**
        - Accessing: O(1) for accessing min/max element
        - Inserting: O(log n)
        - Deleting: O(log n)

Prefix Tree
-----------
    * also called trie, a kind of tree ideal for text-based features, such as autocomplete
    * each node represents single character, usually an alphabet
    * each node can have any number of child nodes, usually max 26 nodes when using alphabets
    * a node should have a value to indicate the end of the word
    * **Pros**
        - searching and inserting are faster as values are prefixed
        - can search for a complete word or a word prefix
    * **Cons**
    * **Prefix Search**
          1\. set a node as current node, usually the root node is set as when start
                    2\. iterate over each character of the search string
                    3\. check if current node has a child with the character as a value
                    4\. return None if doesn't exist and searching ends without finding the string
                    5\. if exists, update the child node as current node and repeat from step 2 to iterate
          another character
                    6\. string is found if end of search string is reached
    * **Inserting**
          1\. set a node as current node, usually the root node is set as when start
                    2\. iterate over each character of the search string
                    3\. check if current node has a child with the character as a value
                    4\. if exists, update the child node as current node and repeat from step 2 to iterate
          another character
                    5\. if doesn't exist, create a child node with the character and update it as current
          node and repeat from step 2
                    6\. after inserting the final character, add a value to indicate the end of the word
    * **Time Complexity**
        - Accessing
        - Searching: O(k), (k = number of characters in the search string)
        - Inserting: O(k)
        - Deleting:

Spanning Tree
-------------

Minimum Spanning Tree
---------------------

Graph
-----
    * group of nodes connected with edges
    * specializes in relationships
    * Linked List and types of Tree are a form of graph
    * all trees are graphs but not all graphs are trees
    * a graph must not have cycles and all nodes must be connected to be a tree
    * in a graph, any node can have arbitrary number of connected nodes
    * vertex: a node in a graph
    * edge: line between nodes
    * neighbors: vertices connected by an edge, adjacent vertices
    * path: sequence of edges from one vertex to another
    * can use hash table, adjaceny list or two-dimensional arrays/adjacency matrix to implement
    * searching is traversing the graph to find connected vertices and can also operate on each
    vertex the same time
    * keep track of visited vertices when searching a graph, e.g use a hash table for visited
    * social networking applications use graph databases
    * **Connected Graph**
        - all vertices are connected
    * **Directed Graph**
        - every edge has a direction
    * **Weighted Graph**
        - additional information on the edges, e.g distance between two vertices
        - use hash table, rather than array, to represent adjacent vertices
        - can use Dijkstra's to find shortest path
        - shortest path in an unweighted graph is the path that takes fewest number of
          vertices to get from one vertex from another, useful in social networking apps
    * [**Traversal/Search Methods**](#graph-traversals)
        - DFS (Depth-first Search), BFS (Breadth-first Search)
        - dfs vs bfs: bfs traverse all the vertices closest to the starting vertex, while dfs
          moves far away from the starting vertex
    * **Pros**
        - having access to only one vertex can find all vertices, unless it is disconnected
    * **Cons**
        - hard to work with and can get complicated as it grows large
        - can use various algorithms to implement
    * for complexity, need to consider number of vertices and number of adjacent neighbors each
    vertex has
    * **Complexity**
        - Searching: O(v + e), (v = no. of vertices, e = no. of edges)
        - Inserting:
        - Deleting:

`back to top <#data-structure--algorithms>`_

Algorithms
==========

* `Binary Search`_, `Recursion`_, `Peak Finding`_, `Dijkstra's Algorithm`_
* `Bubble Sort`_, `Selection Sort`_, `Insertion Sort`_, `Quick Sort`_, `Heap Sort`_
* `Quick Select`_, `Greedy Algorithm`_, `Boyer-Moore Voting Algorithm`_
* `Tree Traversals`_, `Graph Traversals`_
* `KMP`_
* `Dynamic Programming`_
* `Optimization`_
* a set of instructions to operate a specific task
* any code that does anything can be called an algorithm
* knowing difference between best, average and worst cases is key in choosing best algorithm
* good to prepare for worst case, but average cases are occur most of the time

Binary Search
-------------
    * fastest search method for sorted search space, usually in ascending order
    * split the search space into two halves and only keep the half that might have the target
    * **steps**
          1\. set left and right boundaries, min(search space) for left and max(search space)
          for right
                    2\. while left < right, calculate middle value with (left + (right - left) / 2)
                    3\. if condition(mid) is true, set right = mid, else set left = mid + 1, condition
          checking can be from simple comparison to complex calculations
                    4\. return left or (left - 1) after exiting the while loop
    * can use binary search if there is monotonicity, e.g if condition(k) is true then
    condition(k + 1) is true
    * **Complexity**
        - best: O(1), first middle element is the target element
        - average: O(log n)
        - worst: O(log n)

Recursion
---------
    * a function calling itself until the base case is reached
    * **Base Case**
        - the case the function will not recurse
        - every recursion function needs at least one to avoid infinite calling
    * **Call Stack**
        - to keep track which function is in the middle of calling, top element is the most
          recently called function
        - each recursive function pass/return a value up through the call stack to its parent
          function
        - infinite calling or large memory usage by function can cause stack overflow
    * example use cases
        - factorial, fibonacci, tower of Hanoi
        - problems that need to execute repeatedly
        - solving problems without knowing how many layers there are, such as filesystem
          traversal
        - problems solved by calculation based on a subproblem
    * use extra function parameters when writing recursive functions
    * **Top-down Strategy**
        - start by thinking the function has already been implemented
        - identify the subproblem of the problem
        - think what will happen when the function is called on the subproblem
    * useful but can slow down the program if not careful
    * avoid unnecessary extra recursive calls
    * **Dynamic Programming**
        - to optimize recursive problems with overlapping subproblems
        - memoization: reduces recursive calls by remembering computed functions, uses extra
          memory, e.g storing calculated values of fibonacci in hash map
        - bottom-up: ignoring recursion and using other approach

Peak Finding
------------

Dijkstra's Algorithm
--------------------
    * graph search algorithm
    * can find shortest path to a point and shortest paths from current to all points
    * has different implementations
    * using simple array can cause to have O(v<sup>2</sup>), priority queue can be faster
    * **steps**
          1\. set starting vertex as current vertex
                    2\. check weights of edges from the current to each of its adjacent vertices
                    3\. if weight from the starting vertex to adjacent vertex is less than the weight seen
          before, update the current weight as the shortest weight
                    4\. visit the next shortest vertex from the starting vertex and set it as current
                    5\. repeat steps 2 to 4 until every vertex is visited

Bubble Sort
-----------
    * **steps**
          1\. compare first item with the second
                    2\. swap if out of order
                    3\. move one to the right
                    4\. repeat steps 1 through 3 until the end of array
                    5\. go back to first two items and execute steps 1 through 4 until no swaps are needed
    * in each pass-through, highest unsorted value bubbles up to its correct position
    * **Complexity**
        - best: O(n)
        - average: O(n<sup>2</sup>)
        - worst: O(n<sup>2</sup>)

Selection Sort
--------------
    * **steps**
          1\. go from start to end and keep track of lowest value
                    2\. swap the tracked lowest value with the value at the beginning of the pass-through
          (will be first value for the first pass-through)
                    3\. repeat pass-throughs with steps 1 and 2, each starting from the next that has been
          swapped in the previous, until a pass-through start at the final index
    * **Complexity**
        - all cases: O(n<sup>2</sup>)

Insertion Sort
--------------
    * **steps**
          1\. at first pass-through, temporarily remove the second value and a gap will be made
          (remove values at the subsequent indexes for subsequent pass-throughs)
                    2\. compare the temp value with the value to the left of the gap, shift to the right
          if greater than the temp, the gap moves leftward as values shift, if a value lower
          than the temp value is found or the left end of the array is reached, stop shifting
                    3\. insert the temp value into the current gap
                    4\. repeat pass-throughs with steps 1 through 3 until a pass-through start at the
          final index
    * in-place sort, O(1) auxiliary space
    * **Complexity**
        - best: O(n)
        - average: O(n<sup>2</sup>)
        - worst: O(n<sup>2</sup>)

Quick Sort
----------
    * used by most languages
    * very fast and efficient algorithm for average cases
    * combination of partitioning and recursions
    * **Partitioning**
          1\. take random value as the pivot from array and move the left pointer to the right
          until value greater than or equal to the pivot is found
                    2\. move the right pointer to the left until value less than or equal to the pivot is
          found
                    3\. if left pointer reach or go beyond the right pointer, swap the values of left and
          right pointers and repeat steps 1, 2 and 3, other wise go to step 4
          4\. swap the pivot with the value of left pointer
    * after partitioning, all values to the left of the pivot are less than and all to the
    right are greater than it
    * partition time complexity O(n)
    * **steps**
          1\. partition the array, the pivot will be in proper place
                    2\. recurse the subarrays to the left and right of the pivot with steps 1 and 2
                    3\. done if subarray has zero or one element
    * **Complexity**
        - best: O(n log n), pivot ends up in the middle after partition
        - average: O(n log n)
        - worst: O(n<sup>2</sup>), pivot ends up on one side after partition

Heap Sort
---------
    * inserts all the values into a heap and pops each one
    * **Complexity**
        - best: O(n log n)
        - average: O(n log n)
        - worst: O(n log n)

Quick Select
------------
    * optimized way to find kth smallest/largest element in unsorted array
    * variation of quick sort and binary search and relies on partitioning
    * can find correct value without sorting the entire array
    * **steps**
          1\. partition the entire array, pivot will now be in the right place
                    2\. divide the array in half from the pivot and use only left or right after comparing
          the desired value with the pivot value
                    3\. recurse that subarray until the value is found
    * **Complexity**
        - best: O(n), only need to partition one half
        - average: O(n)
        - worst: O(n<sup>2</sup>)

Greedy Algorithm
----------------
    * choose what is available at the moment as the best

Boyer-Moore Voting Algorithm
----------------------------
    * to find majority element in an array
    * majority element: occured more than n/2 times
    * **steps**
          1\. initialize count as 0 and iterate the array
                    2\. if count is 0, set current number as majority number and increase count by 1
                    3\. else check if current number equal to the majority number, if equal, increase
          count by 1, if not decrease count by 1
    * can have extra loop to check if chosen number's count is really greater than n/2 or not
    * **Complexity**
        - all cases: O(n), space O(1)

Tree Traversals
---------------
    * **In-Order Traversal** (from left, root, right)
        - recursive calling on the node's left subtree, visit the node, recursive calling on
          the node's right subtree
        - if a BST, traversal is in ascending order
        - use to get ascending order, to flatten the tree to its original sequence
    * **Pre-Order Traversal** (from root, left, right)
        - visit the node, recursive calling on the node's left subtree, recursive calling on
          the node's right subtree
        - use to explore the roots before leaves, to create a copy of binary tree
    * **Post-Order Traversal** (from left, right, root)
        - recursive calling on the node's left subtree, recursive calling on the node's right
          subtree, visit the node
        - use to explore the leaves before roots, to delete a tree from leaf to root
    * **Level-Order Traversal** (level by level)
        - visit nodes level by level, same as breadth-first search

Graph Traversals
----------------
    * **Depth-first Search**
          1\. start at any random vertex
                    2\. mark current vertex as visited
                    3\. iterate current vertex's adjacent vertices
                    4\. ignore if the adjacent vertex has been visited
                    5\. if the adjacent vertex is not visited, do recursive depth-first search on it
    * **Breadth-first Search**
          1\. start at any random vertex
                    2\. mark the starting vertex as visited
                    3\. add the starting vertex to a queue
                    4\. run a loop while the queue isn't empty
                    5\. in the loop, remove and use the first vertex as current vertex
                    6\. iterate all adjacent vertices of the current
                    7\. ignore if the adjacent vertex has been visited
                    8\. if the adjacent vertex is not visited, mark it as visited and add it to the queue
                    9\. repeat the loop from step 4 until the queue is empty

KMP
---
    * Knuth-Morris-Pratt string matching algorithm
    * efficiency of the algorithm is avoiding comparison if the beginning part of the pattern
      is already matched
    * **Longest Prefix Suffix**
        - check the longest prefix of the pattern that is also the suffix
        - the first part of the algorithm is to build LPS array with every prefix length of
          the pattern substring
        - the first value of LPS array will always be 0 as it cannot match itself
        - loop through the pattern with ``curr_ptr`` and ``prev_lps_ptr``
        - if characters at both pointers match, set LPS value at ``curr_ptr`` to
          ``prev_lps_ptr + 1``, and increment both by 1
        - if ``prev_lps_ptr == 0``, set LPS value at ``curr_ptr`` to 0 and only increment
          ``curr_ptr`` by 1
        - otherwise, set ``prev_lps_ptr`` to LPS value at ``prev_lps_ptr - 1``
    * **Pattern Matching**
        - loop with two pointers for search string, ``search_ptr``, and pattern string,
          ``pattern_ptr``
        - increment both pointers if characters match
        - if characters do not match, need to check the value of LPS with last matched index,
          by setting ``pattern_ptr = LPS_ARR[pattern_ptr - 1]``, but if ``pattern_ptr == 0``, just
          increment ``search_ptr``
        - when ``pattern_ptr`` is at the end, we have found a match
    * **Complexity**
        - all cases: O(n + m), space O(m)

Dynamic Programming
-------------------
    * problems have patterns such as find min/max, distinct ways, merging intervals, dp on
    strings or decision making
    * **min/max**
        - find min/max cost, path or sum to reach the target
        - approach: choose min/max path among all possible paths before the current state,
          then add value for the current state

Optimization
------------
    * determine the current efficiency before optimizing
    * come up with best imaginable Big O or best conceivable runtime
    * if current and the imagined complexities are not same, it can be optimized
    * it's not always possible to achieve the imaginable Big O
    * find patterns within the problem by using many example inputs/outputs
    * sometimes changing data structure can help with optimization

`back to top <#data-structure--algorithms>`_

Maths
=====

* `Modular Arithmetic`_, `Prime Number`_, `Factors`_

Modular Arithmetic
------------------
    * finding remainder and using it to calculate further to fit in the given range
    * p = any given prime number for calculation
    * Addition
        - for ``a + b = c``, use ``((a%p) + (b%p)) % p = c``
    * Subtraction
        - for ``a - b = c``, use ``((a%p) - (b%p)) % p = c``
        - if ``((a%p) - (b%p)) < 0``, use ``(((a%p) - (b%p)) + p) %p = c``
    * Multiplication
        - for ``a * b = c``, use ``((a%p) * (b%p)) % p = c``
    * Division
        - for ``a/b = c``, inverse the denominator first, ``a * inv(b) = c``, then do
          multiplication, ``((a%p) * (inv(b)%p)) % p = c``
        - ``inv(b) = power(b, p-2)``

Prime Number
------------
    * x = number given to check prime or not
    * O(n)
        - check if divisible by numbers from 2 to x
    * O(log n)
        - check if divisible by numbers from 2 to x/2
    * O(sqrt(n))
        - check if divisible by numbers from 2 to sqrt(x)

Factors
-------
    * x = number given to find factors
    * O(sqrt(n))
        - divide by 2 until all factors of 2 are gone
        - then divide by odd numbers starting from 3 to sqrt(x/2)

`back to top <#data-structure--algorithms>`_
