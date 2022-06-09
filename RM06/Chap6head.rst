``'head'`` 表
======================

``'head'`` 表包含有关字体的全局信息。这个表记录字体版本号、创建以及修改时间、修订号以及用于整个字体的基本排版数据。它包含字体的定界框，字体字形最可能书写的方向，以及字形在定界框中的占位信息。校验和用于验证字体数据的完整性。它也可以用于区分相似的字体。

标识给出了关于字体的全局信息。如果第0个比特位设置为1，那么这个字体的水平标线在y=0（x轴）的位置。如果第1个比特位设置为1，那么最左边的置1的比特位可能在竖轴（y轴）的左边。如果第2个比特位设置为1，指令集可能使用点的大小代替每个em的像素。也就是说，缩放12点的屏幕字体以得到等效的打印机字体可能会得到得到不同的结果。第3个比特位设置为1导致缩放整数倍而非小数倍。第4个比特位置1，则允许字体改变设备相关的宽度。

.. toggle::

   The ``'head'`` table contains global information about the font. It
   records such facts as the font version number, the creation and
   modification dates, revision number and basic typographic data that
   applies to the font as a whole. This includes a specification of the
   font bounding box, the direction in which the font's glyphs are most
   likely to be written and other information about the placement of
   glyphs in the em square. The checksum is used to verify the integrity
   of the data in the font. It can also be used to distinguish between
   two similar fonts.

   The flags give global information about the font. If bit 0 is set to
   one, the baselines for the font is at y= 0 (that is, the x-axis). If
   bit 1 is set to one, the x-position of the leftmost black bit is
   assumed to be the left side bearing. If bit 2 is set to one,
   instructions may use point size explicitly in place of pixels per em.
   This means that scaling a 12 point screen font to obtain the
   equivalent printer font may not produce the identical result as
   requesting a 12 point printer font. Setting bit 3 causes the use of
   integer scaling instead of fractional scaling. Setting bit 4 allows
   fonts to alter device dependent widths.

xMin, yMin, xMax 和 yMax 这四个值必须通过读取字体中所有字形轮廓数据来得到。与此同时，这四个数据组成一个边界框，能把所有字形囊括其中。

这个表也定义了每个em的点数。这个值必须是2的方幂，范围从64到16384。

fontDirectionHint提供了有关字形最有可能的方向信息。这个值是0表明是混合方向字体；这个值是1表明字形只能从左到右；值为-1则字形只能从右到左。值为±2表明字体包含某些中性字形，也就是说字形没有严格的方向。

.. toggle::

   The values xMin, yMin, xMax and yMax must be computed by looking at
   the outline data for the glyphs in the font. Together they specify a
   rectangle that constitute a bounding box for all of the
   (ungridfitted) glyphs in the font.

   The number of units per em for the font is set in this table. This
   value should be a power of 2. Its range is from 64 through 16384.

   This value lowestRecPPEM is the smallest readable size in pixels per
   em for this font.

   The fontDirectionHint provides information about the way in which the
   glyphs in this font are likely to be set. A value of 0 indicates a
   mixed directional font; a value of one indicates only left to right
   glyphs, -1 only right to left glyphs. The values 2 and -2 refer to
   fonts which contain some neutral glyphs, that is glyphs without a
   strong directionality.

一个中性字符没有严格的方向（并非是说这个字符宽度为0）。空格和标点是中性字符的例子。非中性字符有固有的方向。比如说罗马字母（从左到右）和阿拉伯字母（从右到左）有方向。在一个罗马字体文件中，如果包含空格和标点，那么字体的方向提示应设置为2。

indexToLocFormat的值指示了在表 ``'loca'`` 中所使用的的偏移格式


.. toggle::

   A neutral character has no inherent directionality. (It is not a
   character with zero width). Spaces and punctuation are examples of
   neutral characters. Non-neutral characters are those with inherent
   directionality. For example Roman letters (left-to-right) and Arabic
   letters (right-to-left) have directionality. In a Roman font where
   spaces and punctuation are present, the font direction hint should be
   set to 2.

   The indexToLocFormat value indicates the type of offset format used
   in the index to loc (``'loca'``) table.
   
**表二十二** : ``'head'`` 表

