## Anaconda简介
Anaconda是一个用于科学计算的Python和R语言的开源发行版，包含了conda、Python及众多科学包。

## Conda基础
Conda是Anaconda附带的包和环境管理器，用于安装、运行和更新包及其依赖。

## 虚拟环境管理
- 创建环境：`conda create --name myenv`（可指定Python版本）
- 列出环境：`conda env list` 或 `conda info --envs`
- 激活环境：`conda activate myenv`
- 退出环境：`conda deactivate`
- 删除环境：`conda remove --name myenv --all`

## 包管理
- 安装包：`conda install numpy`
- 列出包：`conda list`
- 更新包：`conda update numpy`
- 卸载包：`conda remove numpy`

## 实践应用
使用Anaconda为每个项目创建独立的环境，以管理不同项目的依赖和避免冲突。
