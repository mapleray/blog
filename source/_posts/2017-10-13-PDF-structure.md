---
title: PDF 文档结构
date: 2017-10-13 13:41:50
permalink: pdf-structure
tags:
  - PDF
categories:
  - Program
---


## 基本要素

### 数据类型

PDF 文件基本元素是 PDF 对象(PDF Object), PDF 对象包括直接对象(Direct Object)和间接对象(Indirect Object)。直接对象如下八种类型；间接对象，又叫 `labelled object`, 嵌套在关键词 `n 0 obj` 和 `endobj` 之间， 是用一种表示来标识一个 PDF 对象，通过标识来让别的 PDF 对象引用，这个标识叫做间接对象的 ID.

<!-- more -->

#### 直接对象类型

1. Boolean value(布尔)

    布尔类型，值只能是 `true` 和 `false`。

2. Integer and Real number(数值)

    数值类型，包括整数和实数，与普通编程语言中的数值类型大体相同。

3. String(字符串)

    字符串类型，包括包含在圆括号 `( )` 内的文字字符串(`literal string`)和包含在单尖括号 `< >` 内的十六进制字符串(`hexadecimal string`)两种。

    例：
        1. (Hello World)
        2. <9ADCF1>

4. Name(名字?)

    名字类型，用字符组成的字符串，用 `/` 作为前导符号，在 PDF 文件中具有唯一性，相同的名字表示相同的对象(`the same sequence of character denotes the same object`)。常见用在 `Dictionary` 里面作 `Key`，用来表示对象名称。

    例：
        /Page
        /Kid

5. Array(数组)

    数组类型，存在于方括号 `[ ]` 内，元素可以是除 `Stream` 外的所有类型。PDF 中数组只支持一维数组。

    例：
        [/Page false 17 (hello)]  该数组包含了4种类型元素

6. Dictionary(字典)

    字典类型，包含在双尖括号 `<< >>` 内，每两个元素为一对，第一个为 `key`, 第二个为 `value`， `key` 只能是 `Name` 类型，`value` 可以是任意类型，即可以嵌套为 `Dictionary`。

    例：
        <</Page 1 0 obj
        /Filter /FlateDecode
        /Name (Hello)>>

7. Stream(流对象)

    流对象，是用字节表示的序列，长度理论上没限制。包含在 `stream` 和 `endstream` 之间。以 `CRLF` 或 `LF` 结尾，不能单独以 `CR` 结尾。`dicionary` 里的内容用来描述该 `stream` 的相关信息。

        dictionary
        stream
            ......
        endstream

8. Null object(空对象)

    空对象类型，用关键词 `null` 表示。

#### 间接对象类型

使用 unique object identifier 来表示，方便其他对象引用。结构如下：

        12 0 obj
        ........
        endobj

第一行第一个 `12` 规定为 `positive integer`， 表示对象 ID; 第二个 `0` 表示生成号(`generation number`)，通常为0；第三个为固定 `obj` 表示，以最后一行 `endobj` 表示结束。中间 `......` 表示内容。其他地方引用该对象时，使用如下格式，其中 `R` 为关键字:

        12 0 R


### 文件结构

文件基本结构如下，文件结构的详细内容分析将在实例分析中说明。

```
            +--------------------------+
            | +----------------------+ |
            | |        Header        | |  <-----文件头，表示版本.%PDF-1.M
            | |                      | |
            | +----------------------+ |
            | |                      | |
            | |         Body         | |  <-----文件体，由一系列PDF对象组成
            | |                      | |
            | |                      | |
            | |                      | |
            | |                      | |
            | |                      | |
            | +----------------------+ |
            | |    Cross-reference   | |  <-----交叉引用表，包含指向所有间接
            | |         table        | |        对象的文件位置索引的列表
            | |        (xref)        | |
            | +----------------------+ |
            | |        Trailer       | |  <-----包含文件的根节点信息和
            | |                      | |        文件解析的起点信息
            | +----------------------+ |
            +--------------------------+

```

#### 交叉引用表

交叉引用表的结构如下：

```
xref
i j
nnnnnnnnnn ggggg N eol
nnnnnnnnnn ggggg N eol
......
nnnnnnnnnn ggggg N eol
```

第一行 `xref` 表示交叉引用表开始；第二行 `i j` 表示下面部分引用的对象从 `i` 开始，共有 `j` 个对象；第三行开始相同的结构，每行20字节，包括换行符。 `nnnnnnnnnn` 10位，字节偏移地址，表示从 **文件开始（beginning of the file)** 到 **该对象开始(beginning of the object)** 的偏移； `ggggg` 5位，生成号； `N` 可以为 `n` 或者 `f`，其中 `n` 表示该对象在使用，`f` 表示该对象为free状态，未被使用。

