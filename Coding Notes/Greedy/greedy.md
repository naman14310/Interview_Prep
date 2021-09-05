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


## @ Problems on Intervals


### 1. Maximum Length of Pair Chain
You are given an array of n pairs pairs where pairs[i] = [lefti, righti] and lefti < righti. A pair p2 = [c, d] follows a pair p1 = [a, b] if b < c. A chain of pairs can be formed in this fashion. Return the length longest chain which can be formed.

Input: pairs = [[1,2],[2,3],[3,4]]

Output: 2

Hint: Sort by pairs.second in ascending order

```cpp
static bool comp (vector<int> & v1, vector<int> & v2){
    return v1[1]<v2[1];
}


int findLongestChain(vector<vector<int>>& pairs) {
    int n = pairs.size();
    sort(pairs.begin(), pairs.end(), comp);     // --> Sort on the basis of pair[1] in ascending order

    int prev = pairs[0][1];
    int cnt = 1;

    for(int i=1; i<n; i++){

        if(pairs[i][0]>prev){
            cnt++;
            prev = pairs[i][1];
        }

    }

    return cnt;
}
```

<br>

### 2. Non-overlapping Intervals
Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

Input: intervals = [[1,2],[2,3],[3,4],[1,3]]

Output: 1

Hint: Minimum remove for overlapping interval = n - longest_chain_pair_len (above question)

```cpp
static bool comp (vector<int> & v1, vector<int> & v2){
    return v1[1]<v2[1];
}


int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    int n = intervals.size();

    sort(intervals.begin(), intervals.end(), comp);

    int prev = intervals[0][1];
    int longest_chain_len = 1;

    for(int i=1; i<n; i++){

        if(intervals[i][0]>=prev){
            prev = intervals[i][1];
            longest_chain_len++;
        }    
    }

    return n - longest_chain_len;
}
```

<br>

### 3. Minimum Number of Arrows to Burst Balloons
There are some spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. 
An arrow can be shot up from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. Given an array points where points[i] = [xstart, xend], return the minimum number of arrows that must be shot to burst all balloons.

Input: points = [[10,16],[2,8],[1,6],[7,12]]

Output: 2

Hint: 
1. Sort all points by end in ascending order. 
2. Then start iterate from left and mark range as endPoint[i]
3. Burst all balloons ahead it, which lies under this range and increment arrow by one.
4. Repeat this process by increment i to first unbursted balloon.

```cpp
static bool comp (vector<int> & v1, vector<int> & v2){
    return v1[1]<v2[1];
}

int findMinArrowShots(vector<vector<int>>& points) {
    int n = points.size();
    sort(points.begin(), points.end(), comp);        // --> sort based on end in ascending order

    int i=0;
    int arrow = 0;

    while(i<n){
        int range = points[i][1];

        int j=i+1;

        while(j<n and points[j][0]<=range)
            j++;

        arrow++;
        i = j;
    }

    return arrow;
}
```

<br>

### 4. Queue Reconstruction by Height (Tricky)
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

### 5. Two City Scheduling (Tricky)
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

### 1. Partition Labels
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

### 2. Max Chunks To Make Sorted
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

### 3. Video Stitching
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

