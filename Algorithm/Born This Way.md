## [原题链接](https://codeforces.com/contest/1148/problem/B)

### 题目概述
>从 A 地飞往 B 地的航班有 n 个，第 i 个航班的起飞时刻为 a[i]，航行时间为 ta。
>从 B 地飞往 C 地的航班有 m 个，第 i 个航班的起飞时刻为 b[i]，航行时间为 tb。
>转机时间忽略不计。
>你可以 ban 掉其中的 k 个航班。
>最大化到达 C 的最早（最小）时刻。
>如果可以让人无法到达 C，输出 -1。

### 题解<br>
根据原题描述，我们取消的航班数可以小于$k$，但题目并不要求我们一定要让目标到达C地，所以我们可以贪心地选择取消$k$趟航班。<br>
首先我们只有两趟路线：A->B, B->C。那么假设我们在A->B中取消$i$趟航班，则在B->C取消的航班数量就为$k - i$。<br>
我们可以枚举每一种取消航班的情况，每一种情况对应一个最小值，我们只要在这些答案中取一个最大的值，对不能达到的情况进行一些特判即可。<br>
接下来的问题就是如何找到这个最小值，由于b数组是升序的，我们可以找到第一个满足$b_j \ge a_i + t_a$的数组元素$b_j$，然后再取消掉$j$之后的$k-i$个航班，我们就可以得到每种情况的最小值：$b_{j+k-i} + tb$ <br>
快速或得第一个满足数组元素$b_j$的方法有两个：二分和双指针.<br>
#### [[双指针]]<br>
```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>

int main()
{
    int n,m,ta,tb,k;
    std::cin>>n>>m>>ta>>tb>>k;
    
    if (k >= m || k>=n) {
        std::cout<< "-1\n";
        return 0;
    }
    
    std::vector<int> a(n), b(m);
    for (int i = 0; i < n; i++) std::cin >> a[i];
    for (int i = 0; i < m; i++) std::cin >> b[i];
    
    int ans = -1;
    for (int i = 0, j = 0; i <= k; i++) {
        while (j < m && b[j] < a[i] + ta) j++;
        if (j + k - i >= m) {
            std::cout << "-1\n";
            return 0;
        }
        ans = std::max(b[j + k - i] + tb, ans);
    }
    
    std::cout << ans << '\n';
    
    return 0;
}
```
#### 二分<br>
```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>

int main()
{
    int n,m,ta,tb,k;
    std::cin>>n>>m>>ta>>tb>>k;
    
    if (k >= m || k>=n) {
        std::cout<< "-1\n";
        return 0;
    }
    
    std::vector<int> a(n), b(m);
    for (int i = 0; i < n; i++) std::cin >> a[i];
    for (int i = 0; i < m; i++) std::cin >> b[i];
    
    int ans = -1;
    for (int i = 0; i <= k; i++) {
        int l = 0, r = m - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (b[mid] >= a[i] + ta) r = mid;
            else l = mid + 1;
        }
        
        if (l + k - i >= m || b[l] < a[i] + ta) {
            std::cout << "-1\n";
            return 0;
        }
        ans = std::max(b[l + k - i] + tb, ans);
    }
    
    std::cout << ans << '\n';
    
    return 0;
}
```