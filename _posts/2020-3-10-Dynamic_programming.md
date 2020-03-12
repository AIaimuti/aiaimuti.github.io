---
layout:     post
title:      动态规划
subtitle:   基础内容
date:       2020-3-10
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - 数据结构及算法
---

## 动态规划问题概述

动态规划问题的一般形式就是求最值，求最值最直观的方法是穷举，动态规划核心问题是穷举，然后在其中找最值。动态规划问题有三个要素：

1.**重叠子问题**

动态规划需要解决的原问题往往存在「重叠子问题」，即原问题暴力穷举时会重复计算子问题，这些重复计算会大幅降低运算效率，其「重叠子问题」可以通过「备忘录」或者「DP table」来优化穷举过程，避免不必要的计算。

2.**最优子结构**

动态规划问题是将原问题拆分为子问题进行求解，其子问题一定会具备「最优子结构」，才能通过子问题的最值得到原问题的最值。

3.**状态转移方程**

状态转移方程在于如何将原问题拆分为子问题进行求解，即根据 dp(i)和 dp(i-1) 的关系得出状态转移方程，只有列出正确的「状态转移方程」才能正确地穷举。

**动态规划的一般解题步骤为：**


1.确定「状态」，明确原问题和子问题中变化的变量；
2.定义 dp 数组/函数，明确 dp(i)应该表示什么（二维情况：dp(i)(j)）；
3.确定初始条件，如 dp(0)dp(0)。
4.根据 dp(i)和 dp(i-1)的关系得出状态转移方程；

## 典型例子

**<a href="https://leetcode-cn.com/problems/coin-change/">零钱兑换（Leetcode 322）</a>**

先看下题目：给你 k 种面值的硬币，面值分别为 c1, c2 ... ck，每种硬币的数量无限，再给一个总金额 amount，问你最少需要几枚硬币凑出这个金额，如果不可能凑出，算法返回 -1 。算法的函数签名如下：

`int coinChange(int[] coins, int amount);`

比如说 k = 3，面值分别为 1，2，5，总金额 amount = 11。那么最少需要 3 枚硬币凑出，即 11 = 5 + 5 + 1。

你认为计算机应该如何解决这个问题？显然，就是把所有肯能的凑硬币方法都穷举出来，然后找找看最少需要多少枚硬币。

1. 暴力递归

首先，这个问题是动态规划问题，因为它具有「最优子结构」的。要符合「最优子结构」，子问题间必须互相独立。啥叫相互独立？你肯定不想看数学证明，我用一个直观的例子来讲解。

比如说，你的原问题是考出最高的总成绩，那么你的子问题就是要把语文考到最高，数学考到最高…… 为了每门课考到最高，你要把每门课相应的选择题分数拿到最高，填空题分数拿到最高…… 当然，最终就是你每门课都是满分，这就是最高的总成绩。

得到了正确的结果：最高的总成绩就是总分。因为这个过程符合最优子结构，“每门科目考到最高”这些子问题是互相独立，互不干扰的。

但是，如果加一个条件：你的语文成绩和数学成绩会互相制约，此消彼长。这样的话，显然你能考到的最高总成绩就达不到总分了，按刚才那个思路就会得到错误的结果。因为子问题并不独立，语文数学成绩无法同时最优，所以最优子结构被破坏。

回到凑零钱问题，为什么说它符合最优子结构呢？比如你想求 amount = 11 时的最少硬币数（原问题），如果你知道凑出 amount = 10 的最少硬币数（子问题），你只需要把子问题的答案加一（再选一枚面值为 1 的硬币）就是原问题的答案，因为硬币的数量是没有限制的，子问题之间没有相互制，是互相独立的。

那么，既然知道了这是个动态规划问题，就要思考如何列出正确的状态转移方程？

**先确定「状态」，也就是原问题和子问题中变化的变量。**由于硬币数量无限，所以唯一的状态就是目标金额 amount。

**然后确定 dp 函数的定义：**当前的目标金额是 n，至少需要 dp(n) 个硬币凑出该金额。

**然后确定「选择」并择优，**也就是对于每个状态，可以做出什么选择改变当前状态。具体到这个问题，无论当的目标金额是多少，选择就是从面额列表 coins 中选择一个硬币，然后目标金额就会减少：

