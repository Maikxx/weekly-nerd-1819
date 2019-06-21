# Version control guidelines

This is one of three articles regarding the experiences that I gathered in the work field and during the minor web development. Some things are new to me, for example testing applications, while others, like this one I frequently use myself and have learned at [Lifely](https://lifely.nl/).

This article focusses on the preferred way to handle version control in a multi-developer environment. This is interesting, since probably every developer in the minor web development at this moment will eventually have to work together with other people inside of one repository, this will create havoc if not guided.

## Table of Contents

* [Branching](#Branching)
    * [Tooling](#Tooling)
    * [How-to](#How-to)
    * [Types of branches](#Types-of-branches)
        * [Master branch](#Master-branch)
        * [Develop branch](#Develop-branch)
        * [Named branches](#Named-branches)
    * [Pull requests](#Pull-requests)
* [Committing](#Committing)
    * [Action](#Action)
    * [Scope](#Scope)
    * [Description](#Description)

## Branching

You enter a project, there are multiple developers working on this same project and no one uses branches.
This situation is highly unlikely, since most developers have probably some unique way to use branches to structure code projects.
While there are certain conventions for commit messages in git, I did not find that much guidelines the way to use branching in git, including naming conventions.

Here is a list of other conventions for branching that I quickly found by [DuckDuckGo](https://duckduckgo.com/)-ing:

* [https://allenan.com/git-branch-naming-conventions/]()
* [https://gist.github.com/digitaljhelms/4287848]()
* [https://gist.github.com/revett/88ee5abf5a9a097b4c88]()
* [http://guyroutledge.co.uk/blog/git-branch-naming-conventions/]()

As you can see there are a ton of different ways to tackle this problem. Personally, I use the bare minimum, with some alterations, [Lifely]([Lifely](https://lifely.nl/)) code standards, which are based on [Angular](https://angular.io/), yes, I know, Angular and code standards...

### Tooling

When using branches, I suggest using a tool called [Fork](https://git-fork.com/) or [SourceTree](https://www.sourcetreeapp.com/) to easily get an overview of the full project.

Simply follow the installation steps and open the project inside one of these programs to get started.

You could also opt to use only the CLI, that is fine too, just please then use [Iterm](https://iterm2.com/) instead of the crappy built in terminal on Macs.
If you don't use a Mac I don't know how it all works, you will need to figure that out yourself.

### How-to

Creating new branches is fairly simple (Most of these commands can also be done with the tools provided above):

1. You change to the branch where you want to branch off from. `git checkout <branchName>`.
2. Create a new branch from your current branch and switch to that branch. `git checkout -b <newBranchName>`.

For more information on git itself checkout the [documentation](https://www.git-scm.com/).

**Warning**

Never delete a branch with `git branch -D <branchName>`.
This can also delete the current branch that you are working on, and it can also delete te master branch, which you know, gives trouble unless the project is backed up somewhere.

### Types of branches

Branches are in itself all equal, which is of not much use to developers.
That is why there is a smart branch naming system.

Branch naming in itself does nothing to enforce the commits pushed on it, however when using a tool such as [Fork](https://git-fork.com/) or [SourceTree](https://www.sourcetreeapp.com/), these branches become _folder-like_, which is very useful to reduce clutter on the `origin` branch.

The default branches are `master` and `develop`.
Every new git project has a master branch. Develop is a branch that you will need to create yourself.

#### Master branch

The master branch is a branch on which should not be committed directly.
You can enforce rules on branches on GitHub, by following [this tutorial](https://help.github.com/en/articles/enabling-branch-restrictions).

This branch is for release versions of the software that is being created.
There should be no explicitly known bugs in the code of this branch.

#### Develop branch

This is the branch that is being staged for release (or in English, this will be merged soon into master).
Technically this is not right for all projects, because sometimes you will want to have `release/` branches as well.
This branch should not be restricted from being pushed to, since some commits are typically allowed. Allowed commits include one of the following [actions](#Action): `chore`, `fix` and `copy`.

Features and refactors should almost never be pushed to this branch, but instead should be created inside `feature/<branchName>` and `refactor/<branchName>` branches, respectively, to reduce clutter on this branch.

It is in no ones interest to be required to stash your current changes and then pull the branch to then unstash your changes to solve merge conflicts. That is why using named branches is a must.

#### Named branches

Branches can be named (wow, I know), but did you also know you could use _slashes_ in that name?
I for one did not until a few months ago.

Version control tools such as the ones mentioned above provide a folder like structure when using these kinds of branches, which is nice.

The semantic branch names I personally use are someway along this line (these are based on Lifely and Angular, with some minor tweaks):

* `feature/` - Branch for specific features.
    Avoid hitting files outside of the scope of this branch.
    Example: `feature/client-app-view-integration`. (Branched from `develop`)
* `refactor/` - Branch for specific refactors (rewrites of code).
    Avoid hitting files outside of the scope of this branch.
    Example: `refactor/client-app-view-integration`. (Branched from `develop`)
* `fix/` - Branch for specific fixes. Avoid hitting files outside of the scope of this branch.
    Example: `fix/client-app-view-integration`.
    These type of branches will be less common, since it is also allowed to perform fixes on `develop`. (Branched from `develop`)
* `hotfix/` - Branch for **high priority fixes**, that are already on the `master` branch.
    These type of branches should really never occur, since all code and designs going to `master` should already be tested.
    Hotfix branches generally have different names from normal branches, I have not thought out this far yet.
    If it occurs that you need it and you read this, ask me again what naming conventions these should follow.
    Everything is allowed in this branch, as long as it aims to fix **high priority bugs** on `master`. (Branched from `master`)

As you can see not all cases are covered with these branch names, for example adding a new dependency. This is in my opinion not immediately a new feature in itself.

You could opt for more branch naming conventions, like `root/` or `chrome/`.

### Pull requests

When you are finished working on a specific branch, feel free to create a pull request on GitHub with a descriptive **name**, **description** containing explanation on what has been changed and a **reviewer**.

You will probably always want to choose `develop` as your **base** branch and the branch you want to merge as the **compare** branch, except for the case when merging a `hotfix` branch.

When merging branches, you come to one of the most crappy things about working with any source control... **merge conflicts**.
This happens when two people have worked on the same file, on the same line where git doesn't know what version to take.
Most of the time these are pretty easy to fix, often you can _take both_, for other cases where you get stuck, let the other developer know that touched that line and they will probably help you out.

[VSCode](https://code.visualstudio.com/) has got a pretty decent merge conflict overview tool, while some of the other tools mentioned above are good too.

## Committing

In contrast to the pretty _"random"_ branch naming conventions explained above, commit messages have some more proven backers.

Here is a list of some further research in my preferred way of writing commit messages and why you would opt for that:

* [https://christianlydemann.com/versioning-your-angular-app-automatically-with-standard-version/]()
* [https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit]()
* [https://github.com/semantic-release/semantic-release]()

The preferred way to write version control (according to me and the sources listed above) messages is as follows: `action(scope): description`.

### Action

The action in the phrase can be one of the following:

* `chore` - Use this if you install new dependencies or change things to the configuration of the application. **These changes need to be pushed on a separate commit from other changes.**
* `feature` - You will probably use this most of the time. This is for the cases when you add something new to the application.
* `fix` - If there is something broken that has already been pushed and you fixed that.
* `refactor` - Most of the time you use this when you rewrite a piece of code so that it looks better, or is written in a nicer way.
* `copy` - Use this if you only change the copy of a piece of text in the application.

As you can see I don't use the `docs` action, because in my opinion you add something new to a file, which I can see as a `feature`. You could choose to use `docs` anyways though.

**Note on multiple actions at once**

Preferably split commits up into separate of these actions.

If for some reason this is not possible or preferable, use the one on top of the other one.
Like so: `feature` > `fix` or `feature` > `refactor`.
Where the left one in these cases is the one you are going to use.

### Scope

The scope of the phrase above is simply the part of the application where you changed things.
In most guidelines, this is optional, I don't like it that way. The scope part is mandatory to get a quick grasp of what has been done in that commit.
This can be either one of the following:

* `docs` - If you change something to the global documentation, including `BRIEFING.md` files.
* `deps` - For the case where dependencies change. Used in combination with chore.
* `client > *` - For cases where you change something in the client folder. The asterisks stands for the part of the application that you changed. For example: `feature(client > home): add homepage`.
* `server > *` - For cases where you change something in the server folder. The asterisks stands for the part of the application that you changed. For example: `feature(server > api): integrate api`.
* `*` - For cases where you just don't give a shit, mostly used when you have tried to fix something multiple times, when it clearly still doesn't work each time.

### Description

This is the easiest of the three pieces, here you just type the message, starting with a _lowercase_, ending without a dot.

Please be descriptive, so: `feature(client > home): add homepage to views folder`, not `feature(client > home): 💩`.

Also it would be nice to not write commit messages the size of a book, keep it short and simple, while also spot-on.