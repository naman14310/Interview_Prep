# Pattern Matching

<br>


## @ Find Method

### 1. Search All
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

### 2. Rotate String (Smart Solution)
A shift on s consists of taking string s and moving the leftmost character to the rightmost position. For example, if s = 'abcde', then it will be 'bcdea' after one shift on s. Return true if and only if s can become goal after some number of shifts on s.

Hint: Append s two times (s+s). If we can find goal as a substring in s then return true;

```cpp
bool rotateString(string s, string goal) {
    if (s.length()!=goal.length()) return false; 
    s += s;
    return s.find(goal)!=-1;
}
```

<br>

### 3. Repeated Substring Pattern (Tricky)
Given a string s, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

Input: s = "abab"

Output: true

Hint:  If we can find a rotation m which is larger than 0 and less than len(s) then the input string consists of repeated substrings.

```cpp
bool repeatedSubstringPattern(string s) {
    if(s.length()<=1) return false;

    string s2 = s+s;
    int idx = s2.find(s, 1);

    return idx<s.length(); 
}
```

<br>


## @ KMP Algo

[KMP Explaination](https://www.youtube.com/watch?v=4jY57Ehc14Y)

<br>

### 1. Longest Prefix Suffix
Given a string of characters, find the length of the longest proper prefix which is also a proper suffix.

Input: s = "abab"

Output: 2

**Approach for Filling LPS array**
1. Initialise two pointers i=0 and j=1. j will points to the character we are currently processing and i will points to prev matched prefix.
2. If s[i]==s[j]: 
        lps[j] = i+1 and increment both i and j
3. Else: 
        decrement i till i>0 and s[i]!=s[j]

```cpp
int lps(string s) {
    int n = s.length();
    vector<int> lps (n, 0);

    int i=0, j=1;

    while(j<n){

        if(s[i]==s[j]){
            lps[j] = i+1;
            i++; j++;
        }
        
        else{

            /*
                Instead of moving i one by one in backward direction
                we can directly jump it to lps[i-1];

                Previous code
                -------------

                while(i>0 and s[i]!=s[j])
                    i--;

                if(i==0 and s[i]!=s[j])
                    j++;
            */

            if(i>0) 
                i = lps[i-1];
            else 
                j++;
        }
    }

    return lps.back();
}
```

<br>

### 2. Substring Match (kmp)
Return the index of the first occurrence of pattern p in a given string s.

```cpp
/* Return: Longest Prefix Suffix vector */

vector<int> compute_LPS (string &p){
    vector<int> lps (p.size(), 0);
    int i=0, j=1;

    while(j<p.size()){

        if(p[i]==p[j]){
            lps[j] = i+1;
            i++; j++;    
        }
        else{

            if(i>0) 
                i = lps[i-1];
            else 
                j++;
        }
    }

    return lps;
}


int KMP (string s, string p, vector<int> & lps){

    /* i will iterate over s and j will iterate over p */

    int i=0, j=0;

    while(i<s.length() and j<p.length()){

        /* If both cur_i and cur_j matches, increment i and j */

        if(s[i]==p[j]){
            i++; j++;
        }

        /* Else shift j to lps[j-1] if j>0 or simply increment i if j==0 */

        else{

            if(j==0)
                i++;
            else
                j = lps[j-1];
        }
    }


    /* If whole pattern get matched return i-len(p) for getting start index of pattern */

    if (j==p.length())
        return i-p.length();
    else 
        return -1;
}



int strStr(string s, string p) {
    vector<int> lps = compute_LPS (p);
    return KMP (s, p, lps);
}
```

<br>

### 3. Minimum Characters required to make a String Palindromic (Tricky)
Given an string A. The only operation allowed is to insert  characters in the beginning of the string. Find how many minimum characters are needed to be inserted to make the string a palindrome string.

Input: A = "AACECAAAA"

Output: 2

<br>

**Approach 1: Naive Algo - O(n2)**

*Min insertion at front == Min deletion from back*

Hence, one by one pop chars from back at chk if string is palindrome or not.

```cpp
bool isPalindrome (string &s){
    int i=0, j=s.length()-1;
    while(i<j){
        if(s[i]!=s[j]) return false;
        i++; j--;
    }
    return true;
}
 
 
int Solution::solve(string A) {
    int n = A.length();
    if(isPalindrome(A)) return 0;
    
    int cnt=0;

    while(A.length()>0){
        A.pop_back();
        cnt++;

        if(isPalindrome(A))
            return cnt;
    }
 
    return n-1;
}
```

**Approach 2: Using LPS array - O(n)**

[Explaination](https://www.geeksforgeeks.org/minimum-characters-added-front-make-string-palindrome/)

Hint: We are only interested in the last value of this lps array because it shows us the largest suffix of the reversed string that matches the prefix of the original string.

Approach:
1. Append rev of same string with one seperator i.e.  s = s + "#" + rev(s)
2. Find lps array of the newly formed string.
3. Our answer will be n-lcs.back(), where n is the length of original string.

```cpp
vector<int> compute_lps(string s){
    int n = s.length();
    vector<int> lps (n, 0);
    int i=0, j=1;

    while(j<n){

        if(s[i]==s[j]){
            lps[j] = i+1;
            i++; j++;
        }
        else{
            if(i>0) i = lps[i-1];
            else j++;
        }
    }

    return lps;
}


int Solution::solve(string A) {
    int n = A.length();
    
    string rev = A;
    reverse(rev.begin(), rev.end());

    A += "#" + rev;   // --> append rev of same string with one seprator

    vector<int> lps = compute_lps(A);
    return n-lps.back();
}
```
