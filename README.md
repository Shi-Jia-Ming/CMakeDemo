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

##### cmake_minmum_required()

```cmake
cmake_minmum_required(VERSION <min>[...<policy_max>] [FATEL_ERROR])
```

_在3.12版本中新增了可选参数`<policy_max>`。_

这条指令设置该 CMake 项目最低的版本，还会更新策略设置，如下所述。

`<min>`和可选参数`<policy_max>`是 CMake 的版本，`...`就是字面意思。如果正在运行的 CMake 版本比`<min>`要求的版本更低，则项目会停止运行并且报错。`<policy_max>`版本如果被指定的话，最低是`<min>`版本，它影响着策略设置。

如果 CMake 的版本低于 3.12，那么`...`将被认为是一个版本组件的分割符，这就导致`...<max>`部分会被忽略，在3.12版本之前的行为将会被保留。

`FATAL_ERROR`参数可以被接收，但是在 CMake 2.6 或者更高版本中，这个参数被忽略。我们应当指定它以确保 CMake 2.4 以及耕地版本构建失败时显示错误，而不仅仅是警告。

##### project()

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

##### add_executable()

此命令用来添加可执行文件，可以添加：常规可执行文件、导入的可执行文件、可执行文件别名

###### 常规可执行文件

```cmake
add_executable(<name> [WIN32] [MACOSX_BUNDLE]
                [EXECLUDE_FROM_ALL]
                [source1] [source2...])
```
该命令用于根据命令后面列出的源文件构建出名为`<name>`的可执行文件。`<name>`对应逻辑名称并且在项目中必须是全局唯一的。具体的可执行文件的扩展名则是基于当前的操作系统(例如name.exe或name.sh)。

_3.11版本新特性：如果某个源文件在后面被加入了`target_sources()`中，那么它可以在这个指令中忽略。_

可执行文件将默认在构建的目录树中生成。查看关于`RUNTIME_OUTPUT_DIRECTORY`的文档来改变输出的位置，查看关于`OUTPUT_NAME`的文档来改变最终文件的名称。

###### 导入的可执行文件

```cmake
add_executable(<name> IMPOERTED [GLOBAL])
```

TODO

###### 可执行文件的别名

TODO

#### 需要编辑的文件

- [CMakeLists.txt](./Help/guide/tutorial/Step1/CMakeLists.txt)

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

### 练习2：指定C++标准

CMake 有一些特殊的变量，这些变量要么是在幕后创建的，要么在由项目代码设置时对 CMake 有意义。这些变量大多以`CMAKE_`开头。在你的项目中你应该避免发生重名。其中，有两个用户可以自己设置的、特殊的变量，分别是`CMAKE_CXX_STANDARD`和`CMAKE_CXX_STANDARD_REQUIRED`。这些可能会共同指定构建项目需要的C++标准。

#### 目标

添加关于C++11的标准。

#### 有帮助的资源

##### CMAKE_CXX_STANDARD

_该属性为3.1版本新增的属性。_

当`CMAKE_CXX_STANDARD`属性被指定，它的值就是`CXX_STANDARD`的默认值。

##### CMAKE_CXX_STANDARD_REQUIRED

当`CMAKE_CXX_STANDARD_REQUIRED`属性被指定，它的值就是`CXX_STANDARD_REQUIRED`的默认值。

##### set()

设置一个普通的、缓存的或者环境变量并给它们一个值。如果`<value>...`中指定了多个值，那么这些值将会以分号分隔并赋值给这个变量。

###### 设置普通的变量

```cmake
set(<variable> <value>... [PARENT_SCOPE])
```

定义或释放(取消定义)当前函数或目录作用域中的变量`<variable>`。
- 如果至少指定了一个`<value>`，那么这个变量将会被指定为这个值。
- 如果没有指定`<value>`，那么这个变量将会被释放。

如果`PARENT_SCOPE`被指定，则这个变量的作用域将变成当前作用域的父作用域。每一个函数或者目录都创建一个新的作用域，`block()`命令同样可以创建作用域。`set(PARENT_SCOPE)`将在父作用域设置变量的值，调用函数等。

