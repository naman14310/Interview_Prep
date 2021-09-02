# Binary Search Tree (BST)
In BST, we need to decide whether to go left or right according to question.

<br>

## @ Operations on BST

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

<br>

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

<br>

#### 3. Delete Node in a BST

```cpp
TreeNode* find_successor(TreeNode* node){
    TreeNode* successor = node;

    while(successor->left)
        successor = successor->left;

    return successor;
}

TreeNode* deleteNode(TreeNode* root, int key) {
    if(!root) return NULL;

    if(root->val < key)
        root->right = deleteNode(root->right, key);

    else if(root->val > key)
        root->left = deleteNode(root->left, key);

    else{

        /* If right subtrees of deleted node doesn't exist, simply return its left child */

        if(!root->right){
            TreeNode* temp = root->left;
            delete root;
            return temp;
        }

        /* Replace node->val with successor->val and delete successor node */

        else{
            TreeNode* successor = find_successor(root->right);
            root->val = successor->val;
            root->right = deleteNode(root->right, successor->val);
        }

    }

    return root;
}
```

<br>

#### 4. Find Inorder Successor and Predecessor in a BST

```cpp
void findPreSuc(Node* root, Node*& pre, Node*& suc, int key){
    pre = NULL; suc = NULL;
    
    while(root){
        
        /* saving successor while going downwards */
        
        if(root->key > key){
            suc = root;
            root = root->left;
        }
        
        /* saving predecessor while going downwards */
        
        else if(root->key < key){
            pre = root;
            root = root->right;
        }
        
        else{
            
            /* if node->left exist then predecessor will be rightmost element of left subtree */
            
            if(root->left){
                Node* temp = root->left;
                while(temp){
                    pre = temp;
                    temp = temp->right;
                }
            }
            
            /* if node->right exist then successor will be leftmost element of right subtree */
            
            if(root->right){
                Node* temp = root->right;
                while(temp){
                    suc = temp;
                    temp = temp->left;
                }
            }
            
            break;
        }
    }
}
```

<br>

#### 5. Serialize and Deserialize BST

Hint: Convert to preorder

```cpp
string serialize(TreeNode* root) {
	if(!root) return "";
	return to_string(root->val) + "," + serialize(root->left) + serialize(root->right);
}

TreeNode* buildTree(vector<int> v, int & i, int min_val, int max_val){
	if(i>=v.size() or v[i]<min_val or v[i]>max_val)
		return NULL;

	TreeNode* root = new TreeNode(v[i]);
	i++;

	root->left = buildTree(v, i, min_val, root->val);
	root->right = buildTree(v, i, root->val, max_val);

	return root;
}

// Decodes your encoded data to tree.
TreeNode* deserialize(string data) {
	vector<int> v;
	stringstream ss(data);
	string tkn;
	int i=0; 

	while(getline(ss, tkn, ','))
		v.push_back(stoi(tkn));

	return buildTree(v, i, INT_MIN, INT_MAX);
}
```

<br>

## Standard Questions

#### 1. Range Sum of BST
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

<br>

#### 2. Trim a Binary Search Tree
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

<br>

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

<br>

#### 4. Validate Binary Search Tree

```cpp
bool isValid(TreeNode* root, long long minRange, long long maxRange){
    if(!root) return true;

    if(root->val <= minRange or root->val >= maxRange) return false;

    return isValid(root->left, minRange, root->val) and isValid(root->right, root->val, maxRange);
}

bool isValidBST(TreeNode* root) {
    return isValid(root, LLONG_MIN, LLONG_MAX);
}
```

<br>

#### 5. Convert BST to Greater Tree
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

<br>

#### 6. Largest BST
Given a binary tree. Find the size of its largest subtree that is a Binary Search Tree.

Hint: Return a structure containing following four fields -

1. bool isBST
2. int size
3. int min_val
4. int max_val

```cpp
struct box{
  bool isBST;
  int size;
  int min_val;
  int max_val;
  
  box(bool isBST, int size, int min_val, int max_val){
      this->isBST = isBST;
      this->size = size;
      this->min_val = min_val;
      this->max_val = max_val;
  }
};

box traverse(Node* root, int & size_of_largest_bst){
    if(!root){
        box b (true, 0, INT_MAX, INT_MIN);
        return b;
    }
    
    auto left = traverse(root->left, size_of_largest_bst);
    auto right = traverse(root->right, size_of_largest_bst);
    
    if(left.isBST and right.isBST and root->data > left.max_val and root->data < right.min_val){
        int size = left.size + right.size + 1;
        int min_val = left.min_val == INT_MAX ? root->data : left.min_val;
        int max_val = right.max_val == INT_MIN ? root->data : right.max_val;
        
        size_of_largest_bst = max(size_of_largest_bst, size);
        box b (true, size, min_val, max_val);
        return b;
    }
    else{
        box b (false, 0, 0, 0);
        return b;
    }
}

int largestBst(Node *root){
	int size_of_largest_bst = 0;
	traverse(root, size_of_largest_bst);
	return size_of_largest_bst;
}
```

