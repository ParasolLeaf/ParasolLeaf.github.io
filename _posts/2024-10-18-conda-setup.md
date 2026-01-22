---
title: AnaConda 环境配置
date: 2024-10-18 18:45:00 +0800
categories: [环境配置, Conda]
tags: [Conda, Anaconda, 配置]
author: wyt
---

# Conda 环境配置及使用
Conda 是一个开源的包管理和环境管理工具，支持多种编程语言，如 Python、R 等。它的主要功能包括安装、更新、管理软件包，以及创建和管理虚拟环境。Conda 不仅可以从官方仓库安装 Python 包，还支持 C、C++ 等其他语言的库，使其成为跨语言开发的理想选择。
ps: 本流程采用win系统，mac或linux系统下载流程类似。

## A. conda的选择与安装下载

1. **Anaconda vs Miniconda**
    conda分为Anaconda与Miniconda，二者都是基于 Conda 的包管理与虚拟环境管理工具，但它们在安装体积和用途上有所不同。下面分别介绍两者的特点和适用场景。
    
    | **特性**           | **Anaconda**                               | Miniconda                                        |
    | ------------------ | ------------------------------------------ | ------------------------------------------------ |
    | **大小**           | 3-4 GB                                     | 约400MB                                          |
    | **预装包数量**     | 超过 1,500 个科学计算和数据分析包          | 仅包含 Conda 和 Python                           |
    | **安装时间**       | 较长                                       | 较快                                             |
    | **灵活性**         | 预装大量工具，开箱即用                     | 需要用户手动安装需要的包，灵活配置               |
    | **图形界面支持**   | 包含 Anaconda Navigator 图形界面           | 不支持，需通过命令行操作                         |
    | **适用场景**       | 数据科学、机器学习、初学者快速搭建开发环境 | 高级用户、服务器部署、对磁盘空间要求高的场景     |
    | **主要优势**       | 开箱即用，适合数据科学和机器学习项目       | 轻量、灵活，适合需要精细化控制包管理的用户       |
    | **资源占用**       | 高，占用更多的磁盘空间和系统资源           | 低，占用较少的磁盘空间和资源                     |
    | **安装包管理方式** | 包含预装的包，但可通过 Conda 管理          | 用户完全手动安装需要的包                         |
    | **适合用户**       | 初学者、希望快速使用科学计算工具的用户     | 有经验的开发者、需要在服务器或云环境中运行的用户 |
    
    **notice:** 作为初学者，建议采用Anaconda。
    
