=======
字体表
=======

介绍
=======
本章记录了构成TrueType字体文件的字体表，包括核心TrueType规范的 AAT 扩展。OpenType 特定表格的文档不包括在内，即使是在OSX和iOS系统支持的OpenType字体， 上支持的表格。有关这些表的更多信息，请参阅 `OpenType 规范 <http://www.microsoft.com/typography/otspec/>`__ 。

表1 描述了所有平台上 TrueType 字体文件中使用的数据类型。

目录表必须出现在字体文件的开头，除此之外，其它字体表可以以任何顺序出现。为方便访问本章提供的信息，字体表按照字母顺序依次描述。

数据类型
=======

除了标准的整数类型，TrueType格式还使用如下数据类型

.. list-table:: 表一：sfnt数据类型
    :widths: 40 200
    :header-rows: 1
    :stub-columns: 1

    * - 数据类型
      - 描述
    * - shortFrac
      - 16位有符号小数
    * - Fixed
      - 16.16位有符号定点数
    * - FWord
      - 16位有符号整数
    * - uFWord
      - 16位无符号整数
    * - F2Dot14
      - 16位定点数
    * - longDateTime
      - 1904年1月1日0点为起点的时间戳。64位有符号整数

.. Note::
    一个shortFrac是一个小数部分为14位，整数部分为2位的有符号小数。这表明它可以表示1.999（0x7fff）到-2.0（0x8000）之间的数。如1.0存储为16384 (0x4000)， -1.0存储为-16384 (0xc000)。

TrueType字体文件总览
===================

一个TrueType字体包含一系列彼此相连的表。一个表由一系列数据组成。每个表长度必须是8的倍数，不足的时候要用0填充。

第一个表是字体目录表。这是一个特殊的表，用于访问字体文件中的其它表。紧随目录表之后的就是一系列包含字体数据的表。这些表可以以任意顺序排列。某些表对于字体文件是必须的。其余的根据字体功能需要选用。

这些表都有名称，称之为标签。标签的类型是32位无符号整型（uint32），即标签包含四个字符。标签名称可能包含空格。在下文展示的时候，标签名会用单引号引起来。

任何有效的TrueType字体文件必须包含某些特定表，这些表及其标签在表二列出。

.. list-table:: 表二：必须包含的表
    :widths: 40 200
    :header-rows: 1
    :stub-columns: 1

    * - 标签
      - 说明
    * - 'cmap'
      - 字符到轮廓映射（character to glyph mapping）
    * - 'glyf'
      - 轮廓（glyph）数据
    * - 'head'
      - 字体表头（header）
    * - 'hhea'
      - 水平表头（horizontal header）
    * - 'hmtx'
      - 水平度量（horizontal metrics）
    * - 'loca'
      - 序号到位置映射（index to location）
    * - 'maxp'
      - 最大值档案（maximum profile）
    * - 'name'
      - 名称（naming）
    * - 'post'
      - PostScript脚本（PostScript）
      
      
.. Warning::
    苹果区分“TrueType字体”（由轮廓定义的一种字体）和“sfnt型字体”（由目录表和一系列其它表组成的字体）。

    由于苹果在OSX和IOS系统支持其它类别的sfnt型字体，比如点阵字体和OpenType字体，因此这个区别很重要。一般而言，人们经常把任何sfnt型字体称为“TrueType字体”，但是这种说法并不准确。
    
    
    在表二中列出的字体表仅仅在“TrueType字体”是必须的，其它系列的sfnt型字体可能不包含这些表。比如说，点阵字体不含'hmtx', 'hhea' 和 'head'表。CoreText是苹果自带的字体文本渲染系统，它要求任何sfnt型字体必须包含'cmap'和'name'表。
    
    对字体厂商而言，如果你不确定一个非TrueType的sfnt型字体是否必须包含某个表，安全起见，你最好包含它，或者可以联系苹果公司以获得建议。
    
    对字体用户而言，永远不要假定OSX系统中的某个字体一定包含某个表。字体可能是非TrueType的sfnt型字体，可能缺少某些必要的表。更进一步，字体可能是错误的，未安装的。如果字体文件缺少你需要的某些特定表，提供一个友好的错误信息给使用者。
    

根据特定字体文件的功能，可能有一些可选表。可选表及其标签列在表三。其中'hdmx'表仅用于Mac平台。'OS/2'在Mac平台是 **必须** 的，但在其它平台是可选的，因此出现在可选表中。

苹果高级排印表（Apple Advanced Typography，AAT）用于苹果系统的CoreText水平布局功能。

