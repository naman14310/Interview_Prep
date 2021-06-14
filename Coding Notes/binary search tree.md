# Binary Search Tree (BST)
In BST, we need to decide whether to go left or right according to question.

## DFS (Recursive) based Questions

#### 1. Search in a Binary Search Tree

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

#### 2. Insert into a Binary Search Tree

```cpp
TreeNode* insertIntoBST(TreeNode* root, int val) {
    if(!root) return new TreeNode(val);

    if(root->val < val)
        root->right = insertIntoBST(root->right, val);
    else
        root->left = insertIntoBST(root->left, val);

    return root;
}
```

#### 3. Range Sum of BST
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

#### 4. Trim a Binary Search Tree
Given the root of a binary search tree and the lowest and highest boundaries as low and high, trim the tree so that all its elements lies in [low, high]. 

```cpp
TreeNode* trimBST(TreeNode* root, int low, int high) {
    if(!root) return root;

    if(root->val >= low and root->val <= high){
        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
        return root;
    }

    else if(root->val < low)
        return trimBST(root->right, low, high);

    else return trimBST(root->left, low, high);
}
```

#### 5. Lowest Common Ancestor of a Binary Search Tree
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
#### 6. Kth Smallest Element in a BST

```cpp
void inorder(TreeNode* root, int & k, int & ans){
    if(root){
        inorder(root->left, k, ans);
        k--;

        if(k==0){
            ans = root->val;
            return;
        } 

        if(k>0) inorder(root->right, k, ans);
    }
}

int kthSmallest(TreeNode* root, int k) {
    int ans = -1;
    inorder(root, k, ans);
    return ans;
}
```

#### 7. Convert BST to Greater Tree
Given the root of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

![img](https://assets.leetcode.com/uploads/2019/05/02/tree.png)

```cpp
void traverse(TreeNode * root, int & greaterSum){
    if(!root) return;

    traverse(root->right, greaterSum);

    int temp = root->val;
    root->val += greaterSum;
    greaterSum += temp;

    traverse(root->left, greaterSum);
}

TreeNode* convertBST(TreeNode* root) {
    int greaterSum = 0;
    traverse(root, greaterSum);
    return root;
}
```

#### 8. Minimum Absolute Difference in BST
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


#### 9. Find Mode in Binary Search Tree
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

#### 10. Increasing Order Search Tree

![img](https://assets.leetcode.com/uploads/2020/11/17/ex1.jpg)

Method 1 : Do inorder traversal and build newtree.
Drawback : Require extra space for building new tree.

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

Method 2 : Flatten binary tree to linked list using recursion

```cpp
/* 
This function return head and tail of linked list formed by flatten bst
return type : pair<head, tail> 
*/

pair<TreeNode*, TreeNode*> flatten(TreeNode* root){

    /* Case 1 : No left and right child */
    
    if(!root->left and !root->right) return {root, root};
    
    /* Case 2 : No Right child */    
    
    else if(!root->left){
        auto right = flatten(root->right);
        root->right = right.first;
        return {root, right.second};
    }

    /* Case 3 : No left child */
    
    else if(!root->right){
        auto left = flatten(root->left);
        left.second->right = root;
        root->left = NULL;
        return {left.first, root};
    }

    /* Case 4 : Have both childs */

    else{
        auto left = flatten(root->left);
        auto right = flatten(root->right);      

        left.second->right = root;
        root->right = right.first;
        root->left = NULL;

        return {left.first, right.second};
    }
}

TreeNode* increasingBST(TreeNode* root) {
    if(!root) return NULL;
    auto ll = flatten(root);
    return ll.first;
}
```

#### 11. Convert Sorted Array to Height Balanced Binary Search Tree
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

#### 12. Two Sum IV - Input is a BST
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




