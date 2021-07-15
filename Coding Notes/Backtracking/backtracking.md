# Recursion & Backtracking 

## 1D Problems

#### 1. Letter Tile Possibilities
You have n  tiles, where each tile has one letter tiles[i] printed on it. Return the number of possible non-empty sequences of letters you can make using the letters printed on those tiles.

Input: tiles = "AAB",  Output: 8

Explanation: The possible sequences are "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA".

```cpp
void solve (vector<int> & freq, int idx, int n, unordered_set<string> & vis, string & s, int & count){
    if(idx==n) return; 

    /* At every pos we have 26 options (A-Z) to fill */

    for(int i=0; i<26; i++){

        /* If freq > 0 that means we can fill it */

        if(freq[i]>0){   

            /* Decrement its freq and push that char in string */

            freq[i]--;
            s.push_back('A'+i);

            /* If this string s is not vis before, mark it vis and increament the count */ 

            if(vis.find(s)==vis.end()){
                vis.insert(s);
                count++;
            }

            /* and simply recurse */

            solve(freq, idx+1, n, vis, s, count);

            /* after coming back, redo the changes */

            s.pop_back();
            freq[i]++;
        }
    }
}

int numTilePossibilities(string tiles) {
    int n = tiles.size();

    vector<int> freq (26, 0);          // --> for storing freq of all chars
    unordered_set<string> vis;         // --> for keeping track of visited strings

    string s = "";
    int count = 0;

    for(char ch : tiles)
        freq[ch-'A']++;

    solve (freq, 0, n, vis, s, count);
    return count;
}
```
