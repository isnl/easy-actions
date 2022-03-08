## 前言

### Github 是什么？

以下来自百度百科：

> Github 是一个面向开源及私有软件项目的托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 Github。

我们把它拆开，git hub，字面意思就是 Git 中心枢纽的意思，其实 Github 就是这么一个项目，它是基于 Git 的，代码托管平台，故名 Github。

相比 Git，Github 提供了更多的功能，比如 Web 管理界面，评论，组织，点赞、关注、图表，俨然已经是一个基于 Github 的社交网站，大家围绕着开源项目，进行使用、讨论，贡献等。

### Github 几个重要的里程碑

- GitHub 平台于 2007 年 10 月 1 日开始开发，由 GitHub 公司（曾称 Logical Awesome）的开发者 Chris Wanstrath、PJ Hyett 和 Tom Preston-Werner 使用 Ruby on Rails 编写而成。网站于 2008 年 2 月以 beta 版本开始上线，4 月份正式上线。

- 2011 年 11 月，启动 GitHub Enterprise 项目，探索盈利模式。也是在 11 月，Github 拥有了 100 万用户。

- 2012 年 7 月，GitHub 在由 Andreessen Horowitz 领导的 A 轮融资中筹集了 1 亿美元。
- 2013 年 3 月，GitHub 达到了 300 万用户 2013 年 12 月，GitHub 上托管了 1000 万个存储库

- 2018 年 6 月 4 日晚，微软宣布，通过 75 亿美元的股票交易收购 GitHub。 10 月 26 日，微软以 75 亿美元收购 GitHub 交易已完成。10 月 29 日，微软开发者服务副总裁奈特·弗里德曼（Nat Friedman）将成为 GitHub 的新一任 CEO

- 2019 年 1 月份，GitHub 宣布私有仓库全部免费，无限创建，但是最多只有有三个合作者

- 2020 年 3 月 17 日，Github 宣布收购 npm，GitHub 现在已经保证 npm 将永远免费使用。

### 引申概念（CI/CD）

##### 持续集成 CI（Continuous integration）

互联网软件的开发和发布，已经形成了一套标准流程，最重要的组成部分就是持续集成（Continuous integration，简称 CI）

持续集成指的是，频繁地（一天多次）将代码集成到主干。

持续集成的目的，就是让产品可以快速迭代，同时还能保持高质量。它的核心措施是，代码集成到主干之前，必须通过自动化测试。只要有一个测试用例失败，就不能集成。

##### 持续交付 CD（Continuous delivery）

持续交付（Continuous delivery）指的是，频繁地将软件的新版本，交付给质量团队或者用户，以供评审。如果评审通过，代码就进入生产阶段。

持续交付可以看作持续集成的下一步。它强调的是，不管怎么更新，软件是随时随地可以交付的。

### Github Actions 介绍

GitHub Actions 是 GitHub 的持续集成服务，于 2018 年 10 月推出。

