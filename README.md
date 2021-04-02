# 2021-04-02
对C语言课本中数组的学习

1、数组越界访问后内存中数据的变化情况
#include<stdio.h>

int main(int argc,char const*argv[])
{
	int a = 1;
	int b = 2;
	int i = 0;
	int c[5] = {0};
	
	printf("c  --> %p\n", c);
	printf("&a --> %p\n",&a);
	printf("&b --> %p\n",&b);
	printf("&i --> %p\n",&i);

	printf("\n");
	
	for (i=0 ; i<5 ; i++)
	{
		printf("c[%d] --> %p\n",i,&c[i]);
	}

	printf("\n");
		
	for (i=0 ; i<=8 ; i++)
	{
		c[i] = i;
		printf("c[%d]=%d --> %p\n",i,c[i],&c[i]);
	}
	printf("Now c[5]=%d\n",c[5]);

	printf("\na = %d,b = %d,i = %d\n",a,b,i);
	printf("\n");

	printf("&a --> %p\n",&a);
	printf("&b --> %p\n",&b);
	printf("&i --> %p\n",&i);
	
	return 0;
}
输出结果如下：
c  --> 000000000062FE00
&a --> 000000000062FE1C
&b --> 000000000062FE18
&i --> 000000000062FE14

c[0] --> 000000000062FE00
c[1] --> 000000000062FE04
c[2] --> 000000000062FE08
c[3] --> 000000000062FE0C
c[4] --> 000000000062FE10

c[0]=0 --> 000000000062FE00
c[1]=1 --> 000000000062FE04
c[2]=2 --> 000000000062FE08
c[3]=3 --> 000000000062FE0C
c[4]=4 --> 000000000062FE10
c[5]=5 --> 000000000062FE14
c[6]=6 --> 000000000062FE18
c[7]=7 --> 000000000062FE1C
c[8]=8 --> 000000000062FE20
Now c[5]=9

a = 7,b = 6,i = 9

&a --> 000000000062FE1C
&b --> 000000000062FE18
&i --> 000000000062FE14

说明：
·数组内存分配形式：从上往下为从高地址到低地址，所以一开始内存中从下往上依次为
c[0]-->c[4]-->a-->b-->i
当数组越界访问时，原来分配给a,b,i的地址被强行挤占，此时对数组赋值也将a,b的值改变了
i的值每一次都会改变，所以循环完毕不管是i的值还是c[5]的值都变成了9.

总结：数组越界可能会导致其他变量的值被强行改变从而使程序出错！千万要细心检查。

2、二维数组的定义和初始化
从键盘输入某年某月（包括闰年），编程输出该年的该月所拥有的天数。
#include<stdio.h>
#define MONTHS 12

int main(int argc,char const*argv[])
{
	int days[2][MONTHS] = {{31,28,31,30,31,30,31,31,30,31,30,31},
						  {31,29,31,30,31,30,31,31,30,31,30,31}};
	int year,month;
	
	do{
		printf("Input year,month:");
		scanf("%d,%d",&year,&month);
	} while (month < 1 || month >12) ;
	
	if ((year % 4 == 0) && (year % 100 != 0) || (year % 400 == 0))
		printf("The number of days is %d\n",days[1][month-1]);
	else 	//非闰年 
		printf("The number of days is %d\n",days[0][month-1]);	
	
	return 0;
}
这样的程序比switch编写的程序更加简洁。

3、向函数传递一维数组
从键盘输入某班学生某门课的成绩（已知每班人数最多不超过40人，具体人数由键盘输入），试编程计算其平均分。
#include<stdio.h>
#define N 40

int Average(int score[], int n);
void ReadScore(int score[], int n);

int main(int argc,char const*argv[])
{
	int score[N],aver,n;
	printf("Input n:");
	scanf("%d",&n);
	ReadScore(score,n);
	aver = Average(score,n);
	printf("Average score is %d\n",aver); 
	return 0;
}
//函数功能：计算n个学生成绩的平均分
int Average(int score[], int n)
{
	int i,sum = 0;
	for (i=0 ; i<n ; i++)
	{
		sum += score[i];
	}
	return n>0 ? sum/n : -1;	
} 
//函数功能：输入n个学生的某门课成绩
void ReadScore(int score[], int n)
{
	int i;
	printf("Input score:");
	for (i=0 ; i<n ; i++)
	{
		scanf("%d",&score[i]);
	}	
}
