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