+-----------------------+-----------------------+-----------------------+
|    类型               |    名称               |    说明               |
+=======================+=======================+=======================+
| Fixed                 | 版本                  | 版本1.0应设置为       |
|                       |                       | 0x00010000            |
+-----------------------+-----------------------+-----------------------+
| Fixed                 | 字体版本              | 由字体制造商设定      |
+-----------------------+-----------------------+-----------------------+
| uint32                |校验和调整             | 计算步骤: 首先把它设置|
|                       |                       | 为0, 计算 ``'head'``  |
|                       |                       | 表的校验和，并把它填入|
|                       |                       | 表目录，把整个字体按照|
|                       |                       | uint32_t求和，然后把  |
|                       |                       | ``0xB1B0AFBA - 和``   |
|                       |                       | 存入这个位置。        |
|                       |                       | （``'head'`` 表的校验 |
|                       |                       | 和是不正确的，但是不需|
|                       |                       | 要修改它）            |
+-----------------------+-----------------------+-----------------------+
| uint32                | 魔幻数                | 设置为 0x5F0F3CF5     |
+-----------------------+-----------------------+-----------------------+
| uint16                | 标识                  | bit 0 - y value of 0  |
|                       |                       | specifies baseline    |
|                       |                       | bit 1 - x position of |
|                       |                       | left most black bit   |
|                       |                       | is LSB                |
|                       |                       | bit 2 - scaled point  |
|                       |                       | size and actual point |
|                       |                       | size will differ      |
|                       |                       | (i.e. 24 point glyph  |
|                       |                       | differs from 12 point |
|                       |                       | glyph scaled by       |
|                       |                       | factor of 2)          |
|                       |                       | bit 3 - use integer   |
|                       |                       | scaling instead of    |
|                       |                       | fractional            |
|                       |                       | bit 4 - (used by the  |
|                       |                       | Microsoft             |
|                       |                       | implementation of the |
|                       |                       | TrueType scaler)      |
|                       |                       | bit 5 - This bit      |
|                       |                       | should be set in      |
|                       |                       | fonts that are        |
|                       |                       | intended to e laid    |
|                       |                       | out vertically, and   |
|                       |                       | in which the glyphs   |
|                       |                       | have been drawn such  |
|                       |                       | that an x-coordinate  |
|                       |                       | of 0 corresponds to   |
|                       |                       | the desired vertical  |
|                       |                       | baseline.             |
|                       |                       | bit 6 - This bit must |
|                       |                       | be set to zero.       |
|                       |                       | bit 7 - This bit      |
|                       |                       | should be set if the  |
|                       |                       | font requires layout  |
|                       |                       | for correct           |
|                       |                       | linguistic rendering  |
|                       |                       | (e.g. Arabic fonts).  |
|                       |                       | bit 8 - This bit      |
|                       |                       | should be set for an  |
|                       |                       | AAT font which has    |
|                       |                       | one or more           |
|                       |                       | metamorphosis effects |
|                       |                       | designated as         |
|                       |                       | happening by default. |
|                       |                       | bit 9 - This bit      |
|                       |                       | should be set if the  |
|                       |                       | font contains any     |
|                       |                       | strong right-to-left  |
|                       |                       | glyphs.               |
|                       |                       | bit 10 - This bit     |
|                       |                       | should be set if the  |
|                       |                       | font contains         |
|                       |                       | Indic-style           |
|                       |                       | rearrangement         |
|                       |                       | effects.              |
|                       |                       | bits 11-13 - Defined  |
|                       |                       | by                    |
|                       |                       | `Adobe                |
|                       |                       | <http://partners.adob |
|                       |                       | e.com/asn/developer/o |
|                       |                       | pentype/head.htm>`__. |
|                       |                       | bit 14 - This bit     |
|                       |                       | should be set if the  |
|                       |                       | glyphs in the font    |
|                       |                       | are simply generic    |
|                       |                       | symbols for code      |
|                       |                       | point ranges, such as |
|                       |                       | for a last resort     |
|                       |                       | font.                 |
+-----------------------+-----------------------+-----------------------+
| uint16                | 每个em的点数          |  64 到  16384         |
+-----------------------+-----------------------+-----------------------+
| longDateTime          | 创建时间              | 国际时间              |
+-----------------------+-----------------------+-----------------------+
| longDateTime          | 修改时间              | 国际时间              |
+-----------------------+-----------------------+-----------------------+
| FWord                 | xMin                  | 所有字形定界框        |
+-----------------------+-----------------------+-----------------------+
| FWord                 | yMin                  | 所有字形定界框        |
+-----------------------+-----------------------+-----------------------+
| FWord                 | xMax                  | 所有字形定界框        |
+-----------------------+-----------------------+-----------------------+
| FWord                 | yMax                  | 所有字形定界框        |
+-----------------------+-----------------------+-----------------------+
| uint16                | 字体样式              | 第0位 加粗            |
|                       |                       | 第1位 倾斜            |
|                       |                       | 第2位 下划线          |
|                       |                       | 第3位 外框            |
|                       |                       | 第4位 阴影            |
|                       |                       | 第5位 窄间距          |
|                       |                       | 第6位 扩展            |
+-----------------------+-----------------------+-----------------------+
| uint16                | 最小可读像素          | 最小可读像素          |
+-----------------------+-----------------------+-----------------------+
| int16                 | 字体方向提示          | 0 混合方向字形        |
|                       |                       | 1 只能从左到右        |
|                       |                       | 2 从左到右且含中性字形|
|                       |                       | -1 只能从右到左       |
|                       |                       | -2从右到左且含中性字形|
+-----------------------+-----------------------+-----------------------+
| int16                 | indexToLocFormat      | 0 偏移短整型（uint16）|
|                       |                       | 1 偏移长整型（uint32）|
+-----------------------+-----------------------+-----------------------+
| int16                 | 字形数据格式          | 当前格式设置为0       |
+-----------------------+-----------------------+-----------------------+


