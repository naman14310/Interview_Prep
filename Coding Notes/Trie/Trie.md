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

#### 3. Replace Words
Given a dictionary consisting of many roots and a sentence consisting of words separated by spaces, replace all the successors in the sentence with the root forming it. If a successor can be replaced by more than one root, replace it with the root that has the shortest length. Return the sentence after the replacement.

Input: dictionary = ["cat","bat","rat"],  sentence = "the cattle was rattled by the battery"

Output: "the cat was rat by the bat"

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
    TrieNode* temp  = root;

    for(auto ch : word){
        int idx = ch - 'a';

        if(!temp->children[idx])
            temp->children[idx] = new TrieNode();

        temp = temp->children[idx];
    }

    temp->isTerminal = true;
}


string find_rootWord (TrieNode* root, string word){
    TrieNode* temp = root;
    string rootWord = "";

    for(auto ch : word){
        int idx = ch - 'a';

        if(!temp->children[idx])
            return word;

        temp = temp->children[idx];
        rootWord.push_back(ch);

        if(temp->isTerminal)
            return rootWord;
    }

    return rootWord;
}


string replaceWords(vector<string>& dictionary, string sentence) {
    string res = "";
    TrieNode* root = new TrieNode();

    stringstream ss(sentence);  
    string token;
    vector<string> tokens;

    while(getline(ss, token, ' '))
        tokens.push_back(token);

    for(string word : dictionary)
        insert(root, word);

    for(int i=0; i<tokens.size()-1; i++)
        res += find_rootWord(root, tokens[i]) + " ";

    res += find_rootWord(root, tokens.back());
    return res;
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

#### 5. Design Add and Search Words Data Structure
Design a data structure that supports adding new words and finding if a string matches any previously added string. Implement the WordDictionary class in which:

1. WordDictionary() : Initializes the object.
2. void addWord(word) : Adds word to the data structure, it can be matched later.
3. bool search(word) : Returns true if there is any string in the data structure that matches word or false otherwise. word may contain dots '.' where dots can be matched with any letter.

```cpp
struct TrieNode{
    vector<TrieNode*> children;
    bool isTerminal;
    TrieNode (){
        children = vector<TrieNode*> (26, NULL);
        isTerminal = false;
    }
};

TrieNode* root;

WordDictionary() {
    root = new TrieNode();
}

/* --------------------------------------------------------------------------------------- */

void addWord(string word) {
    TrieNode* temp = root;

    for(char ch : word){
        int idx = ch - 'a';

        if(!temp->children[idx])
            temp->children[idx] = new TrieNode();

        temp = temp->children[idx];
    }

    temp->isTerminal = true;
}

/* --------------------------------------------------------------------------------------- */


bool search_helper(string word, int i, TrieNode* temp){
    if(i==word.size()) return temp->isTerminal;

    /* If we get dot at any index i of word, then we will check further in all 26 children */

    if(word[i]=='.'){
        bool found = false;

        for(int j=0; j<26; j++){
            if(temp->children[j]){
                found = search_helper(word, i+1, temp->children[j]);
                if(found) return true;
            }
        }
        return false;
    }

    int idx = word[i]-'a';

    if(temp->children[idx])
        return search_helper(word, i+1, temp->children[idx]);

    return false;
}

bool search(string word) {
    TrieNode* temp = root;
    return search_helper(word, 0, temp);
}
```

#### 6. Stream of Characters
Implement the StreamChecker class as follows:

1. StreamChecker(words): Constructor, init the data structure with the given words.
2. query(letter): returns true if and only if for some k >= 1, the last k characters queried (in order from oldest to newest, including this letter just queried) spell one of the words in the given list.

```cpp
class StreamChecker {
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
    string s;
    
    
    /* Insert all words in Trie in reverse order */
    
    void insert(string word){
        TrieNode* temp = root;
        
        for(int i=word.length()-1; i>=0; i--){
            char ch = word[i];
            int idx = ch-'a';
            
            if(!temp->children[idx])
                temp->children[idx] = new TrieNode();
            
            temp = temp->children[idx];
        }
        
        temp->isTerminal = true;
    }
    
    StreamChecker(vector<string>& words) {
        root = new TrieNode();
        for(string word : words)
            insert(word);
    }
    
    bool query(char letter) {
        s.push_back(letter);
        TrieNode* temp = root;

        for(int i=s.length()-1; i>=0; i--){
            char ch = s[i];
            int idx = ch-'a';
            
            if(temp->children[idx])
                temp = temp->children[idx];
            
            else break;
            
            if(temp->isTerminal) return true;
        }
        
        return false;
    }
};

/**
 * Your StreamChecker object will be instantiated and called as such:
 * StreamChecker* obj = new StreamChecker(words);
 * bool param_1 = obj->query(letter);
 */
```
