MexScript
=========
原文地址：<http://wiki.xentax.com/index.php?title=MexScript>

译文地址：<https://github.com/wisaly/document/blob/master/MexScript.md>

译者：[Kris](http://www.maqiming.com)

## 简介 ##
BMS代表Binary MultiEx Scripts（二进制MultiEx脚本）。 这是[MultiEx Commander](http://wiki.xentax.com/index.php?title=MultiEx_Commander)（MexCom)用来“解开”一组常见的[GRAFs](http://wiki.xentax.com/index.php?title=GRAFs)（Game Resource Archive Formats，游戏资源文件格式）的脚本格式。 这种脚本由一个文本文件组成，它包含一系列能够被解释器执行的指令。解释器使用这些指令在GRAF文件中搜索关键数据，例如构成文件的名字、偏移量和大小。然后就可以用MultiEx Commander从这些文件中提取和替换资源了。

**这种脚本的强大之处在于使用简单、格式简洁。任何新的游戏资源文件只要几行脚本就可以支持，所以你不必费心编写大型插件或者底层脚本的代码。**

使用MexCom，用户可以运行他们自己编写的BMS来处理他们的文件。在[XeNTaX WIKI](http://wiki.xentax.com/)和[XeNTaX Forum](http://forum.xentax.com/)能够找到非常多的BMS，可以帮助你理解脚本的工作原理。你也可以添加新的BMS到WIKI里，那里有一个能够被MexCom调用的数据库。见：<http://wiki.xentax.com/index.php/BMS_to_Wiki>。

MexScriptor是一个MexCom的配套工具，它可以编写BMS脚本、保存为能被MexCom读取BMS文件。这个编辑器能在MexBinderPlus中找到。BMS打包工具可以把一组BMS文件保存为一个MRF（MultiEx Resource File，MultiEx资源文件）。在当前版本，帮助对话框里提供了可用语句的基本概览。

### 基础 ###
BMS脚本的每一行都是一条语句。语句又可以被划分为词法单元（token）。

每行只能包含一条语句，但仍然需要以分号（;）结束。这有点多余，没错，但这样是为了表达跨行语句。事实上，将来某个更新之后也许就不再需要分号了。但现在，每行的末尾必须是一个分号，而且分号之前有一个空格。

如果你从因特网上复制了一个BMS脚本，在分号后面常有多余的空格。保存之前必须先删掉空格，否则将会返回一个错误。

### 注释 ###
截止到4.3版本，允许添加单行注释，在行首添加一个井号（#），而单引号是被禁止的。例如：

	# 我是一行注释
    # 但每一个新行
    # 都需要井号
    # 它们不会“持续下去”

### 大小写 ###
语句是大小写不敏感的，例如你可以把IDString写作idStrIng。语句和操作符需要被空格分隔成几列，所以大小写敏感是不必要的。然而，变量名**是**大小写敏感的。所以，如果你声明了一个变量‘FileOffset’，确保稍后引用这个变量时的大小写与声明一致。

## 变量操作 ##
BMS脚本可以声明变量，然后对变量进行基本算术运算或控制操作。

只要在Set、Get或SavePos语句中第一次使用变量，就可以声明一个新变量。变量**必须**在别的语句使用它之前进行声明。

Set语句可以用来把一个值赋给你选择的变量。这个值可以是一个数字、其它变量或一个文本串。例如，你可以用`Set MyVar Long 1024 ;`把1024这个值赋给一个长整形（32位）变量MyVar。另外，你可以用`Set MyVar Long MyVar2 ;`把MyVar2赋给MyVar。最后，你可以用`Set MyVar String MultiEx ;`，这样MyVar现在就成了一个值为'MultiEx'的字符串变量。

Math语句可以用来做一些简单的算术计算。`Math MyVar += 2 ;`将使MyVar增加2。类似的，`Math MyVar /= MyVar2 ;`将使MyVar除以MyVar2。注意这个命令不仅使用目标变量的值，同时也将它的值变为计算的结果。

最后，String语句能够做一些简单的字符串操作。准确的说，它将另一个字符串（或数值转换成字符串）添加到目标字符串中。例如，`String MyVar += MyVar2`将在MyVar字符串的末尾追加MyVar2中的字符串。注意MyVar2是一个数字值，它将被转换成一个字符串并追加到MyVar后。

## 控制结构 ##
BMS脚本按从上到下的顺序执行，除非碰到一个控制结构。这些控制结构包括Do..While、For..Next和经典的If..Then..Else。

_提问：是否存在一个结构，等价于C语言中的`while{}`或BASIC中的`WHILE..WEND`？换言之，能否使用一个预计算（pre-evaluation）的控制结构而不是后计算（post-evaluation）？_好吧，你可以使用IF...THEN...ELSE循环并改变IF控制结构中使用的参数。

### If..Then..Else ###
If..Then..Else的结构是这样的：

    If <var1> <operator> <var2> ;
    (do something)
    Else ;
    (do something else)
    EndIf ;
一个If结构总是以`EndIf ;`结尾。注意`Else ;`是可选的。

### Do..While ###
Do..While的结构是这样的：

    Do ;
    statement 1 ;
    --
    statement n ;
    While condition ; 
条件中比较两个值，例如，一个变量和一个常量，如果条件的结果是true，那么将会返回到语句1执行。可用的比较操作符有：

    < 小于
    > 大于
    <> 不等于
    = 等于
    >= 大于等于 
    <= 小于等于

### For..Next ###
For..Next 的结构是这样的：

    For I = 1 to M;
    statement 1 ;
    --
    statement n ;
    Next I ; 
这个例子顺序执行For和Next之间的语句，迭代M次。

## 语句参考 ##

### CLog ###
    CLog name offset size offsetoffset resourcesizeoffset uncompressedsize uncompressedsizeoffset ;

CLog语句和Log语句类似，是发给解释引擎的一个信号，表示已经找到了一个特殊的**压缩过的**资源，并且包含资源的名字、GRAF文件中的绝对偏移量、占用了GRAF文件中多少个连续的字节、在GRAF中到前述偏移量和大小的偏移量、资源解压缩后的大小（如果已知）和资源解压缩后的大小在文件中的位置。

- name: 表示资源名字的字符串，如果不能判定资源名字，把它置为NULL（""）。
- offset: 资源在GRAF中的绝对偏移量。
- size: 资源在GRAF文件中从offset指向的位置开始占用的连续字节数。
- offsettooffset: GRAF文件中指向资源的指针变量的偏移量。支持替换功能需要使用此参数（见 ImpType语句）。
- sizeoffset: GRAF文件中表示资源大小的变量的偏移量。 支持替换功能需要使用此参数（见 ImpType语句）。
- uncompressedsize: 解压缩后资源将占用的字节数。
- uncompressedsizeoffset: GRAF文件中表示原始资源原始大小的变量的偏移量。

### Do ###
    Do ;

Do语句是一个Do..While控制结构的开始。
 
### FindLoc ###
    FindLoc <var> <datatype> <text/number> <filenumber>

FindLoc在打开的文件中查找一个字符串，当找到时将偏移量返回到一个用户变量中。
例如：

    FindLoc MyOffset String RIFF 0 ;
将在文件0中查找字符串'RIFF'，当找到时将偏移量赋给MyOffset。

### For ###
是标准For...Next循环的一部分。
例如：

    For T = 1 To FileNum
    {some chore}
    Next T
这将给T赋值1，并且执行{some chore}语句Filenum次（无论之前赋值的数值是多少）。For循环以Next语句结束。支出循环变量（例子中是T）是必须的。

### Get ###
    Get VarName Type File
从File文件中的当前位置读取数据类型Type的值，赋给（如果需要的话就创建）'VarName'变量（任何名字都可以）。例如：

    Get FC Long 0
将从文件0中的当前位置读取一个32位数值，赋给（并创建）名叫FC的变量。这个例子中的文件0应该用脚本事先处理过，通常是资源文件。

一些可用的数据类型：

    Long 32位值（4字节，小字节序）
    Int 16位值（2字节，小字节序）
    Byte 8位值（1字节）
    ThreeByte 24位值（3字节，小字节序）
    String 零结尾的字符串（字符串最后一个字节是0）

###  GetDString ###
    GetDString VarName NumberOfCharacters File
获得一个字符串，从File文件中读取NumberOfCharacters个字符，并把它存入变量'VarName'中。当字符串是非零结尾时很有用，但需要事先判定大小（preDetermined size，因此语句中有字母D）。

### GoTo ###
    GoTo pos file ;
GoTo语句导致解释器跳转到GRAF文件中的指定偏移量。

- pos: 跳转的偏移量。
- file: 执行这个命令的打开的文件。通常是文件0（主文件）。BMS在一个文件上运行，通常自动将该文件设为0。脚本中使用Open语句打开的任何新文件都将获得一个新的数字，以递增的方式。

### IDString ###
    IDString filenumber bytes ;
IDString语句顺序按字节比较，以检查一个确定的签名。如果签名不匹配，那么BMS脚本将会终止。

filenumber: ID字符串应用的文件（0=主文件）。这个字符串通常在0偏移量的位置开始。
bytes: 用来比较的原始文本字节。注意当它没有使用引号围起来的时候不是一个典型的字符串。它只是原始文本字节，这意味着不能包含空格。
例如：

    IDString 0 BIFFV1 ;
这个语句验证一个文件的前6个字节（偏移量0..5）是否包含字符"BIFFV1"，这是 [Baldur's Gate BIFF](http://wiki.xentax.com/index.php?title=Baldurs_Gate_BIF)格式的签名。

### ImpType ###
    ImpType <Imptype>
ImpType用来告诉MultiEx一个格式支持资源替换，表示资源的大小和（或）偏移量的参数的偏移量应该在Log和CLog语句中使用。

ImpType可以是：

- Standard: 告诉MultiEx指定了ResourceOffset和ResourceSize参数，并且将会被记录。
- StandardTail: 告诉MultiEx资源信息不在开头，而是在末尾，因此后面不全是资源数据。并且告诉MultiEx指定了ResourceOffset和ResourceSize参数，并且将会被记录。
- SFileSize : 告诉MultiEx只有ResourceSize的偏移量能够在Log语句中指定。
- SFileOff : 告诉MultiEx只有ResourceOffset的偏移量能够在Log语句中指定。  

### Log ###
    Log name offset size offsetoffset resourcesizeoffset ;
Log语句是发给解释引擎的一个信号，表示已经找到了一个特殊的资源，并且包含资源的名字、GRAF文件中的绝对偏移量、占用了GRAF文件中多少个连续的字节、在GRAF中到前述偏移量和大小的偏移量。

name: 表示资源名字的字符串，如果不能判定资源名字，把它置为NULL（""）。
- offset: 资源在GRAF中的绝对偏移量。
- size: 资源在GRAF文件中从offset指向的位置开始占用的连续字节数。
- offsettooffset: GRAF文件中指向资源的指针变量的偏移量。支持替换功能需要使用此参数（见 ImpType语句）。
- sizeoffset: GRAF文件中表示资源大小的变量的偏移量。 支持替换功能需要使用此参数（见 ImpType语句）。

### Math ###
    Math var1 op var2 ;
Math语句对一个变量执行算术操作。

- var1: 要被修改的变量
- op: 要执行的算术操作。可用的运算符包括：
	- += : var2加var1，和存入var1。
	- -= : var1减var2，差存入var1。
	- *= : var1乘以var2，积存入var1。
	- /= : var1除以var2，商存入var1。 
- var2: 要和第一个变量计算的值。注意这可以是一个变量也可以是常量。

### Next ###
    Next VarName
参考For语句。是For循环结束的标记，在每次循环体执行之后'VarName'变量就自增，直到'VarName'变量达到For语句中的限制。

### Open ###
    Open Folder/Specifier Filename/Extension File
Open语句将会打开指定文件。在MultiEx3中目录需要分开指定。另外，可以使用目录的别名（标签）：

- FileDir: 和处理过的文件同一个目录（例如文件0）。
- FDDE: 和文件同一个目录，同一个文件名，但扩展名不同。 

你也必须提供文件得到的句柄（例如0=主文件，而所有新打开文件的句柄都应该递增：1,2,3等）。

例如：

    Open FDDE FAT 1 ;
这将打开一个和主文件同目录同名的文件，但扩展名是*.fat，句柄是1。

    Open FileDir sector.h 1 ;
这将打开一个和主文件同目录，名为sector.h的文件，句柄是1。

### SavePos ###
    SavePos VarName File
获取文件的当前位置，将结果存入名为'VarName'的变量中。

### Set ###
    Set VarName Var/Number ;
将指定数值或变量赋给'VarName'变量。

例如:

    Set A B ;
    Set A 1024 ;
第一句A被赋予B的值。第二句，A被赋予1024。

### While ###
    Do ;
    {some chore}
    While Varname Criterium VarName2 ;
是Do..While循环结束的标识，检查循环终止的条件是否满足。它检查'VarName'的值与'VarName2'的值是否满足条件。见控制结构。

### String ###
    String VarName1 +=/-= VarName2
String用来做简单的字符串操作。你可以使用+=或-=操作符对两个字符串进行“加”或“减”操作。 +=将会在末尾追加，-=将会在末尾截断。你也可以在var2上使用一个数值变量，数字会被转换成字符串再追加到第一个字符串后。

### CleanExit ###
    CleanExit
这将结束所有运行的操作，解释器将释放内存并推出。
This will abort all running operations, clean up and exit the interpreter 

## Statements (MexScriptor's Help)  ##

## 范例 ##
别忘了，wiki上已经有了[很多的BMS脚本范例](http://wiki.xentax.com/index.php/Special:Search?search=BMS&fulltext=Search)，你也可以在[XeNTaX](http://forum.xentax.com/viewtopic.php?t=1086)论坛上找到一个巨大的列表。
