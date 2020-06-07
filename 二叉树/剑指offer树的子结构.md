# 树的子结构

## 题目描述

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:\
给定的树 A:

     3
     
    / \
    
   4   5
   
  / \
  
 1   2
 
给定的树 B：

   4 
   
  /
  
 1
 
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

输入：A = [1,2,3], B = [3,1]\
输出：false

输入：A = [3,4,5,1,2], B = [4,1]\
输出：true


## 代码

```C++
//首先找到A中与B根节点值相同的节点，存储在列表中，然后从这些节点出发，和B中节点比对，如果该位置B中节点为NULL,也默认符合条件
vector<TreeNode*> target; //存储和B根节点值相同的A中节点
void dfs(TreeNode* root,TreeNode* B) //寻找A中和B根节点值相同的节点
{
    if(root==NULL)
        return;
    if(root->val==B->val)
        target.push_back(root);
    dfs(root->left,B);
    dfs(root->right,B);
    
}
bool judge(TreeNode* A,TreeNode* B) //判断B是不是A的子结构
{
    if(B==NULL)
        return true;
    if(A==NULL)
        return false; //注意这两个顺序不能倒，这里代表B中位置不为空，而A中位置为空
    if(A->val!=B->val)
        return false;
    if(judge(A->left,B->left) && judge(A->right,B->right)) //对应位置比较
        return true;
    return false;
}
bool issubstructure(TreeNode* A,TreeNode* B)
{
    if(B==NULL)
        return false;
    dfs(A,B);
    for(int i=0;i<taarget.size();i++)
    {
        if(judge(target[i],B))
            return true;
    }
    return false;
}
```
