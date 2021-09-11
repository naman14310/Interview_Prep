# HashMap (Unordered Map)

<br>

#### 1. Number of Matching Subsequences
Given a string s and an array of strings words, return the number of words[i] that is a subsequence of s.

Input: s = "abcde", words = ["a","bb","acd","ace"]

Output: 3

[Video Explaination](https://www.youtube.com/watch?v=Vch3pFgmKD8)

```cpp
int numMatchingSubseq(string s, vector<string>& words) {
    unordered_map<char, queue<pair<string,int>>> mp;          // map with keys a-z --> storing all words waiting for char word[pos]

    for(string word : words)
        mp[word[0]].push({word,0});

    int ans = 0;

    for(char ch : s){

        if(mp.find(ch)!=mp.end()){
            int n = mp[ch].size();

            for(int i=0; i<n; i++){
                auto p = mp[ch].front();
                mp[ch].pop();

                string w = p.first;       // --> worrd
                int idx = p.second;       // --> pos till that word is matched so far

                idx++;

                if(idx<w.length())
                    mp[w[idx]].push({w, idx});
                else
                    ans++;
            }
        }
    }
    return ans;   
}
```

<br>

#### 2. Couples Holding Hands
There are n couples sitting in 2n seats. The people and seats are represented by an integer array row where row[i] is the ID of the person sitting in the ith seat. The couples are numbered in order, the first couple being (0, 1), the second couple being (2, 3), and so on with the last couple being (2n - 2, 2n - 1).
Return the minimum number of swaps so that every couple is sitting side by side.

Input: row = [0,2,1,3]

Output: 1

```cpp
int minSwapsCouples(vector<int>& row) {
    int swap = 0;
    unordered_map<int,int> mp;            

    /* Insert all element with their indexes in hashmap */

    for(int i=0; i<row.size(); i++)
        mp[row[i]] = i;

    /* Iterate every even index and crrct its next element */

    for(int i=0; i<row.size()-1; i+=2){

        int first = row[i];
        int second = first%2==0 ? first+1 : first-1;

        /* if second val is not correct, swap it with crrct value and increment the count */

        if(row[i+1]!=second){

            mp[row[i+1]] = mp[second];     // ----> Don't forget to update the index in hashmap

            int temp = row[i+1];
            row[i+1] = row[mp[second]];
            row[mp[second]] = temp;

            swap++;
        }
    }

    return swap;
}
```

<br>

#### 3. Array of Doubled Pairs
Given an array of integers arr of even length, return true if and only if it is possible to reorder it such that arr[2 * i + 1] = 2 * arr[2 * i] for every 0 <= i < len(arr) / 2.

Input: arr = [4,-2,2,-4]

Output: true

```cpp
bool canReorderDoubled(vector<int>& arr) {
    int n = arr.size();
    sort(arr.begin(), arr.end());       // --> sort the array 

    unordered_map<int,vector<int>> freq;
    vector<bool> done (n, false);

    /* Insert all nums in hashmap with their indexes after sorting */

    for(int i=0; i<n; i++)
        freq[arr[i]].push_back(i);

    /* Iterate for every num and find if its pair (2*num) exist or not */

    for(int i=0; i<n; i++){
        if(done[i]) continue;       // --> If num is already used as pair then continue

        int num = arr[i];

        /* Else found out its pair */

        if(freq.find(2*num)!=freq.end()){

            /* One by one pop out indexes from end of pair's vector till we get any unused index */

            while(!freq[2*num].empty() and done[freq[2*num].back()])
                freq[2*num].pop_back();

            /* If that vector becomes empty we will simply continue */

            if(freq[2*num].empty()) continue;

            /* Else mark both the indexed to be true (used) */

            done[i] = true;
            done[freq[2*num].back()] = true;
        }
    }

    /* If we found any unused number then return false else return true */

    for(bool d : done)
        if(!d) return false;

    return true;
}
```

<br>

#### 4. Print Anagrams Together
Given an array of strings, return all groups of strings that are anagrams. The groups must be created in order of their appearance in the original array.

Input: words[] = {no,on,is}

Output: [[no, on],[is]]

Hint: Use unordered_map for mapping key to indexes

```cpp
vector<vector<string> > Anagrams(vector<string>& string_list) {
    unordered_map<string, int> mp;       // ----> {key, index in res vector}
    vector<vector<string>> res;
    
    for(string s : string_list){
        string key = s;
        sort(key.begin(), key.end());
        
        if(mp.find(key)!=mp.end()){
            int idx = mp[key];
            res[idx].push_back(s);
        }
        else{
            res.push_back({s});
            mp[key] = res.size()-1;
        }
    }

    return res;
}
```

<br>

#### 5. Longest String Chain
You are given an array of words. wordA is a predecessor of wordB if and only if we can insert exactly one letter anywhere in wordA without changing the order of the other characters to make it equal to wordB. For example, "abc" is a predecessor of "abac", while "cba" is not a predecessor of "bcad".

A word chain is a sequence of words [word1, word2, ..., wordk] with k >= 1, where word1 is a predecessor of word2, word2 is a predecessor of word3, and so on. A single word is trivially a word chain with k == 1. Return the length of the longest possible word chain with words chosen from the given list of words.

Input: words = ["a","b","ba","bca","bda","bdca"]

Output: 4

Approach: 
1. Sort the input vector by string length and iterate for all words from left to right. 
2. For every word, from every possible word by erasing all char at each index one by one, and found these word in map. 
3. Calculate the len of longest chain for curr word so far and insert it in the map.

```cpp
static bool comp (string & s1, string & s2){
    return s1.length() < s2.length();
}

int longestStrChain(vector<string>& words) {
    unordered_map<string,int> mp;
    int longest_chain = 1;

    sort(words.begin(), words.end(), comp);

    for(string w : words){

        if(w.length()==1){
            mp[w] = 1;
            continue;
        }

        int chain_len = 0;

        for(int i=0; i<w.length(); i++){
            string s = w;
            s.erase(s.begin()+i);

            if(mp.find(s)!=mp.end())
                chain_len = max(chain_len, mp[s]);
        }

        mp[w] = chain_len+1;
        longest_chain = max(longest_chain, mp[w]);
    }

    return longest_chain;
}
```

<br>

#### 6. Implement Magic Dictionary
Design a data structure that is initialized with a list of different words. Provided a string, you should determine if you can change exactly one character in this string to match any word in the data structure.

```cpp
class MagicDictionary {
public:
    /** Initialize your data structure here. */
    
    unordered_map<int, unordered_set<string>> mp;
    
    MagicDictionary() {
    }
    
    void buildDict(vector<string> dictionary) {
        for(auto s : dictionary)
            mp[s.length()].insert(s);
    }
    
    bool search(string word) {
        int len = word.length();
        if(mp.find(len)==mp.end()) return false;
        
        for(int i=0; i<len; i++){
            string temp = word;
            
            for(int j=0; j<26; j++){
                if(word[i]=='a'+j) continue; 

                temp[i] = 'a' + j;
                                
                if(mp[len].find(temp)!=mp[len].end()) 
                    return true;   
            }
        }
        
        return false;
    }
};
```

<br> 

#### 7. Time Based Key-Value Store
Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.

```cpp
class TimeMap {
public:
    
    unordered_map<string, map<int,string> > mp;
    
    
    TimeMap() {
    }

    
    void set(string key, string value, int timestamp) {
        mp[key].insert({timestamp, value});
    }

    
    string get(string key, int timestamp) {
        if(mp.find(key)==mp.end()) return "";

        auto itr = mp[key].upper_bound(timestamp);
        
        if(itr==mp[key].begin())
            return "";
        else
            return prev(itr)->second;           // --> prev will give the prev element of itr
    }
};
```

<br>

#### 8. Insert Delete GetRandom O(1) (Tricky)

Approach: Use two data structures, a Hashmap and a vector. The Hashmap maps the values to their respective indices in the array.

1. Insert Operation

Insert new value to the end of vector and updating its index in the Hash Table.

2. Remove Operation

We will swap the value we want to remove with the last element of the array and then remove the last element of the array and erase it from hashmap. Also update indexes in hashmap.

3. getRandom Operation

Use C++ inbuilt rand() on the array.

```cpp
class RandomizedSet {
public:
    
    vector<int> v;
    unordered_map<int,int> mp;          // --> map of {val, index in array}
    
    RandomizedSet(){
    }
    
    /* Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    
    bool insert(int val) {
        if(mp.find(val)!=mp.end()) return false;
        
        v.push_back(val);
        mp[val] = v.size()-1;
        
        return true;
    }
    
    /* Removes a value from the set. Returns true if the set contained the specified element. */
    
    bool remove(int val) {
        if(mp.find(val)==mp.end()) return false;
        
        int idx1 = mp[val];
        int idx2 = mp[v.back()];
        
        mp[v.back()] = idx1;
        swap(v[idx1], v[idx2]);
        
        v.pop_back();
        mp.erase(val);
        
        return true;
    }
    
    /* Get a random element from the set. */
    
    int getRandom() {
        return v[rand()%v.size()];
    }
};
```

<br>

#### 9. Snapshot Array
Implement a SnapshotArray that supports the following interface:
1. SnapshotArray(int length) initializes an array-like data structure with the given length.  Initially, each element equals 0.
2. void set(index, val) sets the element at the given index to be equal to val.
3. int snap() takes a snapshot of the array and returns the snap_id: the total number of times we called snap() minus 1.
4. int get(index, snap_id) returns the value at the given index, at the time we took the snapshot with the given snap_id

```cpp
class SnapshotArray {
public:
    
    unordered_map<int, unordered_map<int, int>> mp;     // --> map of {index, map of {snap_id, val}}
    int snap_id = 0;
    
    SnapshotArray(int length){
    }
    
    /* Insert val with snap_id */
    
    void set(int index, int val) {
        mp[index][snap_id] = val;
    }
    
    /* Jst increment snap_id */
    
    int snap() {
        snap_id++;
        return snap_id-1;
    }
    
    /* return val with asked snap_id or smaller snap_id (which is latest of all) */
    
    int get(int index, int snap_id) {
        
        for(int i=snap_id; i>=0; i--){
            if(mp[index].find(i)!=mp[index].end())
                return mp[index][i];
        }
        
        return 0;
    }
};
```
