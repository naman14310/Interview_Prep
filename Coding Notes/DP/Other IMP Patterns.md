## @ House Robber Pattern

### 1. Delete and Earn
You are given an integer array nums. You want to maximize the number of points you get by performing the following operation any number of times: Pick any nums[i] and delete it to earn nums[i] points. Afterwards, you must delete every element equal to nums[i] - 1 and every element equal to nums[i] + 1. Return the maximum number of points you can earn by applying the above operation some number of times.

Input: nums = [3,4,2]

Output: 6

```cpp
int deleteAndEarn(vector<int>& nums) {
    int mx = *max_element(nums.begin(), nums.end());
    vector<int> res (mx+1, 0);   

    /* storing frequency of every number in array */ 

    for(int n : nums)
        res[n]++;

    /* After that, implement house robber logic */

    for(int i=2; i<res.size(); i++)
        res[i] = max(res[i-2]+res[i]*i, res[i-1]);

    return res.back();
}
```
