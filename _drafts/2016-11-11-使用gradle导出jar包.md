# android studio使用gradle导出jar包

type为copy的完整代码

~~~gradle
task clearJar(type: Delete) {
    delete 'build/libs/yutils.jar'
}

task makeJar(type: Copy) {
from('build/intermediates/bundles/release/')
into('build/libs/')
include('classes.jar')
rename ('classes.jar', 'blhomeautomationandroidnetwork.jar')
}

makeJar.dependsOn(clearJar, build)
~~~

导出失败可能

1. 找不到baseName 
1. task重复
1. 错误的task type
1. 指定目录生成的jar，导入的时候，会报错(可能是导出过程哪里出了错误)
1. task type 有什么区别
1. 使用gradle打jar包 ， 也是需要jdk的， 所以如果gradle找不到jdk的话，就会打包失败。 而对于不是自己配置的gradle环境，是由编译器自动引入gradle(比如eclipse自动引入的gradle)，可能需要手动配置gradle的java环境。

