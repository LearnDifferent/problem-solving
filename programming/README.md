# Programming

## 使用 Maven 打包后，只能加载默认环境的配置（application.yml）的问题

问题描述：

使用 Maven 打包成 jar 包后，放到生产环境下运行，发现只有默认的 application.yml 生效，生产环境的 application-prod 配置没有生效。

排查结果：

Maven 打包的时候，没有将 prod 环境的 application-prod.yml 打包进去。

## Maven 下载依赖的时候，一直报错 `Could not find artifact xxx` ，重新加载 Maven 也不行

问题描述：

使用 Maven 下载依赖的时候，因为各种原因（比如网络）而下载依赖失败的时候，因为 `～/.m2/repository` 路径下已经下载了一部份依赖，所以 Maven 会以为依赖已经下载好了。

解决方案：

去到 `～/.m2/repository` 路径下，找到对应的依赖的路径那里，然后直接删掉该文件夹，再重新加载 Maven 就可以触发依赖的重新下载。

## 内存泄漏（Memory Leak）

问题描述：

之前在工作中遇到一个 Memory Leak 的问题，内存还是一直被占用，手动 GC 也无法处理。

解决方案：

1. 在 Collection 或 Map 使用完成后，设置为 null：因为这样才能释放存入其中的对象

2. 释放各种连接

3. 单例模式在初始化后始终在 JVM 的生命周期中存在，如果其持有了外部的引用，则该引用的对象无法被回收

4. 使用完成的对象，赋值为 null

5. 使用 `Runtime.getRuntime().gc();`，也就是 `System.gc();` 触发 GC

6. 如果是线程一直无法释放，尝试结束线程

衍生问题：

- 内存碎片: 如果 GC 使用的是 Mark Sweep，可能会存在大量内存碎片。这就需要使用其他 GC 方式

## 遇到需要批量替换的情况，利用 AI 生成正则表达式

当我们需要批量替换 SQL 或代码里面的内容时，建议使用 ChatGPT 等 AI 生成正则表达式。

> 注意：下面的正则表达式是在 IDEA 上可以使用的，有些编辑器不能写 `$1` ，而是 `\1` ；有些不能写 `$0` 而是 `\g<0>`

比如我需要将 `coalesce()` 函数的第一个参数使用 `to_char()` 函数包裹一下，转为 char 类型，然后将第二个参数变为 '-'，也就是 `coalesce(param, 0)` 的变为 `coalesce(to_char(param), '-')` ：

```regexp
Search: coalesce\(([^,]+),?\s*([^)]+)\)
Replace: coalesce(to_char($1), '-')
```

如果在上面的基础上，我想排除这种 `coalesce()` 函数前面或后面有加号 `+` 的情况，也就是在上面的基础上，排除 `coalesce(param1, 0)+coalesce(param2, 0)+coalesce(param3, 0)` 这种模式的：

```regexp
Search: (?<!\+)\bcoalesce\(([^,]+),?\s*([^)]+)\)(?!\+)
Replace: coalesce(to_char($1), '-')
```

再比如，我想把 `to_number(...)` 函数（注意是包括了 `to_number(...)` 函数里面的参数）的模式的字符串，替换为类似 `decode(to_number(...), 0, '-', to_number(...))` 的模式：

```regexp
Search: to_number\((?:[^()]*|\((?:[^()]*|\([^()]*\))*\))*\)
Replace: decode($0, 0, '-', $0)
```

## IDEA 配置微服务的多实例

IDEA 判断一个项目为微服务时，会有一个【Services】选项。我们可以在该选项中快速管理微服务项目。

如果一个服务希望有多个实例，可以在【Services】选项中，找到希望开启多个实例的服务名，然后【右键】->【Copy Configuration...】打开【Edit Configuration】选项（也可以使用 ctrl + D / Command + D 打开）。

在【Edit Configuration】选项中，【Name】的输入框可以修改新实例的名称。

在【Edit Configuration】选项中，最关键的是修改端口。展开【Environment】，在【VM options】的输入框中，输入 `-Dserver.port=端口号` 来映射新的端口号。

设置完毕后，点击最下方的【OK】就可以了。

此时去【Not Started】里面就能找到新创建好的实例。

> 相关内容可以查看 [LearnDifferent/spring-cloud-demo 的 notes.md](https://github.com/LearnDifferent/spring-cloud-demo/blob/master/notes/notes.md#idea-%E9%85%8D%E7%BD%AE%E5%A4%9A%E5%AE%9E%E4%BE%8B%E6%8A%80%E5%B7%A7)

## 使用 Java 9 以上版本启动 JDK 8 及以前的程序时报错

问题描述：我使用的是 M 系列的 Mac，17.0.6 版本的 JDK 启动 Sentinel 1.8.1 的 jar 包时报错，详细内容可以查看 [我的 spring-cloud-demo 中的记录](https://github.com/LearnDifferent/spring-cloud-demo/blob/master/notes/Sentinel_Startup_Error.md)

简单来说就是因为 Java 9及以上版本中引入的模块系统（Project Jigsaw）。而我的错误信息中提到 `java.lang.reflect.InaccessibleObjectException` ，即尝试访问一个模块中的类或成员时，但没有相应的`opens`声明来允许这种访问。

从堆栈跟踪来看，问题出现在 Spring 框架使用 CGLIB 进行动态代理时，尝试访问 `java.lang.ClassLoader.defineClass` 方法，但是这个访问被Java模块系统阻止了。

解决方案就是添加 JVM 启动参数：

```bash
--add-opens java.base/java.lang=ALL-UNNAMED
```

这个参数的作用是允许所有未命名模块（即传统的 classpath 上的代码）访问 `java.base` 模块中的 `java.lang` 包。

>如果是在IDE中运行，需要在运行配置中添加这个 VM 选项

最后，我成功启动 Sentinel 的命令如下：

```bash
java --add-opens java.base/java.lang=ALL-UNNAMED -jar sentinel-dashboard-1.8.1.jar
```


