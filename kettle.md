# Kettle本机编译

首先开源软件的编译对我来说永远就是一个恶梦。非常的恶心。

为什么？我不知道mvn、ant这些东西有什么好。而且java的程序为什么一定要jar包拷来拷去的？你不知道中国的网络很慢么？哎。。。难道其它人就没有遇到跟我一样的问题吗？网上教程很多。但是没有一个是可以用的。并且都很老旧。根本就用不了。
这也就是第二个问题。开源的技术发展太快了。网上的资料几天就变得没有用处了。那么我不追时髦。我用旧的不行么。真不行。人家开发那么多新东西不就是想让你用么。有的好心的把旧的软件打个包。还可以下载。有的直接就找不了。只能用相对新一点的。这我也忍了。
第三个问题。您文档能同步更新么。不看文档不行。看了文档也不行。这就是我们的处境呀。文档都是大神们心情好的时候写的。可能几年不更新一下。但是代码天天变。变变新特性不就得了么。为啥动不动就把底层编译搞一下。搞得文档对不上代码。你让我找谁去。。。

好了，吐槽就到这里吧。下面说一下上个周未我都整了啥。其实不是周未了。上周一整周我就在想既然要用kettle做项目。不如把源码下载下来学习一下。现在也不是太忙。所以就下了一下代码。也开始了无尽的fail之旅。。。

首先。咱们先下载代码吧。
git clone https://github.com/pentaho/pentaho-kettle.git
新鲜出炉的代码直接主线。时时有人在更新。不过。这个项目是不是太大了。下载怎么要这么久呀。半天时间就在一点一点的下载。话说我网络也还行呀。mkv的电影8、9g也不过1-2个小时。您是有多牛呀。文件不大。下载怎么那么慢。。。
终于终于下载完了。

然后咱们编译吧。

按照文档：ant clean-all resolve create-dot-classpath。我们执行一下吧。对了忘了说了。咱们要先装一下ant\mvn\jdk-1.7.一定要1.7呦。不然玩不了。尽管咱们还是1.6控。也没办法呀。而且人家都说了要1.7。咱就1.7吧。装一下。1.7几百兆。几分钟就下载好了。哎。羡慕嫉妒恨呀。我恨！ant\mvn你们就不能让我自已下载么。你做好你的工作就行了。下载交给更擅长的吧。迅雷、百度都行呀。实在不行。我用bt不行么。。。好了好了。言归正传。这个命令是不会一次成功的。不成功怎么办？没事重试一下。慢慢的所有jar包都下载好了。就可以成功了。大概10几20次之后。看到SUCCESS了吧。恭喜你。这步可以了。

继续按文档来。下一步就该ant dist了。来吧，baby。大概几十次出错后，终于看到了内存不足的问题。。。这个为啥？难道我机器太破了。不会吧？好吧。开始百度各种解决的办法。最终。。。最终还是过不去呀。

没办法了。看build.xml吧。看看为啥就不行呢。

  <target name="dist" depends="build">
    <ant antfile="assembly/build.xml" inheritall="false">
      <target name="clean-all"/>
      <target name="assemble-full"/>
    </ant>
    <move todir="${distrib}">
      <fileset dir="${assembly.stage.pdi-client.dir}"/>
    </move>
  </target>
  
 上面就是dist相关的任务。它要干什么呢？首先它是不会执行滴。需要build先完成才能执行。好吧。那我们先一个一个build吧。这样不会内存出错了吧。转念一想。它不是插件么。不用都编吧。咱先把主要的编了。启起来看看呗。好吧。那我就挑着编吧。这块也挺周折的。不过还好。任务小了。还是不错的。一个是编译的快。相对快一点。另一个不知道是我的机器问题？还是ant的问题。我每次执行dist都会把好好编完在jar删除。然后重新编呀。。。你这是在耍我玩呢么。。。
 好了build的部分咱就算解决了。下面我们可以dist了吧。dist分成两部分。move todir这部分。不会出错的。就是mv的过程。为了发布用的吧。不管了。那重要的就是assemble-full.顺便说一下clean-all就删除了吧。不然又重玩了。。。老外为什么就喜欢clean呢。
 assemble-full的过程就是收集各种jar.别的不说了。慢慢下吧。出错了就重来。反复试。不过有个big-data。你妹呀。你有600多兆。你让我用lvy来下。你是不是在搞笑呀。还是国内就不适合写开源的东西。最终我选择了放弃。
 怎么放弃。就是在assemble中的lvy.xml中删除big-data的依赖。然后再试一下。可能还用一些比较大的方件。但是都没有这么恐怖的了。终于这关又过了。
 
 接下来。我们就可以在dist中运行了。首先。双击spoon.sh。然后就没有然后了。。。双击。双击。双击。没有反应。为什么。。。难道就这样失败了。呃。。。先睡会吧。。。
 
 醒了。想想。难道就这样放弃了么。有点不甘心呀。想想怎么办呢。再看一下代码。启动的应该是lancher中的那个jar。难道是这个jar有问题？突然想到。我不是有一个可以运行的版本么。要不在那个环境下试试。把jar拷过去。点击了一下。运行正常。呃。不是这个的问题。那怎么办呢。又想了个法字。要不把能运行版本的lib拷过来试试。一试。真的可以！阿。小爽了一下。到现在终于看到自已编的版本了。
 
 但是换回来。用我自己的lib跑一下。呃。。。又不行了。这个。为什么呀。逐个比对一下吧。lib中的jar文件我一个一个比对了一版。发现自已编的少了一个commands-3.3.0-I20070605-0010.jar。加上它就可以正常运行。没有它。就不行。打开来看看吧。呃。原来是eclipse的包。那就明白了。kettle不就是靠swt来画的么。没有这个。难怪我看不到界面。
 
 好了。到这里。这次编译暂时告一段落。不过我怎么编译完了。反尔没有心情看代码了呢。呵呵。改天再看吧。
 
 先说一下总体的感觉。首先不得不承认kettle软件还是非常牛的。不然也不会能买钱了。而且用它作etl还是可以轻松一点的。尽管出现问题解决起来比较困难。但是磨合一段时间暂时还是不错的。
 所以我才愿意花时间来搞这个东西。但是不可否认java的编译还是非常不人性化的。mvn和ant我也不太喜欢。尽管可以很强大。但是我还是不太喜欢。而且对java的部署结构也挺反感的。依赖来依赖去的。但是也没办法。我不牛。我也写不出来更好的。所以我默默的。但是我相信未来肯定会有更人性化的编译工具的。比ant好。也比mvn好。不过我可能看不到了。也用不到了。呵呵。但是我还是相信。未来的程序员肯定会过上好日子的。
 