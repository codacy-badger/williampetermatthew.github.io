---
title: 平衡树
date: 2019-01-03 11:50:54
tags: 
  - 数据结构
---

平衡树的一些操作：  
- 查询x的排名
- 查询第x的数
- 查询x的前驱/后继
- 翻转给定区间
- 查询最大/最小的数
- 在某个位置插入x
- 在某个位置插入一串数
- 删除某个位置的x
- 删除某个位置开始的一串数
- 修改某个位置开始的一串数为x
- 查询区间和/本质不同的数值个数
- 将数x移至开头/末尾
- 将数与他的前驱/后继换位
- 进行某个特殊操作：
	- 求和最大的子列
    - 在原数列的第i个元素所有新元素后面添加一个新元素
    - 查询相邻/所有两个元素中差值（绝对值）的最小值
    - ······
- ······

# 正常版本
## 普通平衡树
Treap
```cpp
#include<bits/stdc++.h>
using namespace std;
const int inf=1e9;
int ch[1000005][2],cnt[1000005],size[1000005],key[1000005],rd[1000005];
int sz,n,root;
int newpoint(int x)
{
	sz++;
	key[sz]=x;
	rd[sz]=rand();
	size[sz]=1;
	cnt[sz]=1;
	return sz;
}
void pushup(int x)
{
	if(x)
	{
		size[x]=cnt[x];
		if(ch[x][0])size[x]+=size[ch[x][0]];
		if(ch[x][1])size[x]+=size[ch[x][1]];
	}
}
void build()
{
	root=newpoint(-inf),ch[root][1]=newpoint(inf);
	pushup(root);
}
void rotate(int &x,int d)
{
	int son=ch[x][d^1];
	ch[x][d^1]=ch[son][d];
	ch[son][d] = x;
	pushup(x),pushup(son);
	x=son;
}
void insert(int &x,int val)
{
	if(!x)
	{
		x=newpoint(val);
		return ;
	}
	if(key[x]==val)
	{
		cnt[x]++;
		pushup(x);
		return ;
	}
	int d=key[x]<val;
	insert(ch[x][d],val);
	if(rd[x]< rd[ch[x][d]])
		rotate(x,d^1);
	pushup(x);
}
void del(int &x,int val)
{
	if(!x)
		return ;
	if(key[x]==val)
	{
		if(cnt[x]>1)
		{
			cnt[x]--;
			pushup(x);
			return ;
		}
		bool d=rd[ch[x][0]]>rd[ch[x][1]];
		if(ch[x][0]||ch[x][1])
		{
			if(!ch[x][1]||d)
				rotate(x,1),del(ch[x][1],val);
			else
				rotate(x,0),del(ch[x][0],val);
			pushup(x);
		}
		else x=0;
	}
	else
	{
		del(ch[x][key[x]<val],val);
		pushup(x);
	}
}
int rnk(int x,int val)
{
	if(!x)return 0;
	if(key[x]==val)
		return size[ch[x][0]]+1;
	if(key[x]>val)
		return rnk(ch[x][0],val);
	return rnk(ch[x][1],val)+size[ch[x][0]]+cnt[x];
}
int kth(int x,int k)
{
	if(!x)return inf;
	if(k<=size[ch[x][0]])
		return kth(ch[x][0],k);
	if(k<=size[ch[x][0]]+cnt[x])
		return key[x];
	return kth(ch[x][1],k-size[ch[x][0]]-cnt[x]);
}
int pre(int x,int val)
{
	int res=-inf;
	while(x)
	{
		if(key[x]>=val)
			x=ch[x][0];
		else
			res=key[x],x=ch[x][1];
	}
	return res;
}
int next(int x,int val)
{
	int res=inf;
	while(x)
	{
		if(key[x]<=val)
			x=ch[x][1];
		else
			res=key[x],x=ch[x][0];
	}
	return res;
}
signed main()
{
	srand(time(0)),srand(rand()),srand(rand()+19260817),srand(time(0)+rand()),srand(rand()*rand()),srand(rand()+359003647);
	scanf("%d",&n);
	build();
	for(int i=1,opt,x;i<=n;i++)
	{
		scanf("%d%d",&opt,&x);
		switch(opt)
		{
			case 1:insert(root,x);break;
			case 2:del(root,x);break;
			case 3:printf("%d\n",rnk(root,x)-1);break;
			case 4:printf("%d\n",kth(root,x+1));break;
			case 5:printf("%d\n",pre(root,x));break;
			case 6:printf("%d\n",next(root,x));break;
		}
	}
}

```
Splay
```cpp
#include<bits/stdc++.h>
using namespace std;
int ch[100005][2],f[100005],size[100005],cnt[100005],key[100005];
int sz,root,n;
void clear(int x)
{
    ch[x][0]=ch[x][1]=f[x]=size[x]=cnt[x]=key[x]=0;
}
bool get(int x)
{
    return ch[f[x]][1]==x;
}
void pushup(int x)
{
    if(x)
    {
        size[x]=cnt[x];
        if(ch[x][0])size[x]+=size[ch[x][0]];
        if(ch[x][1])size[x]+=size[ch[x][1]];
    }
}
void rotate(int x)
{
    int old=f[x],oldf=f[old],whichx=get(x);
    ch[old][whichx]=ch[x][whichx^1];
    f[ch[old][whichx]]=old;
    ch[x][whichx^1]=old;
    f[old]=x;
    f[x]=oldf;
    if(oldf)
        ch[oldf][ch[oldf][1]==old]=x;
    pushup(old);
    pushup(x);
}
void splay(int x)
{
    for(int fa;fa=f[x];rotate(x))
        if(f[fa])
            rotate((get(x)==get(fa))?fa:x);
    root=x;
}
void insert(int x)
{
    if(root==0)
    {
        sz++;
        ch[sz][0]=ch[sz][1]=f[sz]=0;
        root=sz;
        size[sz]=cnt[sz]=1;
        key[sz]=x;
        return ;
    }
    int now=root,fa=0;
    while(1)
    {
        if(x==key[now])
        {
            cnt[now]++;
            pushup(now);
            pushup(fa);
            splay(now);
            break;
        }
        fa=now;
        now=ch[now][key[now]<x];
        if(now==0)
        {
            sz++;
            ch[sz][0]=ch[sz][1]=0;
            f[sz]=fa;
            size[sz]=cnt[sz]=1;
            ch[fa][key[fa]<x]=sz;
            key[sz]=x;
            pushup(fa);
            splay(sz);
            break;
        }
    }
}
int rnk(int x)
{
    int now=root,ans=0;
    while(1)
    {
        if(x<key[now])
            now=ch[now][0];
        else
        {
            ans+=(ch[now][0]?size[ch[now][0]]:0);
            if(x==key[now])
            {
                splay(now);
                return ans+1;
            }
            ans+=cnt[now];
            now=ch[now][1];
        }
    }
}
int kth(int x)
{
    int now=root;
    while(1)
    {
        if(ch[now][0]&&x<=size[ch[now][0]])
            now=ch[now][0];
        else
        {
            int tmp=(ch[now][0]?size[ch[now][0]]:0)+cnt[now];
            if(x<=tmp)return key[now];
            x-=tmp;
            now=ch[now][1];
        }
    }
}
int pre()
{
    int now=ch[root][0];
    while(ch[now][1])now=ch[now][1];
    return now;
}
int next()
{
    int now=ch[root][1];
    while(ch[now][0])now=ch[now][0];
    return now;
}
void del(int x)
{
    int whatever=rnk(x);
    if(cnt[root]>1)
    {
        cnt[root]--;
        pushup(root);
        return ;
    }
    if(!ch[root][0]&&!ch[root][1])
    {
        clear(root);
        root=0;
        return ;
    }
    if(!ch[root][0])
    {
        int oldroot=root;
        root=ch[root][1];
        f[root]=0;
        clear(oldroot);
        return ;
    }
    else if(!ch[root][1])
    {
        int oldroot=root;
        root=ch[root][0];
        f[root]=0;
        clear(oldroot);
        return ;
    }
    int leftbig=pre(),oldroot=root;
    splay(leftbig);
    ch[root][1]=ch[oldroot][1];
    f[ch[oldroot][1]]=root;
    clear(oldroot);
    pushup(root);
}
int main()
{
    scanf("%d",&n);
    for(int i=1,opt,x;i<=n;i++)
    {
        scanf("%d%d",&opt,&x);
        switch(opt)
        {
            case 1: insert(x);                                  break;
            case 2: del(x);                                     break;
            case 3: printf("%d\n",rnk(x));                      break;
            case 4: printf("%d\n",kth(x));                      break;
            case 5: insert(x);printf("%d\n",key[pre()]); del(x);break;
            case 6: insert(x);printf("%d\n",key[next()]);del(x);break;
        }
    }
}
```
## 文艺平衡树
```cpp
#include<bits/stdc++.h>
using namespace std;
const int inf=1e9;
int ch[100005][2],f[100005],size[100005],cnt[100005],key[100005],tag[100005],data[100005];
int sz,root,n,m,x,y;
bool get(int x)
{
    return ch[f[x]][1]==x;
}
void pushup(int x)
{
    size[x]=size[ch[x][0]]+size[ch[x][1]]+1;
}
void pushdown(int x)
{
    if(x&&tag[x])
    {
        tag[ch[x][0]]^=1;
        tag[ch[x][1]]^=1;
        swap(ch[x][0],ch[x][1]);
        tag[x]=0;
    }
}
void rotate(int x)
{
    int old=f[x],oldf=f[old],whichx=get(x);
    pushdown(old);
	pushdown(x);
    ch[old][whichx]=ch[x][whichx^1];
	f[ch[old][whichx]]=old;
    ch[x][whichx^1]=old;
	f[old]=x;
    f[x]=oldf;
    if(oldf)
		ch[oldf][ch[oldf][1]==old]=x;
    pushup(old);
	pushup(x);
}
void splay(int x,int goal)
{
    for(int fa;(fa=f[x])!=goal;rotate(x))
        if(f[fa]!=goal)
            rotate((get(x)==get(fa))?fa:x);
    if(!goal)root=x;
}
int build_tree(int fa,int l,int r)
{
    if(l>r)return 0;
    int mid=(l+r)>>1,now=++sz;
    key[now]=data[mid];
	f[now]=fa;
	tag[now]=0;
    ch[now][0]=build_tree(now,l,mid-1);
    ch[now][1]=build_tree(now,mid+1,r);
    pushup(now);
    return now;
}
int rnk(int x)
{
    int now=root;
    while(1)
    {
        pushdown(now);
        if(x<=size[ch[now][0]])
			now=ch[now][0];
        else
        {
            x-=size[ch[now][0]]+1;
            if(!x)return now;
            now=ch[now][1];
        }
    }
}
void turn(int l,int r)
{
    l=rnk(l);
    r=rnk(r+2);
    splay(l,0);
    splay(r,l);
    pushdown(root);
    tag[ch[ch[root][1]][0]]^=1;
}
void write(int now)
{
    pushdown(now);
    if(ch[now][0])write(ch[now][0]);
    if(key[now]!=-inf&&key[now]!=inf)printf("%d ",key[now]);
    if(key[ch[now][1]])write(ch[now][1]);
}
int main()
{
    scanf("%d%d",&n,&m);
    for(int i=1;i<=n;i++)data[i+1]=i;
    data[1]=-inf;data[n+2]=inf;
    root=build_tree(0,1,n+2);
    for(int i=1;i<=m;i++)
    {
        scanf("%d%d",&x,&y);
        turn(x,y);
    }
    write(root);
    return 0;
}
```
# 平板电视（pb_ds）版本
## 普通平衡树
```cpp
#include<bits/stdc++.h>
#include<bits/extc++.h>
using namespace std;
using namespace __gnu_pbds;
tree<long long,null_type,less<long long>,rb_tree_tag,tree_order_statistics_node_update>t;
int n;
int main()
{
    cin>>n;
    for(int i=1,opt;i<=n;i++)
    {
        long long x;
        cin>>opt>>x;
        if(opt==1)
            t.insert((x<<20)+i);
        if(opt==2)
            t.erase(t.lower_bound(x<<20));
        if(opt==3)
            cout<<(t.order_of_key(x<<20)+1)<<endl;
        if(opt==4)
            cout<<(*t.find_by_order(x-1)>>20)<<endl;
        if(opt==5)
            cout<<(*--t.lower_bound(x<<20)>>20)<<endl;
        if(opt==6)
            cout<<(*t.lower_bound((x+1)<<20)>>20)<<endl;
    }
}
```
## 文艺平衡树
```cpp
#include<bits/stdc++.h>
#include<bits/extc++.h>
using namespace std;
using namespace __gnu_cxx;
rope<int>str,rstr;
int n,m;
int main()
{
    cin>>n>>m; 
    for(int i=1;i<=n;i++)
    {
    	str.append(i),rstr.append(n-i+1);
    }
    for(int i=1,l,r,st,ed;i<=m;i++)
    {
        cin>>l>>r;
        st=l-1,ed=r;
        if(st>=ed)continue;
        rope<int>tmp=str.substr(st+str.begin(),ed+str.begin());
        str=str.substr(0+str.begin(),st+str.begin())+rstr.substr(n-ed+rstr.begin(),n-st+rstr.begin())+str.substr(ed+str.begin(),str.length()+str.begin());
        rstr=rstr.substr(rstr.begin(),n-ed+rstr.begin())+tmp+rstr.substr(n-st+rstr.begin(),rstr.length()+rstr.begin());
    }
    for(int i=0;i<str.length();i++)
    {
    	printf("%d ",str[i]);
    }
    return 0;
}
```

------------

> [![知识共享许可协议](https://t1.picb.cc/uploads/2018/09/06/JW1a8i.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://t1.picb.cc/uploads/2018/09/06/JW1bXL.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。