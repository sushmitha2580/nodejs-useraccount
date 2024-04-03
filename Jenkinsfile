@Library('sharedlibrary')_

pipeline {
    environment {
        dockerImage = "artisantek/nov23-jenkins"
        branchName = "main"
        dockerTag = "ver-${BUILD_NUMBER}"
        helmChartName="nodejs-useraccount"
    }
    
    agent {label 'docker'}
    stages {
        stage('Git Checkout') {
            steps {
                gitCheckout('https://github.com/artisantek/nodejs-useraccount.git', 'main', 'githubCred')
            }
        }

        stage('Docker Build') {
            steps {
                dockerImageBuild('$dockerImage', '$dockerTag')
            }
        }

        stage('Docker Push') {
            steps {
                dockerHubImagePush('$dockerImage', '$dockerTag', 'dockerhubCred')
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                kubernetesHelmDeploy('$dockerImage', '$dockerTag', '$helmChartName')
            }
        }

    }
}