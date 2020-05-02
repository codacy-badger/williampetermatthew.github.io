---
title: 排序
date: 2018-10-09 20:00:10	
tags: 
  - 算法
---
# 插入排序
插入排序分为直接插入排序、二分插入排序、二路插入排序、表插入排序和希尔插入排序。
## 直接插入排序
```cpp
void InsertSort(int *sq,int length)
{
	for(int i=2,j;i<=length;i++)
    {
    	sq[0]=sq[i];
        for(j=i-1;sq[0]<sq[j];j--)
        {
        	sq[j+1]=sq[j];
        }
    sq[j+1]=sq[0];
}
```
一个一个数插入，最后得到了序列。当序列基本有序时，需要往前插入的次数会减少很多，效率还是较高的。
复杂度$O(n^2)$。
## 二分插入排序
```cpp
void BinaryInsertSort(int *sq,int length)
{
	for(int i=2,lwo,mid,high,j;i<=length;i++)
    {
		if(sq[i]<sq[i-1])
        {
			sq[0]=sq[i];
			low=1;
			high=i-1;
			while(low<=high)
            {
				mid=(low+high)/2;
				if(sq[0]<sq[mid])
					high=mid-1;
				else if(sq[0]>sq[mid])
					low=mid+1;
				else
                {
					low=high=mid;
					break;
				}
			}
			printf("low=%d,high=%d\n",low,high);
			for(j=i-1;j>=high+1;j--)
				sq[j+1]=sq[j];
			sq[high+1]=sq[0];
		}
	}	
}
```
直接插入排序中，前面的序列已经是有序的，需要找到当前需要插入的数在前面的数的位置，然后进行插入，直接插入排序采用从后往前依次找插入位置，如果前面的序列是有序表的话，可以用二分查找方法来找到要插入的位置。二分查找，设置low，high，当没有找到待插入元素时，>=high的元素都是大于当前待插入元素的，所以找到了插入位置，即high之前。
可见二分插入排序，只是减少了关键字之间的比较次数，并不能减少交换次数，只是在查找插入位置时提高了速度，而且必须是有序表的限制。
## 二路插入排序
```cpp
void DoubleRouterInsertSort(int *sq,int length)
{
	int *sqadd=(int *)malloc((length+1)*sizeof(int));
	*(sq+0)=*(sqadd+length)=*(sq+1);
	int insertpos;
	int pivotA=0;
	int pivotB=length;
	for(int i=2;i<=length;i++)
    {
		if((*(sq+i))>*((sq+0)))
        {
			insertpos=pivotB;
			while((insertpos<=length)&&((*(sq+i))>(*(sqadd+insertpos))))
            {
				*(sqadd+insertpos-1)=*(sqadd+insertpos);
				insertpos++;
			}
			*(sqadd+insertpos-1)=*(sq+i);
			pivotB--;
		}
		else
        {
			if(pivotA==0)
            {
				*(sq+1)=*(sq+i);
				pivotA=1;
			}
			else
            {
				insertpos=pivotA;
				while((insertpos>=1)&&((*(sq+i))<(*(sq+insertpos))))
                {
					*(sq+insertpos+1)=*(sq+insertpos);
					insertpos--;
				}
				*(sq+insertpos+1)=*(sq+i);
				pivotA++;
			}
		}
	}
	for(;pivotA<length;pivotA++)
    {
		*(sq+pivotA+1)=*(sqadd+pivotB);
		pivotB++;
	}
	free(sqadd);
}
```
二路插入排序是在二分插入排序的基础上改进的方法，目的是为了减少排序过程中交换记录的次数，但为此需要额外的n个辅助记录空间。用一个记录（目前取第一个记录）作为枢纽点，然后把比该记录关键字大大记录插入当前空间，比该记录大的插入到辅助空间，最后合并两个数组，完成排序。
二路插入排序中，移动记录的次数约为$\frac{n^2}{8}$。因此，二路插入排序只能减少移动记录的次数，而不能绝对避免移动记录。并且当，*(sq+1)为关键字最小/大的记录时，二路插入排序就完全失去其优越性。
## 表插入排序
```cpp
typedef struct Node{
	int key;
	int next;
}Node;
typedef struct SList{
	Node sq[MAX_LEN+1];
	int length;
}SList;
void printSL(SList test){
	int pos=test.sq[0].next;
	while(pos!=0){
		printf("%d ",test.sq[pos].key);
		pos=test.sq[pos].next;
	}
	putchar('\n');
}
void printSN(SList test){
	for(int i=1;i<=test.length;i++){
		printf("%d ",test.sq[i].key);
	}
	putchar('\n');
}
void TableInsertSort(SList &sl){
	sl.sq[0].next=1;
	sl.sq[1].next=0;
 
	int insertpos;
	int preinsertpos;	
 
	for(int i=2;i<=sl.length;i++){
		insertpos=sl.sq[0].next;
		preinsertpos=0;
 
		while(sl.sq[i].key>sl.sq[insertpos].key){
			preinsertpos=insertpos;
			insertpos=sl.sq[insertpos].next;
		}
 
		sl.sq[preinsertpos].next=i;
		sl.sq[i].next=insertpos;
	}
}
void Arrange(SList &test){
	int q,p=test.sq[0].next;
	Node tmp;
	for(int i=1;i<test.length;i++){
		while(p<i){
			p=test.sq[p].next;
		}
		q=test.sq[p].next;
		if(p!=i){
			//sq[p]<--->sq[i]
			tmp=test.sq[p];
			test.sq[p]=test.sq[i];
			test.sq[i]=tmp;
 
			test.sq[i].next=p;
		}
		p=q;
	}
}

```
前面3中插入排序都无法完全避免记录之间的交换，只有改变数据结构才能解决这个问题，表插入排序通过修改next域来避免直接的记录交换。
可见，表插入排序基本操作依然是将一个记录插入到已排序的有序表中，和直接插入排序相比，不同之处是以修改$2n$次“指针”域代替了移动记录，排序过程中关键字之间的比较次数相同，因此表插入排序的时间复杂度依然是$O(n^2)$ 。
## 希尔插入排序
```cpp
static void ShellInsert(int *sq,int length,int dk)
{
	int tmp;
	for(int i=dk+1;i<=length;i++)
    {
		if((*(sq+i))<(*(sq+i-dk)))
        {
			tmp=*(sq+i);
			int j;
			for(j=i-dk;(j>0)&&((*(sq+j))>tmp);j-=dk)
            {
				*(sq+j+dk)=*(sq+j);
			}
			*(sq+j+dk)=tmp;
		}
	}
}
static int* CreateIncrement(int times)
{
	int *increment=(int *)malloc(times*sizeof(int));
	if(NULL!=increment)
    {
		for(int i=0;i<times-1;i++)
        {
			*(increment+i)=(int)(pow((double)(2),(double)(times-i-1))+1);
		}
		*(increment+times-1)=1;
	}
	return increment;
}
const int SHELL_TIMES=5;
void ShellSort(int *sq,int length)
{

	int *dk=CreateIncrement(SHELL_TIMES);
	if(NULL!=dk)
    {
		for(int i=0;i<SHELL_TIMES;i++)
        {
			ShellInsert(sq,length,*(dk+i));
		}
		free(dk);
		dk=NULL;
	}
}
```
希尔排序又称“缩小增量排序”（diminishing increment sort），它也是一种插入排序类的方法，但在效率上有较大提高。
对直接插入排序的分析可知，其算法时间复杂度为$O(n^2)$ ，但是，若待排序记录序列为 “正序” 时，时间复杂度可提高到$O(n)$  。由此可设想，若待排序序列按关键字基本有序，n也很小时，直接插入排序的效率还是很高的，希尔排序正式从这两点出发，对直接插入排序的一种改进排序算法。
可见，希尔排序的特点是：子序列的构成不是简单地“逐段分割”，而是将相隔某个“增量”的记录组成一个子序列。因此，希尔排序中关键字小的记录不是一步一步向前挪动，而是跳跃式地向前移，从而使得最后一趟增量为1的插入排序时，序列已基本有序，这就是希尔排序效率提高的关键。
# 快速排序
快速排序是一种借助“交换”的方式，对冒泡排序的一种改进的排序算法。
## 冒泡排序
```cpp
void BubbleSort(int *sq,int length){
	int tmp;
	for(int i=length;i>1;i--){
		for(int j=1;j<i;j++){
			if(sq[j]>sq[j+1]){
				tmp=sq[j];
				sq[j]=sq[j+1];
				sq[j+1]=tmp;
			}
		}
	}
}
```
冒泡排序时间复杂度是 $O(n^2)$，效率比较低，在最坏情况下需要进行  $\frac{n(n-1)}{2}$ 次比较，并作同等数量级的记录移动。
## 快速排序
```cpp
static int Partion(int *sq,int low,int high)
{
	int pivotkey=sq[low];
	while(low<high)
    {
		while((low<high)&&(sq[high]>pivotkey))high--;
		sq[low]=sq[high];
		while((low<high)&&(sq[low]<pivotkey))low++;
		sq[high]=sq[low];
	}
	sq[low]=pivotkey;
	return low;
}
```
快速排序对冒泡排序有较大改进，它的基本思想是：通过一趟排序将待排序记录分割成两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。
假设待排序的序列为 r[low]，r[low+1]，......，r[high]，任选一个记录，通常是第一个记录 r[low] 为枢轴（pivot），然后将所有关键字比枢轴大的记录都安置在枢轴之后，所有比枢轴小的记录都安置在枢轴前面，以枢轴最后落点位置pos为分界点，将序列分割成两个序列：r[low]，r[low+1]，.....，r[pos-1]和r[pos+1]，r[pos+2]，.....，r[high]，这个过程称为一趟快速排序或一次划分。
```cpp
static void Qsort(int *sq,int low,int high)
{
	if(low<high)
    {
		int pivot=Partion(sq,low,high);
		Qsort(sq,low,pivot-1);
		Qsort(sq,pivot+1,high);
	}
}
void QuickSort(int *sq,int length)
{
	Qsort(sq,1,length);
}
```
记录一趟排序后剩下两个记录序列，又可以分别对这两个序列进行划分，同样的方法，递归实现。
快速排序时间复杂度为 $O(n\log{n})$，平均时间复杂度低，但是最坏情况的话就沦落到冒泡排序。
# 选择排序
选择排序（selection sort）的基本思想是：每一趟在序列 sq[1],sq[2],...,sq[length]中选取关键字最大的记录作为序列最后一个记录，然后再从剩下的记录中选取最大的记录作为序列倒数第二个记录...，直到整个序列有序。《数据结构》中如是描述：每一趟在 n-i+1 (i=1,2,...,n-1)个记录中选取最小的记录作为有序序列中第 i 个记录。常见的选择排序包括：简单选择排序，树形选择排序，堆排序。分别给予实现
## 简单选择排序
```cpp
void SelectSort(int *sq,int length)
{
	for(int i=1,j,tmp,min;i<length;i++)
    {
		min=i;
		for(j=i+1;j<=length;j++)
        {
			if(sq[j]<sq[min])
				min=j;
		}
		if(min!=i)
        {
			tmp=sq[i];
			sq[i]=sq[min];
			sq[min]=tmp;
		}
	}
}
```
对 sq[1,2,...,n]中记录进行简单选择排序算法为：令 i 从1至n-1，进行n-1趟选择操作，每次选择最小的和sq[i]交换。
可见，简单选择排序过程中需要进行移动记录的次数较少，最小值为0，最大时为$3(n-1)$。然而，无论记录的初始排列如何，所需进行的关键字之间的比较操作次数相同，均为 $\frac{n(n-1)}{2}$ ，因此总时间复杂度为$O(n^2)$。
## 树形选择排序
从上述可见，选择排序主要进行关键字之间的比较，因此改进简单选择排序应从如何减少比较次数出发考虑。显然从 n 个关键字中选出最小值，至少需要 n-1 次比较，然而，继续在剩下的 n-1 个关键字中选择次小关键字就并非一定要进行 n-2 次比较，若能利用前 n 次比较所得信息，则可减少以后每趟的比较次数。锦标赛便是一种选择排序。例如，8个运动员决出前三名至多需要 11 场比赛，而不是 7+6+5=18 场比赛(它的前提就是，若乙胜丙，甲胜乙，则甲定胜丙)，亚军只能产生于分别在决赛和半决赛和第一轮比赛中输给冠军的选手，这就是树形选择排序。

