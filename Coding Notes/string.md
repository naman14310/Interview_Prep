# String

## Easy

#### 1. Search All (Find method)
Implement a function that return all occurances of subtring in a given string

```cpp
vector<int> stringSearch(string big,string small){
    //store the occurrences in the result vector
    vector<int> result;
    int index = 0;
    while(true){
        index = big.find(small, index);
        if(index==-1) break;
        
        result.push_back(index);
        index++;
    }
    return result;
}
```

#### 2. Tokenize string (stringstream)

```cpp
int main(){
  string input;
  getline(cin, input);
  
  stringstream ss(input);
  
  string token;
  vector<string> tokens;
  
  while(getline(ss, token, ' '){
      tokens.push_back(token);
  }
  
  for(auto tkn : tokens) cout<<tkn<<endl;
  return 0;
}
```

#### 3. Run Length Encoding

```cpp
string compressString(const string &str){   
  string res = "";
  res.push_back(str[0]);
  int i=1;
  int count=1;
  char run = str[0]; 
  
  while(i<str.length()){
      if(str[i]!=run){
          res+=to_string(count);
          count=0;
          run = str[i];
          res.push_back(run);
      }
      count++;
      i++;
  }

  res+=to_string(count);
  if(res.length()>=str.length()) return str;
  return res;
}
```

#### 4. String Normalization
Convert given string to Camelcase

```cpp
string normalize(const string &sentence) {
    string copy = sentence;
    string res = "";
    
    for(int i=0; i<copy.length(); i++){
        if(i==0){
            if(isalpha(copy[i]))
                res.push_back(toupper(copy[i]));
            else
                res.push_back(copy[i]);
        }
        else{
            if(copy[i-1]==' '){
                if(isalpha(copy[i])) res.push_back(toupper(copy[i]));
                else res.push_back(copy[i]);
            }
            else
                res.push_back(tolower(copy[i]));
        }
    }    
    return res;
}
```

#### 5. Palindrome break
Convert a given palindromic string to non palindrome (which is lexicographically smallest) by replacing only one character.

```cpp
string breakPalindrome(string s) {
  int n = s.length();    
  if(n<=1) return "";

  for(int i=0; i<n/2; i++){
      if(s[i]!='a'){
          s[i] = 'a';
          return s;
      }
  }

  s[n-1] = 'b';
  return s;    
}
```
