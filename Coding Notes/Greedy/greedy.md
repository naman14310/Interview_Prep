# Greedy Problems

<br>

#### 1. Maximize Sum Of Array After K Negations
Given an integer array nums and an integer k, modify the array by choosing an index i and replace nums[i] with -nums[i]. You should apply this process exactly k times. You may choose the same index i multiple times. Return the largest possible sum of the array after modifying it in this way.

Input: nums = [3,-1,0,2], k = 3

Output: 6

Approach:
1. sort the numbers in ascending order
2. flip all the negative numbers, as long as k > 0
3. find the sum of the new array (with flipped numbers if any) and keep track of the minimum number. If k is remaining, then apply operation on that smallest num.

```cpp
int largestSumAfterKNegations(vector<int>& nums, int k) {
    int n = nums.size();
    sort(nums.begin(), nums.end());

    int i=0;
    int sm_idx = 0;        // --> stores the index of smallest element processed so far 

    while(k>0){

        /* update sm_idx if absolute val of current element formed is new smallest */

        if(i<n and nums[sm_idx] > abs(nums[i]))
                sm_idx = i;

        if(i<n and nums[i]<0){
            nums[i] = abs(nums[i]);
            i++; k--;
        }

        else{

            /* If k is odd, we need to flip the sign of smallest number (for even, do nothing) */

            if(k%2!=0) nums[sm_idx] *= -1;

            break;
        }

    }

    int sum = accumulate(nums.begin(), nums.end(), 0);
    return sum;
}
```

<br>

#### 2. Advantage Shuffle
Given two arrays A and B of equal size, the advantage of A with respect to B is the number of indices i for which A[i] > B[i]. Return any permutation of A that maximizes its advantage with respect to B.

Input: A = [2,7,11,15], B = [1,10,4,11]

Output: [2,11,7,15]

Approach: Sort both the arrays (B with its index). Iterate over A. if A[i] is just greater then B[i] then store it at index of B[i]. Otherwise, store it at an empty place from the end.

```cpp
static bool mysort(pair<int,int> a, pair<int,int> b){
    if(a.first>b.first) return true;
    return false;
}

vector<int> advantageCount(vector<int>& A, vector<int>& B) {
    vector<pair<int,int>> v;
    for(int i=0; i<B.size(); i++)
        v.push_back({B[i], i});

    vector<int> res(A.size(), 0);

    sort(A.begin(), A.end());
    sort(v.begin(), v.end(), mysort);

    int i=0, j=A.size()-1;

    for(int k=0; k<A.size(); k++){
        int pos = v[k].second;
        if(A[j]>v[k].first){
            res[pos] = A[j];
            j--;
        }
        else{
            res[pos] = A[i];
            i++;
        }
    }
    return res;
}
```
