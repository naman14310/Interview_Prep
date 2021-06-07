# Linked List

## Easy

#### 1. Reverse a linked list

**Iterative**
```cpp
ListNode* reverseList(ListNode* head) {
    ListNode *prev=NULL, *curr=head, *next=NULL;
    while(curr){
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

**Recursive** 
```cpp
  ListNode* new_head;

  void reverse(ListNode* prev, ListNode* curr){
      if(!curr) return;

      if(!curr->next) new_head = curr; 

      reverse(curr, curr->next);
      curr->next = prev;
  }

  ListNode* reverseList(ListNode* head) {
      reverse(NULL, head);
      return new_head;
  }
```

#### 2. Middle of a Linked list (slow and fast pointer)

```cpp
ListNode* middleNode(ListNode* head) {
    ListNode* slow = head, *fast = head;

    while(fast && fast->next){
        slow = slow->next;
        fast=fast->next->next;
    }
    return slow;
}
```

#### 3. Delete Node in a Linked List
Write a function to delete a node in a singly-linked list. You will not be given access to the head of the list, instead you will be given access to the node to be deleted directly. It is guaranteed that the node to be deleted is not a tail node in the list.

Hint: Copy next node to current node

```cpp
void deleteNode(ListNode* node) {
    node->val = node->next->val;
    node->next = node->next->next;
}
```

#### 4. Merge Two Sorted Lists

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    if(!l1) return l2;
    if(!l2) return l1;

    if(l1->val<=l2->val){
        l1->next = mergeTwoLists(l1->next, l2);
        return l1;
    }
    else{
        l2->next = mergeTwoLists(l1, l2->next);
        return l2;
    }
}
```

#### 5. Remove Duplicates from Sorted List

Hint: Use two pointers

```cpp
ListNode* deleteDuplicates(ListNode* head) {
    if(!head or !head->next) return head;

    ListNode* temp1 = head;
    ListNode* temp2 = head->next;

    while(temp1){
        while(temp2 and temp2->val==temp1->val)
            temp2 = temp2->next;

        temp1->next = temp2;
        temp1 = temp2;

        if(!temp2) break;

        temp2 = temp2->next;
    }
    return head;
}
```

#### 6. Remove Linked List Elements
Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.

```cpp
ListNode* removeElements(ListNode* head, int val) {
    while(head and head->val==val)
        head = head->next;

    if(!head or !head->next) return head;

    ListNode *temp1 = head, *temp2 = head->next;

    while(temp1){
        while(temp2 and temp2->val==val)
            temp2 = temp2->next;

        temp1->next = temp2;
        temp1 = temp2;

        if(!temp2) break;
        temp2 = temp2->next;
    }

    return head;
}
```

#### 7. Intersection of Two Linked Lists

```cpp
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    ListNode* temp1=headA;
    ListNode* temp2=headB;

    while(temp1!=temp2){
        if(!temp1 and !temp2) return NULL;

        if(temp1)
            temp1 = temp1->next;
        else
            temp1 = headB;

        if(temp2)
            temp2 = temp2->next;
        else
            temp2 = headA;
    }
    return temp1;
}
```

#### 8. Linked List Cycle (Floyds cycle detection algo)

```cpp
bool hasCycle(ListNode *head) {
    ListNode *slow = head, *fast = head;

    while(fast and fast->next){
        fast = fast->next->next;
        slow = slow->next;

        if(slow==fast) return true;
    }

    return false;
}
```

#### 9. Linked List Cycle II (return the node where the cycle begins)

Approach:

1. Find the meeting point using above Floyds cycle detection algo. If no cycle is present return NULL else return meeting point.
2. Start one pointer temp1 from head and other pointer temp2 from meetPoint with same speed. Where they both meets, will be our starting node of the cycle.

```cpp
ListNode* findMeetPoint(ListNode* head){
    ListNode* slow=head, *fast=head;

    while(fast and fast->next){
        fast = fast->next->next;
        slow = slow->next;

        if(slow==fast) return slow;
    }

    return NULL;
}

ListNode *detectCycle(ListNode *head) {

    ListNode* meetPoint = findMeetPoint(head);
    if(!meetPoint) return NULL;

    ListNode* temp1 = head, *temp2 = meetPoint;

    while(temp1!=temp2){
        temp1 = temp1->next;
        temp2 = temp2->next;
    }

    return temp1;
}
```

#### 10. Palindrome Linked List

