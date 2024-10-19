pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/vijay2181/jenkins-pr-merge-on-success.git'
        GITHUB_CREDENTIALS_ID = 'git-credentials' // Update with your GitHub credentials ID in Jenkins
    }

    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'action', value: '$.action'],
                [key: 'pr_id', value: '$.pull_request.id'],
                [key: 'pr_number', value: '$.pull_request.number'],
                [key: 'pr_state', value: '$.pull_request.state']
            ],
            causeString: 'Triggered on $action of Pull Request $pr_id',
            token: 'vijay-token', // Optional: if you use a secret token in the webhook.
            printContributedVariables: false,
            printPostContent: false,
            regexpFilterText: '$action',
            regexpFilterExpression: 'opened|reopened|synchronize'
        )
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: "${env.GITHUB_REPO}", branch: 'main'
                script {
                    // Ensure GIT_COMMIT is set correctly
                    env.GIT_COMMIT = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Add your testing scripts here
                    echo "Pull Request Number: ${env.pr_number}"
                    echo "Pull Request ID: ${env.pr_id}"
                    echo "Pull Request Action: ${env.action}"
                    echo "Pull Request State: ${env.pr_state}"
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
        success {
            script {
                // Post success status to GitHub
                setGitHubCommitStatus('success', 'All checks passed')
            }
        }
        failure {
            script {
                // Post failure status to GitHub
                setGitHubCommitStatus('failure', 'Build failed')
            }
        }
        always {
            echo "Pipeline for PR ${env.pr_id} completed."
        }
    }
}

// Function to set GitHub commit status
def setGitHubCommitStatus(String state, String description) {
    withCredentials([usernamePassword(credentialsId: env.GITHUB_CREDENTIALS_ID, usernameVariable: 'GITHUB_USER', passwordVariable: 'GITHUB_TOKEN')]) {
        def commitSha = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
        def context = 'Jenkins'

        def payload = [
            state: state,
            target_url: "${env.BUILD_URL}",
            description: description,
            context: context
        ]

        def payloadJson = groovy.json.JsonOutput.toJson(payload)

        sh """
            curl -s -X POST -H "Authorization: token ${GITHUB_TOKEN}" -H "Content-Type: application/json" -d '${payloadJson}' "https://api.github.com/repos/vijay2181/jenkins-pr-merge-on-success/statuses/${commitSha}"
        """
    }
}