![https://static.iiter.cn/article/417f45beda6e1da9ff70f9413bc1db45.png](https://static.iiter.cn/article/417f45beda6e1da9ff70f9413bc1db45.png)

持续集成由很多操作组成，比如抓取代码、运行测试、登录远程服务器，发布到第三方服务等等。GitHub 把这些操作就称为 actions。

很多操作在不同项目里面是类似的，完全可以共享。GitHub 注意到了这一点，想出了一个很妙的点子，允许开发者把每个操作写成独立的脚本文件，存放到代码仓库，使得其他开发者可以引用。

如果你需要某个 action，不必自己写复杂的脚本，直接引用他人写好的 action 即可，整个持续集成过程，就变成了一个 actions 的组合。这就是 GitHub Actions 最特别的地方。

GitHub 做了一个官方市场，可以搜索到他人提交的 actions。另外，还有一个 awesome actions 的仓库，也可以找到不少 action（[https://github.com/sdras/awesome-actions](https://github.com/sdras/awesome-actions)）

![Github 官方actions市场](https://static.iiter.cn/article/69ea143cdfdf3ae919947648c496b782.png)

上面说了，每个 action 就是一个独立脚本，因此可以做成代码仓库，使用 userName/repoName 的语法引用 action。比如，actions/setup-node 就表示 [https://github.com/actions/setup-node](https://github.com/actions/setup-node) 这个仓库，它代表一个 action，作用是安装 Node.js。事实上，GitHub 官方的 actions 都放在 [https://github.com/actions](https://github.com/actions) 里面。

既然 actions 是代码仓库，当然就有版本的概念，用户可以引用某个具体版本的 action。下面都是合法的 action 引用.

```
actions/setup-node@74bc508 # 指向一个 commit
actions/setup-node@v1.0    # 指向一个标签
actions/setup-node@master  # 指向一个分支
```

### Github Actions 基本概念

GitHub Actions 有一些自己的术语。

- 1.workflow （工作流程）：持续集成一次运行的过程，就是一个 workflow。

- 2.job （任务）：一个 workflow 由一个或多个 jobs 构成，含义是一次持续集成的运行，可以完成多个任务。

- 3.step（步骤）：每个 job 由多个 step 构成，一步步完成。

- 4.action （动作）：每个 step 可以依次执行一个或多个命令（action）。

### workflow 文件

GitHub Actions 的配置文件叫做 `workflow` 文件，存放在代码仓库的`.github/workflows`目录。

workflow 文件采用 `YAML` 格式，文件名可以任意取，但是后缀名统一为`.yml`，比如`foo.yml`。一个库可以有多个 `workflow` 文件。GitHub 只要发现`.github/workflows`目录里面有`.yml`后缀的文件，就会自动运行该文件。

workflow 文件的配置字段非常多，详见[官方文档](https://help.github.com/en/articles/workflow-syntax-for-github-actions)。我们只介绍一些常用的。

（1）name

name 字段是 workflow 的名称。如果省略该字段，默认为当前 workflow 的文件名。

```yml
name: GitHub Actions Demo
```

（2）on

on 字段指定触发 workflow 的条件，通常是某些事件。

```yml
on: push
```

上面代码指定，push 事件触发 workflow。

on 字段也可以是事件的数组。

```yml
on: [push, pull_request]
```

上面代码指定，push 事件或 pull_request 事件都可以触发 workflow。

完整的事件列表，请查看[官方文档](https://help.github.com/en/articles/events-that-trigger-workflows)。除了代码库事件，GitHub Actions 也支持外部事件触发，或者定时运行。

（3）on.<push|pull_request>.<tags|branches>

指定触发事件时，可以限定分支或标签。

```yml
on:
  push:
    branches:
      - master
```

上面代码指定，只有 master 分支发生 push 事件时，才会触发 workflow。

（4）jobs.<job_id>.name

workflow 文件的主体是 jobs 字段，表示要执行的一项或多项任务。

jobs 字段里面，需要写出每一项任务的 job_id，具体名称自定义。job_id 里面的 name 字段是任务的说明。

```yml
jobs:
  my_first_job:
    name: My first job
  my_second_job:
    name: My second job
```

上面代码的 jobs 字段包含两项任务，job_id 分别是 my_first_job 和 my_second_job。

（5）jobs.<job_id>.needs

needs 字段指定当前任务的依赖关系，即运行顺序。

```yml
jobs:
  job1:
  job2:
    needs: job1
  job3:
    needs: [job1, job2]
```

上面代码中，job1 必须先于 job2 完成，而 job3 等待 job1 和 job2 的完成才能运行。因此，这个 workflow 的运行顺序依次为：job1、job2、job3。

（6）jobs.<job_id>.runs-on

runs-on 字段指定运行所需要的虚拟机环境。它是必填字段。目前可用的虚拟机如下。

- ubuntu-latest，ubuntu-18.04 或 ubuntu-16.04
- windows-latest，windows-2019 或 windows-2016
- macOS-latest 或 macOS-10.14

下面代码指定虚拟机环境为 ubuntu-18.04。

```yml
runs-on: ubuntu-18.04
```

（7）jobs.<job_id>.steps

steps 字段指定每个 Job 的运行步骤，可以包含一个或多个步骤。每个步骤都可以指定以下三个字段。

jobs.<job_id>.steps.name：步骤名称。
jobs.<job_id>.steps.run：该步骤运行的命令或者 action。
jobs.<job_id>.steps.env：该步骤所需的环境变量。


### React项目发布到Github Pages

```

   
name: GitHub Actions Build and Deploy Demo
on:
  push:
    branches:
      - master
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2 # If you're using actions/checkout@v2 you must set persist-credentials to false in most cases for the deployment to work correctly.
      with:
        persist-credentials: false
    - name: Install and Build
      run: |
        npm install
        npm run-script build
    - name: Deploy
      uses: JamesIves/github-pages-deploy-action@releases/v3
      with:
        ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
        BRANCH: gh-pages
        FOLDER: build
```
上面这个 workflow 文件的要点如下。

- 整个流程在master分支发生push事件时触发。
- 只有一个job，运行在虚拟机环境ubuntu-latest。
- 第一步是获取源码，使用的 action 是actions/checkout。
- 第二步是构建和部署，使用的 action 是JamesIves/github-pages-deploy-action。
- 第二步需要四个环境变量，分别为 GitHub 密钥、发布分支、构建成果所在目录、构建脚本。其中，只有 GitHub 密钥是秘密变量，需要写在双括号里面，其他三个都可以直接写在文件里。