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

