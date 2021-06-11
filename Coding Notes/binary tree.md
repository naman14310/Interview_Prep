# Binary Tree

## DFS (Recursive) based Questions

#### 1. Merge Two Binary Trees

![img](https://assets.leetcode.com/uploads/2021/02/05/merge.jpg)

```cpp
TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
    if(!root1) return root2;
    if(!root2) return root1;

    root1->val += root2->val;

    root1->left = mergeTrees(root1->left, root2->left);
    root1->right = mergeTrees(root1->right, root2->right);

    return root1;
}
```

#### 2. Increasing Order Search Tree

![img](https://assets.leetcode.com/uploads/2020/11/17/ex1.jpg)

```cpp
void inorder(TreeNode* root, TreeNode* & itr){
    if(root){
        inorder(root->left, itr);
        itr->right = new TreeNode(root->val);
        itr = itr->right;
        inorder(root->right, itr);
    }
}

TreeNode* increasingBST(TreeNode* root) {
    TreeNode* dummy = new TreeNode(0);
    TreeNode* newRoot = dummy;

    inorder(root, dummy);

    return newRoot->right;
}
```

Better Solution : [Without creating new Tree](https://leetcode.com/problems/increasing-order-search-tree/discuss/165885/C%2B%2BJavaPython-Self-Explained-5-line-O(N)) 

#### 3. N-ary Tree Preorder Traversal

```cpp
void traverse(Node* root, vector<int> & ans){
    if(root){
        ans.push_back(root->val);

        for(int i=0; i<root->children.size(); i++)
            traverse(root->children[i], ans);
    }
}

vector<int> preorder(Node* root) {
    vector<int> ans;
    traverse(root, ans);
    return ans;
}
```
