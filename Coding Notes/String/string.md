# String

## Easy

#### 1. Search All (Find method)
Implement a function that return all occurances of subtring in a given string

```cpp
vector<int> stringSearch(string big,string small){
    //store the occurrences in the result vector
    vector<int> result;
    int index = 0;
    while(true){
        index = big.find(small, index);
        if(index==-1) break;
        
        result.push_back(index);
        index++;
    }
    return result;
}
```

#### 2. Tokenize string (stringstream)

```cpp
int main(){
  string input;
  getline(cin, input);
  
  stringstream ss(input);
  
  string token;
  vector<string> tokens;
  
  while(getline(ss, token, ' '){
      tokens.push_back(token);
  }
  
  for(auto tkn : tokens) cout<<tkn<<endl;
  return 0;
}
```

#### 3. Run Length Encoding

```cpp
string compressString(const string &str){   
  string res = "";
  res.push_back(str[0]);
  int i=1;
  int count=1;
  char run = str[0]; 
  
  while(i<str.length()){
      if(str[i]!=run){
          res+=to_string(count);
          count=0;
          run = str[i];
          res.push_back(run);
      }
      count++;
      i++;
  }

  res+=to_string(count);
  if(res.length()>=str.length()) return str;
  return res;
}
```

#### 4. String Normalization
Convert given string to Camelcase

```cpp
string normalize(const string &sentence) {
    string copy = sentence;
    string res = "";
    
    for(int i=0; i<copy.length(); i++){
        if(i==0){
            if(isalpha(copy[i]))
                res.push_back(toupper(copy[i]));
            else
                res.push_back(copy[i]);
        }
        else{
            if(copy[i-1]==' '){
                if(isalpha(copy[i])) res.push_back(toupper(copy[i]));
                else res.push_back(copy[i]);
            }
            else
                res.push_back(tolower(copy[i]));
        }
    }    
    return res;
}
```

#### 5. Palindrome break
Convert a given palindromic string containing characters a and b to non palindrome (which is lexicographically smallest) by replacing only one character.

```cpp
string breakPalindrome(string s) {
  int n = s.length();    
  if(n<=1) return "";

  for(int i=0; i<n/2; i++){
      if(s[i]!='a'){
          s[i] = 'a';
          return s;
      }
  }

  s[n-1] = 'b';
  return s;    
}
```

#### 6. Remove Palindromic Subsequences (Tricky)
You are given a string s consisting only of letters 'a' and 'b'. In a single step you can remove one palindromic subsequence from s. Return the minimum number of steps to make the given string empty.

```cpp
bool isPalindrome(string s){
    int i=0, j=s.length()-1;
    while(i<j){
        if(s[i]!=s[j]) return false;
        i++;
        j--;
    }
    return true;

}

int removePalindromeSub(string s) {
    if(s.length()==0) return 0;
    if(isPalindrome(s)){
        return 1;    
    }
    else return 2;
}
```

#### 7. Count Binary Substrings
Give a binary string s, return the number of non-empty substrings that have the same number of 0's and 1's, and all the 0's and all the 1's in these substrings are grouped consecutively. Substrings that occur multiple times are counted the number of times they occur.

```cpp
int countBinarySubstrings(string s) {
    int i = 1;        
    int left = 1, right=0;

    bool flip = false;
    int ans = 0;

    while(i<s.length()){
        if(s[i]!=s[i-1]){
            if(!flip){
                right++;
                flip = true;
            }
            else{
                ans += min(left, right);
                left = right;
                right = 1;
            }
        }

        else if(flip){
            right++;
        }
        else{
            left++;
        }

        i++;
    }
    ans += min(left, right);
    return ans;
}
```

#### 8. Roman to Integer

```cpp
int romanToInt(string s) {
    unordered_map<char, int> mp = {{'I',1},{'V',5},{'X',10},{'L',50},{'C',100},{'D',500},{'M',1000}};
    int sum = 0;
    int i=0;
    
    while(i<s.length()){

        if(s[i]=='I'){
            if(i+1<s.length() && s[i+1]=='V'){
                sum+=4;
                i+=2;
            }
            else if(i+1<s.length() && s[i+1]=='X'){
                sum+=9;
                i+=2;
            }
            else{
                sum+=1;
                i+=1;
            }
        }

        else if(s[i]=='X'){
            if(i+1<s.length() && s[i+1]=='L'){
                sum+=40;
                i+=2;
            }
            else if(i+1<s.length() && s[i+1]=='C'){
                sum+=90;
                i+=2;
            }
            else{
                sum+=10;
                i+=1;
            }
        }

        else if(s[i]=='C'){
            if(i+1<s.length() && s[i+1]=='D'){
                sum+=400;
                i+=2;
            }
            else if(i+1<s.length() && s[i+1]=='M'){
                sum+=900;
                i+=2;
            }
            else{
                sum+=100;
                i+=1;
            }
        }

        else{
            sum+=mp[s[i]];
            i+=1;
        }
    }
    return sum;
}
```

#### 9. Longest Common Prefix

```cpp
string longestCommonPrefix(vector<string>& strs) {
    if(strs.size()<1) return "";
    
    string ans = "";
    bool flag = false;
    
    for(int i=0; i<=200; i++){
        string temp = ans;

        if(i<strs[0].length())
        temp.push_back(strs[0][i]);

        for(auto s : strs){
            if(i>=s.length()){
                flag=true;
                break;
            } 
            else if(s[i]!=temp[i]){
                flag=true;
                break;
            } 
        }

        if(flag) break;
        ans = temp;
    }
    return ans;
}
```

#### 10. Rotate String (Smart Solution)
We are given two strings, s and goal. A shift on s consists of taking string s and moving the leftmost character to the rightmost position. For example, if s = 'abcde', then it will be 'bcdea' after one shift on s. Return true if and only if s can become goal after some number of shifts on s.

```cpp
bool rotateString(string s, string goal) {
    if (s.length()!=goal.length()) return false; 
    s += s;
    return s.find(goal)!=-1;
}
```

#### 11. Repeated Substring Pattern (Tricky)
Given a string s, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

Input: s = "abab"

Output: true

Approach:  If we can find a rotation m which is larger than 0 and less than n then the input string consists of repeated substrings.

```cpp
bool repeatedSubstringPattern(string s) {
    if(s.length()<=1) return false;

    string s2 = s+s;
    int idx = s2.find(s, 1);

    return idx<s.length(); 
}
```

## Medium

#### 1. Group Anagrams (Using Hashmap)
Given an array of strings strs, group the anagrams together. You can return the answer in any order.

Hint: Use sorted words to make key for hashmap

```cpp
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> mp;
    for(int i=0; i<strs.size(); i++){
        string s = strs[i];

        string srt = s;
        sort(srt.begin(), srt.end());

        mp[srt].push_back(s);
    }
    vector<vector<string>> res;
    for(auto p : mp){
        res.push_back(p.second);
    }
    return res;
}
```

#### 2. Delete Operation for Two Strings (LCS Variation)
Given two strings word1 and word2, return the minimum number of steps required to make word1 and word2 the same.

```cpp
int find_lcs(string s1, string s2){
    int row = s1.length()+1;
    int col = s2.length()+1;
    vector<vector<int>> dp (row, vector<int> (col, 0));

    for(int i=1; i<row; i++){
        for(int j=1; j<col; j++){
            if(s1[i-1]==s2[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
            else dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    return dp[row-1][col-1];
}

int minDistance(string word1, string word2) {
    int lcs = find_lcs(word1, word2);        
    return (word1.length()-lcs)+(word2.length()-lcs);
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
