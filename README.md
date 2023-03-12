## Gradle基础使用
### Gradle中的常用指令
| 常用gradle指令           | 作用            |
|----------------------|---------------|
| gradle clean         | 清空build目录     |
| gradle classes       | 编译业务代码和配置文件   |
| gradle test          | 编译测试代码，生成测试报告 |
| gradle build         | 构建项目          |
| gradle build -x test | 跳过测试构建构建      |
### 修改maven下载源
Gradle自带的Maven源地址是国外的，该Maven源在国内的访问速度是很慢的，除非使用特殊手段。
一般情况下建议使用国内的第三方开放的Maven源或企业内部自建Maven源。
#### 认识init.d文件夹
我们可以在gradle的init.d目录下创建以.gradle结尾的文件，
.gradle文件可以实现在build开始之前执行，所以你可以在这个文件配置一些你想预先加载的操作
#### 在init.d文件夹创建init.gradle文件
```groovy
 allprojects {
    repositories {
        mavenLocal()
        maven { name "Alibaba"; url "https://maven.aliyun.com/repository/public" }
        maven { name "Bstek"; url "https://nexus.bsdn.org/content/groups/public/" }
        mavenCentral()
    }

    buildscript {
        repositories {
            maven { name "Alibaba"; url 'https://maven.aliyun.com/repository/public' }
            maven { name "Bstek"; url 'https://nexus.bsdn.org/content/groups/public/' }
            maven { name "M2"; url 'https://plugins.gradle.org/m2/' }
        }
    }
}
```
#### 扩展1：启用init.gradle文件的方法有：
```text
1、在命令行指定文件，例如:gradle --init-script yourdir/init.gradle -q taskName 可以多次输入此命令来指定多个init文件
    示例：gradle --init-script C:\Personal\Study\Project\gradle-application\init.gradle -q showRepos
2、把init.gradle文件放到USER_HOME/.gradle/目录下
    USER_HOME指的是用户目录，示例：C:\Users\liuhx\.gradle
3、把以.gradle结尾的文件放到USER_HOME/.gradle/init.d/目录下
4、把以.gradle结尾的文件放到GRADLE_HOME/init.d/目录下
    GRADLE_HOME指的是配置的环境变量，也就是GRADLE安装目录
如果存在上谜案的4种方式的2种以上，gradle会按上谜案的1-4序号以此执行这些文件，
如果给定目录下存在多个init脚本，会按照拼音a-z顺序执行这些脚本，
每个init脚本都存在一个对应的gradle实例，你在这个文件中调用的所有方法和属性，都会委托给这个gradle实例，每个init脚本都实现了Scrip接口。
```
#### 