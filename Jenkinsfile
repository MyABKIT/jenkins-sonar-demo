pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    stages{
        
        stage('checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/vcjain/jenkins-sonar-demo.git']])
            }    
        }
        stage('Build'){
            steps{
                echo 'Building Maven project'
                sh 'mvn clean compile'
            }
        }
        stage('SonarQube Analysis'){
            steps{
                echo 'Scanning Maven project'
                withCredentials([string(credentialsId: 'sonar_cred', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv(installationName: 'sonar_cloud', credentialsId: 'sonar_cred') { 
                        sh 'mvn sonar:sonar -Dsonar.projectKey=sonardemo1 -Dsonar.organization=myaborg -Dsonar.host.url=https://sonarcloud.io'
                        sh 'sleep 50'
                        
                    }
                }    
            }
        }
        stage('Test'){
            steps{
                echo 'Building Maven project'
                sh 'mvn test'
                junit '**/target/surefire-reports/*.xml'
                jacoco classPattern: '**/target/classes', exclusionPattern: '**/*Test*.class', execPattern: '**/target/jacoco.exec', inclusionPattern: '**/*.class', sourceExclusionPattern: 'generated/**/*.java', sourceInclusionPattern: '**/*.java'
            }
        }
    }
        

}
