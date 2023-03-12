# Trees 

## Introduction 

A tree is a data structure that stores values in a hierarchical fashion 

We often use **linked lists** to build trees

### Basic Tree Facts 

- Trees are made of **nodes** 

- Every tree has a root pointer 

```
struct node 
{
    int value; 
    node *left, *right; 
}; 
node *rootPtr; 
```
- The top node of a tree is the root node 

- Every node may have 0 or more children nodes 

- A node with 0 children is a leaf node 

- A tree with no nodes is called an empty tree 

A tree node can have ore than just 2 children 
```
struct node 
{
    int value; 
    node *pChild1, *pChild2, *pChild3, ...; 
}

struct node 
{
    int value; 
    node *pChildren[26]; 
}
```
---
## Binary Trees 

- Every node has at most two children nodes: **left and right** 

```
struct BTNODE
{
    string value; 
    BTNODE *pLeft, *pRight; 
}
```

### Operations on Binary Trees 

Common Operations 

- Enumerating all the tiems 

- Searching for an item 

- Adding a new item at a certain position on the tree 

- Deleting an item 

- Deleting the entire tree 

- Remove a whole section of a tree (pruning)

- Adding a whole section to a tree (grafting)

Example 

```
struct BTNODE 
{
    int value; 
    BTNODE *left, *right; 
}

main()
{
    BTNODE *temp, *pRoot; 
    pRoot = new BTNODE; 
    pRoot->value = 5; 

    temp = new BTNODE; 
    temp->value = 7; 
    temp->left = NULL; 
    temp->right = NULL; 
    pRoot->left = temp; 

    temp = new BTNODE; 
    temp->value = -3; 
    temp->left = NULL; 
    temp->right = NULL; 
    pRight->right = temp; 
}
```

---

## Binary Tree Traversals 

Four common ways 

- Pre-order 

- In-order

- Post-order (^Recursion)

- Level-order (BFS)

### Pre-order 

```
void preorder(Node *p)
{
    if (p == nullptr)
        return; 
    cout << p->value; 
    preorder(p->left); 
    preorder(p->right); 
    
}
```
- handle base case: empty tree

- order

    - process current node's value 

    - fully process left subtree of current node 

    - fully process right subtree of current node

### Inorder 

```
void inorder(Node *p)
{
    if (p == nullptr)
        return; 
    inorder(p->left); 
    cout << p->value; 
    inorder(p->right); 
}
```
- process left first, root, then right 

### Postorder 

```
void postorder(Node *p)
{
    if (p == nullptr)
        return; 
    postorder(p->left); 
    postorder(p->right); 
    cout << p->value; 
}
```

### Level-order Traversal 

We start at the root and visit each level's nodes, from left to right, before visiting nodes in the next level 

Algorithm 

1. Use a temp pointer variabe and queue of node pointers 

2. Insert the root node pointer into the queue 

3. While the queue is not empty 

    - Dequeue the top node pointer and put it in temp 

    - Process the node 

    - Add the node's children to queue if they are not nullptr 

--- 

## Big-O of Traversals 

Each of our traversals performs three operations per node: 

- Process the value in the current node 

- Initiate processing of its left subtree

- Initiate processing of its right subtree 

For a tree with n nodes, that's 3*n operations: **O(n)** 

---
## Traversal Use Case: Expression Evaluation 

```
(5+6)*(3-1)
```

Algorithm 

1. If the current node is a number, return its value 

2. Recursively evaluate the left subtree and get the result 

3. Recursivley evaluate the right subtree and get the result 

4. Apply the operator in the current node to the left and right results; return result 

- A post order traversal 

```
int eval(Node *p)
{
    if (p->type == VALUE)
        return p->value; 
    int left = eval(p->left); 
    int right = eval(p->right); 
    return apply(p->operator, left, right); 
}
```

---

## Binary Search Trees 

A binary search tree enables fast (log2N) searches by ordering its data in a special way 

> For every node j in the tree, all children to j's left must be less than it, and all children to j's right must be greater than it 

To see if a value V is in the tree: 

1. Start at the root node 

2. Compare V against the node, moving down left or right if V is less or greater 

3. Repeat until you find V or hit a dead end 

---

## Operations on a Binary Search Tree 

### Searching a BST 

Input: A value V to search for 

Output: TRUE if found, FALSE otherwise 

Algorithm 

- Start at the root of the tree 

- Keep going until we hit the NULL pointer 

    - If V is equal to current node's value, then found 

    - If V is less than current node's value, go left 

    - If V is greater than current node's value, go right 

Two different search algorithms: iterative and recursive 

```
bool Search(int v, Node *root)
{
    Node *ptr = root; 
    while (ptr != nullptr)
    {
        if (v == ptr->value)
            return true; 
        else if (v < ptr->value)
            ptr = ptr->left; 
        else
            ptr = ptr->right;
    }
    return false; //nope 
}
```
```
bool Search(int v, Node *root)
{
    if (root == nullptr)
        return false; 
    else if (v == root->value)
        return true; 
    else if (v < root->value)
        return Search(V, root->left); 
    else 
        return Search*V, root->right); 
}
```

