hmtx - 水平度量表
=================


本文内容
~~~~~~~~

用于水平文本布局的字形度量包括字形前进宽度、侧边距和 X
方向的最小值和最大值（xMin、xMax）。这些是使用字形轮廓数据（'glyf'、'CFF'
或 CFF2）和水平度量表的组合得出的。水平度量 ('hmtx')
表提供字形前进宽度和左侧方位。

在具有 TrueType 轮廓数据的字体中，glyf表提供 xMin 和xMax
值，但不提供提前宽度或侧边距。前进宽度总是从“hmtx”表中获得。在某些字体中，根据head表中标志的状态，左侧方位可能与“glyf”表中的
xMin 值相同，但并非所有字体都如此。（参见“head”表中标志字段第 1
位的描述。）因此，“hmtx”表中提供了左侧轴承。右侧方位始终使用“hmtx”表中的提前宽度和左侧方位值以及字形描述中的边界框信息得出
- 更多详细信息请参见下文。

在具有 TrueType 轮廓数据的可变字体中，“hmtx”表中的左侧方位值必须始终等于
xMin（必须设置“head”标志字段的第 1
位）。因此，这些值也可以直接从“glyf”表中导出。请注意，这些值仅适用于可变字体的默认实例：非默认实例可能具有不同的侧向值。这些可以使用“gvar”表从插值的“幻象点”坐标导出（有关更多详细信息，请参见下文），或者通过将HVAR表中的变化数据应用于“glyf”或“hmtx”表中的默认实例值。

在具有 CFF 版本 1 轮廓数据的字体中，“CFF”表确实包含提前宽度。这些值由
PostScript 处理器使用，但不用于 OpenType 布局。在 OpenType
上下文中，“hmtx”表是必需的，并且必须用于提前宽度。请注意，共享“CFF”表的字体集合文件中的字体可能会在特定字形索引的特定字体“hmtx”表中指定不同的前进宽度。另请注意，CFF2
表不包括提前宽度。此外，对于 CFF 或 CFF2 数据，没有明确的 xMin 和 xMax
值；侧方位隐含在 CharString 数据中，可以从 CFF / CFF2
光栅化器获得。但是，一些布局引擎可能会使用“hmtx”表中的左侧方位角值；因此，字体制作工具应确保“hmtx”表中的左侧方位值与
CharString 数据中反映的隐式 xMin 值相匹配。在具有 CFF2
轮廓数据的可变字体中，非默认实例的左侧方位角和前进宽度值应通过组合来自“hmtx”和
HVAR 表的信息来获得。

该表使用 longHorMetric 记录来给出字形的前进宽度和左侧方位角。记录按字形
ID
进行索引。作为优化，记录数可以小于字形数，在这种情况下，最后一条记录的前进宽度值适用于所有剩余的字形
ID。这在等宽字体或具有大量具有相同提前宽度的字形的字体中非常有用（前提是字形的顺序正确）。longHorMetric
记录的数量由“hhea”表中的 numberOfHMetrics 字段确定。

如果 numberOfHMetrics 小于字形的总数，则 hMetrics
数组后跟一个数组，用于剩余字形的左侧轴承值。leftSideBearings
数组中的元素数将从“maxp `” <maxp>`__\ 表中的 numGlyphs 字段减去
numberOfHMetrics 得出。

*水平指标表：*

.. container:: has-inner-focus

   +---------------+-------------------------+-------------------------+
   | 类型          | 姓名                    | 描述                    |
   +===============+=========================+=========================+
   | longHorMetric | hMetrics                | 每个                    |
   |               | [numberOfHMetrics]      | 字形的配对前进宽度和左  |
   |               |                         | 侧方位角值。记录按字形  |
   |               |                         | ID 进行索引。           |
   +---------------+-------------------------+-------------------------+
   | int16         | lef                     | 大于或等于              |
   |               | tSideBearings[numGlyphs | numberOfHMetrics 的字形 |
   |               | - numberOfHMetrics]     | ID 的左侧方位角。       |
   +---------------+-------------------------+-------------------------+

*longHorMetric 记录：*



====== ======== ==================================
类型   姓名     描述
====== ======== ==================================
uint16 提前宽度 前进宽度，以字体设计单位为单位。
int16  lsb      字形左侧轴承，以字体设计单位表示。
====== ======== ==================================

在具有 TrueType 轮廓的字体中，每个字形的 xMin 和 xMax
值在“glyf”表中给出。前进宽度（“aw”）和左侧方位角（“lsb”）可以从字形“幻象点”推导出来，它们是由
TrueType 光栅化器计算的；或者它们可以从“hmtx”表中获得。在具有 CFF 或
CFF2 轮廓的字体中，xMin（= 左侧方位角）和 xMax 值可以从 CFF / CFF2
光栅化器获得。根据这些值，右侧方位角 (“rsb”) 计算如下：

.. code::

    rsb = aw - (lsb + xMax - xMin)

如果 pp1 和 pp2 是用于控制 lsb 和 rsb 的 TrueType 幻象点，则它们在 X
方向的初始位置计算如下：

.. code::

   pp1 = xMin - lsb
   pp2 = pp1 + aw

如果字形没有轮廓，则不定义
xMax/xMin。此类字形的“hmtx”表中指示的左侧方位角应为零。
