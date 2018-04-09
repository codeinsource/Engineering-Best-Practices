We version control all projects at 10up using [Git](https://git-scm.com/). Version control allows us to track codebase history, maintain parallel tracks of development, and collaborate without stomping out each other's changes.
我们使用版本控制所有项目在10up中使用git。版本控制允许我们跟踪代码库历史，维护开发的并行轨迹，并协作，而无需相互修改对方的更改。
<h2 id="structure-package-management">Structure and Package Management{% include Util/top %}</h2>
结构与包装管理
We structure our projects in such a way that we are not version controlling third party code, rather, they are included via a package manager. For PHP, we use [Composer](https://getcomposer.org/) to manage PHP dependencies (also see package managers](/Engineering-Best-Practices/tools/#package-managers)) e.g. WordPress core itself, plugins, and themes. Dependency management structuring is explained more in the [Structure](https://10up.github.io/Engineering-Best-Practices/structure/#composer-based-project-structure) section.
我们以这样的方式构造我们的项目，而不是版本控制第三方代码，而是通过包管理器来包含它们。对于php，我们使用作曲家为了管理php依赖关系(也见软件包管理器)(/工程-最佳实践/工具/#包-管理器)，例如wordpress核心本身、插件和主题。关联管理结构在结构第二节。
We also do not commit compiled files (JS/CSS). This saves us from having to deal with people forgetting to compile files and large merge conflicts. Instead we generate compile files during deployment.
我们也没有编写编译文件(js/css)。这使得我们不必处理那些忘记编译文件和大型合并冲突的人。相反，我们在部署期间生成编译文件。
<h2 id="workflows">Workflows {% include Util/top %}</h2>
工作流
At 10up we consider standardizing a workflow to be a very important part of the development process. Utilizing an effective workflow ensures efficient collaboration and quicker project onboarding. For this reason we use the following workflows company-wide both for internal and client projects.
在10up，我们认为标准化工作流是开发过程中非常重要的一部分。使用有效的工作流确保高效协作和更快的项目入职。为此，我们使用了以下工作流，包括内部和客户端项目。
### Commits
承诺
Commits should be small and independent items of work, containing changes limited to a distinct idea. Distinct commits are essential in keeping features separate, pushing specific features forward, or reversing or rolling back code if necessary.
承诺应是小而独立的工作项目，包含有限的改变，局限于一个明确的概念。不同的提交对于保持特性分离、推动特定特性向前、或在必要时逆转或滚动代码是必不可少的。
#### Commit Messages
提交消息
The first line of a commit message is a brief summary of the changeset, describing the expected result of the change or what is done to affect change.
提交消息的第一行是变更集的简要摘要，描述了更改的预期结果，或者为影响更改所做的操作。
```sh
git log --oneline -5

# fca8925 Update commit message best practices
# 19188a0 Add a note about autoloading transients
# 9630552 Fix a typo in PHP.md
# 2309e04 Remove extra markdown header hash
# 5cd2604 Add h3 and h4 styling
```

This brief summary is always required. It is around 50 characters or less, always stopping at 70. The high visibility of the first line makes it critical to craft something that is as descriptive as possible within space limits.
这一简短摘要一直是必需的。它大约有50个字符或更少，总是停留在70。第一行的高可视性使得在空间限制范围内制作描述尽可能多的东西是至关重要的。
```sh
git commit -m "Add an #Element.matches polyfill to support old IE"
```

Separated from the summary by a blank line is the longer description. It is optional and includes more details about the commit and its repercussions for developers. It may include links to the related issue, side effects, other solutions that were considered, or backstory. A longer description may be useful during the merge of a feature branch, for example.
从摘要中分离出空行是较长的描述。它是可选的，包含更多关于提交及其对开发人员的影响的详细信息。它可能包括与相关问题、副作用、其他解决方案或非相关解决方案的链接。例如，在合并一个功能分支时，更长的描述可能是有用的。
```sh
git commit

# Merge 'feature/polyfill' into gh-pages
#
# matches and closest are used to simplify event delegation:
# http://caniuse.com/#feat=matchesselector
# http://caniuse.com/#feat=element-closest
# Promise and fetch are used to simplify async/ajax functionality:
# http://caniuse.com/#feat=promises
# http://caniuse.com/#feat=fetch
```

### Merges
合并
In order to avoid large merge conflicts, merges should occur early and often. Do not wait until a feature is complete to merge ```master``` into it. Merging should be done as non-fast-forwards (`--no-ff`) to ensure a record of the merge exists.
为了避免大规模合并冲突，合并应该早且频繁发生。不要等到一个特性完成合并才行master投入其中。合并应以非快速转发为目的(--no-ff()确保存在合并记录。
### Themes
主题
All new development should take place on feature branches that branch off ```master```. When a new feature or bugfix is complete, we will do a non-fast-forward merge from that branch to ```staging``` to verify the feature or fix on the stage environment.
所有新开发都应该在分支上进行。master。当一个新特性或bugfix完成时，我们将从该分支中执行一个非快速转发合并到staging验证阶段环境中的特性或修复方法。
When things are absolutely ready to go, we'll deploy the feature or fix by performing a non-fast-forward merge from that branch to ```master```
当事情完全准备好了，我们将通过从该分支执行非快速转发合并到m
#### Branching
分枝
All theme projects will treat the ```master``` branch as the canonical source for live, production code. Feature branches will branch off ```master``` and should always have ```master``` merged back into them before requesting peer code review and before deploying to any staging environments.
所有主题项目将处理master分支作为规范源代码的生活，生产代码。特征分支将分支master而且应该总是有master在请求对等代码审查之前并在部署到任何登台环境之前重新合并到它们中。
All staging branches will branch off ```master``` as well, and should be named ```staging``` or ```stage-{environment}``` for consistency and clarity. Staging branches will never be merged into any other branches. The ```master``` branch can be merged into both staging and feature branches to keep them up-to-date.
所有分支都会分支master同样也应该命名staging或stage-{environment}为了一致性和清晰性。分支将永远不会合并到任何其他分支中。|||master分支可以合并到两个阶段和功能分支，以保持它们的最新
#### Complex Feature Branches
复杂特征分支
In some cases, a feature will be large enough to warrant multiple developers working on it at the same time. In order to enable testing the feature as a cohesive unit and avoid merge conflicts when pushing to ```staging``` and ```master``` it is recommended to create a feature branch to act as a staging area. We do this by branching from ```master``` to create the primary feature branch, and then as necessary, create additional branches from the feature branch for distinct items of work. When individual items are complete, merge back to the feature branch. To pull work from ```master```, merge ```master``` into the feature branch and then merge the feature branch into the individual branches. When all work has been merged back into the feature branch, the feature branch can then be merged into ```staging``` and ```master``` as an entire unit of work.
在某些情况下，一个特性将足够大，足以保证多个开发人员同时在此工作。为了使特征测试成为一个衔接单元，避免合并冲突时，staging以及master建议创建一个功能分支作为一个登台区域。我们通过分支机构来做这件事master创建主功能分支，然后必要时，从功能分支中创建其他分支，以获取不同的工作项。当单个项完成时，将其合并回特征分支。从工作中拉出工作master合并master进入功能分支，然后将特征分支合并到各个分支中。当所有工作都合并回特征分支时，将特征分支合并到staging以及master作为整个工作单位。
#### Working with WordPress.com VIP (not Go)
与WordPress.com合作(不去)
In a VIP environment, we want every commit to the theme's Subversion repository to be matched 1:1 with a merge commit on our Beanstalk Git repository. This means we add a step to our deployment above: Create a diff between the branch and ```master``` before merging. We can apply this diff as a patch to the VIP Subversion repository. Again, make sure to use non-fast-forward merges.
在vip环境中，我们希望每个提交到主题的subversion存储库中的每个提交都会在01：1内匹配，并将合并提交到我们的Beanstalk存储库中。这意味着我们将添加一个步骤来部署上面的部署：创建分支之间的差异和master在合并之前。我们可以将此差异应用到vip subversion存储库中。再次，确保使用非快速转发合并。
##### Backporting VIP
n.Backporting vip
In the event that VIP makes a change to the repository, we'll capture the diff of their changeset and import it to our development repository by:
如果vip对存储库进行更改，我们将捕获它们的变更集的差异并将其导入到开发库中：
* Grabbing the diff of their changes
抓住他们变化的差异
* Creating a new ```vip-rXXXX``` branch off ```master```
创建一个新的vip-rXXXX分支master
* Applying the diff to the new branch
将差异应用于新分支
* Merging the branch to ```staging```, using a non-fast-forward merge
将分支合并到staging使用非快速转发合并
* Merging the branch back to ```master```, again using a non-fast-forward merge
将分支合并到master再次使用非快速转发合并
#### Deleting Branches
删除分支
This workflow will inevitably build up a large list of branches in the repository. To prevent a large number of unused branches living in the repository, we'll delete them after feature development is complete.
这个工作流不可避免地会在存储库中构建一个大型分支列表。为了防止大量未使用的分支存储在存储库中，我们将在功能开发完成后删除它们。
* Move to another branch (doesn't matter which)
搬到另一个分支(不管哪种)
* Delete the branch (both on local and remote)
删除分支(无论是本地还是远程)
### Plugins
插件
Unlike theme development, the `master` branch represents a stable, released, versioned product. Ongoing development will happen in feature branches branched off a `develop` branch, which is itself branched off `master`. This pattern is commonly referred to as [the Gitflow workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow).
与主题开发不同，master分支代表一个稳定、发布、版本化的产品。正在进行的开发将发生在分支上的功能分支。develop分支，它本身是分支master。这个模式通常称为基于工作流的工作流。
#### Branching
分枝
New features should be branched off `develop` and, once complete, merged back into `develop` using a non-fast-forward merge.
新特性应被分支develop然后，一旦完成，合并回到develop使用非快速转发合并。
#### Deploying
部署
When `develop` is at a state where it's ready to form a new release, create a new `release/<version>` branch off of `develop`. In this branch, you'll bump version numbers, update documentation, and generally prepare your release. Once ready, merge your release branch (using a non-fast-forward merge) into `master` and tag the release:
何时develop在一个准备创建新发行版的状态中创建一个新版本release/<version>分支develop。在这个分支中，您将遇到版本号、更新文档，并通常准备发布。一旦准备好，将发行分支合并(使用非快速转发合并)到master并标记了发行版本：
```sh
git tag -a <version> -m "Tagging <version>"
```

> **Note:** Once a version is tagged and released, the tag must never be removed. If there is a problem with the project requiring a re-deployment, create a new version and tag to reflect the change.
注：一旦标记和释放版本，标记就永远不会被删除。如果需要重新部署的项目存在问题，请创建一个新版本和标记来反映更改。
Finally, merge `master` into `develop` so that `develop` includes all of the work that was done in your release branch and is aware of the current state of `master`.
最后，合并master进develop所以develop包括在释放分支中完成的所有工作，并了解当前的状态master。
##### Semantic Versioning
语义版本化
As we assign version numbers to our software, we follow [the Semantic Versioning pattern](http://semver.org/), wherein each version follows a MAJOR.MINOR.PATCH scheme:
当我们将版本号分配给我们的软件时，我们会遵循语义版本化模式每一版本都遵循一个MAJOR.MINOR.PATCH方案：
* **MAJOR** versions are incremented when breaking changes are introduced, such as functionality being removed or otherwise major changes to the codebase.
主修当引入了更改时，版本将递增，例如删除功能或对代码库进行重大更改。
* **MINOR** versions are incremented when new functionality is added in a backwards-compatible manner.
小当新功能添加到反向兼容的方式时，版本会递增。
* **PATCH** versions are incremented for backwards-compatible bugfixes.
补片对于后兼容的bugfixes，版本会递增。
Imagine Jake has written a new plugin: 10upalooza. He might give his first public release version **1.0.0.** After releasing the plugin, Jake decides to add some new (backwards-compatible) features, and subsequently releases version **1.1.0**. Later, Taylor finds a bug and reports it to Jake via GitHub; no functionality is added or removed, but Jake fixes the bug and releases version **1.1.1**.
想象一下杰克已经写了一个新插件：10upalooza。他可能会给他第一次公开发行版1.0.0。在释放插件之后，杰克决定添加一些新的(后兼容)特性，然后发布版本n.1.1.0。后来，泰勒找到了一个bug并通过GitHub向杰克报告了它；没有添加或删除功能，但是杰克修复了bug并发布版本。1.1.1。
Down the road, Jake decides to remove some functionality or change the way some functions are used. Since this would change how others interact with his code, he would declare this new release to be version **2.0.0**, hinting to consumers that there are breaking changes in the new version of his plugin.
在道路上，杰克决定删除一些功能或改变使用某些函数的方式。因为这将改变其他人如何与其代码交互，所以他会声明这个新版本是版本。n.2.0.0暗示消费者，他的插件的新版本正在发生变化。
##### Change Logs
更改日志
If your plugin  is being distributed, it's strongly recommended that your repository contains a `CHANGELOG.md` file, written around [the Keep A Changelog standard](http://keepachangelog.com/).
如果您的插件正在分发，那么强烈建议您的存储库包含CHANGELOG.md文件，写在保持一个标准。
Maintaining an accurate, up-to-date change log makes it easy to see – in human-friendly terms – what has changed between releases without having to read through the entire Git history. As a general rule, every merge into the `develop` branch should contain at least one line in the change log, describing the feature or fix.
保持一个准确、最新的更改日志使得易于看到-以人友好的术语-在发布之间改变了，而不必读取整个Git历史。作为一般规则，每个合并到develop分支应该至少包含一个行，在更改日志中描述该特性或修复。