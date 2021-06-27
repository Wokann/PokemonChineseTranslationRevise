---
last_modified_at: 2021-06-28 00:00
title: 汉化工具说明
---
## 更新日志
- 1.0.1（2020-02-22）：
  - 最初发布版本。
- 1.1.0（2020-02-28）：
  - 将标点符号所用字体改为 MS Gothic。
  - 将中文字体与英文字体和数字下基线基本对齐。
  - 根据《心金／魂银》的字体进行修正。
  - 其他零碎的修正。
- 1.2.0（2020-03-05）：
  - 修正了无法读取《钻石／珍珠》字库的问题。
  - 修改了控制符的处理方式，兼容前一版本已导出的文本。
- 2.0.0（2020-03-27）：由于本次更新后的工具可能存在一些问题，请详细阅读使用说明。
  - 增加了对第五世代《黑／白》的支持（文本、字库）。
- 3.0.0（2021-06-22）：
  - 修改了第四世代《钻石／珍珠／白金／心金／魂银》的控制符。详请见下。
- 3.1.0（2021-06-27）：
  - 增加了自制的汉化补丁应用工具。详请见下。

## 总体说明
本人所使用的工具由C#编写，按照功能区分为文本处理工具、字库处理工具、汉化补丁应用工具，前两者为命令行程序，无图形化界面，命令行参数可通过`--help`指令查看。后者为图形化界面程序。

文本处理工具为`PokemonCTRText.exe`，目前实现的功能包括：从narc文件导出txt文本、将txt文本导入并创建新的narc文件。

字库处理工具为`PokemonCTRFont.exe`，目前实现的功能仅有：从narc文件根据码表创建新的narc文件。

1.2.0版本之前仅支持第四世代的文件，2.0.0版本加入对第五世代文件的初步支持。

在使用之前，请先从ROM中提取包含了文本和字库的narc文件，以及一份码表。详情见以下部分说明。本人不以任何形式提供完整ROM文件。

汉化补丁应用工具为`PokemonCTRPatch.exe`，目前实现的功能仅有：给ROM打补丁。

