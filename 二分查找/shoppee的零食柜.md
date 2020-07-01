## shoppee的零食柜

 shopee的零食柜，有着各式各样的零食，但是因为贪吃，小虾同学体重日益增加，终于被人叫为小胖了，他终于下定决心减肥了，他决定每天晚上去操场跑两圈，\
 但是跑步太累人了，他想转移注意力，忘记痛苦，正在听着音乐的他，突然有个想法，他想跟着音乐的节奏来跑步，音乐有7种音符，对应的是1到7，那么他对应的\
 步长就可以是1-7分米，这样的话他就可以转移注意力了，但是他想保持自己跑步的速度，在规定时间m分钟跑完。为了避免被累死，他需要规划他每分钟需要跑过的\
 音符，这些音符的步长总和要尽量小。下面是小虾同学听的歌曲的音符，以及规定的时间，你能告诉他每分钟他应该跑多少步长？
 
 输入的第一行输入 n（1 ≤ n ≤ 1000000，表示音符数），m（1<=m< 1000000, m <= n）组成，

第二行有 n 个数，表示每个音符（1<= f <= 7）

输入： 8 5 6 5 6 7 6 6 3 1

输出： 11

## 代码

```C++
#include<bits/stdc++.h>
using namespace std;

int timecheck(vector<int>& times,int step)
{
    int sum=0;
    int time=1;
    for(int i:times)
    {
        if(sum+i>step)
        {
            sum=i;
            time++;
        }
        else
            sum+=i;
    } 
    return time;
}

int main()
{
    int time=0;
    int sum=0;
    int maxvalue=0;
    int n,m;
    int step=0;
    cin>>n>>m;
    vector<int> store(n,0);
    for(int i=0;i<n;i++)
    {
        cin>>store[i];
        maxvalue=max(maxvalue,store[i]);
        sum+=store[i];
    }
    while(maxvalue<sum)
    {
        step=maxvalue+(sum-maxvalue)/2;
        time=timecheck(store,step);
        if(time>m)
            maxvalue=step+1;
        else if(time<m)
            sum=step-1;
        else
            sum=step;
    }
    cout<<maxvalue<<endl;
    return 0;
}
```
