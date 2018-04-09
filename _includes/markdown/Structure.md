<h2 id="file-organization">Theme and Plugin File Organization {% include Util/top %}</h2>
主题与插件文件组织
File structure unity across themes and plugins improves engineering efficiency and maintainability. We believe the following structure is segmented enough to keep projects organized—and thus maintainable—but also flexible and open ended enough to enable engineers to comfortably modify as necessary. All themes and plugins should derive from this structure:
文件结构统一跨主题和插件提高了工程效率和可维护性。我们相信以下结构可以分割足够保持项目的组织-从而维护-而且也灵活开放，足够使工程师能够根据需要舒适地修改。所有主题和插件都应该从这个结构中派生出来：
```
|- bin/ __________________________________ # WP-CLI and other scripts
|- node_modules/ _________________________ # npm modules
|- vendor/ _______________________________ # Composer dependencies
|- assets/
|  |- images/ ____________________________ # Theme images
|  |- fonts/ _____________________________ # Custom/hosted fonts
|  |- js/
|    |- src/ _____________________________ # Source JavaScript
|  |- css/
|    |- scss/ ____________________________ # See below for details
|- includes/ _____________________________ # PHP classes and files
|    |- classes/ _________________________ # PHP classes
|- templates/ ____________________________ # Page templates
|- partials/ _____________________________ # Template parts
|- languages/ ____________________________ # Translations
|- tests/
|  |- php/ _______________________________ # PHP testing suite
|  |- js/ ________________________________ # JavaScript testing suite
|- .editorconfig _________________________ # Editor Config settings
|- composer.json _________________________ # Composer package file
```

The `scss` folder is described separately, below to improve readability:
scss文件夹分别描述如下，以提高可读性：
```
|- assets/css/scss/
|  |- global/ ____________________________ # Functions, mixins, placeholders, and variables
|  |- base/
|    |- reset, normalize, or sanitize
|    |- typography
|    |- icons
|    |- wordpress ________________________ # Partial for WordPress default classes
|  |- components/
|    |- buttons
|    |- callouts
|    |- toggles
|    |- all other modular reusable UI components
|  |- layout/
|    |- header
|    |- footer
|    |- sidebar
|  |- templates/
|    |- home page
|    |- single
|    |- archives
|    |- blog
|    |- all page, post, and custom post type specific styles
|  |- admin/ _____________________________ # Admin specific partials
|  |- editor/ ____________________________ # Editor specific partials (leverage placeholders to use in front-end and admin area)
|  |- admin.scss
|  |- project.scss
|  |- editor-styles.scss
```

