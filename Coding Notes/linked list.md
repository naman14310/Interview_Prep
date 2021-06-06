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

#### 9. Palindrome Linked List

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
