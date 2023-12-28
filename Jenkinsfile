pipeline{
    agent any 
    tools {
        jdk 'JDK'
        maven 'MVN'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'

    }
    stages{
        stage('Clean WorkSpace'){
            steps {
                cleanWs()
            }
        }
        stage('Git CheckOut'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Ravitejadarla5/DevSecOps-Continuous-Deployment-Application-on-Kubernetes.git']])
            }
        }
        stage('Code Compile'){
            steps{
                sh 'mvn clean compile'
            }
        }
        stage('Unit Testing'){
            steps{
                sh 'mvn clean test'
                junit '**/target/surfire-reports/*.xml'
            }
        }
        stage('SonarQube Analysis'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token'){
                        sh 'mvn sonar:sonar'
                        sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=petclinic -Dsonar.java.binaries=. -Dsonar.projectKey=petclinic"
                    }
                }
            }
        }
        stage('SonarQube Quality Gate'){
            steps{
                script{
                    waitForQualityGate abortPipeline:false, credentialsId: 'sonar-token'
                }
            }
        }
        stage('Package-Install'){
            steps{
                sh 'mvn clean install'
                archiveArtifacts artifacts: 'target/*.war', followSysmlinks: false 
            }
        }
    }
}