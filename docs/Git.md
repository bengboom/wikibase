为什么要把git放在知识库的根目录？足以说明其重要性

**未同步到remote repository的所有工作进度一律不算真正的工作进度**
**写完代码要顺便add commit push 以防万一！**

> 20231203
> 我在某项目中使用ssh连接远程HPC服务器写代码。
> 因为当时运行代码的sh脚本的output和error命名有点乱，我就改了命名格式，然后把之前乱命名的输出文件删掉了。
> 我是按着shift多选之后点的删除，但不小心把主文件夹也点到了，导致整个文件夹没了。
>
> git repo的备份只建了仓库，后续没有再commit过，所以恢复不了现场。
> 从我以前和chatgpt的对话中恢复了很小一部分。
> 剩下的不知道怎么办了。

- **git add**
	- 此命令用于将文件的改动添加到暂存区（staging area）。在提交改动之前，你需要使用 `git add` 命令来指定哪些文件的更改会被包含在即将进行的提交中。这个步骤可以让你有选择性地添加文件的改动，而不是一次性提交所有改动。
- **git commit**
	- 这个命令用于将暂存区中的改动提交到本地仓库。提交（commit）是 Git 中的一个核心概念，它记录了对代码库的一次显著修改。每个提交都会有一个独特的ID（通常称为提交哈希）和相关的元数据，如作者、日期和提交信息。
- **git push**
	- 此命令用于将本地仓库中的提交推送到远程仓库。这是与团队成员共享代码更改的主要方式。例如，当你完成一个功能的开发并提交到你的本地仓库后，你可以使用 `git push` 命令将这些更改推送到远端，使其他团队成员能够看到和访问这些更改。