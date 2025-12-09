使用C++标准库 **fstream**
要进行文件操作，必须包含头文件iostream与fstream

fsteam数据类型：

| 数据类型     | 描述                                                                       |
| -------- | ------------------------------------------------------------------------ |
| ofstream | 该数据类型表示输出文件流，用于创建文件并向文件写入信息。                                             |
| ifstream | 该数据类型表示输入文件流，用于从文件读取信息。                                                  |
| fstream  | 该数据类型通常表示文件流，且同时具有 ofstream 和 ifstream 两种功能，这意味着它可以创建文件，向文件写入信息，从文件读取信息。 |

进行 ***写操作*** 的对象：ofstream
进行 ***读操作*** 的对象：ifstream
都行的（通用）：fstream

## 打开文件
```cpp
void open(const char *filename, ios::openmode mode);
```

**open()** 成员函数的第一参数指定要打开的文件的名称和位置，第二个参数定义文件被打开的模式。

| 模式标志        | 描述                                               |
| ----------- | ------------------------------------------------ |
| ios::app    | 追加模式。所有写入都追加到文件末尾。                               |
| ios::ate    | 文件打开后定位到文件末尾。                                    |
| ios::in     | 打开文件用于读取。（仅in模式）                                 |
| ios::out    | 打开文件用于写入。（仅out模式）                                |
| ios::trunc  | 如果该文件已经存在，其内容将在打开文件之前被截断，即把文件长度设为 0。（仅out模式）（默认） |
| ios::binary | 以二进制方式进行IO                                       |

可以把以上两种或两种以上的模式结合使用，例如：想要以写入模式打开文件，并希望截断文件，以防文件已存在
```cpp
ofstream outfile;
outfile.open("file.dat", ios::out | ios::trunc );
```
类似地，如果想要打开一个文件用于读写，可以使用下面的语法：

```cpp
ifstream  afile;
afile.open("file.dat", ios::out | ios::in );
```


## 关闭文件
当 C++ 程序终止时，它会自动关闭刷新所有流，释放所有分配的内存，并关闭所有打开的文件
但是
你需要养成一个良好的编码习惯
下面是 close() 函数的标准语法
```cpp
void close();
```

## 写入文件

我们使用流插入运算符（ << ）向文件写入信息，就像使用该运算符输出信息到屏幕上一样。唯一不同的是，在这里使用的是 **ofstream** 或 **fstream** 对象，而不是 **cout** 对象。
写入示例：
```cpp
// read_example.cpp
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

int main()
{
	ifstream in;
	string input;
	
	// 输入
	in.open("file.in");
	in >> input;
	in.close();
	
	cout << input;
	
	return 0;
}
```

## 读取文件

我们使用流提取运算符（ >> ）从文件读取信息，就像使用该运算符从键盘输入信息一样。唯一不同的是，在这里使用的是 **ifstream** 或 **fstream** 对象，而不是 **cin** 对象。
```cpp
// output_example.cpp

#include <iostream>
#include <fstream>

using namespace std;

int main()
{
	ofstream out;
	out.open("file.out");
	out << "Hello, world" << endl;
	out.close();
	
	return 0;
}
```
#### 读写示例
这个示例将以读写模式打开一个文件。向文件afile.dat写入用户输入的信息后，程序从文件读取信息并输出
```cpp
#include <fstream>
#include <iostream>
using namespace std;
 
int main() {
    
   char data[100];
 
   // 以写模式打开文件
   ofstream outfile;
   outfile.open("afile.dat");
 
   cout << "Writing to the file" << endl;
   cout << "Enter your name: "; 
   cin.getline(data, 100);
 
   // 向文件写入用户输入的数据
   outfile << data << endl;
 
   cout << "Enter your age: "; 
   cin >> data;
   cin.ignore();
   
   // 再次向文件写入用户输入的数据
   outfile << data << endl;
 
   // 关闭打开的文件
   outfile.close();
 
   // 以读模式打开文件
   ifstream infile; 
   infile.open("afile.dat"); 
 
   cout << "Reading from the file" << endl; 
   infile >> data; 
 
   // 在屏幕上写入数据
   cout << data << endl;
   
   // 再次从文件读取数据，并显示它
   infile >> data; 
   cout << data << endl; 
 
   // 关闭打开的文件
   infile.close();
 
   return 0;
}
```

## 使用fstream读写

在 C++ 中，_std::fstream_ 是一个用于文件输入输出操作的类，它继承自 _std::istream_ 和 _std::ostream_，因此可以同时用于读取和写入文件。_std::fstream_ 提供了一种方便的方式来处理文本和二进制文件。

#### 基本用法：
要使用 _std::fstream_ 写文件，首先需要包含 fstream 头文件，然后创建一个 _std::fstream_ 对象，并使用 _open_ 方法打开文件。可以指定不同的文件模式，如输入模式（_std::ios::in_）、输出模式（_std::ios::out_）、追加模式（_std::ios::app_）等。

以下是一个简单的示例，演示如何使用 _std::fstream_ 写入文本文件
```cpp
#include <fstream>
#include <iostream>
int main() {
   std::fstream file;
   file.open("example.txt", std::ios::out); // 以输出模式打开文件
   if (!file) {
       std::cerr << "Unable to open file!" << std::endl;
       return 1; // 文件打开失败
   }
   file << "Hello, World!" << std::endl; // 写入文本
   file.close(); // 关闭文件
   return 0;
}
```
在上述示例中，程序在当前目录下创建一个名为 _example.txt_ 的文件，并写入 "Hello, World!"。

#### 追加模式:
如果希望在文件末尾追加内容，可以使用 _std::ios::app_ 模式打开文件。以下是一个示例：
```cpp
#include <fstream>
#include <iostream>
int main() {
   std::fstream file;
   file.open("example.txt", std::ios::app); // 以追加模式打开文件
   if (!file) {
       std::cerr << "Unable to open file!" << std::endl;
       return 1; // 文件打开失败
   }
   file << "Appending this line to the file." << std::endl; // 追加文本
   file.close(); // 关闭文件
   return 0;
}
```
执行上述程序后，_example.txt_ 文件的内容将变为：
```
Hello, World!
Appending this line to the file
```

