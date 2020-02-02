Back To Back SWE

---
Stacks & Queues
https://github.com/bephrem1/backtobackswe/tree/master/Stacks%20%26%20Queues
---

Reverse Polish Notation: Types of Mathematical Notations & Using A Stack To Solve RPN Expressions

postfix(reverse polish)
infix
prefix(polish)

Question: Given an array with a sequence that represents a RPN expression, evaluate the Reverse Polish Notation expression.

The code: https://github.com/bephrem1/backtobac...

This is one of those textbook problems. It is a classic expression of when LIFO behaviour is favorable to us when solving certain problems.

To have an RPN expression we need 2 things by definition:

A single digit or series of digits.

It is in the form ["A", "B", "o"] where A and B are integers and o is an operator (either +, -, *, or / ).


Examples:

[ "3",  "4", "+", "2", "*", "1", "+" ]

is the same things as ( ( 3 + 4 ) * 2 ) + 1

which is the same things as ( 3 + 4 ) * 2 + 1 because of order of opeartions.

Example 2:

[ "1",  "1", "+", "2", "2", "*", "+" ]


Approach

This is a classic stack problem, let us just do this.

The 2 key operations:

When we see a digit we push it to the stack.

When we see an operation we perform 2 pops, apply the operation between the 2 values (first popped item goes on left of the sign, 2nd popped item goes on the right of the sign), and then push the result back onto the stack so we can work with it as we continue.

If it is a valid RPN expression then we should have no problems with mismatches and null pointers, clarify that it is a valid RPN string always with your interviewer.

Complexities

n is the length of the RPN expression

Time: O( n )

We will process all n operators/operands in the expression. Each will either entail an O(1) push/pop or an O(1) arithmetic calculation.

Arithmetic operations take O(1).
Push and pop take O(1).

Space: O( d )

Let d be the total operands (numbers).
Let b be the number of operators (+,  -,  *,  /)

If we have d digits and b operators where d + b = n, we certainly will not use O( d + b ) space (operators are not pushed to the stack).

A worst case is that we have all numbers, followed by the operators. And we will have a stack height of d digits. So we can bound to that.

---

Implement A Queue Using Stacks - The Queue ADT ("Implement Queue Using Stacks" on LeetCode)

Question: Implement a queue (a FIFO structure...first-in-first-out) only using stacks internally as efficiently as possible.

The code: https://github.com/bephrem1/backtobac...

Complexities

n is the total items between the 2 stacks (in the overarching queue)

Time: O( 1 ) - amortized (for enqueue and dequeue operations)

