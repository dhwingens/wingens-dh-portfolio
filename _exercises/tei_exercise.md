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
        <hi rend=uppercase>I CELEBRATE</hi> myself,
        And what I assume you shall assume,
        For every atom belonging to me, as good belongs
        to you.

        I loafe and invite my soul,
        I lean and loafe at my ease, observing a spear of
        summer grass.

        Houses and rooms are full of perfumes—the
        shelves are crowded with perfumes,
        I breathe the fragrance myself, and know it and
        like it,
        The distillation would intoxicate me also, but I
        shall not let it.

        The atmosphere is not a perfume, it has no taste
        of the distillation, it is odorless,

      </body>
    </text>
  </TEI>
```