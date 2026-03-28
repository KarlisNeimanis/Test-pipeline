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
                    deployEnv("dev", "7001")
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
                    deployEnv("stg", "7002")
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
                    deployEnv("preprod", "7003")
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
                    deployEnv("prod", "7004")
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
}

def deployEnv(envName, port) {
    echo "Deploying application to %envName% environment on port %port%..."

    bat """
    rmdir /s /q python-greetings 2>nul
    git clone https://github.com/mtararujs/python-greetings.git
    cd python-greetings

    echo Stopping existing app if running...
    pm2 delete greetings-app-${envName} & exit /B 0

    echo Starting application...
    pm2 start app.py --name greetings-app-${envName} -- ${port}
    """
}

def testEnv(envName) {
    echo "Running tests on ${envName} environment..."

    bat ""
    rmdir /s /q course-js-api-framework 2>nul
    git clone https://github.com/mtararujs/course-js-api-framework.git
    cd course-js-api-framework

    echo Installing npm dependencies...
    npm install

    echo Running API tests...
    npm run greetings greetings_${envName}
    """
    }
}