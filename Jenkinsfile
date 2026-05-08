pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    environment {
        SCANNER_VERSION = '8.0.1.6346'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/anhtu000/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    echo "Installing Node.js dependencies..."
                    npm install
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    echo "Running tests..."
                    npm test || true
                '''
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh '''
                    echo "Generating coverage report..."
                    npm run coverage || true

                    echo "Checking coverage report..."
                    ls -la coverage || true
                '''
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh '''
                    echo "Running npm audit security scan..."
                    npm audit || true
                '''
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        set +x

                        echo "Checking SONAR_TOKEN credential..."
                        if [ -n "$SONAR_TOKEN" ]; then
                            echo "SONAR_TOKEN is available in the pipeline."
                        else
                            echo "SONAR_TOKEN is missing."
                            exit 1
                        fi

                        echo "Preparing SonarScanner CLI..."

                        if [ ! -f "sonar-scanner/bin/sonar-scanner" ]; then
                            echo "SonarScanner not found. Downloading now..."

                            rm -rf sonar-scanner
                            rm -rf sonar-scanner-${SCANNER_VERSION}-linux-x64
                            rm -f sonar-scanner-cli-${SCANNER_VERSION}-linux-x64.zip

                            curl -L -o sonar-scanner-cli-${SCANNER_VERSION}-linux-x64.zip \
                            https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SCANNER_VERSION}-linux-x64.zip

                            unzip -q sonar-scanner-cli-${SCANNER_VERSION}-linux-x64.zip
                            mv sonar-scanner-${SCANNER_VERSION}-linux-x64 sonar-scanner
                        else
                            echo "SonarScanner already exists. Skipping download."
                        fi

                        echo "Running SonarCloud analysis..."
                        ./sonar-scanner/bin/sonar-scanner \
                        -Dsonar.token=$SONAR_TOKEN
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed. Check Jenkins console output and SonarCloud dashboard."
        }
    }
}
