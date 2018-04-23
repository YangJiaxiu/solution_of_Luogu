序：

这题是我看自己太~~颓废~~，为了缓解放假之后一题没做的尴尬，决定不再~~颓废~~，认真写题qwq

---

### 20分做法：

$dfs$，把每棵花都枚举一遍，没了。

   有鉴于~~本人太弱，暴力都打错还不要脸地在讨论区发帖求救~~，我们来研究其他方法：

---

dp：~~搬一下dalao的思路 @yangkai~~

用$f_{i, 1}$来存：在$i$之前（不包括$i$）$h$下降的次数，进而得出：“波谷”的个数

用$f_{i, 0}$来存：在$i$之前（不包括$i$）$h$上升的次数，进而得出：“波峰”的个数

这里有一份代码（@yangkai），我稍微解释一下：

```c++
if(a[i]>a[i-1])f[i][0]=f[i-1][1]+1;
else f[i][0]=f[i-1][0];
/*如果这个数列在上升，
  则f[i][0] = 之前上升时期的波峰个数+当前“转弯”为上升起点的第一个（也就是1）*/
if(a[i]<a[i-1])f[i][1]=f[i-1][0]+1;
else f[i][1]=f[i-1][1];
/*如果在下降，
  则：f[i][1] = 之前下降的波谷个数+当前在下降的第一个*/
```

---

接下来是题解区另一个dalao的思路：**贪心**

80分代码（错误示范）：

```c++
#include<iostream>
#include<cstdio>

const int MaxN = 100000 + 10;

int n, ans = 0, h[MaxN] = {0};

int main() {
    std::cin >> n;
    for (int i = 1; i <= n; ++i) std::cin >> h[i];
    for (int i = 2; i < n; ++i) {
        if (((h[i] > h[i - 1]) && (h[i] > h[i + 1])) || ((h[i] < h[i - 1]) && (h[i] < h[i + 1]))) ans++;
    }
    std::cout << ans + 2;
    return 0;
}
```

为什么这样做不可行呢？因为（个人xjb猜的）：假设有这样一组数据：

5

4 3 3 3 5

那么这个程序在**从2到$n - 1$**什么都找不出来，$ans$加上 2**直接输出**

然而，我们可以看出，Ans = 3（序列4， 3， 5），

怎么办呢？

AC代码：

```c++
#include<iostream>
#include<cstdio>

const int MaxN = 100000 + 10;

int n, ans = 1, h[MaxN] = {0};
bool isMono = false;//true表示现在在单调上升中，false表示现在单调下降中

int main() {
    std::cin >> n;
    for (int i = 1; i <= n; ++i) std::cin >> h[i];
    
    if (h[2] >= h[1]) isMono = true;
    for (int i = 1; i <= n; ++i) {
        if (i == n && isMono == false) {
            ans++;
            break;
        }
        if ((isMono == true) && (h[i + 1] < h[i])) {
            isMono = false;
            ans++;
            continue;
        }
        if ((isMono == false) && (h[i + 1] > h[i])) {
            isMono = true;
            ans++;
            continue;            
        }
    }
    std::cout << ans;    
    return 0;
}
```

上面这份代码个人感觉是有点$DP$思想在里面的，把刚才那种一个点和左右之间进行比较 变成了 一个点“延续的”跟其他点进行比较，（意识流不怎么清新，看不懂的可以私信我）

以上

