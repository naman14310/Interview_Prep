# Greedy Problems

Mostly Used Data Structures and Algos : Sorting & Heaps

<br>


## @ Sorting based problems

### 1. Assign Mice to Holes
There are N Mice and N holes that are placed in a straight line. Each hole can accomodate only 1 mouse. The positions of Mice are denoted by array A and the position of holes are denoted by array B. A mouse can stay at his position, move one step right from x to x + 1, or move one step left from x to x − 1. Any of these moves consumes 1 minute. Assign mice to holes so that the time when the last mouse gets inside a hole is minimized.

Input: A = [-4, 2, 3], B = [0, -2, 4]

Output: 2

```cpp
int Solution::mice(vector<int> &A, vector<int> &B) {

    sort(A.begin(), A.end());
    sort(B.begin(), B.end());

    int ans = INT_MIN;

    for(int i=0; i<A.size(); i++)
        ans = max(ans, abs(A[i]-B[i]));
    
    return ans;
}
```

<br>

### 2. Maximize Sum Of Array After K Negations
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

### 3. Advantage Shuffle
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

### 4. Minimum Increment to Make Array Unique
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

### 5. Most Profit Assigning Work
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

### 6. Queue Reconstruction by Height (Tricky)
You are given an array of people. Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a height greater than or equal to hi. Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where queue[j] = [hj, kj] is the attributes of the jth person in the queue.

Input: people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]

Output: [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]

Approach: 
1. Sort based on height in ascending order. If height is equal, then sort based on K in descending order.
2. Iterate over sorted input array and place one person at their correct pos in each iteration.
3. We need to fill from left but, for placing ith person, we need to leave k blank spaces on the left side of its posn.

```cpp
/*  
    Sort logic:
    sort vector based on height in ascending order,
    BUT, if heights are equal, give preference to person whose k is greater

    --> [4,6] comes before [5,2]
    --> [5,2] comes before [5,0]

*/


static bool comp (vector<int> & v1, vector<int> & v2){
    if(v1[0]<v2[0]) return true;
    else if(v1[0]==v2[0] and v1[1]>v2[1]) return true;
    else return false;
}


vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
    int n = people.size();
    vector<vector<int>> res (n, {-1, -1});

    sort(people.begin(), people.end(), comp);    

    /* Iterate on sorted vector */

    for(auto p : people){
        int k = p[1];           // --> we need leave k blank position before placing curr person

        /* Hence iterate from 0 to n and leave k spaces and place curr person on the (k+1)th empty space */

        int i=0;
        while(i<n){

            if(res[i][0]==-1){
                if(k==0){
                    res[i] = p;
                    break;
                } 
                else k--;
            }
            
            i++;
        }
    }

    return res;
}
```

<br>

### 7. Two City Scheduling (Tricky)
A company is planning to interview 2n people. Given the array costs where costs[i] = [aCosti, bCosti], the cost of flying the ith person to city a is aCosti, and the cost of flying the ith person to city b is bCosti. Return the minimum cost to fly every person to a city such that exactly n people arrive in each city.

Input: costs = [[10,20],[30,200],[400,50],[30,20]]

Output: 110

Hint: To minimize the total cost, we should fly the person with the maximum saving to A, and with the minimum - to B.

Example: [30, 100], [40, 90], [50, 50], [70, 50] --> Savings: 70, 50, 0, -20.

```cpp
static bool comp (vector<int> & v1, vector<int> & v2){

    return (v1[0]-v1[1]) > (v2[0]-v2[1]);
}

int twoCitySchedCost(vector<vector<int>>& costs) {

    /* 
        Saving[i] = costs[i][0] - costs[i][1]
        So, greater saving means, it is better to travel B
        and, lesser saving means, it is better to travel A 

        Hense, Sort the vector in descending order of savings
    */

    sort(costs.begin(), costs.end(), comp);    

    /* Now since, vector is sorted in descending order of savings, allot first n to B and next n to A */

    int minCost = 0;
    int n = costs.size()/2;

    for(int i=0; i<n; i++)
        minCost += costs[i][1];

    for(int i=n; i<costs.size(); i++)
        minCost += costs[i][0];

    return minCost;
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

<br>

### 6. Minimum Number of Taps to Open to Water a Garden (Tricky)
There is a one-dimensional garden starts at the point 0 and ends at the point n. There are n + 1 taps located at points [0, 1, ..., n] in the garden. Given an integer n and an integer array ranges of length n + 1 where ranges[i] (0-indexed) means the i-th tap can water the area [i - ranges[i], i + ranges[i]] if it was open. Return the minimum number of taps that should be open to water the whole garden, If the garden cannot be watered return -1.

Input: n = 7, ranges = [1,2,1,0,2,1,0,1]

Output: 3

```cpp
/*      
        Let n = 4, ranges = [2, 2, 0, 1, 0]             


        2       2       0       1       0               |       Ranges
                                                        |                    
        x--------------->                               |       [-2, 2]
        <-------x--------------->                       |       [-1, 3]
                        x                               |          x
                        <-------x------->               |       [2, 4]
                                        x               |          x


        x represents every tap at index i
        and line represents the range of every tap
*/


