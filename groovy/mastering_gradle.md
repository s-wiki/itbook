# Gradle


## gradle setup脚本

1. All the files with .gradle extension under <USER_HOME>/.gradle/init.d directory are treated as initialization scripts. Gradle will execute all the .gradle files under this directory before the execution of any Gradle


## Build script basics

When you execute any build script, Gradle instantiates the org.gradle.api.Project object for the build  le and gives an implicit project object. You can use this object toaccesstheProjectAPIinthebuild leeitherthroughproject.<methodname | property>orsimply<methodname | property>.


## 定义Property
Gradle还为我们提供了多种方法来自定义Project的Property。

### 在build.gradle文件中定义Property
在build.gradle文件中向Project添加额外的Property时，我们并不能直接定义，而是应该通过ext来定义。如果要添加一个名为property1的Property，我们应该：

	ext.property1 = "this is property1"
另外，我们也可以通过闭包的方式：

	ext {
   		property2 = "this is property2"
	}

在定义了Property后，使用这些Property时我们则不需要ext，而是可以直接访问
	
	task showProperties << {
   		println property1
	   println property2
	}	
	
### 通过命令行参数定义Property	

Gradle还提供了-P命令行参数来设置Property，比如：

	gradle -Pproperty3="this is property3" showCommandLieProperties

### 通过JVM系统参数定义Property

我们知道，在java中，我们可以通过-D参数定义JVM的系统参数，然后在代码中可以可以通过System.getProperty()进行获取。在Gradle中，我们也可以通过-D的方式向Project传入Property，只是此时我们需要遵循一些约定：每一个通过-D方式声明的Property都需要以“org.gradle.project”为前缀	

### task的执行顺序
被依赖的任务先执行，任务之间没依赖的话，按字母顺序表进行。
另外，提供了3个规则来控制

* shouldRunAfter
* mustRunAfter
* finalyzedBy (more strict in nature)


The difference between mustRunAfter and shouldRunAfter is that mustRunAfter is strict ordering, whereas shouldRunAfter is lenient ordering.


### task任务执行条件

	prodTask.onlyIf {project.hasProperty('environment') && project.environment=='prod' }


### Task rules


### Custom tasks

Gradle provides different ways to add custom tasks in the build script:


A custom task is a Java or Groovy class that extends from DefaultTask. We can use the @TaskAction annotation to de ne the task actions. You can add multiple actions in a single task. They will execute in the order they are de ned. 