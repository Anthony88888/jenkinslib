#!groovy

@Library('jenkinslib') _

def tools = new org.devops.tools()

String workspace = "/opt/jenkins/workspace"

//Pipeline
pipeline{
//指定运行此流水线的节点
agent { node { label "master"
        customWorkspace "${workspace}"  //指定运行工作目录 (可选)
        }
}

parameters { string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '') }


options {
    timestamps()  //日志会有时间
    skipDefaultCheckout()  //删除隐式checkout scm语句
    disableConcurrentBuilds()  //禁止并行
    timeout(time: 1, unit: 'HOURS')  //流水线超时设置1h
}
    

//流水线的阶段
stages{
    //阶段1 下载代码
    stage("GetCode"){  //阶段名称
        when { environment name: 'DEPLOY_ENV', value: 'staging' }
        steps{  //步骤
            timeout(time:5, unit:"MINUTES"){ //步骤超时时间
                script{ //填写运行代码
                    println("获取代码")
                    
                    input id: 'Test', message: '我们是否要继续?', ok: '是,继续吧.', parameters: [choice(choices: ['a', 'b'], description: '', name: 'test1')], submitter: 'curry'
                }
            }
        }
    }

    stage("01"){
        failFast true
        parallel {

     //构建
    stage("Build"){
        steps{
            timeout(time:20, unit:"MINUTES"){
                script{
                    println("应用打包")
                    
                    mvnHome = tool  "m2"
                    println(mvnHome)
                    
                    sh "${mvnHome}/bin/mvn --version"
                }
            }
        }
    }

    //代码扫描
    stage("CodeScan"){
        steps{
            timeout(time:30, unit:"MINUTES"){
                        script{
                            println("代码扫描")

                            tools.PrintMes("this is my lib!")
                        }
                    }
                }
            }   
        }
    }
}

//构建后操作
post {
    always{
        script{
            println("always")
        }
    }
        
    success{
        script{
            currentBuild.description = "\n 构建成功!"
        }
   }

    failure{
        script{
            currentBuild.description = "\n 构建失败!"
        }
    }
        
    aborted{
        script{
            currentBuild.description = "\n 构建取消!"
        }
    }
}
}
