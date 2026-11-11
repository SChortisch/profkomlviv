// 14.3 C004: крок addBadge з плагіна badge — працює лише в образі, перезібраному з цим плагіном.
// У badge 3.x кроку addShortText уже немає; форму addBadge підказав Snippet Generator.
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
            addBadge(text: "smoke: ${currentBuild.currentResult}", style: 'background: lightyellow; border: 1px solid')
        }
    }
}
