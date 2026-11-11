// 14.3 C002: пайплайн з гілки course/b14 бере CSV з гілки course/data і публікує звіт.
pipeline {
    agent any
    environment { BASE_URL = 'http://host.docker.internal:8082' }
    stages {
        stage('CSV з course/data') {
            steps {
                sh 'git fetch --depth=1 origin +refs/heads/course/data:refs/remotes/origin/course/data'
                sh 'git show origin/course/data:course-tests/team-ids.csv > team-ids.csv && cat team-ids.csv'
            }
        }
        stage('DDT') {
            steps {
                sh 'newman run course-tests/team-by-id.postman_collection.json -d team-ids.csv --env-var baseUrl=$BASE_URL -r cli,junit,htmlextra --reporter-junit-export reports/ddt.xml --reporter-htmlextra-export reports/ddt.html || true'
            }
        }
    }
    post {
        always {
            junit 'reports/ddt.xml'
            publishHTML(target: [reportName: 'DDT HTML', reportDir: 'reports', reportFiles: 'ddt.html', keepAll: true, alwaysLinkToLastBuild: true, allowMissing: false])
        }
    }
}
