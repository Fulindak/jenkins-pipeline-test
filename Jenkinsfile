pipeline {
    agent {
        label 'default'
    }

    parameters {
        string(name: 'STUDENT_NAME', defaultValue: 'dkuzmin', description: 'Имя студента')
    }

    environment {
        PYTHON_VENV = 'venv'
        MY_PASSWORD = credentials('MY_PASSWORD')
    }

    options {
        timeout(time: 5, unit: 'MINUTES')
    }

    triggers {
        pollSCM('H/5 * * * *') // Проверка изменений в репозитории каждые 5 минут
    }

    stages {
        stage('clone') {
            steps {
                checkout scm // Клонирование репозитория
            }
        }

        stage('setup environment') {
            steps {
                script {
                    if (fileExists(PYTHON_VENV)) {
                        sh "rm -rf ${PYTHON_VENV}"
                    }
                }
                //sh "python3 -m venv ${PYTHON_VENV}"
                //sh "source ${PYTHON_VENV}/bin/activate && pip install -r requirements.txt"
                sh "echo устанавливаем зависимости"
            }
        }
        
        stage('run') {
            steps {
                //sh 'source venv/bin/activate && python hello.py --name ${STUDENT_NAME} > result.txt'
                sh "echo результат сборки cтудента ${STUDENT_NAME}"
                sh "echo результат сборки cтудента ${STUDENT_NAME} > result.txt"
            }
        }
        
        stage('result') {
            steps {
                archiveArtifacts artifacts: 'result.txt', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
