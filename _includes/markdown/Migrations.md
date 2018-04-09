Data migrations are a necessary but often feared part of building applications on the web. Migrations can, and typically are, difficult, but if planned properly, they can be fairly painless. The following are some general guidelines to keep in mind when dealing with a migration. Note that this is not a how-to guide on doing a migration, since all migrations are unique, but these are some general guidelines to follow.
数据迁移是web应用程序中必不可少但常常害怕的部分。迁移可以而且通常是困难的，但是如果计划得当，它们就会相当痛苦。以下是一些一般性准则，以便在处理迁移时牢记在心。注意，这不是一个用于迁移的方法-因为所有迁移都是惟一的，但是这些都是遵循的一般准则。
## Migration Plan
移民规划
The first step in any migration project is to document a detailed plan. A typical migration plan is expected to outline:
任何迁移项目中的第一步是记录详细计划。预计一个典型的移徙计划将概述：
* How the content and data will be “pulled” out of the originating site (direct database connection, XML or WXR export, flat files such as SQL scripts, scraper, etc.). For many migrations, a SQL dump is used to move the data to a migration server where we can more easily handle it;
如何将内容和数据从原始站如何将内容和数据从原始站点中“拉出”(直接数据库连接、xml或WXR导出、诸如sql脚本、刮刀等平面文件)。对于许多迁移，使用一个sql转储将数据移动到迁移服务器，在此我们可以更容易地处理它；点中“拉出”(直接数据库连接、xml或WXR导出、诸如sql脚本、刮刀等平面文件)。对于许多迁移，使用一个sql转储将数据移动到迁移服务器，在此我们可以更容易地
* Scripting requirements. Many migrations need [WP-CLI](https://wp-cli.org/) scripts for mapping data;
脚本需求。许多迁移需要wp-cli绘图数据的脚本；
* How long the migration is expected to run;
预计迁移时间将持续多长时间；
* How the data and content will be mapped to the new site’s information architecture;
如何将数据和内容映射到新站点的信息架构；
* The impact of the migration on any production editorial processes (i.e. content freezes, differential migration requirements);
移徙对任何生产编辑进程的影响(即内容冻结、差异移徙要求)；
* Potential “gotchas” - or risks - to watch out for;
潜在的“风险-或风险-
* Staffing implications. What parties need to be available during the migration;
所涉人员配置问题。在移徙期间需要哪些缔约方；
* How will migrated content be reviewed for accuracy;
如何对迁移内容进行准确的审查；
* Making changes to data structure can influence site SEO. Proper redirects mitigate most of this, but this should be thoroughly accounted for in the plan;
对数据结构进行修改可以影响站点搜索引擎优化。正确的重定向减轻了大部分的内容，但是这应该在计划中得到彻底的解释；
* How will backups be restored. How will a failed migration be handled.
备份如何恢复。如何处理失败的迁移。
Make sure to have the plan peer reviewed by *at least* one other engineer.
确保计划同行评审至少另一个工程师。
## Writing Migration Scripts
写入迁移脚本
Now that we have a solid migration plan in place, we are ready to actually write the scripts that will handle the migration. All migrations will vary heavily at this step. Sometimes we can get WXR files and just use the WordPress importer (typically using [WP CLI](https://wp-cli.org/commands/import/)) to handle everything we need. Other times we'll use the WordPress Importer then write a script to modify data once it's in WordPress. Other times we'll write scripts that handle the entire migration process for us.
既然我们已经有了一个坚实的迁移计划，我们就准备好编写处理迁移的脚本。在这一步中，所有迁移都会发生很大变化。有时我们可以获得WXR文件，并且只需使用wordpress导入器(通常使用wp cli来处理我们需要的一切。有时我们会使用wordpress导入器，然后编写一个脚本来修改数据一旦它在wordpress中。其他时候，我们将编写脚本来处理整个迁移过程。
All of these decisions should be part of the migration plan we put together, so at this point, we know exactly what we are going to do, and we can start work accordingly. 10up will generally use [WP CLI](https://wp-cli.org/) to power these scripts, which gives us great flexibility on what we can do, what output we can show, and typically you'll have a lot less issues with performance, like memory limits and timeouts.
所有这些决定都应该是我们共同制定的移民计划的一部分，因此，在这个时候，我们确切地知道我们将要做什么，我们可以相应地开始工作。10up通常会使用wp cli为这些脚本提供权力，这给我们提供了很大的灵活性，我们可以做什么，我们可以显示什么输出，通常您会有很多性能问题，比如内存限制和超时。
## Thou Shalt Not Forget
你不能忘记
The following are some general things to keep in mind when doing these migrations. Note that not all will apply in every scenario and there are items missing from this list, but this gives a good general overview of things to keep in mind.
下面是一些一般性的事情，在做这些迁移时要记住。注意，并非所有的项目都适用于每个场景，并且在列表中遗漏了一些项目，但是这给出了一个很好的总体概述，以记住这些内容。
* *Test all migration scripts*
测试所有迁移脚本
	This probably goes without saying but migrations should be run multiple times: locally, staging, preproduction (if available), before it's ever run on production. There will inevitably be issues that need to be fixed, so the more the migration process is tested, the more likely these issues are found and fixed before it's run on production. It's always better to write and test iteratively, so any potential issues can be found and fixed sooner.
这可能不是说，但是迁移应该多次运行：本地、登台、预准备(如果可用)，在它运行之前运行。不可避免地会出现一些需要修正的问题，因此迁移过程越多测试，这些问题越可能在生产运行之前找到并加以修复。反复编写和测试总是最好的，所以任何潜在问题都可以尽早找到并加以修正。
	 * In the same vein as this, make sure all scripts are code reviewed before running on stage/preprod/prod.
与此相同的话，请确保所有脚本都是在运行于舞台/preprod/prod之前审查的代码。
* *Review data*
审查数据
	Once data has been migrated to an accessible server (typically stage), all data should be heavily reviewed to make sure it was migrated correctly. This means checking to make sure posts have correct titles, correct content, correct authors, correct dates, etc. Normally the client will be helping significantly with this step, as they are the ones most familiar with the content.
一旦数据迁移到一个可访问服务器(通常是阶段)，那么所有数据都应该被重审查以确保它正确地迁移。通常，客户端将大大地帮助这个步骤，因为它们是最熟悉内容的。
* *Schedule with host*
与主机一起安排
	When running a migration on a hosted server, make sure the host is aware when these are going to be run. Most migrations take up quite a few resources, so planning this in advance, and making sure the host is okay with it, is always a good idea. It's good to work with a host weeks in advance to plan a migration.
当在托管服务器上运行迁移时，请确保主机在运行这些服务器时知道。大多数迁移占用了相当多的资源，所以提前计划这个任务，并且确保主机对它是正确的，一直是个好主意。很好地与一个主机周一起工作来计划迁移。
* *Images (and other media)*
图像(以及其他媒体)
	Media, usually images but also videos or audio, should be considered when putting together your migration plan. These types of items typically take longer to migrate, as they have to be downloaded to the new site. Having a plan in place to handle this is important.
媒体，通常是图片，也是视频或音频，应该考虑当组合您的迁移计划。这些类型的项目通常需要更长的时间才能迁移，因为它们必须被下载到新站点。有一个计划来处理这件事是很重要
	Sometimes media can be left as-is, meaning, for instance, any image that's referenced somewhere can stay as-is and doesn't need to be moved over. But often we'll want all media brought over to the new site. In this case, if the amount of total media items is small, this can be done using core WordPress functions. If the amount of media is great, this can significantly slow down the migration process and sometimes crash it all together. In this scenario, looking into options to offload this processing is usually the way to go (something like using [Gearman](http://gearman.org/) to process multiple media items at once).
有时媒体可以被保留为-，例如，任何一个被引用的图像都可以停留在-是的，并且不需要被移动。但是通常我们会希望所有媒体都能带到新网站。如果媒体的数量很大，那么这就会大大减缓迁移过程，有时会把它们全部崩溃。在此场景中，搜索卸载此处理的选项通常是要走的方式(比如使用n.Gearman要立即处理多个媒体项目。
* *Redirects*
重定向
	In some cases, the data structure from the old site to the new changes in such a way that new URLs are needed. Sometimes this is on purpose, sometimes it's necessary because of some data changes. In any case, these need to be accounted for and the client consulted so we can add proper redirects, so any links to the old content will redirect to the new content. Failure to properly handle redirects can have disasterous search engine indexing consequences. Always work with the client closely on redirect plans.
在某些情况下，从旧站点到新的更改的数据结构需要新的url。有时候这是故意的，有时是必要的，因为有些数据发生了变化。无论如何，这些都需要被解释，客户端协商以便我们可以添加适当的重定向，所以任何指向旧内容的链接都会重新定向到新内容。未能正确处理重定向可能会产生多个搜索引擎索引结果。总是与客户密切合作，重新定向计划。
	There are multiple ways to handle redirects, whether these are set on the server level, whether they are set using a [plugin](https://wordpress.org/plugins/safe-redirect-manager/) or sometimes a custom approach is needed. Generally it's good practice to keep track of all needed redirects as the migration runs, so by the time the migration is done, we have a list of all needed redirects. This can be achieved in multiple ways but one way would be a custom mapping table, that maps old content to new content. However these are saved though, they can then be implemented with our chosen approach.
处理重定向的方法有多种，无论这些重定向是否设置在服务器级别上，无论它们是否设置为插件或者有时需要定制方法。通常，当迁移运行时，保持跟踪所有需要重定向的良好做法是好的，因此在迁移完成时，我们有一个所有需要重定向的列表。这可以通过多种方式实现，但是一种方法是定制映射表，它将旧内容映射到新内容。然而，这些都保存下来，然后可以使用我们所选的方法来实现。
* *Authors*
作者
	We always need to make sure authors are brought over and assigned correctly. When creating authors, only create them once, so we don't end up with duplicate authors. Once an author has been brought over the first time, check for and re-use that author each time we need it in the future. Some platforms have the ability to set multiple authors on a post, so make sure there's a solution figured out for that if needed.
我们总是需要确保作者被正确地完成并分配。创建作者时，只创建一个作者，这样我们就不会有重复的作者了。一旦作者首次被带回来，每次我们需要它时检查和重新使用这个作者。有些平台有能力设置多个作者在一个帖子上，所以确保有一个解决方案，如果需要的话，就可以解决。
## Potential Side Effects
潜在副作用
Even the most carefully planned migration can have issues. The following are some things that might occur if they're not planned for and tested.
即使是最周密计划的移民也会有问题。下面是一些可能会发生的事情，如果他们没有计划和测试。
* *Missing data*
缺失数据
	There will be occasions when some data can't be found when trying to migrate it. Typically this will be media items, like images. Having a plan in place to deal with any content that can't be found will ensure your scripts don't fail.
当试图迁移时，有些数据是无法找到的。通常情况下，这将是媒体项目，比如图片。如果有一个计划来处理任何无法找到的内容，那么你的脚本就不会失败。
* *SEO*
SEO
	When making changes to data structure, there's always a possibility to negatively impact SEO. If proper thought is put into redirects, as mentioned above, that should help mitigate most of this. But definitely something that should be thought through and accounted for.
当改变数据结构时，总会有可能对seo产生负面影响。如上面提到的，如果适当的思想被放入重定向中，那么这应该有助于减轻大部分的问题。但肯定是应该考虑并考虑的东西。
* *Social*
社会
	One thing to make sure of when running migrations is to not trigger any social services that might be set up. This includes sending posts to Facebook, Twitter or email. We don't want to spam subscribers with hundreds, if not thousands of items all in a row, as those items are migrated in. There are multiple ways to prevent this from happening and will depend heavily on how the site is set up (i.e. how these social services are powered). Make sure as part of the migration plan you account for this.
当运行迁移时要确保不触发任何可能设置的社交服务，必须确保其中一件事是要确保。这包括向facebook、twitter或电子邮件发送帖子。我们不想将数百个(如果不是数千项)的订阅者发送垃圾邮件，因为这些条目都被迁移进来。有多种方法来防止这种情况发生，并且将严重依赖于如何设置网站(即这些社会服务是如何供电)。请确保作为迁移计划的一部分，您可以考虑到这个问题。
* *Time*
时间
	Migrations tend to take a lot of time, from putting together a good migration plan, writing the migration scripts, testing those scripts, fixing issues found and then finally running the migration on production. Always make sure there's enough time estimated for this in a project and enough time set aside to get all this done within the required time frame.
迁移通常需要花费大量时间，从构建好的迁移计划、编写迁移脚本、测试这些脚本、修复发现的问题、然后最终运行在生产上的迁移。始终确保在项目中有足够的时间来估算这个时间，并且足够的时间可以在必要的时间框架内完成这些工作。
* *Backups*
备份
	As has been hinted at a few times in this guide, migrations tend to never work right the very first time, and they end up needing to be run over and over, with tweaks in between to get to the end goal. Always keep a backup of the original data, so at any point we can re-run a migration with a fresh data set. Also, usually it's a good idea to save a piece of meta with any content that gets migrated. This allows us at any point to find all migrated content and do something with that data (a typical use-case would be deleting all migrated content to be able to re-run the migration).
正如在本指南中曾暗示过的那样，迁移往往是第一次不正常运行，最终需要运行一遍又一遍，并在两者之间进行调整以达到最终目标。始终保留原始数据的备份，因此在任何时候我们都可以使用一个新的数据集重新运行迁移。此外，通常要保存一个元元素，并且可以将任何内容迁移到一个元素中。这允许我们在任何时候都找到所有迁移的内容，并使用这些数据做一些事情(典型的用例将删除所有迁移的内容以便重新运行迁移)。
	Also, inevitably there will come a time when after a successful production migration has been done, some other piece of data is found that wasn't originally thought of and now needs to be brought over. As long as an original data source was kept (typically a database backup), new migration scripts can be written and run to get the missing content.
此外，不可避免地会出现一段时间，当成功的生产迁移完成之后，发现一些其他数据并不是最初想出来的，而且现在需要被提升。只要保留原始数据源(通常是数据库备份)，就可以编写并运行新的迁移脚本来获取丢失的内容。
## Tips to speed up migrations
加快迁移速度的提示
* __Separate tasks.__
分开任务。
	Prioritize faster tasks. This doesn’t necessarily speed up the total execution time, but will get some data in place sooner, meaning it can be reviewed more quickly to ensure the migration is on track. Importing media files, and fetching assets from a remote site tend to be the slowest tasks.
优先处理更快的任务。这并不一定加快整个执行时间，但会更快地获得一些数据，这意味着它可以更快地审查，以确保迁移正在进行。导入媒体文件，从远程站点获取资产往往是最慢的任务。
* __Use a dedicated migration server.__
使用专用迁移服务器。
	A separate server dedicated solely to migration provides system resources for a faster migration that isn’t influenced by other applications.
单独用于迁移的单独服务器为较快迁移提供了系统资源，而迁移并不受其他应用程序的影响。
* __Free up memory.__
释放记忆。
	During migration script execution, make sure to periodically free up memory. A typical function used at 10up is:
在迁移脚本执行期间，请确保定期释放内存。在10up中使用的一个典型功能是：
```php
<?php
function stop_the_insanity() {
	global $wpdb, $wp_object_cache;

	$wpdb->queries = array();

	if ( is_object( $wp_object_cache ) ) {
		$wp_object_cache->group_ops      = array();
		$wp_object_cache->stats          = array();
		$wp_object_cache->memcache_debug = array();
		$wp_object_cache->cache          = array();

		if ( method_exists( $wp_object_cache, '__remoteset' ) ) {
			$wp_object_cache->__remoteset();
		}
	}
}
```

* __Defer counting.__
推迟计数。
	When migrating taxonomies, call `wp_defer_term_counting( true )` before, and then `wp_defer_term_counting( false )` afterwards. This technique also applies to comments, by using `wp_defer_comment_counting( true )`.
迁移分类法时，调用wp_defer_term_counting( true )以前，然后wp_defer_term_counting( false )之后。该技术还适用于注释，使用wp_defer_comment_counting( true )。
* __Use helpful constants that eliminate unneeded processing.__
使用帮助常量消除不必要的处理。
	Setting `define( 'WP_POST_REVISIONS', 0 )` can cut down significantly on memory usage.
设置define( 'WP_POST_REVISIONS', 0 )可以大大降低内存使用量。
* __Use PHP7__
	使用PHP7
	If PHP 7 is an option, use it.
如果php 7是选项，则使用它。
### Requirements for a successful migration
成功移徙的要求
* __Test all migration scripts.__
	测试所有迁移脚本。
	Migration scripts must be tested repeatedly on a local development environment, staging, and pre-production (if available) before being run on production, so that issues can be safely corrected. It’s always better to write and test scripts iteratively, to minimize the complexity involved in troubleshooting and fix bad assumptions or dependencies early in the process. As with the migration plan, have another engineer review all script code before running.
迁移脚本必须在本地开发环境、登台和预生产(如果可用)上反复测试，然后才能在生产上运行，这样就可以安全地纠正问题。迭代脚本编写和测试总是最好的，以最小化在处理过程中早期故障排除和修复错误假设或依赖项时所涉及的复杂性。与迁移计划一样，在运行之前，还有另一个工程师检查所有脚本代码。
* __Be mindful of external integrations.__
注意外部集成。
	It’s *critically important* to make sure migration scripts don’t trigger unintended external integrations. It’s common in WordPress development to tie functionality to hooks that fire when content is created and/or saved, which is part of almost every data migration. A migration script that triggers these hooks might trigger a variety of unintended actions, like publishing content to Facebook or Twitter, or firing off emails to site subscribers. In this latter case, hundreds, if not thousands, of emails could flood subscribers’ inboxes as content is migrated, possibly even during a test migration to a development site. *At best, this can be extremely embarrassing; at worst, this could do harm to a customer and even be considered negligent.*
	它是至关重要的为了确保迁移脚本不会触发意外的外部集成。wordpress开发中通常将功能连接到钩子上，当内容创建和/或保存时，它是几乎每个数据迁移的一部分。触发这些钩子的迁移脚本可能触发各种非预期操作，比如向facebook或twitter发布内容，或者向站点订阅服务器发送邮件。在后一种情况下，数百个如果不是数千封电子邮件，就会随着内容迁移而淹没订阅服务器收件箱，甚至可能在测试迁移到开发站点时。最好的做法是，这可能会非常尴尬；最糟糕的是，这可能会损害客户，甚至被认为是疏忽。
	One way to prevent some unintended triggers is by checking the `WP_IMPORTING` constant. A migration script *must* set this constant, `define( 'WP_IMPORTING', true )`, as well engineered integrations will check for that before executing.
防止某些意外触发的方法之一是检查WP_IMPORTING恒久不变。迁移脚本必须设置这个常数，define( 'WP_IMPORTING', true )而且，设计集成将在执行之前检查它。
* __Editorial data review.__
编辑数据审查。
	As engineers, it’s easy to look at data and say “this looks right”. However, editors and those deeply involved with site management have a much closer relationship to the content. Have a member of the site editorial team check posts to make sure they have correct titles, content, authors, dates, and other related metadata.
作为工程师，可以很容易地查看数据并说“这看起来正确”。然而，编辑和深入参与网站管理的人与内容的关系也越来越密切。有网站编辑团队成员检查帖子，以确保他们有正确的标题、内容、作者、日期和其他相关元数据。
* __Coordinate with the host.__
与主机协调。
	When running a migration on an externally hosted server, make sure the host is aware. Most migrations require significant server resources, and could impact other hosted applications or trigger flags.
当在外部托管服务器上运行迁移时，请确保主机知道。大多数迁移需要大量服务器资源，并且可能影响其他托管应用程序或触发器标志。
* __Make specific plans for content stored in the file system (i.e. site media).__
为文件系统中存储的内容(即站点媒体)制定具体计划。
	Media, like images, videos, or audio files typically take longer to migrate, as they have to be downloaded to the new site, and sometimes require more carefully considered content transformation (e.g. changing URLs in the content). If there is an unusually large volume of attached media to move, consider offloading processing using tools like [Gearman](https://github.com/10up/WP-Gears) to process multiple files at once.
媒体，比如图像、视频或音频文件通常需要更长的时间才能迁移，因为它们必须被下载到新站点，有时需要更仔细考虑内容转换(例如更改内容中的url)。如果有异常大的附加媒体移动，请考虑使用类似于n.Gearman一次处理多个文件。
* __Doublecheck author metadata.__
作者元数据。
	Always make sure that authors are fully migrated and assigned to the correct content. Ensure only a single account is made per author. Be careful to consider whether the site supports assigning multiple authors to a single post.
始终确保作者完全迁移并分配到正确的内容。确保每个作者只编写一个帐户。要小心考虑站点是否支持将多个作者分配给单个帖子。
* __Save a piece of meta with each migrated piece of content referring back to the original content.__
保存一个元元素，每个迁移的内容都引用原始内容。
	This allows all migrated content to be found easily (a typical use case is deleting all migrated content in preparation for re-running the migration scripts). If, after a successful production migration is completed, a piece of data is found that wasn’t originally included in the planning and now needs to be brought over, new migration scripts can be written to interact with the saved meta.
这样可以轻松地找到所有迁移的内容(典型的用例正在删除所有已迁移的内容，准备重新运行迁移脚本)。如果成功地完成了迁移，那么发现一个数据并不是最初包含在规划中，而且现在需要被提升，新迁移脚本可以编写与保存的元进行交互。