<h2 id="dependencies">Dependencies {% include Util/top %}</h2>
依赖关系
Projects generally use two different types of dependency management:
项目通常使用两种不同类型的依赖管理：
- [npm](https://npmjs.org) is used to manage JavaScript dependencies.
NPM用于管理javascript依赖项。
- [Composer](https://getcomposer.org) is used primarily for back-end (i.e. admin or PHP-based) dependencies
作曲家主要用于后端(即基于admin或php)依赖项。
Generally, dependencies pulled in via a manager are _not_ committed to the repository, just the file defining the dependencies. This allows all developers involved to pull down local copies of each library as needed, and keeps the repository fairly clean.
通常，通过管理人员拉出的依赖项是不只向存储库提交，只定义依赖项的文件。这使得所有开发人员都可以根据需要从每个库中提取本地副本，并使存储库保持相当干净。
With some projects, using an automated dependency manager won't make sense. In server environments like VIP, running dependency software on the server is impossible. If required repositories are private (i.e. invisible to the clients' in-house developers), expecting the entire team to use a dependency manager is unreasonable. In these cases, the dependency, its version, and the reason for its inclusion in the project outside of a dependency manager should be documented.
通过一些项目，使用自动化依赖管理器是不合理的。在服务器环境中，如vip，在服务器上运行依赖软件是不可能的。如果需要的存储库是私有的(即客户内部开发人员不可见)，那么期望整个团队使用依赖管理器是不合理的。在这些情况下，应记录依赖项、其版本以及将其包含在依赖管理器之外的项目中的原因。
## Composer Based Project Structure
基于作曲家的项目结构
Here's how we might structure a project with Composer:
下面是我们如何与作曲家构建一个项目：
```
|- composer.json ____________________________ # Define third party depedencies
|- wp-config.php ____________________________ # WordPress configuration
|- wp/ ______________________________________ # Composer install WordPress here
|- wp-content/ ______________________________ # Composer dependencies
|  |- themes/ _______________________________ # Themes directory
|    |- custom-theme/ _______________________ # Custom theme
|  |- plugins/ ______________________________ # Plugins directory
|    |- custom-plugin/ ______________________ # Custom plugin
```

Here's what `composer.json` might look like with some example plugins:
这是什么？composer.json可能会像一些示例插件那样：
```json
{
  "name": "10up/project-slug",
  "description": "Project description",
  "repositories":[
    {
      "type":"composer",
      "url":"https://wpackagist.org"
    }
  ],
  "extra": {
    "wordpress-install-dir": "wp"
  },
  "require": {
    "johnpbloch/wordpress": "4.9",
    "wpackagist-plugin/wordpress-importer": "dev-trunk",
    "wpackagist-plugin/debug-bar": "dev-trunk",
    "wpackagist-plugin/debug-bar-extender": "dev-trunk",
  }
}
```

<h2 id="integrations">Third-Party Integrations</h2>
第三方集成
Any and all third-party integrations need to be documented in an `INTEGRATIONS.md` file at the root of the project repository. This file includes a list of third-party services, which components of the project those services power, how the project interacts with the remote APIs, and when the interaction is triggered. An integration that could result in unexpected consequences during something like a migration (such as sending out a tweet) should be clearly documented (see [Migrations](/Engineering-Best-Practices/migrations/) section).
任何和所有第三方集成都需要在INTEGRATIONS.md文件位于项目存储库的根目录中。该文件包含第三方服务列表，其中包括项目的组件、服务功率、项目如何与远程api交互，以及当交互触发时。在诸如迁移之类的事情中可能导致意外后果的集成(例如发送tweet)应该清楚地记录下来(参见迁移(第一节)。
For example:
例如：
```
## CrunchBase
Remote service for fetching funding and other investment data related to tech startups.

### Scheduling
- The CrunchBase API is used via JS in a dynamic product/company search on post edit pages
- Funding data is pulled every 6 hours by WP Cron to update cached data

### Integration Points
- /assets/js/src/crunchbase-autocomplete.js
- /includes/classes/crunchbase.php
- /includes/classes/cron.php

### Development API
- See https://somesitethatrequireslogin.com/credentials-for-project
```

### API Keys and Credentials
api密钥和凭据
Authentication credentials and API keys should _never_ be hard-coded into a project. Hard-coding production credentials leads to embarrassing eventualities like posting development content to Twitter or emailing such content to clients' mail lists.
身份验证凭据和api密钥应该绝不可能将硬编码成一个项目。硬编码生产凭据导致了尴尬的可能性，比如向twitter发布开发内容或将这些内容发送给客户的邮件列表。
Where possible, the project should expose a UI for entering and managing third party credentials.
在可能的情况下，该项目应该公开一个用于输入和管理第三方凭据的ui。
If a management UI is impossible due to the nature of the project, credentials should be loaded via either PHP constants or WordPress filters. These options can - and should - default to developer credentials in the absence of production data.
如果由于项目本身的性质而无法管理ui，则凭据应该通过php常量或wordpress过滤器加载。在没有生产数据的情况下，这些选项可以-而且应该默认为开发人员凭据。
```php
<?php
// Production API keys should ideally be defined in wp-config.php
// This section should default to a development or noop key instead.
if ( ! defined( 'CLIENT_MANDRILL_API_KEY' ) && ! ENV_DEVELOPMENT ) {
	define( 'CLIENT_MANDRILL_API_KEY', '1234567890' );
}
```

The `ENV_DEVELOPMENT` constant should always be set to `true` for local development and should be used whenever and wherever possible to prevent production-only functionality from triggering in a local environment.
ENV_DEVELOPMENT常量应该始终设置为true对于本地开发，应该随时随地使用，以防止仅生产功能从本地环境中触发。
The location where other engineers can retrieve developer API keys (i.e. project management tool) can and should be logged in the `INTEGRATIONS.md` file to aid in local testing. Production API keys must _never_ be stored in the repository, neither in text files or hard-coded into the project itself.
其他工程师可以检索开发人员api键(即项目管理工具)的位置可以并且应该登录到INTEGRATIONS.md文件用于协助本地测试。生产api密钥必须绝不可能存储在存储库中，既不能在文本文件中，也不能硬编码到项目本身。
<h2 id="modular-code">Modular Code</h2>
模块化代码
Every project, whether a plugin a theme or a standalone library, should be coded to be reusable and modular.
每个项目，无论是插件、主题还是独立库，都应该被编码为可重用和模块化。
### Plugins
插件
If the code for a project is split off into a functionality plugin, it should be done in such a way that the theme can function when the functionality plugin is disabled, broken, or missing. Each plugin should operate within its own namespace, both in terms of code isolation and in terms of internationalization.
如果项目的代码被拆分为一个功能插件，那么它应该以这样的方式完成，当功能插件禁用、中断或丢失时，主题可以起作用。每个插件都应该在自己的命名空间中运行，无论是代码隔离还是国际化方面。
Any functions the plugin exposes for use in a theme should be done so through actions and filters - the plugin should contain multiple calls to `add_filter()` and `add_action()` as the hooks themselves will be defined in the theme.
插件公开用于主题的任何功能都应该通过操作和筛选来完成-插件应该包含多个调用add_filter()以及add_action()因为钩子本身将在主题中定义。


### Themes
主题
 Any theme dependencies on functionality plugins should be built with the use of `do_action()` or `apply_filters()`.
对于功能插件的任何主题依赖关系都应该使用do_action()或apply_filters()。
**In short,** changing to the default theme should not trigger errors on a site. Nor should disabling a functionality plugin - every piece of code should be decoupled and use standard WordPress paradigms (hooks) for interacting with one another.
总之，更改到默认主题不应触发站点上的错误。也不应该禁用功能插件-每个代码都应该解耦并使用标准wordpress范式(Hook)来相互交互。
### General Notes
一般性说明
Every project, whether it includes Composer-managed dependencies or not, _must_ contain a `composer.json` file defining the project so it can in turn be pulled in to _other_ projects via Composer.
每个项目，无论它包含了作曲家托管的依赖项，必须包含一个composer.json文件定义项目，以便它可以反过来被拉到其他通过作曲家项目。
### Editor Config
编辑Config
Every project should include an Editor Config file, `.editorconfig` in the root directory.
This file will define and maintain consistent coding styles between the different IDEs and Code Editors used on the project.
每个项目都应该包含一个编辑器的Config文件，.editorconfig在根目录中。该文件将定义并维护在项目中使用的不同代码编辑器和代码编辑器之间一致的编码样式。
All developers should install the corresponding Editor Config plugin for their
preferred development editor from [EditorConfig.org](http://editorconfig.org/#download).
所有开发人员都应该为他们安装相应的编辑器插件首选开发编辑器n.EditorConfig.org。
The editor config file with standard settings for commonly used files is shown below.
下面显示了用于常用文件的标准设置的编辑器配置文件。
```ini
root = true

[*]
indent_style = space
indent_size = 2
trim_trailing_whitespace = true

[*.{php,js,css,scss}]
end_of_line = lf
insert_final_newline = true
indent_style = tab
indent_size = 4
```

Developers may extend and/or customize these rules as new file formats are added to the project.
开发人员可以扩展和/或自定义这些规则，因为新文件格式被添加到项目中。