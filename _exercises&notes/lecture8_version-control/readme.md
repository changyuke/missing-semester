







***

#### FAQ

##### **Q：git通过差异文件实现版本同步**

:robot:

> 是的，您可以通过导入差异文件来更改代码，而不必手动修改每个文件。GitLab提供了许多工具和方法来处理代码的合并和同步。以下是使用GitLab进行代码同步的一般步骤：
>
> 1. **克隆代码仓库：** 在另一个电脑上，首先克隆包含您想要同步代码的GitLab仓库。使用Git命令行或Git客户端，在目标电脑上执行类似以下的命令：
>
> ```bash
> git clone <repository_url>
> ```
>
> 2. **切换到分支：** 确保您在目标电脑上切换到正确的分支，这样您可以将差异文件应用到正确的分支上。
>
> ```bash
> git checkout <branch_name>
> ```
>
> 3. **导入差异文件：** 将从GitLab上下载的差异文件保存为.patch或.diff格式。然后，在目标电脑上使用以下命令应用差异文件：
>
> ```bash
> git apply <path_to_diff_file>
> ```
>
> 这将根据差异文件中的更改，将代码应用到目标分支上。
>
> 4. **提交更改：** 确认差异文件的更改是否正确应用，然后使用Git提交这些更改：
>
> ```bash
> git add .
> git commit -m "Apply changes from GitLab diff file"
> ```
>
> 5. **推送更改：** 最后，将更改推送回GitLab仓库：
>
> ```bash
> git push origin <branch_name>
> ```
>
> 这样，您就可以成功地将差异文件中的更改应用到另一个电脑的代码库中。
>
> 请注意，使用差异文件导入更改有时可能会有冲突或其他问题，特别是当代码库在您应用差异文件之后已经有了其他更改。因此，建议在使用此方法之前备份代码库，并确保对代码库中的更改和版本控制有充分的理解。如果您在合并过程中遇到冲突，可以使用Git的合并工具（如`git mergetool`）来解决冲突。

过程：一直一直无法下载从gitlab上下载差异文件，直接从公司电脑上`git diff <commit id> <commit id>`生成了差异文件，但是`git apply`发现无法应用，报错为找到提交的分支。新增了文件 `git clone`了M171远程仓库

##### **Q：仓库子目录中`git clone`后提交报错**

错误信息

```bash
.../M171_Maintenance> git add .
warning: adding embedded git repository: M171_MCU/MCU_ACU202
hint: You've added another git repository inside your current repository.
hint: Clones of the outer repository will not contain the contents of
hint: the embedded repository and will not know how to obtain it.
hint: If you meant to add a submodule, use:
hint:
hint:   git submodule add <url> M171_MCU/MCU_ACU202
hint:
hint: If you added this path by mistake, you can remove it from the
hint: index with:
hint:
hint:   git rm --cached M171_MCU/MCU_ACU202
hint:
hint: See "git help submodule" for more information.
```

:robot:

> 在执行 `git add .` 命令时，您遇到了一个警告，指示在当前代码库中添加了另一个嵌套的 Git 仓库。这通常发生在您尝试将另一个 Git 仓库作为子模块添加到当前仓库中，但是使用了错误的添加方式。
>
> 警告信息建议您使用 `git submodule add` 命令来将该嵌套仓库作为子模块添加到当前仓库中，而不是直接使用 `git add .` 命令来添加。
>
> 子模块是 Git 仓库中的一个重要概念，它允许您将一个 Git 仓库嵌套到另一个仓库中，并在该仓库中跟踪子模块的版本和更改。通过子模块，您可以在一个仓库中引用另一个独立的仓库，方便代码的复用和管理。
>
> 要将该嵌套仓库作为子模块添加到当前仓库中，您可以按照警告信息中给出的建议执行以下步骤：
>
> 1. 移除已经添加的嵌套仓库：
>
> ```bash
> git rm --cached M171_MCU/MCU_ACU202
> ```
>
> 2. 使用 `git submodule add` 命令添加该嵌套仓库作为子模块：
>
> ```bash
> git submodule add <url> M171_MCU/MCU_ACU202
> ```
>
> 将 `<url>` 替换为嵌套仓库的远程 URL 地址。
>
> 3. 提交更改：
>
> ```bash
> git commit -m "Add submodule: M171_MCU/MCU_ACU202"
> ```
>
> 4. 推送更改（可选）：
>
> 如果当前仓库是远程仓库，并且您希望将子模块的更改一并推送到远程仓库中，执行：
>
> ```bash
> git push origin <branch_name>
> ```
>
> 通过以上步骤，您将成功将该嵌套仓库添加为子模块，并使得在克隆当前仓库时也会获取子模块的内容。

