# 字典树

## 简介

    又称单词查找树，Trie树，典型应用是用于* 统计 *，排序和保存大量的字符串，所以经常被搜索引擎系统用于文本词频统计。
    比如，给定N个单词，需要按照字典序输出。就可以使用数组的方式创建字典树，然后先序遍历输出。

## 特征

    根节点不包含字符，除根节点外每一个节点都只包含一个字符； 从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串； 每个节点的所有子节点包含的字符都不相同。

## 数据结构

~~~
#define MAX 26//字符集大小
typedef struct TrieNode
{
    int nCount;//记录该字符出现次数
    struct TrieNode* next[MAX];
}TrieNode;

// 字典树只涉及到增加节点，使用数组建立资源池使用更方便。
TrieNode Memory[1000000];  
~~~

## 代码实现

~~~
int allocp=0;
 
/*初始化*/
void InitTrieRoot(TrieNode* *pRoot)
{
    *pRoot=NULL;
}
 
/*创建新结点*/
TrieNode* CreateTrieNode()
{
    int i;
    TrieNode *p;
    p=&Memory[allocp++];
    p->nCount=1;
    for(i=0;i<MAX;i++)
    {
        p->next[i]=NULL;
    }
    return p;
}
 
/*插入*/
void InsertTrie(TrieNode* *pRoot,char *s)
{
    int i,k;
    TrieNode*p;
    if(!(p=*pRoot))
    {
        p=*pRoot=CreateTrieNode();
    }
    i=0;
    while(s[i])
    {
        k=s[i++]-'a';//确定branch
        if(!p->next[k])
            p->next[k]=CreateTrieNode();
                else
            p->next[k]->nCount++;
        p=p->next[k];
    }
}
 
//查找
int SearchTrie(TrieNode* *pRoot,char *s)
{
    TrieNode *p;
    int i,k;
    if(!(p=*pRoot))
    {
        return0;
    }
    i=0;
    while(s[i])
    {
        k=s[i++]-'a';
        if(p->next[k]==NULL) return 0;
        p=p->next[k];
    }
    return p->nCount;
}
~~~