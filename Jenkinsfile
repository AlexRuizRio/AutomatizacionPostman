pipeline {
    agent any

    environment {
        REPORT_DIR = "reports"
        REPORT_HTML = "${REPORT_DIR}\\reporte.html"
        REPORT_XML = "${REPORT_DIR}\\reporte.xml"
    }

    stages {

        stage('Preparar Workspace') {
            steps {
                echo 'Preparando carpeta de reportes...'
                bat 'if not exist reports mkdir reports'
            }
        }

        stage('Instalar Newman Local') {
            steps {
                echo 'Instalando Newman localmente en el workspace...'
                bat 'npm install newman newman-reporter-htmlextra'
            }
        }

        stage('Ejecutar Collection') {
            steps {
                echo 'Ejecutando Collection de Postman...'
                bat """node node_modules\\newman\\bin\\newman.js run Onboarding.postman_collection.json \\
                    -e local3478.postman_environment.json \\
                    -d data.csv \\
                    --folder "Ejercicio8" \\
                    -r cli,htmlextra,junit \\
                    --reporter-htmlextra-export "${REPORT_HTML}" \\
                    --reporter-junit-export "${REPORT_XML}" """
            }
        }

        stage('Publicar Reportes') {
            steps {
                echo 'Publicando reportes HTML y JUnit...'
                script {
                    if (fileExists(REPORT_XML)) {
                        junit REPORT_XML
                    } else {
                        echo "No se encontró el reporte JUnit, saltando junit."
                    }

                    if (fileExists(REPORT_HTML)) {
                        publishHTML([
                            allowMissing: false,
                            reportDir: REPORT_DIR,
                            reportFiles: 'reporte.html',
                            reportName: 'Reporte HTML Newman',
                            keepAll: true,
                            alwaysLinkToLastBuild: true
                        ])
                    } else {
                        echo "No se encontró el reporte HTML, algo falló en Newman."
                    }
                }
            }
        }

    }

    post {
        always {
            echo 'Pipeline finalizado. Revisa los reportes en Jenkins.'
        }
    }
}
