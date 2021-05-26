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
