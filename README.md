# Monorepo / Monolith / Multirepo

> A monolith is a massive codebase.

> A _monorepo_ is a version-controlled code repository that holds many projects. While these projects may be related, they are often logically independent and run by different teams.

> A multirepo (multiple repositories) typically has one repository for each project. The more projects, the more repositories. A multirepo is also known as polyrepo.

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

**Monorepo Advantages:**

* **One place to store all configs and tests.** Since everything is located inside one repo, you can configure your CI/CD and bundler once and then just re-use configs to build all packages before publishing them to remote. Same goes for unit, e2e, and integration tests—your CI will be able to launch all tests without having to deal with additional configuration.
* **Easily refactor global features with atomic commits.** Instead of doing a pull request for each repo, figuring out in which order to build your changes, you just need to make an atomic pull request which will contain all commits related to the feature that you are working against.
* **Simplified package publishing.** If you plan to implement a new feature inside a package that is dependent on another package with shared code, you can do it with a single command. It is a function that needs some additional configurations, which will be later discussed in a tooling review part of this article. Currently, there is a rich selection of tools, including Lerna, Yarn Workspaces, and Bazel.
* **Easier dependency management.** Only one _package.json_. No need to re-install dependencies in each repo whenever you want to update your dependencies.
* **Re-use code with shared packages while still keeping them isolated.** Monorepo allows you to reuse your packages from other packages while keeping them isolated from one another. You can use a reference to the remote package and consume them via a single entry point. To use the local version, you are able to use local symlinks. This feature can be implemented via bash scripts or by introducing some additional tools like Lerna or Yarn.

**Monorepo Disadvantages:**

* **No way to restrict access only to some parts of the app.** Unfortunately, you can’t share only the part of your monorepo—you will have to give access to the whole codebase, which might lead to some security issues.
*   **Poor Git performance when working on large-scale projects.** This issue starts to appear only on **huge** applications with more than a million commits and hundreds of devs doing their work simultaneously every day over the same repo. This becomes especially troublesome as Git uses a directed acyclic graph (DAG) to represent the history of a project. With a large number of commits, any command that walks the graph could become slow as the history deepens. Performance slows down as well because of the number of refs (i.e., branches or tags, solvable by removing refs you don’t need anymore) and amount of files tracked (as well as their weight, even though heavy files issue can be resolved using [Git LFS](https://git-lfs.github.com/)).

    _**Note:** Nowadays, Facebook tries to resolve issues with VCS scalability_ [_by patching Mercurial_](https://code.fb.com/core-data/scaling-mercurial-at-facebook/) _and, probably soon, this won’t be such a big issue._
* **Higher build time.** Because you will have a lot of source code in one place, it will take way more time for your CI to run everything in order to approve every PR.
