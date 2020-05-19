# FloodFill填充

应用场景：颜色填充，扫雷，消消乐等。

## 框架

```C++
void fill(int x,int y)
{
    fill(x-1,y);
    fill(x+1,y);
    fill(x,y+1);
    fill(x,y-1);
}
```

## 颜色填充

一幅以二维整数数组表示的图画，每一个整数表示该图画的像素值大小，数值在0~65535之间。给定一个坐标(sr,sc)表示图像渲染开始的像素值（行列）和一个新的颜色值newcolor.让你重新上色这幅图。返回上色后的图像。

```C++
vector<vector<int>> floodfill(vector<vector<int>>& image,int sr,int sc,int newcolor)
{
    int oldcolor=image[sr][sc];
    fill(image,sr,sc,oldcolor,newcolor);
    return image;
}
void fill(vector<vector<int>>& image,int x,int y,int oldcolor,int newcolor)
{
    if(x<0 || x>=image.size() || y<0 || y>=image[0].size())
        return;
    if(image[x][y]!=oldcolor)
        return;
    if(image[x][y]==-1)
        return; //不走回头路
    //为防止陷入无限递归，此处先置-1
    image[x][y]=-1;
    fill(image,x-1,y,oldcolor,newcolor);
    fill(image,x+1,y,oldcolor,newcolor);
    fill(image,x,y-1,oldcolor,newcolor);
    fill(image,x,y+1,oldcolor,newcolor);
    image[x][y]=newcolor;
}
```
如果只上色边界部分？需要发现边界点和内部点的特点：\
边界上的点至少有一个方向上不是oldcolor,区域内的点四个方向上全是oldcolor,使用visited数组记录访问过的坐标。

```C++
vetctor<vector<bool>> visited(image.size(),vector<bool>(image[0].size(),false));
int fill(vector<vector<int>>& image,int x,int y,int oldcolor,int newcolor)
{
    if(x<0 || x>=image.size() || y<0 || y>=image[0].size())
        return 0;
    if(visited[x][y])
        return 1;
    if(image[x][y]!=oldcolor)
        return0;
    visited[x][y]=true;
    int surround=fill(image,x-1,y,oldcolor,newcolor)+fill(image,x+1,y,oldcolor,newcolor)+fill(image,x,y+1,oldcolor,newcolor)+fill(image,x,y-1,oldcolor,newcolor);
    if(surround<4)
        image[x][y]=newcolor;
    return 1;
}
```
