## NP旅游问题

小明目前在做一份毕业旅行的规划。打算从北京出发，分别去若干个城市，然后再回到北京，每个城市之间均乘坐高铁，且每个城市只去一次。由于经费有限，\
希望能够通过合理的路线安排尽可能的省一些路上的花销。给定一组城市和每对城市之间的火车票的价钱，找到每个城市只访问一次并返回起点的最小车费花销。\
城市个数n（1<n≤20，包括北京）

城市间的车票价钱 n行n列的矩阵 m[n][n]

输出最小车费花销 s

4
0 2 6 5
2 0 4 4
6 4 0 2
5 4 2 0

13

## 代码

```C++
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int num;
    cin>>num;
    int dis[22][22];//题目限制了最多有20个城市
    int jh=pow(2,num-1)-1; //以位表示城市，比如4个城市， 000不表示任何城市，001表示1号城市，111表示1,2,3号城市
    vector<vector<int>> dp(num,vector<int>(jh+1,99999)); //dp[j][i]表示由城市j出发经过集合i里的城市回到0号城市的最短路径
    for(int i=0;i<num;i++)
    {
        for(int j=0;j<num;j++)
            cin>>dis[i][j];
    }
    //dp base:
    for(int i=0;i<num;i++)
        dp[i][0]=dis[i][0];//从城市i出发不经过任何城市回到城市0，就等于这个城市和0号城市的距离
    for(int i=1;i<=jh;i++)
    {
        for(int j=0;j<num;j++)
        {
            if(j!=0 && i>>(j-1) & 1==0) 
                continue;
            else
            {
                for(int k=1;k<num;k++)
                {
                    if(i>>(k-1) & 1==0)//非第k个城市
                        continue;
                    else
                    {
                        if(dp[j][i]>(dp[k][i^(1<<(k-1))]+dis[j][k])) //i^(1<<(k-1) 剔除第k个城市
                            dp[j][i]=dp[k][i^(1<<(k-1))]+dis[j][k];
                    }
                }
            }
        }
    }
    cout<<dp[0][jh]<<endl;
    return 0;
}
```
