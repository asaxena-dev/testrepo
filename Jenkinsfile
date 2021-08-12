def type = "Build"

def wstatus (String varstat) {
    println (varstat)
}

def gitcheckout() {
    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'outscripts'], [$class: 'CleanBeforeCheckout']], gitTool: 'git', submoduleCfg: [], userRemoteConfigs: [[credentialsId: '945e8f1b-3a65-45b1-96a5-7ff4df589b3b', refspec: '+refs/heads/master:refs/remotes/origin/master', url: 'https://github.com/asaxena-dev/testrepo']]]);
}

pipeline {
    parameters {
        string(name:'testRun', defaultValue:'yes',description: 'Want to run Test?',trim:true)
    }
    agent "none"
    stages {
        stage("Build") {
            agent {
                label "master"
            }
            steps {               
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'outscripts'], [$class: 'CleanBeforeCheckout']], gitTool: 'git', submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'MyGitHubCred', refspec: '+refs/heads/master:refs/remotes/origin/master', url: 'https://github.com/asaxena-dev/mslearn-tailspin-spacegame-web']]])
                input("Do you want to continue")
                sh "echo Building: "+type
               
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
                label "master"
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
