pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/vijay2181/jenkins-pr-merge-on-success'
        CONTEXT = 'ci/jenkins/build-status'
        GITHUB_API_URL = 'https://api.github.com/repos/vijay2181/jenkins-pr-merge-on-success'
    }

    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'action', value: '$.action'],
                [key: 'pr_id', value: '$.pull_request.id'],
                [key: 'pr_number', value: '$.pull_request.number'],
                [key: 'pr_state', value: '$.pull_request.state'],
                [key: 'pr_branch', value: '$.pull_request.head.ref']
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
        stage('Checkout') {
            steps {
                script {
                    // Ensure branch name is set correctly
                    def branchName = env.pr_branch
                    git url: "${REPO_URL}", branch: branchName
                    env.GIT_COMMIT = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
                }
            }
        }

        stage('Initialize') {
            steps {
                script {
                    // Set the initial commit status to pending
                    setBuildStatus("pending", "Build started")
                }
            }
        }

        stage('Build Steps') {
            steps {
                script {
                    // Add your testing scripts here
                    echo "Pull Request Number: ${env.pr_number}"
                    echo "Pull Request ID: ${env.pr_id}"
                    echo "Pull Request Action: ${env.action}"
                    echo "Pull Request State: ${env.pr_state}"
                    echo "Pull Request Branch: ${env.pr_branch}"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo "Testing..."
                    boolean testsPassed = true // Replace with actual test result
                    if (!testsPassed) {
                        setBuildStatus("failure", "Tests failed")
                        error("Tests failed")
                    }
                }
            }
        }
    }

    post {
        success {
            script {
                setBuildStatus("success", "Build and tests succeeded")
            }
        }
        failure {
            script {
                setBuildStatus("failure", "Build or tests failed")
            }
        }
    }
}

// Define the function to set the build status
void setBuildStatus(String state, String description) {
    withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
        sh """
            curl -X POST \
            -H "Authorization: token $GITHUB_TOKEN" \
            -H "Content-Type: application/json" \
            -d '{
                "state": "${state}",
                "description": "${description}",
                "context": "${CONTEXT}"
            }' \
            ${GITHUB_API_URL}/statuses/${env.GIT_COMMIT}
        """
    }
}
