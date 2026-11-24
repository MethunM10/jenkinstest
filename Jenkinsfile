pipeline {
    agent { label 'secret-scan' }

    options {
        skipDefaultCheckout()
    }

    stages {
        stage('Checkout') {
            steps {
                deleteDir()
                checkout scm
            }
        }

        stage('Scan Git history') {
            steps {
                sh '''
                    set -eu

                    if [ "$(git rev-parse --is-shallow-repository)" != "false" ]; then
                        echo "Disable shallow clone so Gitleaks can scan the full history."
                        exit 1
                    fi

                    /usr/local/bin/gitleaks git --redact=100 --log-opts="HEAD" .
                '''
            }
        }
    }
}