**Big-O of BST Search**

In the average BST with N values, how many steps are required to find our value? 

- log2N steps 

In the worst case BST with N values, how mnay steps are required to find our value? 

- N steps 

### Inserting a New Value Into A BST 

To insert a new node in our BST, we must place the new node so that the resulting tree is still a valid BST 

Input: A value V to insert 

Algorithm: 

- If the tree is empty 

    - Allocate a new node and put V into it 

    - Point the root pointer to our new node

- Start at the roto of the tree 

- While we are not done 

    - If V is equal to current node's value, DONE

    - If V is less than current nodes value 

        - If there is a left child, then go left 

        - Else allocate a new node and put V into it, and set the current node's left pointer to a new node, DONE 

    - If V is greater than current node's value 

        - If there is a right child, then go right 

        - Else allocate a new ndoe and put V into it, set current node's right pointer to a new node, DONE 

Code 
```
struct Node 
{
    Node(const std::string &myVal)
    {
        value = myVal; 
        left = right = nullptr; 
    }
    std::string value; 
    Node *left, *right; 
}
```
```
class BST
{
    public: 
        BST()
        {
            m_root = nullptr; 
        }

        void insert(const std::string &value)
        {
            ...
        }
    private:
        Node *m_root; 
}; 
```
- our BST class has a single member variable, the root pointer to the tree 

- our constructos initializes that root pointer to nullptr when we create a new tree (indicate tree is empty) 

```
void insert(const std::string &value)
{
    if (m_root == nullptr)
        { m_root = new Node(value); return; }
    Node *cur = m_root; 
    for (;;)
    {
        if (value == cur->value) return; 
        if (value < cur->value)
        {
            if (cur->left != nullptr)
                cur = cur->left; 
            else 
            {
                cur->left = new Node(value); 
                return; 
            }
        }
        else if (value > cur->value)
        {
            if (cur->right != nullptr)
                cur = cur->right; 
            else 
            {
                cur->right = new Node(value); 
                return; 
            }
        }
    }
}
```
- If our tree is empty, allocate a new node and point the root pointer to it 

- Start traversing down from the root of the tree 

    - `for(;;)` is the same as an infinite loop 

- If our value is already in the tree, then we're done, return 

- If the value to insert is less than the current node's value, go left 

- If there is a node to our left, advance to that node and continue

    - Otherwise, we've found the proper spot for our new value 

    - Add our value as the left child of the current node 

- If the value we want to isnert is greater than the current node's value, then traverse/insert to the right 

> As with BST Search, there is a **recursive version** of the Insertion algorithm too! 

**Big-O of BST Insertion** 

O(log2n)

- We have to first use a binary serach to find where to insert out node and binary search is O(log2n)

- Once we found the right spot, we can insert our new node in O(1) time 

### Finding Min & Max of a BST 

How do we find the minimum and maximum values in a BST? 

> The minimum value is located at the left-most node 

> The maximum value is located at the right-most node

```
int GetMin(node *pRoot)
{
    if (pRoot == nullptr)
        return -1; //empty 

    while (pRoot->left != nullptr)
        pRoot = pRoot->left; 

    return pRoot->value; 
}
```
```
int GetMax(Node *pRoot)
{
    if (pRoot = nullptr)
        return -1; 

    while (pRoot->right != nullptr)
        pRoot = pRoot->right; 
    
    return pRoot->value; 
}
```

**Big-O** 

O(log2(n))

- We just go to the bottom of the tree 

Recursion 

```
int GetMin(node *pRoot)
{
    if (pRoot == nullptr)
        return -1; //empty 

    if (pRoot->left == nullptr)
        return pRoot->value; 

    return GetMin(pRoot->left); 
}
```
```
int GetMax(Node *pRoot)
{
    if (pRoot = nullptr)
        return -1; 

    if (pRoot->right == nullptr)
        return pRoot->value; 
    
    return GetMax(pRoot->right); 
}
```

### Printing a BST In Alphabetical Order 

**In order traversal** 

```
void InOrder(Node* cur)
{
    if (cur == nullptr)
        return; 

    InOrder(cur->left); 
    cout << cur->value; 
    InOrder(cur->right); 
}
```

**Big-O**: O(n) 

- Since we have to visit and print all n items 

### Freeing The Whole Tree 

It's another traversal 
s
```
void FreeTree(Node* cur)
{
    if (cur == nulltpr)
        return; 
    FreeTree(cur->left); //delete nodes in left sub-tree
    FreeTree(cur->right); //delete nodes in right sub-tree
    delete cur; //free current node 
}
```
**Big-O**: O(n)