树形选择排序，又称为锦标赛排序，是一种按照锦标赛思想进行选择排序的方法。首先对 n 个记录的关键字进行两两比较，然后在剩下的 $\frac{n+1}{2}$ 个记录中再进行两两比较，如此重复，直至选出最小关键字为止(选冠军)。输出最小关键字，然后把最小关键字置为_MAX_(MAX_INT)，然后又重新从这 n 个记录中选择最小的，此时的最小即次小，如此重复，直到输出所有关键字。
```cpp
static void AdjustTree(int *Tree,int length,int high)
{
	if(length==1)return;
	else
    {
		int nextlength=(length+1)/2;
		int adjustpos=high-length+1-nextlength;
		for(int i=high-length+1;i<=high;i+=2)
        {
			if((i<high)&&(Tree[i]>Tree[i+1]))
				Tree[adjustpos]=Tree[i+1];
			else
				Tree[adjustpos]=Tree[i];
			adjustpos++;
		}
		AdjustTree(Tree,nextlength,high-length);
	}
}
static int GetTreeSize(int n)
{
	if(n==1)
    	return 1;
	else
		return (n+GetTreeSize((n+1)/2));
}
void TreeSelectSort(int *sq,int length)
{
	int TreeSize=0;
	int *Tree=NULL;
	TreeSize=GetTreeSize(length);
	Tree=(int *)malloc(TreeSize*sizeof(int));
	if(Tree!=NULL)
    {
		memset(Tree,0,TreeSize*sizeof(int));
		for(int i=TreeSize-length,j=1;i<TreeSize;i++,j++)
			Tree[i]=sq[j];
		for(int pos=1;pos<=length;)
        {
			AdjustTree(Tree,length,TreeSize-1);
			for(int i=TreeSize-length;i<TreeSize;i++)
            {
				if(Tree[i]==Tree[0])
                {
					Tree[i]=MAX_INT;
					sq[pos]=Tree[0];
					pos++;
				}
			}
		}
		free(Tree);
		Tree=NULL;
	}
}
```
由于含有 n 个叶子节点的完全二叉树的深度为  $\left\lceil(\log_{2}{n})\right\rceil+1$ ，则在树形选择排序中除最小关键字之外，每选择一个次小关键字仅需进行 $\left\lceil(\log_{2}{N})\right\rceil$次比较，因此它的时间复杂度为 $O(n\log{n})$。$\left\lceil\right\rceil$代表上取整。
## 堆排序
从树形选择排序中我们知道，为了构成完全二叉树，需要额外很大的辅助空间，为了弥补，威洛姆斯在1964年提出了堆排序。堆排序只需要一个记录大小的辅助空间，每个待排序的记录仅占一个存储空间。
堆顶一如下：n个元素的序列{K1,K2,...,Kn} 当且仅当满足如下关系时，称之为堆。

