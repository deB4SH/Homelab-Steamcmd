pipeline{
    //global agent - its just an maven jar
    agent {
        kubernetes {
            yaml '''
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: maven
                image: jeekajoo/maven-dind:latest
                command:
                - sleep
                args:
                - 9999999
                volumeMounts:
                - name: dockersock
                  mountPath: /var/run/docker.sock
              volumes:
              - name: dockersock
                hostPath:
                  path: /var/run/docker.sock
            '''
            defaultContainer 'maven'
        }
    }
    //options
    options{
            disableConcurrentBuilds()
            buildDiscarder(logRotator(numToKeepStr: '2'))
            disableResume()
            timeout(time: 1, unit: 'HOURS')
    }
    //define trigger
    triggers{
        pollSCM 'H/30 * * * *'
    }
    //stages to build and deploy
    stages {
        stage ('check: prepare') {
            steps {
                sh '''
                    mvn -version
                    export MAVEN_OPTS="-Xmx1024m"
                '''
            }
        }
        stage ('build: setVersion') {
            when {
                branch 'release/*'
            }
            environment {
                BRANCHVERSION = sh(
                    script: "echo ${env.BRANCH_NAME} | sed -E 's/release\\/([0-9a-zA-Z.\\-]+)/\\1/'",
                    returnStdout: true
                ).trim()
            }
            steps {
                echo 'Setting release version'
                echo "${BRANCHVERSION}"
                sh 'mvn versions:set -DnewVersion=${BRANCHVERSION} -f ./pom.xml'
            }
        }
        stage('build') {
            steps {
                sh 'mvn clean install -f pom.xml'
            }
        }
        stage ('deploy: whenRelease') {
            when {
                branch 'release/*'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-push-token', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh 'docker login ghcr.io -u $user -p $pass'
                    sh 'mvn docker:push -f pom.xml'
                }
            }
        }
    }

}