```cpp
ListNode* reverseSecondHalf(ListNode* mid){
    ListNode *prev=NULL, *curr=mid, *next=NULL;
    while(curr){
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }

    return prev;
}

ListNode* findMid(ListNode* head){
    ListNode *slow=head, *fast=head;

    while(fast and fast->next){
        fast = fast->next->next;
        slow = slow->next;
    }

    return slow;
}

bool isPalindrome(ListNode* head) {
    if(!head or !head->next) return true;

    if(!head->next->next){
        if(head->val == head->next->val) return true;
        else return false;
    }

    ListNode* temp1 = head;
    ListNode* mid = findMid(head);
    ListNode* temp2 = reverseSecondHalf(mid);

    while(temp1!=temp2 and temp2){
        if(temp1->val != temp2->val) return false;
        temp1 = temp1->next;
        temp2 = temp2->next;
    }
    return true;
}
```

#### 11. Swapping Nodes in a Linked List
You are given the head of a linked list, and an integer k. Return the head of the linked list after swapping the values of the kth node from the beginning and the kth node from the end (the list is 1-indexed).

![img](https://assets.leetcode.com/uploads/2020/09/21/linked1.jpg)

```cpp
/* Single pass solution */

ListNode* swapNodes(ListNode* head, int k) {
    if(!head or !head->next) return head;

    ListNode *temp1 = head;
    for(int i=1; i<k; i++)
        temp1 = temp1->next;

    ListNode *slow = head, *fast = temp1;
    while(fast->next){
        fast = fast->next;
        slow = slow->next;
    }

    swap(temp1->val, slow->val);

    return head;
}
```

## Medium

#### 1. Odd Even Linked List
Given the head of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return the reordered list. The first node is considered odd, and the second node is even, and so on. Note that the relative order inside both the even and odd groups should remain as it was in the input.

![img](https://assets.leetcode.com/uploads/2021/03/10/oddeven2-linked-list.jpg)

```cpp
ListNode* oddEvenList(ListNode* head) {
    if (!head or !head->next) return head;

    ListNode* head2 = head->next;
    ListNode *temp1 = head, *temp2 = head2, *prev=NULL;

    while(temp1 and temp2){
        temp1->next = temp2->next;
        prev = temp1;
        temp1 = temp1->next;

        if(temp1){
            temp2->next = temp1->next;
            temp2 = temp2->next;
        }
    }

    if(!temp2)
        temp1->next = head2;
    else
        prev->next = head2;

    return head;
}
```

#### 2. Swap Nodes in Pairs (without modifying the values)

![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

**Recursive**

```cpp
ListNode* swapPairs(ListNode* head) {
    if(!head or !head->next) return head;

    ListNode* temp = head->next;
    head->next = swapPairs(temp->next);
    temp->next = head;

    return temp;
}
```

#### 3. Sort List

Hint: Use divide and conquer approach

```cpp
ListNode* findMid(ListNode* head){
    ListNode *slow=head, *fast=head, *prev=NULL;

    while(fast and fast->next){
        fast = fast->next->next;
        prev = slow;
        slow = slow->next;
    }

    if(prev) prev->next = NULL;
    return slow;
}

ListNode* mergeTwoSortedLists(ListNode* l1, ListNode* l2){
    if(!l1) return l2;
    if(!l2) return l1;

    if(l1->val <= l2->val){
        l1->next = mergeTwoSortedLists(l1->next, l2);
        return l1;
    }

    else{
        l2->next = mergeTwoSortedLists(l1, l2->next);
        return l2;
    }        
}

ListNode* sortList(ListNode* head) {
    if(!head or !head->next) return head;

    ListNode* mid = findMid(head);
    ListNode* head1 = sortList(head);
    ListNode* head2 = sortList(mid);

    return mergeTwoSortedLists(head1, head2);
}
```

#### 4. Partition list
Given the head of a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x. You should preserve the original relative order of the nodes in each of the two partitions.

![img](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)

**Solution by creating two seperate lists**

Approach: Use to new linked list, one for storing nodes lesser then x and other for storing nodes greater then or equals to x. 

```cpp
ListNode *partition(ListNode *head, int x) {
    ListNode node1(0), node2(0);
    ListNode *p1 = &node1, *p2 = &node2;
    while (head) {
        if (head->val < x)
            p1 = p1->next = head;
        else
            p2 = p2->next = head;
        head = head->next;
    }
    p2->next = NULL;
    p1->next = node2.next;
    return node1.next;
}
```

**Inplace solution**

Approach:

1. Use two pointers to traverse
2. Left pointer is points the location where node lesser then x will be inserted
3. Right pointer iterates over the whole linked list, find nodes >= x, removes them, copy their data to newNode and attach that newNode in left pointer's pos.

```cpp
void insertAtPos(ListNode* pos, ListNode* newNode){
    newNode->next = pos->next;
    pos->next = newNode;
}

void insertAtBegin(ListNode* &head, ListNode* newNode){
    newNode->next = head;
    head = newNode;
}

ListNode* partition(ListNode* head, int x) {
    if(!head or !head->next) return head;
    ListNode* temp1 = head->val<x ? head : NULL;

    while(temp1 and temp1->val<x and temp1->next and temp1->next->val<x)
        temp1 = temp1->next;

    ListNode* temp2 = temp1 ? temp1 : head;
    while(temp2 and temp2->next){
        if(temp2->next->val < x){
            ListNode* newNode = new ListNode(temp2->next->val);
            temp2->next = temp2->next->next;

            if(temp1){
                insertAtPos(temp1, newNode);
                temp1 = temp1->next;
            }
            else{
                insertAtBegin(head, newNode);
                temp1 = head;
            }

        }

        else temp2 = temp2->next;
    }

    return head;
}
```

#### 5. Copy List with Random Pointer (Tricky)

Approach:

1. Create cross connections using next pointers
2. Copy random pointers using cross connections
3. Reconnect next pointers

```cpp
Node* copyRandomList(Node* head) {
    if(!head) return head;

    Node* temp1 = head;
    Node* newHead;
    bool first = true;

    /* Creating new list and assign cross connections */ 

    while(temp1){
        Node* newNode = new Node(temp1->val);
        newNode->next = temp1->next;

        if(first){
            newHead = newNode;
            first = false;
        }

        temp1->next = newNode;
        temp1 = newNode->next;
    }

    /* Copying random pointers */

    temp1 = head;

    while(temp1){
        temp1->next->random = temp1->random ? temp1->random->next : NULL;
        temp1 = temp1->next->next;
    }

    /* Correcting next pointers */ 

    temp1 = head;

    while(temp1){
        Node* temp2 = temp1->next->next;
        temp1->next->next = temp2 ? temp2->next : NULL;
        temp1->next = temp2;
        temp1 = temp2;
    }

    return newHead;
}
```

#### 6. Reorder List (Cyclic)

![img](https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg)

Approach:

1. Find mid using slow and fast pointers
2. Reverse the linked list after mid (pass mid in reverse function)
3. Iterate two pointers, temp1 from left and temp2 from right till temp1!=mid.
4. Also add another breaking condition inside while loop i.e. temp2==mid (before assiging temp2)

```cpp
ListNode* reverse(ListNode* curr){
    ListNode *prev=NULL, *next=NULL;
    while(curr){
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;        // returning last node of linked list
}

ListNode* findMid(ListNode* head){
    ListNode* slow=head, *fast=head;
    while(fast and fast->next){
        fast = fast->next->next;
        slow = slow->next;
    }
    return slow;
}

void reorderList(ListNode* head) {
    if(!head or !head->next or !head->next->next) return;

    ListNode* temp1 = head;

    ListNode* mid = findMid(head);
    ListNode* temp2 = reverse(mid);

    while(temp1!=mid){
        ListNode* ptr;
        ptr = temp1->next;
        temp1->next = temp2;
        temp1 = ptr;

        if(temp2==mid) break;
        ptr = temp2->next;
        temp2->next = temp1;
        temp2 = ptr;
    }
}
```

#### 7. Reverse Linked List II
Given the head of a singly linked list and two integers left and right where left <= right, reverse the nodes of the list from position left to position right, and return the reversed list.

![img](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

```cpp
ListNode* reverseBetween(ListNode* head, int left, int right) {

    /*
        temp1 -> points towards the node from where we need to reverse the linked list
        prev_temp -> points towards the previous of temp1
    */

    ListNode* temp1 = head, *prev_temp = NULL;
    int count=1;

    while(count<left){
        prev_temp = temp1;
        temp1 = temp1->next;
        count++;
    }

    /* Reversing logic */

    ListNode* prev = NULL, *curr = temp1, *next = NULL;

    while(curr and count<=right){
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
        count++;
    }

    /* Reconnecting list */

    temp1->next = curr;

    if(prev_temp) prev_temp->next = prev;
    else head = prev;                              // Head will change if reversing starts from head

    return head;
}
```

### @ Problems on Conversion of Linked List to Trees and vice versa

#### 1. Convert Sorted List to Binary Search Tree

![img](https://assets.leetcode.com/uploads/2020/08/17/linked.jpg)

Note: Do not forget to make prev null

```cpp
ListNode* findMid(ListNode* head){
    ListNode *slow=head, *fast=head, *prev=NULL;

    while(fast and fast->next){
        fast=fast->next->next;
        prev=slow;
        slow=slow->next;
    }

    if(prev) prev->next = NULL;
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