Ki>=K2i && Ki>=K(2i+1)  或者 Ki<=K2i && Ki<=K(2i+1)  $i=(1,2,...,\left\lfloor\frac{n}{2}\right\rfloor)$ $\left\lfloor\right\rfloor$代表下取整
即以一维数组来存储这个序列，并看成一个完全二叉树，则堆的含义是，完全二叉树中所有非终端节点的值均不大于（或不小于）其左右孩子节点的值。
```cpp
void HeapAdjust(int *sq,int low,int high)
{
	int rc=sq[low];
	for(int j=2*low;j<=high;j++)
    {
		if((j<high)&&(sq[j+1]>sq[j]))j++;
		if(sq[j]>rc)
        {
			sq[low]=sq[j];
			low=j;
		}
		else
			break;
	}
	sq[low]=rc;
}
void HeapSort(int *sq,int length)
{
	for(int i=length/2;i>0;i--)
		HeapAdjust(sq,i,length);
	for(int i=length,tmp;i>0;i--)
    {
		tmp=sq[1];
		sq[1]=sq[i];
		sq[i]=tmp;
		HeapAdjust(sq,1,i-1);
	}
}
```
若在输出堆顶元素(如果堆顶元素是最大的那么可以将之与堆最后一个元素交换)之后，使得剩余的 n-1 个元素又重新建堆，则得到 n 个元素中次小值。如此反复执行便能得到一个有序序列，这个过程称为堆排序。
堆排序在最坏情况下，其时间复杂度也为 $O(n\log{n})$。相对于快速排序来说，这是堆排序的最大优点，此外堆排序仅需一个记录大小的供交换的辅助空间。
# 归并排序
归并排序（Merging Sort）是又一类不同的排序算法。归并的含义是将两个或两个以上的有序序列组合成一个新的有序序列。合并两个有序序列，采用齐头并进的方法，无论是顺序存储结构还是链式存储结构，都可以在 $O(m+n)$ 的时间数量级上实现。
2-路归并的思想就是，假设初始序列含有 n 个记录，则可看成是 n 个有序的子序列(长度均为1)，然后两两归并，得到$\left\lceil\frac{n}{2}\right\rceil$ 个长度为 2 或者1 的有序子序列，再两两归并，并如此重复下去，直到得到一个长度为 n 的有序序列为止，这种排序方法称为 2-路归并排序。
归并排序在平均和最坏情况下时间复杂度都是$O(n\log{n})$，与快速排序和堆排序相比，归并排序的最大特点是，它是一种稳定的排序算法。
## 合并两个有序序列
```cpp
static void Merge(int *SR,int *TR,int i,int m,int n)
{
	assert(SR!=NULL&&TR!=NULL);
	assert(i<=m&&m<=n);
	int pleft,pright,pos;
	for(pos=pleft=i,pright=m+1;pleft<=m&&pright<=n;pos++)
    {
		if(SR[pleft]<=SR[pright])
        {
			TR[pos]=SR[pleft];
			pleft++;
		}
		else
        {
			TR[pos]=SR[pright];
			pright++;
		}
	}
	while(pleft<=m)
    {
		TR[pos]=SR[pleft];
		pos++;
		pleft++;
	}
	while(pright<=n)
    {
		TR[pos]=SR[pright];
		pos++;
		pright++;
	}
	for(int j=i;j<=n;j++)
		SR[j]=TR[j];
}
```
归并排序的核心思想就是：将一维数组中前后两个有序序列合并成一个有序序列
## 递归形式的二路归并排序
```cpp
static void MSort(int *SR,int *TR,int left,int right)
{
	if(left==right)
		TR[left]=SR[left];
	else
    {
		int mid=(left+right)/2;
		MSort(SR,TR,left,mid);
		MSort(SR,TR,mid+1,right);
		Merge(SR,TR,left,mid,right);
	}
}
void MergeSort(int *sq,int length)
{
	int *result=(int *)malloc((length+1)*sizeof(int));
	memset(result,0,(length+1)*sizeof(int));
	MSort(sq,result,1,length);
	free(result);
}
```
二路递归的归并排序思想：合并两个 $\left\lceil\frac{n}{2}\right\rceil$ 两个序列
## 非递归形式的二路归并排序
在归并排序中，递归形式的归并算法在形式上比较简洁，但实用性较差，那么就实现其非递归形式。
```cpp
void MergeSort_SIM(int *sq,int length)
{
	int *result=(int *)malloc((length+1)*sizeof(int));
	memset(result,0,(length+1)*sizeof(int));
	int i;
	for(int sublength=1;sublength<=length;sublength*=2)
    {
		for(int pos=1;pos<=length;pos+=2*sublength)
        {
			i=pos+sublength;
			if(i>length)break;
			else if(i+sublength-1<=length)
				Merge(sq,result,pos,i-1,i+sublength-1);
			else
				Merge(sq,result,pos,i-1,length);
		}
	}
	free(result);
}
```

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。