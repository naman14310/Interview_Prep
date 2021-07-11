# Trie 
Trie is an efficient information reTrieval data structure. 

**Applications of Trie:**

1. Using Trie, search complexities can be brought to optimal limit (key length).
2. We can easily print all words in alphabetical order which is not easily possible with hashing.
3. We can efficiently do prefix search (or Auto-complete) with Trie.

**Issues with Trie:**

The main disadvantage of tries is that they need a lot of memory for storing the strings.

#### 1. Implement Trie (Prefix Tree)

```cpp
class Trie {
public:
    
    struct TrieNode{
        vector<TrieNode*> children;
        bool isTerminal;
        
        TrieNode(){
            children = vector<TrieNode*> (26, NULL);    
            isTerminal = false;
        }
    };
    
    TrieNode* root;
    
    Trie(){
        root = new TrieNode();
    }
    
    /* ------------------------ Inserts a word into the trie ------------------------- */
    
    void insert(string word) {
        TrieNode* temp = root;
        
        for(char ch : word){
            int idx = ch-'a';
            
            if(!temp->children[idx])
                temp->children[idx] = new TrieNode();
            
            temp = temp->children[idx];
        }
        
        temp->isTerminal = true;
    }
    
    /* ----------------------- Returns if the word is in the trie --------------------- */
    
    bool search(string word) {
        TrieNode* temp = root;
        
        for(char ch : word){
            int idx = ch-'a';
            
            if(!temp->children[idx])
                return false;
            
            temp = temp->children[idx];
        }
        
        return temp->isTerminal;
    }
    
    /* -------- Returns true if there is any word in the trie that starts with the given prefix -------- */
    
    bool startsWith(string prefix) {
        TrieNode* temp = root;
        
        for(char ch : prefix){
            int idx = ch-'a';
            
            if(!temp->children[idx])
                return false;
            
            temp = temp->children[idx];
        }
        
        return true;
    }
};
```
