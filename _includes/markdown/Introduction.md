## Audience

The 10up Engineering Best Practices are not geared to teach anyone to become an engineer. Rather, they aim to illustrate how to engineer the 10up way. Therefore, these best practices are intended for capable engineers.
10up工程师最佳做法并不是教会任何人去变成工程师。                                           相反，他的目标是如何设计出工程师最佳做法                       因此，10up工程师的最佳做法是为有基础的工程师准备的
<h2 id="goal">Goal {% include Util/top %}</h2>

As a company, we strive to provide websites and components that yield a top-notch user experience. In order to improve efficiency, we need to standardize what we use and how we use it. Standardizing our tools, frameworks, libraries, style, version control, and even languages will allow us to better understand the inner workings of someone else’s project and produce better solutions ourselves.
作为一个公司，我们为了使用户获得一流的体验，我们努力提供网页和组件为了提高工作效率，我们用什么？怎么用？都需要标准化。规范我们的工具、框架、软件库、样式、版本控制，甚至语言，这样我们才能更好的理解别人项目和工程的核心工作并且产生我们自己更好的解决方案，品牌设计并建立了定制网页出版
As such, 10up engineers should follow these best practices in all their work. Our best practices are not meant to be restrictive or comprehensive; we value creativity at 10up. The aim is for this document to provide a strong guidance, not an authoritative direction. It's our hope that these best practices will not only influence 10uppers but community members as well.
因此，10up工程师在他们的工作中应该遵循他们的最佳做法，
<h2 id="philosophy">Philosophy {% include Util/top %}</h2>

> "We make web publishing easy. Maybe even fun."
我们使网页出版更加容易也更加开心

At the very heart of 10up is the publishing or user experience. WordPress, we firmly believe, is the best starting point to achieve this. We design and build custom publishing experiences for major companies and brands around the world. Our publishing experiences or websites are tailor-made for our clients and their specific needs.
出版和用户体验是许多10up的核心，WordPress，我们始终坚信，它是实现10up核心的最好起点，我们为主流公司和世界各地品牌设计并建立了定制网页出版。我们为我们的客户和他们的特殊需求
As such, the content management experience cannot be made to be generic. We don't cut corners when it comes to user experience and interface. We don't take shortcuts that compromise the end experience for the user. We don't distribute pre-packaged, auto-generated user interfaces or components.
因此，内容管理体验不能做成普通的，当涉及到体验和界面的时候我们不能偷工减料。                                                                     我们不能走捷径来损害用户体验。                                            我们不会分发预先打包的，自动生成的界面和组件给用户
> "Keep it simple."
保持简单

While our solutions are complex, we want our code, tools, processes, systems, and practices to be as simple as possible. Simplicity facilitates collaboration as there is a lower barrier of entry. This goes for things like PHP design patterns as well as workflow. We discourage practices such as writing extra levels of code abstraction (wrapping existing API's) as they complicate debugging and add another component that needs to be maintained.
当我们的解决方案复杂的时候，我们想要我们的代码，工具，程序，系统，和工作变得尽可能的简单，因为入门门槛较低，所以简单便于协作
> "We are always learning."

We are constantly challenging ourselves and learning. Knowledge gives us a competitive edge. Everyone around us is growing; if we stop growing individually or collectively and stop challenging ourselves to improve, we fall behind. For that reason, this document is not set in stone and will change. Evolving these best practices through contributions is incredibly important to us.
我们不断挑战自己和学习，知识给了我们竞争上的优势，                                             我们身边的人都在成长，如果我们自己或者整个集体停止通过挑战自己来提高                                                        我们就会变得落后，因为这个原因，我们的文件也应该改变而不是一成不变。                  10up最佳做法的逐步发展，意见对于我们是非常重要的
<h2 id="contributing">Contributing {% include Util/top %}</h2>

Please contribute via [pull requests on GitHub](https://github.com/10up/Engineering-Best-Practices).
