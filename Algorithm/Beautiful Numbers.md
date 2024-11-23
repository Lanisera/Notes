## [原题链接](https://codeforces.com/problemset/problem/1265/B)

### 题目概述 <br>
>输入 T(≤1e3) 表示 T 组数据。所有数据的 n 之和 ≤2e5。<br>
>每组数据输入 n(1≤n≤2e5) 和一个 1~n 的排列 p。<br>
>对于每个长度 L=1,2,3,...,n，判断 p 是否存在一个长为 L 的连续子数组，包含 1~L 所有数字。<br>
>输出一个长为 n 的 01 字符串，依次表示 L=1,2,3,...,n 的答案，其中 0 表示 false，1 表示 true。

### 题解 <br>
预处理每数组中每个元素下标，即将数组元素与数组下标进行一个映射。<br>
然后枚举 $i = 1, 2, \ldots, n$ ,在枚举的过程中维护 $1 \sim i$ 中的最大下标$mx$和最小下标$mn$。 <br>
若对于$i$，有$i = mx - mn + 1$则说明原数组中存在这样的连续子数组，只包含$1 \sim i$的所有数。<br>
### 证明<br>
**为什么只要有$i = mx - mn + 1$就满足题目要求?** <br>
首先我们可以比较容易的看出: $i \le mx - mn + 1$。<br>
因为每个元素对应一个下标，而$mx$和$mn$是数组元素$1 \sim i$的最大下标和最小下标，这两下标之间的元素个数必然要大于$i$。<br>
接着用反正法来证明为什么在答案在取等时成立：<br>
假设存在$i \lt mx - mn + 1$, 使得原数组中有这样的连续子数组只包含$1 \sim i$中的所有数<br>
注意这里的修饰词，**连续， 只包含**，说明该数组中只能有$i$个数，通过我们的最大和最小下标计算可得这个连续数组的元素个数大于$i$，与假设冲突，故只有当$i = mx - mn + 1$时满足要求