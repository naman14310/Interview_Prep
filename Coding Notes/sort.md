# Sorting

## Sorting Algos

#### 1. Merge Sort 

**Time Complexity : O(nlogn) (for all cases) 
**Space Complexity : O(n) (drawback)

```cpp
void merge(vector<int> & v, int start, int end, int mid){
    vector<int> temp;
    int i=start, j=mid+1;

    while(i<=mid && j<=end){
        if(v[i]<=v[j]) 
            temp.push_back(v[i++]);
        else 
            temp.push_back(v[j++]);
    }

    while(i<=mid) 
        temp.push_back(v[i++]);

    while(j<=end) 
        temp.push_back(v[j++]);

    for(int i=start; i<=end; i++)
        v[i] = temp[i-start];
}

void mergesort(vector<int> & v, int start, int end){
    if(start>=end) return;

    int mid = start + (end-start)/2;

    mergesort(v, start, mid);
    mergesort(v, mid+1, end);
    merge(v, start, end, mid);
}

void sort(vector<int> & v){
    mergesort(v, 0, v.size()-1);
}
```

#### 2. Quick Sort 

**Time Complexity : O(n2) (worst case, Rare) | O(nlogn) (avg case) 
**Space Complexity : O(1)

```cpp
int get_partition(vector<int> & v, int start, int end){
    int pivot = end;
    int i=start, j=start;

    while(j<end){
        if(v[j]<v[pivot]){
            swap(v[i], v[j]);
            i++;
            j++;
        }
        else j++;
    }
    swap(v[i], v[pivot]);
    return i;
}

void quicksort(vector<int> & v, int start, int end){
    if(start>=end) return;

    int p = get_partition(v, start, end);
    quicksort(v, start, p-1);
    quicksort(v, p+1, end);
}
```

#### 3. Quick Select (Variation of Quick sort for finding Kth smallest/largest element in array without sorting)

**Time Complexity : O(n2) (worst case, Rare) | O(n) (avg case) 
**Space Complexity : O(1)

```cpp
/* Exact same partition function of quicksort */

int get_partition(vector<int> & v, int start, int end){
    int pivot = end;
    int i=start, j=start;
    
    while(j<end){
        if(v[j]<v[pivot]){
            swap(v[i], v[j]);
            i++;
            j++;
        }
        else j++;
    }
    swap(v[i], v[pivot]);
    return i;
}

int quickselect(vector<int> & v, int start, int end, int k){
    if(start>end) return -1;
    int p = get_partition(v, start, end);

    if(p==k-1) 
        return v[p];
    else if(p<k-1)
        return quickselect(v, p+1, end, k);
    else
        return quickselect(v, start, p-1, k);
}

int search(vector<int> & v, int k){
    return quickselect(v, 0, v.size()-1, k);
}
```

## Easy

#### 1. Count Inversions

```cpp
void merge(long long arr[], long long start, long long end, long long mid, long long &invCount){
    vector<long long> temp;
    long long i=start, j= mid+1;

    while(i<=mid && j<=end){
        if(arr[i]<=arr[j])
            temp.push_back(arr[i++]);
        else{
            invCount+=mid-i+1;           // just added this one line to merge sort code
            temp.push_back(arr[j++]);
        }
    }

    while(i<=mid) temp.push_back(arr[i++]);

    while(j<=end) temp.push_back(arr[j++]);

    for(long long i=start; i<=end; i++)
        arr[i] = temp[i-start];
}

void mergesort(long long arr[], long long start, long long end, long long & invCount){
    if(start>=end) return;

    long long mid = start + (end-start)/2;

    mergesort(arr, start, mid, invCount);
    mergesort(arr, mid+1, end, invCount);
    merge(arr, start, end, mid, invCount);
}

long long int inversionCount(long long arr[], long long N){
    long long invCount=0;
    mergesort(arr, 0, N-1, invCount);
    return invCount;
}
```

## Medium

#### 1. Minimum Swaps to Sort
Given an array of n distinct elements. Find the minimum number of swaps required to sort the array in strictly increasing order.

Hint: Think of something related to cycle detection. Found all cycle lengths and add them to find minimum no. of swaps.

```cpp
int minSwaps(vector<int>&nums){
    vector<pair<int,int>> v;
    int minswaps = 0;
    for(int i=0; i<nums.size(); i++)
        v.push_back({nums[i], i});

    sort(v.begin(), v.end());
 
    vector<bool> vis (nums.size(), false);

    for(int i=0; i<v.size(); i++){
        if(i==v[i].second || vis[i]) 
          continue;

        int cycle=0;
        int j=i;
        while(!vis[j]){
            vis[j] = true;
            j = v[j].second;
            cycle++;
        }
        minswaps += cycle-1;
    }
    return minswaps;
}
```

#### 2. Biggest Number String
You're given a vector of numbers, Create a lexiographically largest no. by concatenating those numbers.

```cpp
bool comp(int a, int b){
    string s1 = to_string(a); 
    string s2 = to_string(b);
    
    int i=0, j=0;
    while(i<s1.length() && j<s2.length()){
        if(s1[i]>s2[j]) return true;
        if(s2[i]>s1[i]) return false;
        i++; j++;
    }
    
    int start=0;
    while(i<s1.length()){
        if(s1[i]>s1[start]) return true;
        else if(s1[i]<s1[start]) return false;
        i++; start++;
    }
    
    start=0;
    while(j<s2.length()){
        if(s2[j]>s2[start]) return false;
        else if(s2[i]<s2[start]) return true;
        j++; start++;
    }
    return true;
}


string concatenate(vector<int> numbers){
    sort(numbers.begin(), numbers.end(), comp);
    string ans;
    
    for(auto i : numbers)
        ans += to_string(i);
        
    return ans;
}
```