Amortized time is the way to express the time complexity when an algorithm has the very bad time complexity only once in a while besides the time complexity that happens most of time (https://medium.com/@satorusasozaki/am...).

The motivation for amortized analysis is that looking at the worst-case run time per operation, rather than per algorithm, can be too pessimistic. (https://en.wikipedia.org/wiki/Amortiz...).

Space: O( n )

We upper bound space to the maximum amount of items that we will ever store.

---

Implement A Max Stack - A Stack With A .max() API (Similar To "Min Stack" on LeetCode)

Question: Design a stack that includes a max operation, in addition to push and pop. The max method should return the maximum value stored in the stack.

The code:
https://github.com/bephrem1/backtobackswe/blob/master/Stacks%20%26%20Queues/MaxStack/MaxStack.java

A stack is an abstract data type (ADT).

An ADT is a data type is defined by its behavior from the point of view of a user of the data, we can implement it however we want as long as we give the user the interface they need to get what they expect from the ADT.


Stack is LIFO since the last item to go in, is the first one to come out.

Here we are asked to implement a stack with a max API which entails.

push()
Add an element

pop()
Get the top element

max()
Get the maximum element


The reason this is a problem is because the stack ADT restricts how data is added and retrieved from the structure so to make the .max() function run optimally we will have to think a little harder than the first approach we think of.


Approach 1 (No Optimizations & Obvious)

To answer a call to .max() we can iterate through the whole structure supporting our stack underneath and return the maximum item
whether it is implemented with an array, a lined list, etc.

This will take O(n) "linear" time always and O(1) "constant" space


Approach 2 (Use Auxiliary Data Structures)

We can use a max heap or a Binary Search Tree in tandem with a hashtable for fast lookups.

This idea is critical. Auxiliary structures with a hash table backing your ordered structures for quick lookups to get immediate reference to objects.

The structure solves the problem.

The hashtable let's you get into it fast.

This can bring us to log(n) time, but worsens space to O(n) since we will store at max all items potentially.


Approach 3 (Remember The Max At Each Element)

Write the optimal code [in EPI], but walkthrough this first.

On push it is easy to know what the max is if we have a global variable knowing the max.

We just compare the item we are adding to that max.

If the pushed item is greater then we update the global variable.
If the pushed item is less then we do nothing to the global variable.

On pop there is a problem, if we pop the element that was the maximum in the stack to that point, we have no idea what the next maximum item is unless we do an O(n) scan to restore the global variable.

So the solution is to remember the maximum in the stack at or below each item.

IF WE REMEMBER THE MAX AT EACH PUSH THEN WE SOLVE THE PROBLEM OF BEING LOST ON POP.

Complexities

Time: O( 1 )
.max() knows the maximum immediately

Space: O( n )
We need to store the max on every element upping our space usage


BIG TWEAK

We can improve the best case.

Notice that if a pushed element is less than the maximum, then it can never be the maximum.

We will use an auxiliary stack to record historical maximums and its count of occurrences (we can't just store the raw values becase of possible duplicates).

Complexities

Time stays O( 1 ), we are just optimizing space
Worst case space stays O( n )

If each pushed item is a max and needs a cached max entry
But best case for space becomes O(1) best case if the max does not change frequently.

---

The Balanced Parentheses Problem - Classic Stack Problem ("Valid Parentheses" on Leetcode)

Question: Write a program that tests if a string made up of the characters '(', ')', '{', '}', '[', and ']' is balanced.

Examples

" [ () () () ] { } { [ ( ) ] } " valid
" { } { ) [ ] " invalid

This is another textbook stack problem. Very widely known.

Notice how your brain tries to solve this problem subconsciously. Your eyes will try to scan from left to right keeping track of 2 critical things.

1.) The parentheses that are open
2.) The type of bracket that is open

When the matching closing parenthesis is found you realize that opening bracket is ok, it has been matched, and your mind then worries about the rest of the open brackets.


The Approach

We will scan the string left to right.

The key is that all points we will keep track somehow of the brackets that have been opened (left brackets) but not closed.

When we see a closing bracket we want to be sure that the most recent open bracket is of the same type (and that there even is a left bracket that it is matching).

A stack is perfect for this problem. (the LIFO property is a good fit)


Our Algorithm

-) When we see an open bracket we will push the bracket to the stack, it is now being tracked. It must be closed by the appropriate bracket at some point or else we have a problem.

-) When we see a closing bracket, we peek the top of the stack to make sure it is the correct type of opening bracket.

-) If it is another closing bracket or the wrong type or there is no bracket then we will return false, the parentheses set is not balanced

-) If we get through the whole string without a mismatch then the set is balanced


Time & Space Complexities

Let n be the length of the parentheses of the string.

Time: O( n )

We will traverse the whole string (if it is well formed and execution does not stop later).

The string is length n (has n brackets).

Space:

Worst Case (for valid string)
O( n / 2 ) = O( n )

Worst case, if your string is well-formed, we have to store half of the string if we have all open brackets followed by all matching closing brackets.

Example: " ( ( ( ( ) ) ) )"
n = 8
The stack will hold maximum 4 items at one point in time.



Worse Case (for invalid string)
O( n )

Worst case, if the string is not well-formed, we at worst could get n open brackets that see no matches at all in the string.

n = 8
" ( ( ( ( ( ( ( ( "
The stack will hold maximum 8 items at one point in time.

---

Trees, Binary Trees, & Binary Search Trees
https://github.com/bephrem1/backtobackswe/tree/master/Trees%2C%20Binary%20Trees%2C%20%26%20Binary%20Search%20Trees

---

Serialize & Deserialize A Binary Tree - Crafting Recursive Solutions To Interview Problems

Question: Given a root node for a binary tree, serialize the binary tree into a string representation. Deserialize that same string representation into a tree identical to the original tree given.

The code: https://github.com/bephrem1/backtobac...

The key is to understand what our fundamental operation/job is in each call to the function.

Our fundamental job is to encode/materialize a single node/int value.

Compiler Design Phases: https://www.geeksforgeeks.org/phases-of-a-compiler/

---

Lowest Common Ancestor Between 2 Binary Tree Nodes (A Recursive Approach)


Question: Given the root of a binary tree and 2 references of nodes that are in the binary tree, find the lowest common ancestor of the 2 nodes. The nodes do not have parent pointers.

The code: https://github.com/bephrem1/backtobac...


The Approach

So there are many modulations of this problem where you can build a hashtable and make parent pointers, etc. We will focus on the recursive solution.


The Algorithm