2. **下载路径**
    
    对于Anaconda的下载，你可以选择在Anaconda官网下载，但官网的下载速度可能较慢，因此你也可以选择清华镜像站下载，链接如下：
    
    - 官网下载：[https://www.anaconda.com/products/distribution](https://www.anaconda.com/products/distribution)
    - 清华镜像站：[https://mirrors.bfsu.edu.cn/anaconda/archive/](https://mirrors.bfsu.edu.cn/anaconda/archive/)
    
    **notice:** 有关版本选择，建议选择最新的24年6月份的版本，以避免后续使用时遇到的第三方库下载可能遇到的Conda HTTP Error。
    
3. **安装包安装**
    
    **notice:** anaconda内已包含python，若你的电脑之前已单独安装了python，建议在安装anaconda前卸载python。（不卸载理论上应该也不会有冲突）
    
    下面流程以win系统为例，mac或linux等其他系统的流程类似。
    
    ![组图1.png](/assets/img/conda/%E7%BB%84%E5%9B%BE1.png)
    
    有关安装时的一些可选项：
    
    - Install for Just Me / All Users
        
        Just Me仅为当前用户下载，All Users将为系统上的每个用户下载，所占用的磁盘空间会更大，安装时间会更长，请根据你的需要选择。
        
    - 安装位置
        
        默认装在C盘，建议修改安装路径（此安装路径配置环境变量时会使用）到其他磁盘下，如：D:\Anaconda
        
    - Create shortcuts / Add to PATH environment / Register Anaconda as default python / Clear package cache
        
        有关上述四个选项，其中Add to PATH建议不要勾选（后续会手动配置环境变量），其他三个请根据需要选择。
        
4. **环境变量配置 (win)**
    
    ![image.png](/assets/img/conda/image.png)
    
    找到此电脑，此电脑(右键)→属性→高级系统设置→环境变量→系统变量中找到Path→编辑→新建下列变量：此时路径的前缀D:\Anaconda 为上述你安装conda时选择的路径。
    - D:\Anaconda
    - D:\Anaconda\Scripts
    - D:\Anaconda\Library\mingw-w64\bin
    - D:\Anaconda\Library\usr\bin
    - D:\Anaconda\Library\bin
    
    在配置好上述环境变量后，建议重启一下电脑。
    
5. **验证安装成功 (win)**
    
    ![image.png](/assets/img/conda/image%201.png)
    
    win+r 输入cmd，在cmd中输入`conda —version`查看conda版本信息，输入`python`检验python是否安装成功，输入`exit()`退出python。
    

## B. condarc文件配置

.condarc文件是conda运行期配置文件，其所在地址位于c盘当前用户的文件夹C:\Users\admin下。而.condarc文件默认是不会创建的，只有当用户第一次使用conda config命令时，系统才会自动创建该文件。

conda的配置命令大致分为两类，一类是添加命令，一类是设置命令。

- 添加：conda config --add [options] [parameters]
- 设置：conda config --set [options] [yes/no]

以下是相关的配置命令，直接在cmd中用户所在目录下输入下述命令即可，有兴趣的同学可以自己搜索相关文档查看具体信息：

- 添加通道channels：conda config --add channels defaults
- 添加环境目录envs_dirs：conda config --add envs_dirs [path]
- 添加pkgs_dirs：conda config --add pkgs_dirs [path]
- 设置ssl_verify：conda config --set ssl_verify yes
- 设置show_channels_url：conda config --set show_channels_url yes

为了方便配置，可以直接打开.condarc文件，将以下配置直接复制粘贴进去，此处添加的是清华镜像源，原版default channels速度较慢。
```plaintext
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  deepmodeling: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/
```

## C. conda的命令用法

以下是conda命令的分类和用途，可以帮助你有效管理包和环境，同学可以在命令行中加以尝试，看看能否正常创建环境并下载相关包：

1. **conda基本命令**
    - 查看conda版本：conda —version
    - 获取conda帮助：conda —help
    - 更新conda自身：conda update conda
2. **环境管理**
    ```bash
    # 创建新环境
    conda create —name [env_name]或conda create -n [env_name] 
    # 创建带特定版本的环境
    conda create -n [env_name] python=3.x
    # 列出所有环境
    conda env list 或 conda info —envs # 当你没有创建过新环境时，该列表下应该只有一个base环境
    # 激活环境
    conda activate [env_name]
    # 关闭当前环境
    conda deactivate
    # 删除环境
    conda remove -n [env_name] —all
    # 克隆环境
    conda create -n [new_env_name] —clone [old_env_name]
    # 导出环境，将当前环境导出为一个.yml文件
    conda env export > environment.yml
    # 根据.yml文件创建环境
    conda env create -f environment.yml
    # 更新现有环境
    conda env update -f encironment.yml
    ```

3. **包管理**（与pip等包管理工具类似）
    ```bash
    # 安装一个包，可以空格隔断一次安装很多包
    conda install [package_name]
    # 指定安装版本
    conda install [package_name]=[verison_number]
    # 升级包
    conda update [package_name]
    # 更新所有包
    conda update —all
    # 移除包
    conda remove [package_name]
    # 检查包是否已经安装
    conda list [package_name]
    # 列出当前环境所有已安装包
    conda list
    # 搜索可用包
    conda search [package_name]
    ```

4. **渠道管理**（在B.condarc文件配置中已有涉及）
    ```bash
    # 添加新渠道
    conda config —add channels [channels_name]
    # 移除渠道
    conda config —remove channels [channels_name]
    # 显示配置的所有渠道
    conda config —show channels
    # 设置频道优先级
    conda config —set channel_priority true
    ```

5. **系统信息**
    ```bash
    # 查看conda系统信息
    conda info
    ```

6. **检查冲突**
    ```bash
    # 查看包安装或更新时的潜在冲突
    conda install package_name —dry-run
    # 查看更新所有包时的潜在冲突
    conda update —all —dry-run
    ```

7. **清理缓存**
    ```bash
    # 删除未使用的包和缓存
    conda clean —all
    ```

8. **创建可共享的虚拟环境**
    ```bash
    # 将本环境信息输出到某个txt文件中
    conda list —explicit > [spec-file.txt]
    # 使用该txt文件创建一个环境
    conda create -n [env_name] —file [spec-file.txt]
    ```

## D. 本次配置功能检验

此时，你应该已经配置好了conda环境，下面你可以完整地在命令行中尝试以下过程：从激活conda→创建一个环境→激活新建的环境→在新环境中安装相关包。

1. **在cmd中激活conda**
    最初命令行前缀是没有(base)的，此时仍未激活conda环境，你需要conda activate激活环境（若报错，可能是没有conda init，或尝试使用activate指令代替conda activate，这与你的环境变量配置有关）
    ![image.png](/assets/img/conda/image%202.png)
    
2. **查看已有哪些虚拟环境**
    此时激活后，命令行前缀有(base)，此时可使用conda env list查看已有哪些虚拟环境，*号所标注的是当前环境，比如当前激活的是base虚拟环境
    ![image.png](/assets/img/conda/image%203.png)
    
3. **创建一个虚拟环境test**
    使用conda create命令创建一个虚拟环境
    ![image.png](/assets/img/conda/image%204.png)
    
4. **激活新创建的虚拟环境test**
    conda activate test。此时，会发现命令行前缀由(base)转变为(test)。
    ![image.png](/assets/img/conda/image%205.png)
    
5. **conda list查看一个虚拟环境中有哪些包**
    ![image.png](/assets/img/conda/image%206.png)
    
6. **conda install在虚拟环境中下载包（也可使用pip install等）**
    ![image.png](/assets/img/conda/image%207.png)
    
7. **关闭虚拟环境test，并删除该虚拟环境**
    ![image.png](/assets/img/conda/image%208.png)
    

到此为止，恭喜你已经成功配置好了conda环境！！！
- 实际使用时，针对不同任务时，你可创建不同的虚拟环境以供使用，以防止从始至终都是一个环境所可能导致的包与包之间的版本冲突。
- 有关程序的运行，你可以在IDE中添加python解释器的时候选择你已经建好的conda环境然后使用IDE可视化运行程序；也可以直接在命令行中conda activate XXX, 激活你的conda环境，并在你的环境下命令行运行python xxx.py运行你的程序。
- 有关IDE工具，自由度较高，同学可以使用vscode，pycharm等各类你熟悉的IDE开发工具。
如果途中有遇到环境配置的问题，可以灵活使用网络搜索解决方案，祝你顺利掌握使用conda！部分问题及解决方案见：
[Conda 使用过程中的常见报错与解决记录]({{ '/posts/conda-errors/' | relative_url }})