======================
字体表（Font Tables）
======================

介绍
=======
本章记录了构成TrueType字体文件的字体表，包括核心TrueType规范的 AAT 扩展。OpenType 字体特有的表不包括在内，即使是在OSX和iOS系统支持的OpenType字体， 上支持的表格。有关这些表的更多信息，请参阅 `OpenType 规范 <http://www.microsoft.com/typography/otspec/>`__ 。

表1 描述了所有平台上 TrueType 字体文件中使用的数据类型。

目录表必须出现在字体文件的开头，除此之外，其它字体表可以以任何顺序出现。为方便访问本章提供的信息，字体表按照字母顺序依次描述。

.. toggle::
    
    This chapter documents the tables that make up a TrueType font file, including AAT extensions to the core TrueType specification. Documentation is not included for OpenType-specific tables, even those supported on OS X and iOS. For further information on those tables, see the OpenType specification.

    Table 1 describes the data types used in TrueType font files on all platforms.

    With the exception of the font directory which must appear first in the font file, the tables that make up a font can appear in any order. For convenience in accessing the information presented in this chapter, tables are described in alphabetical order.

数据类型
========

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

.. toggle::
   
   In addition to standard integer data types, the TrueType font format uses the following:
   
    .. list-table:: 表一：sfnt数据类型
        :widths: 40 200
        :header-rows: 1
        :stub-columns: 1

        * - Data type
          - Description
        * - shortFrac
          - 16-bit signed fraction
        * - Fixed
          - 16.16-bit signed fixed-point number
        * - FWord
          - 16-bit signed integer that describes a quantity in FUnits, the smallest measurable distance in em space.
        * - uFWord
          - 16-bit unsigned integer that describes a quantity in FUnits, the smallest measurable distance in em space.
        * - F2Dot14
          - 16-bit signed fixed number with the low 14 bits representing fraction.
        * - longDateTime
          - The long internal format of a date in seconds since 12:00 midnight, January 1, 1904. It is represented as a signed 64-bit integer.

   NOTE: A shortFrac is an int16_t with a bias of 14. This means it can represent numbers between 1.999 (0x7fff) and -2.0 (0x8000). 1.0 is stored as 16384 (0x4000) and -1.0 is stored as -16384 (0xc000).


TrueType字体文件总览
=====================

一个TrueType字体包含一系列彼此相连的表。一个表由一系列数据组成。每个表长度必须是8的倍数，不足的时候要用0填充。

第一个表是字体目录表。这是一个特殊的表，用于访问字体文件中的其它表。紧随目录表之后的就是一系列包含字体数据的表。这些表可以以任意顺序排列。某些表对于字体文件是必须的。其余的根据字体功能需要选用。

这些表都有名称，称之为标签。标签的类型是32位无符号整型（uint32），即标签包含四个字符。标签名称可能包含空格。在下文展示的时候，标签名会用单引号引起来。

任何有效的TrueType字体文件必须包含某些特定表，这些表及其标签在表二列出。

.. toggle:: 

    A TrueType font file consists of a sequence of concatenated tables. A table is a sequence of words. Each table must be long aligned and padded with zeroes if necessary.

    The first of the tables is the font directory, a special table that facilitates access to the other tables in the font. The directory is followed by a sequence of tables containing the font data. These tables can appear in any order. Certain tables are required for all fonts. Others are optional depending upon the functionality expected of a particular font.

    The tables have names known as tags. Tags have the type uint32. Currently defined tag names consist of four characters. Tag names with less than four characters have trailing spaces. When tag names are shown in text they are enclosed in straight quotes.

    Tables that are required must appear in any valid TrueType font file. The required tables and their tag names are shown in Table 2.

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

.. list-table:: Table 2: The required tables
    :widths: 40 200
    :header-rows: 1
    :stub-columns: 1
    :class: toggle

    * - tag
      - Table
    * - 'cmap'
      - character to glyph mapping
    * - 'glyf'
      - glyph
    * - 'head'
      - header
    * - 'hhea'
      - horizontal header
    * - 'hmtx'
      - horizontal metrics
    * - 'loca'
      - index to location
    * - 'maxp'
      - maximum profile
    * - 'name'
      - naming
    * - 'post'
      - PostScript


