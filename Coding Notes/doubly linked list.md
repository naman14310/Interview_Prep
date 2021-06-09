# Doubly Linked list

#### 1. Reverse a Doubly Linked List 

Hint: Use two pointers, curr and next (prev is not required)

```cpp
Node* reverseDLL(Node * head){
    Node* curr = head, *next = NULL;
    
    while(curr->next){
        next = curr->next;
        curr->next = curr->prev;
        curr->prev = next;
        curr = next;
    }
    
    curr->next = curr->prev;
    curr->prev = NULL;
    
    return curr;
}
```
