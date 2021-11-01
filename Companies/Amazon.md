# Amazon

<br>

### [Maximum Units on a Truck](https://leetcode.com/problems/maximum-units-on-a-truck/)

```cpp
static bool comp (vector<int> &v1, vector<int> &v2){
    return v1[1]>=v2[1];
}


int maximumUnits(vector<vector<int>>& boxTypes, int truckSize) {
    sort(boxTypes.begin(), boxTypes.end(), comp);
    int ans = 0;

    for(auto b : boxTypes){

        if(b[0]<=truckSize){
            ans += b[0]*b[1];
            truckSize -= b[0];
        }

        else{
            ans += truckSize*b[1];
            return ans;
        }
    }

    return ans;
}
```

<br>
