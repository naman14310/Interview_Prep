# Binary Tree (DFS or Recursive based Questions)
## PART 2

<br>

## @ Iterative Tree Traversals

### 1. Preorder using stack

Hint : Use single stack of type TreeNode*

Approach:

1. Create an empty stack and push root initially
2. Iterate till stack becomes empty. Pop one element, print it and push its Right Child and then Left child to stack

```cpp
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    if(!root) return res;

    stack<TreeNode*> stk;
    stk.push(root);

    while(!stk.empty()){

        TreeNode* node = stk.top(); stk.pop();
        res.push_back(node->val);

        if(node->right) stk.push(node->right);     /* First Right then Left */
        if(node->left) stk.push(node->left);
    }

    return res;
}
```

<br>

### 2. Inorder using stack and variable

Hint: Use one stack of type TreeNode* + one variable of type TreeNode*

Approach:
1. Create an empty stack and push root. Also create one variable named curr pointing to root->left
2. Iterate till stack becomes empty and curr becomes NULL (i.e. !stk.empty() or curr)
3. If curr is NULL then pop one element 'node' from stack, print it and make curr = node->right
4. Else push curr to top of the stack and make curr = curr->left

```cpp
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    if(!root) return res;

    stack<TreeNode*> stk;
    stk.push(root);

    TreeNode* curr = root->left;

    while(!stk.empty() or curr){

        if(curr){
            stk.push(curr);
            curr = curr->left;
        }
        else{
            TreeNode* node = stk.top(); stk.pop();
            res.push_back(node->val);
            curr = node->right;
        }
    }

    return res;
}
```

<br>

### 3. Postorder using two stacks

Hint: Use two stacks of type TreeNode*

Approach:
1. Create two stacks and push root to stk1.
2. Iterate till stk1 becomes empty. 
3. Pop one node from stk1, push it to stk2, and also push its Left Child and then Right Child (Opposite from preorder) to stk1.
4. After stk1 becomes empty, second stack will store the postorder in reverse
5. Pop and print elements from stk2 one by one.

```cpp
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    if(!root) return res;

    stack<TreeNode*> stk1, stk2;
    stk1.push(root);

    while(!stk1.empty()){

        TreeNode* node = stk1.top(); stk1.pop();
        stk2.push(node);

        if(node->left) stk1.push(node->left);           /* First Left then Right */
        if(node->right) stk1.push(node->right);
    }

    while(!stk2.empty()){
        res.push_back(stk2.top()->val);
        stk2.pop();
    }

    return res;
}
```

<br>

### 4. Morris Inorder Traversal - O(n) | O(1)

