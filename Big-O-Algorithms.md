# Big-O 

## Introduction 

Big-O is a technique that lets you quickly understand an algo's efficiency 

- Big-O of $n^2$: if you pass in $n$ items to it, it will take roughly $n^2$ steps to process its data

- **NOT EXACT**, just rough estimates 

### Ways to Measure Speed of Algorithm

Time it takes for an algorithm to run 

1. Runtime? 

    - Nah, not that useful 

2. Number of computer instructions it takes to solve a problem of a given size 

    - Also not too much info 

    - An algo might look efficient when applied to a small amount of data (e.g. 1000 numbers) but really slow when applied to lot of data 

> Understand how algo performs under all circumstances 

3. The number of instructions used by an algorithm as a function of the size of the input data 

- Sort N numbers 

    - Algorithm A takes $5n^2$ instructions 

    - B takes $37000n$ instructions 

    - Compare 

        - If sort 1000 numbers 

            - A takes 5M instructions, B takes 37M 

        - If sort 10000 numbers 

            - A takes 500M instructions, B takes 370M 

### Concept 

> Big-O approahc measures an algorithm by the **gross number of steps** that it requires to **process an input of size N** in the *WORST CASE SCENARIO* 

- Ignore coefficients and lower-order terms 

    - Algorithm X requires $5n^2 + 3n + 20$ steps -> Big-O of Algorithm X is $N^2$ 

---

## Compute Big-O

### 1. Determine number of operations an algorithm performs 

- `f(n)` 

- Operations are any of the following: 

    - Access an item (e.g. in the array)

    - Evaluate a mathematical expression 

    - Traverse a single link in a linked list 

Example: Compute `f(n)`: the # of critical operations that this algorithm performs 

```
int arr[n][n]; 

for (int i = 0; i < n; i++)
{
    for (int j = 0; j < n; j++)
    {
        arr[i][j] = 0; 
    }
}
```
1. Algorithm initializes the value of `i` once: `f(n) += 1` 

2. Performs `n` comparisons between `i` and `n`:  `f(n) += n` 

3. Increments the variabl `i`, `n` times: `f(n) += n`

4. Initializes the value of `j`, `n` different times: `f(n) += n` 

5. Performs $n^2$ comparisons between `j` and `n`: `f(n) +=` $n^2$ 

6. Increments the variable `j`, $n^2$ times: `f(n) +=` $n^2$ 

7. Algorithm sets `arr[i][j]`'s value $n^2$ times: `f(n) +=` $n^2$

$f(n) = 1 + n + n + n + n^2 + n^2 + n^2 = 3n^2 + 3n + 1$

### 2. Keep the most significant term of that function and throw away the rest 

$f(n) = 3n^2 + 3n + 1$ BECOMES $f(n) = 3n^2$ 

### 3. Remove any constant multiplier from the function

$f(n) = 3n^2$ BECOMES $f(n) = n^2$

### 4. BIG-O 

$f(n) = 3n^2 + 3n + 1$ is **$O(n^2)$**

### Simplified Approach 

There's no need to compute exact `f(n)` because we end up throwing away all lower order terms and coefficients 

JUST focus on most frequently occuring operators 

```
arr[i][j] = 0
```
$f(n) = n^2$ 

Hence the algorithm is $O(n^2)$