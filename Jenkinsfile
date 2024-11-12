pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/saraamas/PainCare_Frontend_Angular.git', branch: 'main'
            }
        }
          stage('Install Node.js') {
            steps {
                nodejs('NodeJs') {
                    bat 'npm install'             
                }
            }
        }
        stage('Build Angular') {
            steps {
                script {
                    nodejs('NodeJs') {
                      bat 'npm run build --prod'
                    }
                    
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                     def scannerHome = tool 'SonarScanner';
                    withSonarQubeEnv('SonarQube') {
                      bat """
                          ${scannerHome}\\bin\\sonar-scanner ^
                          -Dsonar.projectKey=PAINCARE_FRONT ^
                          -Dsonar.host.url=http://localhost:9000 ^
                          -Dsonar.login=sqp_104e5844eff7f637d4decd7c6bbc96eb9b8ddc10 ^
                          -Dsonar.sources=src ^
                          -Dsonar.exclusions="**/node_modules/**"
                      """
                          }
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
    post {
        always {
            echo "Analyse terminée, vérifiez SonarQube pour les résultats."
        }

        failure {
            script {
                emailext(
                    subject: "Pipeline Failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                    body: """<p>Bonjour,</p>
                             <p>Le pipeline <strong>${env.JOB_NAME}</strong> a échoué à l'étape de Quality Gate lors de l'exécution de la build numéro <strong>${env.BUILD_NUMBER}</strong>.</p>
                             <p>Statut de la Quality Gate: <strong style="color: red;">${currentBuild.result}</strong></p>
                             <p>Vérifiez les détails de la build ici : <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                             <p>Cordialement,</p>
                             <p>Votre serveur Jenkins</p>""",
                    to: 'saramasmoudi2001@gmail.com', // Remplacez par les adresses souhaitées
                    from:"chakra.hs.business@gmail.com",
                    replyTo:"chakra.hs.business@gmail.com",
                    mimeType: 'text/html'
                )
            }
        }

        success {
            script {
                emailext(
                    subject: "Pipeline Succeeded: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                    body: """<p>Bonjour,</p>
                             <p>Le pipeline <strong>${env.JOB_NAME}</strong> s'est terminé avec succès à l'étape de Quality Gate lors de l'exécution de la build numéro <strong>${env.BUILD_NUMBER}</strong>.</p>
                             <p>Statut de la Quality Gate: <strong style="color: green;">${currentBuild.result}</strong></p>
                             <p>Vérifiez les détails de la build ici : <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                             <p>Cordialement,</p>
                             <p>Votre serveur Jenkins</p>""",
                    to: 'saramasmoudi2001@gmail.com', // Remplacez par les adresses souhaitées
                    from:"chakra.hs.business@gmail.com",
                    replyTo:"chakra.hs.business@gmail.com",
                    mimeType: 'text/html'
                )
            }
        }
    }
}
