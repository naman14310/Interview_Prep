# String Tricks

<br>

### 1. Palindrome break
Convert given palindromic string containing only a and b to non palindrome (lexicographically smallest) by replacing only one character.

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

### 2. Remove Palindromic Subsequences (Tricky)
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

### 3. Longest Palindrome
Given a string s consists of lowercase or uppercase letters, return the length of the longest palindrome that can be built with those letters.

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

### 4. Construct K Palindrome Strings
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

### 5. Count minimum swap to make string palindrome
Given a string s, the task is to find out the minimum no of adjacent swaps required to make string s palindrome. If not possible, return -1.

Input: aabcb

Output: 3 

```cpp
int minSwaps (string &s){
    int n = s.length();
    int i=0, j=n-1;
    int moves = 0;

    while(i<=n/2){

        if(s[i]==s[j]){
            i++; j--;
            continue;
        }

        int k=j-1;
        while(k>i and s[k]!=s[i]) k--;

        if(k==i) return -1;

        while(k<j){
            swap(s[k], s[k+1]);
            moves++;
            k++;
        }

        i++; j--;
    }

    return moves;
}
```

<br>

### 6. Minimum Deletions to Make Character Frequencies Unique
A string s is called good if there are no two different characters in s that have the same frequency. Given a string s, return the minimum number of characters you need to delete to make s good.

Input: s = "ceabaacb"

Output: 2

```cpp
int minDeletions(string s) {
    unordered_map<char, int> mp;
    for(char ch : s)
        mp[ch]++;

    unordered_set<int> st;
    int moves = 0;

    for(auto p : mp){
        int freq = p.second;

        while(st.find(freq) != st.end())
            freq--;

        moves += p.second-freq;
        if(freq>0) st.insert(freq);
    }

    return moves;
}
```

<br>

### 7. Orderly Queue
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


