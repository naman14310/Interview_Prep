# HashMap (Unordered Map)

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

#### 3. Print Anagrams Together
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