![img](https://2.bp.blogspot.com/-Oi7ZBzR9Wzs/V5YWWkB1FII/AAAAAAAAY8k/hVhzEWlwigM5HpHHN1VIZITzw9By4zAOACLcB/s1600/threadedBT.png)

Approach: In this traversal, we first create links to Inorder successor and print the data using these links, and finally revert the changes to restore original tree. 

1. Create a var curr which initially points to root.
2. If left child exists --> find predecessor in left subtree

    If predecessor->right == NULL, make predecessor->right = curr and move curr to left (we are creating link here)
        
    Else make predecessor->right = NULL --> print the curr->val and move curr to right (we are removing link here)
3. Else print curr->val and move curr to right.

```cpp
/* Morris inorder traversal will takes O(1) space */

TreeNode* findPredecessor_in_leftSubtree(TreeNode* node){
    TreeNode* temp = node->left;
    while(temp->right and temp->right!=node)             // --- > pick the rightmost node not equal to NULL or curr_node
        temp = temp->right;

    return temp;
}

vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    TreeNode* curr = root;

    while(curr){

        if(curr->left){
            TreeNode* predecessor = findPredecessor_in_leftSubtree(curr);

            if(!predecessor->right){
                predecessor->right = curr;
                curr = curr->left;
            }
            else{
                predecessor->right = NULL;
                res.push_back(curr->val);
                curr = curr->right;
            }

        }
        else{
            res.push_back(curr->val);
            curr = curr->right;
        }
    }

    return res;
}
```

<br>



## @ Questions based on Path Traversals

### 1. Path to Given Node
Find the path from Root to a given node B.

```cpp
void findPath (TreeNode* root, int B, vector<int> &path, bool &found){
    if(!root) return;

    path.push_back(root->val);
    if(root->val==B){
        found = true;
        return;
    } 

    findPath (root->left, B, path, found);
    if(!found) findPath (root->right, B, path, found);

    if(!found) path.pop_back();
}

vector<int> Solution::solve(TreeNode* A, int B) {
    vector<int> path;
    bool found = false;
    
    findPath(A, B, path, found);
    return path;
}
```

<br>

### 2. Binary Tree Paths
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

<br>

### 3. Sum Root to Leaf Numbers
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

<br>

### 4. Sum of Root To Leaf Binary Numbers (Tricky)
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

<br>

### 5. Path Sum
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

<br>

### 6. Path Sum II
Given the root of a binary tree and an integer targetSum, return all root-to-leaf paths where each path's sum equals targetSum.

```cpp
void solve(TreeNode* root, int targetSum, vector<vector<int>> & res, vector<int> path){
    if(!root) return;

    targetSum -= root->val;
    path.push_back(root->val);

    if(!root->left and !root->right){
        if(targetSum==0)
            res.push_back(path);
        return;
    }

    solve(root->left, targetSum, res, path);
    solve(root->right, targetSum, res, path);
}

vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
    vector<vector<int>> res;
    vector<int> path;

    solve(root, targetSum, res, path);
    return res;
}
```

<br>

### 7. Path Sum III (Tricky)
Given the root of a binary tree and an integer targetSum, return the number of paths where the sum of the values along the path equals targetSum. The path does not need to start or end at the root or a leaf, but it must go downwards.

![img](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)

Hint: Similar to Subarray sum equals k

```cpp
/* Time Complexity -> O(n) */
/* Optimization : Pass unordered_map by reference to avoid copying on every recursion call */

void solve(TreeNode* root, int targetSum, int curr_sum, unordered_map<int,int> & mp, int & count){
    if(!root) return;

    curr_sum += root->val;

    if(mp.find(curr_sum - targetSum) != mp.end())
        count += mp[curr_sum - targetSum];

    mp[curr_sum]++;

    solve(root->left, targetSum, curr_sum, mp, count);
    solve(root->right, targetSum, curr_sum, mp, count);

    mp[curr_sum]--;
}

int pathSum(TreeNode* root, int targetSum) {
    if(!root) return 0;

    int count = 0;
    unordered_map<int,int> mp;
    mp[0] = 1;

    solve(root, targetSum, 0, mp, count);
    return count;
}
```

<br>

### 8. Sum of Nodes on the Longest path from root to leaf node 
Find the sum of all nodes on the longest path from root to leaf node. If two or more paths compete for the longest path, then the path having maximum sum of nodes is being considered.

```cpp
void solve(Node* root, int curr_depth, int & prev_depth, int pathsum, int & maxsum){
    if(!root) return ;

    pathsum += root->data;

    if(!root->left and !root->right){
        if(curr_depth>prev_depth){
            prev_depth = curr_depth;
            maxsum = pathsum;
        } 
        else if(curr_depth == prev_depth) maxsum = max(maxsum, pathsum);
        return;
    }

    solve(root->left, curr_depth+1, prev_depth, pathsum, maxsum);
    solve(root->right, curr_depth+1, prev_depth, pathsum, maxsum);
}

int sumOfLongRootToLeafPath(Node *root){
    int prev_depth = -1;
    int maxsum = 0;

    solve(root, 0, prev_depth, 0, maxsum);
    return maxsum;
}
```

<br>

### 9. Count Good Nodes in Binary Tree
Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

![img](https://assets.leetcode.com/uploads/2020/04/02/test_sample_1.png)

```cpp
void traverse(TreeNode* root, int max_so_far, int & count){
    if(!root) return;

    if(root->val >= max_so_far){
        count++;
        max_so_far = root->val;
    } 

    traverse(root->left, max_so_far, count);        
    traverse(root->right, max_so_far, count);        
}

int goodNodes(TreeNode* root) {
    int count = 0;
    traverse(root, INT_MIN, count);
    return count;
}
```

<br>

### 10. Longest ZigZag Path in a Binary Tree

![img](https://assets.leetcode.com/uploads/2020/01/22/sample_1_1702.png)

```cpp
/*
    dir = true -->left
    dir = false --> right
*/

void traverse(TreeNode* root, bool dir, int zig_zag_len, int & res){
    if(!root) return;

    if(dir){
        if(root->left)
            traverse(root->left, true, 1, res);

        if(root->right){
            res = max(res, zig_zag_len+1);
            traverse(root->right, false, zig_zag_len+1, res); 
        }
    }
    else{
        if(root->right)
            traverse(root->right, false, 1, res);

        if(root->left){
            res = max(res, zig_zag_len+1);
            traverse(root->left, true, zig_zag_len+1, res); 
        }
    }
}

int longestZigZag(TreeNode* root) {
    int res = 0;
    traverse(root, true, 0, res);
    return res;
}
```

<br>

### 11. Maximum Difference Between Node and Ancestor
Given the root of a binary tree, find the maximum value v for which there exist different nodes a and b where v = |a.val - b.val| and a is an ancestor of b.

![img](https://assets.leetcode.com/uploads/2020/11/09/tmp-tree.jpg)

Output: 7

Hint: Iterate every path in tree. Use two vectors to store running max and min of curr path and find maxDiff everytime.

```cpp
/* Here maxV and minV stores running max and running min of currPath */

void solve (TreeNode* root, vector<int> &maxV, vector<int> &minV, int &ans){
    if(!root) return;

    int diff1 = abs(root->val - maxV.back());
    int diff2 = abs(root->val - minV.back());

    ans = max({ans, diff1, diff2});

    /* If currVal >= max of currPath --> push_back() it in maxV */

    if(root->val >= maxV.back())
        maxV.push_back(root->val);

    /* If currVal <= min of currPath --> push_back() it in minV */

    if(root->val <= minV.back())
        minV.push_back(root->val);


    solve(root->left, maxV, minV, ans);
    solve(root->right, maxV, minV, ans);

    /* If currVal == max of currPath --> pop_back() from maxV */

    if(root->val == maxV.back())
        maxV.pop_back();

    /* If currVal == min of currPath --> pop_back() from minV */

    if(root->val == minV.back())
        minV.pop_back();
}


int maxAncestorDiff(TreeNode* root) {
    vector<int> maxV, minV;
    int ans = 0;

    maxV.push_back(root->val);
    minV.push_back(root->val);

    solve(root, maxV, minV, ans);
    return ans;
}
```

<br>



## @ BT to Linked List Conversion

### 1. Flatten Binary Tree to Linked List
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

<br>

### 2. Binary Tree to Doubly Linked List
Given a Binary Tree (BT), convert it to a Doubly Linked List(DLL) In-Place. The left and right pointers in nodes are to be used as previous and next pointers respectively in converted DLL. The order of nodes in DLL must be same as Inorder of the given Binary Tree. The first node of Inorder traversal (leftmost node in BT) must be the head node of the DLL.

![img](https://www.geeksforgeeks.org/wp-content/uploads/TreeToList.png)

```cpp
/* pair <head, tail> */

pair<Node*, Node*> convert(Node* root){
    if(!root) return {NULL, NULL};
    if(!root->left and !root->right) return {root, root};

    if(!root->left){
        auto right = convert(root->right);
        root->right = right.first;
        right.first->left = root;
        return {root, right.second};
    }

    if(!root->right){
        auto left = convert(root->left);
        root->left = left.second;
        left.second->right = root;
        return {left.first, root};
    }

    else{
        auto left = convert(root->left);
        auto right = convert(root->right);

        root->left = left.second;
        left.second->right = root;

        root->right = right.first;
        right.first->left = root;

        return {left.first, right.second};
    }
}

Node * bToDLL(Node *root){
    auto p = convert(root);
    return p.first;
}
```

<br>



## @ Questions based on Tree Construction 

### 1. Maximum Binary Tree (Tricky - O(n))
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

<br>

### 2. Construct Binary Tree from Preorder and Inorder Traversal

```cpp
int find_root_index(vector<int> & v, int start, int end, int root_value){
    for(int i=start; i<=end; i++){
        if(v[i]==root_value)
            return i;
    }
    return -1;
}

TreeNode* build(vector<int> & preorder, vector<int> & inorder, int pstart, int pend, int istart, int iend){
    if(pstart>pend) return NULL;

    int root_value = preorder[pstart];
    TreeNode* root = new TreeNode(root_value);

    int pivot = find_root_index(inorder, istart, iend, root_value);
    int left_subtree_size = pivot - istart;

    root->left = build(preorder, inorder, pstart+1, pstart+left_subtree_size, istart, pivot-1);
    root->right = build(preorder, inorder, pstart+left_subtree_size+1, pend, pivot+1, iend);

    return root;
}

TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    int n = preorder.size();
    return build(preorder, inorder, 0, n-1, 0, n-1);
}
```

<br>

### 3. Construct Binary Tree from Inorder and Postorder Traversal

```cpp
int find_root_index(vector<int> & v, int start, int end, int val){
    for(int i=start; i<=end; i++){
        if(v[i]==val) return i;
    }
    return -1;
}

TreeNode* build(vector<int>& inorder, vector<int>& postorder, int istart, int iend, int pstart, int pend){
    if(pstart>pend) return NULL;

    int root_val = postorder[pend];
    TreeNode* root = new TreeNode(root_val);

    int pivot = find_root_index(inorder, istart, iend, root->val);
    int right_subtree_size = iend - pivot;

    root->left = build(inorder, postorder, istart, pivot-1, pstart, pend-right_subtree_size-1);
    root->right = build(inorder, postorder, pivot+1, iend, pend-right_subtree_size, pend-1);

    return root;
}

TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
    int n = inorder.size();
    return build(inorder, postorder, 0, n-1, 0, n-1);
}
```

<br>



## @ Playing with Tree pointers

### 1. Populating Next Right Pointers in Each Node (Perfect Binary Tree)

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

<br>

### 2. Populating Next Right Pointers in Each Node II (Binary Tree)

![img](https://assets.leetcode.com/uploads/2019/02/15/117_sample.png)

```cpp
/* Move temp to temp->next till we get first Not NULL child or temp==NULL */

Node* getnext (Node* node){
    Node* temp = node;

    while(temp){
        if(temp->left) return temp->left;
        if(temp->right) return temp->right;
        temp = temp->next;
    }

    return NULL;
}


Node* connect(Node* root) {
    if(!root) return root;

    if(root->left){
        if(root->right)
            root->left->next = root->right;
        else
            root->left->next = getnext (root->next);
    }

    if(root->right)
        root->right->next = getnext (root->next);

    connect(root->right);
    connect(root->left);

    return root;
}
```

<br>

### 3. Add One Row to Tree
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

<br>



## @ DP on Trees

### 1. Diameter of Binary Tree (Longest path from leaf to leaf)

Hint: Use DP on tree concept

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

<br>

### 2. Distribute Coins in Binary Tree
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

<br>

### 3. House Robber III (Similar as Max Subset Sum in Binary tree)
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

<br>

### 4. Binary Tree Cameras
You are given the root of a binary tree. We install cameras on the tree nodes where each camera at a node can monitor its parent, itself, and its immediate children. Return the minimum number of cameras needed to monitor all nodes of the tree.

![img](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_01.png)

```cpp
/*
    0 -> already covered by neighbour node
    1 -> request parent to get covered
    2 -> installed camera
*/

int traverse(TreeNode* root, int & camera){
    if(!root) return 0;
    if(!root->left and !root->right) return 1;

    int left = traverse(root->left, camera);
    int right = traverse(root->right, camera);

    if(left==1 or right==1){
        camera++;
        return 2;
    }

    else if(left==2 or right==2)
        return 0;

    else return 1;
}

int minCameraCover(TreeNode* root) {
    int camera = 0;
    int status = traverse(root, camera);
    if(status==1) 
        camera++;
    return camera;
}
```

<br>

### 5. Binary Tree Maximum Path Sum (from any node to any node)
Refer DP on trees playlist of Aditya verma

Hint: At every node, check whether it can become the best solution and return max off all to its parent. 

```cpp
int solve(TreeNode* root, int & maxSum){
    if(!root) return 0;

    int left = solve(root->left, maxSum);
    int right = solve(root->right, maxSum);

    int curr_subtree_sum = left+right+root->val;
    int tempSum = max(curr_subtree_sum, max(root->val, max(left,right)+root->val));

    maxSum = max(tempSum, maxSum);

    return max(root->val, max(left, right)+root->val);
}

int maxPathSum(TreeNode* root) {
    int maxSum = INT_MIN;
    solve(root, maxSum);

    return maxSum;
}
```

<br>

### 6. Longest Univalue Path (Tricky)
Given the root of a binary tree, return the length of the longest path, where each node in the path has the same value. This path may or may not pass through the root. The length of the path between two nodes is represented by the number of edges between them.

![img](https://assets.leetcode.com/uploads/2020/10/13/ex2.jpg)

Hint: Apply DP on trees. At every node, compute the longest path and return the max possible path (from left or right subtree) for its parent.

```cpp
/* Return type : pair< recent node_val, longest path for that node_val> */

pair<int, int> traverse(TreeNode* root, int & max_path_len){
    if(!root) return {INT_MIN, 0};    

    auto left = traverse(root->left, max_path_len);
    auto right = traverse(root->right, max_path_len);

    if(root->val == left.first and root->val == right.first){
        max_path_len = max(max_path_len, left.second + right.second + 1);
        int path_len = max(left.second, right.second) + 1;
        return {root->val, path_len};
    }

    else if(root->val == left.first){
        max_path_len = max(max_path_len, left.second + 1);
        int path_len = left.second + 1;
        return {root->val, path_len};
    }

    else if(root->val == right.first){
        max_path_len = max(max_path_len, right.second + 1);
        int path_len = right.second + 1;
        return {root->val, path_len};
    }

    else return {root->val, 1};
}

int longestUnivaluePath(TreeNode* root) {
    if(!root) return 0;

    int max_path_len = 1;
    traverse(root, max_path_len);
    return max_path_len-1;
}
```
