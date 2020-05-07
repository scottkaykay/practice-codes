# 并查集算法

## 实现的功能

```
class UF{
public:
    void union(int p,int q); //连接两点
    bool connected(int p,int q);// 判断p和q是否连通
    int count(); //返回连通分量的个数

};
```

这里所说的「连通」是一种等价关系，也就是说具有如下三个性质：

1、自反性：节点p和p是连通的。

2、对称性：如果节点p和q连通，那么q和p也连通。

3、传递性：如果节点p和q连通，q和r连通，那么p和r也连通。

比如说之前那幅图，0～9 任意两个不同的点都不连通，调用connected都会返回 false，连通分量为 10 个。

如果现在调用union(0, 1)，那么 0 和 1 被连通，连通分量降为 9 个。

再调用union(1, 2)，这时 0,1,2 都被连通，调用connected(0, 2)也会返回 true，连通分量变为 8 个。

如图：

![]()

判断这种「等价关系」非常实用，比如说编译器判断同一个变量的不同引用，比如社交网络中的朋友圈计算等等。

## 思路

使用森林表示图的动态连通性，使用数组来实现功能。\
参考：https://leetcode-cn.com/problems/friend-circles/solution/union-find-suan-fa-xiang-jie-by-labuladong/

## 代码

```C++
class UF{
public:
    UF(int n)
    {
        this->count=n;
        parent=new int[n];
        size=new int[n];
        for(int i=0;i<n;i++)
        {
            parent[i]=i;
            size[i]=1;
        }
        
    }
    
    void union(int p,int q)
    {
        int rootp=find(p);
        int rooq=find(q);
        if(rootp==rootq)
            return;
        if(size[rootp]>size[rooq])
        {
            parent[rooq]=rootp;
            size[rootp]+=size[rootq];
        }
        else
        {
            parent[rootp]=rootq;
            size[rootq]+=size[rootp];
        }
        count--;
    }
    bool connect(int p,int q)
    {
        if(find(p)==find(q))
            return true;
        return false;
    }
    int count()
    {
        return count;
    }
private:
    int count;   //记录连通分量的个数
    int parent[]; //记录节点父节点
    int size[]; //记录树的节点个数
    
    int find(int x)
    {
        while(parent[x]!=x)
        {
            parent[x]=parent[parent[x]];
            x=parent[x];
        }
        return x;
    }
};
```

```C++
//朋友圈那题
int findCircleNum(int[][] M)
{
    int n=M.size();
    UF uf=new UF(n);
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<i;j++)
        {
            if(M[i][j]==1)
                uf.union(i,j);
        }
    }
    return uf.count();
}
```
