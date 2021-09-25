# Binary Tree (BFS or Queue based Question)

<br>

## @ Problems on Level Order Traversal

### 1. Average of Levels in Binary Tree

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

<br>

### 2. Binary Tree Zigzag Level Order Traversal

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

<br>

### 3. Deepest Leaves Sum
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

<br>



## @ Few IMP Interview Questions

### 1. All Nodes Distance K in Binary Tree (VIMP)
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

<br>

### 2. Maximum Width of Binary Tree
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

<br>

### 3. Vertical Order Traversal of a Binary Tree
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

<br>



## @ Questions based on Different views of Binary Tree

### 1. Right Side View

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

<br>

### 2. Top View 

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
