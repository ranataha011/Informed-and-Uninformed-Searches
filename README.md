# Informed-and-Uninformed-Searches
1. Steps Involved in the Program
Global Architecture & Constraints
•	Manual Data Structures: To strictly adhere to the assignment requirement of avoiding built-in dynamic list methods (e.g., .append(), .pop(), slicing, or heapq), custom MyStack, MyQueue, and MyPriorityQueue classes were implemented from scratch. These rely on fixed-size arrays ([None] * capacity) and manual index tracking (top, head, tail, size).
•	Grid Encoding & Indexing: The grid operates strictly on a 0-based indexing system for row and column coordinates. Internally, the grid exclusively uses 0 to represent a free cell and 1 to represent a blocked cell.
•	Cost Handling (Parallel Array): To correctly demonstrate cost-aware algorithms (UCS and A*) without violating the 0 and 1 structural encoding rule, a parallel costs 2D array was implemented. This allows the GUI to "paint" variable traversal costs (1, 5, or 9) onto free cells.
Algorithm Execution Steps
1.	Breadth-First Search (BFS):
o	Utilizes the manual FIFO MyQueue.
o	Explores the grid level by level. Nodes are marked as visited immediately upon enqueuing to prevent redundant checks.
o	BFS completely ignores the parallel terrain costs array, guaranteeing the optimal/shortest path specifically in terms of the total number of steps.
2.	Depth-First Search (DFS):
o	Utilizes the manual LIFO MyStack.
o	Explores paths deeply down a single branch until a dead end or the goal is reached.
o	Infinite loops are successfully prevented by verifying nodes against a 2D boolean visited array before expansion.
3.	Uniform Cost Search (UCS):
o	Utilizes the manual Min-Heap MyPriorityQueue.
o	Nodes are ordered dynamically by their cumulative path cost ($g$). When expanding, the algorithm adds the specific terrain cost of the destination cell to the running total.
o	If a cheaper path to an already-discovered node is found, its parent pointer and cost are updated, and it is pushed back into the priority queue to ensure strict least-cost optimality.
4.	A Search (Manhattan & Euclidean):*
o	Utilizes the same MyPriorityQueue, but nodes are ordered by the evaluation function $f = g + h$.
o	The heuristic ($h$) calculates either the Manhattan or Euclidean distance to the goal.
o	Tie-breaking: If two nodes share the same $f$-value, the custom priority queue's _bubble_up logic forces a tie-break by prioritizing the node with the lower $g$-value.
5.	Path Reconstruction:
o	Once the goal is reached, the algorithm traces parent pointers backward. The MyStack structure is used to naturally reverse this sequence, printing the final path to the console (Row Col) and painting it blue on the GUI.
________________________________________
2. Screenshots of the Results
A. Graphical User Interface  Overview
B. Breadth-First Search (BFS) vs. Terrain
 
C. Uniform Cost Search (UCS) Finding the Least-Cost Path
 
D. Depth-First Search (DFS) Exploration 
E. A* Search (Manhattan Heuristic)
 F. A* Search (Euclidean Heuristic)
 G. Failure Handling (Goal Unreachable)
  
________________________________________
3. Time Complexity of Implementations
Assuming $N$ represents the total number of rows and $M$ represents the total number of columns:
•	BFS: O(N \times M)
o	Reasoning: In the worst-case scenario (e.g., a maze), every free cell in the grid and its up to 4 valid neighbors are evaluated exactly once. Manual queue push/pop operations take $O(1)$ time.
•	DFS: O(N \times M)
o	Reasoning: Similar to BFS, DFS visits each node and its edges at most once before backtracking. Manual stack push/pop operations take $O(1)$ time.
•	UCS: O(N \times M \log(N \times M))
o	Reasoning: In the worst case, every cell is inserted into the manual priority queue. The custom _bubble_up and _bubble_down heap array operations take logarithmic time relative to the number of elements currently in the frontier.
•	A (Manhattan & Euclidean):* O(N \times M \log(N \times M))
o	Reasoning: The strict worst-case mathematical bound is identical to UCS because it relies on the same priority queue sorting mechanism. However, in practical application, the heuristic heavily prunes the search space, resulting in significantly fewer node expansions than UCS.
________________________________________
4. Space Complexity of Implementations
Because Python's dynamic list .append() functionality was strictly prohibited, all data structures were manually initialized with fixed-size arrays (capacity=10000). Therefore, the allocated space is technically $O(1)$ constant overhead. However, analyzing the effective algorithmic space complexity yields:
•	BFS: O(N \times M)
o	Reasoning: Requires spatial memory for the manual queue tracking the frontier, the 2D boolean visited array, and the 2D parent pointer array.
•	DFS: O(N \times M)
o	Reasoning: Requires spatial memory for the manual stack (acting as the recursion tree equivalent), alongside the visited and parent 2D arrays.
•	UCS: O(N \times M)
o	Reasoning: Requires memory for the manual priority queue array, the 2D $g$-costs tracking matrix, and the 2D parent pointer array.
•	A:* O(N \times M)
o	Reasoning: Operates with the exact same spatial footprint requirements as UCS, storing the running $g$-costs, parent matrices, and the heap array.

