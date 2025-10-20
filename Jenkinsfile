pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Realizando checkout del repositorio...'
                checkout scm
            }
        }

        stage('Instalar Newman') {
            steps {
                echo 'Instalando Newman globalmente...'
                bat 'npm install -g newman newman-reporter-htmlextra'
            }
        }

        stage('Ejecutar Tests con Newman') {
            steps {
                echo 'Ejecutando colección de Postman...'
                bat '''
                    newman run Onboarding.postman_collection.json ^
                    -e local3478.postman_environment.json ^
                    -d data.csv ^
                    --folder "Ejercicio8" ^
                    -r cli,htmlextra,junit ^
                    --reporter-htmlextra-export reporte.html ^
                    --reporter-junit-export reporte.xml
                '''
            }
        }

        stage('Publicar Reportes') {
            steps {
                echo 'Publicando reportes...'
                junit '**/reporte.xml'
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: '.',
                    reportFiles: 'reporte.html',
                    reportName: 'Reporte HTML Newman'
                ])
            }
        }
    }

    post {
        always {
            echo '✅ Pipeline finalizado. Revisa los reportes en Jenkins.'
        }
    }
}
