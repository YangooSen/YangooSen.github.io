---
title: DynamicProgramming
katex: true
date: 2024-12-30 21:03:05
excerpt: 动态规划系列问题
tag: ds
---

[【动态规划专题班】ACM总冠军、清华+斯坦福大神带你入门动态规划算法](https://www.bilibili.com/video/BV1xb411e7ww/?spm_id_from=333.880.my_history.page.click&vd_source=6c26f427606a59575440e9bc6cec44af)
01背包来自[动态规划：关于01背包问题，你该了解这些！](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html#_01-%E8%83%8C%E5%8C%85)
```cpp
void test_2_wei_bag_problem1() {
    vector<int> weight = {1, 3, 4};
    vector<int> value = {15, 20, 30};
    int bagweight = 4;

    // 二维数组
    vector<vector<int>> dp(weight.size(), vector<int>(bagweight + 1, 0));

    // 初始化
    for (int j = weight[0]; j <= bagweight; j++) {
        dp[0][j] = value[0];
    }

    // weight数组的大小 就是物品个数
    for(int i = 1; i < weight.size(); i++) { // 遍历物品
        for(int j = 0; j <= bagweight; j++) { // 遍历背包容量
            if (j < weight[i]) dp[i][j] = dp[i - 1][j];
            else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

        }
    }

    cout << dp[weight.size() - 1][bagweight] << endl;
}

int main() {
    test_2_wei_bag_problem1();
}
```
滚动数组来自[动态规划：关于01背包问题，你该了解这些！（滚动数组）](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-2.html#%E4%B8%80%E7%BB%B4dp%E6%95%B0%E7%BB%84-%E6%BB%9A%E5%8A%A8%E6%95%B0%E7%BB%84)
```cpp
void test_1_wei_bag_problem() {
    vector<int> weight = {1, 3, 4};
    vector<int> value = {15, 20, 30};
    int bagWeight = 4;

    // 初始化
    vector<int> dp(bagWeight + 1, 0);
    for(int i = 0; i < weight.size(); i++) { // 遍历物品
        for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    cout << dp[bagWeight] << endl;
}

int main() {
    test_1_wei_bag_problem();
}
```
[最小邮票数](https://www.nowcoder.com/questionTerminal/83800ae3292b4256b7349ded5f178dd1)，[Piggy-Bank](https://blog.csdn.net/qq_63302818/article/details/124694668)都有不可达问题的判断
```cpp
0-1最小背包问题 求解装满背包的前提下使用物品最少的数量
其中：
    邮票总值相当于背包容量，
    邮票的面值相当于物品的重量，
    邮票的数量相当于物品的总价值，
    每张邮票的价值默认为1。
#include<stdio.h>
//dp[i][j]是从[0,i]里挑邮票凑成面值为j的最小数量,如果凑不成就是99999999
int dp[20][105],Max=99999999;
int main(){
	int v[50],N,i,j,M;
	while(scanf("%d",&M)!=EOF){
		for(scanf("%d",&N),i=0;i<N;i++) scanf("%d",&v[i]);
		for(i=0;i<N;i++) dp[i][0]=0;
		for(j=0;j<=M;j++)
			if(j==v[0]) dp[0][j]=1;
			else dp[0][j]=Max;
		for(i=1;i<N;i++)
			for(j=1;j<=M;j++){
				dp[i][j]=dp[i-1][j];
				if(j>=v[i]&&dp[i][j]>dp[i-1][j-v[i]]+1) //注意这里是最小
					dp[i][j]=dp[i-1][j-v[i]]+1;
			}
		printf("%d\n",dp[N-1][M]<Max?dp[N-1][M]:0);
	}
}
```

[416. 分割等和子集](https://programmercarl.com/0416.%E5%88%86%E5%89%B2%E7%AD%89%E5%92%8C%E5%AD%90%E9%9B%86.html)，[1049. 最后一块石头的重量 II](https://programmercarl.com/1049.%E6%9C%80%E5%90%8E%E4%B8%80%E5%9D%97%E7%9F%B3%E5%A4%B4%E7%9A%84%E9%87%8D%E9%87%8FII.html)，[494. 目标和](https://programmercarl.com/0494.%E7%9B%AE%E6%A0%87%E5%92%8C.html)都是从全集中找出子集，使子集的和等于给定数，这类问题可以用背包问题思路解决

[完全背包](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html#%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85)和01背包的区别只有内层背包容量的遍历顺序
```cpp
// 先遍历物品，再遍历背包
void test_CompletePack() {
    vector<int> weight = {1, 3, 4};
    vector<int> value = {15, 20, 30};
    int bagWeight = 4;
    vector<int> dp(bagWeight + 1, 0);
    for(int i = 0; i < weight.size(); i++) { // 遍历物品
        for(int j = weight[i]; j <= bagWeight; j++) { // 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    cout << dp[bagWeight] << endl;
}
int main() {
    test_CompletePack();
}

```

多重背包来自[动态规划：关于多重背包，你该了解这些！](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85.html#%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85)
```cpp
void test_multi_pack() {
    vector<int> weight = {1, 3, 4};
    vector<int> value = {15, 20, 30};
    vector<int> nums = {2, 3, 2};
    int bagWeight = 10;
    for (int i = 0; i < nums.size(); i++) {
        while (nums[i] > 1) { // nums[i]保留到1，把其他物品都展开
            weight.push_back(weight[i]);
            value.push_back(value[i]);
            nums[i]--;
        }
    }

    vector<int> dp(bagWeight + 1, 0);
    for(int i = 0; i < weight.size(); i++) { // 遍历物品
        for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
        for (int j = 0; j <= bagWeight; j++) {
            cout << dp[j] << " ";
        }
        cout << endl;
    }
    cout << dp[bagWeight] << endl;

}
int main() {
    test_multi_pack();
}
```
[多重背包还有二进制优化的方法](https://blog.csdn.net/as3asddd/article/details/50839324?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167385982516800213079932%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=167385982516800213079932&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~pc_rank_34-1-50839324-null-null.142%5Ev71%5Epc_new_rank,201%5Ev4%5Eadd_ask&utm_term=%E7%8F%8D%E6%83%9C%E7%8E%B0%E5%9C%A8,%E6%84%9F%E6%81%A9%E7%94%9F%E6%B4%BB%20%E4%BA%8C%E8%BF%9B%E5%88%B6%E4%BC%98%E5%8C%96&spm=1018.2226.3001.4187)
[518. 零钱兑换 II](https://programmercarl.com/0518.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2II.html#%E6%80%9D%E8%B7%AF)，[494. 目标和](https://programmercarl.com/0494.%E7%9B%AE%E6%A0%87%E5%92%8C.html)，[70. 爬楼梯](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E7%89%88%E6%9C%AC.html)，[吃糖果](https://www.nowcoder.com/questionTerminal/72015680c32b449899e81f1470836097)，[377. 组合总和 Ⅳ](https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html#_377-%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C-iv)，[96.不同的二叉搜索树](https://programmercarl.com/0096.%E4%B8%8D%E5%90%8C%E7%9A%84%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E6%80%9D%E8%B7%AF)，[\[编程题\]整数拆分](https://www.nowcoder.com/questionTerminal/376537f4609a49d296901db5139639ec)，[放苹果](https://www.nowcoder.com/questionTerminal/4f0c1e21010e4d849bde5297148e81d9)都是计数类问题，计数时要注意dp初始化问题，计数时一般用的是
```cpp
dp[j] += dp[j - nums[i]];
```
- 如果求**组合**数就是外层for循环遍历**物品**，内层for遍历**背包**

- 如果求**排列**数就是外层for遍历**背包**，内层for循环遍历**物品**

[746. 使用最小花费爬楼梯](https://programmercarl.com/0746.%E4%BD%BF%E7%94%A8%E6%9C%80%E5%B0%8F%E8%8A%B1%E8%B4%B9%E7%88%AC%E6%A5%BC%E6%A2%AF.html)，[343. 整数拆分](https://programmercarl.com/0343.%E6%95%B4%E6%95%B0%E6%8B%86%E5%88%86.html#%E6%80%9D%E8%B7%AF)，[322. 零钱兑换](https://programmercarl.com/0322.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2.html#_322-%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2)，[474.一和零](https://programmercarl.com/0474.%E4%B8%80%E5%92%8C%E9%9B%B6.html)，[279.完全平方数](https://programmercarl.com/0279.%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0.html#_279-%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0)，吃糖果是典型的背包问题，**在约束（背包大小，数字总和为定值，自定义限定条件等等）的条件下做最优化问题**。[474.一和零](https://programmercarl.com/0474.%E4%B8%80%E5%92%8C%E9%9B%B6.html)和[139.单词拆分](https://programmercarl.com/0139.%E5%8D%95%E8%AF%8D%E6%8B%86%E5%88%86.html#%E6%80%9D%E8%B7%AF)涉及到了字符串拼接。


[337.打家劫舍 III](https://programmercarl.com/0337.%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8DIII.html#_337-%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8D-iii)，[121. 买卖股票的最佳时机](https://programmercarl.com/0121.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA.html)等买卖股票问题的情况是到每一步有多种状态，状态转移的时候要从上一步的m种状态综合推出这一步的m种状态

子序列不需要连续，求解需要遍历。**序列问题dp[i]都是以v[i]作为结尾的求解结果（这个结果可以是最大和，最长长度等）**：[300.最长递增子序列](https://programmercarl.com/0300.%E6%9C%80%E9%95%BF%E4%B8%8A%E5%8D%87%E5%AD%90%E5%BA%8F%E5%88%97.html#_300-%E6%9C%80%E9%95%BF%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97)此步需要用之前的所有步来推出，[拦截导弹](https://www.nowcoder.com/questionTerminal/dad3aa23d74b4aaea0749042bba2358a) 求最长不增子序列，[\[编程题\]最大上升子序列和](https://www.nowcoder.com/questionTerminal/dcb97b18715141599b64dbdb8cdea3bd)，[\[编程题\]合唱队形](https://www.nowcoder.com/questionTerminal/cf209ca9ac994015b8caf5bf2cae5c98)

[718. 最长重复子数组](https://programmercarl.com/0718.%E6%9C%80%E9%95%BF%E9%87%8D%E5%A4%8D%E5%AD%90%E6%95%B0%E7%BB%84.html)和[1143.最长公共子序列](https://programmercarl.com/1143.%E6%9C%80%E9%95%BF%E5%85%AC%E5%85%B1%E5%AD%90%E5%BA%8F%E5%88%97.html)的区别是子数组是连续的，子序列不一定是连续的，求公共部分要想到元素的二维表：
![动态规划](1.png)
[\[编程题\]最大连续子序列](https://www.nowcoder.com/questionTerminal/afe7c043f0644f60af98a0fba61af8e7)，注意dp必须是**以v[i]结尾**的最大连续子序列
```cpp
#include<bits/stdc++.h>
using namespace std;
struct state{
    int start,end,sum;
    state(int s,int e,int s_):start(s),end(e),sum(s_){};
    state(){};
    bool operator>(const state& s)const{
        if (sum!=s.sum) return sum>s.sum;
        else if (start!=s.start) return start<s.start;
        return end<s.end;
    }
};

int main(){
    int n;
    while (cin>>n){
        if (!n) return 0;
        vector<int> v(n);
        for (int i=0;i<n;i++) scanf("%d",&v[i]);
        if (n==1){
            if (v[0]<0) cout << 0 << ' ' << v[0] << ' ' << v[0] << endl;
            else cout << v[0] << ' ' << v[0] << ' ' << v[0] << endl;
            continue;
        }
        //for (auto x:v) cout << x << ' ';
        vector<state> dp(n);    //[v[0],v[i]]且以v[i]结尾的最大子序列
        bool noPositive=v[0]<0?true:false;
        dp[0]=state(0,0,v[0]);
        state max=dp[0];
        for (int i=1;i<n;i++){
            if (v[i]>=0) noPositive=false;
            if (v[i]>=dp[i-1].sum+v[i]){
                dp[i]=state(i,i,v[i]);
            }
            else dp[i]=state(dp[i-1].start,i,dp[i-1].sum+v[i]);
            max=max>dp[i]?max:dp[i];
        }
        //for (auto x:dp) cout << x << ' ';
        if (!noPositive) cout << max.sum << ' ' << v[max.start] << ' ' << v[max.end] << endl;
        else cout << 0 << ' ' << v[0] << ' ' << v[n-1] << endl;
    }
}
```

[最大子矩阵](https://www.nowcoder.com/practice/a5a0b05f0505406ca837a3a76a5419b3?tpId=230&tqId=40416&ru=/exam/oj)，利用最大连续子列和求：最大子矩阵所在行是第i行到j行，遍历每一个行组合，求它们按列求和之后的最大连续子列和
```cpp
#include <cstdio>
#include <iostream>
#include <climits>
#include <cstring>

using namespace std;

const int MAXN = 100 + 10;

const int INF = INT_MAX;

int matrix[MAXN][MAXN];
int dp[MAXN];
int coltotal[MAXN][MAXN];//概率论中“分布函数”的感觉
int arr[MAXN];

int MaxSubsequence(int n){
	fill(dp, dp+n, -INF);
	int ans = -INF;
	for(int i=0; i<n; i++){
		if(i==0){
			dp[i] = arr[i];
		}else{
			dp[i] = max(arr[i], dp[i-1] + arr[i]);
		}
		if(dp[i] > ans){
			ans = dp[i];
		}
	}
	return ans;
}

int MaxSubmatrix(int m, int n){
	for(int j=0; j<n; j++){
		coltotal[0][j] = matrix[0][j];
	}
	for(int i=1; i<m; i++){
		for(int j=0; j<n; j++){
			coltotal[i][j] = matrix[i][j] + coltotal[i-1][j];
		}
	}//至此都是在初始化coltotal，这件事情在答案中是在main里面做的
	int ans = -INF;
	for(int i=0; i<m; i++){//定位到某一行
		for(int j=i; j<m; j++){//也是定位到某一行。
			for(int k=0; k<n; k++){//定位到某一列。本列中i行至j行的元素之和存入arr[k]以使用Subsequence的方法
									//求和的方式：就用到了那个分布函数的思想，coltotal这一完美设计
									//这样子写代码，比书上那个例题答案清晰太多了
				if(i==0){
					arr[k] = coltotal[j][k];
				}else{
					arr[k] = coltotal[j][k] - coltotal[i-1][k];
				}
			}//此时本轮arr的填充结束，可以执行一次MaxSubsequence
			int tmp = MaxSubsequence(n);
			if(tmp > ans){
				ans = tmp;
			}
		}
	}
	return ans;
}

int main(){
	int n;
	while(scanf("%d", &n) != EOF){
		for(int i=0; i<n; i++){
			for(int j=0; j<n; j++){
				scanf("%d", &matrix[i][j]);
			}
		}
		int ans = MaxSubmatrix(n, n);
		printf("%d\n", ans);
	}
	return 0;
}

```

[The Triangle](https://blog.csdn.net/y_universe/article/details/78116768)，[LightOJ 1004 Monkey Banana Problem](https://blog.csdn.net/qinyibut/article/details/78480034?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167386183516782427437680%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=167386183516782427437680&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-3-78480034-null-null.142%5Ev71%5Epc_new_rank,201%5Ev4%5Eadd_ask&utm_term=monkey%20banana%20problem&spm=1018.2226.3001.4187) 都是在正整数中找最大序列和
