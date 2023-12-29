@Library('Jenkins-shared-library') _
pipeline{
    agent any 
    // tools {
    //     maven 'maven'
    // }
    // environment {
    //     SCANNER_HOME=tool 'sonar-scanner'
    // }
    parameters{
        choice(
            name: 'action',
            choices: 'create\ndelete',
            description: 'Chose create/Destroy'
        )
    }
    stages{
        stage('Clean WorkSpace'){
            steps {
                cleanWs()
            }
        }
        stage('Git CheckOut'){
            when { expression { params.action == 'create' } }
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
            when { expression { params.action == 'create' } }
            steps{
                script{
                    mvnTest
                }
            }
        }
        stage('Integration Testing'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    mvnIntegrateTest()
                }
            }
        }
        stage('SonarQube Analysis'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    def sonarCred = 'sonar-api'
                    sonarqube(sonarCred)
                }
            }
        }
        stage('Quality Gate Analysis'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    def sonarCred = 'sonar-api'
                    qualityGate(sonarCred)
                }
            }
        }
        stage('Maven Build Analysis'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    def sonarCred = 'sonar-api'
                    qualityGate(sonarCred)
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
        // stage('Package-Install'){
        //     steps{
        //         sh 'mvn clean install'
        //         archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
        //     }
        // }
    }
}