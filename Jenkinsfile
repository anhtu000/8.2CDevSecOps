pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {
        stage('Stage 1: Build') {
            steps {
                echo 'Task: Compile and package the application code.'
                echo 'Tool: Maven'
            }
        }

        stage('Stage 2: Unit and Integration Tests') {
            steps {
                echo 'Task: Run unit tests and integration tests to verify application behaviour.'
                echo 'Tools: JUnit, Selenium'
            }
        }

        stage('Stage 3: Code Analysis') {
            steps {
                echo 'Task: Analyse code quality, bugs, code smells, and maintainability issues.'
                echo 'Tool: SonarQube / SonarCloud'
            }
        }

        stage('Stage 4: Security Scan') {
            steps {
                echo 'Task: Scan the code and dependencies for known security vulnerabilities.'
                echo 'Tool: OWASP Dependency-Check / npm audit'
            }
        }

        stage('Stage 5: Deploy to Staging') {
            steps {
                echo 'Task: Deploy the application to a staging environment.'
                echo 'Tool: AWS EC2'
            }
        }

        stage('Stage 6: Integration Tests on Staging') {
            steps {
                echo 'Task: Run integration tests in the staging environment.'
                echo 'Tool: Postman / Selenium'
            }
        }

        stage('Stage 7: Deploy to Production') {
            steps {
                echo 'Task: Deploy the verified application to production.'
                echo 'Tool: AWS EC2 / Docker'
            }
        }
    }
}
