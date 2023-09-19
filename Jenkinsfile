pipeline {
    agent { label 'GOI' }
    options {
        retry(3)
        timeout (time: 30, unit:'MINUTES')
    }
    triggers {
        pollSCM('* * * * *')
    }
    tools {
        jdk 'JDK-8'
        maven 'Maven 3.9'
    }
    parameters {choice(name: 'CHOICE', choices: ['package', 'clean package', 'install', 'clean install'])}
    stages {
        stage('VCS') {
            steps {
                git url: 'https://github.com/AkhilTejas/game-of-life.git',
                    branch: 'master'
            }
        }
        stage('build and package') {
            steps {
                sh script: "mvn $params.CHOICE"
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
            mail subject: "This $JOB_NAME is successfull",
                 body: "This build is successfull \n build url $BUILD_URL",
                 to: 'info@test.com'
        }
        failure {
            mail subject: "This $JOB_NAME is failure",
                 body: "This build is failure \n build url $BUILD_URL",
                 to: 'info@test.com'
        }    
    }
}