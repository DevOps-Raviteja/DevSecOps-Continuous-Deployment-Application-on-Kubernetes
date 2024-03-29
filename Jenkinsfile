@Library('Jenkins-shared-library') _
pipeline{
    agent any
    parameters{
        choice(
            name: 'action',
            choices: 'create\ndelete',
            description: 'Chose create/Destroy'
        )
        string(
            name: 'ImageName', 
            description: 'Name of the Docker Build', 
            defaultValue: 'javapp'
        )
        string(
            name: 'ImageTag',
            description: 'Tag of the docker Build', 
            defaultValue: 'v1'
        )
        string(
            name: 'DockerHubUser', 
            description: 'Name of the Application', 
            defaultValue: 'ravitejadarla5'
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
                    mvnTest()
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
        stage('Maven Build'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    mvnBuild()
                }
            }
        }
        stage('Docker Build'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    dockerBuild(
                        "${params.ImageName}",
                        "${params.ImageTag}",
                        "${params.DockerHubUser}"
                    )
                }
            }
        }
        stage('Docker Image Scan Trivy'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    trivyImgScan(
                        "${params.ImageName}",
                        "${params.ImageTag}",
                        "${params.DockerHubUser}"
                    )
                }
            }
        }
        stage('Docker Push'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    dockerPush(
                        "${params.ImageName}",
                        "${params.ImageTag}",
                        "${params.DockerHubUser}"
                    )
                }
            }
        }
        stage('Deploying'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                        sh 'kubectl apply -f deployment.yaml'
                    }
                }
            }
        }
    }
    post {
        always {
            emailext attachLog: true,
            subject: "'${currentBuild.result}'",
            body: "Project: ${env.JOB_NAME} <br/>" + "Build Number: ${env.BUILD_NUMBER} <br/>" + "url: ${env.BUILD_URL} <br/>",
            to: 'ravitejadarla5@gmail.com',
            attachmentsPattern: 'trivyfs.txt, trivyImage.txt' 
        }
    }
}
