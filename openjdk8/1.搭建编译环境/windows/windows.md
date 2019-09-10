本人准备的是32位编译环境，编译完成后拷贝到64位上也可以运行，也可以参考网站https://www.jianshu.com/p/e85f93cc74cb，下面主要是本人遇到的一些问题的解决
一、准备编译工具
1.安装cygwin，这个工具提供linux模拟运行环境，因为openjdk工程主要由makefile组成（openjdk核心代码是c、c++）。
  工具下载地址：https://www.cygwin.com/setup-x86.exe，注意安装该工具的时候要保持联网状态，下载完之后直接安装，但是不能默认安装，安装步骤可参考https://www.jianshu.com/p/e85f93cc74cb，主要注意的是要把
  cygwin中需要的工具安装完全，如果漏了可以再执行cygwin安装文件进行添加。
2. 安装vs2010，注意一定要是英文版本，下载地址：https://msdn.itellyou.cn/，在安装前最好看下系统里面有没有安装.net-framework，有的话最好卸载掉。
3.准备一个jdk，我这边jdk版本是jdk1.7.0_80版本
4.下载freetype，下载地址:https://sourceforge.net/projects/freetype/files/latest/download?source=files这个网址下载的是库文件直接可以使用，头文件目录记得下载
5.下载openjdk源码，需要用到下载工具mercurial(https://www.mercurial-scm.org/downloads)，安装完之后打开cmd，运行hg clone http://hg.openjdk.java.net/jdk8u/jdk8u/
  这个时候运行cygwin终端进入opnejdk目录，运行bash get_source.sh，下载代码有点慢，务必耐心等待
5.本人的目录结构
  e:/hub/
       |--freetype
	      |--lib 里面放下载的库文件
		  |--include 里面放头文件
       |--jdk8\Java\jdk1.7.0_80 bootstrap jdk
       |--openjdk8u\jdk8u openjdk源码
二、编译构建
1.进入openjdk目录 cd /cygdrive/e/hub/openjdk8u/jdk8u/
2.执行./configure --enable-debug --with-freetype=/cygdrive/e/hub/freetype --with-boot-jdk=/cygdrive/e/hub/jdk8/Java/jdk1.7.0_80/ --with-target-bits=32
这里我遇到的坑：vs是中文版本、代码没有下完全，openjdk代码目录权限没有修改
3.执行make all
三、创建vs2010工程
1、进入E:\hub\openjdk8u\jdk8u\hotspot\make\windows，下面有个create.bat,如果你的cygwin路径是c:/cygwin/bin则不用修改，否则要把cygwin路径修改掉。
2、打开cmd进去该目录，执行create.bat E:\hub\openjdk8u\jdk8u\build\windows-x86-normal-server-fastdebug\jdk
注意这里有个坑会执行不成功，E:\hub\openjdk8u\jdk8u\hotspot这个目录的权限要修改掉，修改方法是右键-属性-安全，要把所有用户的拒绝属性全部去掉
四、单步调试
1、E:\hub\openjdk8u\jdk8u\hotspot\build\vs-i486目录下已经存在vs的工程文件，用vs打开
2、调试器选择compiler2_fastdebug
3、修改项目属性，右键项目-属性-调试-调试命令行参数，后面加上-Dsun.java.launcher=gamma -Djava.class.path=E:\hub\class TestMain，其中TestMain是写的主类，里面有main函数
4、执行F5，查看命令行是否输出hello world
5、上面步骤都ok后就可以单步调试了，自己加断点查看
	   
