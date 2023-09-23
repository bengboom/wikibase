# Leetcode 刷题笔记

- [Leetcode 刷题笔记](#leetcode-刷题笔记)
  - [思维](#思维)
    - [逆向思维](#逆向思维)
    - [关系思维](#关系思维)
    - [数学证明](#数学证明)
    - [反向循环](#反向循环)
    - [从两边往中间](#从两边往中间)
    - [Dynamic Programmning](#dynamic-programmning)
    - [维度上升](#维度上升)

## 思维

### 逆向思维

[*918. Maximum Sum Circular Subarray*](https://leetcode.com/problems/maximum-sum-circular-subarray/)

    求一个环中最长的连续子串
    由于环上数之和一定，先找到不成环（单链）的最大子串[Link](https://leetcode.com/problems/maximum-subarray/)
    除此之外，过连接点的情况：用总和减去最小连续子串即可

### 关系思维

[*116. Populating Next Right Pointers in Each Node*](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

    找到二叉树中一个节点右边的节点
    root->right->next=root->next->left

### 数学证明

[*142.Linked List Cycle II*](https://leetcode.com/problems/linked-list-cycle-ii/)

    找到环的起点
    证明：设环的起点到相遇点的距离为a，相遇点到环起点的距离为b，环的长度为c
    设快指针走了2n，慢指针走了n，快指针比慢指针多走了n
    2n=n+a+b+c+b
    n=a+b
    a+b=c
    a=c-b
    证明完毕
    前文：证明是否成环：快慢指针相遇[Link](https://leetcode.com/problems/linked-list-cycle/)
    后文：找到环的长度：快慢指针相遇后，慢指针继续走，直到再次相遇，计数即可

### 反向循环

[*121.Best time to buy and sell stock*](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

    从后往前遍历，记录最大值，然后计算最大值与当前值差值，差值的最大值即为最大收益

### 从两边往中间

[*42.Trapping Rain Water*](https://leetcode.com/problems/trapping-rain-water/)

    从两边往中间遍历，记录左边最大值和右边最大值，然后计算当前值与左右最大值的差值，差值的最小值即为当前位置的水量，累加水量得到答案。

### Dynamic Programmning

[*309. Best Time to Buy and Sell Stock with Cooldown*](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

```cpp
    int maxProfit(vector<int>& prices) {
        int buy[5001],sell[5001];
        //记录当前位置做当前操作时已经收获的钱
        buy[0]=-prices[0];sell[0]=0;
        for(int i=1;i<prices.size();i++){
            buy[i]=max(buy[i-1],sell[i>1?i-2:0]-prices[i]);
            sell[i]=max(sell[i-1],buy[i-1]+prices[i]);
        }
        return sell[prices.size()-1];
    }
```

[*42. Trapping Rain Water*](https://leetcode.com/problems/trapping-rain-water/?envType=study-plan&id=dynamic-programming-i)

```cpp
    class Solution {
    public:
    int trap(vector<int>& height) {
        int maxleft=0,maxright=0;
        int ans=0;
        int left=0,right=height.size()-1;
        while(left<=right){
            if(height[left]<=height[right]){ //左边可以盛水
                if(height[left]>maxleft){
                    maxleft=height[left];
                }
                else{
                    ans+=maxleft-height[left];
                }
                left++;
            }
            else{ //右边可以盛水
                if(height[right]>maxright){
                    maxright=height[right];
                }
                else{
                    ans+=maxright-height[right];
                }
                right--;
                }
            }
            return ans;
            }
        };
```

[*264. Ugly Number II*](https://leetcode.com/problems/ugly-number-ii/)

```cpp
    class Solution {
    public:
        int nthUglyNumber(int n) {
            int dp[2000];
            dp[0]=1;
            int p2=0,p3=0,p5=0;
            for(int i=1;i<n;i++){
                dp[i]=min(dp[p2]*2,min(dp[p3]*3,dp[p5]*5));
                if(dp[i]==dp[p2]*2) p2++;
                if(dp[i]==dp[p3]*3) p3++;
                if(dp[i]==dp[p5]*5) p5++;
            }
            return dp[n-1];
        }
    };
```

[*96. Unique Binary Search Trees*](https://leetcode.cn/problems/unique-binary-search-trees/?envType=study-plan&id=dynamic-programming-i)

    给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。
    难点：找规律。

```cpp
    class Solution {
    public:
        int numTrees(int n) {
            vector<int> G(n + 1, 0);
            G[0] = 1;
            G[1] = 1;

            for (int i = 2; i <= n; ++i) {
                for (int j = 1; j <= i; ++j) {
                    G[i] += G[j - 1] * G[i - j];
                }
            }
            return G[n];
        }
    };
```

### 维度上升
[*304. Range Sum Query 2D - Immutable*](https://leetcode.com/problems/range-sum-query-2d-immutable/)
    
    一维的时候可以用前缀和来解决问题，二维的前缀和是从(0,0)到(i,j)的和，可以用一个二维数组来存储前缀和，然后计算的时候就是从(0,0)到(i,j)的和减去从(0,0)到(i-1,j)的和减去从(0,0)到(i,j-1)的和加上从(0,0)到(i-1,j-1)的和，这样就可以得到从(i,j)到(i,j)的和了。
