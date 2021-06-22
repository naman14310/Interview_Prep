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
