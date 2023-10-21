# A - Building an Aquarium
[題目連結](https://vjudge.net/contest/588756#problem)  
## 題目
You love fish, that's why you have decided to build an aquarium. You have a piece of coral made of n
 columns, the i
-th of which is ai
 units tall. Afterwards, you will build a tank around the coral as follows:

* Pick an integer h≥1
 — the height of the tank. Build walls of height h
 on either side of the tank.
* Then, fill the tank up with water so that the height of each column is h
, unless the coral is taller than h
; then no water should be added to this column.  


For example, with a=[3,1,2,4,6,2,5]
 and a height of h=4
, you will end up using a total of w=8
 units of water, as shown.  
![](https://github.com/archie0732/CPEB1018/blob/main/picture/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202023-10-21%20100534.png)  
You can use at most x
 units of water to fill up the tank, but you want to build the biggest tank possible. What is the largest value of h
 you can select?  
**`input:`**    
The first line contains a single integer t
 (1≤t≤10^4
) — the number of test cases.

The first line of each test case contains two positive integers n
 and x
 (1≤n≤2⋅10^5
; 1≤x≤10^9
) — the number of columns of the coral and the maximum amount of water you can use.

The second line of each test case contains n
 space-separated integers ai
 (1≤a≤10^9
) — the heights of the coral.

The sum of n
 over all test cases doesn't exceed 2⋅10^5
.   
**`output:`**  
For each test case, output a single positive integer h
 (h≥1
) — the maximum height the tank can have, so you need at most x
 units of water to fill up the tank.

We have a proof that under these constraints, such a value of h
 always exists.  
 ### sample
 |input|output|
 |-----|------|
 |5<br>`7 9`<br>`3 1 2 4 6 2 5`<br>3 10<br>1 1 1<br>`4 1`<br>`1 4 3 4`<br>6 1984<br>2 6 5 9 1 8<br>`1 1000000000`<br>`1`|`4`<br>4<br>`2`<br>335<br>`1000000001`|   

 ## 解題
**應先具備以下能力**
* [binary search]()
* [vertor]()
* 知道從底部一層一層的看太慢了會`LTE`(~~MD我-2~~)

**代名詞**  
* `top` 題目給並的最高處(可以用`1e10`或`INT_MAX`)
* `bottom`最低點初始值為1，也是這次的輸出值
* `mid`二分搜尋法的關鍵，用其來測試是否會`超過`或`少於`可使用的水量
* `x`題目給的可使用最大水量
* `use_water`計算這層所需要的水量

解題關鍵:   
一.用二分搜尋法找中心層  
二.用中心層去計算使用的水量  
三.如果用的水量大於允許的水量那就把`新最高點`移到`中心點`====>`新的中心點`就會是(`新的最高點`+最低點)/2+最低點
1. 從上往下看，找到中心點mid(`mid=bottom+(top-bottom)/2`)，用mid計算要多少水(use_water)
2. >if:use_water > x  ==>把`top=mid`    ==> 會在得到新的mid==>再去計算  
   >if:use_water < x  ==>把`bottom=mid` ==>會的到新的mid==>再去計算
    
**如果這樣還看不懂可以直接來問我，但請先至少知道二分搜尋法**
 ## code
 ![](https://github.com/archie0732/CPEB1018/blob/main/picture/anime-7914238_1280.jpg)  
 ```cpp
#include <iostream>
#include <vector>

#define ll long long

using namespace std;

int main()
{

    int t;
    cin >> t;

    while (t--)
    {
        ll n, x, bottom = 1, top = 0;

        cin >> n >> x;
        vector<int> v(n);
        for (ll i = 0; i < n; i++)
        {
            cin >> v[i];
        }

        // binary search
        top = INT_MAX;
        while (bottom < top - 1)
        {
            ll mid = bottom + (top - bottom) / 2;
            ll use_water = 0;
            for (int i = 0; i < n; i++)
            {
                if (mid > v[i])
                {
                    use_water += (mid - v[i]);
                }
            }
            if (use_water > x)
            {
                top = mid;
            }
            else
            {
                bottom = mid;
            }
        }

        cout << bottom << endl;
    }
    return 0;
}
```
 

