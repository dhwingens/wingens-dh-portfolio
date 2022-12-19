---
layout: page
title: TEI Exercise
description: Encoding the first poem of the 1856 edition of Leaves of Grass in XML
---


```xml
<TEI xmlns="http://www.tei-c.org/ns/1.0">
    <teiHeader>
      <fileDesc>
        <titleStmt>
          <title>Leaves of Grass</title>
          <author>Walt Whitman</author>
        </titleStmt>
        <publicationStmt>     
          <p>
            <!--Publication Information-->
          </p>
        </publicationStmt>
        <sourceDesc>
          <p>
            <!--Information about the source-->
          </p>
        </sourceDesc>
      </fileDesc>
    </teiHeader>
    <text xml:lang="eng">
      <head><hi rend=uppercase>leaves of grass</hi></head>
        <hi>1—Poem of Walt Whitman, an American.</hi>
      <body>
        <l><hi rend=uppercase>I CELEBRATE</hi> myself,</l>
        <l>And what I assume you shall assume,</l>
        <l>For every atom belonging to me, as good belongs</l>
        <l>to you.</l>

        <l>I loafe and invite my soul,</l>
        <l>I lean and loafe at my ease, observing a spear of</l>
        <l>summer grass.</l>

        <l>Houses and rooms are full of perfumes—the</l>
        <l>shelves are crowded with perfumes,</l>
        <l>I breathe the fragrance myself, and know it and</l>
        <l>like it,</l>
        <l>The distillation would intoxicate me also, but I</l>
        <l>shall not let it.</l>
        
        <l>The atmosphere is not a perfume, it has no taste</l>
        <l>of the distillation, it is odorless,</l>
      </body>
    </text>
  </TEI>
```