###### 设置缓存变量

```cmake
set(<variable> <value>... CACHE <type> <docstring> [FORCE])
```

TODO


###### 设置环境变量

TODO

#### 需要编辑的文件

- [CMakeLists.txt](./Help/guide/tutorial/Step1/CMakeLists.txt)
- [tutorial.cxx](./Help/guide/tutorial/Step1/tutorial.cxx)

#### 完成教程练习

继续编辑Step1目录中的文件，从TODO4做到TODO6。首先，我们需要编辑tutorial.cxx，添加C++11标准中的一个新功能。然后更新CMakeLists.txt，以要求C++11标准。

### 练习3：添加版本号并配置头文件

有时候一个在源代码中同样有效的变量也是非常有帮助的。这种情况下，我们期望打印出项目的版本号。

其中一种解决方法就是使用一个配置好的头文件。我们可以创建一个输入文件并使用一个或几个变量来代替。这些变量有特殊的语法，类似`@VAR@`。然后我们使用`configure_file()`命令把这个输入文件复制到一个给定的输出文件中并用CMakeLists.txt中的`VAR`变量的值代替。

当我们可以直接在源代码中编辑版本时，使用这个特性是更常见的，因为它创建了真实的变量的单一来源，避免了重复。

#### 目标

定义并报告项目的版本号。

#### 有帮助的资源

##### <PROJECT-NAME>_VERSION_MAJOR

