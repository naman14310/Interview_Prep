# Slow and Fast Pointers

<br>

### 1. Middle of a Linked list (slow and fast pointer)

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

<br>

### 2. Linked List Cycle (Floyds cycle detection algo)

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

<br>

### 3. Linked List Cycle II (return the node where the cycle begins)

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

Note: For removing cycle, check for the condition temp1->next==temp2->next. Whenever it occurs, store NULL in temp2->next;

<br>

### 4. Remove Nth Node From End of List

```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    if(!head->next and n==1) return NULL;

    ListNode* slow=head, *fast=head;
    int i=0;
    while(fast and i<n){
        fast = fast->next;
        i++;
    }

    if(!fast) return head->next;    // Boundary case, when first node needs to be deleted

    while(fast->next){
        fast = fast->next;
        slow = slow->next;
    }

    slow->next = slow->next->next;
    return head;
}
```

<br>


### 5. Swapping Nodes in a Linked List
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