The key is that we want to root ourselves at a node and then search left and then right for either of the 2 nodes given.

If we see either node, we will return it, if we do not find the node in a subtree search the value of null will be returned and bubbled up.

After we search both left and right we ask ourselves what our results mean.

If we found nothing to the left, we just bubble up what is on the right (whatever that search result may be). This node we sit at cannot be the LCA since the left and right did not yield the 2 nodes we want.

If we found nothing to the right, we just bubble up what is on the left (whatever that search result may be). This node we sit at cannot be the LCA since the left and right did not yield the 2 nodes we want.

If both the right and left result are not null, we have found our LCA.


Why?

We know it is an ancestor at the least but we definitely know it is the lowest common ancestor because we went bottom upwards, whatever we hit will be the LCA and it will bubble up.


Complexities

Time: O( n )

We will be touching the whole tree in the search, there are n nodes in the tree and we do O(1) work at each node. There are not exactly n calls though but I need to double check this...I need to solve the recurrence but oh well...we know it will stay linear in the asymptotic regard.

Space: O( h )

Stack usage at maximum will be the trees height. Worst case would be O(n) if our tree is skewed purely to the left or right and we need to find deep nodes. But n IS h in that case. But we say O( n ) in that case since it is more accurate to what is happening, the tree's size in nodes dominating the height.

---

All Nodes Distance K In A Binary Tree - Performing Bidirectional Search On A Tree Using A Hashtable

Question: You are given 3 things. The root of a binary tree, a single start node in the binary tree, and a number k. Return all nodes that are k "hops" away from the start node in the binary tree. Return a list of the values held at those nodes.

The code: https://github.com/bephrem1/backtobac...


Complexities

n = total amount of nodes in the binary tree
m = total edges
n=m+1

Time: O( n + m )=O(n)

This is standard to Breadth First Search. We upper bound the time by the number of nodes we can visit and edges we can traverse (at maximum).

Space: O( n )

We have a hashtable upper bounded by n mappings, a mapping to each node's parent.

queue:bfs
stack:dfs

---

Binary Tree Level Order Traversal - Drawing The Parallel Between Trees & Graphs

Question: You are given the root node of a binary tree. Return a list consisting of each level of the tree (we will have a list of lists, a list for each level's items), where each level is traversed from left to right. A level order traversal.

The code: https://github.com/bephrem1/backtobac...

What Do We Know?

We know our preorder, inorder, and postorder traversals...but this is a little different.

We want to go level by level here...what does this remind us of?

Every acyclic connected graph is a tree, and all trees are acyclic connected graphs.

This unlocks our ability to search a tree list we search a graph, using Breadth First Search and Depth First Search.

DFS will go deep, and BFS will go out level by level. We want BFS since it is a more natural approach to something like this.

We realize that this is just the breadth-first search of a tree structure. 

We shift our knowledge from graph search to this problem.

Complexities

n is the total amount of nodes in the binary tree

Time: O( n )

Space:

With output: O( n ) because we will store n nodes in the list of levels structure.

Without output: O( n ) the queue at maximum at any point in time will hold some fractional component of the total nodes in the tree.

The fractional constant disappears and the asymptotic behavior is linear.

We can't bound the max space in the queue to the widest level because when we process the widest level...if there is a level below it we will have all the nodes from the widest level PLUS what we are adding into the queue as we process each node in the level (we remove 1 node and can add 2 children which will push us past the size of the widest level initially).

---

Binary Tree Bootcamp: Full, Complete, & Perfect Trees. Preorder, Inorder, & Postorder Traversal.

The code: https://github.com/bephrem1/backtobac...

Full Binary Tree: Every node (besides children) has exactly 2 children (the maximum children a node can have in a binary tree). (or no children)

Complete Binary Tree: Every level, except possibly the last, is completely filled, and all nodes are as far left as possible.
binary heap 
top->bottom left->right

Perfect Binary Tree: All interior nodes have two children and all leaves have the same depth or same level. Perfect binary trees are both full and complete.

Preorder Traversal: node left right

Inorder Traversal: left node right 
binary search tree

Postorder Traversal: left right node

O(n)

---

Implement A Binary Heap - An Efficient Implementation of The Priority Queue ADT (Abstract Data Type)

Question: Implement a binary heap (a complete binary tree which satisfies the heap ordering property). It can be either a min or a max heap.

The code: https://github.com/bephrem1/backtobac...

Video On Heap Sort: https://www.youtube.com/watch?v=k72Dt...

complete binary tree

Priority Queue ADT (Abstract Data Type)

The ADT's Fundamental API

isEmpty()
insertWithPriority()
pullHighestPriorityElement()
peek() (which is basically findMax() or findMin()) is O(1) in time complexity and this is crucial.

A heap is one maximally efficient implementation of a priority queue.

We can have:
-) a min heap: min element can be peeked in constant time.
-) or a max heap: max element can be peeked in constant time.

