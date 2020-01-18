# Jenkins Declarative Pipeline Examples

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

1.直接 shell 命令跳转到某个目录

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

2.用 Pipeline 的特定语法

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

### 设置参数

1.选项

```groovy
properties([
  parameters([
    choice(
      choices: ["a","b","c"],
      description: "name of project",
      name: "PROJECT"
    )
  ])
])

pipeline {
  agent {
    label "master"
  }
  stages {
    stage ("get project name") {
      steps {
        echo "You choose ${params.PROJECT} project."
      }
    }
  }
}
```

2.动态选项（动态获取 Git 仓库分支名列表）

```groovy
properties([
  parameters([
    [
      $class: "CascadeChoiceParameter",
      choiceType: "PT_SINGLE_SELECT",
      description: "branch of git repository",
      filterLength: 1,
      filterable: false,
      name: "BRANCH_NAME",
      randomName: "xxx-xxx-xxx",
      referencedParameters: "PROJECT",
      script: [
        $class: "GroovyScript",
        fallbackScript: [
          classpath: [],
          sandbox: true,
          script: "return ['error']"
        ],
        script: [
          classpath: [],
          sandbox: true,
          script:
            '''
            def gettags = ("git ls-remote -t -h git@gitlab.lrts.me:fed/${PROJECT}.git").execute()
            return gettags.text.readLines().collect { 
              it.split()[1]
                .replaceAll(\"refs/heads/\", \"\")
                .replaceAll(\"refs/tags/\", \"\")
                .replaceAll("\\\\^\\\\{\\\\}", \"\")
            }
            '''
        ]
      ]
    ]
  ])
])

pipeline {
  agent {
    label "master"
  }
  stages {
    stage ("get branch name") {
      steps {
        echo params.BRANCH_NAME
      }
    }
  }
}
```

3.字符串参数

```groovy
properties([
  parameters([
    string(
      defaultValue: "Nothing",
      description: "name of environment",
      name: "ENV_NAME",
      trim: true
    )
  ])
])

pipeline {
  agent {
    label "master"
  }
  stages {
    stage ("get environment name") {
      steps {
        echo params.ENV_NAME
      }
    }
  }
}
```

### 判断上一次构建到现在有没有代码变化

```groovy
pipeline {
  agent {
    label "master"
  }
  stages {
    stage ("checkout code") {
      steps {
        script {
          scmVars = git branch: "master",
            credentialsId: "xxx-xxx-xxx-xxx",
            url: "https://github.com/xxx/xxx.git"
          nothingChanged = (scmVars.GIT_COMMIT == scmVars.GIT_PREVIOUS_SUCCESSFUL_COMMIT) ? "true" : "false"
        }
      }
    }
    stage ("check params") {
      steps {
        echo scmVars.GIT_COMMIT
        echo scmVars.GIT_PREVIOUS_SUCCESSFUL_COMMIT
        echo scmVars.GIT_BRANCH
        echo nothingChanged
      }
    }
  }
}
```

### 判断某个文件或目录是否存在

```groovy
pipeline {
  agent {
    label "master"
  }
  stages {
    stage ("check file exists") {
      steps {
        script {
          existed = fileExists "hello.txt"
          echo existed
        }
      }
    }
  }
}
```

### 发送邮件

```groovy
pipeline {
  agent {
    label "master"
  }
  stages {
    stage () {
      steps {
        emailext body:
          '''
          <p>${DEFAULT_CONTENT}</p>
          '''
        subject: '${DEFAULT_SUBJECT}',
        to: 'somebody@xxx.com'
      }
    }
  }
}
```
