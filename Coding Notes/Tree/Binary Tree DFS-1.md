# Binary Tree (DFS or Recursive based Questions)
## Part 1

<br>

## @ Standard BT Questions

### 1. Invert a Binary Tree

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

<br>

### 2. Is Siblings
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

<br>

### 3. Cousins in Binary Tree
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

<br>

### 4. Minimum Depth of Binary Tree
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

<br>

### 5. Balanced Binary Tree
Given a binary tree, determine if it is height-balanced.

```cpp
pair<int, bool> solve(TreeNode* root){
    if(!root) return {0, true};

    auto left = solve(root->left);
    auto right = solve(root->right);

    bool is_subtrees_balanced = left.second and right.second;
    bool is_tree_balanced = abs(left.first - right.first)<=1;

    int h = max(left.first, right.first) + 1;

    if(is_subtrees_balanced and is_tree_balanced)
        return {h, true};
    else
        return {h, false};
}

bool isBalanced(TreeNode* root) {
    auto p = solve(root);
    return p.second;
}
```

<br>

### 6. Check Completeness of a Binary Tree (Tricky)

Method 1: DFS (Recursive approach) O(n) | O(h)

For a complete tree, it must satify at least one of the following condition:
1. If left subtree is a full tree with l nodes, right subtree must have r nodes that l / 2 <= r <= l
2. if right subtree is a full tree with r nodes, left subtree must have l nodes that r <= l <= r * 2 + 1.

```cpp
/* 
    x = 2^k -1, x has a property that x & (x+1) == 0
    where x can be 1, 3, 7, 15, 31 

    return type : pair< isCompleteBT, countOfNodes > 
*/

pair<bool, int> traverse(TreeNode* root){
    if(!root) return {true, 0};

    auto left = traverse(root->left);
    auto right = traverse(root->right);

    if(left.first and right.first){
        int lc = left.second, rc = right.second;

        // --> If left subtree is full with lc nodes 
        
        if( (lc&(lc+1))==0 and lc/2<=rc and rc<=lc )
            return {true, lc+rc+1};

        // --> If right subtree is full with rc nodes 

        else if( (rc&(rc+1))==0 and rc<=lc and lc<=2*rc+1 )
            return {true, lc+rc+1};
    }
    
    return {false, -1};
}

bool isCompleteTree(TreeNode* root) {
    auto res = traverse(root);
    return res.first;
}
```

Method 2: BFS (using queue)

For a complete binary tree, there should not be any node after we met an empty one.

```cpp
bool isCompleteTree(TreeNode* root) {
    queue<TreeNode*> q;
    q.push(root);

    bool first_null_inserted = false;

    while(!q.empty()){
        TreeNode* node = q.front(); q.pop();

        /* breaking condition when we found first NULL in queue */
        if(!node){
            if(q.empty()) return true;
            else return false;
        }

        if(!first_null_inserted or node->left){
            q.push(node->left);
            if(!node->left) first_null_inserted = true;
        }

        if(!first_null_inserted or node->right){
            q.push(node->right);
            if(!node->right) first_null_inserted = true;
        }
    }

    return true;
}
```

<br>

### 7. Lowest Common Ancestor of a Binary Tree

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

<br>

### 8. Min distance between two given nodes of a Binary Tree

Hint: First find their lca. Then min distance between nodes n1, n2 will be : dist(n1, n2) = dist(lca, n1) + dist(lca, n2)

```cpp
void calculate_distance(Node* temp, int d, int a, int b, int & dist, int & found){
    if(!temp) return;
    
    if(temp->data == a or temp->data == b){
        dist += d;
        found++;              /* using found variable for pruning search space */
        if(d>0) return;      /* Remember return only when d>0 */
    }
    
    calculate_distance(temp->left, d+1, a, b, dist, found);
    
    if(found<2)
        calculate_distance(temp->right, d+1, a, b, dist, found);
}

int findDist(Node* root, int a, int b) {
    
    Node* lca = findlca(root, a, b);          /* findlca function will return pointer to lca node of a, b */
    
    int dist = 0;
    int found = 0;
    
    calculate_distance(lca, 0, a, b, dist, found);
    
    return dist;
}
```

<br>

### 9. Lowest Common Ancestor of Deepest Leaves (Tricky)

Similar Question : Smallest Subtree with all the Deepest Nodes

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)

Approach (for one pass solution):
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

<br>

### 10. Check if a Binary Tree contains duplicate subtrees of size 2 or more 
Given a binary tree, find out whether it contains a duplicate sub-tree of size two or more, or not.

