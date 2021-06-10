# Doubly Linked list

#### 1. Implement doubly linkedlist and create a Header file 

```cpp
#include<bits/stdc++.h>
using namespace std;

template <typename T>
class ListNode{

    public:

        T val;
        ListNode *next;
        ListNode *prev;

        ListNode(T val){
            this->val = val;
            this->next = NULL;
            this->prev = NULL; 
        }
};

template <typename T>
class doubly_linkedlist{

    ListNode<T> * head;

    public:

        doubly_linkedlist(){
            head = NULL;
        }

        void insert_at_begin(T val){
            ListNode<T> *node = new ListNode<T>(val);
            
            if(!head) head = node;
            else{
                node->next = head;
                head->prev = node;
                head = node;
            }
        }

        void insert_at_end(T val){
            ListNode<T> *node = new ListNode<T>(val);

            if(!head) head = node;
            else{
                ListNode<T> *temp = head;
                while(temp->next)
                    temp = temp->next;
                
                node->prev = temp;
                temp->next = node;
            }
        }

        void insert_at_pos(T val, int pos){
            if(pos==1){
                insert_at_begin(val);
                return;
            }

            ListNode<T>* node = new ListNode<T>(val);
            ListNode<T>* temp = head;
            int cnt = 1;

            while(temp and cnt<pos-1){
                temp = temp->next;
                cnt++;
            }

            if(!temp){
                insert_at_end(val);
                return;
            }

            node->next = temp->next;
            node->prev = temp;
            temp->next->prev = node;
            temp->next = node;
        }

        void delete_from_begin(){
            if(!head) return;
            ListNode<T>* p = head;
            head = head->next;
            head->prev = NULL;
            delete p; 
        }

        void delete_from_end(){
            if(!head) return;
            else if(!head->next) head = NULL;
            else{
                ListNode<T>* temp = head;
                while(temp->next->next)
                    temp = temp->next;

                ListNode<T> *p = temp->next;
                temp->next = NULL;
                delete p;
            }
        }

        void delete_from_pos(int pos){
            
            if(pos==1) delete_from_begin();
            else{
                int cnt = 1;
                ListNode<T> *temp = head;

                while(temp->next->next and cnt<pos-1){
                    temp = temp->next;
                    cnt++;
                }

                if(!temp->next->next){
                    delete_from_end();
                    return;
                }

                ListNode<T>* p = temp->next;
                temp->next = temp->next->next;
                temp->next->prev = temp;
                delete p;
            }
        }

        void traverse_forward(ListNode<int>* startNode){
            ListNode<T>* temp = head;
            while(temp){
                cout<<temp->val<<" --> ";
                temp = temp->next;
            }
            cout<<"NULL\n";
        }

        void traverse_cyclic(ListNode<int>* startNode){
            ListNode<T>* temp = startNode;
            while(temp){
                cout<<temp->val<<" --> ";
                if(!temp->next) break;
                else temp = temp->next;
            }

            while(temp){
                cout<<temp->val<<" --> ";
                temp = temp->prev;
            }
            cout<<"NULL\n";
        }

        ListNode<int>* get_head(){
            return head;
        } 
};

```

#### 2. Reverse a Doubly Linked List 

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

#### 3. Reverse a doubly linked list in groups of given size k

Approach:
1. First reverse only next pointers of doubly linked list
2. Then traverse whole linked list using next pointers and reset the prev pointers accordingly



```cpp
#include<iostream>
#include "doubly_linkedlist.h"      // --> for including header files from current folder
using namespace std;


ListNode<int>* reverse_next_pointers(ListNode<int> *head, int k) {
    if(!head or !head->next) return head;

    int cnt = 1;
    ListNode<int>* curr=head, *next=NULL;

    while(curr->next and cnt<k){
        next = curr->next;
        curr->next = curr->prev;
        curr = next;
        cnt++;
    }

    next = curr ? curr->next : NULL;
    curr->next = curr->prev;

    head->next = reverse_next_pointers(next, k);

    head = curr;
    return head;
}


void reset_prev_pointers(ListNode<int>* head){
    ListNode<int>* curr=head, *prev=NULL;
    while(curr){
        curr->prev = prev;
        prev = curr;
        curr = curr->next;
    }
}


ListNode<int>* reverse_in_k_group(ListNode<int>* head, int k){
    head = reverse_next_pointers(head, k);
    reset_prev_pointers(head);
    return head;
}


int main(){
    doubly_linkedlist<int> dll;
    int n, k;
    cin>>n>>k;

    for(int i=0; i<n; i++){
        int val; cin>>val;
        dll.insert_at_end(val);
    }

    ListNode<int>* head = dll.get_head();

    cout<<"\nBefore\n";
    cout<<"-------\n\n";
    dll.traverse_cyclic(head);

    head = reverse_in_k_group(head, k);

    cout<<"\nAfter\n";
    cout<<"-----\n\n";
    dll.traverse_cyclic(head);
    
    return 0;
}
```
