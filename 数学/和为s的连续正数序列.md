## 和为s的连续正数序列

## 题目描述

输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

输入：target = 9\
输出：[[2,3,4],[4,5]]

输入：target = 15\
输出：[[1,2,3,4,5],[4,5,6],[7,8]]

## 思路

1.可使用前缀和方法，但超时了；
2.找规律：  

2个数：x+x+1=target   x=(target-1)/2

3个数：x+x+1+x+2=target   x=(target-1-2)/3

...以此类推

## 代码

```C++
vector<vector<int>> findcontinuousSequence(int target)
{
    vector<vector<int>> res;
    int i=1;
    while(target>0)
    {
        target-=i++;//先减后自增
        if(target>0 && target%i==0) //寻找i个数之和为target的方案
        {
            vector<int> tmp;
            for(int j=0;j<i;j++)
                tmp.push_back(target/i+j);
            res.push_back(tmp);
        }
    }
    reverse(res.begin(),res.end());
    return  res;
    
}
```