<br>

## @ Questions based on sorted Inorder Property

#### 1. Kth Smallest Element in a BST

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

<br>

#### 2. Minimum Absolute Difference in BST
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

<br>

#### 3. Median of BST

```cpp
void count_nodes(Node* root, int & count){
    if(!root) return;
    
    count++;
    count_nodes(root->left, count);
    count_nodes(root->right, count);
}

void inorder(Node* root, int & mid, int & curr, int & prev){
    if(!root) return;
    
    inorder(root->left, mid, curr, prev);
    
    mid--;
    if(mid==1) prev = root->data;
    else if(mid==0) curr = root->data;
    
    if(mid>0) inorder(root->right, mid, curr, prev);
}

float findMedian(struct Node *root){
    int count = 0;
    count_nodes(root, count);

    int mid = (count/2)+1, curr = -1, prev = -1;
    inorder(root, mid, curr, prev);

    if(count&1) return curr;
    else return (prev + curr)/2.0;
}
```

Optimization : Use morris order traversal for reducing space

<br>

#### 4. Find Mode in Binary Search Tree
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

<br>

#### 5. Recover Binary Search Tree
You are given the root of a binary search tree (BST), where exactly two nodes of the tree were swapped by mistake. Recover the tree without changing its structure.

![img](https://assets.leetcode.com/uploads/2020/10/28/recover2.jpg)

Hint: Just do inorder traversal and keep track of first and last incorrect placed node

```cpp
void inorder(TreeNode* root, TreeNode* & prev, TreeNode* & first, TreeNode* & second){
    if(!root) return;

    inorder(root->left, prev, first, second);

    if(prev and root->val < prev->val){
        if(!first) first = prev;
        second = root;
    } 
    prev = root;

    inorder(root->right, prev, first, second);
}

void recoverTree(TreeNode* root) {
    TreeNode* first = NULL, *second = NULL, *prev = NULL;
    inorder(root, prev, first, second);
    swap(first->val, second->val);
}
```

<br>

#### 6. Two Sum IV - Input is a BST
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

<br>

#### 7. Print Elements of two BST in Sorted Order (Merge two BST's inorder)

Hint: Use Iterative inorder traversal. Use two stacks and two curr var for processing both the trees simultaneously.

```cpp
void process_remaining_stk(TreeNode* curr, stack<TreeNode*> & stk, vector<int> & res){
	while(!stk.empty() or curr){
		while(curr){
			stk.push(curr);
			curr = curr->left;
		}

		res.push_back(stk.top()->val);
		curr = stk.top()->right;
		stk.pop();
	}
}

vector<int> getAllElements(TreeNode* root1, TreeNode* root2) {
	vector<int> res;
	stack<TreeNode*> stk1, stk2;
	TreeNode* curr1 = root1, *curr2 = root2;

	while((!stk1.empty() or curr1) and (!stk2.empty() or curr2)){
		while(curr1){
			stk1.push(curr1);
			curr1 = curr1->left;
		}

		while(curr2){
			stk2.push(curr2);
			curr2 = curr2->left;
		}

		/* Push smaller element of both stk1.top() and stk2.top() */

		if(stk1.top()->val <= stk2.top()->val){
			res.push_back(stk1.top()->val);
			curr1 = stk1.top()->right;
			stk1.pop();
		}
		else{
			res.push_back(stk2.top()->val);
			curr2 = stk2.top()->right;
			stk2.pop();
		}
	}

	if(!stk1.empty() or curr1)
		process_remaining_stk(curr1, stk1, res);

	if(!stk2.empty() or curr2)
		process_remaining_stk(curr2, stk2, res);

	return res;
}
```

Note: For merging n trees, convert them to linked list and then merge n linked lists to get combined sorted inorder.

<br>

#### 8. Minimum swap required to convert Binary Tree to Binary Search Tree

Approach: Find the inorder of binary tree. Now since inorder of BST is sorted so the problem is reduced to minimum no. of swaps to sort the array. Find the solution of this problem in Sorting section. 

<br>


## @ Questions based on BST construction and BST to LL conversion

#### 1. Convert Sorted Array to Height Balanced Binary Search Tree
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

<br>

#### 2. Convert Sorted List to Binary Search Tree

```cpp
ListNode* findMid(ListNode* head){
    ListNode* slow = head, *fast = head, *prev=NULL;

    while(fast and fast->next){
        fast = fast->next->next;
        prev = slow;
        slow = slow->next;
    }

    prev->next = NULL;
    return slow;
}

TreeNode* sortedListToBST(ListNode* head) {
    if(!head) return NULL;
    if(!head->next) return new TreeNode(head->val);

    ListNode* mid = findMid(head);

    TreeNode* root = new TreeNode(mid->val);
    root->left = sortedListToBST(head);
    root->right = sortedListToBST(mid->next);

    return root;
}
```

<br>

#### 3. Construct Binary Search Tree from Preorder Traversal (Tricky O(n) Solution)

Hint: The trick is to set a range {min .. max} for every node. Initialize the range as {INT_MIN .. INT_MAX}. The first node will definitely be in range, so create a root node. To construct the left subtree, set the range as {INT_MIN …root->data}. If a value is in the range {INT_MIN .. root->data}, the values are part of the left subtree. To construct the right subtree, set the range as {root->data..max .. INT_MAX}.

```cpp
TreeNode* buildTree(vector<int> & preorder, int & i, int minRange, int maxRange){
	if(i>=preorder.size() or preorder[i]<minRange or preorder[i]>maxRange)
	    return NULL;

	TreeNode* root = new TreeNode(preorder[i]);
	i++;

	root->left = buildTree(preorder, i, minRange, root->val);
	root->right = buildTree(preorder, i, root->val, maxRange);

	return root;
}


TreeNode* bstFromPreorder(vector<int>& preorder) {
	int i = 0;
	return buildTree(preorder, i, INT_MIN, INT_MAX);
}
```

<br>

#### 4. Unique Binary Search Trees II
Given an integer n, return all the structurally unique BST's (binary search trees), which has exactly n nodes of unique values from 1 to n. Return the answer in any order.

For, n = 3

![img](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

```cpp
vector<TreeNode*> solve (int start, int end){

	if(start>end) return {NULL};    // --> If we have zero nodes, return empty vector

	/* If we have only one val then return vector containing only one node */

	if(start==end)
		return {new TreeNode(start)};

	vector<TreeNode*> v;

	for(int i=start; i<=end; i++){

		auto left = solve(start, i-1);      // --> recurse for all smaller nums
		auto right = solve(i+1, end);       // --> recurse for all greater nums

		/* Make all possible combination of left and right by attaching them nestedly on left and right of root */

		for(auto l : left){
			for(auto r : right){

				TreeNode* root = new TreeNode(i);
				root->left = l;
				root->right = r;
				v.push_back(root);
			}
		}
	}

	return v;
}


vector<TreeNode*> generateTrees(int n) {
	return solve (1, n);
}
```

<br>

#### 5. Increasing Order Search Tree (Flatten BST)

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

<br>

#### 5. Convert a normal BST into a Balanced BST

Method 1 : Use AVL rotation functions

Method 2 : By converting BST to sorted array (inorder) and build new tree. --> O(n) | O(n)

Method 3 : By converting BST to sorted linked list (inorder) inplace and convert it again to BST. --> O(n) | O(1)

[Most Optimized Solution](https://leetcode.com/problems/balance-a-binary-search-tree/discuss/541785/C%2B%2BJava-with-picture-DSW-O(n)orO(1))

<br>


## @ Other IMP Questions

#### 1. Populate Inorder Successor for all nodes
Given a Binary Tree, write a function to populate next pointer for all nodes. The next pointer for every node should be set to point to inorder successor.

Hint: Do reverse inorder traversal (RNL) and use one variable prev.

```cpp
void connect(Node* root, Node* & prev){
    if(!root) return;

    connect(root->right, prev);
    root->next = prev;
    prev = root;
    connect(root->left, prev);
}


void populateNext(Node *root){
    Node* prev = NULL;
    connect(root, prev);
}
```

<br>

#### 2. Check whether BST contains Dead End
Given a Binary search Tree that contains positive integer values greater then 0. Return true if BST contains a dead end else returns false. Here Dead End means, we are not able to insert any element after that node.

Hint: Use unordered_map to store values while traversing. On reaching leaf node, if val-1 and val+1 exists in the map then that leaf is a dead end.

```
Input :     
              8
            /   \ 
           7     10
         /      /   \
        2      9     13

Output : Yes
Explanation : We can't insert any element at node 9.  
```

```cpp
bool traverse(Node* root, unordered_set<int> & s){
    if(!root) return false;
    
    int val = root->data;
    
    if(!root->left and !root->right){
    
        if (val==1 and s.find(2)!=s.end()) 
            return true;   // --> boundary case
        
        if(s.find(val-1)!=s.end() and s.find(val+1)!=s.end()) 
            return true;
    }

    s.insert(val);
    
    bool left = traverse(root->left, s);
    bool right = traverse(root->right, s);
    
    return left or right;
}

bool isDeadEnd(Node *root){
    unordered_set<int> s;
    return traverse(root, s);
}
```

<br>

#### 3. Valid BST from Preorder (Tricky - O(n) solution)

Hint: Use a stack. Here we find the next greater element and after finding next greater, if we find a smaller element, then return false.

[Explaination](https://www.youtube.com/watch?v=GYdC4hQSo8A)

```cpp
int solve(vector<int> &A) {
    stack<int> stk;
    int root = INT_MIN;

    for(int i=0; i<A.size(); i++){

        while(!stk.empty() and A[i]>stk.top()){
            root = stk.top();
            stk.pop();
        }

        if(A[i]<root) return 0;

        stk.push(A[i]);
    }

    return 1;
}
```


