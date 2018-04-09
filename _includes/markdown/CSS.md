<h2 id="philosophy">Philosophy</h2>

At 10up, we value content and the experience users will have reading it. We write CSS with this in mind and don't sacrifice our clients' most important assets over the latest, shiniest, half-supported CSS features just for the sake of using them. CSS should help enhance content, not bury it under "cool" distractions.
在10up，我们珍视内容和用户的阅读体验。                                    我们将css写在这个思想上，不要以用户最重要的资源为牺牲品来使用它，                                                                                                               css应该有助于提高内容，而不是把它埋在“酷”的干扰之下。
Our websites are built mobile first, using performant CSS. Well-structured CSS yields maintainability and better collaboration which ultimately yields better client experiences.
我们的网站是先建立移动第一，          使用performant。结构化的css能够产生可维护性和更好的协作，最终会带来更好的客户体验。
<h2 id="syntax-formatting">Syntax and Formatting {% include Util/top %}</h2>

Syntax and formatting are keys to a maintainable project. By keeping our code style consistent, we not only help ourselves debug faster but we're also lessening the burden on those who will have to maintain our code (maybe ourselves too!).
语法和格式是可维护项目的关键。                             通过保持代码样式一致，我们不仅帮助自己调试速度更快，而且我们还减少了那些必须维护代码的负担(也许就是我们自己)。
### CSS Syntax

CSS syntax is not strict and will accept a lot of variations, but for the sake of legibility and fast debugging, we follow basic code styles:
css语法不严格，              并且会接受很多变体，               但是为了可读性和快速调试，                           我们遵循基本代码样式：
* Write one selector per line
每行写入一个选择器
* Write one declaration per line
每行写一个声明
* Closing braces should be on a new line
关闭大括号应该放在一个新行上
Avoid:
避免
```css
.class-1, .class-2,
.class-3 {
width: 10px; height: 20px;
color: red; background-color: blue; }
```

Prefer:
更喜欢
```css
.class-1,
.class-2,
.class-3 {
  width: 10px;
  height: 20px;
  color: red;
  background-color: blue;

}
```

* Include one space before the opening brace
在打开支架前包括一个空格
* Include one space before the value
包含值之前的一个空格
* Include one space after each comma-separated values
在每个逗号分隔值之后包含一个空格
Avoid:
避免
```css
.class-1,.class-2{
  width:10px;
  box-shadow:0 1px 5px #000,1px 2px 5px #ccc;
}
```

Prefer:
更喜欢
```css
.class-1,
.class-2 {
  width: 10px;
  box-shadow: 0 1px 5px #000, 1px 2px 5px #ccc;
}
```

* Try to use lowercase for all values, except for font names
尝试使用小写字母来计算所有值，除了字体名称外
* Zero values don't need units
零值不需要单元
* End all declarations with a semi-colon, even the last one, to avoid errors
用分号，甚至最后一个结束所有声明，以避免错误
* Use double quotes instead of single quotes
使用双引号代替单引号
Avoid:
避免：
```css
section {
  background-color: #FFFFFF;
  font-family: 'Times New Roman', serif;
  margin: 0px
}
```

Prefer:
更喜欢：
```css
section {
  background-color: #fff;
  font-family: "Times New Roman", serif;
  margin: 0;
}
```

If you don't need to set all the values, don't use shorthand notation.
如果您不需要设置所有值，请不要使用速记符号。
Avoid:
避免：
```css
.header-background {
  background: blue;
  margin: 0 0 0 10px;
}
```

Prefer:
更喜欢：
```css
.header-background {
  background-color: blue;
  margin-left: 10px;
}
```

### Declaration ordering

Declarations should be ordered alphabetically or by type (Positioning, Box model, Typography, Visual). Whichever order is chosen, it should be consistent across all files in the project.
声明应按字母顺序或按类型(定位、盒模型、排版、视觉)顺序排列。无论选择哪个顺序，它都应该在项目中所有文件之间保持一致。
If you're using Sass, use this ordering:
如果您使用Sass，请使用以下命令：
1. @extend
@扩展
2. Regular styles (allows overriding extended styles)
正则样式(允许重写扩展样式)
3. @include (to visually separate mixins and placeholders) and media queries
@包括(可视化地将mixins和占位符)分开，以及媒体查询
4. Nested selectors
嵌套选择器
### Nesting

