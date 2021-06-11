# Binary Search Tree (BST)

## DFS (Recursive) based Questions

#### 1. Range Sum of BST
Given the root node of a binary search tree and two integers low and high, return the sum of values of all nodes with a value in the inclusive range [low, high].

```cpp
int rangeSumBST(TreeNode* root, int L, int R) {
    if(!root) return 0;

    if(root->val < L)
        return rangeSumBST(root->right, L, R);

    else if(root->val > R)
        return rangeSumBST(root->left, L, R);

    else
        return root->val + rangeSumBST(root->left, L, R) + rangeSumBST(root->right, L, R);
}
```