```
def coinChange(coins: List[int], amount: int):
    # 定义：要凑出金额 n，至少要 dp(n) 个硬币
    def dp(n):
        # 初始条件
        if n == 0: return 0
        if n < 0: return -1
        # 求最小值，所以初始化为正无穷
        res = float('INF')
        # 状态转移，做选择，选择需要硬币最少的那个结果
        for coin in coins:
            subproblem = dp(n - coin)
            # 子问题无解，跳过
            if subproblem == -1: continue
            res = min(res, 1 + subproblem)
        return res if res != float('INF') else -1
    # 我们要求的问题是 dp(amount)
    return dp(amount)
```
以上算法已经是暴力解法了，以上代码的数学形式就是状态转移方程,状态转移方程如下所示：

<a href="https://www.codecogs.com/eqnedit.php?latex=f(n)&space;\begin{cases}&space;0,&space;&&space;n&space;=&space;0&space;\\&space;-1,&space;&&space;n&space;<&space;0&space;\\&space;min(dp(n&space;-&space;coins)&space;&plus;&space;1|&space;coin&space;\in&space;coins),&space;&&space;n&space;>&space;0&space;\\&space;\end{cases}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f(n)&space;\begin{cases}&space;0,&space;&&space;n&space;=&space;0&space;\\&space;-1,&space;&&space;n&space;<&space;0&space;\\&space;min(dp(n&space;-&space;coins)&space;&plus;&space;1|&space;coin&space;\in&space;coins),&space;&&space;n&space;>&space;0&space;\\&space;\end{cases}" title="f(n) \begin{cases} 0, & n = 0 \\ -1, & n < 0 \\ min(dp(n - coins) + 1| coin \in coins), & n > 0 \\ \end{cases}" /></a>

但是上述算法进行了很多重复性计算的操作，每个子问题中含有一个for循环，复杂度为 O(k)。所以总时间复杂度为 O(n^k)，指数级别。

2. 带备忘录的递归

这些重复性操作可以通过使用一个「备忘录」进行记录来优化：

```
def coinChange(coins: List[int], amount: int):
    # 备忘录
    memo = dict()
    def dp(n):
        # 查备忘录，避免重复计算
        if n in memo: return memo[n]
        if n == 0: return 0
        if n < 0: return -1
        res = float('INF')
        for coin in coins:
            subproblem = dp(n - coin)
            if subproblem == -1: continue
            res = min(res, 1 + subproblem)
        # 记入备忘录
        memo[n] = res if res != float('INF') else -1
        return memo[n]
    return dp(amount)
```
「备忘录」大大减小了子问题数目，消除了子问题的冗余，所以子问题总数不会超过金额数 n，即子问题数目为 O(n)。处理一个子问题的时间不变，仍是 O(k)，所以总的时间复杂度是 O(n)。

3.dp 数组的迭代解法

当然，我们也可以自底向上使用 dp table 来消除重叠子问题，dp 数组的定义和刚才 dp 函数类似，定义也是一样的：

dp[i] = x 表示，当目标金额为 i 时，至少需要 x 枚硬币。

``` C++ 
int coinChange(vector<int>& coins, int amount) {
    // 数组大小为 amount + 1，初始值也为 amount + 1
    vector<int> dp(amount + 1, amount + 1);
    // base case
    dp[0] = 0;
    for (int i = 0; i < dp.size(); i++) {
        // 内层 for 在求所有子问题 + 1 的最小值
        for (int coin : coins) {
            // 子问题无解，跳过
            if (i - coin < 0) continue;
            dp[i] = min(dp[i], 1 + dp[i - coin]);
        }
    }
    return (dp[amount] == amount + 1) ? -1 : dp[amount];
}

```
为什么将 dp 数组的所有元素都初始化为 amount + 1:这是由于 dp[amount] 最大不可能超过 amount（最小面值为 1 元），所以 amount + 1 就是一个无意义的数了。这个用例里面，也就是 dp[0]=dp[1]=dp[2]=dp[3]=4，而由于 dp[3]=4>amount，所以我们返回 -1

**股票问题**

**<a href="https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/">买卖股票的最佳时机 （Leetcode 121）</a>**

**<a href="https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/">买卖股票的最佳时机II （Leetcode 122）</a>**

**<a href="https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/">买卖股票的最佳时机III （Leetcode 123）</a>**

**<a href="https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/">买卖股票的最佳时机IV （Leetcode 188）</a>**

**<a href="https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/">最佳买卖股票时机含冷冻期 （Leetcode 309）</a>**

**<a href="https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/">买卖股票的最佳时机含手续费 （Leetcode 714）</a>**

对于此类问题，有通用的模板，</br>
首先，确认每天股票的「状态」和「选择」，即：
```
for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 择优(选择1，选择2...)
```
这个问题的「状态」有三个，第一个是**天数**，第二个是**允许交易的最大次数**，第三个是**当天的持有状态**。</br>

