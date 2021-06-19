# Binary Tree

## DFS (Recursive) based Questions

### @ Iterative Tree Traversals

#### 1. Preorder using stack

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

#### 2. Inorder using stack and variable

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

#### 3. Postorder using two stacks

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

#### 4. Morris Inorder Traversal (Time complexity : O(n) | Space complexity : O(1))

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

#### 4. Binary Tree to Doubly Linked List
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

#### 5. Add One Row to Tree
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

#### 6. Populating Next Right Pointers in Each Node (Tricky)

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

#### 6. Path Sum II
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

#### 7. Path Sum III (Tricky)
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

#### 8. Sum of Nodes on the Longest path from root to leaf node 
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

#### 9. Same Tree
Given the roots of two binary trees p and q, write a function to check if they are the same or not. Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

```cpp
bool isSameTree(TreeNode* p, TreeNode* q) {
    if(!p and !q) return true;
    if(!p or !q) return false;

    return (p->val==q->val and isSameTree(p->left, q->left) and isSameTree(p->right, q->right));
}
```

#### 10. Symmetric Tree
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

#### 11. Check if Tree is Isomorphic
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

#### 12. Subtree of Another Tree
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

#### 13. Check if a Binary Tree contains duplicate subtrees of size 2 or more 
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

#### 14. Find all Duplicate subtrees in a Binary tree

![img](https://contribute.geeksforgeeks.org/wp-content/uploads/tree1-1.png)

Output : 2 4

         4

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

#### 15. Lowest Common Ancestor of a Binary Tree

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

#### 16. Min distance between two given nodes of a Binary Tree

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

#### 17. Sum of Left Leaves
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

#### 18. Check if all leaves are at same level

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

#### 19. Transform to Sum Tree
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

#### 20. Check if Binary tree is Sum tree or not
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

#### 21. Is Siblings
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

#### 22. Cousins in Binary Tree
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

#### 23. Balanced Binary Tree
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

#### 24. Diameter of Binary Tree (Longest path from leaf to leaf)

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

#### 25. Minimum Depth of Binary Tree
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

#### 26. Distribute Coins in Binary Tree
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

#### 27. House Robber III (Similar as Max Subset Sum in Binary tree)
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

#### 28. Lowest Common Ancestor of Deepest Leaves (Tricky)

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

#### 29. Binary Tree Cameras
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

#### 30. Binary Tree Maximum Path Sum (from any node to any node)
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

#### 2. Construct Binary Tree from Preorder and Inorder Traversal

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

#### 3. Construct Binary Tree from Inorder and Postorder Traversal

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

### @ Questions based on unique Tree Traversals

#### 1. Diagonal Traversal of Binary Tree 

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

#### 2. Boundary Traversal of Binary Tree

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

#### 4. Maximum Width of Binary Tree
The maximum width of a tree is the maximum width among all levels. The width of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes are also counted into the length calculation.

```cpp
int widthOfBinaryTree(TreeNode* root) {
    if(!root) return 0;

    queue<pair<TreeNode*,unsigned long long>> q;
    q.push({root, 1});

    unsigned long long currentCount = 1, childCount = 0;
    unsigned long long width = 1;

    while(!q.empty()){
        auto p = q.front(); q.pop();
        TreeNode* node = p.first; 
        unsigned long long pos = p.second;
        currentCount--;

        if(node->left){
            q.push({node->left, 2*pos});
            childCount++;
        }

        if(node->right){
            q.push({node->right, 2*pos+1});
            childCount++;
        }

        if(currentCount==0 and !q.empty()){
            unsigned long long left = q.front().second;
            unsigned long long right = q.back().second;

            width = max(width, right-left+1);

            currentCount = childCount;;
            childCount = 0;
        }
    }

    return (int)width;
}
```

#### 5. Vertical Order Traversal of a Binary Tree
The vertical order traversal of a binary tree is a list of top-to-bottom orderings for each column index starting from the leftmost column and ending on the rightmost column. There may be multiple nodes in the same row and same column. In such a case, sort these nodes by their values.

```cpp
vector<vector<int>> verticalTraversal(TreeNode* root) {
    vector<vector<int>> res;
    if(!root) return res;

    map<int,vector<int>> mp;               /* <pos, vector of values in top to bottom manner> */

    queue<pair<TreeNode*, int>> q;
    q.push({root,0});

    int currentCount = 1, childCount = 0;

    vector<pair<int, int>> v;          // ---- > used for removing conflict between two nodes at same level and at same pos

    while(!q.empty()){

        auto p = q.front(); q.pop();
        TreeNode* node = p.first;
        int pos = p.second;
        currentCount--;

        v.push_back({pos, node->val});

        if(node->left){
            q.push({node->left, pos-1});
            childCount++;
        }

        if(node->right){
            q.push({node->right, pos+1});
            childCount++;
        }

        if(currentCount==0){
            currentCount = childCount;
            childCount = 0;

            sort(v.begin(), v.end());

            for(auto item : v){
                int key = item.first, val = item.second;
                mp[key].push_back(val);
            }

            v.clear();
        }
    }

    for(auto p : mp){
        vector<int> temp;
        for(int i : p.second)
            temp.push_back(i);

        res.push_back(temp);
    }

    return res;
}
```

#### 6. Deepest Leaves Sum
Given the root of a binary tree, return the sum of values of its deepest leaves.

```cpp
int deepestLeavesSum(TreeNode* root) {
    if(!root) return 0;

    queue<TreeNode*> q;
    q.push(root);

    int currentCount = 1, childCount = 0;
    int deepest_leaves_sum = 0;

    while(!q.empty()){
        TreeNode* node = q.front(); q.pop();
        currentCount--;
        deepest_leaves_sum += node->val;

        if(node->left){
            q.push(node->left);
            childCount++;
        }

        if(node->right){
            q.push(node->right);
            childCount++;
        }

        if(currentCount == 0){
            if(childCount==0) return deepest_leaves_sum;
            currentCount = childCount;
            childCount = 0;
            deepest_leaves_sum = 0;
        }
    }
    return 0;
}
```

### @ Questions based on Different views of Binary Tree

#### 1. Right Side View

Hint: Print last node of each level (when currentCount = 0)

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

Note: For left view, just print first node of every level (when childCount = 0)


#### 2. Top View 

Hint: Insert only when the pos does not occur before (i.e. not present in the map)

```cpp
vector<int> topView(Node *root){
    vector<int> res;
    if(!root) return res;

    map<int, int> mp;     /* <pos, val first occured for that pos >*/

    queue<pair<Node*,int>> q;
    q.push({root,0});

    while(!q.empty()){
        auto p = q.front(); q.pop();
        Node* node = p.first;
        int pos = p.second;

        if(mp.find(pos)==mp.end())              // ----> Just remove this condition for printing bottom view
            mp[pos] = node->data;

        if(node->left) q.push({node->left, pos-1});
        if(node->right) q.push({node->right, pos+1});
    }

    for(auto p : mp)
        res.push_back(p.second);

    return res;
}
```

Note: For bottom view, just remove the above condition stated in comment