The name of the heap indicates what we can peek in O(1) time.

It does not matter how we implement this as long as we support the expected behaviors of the ADT thus giving our caller what they want from our heap data structure.

A binary heap is a complete binary tree with a total ordering property hence making it a heap with O(1) peek time to the min or max element.

We can implement the heap with actual nodes or we can just use an array and use arithmetic to know who is a parent or left or right child of a specific index.

Insertion into a binary heap: O(logn)

We insert the item to the "end" of the complete binary tree (bottom right of the tree).

We "bubble up" the item to restore the heap. We keep bubbling up while there is a parent to compare against and that parent is greater than (in the case of a min heap) or less than (in the case of a max heap) the item. In those cases, the item we are bubbling up dominates its parent.

Removal from a binary heap: O(logn)

We give the caller the item at the top of the heap and place the item at the "end" of the heap at the very "top".

We then "bubble the item down" to restore the heap property.

Min Heap: We keep swapping the value with the smallest child if the smallest child is less than the value we are bubbling down.

Max Heap: We keep swapping the value with the largest child if the largest child is greater than the value we are bubbling down.

In this manner, we restore the heap ordering property.

The critical thing is that we can have O(1) knowledge of the min or max of a set of data, whenever you see a problem dealing with sizes think of a min or max heap.

---

Test If A Binary Tree Is Height Balanced ("Balanced Binary Tree" on LeetCode)

Question: Write a program that takes the root of a binary tree as input and checks whether the tree is height-balanced.

The code: https://github.com/bephrem1/backtobac...

A tree is height balanced if for each node in the tree, the difference in the height of its left and right subtrees is at most one.


Approach 1 (Get Height Of Tree Rooted At Each Node)

We can perform a traversal of the tree and at each node get the height of its left and right subtrees.

This wastes time as we will be repeating work and the traversal of nodes.


Approach 2 (Drill Down With Recursion And Respond Back Up)

We can notice that we don't need to know the heights of all of the subtrees all at once.

All we need to know is whether a subtree is height balanced or not and the height of the tree rooted at that node, not information about any of its descendants.

Our base case is that a null node (we went past the leaves in our recursion) is height balanced and has a height of -1 since it is an empty tree.

So the key is that we will drive towards our base case of the null leaf descendant and deduce and check heights on the way upwards.


Key points of interest:

1.) Is the subtree height balanced?
2.) What is the height of the tree rooted at that node?


Complexities

Time: O( n )

This is a postorder traversal (left right node) with possible early termination if any left subtree turns out unbalanced and an early result bubbles back up.

At worst we will still touch all n nodes if we have no early termination.

Space: O( h )

Our call stack (from recursion) will only go as far deep as the height of the tree, so h (the height of the tree) is our space bound for the amount of call stack frames that we will create

---

Count Total Unique Binary Search Trees - The nth Catalan Number (Dynamic Programming)

Question: Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?

The code:
https://github.com/bephrem1/backtobac...

Catalan Numbers(sequence):  https://en.wikipedia.org/wiki/Catalan_number

Define G(n): the number of unique BST for a sequence of length n.

G(0) = 1
G(1) = 1

For each element, we can place it and recurse on the left and right not including that number we choose to root that subtree.

Define F(i,n): the number of unique BST, where the number i is served as the root of BST (1 is less than or equal to i which is less than or equal to n).

G(n) = the summation from 1 to n of F(i, n)

When we choose an element to root a subtree from the possible elements to root there, we split the possibilities into a left and a right of the original enumeration.


Example:

n = 5 aka [1, 2, 3, 4, 5]

When evaluating F(3, 5) we will have
a left subtree with [1, 2] aka G(2)
a right subtree with [4, 5] aka G(2)

G(n) evaluates the # of unique trees we can construct from a total of n possible values, regardless of what those values are.


The Realization

We notice that F(i, n) = G(i - 1) * G(n - i)

To get F(i, n) we are considering all combinations of left tree possibilities with all of the right tree possibilities.

These exhaustive pairings between two sets of items is called a cartesian product.

Now we have a new way to express F(i, n)
F(i, n) = G(i - 1) * G(n - i)

G(n) = summation from 1 to n of G(i - 1) * G(n - i)

