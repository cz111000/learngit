# 算法 #
***
What's Algorithm ?
------------------
提及“**算法**”，小白面对高大上说词仰慕之心油然而生 @o@

然而，算法并不神秘 = =

顾名思义，**计算方法**简称算法（什，什，什，什么！这么简单？→_→）


举个栗子：
> 从小学就接触的乘法：对 n+n+n+...+n 的简写就是一种算法（递推法[^A]）,
算法注定要有输入与输出：算式中的 n 即为输入，乘积即为输出。

注：

* 算法中的状态变化是不确定的
* 算法必定能在执行有限个步骤之后终止
* 某些算法加入随机输入，称为随机化算法

How to Assess Algorithm ?
-------------------------
对算法的评定分析主要从**时间复杂度**(所耗时间长短)与**空间复杂度**(空间开支大小)来考虑。

关于复杂度的计算需要引入如下渐近记号：
> *大O记号：*
>> *O(g(n)) = { f(n) : 存在正常数c和n0 ，使对所有的n >= n0，都有 0 <= f(n) <= cg(n) }*

> *大Ω记号：*
>> *Ω(g(n)) = { f(n) : 存在正常数c和n0 ，使对所有的n >= n0，都有 0 <= cg(n) <= f(n) }*

> *大Θ记号：*
>> *Θ(g(n)) = { f(n) : 存在正常数c1和c2和n0 ，使对所有的n >= n0，都有 0 <= c1g(n) <= f(n) <= c2g(n) }*

