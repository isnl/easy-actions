# umi project

## Getting Started

Install dependencies,

```bash
$ yarn
```

Start the dev server,

```bash
$ yarn start
```


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