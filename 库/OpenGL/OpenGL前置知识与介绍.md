由于OpenGL是一个==图形AP==I，并不是一个独立的平台，它需要一个编程语言来工作，在这里我们使用的是C++。所以，对C++的熟练掌握在学习这个教程中是必不可少的。当然，我仍将尝试解释大部分用到的概念，包括一些高级的C++话题，所以，你并不一定要是一个C++专家才能来学习。不过，请确保你至少应该能写个比 ‘Hello World’ 复杂的程序。
除此之外，我们也将用到一些==数学知识==（线性代数、几何、三角学）

# OpenGL
|原文|[OpenGL](http://learnopengl.com/#!Getting-started/OpenGL)|
|---|---|
|作者|JoeyDeVries|
|翻译|gjy_1992, Krasjet|
|校对|暂未校对|
OpenGL规范严格规定了每个函数该如何执行，以及它们的输出值。至于内部具体每个函数是如何实现(Implement)的，将由OpenGL库的开发者自行决定
因为OpenGL规范并没有规定实现的细节，具体的OpenGL库允许使用不同的实现，只要其功能和结果与规范相匹配（亦即，作为用户不会感受到功能上的差异）。

[opengl3.3规范文档](https://www.opengl.org/registry/doc/glspec33.core.20100311.withchanges.pdf)如果你想知道每个函数**具体的**运作方式，这个规范也是一个很棒的参考。

## 核心模式与立即渲染模式
早期的OpenGL使用立即渲染模式（Immediate mode，也就是固定渲染管线），这个模式下绘制图形很方便。OpenGL的大多数功能都被库隐藏起来，开发者很少有控制OpenGL如何进行计算的自由。
从OpenGL3.2开始，规范文档开始==废弃立即渲染模式==，并鼓励开发者在OpenGL的核心模式(Core-profile)下进行开发，这个分支的规范完全移除了旧的特性。

当使用OpenGL的核心模式时，OpenGL迫使我们使用现代的函数。当我们试图使用一个已废弃的函数时，OpenGL会抛出一个错误并终止绘图。现代函数的优势是更高的灵活性和效率，然而也更难于学习。立即渲染模式从OpenGL**实际**运作中抽象掉了很多细节，因此它在易于学习的同时，也很难让人去把握OpenGL具体是如何运作的。现代函数要求使用者真正理解OpenGL和图形编程，它有一些难度，然而提供了更多的灵活性，更高的效率，更重要的是可以更深入的理解图形编程。

## 状态机
OpenGL自身是一个巨大的状态机(State Machine)：一系列的变量描述OpenGL此刻应当如何运行。OpenGL的状态通常被称为OpenGL上下文(Context)。
我们通常使用如下途径去更改OpenGL状态：设置选项，操作缓冲。最后，我们使用当前OpenGL上下文来渲染。
假设当我们想告诉OpenGL去画线段而不是三角形的时候，我们通过改变一些上下文变量来改变OpenGL状态，从而告诉OpenGL如何去绘图。一旦我们改变了OpenGL的状态为绘制线段，下一个绘制命令就会画出线段而不是三角形。
当使用OpenGL的时候，我们会遇到一些==状态设置函数==(State-changing Function)，这类函数将会改变上下文。以及==状态使用函数==(State-using Function)，这类函数会根据当前OpenGL的状态执行一些操作。

## 对象
在OpenGL中一个对象是指一些选项的集合，它代表OpenGL状态的一个子集。比如，我们可以用一个对象来代表绘图窗口的设置，之后我们就可以设置它的大小、支持的颜色位数等等。可以把对象看做一个C风格的结构体(Struct)：

```c++
struct object_name {
    float  option1;
    int    option2;
    char[] name;
};
```
当我们使用一个对象时，通常看起来像如下一样（把OpenGL上下文看作一个大的结构体）：

```c++
// OpenGL的状态
struct OpenGL_Context {
    ...
    object* object_Window_Target;
    ...     
};
```

```cpp
// 创建对象
unsigned int objectId = 0; glGenObject(1, &objectId); // 绑定对象至上下文 
glBindObject(GL_WINDOW_TARGET, objectId); // 设置当前绑定到 GL_WINDOW_TARGET 的对象的一些选项
glSetObjectOption(GL_WINDOW_TARGET, GL_OPTION_WINDOW_WIDTH, 800); glSetObjectOption(GL_WINDOW_TARGET, GL_OPTION_WINDOW_HEIGHT, 600); // 将上下文对象设回默认 
glBindObject(GL_WINDOW_TARGET, 0);
```
这一小段代码展现了你以后使用OpenGL时常见的工作流。我们首先创建一个对象，然后用一个id保存它的引用（实际数据被储存在后台）。然后我们将对象绑定至上下文的目标位置（例子中窗口对象目标的位置被定义成GL_WINDOW_TARGET）。接下来我们设置窗口的选项。最后我们将目标位置的对象id设回0，解绑这个对象。设置的选项将被保存在objectId所引用的对象中，一旦我们重新绑定这个对象到GL_WINDOW_TARGET位置，这些选项就会重新生效。

> 目前提供的示例代码只是OpenGL如何操作的一个大致描述，通过阅读以后的教程你会遇到很多实际的例子。