This now can be solved with DP via top-down recursion or bottom up.

For each G(n) up to our requested n we will solve the G(n) summation equation.

At the end we return the G(n) requested which by the end we will have an answer for.

Time: O(n^2)
Space: O(n)

---

Test If A Binary Tree Is Symmetric ("Symmetric Tree" on Leetcode)

Question: Write a program to check if a binary tree is symmetric in structure as well as value.

A binary tree is symmetric if we can draw a vertical line down from the root and the left and right subtrees are mirrors of each other in structure and value.

The least this could be is O(n) time because we have to inspect all n nodes values to ensure that they conform to the tree being symmetric.

All we need to do is traverse the tree correctly with our recursion checking pairs for conformity.


The Conditions

If the root is null, the tree is symmetric.

Our checking function will take 2 nodes in to compare for symmetry.


Our Base Cases

If both nodes passed in are null, by default we have a symmetric tree.

If one node is null and the other is not, then we have incongruence, the tree is not symmetric since a pair failed.

If both nodes are null, then compare values. If values are equal, then good. Go left and go right.


How do we come up with this on the spot?

First recognize the base cases.

Then recognize we will need some way to compare pairs of nodes since that's what matters when checking for symmetry.

Even if this were an array that is how we would do it, pairs of indices.

Then recognize the way the recursive case must continue the work on the way downward and your solution would look like this at the end.


Complexities

Time: O(n)

Space: O(h) (as we discussed, it can't really be O(n) worst case since
the first check would fail if the tree is skewed. The worst case IS O(h))

---

Linked Lists & Hashtables

---

Clone An Undirected Graph - The Utility of Hashtable Mappings ("Clone Graph" on Leetcode)

Question: Design an algorithm that takes a reference to a vertex u, and creates a copy of the graph on the vertices reachable from u. Return the copy of u (as it is the entry to the cloned section of the graph we will have just created).

The code:
https://github.com/bephrem1/backtobac...


The Approach

When we clone a structure like a graph or linked list we will use a hashtable to assist in the cloning. We will map each node or structure to its corresponding clone.

We will clone in a breadth-first manner so we can fill out all of the adjacent relationships in the cloned graph.

This means we will use a queue. (Queue for BFS, Stack for DFS. This is because each data structure enforces how nodes are searched).


The Algorithm

Add the start node the caller gave us to the queue and map the node to its clone through the hashtable.

We will continue the cloning until the queue is empty (there will be no more nodes to process).

We then pull a node from the queue, call it currVertex.

We will iterate all of currVertex's adjacent nodes, call each object yielded adjVertex.

Do we create a cloned node for adjVertex?

If the adjVertex is NOT in the hashtable create a mapping for adjVertex (adjVertex to its value clone, as described before) and add adjVertex to the queue since it needs its adjacent nodes mapped out in the cloned graph.

Add the cloned node of adjVertex as an adjacent node to the cloned node of currVertex.


Complexities

V is the number of vertices in the graph section that is cloned.
E the number of adjacent edges coming off vertices.

Time: O( | V | + | E | )

We will touch V nodes and traverse E edges.

Space: O( | V | + | E | ) with result

The cloned graph we return (if we include it in the space complexity) will be the sum of the space of the cloned vertices and edge relationships that each node must maintain (we represent our graph as an adjacency list).

Without the result it is O( | V | ) because we will store V vertices in the hashtable (and the queue can hold at worst some fractional multiple of the total number for vertices...imagine 1 node connected to 9 nodes all at once in a graph of size 10...and we start from that 1 node. Our queue would have 9 nodes in it at once on the first iteration).

---

Minimum Window Substring: Utilizing Two Pointers & Tracking Character Mappings With A Hashtable

Question: Given a string S and a string T, find the minimum window in S which will contain all the characters in T (matching or exceeding in frequency) in complexity O(n).

The code: https://github.com/bephrem1/backtobac...

Anytime we have time reduced to linear time and we know that much must be done, that is indicative of using some sort of auxiliary structure to keep track of information for us so time can stay low in complexity.

Complexities

s = length of search string
t = length of "pattern" or "character" string

Time: O( s + t )
At worst we will touch each element twice, once by the left pointer and once by the right pointer.

Ex:
s = "aaaaat"
t = "t"

We will spend t time to build the character requirements hashtable.

Space: O( s + t )

At worst the window hashtable will have a mapping for every character in s.

At worst t will have all unique characters.

s = "abcdef"
t = "abcdef"

Bottleneck, unnecessary work, duplicate work

lower bound omega(n^2) least work could be done, may be optimized later to linear