.. toggle::

   **Table 22** : ``'head'`` table

   +-----------------------+-----------------------+-----------------------+
   |    Type               |    Name               |    Description        |
   +=======================+=======================+=======================+
   | Fixed                 | version               | 0x00010000 if         |
   |                       |                       | (version 1.0)         |
   +-----------------------+-----------------------+-----------------------+
   | Fixed                 | fontRevision          | set by font           |
   |                       |                       | manufacturer          |
   +-----------------------+-----------------------+-----------------------+
   | uint32                | checkSumAdjustment    | To compute: set it to |
   |                       |                       | 0, calculate the      |
   |                       |                       | checksum for the      |
   |                       |                       | ``'head'`` table and  |
   |                       |                       | put it in the table   |
   |                       |                       | directory, sum the    |
   |                       |                       | entire font as a      |
   |                       |                       | uint32_t, then store  |
   |                       |                       | ``0xB1B0AFBA - sum``. |
   |                       |                       | (The checksum for the |
   |                       |                       | ``'head'`` table will |
   |                       |                       | be wrong as a result. |
   |                       |                       | That is OK; do not    |
   |                       |                       | reset it.)            |
   +-----------------------+-----------------------+-----------------------+
   | uint32                | magicNumber           | set to 0x5F0F3CF5     |
   +-----------------------+-----------------------+-----------------------+
   | uint16                | flags                 | bit 0 - y value of 0  |
   |                       |                       | specifies baseline    |
   |                       |                       | bit 1 - x position of |
   |                       |                       | left most black bit   |
   |                       |                       | is LSB                |
   |                       |                       | bit 2 - scaled point  |
   |                       |                       | size and actual point |
   |                       |                       | size will differ      |
   |                       |                       | (i.e. 24 point glyph  |
   |                       |                       | differs from 12 point |
   |                       |                       | glyph scaled by       |
   |                       |                       | factor of 2)          |
   |                       |                       | bit 3 - use integer   |
   |                       |                       | scaling instead of    |
   |                       |                       | fractional            |
   |                       |                       | bit 4 - (used by the  |
   |                       |                       | Microsoft             |
   |                       |                       | implementation of the |
   |                       |                       | TrueType scaler)      |
   |                       |                       | bit 5 - This bit      |
   |                       |                       | should be set in      |
   |                       |                       | fonts that are        |
   |                       |                       | intended to e laid    |
   |                       |                       | out vertically, and   |
   |                       |                       | in which the glyphs   |
   |                       |                       | have been drawn such  |
   |                       |                       | that an x-coordinate  |
   |                       |                       | of 0 corresponds to   |
   |                       |                       | the desired vertical  |
   |                       |                       | baseline.             |
   |                       |                       | bit 6 - This bit must |
   |                       |                       | be set to zero.       |
   |                       |                       | bit 7 - This bit      |
   |                       |                       | should be set if the  |
   |                       |                       | font requires layout  |
   |                       |                       | for correct           |
   |                       |                       | linguistic rendering  |
   |                       |                       | (e.g. Arabic fonts).  |
   |                       |                       | bit 8 - This bit      |
   |                       |                       | should be set for an  |
   |                       |                       | AAT font which has    |
   |                       |                       | one or more           |
   |                       |                       | metamorphosis effects |
   |                       |                       | designated as         |
   |                       |                       | happening by default. |
   |                       |                       | bit 9 - This bit      |
   |                       |                       | should be set if the  |
   |                       |                       | font contains any     |
   |                       |                       | strong right-to-left  |
   |                       |                       | glyphs.               |
   |                       |                       | bit 10 - This bit     |
   |                       |                       | should be set if the  |
   |                       |                       | font contains         |
   |                       |                       | Indic-style           |
   |                       |                       | rearrangement         |
   |                       |                       | effects.              |
   |                       |                       | bits 11-13 - Defined  |
   |                       |                       | by                    |
   |                       |                       | `Adobe                |
   |                       |                       | <http://partners.adob |
   |                       |                       | e.com/asn/developer/o |
   |                       |                       | pentype/head.htm>`__. |
   |                       |                       | bit 14 - This bit     |
   |                       |                       | should be set if the  |
   |                       |                       | glyphs in the font    |
   |                       |                       | are simply generic    |
   |                       |                       | symbols for code      |
   |                       |                       | point ranges, such as |
   |                       |                       | for a last resort     |
   |                       |                       | font.                 |
   +-----------------------+-----------------------+-----------------------+
   | uint16                | unitsPerEm            | range from 64 to      |
   |                       |                       | 16384                 |
   +-----------------------+-----------------------+-----------------------+
   | longDateTime          | created               | international date    |
   +-----------------------+-----------------------+-----------------------+
   | longDateTime          | modified              | international date    |
   +-----------------------+-----------------------+-----------------------+
   | FWord                 | xMin                  | for all glyph         |
   |                       |                       | bounding boxes        |
   +-----------------------+-----------------------+-----------------------+
   | FWord                 | yMin                  | for all glyph         |
   |                       |                       | bounding boxes        |
   +-----------------------+-----------------------+-----------------------+
   | FWord                 | xMax                  | for all glyph         |
   |                       |                       | bounding boxes        |
   +-----------------------+-----------------------+-----------------------+
   | FWord                 | yMax                  | for all glyph         |
   |                       |                       | bounding boxes        |
   +-----------------------+-----------------------+-----------------------+
   | uint16                | macStyle              | bit 0 bold            |
   |                       |                       | bit 1 italic          |
   |                       |                       | bit 2 underline       |
   |                       |                       | bit 3 outline         |
   |                       |                       | bit 4 shadow          |
   |                       |                       | bit 5 condensed       |
   |                       |                       | (narrow)              |
   |                       |                       | bit 6 extended        |
   +-----------------------+-----------------------+-----------------------+
   | uint16                | lowestRecPPEM         | smallest readable     |
   |                       |                       | size in pixels        |
   +-----------------------+-----------------------+-----------------------+
   | int16                 | fontDirectionHint     | 0 Mixed directional   |
   |                       |                       | glyphs                |
   |                       |                       | 1 Only strongly left  |
   |                       |                       | to right glyphs       |
   |                       |                       | 2 Like 1 but also     |
   |                       |                       | contains neutrals     |
   |                       |                       | -1 Only strongly      |
   |                       |                       | right to left glyphs  |
   |                       |                       | -2 Like -1 but also   |
   |                       |                       | contains neutrals     |
   +-----------------------+-----------------------+-----------------------+
   | int16                 | indexToLocFormat      | 0 for short offsets,  |
   |                       |                       | 1 for long            |
   +-----------------------+-----------------------+-----------------------+
   | int16                 | glyphDataFormat       | 0 for current format  |
   +-----------------------+-----------------------+-----------------------+

