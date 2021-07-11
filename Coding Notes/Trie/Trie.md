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

#### 2. Search Suggestions System
Given an array of strings products and a string searchWord. We want to design a system that suggests at most three product names from products after each character of searchWord is typed. Suggested products should have common prefix with the searchWord. If there are more than three products with a common prefix return the three lexicographically minimums products. Return list of lists of the suggested products after each character of searchWord is typed. 

```cpp
struct TrieNode{
    vector<TrieNode*> children;
    bool isTerminal;

    TrieNode(){
        children = vector<TrieNode*> (26, NULL);
        isTerminal = false;
    }
};


void insert (TrieNode* root, string word){
    TrieNode* temp = root;

    for(char ch : word){
        int idx = ch-'a';

        if(!temp->children[idx])
            temp->children[idx] = new TrieNode();

        temp = temp->children[idx];
    }

    temp->isTerminal = true;
} 


void fetch_words (TrieNode* temp, int & count, vector<string> & suggestions, string & s){        
    if(count==3) return;           // --> we will not do further dfs after finding 3 words

    if(temp->isTerminal){
        suggestions.push_back(s);
        count++;
    }

    for(int i=0; i<26; i++){
        if(temp->children[i]){
            s.push_back('a' + i);
            fetch_words (temp->children[i], count, suggestions, s);
            s.pop_back();
        }
    }
}


vector<vector<string>> give_suggestion (TrieNode* root, string word){
    TrieNode* temp = root;
    vector<vector<string>> res;
    string s = "";

    for(char ch : word){
        int idx = ch - 'a';

        if(!temp->children[idx]) break;

        temp = temp->children[idx];
        s.push_back(ch);

        vector<string> suggestions;
        int count = 0;

        fetch_words(temp, count, suggestions, s);
        res.push_back(suggestions);
    }

    /* adding empty vectors to res if no suggestions found */

    while(res.size()!=word.size())
        res.push_back(vector<string> ());

    return res;
}


vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
    TrieNode* root = new TrieNode();

    for(string word : products)
        insert(root, word);

    return give_suggestion (root, searchWord);
}
```

#### 4. Maximum XOR of Two Numbers in an Array
Given an integer array nums, return the maximum result of nums[i] XOR nums[j], where 0 <= i <= j < n.

```cpp
struct TrieNode{
    vector<TrieNode*> children;
    bool isTerminal;

    TrieNode(){
        children = vector<TrieNode*> (2, NULL);
        isTerminal = false;
    }
};


void insert (TrieNode* root, int num){
    TrieNode* temp = root;

    for(int i=31; i>=0; i--){
        int idx = (num>>i) & 1;          //--> checking ith bit of num

        if(!temp->children[idx])
            temp->children[idx] = new TrieNode();

        temp = temp->children[idx];
    }

    temp->isTerminal = true;
}


int find_max_xor (TrieNode* root, int num){
    TrieNode* temp = root;
    int ans = 0;

    for(int i=31; i>=0; i--){
        int idx = (num>>i) & 1;

        /* For making max_xor with num, first preference will be given to opposite bit */

        if(temp->children[!idx]){
            ans = ans | (1<<i);             //--> Setting ith bit of answer to 1
            temp = temp->children[!idx];
        }      

        else temp = temp->children[idx];
    }

    return ans;
}


int findMaximumXOR(vector<int>& nums) {
    TrieNode* root = new TrieNode();
    int XOR = 0;

    for(int n : nums)
        insert(root, n);

    for(int n : nums)
        XOR = max(XOR, find_max_xor(root, n));

    return XOR;
}
```