2 choices: expand / contract the window

---

Clone A Linked List (With Random Pointers) - Linear Space Solution & Tricky Constant Space Solution

Question: You are given a standard linked list with next pointer BUT each node carries an additional random pointer to any given node in the linked list. Clone the linked list.

The code: https://github.com/bephrem1/backtobac...

Let us first see the mental barriers and problems that we face while solving this.

It is trivial to make a copy of the linked list nodes with only the next pointers, but the random pointer on each node presents a problem.

We will notice that we have trouble establishing the connection between the original linked list and the random pointers between nodes in the cloned linked list.


Approach 1 (Linear Space Solution)

Use a hashtable to facilitate the cloning.

Complexities

Time: O( n )

We will perform linear time traversals that keep our asymptotic behavior linear.

Space: O( n )

We will store a clone mapping for each node entailing linear space complexity with respect to the original list passed to us.


Approach 2 (Constant Space Solution)

Use the original list's node's next pointer to map to the clone list.

Complexities (normally best case)

Time: O( n )

We are still only doing linear time traversals

Space: O( 1 )

We do not store any auxiliary space that will scale as the input size gets arbitrarily large.

---

Merge 2 Sorted Lists - A Fundamental Merge Sort Subroutine ("Merge Two Sorted Lists" on LeetCode)

Question: Given two lists that are sorted, merge them into a single sorted sequence as efficiently as possible.

The code: https://github.com/bephrem1/backtobac...

The key with linked list problems is pointers. Most of what we will do will be O(n) time and O(1) space.

It is all about hardwiring things together and moving pointers around, have a strong understanding of techniques to advance, stash, and move references around.


Approach 1 (Brute Force)

Append the lists together and then sort the lists.

The best we can do with this is O( n * log(n) ) because we will only know the total ordering property of the lists which lets us use (Mergesort or Quicksort, etc.)


Approach 2 (Use Pointers And Rewire Lists)

Huge tip. When building a new list while doing linked list problems dummy heads are your best friend. They prevent you from having to do null checks on a list and you can immediately append to the .next value through a pointer to it with no fear of a null pointer exception.

We just keep:
1.) a pointer to the last item in the new list we are building
2.) a pointer into the first list
3.) a pointer into the second list

We then do pair comparisons and advance accordingly.


Complexities

Time: O( m + n )

Let n be the length of list 1.
Let m be the length of list 2.

