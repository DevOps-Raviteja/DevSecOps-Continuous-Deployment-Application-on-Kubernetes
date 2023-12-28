@Library('Jenkins-shared-library') _
pipeline{
    agent any 
    // tools {
    //     maven 'maven'
    // }
    // environment {
    //     SCANNER_HOME=tool 'sonar-scanner'
    // }
    stages{
        stage('Clean WorkSpace'){
            steps {
                cleanWs()
            }
        }
        stage('Git CheckOut'){
            steps{
                script{
                    gitCheckout(
                        branch: 'main',
                        url: 'https://github.com/Ravitejadarla5/DevSecOps-Continuous-Deployment-Application-on-Kubernetes.git'
                    )
                }
            }
        }
        stage('Unit Testing'){
            steps{
                script{
                    mvnTest()
                }
            }
        }
        // stage('Code Compile'){
        //     steps{
        //         sh 'mvn clean compile'
        //     }
        // }
        // stage('Unit Testing'){
        //     steps{
        //         sh 'mvn clean test'
        //         junit '**/target/surefire-reports/*.xml'
        //     }
        // }
        // stage('SonarQube Analysis'){
        //     steps{
        //         script{
        //             withSonarQubeEnv(credentialId: 'sonar-token'){
        //                 sh 'mvn sonar:sonar'
        //                 sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=petclinic -Dsonar.java.binaries=. -Dsonar.projectKey=petclinic"
        //             }
        //         }
        //     }
        // }
        // stage('SonarQube Quality Gate'){
        //     steps{
        //         script{
        //             waitForQualityGate abortPipeline:false, credentialId: 'sonar-token'
        //         }
        //     }
        // }
        // stage('Package-Install'){
        //     steps{
        //         sh 'mvn clean install'
        //         archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
        //     }
        // }
    }
}