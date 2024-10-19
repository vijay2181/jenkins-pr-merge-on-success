pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/vijay2181/jenkins-pr-merge-on-success.git'
    }

    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'action', value: '$.action'],
                [key: 'pr_id', value: '$.pull_request.id'],
                [key: 'pr_state', value: '$.pull_request.state']
            ],
            causeString: 'Triggered on $action of Pull Request $pr_id',
            token: 'your-webhook-token', // Optional: if you use a secret token in the webhook
            printContributedVariables: true,
            printPostContent: true,
            regexpFilterText: '$action',
            regexpFilterExpression: 'opened|reopened|synchronize'
        )
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: "${env.GITHUB_REPO}", branch: 'main'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Add your testing scripts here
                    echo "Running tests for Pull Request ${env.pr_id}"
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Add your build scripts here
                    echo "Building project for Pull Request ${env.pr_id}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Add your deployment scripts here
                    echo "Deploying project for Pull Request ${env.pr_id}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline for PR ${env.pr_id} completed"
        }
    }
}