This is the worst case. We will be traversing the whole length of the lists in the case where they are nearly similar in length and value comparison results (aka we don't exhaust a list early before another).

Best case is O( min( m, n ) ) because we will only traverse as far as we need to exhaust the shorter of the lists, then we just append the rest of the other onto the result since it will all be greater than the exhausted list by default.

Space: O( 1 )

We are only working with pointers. We are using the existing nodes given to us.

---

Implement An LRU Cache - The LRU Cache Eviction Policy ("LRU Cache" on LeetCode)

It is a cache replacement policy that says to evict the least recently used item in the cache.

Every time an item is used it goes to the "front" of the cache since it has been used and has priority.

Items that are not used will go to the "end" of the cache eventually and get evicted since they are the least used items, they never got a bump up by being used.

Additional Cache Eviction Policies: https://en.wikipedia.org/wiki/Cache_r...


What is a Cache?

A cache is a hardware or software component that stores data so that future requests for that data can be served faster; the data stored in a cache might be the result of an earlier computation or a copy of data stored elsewhere.


What Is An LRU Cache?

So a LRU Cache is a storage of items so that future access to those items can be serviced quickly and an LRU policy is used for cache eviction.


The Constraints/Operations

Lookup of cache items must be O(1)
Addition to the cache must be O(1)
The cache should evict items using the LRU policy


The Approach

There are many ways to do this but the most common approach is to use 2 critical structures: a doubly linked list and a hashtable.


Our Structures

Doubly Linked List: This will hold the items that our cache has. We will have n items in the cache.

This structure satisfies the constraint for fast addition since any doubly linked list item can be added or removed in O(1) time with proper references.

Hashtable: The hashtable will give us fast access to any item in the doubly linked list items to avoid O(n) search for items and the LRU entry (which will always be at the tail of the doubly linked list).

This is a very common pattern, we use a structure to satisfy special insertion or remove properties (use a BST, linked list, etc.) and back it up with with a hashtable so we do not re-traverse the structures every time to find elements.


Complexities

Time

Both get and put methods are O( 1 ) in time complexity.

Space

We use O( n ) space since we will store n items where n ist the capacity of the cache.

---

How To Reverse A Singly Linked List | The Ultimate Explanation (Iteratively & Recursively)

So the 2 ways we will really do it is to move the pointers around

Way 1 (Iterative):

For this approach we need to keep track of a prev and curr node. Why? This list is singly-linked so we can't get a node's predecessor, so during the traversal we must remember who came before the node we are at.

Before doing any of that we save the next node's address since if we move the node we are at's address, we would lose where to go next in the traversal.

Then finally we set the current node to the next node that we saved at the start of the iteration.

Complexities:

Time: O(n)
Space: O(1)

Way 2 (Recursive):

This is "top down" recursion, we start at the top and drill down. Then when we reach the bottom (the base case) we will come back upwards with the answer.

The recursive code will drill down in recursive calls until we get to the base case.

The base case gets triggered when the current node has no next, this means we are at the list's end, in that case we just return the node passed into the call.

Then the node before the last node has itself and the last node to work with.

We do some pointer movement.

Last node points to curr.

curr pointer to null since we don't know if this is the first node in the list.

first node will end up the last node in the list.

Then we return the head of the new linked list on the way back upwards.

The we repeat, what we get from the recursive call is a reversed list, we append our node to that list, and then return back upwards with an even longer reversed list.

In the top call we say:

"Reverse the rest of the list"
"Reverse the rest of the list"
"Reverse the rest of the list"
I'm the last node "Here I am 2nd to last node"
"processing"
3rd to last gets a reversed list, append
4th to last gets a reversed list, append
......and so on

Complexities:

Time: O(n)
Space: O(n)

Call stack space used.

---

Find The Middle of A Linked List


All a linked list is an idea. It is what we call a collection of linked objects called nodes. A linked list is not really a concrete list, it is an abstraction.

Each of these linked structures is called a node.

Our nodes will have the data we want whatever that may be. Then they have a pointer to the next node.

A pointer is just the memory address of the next item. It points to something, that something being a location. Locations in computers are notated by memory addresses.

When we have a linked list we work with pointers. It is all about messing around with pointers and doing little tricks and rearrangements.

Linked list problems are all about doing pointer manipulation, solutions will often have O(n) time and O(1) space.

For this problem there are 2 ways we can do it, the obvious way and the less obvious way.

Approach 1:

Go over the whole list to get the size. Divide the size by 2 and start from the beginning again and advance that many nodes forward. You now have the middle.

Time: O(n)
Space: O(1)

Approach 2:

Use 2 pointers. A fast and slow pointer.

This is a common linked list problem thing.

Advance the fast pointer 2 memory addresses per loop iteration, advance the slow pointer 1 memory address per loop iteration.

aka

while (fast != null && fast.next != null)

At the end the answer is the slow pointer.

The even and odd list case have no difference if we do it like this.

Time: O(n/2) = O(n)
Space: O(1)

Time complexity is the same as the first approach, but this will be typically faster but we have no idea what optimizations the compiler is making when compiling our code.

---

Graphs, Greedy Algorithms, & Other

---

Fast Multiplication: From Grade-School Multiplication To Karatsuba's Algorithm (later)

https://www.youtube.com/watch?v=-dfsxsiGoC8&list=PLiQ766zSC5jNlilqsJSiVcvIMxTdMlJbl&index=1

---

Asymptotic Bounding 101: Big O, Big Omega, & Theta (Deeply Understanding Asymptotic Analysis)

Great Resource: https://cathyatseneca.gitbooks.io/data-structures-and-algorithms/content/analysis/notations.html

Big O Cheat Sheet: http://bigocheatsheet.com


We will not only consider the informal definition but rather also look at the mathematical understandings behind why we call these asymptotic “bounds”.

Again, we care about this because the true colors of an algorithm can only be seen in the asymptotic nature of runtime and space.

So imagine this, we have these components:

A function T(n) which is the actual number of comparisons, swaps...just...resources an algorithm needs in terms of time or space. It is a function of n. When n changes, T(n) changes.

Our job is to classify behaviour.

A bound O( f(n) ) is the function that we choose to apply for the specific bounding.

The definitions, an example:
"T(n) is O(f(n))" iff for some constants c and n0, T(n) less than or equal to c * f(n) for all n greater than or equal to n0

In English...this means...we can say that f(n) is a fundamental function that can upper bound T(n)'s value for all n going on forever.

We have an infinite choice for what c is.

Our constant does not change behavior, it changes "steepness" of the graph.

We are saying that...if I declare f(n) as an upper bound, then I can find a constant c to multiply against f(n) to ALWAYS always always keep T(n) beneath my c * f(n)...T(n) will never beat c * f(n) for infinite n values...hence asymptotic bounding.

If we can't find this c then f(n) fails as an upper bound because it does not satisfy the asymptotic requirement.


So why are constants dropped?

Well...think about what we just did. The injection of the arbitrary c as a multiple onto a base function removes the need for a constant. It adds no meaning to a bound because it is conceptually already a part of the definition of what a bound is.


Big Bounds

Big O: Upper bound on an algorithm's runtime

Theta (Θ): This is a "tight" or "exact" bound. It is a combination of Big 

For example:
An algorithm taking Ω(n log n) takes at least n log n time, but has no upper limit.

An algorithm taking Θ(n log n) is far preferential since it takes at least n log n (Ω(n log n)) and no more than n log n (O(n log n)).

Big Omega (Ω): Lower bound on an algorithm's runtime.

Little Bounds

Little o: Upper bound on an algorithm's runtime but the asymptotic runtime cannot equal the upper bound.

There is no little theta (θ).

Little Omega (ω): Lower bound on an algorithm's runtime but the asymptotic runtime cannot equal the lower bound.

If you can't get an exact upper bound, try lower bounding (although it is less useful to be honest).

---

What Is Asymptotic Analysis? And Why Does It Matter? A Deeper Understanding of Asymptotic Bounding.

First, we must ask what asymptotic means. Well, you have probably heard of the word "asymptote".

An asymptote is a "line that continually approaches a given curve but does not meet it at any finite distance".

Therefore, asymptotic analysis is the analysis of tail behaviors not reaching any finite point. It is a method of describing limiting behavior.


Wall Time vs. Asymptotic Complexity

Well...why not just measure the seconds our code takes to run? And get the Elapsed real time (wall time)?...like Leetcode.

So why do we care about this...well in computer science we often deal with problems that are at a grand scale with inputs to the order of millions and billions.

And thus, the true measure of the efficiency of an algorithm is best expressed in its tail behavior on very large input. It only then shows its true colors.


An Expression of Asymptotic Behaviour

Insertion Sort: 2 * n^2
Merge Sort: 50 * n * log(n)

We have 2 computers:

Computer A: runs 10 Billion instructions / second
Computer B:  runs 10 Million instructions / second

Computer A is 1000x faster than Computer B
Computer A runs insertion sort, Computer B runs merge sort

How long will each computer take to sort 10 million numbers?

Computer A: 5.5 hours
Computer B: 20 minutes

A computer that runs 1000x faster lost horrendously to a computer that runs 1000x slower than it.

But the thing is that insertion sort will be faster for an initial amount, but it will lose as the input gets larger (and that's what we care about and what is a true expression of its efficiency).

---

Deeply Understanding Logarithms In Time Complexities & Their Role In Computer Science

Logarithms and logarithmic time complexities used to confuse me for a very long time since it was one of those scary things in math where you see a symbol and then you get scared that it is something complex.

Logarithms are very simple. At the most fundamental level, the logarithm asks us a question.

log(8) with a base of 2 asks me: "what do I need to power 2 by to get 8? The answer is 3. 2^3 = 8

log(100) with a base of 10 asks me: "what do I need to power 10 by to get 100? The answer is 2. 10^2 = 100

This generalizes us to understand that a log asks us...what do I need to power my base by to get the number we are taking a log against? 

That is what the logarithmic expression evaluates to.

Also, if we have 8 and a log base of 2, we can halve 8 3 times. 8 - 4 - 2 - 1 before we get to a 1 and we cannot cut in half anymore.

In standard mathematics, it is assumed that the base is a base of 10.

In computer science, the base is almost always 2. We will see why.


Where We See Logs In Computer Science:

Levels In A Binary Tree

In general, a binary tree with n nodes will have at least 1 + floor(log_2(n)) levels

When we do something like a tree traversal or heap insertion or removal this is why we use a bound of O(h) which for a balanced binary tree really means O(log(n)).

We will traverse at most a log amoung of levels in the asymptotic sense since that is our tail behavior. Our asymptotic behavior is logarithmic.


Merge Sort & Quicksort

In mergesort, we can only cut the input in half up to log(n) times.
Same for quicksort.

Each sorting algorithm will have log(n) levels of work roughly and then the question becomes what amount of work do we do in each of those levels.


Binary Search

In a binary search, we cut our search space in half every operation based on some predefined searching criteria we define for narrowing search space.

We can only cut our search space in half up to log(n) times.
The logarithm is critical for all of these applications since the question it asks is exactly what we are interested in.

---











