##### Q：子模块相关（初始化、拉取等等）

:robot:

> 在 Git 中，子模块（Submodule）是一种特殊的仓库，它可以嵌套在一个主仓库中。当你克隆（clone）或拉取（pull）一个包含子模块的主仓库时，默认情况下，子模块的内容并不会自动跟着一起拉取。
>
> 要拉取主仓库和子模块的内容，你需要执行额外的命令来初始化子模块并获取它们的内容。以下是在拉取包含子模块的主仓库时，如何拉取子模块内容的步骤：
>
> 1. 首先，克隆主仓库到本地：
>
>    ```bash
>    git clone <repository_url>
>    ```
>
> 2. 进入克隆下来的主仓库目录：
>
>    ```bash
>    cd <repository_name>
>    ```
>
>    `<repository_name>` 是克隆的仓库的名称。
>
> 3. 初始化子模块：
>
>    ```bash
>    git submodule init
>    ```
>
>    这将初始化主仓库中的子模块，准备拉取子模块的内容。
>
> 4. 拉取子模块的内容：
>
>    ```bash
>    git submodule update
>    ```
>
>    或者你也可以使用以下命令来合并上述两个步骤：
>
>    ```bash
>    git submodule update --init
>    ```
>
>    这将拉取主仓库中的所有子模块的内容。
>
> 通过执行以上步骤，你可以将主仓库和所有子模块的内容一起拉取到本地。请注意，每个子模块都是一个独立的仓库，它们具有自己的提交历史和版本控制。因此，在拉取子模块的内容时，Git 会获取子模块仓库中的最新版本。
>
> 如果主仓库的子模块发生了变化（例如，子模块的提交被更新），你需要定期更新子模块。可以使用以下命令来更新子模块的内容：
>
> ```bash
> git submodule update --remote
> ```
>
> 这将更新主仓库中的所有子模块到最新的远程提交。如果你只想更新单个子模块，可以在命令后面指定子模块的路径。
>
> 总结起来，主仓库和子模块的内容是相互独立的，需要执行额外的命令来获取子模块的内容。在拉取主仓库时，并不会自动拉取子模块的内容。

##### Q：公司电脑里面program一直提交不了，也找不到`.gitmodule`和`.gitignore`文件

:robot:

> `.gitmodules` 文件是一个位于 Git 仓库根目录下的隐藏文件，用于记录子模块的信息和配置。如果你的项目使用了子模块（Submodule），应该能在 Git 仓库的根目录下找到 `.gitmodules` 文件。
>
> 但要注意，`.gitmodules` 文件不是在子模块目录中，而是在包含子模块的主仓库根目录下。
>
> 如果你在项目根目录下执行以下命令，应该能找到 `.gitmodules` 文件：
>
> ```bash
> ls -a | grep .gitmodules
> ```
>
> 或者在 Windows 系统的 Git Bash 或命令提示符下，使用以下命令查找 `.gitmodules` 文件：
>
> ```bash
> dir /ah
> ```
>
> 请确保在 Git 仓库的根目录下查找 `.gitmodules` 文件，而不是在子模块目录中查找。如果你仍然找不到 `.gitmodules` 文件，可能说明该项目没有使用子模块。**(没找到)**

**Q：push S32k_application到远程一直失败报错信息显示”文件大小超过限制“**

:robot:

> 根据输出信息，你在推送到远程仓库时仍然遇到了文件大小超过 GitHub 限制的问题。GitHub 对单个文件的大小有限制，超过限制的文件将无法直接推送到远程仓库。
>
> 在你的错误提示中，有两个文件大小超过了 GitHub 的限制：
>
> 1. `AT202_learn/backup/5.5.1.rar` 文件大小为 74.63 MB，超过了 GitHub 推荐的最大文件大小 50.00 MB。
> 2. `AT202_learn/backup/program(20230317).rar` 文件大小为 439.84 MB，超过了 GitHub 的文件大小限制 100.00 MB。
>
> 为了解决这个问题，你需要使用 Git Large File Storage (LFS) 来管理大型文件。Git LFS 是一个扩展，用于存储大型文件，使它们不会直接存储在 Git 仓库中。
>
> 请按照以下步骤来使用 Git LFS：
>
> 1. 安装 Git LFS：首先，确保已经安装了 Git LFS。你可以在 https://git-lfs.github.com/ 下载适合你操作系统的版本并进行安装。
>
> 2. <font color = Red>**启用 Git LFS**</font>：在你的项目根目录下，执行以下命令以启用 Git LFS：
>
>    ```bash
>    git lfs install
>    ```
>
> 3. 将大文件添加到 Git LFS：将大文件添加到 Git LFS 跟踪列表中，让 Git LFS 来管理这些文件。运行以下命令：
>
>    ```bash
>    git lfs track "AT202_learn/backup/5.5.1.rar"
>    git lfs track "AT202_learn/backup/program(20230317).rar"
>    ```
>
>    请根据实际情况替换文件路径。
>
> 4. 提交更改并推送到远程仓库：
>
>    ```bash
>    git add .
>    git commit -m "Add large files using Git LFS"
>    git push origin main
>    ```
>
> 通过使用 Git LFS，你应该能够成功推送包含大文件的提交到 GitHub 远程仓库，并绕过 GitHub 的文件大小限制。请注意，使用 Git LFS 时，大文件不会直接存储在 Git 仓库中，而是存储在 Git LFS 服务器上，并且只会在需要时下载到本地。

