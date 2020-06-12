## 字典序的第k小数字

给定整数 n 和 k，找到 1 到 n 中字典序第 k 小的数字。

注意：1 ≤ k ≤ n ≤ 10^9。

输入:
n: 13   k: 2

输出:
10

解释:
字典序的排列是 [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9]，所以第二小的数字是 10。

## 思路

根据题意，可以发现这就是按照前缀排序的数字。如给定13,2: 1~13排序后可得到：1 10 11 12 13 2 3 4 5 6 7 8 9

开始以1为前缀，后面可以接0~9，但是给定上界为13，所以只能接0~3，计算每个前缀下可以拥有的子节点个数，与k进行比较，寻找对应的数字。

## 代码

```C++
int getcount(int prefix,int n)
{
    //使用long long 防止溢出
    long long cur=prefix;
    long long next=prefix+1;
    int count=0;
    while(cur<=n)
    {
        long long tmp=n+1;
        if(tmp<next)
            count+=tmp-cur;
        else
            count+=next-cur;
        cur*=10;
        next*=10;
    }
    return count;
}

int findKthNumber(int n,int k)
{
    int p=1,prefix=1;
    while(p<k)
    {
        int count=getcount(prefix,n);
        if(count+p>k) //在范围内
        {
            prefix*=10;
            p++;
        }
        else if(count+p<=k)
        {
            prefix++;
            p+=count; //定位到下一前缀
        }
    }
    return prefix;
}
```
