pipeline {
    agent any
    
    stages {
        
    stage('SonarQube analysis') {
    // requires SonarQube Scanner 2.8+
    steps {
    def sonarHome= tool name: 'scanner-3.2', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    withSonarQubeEnv('sonarqube209') {
      sh "${sonarHome}/bin/sonar-runner -Dsonar.host.url=${SONAR_HOST_URL}  -Dsonar.login=${SONAR_AUTH_TOKEN}    -Dsonar.projectName=cloudfuse -Dsonar.projectVersion=1.0 -Dsonar.projectKey=test:cloudfuse -Dsonar.sources=."
        }
      }
     }
        
        stage('Build') {
            steps {
                echo 'Building Cloudfuse code'
                sh 'apt-get install build-essential libcurl4-openssl-dev libxml2-dev libssl-dev libfuse-dev libjson-c-dev pkg-config -y'
                sh './configure'
                sh 'make'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }
            steps {
                echo 'Deploying....'
                sh 'make install'
            }
        }
    }
}
