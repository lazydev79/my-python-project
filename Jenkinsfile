pipeline {
    agent any
    environment {
        IMAGE_NAME = "lazydev79/my-python-application"
        DOCKER_LOGIN = "lazydev79" // Initialisation de la variable
    }
   stages { 
   
    stage('Flake8 Linting') {
                    steps {
                        script {
                            try {
                                sh 'python3 --version'
                                sh 'python3 -m flake8 --version'
                                sh 'python3 -m flake8 . --count --show-source --statistics || true'
                            } catch (Exception e) {
                                echo "Flake8 a retourné des erreurs mais n'a pas bloqué le pipeline."
                            }
                        }
                    }
                }
        stage('Unit Tests (Pytest)') {
                    steps {
                        script {
                            try {
                                //sh 'pip install -r requirements.txt'
                                //sh 'pip install pytest'
                                sh 'pytest | tee report.txt'
                            } catch (Exception e) {
                                echo "Les tests unitaires ont échoué!"
                                currentBuild.result = 'UNSTABLE'  // Marque le build comme instable si les tests échouent
                            }
                        }
                    }
                    post {
                        always {
                            archiveArtifacts artifacts: 'report.txt', fingerprint: true
                        }
                    }
                }
    
 stage('Build & Push Docker Image') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'DOCKER_PASSWORD_SCOL', variable: 'DOCKER_PASSWORD_SCOL')]) {
                        sh """
                            docker build -t ${IMAGE_NAME}:${env.BUILD_VERSION} .
                            docker login -u $DOCKER_LOGIN -p $DOCKER_PASSWORD_SCOL
                            docker push ${IMAGE_NAME}:${env.BUILD_VERSION}
                        """
                    }
                }
            }
        }
        }
}
