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
      steps {
        git branch: "master",
          credentialsId: "xxx-xxx-xxx-xxx",
          url: "https://github.com/xxx/xxx.git"
      }
    }
  }
}
```

### 执行 shell 命令

```groovy
pipeline {
  agent {
    label "master"
  }
  stages {
    stage ("run shell commands") {
      steps {
        sh "npm run install"
        sh "./run.sh param"
      }
    }
  }
}
```

### 跳转到特定的目录执行命令

1. 直接 shell 命令跳转到某个目录

```groovy
pipeline {
  agent {
    label "master"
  }
  stages {
    stage ("run command in directory") {
      steps {
        sh "cd ./directory"
        sh "npm run start"
      }
    }
  }
}
```

2. 用 Pipeline 的特定语法

```groovy
pipeline {
  agent {
    label "master"
  }
  stages {
    stage ("run command in directory") {
      steps {
        dir ("./directory") {
          sh "npm run start"
        }
      }
    }
  }
}
```
