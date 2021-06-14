# Binary Tree

## DFS (Recursive) based Questions

### @ Questions based on changing Tree pointers

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

#### 3. Flatten Binary Tree to Linked List
The "linked list" should be in the same order as a pre-order traversal of the binary tree.

![img](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

Hint: Approach same as flatten inorder (or Increasing Order Search Tree)

```cpp
pair<TreeNode*, TreeNode*> flatten_preorder(TreeNode* & root){

    /* Case 1 : No left and right child */

    if(!root->left and !root->right) return {root, root};

    /* Case 2 : No right child */

    else if(!root->right){
        auto left = flatten_preorder(root->left);
        root->left = NULL;
        root->right = left.first;
        return {root, left.second};
    }

    /* Case 3 : No left child */

    else if(!root->left){
        auto right = flatten_preorder(root->right);
        root->right = right.first;
        return {root, right.second};
    }

    /* Case 4 : Have both left and right child */

    else{
        auto left = flatten_preorder(root->left);
        auto right = flatten_preorder(root->right);

        root->left = NULL;
        root->right = left.first;
        left.second->right = right.first;

        return {root, right.second};
    }
}

void flatten(TreeNode* root) {
    if(!root) return;
    flatten_preorder(root);
}
```

#### 4. Add One Row to Tree
Given the root of a binary tree and two integers val and depth, add a row of nodes with value val at the given depth depth. Note that the root node is at depth 1.

![img](https://assets.leetcode.com/uploads/2021/03/11/add2-tree.jpg)

```cpp
void addRow(TreeNode* root, int v, int d, int level){
    if(!root) return;

    if(level==d-1){
        TreeNode* newLeftNode = new TreeNode(v);
        newLeftNode->left = root->left;
        root->left = newLeftNode;

        TreeNode* newRightNode = new TreeNode(v);
        newRightNode->right = root->right;
        root->right = newRightNode;
        return;
    }

    addRow(root->left, v, d, level+1);
    addRow(root->right, v, d, level+1);
}

TreeNode* addOneRow(TreeNode* root, int v, int d) {
    if(d==1){
        TreeNode* newRoot = new TreeNode(v);
        newRoot->left = root;
        return newRoot;
    }

    addRow(root, v, d, 1);
    return root;
}
```

#### 5. Populating Next Right Pointers in Each Node (Tricky)

![img](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

Hint: Observe that root->left nodes can be easily linked with root->right nodes. Trick here is to connect root->right nodes with its next node at same level. Look closely, every root->right->next is pointing to root->next->left.

```cpp
Node* connect(Node* root) {
    if(!root) return NULL;

    if(root->left)
        root->left->next = root->right;

    if(root->right and root->next)
        root->right->next = root->next->left;

    connect(root->left);
    connect(root->right);

    return root;
}
```

### @ Questions based on Path traversals

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

#### 3. Sum Root to Leaf Numbers
You are given the root of a binary tree containing digits from 0 to 9 only. Return the total sum of all root-to-leaf numbers.

```cpp
void traverse(TreeNode* root, string num, int & sum){
    if(!root) return;

    num.push_back('0' + root->val);

    if(!root->left and !root->right){
        sum += stoi(num);
        return;
    }

    traverse(root->left, num, sum);
    traverse(root->right, num, sum);
}

int sumNumbers(TreeNode* root) {
    int sum = 0;
    traverse(root, "", sum);
    return sum;
}
```

#### 4. Sum of Root To Leaf Binary Numbers (Tricky)
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

#### 5. Path Sum
Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.

```cpp
bool hasPathSum(TreeNode* root, int targetSum) {
    if(!root) return false;

    if(!root->left and !root->right){
        if(targetSum - root->val == 0)
            return true;
        else
            return false;
    }

    return hasPathSum(root->left, targetSum-root->val) or hasPathSum(root->right, targetSum-root->val);
}
```

#### 6. Same Tree
Given the roots of two binary trees p and q, write a function to check if they are the same or not. Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

```cpp
bool isSameTree(TreeNode* p, TreeNode* q) {
    if(!p and !q) return true;
    if(!p or !q) return false;

    return (p->val==q->val and isSameTree(p->left, q->left) and isSameTree(p->right, q->right));
}
```

#### 7. Symmetric Tree
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

#### 8. Subtree of Another Tree
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


#### 9. Lowest Common Ancestor of a Binary Tree

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(!root) return NULL;

    if(root == p or root == q) return root;

    auto left = lowestCommonAncestor(root->left, p, q);
    auto right = lowestCommonAncestor(root->right, p, q);

    if(!left and !right) return NULL;

    if(!left or !right) return left ? left : right;

    return root;
}
```

#### 10. Sum of Left Leaves
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

#### 11. Is Siblings
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

#### 12. Cousins in Binary Tree
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

#### 13. Diameter of Binary Tree

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

#### 14. Minimum Depth of Binary Tree
Given a binary tree, find its minimum depth. The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

```cpp
int minDepth(TreeNode* root) {
    if(!root) return 0;

    int left = minDepth(root->left);
    int right = minDepth(root->right);

    /* Boundary Condition */

    if(min(left, right) == 0)
        return max(left, right) + 1;
    else
        return min(left, right) + 1;
}
```

#### 15. Distribute Coins in Binary Tree
You are given the root of a binary tree with n nodes where each node in the tree has node.val coins. There are n coins in total throughout the whole tree. In one move, we may choose two adjacent nodes and move one coin from one node to another. A move may be from parent to child, or from child to parent. Return the minimum number of moves required to make every node have exactly one coin.

![img](https://assets.leetcode.com/uploads/2019/01/18/tree4.png)

Hint: Use postorder traversal, compute total coins at each node by left + right + root->val - 1. Add this abs(total_coins) to moves and return total_coins without abs. +ve total coins means that node has extra coins, negative total coins means, that node or its subtrees require more coins.

```cpp
int traverse(TreeNode* root, int & moves){
    if(!root) return 0;

    int left = traverse(root->left, moves);
    int right = traverse(root->right, moves);

    int extra_coins = left + right + root->val - 1;
    moves += abs(extra_coins);

    return extra_coins;
}

int distributeCoins(TreeNode* root) {
    int moves = 0;
    traverse(root, moves);
    return moves;
}
```

#### 16. House Robber III (Similar as Max Subset Sum in Binary tree)
All houses in this place form a binary tree. It will automatically contact the police if two directly-linked houses were broken into on the same night. Given the root of the binary tree, return the maximum amount of money the thief can rob without alerting the police.

![img](https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg)

Approach:

1. Create a funtion which will return pair<included, not_included> 
2. Recurse on left subtree and right subtres and store their results in variable left, right
3. Now included_cost = left.second + right.second + root->val
4. and Not_included_cost = max(left.first, left.second) + max(right.first, right.second)
5. return this computed pair

```cpp
/* pair<included, not_included> */

pair<int,int> solve(TreeNode* root){
    if(!root) return {0,0};

    if(!root->left and !root->right) return {root->val, 0};

    auto left = solve(root->left);
    auto right = solve(root->right);

    int include = root->val + left.second + right.second;
    int not_include = max(left.first, left.second) + max(right.first, right.second);

    return {include, not_include};
}

int rob(TreeNode* root) {
    auto res = solve(root);
    return max(res.first, res.second);
}
```

#### 17. Lowest Common Ancestor of Deepest Leaves (Tricky)

Similar Question : Smallest Subtree with all the Deepest Nodes

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)

Approach (for one pass solution) :

1. Create a function which return maxdepth of tree
2. Recurse for left and right subtree
3. Maintain a referenced variable deepest which store the deepest node so far
4. If left == right and left>=deepest then that node may be the lca

```cpp
int maxdepth(TreeNode* root, int level, int & deepest, TreeNode* & lca){
    if(!root) return level;

    int left = maxdepth(root->left, level+1, deepest, lca);
    int right = maxdepth(root->right, level+1, deepest, lca);

    if(left==right and left>=deepest){
        deepest = left;
        lca = root;
    }

    return max(left, right);
}

TreeNode* lcaDeepestLeaves(TreeNode* root) {
    int deepest = -1;
    TreeNode* lca = NULL;

    maxdepth(root, 0, deepest, lca);
    return lca;
}
```

### @ Questions based on Tree Construction (Tricky)

#### 1. Maximum Binary Tree
You are given an integer array nums with no duplicates. A maximum binary tree can be built recursively from nums using the following algorithm:

1. Create a root node whose value is the maximum value in nums.
2. Recursively build the left subtree on the subarray prefix to the left of the maximum value.
3. Recursively build the right subtree on the subarray suffix to the right of the maximum value.

INPUT : nums = [3,2,1,6,0,5]

![img](https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg)

Approach:

1. We scan numbers from left to right, build the tree one node by one step;
2. We use a stack to keep some (not all) tree nodes and ensure a decreasing order;
3. For each number, we keep pop the stack until empty or a bigger number; The bigger number (if exist, it will be still in stack) is current number's root, and the last popped number (if exist) is current number's left child (temporarily, this relationship may change in the future); Then we push current number into the stack.

```cpp
TreeNode* constructMaximumBinaryTree(vector<int>& nums) {

    stack<TreeNode*> stk;

    for(int i=0; i<nums.size(); i++){

        TreeNode* node = new TreeNode(nums[i]);

        while(!stk.empty() and stk.top()->val < nums[i]){
            node->left = stk.top();
            stk.pop();
        }

        if(!stk.empty())
            stk.top()->right = node;

        stk.push(node);
    }

    while(stk.size()>1) stk.pop();

    return stk.top();
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

#### 2. Binary Tree Zigzag Level Order Traversal

```cpp
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if(!root) return res;

    queue<TreeNode*> q;
    q.push(root);

    vector<int> row;
    int currentCount = 1, childCount = 0;
    bool rev = false;

    while(!q.empty()){
        TreeNode* node = q.front(); q.pop();
        row.push_back(node->val);
        currentCount--;

        if(node->left){
            q.push(node->left);
            childCount++;
        }

        if(node->right){
            q.push(node->right);
            childCount++;
        }

        if(currentCount==0){
            if(rev) reverse(row.begin(), row.end());

            res.push_back(row);
            currentCount = childCount;
            childCount = 0;

            row.clear();
            rev = !rev;
        }
    }

    return res;
}
```

#### 3. All Nodes Distance K in Binary Tree
We are given a binary tree (with root node root), a target node, and an integer value k. Return a list of the values of all nodes that have a distance k from the target node. 

Hint : Convert tree into graph by creating parent child mappings. Then use bfs to get all nodes at distance k.

```cpp
/* This function will create parent child mappings */

void create_parent_links(TreeNode* root, unordered_map<int,TreeNode*> & parent){
    if(!root) return;
    if(!root->left and !root->right) return;

    if(root->left) parent[root->left->val] = root;
    if(root->right) parent[root->right->val] = root; 

    create_parent_links(root->left, parent);
    create_parent_links(root->right, parent);
}

vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
    vector<int> res;
    unordered_map<int,TreeNode*> parent;
    unordered_map<int, bool> vis;

    create_parent_links(root, parent);

    queue<TreeNode*> q;
    q.push(target);
    q.push(NULL);      /* Will act as a ending delimeter for levels in bfs */

    vis[target->val] = true;
    int dist = 0;

    while(!q.empty()){
        TreeNode* node = q.front(); q.pop();

        if(node==NULL){
            if(q.empty()) break;
            else{
                q.push(NULL);
                dist++;
            }
            continue;
        }

        if(dist==k) res.push_back(node->val);
        else if(dist>k) break;

        if(node->left and !vis[node->left->val]){
            q.push(node->left);
            vis[node->left->val] = true;
        }

        if(node->right and !vis[node->right->val]){
            q.push(node->right); 
            vis[node->right->val] = true;
        }

        if(parent[node->val] and !vis[parent[node->val]->val]){
            q.push(parent[node->val]);
            vis[parent[node->val]->val] = true;
        }
    }

    return res;
}
```

### @ Questions based on Different views of Binary Tree

#### 1. Binary Tree Right Side View

```cpp
vector<int> rightSideView(TreeNode* root) {
    vector<int> res;
    if(!root) return res;

    queue<TreeNode*> q;
    q.push(root);
    int currentCount = 1, childCount = 0;

    while(!q.empty()){
        TreeNode* node = q.front(); q.pop();
        currentCount--;

        if(node->left){
            q.push(node->left);
            childCount++;
        }
        
        if(node->right){
            q.push(node->right);
            childCount++;
        }

        if(currentCount == 0){
            res.push_back(node->val);
            currentCount = childCount;
            childCount = 0;
        }
    }
    return res;
}
```
