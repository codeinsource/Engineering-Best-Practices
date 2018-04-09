The following are tools we use at 10up. This list will grow and change over time and is not meant to be comprehensive. Generally, we encourage or require these tools to be used in favor of other ones. Rules governing tools to be used and packaged with a client site will be much stricter than those used on internal projects.
下面是我们在10up中使用的工具。这个列表会随着时间的推移而不断增长和变化，并不意味着全面。一般来说，我们鼓励或要求这些工具用于其他方面。使用与客户端一起使用和打包的工具的规则要比内部项目使用的工具要严格得多。
<h2 id="local-development">Local Development Environments</h2>
地方发展环境
At 10up, we use [Vagrant](https://www.vagrantup.com/) and/or [Docker](https://www.docker.com/) to build and interact with virtual environments that match production as closely as possible. There are many setups and configurations available. The following setups are the only ones we support internally.
在10up，我们使用流浪者和/或n.Docker与生产尽可能紧密地匹配的虚拟环境进行构建和交互。有许多设置和配置可用。下面的设置是我们内部唯一支持的。
[Varying Vagrant Vagrants (VVV)](https://github.com/Varying-Vagrant-Vagrants/VVV) - Our standard Vagrant setup for client sites and local development. This was originally a 10up project (now open-sourced) and something with which we have a lot of familiarity.
[Varying Vagrant Vagrants (VVV)]-我们的标准的客户端站点和本地开发的安装程序。这最初是一个基于.的项目(现在是开放源码的)，还有一些我们非常熟悉的项目。
[WP Local Docker](https://github.com/10up/wp-local-docker) - A simple Docker-based development environment. This setup is great because it's very easy to setup, simple to interact with, and makes standardizing package versions (i.e. PHP) extremely easy.
wp局部化-一个简单的基于Docker的开发环境。这个设置很好，因为它很容易设置，简单地交互，并且使得标准化软件包版本(即php)非常容易。
<h2 id="task-runners">Task Runners {% include Util/top %}</h2>
任务赛跑者
[Grunt](http://gruntjs.com/) - Grunt is a task runner built on Node that lets you automate tasks like Sass preprocessing and JS minification. Grunt is our default task runner and has a great community of plugins and solutions we use for on company and client projects.
Grunt是一个基于节点构建的任务运行器，它可以使您自动化任务，比如Sass预处理和js minification。grunt是我们默认的任务运行者，它拥有一个很好的插件和解决方案，我们使用在公司和客户项目中。
[Gulp](http://gulpjs.com/) - Gulp is also a task runner build on Node that offers a similar suite of plugins and solutions to Grunt. The biggest difference is Gulp allows you direct access to the [stream](https://nodejs.org/api/stream.html) of information from your source files and allows you to modify this data directly.
Gulp也是一个基于节点构建的任务运行器，它提供了类似的插件和解决方案套件。最大的区别是Gulp允许您直接访问溪流从源文件中获取信息，并允许您直接修改此数据。
[Webpack](https://webpack.github.io/) - Webpack is a bundler for JS/CSS. It's extremely useful when building larger JavaScript applications (i.e. React.js).
Webpack是js/css的一个子模块。当构建更大的javascript应用程序(即React.js)时，它非常有用。
<h2 id="package-managers">Package/Dependency Managers</h2>
包/管理人
[Bower](https://bower.io/) - A good tool to manage front end packages. Usually everything we need is bundled with WordPress. Sometimes we need something like "Chosen.js" that isn’t included. Bower is a good way to manage external libraries like that but is not necessary on most projects.
浏览器一个很好的工具来管理前端包。通常我们需要的东西都是用wordpress捆绑的。有时候我们需要一些不包括在内的“Chosen.js”。鲍尔是管理外部图书馆的好方法，但在大多数项目中并不需要。
[Composer](https://getcomposer.org) - We use Composer for managing PHP dependencies. Usually everything we need is bundled with WordPress, but sometimes we need external PHP libraries like "Patchwork". Composer is a great way to manage those external libraries.
我们使用作曲家来管理php依赖项。通常我们需要的东西都是wordpress捆绑的，但是有时候我们需要外部的php库，比如“拼凑”。作曲家是管理这些外部图书馆的好方法。
When a WordPress install is managed and maintained by an engineering team, and when the infrastructure supports it, plugins in a WordPress project can be easily managed using Composer. [WordPress Packagist](https://wpackagist.org/) provides a Composer repository that mirrors all public WordPress plugins and themes.
当一个wordpress安装由一个工程团队管理和维护时，当基础设施支持它时，wordpress项目中的插件可以很容易地使用作曲家来管理。wordpress提供一个用于镜像所有公共wordpress插件和主题的作曲家资源库。
<h2 id="version-control">Version Control {% include Util/top %}</h2>
版本控制
[Git](https://git-scm.com) - At 10up we use Git for version control. We encourage people to use the command line for interacting with Git. GUIs are permitted but will not be supported internally.
git-在10up中，我们使用Git来进行版本控制。我们鼓励人们使用命令行来与Git交互。允许使用guis，但不会在内部支持。
[SVN](https://subversion.apache.org/) - We use SVN, but only in the context of WordPress.com VIP. Again, we encourage people to use the command line as we do not support GUIs internally.
SVN-我们使用svn，但只有在中的的上下文中。再次，我们鼓励人们使用命令行，因为我们不支持内部的guis。
<h2 id="command-line">Command Line Tools</h2>
命令行工具
[WP-CLI](https://wp-cli.org) - A command line interface for WordPress. This is an extremely powerful tool that allows us to do imports, exports, run custom scripts, and more via the command line. Often this is the only way we can affect a large database (WordPress.com VIP or WP Engine). This tool is installed by default on VVV and VIP Quickstart.
wp-cli-wordpress的命令行接口。这是一个非常强大的工具，它允许我们进行导入、出口、运行自定义脚本以及更多的命令行。通常，这是唯一可以影响大型数据库(WordPress.com，vip或wp引擎)的方法。默认情况下，该工具安装在VVV和vip Quickstart上。
<h3 id="a11y-testing">Accessibility Testing</h3>
无障碍测试
We use a variety of tools to test our sites for accessibility issues. WebAim has some great resources on [how to evaluate sites](http://webaim.org/articles/screenreader_testing/) with a screen reader.
我们使用各种工具来测试我们的站点以获取可访问性问题。WebAim有一些很好的资源如何评价站点还有一个屏幕阅读器。
* [Using VoiceOver](http://webaim.org/articles/voiceover/)
使用画外音
* [Using NVDA](http://webaim.org/articles/nvda/)
使用NVDA
* [Using JAWS](http://webaim.org/articles/jaws/)

We're also a fan of a few browser tools that lend us a hand when it comes to testing areas like color contrast, heading hierarchy, and ARIA application.
我们也是一些浏览器工具的爱好者，它可以帮助我们在测试领域，比如颜色对比度、标题层次结构和ARIA应用程序。

* [Headings Map for Chrome](https://chrome.google.com/webstore/detail/headingsmap/flbjommegcjonpdmenkdiocclhjacmbi?hl=es) or [Headings Map for Firefox](https://addons.mozilla.org/en-us/firefox/addon/headingsmap/) - A browser extension that allows you to see the heading structure of a webpage.
chrome标题地图或firefox标题映射-浏览器扩展，允许您查看网页的标题结构。
* [The Visual ARIA Bookmarklet](http://whatsock.com/training/matrices/visual-aria.htm) - A bookmarklet that can be run on a webpage and color codes ARIA roles.
visual ARIA Bookmarklet-一个可以在网页上运行的bookmarklet和颜色代码ARIA角色。
* [WAVE](http://wave.webaim.org/) - A toolkit from WebAIM, that also has an extension for Chrome/Firefox. It evaluates a webpage and returns accessibility errors.
来自WebAIM的工具包，它也有一个扩展chrome/firefox。它评估一个网页并返回可访问性错误。
* [Accessibility Developer Tools](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb) - A Chrome extension that adds an "Audit" tab in Chrome's developer tools that can scan a webpage for accessibility issues.
可访问性开发工具-chrome扩展，它添加了chrome开发工具中的“审计”选项卡，可以扫描网页以获取可访问性问题。
* [Tota11y](https://khan.github.io/tota11y/) - A visualization toolkit that can be used as a bookmarklet, and reveals accessibility errors on a webpage.
图11y-可视化工具包，可以用作一个可扩展的工具，并显示网页上的可访问性错误。
* [Contrast Ratio](https://leaverou.github.io/contrast-ratio/) - A color contrast tool to compare two colors against [levels of conformance](https://www.w3.org/TR/UNDERSTANDING-WCAG20/conformance.html) and see if they pass.
对比比-一种颜色对比工具，用来比较两种颜色一致性水平看看他们是否能及格。
* [Tanagaru Contrast Finder](http://contrast-finder.tanaguru.com/?lang=en) - Another color contrast tool that tests colors against the levels of conformance, but also provides you with alternatives should your provided colors fail.
反相对比器-另一个颜色对比工具，它测试颜色与一致性水平，但也提供您的替代品，如果您提供的颜色失败。