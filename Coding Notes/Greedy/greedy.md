# Greedy Problems

Mostly Used Data Structures and Algos : Sorting & Heaps

<br>


## @ Sorting based problems


### 1. Maximize Sum Of Array After K Negations
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

### 2. Advantage Shuffle
Given two arrays A and B of equal size, the advantage of A with respect to B is the number of indices i for which A[i] > B[i]. Return any permutation of A that maximizes its advantage with respect to B.

Input: A = [2,7,11,15], B = [1,10,4,11]

Output: [2,11,7,15]

Approach: 
1. Sort both the arrays (B with its index). 
2. Iterate over A. if A[i] is just greater then B[i] then store it at index of B[i]. 
3. Otherwise, store it at an empty place from the end.

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

<br>

### 3. Minimum Increment to Make Array Unique
You are given an integer array nums. In one move, you can pick an index i where 0 <= i < nums.length and increment nums[i] by 1. Return the minimum number of moves to make every value in nums unique.

Input: nums = [3,2,1,2,1,7]

Output: 6

Hint: Sort the input array. Compared with previous number, the current number need to be at least prev + 1.

```cpp
int minIncrementForUnique(vector<int>& nums) {

    int moves = 0;
    sort(nums.begin(), nums.end());

    for(int i=1; i<nums.size(); i++){

        if(nums[i]<=nums[i-1]){
            moves += (nums[i-1]+1) - nums[i];
            nums[i] = nums[i-1]+1;
        }

    }

    return moves;
}
```

<br>

### 4. Most Profit Assigning Work
You have n jobs and m workers. You are given three arrays: difficulty, profit, and worker where:
1. difficulty[i] and profit[i] are the difficulty and the profit of the ith job, and
2. worker[j] is the ability of jth worker (i.e., the jth worker can only complete a job with difficulty at most worker[j]).

Every worker can be assigned at most one job, but one job can be completed multiple times. Return the maximum profit we can achieve after assigning the workers to the jobs.

Input: difficulty = [2,4,6,8,10], profit = [10,20,30,40,50], worker = [4,5,6,7]

Output: 100

Hint: Sort jobs by profit and then difficulty in descending order. Sort workers in descending order.

```cpp
int maxProfitAssignment(vector<int>& difficulty, vector<int>& profit, vector<int>& worker) {
    int n = profit.size();
    vector<pair<int,int>> jobs;

    for(int i=0; i<n; i++)
        jobs.push_back({profit[i], difficulty[i]});

    /* sort jobs by profit and then by difficulty in descending order */

    sort(jobs.begin(), jobs.end(), greater<pair<int,int>>());      

    /* sort workers by descending order */

    sort(worker.begin(), worker.end(), greater<int>());

    int i=0;
    int p = 0;

    /* 
        Now iterate for every worker and increment i untill he has ability to do job 
        Since, all remaining workers have lesser ability then curr, so we will not
        recheck prev left jobs for others.
    */

    for(auto w : worker){

        while(w<jobs[i].second and i<n)
            i++;

        if(i==n) break;

        p += jobs[i].first;
    }

    return p;
}
```

<br>


## @ Jump Game Pattern (Flag-Race approach)

### 1. Jump Game - O(n) | O(1)
You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position. Return true if you can reach the last index, or false otherwise.

Input: nums = [2,3,1,1,4]

Output: true

```cpp
bool canJump(vector<int>& nums) {
    int n = nums.size();
    int reach = 0;

    for(int i=0; i<=min(reach, n-1); i++)
        reach = max(reach, nums[i]+i);

    return reach>=n-1;
}
```

<br>

### 2. Jump Game II - O(n) | O(1)
Given an array of non-negative integers nums, you are initially positioned at the first index of the array. Each element in the array represents your maximum jump length at that position. Your goal is to reach the last index in the minimum number of jumps.

Input: nums = [2,3,0,1,4]

Output: 2

```cpp
int jump(vector<int>& nums) {
    int n = nums.size();

    int steps = 0;
    int begin = 0, end = 0, farthest = 0;

    for(int i=0; i<n-1; i++){

        farthest = max(farthest, i+nums[i]);

        /* If we reached to the end of prev jump, then we need to increment jump steps and update end to farthest */

        if(i==end){
            end = farthest;
            steps++;
        }
    }

    return steps;
}
```

<br>

### 3. Partition Labels
You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part. Return a list of integers representing the size of these parts.

Input: s = "ababcbacadefegdehijhklij"

Output: [9,7,8]

```cpp
vector<int> partitionLabels(string s) {
    int n = s.length();
    vector<int> res;

    unordered_map<char, int> mp;        // --> map of {char, end_pos}

    for(int i=0; i<n; i++)
        mp[s[i]] = i;

    int prev = -1;                      // --> points to the end of prev partition (initially -1)
    int end = 0;

    for(int i=0; i<n; i++){

        end = max(end, mp[s[i]]);       // update end (or maxreach) everytime

        if(i==end){
            int partition_len = i-prev;
            res.push_back(partition_len);
            prev = i;
        }
    }

    return res;
}
```

<br>

### 4. Max Chunks To Make Sorted
You are given an integer array arr of length n that represents a permutation of the integers in the range [0, n - 1]. We split arr into some number of chunks (i.e., partitions), and individually sort each chunk. After concatenating them in same order as they present, the result should equal the sorted array. Return the largest number of chunks we can make to sort the array.

Input: arr = [2,0,1,4,3,5]

Output: 3

```cpp
int maxChunksToSorted(vector<int>& arr) {
    int end = 0;
    int chunks = 0;

    for(int i=0; i<arr.size(); i++){    
        end = max(end, arr[i]);

        if(i==end) 
            chunks++;
    }

    return chunks;
}
```

<br>

### 5. Video Stitching
You are given a series of video clips. Each video clip is described by an array clips where clips[i] = [starti, endi]. We can cut these clips into segments freely. For example, a clip [0, 7] can be cut into segments [0, 1] + [1, 3] + [3, 7]. Return the minimum number of clips needed so that we can cut the clips into segments that cover the entire sporting event [0, time]. If the task is impossible, return -1.

Input: clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]], time = 10

Output: 3

```cpp
int videoStitching(vector<vector<int>>& clips, int time) {
    int reach = 0; 
    int clips_required = 0;

    while(reach<time){

        int farthest = reach;               // -- farthest var tells us, how far we can go with clips that start before reach.

        for(int i=0; i<clips.size(); i++){

            if(clips[i][0]<=reach and clips[i][1]>farthest)
                farthest = clips[i][1];
        }

        if(farthest==reach) return -1;      // --> If we made no progress, simply return -1

        reach = farthest;
        clips_required++;
    }

    return clips_required;
}
```

