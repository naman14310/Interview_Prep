# String

Some important string functions:

1. [find()](https://www.geeksforgeeks.org/string-find-in-cpp/) - find subtring in given string
2. [Tokenization](https://www.geeksforgeeks.org/tokenizing-a-string-cpp/) - same as python split()
3. [iswalnum()](https://www.geeksforgeeks.org/iswalnum-function-in-c-stl/) - checks if the given wide character is an alphanumeric character or not
4. [isalpha() and isdigit()](https://www.tutorialspoint.com/isalpha-and-isdigit-in-c-cplusplus) - check that a character is an alphabet or digit
5. [transform()](https://www.programiz.com/cpp-programming/library-function/cctype/toupper) - performs a transformation on given array/string.

<br>

## Easy

#### 1. Search All (Find method)
Implement a function that return all occurances of subtring in a given string

```cpp
vector<int> stringSearch(string big,string small){
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

<br>

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

<br>

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

<br>

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

<br>

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

<br>

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

<br>

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

<br>

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

<br>

#### 9. Rotate String (Smart Solution)
We are given two strings, s and goal. A shift on s consists of taking string s and moving the leftmost character to the rightmost position. For example, if s = 'abcde', then it will be 'bcdea' after one shift on s. Return true if and only if s can become goal after some number of shifts on s.

```cpp
bool rotateString(string s, string goal) {
    if (s.length()!=goal.length()) return false; 
    s += s;
    return s.find(goal)!=-1;
}
```

<br>

#### 10. Repeated Substring Pattern (Tricky)
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

<br>

#### 11. Longest Palindrome
Given a string s which consists of lowercase or uppercase letters, return the length of the longest palindrome that can be built with those letters.

Input: s = "abccccdd"

Output: 7

Hint: Count how many letters appear an odd number of times. Because we can use all letters, except for each odd-count letter we must leave one, except one of them we can use.

```cpp
int longestPalindrome(string s) {

    unordered_map<char, int> mp;
    int oddFreq = 0;

    for(char ch : s)
        mp[ch]++;

    /* Count all char with odd freq */

    for(auto p : mp)
        if(p.second%2 != 0) oddFreq++;

    if(oddFreq>0) oddFreq--;        // --> becoz we can use one single char at middle of the palindrome

    return s.length()-oddFreq;
}
```

<br>

#### 12. Construct K Palindrome Strings
Given a string s and an integer k. You should construct k non-empty palindrome strings using all the characters in s. Return True if you can use all the characters in s to construct k palindrome strings or False otherwise.

Input: s = "aabababccdsb", k = 3

Output: true

Hint: Just cnt number of char with odd freq.

```cpp
bool canConstruct(string s, int k) {
    unordered_map<char, int> mp;
    for(char ch : s)
        mp[ch]++;

    int odd_cnt = 0;
    
    for(auto p : mp)
        if(p.second % 2 != 0) odd_cnt++;

    return odd_cnt<=k and s.length()>=k;
}
```

<br>

## Medium

#### 1. Minimum Add to Make Parentheses Valid
You are given a parentheses string s. In one move, you can insert a parenthesis at any position of the string. Return the minimum number of moves required to make s valid.

Input: s = "()))(("

Output: 4

**Two Pass solution - O(N) | O(1)**

```cpp
int minAddToMakeValid(string s) {
    int n = s.length();
    int bcnt = 0;
    int moves = 0;

    /* Iterate from left to right and cnt extra ')' brackets */

    for(int i=0; i<n; i++){

        if(s[i]=='(') bcnt++;

        if(s[i]==')'){
            if(bcnt>0) bcnt--;
            else moves++;
        }
    }

    bcnt = 0;

    /* Iterate from right to left and cnt extra '(' brackets */

    for(int i=n-1; i>=0; i--){

        if(s[i]==')') bcnt++;

        if(s[i]=='('){
            if(bcnt>0) bcnt--;
            else moves++;
        }
    }

    return moves;
}
```

**One Pass Solution - O(N) | O(1)**

```cpp
int minAddToMakeValid(string s) {
    int n = s.length();
    int left = 0, right = 0;

    for(int i=0; i<n; i++){

        if(s[i]=='(') right++;

        if(s[i]==')'){
            if(right>0) right--;
            else left++;
        }
    }

    return left+right;
}
```

<br>

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

<br>

#### 3. Maximum Product of Word Lengths
Given a string array words, return the maximum value of length(word[i]) * length(word[j]) where the two words do not share common letters. If no such two words exist, return 0.

Input: words = ["abcw","baz","foo","bar","xtfn","abcdef"]

Output: 16

Hint: Use bitmasking for checking common letters in two words.

```cpp
bool check(vector<bool> & v1, vector<bool> & v2){
    for(int i=0; i<26; i++){
        if(v1[i] && v2[i]) return false;
    }
    return true;
}    

int maxProduct(vector<string>& words) {
    vector<vector<bool>> v;         // --> It contains bitmask for every word
    unsigned long ans = 0;

    for(auto w : words){
        vector<bool> temp (26, false);
        for(auto ch : w)
            temp[ch-'a'] = true;
        
        v.push_back(temp);
    }

    for(int i=0; i<words.size(); i++){
        for(int j=i+1; j<words.size(); j++){
            if(check(v[i], v[j]))
                ans = max(ans, words[i].size()*words[j].size());
        }
    }

    return ans;
}
```

<br>

## Hard

#### 1. Orderly Queue
You are given a string s and an integer k. You can choose one of the first k letters of s and append it at the end of the string. Return the lexicographically smallest string you could have after applying the mentioned step any number of moves.

Input: s = "cba", k = 1

Output: "acb"

[Explaination](https://leetcode.com/problems/orderly-queue/discuss/165878/C%2B%2BJavaPython-Sort-String-or-Rotate-String)

```cpp
string orderlyQueue(string s, int k) {

    /* If k>1, simply return sorted string */

    if(k>1){
        sort(s.begin(), s.end());
        return s;
    }

    /* 
        Else if k==1, return smallest of all n-1 permutations that can be formed by
        poping one element from front and pushing it to back
    */

    deque<char> dq (s.begin(), s.end());
    string ans = s;

    for(int i=0; i<s.length()-1; i++){

        char ch = dq.front(); dq.pop_front();
        dq.push_back(ch);

        string temp (dq.begin(), dq.end());
        if(temp<ans) ans = temp;
    }

    return ans;
}
```
