# Greenfield 🌳 🌳 🌳

Guidelines for new projects at Ordermentum

- [Git](#git)
- [Documentation](#documentation)
- [Environments](#environments)
- [Dependencies](#dependencies)
- [Testing](#testing)
- [Structure and Naming](#structure-and-naming)
- [Code style](#code-style)
- [Logging](#logging)
- [API Design](#api-design)
- [Licensing](#licensing)

----
## 1. Git <a name="git"></a>

### 1.1 Git Workflow
We use [Feature-branch-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) with [Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing) and some elements of [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow) (naming and having a develop branch). The main steps are as follow:

* Checkout a new feature/bug-fix branch
    ```sh
    git checkout -b <branchname>
    ```
* Make Changes
    ```sh
    git add
    git commit -m "<description of changes>"
    ```
* Sync with remote to get changes you’ve missed
    ```sh
    git checkout develop
    git pull -r
    ```
* Update your feature branch with latest changes from develop by rebasing ([More info](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing))
    ```sh
    git checkout <branchname>
    git rebase -i origin/develop
    ```
* If you don’t meet conflicts skip this step. If you have conflicts, resolve them either using your code editor, or the [CLI](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/)  , and then continue rebasing
    ```sh
    # resolve conflicts
    git add <file1> <file2> ...
    # NB. do NOT `git commmit`
    git rebase --continue
    ```
* Push your branch. Rebase will change history, so you'll have to use `-f` to force changes into the remote branch. If someone else is working on your branch, use the less destructive `--force-with-lease` ([Here is why](https://developer.atlassian.com/blog/2015/04/force-with-lease/)).
    ```sh
    git push -f origin/<branchname>
    ```
* Make a Pull Request.
* Pull request will be accepted, merged, and closed by reviewer.
* Remove your local feature branch if you're done.


### 1.2 Commit messages

* Capitalize the subject line
* Do not end the subject line with a full-stop
* Emojis should be used wisely
* Commit messages are not always for story time

---
Sources:
[Hive](https://github.com/wearehive/project-guidelines),
[RisingStack Engineering](https://blog.risingstack.com/),
[Mozilla Developer Network](https://developer.mozilla.org/),
[Heroku Dev Center](https://devcenter.heroku.com),
[Airbnb/javascript](https://github.com/airbnb/javascript),
[Atlassian Git tutorials](https://www.atlassian.com/git/tutorials)
