#android之gradle
gradle是与ant/maven一样的**自动化构建/依赖管理工具**，与ant/maven使用的**xml** 语言不一样，他使用的是**Groovy**语言。

使用gradle可以更容易的**创建多个版本的应用程序**，**多个类型的apk包**，更**容易配置扩展**，更好的**ide集成**，跟**容易重用资源和代码**(这里可以仔细体会一会，上面说的那些优点到底在哪里体现出来了)

###入门须知
**1. 基本配置** 

跟maven一样有配置文件，maven里面是叫pom.xml,在gradle里面叫build.gradle，android中每一个module都有一个build.gradle配置文件，每一个project又有一个build.gradle,而一个project可有多个module，如图

![](http://i.imgur.com/Q2cbqQ6.png)

上面那个build.gradle是module的配置文件，下面的那个整个project的配置文件，两个文件是有区别的，module的配置文件是基于当前module的，而project是基于整个项目的（感觉说的不是很明确）,

下面看一下project中的build.gradle文件

	// Top-level build file where you can add configuration options common to all sub-projects/modules.

	buildscript {
	//构建过程依赖的仓库
    repositories {
        jcenter()
    }
	//构建过程需要依赖的库
    dependencies {//下面声明的是gradle插件的版本
        classpath 'com.android.tools.build:gradle:1.1.0'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
   	 }
	}
	//这里面配置整个项目依赖的仓库,这样每个module就不用配置仓库了
	allprojects {
    repositories {
        jcenter()
    }
	}
project有两个repositories（存贮室）,第一个是buildgradle脚本自身需要的资源，在dependencies中依赖了gradle，而allprojects中的repositories是项目所有模块需要的资源，

module的build.gradle

	//声明插件，这是一个android程序，如果是android库，应该是com.android.library
	apply plugin: 'com.android.application'
	android {//安卓构建过程需要配置的参数
    compileSdkVersion 21//编译版本
    buildToolsVersion "21.1.2"//buildtool版本
    defaultConfig {//默认配置，会同时应用到debug和release版本上
        applicationId "com.taobao.startupanim"//包名
        minSdkVersion 15
        targetSdkVersion 21
        versionCode 1
        versionName "1.0"
    }
    buildTypes {//这里面可以配置debug和release版本的一些参数，比如混淆、签名配置等
        release {//release版本
            minifyEnabled false//是否开启混淆
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'//混淆文件位置
        }
    }
	}
	dependencies {//模块依赖
    compile fileTree(dir: 'libs', include: ['*.jar'])//依赖libs目录下所有jar包
    compile 'com.android.support:appcompat-v7:21.0.3'//依赖appcompat库
	}

在module中，android内容块里面配置的是，module的一些版本信息，比如编译sdk版本，最小支持版本，buildtool是在android SDK中，android用来构建应用程序需要的工具。最好升级到最新版本,[参考资源][1]。

defaultConfig中是一些基本配置，下面列出来了所有的值，

![](http://i.imgur.com/uFGywH7.png)

buildTypes很重要，在这里可以配置**构建的版本**的一些参数，默认有两个版本**release/debug**，也可以自定义版本，比如叫foo,然后通过gradlew assembleFoo就可以生成对应的apk了。buildTypes里还有很多可配置项，下面列举了所有可配项以及debug/release版本的默认值：

![](http://i.imgur.com/3kdJl7c.png)

还有一些与gradle有关的文件，

**1. gradle.properties** 

在里面可以定义一些常量给build.gradle使用，比如可以配置签名相关的信息如，keystore位置，密码，keyalias等

**2. settings.gradle**

这个文件是用来配置多模块的。一个项目中有两个模块，就需要下面这样配置，

	include ':module-1',':module-b'

**3. gradle文件夹** 

这里面有两个文件，gradle-wrapper.jar和gradle-wrapper.properties，他们就是gradle wrapper （这是什么）。gradle项目都会有，可以通过命令gradle init来创建它们（前提是安装了gradle并且配置到了环境变量中）。

**4. gradlew和gradle.bat**

分别是linux下的shell脚本和windows下的批处理文件，他们的作用是根据gradle-properties文件中distributionUrl下载对应的gradle版本。这样就可以保证在不同的环境下构建时都是使用的统一版本的gradle，即使该环境没有安装gradle也可以，因为gradle wrapper会自动下载对应的gradle版本。gradlew的用法跟gradle一模一样，比如执行构建gradle build命令，你可以用gradlew build。gradlew即gradle wrapper的缩写。

**5. gradle仓库**

gradle有三种仓库，maven仓库，ivy仓库，flat本地仓库。声明方法如下:

	maven{
  	 url  "..."
	}
	ivy{
  	url  "..."
	}
	flatDir{
 	dirs 'xxx'
	}
有一些仓库提供了别名，可以直接使用，

	repositories{
 	 mavenCentral()
 	 jcenter()
	  mavenLocal()
	}

**6. gradle任务**
gradle提供了四个顶级任务，分别是：
	
	assemble 构建项目输出
	check 运行监测和测试任务
	build 运行assemble和check任务
	clean 清理输出任务
执行顶级任务同时会执行与其依赖的任务，比如执行
	
	gradlew assemble
会执行：
	gradlew assembleDebug
	gradlew assembleRelease
这时会在你项目的build/outputs/apk或者build/outputs/aar目录生成输出文件

可以通过：
	
	gradlew tasks
列出所有可用的任务。在Android Studio中可以打开右侧gradle视图查看所有任务。

##常见问题
**1. 导入本地jar包**

在需要导入的module中	
	
	complie file('lib/xxx.jar')
如果有多个jar包需要同事导入，

	complie fileTree(dir: 'libs',include： ['*.jar'])
**2. 导入maven库**

	complie 'com.android.support:appcompat-v7:21.0.3'
可见，其格式为
	
	complie 'groupId:artifactId:version'

**3. 导入某个project**

app是多模块的时候，module-a依赖于mudole-b

	complie project(':module-b')
并且去要在setting.gradle中修改

	include ':module-a',':module-b'

此时module-b是作为库而存在的，所以在module-b的build.gradle中声明为库，而不是app

	apply plugin'com.android.library'
并且在module-b的build.gradle中的buildType中是不可以有applicationId的。

**4. 声明第三方maven仓库**

这是使用jcenter就不可以了，需要在project或者在用到该第三地方库的build.gradle中配置

	repositories{
	maven{
          url="http://mvnrepo.xxx.com"
          }
	}

**5. 以来第三方arr文件** （研究一下arr与jar差别）


---------------------

##个人理解，

gradle是一个可以在java中使用的框架，在android studio中已经自动集成了gradle，在android开发当中，我们在管理依赖关系的时候，只需要在build.gradle中配置即可。对于gradle的引入等，不用关心。
在java中使用gradle自动下载jar包。

	apply plugin: 'Java'


	repositories {
    mavenCentral()
	}


	dependencies {
    compile "javax.servlet:javax.servlet-api:3.1-b07",  
            "org.slf4j:slf4j-log4j12:1.7.5",  
            "org.slf4j:slf4j-jdk14:1.7.5",  
            "MySQL:mysql-connector-java:5.1.24" 
	}


	task copyJars(type: Copy) {
  	from configurations.runtime
 	 into 'lib' // 目标位置
	}


[1]: http://www.tuicool.com/articles/eqAbQ3e "title"
