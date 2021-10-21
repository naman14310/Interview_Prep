# String

Some important string functions:

1. [find()](https://www.geeksforgeeks.org/string-find-in-cpp/) - find subtring in given string
2. [Tokenization](https://www.geeksforgeeks.org/tokenizing-a-string-cpp/) - same as python split()
3. [iswalnum()](https://www.geeksforgeeks.org/iswalnum-function-in-c-stl/) - checks if the given wide character is an alphanumeric character or not
4. [isalpha() and isdigit()](https://www.tutorialspoint.com/isalpha-and-isdigit-in-c-cplusplus) - check that a character is an alphabet or digit
5. [transform()](https://www.programiz.com/cpp-programming/library-function/cctype/toupper) - performs a transformation on given array/string.

<br>



## @ Standard Problems

### 1. Tokenize string (stringstream)

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

### 2. Run Length Encoding

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

### 3. String Normalization
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

### 4. Roman to Integer

```cpp
int romanToInt(string A) {
    unordered_map<char, int> mp = {{'I',1}, {'V', 5}, {'X', 10}, {'L', 50}, {'C', 100}, {'D', 500}, {'M', 1000}};
    int ans = 0;
    int prev = INT_MAX;

    for(int i=0; i<A.size(); i++){
        char ch = A[i];

        if(prev<mp[ch])
            ans += mp[ch] - (2*prev);
        else
            ans += mp[ch];

        prev = mp[ch];
    }

    return ans;
}
```

<br>

### 5. Integer to Roman

```cpp
string Solution::intToRoman(int A) {
    string res = "";

    while(A>=1000){
        res += "M";
        A-=1000;
    }

    while(A>=900){
        res += "CM";
        A-=900;
    }

    while(A>=500){
        res += "D";
        A-=500;
    }

    while(A>=400){
        res += "CD";
        A-=400;
    }

    while(A>=100){
        res += "C";
        A-=100;
    }

    while(A>=90){
        res += "XC";
        A-=90;
    }

    while(A>=50){
        res += "L";
        A-=50;
    }

    while(A>=40){
        res += "XL";
        A-=40;
    }

    while(A>=10){
        res += "X";
        A-=10;
    }

    while(A>=9){
        res += "IX";
        A-=9;
    }

    while(A>=5){
        res += "V";
        A-=5;
    }

    while(A>=4){
        res += "IV";
        A-=4;
    }

    while(A>=1){
        res += "I";
        A-=1;
    }

    return res;
}
```

<br>

### 6. Reverse Words in a String
Return the string A after reversing the string word by word.

Input: s = "the sky is blue"

Output: "blue is sky the"

```cpp
string reverseWords(string s) {
    string res = "";
    string word = "";

    int i=s.length()-1;

    while(i>=0){
        if(s[i]!=' ') word.push_back(s[i]);

        else if(word!=""){
            reverse(word.begin(), word.end());
            res += word + " ";
            word = "";
        }

        i--;
    }

    if(word!=""){
        reverse(word.begin(), word.end());
        res+=word;
    } 

    while(res.length()>0 and res.back()==' ')       // --> Removing extra spaces from back
        res.pop_back();

    return res;
}
```

<br>



## @ Intuitive

### 1. Minimum Add to Make Parentheses Valid
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

### 2. Delete Operation for Two Strings (LCS Variation)
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

### 3. Maximum Product of Word Lengths
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

### 4. Maximum Length of a Concatenated String with Unique Characters
You are given an array of strings arr. A string s is formed by the concatenation of a subsequence of arr that has unique characters. Return the maximum possible length of s.

Input: arr = ["un","iq","ue"]

Output: 4

```cpp
bool isValid (vector<bool> &v1, vector<bool> &v2){    
    for(int i=0; i<26; i++)
        if(v1[i] and v2[i]) return false;

    return true;
}


void combine(vector<bool> &v1, vector<bool>&v2){
    for(int i=0; i<26; i++)
        if(v2[i]) v1[i] = true;    
}


void detach(vector<bool> &v1, vector<bool>&v2){
    for(int i=0; i<26; i++)
        if(v2[i]) v1[i] = false;
}


int solve (vector<vector<bool>> &bitvector, int idx, vector<bool> &v, vector<int> &wordLen){
    if(idx==bitvector.size()) return 0;

    if(isValid(v, bitvector[idx])){
        combine(v, bitvector[idx]);
        int len1 = solve(bitvector, idx+1, v, wordLen) + wordLen[idx];

        detach(v, bitvector[idx]);
        int len2 = solve(bitvector, idx+1, v, wordLen);

        return max(len1, len2);
    }

    else{
        int len2 = solve(bitvector, idx+1, v, wordLen);
        return len2;
    }

}



int maxLength(vector<string>& arr) {
    vector<vector<bool>> bitvector;
    vector<int> wordLen;

    for(string s : arr){
        vector<bool> v (26, false);
        bool flag = false;

        for(char ch : s){
            if(v[ch-'a']){
                flag = true;
                break;
            }

            v[ch-'a'] = true;
        }

        if(!flag){
            bitvector.push_back(v);
            wordLen.push_back(s.length());
        } 
    }

    vector<bool> v (26, false);
    return solve (bitvector, 0, v, wordLen);
}
```

<br>



## @ String Parsing

### 1. String to Integer (atoi)
Implement the myAtoi(string s) function, which converts a string to a 32-bit signed integer (similar to C/C++'s atoi function).

[Algorithm](https://leetcode.com/problems/string-to-integer-atoi/)

```cpp
class Solution {
public:
    int myAtoi(string s) {
        int n = s.length();
        long long num = 0;
        
        bool neg = false;
        bool start = true;
        bool boundary = false;
        
        int i=0;
        
        
        /* Skipping leading white spaces if present */
        
        while(i<n and s[i]==' ')
            i++;
        
        
        /* Checking for negative sign if present */
        
        if(i<n and s[i]=='-'){
            neg = true;
            i++;
        }
        
        
        /* Skipping positive sign if present */
        
        else if(i<n and s[i]=='+') i++;
        
        
        /* processing digits one by one */
        
        while(i<n and isdigit(s[i])){
            
            if(start and s[i]=='0'){
                i++;
                continue;
            }
            
            start = false;
            num = num*10 + s[i]-'0'; 
            
            if(num>INT_MAX){
                boundary = true;
                break;
            }
            i++;
        }
        
        if(boundary){
            if(neg) return INT_MIN;
            else return INT_MAX;
        }
        else{
            if(neg) return num*-1;
            else return num;
        }
        
    }
};
```

<br>

### 2. Compare Version Numbers
Compare two version numbers version1 and version2.
1. If version1 > version2 return 1,
2. If version1 < version2 return -1,
3. otherwise return 0.

Here is an example of version numbers ordering:

0.1 < 1.1 < 1.2 < 1.13 < 1.13.4

```cpp
int isGreater (string & s1, string & s2){
    if(s1.length()>s2.length()) return 1;
    if(s1.length()<s2.length()) return -1;
 
    for(int i=0; i<s1.length(); i++){
        if(s1[i]>s2[i]) return 1;
        if(s1[i]<s2[i]) return -1;
    }
 
    return 0;
}
 
int Solution::compareVersion(string A, string B) {
    int i=0 , j=0;
 
    while(i<A.length() and j<B.length()){
 
        string s1 = "", s2 = "";
        bool start = false;
 
        while(A[i]!='.' and i<A.length()){
            if (!start and A[i]=='0'){i++; continue;}         // --> remove leading zeros
            s1.push_back(A[i]);
            i++;
            start = true;
        }
 
        i++;
        start = false;
 
        while(B[j]!='.' and j<B.length()){
            if (!start and B[j]=='0'){j++; continue;}          // --> remove leading zeros
            s2.push_back(B[j]);
            j++;
            start = true;
        }
 
        j++;
        
        int comp = isGreater(s1, s2);
        if(comp==1) return 1;
        if(comp==-1) return -1;
    }
 
    while(i<A.length()){
        if(A[i]!='0' and A[i]!='.') return 1;
        i++;
    }
 
    while(j<B.length()){
        if(B[j]!='0' and B[j]!='.') return -1;
        j++;
    }
 
    return 0;
}
```
