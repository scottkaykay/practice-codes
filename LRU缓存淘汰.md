# LRU缓存淘汰

LRU是一种缓存淘汰策略，缓存满了，需要删除一些作用不大的内容，用来腾出空间放新内容。LRU(Least Recently Used)认为最近使用的是有用的，\
长时间不用的是没什么价值的，删除内容时会优先删除这些长时间没有用的，而像LFU这样的策略，判断标准是访问次数(频率)。\
LRU实际上是一种数据结构，需要利用它实现O(1)时间的访问，删除操作。

这样一来，只有哈希表和链表符合要求了。哈希表支持随机访问，但删除插入操作不方便，链表可以弥补这个缺点，二者一结合，变成了哈希链表。\
由于存在删除操作，还需要O(1)时间，所以链表每个节点需要知道其前驱和后继，使用双向链表。

## 实现

```C++
struct CacheNode {
  int key;      // 键
  int value;    // 值
  CacheNode *pre, *next;  // 节点的前驱、后继指针
  CacheNode(int k, int v) : key(k), value(v), pre(NULL), next(NULL) {}
};
class LRUCache{
private:
  int size;                     // Maximum of cachelist size.
  CacheNode *head, *tail;
  map<int, CacheNode *> mp;          // Use hashmap to store
public:
  LRUCache(int capacity)
  {
    size = capacity;
    head = NULL;
    tail = NULL;
  }

  int get(int key)
  {
      map<int, CacheNode *>::iterator it = mp.find(key);
      if (it != mp.end())
      {
          CacheNode *node = it -> second;
          remove(node);
          setHead(node);
          return node -> value;
      }
      else
      {
          return -1;
      }
  }

  void set(int key, int value)
  {
      map<int, CacheNode *>::iterator it = mp.find(key);
      if (it != mp.end())
      {
          CacheNode *node = it -> second;
          node -> value = value;
          remove(node);
          setHead(node);
      }
      else
      {
          CacheNode *newNode = new CacheNode(key, value);
          if (mp.size() >= size)
          {
              map<int, CacheNode *>::iterator iter = mp.find(tail -> key);
              remove(tail);
              mp.erase(iter);
          }
          setHead(newNode);
          mp[key] = newNode;
      }
  }

  void remove(CacheNode *node)
  {
      if (node -> pre != NULL)
      {
        node -> pre -> next = node -> next;
      }
      else
      {
        head = node -> next;
      }
      if (node -> next != NULL)
      {
        node -> next -> pre = node -> pre;
      }
      else
      {
        tail = node -> pre;
      }
  }

  void setHead(CacheNode *node)
  {
      node -> next = head;
      node -> pre = NULL;

      if (head != NULL)
      {
        head -> pre = node;
      }
      head = node;
      if (tail == NULL)
      {
        tail = head;
      }
  }
};
int main(int argc, char **argv)
{
    LRUCache *lruCache = new LRUCache(2);
    lruCache -> set(2, 1);
    lruCache -> set(1, 1);
    cout << lruCache -> get(2) << endl;
    lruCache -> set(4, 1);
    cout << lruCache -> get(1) << endl;
    cout << lruCache -> get(2) << endl;
}
```
