pipeline {
    agent any

    stages {

        stage('install-pip-deps') {
            steps {
                script {
                    installDeps()
                }
            }
        }

        stage('deploy-to-dev') {
            steps {
                script {
                    deployEnv("dev", 7001)
                }
            }
        }

        stage('tests-on-dev') {
            steps {
                script {
                    testEnv("dev")
                }
            }
        }

        stage('deploy-to-stg') {
            steps {
                script {
                    deployEnv("stg", 7002)
                }
            }
        }

        stage('tests-on-stg') {
            steps {
                script {
                    testEnv("stg")
                }
            }
        }

        stage('deploy-to-preprod') {
            steps {
                script {
                    deployEnv("preprod", 7003)
                }
            }
        }

        stage('tests-on-preprod') {
            steps {
                script {
                    testEnv("preprod")
                }
            }
        }

        stage('deploy-to-prod') {
            steps {
                script {
                    deployEnv("prod", 7004)
                }
            }
        }

        stage('tests-on-prod') {
            steps {
                script {
                    testEnv("prod")
                }
            }
        }
    }
}

def installDeps() {
    echo "Installing all required dependencies..."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    bat "dir"
    bat "python3 -m venv venv"
    bat "venv\\Scripts\\activate"
    bat "pip install -r requirements.txt"
}

def deployEnv(String environment, int port) {
    echo "Deploy"
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings.git'
    bat "pm2 delete greetings-app-${environment} & EXIT /B 0)"
    bat "pm2 start app.py --name python-greetings --interpreter ./venv/bin/python -- --port ${port}"

}

def testEnv(String environment, int port) {

}