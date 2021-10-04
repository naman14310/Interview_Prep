# Problems based on Catalan Numbers
Catalan numbers are a sequence of natural numbers that occurs in many interesting counting problems.

<br>

**Applications of Catalan Numbers**
1. Count the number of expressions containing n pairs of parentheses which are correctly matched. For n = 3, possible expressions are ((())), ()(()), ()()(), (())(), (()()).
2. Count the number of possible Binary Search Trees with n keys.
3. Count the number of full binary trees (A rooted binary tree is full if every vertex has either two children or no children) with n+1 leaves.
4. Given a number n, return the number of ways you can draw n chords in a circle with 2 x n points such that no 2 chords intersect.
5. Number of ways a convex polygon of n+2 sides can split into triangles by connecting vertices.
6. Number of different Unlabeled Binary Trees can be there with n nodes.

<br>

[Read More..](https://www.geeksforgeeks.org/applications-of-catalan-numbers/)

<br>


![img](https://image.slidesharecdn.com/catalannumbersclaire-151221024005/95/a-tour-of-the-catalan-numbers-3-638.jpg?cb=1450666200)

<br>

### 1. Unique Binary Search Trees
Given an integer n, return the number of structurally unique BST's which has exactly n nodes of unique values from 1 to n.

![img](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

Input: n = 3

Output: 5

```cpp
int catalan (int n, vector<int> & dp){
    if(n==0 or n==1) return 1;

    if(dp[n]!=-1) return dp[n];

    int ans = 0;

    for(int i=0; i<n; i++){
        ans += catalan(i, dp) * catalan(n-i-1, dp);
    }

    return dp[n] = ans;
}


int numTrees(int n) {
    vector<int> dp (n+1, -1);
    return catalan (n, dp);
}
```