## 处理流程

```
            +--------------------------+
            |       读取 PDF 文件        |
            |                          |
            +------------+-------------+
                         |
            +------------v-------------+
            |   从文件末尾开始解析得到     |
            |   索引和文件对象起始位置     |
            +------------+-------------+
                         |
            +------------v-------------+
            |      得到PDF基本信息以及    |
            |        PDF的根目录对象     |
            +------------+-------------+
                         |
            +------------v-------------+
            |    通过根目录对象得到PDF的  |
            |   所有大纲和页面的对象集合   |
            +------------+-------------+
                         |
            +------------v-------------+
            |                          |
            |   当用户转到第几页时，解析   |
            |   该PDF页面上的所有对象     |
            |   包括文字，图片，流媒体等   |
            |                          |
            |                          |
            +------------+-------------+
                         |
            +------------v-------------+
            | 将得到的文件流通过渲染引擎，  |
            |     生成特定文件显示出来     |
            +--------------------------+
```

## 实例分析

```
%PDF-1.6            # 文件头，表示该文档符合 PDF 1.6 规范 ， % 表示注释
%鲣                # (binary data)二进制， 主要用来表示文件内容是 text 还是 binary
12 0 obj            # object 对象， 12是顺序号，0是生成号， obj 为关键词
<</Filter /FlateDecode     # 过滤器类别，处理stream 和 endstream 之间的数据时候需要用到的, FlateDecode表示使用zip算法
/Length 1732>>      # 表示 stream 和 endstream 之间数据的长度
stream              # stream的内容部分
......
endstream
endobj              # 标识该对象结束

1 0 obj
<</Contents 12 0 R
/Parent 2 0 R
/MediaBox [0 0 595 842]
/Resources 7 0 R
/Rotate 0
/Type /Page>>
endobj
2 0 obj
<</Kids [1 0 R] /Type/Pages /Count 1>>
endobj
......
9 0 obj
<</Fields [11 0 R] /XFA [5 0 R] /DA (/Helv 0 Tf 0 g )>>
endobj
11 0 obj
<</T (gapejess[0]) /Kids [4 0 R]>>
endobj
13 0 obj
<</AcroForm 9 0 R /Lang (en-us) /Pages 2 0 R /Type/Catalog>>
endobj
5 0 obj
<</Length 3602 /Filter/FlateDecode>>
stream
......
endstream
endobj

xref                    # 标识交叉引用表开始
0 14                    # 说明下面对象编号是从0开始，总共有14个对象， 从 0 到 13
0000000000 65535 f      # 第0个对象，规定生成号为65535，f 表示 free entry，对象不存在或者删除
0000003079 00000 n      # 第1个对象，偏移地址为3079，生成号为0表示未被修改过, n 表示 in use
0000003191 00000 n
0000003245 00000 n
0000003695 00000 n
0000003955 00000 n
0000003515 00000 n
0000003347 00000 n
0000003446 00000 n
0000003756 00000 n
0000001823 00000 n
0000003827 00000 n
0000000017 00000 n
0000003878 00000 n


trailer                 # 标识文件尾trailer对象开始
<</Root 13 0 R          # 表明根对象的对象号为13，即交叉表中的最后一个对象
/ID [<4E76CDCEDB1E2EC4AC47475DB4EE376E> <C8B1AEBC2C6615E39860F1C150A2847C>]
/Size 14                # 表明PDF文件的对象数目
/Info 8 0 R>>

startxref
7630       # 交叉引用表的偏移地址，相对于文件开始
%%EOF      # 标识文件结束
```

## 其他

### 工具:

- [peepdf](http://eternal-todo.com/tools/peepdf-pdf-analysis-tool)
- [pdfium](pdfium.googlesource.com/pdfium/)

### 参考:

- [Adobe PDF Reference](https://www.adobe.com/cn/devnet/pdf/pdf_reference.html)
- [一个简单PDF文件的结构分析](http://blog.csdn.net/pdfMaker/article/details/573990)
- [PDF文件格式的一些研究心得](https://www.cnblogs.com/Ironsoft/archive/2006/01/05/311467.html)
- [C# Parsing 类实现的 PDF 文件分析器](https://www.oschina.net/translate/pdf-file-analyzer-with-csharp-parsing-classes-vers)
- [PDF文件格式分析](http://blog.csdn.net/wzyzzu/article/details/50460423)
- [PDF源文件浅析](http://superleo.iteye.com/blog/283848)
