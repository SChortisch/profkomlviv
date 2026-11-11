// 14.3 C004: крок addShortText з плагіна badge — працює лише в образі, перезібраному з цим плагіном.
pipeline {
    agent any
    environment { BASE_URL = 'http://host.docker.internal:8082' }
    stages {
        stage('Smoke') {
            steps {
                sh 'newman run course-tests/b06-main.postman_collection.json --env-var baseUrl=$BASE_URL -r cli,junit --reporter-junit-export reports/smoke.xml'
            }
        }
    }
    post {
        always {
            junit 'reports/smoke.xml'
            addShortText(text: "smoke: ${currentBuild.currentResult}", background: 'lightyellow', border: 1)
        }
    }
}
