# Binary Tree

## DFS (Recursive) based Questions

#### @ Questions based on changing Tree pointers

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

#### 2. Invert Binary Tree

![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```cpp
TreeNode* invertTree(TreeNode* root) {
    if(!root) return root;
    if(!root->left and !root->right) return root;

    TreeNode* left = invertTree(root->left);
    TreeNode* right = invertTree(root->right);

    root->left = right;
    root->right = left;
    return root;
}
```

#### 3. Increasing Order Search Tree

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


#### @ Questions based on Path traversals

#### 1. N-ary Tree Preorder Traversal

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

#### 1. Sum of Root To Leaf Binary Numbers (Tricky)
You are given the root of a binary tree where each node has a value 0 or 1.  Each root-to-leaf path represents a binary number starting with the most significant bit.  For example, if the path is 0 -> 1 -> 1 -> 0 -> 1, then this could represent 01101 in binary, which is 13.

Hint : Use Bitwise logic to form integer on the go.

```cpp
void solve(TreeNode* root, int & sum, int num){
    if(!root) return;

    if(!root->left and !root->right){
        sum += (num<<1) | root->val;
        return;
    }

    solve(root->left, sum, (num<<1) | root->val);
    solve(root->right, sum, (num<<1) | root->val);
}

int sumRootToLeaf(TreeNode* root) {
    int sum = 0;
    solve(root, sum, 0);
    return sum;
}
```


----


## BFS (Queue) based Questions

#### 1. Average of Levels in Binary Tree

```cpp
vector<double> averageOfLevels(TreeNode* root) {
    vector<double> res;
    if(!root) return res;

    queue<TreeNode*> q;
    q.push(root);

    int currentCount = 1, childCount = 0;
    double levelSum = 0, levelCount = 1;

    while(!q.empty()){

        TreeNode* node = q.front(); q.pop();
        currentCount--;

        levelSum += node->val;

        if(node->left){
            q.push(node->left);
            childCount++;
        }

        if(node->right){
            q.push(node->right);
            childCount++;
        }

        if(currentCount == 0){
            res.push_back(levelSum/levelCount);
            currentCount = childCount;
            levelCount = childCount;
            levelSum = 0;
            childCount = 0;
        }
    }
    return res;
}
```
