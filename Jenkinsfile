pipeline {
    agent any
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
    }

}