.. Warning::
    苹果区分“TrueType字体”（由轮廓定义的一种字体）和“sfnt型字体”（由目录表和一系列其它表组成的字体）。

    由于苹果在OSX和IOS系统支持其它类别的sfnt型字体，比如点阵字体和OpenType字体，因此这个区别很重要。一般而言，人们经常把任何sfnt型字体称为“TrueType字体”，但是这种说法并不准确。
    
    
    在表二中列出的字体表仅仅在“TrueType字体”是必须的，其它系列的sfnt型字体可能不包含这些表。比如说，点阵字体不含'hmtx', 'hhea' 和 'head'表。CoreText是苹果自带的字体文本渲染系统，它要求任何sfnt型字体必须包含'cmap'和'name'表。
    
    对字体厂商而言，如果你不确定一个非TrueType的sfnt型字体是否必须包含某个表，安全起见，你最好包含它，或者可以联系苹果公司以获得建议。
    
    对字体用户而言，永远不要假定OSX系统中的某个字体一定包含某个表。字体可能是非TrueType的sfnt型字体，可能缺少某些必要的表。更进一步，字体可能是错误的，未安装的。如果字体文件缺少你需要的某些特定表，提供一个友好的错误信息给使用者。

.. toggle::

    .. Warning::
        
        Apple makes a distinction between a "TrueType font" (which refers to a particular font outline definition technology) and an "sfnt-housed font," which refers to any font which uses the same packaging format as a TrueType font: that is, it uses the same directory structure and the same table format and meaning for any tables present.

        This is an important distinction, because Apple supports other varieties of sfnt-housed font on OS X and iOS, most notably bitmap-only fonts and OpenType fonts. Informally, people often refer to any sfnt-housed font as a "TrueType font," but this is strictly speaking inaccurate.

        The "required" tables listed in Table 2 are only required for TrueType fonts. Other varieites of sfnt-housed font may not have them. For example, bitmap-only sfnt-housed fonts do not have an 'hmtx', 'hhea' or 'head' table. CoreText, Apple's rendering system for Unicode-encoded text, does require that any sfnt-housed font have a 'cmap' and 'name' table.

        For font vendors: If you are unsure whether a particular table should be included for your non-TrueType sfnt-housed font, it is generally safe to include it, or you may contact Apple for advice.

        For font users: Never assume that any particular table is present in a font on OS X. Fonts may be non-TrueType sfnt-housed fonts and lack some of TrueType's required tables. Moreover, fonts may be ill-formed and yet installed. Provide graceful error handling if a font you are using lacks a table you require.

根据特定字体文件的功能，可能有一些可选表。可选表及其标签列在表三。其中'hdmx'表仅用于Mac平台。'OS/2'在Mac平台是 **必须** 的，但在其它平台是可选的，因此出现在可选表中。

苹果高级排印表（Apple Advanced Typography，AAT）用于苹果系统的CoreText水平布局功能。

其他的表可能用于支持其它平台，比如OpenType，或者为了将来进一步扩展。表的标签必须注册，可以联系苹果开发者技术支持以获取注册方式和流程。所有字符为小写字母的标签保留为苹果所用。

.. toggle::

    Optional tables may be needed depending upon the functionality expected of a given font file'. The optional tables and their tag names are listed in Table 3. The 'hdmx' table is used on Macintosh platforms only. The 'OS/2' table is required for fonts that are to be used on that platform but appears in the optional table because it is not required for all TrueType font files.

    Apple Advanced Typography (AAT) Tables are used with the Line Layout capability of Apple's CoreText.

    Additional tables may be defined to support other platforms, such as OpenType, or to provide for future expansion. Tags for these tables must be registered. Contact Apple Developer Technical Support for information regarding the registration process. Tag names consisting of all lower case letters are reserved for Apple's use.

.. list-table:: 表三：可选表
    :widths: 40 200
    :header-rows: 1
    :stub-columns: 1
    
    * - 标签
      - 说明
    * - 'cvt '
      - 控制值（control value）
    * - 'fpgm'
      - 字体编程（font program）
    * - 'hdmx'
      - 水平设备度量（horizontal device metrics）
    * - 'kern'
      - 抗锯齿（kerning）
    * - 'OS/2'
      - 用于苹果系统（OS/2）
    * - 'prep'
      - 控制值程序（control value program）

.. toggle::

    .. list-table:: Table 3: The optional tables
        :widths: 40 200
        :header-rows: 1
        :stub-columns: 1
        
        * - Tag
          - Table
        * - 'cvt '
          - control value
        * - 'fpgm'
          - font program
        * - 'hdmx'
          - horizontal device metrics
        * - 'kern'
          - kerning
        * - 'OS/2'
          - OS/2
        * - 'prep'
          - control value program

字体目录
=========

