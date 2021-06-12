# Binary Search Tree (BST)
In BST, we need to decide whether to go left or right according to question.

## DFS (Recursive) based Questions

#### 1. Search in a Binary Search Tree
Find the node in the BST that the node's value equals val and return the subtree rooted with that node. If such a node does not exist, return null.

```cpp
TreeNode* searchBST(TreeNode* root, int val) {
    if(!root or root->val == val) 
        return root;
    else if(root->val < val) 
        return searchBST(root->right, val);
    else return searchBST
        (root->left, val);
}
```

#### 2. Range Sum of BST
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

#### 3. Lowest Common Ancestor of a Binary Search Tree
Here we allow a node to be a descendant of itself.

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(!root) return NULL;

    if(root->val < min(p->val, q->val)) 
        return lowestCommonAncestor(root->right, p, q);

    if(root->val > max(p->val, q->val)) 
        return lowestCommonAncestor(root->left, p, q);

    return root;
}
```

#### 4. Minimum Absolute Difference in BST
Given the root of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.

Approach 1 : Simply perform inorder traversal (which gives sorted order). Everytime compute minDiff with curr node and prev node. 

```cpp
void inorder(TreeNode* root, int & prev, int & minDiff){
    if(root){
        inorder(root->left, prev, minDiff);
        minDiff = min(minDiff, abs(root->val - prev));
        prev = root->val;
        inorder(root->right, prev, minDiff);
    }
}

int getMinimumDifference(TreeNode* root) {
    int minDiff = INT_MAX;
    int prev = INT_MAX;

    inorder(root, prev, minDiff);
    return minDiff;
}
```
Approach 2 : Do postorder traversal and return a pair of {low, high} for every recursive call, compute diff of curr->val, maxL and curr->val, minR and return minimum of both. Approach 1 is preferable over this.

#### 5. Find Mode in Binary Search Tree
Given the root of a binary search tree (BST) with duplicates, return all the mode(s) (i.e., the most frequently occurred element) in it. If the tree has more than one mode, return them in any order.

Approach : Do Inorder traversal (as it gives sorted order on BST) and compute mode.

```cpp
void inorder(TreeNode* root, int & curr_count, int & running_item, int & mode, vector<int> & mode_item){
    if(root){
        inorder(root->left, curr_count, running_item, mode, mode_item);

        if(curr_count==0 or root->val!=running_item){
            curr_count = 1;
            running_item = root->val;
        }
        else curr_count++;

        if(curr_count > mode){
            mode = curr_count;
            mode_item.clear();
            mode_item.push_back(running_item);
        }
        else if(curr_count == mode)
            mode_item.push_back(running_item);
        
        inorder(root->right, curr_count, running_item, mode, mode_item);
    }
}

vector<int> findMode(TreeNode* root) {
    int curr_count = 0, running_item = INT_MIN, mode = 0;
    vector<int> mode_item;

    inorder(root, curr_count, running_item, mode, mode_item);
    return mode_item;
}
```

#### 6. Convert Sorted Array to Height Balanced Binary Search Tree
Given an integer array nums where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.

```cpp
TreeNode* buildTree(vector<int> & nums, int start, int end){
    if(start>end) return NULL;

    int mid = start + (end-start)/2;

    TreeNode* root = new TreeNode(nums[mid]);
    root->left = buildTree(nums, start, mid-1);
    root->right = buildTree(nums, mid+1, end);

    return root;
}

TreeNode* sortedArrayToBST(vector<int>& nums) {
    return buildTree(nums, 0, nums.size()-1);
}
```

#### 7. Two Sum IV - Input is a BST
Given the root of a Binary Search Tree and a target number k, return true if there exist two elements in the BST such that their sum is equal to the given target.

```cpp
void inorder(TreeNode* root, vector<int> & v){
    if(root){
        inorder(root->left, v);
        v.push_back(root->val);
        inorder(root->right, v);
    }
}

bool findTarget(TreeNode* root, int k) {
    vector<int> v;
    inorder(root, v);

    int start = 0, end = v.size()-1;
    while(start<end){
        int sum = v[start] + v[end];

        if(sum==k) return true;
        else if(sum<k) start++;
        else end--;
    }
    return false;
}
```




