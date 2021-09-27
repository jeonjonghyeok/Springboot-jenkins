pipeline {
    agent any
    options {
        timeout(time: 1, unit: 'HOURS')
    }
    environment {
        SOURCECODE_JENKINS_CREDENTIAL_ID = 'jenkins-github-wh'
        SOURCE_CODE_URL = 'https://github.com/MinjiY/Springboot-jenkins.git'
        RELEASE_BRANCH = 'master'
    }
    stages {
        stage('Init') {
            steps {
                echo 'clear'
                //sh 'docker stop $(docker ps -aq)'
                sh 'docker stop demo'
                //sh 'docker rm $(docker ps -aq)'
                sh 'docker rm demo'
                deleteDir()
            }
        }
        stage('clone') {
            steps {
                git url: "$SOURCE_CODE_URL",
                    branch: "$RELEASE_BRANCH",
                    credentialsId: "$SOURCECODE_JENKINS_CREDENTIAL_ID"
                sh "ls -al"
            }
        }
        stage('dockerizing') {
            steps {
                //sh "pwd"
                //dir("./backend"){
                    sh "pwd"
                    sh "chmod +x ./gradlew"
                    sh "./gradlew build --stacktrace"
                    //sh "gradle wrapper --stacktrace"
                    //sh "gradle bootJar"

                    sh "docker build -t upi907/demodeploy ."
                //}
            }
        }
        stage('deploy') {
            steps {
                sh '''
                
                  docker run -d -p 8080:8080 --name demo upi907/demodeploy
                '''
            }
        }
    }
}