字体目录是第一个表，用于访问字体文件的其它内容。目录表提供了必需的数据，用于访问其它表。目录包含两部分：偏移字表和表目录。偏移字表记录了字体中表的数目，并提供了偏移信息用于快速访问表目录。表目录包含一系列条目，每一条对应字体的一个表。

.. toggle::

    The font directory, the first of the tables, is a guide to the contents of the font file. It provides the information required to access the data in the other tables. The directory consists of two parts: the offset subtable and the table directory. The offset subtable records the number of tables in the font and provides offset information enabling quick access to the directory tables. The table directory consists of a sequence of entries, one for each table in the font.

偏移字表
-------------------

偏移子表记录在表四，以一个标量记录字体文件的类型。紧随其后是'sfnt' 中表的数量，表目录本身和任何子表不包括在此计数中。 searchRange、entrySelector 和 rangeShift 用于对表目录进行二分搜索。不过，除非字体有大量表，否则顺序搜索的方式已经足够快了。

如果需要更快搜索表，二分搜索是最简单的，搜索的次数是表数目的对数。这使得可以通过移位将要搜索的项目数量减少一半。剩余的偏移子表条目应设置如下：

searchRange 是小于等于表中项目数的 2 的最大幂，即可以轻松搜索到的最大项目数。rangeShift 是项目数减去 searchRange；即，如果您只查看 searchRange 项目，则不会查看的项目数。在搜索循环开始之前，将目标项目与编号为 rangeShift 的项目进行比较。如果目标项小于 rangeShift，则从表的开头开始搜索。如果它更大，则从编号为 rangeShift 的项目开始搜索。

entrySelector 是 log2(searchRange)。它告诉搜索循环需要多少次迭代。 （即将范围减半的次数）请注意，searchRange、entrySelector 和 rangeShift 都乘以 16，表示目录条目的大小。

.. toggle::

    The offset subtable, documented in Table 4, begins with the scaler type of the font. The number of tagged tables in the 'sfnt' follows.The table directory itself and any subtables are not included in this count. The entries for searchRange, entrySelector and rangeShift are used to facilitate quick binary searches of the table directory that follows. Unless a font has a large number of tables, a sequential search will be fast enough.

    If a faster search is necessary, a binary search is most easily done on a number of entries that is a power of two. This makes it possible to cut the number of items to search in half by shifting. The remaining offset subtable entries should be set as follows:

    searchRange is the largest power of two less than or equal to the number of items in the table, i.e. the largest number of items that can be easily searched.
    rangeShift is the number of items minus searchRange; i.e. the number of items that will not get looked at if you only look at searchRange items.
    Before the search loop starts, compare the target item to the item with number rangeShift. If the target item is less than rangeShift, search from the beginning of the table. If it is greater, search starting at the item with number rangeShift.

    entrySelector is log2(searchRange). It tells how many iterations of the search loop are needed. (i.e. how many times to cut the range in half)
    Note that the searchRange, the entrySelector and the rangeShift are all multiplied by 16 which represents the size of a directory entry.

.. csv-table:: 表四: 偏移字表
   :header: 类型,名称,说明
   :widths: 15, 10, 30

    uint32,scaler type,用于光栅化字体的标记，详见下方说明
    uint16,numTables,表的数目
    uint16,searchRange,(log2 numTables下取整)*16
    uint16,entrySelector,log2(log2 numTables下取整)下取整
    uint16,rangeShift,numTables*16-searchRange

.. csv-table:: Table 4 : The offset subtable
   :header: Type,	Name,	Description
   :widths: 15, 10, 30
   :class: toggle

    uint32,scaler type,A tag to indicate the OFA scaler to be used to rasterize this font; see the note on the scaler type below for more information.
    uint16,numTables,number of tables
    uint16,searchRange,(maximum power of 2 <= numTables)*16
    uint16,entrySelector,log2(maximum power of 2 <= numTables)
    uint16,rangeShift,numTables*16-searchRange

类型标量
++++++++++++++++++

在OSX和IOS系统中，类型标量用于决定这个字体使用哪个标量。也就是说，它决定了如何解析字体轮廓表中的数据。除了最基本的TrueType字体结构，不同的类型标量代表不同的字体格式；字体目录中偏移字表的类型标量用于指示这个字体应当使用什么样的标量。（有类似结构的非TrueType字体称之为sfnt型字体）

