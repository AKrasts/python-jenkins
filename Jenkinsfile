pipeline {
    agent any

    stages {
        stage('Install pip deps') {
            steps {
                cleanWs(notFailBuild: true)
                script{
                    install_pip_dep()
                }
            }
        }

        stage('Deploy to DEV') {
            steps {
                script{
                    deploy("dev", 7001) 
                }
            }
        }

        stage('Tests on DEV') {
            steps {
                script{
                    test("dev")
                }
            }
        }

        stage('Deploy to STG') {
            steps {
                script{
                    deploy("stg", 7002) 
                }
            }
        }

        stage('Tests on STG') {
            steps {
                script{
                    test("stg")
                }
            }
        }

        stage('Deploy to PREPRD') {
            steps {
                script{
                    deploy("preprod", 7003)
                }
            }
        }

        stage('Tests on PREPRD') {
            steps {
                script{
                    test("preprod")
                }
            }
        }

        stage('Deploy to PRD') {
            steps {
                script{
                    deploy("prod", 7004) // 1030
                }
            }
        }

        stage('Tests on PRD') {
            steps {
                script{
                    test("prod")
                }
            }
        }
    }
}

def install_pip_dep(){
    echo 'Installation of pip dependencies is starting ...'
    bat "dir"
    bat "git clone https://github.com/AKrasts/python-greetings.git"
    bat "git clone https://github.com/AKrasts/course-js-api-framework.git"
    bat "dir"
    dir('python-greetings') {
        bat "pip install -r requirements.txt"
        bat "npm install -g pm2"
    }  
    dir('course-js-api-framework') {
        bat "npm install"
    }
}

def deploy(String enviroment, int port) {
    echo "Deployment to ${enviroment} has started ..."
    dir('python-greetings') {        
        bat "pm2 delete greetings-${enviroment} & EXIT /B 0"
        bat "pm2 start app.py --name greetings-\"${enviroment}\" -- --port ${port}"
    }  
}

def test(String enviroment) {
    echo "Testing on ${enviroment} has started ..."    
    dir('course-js-api-framework') {    
        bat "npm run greetings greetings_${enviroment}"
    }  
}

