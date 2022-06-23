.. _cff--compact-font-format-table:

CFF - 紧凑字体格式表
====================

.. container::
display-flex-tablet justify-content-space-between-tablet page-metadata-container

   -  项目
   -  2021/12/09
   -  2分钟可浏览
   -  2个参与者

   .. container:: display-flex
      :name: user-feedback

本文内容
~~~~~~~~

此表包含紧凑字体格式字体表示（也称为 PostScript Type 1 或
CIDFont），其结构根据 Adob​​e `Technical Note #5176：“The Compact Font
Format
Specification” <http://partners.adobe.com/public/developer/en/font/5176.CFF.pdf>`__\ 和\ `Adob​​e
Technical Note #5177：“Type
2字符串格式。” <http://partners.adobe.com/public/developer/en/font/5177.Type2.pdf>`__

带有 TrueType 轮廓的 OpenType
字体使用字形索引来指定和访问字体中的字形；例如，在\ `“loca”表 <loca>`__\ 中建立索引，从而访问“glyf”表中的字形数据。这个概念保留在
OpenType CFF 字体中，除了字形数据是通过“CFF”表的 CharStrings INDEX
访问的。

CFF 数据中的 Name INDEX 必须只包含一个条目；也就是说，CFF FontSet
中必须只有一种字体。此名称不要求与\ `“名称”表 <name>`__\ 中的名称 ID 6
条目相同。请注意，在 OpenType
字体集合文件中，单个“CFF”表可以跨多种字体共享；应用程序使用的名称必须是“名称”表中提供的名称，而不是名称索引条目。CFF
Top DICT 必须指定 CharstringType 值为 2。“maxp `”表 <maxp>`__\ 中的
numGlyphs 字段必须与 CFF 的 CharStrings INDEX 中的条目数相同。OpenType
字体字形索引与字体中所有字形的 CFF 字形索引相同。
