@Library('aol-on-jenkins-lib') _

pipeline {
    agent {
        node {
            label "${getLabelCompile()}"
        }
    }

    options {
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    stages {
        stage('Prepare') {
            steps {
                cleanup()
                checkout scm
            }
        }
        stage("build image") {
            steps {
                sh 'docker build . -t main-virtual.docker.vidible.aolcloud.net/main/aol-on-android-sdk'
            }
        }

        stage("Deploy") {
            when {
                branch 'master'
            }
            steps {
                sh '''docker tag main-virtual.docker.vidible.aolcloud.net/main/aol-on-android-sdk main-virtual.docker.vidible.aolcloud.net/main/aol-on-android-sdk:latest
                      docker tag main-virtual.docker.vidible.aolcloud.net/main/aol-on-android-sdk main-virtual.docker.vidible.aolcloud.net/main/aol-on-android-sdk:1.$BUILD_NUMBER

                      docker images
                      docker push main-virtual.docker.vidible.aolcloud.net/main/aol-on-android-sdk:latest
                      docker push main-virtual.docker.vidible.aolcloud.net/main/aol-on-android-sdk:1.$BUILD_NUMBER'''
            }
        }
    }
}