在OSX和IOS系统中，可以识别值'true' (0x74727565) 和 0x00010000。值'typ1'被识别为一种老式的用PostScript描绘的sfnt型字体。值'OTTO' (0x4F54544F)表明这是一个使用PostScript描绘的OpenType字体（也就是说'glyf'表替换为'CFF '表）。其它的值当前还不支持。

如果TrueType字体专门为OSX和IOS系统制作，那么建议把类型标量的值设置为'true' (0x74727565)。如果同时用于苹果和Windows系统，那么必须设置为0x00010000。

.. toggle::
    
    The scaler type is used by OS X and iOS to determine which scaler to use for this font, that is, to determine how to extract glyph data from the font. Different font scalers wrap different font formats within the basic structure of a TrueType font; the scaler type in the offset subtable of the font's directory is used to indicate which scaler should be used with a particular font. (Non-TrueType fonts housed within the same structure as a TrueType font are referred to as "sfnt-housed fonts.")

    The values 'true' (0x74727565) and 0x00010000 are recognized by OS X and iOS as referring to TrueType fonts. The value 'typ1' (0x74797031) is recognized as referring to the old style of PostScript font housed in a sfnt wrapper. The value 'OTTO' (0x4F54544F) indicates an OpenType font with PostScript outlines (that is, a 'CFF ' table instead of a 'glyf' table). Other values are not currently supported.

    Fonts with TrueType outlines produced for OS X or iOS only are encouraged to use 'true' (0x74727565) for the scaler type value. Fonts for Windows or Adobe products must use 0x00010000.

表目录
---------------------------

表目录紧随偏移字表之后。表目录中的条目必须按照标签名升序排列。字体中每个表必须在表目录中有对应的条目。表五展示了表目录的结构。



.. csv-table:: 表五：表目录
   :header: 类型,名称,说明
   :widths: 15, 10, 30

    uint32,tag,4字节标签
    uint32,checkSum,这个表的校验和
    uint32,offset,表在字体的偏移位置
    uint32,length,表的实际长度（不含填充位）
    
表目录包含校验和。校验和用于验证对应表数据的准确性。表校验和是一个表所有数据按照无符号长整型（uint64）相加之和。下面的C代码可以用于计算给定表的校验和。


.. toggle::

    The table directory follows the offset subtable. Entries in the table directory must be sorted in ascending order by tag. Each table in the font file must have its own table directory entry. Table 5 documents the structure of the table directory.

    .. csv-table:: Table 5: The table directory
       :header: Type, Name, Description 
       :widths: 15, 10, 30

        uint32,tag,4-byte identifier 
        uint32,checkSum,checksum for this table
        uint32,offset,offset from beginning of sfnt
        uint32,length,length of this table in byte (actual length not padded length)
    
    The table directory includes checkSum, a number which can be used to verify the identity of and the integrity of the data in its associated tagged table. Table checksums are the unsigned sum of the longs in a table. The following C function can be used to determine the checksum of a given table:

.. code-block:: C

    uint32 CalcTableChecksum(uint32 *table, uint32 numberOfBytesInTable)
    {
        uint32 sum = 0;
        uint32 nLongs = (numberOfBytesInTable + 3) / 4;
        while (nLongs-- > 0)
            sum += *table++;
        return sum;
    }

'head'表包含整个字体的校验和，因此要计算'head'表的校验和，按照以下步骤：

- 设置字体校验和为0
- 计算所有表的校验和，包括'head'表，并把计算结果填入表目录
- 计算整个字体的校验和
- 用这个值减去B1B0AFBA
- 把结果存储在字体的校验和位置

对包含字体校验和的'head'表校验和目前是不正确的，但这个不是问题，不要更改它。要验证'head'表的完整性，应当计算表的校验和，但是不包含checkSumAdjustment值，并与表目录的对应值比较。

表目录也包括对应表的偏移量和长度。

.. toggle::

    To calculate the checkSum for the 'head' table which itself includes the checkSumAdjustment entry for the entire font, do the following:

    - Set the checkSumAdjustment to 0.
    - Calculate the checksum for all the tables including the 'head' table and enter that value into the table directory.
    - Calculate the checksum for the entire font.
    - Subtract that value from the hex value B1B0AFBA.
    - Store the result in checkSumAdjustment.
    
    The checkSum for the 'head table which includes the checkSumAdjustment entry for the entire font is now incorrect. That is not a problem. Do not change it. An application attempting to verify that the 'head' table has not changed should calculate the checkSum for that table by not including the checkSumAdjustment value, and compare the result with the entry in the table directory.

    The table directory also includes the offset of the associated tagged table from the beginning of the font file and the length of that table.                                                                                                 |
