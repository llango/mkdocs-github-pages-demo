# 编写你的文档

如何布局和编写 Markdown 源文件。

---

## 文件布局

文档的源文件应当是一个标准的 Markdown 的文件（请参阅下面的[使用 Markdown 编写](#markdown)，并放置于[文档目录](configuration.md#docs_dir)中。默认情况下，此目录将被命名为`docs`，并且将与`mkdocs.yml`配置文件一起存在于项目的顶层。

您可以创建的最简单的项目如下所示：

```no-highlight
mkdocs.yml
docs/
    index.md
```

按照惯例，您的项目主页应命名为`index`。以下任何扩展名都可用于您的 Markdown 源文件：`markdown`，`mdown`，`mkdn`，`mkd`，`md`。无论设置如何，文档目录中包含的所有 Markdown 文件都将在构建的站点中呈现。

您还可以通过创建多个 Markdown 文件来创建多页文档：

```no-highlight
mkdocs.yml
docs/
    index.md
    about.md
    license.md
```

文件的布局决定了最终生成的页面的网址。上述布局，将生成如下的结构的网址：

```no-highlight
/
/about/
/license/
```

你也可以将 Markdown 文件置于嵌套目录中，以满足不同的布局需求。

```no-highlight
docs/
    index.md
    user-guide/getting-started.md
    user-guide/configuration-options.md
    license.md
```

Source files inside nested directories will cause pages to be generated with nested URLs, like so:
如果使用了嵌套目录，那么生成页面的网址也将包含嵌套的网址：

```no-highlight
/
/user-guide/getting-started/
/user-guide/configuration-options/
/license/
```

### 索引页面

当请求目录时，默认情况下，大多数 Web 服务器将返回该目录中包含的索引文件（通常名为`index.html`）（如果存在）。 因此，上面所有示例中的主页都被命名为`index.md`，MkDocs 在构建站点时将生成为`index.html`。

许多存储库托管站点通过在浏览目录内容时显示 README 文件的内容来为 README 文件提供特殊处理。 因此，MkDocs 允许您将索引页面命名为`README.md`而不是`index.md`。这样，当用户浏览您的源代码时，存储库托管站点可以使用 README 文件来显示为该目录的索引页。但是，当 MkDocs 渲染您的站点时，该文件将重命名为`index.html`，以便服务器将其视为正确的索引文件处理。

如果在同一目录中同时存在`index.md`文件和`README.md`文件，则使用`index.md`文件并忽略`README.md`文件。

### 配置页面和导航

`mkdocs.yml`文件中的[nav]（configuration.md＃nav）配置设置定义了全局站点导航菜单中包含的页面以及该菜单的结构。如果未提供，将通过搜索[文档目录]（configuration.md＃docs_dir）中的所有 Markdown 文件并自动创建导航。自动创建的导航配置将始终根据文件名的字母数字顺序排序（索引文件将始终显示在最前面）。如果您希望导航菜单的排序方式不同，则需要手动定义导航配置。

简单的导航配置如下所示：

```no-highlight
nav:
- 'index.md'
- 'about.md'
```

导航配置中的所有路径必须相对于`docs_dir`配置选项。 如果该选项设置为默认值`docs`，则上述配置的源文件将位于`docs/index.md`和`docs/about.md`。

上面的示例将导致在顶层创建两个导航项，并根据文件内容（如果文件中未指定标题，则是文件名）推断其标题。要为页面自定义标题，可以在文件名之前添加标题。

```no-highlight
nav:
- Home: 'index.md'
- About: 'about.md'
```

请注意，如果为导航中的页面定义了标题，则该标题将在整个站点中用于该页面，并将覆盖页面本身定义的任何标题。

可以通过如下方式来创建二级导航：

```no-highlight
nav:
- Home: 'index.md'
- User Guide:
    - 'Writing your docs': 'writing-your-docs.md'
    - 'Styling your docs': 'styling-your-docs.md'
- About:
    - 'License': 'license.md'
    - 'Release Notes': 'release-notes.md'
```

通过上述配置，我们有三个顶级项目：“Home”，“User Guide”和“About”。 “Home”是指向该网站主页的链接。在“User Guide”部分下面列出了两个页面：“Writing your docs”和“Styling your docs”。在“About”部分下面列出了另外两个页面：“License”和“Release Notes”。

请注意，二级导航本身不能包含页面。二级导航只能包含子页面，或者子一级的导航。你可以根据需要，逐级嵌套。但是，请注意不要使用太多的嵌套，以免影响用户的使用。虽然二级导航模仿了你的文件布局结果，但是不是必须的

导航配置中未列出的任何页面仍将呈现并包含在构建的站点中，但是，它们不会出现在全局导航中，也不会包含在“previous”和“next”链接中。 除非直接链接，否则此类页面将被“隐藏”。

## 使用 Markdown 写作

MkDocs 的页面需使用[Markdown][md]来创作，这是一种轻量级标记语言，可生成易于阅读，易于编写的纯文本文档，可以以可预测的方式转换为有效的 HTML 文档。

MkDocs 使用[Python-Markdown]库将 Markdown 文档转换为 HTML。Python-Markdown 几乎完全符合[eference implementation][md]，尽管有一些非常小的[差异]。

In addition to the base Markdown [syntax] which is common across all Markdown implementations, MkDocs includes support for extending the Markdown syntax with Python-Markdown [extensions]. See the MkDocs' [markdown_extensions] configuration setting for details on how to enable extensions.
除了适配基本的 Markdown[语法][syntax]之外，MkDocs 还支持使用 Python-Markdown[extensions]扩展 Markdown 语法。有关如何启用扩展的详细信息，请参阅 MkDocs 的[markdown_extensions]配置设置。

MkDocs 默认包含一些扩展，在下面突出显示。

[python-markdown]: https://python-markdown.github.io/
[md]: https://daringfireball.net/projects/markdown/
[差异]: https://python-markdown.github.io/#differences
[syntax]: https://daringfireball.net/projects/markdown/syntax
[extensions]: https://python-markdown.github.io/extensions/
[markdown_extensions]: configuration.md#markdown_extensions

### 内部链接

MkDocs 允许您通过使用常规 Markdown[链接][links]来链接您的文档。但是，如下所述，格式化专门针对 MkDocs 的链接还有一些额外的好处。

[links]: https://daringfireball.net/projects/markdown/syntax#link

#### 链接到页面

在文档中的页面之间进行链接时，您只需使用常规的 Markdown[链接][links]语法，包括*相对路径*到您希望链接到的 Markdown 文档。

```no-highlight
Please see the [project license](license.md) for further details.
```

当 MkDocs 构建运行时，这些 Markdown 链接将自动转换为指向相应 HTML 页面的 HTML 超链接。

!!! warning
使用带链接的绝对路径不受官方支持。 相对路径由 MkDocs 调整，以确保它们始终相对于页面。绝对路径根本不会被修改。这意味着使用绝对路径的链接可能在本地环境中正常工作，但是一旦将它们部署到生产服务器，它们可能会不能正常实现。

如果目标文档文件位于另一个目录中，则需要确保在链接中包含任何相对目录路径。

```no-highlight
Please see the [project license](../about/license.md) for further details.
```

MkDocs 使用[toc]扩展名为 Markdown 文档中的每个 header 生成一个 ID。 您可以使用该 ID 链接到目标文档中的相应部分。 生成的 HTML 将正确转换链接的路径，并使锚点来完成跳转。

```no-highlight
Please see the [project license](about.md#license) for further details.
```

请注意，ID 是从 header 的文本来创建的。 所有文本都转换为小写，任何不允许的字符（包括空格）都将转换为下划线。 连续的下划线将转换为一个下划线。

toc 扩展提供了一些配置设置，您可以在`mkdocs.yml`配置文件中设置这些设置以更改默认行为：

`permalink`:

: 在每个标头的末尾生成永久链接。 默认值：`False`。

    设置为True时，段落符号（＆para;或`＆para;`）用作链接文本。 设置为字符串时，提供的字符串将用作链接文本。 例如，要使用哈希符号（`＃`），请执行以下操作：

        markdown_extensions:
            - toc:
                permalink: "#"

`baselevel`:

: 标头的基本级别。 默认值：`1`。

    此设置允许自动调整标题级别以适合HTML模板的层次结构。例如，如果页面的Markdown文本不应包含高于级别2（`<h2>`）的任何标题，请执行以下操作：

        markdown_extensions:
            - toc:
                baselevel: 2

    然后，文档中的任何标题都将增加1.例如，标题`#Header`将在HTML输出中呈现为2级标题（`<h2>`）。

`separator`:

: 单词分隔符。 默认值：`-`。

    替换生成的ID中的空格的字符。 如果你喜欢下划线，那么：

        markdown_extensions:
            - toc:
                separator: "_"

请注意，如果要定义上述设置中的多个，则必须在`markdown_extensions`配置选项中的单个`toc`条目下执行此操作。

```yml
markdown_extensions:
  - toc:
      permalink: "#"
      baselevel: 2
      separator: "_"
```

[toc]: https://python-markdown.github.io/extensions/toc/

#### 链接到图像和媒体

除 Markdown 源文件外，您还可以在文档中包含其他文件类型，这些文件类型将在生成文档站点时复制。这些可能包括图像和其他媒体。

例如，如果您的项目文档需要包含[GitHub 页面 CNAME 文件][github pages cname file]和 PNG 格式的屏幕截图图像，那么您的文件布局可能如下所示：

```no-highlight
mkdocs.yml
docs/
    CNAME
    index.md
    about.md
    license.md
    img/
        screenshot.png
```

要在文档源文件中包含图像，只需使用任何常规的 Markdown 图像语法：

```Markdown
Cupcake indexer is a snazzy new project for indexing small cakes.

![Screenshot](img/screenshot.png)

*Above: Cupcake indexer in progress*
```

现在，您可以在构建文档时嵌入图像，如果您使用的是 Markdown 编辑器来处理文档，也应该可以预览该图像。

[github pages cname file]: https://help.github.com/articles/using-a-custom-domain-with-github-pages/

#### 从原始 HTML 链接

当 Markdown 语法不符合作者的需要时，Markdown 允许文档作者回退到原始 HTML。MkDocs 在这方面并不限制 Markdown。 但是，由于 Markdown 解析器忽略了所有原始 HTML，因此 MkDocs 无法验证或转换原始 HTML 中包含的链接。在原始 HTML 中包含内部链接时，您需要为呈现的文档手动格式化链接。

### Meta-Data

### 元数据

MkDocs 包括支持 YAML 和 MultiMarkdown 两种样式的元数据（通常称为前端内容）。元数据由 Markdown 文档开头定义的一系列关键字和值组成，这些关键字和值在 Python-Markdown 处理之前从文档中删除。键/值对由 MkDocs 传递给页面模板。 因此，如果主题支持，则任何键的值都可以显示在页面上或用于控制页面呈现。有关可能支持哪些关键字的详细信息，请参阅相应的主题支持文档（如果有）。

除了在模板中显示信息之外，MkDocs 还支持一些预定义的元数据键，这些键可以改变该特定页面的 MkDoc 的行为。支持以下键：

`template`:

: The template to use with the current page.
: 用于当前页面的模板。

    默认情况下，MkDocs使用主题的`main.html`模板来呈现Markdown页面。您可以使用`template`元数据键为该特定页面定义不同的模板文件。模板文件必须在主题环境中定义的路径上可用。

`title`:

: 用于文档的“标题”。

    MkDocs将尝试按以下方式确定文档的标题：

    1.在文档的[nav]配置设置中定义的标题。

    2.在文档的“title”元数据键中定义的标题。

    3.文档正文第一行上的1级Markdown标题。

    4.文档的文件名。

    在找到页面的标题后，MkDoc不会继续检查上面列表中的任何其他来源。

#### YAML 样式元数据

YAML 样式元数据由 YAML 样式分隔符中包含的[YAML]键/值对组成，以标记元数据的开始和/或结束。 文档的第一行必须是`---`。 元数据在结束的那一行，需包含一个结束分隔符（`---`或`...`）。 分隔符之间的内容被解析为[YAML]。

```no-highlight
---
title: My Document
summary: A brief description of my document.
authors:
    - Waylan Limberg
    - Tom Christie
date: 2018-07-10
some_url: https://example.com
---
This is the first paragraph of the document.
```

YAML 能够检测数据类型。因此，在上面的例子中，`title`，`summary`和`some_url`的值是字符串，`authors`的值是字符串列表，`date`的值是`datetime.date`对象。请注意，YAML 键区分大小写，MkDocs 期望键全部为小写。YAML 的顶级必须是键/值对的集合，这导致返回 Python`dict`。如果返回任何其他类型或 YAML 解析器遇到错误，则 MkDocs 不会将该部分识别为元数据，页面的`meta`属性将为空，并且该部分不会从文档中删除。

#### MultiMarkdown 样式元数据

MultiMarkdown 样式元数据使用[MultiMarkdown]项目首先引入的格式。该数据由 Markdown 文档开头定义的一系列关键字和值组成，如下所示：

```no-highlight
Title:   My Document
Summary: A brief description of my document.
Authors: Waylan Limberg
         Tom Christie
Date:    January 23, 2018
blank-value:
some_url: https://example.com

This is the first paragraph of the document.
```

关键字不区分大小写，可以包含字母，数字，下划线和短划线，并且必须以冒号结尾。值由冒号后面的任何内容组成，甚至可以为空。

如果一行由 4 个或更多空格缩进，则该行被假定为前一个关键字的值的附加行。关键字可以包含所需的行数。所有行都连接成一个字符串。

第一个空白行结束文档的所有元数据。 因此，文档的第一行不能为空。

!!! note

    对于MultiMarkdown样式的元数据，MkDocs不支持YAML样式的分隔符（`---`或` ``` `）。实际上，MkDocs依赖于分隔符的存在与否来确定是否正在使用YAML样式的元数据或MultiMarkdown样式的元数据。 如果检测到分隔符，但是分隔符之间的内容不是有效的YAML元数据，则MkDocs不会尝试将内容解析为MultiMarkdown样式的元数据。

[yaml]: http://yaml.org
[multimarkdown]: http://fletcherpenney.net/MultiMarkdown_Syntax_Guide#metadata
[nav]: configuration.md#nav

### 表格

[表格][tables]扩展为 Markdown 添加了一个基本的表格语法，它在多个实现中很流行。 语法相当简单，通常仅对简单的表格数据有用。

一个简单的表看起来像这样：

```no-highlight
First Header | Second Header | Third Header
------------ | ------------- | ------------
Content Cell | Content Cell  | Content Cell
Content Cell | Content Cell  | Content Cell
```

如果您愿意，可以在表格的每一行添加一个前导和尾随管道：

```no-highlight
| First Header | Second Header | Third Header |
| ------------ | ------------- | ------------ |
| Content Cell | Content Cell  | Content Cell |
| Content Cell | Content Cell  | Content Cell |
```

通过向分隔线添加冒号来指定每列的对齐方式：

```no-highlight
First Header | Second Header | Third Header
:----------- |:-------------:| -----------:
Left         | Center        | Right
Left         | Center        | Right
```

请注意，表格单元格不能包含任何块级别元素，并且不能包含多行文本。 但是，它们可以包含 Markdown[语法][syntax]规则中定义的内联 Markdown。

此外，表必须用空行包围。 表格前后必须有一个空行。

[tables]: https://python-markdown.github.io/extensions/tables/

### 受控代码块

[fenced code blocks]扩展添加了一种定义代码块的替代方法，无需缩进。

第一行应包含 3 个或更多反引号（`` ` ``）字符，最后一行应包含相同数量的反引号字符（`` ` ``）：

````no-highlight
```
Fenced code blocks are like Standard Markdown’s
regular code blocks, except that they’re not indented and
instead rely on start and end fence lines to delimit the code block.
```
````

使用这种方法，可以选择在反引号后的第一行指定语言，该反引号通知所使用语言的任何语法高亮显示器：

````no-highlight
```python
def fn():
    pass
```
````

请注意，受限制的代码块不能缩进。 因此，它们不能嵌套在列表项，块引用等内。

[fenced code blocks]: https://python-markdown.github.io/extensions/fenced_code_blocks/