其中，每天持有状态都有三种「选择」：买入、卖出、无操作，我们用 buy, sell, rest 表示这三种选择。</br>

这三种选择相互之间有些耦合限制：
1.sell 必须在 buy 之后，buy 必须在 sell 之后。
2.rest 包含两种情况：1）一种是 buy 之后的 rest（持有了股票），一种是 sell 之后的 rest（没有持有股票），可以用 1 表示持有，0 表示没有持有。
3.buy操作只能在交易次数 k > 0 的前提下操作。
我们用一个三维数组就可以装下这几种状态的全部组合：
```
dp[i][k][0 or 1]
0 <= i <= n-1, 1 <= k <= K
n 为天数，大 K 为最多交易数
此问题共 n × K × 2 种状态，全部穷举就能搞定。
for 0 <= i < n:
    for 1 <= k <= K:
        for s in {0, 1}:
            dp[i][k][s] = max(buy, sell, rest)
```
而且我们可以用自然语言描述出每一个状态的含义，比如说 dp[3][2][1] 的含义就是：今天是第三天，我现在手上持有着股票，至今最多进行 2 次交易。再比如 dp[2][3][0] 的含义：今天是第二天，我现在手上没有持有股票，至今最多进行 3 次交易。很容易理解，对吧？</br>

我们想求的最终答案是 dp[n - 1][K][0]，即最后一天，最多允许 K 次交易，最多获得多少利润。读者可能问为什么不是 dp[n - 1][K][1]？因为 [1] 代表手上还持有股票，[0] 表示手上的股票已经卖出去了，很显然后者得到的利润一定大于前者。</br>

现在，我们完成了「状态」的穷举，我们开始思考每种「状态」有哪些「选择」，应该如何更新「状态」。只看「持有状态」，可以画个状态转移图。

 ![](https://github.com/AIaimuti/aiaimuti.github.io/blob/master/img/sell_rest.png)
 通过这个图可以很清楚地看到，每种状态（0 和 1）是如何转移而来的。根据这个图，我们来写一下状态转移方程：
 ```
 dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
              max(   选择 rest  ,           选择 sell      )

解释：今天我没有持有股票，有两种可能：
要么是我昨天就没有持有，然后今天选择 rest，所以我今天还是没有持有；
要么是我昨天持有股票，但是今天我 sell 了，所以我今天没有持有股票了。

dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
              max(   选择 rest  ,           选择 buy         )

解释：今天我持有着股票，有两种可能：
要么我昨天就持有着股票，然后今天选择 rest，所以我今天还持有着股票；
要么我昨天本没有持有，但今天我选择 buy，所以今天我就持有股票了。
```
这个解释应该很清楚了，如果 buy，就要从利润中减去 prices[i]，如果 sell，就要给利润增加 prices[i]。今天的最大利润就是这两种可能选择中较大的那个。而且注意 k 的限制，我们在选择 buy 的时候，把 k 减小了 1，很好理解吧，当然你也可以在 sell 的时候减 1，一样的。

现在，我们已经完成了动态规划中最困难的一步：状态转移方程。如果之前的内容你都可以理解，那么你已经可以秒杀所有问题了，只要套这个框架就行了。不过还差最后一点点，就是定义 base case，即最简单的情况。
```
dp[-1][k][0] = 0
解释：因为 i 是从 0 开始的，所以 i = -1 意味着还没有开始，这时候的利润当然是 0 。
dp[-1][k][1] = -infinity
解释：还没开始的时候，是不可能持有股票的，用负无穷表示这种不可能。
dp[i][0][0] = 0
解释：因为 k 是从 1 开始的，所以 k = 0 意味着根本不允许交易，这时候利润当然是 0 。
dp[i][0][1] = -infinity
解释：不允许交易的情况下，是不可能持有股票的，用负无穷表示这种不可能。
```
把上面的状态转移方程总结一下：
```
base case：
dp[-1][k][0] = dp[i][0][0] = 0
dp[-1][k][1] = dp[i][0][1] = -infinity

状态转移方程：
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
```
读者可能会问，这个数组索引是 -1 怎么编程表示出来呢，负无穷怎么表示呢？这都是细节问题，有很多方法实现。现在完整的框架已经完成，下面开始具体化。
本文参考内容
https://leetcode-cn.com/problems/coin-change/solution/dong-tai-gui-hua-tao-lu-xiang-jie-by-wei-lai-bu-ke/
https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/solution/gu-piao-wen-ti-python3-c-by-z1m/
https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/solution/yi-ge-fang-fa-tuan-mie-6-dao-gu-piao-wen-ti-by-l-3/


