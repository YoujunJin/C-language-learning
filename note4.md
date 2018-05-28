第四章 语句
C实现了其他现代高级语言所具有的所有语句。
4.1 空语句
  只含一个分号
4.2 表达式语句
  C语言没有赋值语句，实现赋值功能的其实是表达式语句，但是表达式语句并不一定要给某个变量赋值，如y+3;完全合法。还有如a++,printf语句等。只是表达式的值被忽略了。
4.3 代码块
  一对花括号之内的语句
4.4 if语句
  if后面必须加括号，括号是if语句的一部分而不是表达式的一部分。
  C并不具备布尔类型，而是用整形来代替，0值表示假，非零值表示真（C99开始有布尔类型_Bool）。表达式的值或者是1或者是0。else从属于最靠近它的不完整if语句。
4.5 while语句
  先判断表达式真假，
  break和continue，只对最内层的循环起作用。
4.6 for语句
  初始化部分，条件部分，调整部分，三个表达式都是可选的，
  同样有break和continue。
  和while的不同：continue后还会执行调整部分。
4.7 do语句
  do，while，测试在循环体执行后才进行，像其他语言的repeat语句。
4.8 switch语句
  类似于其他语言的case语句。
  括号中表达式的结果必须是整形值。
  case是执行流的进入点，而不划分语句列表，若要只执行一个case，需要用break跳到语句列表末尾。
  continue没有效果，除非在循环中但是也是用来控制循环语句。
  几个标签具有同样的操作时，可以把它们连着，然后只在最后一个case里写操作。
  default是不匹配所有case时进入的点，没有break也会执行。
4.9 goto语句
  在希望跳转的语句前面加语句标签即标识符后面加冒号。少用或不用，用循环或函数解决。
编程习题
1：
#include <stdio.h>
#include <stdlib.h>
int main(void)
{
  double n,aii,ai=1;
  scanf("%g",&n);
  while(1)
  {
    aii = (ai+n/ai)/2;
    printf("%g ", aii);
    if(aii == ai)
    {
      break;
    }
  }
  printf("%g\n",aii);
  return EXIT_SUCCESS;
}
4:
void copy_n(char dst[], char src[], int n)
{
  int i;
  char temp;
  for (i=0; i<n; i++)
  {
    if(src[i])
    {
      temp = src[i];
    }
    else
    {
      temp = 0;
    }
    dst[i] = temp;
  }
}
