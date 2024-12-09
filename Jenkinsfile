pipeline {
    agent any

    environment {
        GITHUB_USER = 'shivajigree'    // Replace with your GitHub username
        GITHUB_REPO = 'https://github.com/shivajigree/PersonalPage.git'
        GITHUB_TOKEN = credentials('DevOps-Jenkins') // Store your GitHub PAT in Jenkins credentials
        DEPLOY_BRANCH = 'gh-pages'              // Branch used for GitHub Pages
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Pull the latest code from the main branch...'
                git branch: 'main', url: GITHUB_REPO
            }
        }

        stage('Test HTML') {
            steps {
                echo 'Validating HTML...'
                sh '''
                # Run HTML validation (optional, requires Node.js)
                npm install -g htmlhint || true
                htmlhint *.html || echo "HTML validation warnings"
                '''
            }
        }

        stage('Prepare for Deployment') {
            steps {
                echo 'Preparing files for deployment...'
                sh '''
                # Create a clean directory for deployment
                mkdir -p deploy
                cp -r * deploy/
                '''
            }
        }

        stage('Deploy to GitHub Pages') {
            steps {
                echo 'Deploying to GitHub Pages...'
                sh '''
                # Set up Git for deployment
                cd deploy
                git init
                git config user.name "${GITHUB_USER}"
                git config user.email "${GITHUB_USER}@users.noreply.github.com"
                git add .
                git commit -m "Automated deployment to GitHub Pages"
                git push --force https://${GITHUB_USER}:${GITHUB_TOKEN}@${GITHUB_REPO#https://} HEAD:${DEPLOY_BRANCH}
                '''
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
