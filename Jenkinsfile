@Library('sharedlibrary')_

pipeline {
    environment {
        dockerImage = "artisantek/nov23-jenkins"
        branchName = "main"
        dockerTag = "ver-${BUILD_NUMBER}"
        helmChartName="nodejs-useraccount"
    }
    
    agent none
    stages {
        
        stage('Git-Checkout') {
            parallel {
                stage ('Docker') {  
                agent {label 'docker'} 
                    steps {
                        gitCheckout('https://github.com/artisantek/nodejs-useraccount.git', 'main', 'githubCred')
                    }
                }

                stage ('Kubernetes') {  
                    agent {label 'k8s'} 
                    steps {
                        gitCheckout('https://github.com/artisantek/nodejs-useraccount.git', 'main', 'githubCred')
                    }
                }
            }
        }

        stage('Git Checkout-k8s') {
            agent {label 'k8s'}
            steps {
                gitCheckout('https://github.com/artisantek/nodejs-useraccount.git', 'main', 'githubCred')
            }
        }

        stage('Docker Build') {
            agent {label 'docker'}
            steps {
                dockerImageBuild('$dockerImage', '$dockerTag')
            }
        }

        stage('Docker Push') {
            agent {label 'docker'}
            steps {
                dockerHubImagePush('$dockerImage', '$dockerTag', 'dockerhubCred')
            }
        }

        stage('Kubernetes Deploy') {
            agent {label 'k8s'}
            steps {
                kubernetesHelmDeploy('$dockerImage', '$dockerTag', '$helmChartName')
            }
        }

    }
}