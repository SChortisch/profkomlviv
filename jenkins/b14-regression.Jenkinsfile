// 14.4 C002: регресія BUG-001 — список команди має читатися.
pipeline {
    agent any
    environment { BASE_URL = 'http://host.docker.internal:8082' }
    stages {
        stage('Регресія BUG-001') {
            steps {
                sh 'newman run course-tests/b14-team-health.postman_collection.json --env-var baseUrl=$BASE_URL -r cli,junit --reporter-junit-export reports/regression.xml'
            }
        }
    }
    post { always { junit 'reports/regression.xml' } }
}