--------------

特定平台信息
+++++++++++++

尽管这个表是TTF字体必须包含的，但不代表所有的OSX系统上的sfnt型字体都包含这张表。事实上，所有的TTF字体根本不需要这张表。

没有轮廓数据，只含位图数据的TTF字体不需要 ``'head'`` 表。应当逐字节使用相同的 ```'bhed'`` 表。OSX系统使用 ``'bhed'`` 表表明这个字体没有字形数据。

依赖
+++++

``'head'`` 表中的许多数据密切依赖于其它表中的数据。比如， ``unitsPerEm`` 值对所有需要处理曲线或者度量的表是基础的。在TTF字体引擎渲染的时候，这张表会全程使用。

对其它表的任意变动都会影响到表的 ``checkSumAdjustment`` 和 ``modified`` 值。所以最明智的做法就是对字体文件的任意变动都要更新 ``'head'`` 表。


.. toggle::

   Although it is officially listed as a "required" TrueType table, it
   is not in fact required for all sfnt-housed fonts on OS X. Indeed, it
   isn't even required by all TrueType fonts.

   TrueType fonts which have no outline data but consist of bitmaps only
   should not have a ``'head'`` table. They should use the byte-by-byte
   identical ```'bhed'`` table <Chap6bhed.html>`__ instead. The OS X
   uses the presence of a ``'bhed'`` to signal the fact that a font has
   no outlines.

   Dependencies
   ++++++++++++++

   Many of the fields in the ``'head'`` table are closely related to the
   values in other tables. For example, the ``unitsPerEm`` field is
   fundamental to all tables which deal with curves or metrics. This
   table is used throughout the TrueType rendering engine as a short-cut
   to determine various key aspects of the font.

   Inasmuch as any change to any other table will have an impact on the
   ``checkSumAdjustment`` and ``modified`` fields of the ``'head'``
   table, it is always wisest to update the 'head' table after any font
   editing operation.
