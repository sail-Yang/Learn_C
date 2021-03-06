# 前言

&emsp;&emsp;这是我刚转到计科的第一个数据结构作业——做一个学生信息管理系统，目的是熟悉C语言。虽然很简单，但是的的确确能够帮我们很好地复习C语言。

&emsp;&emsp;做完一项作业后，我认为还没有结束，我们还应该对做作业的过程做一个总结，比如在敲代码的时候遇到了什么困难，有什么Bug，是怎么解决的，我认为这些反思跟作业本身一样重要。具体的内容我会放到其它平台上，这里只是记录一些注意事项。

 # 题目

&emsp;&emsp;从txt文件中读取10个学生的信息，每个学生含有成员名为“学号、姓名、语文、数学、英语、政治、计算机、总分、平均分、名次”，分别编写六个函数求：

1. 输入一个学生的学号，查询该学生的信息并输出，若不存在显示没找到。
2. 输入一个新学生的信息，按学号顺序将该学生的信息插入后输出。
3. 输入一个已存在学生的姓名信息，删除该学生的信息后输出。
4. 求每个学生的总分、平均分并输出；
5. 求所有学生每门课程的平均分并输出；
6. 对所有学生的信息按总分排序，并填写名次后输出；

&emsp;&emsp;要求：

&emsp;&emsp;学生的数据用文件存储，每个学生的结构体用数组和单链表（写两个文件）。

&emsp;&emsp;当程序执行后先显示“菜单”，输入为1时，执行第（1）个函数；输入为2时，执行第（2）个函数；输入为3时，执行第（3）个函数；输入为4时，执行第（4）个函数；输入为5时，执行第（5）个函数；输入为6时，执行第（6）个函数；当输入其他数字时，退出程序运行。

**老师给的数据格式**：

```c
1,yang,97,95,94,90,96
2,zhu,95,94,95,92,97
3,wang,87,85,98,99,98
4,li,74,76,88,87,99
5,cai,96,95,93,98,95
6,sun,95,95,92,98,94
7,zhang,64,98,78,90,96
8,wu,88,98,89,98,90
9,cheng,98,96,97,90,90
10,chen,86,78,88,90,90
0,0
```



#  注意事项

- `fopen()` `fclose()`是一对，不要拆散他们，有`fopen()`就有`fclose()`，不要忘记`fclose()`。

- 像1,yang,97怎么读到结构体里(尤其是字符串)？我是这么写的:

  ```c
  fscanf(fp1,"%d,%[^,],%lf",&p->num.p->name,&p->math);
  //这里的%[^,]指的是取","前面所有的字符
  //还有scanf别忘里取地址符&……,字符串就不用了，本身就是地址
  ```

- 生成一个链表的时候，别忘了==让尾结点指向NULL==

- 在函数里最好把头结点返回出来，而不是直接放在函数里面。一开始我没有这么做，导致出现了许多莫名其妙的Bug,至于是为什么，我现在还不知道，得等我去看看。

  ```c
  //把头结点返回的意思是像下面这么写,其中Student是typedef后的结构体
  Student *func(Student *head)
  {
      ……;
      return head;
  }
  //我一开始函数的风格是下面这样的，在排序后或者删除的时候，总是会出现莫名其妙的错误
  void func(Student *head)
  {
      ……;
      return;
  }
  ```

- 下面是一些表格的制作符号

  ```c
  //提供一些制作表格的特殊符号：─│┌ ┐└ ┘├ ┤┬ ┴ ┼
  /*下面是一些示例:*/
  printf("┌────────┬────────┬────────────────┬────────┬────────┬────────┬────────┬────────┬────────┬────────┐\n");
  printf("│排名    │学号    │  名      字    │数学    │语文    │英语    │政治    │计算机  │总分    │平均分  │\n");
  printf("├────────┼────────┼────────────────┼────────┼────────┼────────┼────────┼────────┼────────┼────────┤\n");
  printf("└────────┴────────┴────────────────┴────────┴────────┴────────┴────────┴────────┴────────┴────────┘\n");
  ```

- 下面是格式化输出的一些小注意点：

  ```markdown
  1. 中文字符占多少字节数呢？在GB2312中，一个汉字是2个字节;在UTF-8中，一个汉字是3个字节。对齐是时候注意一下
  2. %8.1f是右对齐，加了负号➖是左对齐
  3. 对齐用制表符\t，别按Tab了
  ```

- 如何输入字符串以及将输入的字符串和已经在的字符串匹配，判断是否相等？

  ```c
  char de_name[NameLen_Max+1];
  printf("\t请输入要删除学生的姓名：");
  scanf("%s",&de_name);
  //现在我怎么才能将表中的学生姓名与这个输入的字符串匹配→用strcmp()（在c.type.h里）
  if(strcmp(p->name,de_name)==0)
  {
      //如果=0，那么就是相等
  }
  ```

- 冒泡排序

  ```markdown
  假设我现在有这么一组数:8 9 6 5 10
  我怎么才能将他们从小到大重新排序？这里看看冒泡排序是怎么做的
  ```

  | 排列（选中的数会加粗） |            说明            |
  | :--------------------: | :------------------------: |
  |     **8** 9 6 5 10     |           第一轮           |
  |     8 **9** 6 5 10     |         8<9，选中9         |
  |     8 6 **9** 5 10     |  9>6，交换位置，还是选中9  |
  |     8 6 5 **9** 10     |  9>5，交换位置，还是选中9  |
  |     8 6 5 9 **10**     |        9<10,选中10         |
  |     **8** 6 5 9 10     |     到头了，开始第二轮     |
  |     6 **8** 5 9 10     | 下面和第一轮同理，就不写了 |
  |     6 5 **8** 9 10     |                            |
  |     6 5 8 **9** 10     |                            |
  |     6 5 8 9 **10**     |                            |
  |     **6** 5 8 9 10     |           第三轮           |
  |     5 **6** 8 9 10     |                            |
  |     **5** 6 8 9 10     |            结束            |

- 数组版的学生信息管理系统

  ```markdown
  数组版的比较简单，主要就是把数据结构从单链表换成结构体数组
  主要问题就是结构体数组的声明和传参
  ```

  ```c
  typedef struct student{
      ……
  }Student;
  Student Std[MAX_LEN]; //没有用动态分配
  int main()
  {
      ……
  }
  /*函数示例，因为数组声明为了全局变量，所以不需要传参什么的，直接改就行*/
  voif func(void)
  {
      ……
  }
  ```

  
