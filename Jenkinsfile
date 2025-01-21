pipeline {
    agent any

    environment {
        GITHUB_USER = 'shivajigree'   // Replace with your GitHub username
        GITHUB_REPO = 'https://github.com/shivajigree/PersonalPage.git'
        DEPLOY_BRANCH = 'gh-pages'   // Branch used for GitHub Pages
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pull the latest code from the main branch...'
                git branch: 'master', 
                    credentialsId: '2eb73cbb-e22a-4b4e-8f67-5610ca29580c', 
                    url: GITHUB_REPO
            }
        }

        stage('Test HTML') {
            steps {
                echo 'Validating HTML...'
                 if (isUnix()) {
                        sh '''
                        # Run HTML validation (optional, requires Node.js)
                        npm install -g htmlhint || true
                        htmlhint *.html || echo "HTML validation warnings"
                        '''
                    } else {
                        bat '''
                        # Run HTML validation (optional, requires Node.js)
                        npm install -g htmlhint || true
                        htmlhint *.html || echo "HTML validation warnings"
                        '''
                    }
            }
        }

        stage('Prepare for Deployment') {
            steps {
                echo 'Preparing files for deployment...'
             if (isUnix()) {
                    sh '''
                # Create a clean directory for deployment
                mkdir -p deploy
                cp -r * deploy/
                '''
                  } else {
 bat '''
                # Create a clean directory for deployment
                mkdir -p deploy
                cp -r * deploy/
                '''
             }
            }
        }

        stage('Deploy to GitHub Pages') {
            steps {
                echo 'Deploying to GitHub Pages...'
                withCredentials([usernamePassword(credentialsId: '2eb73cbb-e22a-4b4e-8f67-5610ca29580c', passwordVariable: 'GITHUB_PASS', usernameVariable: 'GITHUB_USER')]) {
                 if (isUnix()) {
                    sh '''
                    # Set up Git for deployment
                    cd deploy
                    git init
                    git config user.name "${GITHUB_USER}"
                    git config user.email "${GITHUB_USER}@users.noreply.github.com"
                    git add .
                    git commit -m "Automated deployment to GitHub Pages"
                    git push --force https://${GITHUB_USER}:${GITHUB_PASS}@${GITHUB_REPO#https://} HEAD:${DEPLOY_BRANCH}
                    '''
                      } else {
                     bat '''
                     # Set up Git for deployment
                    cd deploy
                    git init
                    git config user.name "${GITHUB_USER}"
                    git config user.email "${GITHUB_USER}@users.noreply.github.com"
                    git add .
                    git commit -m "Automated deployment to GitHub Pages"
                    git push --force https://${GITHUB_USER}:${GITHUB_PASS}@${GITHUB_REPO#https://} HEAD:${DEPLOY_BRANCH}
                    '''
                 }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment to GitHub Pages completed successfully!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
