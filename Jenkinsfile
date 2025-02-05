node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    
                    // Configure Git user details
                    sh "git config user.email 'thanmyinttun2021@gmail.com'"
                    sh "git config user.name 'Thanmyint'"
                    
                    // Display deployment.yaml before and after modification
                    sh "cat deployment.yaml"
                    sh "sed -i 's+02042025/dockerhub.*+02042025/dockerhub:${env.DOCKERTAG}+g' deployment.yaml"
                    sh "cat deployment.yaml"
                    
                    // Commit changes
                    sh "git add ."
                    sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                    
                    // Secure Git push with token authentication
                    sh '''
                      git remote set-url origin https://x-access-token:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git
                      git push origin HEAD:main
                    '''
                }
            }
        }
    }
}
