## n个骰子的点数

把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。

你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。

输入: 1

输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]

输入: 2

输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]

## 思路

考虑使用动态规划，定义dp数组：dp[n][s] 代表n个骰子，和为s的排列总数。

如果不看前n-1个骰子，最后一个骰子可出现1到6的点数，那么:

dp[n][s]=dp[n-1][s-1]+dp[n-1][s-2]+dp[n-1][s-3]+dp[n-1][s-4]+dp[n-1][s-5]+dp[n-1][s-6]

初始情况： dp[1][1]=dp[1][2]=dp[1][3]=dp[1][4]=dp[1][5]=dp[1][6]=1

## 代码

```C++
vector<double> twosum(int n)
{
    vector<double> res(5*n+1,0.0);
    double all=pow(6,n);
    vector<vector<int>> dp(n+1,vector<int>(6*n+1,0));
    for(int i=1;i<=6;i++)
        dp[1][i]=1;
    for(int i=1;i<=n;i++)
    {
        for(int j=i;j<=6*n;j++)
        {
            for(int  k=1;k<=6;k++)
            {
                if(j>=k)
                    dp[i][j]+=dp[i-1][j-k];
                if(i==n)
                    res[j-i]=dp[i][j]/all;
            }
        }
    }
    return res;
}
```

```C++
//降维
vector<double> twosum(int n)
{
    vector<double> res(5*n+1,0.0);
    double all=pow(6,n);
    vector<int> dp(6*n+1,0);
    for(int i=1;i<=6;i++)
    {
        dp[i]=1;
        res[i-1]=1.0/6;
    }
    
    for(int i=2;i<=n;i++)
    {
        for(int j=6*n;j>=1;j--)
        {
            int temp=0;
            for(int k=1;k<=6;k++)
            {
                if(j>=k)
                    temp+=dp[j-k];
                
            }
            dp[j]=temp;
            if(i==n && j>=n)
                res[j-i]=dp[j]/all; 
        }
    }
    
    return res;
}
```
