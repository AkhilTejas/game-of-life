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
                archiveArtifacts artifacts: '**/target/gameoflife.war'
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
    post {
        success {
            mail subject: 'This build is successfull',
                 body: 'This build is successfull',
                 to: 'info@test.com'
        }
        failure {
            mail subject: 'This build is failure',
                 body: 'This build is failure',
                 to: 'info@test.com'
        }    
    }
}