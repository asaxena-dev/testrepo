def type = "Build"

def wstatus (String varstat) {
    println (varstat)
}

def gitcheckout() {
    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'outscripts'], [$class: 'CleanBeforeCheckout']], gitTool: 'git', submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'MyGitHubCred', refspec: '+refs/heads/master:refs/remotes/origin/master', url: 'https://github.com/asaxena-dev/testrepo']]]);
}

pipeline {
    parameters {
        string(name:'testRun', defaultValue:'yes',description: 'Want to run Test?',trim:true)
    }
    agent "none"
    stages {
        stage("Build") {
            
            steps {
                checkout scm
                
              //  checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'outscripts'], [$class: 'CleanBeforeCheckout']], gitTool: 'git', submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'MyGitHubCred', refspec: '+refs/heads/master:refs/remotes/origin/master', url: 'https://github.com/asaxena-dev/mslearn-tailspin-spacegame-web']]])
             //   input("Do you want to continue")
                bat "echo Building: "+type
               
                wstatus "Sucessful"
                
            }
        }       
        stage("Test") {
            when {
                expression {
                    testRun == 'yes'
                }
            }
            agent {
                label "testLinux"
            }
            stages {
                stage('Test') {
 
                parallel {
                    stage('Test-A') {
                        steps {
                            script {
                                type = "Test"
                            }                
                            sh "echo Testing A: "+type
                        }
                    }
                    stage('Test-B') {
                        steps {
                            sh "echo Test-B"   
                        }
                    }
                }
                }
            }
        }
    }
    post {
        success {
            wstatus "Sucessful"
        }
        failure {
            wstatus "Failed"
        }
    }
}
