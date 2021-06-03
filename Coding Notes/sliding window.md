# Sliding Window

## Type 1 : Fixed Window Size

#### 1. First negative integer in every window of size k

Hint: The idea is to have a variable firstNegativeIndex to keep track of the first negative element in the k sized window.

```cpp
vector<long long> printFirstNegativeInteger(long long int A[], long long int N, long long int k) {                                            
    vector<long long> v;                                   
    int first_negative_index = -1;
    int next_index = 0;
    
    for(int i=0; i<k; i++){
        if(A[i]<0){
            first_negative_index=i;
            break;
        }
    }
    
    if(first_negative_index != -1){
        v.push_back(A[first_negative_index]);
        next_index = first_negative_index+1;
    }
    else{
        v.push_back(0);
        next_index = k; 
    }
        
    for(int i=1; i<N-k+1; i++){
        
        if(first_negative_index>=i) 
            v.push_back(A[first_negative_index]);
        
        else{
            for(int j=next_index; j<i+k; j++){
                if(A[j]<0){
                    first_negative_index = j;
                    break;
                }
            }
            if(first_negative_index>=i){
                v.push_back(A[first_negative_index]);
                next_index = first_negative_index+1;
            } 
            else{
                v.push_back(0);
                next_index = i+k;
            }
        }
    }
    return v;
 }
```