Nesting has changed the lives of many, but like everything in life, abusing good things will ultimately be bad. Nesting makes the code more difficult to read and can create confusion. Too much nesting also adds unnecessary specificity, forcing us to add the same or greater specificity in overrides. We want to avoid selector nesting and over-specificity as much as possible.
嵌套改变了许多人的生活，                但是像生活中的一切一样，滥用好东西最终会变得糟糕。                        嵌套使得代码更难读取，并且会产生混乱。                                    过多嵌套也增加了不必要的特殊性，迫使我们在重写中添加相同或更大的特定性。                                               我们想尽可能避免选择器嵌套和超特定性。
If you're using PostCSS or Sass **nesting is required** in the following cases, because it will make the code easier to read:
如果你使用的是PostCSS或Sass嵌套是必需的在下列情况下，因为它将使代码更易于读取：
* pseudo-classes
伪类
* pseudo-elements
伪元素
* component states
组件状态
* media queries
媒体查询
### Selector Naming

Selectors should be lowercase, and words should be separated with hyphens. Please avoid camelcase, but underscores are acceptable if they’re being used for [BEM](https://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) or another syntax pattern that requires them. The naming of selectors should be consistent and describe the functional purpose of the styles they’re applying.
选择器应该是小写的，            并且单词应该与连字符分隔。                   请避免使用CamelCase，   但是下划线是可以接受的，        如果它们被用于边界元或者另一个需要它们的语法模式。                                                              选择器的命名应该是一致的，并描述它们所应用的样式的功能目的。
Avoid:
避免：
```css
.btnRed {
	background-color: red;
}
```

Prefer:
更喜欢：
```css
.btn-warning {
	background-color: red;
}
```

For components that could possibly conflict with plugins or third-party libraries, use vendor prefixes. Don’t use names that can be blocked by adblockers (e.g. “advertisement”). When in doubt, you can check a class name against [this list](https://easylist-downloads.adblockplus.org/easylist.txt) to see if it's likely to be blocked.
对于可能与插件或第三方库冲突的组件，请使用供应商前缀。                                                    不要使用可以被adblockers屏蔽的名称(例如“广告”)。                                                                                当您怀疑时，您可以检查类名。这份清单看看它是否可能被屏蔽。
<h2 id="documentation">Documentation {% include Util/top %}</h2>

Code documentation serves two purposes: it makes maintenance easier and it makes us stop and think about our code. If the explanation is too complex, maybe the code is overly complex too. Documenting helps keep our code simple and maintainable.
代码文档提供了两个目的：                 它使维护更容易，它使我们停止并思考我们的代码。                               如果解释过于复杂，那么代码也太复杂了。                                    文档记录有助于维护代码简单且易于维护。
### Commenting

We follow WordPress official commenting standards. Do not hesitate to be very verbose with your comments, especially when documenting a tricky part of your CSS. Use comment blocks to separate the different sections of a partial, and/or to describe what styles the partial covers:
我们遵循wordpress官方评论标准。                   不要因为你的注释冗长有顾虑，                              尤其是当你记录你css中棘手的部分时。                     使用注释块分隔部分的不同部分，                                       并/或描述部分覆盖的样式：
```css
/**
 * Section title
 *
 * Description of section
 */
```

For single selectors or inline comments, use this syntax:
对于单个选择器或内联注释，请使用以下语法：
```css
/* Inline comment */
```

Make sure to comment any complex selector or rule you write. For example:
请确保注释任何复杂的选择器或您编写的规则。例如：
```css
/* Select list item 4 to 8, included */
li:nth-child(n+4):nth-child(-n+8) {
	color: red;
}
```

<h2 id="performance">Performance {% include Util/top %}</h2>

Let's be honest, CSS "speed" and performance is not as important as PHP or JavaScript performance. However, this doesn't mean we should ignore it. A sum of small improvements equals better experience for the user.
老实说，         css“速度”和性能不如php或javascript性能那么重要。                                 然而，这并不意味着我们应该忽略它。                一笔小的改进等于用户的更好的经验。
Three areas of concern are [network requests](#network-requests), [CSS specificity](#css-specificity) and [animation](#animations) performance.
三个令人关切的领域是网络请求，css特异性和动画表演。
Performance best practices are not only for the browser experience, but for code maintenance as well.
性能最佳实践不仅适用于浏览器体验，                                   而且还适用于代码维护。
### Network Requests

* Limit the number of requests by concatenating CSS files and encoding sprites and font files to the CSS file.
通过将css文件和编码精灵和字体文件连接到css文件中来限制请求的数量。
* Minify stylesheets
minify样式表
* Use GZIP compression when possible
当可能时使用GZIP压缩使用php或/和javascript构建过程自动化这些任务。
Automate these tasks with a PHP or/and JavaScript build process.

### CSS Specificity

Stylesheets should go from less specific rules to highly specific rules. We want our selectors specific enough so that we don't rely on code order, but not too specific so that they can be easily overridden.
Stylesheets应该从较少特定规则到高度特定的规则。                            我们希望我们的选择器足够具体，以便我们不依赖代码顺序，                     但不是太具体，以便它们可以轻松地重写。
For that purpose, **classes** are our preferred selectors: pretty low specificity but high reusability.
为此目的，class是我们首选的选择器：                         非常低的特异性，但可重用性高。
Avoid using `!important` whenever you can.

Use [efficient selectors](https://csswizardry.com/2011/09/writing-efficient-css-selectors/).

Avoid:
避免：
```css
 div div header#header div ul.nav-menu li a.black-background {
  background: radial-gradient(ellipse at center,  #a90329 0%,#8f0222 44%,#6d0019 100%);
}
```

### Inheritance

Fortunately, many CSS properties can be inherited from the parent. Take advantage of inheritance to avoid bloating your stylesheet but keep [specificity](#css-specificity) in mind.
幸运的是，   许多css属性可以从父类继承。                            利用继承来避免膨胀样式表，但保持专一性在心里。
Avoid:
避免
```css
.sibling-1 {
	font-family: Arial, sans-serif;
}
.sibling-2 {
	font-family: Arial, sans-serif;
}
```

Prefer:
更喜欢：
```css
.parent {
	font-family: Arial, sans-serif;
}
```

### Reusable code

Styles that can be shared, should be shared (aka DRY, Don’t Repeat Yourself). This will make our stylesheets less bloated and prevent the browser from doing the same calculations several times. Make good use of Sass placeholders. (also see [Inheritance](#inheritance))
可以共享的样式应该共享(也可以是干的，不要重复)。                                 这将使样式表变得更加臃肿，并且防止浏览器多次进行相同的计算。                                                          充分利用Sass占位符。(也参见继承)
### CSS over assets

Don't add an extra asset if a design element can be translated in the browser using CSS only. We value graceful degradation over additional HTTP requests.
如果设计元素只能使用css来翻译，则不要添加额外的资产。我们对额外的http请求进行了优雅的退化。
Very common examples include gradients and triangles.
非常常见的例子包括渐变和三角形。
### Animations

It's a common belief that CSS animations are more performant than JavaScript animations. A few articles aimed to set the record straight (linked below).
通常认为css动画比javascript动画更具有现实意义。                                           一些文章旨在将记录直接设置(链接如下)。
* If you're only animating simple state changes and need good mobile support, go for CSS (most cases).
如果您只是动画简单的状态更改并且需要良好的移动支持，请去使用css(大多数情况)。
* If you need more complex animations, use a JavaScript animation framework or requestAnimationFrame.
如果需要更复杂的动画，请使用javascript动画框架或requestAnimationFrame。
Limit your CSS animations to 3D transforms (translate, rotate, scale) and opacity, as those are aided by the GPU and thus smoother. Note that too much reliance on the GPU can also overload it.
限制您的css动画到3d转换(翻译，旋转，尺度)和透明度，因为这些是由gpu帮助，从而更加平滑。注意，过多依赖gpu也可以重载它。
Avoid:
避免
```css
#menu li{
  left: 0;
  transition: all 1s ease-in-out;
}
#menu li:hover {
  left: 10px
}
```

Always test animations on a real mobile device loading real assets, to ensure the limited memory environment doesn't tank the site.
总是测试动画在真实移动设备加载真实资源，以确保有限的内存环境不会出现坦克站点。
Articles worth reading:
* [CSS animations performance: the untold story](https://greensock.com/css-performance)
* [Myth Busting: CSS Animations vs. JavaScript](https://css-tricks.com/myth-busting-css-animations-vs-javascript/)
* [CSS vs. JS Animation: Which is Faster?](https://davidwalsh.name/css-js-animation)
* [Why Moving Elements With Translate() Is Better Than Pos:abs Top/left](https://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/)
* [CSS vs JavaScript Animations](https://developers.google.com/web/fundamentals/look-and-feel/animations/css-vs-javascript?hl=en)
* [requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)

<h2 id="responsive-websites">Responsive websites {% include Util/top %}</h2>

We build our websites mobile first. We do not rely on JavaScript libraries such as `respond.js` as it does not work well in certain environments. Instead, we leverage a natural, movile-first build process and allow sites gracefully degrade.
我们首先建立我们的网站移动。我们不依赖于javascript库，例如respond.js因为它在某些环境中工作不太好。                                                   相反，我们使用了一个自然的、基于movile的构建过程，允许站点优雅地降级。
### Min-width media queries

A responsive website should be built with min-width media queries. This approach means that our media queries are consistent, readable and minimize selector overrides.
一个响应网站应该用最小宽度的媒体查询来构建。                        这种方法意味着我们的媒体查询是一致的、                       可读的和     最小化选择器重写。
* For most selectors, properties will be added at later breakpoints. This way we can reduce the usage of overrides and resets.
对于大多数选择器，     属性将在以后断点中添加。                             这样我们就可以减少重写和重置的用法。
* It targets the least capable browsers first which is philosophically in line with mobile first — a concept we often embrace for our sites
它的目标是最缺乏能力的浏览器，首先是与移动第一-我们经常为我们网站所拥抱的概念。
* When media queries consistently "point" in the same direction, it makes it easier to understand and maintain stylesheets.
当媒体查询一致地“点”在同一个方向时，它使得理解和维护样式表变得更加容易。
Avoid mixing min-width and max-width media queries.
避免混合最小宽度和最大宽度媒体查询。
### Breakpoints

Working with build tools that utilize Sass or PostCSS processing, we can take advantages of reusability and avoid having an unmaintainable number of breakpoints. Using variables and reusable code blocks we can lighten the CSS load and ease maintainability.
使用Sass或PostCSS处理的构建工具，                                  我们可以利用重用性和避免具有多个断点数的优点。使用变量和可重用代码块可以减轻css负载，减轻维护性。
### Media queries placement

In your stylesheet files, nest the media query within the component it modifies. **Do not** create size-based partials (e.g. `_1024px.(s)css`, `_480px.(s)css`): it will be frustrating to hunt for a specific selector through all the files when we have to maintain the project. Putting the media query inside the component will allow developers to immediately see all the different styles applied to an element.
在样式表文件中，                       将媒体查询嵌套在它修改的组件中。              不要创建基于大小的部分(例如：_1024px.(s)css，_480px.(s)css：                   当我们需要维护项目时，                                  通过所有文件搜索特定的选择器将是令人沮丧的。                 将媒体查询放在组件内，将允许开发人员立即看到应用于元素的所有不同样式。
Avoid:
避免：
```css
@media only screen and (min-width: 1024px) {
	@import "responsive/1024up";
}
.some-class {
	color: red;
}
.some-other-class {
	color: orange;
}
@media only screen and (min-width: 1024px) {
	.some-class {
		color: blue;
	}
}
```

Prefer:
更喜欢：
```css
.some-class {
	color: red;
	@media only screen and (min-width: 1024px) {
		color: blue;
	}
}
.some-other-class {
	color: orange;
}
```

### IE8 and older browser support

We prefer showing a fixed-width non-responsive desktop version to older IE users rather than showing a mobile version.
我们更喜欢向老ie用户展示一个固定宽度、不响应的桌面版本，                           而不是显示移动版本。
* Use a feature detection to target older browsers.
使用特征检测来针对旧浏览器。
* Load a different stylesheet for older browsers.
为旧浏览器加载不同样式表。

<h2 id="frameworks">Frameworks {% include Util/top %}</h2>

### Grids

Our preference is not to use a 3rd party grid system, use your best judgement and keep them simple! All too often we are faced with a design that isn’t built on a grid or purposefully breaks a loosely defined grid. Even if the designer had a grid in mind, there are often needs that require more creative solutions. For example: fixed-width content areas to accommodate advertising.
我们的首选是不要使用第三方的网格系统，                使用你最好的判断，并保持他们简单！                      我们经常面临一个不是建立在网格上，或者有意地打破松散定义的网格的设计。                                         即使设计师们头脑中有一个网格，            往往需要更多创造性的解决方案。                               例如：固定宽度的内容区域以适应广告。
Sometimes a more complex grid system is warranted and leveraging a 3rd party library will gain some efficiency. However, keep in mind that by adopting a grid system you are forcing all future collaborators on the project to learn this system.
有时，更复杂的网格系统是有保障的，                 并且利用第三的政党图书馆会获得一些效率。                        但是，请记住，通过采用网格系统，您将迫使项目中的所有未来合作者学习该系统。
### Resets

As of [August 13th, 2015](https://10up.com/blog/2015/sponsoring-sanitize-css/) 10up has taken stewardship of [sanitize.css](https://github.com/10up/sanitize.css), making it our primary tool for resets. Although we can still consider using [normalize.css](https://necolas.github.io/normalize.css/).
截至2015年月十三日10up已经管理了n.sanitize.css                                                                                                                      使它成为我们重置的主要工具。虽然我们仍然可以考虑使用n.normalize.css。
## Further reading {% include Util/top %}

[CSS: Just Try and Do a Good Job](https://css-tricks.com/just-try-and-do-a-good-job/)