*数学证明资料：[计算机算法分析之渐进记号*](http://www.cnblogs.com/zabery/archive/2011/07/19/2110994.html)

具体计算方式将于实例中给出 ～(~▽~)～

DIY Algorithm ！
---------------
以下给出排序算法C++代码：

1.冒泡排序（Bubble Sort）[^B]
```C++
template<typename T>
void bubbleSort(T arr[],int n)
{
	T temp;
	for(int i = 0;i < n - 1;i++)
		for(int j = 0;j < n - 1 - i;j++)
			if(arr[j] > arr[j + 1])
			{
				temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
			}
}
```
时间复杂度分析：

考虑最坏情况--将逆序数列变为顺序（此情况下，每一次比较都需要进行交换运算） T_T
> 再举个栗子：**数列 5 4 3 2 1 进行冒泡升序排列**
第一次外层循环从第一个数5开始到倒数第二个数2结束，
共进行4次比较交换运算，5移到末尾。
第二次外层循环从第一个数4开始到倒数第三个数2结束。
共进行3次比较交换运算，4移到倒数第二个数。
……
依次推类，总比较次数为 4 + 3 + 2 + 1 = 10 次

证明：

根据数学归纳法,对于n位的数列则有比较次数为`(n-1) + (n-2) + ... + 1 = n \* (n - 1) / 2 = (n^2 - n) / 2`

如图![比较次数](http://images.51cto.com/files/uploadimg/20110826/211021569.jpg)

若n = 10000，则`(n^2 - n) / 2 = (100000000 - 10000) / 2`

相对 100000000 来说 10000 微乎其微，故总计算次数约为`0.5 \* N^2`

用O(N^2)就表示了其数量级（忽略前面系数0.5）

得 **冒泡排序的时间复杂度为O(N^2)**
```C++
//冒泡排序改进版：若遍历中得知数组有序则结束排序
template<typename T>
void bubbleSort(T arr[],int n)
{
	bool flag = false;
	T temp;
	for(int i = 0;i < n - 1;i++)
	{
		flag = false;
		for(int j = 0;j < n - 1 - i;j++)
			if(arr[j] > arr[j + 1])
			{
				flag = true;
				temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
			}
		if(flag == false)
			break;
	}
}
```
改进算法后，对于一个有序数组完成一次外层循环后就会结束，共发生 N - 1 次比较，故升级版冒泡排序在最优情况下的时间复杂度为O(N)

2.插入排序(Insert Sort)
```C++
template<typename T>
void insertSort(T arr[],int n)
{
	T temp;
	for(int i = 1;i < n;i++)
		if(arr[i] < arr[i - 1])
		{
			temp = arr[i];
			for(int j = i - 1;j >= 0 && arr[j] > temp;j--)
				arr[j + 1] = arr[j];
			arr[j + 1] = temp;
		}
}
```
*资料：[插入排序舞蹈](http://baidu.ku6.com/watch/05586300336858352205.html)*

3.\*归并排序（Merge Sort）
```C++
template<typename T>
void mergeSort(T arr[],T temp[],int start,int last)
{
	if(start < last)
	{
		int mid = (start + last) / 2;
		mergeSort(arr,temp,start,mid);
		mergeSort(arr,temp,mid + 1,last);
		arrUnion(arr,temp,start,mid,last);
	}	
}

template<typename T>
void arrUnion(T arr[],T temp[],int start,int mid,int last)
{
	int i,j,k;
	i = start;
	j = mid + 1;
	k = start;
	while(i != mid + 1 && j != last + 1)
	{
		if(arr[i] < arr[j])
			temp[k++] = arr[i++];
		else
			temp[k++] = arr[j++];
	}
	while(i != mid + 1)
		temp[k++] = arr[i++];
	while(j != last + 1)
		temp[k++] = arr[j++];
	for(i = start;i < last + 1;i++)
		arr[i] = temp[i];
}
```
*资料：[排序动画演示](http://visualgo.net/sorting)*

Comparison .
------------
下面我们通过这段C语言代码比较排序速度，直观感受时间复杂度。
```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 5000

template<typename T>
void printArr(T arr[],int n);
template<typename T>
void arrUnion(T arr[],T temp[],int start,int mid,int last);
template<typename T>
void mergeSort(T arr[],T temp[],int start,int last);
template<typename T>
void insertSort(T arr[],int n);
template<typename T>
void bubbleSort(T arr[],int n);
template<typename T>
void bubbleSortPlus(T arr[],int n);

int main(int argc, char * argv[])
{
	clock_t start,end;
    int a[N];
	int b[N];
	int c[N];
	int d[N];
	srand(time(NULL));
	for(int i = 0;i < N;i++)
	{
		a[i] = rand()%100 + rand()%1000;
		d[i] = c[i] = b[i] = a[i];
	}

    int* temp = (int*)malloc(sizeof(int) * N);
	start = clock();
	mergeSort(a,temp,0,N - 1);
	end = clock();
	printf("归并排序用时%lfsec\n",(double)(end - start) / CLOCKS_PER_SEC);
	//printArr(a,N);
	free(temp);

	start = clock();
	insertSort(b,N);
	end = clock();
	printf("插入排序用时%lfsec\n",(double)(end - start) / CLOCKS_PER_SEC);
	//printArr(b,N);

	start = clock();
	bubbleSort(c,N);
	end = clock();
	printf("冒泡排序用时%lfsec\n",(double)(end - start) / CLOCKS_PER_SEC);
	//printArr(c,N);

	start = clock();
	bubbleSortPlus(d,N);
	end = clock();
	printf("升级版冒泡排序用时%lfsec\n",(double)(end - start) / CLOCKS_PER_SEC);
	//printArr(c,N);

    return 0;
}

template<typename T>
void arrUnion(T arr[],T temp[],int start,int mid,int last)
{
	int i,j,k;
	i = start;
	j = mid + 1;
	k = start;
	while(i != mid + 1 && j != last + 1)
	{
		if(arr[i] < arr[j])
			temp[k++] = arr[i++];
		else
			temp[k++] = arr[j++];
	}
	while(i != mid + 1)
		temp[k++] = arr[i++];
	while(j != last + 1)
		temp[k++] = arr[j++];
	for(i = start;i < last + 1;i++)
		arr[i] = temp[i];
}

template<typename T>
void mergeSort(T arr[],T temp[],int start,int last)
{
	if(start < last)
	{
		int mid = (start + last) / 2;
		mergeSort(arr,temp,start,mid);
		mergeSort(arr,temp,mid + 1,last);
		arrUnion(arr,temp,start,mid,last);
	}	
}

template<typename T>
void insertSort(T arr[],int n)
{
	T temp;
	for(int i = 1;i < n;i++)
		if(arr[i] < arr[i - 1])
		{
			temp = arr[i];
			for(int j = i - 1;j >= 0 && arr[j] > temp;j--)
				arr[j + 1] = arr[j];
			arr[j + 1] = temp;
		}
}

template<typename T>
void bubbleSort(T arr[],int n)
{
	T temp;
	for(int i = 0;i < n - 1;i++)
		for(int j = 0;j < n - 1 - i;j++)
			if(arr[j] > arr[j + 1])
			{
				temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
			}
}

template<typename T>
void bubbleSortPlus(T arr[],int n)
{
	bool flag = false;
	T temp;
	for(int i = 0;i < n - 1;i++)
	{
		flag = false;
		for(int j = 0;j < n - 1 - i;j++)
			if(arr[j] > arr[j + 1])
			{
				flag = true;
				temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
			}
		if(flag == false)
			break;
	}
}

template<typename T>
void printArr(T arr[],int n)
{
	for(int i = 0;i < n;i++)
		printf("%d ", arr[i]);
	printf("\n");
}
```
执行条件：

**编译程序VC 6.0**

**数组成员 5000**

执行结果：

**归并排序用时0.002000sec**

**插入排序用时0.023000sec**

**冒泡排序用时0.077000sec**

**升级版冒泡排序用时0.071000sec**

[^A]: 递推法即是把一个复杂的庞大的计算过程转化为简单过程的多次重复

[^B]: 冒泡排序的思想是让最大的数浮动到数组最后的位置，其次大的数浮动到数组倒数第二个位置，依次推类
