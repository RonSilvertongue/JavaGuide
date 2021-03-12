# 1 Maven

## 1.1 Maven LifeCycles

> Maven is based around the central concept of a build lifecycle. What this means is that the process for building and distributing a particular artifact (project) is clearly defined.
>
> For the person building a project, this means that it is only necessary to learn a small set of commands to build any Maven project, and the [POM](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html) will ensure they get the results they desired.
>
> There are three built-in build lifecycles: default, clean and site. The `default` lifecycle handles your project deployment, the `clean` lifecycle handles project cleaning, while the `site` lifecycle handles the creation of your project's site documentation.

Maven有三种Lifecycle

- default
- clean
- site

Maven有多种Phase，其中几种为

- validate：检查项目的正确性和必要信息的可用性
  - 语法检查和依赖检查？
- compile：编译项目的源代码
- test：用合适的单元测试框架测试编译好的源代码，进行测试的前提是不要求源代码被打包或者部署
- package：把源代码打包发布，例如打包成jar包
- verify：为了保证符合质量标准，在集成测试上检查测试的结果
- install：把打包好的源代码放到本地仓库中，这样就可以作为依赖存在于其他项目
- deploy：将最终打包好的源代码发送到远程仓库，与其他开发者或者项目共享

选择不同的Phase可以组成一个Lifecycle

[不同Lifecycle所包含的Phase，这里你能查看到其他的Phase](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)





