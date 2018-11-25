Git使用 
开放源码的版本控制软件
Github托管代码
Gitlab是公司或学校专用 有一定保密性
GIT不仅仅是个版本控制系统，它也是个内容管理系统(CMS),工作管理系统等。
Git 与 SVN 区别点：

1、GIT是分布式的，SVN不是：这是GIT和其它非分布式的版本控制系统，例如SVN，CVS等，最核心的区别。

2、GIT把内容按元数据方式存储，而SVN是按文件：所有的资源控制系统都是把文件的元信息隐藏在一个类似.svn,.cvs等的文件夹里。

3、GIT分支和SVN的分支不同：分支在SVN中一点不特别，就是版本库中的另外的一个目录。

4、GIT没有一个全局的版本号，而SVN有：目前为止这是跟SVN相比GIT缺少的最大的一个特征。

5、GIT的内容完整性要优于SVN：GIT的内容存储使用的是SHA-1哈希算法。这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。

一般工作流程如下：

克隆 Git 资源作为工作目录。
在克隆的资源上添加或修改文件。
如果其他人修改了，你可以更新资源。
在提交前查看修改。
提交修改。
在修改完成后，如果发现错误，可以撤回提交并再次修改并提交。

公钥加密
私钥解密
Rsa key 公钥（ssh）
在sts的首选项 ssh2里创建公钥 在github里使用

Github  Create a new repository新建仓库
Releases是版本号

Pull requests 版本融合 
冲突
<<<<<<< branch3  下面是branch3修改的
#branch1 change
=======
#branch3 change
>>>>>>> master  上面是master修改的

.project是eclips的配置文件 不需要上传 因为各机器不一样 ignore

所有的操作都是在右键team选项里
Show in history可以实现版本回退
Revert 恢复
Reset 删除 删除本地 但是线上没改

可以通过命令行 git add 添加多个远程库 默认名称origin

Maven教程
项目管理工具 包括项目构建
Maven中也有仓库 分本地和远程

同样 在sts的windows 首选项的maven里面可以修改
Installations和user settings
user settings 里的user settings控制的是用户空间的设置（一个用户可能拥有多个用户空间） global settings是全局设置
E:\taiji\apache-maven-3.6.0\conf 改settings 文件
复制<localRepository>/path/to/local/repo</localRepository>
/path/to/local/repo是本地仓库路径 注意斜杠
这是在<settings>下面的 所以找到<settings>标签 在标签下直接添加就行
Settings是默认从中央仓库下载jar包

New maven project
Group id 倒着写公司名
Artifact id 项目名

Target目录下放jar包 编译后文件
Pom.xml重要 pom项目对象模型 是maven基本工作单元，坐标管理 依赖管理 构建配置文件
坐标唯一确定jar包
<groupId>taiji</groupId>//公司或组织的唯一id
  <artifactId>test1</artifactId>//项目唯一id
  <version>0.0.1-SNAPSHOT</version>//版本号
三个值唯一确定项目
<packaging>jar</packaging> 打包方式 jar war pom
mvnrepository.com 里面可以找到坐标 并下载jar包
构建配置文件
	设置或覆盖maven的默认配置
	三个级
		项目级（即只在本项目中生效）
			在pom.xml可以添加 修改
			Id和url是必须 其它两个可选
<profiles><profile></profile><profile></profile></profiles>
			其下<pluginRepository>是插件仓库
			<profile>
      <id>nexus</id>
      <repositories>
        <repository>
          <id>central</id>
          <url>http://repo.taiji.com.cn:8081/repository/maven-public/</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </repository>
      </repositories>
     <pluginRepositories>
        <pluginRepository>
          <id>central</id>
          <url>http://repo.taiji.com.cn:8081/repository/maven-public/</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </pluginRepository>
      </pluginRepositories>
</profile>
	注意 以上代码构置了两个profile配置文件（每个都是单独的一个配置）还需要激活才能起效
1、	命令行进入项目文件  mvn test –P+profileid (比如mvn test –Pnexus  p大写)
2、	在要激活的<profile>下加标签
<activation>
<activeByDefault>true</activeByDefault>
			</activation>  
3、	在settings.xml文件<settings>下直接加标签显示激活的profileID
<activeProfiles>
    <activeProfile>nexus</activeProfile>
</activeProfiles>
  简单点直接配置仓库 pom下改
		<repositories>
        <repository>
          <id>central</id>
          <url>http://repo.taiji.com.cn:8081/repository/maven-public/</url>
        </repository>
      </repositories>		
	在2、3、4（2、3步要改的是windows 首选项下面的settings即工作区间的配置文件）设置后命令行mvn test就行
	<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.2</version>
      <scope>test</scope>//表示依赖范围 
    </dependency>
  </dependencies>下载依赖的jar包
用户级
		全局
Src/main下是代码
Jar包依赖范围 
compile 是默认
test
provide 打包的时候不会把这个包加入（以避免不同软件的同名包运行时冲突）
runtime
system 引入外部依赖 是互联网上没有的 所以groupID和artifactId无效 需要加一个<systempath>指明包所在的路径（比如本机上的包的绝对路径）
直接依赖和间接依赖的依赖原则
	1.间接依赖路径最短优先
一个项目test依赖了a和b两个jar包。其中a-b-c1.0 ， d-e-f-c1.1 。由于c1.0路径最短，所以项目test最后使用的是c1.0。

2.pom文件中申明顺序优先
有人就问了如果 a-b-c1.0 ， d-e-c1.1 这样路径都一样怎么办？其实maven的作者也没那么傻，会以在pom文件中申明的顺序那选，如果pom文件中先申明了d再申明了a，test项目最后依赖的会是c1.1

所以maven依赖原则总结起来就两条：路径最短，申明顺序其次。
依赖排除
	在<dependencies>下添加
	<exclusions><exclusion>
需要groupId和artifactId
</exclusion><exclusions>//不需要版本号 因为在排除冲突时已经知道了
mvn help:describe –Dplugin=help安装help插件

maven生命周期
生命周期和阶段有关联 阶段和插件有关联 插件和目标有关联
clean
build（default）
site

项目继承和聚合（父项目和聚合项目都要设置<packaging>pom</packaging>）
继承 父项目不指定子项目 子项目知道自己的父项目
聚合 被聚合的项目不知道聚合成的项目 

父项目添加依赖管理
<dependencyManagement>//依赖管理
		<dependencies>
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>3.8.1</version>
				<scope>test</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>
<pluginManagement>//插件管理 和依赖一样
子项目添加父项目标签
<parent>
		<groupId>cn.com.taiji</groupId>
		<artifactId>Parent</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>
由于父项目依赖管理中插件和包依赖作了规定 所以子项目用到的这些插件与包的标签中不再需要添加版本号
统一版本号${版本名}
<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<junit.version>4.12</junit.version>
	</properties>
	
	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>${junit.version}</version>
				<scope>test</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>

聚合
项目名 右键 maven new maven module project
插件 依赖管理和继承一样

命令行创建maven项目
mvn archetype:generate –DgroupId=cn.com.taiji –DartifactId=Test –DarchetypeArtifactId=maven-archetype-quickstart –DinteractiveMode=false

mvn [-P+配置名] dependency:resolve 包下不下来出错时用的命令 要进入项目文件下执行
这里的配置名是本地仓库的maven配置 默认就是nexus