本工具为开源软件，源代码发布于[GitHub](https://github.com/Xzonn/PokemonChineseTranslationRevise/)，按照GPL-3.0协议授权。

## 码表
码表文件实际为txt文本文件，每个字符为一行，每行以`\t`分隔符隔开，前为编码（无前缀16进制），后为字符。

### 第四世代
在第四世代中，游戏存储字符的编码与Unicode编码并非一一对应，因此需要使用码表标识字符。对于使用不同码表的游戏，同一字符对应的编码并不相同，同一编码对应的字符也不相同，因此两者间连接交换会出现字符不匹配，即“乱码”。

本人提供了《[钻石／珍珠](https://github.com/Xzonn/PCTRAutoBuild/raw/dist/CharTable/Original_DP.txt)》《[白金](https://github.com/Xzonn/PCTRAutoBuild/raw/dist/CharTable/Original_Pt.txt)》《[心金／魂银](https://github.com/Xzonn/PCTRAutoBuild/raw/dist/CharTable/Original_HGSS.txt)》原始汉化版所用的码表，同时发售的双版本游戏使用了同样的码表和文本，因此合并发布，下同。

《珍珠／钻石》和《白金》的码表信息来自于“PokeSav字符转换器”（文件名：`WordChanger.exe`），由 **@Easy_World** 制作，原始发布地址已不可考。其中，该软件提供的《珍珠／钻石》码表最大编号为`0x1C70`（7280），但由于字库文件限制，实际最大编号为`0x119D`（4509），但这个大小还是比《白金》和《心金／魂银》的字库大出不少，常用字基本已经包括在内。《白金》码表最大编号为`0x0DD9`（3545）。

《心金／魂银》的码表信息为我个人校对而成，最大编号为`0x0E81`（3713）。

由于本人发布的修正补丁使用了同一码表，因此可以互相连接交换。同时本人建议其他使用本工具者也使用本人的码表，以方便连接交换，缺失的字符可以在最后追加并提交Pull Request。[修正补丁码表链接](https://github.com/Xzonn/PCTRAutoBuild/raw/dist/CharTable/Expanded.txt)。

### 第五世代
在第五世代中，游戏存储字符的编码为Unicode，因此在任意版本的游戏中字符与编码都是一一对应的，连接交换不会出现问题。因此，对第五世代来说“码表”仅用于标识字库中需要包含哪些字符。由于一些原因，本工具仅会读取Unicode编码为`0x4E00` ~ `0xE000`之间的字符，其他字符会被直接忽略。

由于不明原因，本人建议第五世代的码表按照Unicode顺序排序，否则可能会出现字符变为“？”的情况。

## narc文件
本工具直接读取和输出的文件即为narc文件。该格式实际是多个文件打包的格式。

narc解包代码参考了“narctool 0.1-p”，由 **@natrium42** 制作，**[@Pipian](https://github.com/pipian)** 修改，原始发布地址已不可考，但我从“[delguoqing/LMDumper](https://github.com/delguoqing/LMDumper/tree/master/tools/narctool-0.1-p)”项目的源代码中发现了该程序的源代码。原始程序为C++，我编写了C#下对打包和解包的实现，未编写压缩相关内容（与本项目无关）。

narc文件需要对原始ROM进行解包，我使用的是“[Tinke 0.9.2](https://github.com/pleonex/tinke)”，由 **[@pleoNeX](https://github.com/pleonex)** 制作并发布于GitHub，开源。该软件为图形化界面，方便操作。

## 文本
### 工具说明
在第四、第五世代中文本均以narc格式保存，路径有所不同：

- 《钻石／珍珠》：`/msgdata/msg.narc`
- 《白金》：`/msgdata/pl_msg.narc`
- 《心金／魂银》：`/a/0/2/7`
- 《黑／白／黑２／白２》：`a/0/0/2`、`a/0/0/3`

文本处理工具为`PokemonCTRText.exe`。第四世代代码参考了“[DS Text Editor](https://github.com/JackHack96/DS-Text-Editor)”，由 **[@JackHack96](https://github.com/JackHack96)** 制作并发布于GitHub，开源，授权协议为GPL-3.0。原始程序为Java，我编写了C#下的实现。第五世代代码参考了“[pptxt](https://projectpokemon.org/home/forums/topic/10583-pptxt-text-editing-tool/)”，由 **[@SCV](https://projectpokemon.org/home/profile/2-scv/)** 制作并发布于Project Pokemon Forums，开源。原始程序为C++，我编写了C#下的实现。

对于第四世代的文本，1.x - 2.x版本中均把控制符`0x25BC`转义为`\r`，`0x25BD`转义为`\f`，但这种转义方法与第五世代有冲突，因此自3.0.0版本开始调换了两个控制符的转义方式。目前，`\r`表示对话移动到下一行，`\f`表示对话翻页。为了兼容之前的版本，使用新版本导出的文本第一行会加入`#3`，表明为新版本导出的文本。

对于第五世代的文本，由于缺乏研究，在某些情况下导入文本可能会出错，原因不明。为了稳定起见，本工具会以原文本占用的最大长度 - 1作为导入文本的长度，长度不足者以`0xFFFF`补齐，长度超过者直接截断并在控制台输出信息。需要注意的是原文本占用的最大长度并非文本长度，而是文本长度 × 2，例如“フシギダネ”占用的长度为10而非5，同时考虑到末尾必须有`0xFFFF`作为控制符，因此允许导入的最大长度为9。后续版本中可能会根据对文件结构的分析解除限制。

### 使用方法
#### 从narc文件导出txt文本
示例：

```
PokemonCTRText.exe -c CharTable/Original_DP.txt -m DP/Original/msgdata/msg.narc -e DP/Original/Messages.txt
```

此操作以`CharTable/Original_DP.txt`为码表，`DP/Original/msgdata/msg.narc`为文本所在的narc文件，`DP/Original/Messages.txt`为导出后保存为的txt文件。

#### 将txt文本导入并创建新的narc文件
示例：

```
PokemonCTRText.exe -c CharTable/Expanded.txt -m DP/Original/msgdata/msg.narc -i DP/Revised/Messages.txt -o DP/Revised/msgdata/msg.narc
```

此操作以`CharTable/Expanded.txt`为码表，`DP/Original/msgdata/msg.narc`为文本所在的narc文件，`DP/Revised/Messages.txt`为需要导入的txt文件，`DP/Revised/msgdata/msg.narc`为导入文本后保存为的narc文件。

如果文本中存在码表未收录的字符，则在导入时程序会有输出的提示。

## 字库
### 工具说明
在第四、第五世代中字库均以narc格式保存，路径有所不同：

- 《钻石／珍珠》：`/graphic/font.narc`
- 《白金》：`/graphic/pl_font.narc`
- 《心金／魂银》：`/a/0/1/6`
- 《黑／白／黑２／白２》：`a/0/2/3`

字库处理工具为`PokemonCTRFont.exe`。代码参考了“[PokeFontDS](https://github.com/TheFearsomeDzeraora/PokeFontDS)”，由 **TheFearsomeDzeraora** 制作并发布于 GitHub，开源；以及“NARCFileReadingDLL”，由 **@evco1** 制作，原始发布地址已不可考，但我从“PokeFontDS”项目的源代码中发现了该程序的反编译源代码。

由于可选的中文点阵字体较少，在比较了多种字体后我选择了Windows自带的中易宋体作为字体。如需更换为其他字体，请自行修改源代码并编译。

使用前请确保已安装以下字体：

- 中易宋体（Simsun）
- MS Gothic
- Zfull-GB（仅第五世代使用）

本人不以任何方式提供上述字体的可安装版本。

本工具先读取日文字库，然后对字库包含的字符数量扩容到所需的大小，最后将中文字符用上述字体重新生成。字号大小为12px（正常大小，中易宋体或MS Gothic）或9px（小号字体，Zfull-GB，仅第五世代使用）。在上述字体中均包含了对应字号下的点阵字库，因此无需抗锯齿即可取得较好的显示效果。

### 使用方法
#### 从narc文件根据码表创建新的narc文件
示例：

```
PokemonCTRFont.exe -c CharTable/Expanded.txt -f DP/Original/graphic/font.narc -o DP/Revised/graphic/font.narc
```

此操作以`CharTable/Expanded.txt`为码表，`DP/Original/graphic/font.narc`为字库所在的narc文件，`DP/Revised/graphic/font.narc`为新字库保存为的narc文件。

### 从narc文件导出字库为png文件（实验性）
本功能为实验性功能，为本人在调试过程中使用，故无法直接调用，如需调用请自行修改源代码并编译。

## 汉化补丁应用工具
为了解决之前很多人提到的“打补丁打不上”“校验值不通过”的问题，我自行编写了汉化补丁应用工具，可以搭配[自动构建项目](https://github.com/Xzonn/PCTRAutoBuild)一起使用。

原理很简单，工具里面内嵌了[ndstool](https://github.com/devkitPro/ndstool)（作者：Rafael Vuijk，Dave Murphy，Alexei Karpenko；按GPL-3.0授权），使用的时候把输入的ROM拆包，然后把补丁里面的文件覆盖掉原来的文件，再打包回去。

本工具为图形化界面，如有使用上的问题请及时反馈。

预计在1.4.1版本发布使用新工具的补丁。