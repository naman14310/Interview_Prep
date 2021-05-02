# Arrays

## Easy

#### 977. Squares of a Sorted Array
Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

```cpp
vector<int> sortedSquares(vector<int>& nums) {
    int len = nums.size();
    vector<int> res(len, 0);
    int l=0, r=len-1, pos = len-1;
    while(l<=r){
        int sqrL = nums[l]*nums[l];
        int sqrR = nums[r]*nums[r];
        if(sqrR > sqrL){
            res[pos] = sqrR;
            r--; 
        }
        else{
            res[pos] = sqrL;
            l++;
        }
        pos--;
    }
    return res;
}
```