Hint: Return inorder traversal and store it everytime in a set to check whether it occurs again or not.

```cpp
pair<bool, string> traverse(Node* root, unordered_set<string> & st){
    if(!root) return {false, ""};
    if(!root->left and !root->right) return {false, to_string(root->data)};

    auto left = traverse(root->left, st);
    auto right = traverse(root->right, st);
    
    if(left.first or right.first) return {true, ""};   /* We do not need to check further */
    
    string s = left.second + to_string(root->data) + right.second;
    
    if(st.find(s)!=st.end()) 
        return {true, s};
    else{
        st.insert(s);
        return {false,s}; 
    }  
}

bool dupSub(Node *root){
    unordered_set<string> st;
    auto ans = traverse(root, st);
    return ans.first;
}
```

<br>

### 11. Find all Duplicate subtrees in a Binary tree

![img](https://contribute.geeksforgeeks.org/wp-content/uploads/tree1-1.png)

Output : [[2 4], [4]]

```cpp
string traverse(Node* root, unordered_map<string, int> & mp, vector<Node*> & res){
    if(!root) return "";
    
    string left = traverse(root->left, mp, res);
    string right = traverse(root->right, mp, res);
    
    string s = "(" + left + to_string(root->data) + right + ")";
    
    if(mp[s]==1) res.push_back(root);
    
    mp[s]++;
    
    return s;
}

vector<Node*> printAllDups(Node* root){
    vector<Node*> res;
    unordered_map<string, int> mp;    /*we use unordered_map instead of unordered_set because we want to print multiple duplicates only once */

    traverse(root, mp, res);
    return res;
}
```

<br>

### 12. Serialize and Deserialize Binary Tree

```cpp
// Encodes a tree to a single string.

string serialize(TreeNode* root) {
    if(!root) return "X";
    return to_string(root->val) + "," + serialize(root->left) + "," + serialize(root->right);  
}


TreeNode* buildTree(vector<string> v, int & i){
    if(v[i]=="X"){
        i++;
        return NULL;
    } 

    TreeNode* root = new TreeNode(stoi(v[i]));
    i++;

    root->left = buildTree(v, i);
    root->right = buildTree(v, i);

    return root;
}


// Decodes your encoded data to tree.

TreeNode* deserialize(string data) {
    stringstream ss(data);
    vector<string> v;
    string tkn;

    while(getline(ss, tkn, ','))
        v.push_back(tkn);

    int i=0;
    return buildTree(v, i);
}
```

<br>



## @ Comparision between two trees

### 1. Merge Two Binary Trees

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

<br>

### 2. Same Tree
Given the roots of two binary trees p and q, write a function to check if they are the same or not. Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

```cpp
bool isSameTree(TreeNode* p, TreeNode* q) {
    if(!p and !q) return true;
    if(!p or !q) return false;

    return (p->val==q->val and isSameTree(p->left, q->left) and isSameTree(p->right, q->right));
}
```

<br>

### 3. Symmetric Tree
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

<br>

### 4. Isomorphic Tree
Two trees are called isomorphic if one can be obtained from another by a series of flips, i.e. by swapping left and right children of several nodes. Any number of nodes at any level can have their children swapped. Two empty trees are isomorphic.

![img](https://media.geeksforgeeks.org/wp-content/cdn-uploads/ISomorphicTrees-e1368593305854.png)

```cpp
bool traverse(Node* root1, Node* root2){
    if(!root1 and !root2) return true;
    if(!root1 or !root2) return false;

    if(root1->data != root2->data) return false;

    /* Check for same orientation */

    bool left1 = traverse(root1->left, root2->left);
    bool right1 = traverse(root1->right, root2->right);

    /* Check for opposite orientation after flipping */

    bool left2 = traverse(root1->left, root2->right);
    bool right2 = traverse(root1->right, root2->left);

    return (left1 and right1) or (left2 and right2);
}

bool isIsomorphic(Node *root1, Node *root2){
    return traverse(root1, root2);
}
```

<br>

### 5. Subtree of Another Tree
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

<br>



## @ Questions based on unique Tree Traversals

### 1. Diagonal Traversal of Binary Tree 

![img](https://www.geeksforgeeks.org/wp-content/uploads/unnamed1.png)

Output : 8 10 14 3 6 7 13 1 4

```cpp
void solve(Node* root, int pos, map<int, vector<int>> & mp){
    if(!root) return;
    
    mp[pos].push_back(root->data);
    
    solve(root->left, pos+1, mp);      // --> increasing vertical distance for left
    solve(root->right, pos, mp);       // --> passing same distance for right child
}

vector<int> diagonal(Node *root){
   map<int, vector<int>> mp;
   solve(root, 0, mp);
   
   vector<int> res;
   for(auto p : mp)
       for(int num : p.second)
           res.push_back(num);
   
   return res;
}
```

<br>

### 2. Boundary Traversal of Binary Tree

![img](https://contribute.geeksforgeeks.org/wp-content/uploads/boundary.png)

Output: 20 8 4 10 14 25 22

```cpp
void leaves(Node* root, vector<int> & res){
    if(!root) return;

    if(!root->left and !root->right){
        res.push_back(root->data);
        return;
    }

    leaves(root->left, res);
    leaves(root->right, res);
}

void right_boundary(Node* root, vector<int> & res){
    if(!root) return;
    if(!root->left and !root->right) return;

    if(root->right)
        right_boundary(root->right, res);
    else
        right_boundary(root->left, res);

    res.push_back(root->data);
}

void left_boundary(Node* root, vector<int> & res){
    if(!root) return;
    if(!root->left and !root->right) return;

    res.push_back(root->data);

    if(root->left) 
        left_boundary(root->left, res);
    else
        left_boundary(root->right, res);
}

vector<int> printBoundary(Node *root){
    vector<int> res;
    if(!root) return res;

    res.push_back(root->data);

    left_boundary(root->left, res);
    leaves(root->left, res);
    leaves(root->right, res);
    right_boundary(root->right, res);

    return res;
}
```

<br>



## @ Few IMP Interview Questions

### 1. Binary Search Tree to Greater Sum Tree
Given the root of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.

![img](https://assets.leetcode.com/uploads/2019/05/02/tree.png)

```cpp
void solve (TreeNode* root, int& sum){
    if(!root) return;

    solve(root->right, sum);

    root->val += sum;
    sum = root->val;

    solve(root->left, sum);
}


TreeNode* bstToGst(TreeNode* root) {
    int sum = 0;
    solve (root, sum);
    return root;
}
```

<br>

### 2. Transform to Sum Tree
Given a Binary Tree of size N. Convert this to a tree where each node contains the sum of the left and right sub trees of the original tree. The values of leaf nodes are changed to 0.

```cpp
int solve(Node* root){
    if(!root) return 0;

    int left = solve(root->left);
    int right = solve(root->right);

    int temp = root->data;
    root->data = left + right;
    return temp + root->data;
}

void toSumTree(Node *root){
    solve(root);
}
```

<br>

### 3. Check if Binary tree is Sum tree or not
Given a Binary Tree. Return 1 if, for every node X in the tree other than the leaves, its value is equal to the sum of its left subtree's value and its right subtree's value. Else return 0. An empty tree is also a Sum Tree. A leaf node is also considered a Sum Tree.

```cpp
pair<bool, int> solve(Node* root){
    if(!root) return {true, 0};
    if(!root->left and !root->right) return {true, root->data};

    auto left = solve(root->left);
    auto right = solve(root->right);

    bool is_sum_tree = left.first and right.first and root->data==left.second+right.second;
    return {is_sum_tree, root->data+left.second+right.second};
}


bool isSumTree(Node* root){
    auto ans = solve(root);
    return ans.first;
}
```

<br>

### 4. Sum of Left Leaves
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

<br>


### 5. Check if all leaves are at same level

```cpp
bool traverse(Node* root, int level, int & leaf_level){
    if(!root) return true;

    if(!root->left and !root->right){
        if(leaf_level==-1){
            leaf_level = level;
            return true;
        }

        else if(level == leaf_level) return true;

        return false;
    }

    bool left = traverse(root->left, level+1, leaf_level);
    bool right = traverse(root->right, level+1, leaf_level);

    return left and right;
}


bool check(Node *root){
    int leaf_level = -1;
    return traverse(root, 0, leaf_level);
}
```

<br>

### 6. Sum of Nodes with Even-Valued Grandparent
Return the sum of values of nodes with an even-valued grandparent. If there are no nodes with an even-valued grandparent, return 0.

![img](https://assets.leetcode.com/uploads/2021/08/10/even1-tree.jpg)

Output: 18

```cpp
void solve (TreeNode* root, int &sum, int grandparent, int parent){
    if(!root) return;

    if(grandparent>0 and grandparent%2==0)
        sum += root->val;

    solve(root->left, sum, parent, root->val);
    solve(root->right, sum, parent, root->val);
}

int sumEvenGrandparent(TreeNode* root) {
    int sum = 0;
    solve (root, sum, -1, -1);
    return sum;
}
```

<br>

### 7. Count Complete Tree Nodes (Tricky)
Given the root of a complete binary tree, return the number of the nodes in the tree. Design an algorithm that runs in less than O(n) time complexity.

```cpp
int find_left_height(TreeNode* root){
    int h = 0;       
    while(root){
        h++;
        root = root->left;
    }
    return h;
} 

int find_right_height(TreeNode* root){
    int h = 0;       
    while(root){
        h++;
        root = root->right;
    }
    return h;
}

int count(TreeNode* root, int leftH, int rightH){

    /* 
        leftH and rightH == -1 --> means we don't know about the height 
        We will only compute left and right height if we don't know it 
    */

    if(leftH==-1) leftH = find_left_height(root);
    if(rightH==-1) rightH = find_right_height(root);

    if(leftH==rightH) return pow(2, leftH)-1;

    int countLeft = count(root->left, leftH-1, -1);
    int countRight = count(root->right, -1, rightH-1);

    return 1 + countLeft + countRight;
}

int countNodes(TreeNode* root) {
    return count(root, -1, -1);
}
```

<br>

### 8. All Possible Full Binary Trees
Given an integer n, return a list of all possible full binary trees with n nodes. Each node of each tree in the answer must have Node.val == 0. A full binary tree is a binary tree where each node has exactly 0 or 2 children.

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/22/fivetrees.png)

```cpp
vector<TreeNode*> allPossibleFBT(int n) {

    if(n%2==0) return {};               // --> FBT can have only odd number of nodes
    if(n==1) return {new TreeNode()};

    vector<TreeNode*> v;     

    for(int i=1; i<n-1; i+=2){          // --> incrementation by 2 to avoid redundant recursive call for even number of nodes

        auto left = allPossibleFBT(i);  
        auto right = allPossibleFBT(n-i-1);

        for(auto l : left){
            for(auto r : right){
                TreeNode* root = new TreeNode();
                root->left = l;
                root->right = r;
                v.push_back(root);
            }
        }    
    }

    return v;
}
```

<br>

### 9. Most Frequent Subtree Sum
Given the root of a binary tree, return the most frequent subtree sum. If there is a tie, return all the values with the highest frequency in any order.

```cpp
int traverse(TreeNode* root, map<int,int> & mp){
    if(!root) return 0;

    int left = traverse(root->left, mp);
    int right = traverse(root->right, mp);

    int sum = left + right + root->val;
    mp[sum]++;

    return sum;
}

vector<int> findFrequentTreeSum(TreeNode* root) {
    vector<int> res;        
    map<int,int> mp;
    int max_freq = INT_MIN;

    traverse(root, mp);

    for(auto p : mp){
        if(p.second == max_freq)
            res.push_back(p.first);

        else if(p.second > max_freq){
            res.clear();
            max_freq = p.second;
            res.push_back(p.first);
        }
    }

    return res;
}
```

<br>

### 10. Maximum Product of Splitted Binary Tree
Given a binary tree root. Split the binary tree into two subtrees by removing 1 edge such that the product of the sums of the subtrees are maximized. Since the answer may be too large, return it modulo 10^9 + 7.

![img](https://assets.leetcode.com/uploads/2020/01/21/sample_1_1699.png)

Input: root = [1,2,3,4,5,6]

Output: 110

Hint: In First pass, get the total sum. In Second pass, find the biggest product by calculating sum of all subtrees and product using sum * (totalSum-sum).

```cpp
int MOD = pow(10, 9) + 7;

long long findTotalSum(TreeNode* root){
    if(!root) return 0;     
    return root->val + findTotalSum(root->left) + findTotalSum(root->right);
}

long long findMaxProduct(TreeNode* root, long long & maxProduct, long long & totalSum){
    if(!root) return 0;

    long long left = findMaxProduct(root->left, maxProduct, totalSum);
    long long right = findMaxProduct(root->right, maxProduct, totalSum);

    long long sum = left + right + root->val;
    long long product = sum * (totalSum-sum);

    maxProduct = max(maxProduct, product);
    return sum;
}

int maxProduct(TreeNode* root) {
    long long maxProduct = 0;
    long long sum = findTotalSum(root);

    findMaxProduct(root, maxProduct, sum);
    return maxProduct%MOD;
}
```

<br>

### 11. Burn a Tree
Given a binary tree denoted by root node A and a leaf node B from this tree. All nodes connected to a given node (left child, right child and parent) get burned in 1 second. Then all the nodes which are connected through one intermediate get burned in 2 seconds, and so on. Find the minimum time required to burn the complete binary tree.

[Problem](https://www.interviewbit.com/problems/burn-a-tree/)

```
Input: B = 4

    1 
   / \ 
  2   3 
 /   / \
4   5   6
        
Output: 4
```

Hint: Find the longest distant leaf node from node B

```cpp

/* Return type: pair of {whether flame comes from that branch, height below that subtree} */

pair<bool, int> burn (TreeNode* root, int B, int &time){
    if(!root) return {false, 0};

    if(!root->left and !root->right){
        if(root->val==B) return {true, 0};      // --> Its the starting point and its nbrs will also burn at same time
        else return {false, 1};                 // --> Normal leaf node, simply return height 1
    }

    auto left = burn(root->left, B, time);
    auto right = burn(root->right, B, time);

    /* If from any branch, flame will come then update the burning time by adding distance from left and right */

    if(left.first or right.first){
        time = max(time, left.second+right.second+1);

        if(left.first) return {true, left.second+1};
        else return {true, right.second+1};
    }
    
    /* Else simply return maxheight of subtree starting from curr node */

    else return {false, max(left.second, right.second)+1};
}


int Solution::solve(TreeNode* A, int B) {
    int time = 0;
    auto p = burn(A, B, time);
    return time;
}
```

<br>



## @ Questions based on Deletion of Nodes

### 1. Delete Leaves With a Given Value

Hint: Traverse in postorder manner

```cpp
TreeNode* removeLeafNodes(TreeNode* root, int target) {
    if(!root) return NULL;

    root->left = removeLeafNodes(root->left, target);
    root->right = removeLeafNodes(root->right, target);

    if(!root->left and !root->right and root->val == target)
        return NULL;
    else
        return root;
}
```

<br>

### 2. Binary Tree Pruning
Given the root of a binary tree, return the same tree where every subtree (of the given tree) not containing a target_val has been removed.

Hint: Return type is boolean which tells whether that subtree contains that value or not

```cpp
bool prune(TreeNode* root, int target){
    if(!root) return false;

    bool left = prune(root->left, target);
    bool right = prune(root->right, target);

    if(!left) root->left = NULL;
    if(!right) root->right = NULL;

    return left or right or root->val==target;
}

TreeNode* pruneTree(TreeNode* root) {
    bool res = prune(root, 1);

    if(!res) return NULL;
    return root;
}
```

<br>

### 3. Delete Nodes And Return Forest
Given the root of a binary tree, each node in the tree has a distinct value. After deleting all nodes with a value in to_delete, we are left with a forest (a disjoint union of trees). Return the roots of the trees in the remaining forest. You may return the result in any order.

Input: root = [1,2,3,4,5,6,7], to_delete = [3,5]

![img](https://assets.leetcode.com/uploads/2019/07/01/screen-shot-2019-07-01-at-53836-pm.png)

Output: [[1,2,null,4],[6],[7]]

Hint: Traverse in postorder manner. If node to be deleted is leaf then simply return NULL. Else if node to be deleted is internal node, save its left child and right child to resultant vector and return NULL.

```cpp
TreeNode* traverse(TreeNode* root, vector<TreeNode*> & res, unordered_set<int> & del){
    if(!root) return NULL;

    root->left = traverse(root->left, res, del);
    root->right = traverse(root->right, res, del);

    if(del.find(root->val)!=del.end()){

        if(!root->left and !root->right) return NULL;
        else{
            if(root->left) res.push_back(root->left);
            if(root->right) res.push_back(root->right);
            return NULL;
        }
    }
    else 
        return root;
}

vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete) {
    vector<TreeNode*> res;
    unordered_set<int> del;

    for(int i : to_delete)
        del.insert(i);

    traverse(root, res, del);

    if(del.find(root->val)==del.end())      // --> Boundary case
        res.push_back(root);

    return res;
}
```

<br>

### 4. Remove Half Nodes
Remove all the half nodes and return the final binary tree. Half nodes are nodes which have only one child. Leaves should not be touched as they have both children as NULL.

```cpp
TreeNode* solve(TreeNode* A) {
    if(!A) return A;
    if(!A->left and !A->right) return A;

    auto left = solve(A->left);
    auto right = solve(A->right);

    if(!left) return right;
    if(!right) return left;

    A->left = left;
    A->right = right;
    return A;
}
```


