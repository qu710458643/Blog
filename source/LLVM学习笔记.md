# LLVM

学习《LLVMCookBook》

## 第一章 LLVM设计与使用

### 传统编译器设计

```{mermaid}

   graph LR
      A[源码] --> B[编译器前端] 
      B --> C[优化器] 
      C --> D[编译器后端] 
      D --> E[机器码]
```

编译器不同于解释器，解释器可以将高级语言边转译边执行。但是编译器需要将所有的源代码编译成机器代码之后，然后才能执行，所以如图不管传统的还是LLVM都一定是源代码输入，输出是机器代码（也就是0101组合），中间还分前端、优化器、后端。

1. 编译器前端
   编译器前端的任务是解析源代码。它会进行：词法分析，语法分析，语义分析, 检查源代码是否存在错误，然后构建抽象语法树(Abstract Syntax Tree,AST) ,LLVM的前端还会生成中间代码(intermediate representation , IR)。
2. 优化器
   优化器负责进行各种优化。改善代码的运行时间，例如消除冗余计算等
3. 编译器后端
   将代码映射到目标指令集。生成机器语言，并且进行机器相关的代码优化

### LLVM设计

```{mermaid}

   graph LR
      A1[C] --> B1[Clang C/C++/ObjC] 
      B1 --> C[LLVM优化] 
      C --> D1[LLVM x86]
      A2[Fortran] --> B2[LLVM-GCC] 
      B2 --> C
      C --> D2[LLVM PowerPC]
      A3[Haskell] --> B3[GHC] 
      B3 --> C 
      C --> D3[LLVM Arm]
```

当编译器决定支持多种源语言或多种硬件架构时，LLVM最重要的地方就来了。 其他的编译器如GCC,它方法非常成功，但由于它是作为整体应用程序设计的, 因此它们的用途受到了很大的限制。
LLVM设计的最重要方面是，使用通用的代码表示形式（IR），它是用来在编译器中标识代码的形式（其实就是一种中间代码）。所以LLVM可以为任何变成语言独立编写前端，并且可以为任意硬件架构独立编写后端。

C语言的编译过程如下:

```{mermaid}
   graph LR
      A[C源码] -- clang -emit-llvm -S --> B[LLVM中间码] 
      B -- opt --> C[优化] 
      C -- llvm-as --> D[LLVM 位流码bitcode] 
      D -- llc --> E[目标平台汇编码] 
      E -- llvm-dis --> D
      D -- llvm-link --> D
```
