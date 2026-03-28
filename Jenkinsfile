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

//
// 🔧 FUNKCIJAS
//

def installDeps() {
    echo "Installing all required dependencies..."

    sh """
    rm -rf python-greetings
    git clone https://github.com/mtararujs/python-greetings.git
    cd python-greetings
    ls -la

    python3 -m venv venv
    ./venv/bin/python -m pip install -r requirements.txt
    """
}

def deployEnv(envName, port) {
    echo "Deploying application to ${envName} environment on port ${port}..."

    sh """
    rm -rf python-greetings
    git clone https://github.com/mtararujs/python-greetings.git
    cd python-greetings

    echo "Stopping existing app if running..."
    pm2 delete greetings-app-${envName} || true

    echo "Starting application..."
    pm2 start app.py --name greetings-app-${envName} -- ${port}
    """
}

def testEnv(envName) {
    echo "Running tests on ${envName} environment..."

    sh """
    rm -rf course-js-api-framework
    git clone https://github.com/mtararujs/course-js-api-framework.git
    cd course-js-api-framework

    echo "Installing npm dependencies..."
    npm install

    echo "Running API tests..."
    npm run greetings greetings_${envName}
    """
}