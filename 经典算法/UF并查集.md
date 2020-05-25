# 并查集算法 适用于发现无向图是否有环，拓补排序可发现有向图的环

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

![](https://github.com/scottkaykay/practice-codes/blob/master/screenshots/bingchaji.png)

判断这种「等价关系」非常实用，比如说编译器判断同一个变量的不同引用，比如社交网络中的朋友圈计算等等。

## 思路

使用森林表示图的动态连通性，使用数组来实现功能。\
参考：https://leetcode-cn.com/problems/friend-circles/solution/union-find-suan-fa-xiang-jie-by-labuladong/

## 代码

```C++
class UF{
public:
    UF(int n)  //构造函数初始化
    {
        this->count=n;
        parent=new int[n];
        size=new int[n];
        for(int i=0;i<n;i++)
        {
            parent[i]=i;  // 每个结点初始的父节点都是它自己
            size[i]=1;    // 每个结点起初互不连通，所以节点数量就1个
        }
        
    }
    
    void unionmerge(int p,int q)  //连通函数
    {
        int rootp=find(p);  //向上遍历找到根结点
        int rooq=find(q);
        if(rootp==rootq)   //根结点相同说明他们属于同一棵树，直接返回
            return;
        if(size[rootp]>size[rooq])  //为了树的平衡性考虑，小的树往大的树上连，比较一下两个树的节点数量判断一下
        {
            parent[rooq]=rootp;
            size[rootp]+=size[rootq];
        }
        else
        {
            parent[rootp]=rootq;
            size[rootq]+=size[rootp];
        }
        count--;     //连通后，连通分量减1
    }
    bool connect(int p,int q)  //判断两点是否连通，看看两点的根结点是否一样就行
    {
        if(find(p)==find(q))
            return true;
        return false;
    }
    int counts()  //返回连通分量的个数
    {
        return count;
    }
private:
    int count;   //记录连通分量的个数
    int *parent; //记录节点父节点
    int *size; //记录树的节点个数
    
    int find(int x)  //向上遍历寻找根结点
    {
        while(parent[x]!=x)
        {
            parent[x]=parent[parent[x]];  //路径压缩
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
                uf.unionmerge(i,j);
        }
    }
    return uf.counts();
}
```
684 冗余连接（无向图）

在本问题中, 树指的是一个连通且无环的无向图。

输入一个图，该图由一个有着N个节点 (节点值不重复1, 2, ..., N) 的树及一条附加的边构成。附加的边的两个顶点包含在1到N中间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组。每一个边的元素是一对[u, v] ，满足 u < v，表示连接顶点u 和v的无向图的边。

返回一条可以删去的边，使得结果图是一个有着N个节点的树。如果有多个答案，则返回二维数组中最后出现的边。答案边 [u, v] 应满足相同的格式 u < v。

输入: [[1,2], [1,3], [2,3]]\
输出: [2,3]\
解释: 给定的无向图为:\
  1
 / \
2 - 3

输入: [[1,2], [2,3], [3,4], [1,4], [1,5]]\
输出: [1,4]\
解释: 给定的无向图为:

5 - 1 - 2
    |   |
    4 - 3


```C++
class Solution{
public:
    vector<int> p;
    int find(int x)
    {
        while(x!=p[x])
        {
            p[x]=p[p[x]]; //即把 x 的父节点变为x的爷爷节点,路径压缩
            x=p[x];
        }
        return x;
    
    }
    vector<int> findRedundantConnection(vector<vector<int>>& edges)
    {
        for(int i=0;i<edges.size()+1;i++)
        {
            p.push_back(i);
        }
        for(auto val:edges)
        {
            int u=find(val[0]);
            int v=find(val[1]);
            if(u!=v)
                p[v]=u;
            else
                return val;
        }
        return {}:
    }
};

//使用拓补排序
class Solution:
    def findRedundantConnection(self, edges):
        import collections
        #准备两个列表，一个记录每个节点的度，一个记录每个节点的邻接点
        degree=collections.defaultdict(set)
        neighbors=collections.defaultdict(int)
        for s,t in edges:
            neighbors[s].add(t)
            neighbors[t].add(s)#因为是无向图，所以两边都得记录
            degree[s]+=1
            degree[t]+=1
        degones=[k for k,v in degree.items() if v==1]#取出度为1的节点
        while degones:
            ndegones=[] #保存经过一次拓补排序后度为1的节点
            for s in degones:
                deletes=set()
                for t in neigbors[s]:
                    degree[t]-=1
                    if degree[t]==1:
                        ndegones.append(t)# 如果该点度为1了，那么保存在ndegones列表
                    deletes.add(t)
                for t in deletes:
                    neigbors[t].remove(s)#将要删除的点从图中移除
                del degree[s]
                del neighbors[s]
            degones=ndegones # 更新degOnes为最新一轮的度为1的节点集合
        for s,t in edges[::-1]: #edges的反向列表
            if min(degree[s],degree[t])>1:
                return [s,t]
        return []

```
