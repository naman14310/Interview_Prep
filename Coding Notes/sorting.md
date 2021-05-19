# Sorting

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
