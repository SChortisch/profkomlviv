// Пайплайн курсу «Тестування API» (блок 08): колекція з цієї гілки проти API курсу в курсовому Jenkins.
pipeline {
    agent any
    triggers {
        // приблизно раз на 2 хвилини Jenkins питає GitHub, чи з'явився в гілці новий коміт (розділ 8.7)
        pollSCM('H/2 * * * *')
    }
    parameters {
        choice(name: 'FOLDER', choices: ['Відділи', 'Команда', 'Новини'], description: 'Папка колекції для прогону')
    }
    environment {
        // у контейнері localhost — сам Jenkins; API курсу на ПК — через host.docker.internal
        BASE_URL = 'http://host.docker.internal:8082'
    }
    stages {
        stage('Newman') {
            steps {
                // params.FOLDER має значення вже в першому білді, а змінна оболонки $FOLDER — лише з другого
                sh "newman run course-tests/b06-main.postman_collection.json --folder '${params.FOLDER}' --env-var baseUrl=${BASE_URL} -r cli,junit --reporter-junit-export results.xml"
            }
        }
    }
    post {
        always {
            junit 'results.xml'
        }
    }
}
// Коміт для перевірки pollSCM (розділ 8.7).