##### Q：我的子模块AT202一直退不了远程

:robot:

> 在 Git 中，`master` 分支通常是一个特殊的分支，它用于表示主要开发分支，也是默认的主分支名称。当你在主分支（通常是 `master`）上进行开发时，通常希望 `master` 分支保持与最新的提交（即 `HEAD` 指向的提交）一致。
>
> 如果你发现 `master` 分支落后于 `HEAD`，可能是因为在其他分支上进行了开发，或者你在 `master` 分支上的提交没有及时合并到最新的提交。为了使 `master` 分支指向最新的 `HEAD`，你可以使用以下方法之一：
>
> 方法一：使用 `git merge` 合并最新的 `HEAD` 到 `master` 分支：
>
> 1. 确保你在 `master` 分支上：
>
>    ```bash
>    git checkout master
>    ```
>
> 2. 合并 `HEAD` 到 `master`：
>
>    ```bash
>    git merge HEAD
>    ```
>
> 方法二：使用 `git reset` 将 `master` 分支重置到最新的 `HEAD`：
>
> 1. 确保你在 `master` 分支上：
>
>    ```bash
>    git checkout master
>    ```
>
> 2. 重置 `master` 分支到 `HEAD`：
>
>    ```bash
>    git reset --hard HEAD
>    ```
>
> 无论选择哪种方法，都会将 `master` 分支指向最新的 `HEAD` 提交。在执行这些操作之前，请确保你没有未提交的更改，因为 `git merge` 或 `git reset` 都可能导致分支上的更改丢失。
>
> 记住，当你将 `master` 分支更新为最新的 `HEAD` 时，这意味着你的 `master` 分支将与最新的开发状态保持一致，可能包含了其他分支上的最新更改。确保你在更新 `master` 分支之前理解这些更改，并准备好解决任何可能的冲突。

:man_astronaut:

> 之前一直是
>
> ```
> （HEAD）
> 
> （origin）
> 
> （master）
> ```
>
> 这样的状态一直搞不好，报错本地分支落后于远程，用了上面的办法 `git checkout master` 后 `git pull` 发现可以拉取了。
>
> 猜测是不是因为之前没有添加好子模块导致，pull的时候分支不对，所以一直不行
>
> 切换master后pull在checkout head应该就可以了:x:
>
> 原来是弄出了两个分支

:woman_astronaut:

> 应该从报错信息里找到问题了？或许报错的地址为
>
> ```bash
> .../S32K144_Application_notes> git push origin main
> Enumerating objects: 6780, done.
> Counting objects: 100% (6780/6780), done.
> Delta compression using up to 8 threads
> Compressing objects: 100% (3840/3840), done.
> Writing objects: 100% (6749/6749), 877.86 MiB | 685.00 KiB/s, done.
> Total 6749 (delta 2835), reused 6392 (delta 2689), pack-reused 0
> remote: Resolving deltas: 100% (2835/2835), completed with 18 local objects.
> remote: warning: File AT202_learn/backup/5.5.1.rar is 74.63 MB; this is larger than GitHub's recommended maximum file size of 50.00 MB
> remote: error: Trace: 4199939233f4a10f6c9bdfdeba7997dd3713103cffa4ec34ad8e406e09e0ebc4
> remote: error: See https://gh.io/lfs for more information.
> remote: error: File AT202_learn/backup/program(20230317).rar is 439.84 MB; this exceeds GitHub's file size limit of 100.00 MB
> remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
> To github.com:changyuke/S32K144-Application-notes.git
>  ! [remote rejected] main -> main (pre-receive hook declined)
> error: failed to push some refs to 'github.com:changyuke/S32K144-Application-notes.git'
> ```
>
> AT202_learn/backup

:woman_astronaut:

