pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/ismailtelhouni/PainCare_Frontend_Angular.git', branch: 'main'
            }
        }

        stage('Install Node.js') {
            steps {
                sh 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'docker run --rm -e SONAR_HOST_URL="http://localhost:9000" -e SONAR_LOGIN="sqa_7ad8a13ca728b4870391fee63c48c03b57211842" -v "$(pwd):/usr/src" sonarsource/sonar-scanner-cli'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}