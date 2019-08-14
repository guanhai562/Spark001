# Task01_Spark之Window

前言:根据task中这篇文章写的[Spark之Window](https://blog.csdn.net/SummerHmh/article/details/89518567)

### JDK部署(略)

### Spark部署

* spark-2.4.1下载

  https://archive.apache.org/dist/spark/spark-2.4.1/

  选 spark-2.4.1-bin-hadoop2.7.tgz     

* 环境变量配置

  * SPARK_HOME
  * PATH 

* 验证
  CMD下输入，spark-shell

### hadoop部署

* hadoop-3.2.0.tar.gz 下载
* %HADOOP_HOME%\bin;
* 验证:再输入spark-shell，效果如下，就不会有hadoop的问题
* ![1565254791345](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1565254791345.png)



### Python 部署

* Python 3.6.8下载
  * 官网下载链接:https://www.python.org/ftp/python/3.6.8/python-3.6.8-amd64.exe
  * 教程:https://blog.51cto.com/5001660/2084273
  * 其他参考:[如何配置得心应手的Python](https://zhuanlan.zhihu.com/p/58209172)
  
* Python虚拟环境管理工具主要有以下两种：**推荐pipenv**(可忽略)

  - virtualenv

  - pipenv

  - 上述两个工具都可以使用pip进行安装：

    ```text
    pip install virtualenv
    pip install pipenv
    ```

  - pipenv更加强大一些，**pipenv之于Python就相当于Php之于Composer、Nodejs之于npm、Golang之于dep，pipenv相当于virtualenv和pip的合体**，用两点说明pipenv对比于virtualenv的优势：

    - virtualenv每次开发完都要手动执行一个pip freeze > requirement.txt 把项目最新的环境读取到requirement中，如果忘记了就不能获取最新的配置环境，而pipenv可以实时监测环境的改变，把最新的环境读取到Pipfile。
    - virtualenv需要先激活虚拟环境，然后用pip配置，而pipenv可以直接使用pipenv进行配置环境

  - **pipenv使用步骤：**

    - 创建

    ```text
    pipenv check
    ```

    > 这是目录下会生成Pipfile。

    - 启动虚拟环境

    ```text
    pipenv shell
    ```

    - 安装第三方包

    ```text
    pipenv install **
    ```

    - 退出虚拟环境

    ```text
    exit
    ```

    - 查看所有安装包

    ```text
    pip list
    ```

    - 查看包依赖关系

    ```text
    pipenv graph
    ```

    - 查看虚拟环境路径

    ```text
    pipenv --venv
    ```

    - 卸载安装包

    ```text
    pipenv uninstall
    ```

### PyCharm 下载

**注意:我下载的 PyCharm 是带有 Miniconda 的,所以后面关于 Miniconda 就不用下载了,直接配环境变量在cmd操作conda 或者 Anaconda Prompt 操作conda**(详见后面)

* PyCharm软件专业版：

  官网网址：http://www.jetbrains.com/pycharm/

  下载的是:[2019.1.4 for Windows with Anaconda plugin (exe)](https://download.jetbrains.com/python/pycharm-professional-anaconda-2019.1.4.exe?_ga=2.104905155.1513071020.1565257649-2123887563.1557978386)

* 安装教程:https://blog.51cto.com/5001660/2084463

* 激活

  * 激活教程:https://blog.csdn.net/qq_32811489/article/details/78636049
  * 激活码:http://idea.lanyus.com/

*  主题样式使用设置:https://zhuanlan.zhihu.com/p/52992468

* 插件:(可忽略)

  * 在“定制PyCharm”窗口中，可以根据自己的需要安装相应的插件。插件安装完成后，点击“Start using PyCharm按钮”，然后开始使用PyCharm软件：

    （IdeaVim插件：作用是在Intellij中模拟vim的操作方式；Markdown插件：是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，可以使普通文本内容具有一定的格式；BashSupport插件：是一个高度定制化的vim插件，允许插入：文件头、补全语句、注释、函数、以及代码块；R LanguageSupport插件：R语言支持插件，R语言用于统计分析、绘图的语言和操作环境。R是属于GNU系统的一个自由、免费、源代码开放的软件，是一个用于统计计算和统计制图的优秀工具   ）

* ![1565266507100](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1565266507100.png)

* **pip用于python包的安装，类似于Redhat下的yum、Ubuntu下的apt-get，可以解决安装包依赖的问题，非常方便。**

* **更改pip源至国内镜像，显著提升下载速度**

  * https://blog.csdn.net/lambert310/article/details/52412059

  * 应该新建到%USERPROFILE%\pip\pip.ini

    ![1565310997377](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1565310997377.png)

  * 临时使用:(可忽略)
  
    可以在使用pip的时候加参数-i https://pypi.tuna.tsinghua.edu.cn/simple
  
    例如：pip install -i https://pypi.tuna.tsinghua.edu.cn/simple gevent，这样就会从清华这边的镜像去安装gevent库。

### Anaconda[Miniconda是简化的Anaconda]

​	**好文多读** [Anaconda、conda、pip、virtualenv的区别](https://zhuanlan.zhihu.com/p/32925500)

* Python管理系统是Anaconda，集成开发工具用的是Pycham

  **Anaconda就是这样一个Python开发管家，是一个完整的集成环境，包含了conda/python等190多个科学报和依赖项，这里的conda是开源包（packages）和虚拟环境（environment）的管理系统。**

  packages：conda可以用来安装，更新，卸载工具；安装anaconda时预先安装了Numpy，Scipy，pandas等数据分析中常用的包；另外conda还可以安装非Python包，比如R语言的集成开发环境Rstudio。

  environment：在conda中可以建立多个虚拟环境，用于隔离不同项目所需的不同版本的工具包，以防止版本上的冲突，对纠结于Python版本的孩子们可以建立Python2和Python3两个环境，来运行不同版本的Python代码

* ```
  conda可以给我们提供一个独立的环境，相当于python的virtualenv
  ```

  **1.Miniconda安装及添加环境变量**:https://mp.weixin.qq.com/s/yqyEknvYLIH5E0nMlWEDSQ

  * 在下载完对应的Miniconda安装包之后，可以直接在开始菜单里找到Anaconda Prompt，直接使用Anaconda Prompt而不是cmd终端进入conda操作；
  * 按照教程中的步骤进行Miniconda的安装和环境变量添加）使用cmd终端进入conda操作。

  ##### 2.添加 conda 的镜像服务器

  因为conda 下载文件要用到国外的服务器，速度一般会比较慢，我们可以通过增加一个清华的镜像服务器来解决。

  打开cmd终端或者Anaconda Prompt（快捷键： win+r ：然后输入cmd，回车）。

  分别在cmd终端或者Anaconda Prompt里粘贴下面两行代码（每粘贴一行回车确认）。

  ```
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  
  ```

  `conda config --set show_channel_urls yes`

  **3.创建 Python3.6 的虚拟环境**(可忽略)

  - **pyspark：~~cmd~~ Anaconda Prompt (miniconda3)--> pip install pyspark**
- **findspark: ~~cmd~~ Anaconda Prompt (miniconda3) --> pip install findspark**
  - **注:在cmd安装,使用jupyter notebook 查询不到对应包**
  - **jupyter使用：**

  **4.安装科学计算所需的 python 工具包**(可忽略)
  
	* 用 conda 安装 scipy 
  cmd输入：`conda install scipy`
  
  * 用 conda 安装 pandas 
  输入：`conda install pandas`
  
  ```
conda安装scipy和pandas都是需要先退出python（也就是说，如果你之前在命令行输入了python的话，需要先使用quit()命令退出），在之前创建的course_py35环境里安装;检查包是否安装成功，需要首先进入python，再使用"import +包名字"进行检查，如检查scipy是否安装成功，可以输入import scipy。
  ```

  * 用 pip 安装 scikit-learn 
    输入：`pip install scikit-learn` 
  
  ​		**pip可以安装一些conda无法安装的包；conda也可以安装一些pip无法安装的包。因此当使用一种命令无法安装包时，可以尝试用另一种命令。**
  
* 简单介绍下Anaconda下的几个工具(可忽略)

  * Anaconda Navigtor ：用于管理工具包和环境的图形用户界面，后续涉及的众多管理命令也可以在 Navigator 中手工实现。
  * Jupyter notebook ：基于web的交互式计算环境，可以编辑易于人们阅读的文档，用于展示数据分析的过程。
  * qtconsole ：一个可执行 IPython 的仿终端图形界面程序，相比 Python Shell 界面，qtconsole 可以直接显示代码生成的图形，实现多行代码输入执行，以及内置许多有用的功能和函数。
  * spyder ：一个使用Python语言、跨平台的、科学运算集成开发环境。
  * 验证
  * ![1565317625902](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1565317625902.png)

### jupyter notebook：(可忽略)

Jupyter Notebook是以网页的形式打开，可以在网页页面中**直接编写代码**和**运行代码**，代码的**运行结果**也会直接在代码块下显示的程序。

jupyter notebook优点：

- 交互式调试
- 随时切换Markdown和code，边做笔记边写代码

如果你是小白，那么建议你通过安装Anaconda来解决Jupyter Notebook的安装问题，因为Anaconda已经自动为你安装了Jupter Notebook及其他工具，还有python中超过180个科学包及其依赖项。

[Jupyter Notebook介绍、安装及使用教程](https://zhuanlan.zhihu.com/p/33105153)

```text
jupyter notebook --generate-config
```

C:\Users\admin\.jupyter\jupyter_notebook_config.py

#### 遇到的问题(可忽略)

下载 spark 时使用IDM下载解压成不可读的文件,如下

![1565226164889](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1565226164889.png)

使用Chrome自带的下载器慢慢下解压的就是正常的,如下

![1565226292726](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1565226292726.png)





