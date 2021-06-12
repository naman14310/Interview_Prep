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

#### 2. Binary Tree Paths
Given the root of a binary tree, return all root-to-leaf paths in any order.

```cpp
void traverse(TreeNode* root, vector<string> & paths, string path){
    if(!root) return;

    if(!root->left and !root->right){
       path += to_string(root->val);
       paths.push_back(path);
       return;
    } 

    path += to_string(root->val) + "->";
    traverse(root->left, paths, path); 
    traverse(root->right, paths, path); 
}

vector<string> binaryTreePaths(TreeNode* root) {
    vector<string> paths;
    traverse(root, paths, "");
    return paths;
}
```

#### 3. Sum of Root To Leaf Binary Numbers (Tricky)
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

#### 4. Same Tree
Given the roots of two binary trees p and q, write a function to check if they are the same or not. Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

```cpp
bool isSameTree(TreeNode* p, TreeNode* q) {
    if(!p and !q) return true;
    if(!p or !q) return false;

    return (p->val==q->val and isSameTree(p->left, q->left) and isSameTree(p->right, q->right));
}
```

#### 5. Symmetric Tree
Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

![img](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

```cpp
bool traverse(TreeNode* root1, TreeNode* root2){
    if(!root1 and !root2) return true;
    if(!root1 or !root2) return false;

    return (root1->val == root2->val) and traverse(root1->left, root2->right) and traverse(root1->right, root2->left);
}

bool isSymmetric(TreeNode* root) {
    if(!root) return true;
    return traverse(root->left, root->right);
}
```

#### 6. Subtree of Another Tree
Given the roots of two binary trees root and subRoot, return true if there is a subtree of root with the same structure and node values of subRoot and false otherwise.

![img](https://assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg)

```cpp
bool isSame(TreeNode* root1, TreeNode* root2){
    if(!root1 and !root2) return true;
    if(!root1 or !root2) return false;

    return root1->val == root2->val and isSame(root1->left, root2->left) and isSame(root1->right, root2->right);
}

bool isSubtree(TreeNode* root, TreeNode* subRoot) {
    if(!root) return false;
    if(isSame(root, subRoot)) return true;

    return isSubtree(root->left, subRoot) or isSubtree(root->right, subRoot);
}
```

Optimization: To optimize the time complexity of our code, instead of checking subtree at every node, we will only check when both roots are at same level.

#### 6. Sum of Left Leaves
Observation : Left leaves are those who are attached on the left side of their parent.

```cpp
void traverse(TreeNode* root, int & sum, bool isLeft){
    if(!root) return;

    if(!root->left and !root->right and isLeft)
        sum += root->val;

    traverse(root->left, sum, true);
    traverse(root->right, sum, false);
}

int sumOfLeftLeaves(TreeNode* root) {
    int sum = 0;
    traverse(root, sum, false);
    return sum;
}
```

#### 7. Is Siblings
Siblings are those nodes which have same parent. Return true if and only if the nodes corresponding to the values x and y are siblings.

```cpp
bool isSiblings(TreeNode* root, int x, int y){
    if(!root) return false;

    if(root->left and root->right){
        if((root->left->val==x and root->right->val==y) or (root->left->val==y and root->right->val==x))
            return true;
    }

    return isSiblings(root->left, x, y) or isSiblings(root->right, x, y);
}
```

#### 8. Cousins in Binary Tree
Two nodes of a binary tree are cousins if they have the same depth, but have different parents (i.e they are not siblings). Return true if and only if the nodes corresponding to the values x and y are cousins.

```cpp
/* Function that will return the level of both nodes in one traversing */

void traverse(TreeNode* root, int x, int y, int & levelx, int & levely, int level){
    if(!root) return;

    if(root->val==x){
        levelx = level;
        if(levely!=-1) return;      // pruning the traversing
    }

    if(root->val==y){
        levely = level;
        if(levelx!=-1) return;      // pruning the traversing
    }

    traverse(root->left, x, y, levelx, levely, level+1);

    if(levelx==-1 or levely==-1)    // Condition to prevent unnecessary traversing
        traverse(root->right, x, y, levelx, levely, level+1);
}

bool isCousins(TreeNode* root, int x, int y) {
    int levelx = -1, levely = -1;
    traverse(root, x, y, levelx, levely, 0);

    return !isSiblings(root, x, y) and levelx==levely;
}
```

#### 9. Diameter of Binary Tree

```cpp
int height(TreeNode* root, int & diameter){
    if(!root) return 0;

    int leftHeight = height(root->left, diameter);
    int rightHeight = height(root->right, diameter);

    diameter = max(diameter, leftHeight + rightHeight);

    return max(leftHeight, rightHeight) + 1;
}

int diameterOfBinaryTree(TreeNode* root) {
    int diameter=0;
    height(root, diameter);
    return diameter;
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
