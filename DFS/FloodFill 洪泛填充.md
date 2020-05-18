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