int minTaps(int n, vector<int>& ranges) {

    int reach = 0;
    int taps_required = 0;
    int idx = 0;

    while(reach<n){

        /* Search for the tap whose range starts before the curr reach and go more farthest from all others */

        int farthest = reach;

        for(int i=idx; i<=n; i++){

            if(i-ranges[i]<=reach and i+ranges[i]>farthest){
                farthest = i+ranges[i];
                idx = i;                // --> we will only chk for tap which are ahead when we updated farthest
            }
        }

        /* If value of farthest does not increase that means we can't go more farther to right using these taps */

        if(farthest==reach) return -1;

        /* Else update our reach and increment taps_required */

        reach = farthest;
        taps_required++;
    }

    return taps_required;
}
```

<br>

### 7. Minimum Lights to Activate
Given an array A of size N. The ith index of this array is 0 if the light at ith position is faulty otherwise it is 1. All the lights are of specific power B which if is placed at position X, it can light the corridor from [ X-B+1, X+B-1]. Return the minimum number of lights to be turned ON to light the whole corridor or -1 if the whole corridor cannot be lighted.

Input: A = [ 0, 0, 1, 1, 1, 0, 0, 1], B = 3

Output: 2

```cpp
int Solution::solve(vector<int> &A, int B) {
    int n = A.size();
    int reach = -1;
    int bulb = 0;
    int idx = 0;

    while(reach<n-1){
        int maxReach = reach;
        
        for(int i=idx; i<n; i++){
            if(A[i]==1 and i-B+1 <= reach+1 and i+B-1 > maxReach){
                maxReach = i+B-1;    
                idx = i+1;    
            }
            else if (i-B+1 > reach) break;
        }

        if(maxReach==reach) return -1;

        reach = maxReach;
        bulb++;
    }

    return bulb;
}
```

<br>


## @ Other Tricky problems

### 1. [Maximum Number of Weeks for Which You Can Work](https://leetcode.com/problems/maximum-number-of-weeks-for-which-you-can-work/)
If the max elements is larger than the sum of the rest elements. then the max answer is 2 * rest + 1, because the best strategy is pick max, pick other, pick max, pick other....pick max. otherwise, we can finish all of the milestones.

```cpp
long long numberOfWeeks(vector<int>& milestones) {

    long long sum = 0;
    long long mx = INT_MIN;

    for(int m : milestones){
        sum += m;
        long long num = m;
        mx = max(mx, num);
    }

    long long remSum = sum - mx;

    if(mx>remSum)
        return 2*remSum+1;
    else
        return sum;
}
```

<br>

### 2. Get the Maximum Score - O(n) | O(1)
You are given two sorted arrays of distinct integers nums1 and nums2. A valid path is defined as follows:

1. Choose array nums1 or nums2 to traverse (from index-0).
2. Traverse the current array from left to right.
3. If you are reading any value that is present in nums1 and nums2 you are allowed to change your path to the other array. (Only one repeated value is considered in the valid path).
4. Score is defined as the sum of uniques values in a valid path.

Return the maximum score you can obtain of all possible valid paths. Since the answer may be too large, return it modulo 10^9 + 7.

![img](https://assets.leetcode.com/uploads/2020/07/16/sample_1_1893.png)

Input: nums1 = [2,4,5,8,10], nums2 = [4,6,8,9]

Output: 30

[Explaination](https://leetcode.com/problems/get-the-maximum-score/discuss/767987/JavaC%2B%2BPython-Two-Pointers-O(1)-Space)

```cpp
int maxSum(vector<int>& nums1, vector<int>& nums2) {
    long long maxScore = 0;

    int i=0, j=0;
    long long hopsum1 = 0, hopsum2 = 0; 

    while(i<nums1.size() and j<nums2.size()){

        /* If val1 == val2 then add max_hopsum to score and increment both i and j, and also reset both hopsums to 0 */

        if(nums1[i]==nums2[j]){
            maxScore += max(hopsum1, hopsum2) + nums1[i];

            i++; j++;
            hopsum1 = 0; hopsum2 = 0;
        }

        /* If val1 < val2 then update hopsum1 and increment i */

        else if(nums1[i]<nums2[j]){
            hopsum1 += nums1[i];
            i++;
        }

        /* Else update hopsum2 and increment j */

        else{
            hopsum2 += nums2[j];
            j++;
        }

    }

    /* Do same processing for last hop (i.e. last common val to list_end) */

    while(i<nums1.size()){
        hopsum1 += nums1[i];
        i++;
    }

    while(j<nums2.size()){
        hopsum2 += nums2[j];
        j++;
    }

    maxScore += max(hopsum1, hopsum2);

    return maxScore % 1000000007;
}
```
