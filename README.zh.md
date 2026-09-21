# TXT2EPUB

一个简单的 TXT 到 [EPUB](https://en.wikipedia.org/wiki/EPUB) 书籍转换工具。

![Image](https://github.com/user-attachments/assets/836e0c03-5fb9-42ab-883c-2fd80f6c1cd3)

## 安装

首先您需要装有 Python3 和 Pip。如果您使用的是 Windows，那么 Python 安装会默认带 Pip。如果您使用的是 Linux，那么您可能需要额外安装 `python3-pip` 之类的包，具体的名称取决于您的发行版。

执行以下命令以安装 TXT2EPUB：

```shell
pip install txt2epub
```

## 使用

您可以直接在命令行里运行：

```shell
txt2epub convert -i <输入文件> -o <输出文件> -t <书名> -a <作者> -l <语言> -c <封面>
```

……或者运行图形化界面：

```shell
txt2epub gui
```

## 章节检测

默认启用章节检测。要关闭此功能，请在图形界面中选择 **No chapters**，
或在命令行中添加 `--no-chapters`：

```shell
txt2epub convert -i book.txt --no-chapters
```

关闭后，所有行（包括第一行）都作为正文保留在一个连续的内容章节中，不生成章节标题。
EPUB 导航中仅保留一个以书名命名的条目。

对于 `# 1`、`# 2001` 这样的编号标题，请选择 **Number chapters**，或使用
`txt2epub convert -i book.txt --number-chapters`。标题必须独占一行，由 `#`、
一个空格和 ASCII 数字组成（允许行首和行尾的空格或制表符，如 ` # 2`）。每章单独存储，章内空行不会分章。
首个标题之前的正文保留为以书名命名的开篇；没有匹配标题时，全文保留为一个正文部分。
此选项不能与 `--no-chapters` 同时使用。
选择 **Custom chapters** 后显示 **Chapter regex** 输入框，默认识别 `877화. 혈식`。
可以输入匹配完整标题行的 Python 正则表达式，例如 `Chapter [0-9]+: .+`。
匹配时忽略行首和行尾的空格及制表符，空行不作为标题；完整匹配行成为标题，捕获组不会改变标题。
不支持跨行标题。空表达式或无效表达式会阻止转换。其他模式不使用此输入框。
命令行示例：`txt2epub convert -i book.txt --custom-chapters --chapter-pattern 'Chapter [0-9]+: .+'`。
`--chapter-pattern` 必须与 `--custom-chapters` 一起使用，省略时保留默认格式。
`--number-chapters`、`--custom-chapters` 和 `--no-chapters` 不能同时使用。
周围的空行及 `*`、`-` 等装饰行保留为正文，不是识别标题的必要条件。

该程序按照标准的 TXT 书籍格式检测书籍的章节和章节标题：

- 章节之间由三个 LF 换行符（即，`\n\n\n`）分隔
- 新章节的第一行是章节的标题。

例如，在下面的文本中，有两个章节，标题分别为 "Chapter 1" 和 "Chapter 2"。

```txt
Chapter 1

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec at sapien ante.

Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae.


Chapter 2

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec at sapien ante.

Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae.
```
