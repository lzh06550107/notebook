# [Emacs Markdown Mode](http://jblevins.org/projects/markdown-mode/)
　　markdown-mode是在GNU Emacs中编辑[Markdown](http://daringfireball.net/projects/markdown/)格式文本文件的主模式。Markdown-mode是自由软件。位于GNU GPL许可下。  
　　最新稳定版本是Markdown-model 2.1，于2016.1.9号释放：

* [markdown-mode.el](http://jblevins.org/projects/markdown-mode/markdown-mode.el)
* [Screenshot](http://jblevins.org/projects/markdown-mode/screenshots/20160108-001.png)
* [Release notes](http://jblevins.org/projects/markdown-mode/rev-2-1)

　　虽然Markdown-model是免费的，但是它是很多年工作的结果。如果你经常使用它，它将会使你的生活或者工作更容器或者更享受，然后，你可以通过支持[Markdown-mode](http://jblevins.org/projects/donate)来感谢它！  
　　最新的开发版本可以从Git仓库获取<http://jblevins.org/git/markdown-mode.git>:

```
git clone git://jblevins.org/git/markdown-mode.git
git clone https://github.com/jrblevin/markdown-mode.git
```

　　Markdown-mode同样在几个包管理器下可用，包括：![](https://travis-ci.org/jrblevin/markdown-mode.svg?branch=master)

* Debian Linux:[elpa-markdown-mode](https://packages.debian.org/sid/lisp/elpa-markdown-mode)和[emacs-goodies-el](http://packages.debian.org/emacs-goodies-el)
* Ubuntu Linux:[elpa-markdown-mod](http://packages.ubuntu.com/search?keywords=elpa-markdown-mode)和[emacs-goodies-el](http://packages.ubuntu.com/search?keywords=emacs-goodies-el)
* RedHat and Fedora Linux:[emacs-goodies](https://apps.fedoraproject.org/packages/emacs-goodies)
* NetBSD:[textproc/markdown-mode](http://pkgsrc.se/textproc/markdown-mode)
* MacPorts:[markdown-model.el](https://trac.macports.org/browser/trunk/dports/editors/markdown-mode.el/Portfile)[(pending)](http://trac.macports.org/ticket/35716)
* FreeBSD:[textproc/markdown-mode.el](http://svnweb.freebsd.org/ports/head/textproc/markdown-mode.el)

# 安装 #

　　确保`markdown-model.el`位于加载路径下且增加如下行到你的`.emacs`文件下来关联以`.text`,`.markdwon`和`.md`为扩展名册文件：
```lisp
(autoload 'markdown-mode "markdown-mode"
   "Major mode for editing Markdown files" t)
(add-to-list 'auto-mode-alist '("\\.text\\'" . markdown-mode))
(add-to-list 'auto-mode-alist '("\\.markdown\\'" . markdown-mode))
(add-to-list 'auto-mode-alist '("\\.md\\'" . markdown-mode))
```
　　官方没有规定markdown文件的扩展名，也没有事实上的标准，所以你可以在上面很容易的增加，改变或者移除任何需要的文件扩展名。  
　　`markdown-mode`依赖`cl-lib`,它自从GNU Emacs24.3开始被捆绑在一起。GNU Emacs24.1和24.2的用户可以使用`package.el`来安装`cl-lib`。

# 使用 #

　　基于它们的功能，通过前缀来分组绑定键。例如，插入链接命名位于`C-c C-a`分组下，这里的`C-a`是一个HTML`<a>`标签的助记符。其它情况，与HTML的联系不是直接的。例如，处理头的命名以`C-c C-t`开头(助记符：titling)。每个组主要的命名将会在下面描述。你可以使用`C-c C-h`来获取所有绑定键的列表。移动和换挡命名趋向于关联成对分隔符，例如`M-{`和`}`或者`C-c <`和`C-c >`。大纲导航绑定键和`org-mode`中的一样。最后，运行Markdown或者维护一个打开的文件的命令位于`C-c C-c`前缀的命令下。最常用的命令描述如下。你可以获取使用`C-c C-h`来获取所有绑定键的列表。

- Hyperlinks:`C-c C-a`  
　　在该组命名中，`C-c C-a l`以`[text](url)`的形式插入一个内部链接。链接文本按照如下规则决定的。首先，如果这是一个活动区域(例如，当transient标记模式打开且标记被激活)，使用它作为链接文本。其次，如果光标位于一个单词上，使用它作为链接文本。在这两种情况下，最初文本将会被链接取代且光标位于插入一个URL位置的地方。否则，插入一个空链接标签且把光标位于插入链接文本的地方。  
　　`C-c C-a L`插入一个`[text][label]`形式的引用链接。可选地，一个对应的引用标签被定义。链接文本被决定的方式和一个内部链接一样(当激活时，使用区域，或者光标所在的单词)，但是不会插入一个空的标签，链接文本将会从minibuffer中读取。(也就是所有的数据通过minibuffer来输入)。引用标签从minibuffer中和当前已经定义的引用集中补全来获取。为了创建一个显示引用链接，按`RET`来获取默认值(一个空标签)。如果输入被引用标签没有定义，将需要额外提示填充URL和可选标题。如果一个URL被提供，一个引用标签定义将会根据`markdown-reference-location`被插入。如果一个标题被给定，它将会被增加到引用定义的末尾且在转换为XHTML时填充title属性。  
　　`C-c C-a u`插入一个裸露url，被尖括号包裹。当位于一个活动区域时，在区域中的文本被用作URL。如果光标位于一个URL上。该URL被使用。否则，插入尖括号然后光标位于其内用来输入URL。  
　　`C-c C-a f`在光标位置插入一个脚注标签，在下面插入一个脚注定义，且光标位于插入脚注文本的位置上。注意，脚注是Markdown的扩展不被所有的处理器支持。  
　　`C-c C-a w`行为很像内部链接插入命名且插入一个`[[WikiLink]]`形式的wiki链接。如果是一个活动区域，使用该区域作为链接文本。如果光标位于一个单词上，使用该单词作为链接文本。如果没有活动区域且光标没有位于一个单词上，简单插入链接标记。注意，wiki链接是一个扩展标记且不被所有处理器支持。

- 图像：`C-c C-i`
　　`C-c C-i i`插入一个行内图像，使用活动区域或者光标所在的单词，如果存在，把它们作为alt文本。`C-c C-i I`行为类似且插入一个引用类型图像。

- 样式：`C-c C-s`
　　`C-c C-s e`插入标记使一个区域或者单词斜体(`e`即`<em>`强调)。如果具有一个活动区域，使该区域斜体。如果光标在一个非斜体单词上，使该单词斜体。如果光标位于一个已经斜体的单词或者短语上，移除斜体标记。否则，简单地插入斜体分隔符然后把光标位于它们之间。类似地，使用`C-c C-s s`插入粗体(`<strong>`)和`C-c C-s c`来插入行内代码(`<code>`)。  
　　如果活动区域存在，`C-c C-s b`为它插入一个块引用或者开启一个新的块引用。`C-c C-s C-b`是一个总是操作在区域上的变种，无论它是否被激活。如果存在，适当数量的缩进根据周边环境被自动计算,但是稍后可以使用区域缩进命令来调整。  
　　`C-c C-s p`行为类似插入已经格式化的代码块，命令`C-c C-s C-p`仅对区域有效。

- 头：`C-c C-t`
　　如果活动区域存在，所有标题插入命令使用在活动区域的文本作为文本标题。否则，如果当前行非空，它们使用当前行的文本。最后，如果既没有活动区域且当前行为空行。设置命令将会提示填充标题文本。  
　　`C-c C-t h`将使用自动选择的类型和层级来插入一个标题(两个都由前一个标题决定)。`C-c C-t H`行为类似，但是使用下划线来标记标题，还是自动计算层级。在自动决定的层级不是你希望的时候，层级可以如下面所描述的可以提升或下降。可选地，在该命令前放置一个`C-u`前缀，可以提升一个层级来插入标题，或者`C-u C-u`前缀来提供降低一个层级。  
　　为了插入一个特定层级和类型的标题，使用`C-c C-t 1`到`C-c C-t 6`来井号标记一个标题和`C-c C-t !`或者`C-c C-t @`来分别设置第一级和第二季标题。注意,`!`是`S-1`和`@`是`S-2`。  
　　如果光标位于标题上，这些命令为了更新层级和类型，将会取代存在的标记。为了移除光标处标题的标签，按`C-c C-k`用来kill标题然后`C-y`用来粘贴删除的标题头回来。

- 水平线:`C-c -`  
　　`C-c -`插入一个水平线。默认地，插入`markdown-hr-strings`列表项中的第一个字符串(最好的水平线)。使用一个`C-u`前缀，插入最后一个字符串。使用一个带有数字的前缀，插入列表指定位置`N`的字符串(从1开始计数)。  

- Markdown和维护命令:`C-c C-c`
　　编译:`C-c C-c m`将会在当前缓存区运行Markdown且在另一个缓冲区显示输出结果。预览：`C-c C-c p`在当前缓冲区运行Markdown然后预览，存储输出文件在一个临时文件中，且显示文件在浏览器中。输出：`C-c C-c e`将会在当前缓冲区运行Markdown然后保存结果在basename.html文件中，basename是Markdown文件去除扩展名的部分。输出和视图：`C-c C-c v`输出文件然后在一个浏览器中查看它。打开：`C-c C-c o`将会使用`markdown-open-command`命令来打开Markdown源码文件。现场输出：按`C-c C-c l`来打开`markdown-live-preview-mode`来并排查看输出结果和Markdown源码。对于所有输出命令，输出文件将会被覆盖而没有任何提示。`markdown-live-preview-window-function`可以被自定义在一个浏览器中打开而不是`eww`。  
　　总结：

	- `C-c C-c m` : `markdown-command` > `*markdown-output*`缓冲区。
	- `C-c C-c p` : `markdown-command` > 临时文件 > 浏览器。
	- `C-c C-c e` : `markdown-command` > `basename.html`。
	- `C-c C-c v` : `markdown-command` > `basename.html` > 浏览器。
	- `C-c C-c w` : `markdown-command` > kill ring。
	- `C-c C-c o` : `markdown-open-command`。
	- `C-c C-c l` : `markdown-live-preview-mode` > `*eww*`缓冲区
	
　　`C-c C-c c`将会检查未被定义的引用。如果存在，一个小的缓冲区将会打开列出所有未定义的引用且具有对应的行号。在Emacs22及以后，从该列表中选择一个引用然后回车在缓冲区结尾处插入一个空引用定义。类似地，选择行号将会跳转到对应的行。  
　　`C-c C-c n`对缓冲区中失序的有序列表重新编号。  
　　`C-c C-c ]`补全所有缓冲区的标题且规格化所有水平线。  

- 打开连接:`C-c C-o`  
　　当光标位于一个行内或者引用链接上，按`C-c C-o`在一个浏览器中打开该URL。当光标位于一个wiki链接，在另一个缓冲区中打开它(在当前窗口，或者使用`C-u`前缀在另一个窗口中)。使用`M-p`和`M-n`快速跳转到前一个或者后一个任何链接类型。

- 跳转:`C-c C-j`  
　　使用`C-c C-j`从光标所在对象跳转到它的文本中的对应搭档。在引用链接和引用定义之间以及在脚注标记和脚注文本之间的跳转。如果多于一个链接使用相同的引用名称，一个新的缓冲区将会被创建包含可点击且可跳转的链接按钮。你可以按`TAB`或者`S-TAB`来在该窗口按钮之间跳转。

- 提升或者降级:`C-c C-`和`C-c C-=`  
　　标题，水平线，列表项以及斜体和粗体文本可以被提升和降级。对于标题，“提升”意味着递减层级(例如，从`<h2>`移动到`<h1>`)。对于水平线，提升和降级意味着在水平线字符串列表内向后和向前移动。对于粗体和斜体文本，提升和降级意味着从下划线到星号之间来改变标记。按`C-c C-`或者`M-LEFT`来提升光标所在的元素。  
　　记住这些命令，注意`-`为减少层级(提升)，而`=`(和`+`号一样)用来增加层级(降级)。类似地，当提升或者降级时，左箭头和右箭头键指示标题标记移动的方向。

- 补全:`C-c C-]`  
　　补全标记是规范化文本标记形式，例如，setext形式的标题头部分的下划线和标题头文本具有同样的长度，或者atx形式的标题开头的标记和结束的标记数量相等且在标题头中没有额外的空白。如果它们被认为是不完整的，`C-c C-]`补全光标处的标记。

- 编辑列表：`M-RET`,`M-UP`,`M-DOWN`,`M-LEFT`和`M-RIGHT`  
　　使用`M-RET`来插入新的列表项。该命令会插入合适的标记(可能是无序列表或者有序列表的下一个数字)且会通过检查附近的列表项来缩进层级。如果在光标所在的前面和后面没有列表，开启一个新的列表。在该命令加`C-u`前缀会减少一个缩进层次。而加`C-u C-u`则会增加一个层级缩进。  
　　存在的列表项可以使用`M-UP`或者`M-DOWN`来上下移动和使用`M-RIGHT`或者`M-LEFT`来降级或者提升。

- 编辑子树:`M-S-UP`,`M-S-DOWN`,`M-S-LEFT`和`M-S-RIGHT`  
　　atx形式标题的整个子树可以使用`M-S-LEFT`和`M-S-RIGHT`来提升和降级它，它绑定的列表项也会做出提供和降级的反映。类似地，子树使用`M-S-UP`和`M-S-DOWN`来上下移动。  
　　请注意遵循提升和降级的边界行为。任何第六层级的标题将不能再降级(例如，它们将维持在第六层级，因为Markdown和HTML只定义六个层级)且任何一级标题将会被完全地移除(例如，标题标记将会被移除，因为一个0级标题没有定义)。

- 移动区域:`C-c <`和`C-c >`  
　　在区域中的文本可以使用`C-c >`以组的形式缩进到下一个缩进点(在当前上下文中计算)，和`C-c <`来提升一个缩进点。这些绑定键和在`python-mode`中的类似命名相同。

- Killing元素:`C-c C-k`  
　　按`C-c C-k`杀掉当前光标处的东西，然后增加重要的文本(不带标记)到kill环中。可以被kill掉的东西包括(大概的处理顺序):行内代码，标题头，水平线，链接(增加链接文本到kill环),图片(增加alt文本到kill环),尖角URIs,邮箱地址，粗体，斜体，引用定义(增加URI到kill环)，脚注标记和文本(杀掉标记和文本，增加文本到kill环)和列表项。

- 大纲导航:`C-c C-n`,`C-c C-p`,`C-c C-b`和`C-c C-u`  
　　在标题头之间导航使用`outliine-mode`是可能的。使用`C-c C-n`和`C-c C-p`在下一个和前一个可视标题之间移动。类似地，`C-c C-f`和`C-c C-b`在同级可视标题之间前后移动。最终，`C-c C-u`将会在移动到一个更低的层级(即向顶层移动)。

- 整个段落或者块移动:`M-{`和`M-}`  
　　一个文本模式中“段落”的定义和Markdown-mode中的定义稍微有点不同，也就是说，因为Markdown-mode支持列表项的填充和遵守硬换行符，两个换行符分隔段落。所以，Markdown-mode覆盖通常地段落导航命令`M-{`和`M-}`。使用带有前缀`C-u`的这些命令，可以在一个完整的文本块开始或者结尾处跳转，“块”是由一个或多行分隔的。

- 通过Defun移动:`C-M-a`,`C-M-e`和`C-M-h`  
　　Defun(顶层主模式定义)通常的Emacs命令可以被用来移动。在Markdown-mode中，一个defun是一个章节。通常地，`C-M-a`将移动光标到当前defun的开始处，`C-M-e`将会移动光标到当前或者后一个defun处，而`C-M-h`将会包含完整defun区域。

　　注意，上面很多命令依赖是否Transient Mark mode开启具有不同的行为。当它由意义时，如果即时标记模式开启且区域激活，该命令应用到区域文本(例如，`C-c C-s s`使区域加粗)。对于更喜欢工作在即时标记模式之外的用户，自从Emacs22开启该模式可以通过`C-SPC C-SPC`临时开启。当没有开启该模式的情况时，很多命令对光标所在的单词或者行进行处理。  
　　当可使用的甚至在即时标记模式之外特别作用在区域上的命令具有和它们标准对应的命令相同的绑定键，但是该字母是大写的。例如，`markdown-insert-blockquote`被绑定在`C-c C-s b`上且只能作用在即时模式的区域上，然而`markdown-blockquote-region`被绑定到`C-c C-s B`上且总被应用到区域上(当非空时)。  
　　注意这些特殊区域函数在很多不是很明显的情况下是很有用的。例如，从kill环中拉出的文本在该文本的开头设置标记然后移动光标到末尾。因此，(交互)区域包含拉出的文本。所以，跟随在`C-c C-s C-b`命令后的`C-y`命令将会拉出文本然后转换为一个块引用。  
　　markdown-mode尝试在怎处理缩进上具有伸缩性。当你重复地按`TAB`。该光标将会循环遍历几个可能缩进层级，该层级可以是一行结尾按下`RET`的地方或者`TAB`地方。例如，你可能想要开始一个新的列表项，使用悬挂缩进来继续列表项，为每一个嵌套块缩进，等等。当backspace被在一行非空白符开头部分被按下，提升被类似的处理。  
　　markdown-mode支持大纲副模式以及对于atx-或者hash-style类型标题循环可见的org-mode-style。可视循环的类型具有两种：按`S-TAB`全局地在目录视图中循环(仅标题)，大纲视图(仅顶层标题)和完整文档视图之间切换。当光标位于一个标题上时，按`TAB`将会循环遍历子树的可视层级：完全折叠，孩子可见和完全可见。注意，混合哈希和下划线类型标题可能出现不期望的结果。

# 自定义
　　虽然配置不是必须的，但是还是有一些东西可以被配置。`M-x customize-mode`命令为所有可能自定义提供一个接口：

- `markdown-command`- 该命令被用来运行Markdown(默认：markdown)。该变量可以自定义用来传递命令行选项到你选择的Markdown处理程序。
- `markdown-command-needs-filename`- 如果`markdown-command`不接受标准输入设置为`t`(默认为:`nil`)。当`nil`时，`markdown-mode`将会使用标准输入(stdin)传递Markdown内容到`markdown-command`。当设置为`t`，`markdown-mode`将会传递文件名称作为最终命令行参数给`markdown-command`。注意，在后一种情况下，你只能从一个引用一个文件的缓冲区中运行`markdown-command`。
- `markdown-open-command`- 该命令被用来调用一个单独Markdown预览器，它具有直接打开Markdown源文件的能力(默认为：`nil`)。该命令带有一个单独当前缓冲区文件名称的参数被调用，一个代表性程序是Mac app [Marked2](https://itunes.apple.com/us/app/marked-2/id890031187?mt=12&uo=4&at=11l5Vs&ct=mm)，一个实时更新的Markdown预览器，它可以通过[一个简单地shell脚本被调用](http://jblevins.org/log/marked-2-command)。
- `markdown-hr-strings`- 当插入一个水平线时使用的字符串列表。不同的字符串在转换为HTML时不会区分，它们将会被转换为`<hr/>`。但是它们可以在普通文本文档中增加可视区分和样式。为了保持一些升级和降级的概念，保持这些存储从最大到最小。
- `markdown-bold-underscore`- 设置为非空值时使用两个下划线，当插入加粗文本时取代两个星号(默认为：`nil`)。
- `markdwon-italic-undersocre`- 设置为非空值时使用一个下划线，当插入斜体文本时取代星号(默认为：`nil`)。
- `markdown-asymmetric-header`- 设置为非空值时使用非对称类型标题标签，把标题标记仅仅放在标题的左边(默认为：`nil`)。
- `markdown-list-indent-width`- 当插入，提升和降级列表项时，列表缩进的深度(默认为：4)。
- `markdown-indent-function`- 用作自动缩进的函数(默认为：`markdown-indent-line`)。
- `markdown-indent-on-enter`- 设置为非空值，当回车键被按下，将自动地缩进新行(默认为：`t`)。
- `markdown-wiki-link-alias-first`- 设置为非空值，如`[[link text|PageName]]`形式来对待wiki(默认为：`t`)。当设置为空，它们将会被对待为`[[PageName|link text]]`。
- `markdown-uri-types`- 一个协议列表(例如，“http”)，为`markdown-mode`应该高亮的URIs列表。
- `markdown-enable-math`- 为LaTeX片段语法高亮(默认为：`nil`)。设置它为`t`来默认地开启数学公式支持。Math支持可以使用该函数切换。
- `markdown-css-paths`- 链接到XHTML输出中的CSS文件(默认为：`nil`)。
- `markdown-content-type`- 当设置为非空字符串时，一个`http-equiv`属性将会被包含在XHTML`<head>`块中(默认为:`""`)。如果需要，建议值为`application/xhtml+xml`或者`text/html`。查看:`markdown-coding-system`。
- `markdown-coding-system`- 用来指定在`http-equiv`属性中字符集标识(默认为：`nil`)。查看`markdown-content-type`，它必须在该变量产生效果前设置。当设置为`nil`，`buffer-file-coding-system`将会被用来自动决定编码系统字符串(当不可用时，回滚到`iso-8859-1`)。通用的设置时`utf-8`和`iso-latin-1`。
- `markdown-xhtml-header-content`- 额外的包含在XHTML`<head>`块中的内容(默认为：`""`)。
- `markdown-xhtml-standlone-regexp`- `markdown-mode`用来决定是否`markdown-command`命令输出是一个标准的XHTML文档或者一个XHTML片段的正则表达式。(默认为：`"^\\(<\\?xml\\|<!DOCTYPE\\|<html\\)"`)。如果在输出的前五行该正则表达式不匹配，`markdown-mode`假设输出是一个片段且增加一个header和footer。
- `markdown-link-space-sub-char`- 当映射wiki链接为文件名称时，一个取代空白的字符(默认为：`"-"`)。例如，使用一个下划线来兼容python Markdown扩展。在`gfm-mode`模式下，该设置为`"-"`来符合GitHub Wiki链接。
- `markdown-reference-location`- 插入引用定义的地方(默认为：`header`)。可能的位置在文档的结尾处(`end`)，当前块的后面(`immediately`)，在下一标题之前(`header`)。
- `markdown-footnote-location`- 插入脚注的地方(默认为：`end`)。设置选项和前一个一样。 
- `comment-auto-fill-only-comments`- 缓冲区本地变量且默认设置为`nil`。在编程语言模式下，当这个变量为非空时，只有注释将会被自动填充模式填充。然而，注释在Markdown文档很少且大多数用户希望文档实际的内容能够被填充。使该变量的缓存区允许`markdown-mode`覆盖当全局变量非空时包含的默认行为。
- `markdown-gfm-additional-languages`-当插入GFM代码块,除了这些在`markdown-gfm-recognized-languages`中预定义的语言外， 额外可用的语言(默认为：`nil`)。
- `markdown-gfm-use-electric-backquote`- 当反引号被输入三次时，使用`markdown-electric-backquote`来交互地插入GFM代码块。(默认为：`t`)。
- `markdown-make-gfm-checkboxes-buttons`- 是否把GitHub风格的Markdown风格任务列表（复选框）应该变成可与鼠标-1或RET进行切换的按钮。如果非空(默认)，然后按钮被开启。它工作在`markdown-mode`以及`gfm-mode`。

　　此外，用作语法高亮的皮肤可以通过发布`M-x customize-group RET markdown-faces`命令来修改为你喜欢的风格，或者在自定义屏幕模式的底部通过"Markdown Faces"链接。
# 扩展
　　除了支持基本Markdown语法，Markdown-mode默认同样包括对`[[Wiki Links]]`语法高亮。当光标位于一个wiki链接时，通过`C-c C-o`Wiki链接可以打开。使用`M-p`和`M-n`来快速跳转到前一个和下一个链接(包括其他类型的链接)。`[[link text|PageName]]`别名或者管道wiki链接同样被支持。因为一些wikis反转这些构件，设置`markdown-wiki-link-alias-first`为nil来处理`[[PageName|link text]]`形式。默认地，Markdown模式仅仅搜索当前目录下的目标文件。连续的父目录搜索(如在[Ikiwiki](https://ikiwiki.info/))通过设置`markdown-wiki-link-search-parent-directories`为一个非空值来开启。  
　　[SmartPants](http://daringfireball.net/projects/smartypants/)通过自定义`markdown-command`来支持。如果你安装`SmartyPants.pl`在`/usr/local/bin/smartypants`，然后你可以设置`markdown-command`为`"markdown | smartypants"`。你可以通过使用`M-x customsize-group markdown`或者把如下的设置放到你的`.emacs`文件下来做到：

```
(setq markdown-command "markdown | smartypants")
```

　　对LaTeX中数学表达式(仅仅由`$..$`,`$$..$$`或者`\[..\]`标记的表达式)的语法高亮可以通过设置`markdown-enable-math`为非空值来开启，或者通过自定义或者通过把`(setq markdown-enable-math t)`放到设置文件`.emacs`中，然后重新启动Emacs或者调用`markdown-reload-extensions`。

# GitHub风格Markdown(GFM)
　　一个GitHub风格Markdown模式，`gfm-mode`同样可用。GitHub实现稍微不同的标准Markdown，在该模式下，它支持的东西如，在单词内的下划线具有不同行为，自动URLs链接，删除文本线和带有可选语言关键字的围栏代码块。  
　　上面GFM特殊功能被应用到`README.md`文件，wiki页面和其它位于GitHub仓库的Markdown格式文件。GitHub同样为在网站上写作开启额外功能(为发布，pull requests,消息等等)进一步扩展GFM。这些功能包括任务列表(checkboxes)，对应于硬换行符的换行符，引用发布和提交的自动链接引用，wiki链接等等。虽然任务列表不是[正确GFM](http://github.github.com/github-flavored-markdown/)一部分，但是为了使事情不是太混乱，自从2014年，它们被渲染到所有站点内仓库中Markdown文档。这些额外扩展不同程度的被`markdwon-mode`和`gfm-mode`所支持，描述如下：

- **URL自动链接**：`markdown-mode`和`gfm-mode`都支持不带尖括号的URLs高亮。
- **在一个单词中多个下划线**:你必须开启`gfm-mode`切换到支持单词内多个下划线。在该模式下变量名称例如`a_test_variable`将不会触发强调(斜体)。
- **包围代码块**：代码块使用带有可选编程语言关键字的反引号引用，在`markdown-mode`和`gfm-mode`中被高亮。它们可以使用`C-c C-s P`。如果具有一个活动区域，区域中文本将会被放在代码块中。
- **删除线**：文本删除线只在`gfm-mode`下被支持且可以使用`C-c C-s d`插入。遵循其它类型绑定键的助记符，单词`d`暗合HTML标记的`<del>`
- **任务列表**：GFM任务列表在`markdown-mode`和`gfm-mode`中当`markdown-make-gfm-checkboxs-buttons`被设置为一个非空值时(默认设置为t)，将会被渲染为checkboxs(Emacs按钮)。该checkboxes通过`mouse-1`或者在按钮上按`RET`触发。
- **Wiki链接**：普通的wiki链接在`Markdown-mode`下被支持，但是在`gfm-mode`下特别是它们位于GitHub中被特殊对待：文件名称中的空格被连字符替代且文件名的第一个字符被大写。例如，`wiki link`将会映射到一个带有和当前文件相同扩展名且命名为`Wiki-link`的文件。
- **Newlines**：`markdown-mode`和`gfm-mode`不会对换行符行为做任何特殊的操作。如果你使用`gfm-mode`大多是为了注释或者在GitHub站点上发表文章，这时换行符很重要且对应硬换行符，然后你可以想要为缓冲区的line wrapping开启`visual-line-mode`。使用如下的`gfm-mode-hook`你可以做到这些：

``` el
;; Use visual-line-mode in gfm-mode
(defun my-gfm-mode-hook ()
  (visual-line-mode 1))
(add-hook 'gfm-mode-hook 'my-gfm-mode-hook)
```
- **预览**：GFM特殊的预览通过设置`markdown-command`为[Docter](https://github.com/alampros/Docter)。它同样把`markdown-open-command`配置为[Marked2](https://itunes.apple.com/us/app/marked-2/id890031187?mt=12&uo=4&at=11l5Vs&ct=mm)

