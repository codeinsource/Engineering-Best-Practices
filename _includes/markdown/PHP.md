## Performance
性能
Writing performant code is absolutely critical, especially at the enterprise level. There are a number of strategies and best practices we must employ to ensure our code is optimized for high-traffic situations.
编写performant代码是绝对关键的，尤其是在企业级。我们必须使用一些策略和最佳做法来确保我们的代码优化于高流量环境。
### Efficient Database Queries
高效数据库查询
When querying the database in WordPress, you should generally use a [```WP_Query```](https://codex.wordpress.org/Class_Reference/WP_Query) object. ```WP_Query``` objects take a number of useful arguments and do things behind-the-scenes that other database access methods such as [```get_posts()```](https://developer.wordpress.org/reference/functions/get_posts/) do not.
当查询wordpress中的数据库时，通常应该使用WP_Query对象。WP_Query对象使用一些有用的参数，然后在幕后做其他数据库访问方法(例如get_posts()不要。
Here are a few key points:
以下是几个关键点：
* Only run the queries that you need.
只运行您需要的查询。
    A new ```WP_Query``` object runs five queries by default, including calculating pagination and priming the term and meta caches. Each of the following arguments will remove a query:
新的WP_Query默认情况下，对象运行五个查询，包括计算分页和启动术语和元缓存。下列参数中的每一个都将删除查询：
    * ```'no_found_rows' => true```: useful when pagination is not needed.
    *在不需要分页时有用。
    * ```'update_post_meta_cache' => false```: useful when post meta will not be utilized.
    *当后元不会被使用时有用。
    * ```'update_post_term_cache' => false```: useful when taxonomy terms will not be utilized.
    *当分类法术语不会被使用时有用。
    * ```'fields' => 'ids'```: useful when only the post IDs are needed (less typical).
    *只需要使用后IDs(较少典型)时才有用。
    * Do not use ```posts_per_page => -1```.
    不使用posts_per_page => -1。
    This is a performance hazard. What if we have 100,000 posts? This could crash the site. If you are writing a widget, for example, and just want to grab all of a custom post type, determine a reasonable upper limit for your situation.
    这是一种性能风险。如果我们有100000名职位呢？这可能会撞到工地。例如，如果您正在编写一个小部件，并且只想获取所有定制的帖子类型，那么确定您的情况的合理上限。
  ```php
  <?php
  // Query for 500 posts.
  new WP_Query( array(
    'posts_per_page' => 500,
  ));
  ?>
  ```

* Do not use ```$wpdb``` or ```get_posts()``` unless you have good reason.
不使用$wpdb或get_posts()除非你有好理由。
    ```get_posts()``` actually calls ```WP_Query```, but calling ```get_posts()``` directly bypasses a number of filters by default. Not sure whether you need these things or not? You probably don't.
get_posts()实际上调用WP_Query但是呼唤get_posts()默认情况下直接绕过一些筛选器。不确定你是否需要这些东西？你可能不会。
* If you don't plan to paginate query results, always pass ```no_found_rows => true``` to ```WP_Query```.
如果您不打算重新计算查询结果，则始终传递no_found_rows => true到WP_Query。
    This will tell WordPress not to run ```SQL_CALC_FOUND_ROWS``` on the SQL query, drastically speeding up your query. ```SQL_CALC_FOUND_ROWS``` calculates the total number of rows in your query which is required to know the total amount of "pages" for pagination.
这会告诉wordpress不要跑SQL_CALC_FOUND_ROWS在sql查询上，大大加快了查询速度。SQL_CALC_FOUND_ROWS计算查询中的行总数，该行需要知道分页的“页”的总数量。
  ```php
  <?php
  // Skip SQL_CALC_FOUND_ROWS for performance (no pagination).
  new WP_Query( array(
    'no_found_rows' => true,
  ));
  ?>
  ```

* Avoid using ```post__not_in```.
避免使用post__not_in。
    In most cases it's quicker to filter out the posts you don't need in PHP instead of within the query. This also means it can take advantage of better caching. This won't work correctly (without additional tweaks) for pagination.
在大多数情况下，过滤掉您不需要的帖子，而不是在查询中过滤掉那些帖子。这将不会正确地工作(没有额外的调整)分页。
    Use :
使用：
  ```php
  <?php
  $foo_query = new WP_Query( array(
      'post_type' => 'post',
      'posts_per_page' => 30 + count( $posts_to_exclude )
  ) );

  if ( $foo_query->have_posts() ) :
      while ( $foo_query->have_posts() ) :
          $foo_query->the_post();
          if ( in_array( get_the_ID(), $posts_to_exclude ) ) {
              continue;
          }
          the_title();
      endwhile;
  endif;
  ?>
  ```

    Instead of:
而不是：
  ```php
  <?php
  $foo_query = new WP_Query( array(
      'post_type' => 'post',
      'posts_per_page' => 30,
      'post__not_in' => $posts_to_exclude
  ) );
  ?>
  ```

    See [WordPress VIP](https://vip.wordpress.com/documentation/performance-improvements-by-removing-usage-of-post__not_in/).
见WordPress VIP。
* A [taxonomy](https://codex.wordpress.org/Taxonomies) is a tool that lets us group or classify posts.
阿分类学是一个工具，允许我们对帖子进行分类或分类。
    [Post meta](https://codex.wordpress.org/Custom_Fields) lets us store unique information about specific posts. As such the way post meta is stored does not facilitate efficient post lookups. Generally, looking up posts by post meta should be avoided (sometimes it can't). If you have to use one, make sure that it's not the main query and that it's cached.
后元让我们存储特定帖子的独特信息。因此，存储后元数据并不能促进有效的后处理。一般来说，应该避免用邮局元查找帖子(有时它不能)。如果您必须使用一个，请确保它不是主要查询，并且它缓存。
* Passing ```cache_results => false``` to ```WP_Query``` is usually not a good idea.
经过cache_results => false到WP_Query通常不是个好主意。
    If ```cache_results => true``` (which is true by default if you have caching enabled and an object cache setup), ```WP_Query``` will cache the posts found among other things. It makes sense to use ```cache_results => false``` in rare situations (possibly WP-CLI commands).
如果cache_results => true(默认情况下，如果启用了缓存并设置对象缓存设置)，WP_Query将缓存找到的帖子与其他东西。使用它是有意义的cache_results => false在罕见情况下(可能是wp-cli命令)。
* Multi-dimensional queries should be avoided.
应避免多维查询。
    Examples of multi-dimensional queries include:
多维查询的例子包括：
      * Querying for posts based on terms across multiple taxonomies  
      * Querying multiple post meta keys
    Each extra dimension of a query joins an extra database table. Instead, query by the minimum number of dimensions possible and use PHP to filter out results you don't need.
      查询的每个额外维度都联接一个额外的数据库表。相反，可以通过尽可能少的维度进行查询，  查询并使用php来筛选出您不需要的结果
    Here is an example of a 2-dimensional query:
    下面是一个二维查询的示例：
  ```php
  <?php
  // Query for posts with both a particular category and tag.
  new WP_Query( array(
    'category_name' => 'cat-slug',
    'tag' => 'tag-slug',
  ));
  ?>
  ```

#### WP\_Query vs. get\_posts() vs. query\_posts()  
 wp_query与get_员额()和query_员额()
As outlined above, `get_posts()` and `WP_Query`, apart from some slight nuances, are quite similar. Both have the same performance cost (minus the implication of skipping filters): the query performed.
如上文所述，get_posts()以及WP_Query除了一些细微的细微差别，也是相当相似的。两者都具有相同的性能成本(减去跳过过滤器的含义)：执行的查询。
[`query_posts()`](https://developer.wordpress.org/reference/functions/query_posts/), on the other hand, behaves quite differently than the other two and should almost never be used. Specifically:
query_posts()另一方面，行为与其他两种行为表现得完全不同，几乎永远也不应该使用。具体而言：
* It creates a new `WP_Query` object with the parameters you specify.
它创建了一个新的WP_Query对象，并指定的参数。
* It replaces the existing main query loop with a new instance of `WP_Query`.
它使用一个新实例替换现有主查询循环。WP_Query。
As noted in the [WordPress Codex (along with a useful query flow chart)](https://codex.wordpress.org/Function_Reference/query_posts), `query_posts()` isn't meant to be used by plugins or themes. Due to replacing and possibly re-running the main query, `query_posts()` is not performant and certainly not an acceptable way of changing the main query.
正如wordpress代码(连同一个有用的查询流程图)，query_posts()不是被插件或主题所使用的。由于替换和可能重新运行主查询，query_posts()不是performant，也不是更改主查询的可接受方法。
#### Build arrays that encourage lookup by key instead of search by value
构建数组，以鼓励通过键查找而不是以值搜索。
[`in_array()`](https://secure.php.net/manual/en/function.in-array.php) is not an efficient way to find if a given value is present in an array.
The worst case scenario is that the whole array needs to be traversed, thus making it a function with [O(n)](https://en.wikipedia.org/wiki/Big_O_notation#Orders_of_common_functions) complexity. VIP review reports `in_array()` use as an error, as it's known not to scale.
in_array()不是一个有效的方法来查找给定值是否存在于数组中。最糟糕的情况是，整个数组需要遍历，从而使它成为一个函数O(N)复杂性。贵宾审查报告in_array()作为错误，因为众所周知不要缩放。
The best way to check if a value is present in an array is by building arrays that encourage lookup by key and use [`isset()`](https://secure.php.net/manual/en/function.isset.php).
`isset()` uses an [`O(1)`](https://en.wikipedia.org/wiki/Big_O_notation#Orders_of_common_functions) hash search on the key and will scale.
检查数组中是否存在值的最佳方法是通过构建支持通过密钥查找和使用的数组。isset()。isset()使用O(1)对密钥进行哈希搜索，并将进行规模化。

Here is an example of an array that encourages lookup by key by using the intended values as keys of an associative array
下面是一个数组示例，该数组通过使用预期值作为关联数组的键来鼓励查找。
```php
<?php
$array = array(
 'foo' => true,
 'bar' => true,
);
if ( isset( $array['bar'] ) ) {
  // value is present in the array
};
```

In case you don't have control over the array creation process and are forced to use `in_array()`, to improve the performance slightly, you should always set the third parameter to `true` to force use of strict comparison.

如果您对数组创建过程没有控制权，并且被迫使用in_array()为了稍微提高性能，您应该始终将第三个参数设置为true强制使用严格的比较。
### Caching
缓存
Caching is simply the act of storing computed data somewhere for later use, and is an incredibly important concept in WordPress. There are different ways to employ caching, and often multiple methods will be used.
缓存仅仅是存储计算数据的行为，以便以后使用，而且是wordpress中一个非常重要的概念。使用缓存有不同的方法，通常会使用多个方法。
#### The "Object Cache"
“对象缓存”
Object caching is the act of caching data or objects for later use. In the context of WordPress, objects are cached in memory so they can be retrieved quickly.
对象缓存是缓存数据或对象的行为，以便以后使用。在wordpress的上下文中，对象缓存在内存中，以便能够快速检索。
In WordPress, the object cache functionality provided by [```WP_Object_Cache```](https://developer.wordpress.org/reference/classes/wp_object_cache/), and the [Transients API](https://codex.wordpress.org/Transients_API) are great solutions for improving performance on long-running queries, complex functions, or similar.
在wordpress中，对象缓存功能由WP_Object_Cache以及瞬变api对于改进长运行查询、复杂函数或类似的性能来说，是一个很好的解决方案。
On a regular WordPress install, the difference between transients and the object cache is that transients are persistent and would write to the options table, while the object cache only persists for the particular page load.
在常规的wordpress安装中，瞬态和对象缓存之间的区别在于，瞬态是持久性的，并且将写入选项表，而对象缓存仅保留特定页面负载。
It is possible to create a transient that will never expire by omitting the third parameter, this should be avoided as any non-expiring transients are autoloaded on every page and you may actually decrease performance by doing so.
创建一个不会通过省略第三个参数而终止的临时程序，所以应该避免这样做，因为任何未到期的瞬态都会在每个页面上进行重新设置，并且您可能实际上通过这样做降低性能。
On environments with a persistent caching mechanism (i.e. [Memcache](https://memcached.org/), [Redis](https://redis.io/), or similar) enabled, the transient functions become wrappers for the normal ```WP_Object_Cache``` functions. The objects are identically stored in the object cache and will be available across page loads.
在具有持久性缓存机制的环境中(即：模缓存，n.redis或者类似的)启用了，临时函数成为正常值的包装器。WP_Object_Cache职能。对象是相同存储在对象缓存中，并且将在页面加载之间可用。
High-traffic environments *not* using a persistent caching mechanism should be wary of using transients and filling the wp_options table with an excessive amount of data. See the "[Appropriate Data Storage](#appropriate-data-storage)" section for details.
高交通环境不使用持久缓存机制应该谨慎使用瞬态和填充wp_options表，并使用过多的数据。看“适当数据存储“详情请看。
Note: as the objects are stored in memory, you need to consider that these objects can be cleared at any time and that your code must be constructed in a way that it would not rely on the objects being in place.
注意：当对象存储在内存中时，您需要考虑这些对象可以在任何时候都被清除，并且您的代码必须以其不依赖于位置的方式构造。
This means you always need to ensure you check for the existence of a cached object and be ready to generate it in case it's not available. Here is an example:
这意味着您始终需要确保检查缓存对象的存在并准备好生成它，以防它无法使用。以下是一个例子：
```php
<?php
/**
 * Retrieve top 10 most-commented posts and cache the results.
 *
 * @return array|WP_Error Array of WP_Post objects with the highest comment counts,
 *                        WP_Error object otherwise.
 */
function prefix_get_top_commented_posts() {
    // Check for the top_commented_posts key in the 'top_posts' group.
    $top_commented_posts = wp_cache_get( 'prefix_top_commented_posts', 'top_posts' );

    // If nothing is found, build the object.
    if ( false === $top_commented_posts ) {
        // Grab the top 10 most commented posts.
        $top_commented_posts = new WP_Query( 'orderby=comment_count&posts_per_page=10' );

        if ( ! is_wp_error( $top_commented_posts ) && $top_commented_posts->have_posts() ) {
            // Cache the whole WP_Query object in the cache and store it for 5 minutes (300 secs).
            wp_cache_set( 'prefix_top_commented_posts', $top_commented_posts->posts, 'top_posts', 5 * MINUTE_IN_SECONDS );
        }
    }
    return $top_commented_posts;
}
?>
```

In the above example, the cache is checked for an object with the 10 most commented posts and would generate the list in case the object is not in the cache yet. Generally, calls to ```WP_Query``` other than the main query should be cached.
在上面的示例中，对于具有10个注释最多的帖子的对象进行缓存，并将生成列表，以防对象不在缓存中。通常，调用WP_Query除了主查询之外，还应该缓存。
As the content is cached for 300 seconds, the query execution is limited to one time every 5 minutes, which is nice.
当内容缓存为300秒钟时，查询执行将限制在每5分钟一次，这很好。
However, the cache rebuild in this example would always be triggered by a visitor who would hit a stale cache, which will increase the page load time for the visitors and under high-traffic conditions. This can cause race conditions when a lot of people hit a stale cache for a complex query at the same time. In the worst case, this could cause queries at the database server to pile up causing replication, lag, or worse.
然而，这个示例中的缓存重构总是由访问者触发，该访问者会命中一个过时的缓存，这将增加访问者的页面加载时间以及在高流量条件下。这可能会导致很多人同时命中一个复杂查询的陈旧缓存时的种族条件。在最糟糕的情况下，这可能会导致数据库服务器上的查询堆积导致复制、滞后或更糟。
That said, a relatively easy solution for this problem is to make sure that your users would ideally always hit a primed cache. To accomplish this, you need to think about the conditions that need to be met to make the cached value invalid. In our case this would be the change of a comment.
也就是说，对于这个问题来说，一个相对简单的解决方案是确保用户最好总是命中一个已启动的缓存。要完成此操作，您需要考虑要满足的条件以使缓存值无效。在我们的例子中，这将是一种评论的改变。
The easiest hook we could identify that would be triggered for any of this actions would be [```wp_update_comment_count```](https://developer.wordpress.org/reference/hooks/wp_update_comment_count/) set as ```do_action( 'wp_update_comment_count', $post_id, $new, $old )```.
我们可以识别出的最简单的钩子，将触发任何此操作将是wp_update_comment_count设置为do_action( 'wp_update_comment_count', $post_id, $new, $old )。
With this in mind, the function could be changed so that the cache would always be primed when this action is triggered.
考虑到这一点，函数可以被更改，以便在触发此动作时始终对缓存进行启动。
Here is how it's done:
下面是如何做的：
```php
<?php
/**
 * Prime the cache for the top 10 most-commented posts.
 *
 * @param int $post_id Post ID.
 * @param int $new     The new comment count.
 * @param int $old     The old comment count.
 */
function prefix_refresh_top_commented_posts( $post_id, $new, $old ) {
    // Force the cache refresh for top-commented posts.
    prefix_get_top_commented_posts( $force_refresh = true );
}
add_action( 'wp_update_comment_count', 'prefix_refresh_top_commented_posts', 10, 3 );

/**
 * Retrieve top 10 most-commented posts and cache the results.
 *
 * @param bool $force_refresh Optional. Whether to force the cache to be refreshed. Default false.
 * @return array|WP_Error Array of WP_Post objects with the highest comment counts, WP_Error object otherwise.
 */
function prefix_get_top_commented_posts( $force_refresh = false ) {
    // Check for the top_commented_posts key in the 'top_posts' group.
    $top_commented_posts = wp_cache_get( 'prefix_top_commented_posts', 'top_posts' );

    // If nothing is found, build the object.
    if ( true === $force_refresh || false === $top_commented_posts ) {
        // Grab the top 10 most commented posts.
        $top_commented_posts = new WP_Query( 'orderby=comment_count&posts_per_page=10' );

        if ( ! is_wp_error( $top_commented_posts ) && $top_commented_posts->have_posts() ) {
            // In this case we don't need a timed cache expiration.
            wp_cache_set( 'prefix_top_commented_posts', $top_commented_posts->posts, 'top_posts' );
        }
    }
    return $top_commented_posts;
}
?>
```

With this implementation, you can keep the cache object forever and don't need to add an expiration for the object as you would create a new cache entry whenever it is required. Just keep in mind that some external caches (like Memcache) can invalidate cache objects without any input from WordPress.
通过这个实现，您可以永久保存缓存对象，并且无需为对象添加过期，因为当需要时您将创建一个新的缓存条目。请记住，一些外部缓存(如memcache)不会使缓存对象无效，而无需wordpress的任何输入。
For that reason, it's best to make the code that repopulates the cache available for many situations.
因此，最好是使缓存的代码能够用于许多情况。
In some cases, it might be necessary to create multiple objects depending on the parameters a function is called with. In these cases, it's usually a good idea to create a cache key which includes a representation of the variable parameters. A simple solution for this would be appending an md5 hash of the serialized parameters to the key name.
在某些情况下，可能需要创建多个对象，取决于调用函数的参数。在这些情况下，创建一个缓存键通常是一个好主意，它包含变量参数的表示。对于此，一个简单的解决方案将序列化参数的md5哈希值追加到密钥名。
#### Page Caching
页缓存
Page caching in the context of web development refers to storing a requested location's entire output to serve in the event of subsequent requests to the same location.
web开发上下文中的页面缓存指存储请求的位置的整个输出，以便在随后的请求发生时服务于同一位置。
[Batcache](https://wordpress.org/plugins/batcache) is a WordPress plugin that uses the object cache (often Memcache in the context of WordPress) to store and serve rendered pages. It can also optionally cache redirects. It's not as fast as some other caching plugins, but it can be used where file-based caching is not practical or desired.
n.Batcache是一个wordpress插件，它使用对象缓存(通常在wordpress上下文中使用)来存储和服务呈现页面。它还可以选择性缓存重定向。它并不像其他缓存插件那样快，但是它可以用于基于文件缓存不实用或不需要的地方。
Batcache is aimed at preventing a flood of traffic from breaking your site. It does this by serving old (5 minute max age by default, but adjustable) pages to new users. This reduces the demand on the web server CPU and the database. It also means some people may see a page that is a few minutes old. However, this only applies to people who have not interacted with your website before. Once they have logged-in or left a comment, they will always get fresh pages.
Batcache旨在防止洪水泛滥，打破你的网站。它通过服务旧(默认为5分钟最大年龄，但可调)页向新用户提供。这也意味着有些人可能会看到一页几分钟老的页面。然而，这只适用于以前没有与您网站交互的人。一旦他们登录或留言，他们将永远得到新页面。
Although this plugin has a lot of benefits, it also has a couple of code design requirements:
虽然这个插件有很多好处，但它也有几个代码设计要求：
* As the rendered HTML of your pages might be cached, you cannot rely on server side logic related to ```$_SERVER```, ```$_COOKIE``` or other values that are unique to a particular user.
由于页面呈现的html可能被缓存，所以您不能依赖于服务器端逻辑$_SERVER，$_COOKIE或者其他特定用户特有的值。
* You can however implement cookie or other user based logic on the front-end (e.g. with JavaScript)
但是，您可以在前端实现cookie或其他基于用户的逻辑(例如使用javascript)
Batcache does not cache logged in users (based on WordPress login cookies), so keep in mind the performance implications for subscription sites (like BuddyPress). Batcache also treats the query string as part of the URL which means the use of query strings for tracking campaigns (common with Google Analytics) can render page caching ineffective.  Also beware that while WordPress VIP uses batcache, there are specific rules and conditions on VIP that do not apply to the open source version of the plugin.
Batcache并不缓存登录用户(基于wordpress登录cookie)，所以请记住订阅站点(如BuddyPress)的性能含义。
There are other popular page caching solutions such as the W3 Total Cache plugin, though we generally do not use them for a variety of reasons.
还有其他流行的页面缓存解决方案，比如在插件，虽然我们一般都没有使用它们来解决各种原因。
##### AJAX Endpoints

AJAX stands for Asynchronous JavaScript and XML. Often, we use JavaScript on the client-side to ping endpoints for things like infinite scroll.
ajax代表异步javascript和xml。通常，我们将javascript在客户端上使用ping端点来实现像无限滚动之类的东西。
WordPress [provides an API](https://codex.wordpress.org/AJAX_in_Plugins) to register AJAX endpoints on ```wp-admin/admin-ajax.php```. However, WordPress does not cache queries within the administration panel for obvious reasons. Therefore, if you send requests to an admin-ajax.php endpoint, you are bootstrapping WordPress and running un-cached queries. Used properly, this is totally fine. However, this can take down a website if used on the frontend.
WordPress提供一个api将ajax端点注册到wp-admin/admin-ajax.php。然而，wordpress并不会因为显而易见的原因在管理面板中缓存查询。因此，如果您向admin-ajax.php端点发送请求，则您将引导wordpress并运行未缓存的查询。使用得当，这完全是好的。然而，如果在前端使用，这可以降低网站。
For this reason, front-facing endpoints should be written by using the [Rewrite Rules API](http://codex.wordpress.org/Rewrite_API) and hooking early into the WordPress request process.
为此原因，前端端点应该使用重写规则api然后尽早加入wordpress请求程序。
Here is a simple example of how to structure your endpoints:
下面是一个简单的例子，说明如何构造端点：
```php
<?php
/**
 * Register a rewrite endpoint for the API.
 */
function prefix_add_api_endpoints() {
	add_rewrite_tag( '%api_item_id%', '([0-9]+)' );
	add_rewrite_rule( 'api/items/([0-9]+)/?', 'index.php?api_item_id=$matches[1]', 'top' );
}
add_action( 'init', 'prefix_add_api_endpoints' );

/**
 * Handle data (maybe) passed to the API endpoint.
 */
function prefix_do_api() {
	global $wp_query;

	$item_id = $wp_query->get( 'api_item_id' );

	if ( ! empty( $item_id ) ) {
		$response = array();

		// Do stuff with $item_id

		wp_send_json( $response );
	}
}
add_action( 'template_redirect', 'prefix_do_api' );
?>
```

#### Cache Remote Requests
缓存远程请求
Requests made to third-parties, whether synchronous or asynchronous, should be cached. Not doing so will result in your site's load time depending on an unreliable third-party!
对于第三方，无论是同步还是异步请求都应该缓存。不会这样做会导致您站点的负载时间取决于不可靠的第三方！
Here is a quick code example for caching a third-party request:
下面是一个快速代码示例，用于缓存第三方请求：
```php
<?php
/**
 * Retrieve posts from another blog and cache the response body.
 *
 * @return string Body of the response. Empty string if no body or incorrect parameter given.
 */
function prefix_get_posts_from_other_blog() {
    if ( false === ( $posts = wp_cache_get( 'prefix_other_blog_posts' ) ) {

        $request = wp_remote_get( ... );
        $posts = wp_remote_retrieve_body( $request );

        wp_cache_set( 'prefix_other_blog_posts', $posts, '', HOUR_IN_SECONDS );
    }
    return $posts;
}
?>
```

```prefix_get_posts_from_other_blog()``` can be called to get posts from a third-party and will handle caching internally.
prefix_get_posts_from_other_blog()可以调用从第三方获取帖子并将处理内部缓存。
### Appropriate Data Storage
适当数据存储
Utilizing built-in WordPress APIs we can store data in a number of ways.
使用内置的WordPress可以以多种方式存储数据。
We can store data using options, post meta, post types, object cache, and taxonomy terms.
我们可以使用选项、元数据、后类型、对象缓存和分类法术语存储数据。
There are a number of performance considerations for each WordPress storage vehicle:
每个wordpress存储工具都有很多性能考虑：
* [Options](https://codex.wordpress.org/Options_API) - The options API is a simple key-value storage system backed by a MySQL table. This API is meant to store things like settings and not variable amounts of data.
备选方案-选项api是一个简单的键值存储系统，由mysql表支持。这个api旨在存储诸如设置和不变量数据量之类的东西。
  Site performance, especially on large websites, can be negatively affected by a large options table. It's recommended to regularly monitor and keep this table under 500 rows. The "autoload" field should only be set to 'yes' for values that need to be loaded into memory on each page load.
网站性能，尤其是大型网站，可以受到大选项表的负面影响。对于需要加载到每个页面负载上的内存的值，“autoload”字段仅应设置为“是”。
  Caching plugins can also be negatively affected by a large wp_options table. Popular caching plugins such as [Memcached](https://wordpress.org/plugins/memcached/) place a 1MB limit on individual values stored in cache. A large options table can easily exceed this limit, severely slowing each page load.
缓存插件也可能受到大wp_options表的负面影响。流行的缓存插件，如梅卡奇将一个1MB限制放置在缓存中存储的单个值上。大型选项表可以轻松超过此限制，严重减慢每页负载。
* [Post Meta or Custom Fields](https://codex.wordpress.org/Custom_Fields) - Post meta is an API meant for storing information specific to a post. For example, if we had a custom post type, "Product", "serial number" would be information appropriate for post meta. Because of this, it usually doesn't make sense to search for groups of posts based on post meta.
后元或自定义字段-后元是一个api，用于存储特定于某个帖子的信息。例如，如果我们有一个定制的帖子类型，“产品”，“序列号”将是一个适合于后元的信息。因此，对于基于后元的帖子组搜索通常是没有意义的。
* [Taxonomies and Terms](https://codex.wordpress.org/Taxonomies) - Taxonomies are essentially groupings. If we have a classification that spans multiple posts, it is a good fit for a taxonomy term. For example, if we had a custom post type, "Car", "Nissan" would be a good term since multiple cars are made by Nissan. Taxonomy terms can be efficiently searched across as opposed to post meta.
分类法和术语分类法本质上是分组。如果我们有一个跨多个帖子的分类，那么它很适合分类术语。例如，如果我们有一个定制的后类型，“汽车”，“日产”将是一个好术语，因为多辆汽车是由日产制造的。分类术语可以有效地搜索到与后元相对立的。
* [Custom Post Types](https://codex.wordpress.org/Post_Types) - WordPress has the notion of "post types". "Post" is a post type which can be confusing. We can register custom post types to store all sorts of interesting pieces of data. If we have a variable amount of data to store such as a product, a custom post type might be a good fit.
自定义邮政类型-wordpress有“post类型”的概念。“邮政”是一个帖子类型，它可能会混淆。我们可以注册定制的帖子类型来存储各种有趣的数据片段。如果我们有可变数量的数据存储如产品，那么定制的后处理类型可能是很合适的。
* [Object Cache](https://codex.wordpress.org/Class_Reference/WP_Object_Cache) - See the "[Caching](#caching)" section.
对象缓存-看“缓存“分段。
While it is possible to use WordPress' [Filesystem API](https://codex.wordpress.org/Filesystem_API) to interact with a huge variety of storage endpoints, using the filesystem to store and deliver data outside of regular asset uploads should be avoided as this methods conflict with most modern / secure hosting solutions.
虽然使用WordPress‘是可能的。文件系统API为了与大量存储端点交互，应该避免使用文件系统存储和传递常规资产上载以外的数据，因为这种方法与大多数现代/安全托管解决方案相冲突。
### Database Writes
数据库写入
Writing information to the database is at the core of any website you build. Here are some tips:
将信息写入数据库是您构建任何网站的核心。以下是一些建议：
* Generally, do not write to the database on frontend pages as doing so can result in major performance issues and race conditions.
一般来说，不要在前端页面上写入数据库，这样做会导致主要性能问题和种族条件。
* When multiple threads (or page requests) read or write to a shared location in memory and the order of those read or writes is unknown, you have what is known as a [race condition](https://en.wikipedia.org/wiki/Race_condition).
当多个线程(或页请求)读取或写入内存中的共享位置以及读取或写入的顺序未知时，您所拥有的是种族条件。
* Store information in the correct place. See the "[Appropriate Data Storage](#appropriate-data-storage)" section.
把信息存储在正确的位置。看“适当数据存储“分段。
* Certain options are "autoloaded" or put into the object cache on each page load. When [creating or updating options](https://codex.wordpress.org/Options_API), you can pass an ```$autoload``` argument to [```add_option()```](https://developer.wordpress.org/reference/functions/add_option/). If your option is not going to get used often, it shouldn't be autoloaded. As of WordPress 4.2, [```update_option()```](https://developer.wordpress.org/reference/functions/update_option/) supports configuring autoloading directly by passing an optional ```$autoload``` argument. Using this third parameter is preferable to using a combination of [```delete_option()```](https://developer.wordpress.org/reference/functions/delete_option/) and ```add_option()``` to disable autoloading for existing options.
某些选项是“autoloaded”，或者在每个页面加载上放入对象缓存。何时创建或更新选项你可以通过$autoload辩论add_option()。如果你的选择不会经常被使用，那么它就不应该被重新使用。就像wordpress一样update_option()通过传递一个可选的方法支持直接配置autoloading$autoload争论。使用第三个参数最好使用delete_option()以及add_option()禁用现有选项的autoloading。
<h2 id="design-patterns">Design Patterns {% include Util/top %}</h2>
设计模式
Using a common set of design patterns while working with PHP code is the easiest way to ensure the maintainability of a project. This section addresses standard practices that set a low barrier for entry to new developers on the project.
使用php代码时使用通用的设计模式是确保项目可维护性最简单的方法。本节介绍了为项目中新开发人员输入的低障碍设置标准做法。
### Namespacing
n.Namespacing
We properly namespace all PHP code outside of theme templates. This means any PHP file that isn't part of the [WordPress Template Hierarchy](https://developer.wordpress.org/themes/basics/template-hierarchy/) should be organized within a namespace or _pseudo_ namespace so its contents don't conflict with other, similarly-named classes and functions ("namespace collisions").
我们正确地将所有php代码命名为主题模板之外。这意味着任何php文件都不是wordpress模板层次结构应该在名称空间中组织或伪命名空间这样它的内容不会与其他类似命名类和函数(“命名空间冲突”)发生冲突。
Generally, this means including a PHP ```namespace``` identifier at the top of included files:
一般来说，这意味着包括php命名空间包含文件顶部的标识符：
```php
<?php
namespace TenUp\Buy_N_Large\Wall_E;

function do_something() {
  // ...
}
```

A namespace identifier consists of a _top-level_ namespace or "Vendor Name", which is usually ```TenUp``` for our projects. We follow the top-level name with a project name, usually a client's name. e.g. ```TenUp\Buy_N_Large;```
命名空间标识符由顶层命名空间或“供应商名称”，通常是TenUp为了我们的项目。我们遵循顶级名称，其中包含项目名称，通常是客户端的名称。TenUp\Buy_N_Large;
Additional levels of the namespace are defined at discretion of the project's lead engineers. Around the time of a project's kickoff, they agree on a strategy for namespacing the project's code. For example, the client's name may be followed with the name of a particular site or high-level project we're building (```TenUp\Buy_N_Large\Wall_E;```).
命名空间的额外级别是由项目的领导工程师斟酌决定的。在项目启动的时间左右，他们同意了一个策略来重新评估项目的代码。例如，客户名称可以跟随一个特定站点的名称或我们正在构建的高级别项目(TenUp\Buy_N_Large\Wall_E;)。
When 10up works on more than one project for a client and we build common plugins shared between sites, "Common" might be used in place of the project name to signal this code's relationship to the rest of the codebase.
当10up在一个客户机上多个项目上工作时，我们构建了网站之间共享的通用插件，那么“common”可能用于代替项目名称来将代码与代码库的关系进行信号处理。
The engineering leads document this strategy so it can be shared with engineers brought onto the project throughout its lifecycle.
工程领导文档的文档，这样它可以与工程师分享整个生命周期中的项目。
[```use``` declarations](https://secure.php.net/manual/en/language.namespaces.importing.php) should be used for classes outside a file's namespace. By declaring the full namespace of a class we want to use *once* at the top of the file, we can refer to it by just its class name, making code easier to read. It also documents a file's dependencies for future developers.
use声明应该用于文件命名空间之外的类。通过声明我们希望使用的类的完整名称空间一次它还会为将来开发人员文档提供一个依赖项。
```php
<?php
/**
 * Example of a 'use' declaration
 */
namespace TenUp\Buy_N_Large\Wall_E;
use TenUp\Buy_N_Large\Common\TwitterAPI;

function do_something() {
  // Hard to read
  $twitter_api = new TenUp\Buy_N_Large\Common\TwitterAPI();
  // Preferred
  $twitter_api = new TwitterAPI();
}
```

If the code is for general release to the WordPress.org theme or plugin repositories, the [minimum PHP compatibility](https://wordpress.org/about/requirements/) of WordPress itself must be met. Unfortunately, PHP namespaces are not supported in version < 5.3, so instead, a class would be used to wrap static functions to serve as a _pseudo_ namespace:
如果代码是用于通用发布到在主题或插件库的代码，则最小php兼容性wordpress本身必须被满足。不幸的是，php命名空间在版本<5.3中不支持，因此，类将用于封装静态函数，作为伪名称空间：
```php
<?php
/**
 * Namespaced class name example.
 */
class Tenup_Utilities_API {
	public static function do_something() {
		// ...
	}
}
```

The similar structure of the namespace and the static class will allow for simple onboarding to either style of project (and a quick upgrade to PHP namespaces if and when WordPress raises its minimum version requirements).
命名空间和静态类的类似结构将允许简单地向任何类型的项目(以及当wordpress提高其最低版本要求时快速升级到php名称空间)。
Anything declared in the global namespace, including a namespace itself, should be written in such a way as to ensure uniqueness. A namespace like ```TenUp``` is (most likely) unique; ```theme``` is not. A simple way to ensure uniqueness is to prefix a declaration with a unique prefix.
全局命名空间中声明的任何内容，包括命名空间本身，都应该以确保唯一性的方式编写。一个命名空间，如TenUp(最有可能)是独一无二的；theme不是。确保唯一性的简单方法是用唯一前缀声明。
### Object Design
对象设计
Firstly, if a function is not specific to an object, it should be included in a functional [namespace](#namespacing) as referenced above.
首先，如果函数不是特定于对象的函数，则应该将其包含在函数中。命名空间正如上面提到的。
Objects should be well-defined, atomic, and fully documented in the leading docblock for the file. Every method and property within the object must themselves be fully documented, and relate to the object itself.
对象应该是明确定义的、原子性的，并且在文件的主引导中完全记录下来。对象内的每种方法和属性都必须完全记录下来，并与对象本身关联。
```php
<?php
/**
 * Video.
 *
 * This is a video object that wraps both traditional WordPress posts
 * and various YouTube meta information APIs hidden beneath them.
 *
 * @package    ClientTheme
 * @subpackage Content
 */
class Prefix_Video {

	/**
	 * WordPress post object used for data storage.
	 *
	 * @access protected
	 * @var WP_Post
	 */
	protected $_post;

	/**
	 * Default video constructor.
	 *
	 * @access public
	 *
	 * @see get_post()
	 * @throws Exception Throws an exception if the data passed is not a post or post ID.
	 *
	 * @param int|WP_Post $post Post ID or WP_Post object.
	 */
	public function __construct( $post = null ) {
		if ( null === $post ) {
			throw new Exception( 'Invalid post supplied' );
		}

		$this->_post = get_post( $post );
	}
}
```

### Visibility
明显性
In terms of [Object-Oriented Programming](https://en.wikipedia.org/wiki/Object-oriented_programming) (OOP), public properties and methods should obviously be `public`. Anything intended to be private should actually be specified as `protected`. There should be no `private` fields or properties without well-documented and agreed-upon rationale.
就...而言面向对象编程(Oop)，公共属性和方法显然应该是public。任何想要私有的都应该被指定为protected。不应该有private没有经过充分证明和同意的理由的字段或属性。
### Structure and Patterns
结构与模式
* Singletons are not advised. There is little justification for this pattern in practice and they cause more maintainability problems than they fix.
单身人士并不是建议。在实践中，这种模式没有什么理由，而且它们造成的维护性问题比修复问题更容易。
* Class inheritance should be used where possible to produce [DRY](https://en.wikipedia.org/wiki/Don't_repeat_yourself) code and share previously-developed components throughout the application.
应尽可能使用类继承来生成干代码并在整个应用程序中共享先前开发的组件。
* Global variables should be avoided. If objects need to be passed throughout the theme or plugin, those objects should either be passed as parameters or referenced through an object factory.
应避免全球变量。如果需要在主题或插件中传递对象，则这些对象都应该作为参数传递或通过对象工厂引用。
* Hidden dependencies (API functions, super-globals, etc) should be documented in the docblock of every function/method or property.
隐藏依赖(api函数、超全局等)应该记录在每个函数/方法或属性的在中。
* Avoid registering hooks in the __construct method. Doing so tightly couples the hooks to the instantiation of the class and is less flexible than registering the hooks via a separate method. Unit testing becomes much more difficult as well.
避免在_构造方法中注册钩子。这样做紧密地耦合到类实例化的钩子上，并且比通过单独方法注册钩子的灵活性要小一些。单元测试也变得更加困难。
### Decouple Plugin and Theme using add_theme_support
使用add_主题_支持解耦插件和主题
The implementation of a custom plugin should be decoupled from its use
in a Theme. Disabling the plugin should not result in any errors in the
Theme code. Similarly switching the Theme should not result in any
errors in the Plugin code.
自定义插件的实现应与其使用解耦在主题里。禁用插件不应导致任何错误主题代码。同样地，切换主题不应导致任何插件代码中的错误。
The best way to implement this is with the use of [add_theme_support](https://developer.wordpress.org/reference/functions/add_theme_support/) and [current_theme_supports](https://codex.wordpress.org/Function_Reference/current_theme_supports).
实现这一点的最好方法是使用add_主题_支持以及current_主题_支持。
Consider a plugin that adds a custom javascript file to the `page` post
type. The Theme should register support for this feature using
`add_theme_support`,
考虑一个插件，该插件将自定义javascript文件添加到page员额类型。主题应注册支持此功能的使用add_主题_支持，
```php
<?php
add_theme_support( 'custom-js-feature' );
```

And the plugin should check that the current theme has indicated support
for this feature before adding the script to the page, using
[current_theme_supports](https://codex.wordpress.org/Function_Reference/current_theme_supports),
并且插件应该检查当前主题是否表示支持对于此特性，在将脚本添加到页面之前，使用current_主题_支持，
```php
<?php
if ( current_theme_supports( 'custom-js-feature' ) ) {
	// ok to add custom js
}
```

### Asset Versioning
资产版本控制
It's always a good idea to keep assets versioned, to make cache busting a simpler process when deploying new code. Fortunately, [wp_register_script](https://developer.wordpress.org/reference/functions/wp_register_script/) and [wp_register_style](https://developer.wordpress.org/reference/functions/wp_register_style/) provide a built-in API that allows engineers to declare an asset version, which is then appended to the file name as a query string when the asset is loaded.
保持资产版本控制总是一个好主意，以便在部署新代码时使缓存破坏一个简单的过程。幸运的是，wp_register_script以及WP寄存器样式提供一个内置api，允许工程师声明一个资产版本，然后将其附加到文件名作为查询字符串时，当资产加载时。
It is recommended that engineers use a constant to define their theme or plugin version, then reference that constant when using registering scripts or styles. For example:
建议工程师使用常量来定义它们的主题或插件版本，然后在使用注册脚本或样式时引用常量。例如：
```php
<?php
define( 'THEME_VERSION', '0.1.0' );

wp_register_script( 'custom-script', get_template_directory_uri() . '/js/asset.js', array(), THEME_VERSION );
```

Remember to increment the version in the defined constant prior to deployment.
请记住在部署之前将版本添加到定义的常数中。
<h2 id="security">Security {% include Util/top %}</h2>
安全性
Security in the context of web development is a huge topic. This section only addresses some of the things we can do at the server-side code level.
网络开发背景下的安全是一个非常重要的课题。本节仅介绍了我们可以在服务器端代码级别上做的一些事情。
### Input Validation and Sanitization
输入验证和杀毒
To validate is to ensure the data you've requested of the user matches what they've submitted. Sanitization is a broader approach ensuring data conforms to certain standards such as an integer or HTML-less text. The difference between validating and sanitizing data can be subtle at times and context-dependent.
要验证，是为了确保您所请求的用户数据与所提交的数据匹配。处理方法是一个更广泛的方法，确保数据符合某些标准，例如整数或html文本。验证和消毒数据之间的区别有时可能是微妙的，并且上下文依赖。
Validation is always preferred to sanitization. Any non-static data that is stored in the database must be validated or sanitized. Not doing so can result in creating potential security vulnerabilities.
验证总是首选的卫生。存储在数据库中的任何非静态数据必须验证或消毒。这样做不会导致潜在的安全漏洞。
WordPress has a number of [validation and sanitization functions built-in](https://codex.wordpress.org/Validating_Sanitizing_and_Escaping_User_Data#Validating:_Checking_User_Input).
wordpress有很多内置验证和处理函数。
Sometimes it can be confusing as to which is the most appropriate for a given situation. Other times, it's even appropriate to write our own sanitization and validation methods.
有时它可能会混淆，因为哪种情况最适合某一特定情况。其他时候，编写自己的卫生和验证方法也是合适的。
Here's an example of validating an integer stored in post meta:
下面是验证在后元中存储的整数的示例：
```php
<?php
if ( ! empty( $_POST['user_id'] ) ) {
    if ( absint( $_POST['user_id'] ) === $_POST['user_id'] ) {
        update_post_meta( $post_id, 'key', absint( $_POST['user_id'] ) );
    }
}
?>
```

```$_POST['user_id']``` is validated using [```absint()```](https://developer.wordpress.org/reference/functions/absint/) which ensures an integer >= 0. Without validation (or sanitization), ```$_POST['user_id']``` could be used maliciously to inject harmful code or data into the database.
$_POST['user_id']使用absint()确保整数>=0。未经验证(或卫生)，$_POST['user_id']可以恶意地使用将有害代码或数据注入数据库中。
Here is an example of sanitizing a text field value that will be stored in the database:
下面是一个示例，该示例将对将存储在数据库中的文本字段值进行杀毒：
```php
<?php
if ( ! empty( $_POST['special_heading'] ) ) {
    update_option( 'option_key', sanitize_text_field( $_POST['special_heading'] ) );
}
?>
```

Since ```update_option()``` is storing in the database, the value must be sanitized (or validated). The example uses the [```sanitize_text_field()```](https://developer.wordpress.org/reference/functions/sanitize_text_field/) function, which is appropriate for sanitizing general text fields.
自update_option()存储在数据库中，必须对值进行消毒(或验证)。该示例使用sanitize_text_field()函数，它适合于对普通文本字段进行消毒。
#### Raw SQL Preparation and Sanitization
原始sql准备和杀毒
There are times when dealing directly with SQL can't be avoided. WordPress provides us with [```$wpdb```](https://codex.wordpress.org/Class_Reference/wpdb).
有时候，直接处理sql的时候是无法避免的。wordpress提供给我们$wpdb。
Special care must be taken to ensure queries are properly prepared and sanitized:
必须特别注意确保查询得到适当准备和杀毒
```php
<?php
global $wpdb;

$wpdb->get_results( $wpdb->prepare( "SELECT id, name FROM $wpdb->posts WHERE ID='%d'", absint( $post_id ) ) );
?>
```

```$wpdb->prepare()``` behaves like ```sprintf()``` and essentially calls ```mysqli_real_escape_string()``` on each argument. ```mysqli_real_escape_string()``` escapes characters like ```'``` and ```"``` which prevents many SQL injection attacks.
$wpdb->prepare()行为像sprintf()本质上调用mysqli_real_escape_string()每一个论点。mysqli_real_escape_string()转义字符'以及"这样可以防止许多sql注入攻击。
By using ```%d``` in ```sprintf()```, we are ensuring the argument is forced to be an integer. You might be wondering why ```absint()``` was used since it seems redundant. It's better to over sanitize than to miss something accidentally.
使用%d在sprintf()我们正在确保这个论点被强制成为整数。你可能会想知道为什么absint()因为它看起来多余。最好不要再做消毒，而不是意外地错过一些东西。
Here is another example:
下面是另一个例子：
```php
<?php
global $wpdb;

$wpdb->insert( $wpdb->posts, array( 'post_content' => wp_kses_post( $post_content ), array( '%s' ) ) );
?>
```

```$wpdb->insert()``` creates a new row in the database. ```$post_content``` is being passed into the ```post_content``` column. The third argument lets us specify a format for our values ```sprintf()``` style. Forcing the value to be a string using the ```%s``` specifier prevents many SQL injection attacks. However, ```wp_kses_post()``` still needs to be called on ```$post_content``` as someone could inject harmful JavaScript otherwise.
$wpdb->insert()在数据库中创建一个新行。$post_content正在被传递到post_content列。第三个参数允许我们为我们的值指定一个格式sprintf()风格。将值强制为字符串，使用%s说明符防止许多sql注入攻击。然而，wp_kses_post()还需要被邀请$post_content否则，有人可能会注入有害的javascript。
### Escape or Validate Output
转义或验证输出
To escape is to ensure data conforms to specific standards before being passed off. Validation, again, ensures that data matches what is to be expected in a much stricter way. Any non-static data outputted to the browser must be escaped or validated.
要想逃避，就是确保数据在被传递之前符合特定标准。再次验证，确保数据与预期的内容相匹配，以更严格的方式进行验证。任何输出到浏览器的非静态数据都必须转义或验证。
WordPress has a number of core functions that can be leveraged for escaping. At 10up, we follow the philosophy of *late escaping*. This means we escape things just before output in order to reduce missed escaping and improve code readability.
wordpress有很多核心功能，可以用来逃避。在10up，我们遵循迟逃。这意味着我们可以在输出之前转义一些东西，以减少遗漏的转义，提高代码可读性。
Here are some simple examples of *late-escaped* output:
下面是一些简单的例子迟逃产出：
```php
<div>
    <?php echo esc_html( get_post_meta( $post_id, 'key', true ) ); ?>
</div>
```

[```esc_html()```](https://developer.wordpress.org/reference/functions/esc_html/) ensures output does not contain any HTML thus preventing JavaScript injection and layout breaks.
esc_html()确保输出不包含任何html，从而防止javascript注入和布局中断。
Here is another example:
下面是另一个例子：
```php
<a href="mailto:<?php echo sanitize_email( get_post_meta( $post_id, 'key', true ) ); ?>">Email me</a>
```

[```sanitize_email()```](https://developer.wordpress.org/reference/functions/sanitize_email/) ensures output is a valid email address. This is an example of validating our data. A broader escaping function like [```esc_attr()```](https://developer.wordpress.org/reference/functions/esc_attr/) could have been used, but instead ```sanitize_email()``` was used to validate.
sanitize_email()确保输出是有效的电子邮件地址。这是验证我们数据的示例。更广泛的转义函数，如esc_attr()可能已经被使用了，但是却是sanitize_email()用来验证。
Here is another example:
下面是另一个例子：
```php
<input type="text" onfocus="if( this.value == '<?php echo esc_js( $fields['input_text'] ); ?>' ) { this.value = ''; }" name="name">
```

[```esc_js()```](https://developer.wordpress.org/reference/functions/esc_js/) ensures that whatever is returned is safe to be printed within a JavaScript string. This function is intended to be used for inline JS, inside a tag attribute (onfocus="...", for example).
esc_js()确保返回的任何东西都是安全的，可以在javascript字符串中打印。此函数用于内联js，在标记属性中(onfocus=“…”)。例如)。
We should not be writing JavaScript inside tag attributes anymore, this means that ```esc_js()``` should never be used. To escape strings for JS another function should be used.
我们不应该再在标签属性中编写javascript，这意味着esc_js()永远不该被使用。为了转义js字符串，应该使用另一个函数。
Here is another example:
下面是另一个例子：
```php
<script>
if ( document.cookie.indexOf( 'cookie_key' ) >= 0 ) {
document.getElementById( 'test' ).getAttribute( 'href' ) = <?php echo wp_json_encode( get_post_meta( $post_id, 'key', true ) ); ?>;
}
</script>
```

[```wp_json_encode()```](https://developer.wordpress.org/reference/functions/wp_json_encode/) ensures that whatever is returned is safe to be printed in your JavaScript code. It returns a JSON encoded string.
wp_json_encode()确保返回的任何东西都是安全的，可以在javascript代码中打印。它返回一个json编码字符串。
Note that ```wp_json_encode()``` includes the string-delimiting quotes for you.
注意：wp_json_encode()包括字符串分隔引号给您。
Sometimes you need to escape data that is meant to serve as an attribute. For that, you can use ```esc_attr()``` to ensure output only contains characters appropriate for an attribute:
有时候，您需要转义作为属性的数据。对于此，您可以使用esc_attr()确保输出仅包含适合于属性的字符：
```php
<div class="<?php echo esc_attr( get_post_meta( $post_id, 'key', true ) ); ?>"></div>
```

If you need to escape such that HTML is permitted (but not harmful JavaScript), the ```wp_kses_*``` functions can be used:
如果您需要转义这样的html允许(但不是有害的javascript)，则wp_kses_*功能可用于：
```php
<div>
    <?php echo wp_kses_post( get_post_meta( $post_id, 'meta_key', true ) ); ?>
</div>
```

```wp_kses_*``` functions should be used sparingly as they have bad performance due to a large number of regular expression matching attempts. If you find yourself using ```wp_kses_*```, it's worth evaluating what you are doing as a whole.
wp_kses_*由于大量正则表达式匹配尝试，函数应该被谨慎地使用，因为它们的性能不好。如果你发现自己使用wp_kses_*这是值得评价你做的整体。
Are you providing a meta box for users to enter arbitrary HTML? Perhaps you can generate the HTML programmatically and provide the user with a few options to customize.
您是否提供了一个元框供用户输入任意html？也许您可以编程生成html，并为用户提供一些定制选项。
If you do have to use ```wp_kses_*``` on the frontend, output should be cached for as long as possible.
如果你必须使用wp_kses_*在前端，输出应该尽可能地缓存。
Translated text also often needs to be escaped on output.
翻译文本也常常需要在输出上转义。
Here's an example:
下面举个例子
```php
<div>
    <?php esc_html_e( 'An example localized string.', 'my-domain' ) ?>
</div>
```

Instead of using the generic [```__()```](https://developer.wordpress.org/reference/functions/__/) function, something like [```esc_html__()```](https://developer.wordpress.org/reference/functions/esc_html__/) might be more appropriate. Instead of using the generic [```_e()```](https://developer.wordpress.org/reference/functions/_e/) function, [```esc_html_e()```](https://developer.wordpress.org/reference/functions/esc_html_e/) would instead be used.
而不是使用泛型__()函数，类似于esc_html__()也许更合适些。而不是使用泛型_e()功能，esc_html_e()取而代之的是使用。
There are many escaping situations not covered in this section. Everyone should explore the [WordPress codex article](https://codex.wordpress.org/Validating_Sanitizing_and_Escaping_User_Data#Escaping:_Securing_Output) on escaping output to learn more.
本节中没有涵盖许多转义情况。每个人都应该探索wordpress百科文章逃避输出来学习更多。
### Nonces

In programming, a nonce, or number used only once, is a tool used to prevent [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) or cross-site request forgery.
在编程中，一个用于防止的工具是一个用于防止的工具。n.CSRF或者跨站点请求伪造。
The purpose of a nonce is to make each request unique so an action cannot be replayed.
一个临时的目的是使每个请求都是独一无二的，所以不能重播一个动作。
WordPress' [implementation](https://codex.wordpress.org/WordPress_Nonces) of nonces are not strictly numbers used once, though they serve an equal purpose.
n.WordPress‘实施对于nonces来说，虽然它们服务于同等用途，但并不是严格使用的数字。
The literal WordPress definition of nonces is "A cryptographic token tied to a specific action, user, and window of time.". This means that while the number is not a true nonce, the resulting number *is* specifically tied to the action, user, and window of time for which it was generated.
WordPress中的的字面定义是“绑定到特定动作、用户和时间窗口的加密令牌”。这意味着虽然数字不是真正的临时值，但所产生的数字是具体地绑定到生成的操作、用户和时间窗口。
Let's say you want to trash a post with `ID` 1. To do that, you might visit this URL: ```https://example.com/wp-admin/post.php?post=1&action=trash```
假设你想把一个帖子放在ID1.为此，您可能会访问此url：https://example.com/wp-admin/post.php?post=1&action=trash
Since you are authenticated and authorized, an attacker could trick you into visiting a URL like this: ```https://example.com/wp-admin/post.php?post=2&action=trash```
由于您已被验证和授权，攻击者可以欺骗您访问类似的url：https://example.com/wp-admin/post.php?post=2&action=trash
For this reason, the trash action requires a valid WordPress nonce.
出于这个原因，垃圾操作需要一个有效的wordpress临时操作。
After visiting ```https://example.com/wp-admin/post.php?post=1&action=trash&_wpnonce=b192fc4204```, the same nonce will not be valid in ```https://example.com/wp-admin/post.php?post=2&action=trash&_wpnonce=b192fc4204```.
访问后https://example.com/wp-admin/post.php?post=1&action=trash&_wpnonce=b192fc4204相同的临时期将无效https://example.com/wp-admin/post.php?post=2&action=trash&_wpnonce=b192fc4204。
Update and delete actions (like trashing a post) should require a valid nonce.
更新和删除操作(比如删除一个帖子)应该需要一个有效的临时操作。
Here is some example code for creating a nonce:
下面是创建一个临时的示例代码：
```php
<form method="post" action="">
    <?php wp_nonce_field( 'my_action_name' ); ?>
    ...
</form>
```

When the form request is processed, the nonce must be verified:
当处理表单请求时，必须验证该临时文件：
```php
<?php
// Verify the nonce to continue.
if ( ! empty( $_POST['_wpnonce'] ) && wp_verify_nonce( $_POST['_wpnonce'], 'my_action_name' ) ) {
    // Nonce is valid!
}
?>
```

### Internationalization
国际化
All text strings in a project should be internationalized using core localization functions. Even if the project does not currently dictate a need for translatable strings, this practice ensures translation-readiness should a future need arise.
项目中的所有文本字符串都应该使用核心本地化函数国际化。即使项目目前没有规定对可翻译字符串的需求，但这种做法保证了翻译准备应该出现将来。
WordPress provides a myriad of [localization functionality](https://codex.wordpress.org/I18n_for_WordPress_Developers). Engineers should familiarize themselves with features such as [pluralization](https://codex.wordpress.org/I18n_for_WordPress_Developers#Plurals) and [disambiguation](https://codex.wordpress.org/I18n_for_WordPress_Developers#Disambiguation_by_context) so translations are flexible and translators have the information they need to work accurately.
wordpress提供了无数的本地化功能。工程师应该熟悉一些特性，例如多元化以及消歧翻译是灵活的，译者有着准确地工作所需的信息。
Samuel Wood (Otto) put together a guide to WordPress internationalization best practices, and engineers should take time to familiarize themselves with its guidance: [Internationalization: You’re probably doing it wrong](http://ottopress.com/2012/internationalization-youre-probably-doing-it-wrong/)
塞缪尔伍德(Otto)把wordpress国际化最佳实践的指南放在一起，工程师们应该花些时间来熟悉它的指导：国际化：你可能做错了
It's important to note that the strings passed to translation functions should always be literal strings, never variables or named constants, and code shouldn't use [string interpolation](https://en.wikipedia.org/wiki/String_interpolation#PHP) to inject values into either string. Most tools used to create translations rely on [GNU gettext](https://www.gnu.org/software/gettext/) scanning source code for translation functions. PHP code won't be interpreted, only scanned like it was a block of plain text and stored similarly. If WordPress's translation APIs can't find an exact match for a string inside the translation files, it won't be able to translate the string. Instead, use [printf formatting codes](https://en.wikipedia.org/wiki/Printf_format_string) inside the string to be translated and pass the translated version of that string to sprintf() to fill in the values.
重要的是要注意传递给翻译函数的字符串应该始终是文字字符串，从不变量或命名常量，代码不应该使用字符串插值将值注入到任何字符串中。用于创建翻译的大多数工具都依赖于gnu gettext扫描源代码用于翻译功能。php代码不会被解释，只是扫描它就像一个纯文本块，并且存储类似。如果wordpress的翻译api找不到一个字符串在翻译文件中的精确匹配，那么它就无法翻译字符串。相反，使用printf格式化代码在字符串内，要翻译并将该字符串的转换版本传递给sprintf()以填充值。
For example:
例如：
```php
<?php
// This will confuse translation software
$string = __( "$number minutes left", 'plugin-domain' );
// So will this
define( 'MINUTES_LEFT', '%d minutes left' );
$string = __( MINUTES_LEFT, 'plugin-domain' );
// Correct way to do a simple translation
$string = sprintf( __( '%d minutes left', 'plugin-domain' ), $number );
// A more complex translation using _n() for plurals
$string = sprintf( _n( '%d minute left', '%d minutes left',  $number, 'plugin-domain' ), $number );
?>
```

Localizing a project differs from the core approach in two distinct ways:
项目本地化与核心方法不同，有两种不同的方式：
* A unique text domain should be used with all localization functions
一个独特的文本域应该与所有本地化函数一起使用
* Internationalized output should always be escaped
国际化产出应永远逃脱
#### Text Domains
文本域
Each project should leverage a unique text domain for its strings. Text domains should be lowercase, alphanumeric, and use hyphens to separate multiple words: `tenup-project-name`.
每个项目都应该为其字符串使用一个惟一的文本域。文本域应该是小写字母，字母数字，并使用连字符分隔多个单词：tenup-project-name。
Like the translated strings they accompany, text domains should never be stored in a variable or named constant when used with core localization functions, as this practice can often produce unexpected results. Translation tools won't interpret PHP code, only scan it like it was plain text. They won't be able to assign the text domain correctly if it's not there in plain text.
与它们所伴随的翻译字符串一样，当使用核心本地化函数时，文本域不应该存储在变量或命名常量中，因为这种做法常常会产生意想不到的结果。翻译工具不会解释php代码，只扫描它就像纯文本一样。如果文本域不在纯文本中，它们就无法正确地分配文本域。
```php
<?php
// Always this
$string = __( 'Hello World', 'plugin-domain' );
// Never this
$string = __( 'Hello World', $plugin_domain );
// Or this
define( 'PLUGIN_DOMAIN', 'plugin-domain' );
$string = __( 'Hello World', PLUGIN_DOMAIN );
?>
```

If the code is for release as a plugin or theme in the WordPress.org repositories, the text domain *must* match the directory slug for the project in order to ensure compatibility with the WordPress language pack delivery system. The text domain should be defined in the "Text Domain" header in the plugin or stylesheet headers, respectively, so the community can use [GlotPress](https://wordpress.org/plugins/glotpress/) to provide new translations.
如果代码是作为插件或主题发布到在存储库中，则文本域必须匹配该项目的目录蛞蝓，以确保与wordpress语言包交付系统兼容。文本域应该分别在插件或样式表头中的“文本域”标题中定义，这样社区就可以使用n.GlotPress提供新的翻译。
#### Escaping Strings
逃逸字符串
Most of WordPress's translation functions don't escape output by default. So, it's important to escape the translated strings like any other content.
wordpress的大部分翻译功能都不会默认地转义输出。所以，转义像任何其他内容一样的转换字符串是非常重要的。
To make this easier, the WordPress API includes functions that translate and escape in a single step. Engineers are encouraged to use these functions to simplify their code:
为了使之更简单，在包含了一个步骤，可以转换和转义函数。鼓励工程师使用这些函数简化代码：
**For use in HTML**
用于html
1. [esc_html__](https://codex.wordpress.org/Function_Reference/esc_html_2): Returns a translated and escaped string
esc_html_返回一个已翻译和转义字符串
1. [esc_html_e](https://codex.wordpress.org/Function_Reference/esc_html_e): Echoes a translated and escaped string
esc_html_e*回声一个翻译和转义字符串
1. [esc_html_x](https://codex.wordpress.org/Function_Reference/esc_html_x): Returns a translated and escaped string, *passing a context* to the translation function
ESC_html_x返回一个已翻译和转义字符串，传递上下文翻译功能
**For use in attributes**
用于属性中的使用
1. [esc_attr__](https://codex.wordpress.org/Function_Reference/esc_attr_2): Returns a translated and escaped string
esc_attr_返回一个已翻译和转义字符串
1. [esc_attr_e](https://codex.wordpress.org/Function_Reference/esc_attr_e): Echoes a translated and escaped string
esc_attr_e*回声一个翻译和转义字符串
1. [esc_attr_x](https://codex.wordpress.org/Function_Reference/esc_attr_x): Returns a translated and escaped string, *passing a context* to the translation function
esc_attr_x返回一个已翻译和转义字符串，传递上下文翻译功能
<h2 id="code-style">Code Style & Documentation {% include Util/top %}</h2>
代码样式&文档
We follow the official WordPress [coding](https://make.wordpress.org/core/handbook/best-practices/coding-standards/php/) and [documentation](https://make.wordpress.org/core/handbook/best-practices/inline-documentation-standards/php/) standards. The [WordPress Coding Standards for PHP_CodeSniffer](https://github.com/WordPress-Coding-Standards/WordPress-Coding-Standards) will find many common violations and flag risky code for manual review.
我们遵循官方的wordpress编码以及文献标准。|||php_CodeSniffer的wordpress编码标准会发现许多常见违规行为和旗帜风险代码进行手动审查。
That said, at 10up we highly value verbose commenting/documentation throughout any/all code, with an emphasis on docblock long descriptions which state 'why' the code is there and 'what' exactly the code does in human-readable prose. As a general rule of thumb; a manager should be able to grok your code by simply reading the docblock and inline comments.
也就是说，在10up中，我们高度重视在所有/所有代码中进行冗长的评论/文档，重点是对代码进行冗长描述，这些描述说明代码在那里，以及代码中的代码在可读的散文中所做的。作为一个一般的经验法则；管理者应该能够通过简单地阅读在和内联注释来重新定义代码。
Example:
例子：
```php
<?php
/**
 * Hook into WordPress to mark specific post meta keys as protected
 *
 * Post meta can be either public or protected. Any post meta which holds
 * **internal or read only** data should be protected via a prefixed underscore on
 * the meta key (e.g. _my_post_meta) or by indicating it's protected via the
 * is_protected_meta filter.
 *
 * Note, a meta field that is intended to be a viewable component of the post
 * (Examples: event date, or employee title) should **not** be protected.
 */
add_filter( 'is_protected_meta', 'protect_post_meta', 10, 2 );

/**
 * Protect non-public meta keys
 *
 * Flag some post meta keys as private so they're not exposed to the public
 * via the Custom Fields meta box or the JSON REST API.
 *
 * @internal                          Called via is_protected_meta filter.
 * @param    bool   $protected        Whether the key is protected. Default is false.
 * @param    string $current_meta_key The meta key being referenced.
 * @return   bool   $protected        The (possibly) modified $protected variable
 */
function protect_post_meta( $protected, $current_meta_key ) {

    // Assemble an array of post meta keys to be protected
    $meta_keys_to_be_protected = array(
        'my_meta_key',
        'my_other_meta_key',
		'and_another_meta_key',
    );

    // Set the protected var to true when the current meta key matches
    // one of the meta keys in our array of keys to be protected
    if ( in_array( $current_meta_key, $meta_keys_to_be_protected ) ) {
        $protected = true;
    }

	// Return the (possibly modified) $protected variable.
    return $protected;
}
?>
```

<h2 id="unit-testing">Unit and Integration Testing {% include Util/top %}</h2>
单元与集成测试
Unit testing is the automated testing of units of source code against certain assertions. The goal of unit testing is to write test cases with assertions that test if a unit of code is truly working as intended. If an assertion fails, a potential issue is exposed, and code needs to be revised.
单元测试是对某些断言进行源代码单元的自动化测试。单元测试的目的是编写测试用例，并使用断言来测试代码单元是否真正按照预期工作。如果断言失败，则会暴露潜在问题，并且代码需要修改。
By definition, unit tests do not have dependencies on outside systems; in other words, only your code (a single unit of code) is being tested. Integration testing works similarly to unit tests but assumptions are tested against systems of code, moving parts, or an entire application. The phrases unit testing and integration testing are often misused to reference one another especially in the context of WordPress.
根据定义，单元测试对外部系统没有依赖关系；换句话说，只有您的代码(一个代码单元)正在测试中。集成测试与单元测试类似，但是假设测试是针对代码、移动部件或整个应用程序进行测试的。短语单元测试和集成测试常常被误用到互相引用，尤其是在wordpress上下文中。
At 10up, we generally employ unit and integration tests only when building applications that are meant to be distributed. Building tests for client themes doesn't usually offer a huge amount of value (there are of course exceptions to this). When we do write tests, we use PHPUnit which is the WordPress standard library.
在10up中，我们通常只在构建要分发的应用程序时使用单元和集成测试。对于客户端主题的构建测试通常不会提供大量的值(当然，这当然是例外)。当我们编写测试时，我们使用了wordpress标准库。
Read more at the [PHPUnit homepage](https://phpunit.de/) and [automated testing for WordPress](https://make.wordpress.org/core/handbook/testing/automated-testing/).
阅读更多信息PHPUnit主页以及wordpress自动化测试。
<h2 id="libraries">Libraries and Frameworks {% include Util/top %}</h2>
图书馆和框架
Generally, we do not use PHP frameworks or libraries that do not live within WordPress for general theme and plugin development. WordPress APIs provide us with 99 percent of the functionality we need from database management to sending emails. There are frameworks and libraries we use for themes and plugins that are being distributed or open-sourced to the public such as PHPUnit.
通常，我们不使用php框架或库，这些框架或库不存在于wordpress中用于通用主题和插件开发。wordpress提供了我们从数据库管理到发送电子邮件所需的99%功能。我们使用的框架和库用于主题和插件，这些主题和插件正在分发或开放源码到公众，例如PHPUnit。
## Avoid *Heredoc* and *Nowdoc*
避开n.Heredoc以及n.Nowdoc
PHP's *doc syntaxes* construct large strings of HTML within code, without the hassle of concatenating a bunch of one-liners. They tend to be easier to read, and are easier for inexperienced front-end developers to edit without accidentally breaking PHP code.
phpdoc语法在代码中构造大量的html字符串，而无需连接一行单行。它们往往更容易阅读，而且对于没有经验的前端开发人员来说，更容易编辑，而无需意外地破坏php代码。
```php
$y = <<<JOKE
I told my doctor
"it hurts when I move my arm like this".
He said, "<em>then stop moving it like that!</em>"
JOKE;
```

However, heredoc/nowdoc make it impossible to practice *late escaping*:
然而，/nowdoc使得不可能练习迟逃：
```php
// Early escaping
$a = esc_attr( $my_class_name );

// Something naughty could happen to the string after early escaping
$a .= 'something naughty';

// 10up & VIP prefer to escape right at the point of output, which would be here
echo <<<HTML
<div class="test {$a}">test</div>
HTML;
```

As convenient as they are, engineers should avoid heredoc/nowdoc syntax and use traditional string concatenation & echoing instead. The HTML isn't as easy to read. But, we can be sure escaping happens right at the point of output, regardless of what happened to a variable beforehand.
工程师们应该避免使用heredoc/nowdoc语法，并且使用传统的字符串连接&相反地进行响应。html并不容易读懂。但是，我们可以肯定的是，无论发生什么变量之前发生了什么，都可以在输出点上发生转义。
```php
// Something naughty could happen to the string...
$my_class_name .= 'something naughty';

// But it doesn't matter if we're late escaping
echo '<div class="test ' . esc_attr( $my_class_name ) . '">test</div>';
```

Even better, [use WordPress' ```get_template_part()``` function as a basic template engine](http://codex.wordpress.org/Function_Reference/get_template_part#Passing_Variables_to_Template). Make your template file consist mostly of HTML, with ```<?php ?>``` tags just where you need to escape and output. The resulting file will be as readable as a heredoc/nowdoc block, but can still perform late escaping within the template itself.
echo '<div class="test ' 。 esc_attr( $my_class_name ) 。 '">test</div>';
更好的是，使用WordPress‘get_template_part()函数作为基本模板引擎。使模板文件主要由html组成，<?php ?>标签就在你需要逃离和输出的地方。所生成的文件将像一个heredoc/nowdoc块一样可读的，但是仍然可以在模板本身中执行后期转义。
### Avoid Sessions
避免集成
Sessions add extra complexity to web sites and extra burden on hosting setups. Avoid using sessions to store individual users' preferences or other data. WordPress VIP [explicitly forbids sessions in their code reviews](https://vip.wordpress.com/documentation/vip/code-review-what-we-look-for/#session_start-and-other-session-related-functions).
会议增加了网站的复杂性和额外负担托管设置。避免使用会话存储个人用户的首选项或其他数据。WordPress VIP显式禁止会话在其代码审查中。
Instead of sessions, use cookies or client-side storage APIs if possible. In addition to keeping this data off the web servers, they empower site visitors to view and delete the data tied to their activity. Systems Engineers can configure full-page caches to ignore custom cookies so they don't interfere with caching.
如果可能，而不是会话，而是使用cookie或客户端存储api。除了将此数据保存到web服务器之外，它们还授权站点访问者查看和删除与其活动相关联的数据。系统工程师可以配置全页缓存来忽略定制cookie，这样它们不会干扰缓存。
If sessions must be used, create them conservatively. Don't create sessions for every visitor. Limit sessions to the smallest group that needs them: logged-in editors and admins, or visitors using a particular feature.
如果会话必须使用，则要保守地创建它们。不要为每个访问者创建会话。将会话限制为需要它们的最小组：登录编辑器和管理员，或使用特定特性的访问者。
Sessions should never be stored in the database. This introduces extra data into a storage system that's not meant for that volume. Database session libraries also rely on PHP code which can't match the performance of PHP's native session handlers. PHP extensions for Memcache and Redis allow sessions to be stored in these in-memory datastores and are a good solution for sessions when multiple webservers are present.
会话不应存储在数据库中。这将额外数据引入到存储系统中，而不是用于该卷。数据库会话库还依赖于php代码，它无法匹配php的本地会话处理程序的性能。对于memcache和Redis，php扩展允许会话存储在这些内存中的插件中，并且对于存在多个插件时会话是一个很好的解决方案。