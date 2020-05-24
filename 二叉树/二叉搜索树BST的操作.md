# 二叉搜索树BST的操作

无非包括遍历，增删查改这些操作，主要注意验证合法性和删除操作。

## 遍历

比如把所有节点的值加1：
```C++
void plusone(TreeNode* root)
{
    if(root==NULL)
        return;
    root->val++;
    plusone(root->left);
    plusone(root->right);
}
```

## 判断两棵二叉树是否相同

```C++
bool issame(TreeNode* root1,TreeNode* root2)
{
    if(root1==NULL && root2==NULL)
        return true;
    if(root1==NULL || root2==NULL)
        return false;
    if(root1->val!=root2->val)
        return false;
    return issame(root1->left,root2->left) && issame(root1->right,root2->right);
}
```

## 判断二叉搜索树BST的合法性

```C++
bool isvalidBST(TreeNode* root)
{
    return isvalidBST(root,NULL,NULL);
}

bool isvalidBST(TreeNode* root,TreeNode* min,TreeNode* max)
{
    if(root==NULL)
        return true;
    if(min!=NULL && root->val<=min->val) return false;
    if(max!=NULL && root->val>=max->val) return false;
    return isvalidBST(root->left,min,root) && isvalidBST(root->right,root,max);//相当于根节点和左右子树所有的节点比
    
}
```
## 在BST中查找一个数是否存在

```C++
bool isinBST(TreeNode* root,int target)
{
    if(root==NULL)
        return false;
    if(root->val==target)
        return true;
    if(root->val<target)
        return isinBST(root->right,target);
    if(root->val>target)
        return isin BST(root->left,target);
}
```

## 在BST中插入一个数

```C++
TreeNode* insertintoBST(TreeNode* root,int val)
{
    

}

```
