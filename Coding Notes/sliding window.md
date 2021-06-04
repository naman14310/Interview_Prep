# Sliding Window

## Type 1 : Fixed Window Size

#### 1. Count Occurences of Anagrams
Given a word pat and a text txt. Return the count of the occurences of anagrams of the word in the text.

Hint: Create a count array for storing frequency

```cpp
int count_occurance(string pat, string txt) {
    int ans = 0;
    vector<int> v1 (26, 0);
    vector<int> v2 (26, 0);

    int k = pat.size();
    int n = txt.size();
    int i=0;

    for( ; i<k; i++){
        v1[pat[i]-'a'] += 1;
        v2[txt[i]-'a'] += 1;
    }
    if(v1==v2) ans+=1;

    for( ; i<n; i++){
        v2[txt[i]-'a'] +=1;
        v2[txt[i-k]-'a'] -=1;

        if(v1==v2) ans+=1;
    }
    return ans;
}
```


#### 2. First negative integer in every window of size k

Hint: The idea is to have a variable firstNegativeIndex to keep track of the first negative element in the k sized window.

```cpp
vector<long long> printFirstNegativeInteger(long long int A[], long long int N, long long int k) {  
    vector<long long> v;                                   
    int first_negative_index = -1;
    int next_index = 0;
    
    for(int i=0; i<k; i++){
        if(A[i]<0){
            first_negative_index=i;
            break;
        }
    }
    
    if(first_negative_index != -1){
        v.push_back(A[first_negative_index]);
        next_index = first_negative_index+1;
    }
    else{
        v.push_back(0);
        next_index = k; 
    }
        
    for(int i=1; i<N-k+1; i++){
        
        if(first_negative_index>=i) 
            v.push_back(A[first_negative_index]);
        
        else{
            for(int j=next_index; j<i+k; j++){
                if(A[j]<0){
                    first_negative_index = j;
                    break;
                }
            }
            if(first_negative_index>=i){
                v.push_back(A[first_negative_index]);
                next_index = first_negative_index+1;
            } 
            else{
                v.push_back(0);
                next_index = i+k;
            }
        }
    }
    return v;
 }
```

## Type 2 : Variable Window Size

#### 1. Longest Substring Without Repeating Characters

```cpp
int lengthOfLongestSubstring(string s) {
    if(s.length()==0) 
        return 0;

    vector<int> last_index (256, -1);  // It will store index of last occurance
    int i=0, j=0;
    int ans = 1;

    while(j<s.length()){
        char ch = s[j];

        if(last_index[ch]>=i)
            i = last_index[ch]+1;

        last_index[ch] = j;
        ans = max(ans, j-i+1);
        j++;
    }

    return ans;
}
```

#### 2. Longest Substring with atmost k distinct characters

Hint: Use vector to store index of last occurance

```cpp
/* This Function will return leftmost index to which pointer i can be directly jump while contracting window size */ 

int get_leftmost_index(vector<int> & last_index, int i){
    int index = INT_MAX;
    for(int idx : last_index){
        if(idx>=i)
            index = min(index, idx);
    }
    return index+1;
}

int longest_substring(string s, int k){
    vector<int> last_index (256, -1);

    int i=0, j=0;
    int ans=0;

    while(j<s.length()){
        char ch = s[j];

        if(last_index[ch]>=i){
            last_index[ch] = j;
            ans = j-i+1;
        }
        else{
            if(k>0){
                last_index[ch] = j;
                ans = j-i+1;
                k--;
            }
            else{
                last_index[ch] = j;
                i = get_leftmost_index(last_index, i);
            }
        }
        j++;
    }
    
    return ans;
}
```