> md烦死了，存个报错消息
>
> ```bash
> .../S32K144_Application_notes> git push origin main
> 
> Enumerating objects: 6786, done.
> Counting objects: 100% (6786/6786), done.
> Delta compression using up to 8 threads
> Compressing objects: 100% (3845/3845), done.
> Writing objects: 100% (6755/6755), 877.86 MiB | 684.00 KiB/s, done.
> Total 6755 (delta 2838), reused 6392 (delta 2689), pack-reused 0
> remote: Resolving deltas: 100% (2838/2838), completed with 17 local objects.
> remote: warning: File AT202_learn/backup/5.5.1.rar is 74.63 MB; this is larger than GitHub's recommended maximum file size of 50.00 MB
> remote: error: Trace: 83d49d0d21e4c125d88dbf59a6e7ce74e408138e745cef0ea84d28185b7ea209
> remote: error: See https://gh.io/lfs for more information.
> remote: error: File AT202_learn/backup/program(20230317).rar is 439.84 MB; this exceeds GitHub's file size limit of 100.00 MB
> remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
> To github.com:changyuke/S32K144-Application-notes.git
>  ! [remote rejected] main -> main (pre-receive hook declined)
> error: failed to push some refs to 'github.com:changyuke/S32K144-Application-notes.git'
> ```

:robot:

> 是的，你的猜测可能是正确的。当你尝试将代码推送到远程仓库时，如果有较大的文件存在于你之前提交的版本中，但没有被正确处理，那么在推送当前版本时可能会因为之前版本中的大文件而导致推送失败。
>
> GitHub 和其他 Git 托管服务通常有文件大小限制，通常是 100 MB（某些服务可能有不同的限制）。如果你之前提交的版本包含超过限制的大文件，那么推送时会被拒绝，无法成功上传这些大文件。
>
> 为了解决这个问题，你需要执行以下步骤：
>
> 1. 清理之前提交中的大文件：首先，确保你的当前版本不包含大文件。然后，你需要修复之前的提交，将大文件从这些提交中移除。可以使用 Git 的历史重写操作，如 `git rebase` 或 `git filter-branch` 来处理之前的提交，并将大文件移除。
>
> 2. 同步远程仓库：在清理之前提交的大文件后，确保你的本地仓库与远程仓库保持同步。可以使用 `git pull` 来拉取最新的更改，并确保没有冲突。
>
> 3. 推送当前版本：一旦确保之前提交中的大文件已经移除，现在你可以尝试推送当前版本了。
>
> 请注意，历史重写操作是一种强大且潜在危险的操作。在进行历史重写之前，请确保你充分了解其影响，并备份重要数据。如果你不确定如何进行历史重写操作，最好寻求专业人士的帮助，或者在 Git 托管服务上创建一个新的仓库，确保新的仓库中不包含大文件，然后重新推送你的代码。

##### Q：如何撤销更改，以及`git reset`和`git checkout`得区别

:robot:

> `git reset` 和 `git checkout` 是两个在 Git 中用于操作代码库的命令，它们有不同的功能和用途。
>
> 1. `git reset`：
>    - 功能：`git reset` 用于移动分支引用或重置分支到指定的提交。
>    - 主要用途：主要用于修改提交历史或撤销提交。
>    - 操作对象：可以用于分支、提交、或者回退暂存区（`git reset HEAD`）和工作区（`git reset --hard HEAD`）。
>    - 注意事项：`git reset` 是一个强制性的命令，执行后会改变历史，慎用。如果已经推送到远程仓库，不推荐在公共分支上使用 `git reset`，因为它会影响其他开发者。
>
> 2. `git checkout`：
>    - 功能：`git checkout` 用于切换分支、查看历史提交或恢复文件到指定的提交。
>    - 主要用途：主要用于切换分支、查看历史提交和恢复文件。
>    - 操作对象：可以用于分支、提交、或者单个文件。
>    - 注意事项：`git checkout` 并不会改变提交历史，而是用于在不同分支之间切换或者查看不同提交的状态。如果在已修改的文件上使用 `git checkout`，它会丢弃工作区对该文件的修改。
>
> 要记住的关键区别是，`git reset` 可以更改提交历史，而 `git checkout` 主要用于切换分支和查看提交状态，不会改变提交历史。另外，`git reset` 在使用时要特别小心，确保你了解其影响，并在必要时备份你的重要数据。

##### 对历史版本进行变基--`git rebase`

:computer:

> http://jartto.wang/2018/12/11/git-rebase/

##### Commit message 和 Change log 编写指南

:computer:

> [Commit message 和 Change log 编写指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
>
> [你可能会忽略的 Git 提交规范](http://jartto.wang/2018/07/08/git-commit/)