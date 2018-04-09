## Philosophy
At 10up, we aim to create the best possible experience for both our clients and their customers; not for the sake of using cool, bleeding edge technologies that may not have widespread browser support. Our markup embodies this approach.
在10up， 我们的目标是创造尽可能好的方法给我们的客户和他们的顾客                                    不是为了使用更酷，               边缘技术并不是广泛的浏览器支持，我们的Markup收录了这种方法
### Principles
Markup is intended to define the structure and outline of a document and to offer semantic structure for the document's contents. Markup should not define the look and feel of the content on the page beyond the most basic structural concepts such as headers, paragraphs, and lists.
Markup是用来预先定义文档的结构和大纲的还有文章内容的语意结构                                                                        Markup不应该是定义文章页面的外观和感觉的，因为超出了基础的文档结构例如页眉，段落，还有列表
At 10up, our projects are often large and ongoing. As such, it's imperative that we engineer projects to be maintainable. From a markup perspective, we do this by adhering to the following principles:
在10up,我们的工程经常是极大而且是持续的，因此，                我们设计的工程必须具有可维护性，                             站在Markup的角度上                                      我们需要遵循以下几点
### Semantic
At 10up, we pride ourselves in writing clean, semantic markup. Semantic markup can be defined as: "the use of HTML markup to reinforce the semantics, or meaning, of the information in web pages rather than merely to define it's presentation or look. Semantic HTML is processed by traditional web browsers as well as by many other user agents. CSS is used to suggest its presentation to human users" (definition from Wikipedia -[https://en.wikipedia.org/wiki/Semantic_HTML](https://en.wikipedia.org/wiki/Semantic_HTML)).
在10up中，我们为自己的书写整洁、语义标记而自豪。语义标记我们可以定义为：                            “使用HTML标记加强网页”语义和意义，                             而不仅仅是定义它的呈现外观                                                               HTML语义是由传统的web浏览器以及其他用户代理处理                                                css用于向人类用户推荐它的演示文稿
Semantic elements are elements with clearly defined meaning to both the browser and the developer. Elements like ```<header>```, ```<nav>```, ```<footer>```, or ```<article>``` do a much better job of explaining the content that is contained within the element than ```<span>``` or ```<div>```. This does not mean that we do not use ```<div>```s in our markup, only that we prefer the right tool (or in this case semantic element) for the job.
语义元素是对浏览器和开发者由明确定义意义的元素                                                      像这些元素<header>,<nav>, <footer>, 和<article>比<span>,<div>能更好的解释包含在元素内的网页内容                                                                                                       这并不意味着在我们的标记中不用<div>，                              只要对于我们的工作中选择到自己喜欢的是正确的工具（或者本例子中的语义元素）

### Minimal &amp; Valid
Websites should be written using the least amount of markup that accomplishes the goal. In the interest of engineering maintainable projects, it's imperative that two completely different types of readers are accounted for: humans and browsers. Writing minimal markup makes it easier for developers to read and understand in a code editor. Valid markup is easier for browsers to process.
网页编写应该用最少量的标记来完成已达到目的。为了工程项目的可维护性                                                                                                                 必须把两种不同类型的读者考虑为                  人类和浏览器。使用少量的标记制作它                    以便于开发者阅读和理解并编辑它                                  有效标记对浏览器处理也更容易
We test our markup against the [W3C validator](https://validator.w3.org/) to ensure that it is well formed and provides a fairly consistent experience across browsers.
我们相对于W3C validator 来对我们的标记进行测试来保证                                       它有好的框架和提供持续的跨浏览器体验

### Optimize Readability
At 10up, we often work with large codebases. As such, it's important to optimize markup for human readability. This allows developers to quickly rotate in and out of projects, eases onboarding processes, and improves code maintainability.
在10up中，我们的工作京城伴随着大型的codebases,  所以,优化标记便于人们阅读是非常重要的                               这就允许了开发者在项目内和项目外快速转换                            简化入职培训和提高代码的可维护性
Always use tabs for indentation. Doing this allows developers to set their editor preferences for tab width.
总是使用制表符缩进，              这样做允许开发者给他们的制表符宽度设置编辑器首选项
When mixing PHP and HTML together, indent PHP blocks to match the surrounding HTML code. Closing PHP blocks should match the same indentation level as the opening block. Similarly, keep PHP blocks to a minimum inside markup. Doing this turns the PHP blocks into a type of tag themselves. Use colon syntax for PHP loops and conditionals so that it's easier to see when a certain loop ends within the block of markup.
当PHP和HTML混合在一起的时候，       缩进PHP代码块来匹配周围的HTML代码。                     关闭的PHP代码块应该相同匹配打开的代码块缩进层级同样的，           保持PHP代码块标记少量这样做    将PHP代码块转换为一个标签类型为PHP循环和条件使用冒号语法，这样，更容易用代码块标记看出循环的结束                                                                                                                                                     

#### Examples

Bad:
坏的：
```php
<ul>
<?php
foreach( $things as $thing ) {
  echo '<li>' . esc_html( $thing ) . '</li>';
}
?>
</ul>
```

Good:
好的：
```php
<ul>
    <?php foreach( $things as $thing ) : ?>
        <li><?php echo esc_html( $thing ); ?></li>
    <?php endforeach; ?>
</ul>
```

### Declaring the Proper Doctype
For all new projects we use HTML5 with the following doctype: ```<!DOCTYPE html>```
对于所有用HTML新建的工程我们用<!DOCTYPE html>来宣告它的文本类型
### Lowercase Tags
Although HTML is not case sensitive, using lowercase tags is our preferred pattern. Again, we're emphasizing readability and consistency.
尽管HTML对于大小写不敏感，但用小写标签是我们首选的模式，再者，我们强调可读性和统一性
#### Examples

Bad:
坏的：
```html
<DIV class="featured-image"></DIV>
```
Good:
好的
```html
<div class="featured-image"></div>
```

### Avoid Unnecessary Presentational Markup
As part of our mission to write clean, semantic markup, avoid writing unnecessary presentational markup. Markup should always dictate what the content is, and CSS should dictate how the content looks. Mixing these two concerns makes maintaining code more difficult.
作为我们书写整洁任务的一部风，                    语义标记书写应该避免不必要的显示标记，                      标记应该总是指向网站内容的，                          css应该支配网站内容如何显示                               二者混在一起，是的代码维护变得更加困难                                                                                                                                        
By using Sass, we're able to better extend our classes within our CSS, allowing us to easily separate concerns between markup and styling. For example, we can ```@extend``` or ```@include``` our grid column sizes as well as media queries to modify the behavior at different sizes so that styles live separately from markup.
使用Sass，我们尽可能最佳继承我们的css类                                   允许我们简易的分离二者之间容易产生冲突的标记和样                      例如我们可以用坐标纵列以及                      媒体查询来修改不同尺寸的行为                                                                       从而使得样式和标记分开运行                              
                                                                                                                                        
                                                                                                                                          
                                                                                                                                           上下文修饰符类当然是可以接受和鼓励的

This is not to say that multiple classes on an element are unacceptable. Contextual modifier classes are certainly acceptable and encouraged.
 这并不是说在元素上多重类是不可接受的                                        上下文修饰符类当然是可以接受和鼓励的
#### Examples 
Bad:

```html
<div class="col-lg-6 col-md-6 col-sm-9 col-xs-12 featured-image"></div>
```
Good:

```html
<nav class="primary-nav"></nav>
<nav class="primary-nav open"></nav>
```

### Schema.org Markup
[Schema.org](https://schema.org) is the result of collaboration between Google, Bing, Yandex, and Yahoo! to provide the information their search engines need to understand content and provide the best search results possible. Adding Schema.org data to your HTML provides search engines with structured data they can use to improve the way pages display in search results.
Schema.org是                                                           google、bing、Yandex和yahoo之间协作的结果，提供他们搜索引擎需要了解内容并提供最佳搜索结果的信息。                                                                将Schema.org数据添加到html中，可以提供搜索引擎，它们可以使用结构化数据来改进搜索结果中页面显示的方式。
For example, if you've ever searched for a restaurant and found that it had star reviews in its search results, this is a product of Schema.org and rich snippets.
例如，如果你曾经搜索过一家餐馆，                             发现它在搜索结果中有明星评论，                           这是一个中的和丰富片段的产物。
Schema.org data is intended to tell the search engines what your data *means*, not just what it says.
Schema.org数据是用来告诉搜索引擎你的数据手段不仅仅是它说的。
To this end, we use Schema.org data where relevant and reasonable to ensure that our clients have the best search visibility that we can provide.
为此，          我们使用了相关和合理的数据，确保我们的客户拥有最好的搜索能见度。
Schema.org data can be provided in two forms: "microdata" markup added to a page's structure or a JSON-LD format. Google prefers developers adopt JSON-LD, which isolates the data provided for search engines from the markup meant for user agents. Even though the JSON-LD spec allows linking to data in an external file, search engines currently only parse JSON-LD data when it appears within a `<script type="application/ld+json">` tag, as shown below.
Schema.org可提供两个表单：                   “微数据”标记添加到页面的结构或json-ld格式。                            google更喜欢开发人员采用JSON，            它将提供给搜索引擎的数据从用于用户代理的标记中分离出来。                                       尽管在规范允许链接到外部文件中的数据，但当前搜索引擎仅在一种中解析json-ld数据。<script type="application/ld+json">标签，如下所示。
Schema.org markup should be validated against the [Google Structured Data Testing Tool](https://search.google.com/structured-data/testing-tool/u/0/).
应对Schema.org标记进行验证。google结构化数据测试工具。
For examples of Schema markup on components, check out the [10up WordPress Component Library](https://10up.github.io/wp-component-library/)
对于组件中的架构标记示例，请检查10up组件库
<h2 id="html5-structural-elements">HTML5 Structural Elements {% include Util/top %}</h2>
HTML5 structural elements allow us to create a more semantic and descriptive codebase and are used in all of our projects. Instead of using ```<div>```s for everything, we can use HTML5 elements like ```<header>```, ```<footer>```, and ```<article>```. They work the same way, in that they're all block level elements, but improve readability and thus maintainability.
html 5结构元素允许我们创建一个更语义和描述性的代码库，                                      并被用于我们所有的项目。而不是使用<div>对于所有的东西，                             我们可以使用html 5元素，比如<header>，<footer>，和<article>。它们的工作方式相同，因为它们都是块级元素，                                                   但提高可读性，从而维护。
There are a few common pitfalls to avoid with HTML structural elements. Not everything is a ```<section>```. The element represents a generic document or application section and should contain a heading.
对于html结构元素来说，避免使用一些常见的陷阱。                               不是一切都是<section>。元素表示泛型文档或应用程序节，                                 并且应该包含标题。
Another misconception is that the ```<figure>``` element can only be used for images. In fact, it can be used to mark up diagrams, SVG charts, photos, and code samples.
另一个误解是：<figure>元素只能用于图像。                                                实际上，它可以用来标记图表、svg图表、照片和代码样本。
Finally, not all groups of links on a page need to be in a nav element. The ```<nav>``` element is primarily intended for sections that consist of major navigation blocks.
最后，并非所有页面上的链接组都需要在导航元素中。                            |||<nav>元素主要用于由主导航块组成的部分。
### Examples

Bad:
坏的：
```html
<section id="wrapper">
    <header>
        <h1>Header content</h1>
    </header>
    <section id="main">
        <!-- Main content -->
    </section>
    <section id="secondary">
        <!-- Secondary content -->
    </section>
</section>
```

Good:
好的：
```html
<div class="wrapper">
    <header>
        <h1>My super duper page</h1>
        <!-- Header content -->
    </header>
    <main role="main">
        <!-- Page content -->
    </main>
    <aside role="complementary">
        <!-- Secondary content -->
    </aside>
</div>
```

### Type attribute on script and style elements is not necessary in HTML5
Since all browsers expect scripts to be JavaScript and styles to be CSS, you don't need to include a type attribute. While it isn't really a mistake, it's a best practice to avoid this pattern.
因为所有浏览器都希望脚本是javascript和样式为css，                           所以不需要包含类型属性。                      虽然这并不是一个错误，           但避免这种模式是最好的做法。
Bad example:
坏的例子：
```html
<link type="text/css" rel="stylesheet" href="css/style.css" />
<script type="text/javascript" src="script/scripts.js"></script>
```
Good example:
好的例子：
```html
<link rel="stylesheet" href="css/style.css">
<script src="script/scripts.js"></script>
```

<h2 id="classes-ids">Classes &amp; IDs {% include Util/top %}</h2>
In order to create more maintainable projects, developers should use classes for CSS and IDs for JavaScript. Separating concerns allows markup to be more flexible without risking breaking both styles and any JavaScript that may be attached to the element on which someone is working.
为了创建更易于维护的项目，                       开发人员应该为javascript使用css和IDs类。                       分离关注允许标记变得更加灵活，           而无需冒险打破两种样式和任何可能附加到某个角色上的javascript的任何javascript。
When using JavaScript to target specific elements in your markup, prefix the ID of the element that is being targeted with `js-`. This indicates the element is being targeted by JavaScript for your future self as well as other developers that may work on the project.
当使用javascript来针对标记中的特定元素时，将所针对的元素的id前缀js-。这表明，javascript为您将来的自我以及可能在项目上工作的其他开发人员提供了javascript的目标。
Example:
例子
```html
<nav id="js-primary-menu" class="primary-menu"></nav>
```

### Avoid using inline styles or JavaScript
避免使用内联样式或javascript
These are not easily maintainable and can be easily lost or cause unforeseen conflicts.
这些情况不易维护，容易丢失或导致不可预见的冲突。
<h2 id="accessibility">Accessibility {% include Util/top %}</h2>
It's important that our clients and their customers are able to use the products that we create for them. Accessibility means creating a web that is accessible to all people: those with disabilities and those without. We must think about people with visual, auditory, physical, speech, cognitive and neurological disabilities and ensure that we deliver the best experience we possibly can to everyone. Accessibility best practices also make content more easily digestible by search engines. Increasingly, basic accessibility can even be a legal requirement. In all cases, an accessible web benefits everyone.
重要的是，           我们的客户和客户能够使用我们为他们创造的产品。                                          无障碍意味着创建一个人人都能接触的网络：                                残疾人和没有残疾人的网站。                    我们必须思考那些视觉、听觉、身体、言语、认知和神经障碍的人，                       并确保我们能够给每个人提供最好的体验。                                                                    可访问性最佳实践也使搜索引擎更容易消化内容。                                               越来越多的基本可访问性甚至可能成为法律要求。                          在所有情况下，一个可访问的网络都会使每个人受益。
### Accessibility Standards
At a minimum, all 10up projects should pass [WCAG 2.0 Level A Standards](https://www.w3.org/WAI/intro/wcag). A baseline compliance goal of Level A is due to [WCAG guideline 1.4.3](https://www.w3.org/WAI/WCAG20/quickref/#visual-audio-contrast-contrast) which requires a minimum contrast ratio on text and images, as 10up does not always control the design of a project. 
至少，         所有的项目都应该通过2.0级a标准。                                                                a级的基线遵守目标是由于WCAG准则1.4.3                                                                                                             这需要在文本和图像上进行最小对比度比率，因为10up并不总是控制项目的设计。
For design projects and projects with a global marketplace (companies with entities outside the US), Level AA should be the baseline goal. The accessibility level is elevated for global markets to properly comply with [EU Functional Accessibility Requirements](http://mandate376.standards.eu/standard/technical-requirements/#9), which aligns closely with WCAG 2.0 Level AA. Having direct access to the designer also allows for greater accessibility standards to be achieved.
对于拥有全球市场的设计项目和项目(与美国以外的实体公司)，                                                  等级aa应该是基线目标。全球市场的可达性水平提高，以适当地遵守欧盟功能可达性要求它与2.0级aa紧密配合。直接访问设计人员也可以实现更大的可访问性标准。
While [Section 508](https://www.section508.gov/) is the US standard, following the guidance of WCAG 2.0 will help a project pass Section 508 and also maintain a consistent internal standard. If a project specifically requires Section 508, additional confirmation testing can be done.
当第508条是美国标准，按照中的2.0的指导，将帮助项目通行证508节，并保持一致的内部标准。如果项目具体要求508节，则可以进行额外的确认测试。
### ARIA Landmark Roles
ARIA里程作用
ARIA (Assistive Rich Internet Applications) is a spec from the W3C. It was created to improve accessibility of applications by providing extra information to screen readers via HTML attributes. Screen readers can already read HTML, but ARIA can help add context to content and make it easier for screen readers to interact with content.
ARIA(辅助rich internet应用程序)是w3c的规范。它通过提供额外的信息来通过html属性来筛选阅读器来改善应用程序的可访问性。屏幕阅读器已经可以读取html，但是ARIA可以帮助添加上下文内容，并且使屏幕阅读器更容易与内容交互。
ARIA is a descriptive layer on top of HTML to be used by screen readers. It has no effect on how elements are displayed or behave in browsers. We use these ARIA Landmark Roles (banner, navigation, main, etc.) to provide a better experience to users with disabilities.ARIA还允许我们描述元素的某些固有特性以及它们的各个状态。设想一下您已经设计了一个站点，其中主要内容区域被分成三个选项卡。当用户首次访问站点时，第一个选项卡将是第一个选项卡，但是屏幕阅读器如何到达第二个选项卡？它如何知道哪个选项卡是主动的？首先，它如何知道哪个元素是制表符？
ARIA是html顶部的描述层，供屏幕阅读器使用。它对元素在浏览器中显示或行为的作用没有影响。我们使用这些ARIA地标角色(横幅、导航、主等等)来为残障用户提供更好的体验。
Example:
例子：
```html
<header id="masthead" class="site-header" role="banner"></header>
```

### States and Properties
状态和属性
ARIA also allows us to describe certain inherent properties of elements, as well as their various states. Imagine you've designed a site where the main content area is split into three tabs. When the user first visits the site, the first tab will be the primary one, but how does a screen reader get to the second tab? How does it know which tab is active? How does it know which element is a tab in the first place?
ARIA还允许我们描述元素的某些固有特性以及它们的各个状态。设想一下您已经设计了一个站点，其中主要内容区域被分成三个选项卡。当用户首次访问站点时，第一个选项卡将是第一个选项卡，但是屏幕阅读器如何到达第二个选项卡？它如何知道哪个选项卡是主动的？首先，它如何知道哪个元素是制表符？
ARIA attributes can be added dynamically with JavaScript to help add context to your content. Thinking about the tabbed content example, it might look something like this:
可以使用javascript动态添加ARIA属性，以帮助添加内容到内容。对于选项卡式内容示例的思考，它可能会看起来像这样：
```html
<ul role="tablist">
    <li role="presentation">
        <a href="#first-tab" role="tab" aria-selected="true" id="tab-panel-1">Panel 1</a>
    </li>
    <li role="presentation">
        <a href="#second-tab" role="tab" aria-selected="false" id="tab-panel-2">Panel 2</a>
    </li>
</ul>

<div role="tabpanel" aria-hidden="false" aria-labelledby="tab-panel-1">
    <h2 id="first-tab">Tab Panel Heading</h2>
</div>

<div role="tabpanel" aria-hidden="true" aria-labelledby="tab-panel-2">
    <h2 id="second-tab">Second Tab Panel Heading</h2>
</div>
```

You can see how effortless it is to make our tabbed interface accessible to screen readers. Be sure to add visibility attributes like ```aria-hidden``` with JavaScript. If it is added manually in HTML and JavaScript doesn't load, the content will be completely removed from screen readers.
您可以看到，使我们的标签界面可以访问屏幕阅读器是多么费力。确保添加可视性属性，如aria-hidden用javascript。如果在html中手动添加，javascript没有加载，则内容将完全从屏幕阅读器中删除。
### Accessible Forms
Forms are one of the biggest challenges when it comes to accessibility. Fortunately, there are a few key things that we can do to ensure they meet accessibility standards:
表单是涉及可访问性的最大挑战之一。幸运的是，我们可以做一些关键的事情来确保它们符合可达性标准：
Each form field should have its own ```<label>```. The label element, along with the ```for``` attribute, can help explicitly associate a label to a form element adding readability screen readers and assistive technology.
每个窗体字段都应该拥有它自己的<label>。标签元素，以及for属性可以帮助显式地将标签关联到表单元素中，添加可读性、屏幕阅读器和辅助技术。
Form elements should also be logically grouped using the ```<fieldset>``` element. Grouped form elements can be helpful for users who depend on screen readers or those with cognitive disabilities.
表单元素也应该用逻辑分组<fieldset>元素。分组表单元素可以帮助依赖屏幕阅读器或具有认知功能障碍的用户。
Finally, we should ensure that all interactive elements are keyboard navigable, providing easy use for people with vision or mobility disabilities (or those not able to use a mouse). In general, the tab order should be dictated by a logical source order of elements. If you feel the need to change the tab order of certain elements, it likely indicates that you should rework the markup to flow in a logical order or have a conversation with the designer about updates that can be made to the layout to improve accessibility.
最后，我们应该确保所有交互元素都是键盘导航，为视觉或移动性残疾(或无法使用鼠标)的人提供方便使用。一般来说，tab顺序应该由元素的逻辑源顺序来决定。如果您觉得需要更改某些元素的tab顺序，那么它可能意味着您应该重新修改标记以逻辑顺序进行流，或者与设计器进行对话，以便对布局进行更新以提高可访问性
### Automated Testing
In most cases, maintaining baseline accessibility requirements for a project can be an automated process. While we can't test everything, and we still need some manual testing, there are certain tools that allow us to keep our finger on the pulse of a project's accessibility compliance.
在大多数情况下，维护项目的基线可访问性需求可以是自动化过程。虽然我们无法测试所有的东西，但是我们仍然需要一些手工测试，但是有些工具允许我们保持对项目的可访问性遵从性的理解。
Automating accessibility tests is easy with a tool like [pa11y](https://github.com/pa11y/pa11y), which is a command line tool that runs [HTML Code Sniffer](http://squizlabs.github.io/HTML_CodeSniffer/) over a URL.
自动化可访问性测试是容易的，比如n.pa11y它是运行的命令行工具。html代码嗅探器在一个url上。
It is easily installed through npm: ```npm install pa11y --save-dev``` and can be adding into a ```package.json``` file as a separate NPM script as to not collide with other build processes that may be running on a project:
它易于通过国家防范机制安装：npm install pa11y --save-dev并且可以添加到package.json文件作为单独的国家防范机制脚本，以避免与项目上运行的其他构建进程发生冲突：
```
"scripts": {
    "pa11y": "pa11y --ignore notice https://projectname.test"
},
```

Running this process allows the engineer to be alerted if a code-level or design change violates the project's accessibility standards.
运行此过程允许工程师在代码级或设计更改违反项目的可访问性标准时提醒您。
<h2 id="progressive-enhancement">Progressive Enhancement {% include Util/top %}</h2>
Progressive enhancement means building a website that is robust, fault tolerant, and accessible. Progressive enhancement begins with a baseline experience and builds out from there, adding features for browsers that support them. It does not require us to select supported browsers or revert to table-based layouts. Baselines for browser and device support are set on a project-by-project basis.
渐进式增强意味着构建一个健壮、容错和可访问的网站。渐进式增强首先从基线经验开始，并从那里构建，为支持它们的浏览器添加特性。它并不要求我们选择支持浏览器或还原到基于表的布局。浏览器和设备支持的基线是按项目设置的。
At 10up, we employ progressive enhancement to ensure that the sites we build for our clients are accessible to as many users as possible. For example, browser support for SVG has not yet reached 100%. When using SVG you should always provide a fallback such as a PNG image for browsers that do not support vector graphics.
在10up，我们使用渐进式增强来确保我们为客户构建的站点可以尽可能多地访问用户。例如，对于svg浏览器支持还没有达到100%。当使用svg时，您应该始终提供一个后退，例如对于不支持向量图形的浏览器来说，png图像。
### Polyfills
When writing markup that does not have wide browser support, using polyfills can help bring that functionality to those older browsers. Providing support for older browsers is incredibly important to the business objectives of our clients. In an effort to prevent code bloat, we only provide polyfills for features that are functionally critical to a site.
当编写没有广泛浏览器支持的标记时，使用polyfills可以帮助这些旧浏览器实现此功能。为老浏览器提供支持对于我们客户的业务目标来说是极其重要的。为了防止代码膨胀，我们只为功能上关键的站点提供了polyfills。
### Modernizr
At 10up, we use Modernizr to test browser support for new features that do not yet have full support across the board. Note that you should never use the Development build of Modernizr on a Production site. Create a custom build of Modernizr that features only the tests that are necessary.
在10up中，我们使用Modernizr测试浏览器支持的新特性，这些功能尚未完全支持整个董事会。注意，您不应该在生产站点上使用中的的开发构建。创建一个自定义构建，它只包含必要的测试。
Included in Modernizr is HTML5Shiv. HTML5Shiv is important because HTML5 elements are not natively recognized by IE8 and other older browsers. Thus scripting support for older browsers can be provided with this fairly lightweight JavaScript library.
包括在Modernizr中是HTML5Shiv。HTML5Shiv很重要，因为html 5元素并不是由ie8和其他旧浏览器所认可的。因此，可以使用这个相当轻量级的javascript库来提供针对旧浏览器的脚本支持。



n.10up
N.10UP.COM  twitter：@10up GitHub
