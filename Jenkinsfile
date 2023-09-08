pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git credentialsId: '6383358799', 
                url: 'https://github.com/VishalKumar-S/Jenkins-ArgoCD-Pipeline-Project',
                branch: 'main'
           }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t abhishekf5/cicd-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push abhishekf5/cicd-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: '6383358799', 
                url: 'https://github.com/VishalKumar-S/Argo-cd-cicd-project-manifest-repo.git',
                branch: 'main'
            }
        }
        
        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    withCredentials([usernamePassword(credentialsId: '6383358799', passwordVariable: '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', usernameVariable: 'VishalKumar-S')]) {
                        sh '''
                        cat deploy.yaml
                        sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/VishalKumar-S/Argo-cd-cicd-project-manifest-repo.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
}
