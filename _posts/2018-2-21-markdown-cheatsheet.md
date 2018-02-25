---
layout: post
title: Markdown Cheatsheet
---



常用：
==
[原文](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* **header**

```markdown
#H1
##H2
###H3
####H4
#####H5``
######H6

H1
==
H2
--
```

VSCode同步编辑markdown小技巧
==

[官网](https://code.visualstudio.com/docs/languages/markdown)

* 1. 拆分预览
* 2. shift+command+v 预览markdown



VSCode post博文样板代码
==
```json

	"new post": {
		"prefix": "post",
		"body": [
			"---",
			"layout: post",
			"title: $1",
			"---"
		],
		"description": "default markdown template for post"
	}
```

在线markdown语法
==
http://commonmark.org/help/tutorial/10-nestedLists.html