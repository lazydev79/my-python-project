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
                 }
}
