## LFU缓存淘汰

这次是按照访问频数来判定无用页面，访问频数越低，优先淘汰，如果频数相同，按照时间顺序排列，越早访问的优先被淘汰。


## 代码

```C++
//总体和LRU很像，为了计算频数，结构体CacheNode引入了count,为了分辨相同count的访问次序，加入了when记录访问的先后，越小表示越早访问。
struct CacheNode{
int key;
int value;
int count;
int when;
CacheNode* pre,*next;
CacheNode(int k,int v):key(k),value(v),count(1),when(0),pre(NULL),next(NULL){}
};
class LFUCache {
private:
    int size;
    CacheNode *head,*tail;
    map<int,CacheNode*> mp;
    int order;
public:
    LFUCache(int capacity) {
        size=capacity;
        head=NULL;
        tail=NULL;
        order=0;
    }
    
    int get(int key) {
        map<int,CacheNode*>::iterator iter=mp.find(key);
        if(iter!=mp.end())
        {
            CacheNode* node=iter->second;
            remove(node);
            sethead(node);
            order++;
            node->count++;
            node->when=order;
            return node->value;
        }
        else
            return -1;
    }
    void remove(CacheNode* node)
    {
        if(node->pre)
            node->pre->next=node->next;
        else
            head=node->next;
        if(node->next)
            node->next->pre=node->pre;
        else
            tail=node->pre;
    }

    void sethead(CacheNode* node)
    {
        node->next=head;
        node->pre=NULL;
        if(head!=NULL)
            head->pre=node;
        head=node;
        if(tail==NULL)
            tail=head;
    }
    void put(int key, int value) {
        if(size==0)
            return;
        map<int,CacheNode*>::iterator iter=mp.find(key);
        order++;
        if(iter!=mp.end())
        {
            CacheNode* node=iter->second;
            node->value=value;
            remove(node);
            sethead(node);
            node->count++;
            node->when=order;
        }
        else
        {
            CacheNode* node=new CacheNode(key,value);
            node->when=order;
            if(mp.size()>=size)
            {
                map<int,CacheNode*>::iterator it=mp.begin();
                map<int,CacheNode*>::iterator todel;
                int minvalue=INT_MAX;
                CacheNode* delnode;
                for(iter=mp.begin();iter!=mp.end();iter++)
                {
                    if(iter->second->count<=minvalue)
                    {
                        if(iter->second->count==minvalue)
                        {
                            if(iter->second->when<delnode->when)
                            {
                                todel=iter;
                                minvalue=iter->second->count;
                                delnode=iter->second;
                            }
                        }
                        else
                        {
                            todel=iter;
                            minvalue=iter->second->count;
                            delnode=iter->second;
                        }
                    }
                }
                mp.erase(todel);
                remove(delnode);

            }
            sethead(node);
            mp[key]=node;
        }
    }
};
```
