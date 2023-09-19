pipeline {
    agent { label 'GOI' }
    options {
        retry(1)
        timeout (time: 30, unit:'MINUTES')
    }
    triggers {
        pollSCM('* * * * *')
    }
    tools {
        jdk 'JDK-8'
        maven 'Maven-3.9'
    }
    parameters {choice(name: 'CHOICE', choices: ['package', 'clean package', 'install', 'clean install'])}
    stages {
        stage('static code analysis'){
            steps{
    withSonarQubeEnv('SONAR_QUBE') { // If you have configured more than one global server connection, you can specify its name
      sh 'mvn clean package sonar:sonar -Dsonar.AkhilTejas -Dsonar.token=f5e9ddab8fc6837562a391d728a613974a643eab -Dsonar.projectKey=akhiltejas'
        }
        }
        }
    stage('reporting') {
            steps {
                
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