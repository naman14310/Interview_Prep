# Problems on Subarrays

### 1. Flip (IB)

You are given a binary string A. In a single operation, you can choose two indices L and R such that 1 ≤ L ≤ R ≤ N and flip the characters AL, AL+1, ..., AR.  Your aim is to perform ATMOST one operation such that in final string number of 1s is maximised.

If you don't want to perform the operation, return an empty array. Else, return an array consisting of two elements denoting L and R. If there are multiple solutions, return the lexicographically smallest pair of L and R.

NOTE: Pair (a, b) is lexicographically smaller than pair (c, d) if a < c or, if a == c and b < d.

Input: A = "010"

Output: [1, 1]

Hint: Iterate only one time and maintain a cnt variable to track 1's and 0's cnt. Start new subaray if cnt becomes negative.

```cpp
vector<int> Solution::flip(string A) {
    int n = A.size();
    vector<int> ans;

    /* 
        zero will increment the cnt while 1 will decrement the cnt
        Now, our goal is to maximize this cnt
    */

    int cnt = 0, maxChange = 0;
    int start = 0;        

    for(int i=0; i<n; i++){
        if(A[i]=='1') cnt--;
        else cnt++;

        /* 
            If cnt<0, means we get more number of 1's so simple start
            new subarray from next index and reset cnt
        */

        if(cnt<0){
            start = i+1;
            cnt = 0;
        }

        /*
            If len of cnt is greater the maxChange so far
            then mark curr subarray till curr index as our answer
            and update maxChange var
        */

        else if(cnt>maxChange){
            ans = {start+1, i+1};       // --> follows 1 based indexing
            maxChange = cnt;
        }
    }

    return ans;
}
```
