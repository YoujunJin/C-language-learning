《C和指针》
第一章，快速上手
通过一个从标准输入读取文本并对其进行修改，然后把它写到标准输出的程序，展示了编写C语言时所需要知道的绝大多数基本技巧。
CODES:
/*
** This program reads input lines from standard input and prints
** each input line, followed by just some portions of the line, to
** the standard output.
**
** The first input is a lint of colum numbers, which ends with a negative
** number. The column numbers are paired and specify ranges of columns from
** the input line that are to be printed.
** For example, 0 3 10 12 -1 indicates that only columns 0 through 3
** and columns 10 through 12 will be printed.
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_COLS        20  /* max # of columns to process*/
#define MAX_INPUT       1000  /* max of input & output lines */
int read_column_numbers(int columns[], int max );
void rearrange(char *output,char const *input, int n_columns, int const columns[]);

int main(void)
{
  int n_columns;          /* # of columns to process */
  int columns[MAX_COLS];  /* the columns to process */
  char input[MAX_INPUT];  /* array for input line */
  char output[MAX_INPUT]; /* array for output line */
  /*
  **  Read the list of column numbers
  */
  n_columns = read_column_numbers(columns, MAX_COLS);

  /*
  ** Read, process and print the remaining lines of input
  */
  while(gets(input) != NULL){
    printf("Original input : %s\n", input);
    rearrange(output,input,n_columns,columns);
    printf("rearranged line: %s\n", output);
  }

  return EXIT_SUCCESS;
}


/*
** Read the list of column numbers, ignoring any beyond the specified maximum
*/
int read_column_numbers(int columns[], int max)
{
  int num = 0;
  int ch;

  /*
  ** Get the numbers, stopping at eof or when a number is < 0.
  */
  while(num < max && scanf("%d", &columns[num] ) == 1
        && columns[num] >=0)
        num +=1;

  /*
  ** Make sure we have an even number of inputs, as they are supposed to be paired.
  */
  if (num % 2 !=0) {
    puts("Last column number is not paired.");
    exit(EXIT_FAILURE);
  }

  /*
  ** Discard the rest of the line that contained the final number.
  */
  while ((ch = getchar())!= EOF && ch!= '\n') {
    ;
  }

  return num;
}

/*
** Process a line of input by concatenating the characters from the indicated
** columns. The output line is the NUL terminated.
*/
void rearrange(char *output, char const *input, int n_columns, int const columns[])
{
  int col;    /* subscript for columns array*/
  int output_col;   /* output column counter*/
  int len;    /* length of input line*/

  len = strlen(input);
  output_col = 0;

  /*
  ** Process each pair of column number.
  */
  for ( col = 0; col < n_columns; col += 2){
    int nchars = columns[col+1] - columns[col] + 1;
    /*
    ** If the input line isn't this long or the output array is full, we're done
    */
    if(columns[col] >= len || output_col == MAX_INPUT-1)
      break;

    /*
    ** If there isn't room in the output array, only copy what will fit
    */
    if(output_col + nchars > MAX_INPUT-1)
      nchars = MAX_INPUT - output_col -1;

    /*
    ** Copy the relevant data.
    */
    strncpy(output + output_col, input+columns[col],nchars);
    output_col += nchars;
  }
  output[output_col] = '\0';
}

1、空白和注释，C是一个自由格式的语言，与python是鲜明的对比，所以用C语言写一大段没有空白的
代码也可以编译，不过这样十分不利于代码的阅读和修改，但同时就产生了一些有意思的代码阅读大赛。
注释的功能是用来告诉读者程序能做些什么以及怎么做。注释可以以/*开始并以/*结束。也可以用//注释
一行。/**/不能嵌套，如果嵌套只把第一个/*和第一个*/之间内容看作注释，所以如果要安全的注释掉
一段代码，更好地办法是使用#if指令，如下：
#if 0
    statements
