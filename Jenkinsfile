pipeline {
    agent any

    stages {
        stage('Clonando Repositório Git') {
            steps {
                git branch: 'main', url: 'https://github.com/mautrip/jk-public-gh.git'
            }
        }
        stage('Construindo Imagem') {
            steps {
                sh 'docker build -t webapp:${BUILD_NUMBER} .'
            }
        }
        stage('Implantando Aplicação') {
            steps {
                script {
                    // Verifica se o container existe e está rodando
                    def containerExists = sh(script: "docker ps -q -f name=webapp_ctr", returnStdout: true).trim()
                    if (containerExists) {
                        // Para o container existente
                        sh 'docker stop webapp_ctr'
                       }
                }
                sh 'docker run --rm -d -p 3000:3000 --name webapp_ctr webapp:${BUILD_NUMBER}'
            }
        }
    }
}
