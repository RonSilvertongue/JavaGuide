### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

``` c++
#include<algorithm>
#include<iostream>
using namespace std;

class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> numscopy = nums;
        sort(numscopy.begin(),numscopy.end());
        for(int i=0;i<numscopy.size();i++){
            int temp = numscopy[i];
            for(int j=i+1;j<numscopy.size();j++){
                int result = temp+numscopy[j];
                if(result>target)
                    break;
                else if(result<target)
                    continue;
                else{
                    vector<int> r;
                    r.push_back(indexOf(numscopy[i],nums,-99999999));
                    r.push_back(indexOf(numscopy[j],nums,numscopy[i]));
                    cout<<numscopy[i]<<"   "<<numscopy[j];
                    return r;
                }
            }
        }
        return nums;
    }
    int indexOf(int x,vector<int>& nums,int pre){
        bool flag=false;
        if(x==pre)
            flag=true;
        for(int i=0;i<nums.size();i++){
            if(x==nums[i]&&!flag){
                return i;
            }
            else if(x==nums[i]&&flag){
                flag=false;
                continue;
            }
        }
        return 0;
    }
};
```



### [7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)

``` c++
#include <algorithm>
#include <iostream>
#include <string>
using namespace std;
class Solution {
public:
    int min = -2147483648;
    int max = 2147483647;
    bool isN = false;
    int reverse(int x) {
        if(x==0)
            return 0;
        string str = toString(x);

        cout<<str<<"\n";

        if(isN){
            str = "-"+str;
        }

        long temp = stol(str);
        cout<<"获取反转后的数："<<temp<<"\n";

        if(temp<min||temp>max){
            return 0;
        }

        return temp;  
    }

    string toString(int x){
        string res = "";
        if(x<0)
            isN=true;

        long temp = abs(x);
        
        while(temp){
            res += to_string(temp % 10);
            temp = temp/10;
        }
        return res;
    }
};
```



### [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)

``` c++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x<0)
            return false;
        if(x==0)
            return true;
        string src = to_string(x);
        int i = 0;
        int j = src.length()-1;
        for(;i<=j;i++,j--){
            if(src[i]==src[j])
                continue;
            else
                return false;
        }

        return true;
    }
};
```



### [13. 罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)

``` C++
class Solution {
public:
    int romanToInt(string s) {
        int length = s.length();
        if(length==1)
            return chooseTheNumber(s[0]);
        
        int result = 0;
        int i=0;
        while(i<length){
            int temp;
            char first = s[i];

            if(i==length-1){
                result += chooseTheNumber(first);
                break;
            }
                
            char second = s[i+1];
            if(first == 'I'&&(second == 'V'||second == 'X')){
                result += getTheNumber(first,second);
                temp = i+2;
                if(temp<length){
                    i+=2;
                    continue;
                } 
                else
                    break;
            }

            if(first == 'X'&&(second == 'L'||second == 'C')){
                result += getTheNumber(first,second);
                temp = i+2;
                if(temp<length){
                    i+=2;
                    continue;
                } 
                else
                    break;
            }

            if(first == 'C'&&(second == 'D'||second == 'M')){
                result += getTheNumber(first,second);
                temp = i+2;
                if(temp<length){
                    i+=2;
                    continue;
                } 
                else
                    break;
            }

            result += chooseTheNumber(first);
            i+=1;
        }

        return result;
        
    }


    int getTheNumber(char first,char second){
        if(second == '0'){
            return chooseTheNumber(first);
        }

        return chooseTheNumber(second)-chooseTheNumber(first);
    }

    int chooseTheNumber(char c){
        switch(c){
            case 'I':return 1;
            case 'V':return 5;
            case 'X':return 10;
            case 'L':return 50;
            case 'C':return 100;
            case 'D':return 500;
            case 'M':return 1000;
        }
        return 0;
    }
};
```

