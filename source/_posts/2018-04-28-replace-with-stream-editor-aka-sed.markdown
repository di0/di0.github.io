---
layout: post
title: "Replace With Stream Editor(AKA Sed)"
date: 2018-04-28 14:08:18 -0300
comments: true
categories: linux
---

The **sed** is command-line utility editor, which filtering and transforming text. Bellow an example of how works <!--more--> replacement order:

```bash
sed -i 's/input/output/g' some_file
```

*where:*

<li> i = --in-place, edit **file.txt** and save it.</li>
<li> s = substitute statement.</li>
<li> input = original match case.</li>
<li> output = the match replace it with.</li>
<li> g = global(replace all occurrence, instead of first occurrence).</li>
<li> some_file = some target file.</li>
