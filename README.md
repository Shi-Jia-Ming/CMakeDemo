# 介绍

CMake教程提供了涵盖常见构建的分布指南 CMake 帮助解决的系统问题。看看各种主题如何在示例项目中一起工作会非常有帮助。

# 步骤

教程源代码示例可在[这里](https://cmake.org/cmake/help/latest/_downloads/56b1b244b81fa3a2a746617b0c1a2987/cmake-3.28.0-rc1-tutorial-source.zip)找到。每个步骤都有自己的子目录，其中包含可用作起点的代码。教程示例是渐进式的，因此每个步骤为上一步提供完整的解决方案。

## 第一步：基本起点

我们该从哪里开始使用 CMake？此步骤将介绍一些 CMake 的基本语法、命令和变量。因为这些概念是介绍，我们将通过三个练习并创建一个简单的 CMake 的项目。

此步骤中的每个练习都将从一些背景信息开始。然后，会提供一个目标和一些有用的资源。`需要编辑的文件`章节中的每个文件都在Step1目录中，并且包含一些代办评论。每一个评论都代表一两行代码的更改或添加。这些代办需要按照顺序完成，`开始`章节中将会提供一些有帮助的提示并引导你进行练习。在`构建和运行`章节中将会一步一步构建并检测这些练习。每个实验的最后都会讨论这个练习的解决方案。

需要注意的是，教程中的每个步骤都为下一个步骤构建做出贡献，所以，举例来说，Step2的代码是Step1的完整解决方案。

### 练习1：构建一个基本的项目

最基础的 CMake 项目是对单个可执行文件进行构建。对于这样的简单项目，CMakeLists.txt文件中只需要三行命令。

> 注意：尽管 CMake 支持大写、小写和大小写混用的命令，但是还是更推荐小写，下面的教程中也会使用小写命令。

任何项目的CMakeLists.txt文件必须以一个指定 CMake 版本的命令`cmake_minimum_required()`开头。这创建了一个策略的设置并确保了下面的 CMake 函数都依赖于正确的 CMake 版本。

要开始一个项目，我们需要使用`project()`命令去设置项目的名称。每一个项目都需要调用，并且这个调用必须写在`cmake_minmum_required()`命令的下一个。正如我们接下来将会看到的，这个命令也可以用来指定一些其他的项目级的信息比如项目使用的语言、项目的版本号等。

最后，`add_executable()`命令让 CMake 根据指定的源文件创建一个可执行文件。

#### 目标

理解如何创建一个简单的 CMake 项目。

#### 有帮助的资源

```cmake
cmake_minmum_required(VERSION <min>[...<policy_max>] [FATEL_ERROR])
```

_在3.12版本中新增了可选参数`<policy_max>`。_

这条指令设置该 CMake 项目最低的版本，还会更新策略设置，如下所述。

`<min>`和可选参数`<policy_max>`是 CMake 的版本，`...`就是字面意思。如果正在运行的 CMake 版本比`<min>`要求的版本更低，则项目会停止运行并且报错。`<policy_max>`版本如果被指定的话，最低是`<min>`版本，它影响着策略设置。

如果 CMake 的版本低于 3.12，那么`...`将被认为是一个版本组件的分割符，这就导致`...<max>`部分会被忽略，在3.12版本之前的行为将会被保留。

`FATAL_ERROR`参数可以被接收，但是在 CMake 2.6 或者更高版本中，这个参数被忽略。我们应当指定它以确保 CMake 2.4 以及耕地版本构建失败时显示错误，而不仅仅是警告。

```cmake
project(<PROJECT-NAME> [<language-name>...])
project(<PROJECT-NAME>
        [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [DESCRIPTION <project-description-string>]
        [HOMEPAGE_URL <url-string>]
        [LANGUAGES <language-name>])
```

这条命令设置此项目的名称，并且将它存储在`PROJECT_NAME`变量中。当它在最高的等级中被调用，CMakeLists.txt还会将项目的名称存储在变量`CMAKE_PROJECT_NAME`。

除此之外，这条命令还设置了下面的变量：
- `PROJECT_SOURCE_DIR, <PROJECT-NAME>_SOURCE_DIR`：项目的根目录的绝对路径；
- `PROJECT_BINARY_DIR, <PROJECT-NAME>_BINARY_DIR`：执行CMake的目录的绝对路径；
- `PROJECT_IS_TOP_LEVEL, <PROJECT-NAME>_IS_TOP_LEVEL`：当前项目是否为最高等级的；
    - _3.21版本的新功能_

参数列表
- `VERSION <version>`：该参数是可选的，很可能不会用到，除非`CMP0048`策略设置为`NEW`。这个参数由非负整数分量组成，即`<major>[.<minor>[.<patch>[.<tweak>]]]`，并设置了以下变量：
    - `PROJECT_VERSION, <PROJECT-NAME>_VERSION`
    - `PROJECT_VERSION_MAJOR, <PROJECT-NAME>_VERSION_MAJOR`
    - `PROJECT_VERSION_MINOR, <PROJECT-NAME>_VERSION_MINOR`
    - `PROJECT_VERSION_PATCH, <PROJECT-NAME>_VERSION_PATCH`
    - `PROJECT_VERSION_TWEAK, <PROJECT-NAME>_VERSION_TWEAK`

    _3.12版本新特性：如果`project()`命令在最高层的CMakeLists.txt中调用，那么版本信息也会被存储在变量`CMAKE_PROJECT_VERSION`中。_

- `DESCRIPTION <project-description-string>`：该参数为3.9版本新增的参数。可选参数，设置该参数之后，下面的变量也会被设置：
    - `PROJECT_DESCRITION`

- `DESCRIPTION <project-description-string>`：该参数为3.9版本新增的参数。可选参数，该参数设置后，下面的变量也会被设置：
    - `PROJECT_DESCRIPTION, <PROJECT-NAME>_MAJOR`

    需要注意，这个介绍一般是相对较少的，通常只有几个词、几句话。
    
    当`project()`命令在最高层的CMakeLists.txt中被调用时，这个简介也会被存储在变量`CMAKE_PROJECT_DESCRIPTION`中。在3.12版本中还新增了`<PROJECT-NAME>_DESCRIPTION`变量。

- `HOMEPAGE_URL <url-string>`：该参数为3.12版本新增的参数。可选参数。该参数设置后，下面的变量也会被设置：
    - `PROJECT_HOMEPAGE_URL, <PROJECT-NAME>_HOMEPAGE_URL`

    `<url-string>`中需要为项目设置规范的主URL。

    当`project()`命令在最高层的CMakeLists.txt中被调用时，这个URL也会被存储在变量`CMAKE_PROJECT_HOMEPAGE_URL`中。

- `LANGUAGE <language-name>...`：可选参数，没有`LANGUAGE`关键字也可以指定，选择构建该项目需要的语言。支持的语言如下：`C`, `CXX(C++)`, `CSharp`, `CUDA`, `OBJC(Object -C)`, `OBJCXX(Object-C++)`, `Fortran`, `HIP`, `ISPC`, `Swift`, `ASM`, `ASM_NASM`, `ASM_MARMASM`, `ASM_MASM` 和 `ASM-ATT`。

    如果支持`ASM`语言，那么它需要放在最后面以确保 CMake 可以检查其他的编译器(比如C语言)是否需要对汇编语言进行支持。

    如果没有设置语言，默认支持的语言是`C`和`CXX`。使用`NONE`语言或者使用`LANGUAGE`关键字但是不列举语言可以设置项目不对任何语言支持。

#### 需要编辑的文件

[CMakeLists.txt](./CMakeLists.txt)

#### 完成教程练习

源代码在Help/guide/tutorial/Step1目录中，cxx文件不需要再做更改了。只需要在CMakeLists.txt中从 TODO1 完成到 TODO3。

#### 构建并运行

当你完成了TODO1到TODO3的要求，下面就要准备构建和运行项目了。首先，运行`cmake`程序，或者运行`cmake-gui`配置项目然后使用你自己选择的构建工具构建项目。

比如说，我们可以从命令行进入/Help/guide/tutorial目录，然后创建一个构建目录：

```bash
mkdir Step1_build
```

然后，导航到构建目录并运行`cmake`来配置项目并产生一个本地的构建系统。

```bash
cd Step1_build
cmake ..
```

然后调用构建系统来完全的编译项目。

```bash
cmake --build .
```

对于多配置生成器(比如Visual Studio)，首先需要进入正确的子目录，例如：

```bash
cd Debug
```

最后，尝试运行新构建的可执行程序。