// 14.4 C003: нічний безпековий прогін SQL_XSS-набору з гілки, звіт і перевірка, що база прибрана.
pipeline {
    agent any
    triggers { cron('H 2 * * *') }
    options { timeout(time: 15, unit: 'MINUTES') }
    environment { BASE_URL = 'http://host.docker.internal:8082' }
    stages {
        stage('SQL_XSS') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'course-admin', usernameVariable: 'ADMIN_USER', passwordVariable: 'ADMIN_PASS')]) {
                    sh 'newman run course-tests/b14-sqlxss.postman_collection.json -d course-tests/sqlxss-departments.csv --env-var baseUrl=$BASE_URL --env-var admin_username=$ADMIN_USER --env-var admin_password=$ADMIN_PASS -r cli,junit,htmlextra --reporter-junit-export reports/sqlxss.xml --reporter-htmlextra-export reports/sqlxss.html || true'
                }
            }
        }
        stage('Прибирання бази') {
            steps {
                sh 'newman run course-tests/b14-baseline-check.postman_collection.json --env-var baseUrl=$BASE_URL -r cli,junit --reporter-junit-export reports/baseline.xml || true'
            }
        }
    }
    post {
        always {
            junit 'reports/*.xml'
            publishHTML(target: [reportName: 'SQL_XSS HTML', reportDir: 'reports', reportFiles: 'sqlxss.html', keepAll: true, alwaysLinkToLastBuild: true, allowMissing: false])
        }
    }
}
