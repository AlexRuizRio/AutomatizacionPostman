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
                echo 'Instalando Newman localmente...'
                bat 'npm install newman newman-reporter-htmlextra'
            }
        }

        stage('Ejecutar Tests con Newman') {
            steps {
                echo 'Ejecutando colección de Postman...'
                bat '''
                node node_modules\\newman\\bin\\newman.js run Onboarding.postman_collection.json ^
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
                junit '**/reporte.xml'
                publishHTML([
                    allowMissing: false,
                    reportDir: '.',
                    reportFiles: 'reporte.html',
                    reportName: 'Reporte HTML Newman',
                    keepAll: true,
                    alwaysLinkToLastBuild: true
                ])
            }
        }
    }

    post {
        always {
            echo 'Pipeline finalizado. Revisa los reportes en Jenkins.'
        }
    }
}