#endif
2、预处理指令
#include和#define等#开头的命令称为预处理指令，因为它们是由预处理器解释的。预处理器读入
源代码，根据预处理指令对其进行修改，然后把修改过的源代码递交给编译器。如stdio.h读入预处理
器后，预处理器把名叫stdio.h的库函数头文件的内容替换到源文件的那个位置。
函数原型，用于声明一个函数的名称，返回值类型，参数个数和类型，参数名不必需。函数定义将在源
文件中定义，声明后编译器就可以对调用这些函数时进行准确性检查，即使还没有函数体定义。
指针，指针指定一个存储于计算机内存中的值的地址，是C语言最强大功能之所在。
const，定义的量是值不会发生改变的常量。
3、main函数
每个C程序都必须有一个main函数，因为它是程序执行的起点。
定义变量是局部变量，变量可以传参给其他函数，数组参数是以引用形式进行传递也就是传址调用，而
标量和常量则是按值传递的（实际上都是按值传递的，但是传的是地址就是引用形式了）。
gets函数从标准输入读取一行文本去掉换行符加上NUL字节即\0，正常读入就返回非NULL
值，没有输入返回NULL值。
字符串，C语言约定字符串是一串以NUL字节结尾的字符，NUL作为终止符本身不被看作字符串的一部分。
字符串常量，注意NUL占据1个字节空间。
printf函数执行格式化输出，
4、read_column_numbers函数
参数个数和类型及返回值类型需要与声明的函数原型完全匹配。
该函数的局部变量在定义时未初始化则其值是一个不可预料的值，也就是垃圾。
scanf函数从标准输入读取字符，存储所读取的数据的标量参数前面必须加上&符号取址。
数组下标的有效性检查需要自己编写代码，否则scanf可以写到数组后的内存中，破坏原数据。
ch是一个整形，因为EOF是一个整形值，防止从输入字符意外解释为EOF。
5、rearrange函数
数组名作为实参时，传给函数的实际上是一个指向数组起始位置的指针，也就是数组在内存的地址。
而声明为const保证了编译器会验证是否允许这个参数被修改。
数组大小固定后，遇到特比长的输入，用gets函数可能会溢出，所以应该换用fgets

第一章的警告和提示总结：
警告：1、在scanf函数的标量参数前未添加&字符。
    2、机械地把printf函数的格式代码照搬于scanf函数。
    3、在应该使用&&操作符的地方误用了&操作符。
    4、误用=操作符而不是==操作符来测试相等性。
提示：1、使用#include指令避免重复声明。
    2、使用#define指令给常量值取名。
    3、在#include文件中放置函数原型。
    4、在使用下标前先检查它们的有效性。
    5、在while和if表达式中蕴含赋值操作可以减少代码冗余。
    6、如何编写一个空循环体。
    7、始终要进行检查数组是否越界。

习题：6、
      （1）、不检查下标是否越界可以有效提高程序运行的效率，因为如果你检查，那么编译器必须
      在生成的目标代码中加入额外的代码用于程序运行时检测下标是否越界，这就会导致程序的运行
      速度下降，所以为了程序的运行效率，C/C++才不检查下标是否越界。
      （2）、不检查下标是为了给程序员更大的空间，也为指针操作带来更多的方便。如果有这个检
      查的话指针的功能将会大大被削弱，C的数组标识符，里面并没有包含该数组长度的信息，只包
      含地址信息，所以语言本身无法检查，只能通过编译器检查，而早期的C语言编译器也不对数组
      越界进行检查，只能由程序员自己检查确保。以及在早期的CRT函数中也不对字符串指针或数组
      进行越界检查，都是要求程序员确保空间足够，因此也才也才有了在VS2005之后微软提供的安
      全的CRT函数版本。
      8、
        虽然input定义的是const指针，编译时会产生warning，但是可以通过编译，运行时input
        所指数组的值会发生改变，而且gets函数不能防止输入溢出，应该换作fgets函数。
