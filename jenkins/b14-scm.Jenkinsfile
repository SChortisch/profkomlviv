// 14.2 C010, 14.4 C001: Jenkinsfile з гілки course/b14; pollSCM сам запускає білд після push.
pipeline {
    agent any
    triggers { pollSCM('H/2 * * * *') }
    environment { BASE_URL = 'http://host.docker.internal:8082' }
    stages {
        stage('Smoke') {
            steps {
                sh 'git log -1 --format="Коміт: %h %s"'
                sh 'newman run course-tests/b06-main.postman_collection.json --env-var baseUrl=$BASE_URL -r cli,junit --reporter-junit-export reports/smoke.xml'
            }
        }
    }
    post { always { junit 'reports/smoke.xml' } }
}
