pipeline {
    agent { label 'life' }
    options {
        retry(3)
        timeout (time: 30, unit:'MINUTES')
    }
    triggers {
        pollSCM('* * * * *')
    }
    tools {
        jdk 'java-8'
        maven 'Maven-3.9'
    }
    stages {
        stage('VCS') {
            steps {
                git url: 'https://github.com/AkhilTejas/game-of-life.git',
                    branch: 'master'
            }
        }
        stage('build and package') {
            steps {
                sh script: 'mvn package'
            }
        }
        stage('reporting') {
            steps {
                archiveartifacts artifacts: '**/target/gameoflife.war'
                junit testresults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}