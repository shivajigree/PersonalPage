pipeline {
    agent any

    environment {
        GITHUB_USER = 'shivajigree' // Replace with your GitHub username
        GITHUB_REPO = 'https://github.com/shivajigree/PersonalPage.git'
        DEPLOY_BRANCH = 'gh-pages' // Branch used for GitHub Pages
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pulling the latest code from the main branch...'
                git branch: 'master',
                    credentialsId: '2eb73cbb-e22a-4b4e-8f67-5610ca29580c',
                    url: GITHUB_REPO
            }
        }

        stage('Validate HTML') {
            steps {
                echo 'Validating HTML files...'
                script {
                    if (isUnix()) {
                        sh '''
                        # Ensure HTMLHint is installed
                        npm install -g htmlhint || true
                        # Validate all HTML files
                        htmlhint *.html || echo "HTML validation completed with warnings."
                        '''
                    } else {
                        bat '''
                        npm install -g htmlhint || true
                        htmlhint *.html || echo "HTML validation completed with warnings."
                        '''
                    }
                }
            }
        }

        stage('Prepare for Deployment') {
            steps {
                echo 'Preparing files for deployment...'
                script {
                    if (isUnix()) {
                        sh '''
                        # Create a clean deployment directory
                        rm -rf deploy
                        mkdir -p deploy
                        cp -r * deploy/
                        '''
                    } else {
                        bat '''
                        rmdir /s /q deploy
                        mkdir deploy
                        xcopy * deploy\\ /s /e /q /y
                        '''
                    }
                }
            }
        }

        stage('Deploy to GitHub Pages') {
            steps {
                echo 'Deploying to GitHub Pages...'
                withCredentials([usernamePassword(
                    credentialsId: '2eb73cbb-e22a-4b4e-8f67-5610ca29580c', 
                    usernameVariable: 'GITHUB_USER', 
                    passwordVariable: 'GITHUB_PASS'
                )]) {
                    script {
                        if (isUnix()) {
                            sh '''
                            # Deploy files to GitHub Pages
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
    }

    post {
        success {
            echo 'Deployment to GitHub Pages completed successfully!'
        }
        failure {
            echo 'Deployment failed. Check the logs for details.'
        }
    }
}
