pipeline {
    agent any

    stages {
        stage ('Clone') {

            steps {
                withMaven(maven : 'maven') {
                    sh 'mvn clean install -Dmaven.test.skip=true'
                    
                }
            }
        }
        stage ('Build Docker') {
            steps {
                sh 'docker pull ashishfulcrum/weblogic_server:11g'
            }
        }

        stage ('Run Docker') {
            
            steps {
                echo "${env.BRANCH_NAME}"
                echo "${currentBuild.number}"
                //bat ("docker stop spring${env.BRANCH_NAME}${currentBuild.number}")
                //bat ("docker rm spring${env.BRANCH_NAME}${currentBuild.number}")
                sh ("docker run --name weblogic${env.BRANCH_NAME}${currentBuild.number} -d -p 7001 ashishfulcrum/weblogic_server:11g")
            }
        }

         stage('Set Weblogic Port') {
            steps {
                script {
                    weblogicPortNumber = sh(returnStdout: true, script: "docker ps|grep weblogic${env.BRANCH_NAME}${currentBuild.number}|sed 's/.*0.0.0.0://g'|sed 's/->.*//g'")
                    echo 'Weblogic port number: ' + weblogicPortNumber
                }
            }
        }


       
    }
}
