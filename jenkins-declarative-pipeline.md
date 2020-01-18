## 背景

Jenkins Pipeline 非常强大，其中 Jenkins Declarative Pipeline 声明式的语法又比较友好。下面记录的都是用 Pipeline 实现一些功能。

## 示例

### 拉取 Git 仓库代码

```groovy
pipeline {
  agent {
    label "master"
  }
  stages {
    stage ("checkout code") {
      git branch: "master", credentialsId: "xxx-xxx-xxx-xxx", url: "https://github.com/xxx/xxx.git"
    }
  }
}
```