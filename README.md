# BintrayUploadGradlePlugin
<p>
Android开发者经常发布自己的Android库到<a href="https://jcenter.bintray.com/" target=_blank>jCenter</a>，通常使用两个<a href="http://blog.fpliu.com/it/software/gradle" target="_blank">Gradle</a>插件：
</p>
<ul>
    <li>
        <a href="https://github.com/dcendents/android-maven-gradle-plugin" target="_blank">android-maven-gradle-plugin</a>
    </li>
    <li>
        <a href="https://github.com/bintray/gradle-bintray-plugin" target="_blank">gradle-bintray-plugin</a>
    </li>
</ul>
<p>
这两个插件还要配合上好几个jar包的生成，POM文件的配置等，非常繁琐，实际上，我们一般的开发者也就是关心几个小点，其他子要自动生成即可，所以，为了简化这两个插件的使用，我在他们两个的基础上进行了包装。大大简化了他们的使用。
</p>

## 1、Android工程使用方法
1、配置classpath：
```
buildscript {
    repositories {
        jcenter()
        google()
    }
    dependencies {
        //Android Gradle插件
        //https://developer.android.com/studio/build/gradle-plugin-3-0-0-migration.html
        classpath("com.android.tools.build:gradle:3.0.1")
        
        //对android-maven-gradle-plugin和gradle-bintray-plugin两个插件的包装、简化插件
        //https://github.com/leleliu008/BintrayUploadGradlePlugin
        classpath("com.fpliu:BintrayUploadGradlePlugin:1.0.0")
    }
}
```
2、应用插件：
```
apply {
    plugin("com.fpliu.bintray")
}
plugins {
    id("com.android.library")
    
    //用于构建jar和maven包
    //https://github.com/dcendents/android-maven-gradle-plugin
    id("com.github.dcendents.android-maven").version("2.0")
        
    //用于上传maven包到jCenter中
    //https://github.com/bintray/gradle-bintray-plugin
    id("com.jfrog.bintray").version("1.7.3")
}
```
3、配置数据(kotlin script)：
```
// 这里是groupId，必须填写,一般填你唯一的包名
group = "com.fpliu"

//这个是版本号，必须填写
version = "1.0.0"

val rootProjectName = rootProject.name

bintrayUploadExtension {
    developerName = "leleliu008"
    developerEmail = "leleliu008@gamil.com"

    projectSiteUrl = "https://github.com/$developerName/$rootProjectName"
    projectGitUrl = "https://github.com/$developerName/$rootProjectName"

    bintrayUserName = "xx"
    bintrayOrganizationName = "xx"
    bintrayRepositoryName = "yy"
    bintrayApiKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxx"
}
```

## 2、基于JVM的语言的工程使用方法
1、配置classpath：
```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        //对maven和gradle-bintray-plugin两个插件的包装、简化插件
        //https://github.com/leleliu008/BintrayUploadGradlePlugin
        classpath("com.fpliu:BintrayUploadGradlePlugin:1.0.0")
    }
}
```
2、应用插件：
```
apply {
    plugin("com.fpliu.bintray")
}
plugins {
    java
    maven
    
    //用于上传maven包到jCenter中
    //https://github.com/bintray/gradle-bintray-plugin
    id("com.jfrog.bintray").version("1.7.3")
}
```
3、配置数据(kotlin script)：
```
// 这里是groupId，必须填写,一般填你唯一的包名
group = "com.fpliu"

//这个是版本号，必须填写
version = "1.0.0"

val rootProjectName = rootProject.name

bintrayUploadExtension {
    developerName = "leleliu008"
    developerEmail = "leleliu008@gamil.com"

    projectSiteUrl = "https://github.com/$developerName/$rootProjectName"
    projectGitUrl = "https://github.com/$developerName/$rootProjectName"

    bintrayUserName = "xx"
    bintrayOrganizationName = "xx"
    bintrayRepositoryName = "yy"
    bintrayApiKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxx"
}
```

## 3、gradle任务

### 3.1、./gradlew :library:install
用于生成必须上传到<a href="https://jcenter.bintray.com/" target="_blank">jCenter</a>必须的文件，
执行此命令后在<code>build</code>目录下生成的内容如下：
```
build
├── libs
│   ├── ${rootProjectName}-${version}-javadoc.jar
│   └── ${rootProjectName}-${version}-sources.jar
├── outputs
│   └── aar
│       └── ${rootProjectName}-release.aar
├── poms
│   └── pom-default.xml
└── ....
```
### 3.2、./gradlew :library:bintrayUpload
这个命令还可以简化成<code>./gradlew :library:bU</code>，这就是上传到<a href="https://jcenter.bintray.com/" target=_blank>jCenter</a>的命令。当然，前提是您已经有了他的账户和仓库。

### 3.3、注意
上面两个任务前面都加了<code>:library</code>，这是因为一般的工程都会包含至少两个子模块，一个一般是<code>app</code>或者是<code>sample</code>，另一个一般是<code>library</code>。从字面意思也可以知道，<code>library</code>模块就是编写我们要发布的库的，而<code>app</code>或者是<code>sample</code>是用来编写示例代码的。
<p>
您的工程如果是单模块的，那么省略<code>:library</code>即可。
</p>

## 4、示例工程
下面这些工程都使用了该插件：
<ul>
    <li>
        <a href="https://github.com/leleliu008/Android-emoji" target="_blank">Android-emoji</a>
    </li>
    <li>
        <a href="https://github.com/leleliu008/RetrofitHelper" target="_blank">RetrofitHelper</a>
    </li>
</ul>