变量`<PROJECT-NAME>_VERSION`的第一个版本号。（变量`<PROJECT-NAME>_VERSION`在[`project()`](#project())命令中被指定。）

##### <PROJECT-NAME>_VERSION_MINOR

变量`<PROJECT-NAME>_VERSION`的第一个版本号。（变量`<PROJECT-NAME>_VERSION`在[`project()`](#project())命令中被指定。）

##### configure_file()

该命令拷贝一个文件到另一个位置并修改它的内容。

```cmake
configure_file(<input> <output>
                [NO_SOURCE_PERMISSIONS | USE_SOURCE_PERMISSIONS |
                FILE_PERMISSIONS <permissions>...]
                [COPYONLY] [ESCAPE_QUOTES] [@ONLY]
                [NEWLINE_STYLE [UNIX|DOS|WIN32|LF|CRLF] ])
```

拷贝一个`<input>`文件到`<output>`文件并用`@VAR@`, `${VAR}`, `$CACHE{VAR}`或者`$ENV{VAR}`等的值替代文件中变量的值。每一个变量都会被当前的变量的值代替，如果变量没有指定，那就用空字符串来代替。除此之外，下面的这种定义方式：

```cmake
#cmakedefine VAR ...
```

也会被替换成

```c
#define VAR
```

如果CMakeLists.txt中定义如下：

```cmake
#cmakedefine01 VAR
```

在新的文件中将会被替换成：

```c
#define VAR 0
#define VAR 1
```

如果CMakeLists.txt中定义如下：

```cmake
#cmakedefine01 VAR ...
```

在新的文件中将会被替换成：

```c
#define VAR ... 0
#define VAR ... 1
```

这将导致一些错误。

###### 参数列表

- `<input>`：输入文件的路径。可以根据`CMAKE_CURRENT_SOURCE_DIR`来指定相对路径。这个参数必须是一个文件，不可以是文件夹。
- `<output>`：输出文件的路径。根据`CMAKE_CURRENT_BINARY_DIR`来指定相对路径。如果值为现存的目录名，那么输出文件和输入文件的名称相同。如果路径包含一个不存在的目录，那么这个目录会被自动创建。

- `NO_SOURCE_PERMISSIONS`：如果指定了这个选项，那么`<input>`文件的权限不会被拷贝。文件的默认权限是`0644`(-rw-r--r--)。
- `USE_SOURCE_PERMISSIONS`：如果指定了这个选项，那么`<input>`文件的权限会被拷贝。
- `FILE_PERMISSIONS <permissions> ...`：如果指定了这个选项，那么`<output>`文件的权限会忽略上面两个参数，被设置成`<permissions>`。`<permissions>`的格式和`chmod`命令的格式相同。
- `COPYONLY`：如果指定了这个选项，那么`<input>`文件中的变量和其他内容不会被替换。这个参数通常不会与`NEWLINE_STYLE`一起使用。
- `ESCAPE_QUOTES`：如果指定了这个选项，那么`<input>`文件中的双引号会被转义（C语言标准）。
- `@ONLY`：如果指定了这个选项，那么`<input>`文件中只有`@VAR@`形式的变量会被替换。这个参数在配置要用到`${VAR}`的脚本的时候很有用。
- `NEWLINE_STYLE [UNIX|DOS|WIN32|LF|CRLF]`：如果指定了这个选项，那么`<input>`文件中的换行符会被替换成指定的格式。默认的换行符是`UNIX`。

###### 举例

考虑一个源文件`foo.h.in`：

```in
#cmakedefine FOO_ENABLE
#cmakedefine FOO_STRING "@FOO_STRING@"
```

邻近的CMakeLists.txt文件就可以使用该文件来生成头文件

```cmake
option(FOO_ENABLE "Enable Foo" ON)
if(FOO_ENABLE)
    set(FOO_STRING "foo")
endif()
configure_file(foo.h.in foo.h @ONLY)
```

这段命令在构建的目录中创建了一个foo.h文件，如果`FOO_ENABLE`被设置，那么文件的内容就是：

```c
#define FOO_ENABLE
#define FOO_STRING "foo"
```

否则这个文件将包含：

```c
/* #undef FOO_ENABLE */
/* #undef FOO_STRING */
```

然后就可以使用`target_include_directories()`命令将这个文件包含到目标中：

```cmake
target_include_directories(<target> [SYSTEM] <INTERFACE|PUBLIC|PRIVATE> "${CMAKE_CURRENT_BINARY_DIR}")
```

这样源代码就可以引用这个头文件：

```c
#include <foo.h>
```

##### target_include_directories()

为目标添加包含头文件的目录。

```cmake
target_include_directories(<target> [SYSTEM] [AFTER|BEFORE]
                            <INTERFACE|PUBLIC|PRIVATE> [items1...]
                            [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
```

当编译一个指定的目标时，CMake会在编译器的搜索路径中添加指定的目录。`<target>`必须是已经被创建的目标名，而且不能是一个目标别名。

使用`AFTER`或`BEFORE`选项，可以控制目标包含的目录的顺序。`AFTER`表示目录会出现在包含列表的末尾，`BEFORE`表示目录会出现在包含列表的开头。如果省略了选项，那么目录会出现在包含列表的中间。

`PUBLIC`, `INTERFACE`和`PRIVATE`选项指定包含目录的可见性。`PUBLIC`表示目录会被包含在生成规则中，`INTERFACE`表示目录只被包含在导入目标中，`PRIVATE`表示目录只被包含在目标中。

_3.11版本新特性：`INTEFACE`可以被设定在导入的目标上。_

如果对一个目标重复调用，那么添加的头文件目录将会按照调用顺序往后面追加。

如果`SYSTEM`选项被指定，那么在一些平台CMake会告诉编译器这些目录是系统的头文件目录。

#### 需要编辑的文件

- [CMakeLists.txt](./Help/guide/tutorial/Step1/CMakeLists.txt)
- [tutorial.cxx](./Help/guide/tutorial/Step1/tutorial.cxx)

#### 完成教程练习

从TODO7到TODO12。这次练习，我们通过CMakeLists.txt为项目添加版本号。同样是在这个文件中，使用`configure_file()`命令将一个给定的输入文件拷贝到输出文件并修改输入文件中的一些变量。

接下来，创建一个输入头文件TutorialConfig.h.in，在这里定义版本号，这个版本号会被`configure_file()`命令替换。

最后，更改tutorial.cxx文件，打印项目的版本号。

#### 构建并运行

重新构建并运行项目。

```bash
cd Step1_build
cmake ../Step1
cmake --build .
```

当不给函数传入参数的时候，函数就会输出版本号。