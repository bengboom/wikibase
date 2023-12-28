Shell是指操作系统的命令解释器或命令行界面。它是用户与操作系统交互的桥梁，允许用户通过输入命令来执行操作。例如，在Unix和Linux系统中，常见的Shell有Bash、sh、csh、zsh等。
“Shell”一词来源于它的外围性质——它像是围绕着操作系统核心的“壳”，提供了与内核进行交互的接口。操作系统的内核（Kernel）负责管理计算机硬件资源和运行程序，而Shell则是用户与内核沟通的中介。
# Shell Basic

## File

```bash
ls # list files

ls -l # list files in long format

ls -a # list all files including hidden files

ls -al # list all files in long format

ls -lh # list files in long format with human readable size

ls -R # list files recursively

ls -t # list files by time

ls -S # list files by size

ls -r # list files in reverse order

ls -d */ # list directories only
```

```bash
pwd # print working directory

cd # change directory

.. # parent directory

. # current directory

~ # home directory

cd - # change to previous directory

cd ~ # change to home directory

mkdir # make directory

rmdir # remove empty directory

rm -r # remove directory and its contents

rm -f # force remove

rm -rf # force remove directory and its contents
```

## Stream

```bash
cat # concatenate files and print on the standard output

cat -n # print line number

cat -b # print non-blank line number

cat -s # squeeze multiple blank lines into one

cat -E # display $ at the end of each line

cat -T # display TAB characters as ^I

cat -A # display non-printing characters as well

cat -v # display non-printing characters as ^ and M-
```

```bash
head # output the first part of files

head -n # output the first n lines

head -c # output the first n bytes

tail # output the last part of files

tail -n # output the last n lines

tail -c # output the last n bytes

tail -f # output appended data as the file grows
```

## Pipe

```bash
| # pipe

|& # pipe both stdout and stderr

< # redirect stdin     Input of A < is output of B

\>  # redirect stdout    Output of A > as input of B
```

## Search

```bash
grep # print lines matching a pattern

grep -v # print lines not matching a pattern

grep -c # print only a count of matching lines

grep -i # ignore case distinctions

grep -l # print only names of files containing matches

grep -n # print line number with output lines

grep -r # recursively search subdirectories

grep -w # match the whole word

grep -x # match the whole line

grep -E # use extended regular expressions

grep -F # use fixed strings instead of regular expressions

grep -A # print lines after matching lines

grep -B # print lines before matching lines

grep -C # print lines before and after matching lines
```

## Process

```bash
ps # report a snapshot of the current processes

ps -e # display all processes
```
