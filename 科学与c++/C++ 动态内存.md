C++ 程序中的内存分为两个部分：

- 栈：在函数内部声明的所有变量都将占用栈内存。
- 堆：这是程序中未使用的内存，在程序运行时可用于动态分配内存。

## new 和 delete 运算符
double* pvalue  = NULL; // 初始化为 null 的指针
pvalue  = new double;   // 为变量请求内存

如果自由存储区已被用完，可能无法成功分配内存。所以建议检查 new 运算符是否返回 NULL 指针，并采取以下适当的操作：

double* pvalue = NULL; if( !(pvalue = new double )) { cout << "Error: out of memory." <<endl; exit(1); }

delete pvalue;        // 释放 pvalue 所指向的内存

## 一维数组

// 动态分配,数组长度为 m int *array=new int [m]; //释放内存 delete [] array;

## 二维数组

int **array; // 假定数组第一维长度为 m， 第二维长度为 n // 动态分配空间 array = new int *[m]; for( int i=0; i<m; i++ ) { array[i] = new int [n]; } //释放 for( int i=0; i<m; i++ ) { delete [] array[i]; } delete [] array;