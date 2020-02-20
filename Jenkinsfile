#!groovy

@Library('jenkinslib') _

def tools = new org.devops.tools()
//看是否执行vars目录里面类的方法
hello()


String workspace = "/opt/jenkins/workspace"

//Pipeline
pipeline{
	agent { node { label "master"  //指定具有该标签的节点运行
	         customWorkspace "${workspace}"
	         // 指定工作目录(可选)
	   }
	}	
	parameters { string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '') }
	options {
		timestamps() 
		//记录日志的时间
		skipDefaultCheckout() //删除隐式checkout-->省得下代码
		disableConcurrentBuilds()
		//禁止并行
		timeout(time: 1, unit: 'HOURS')
		//流水线的超时时间

	}

	stages{
		//（1）下载代码
		//阶段名称
		stage("GetCode"){
		    //when { environment name: 'test', value: 'abcd'}
			steps{
				// 步骤超时时间
				timeout(time:5,unit:"MINUTES"){
					//填写运行的代码
					script{
						println('获取代码')
						tools.PrintMes('获取代码','green')
					}
				}
			}
		}
		// 并行配置配置
		stage("parallel"){
		failFast true
		parallel {
		//（2）构建
		stage("Build"){
			steps{
				timeout(time:20,unit:"MINUTES"){
					script{
						println('应用打包')
						println("${test}")
					}
				}
			}
		}

		//（3）代码扫描
		stage("CodeScan"){
			steps{
				timeout(time:30,unit:"MINUTES"){
					script{
						print("代码扫描")
						mv = tool "maven"
						sh "${mv}/bin/mvn --version"
						// 调用lib
						tools.PrintMes('代码扫描!','red')
					}
				}
			}
		}
  }
 }	
}

	//构建后的操作
	post{
		always {
			script{
				println("always")
			}
		}
		success {
			script{
				currentBuild.description += "\n 构建成功"
			}
		}
		failure{
			script{
				currentBuild.description += "\n 构建失败"
			}
		}
		aborted {
			script{
				currentBuild.description += "\n 构建取消"
			}
		}
	}
}
