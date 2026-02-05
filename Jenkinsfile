pipeline {
    agent any

    stages {

        stage('Set Container & Port') {
            steps {
                script {
                    if (env.BRANCH_NAME == "2026Q1") {
                        env.CONTAINER = "q1-httpd"
                        env.PORT = "80"
                    }
                    else if (env.BRANCH_NAME == "2026Q2") {
                        env.CONTAINER = "q2-httpd"
                        env.PORT = "90"
                    }
                    else if (env.BRANCH_NAME == "2026Q3") {
                        env.CONTAINER = "q3-httpd"
                        env.PORT = "8080"
                    }
                    else {
                        error "Unsupported branch: ${env.BRANCH_NAME}"
                    }
                }
            }
        }

        stage('Clone') {
            steps {
                git branch: "${env.BRANCH_NAME}",
                url: 'https://github.com/Rohitbavadekar/Rohit.App.git'
            }
        }

        stage('Pull Image') {
            steps {
                sh 'docker pull httpd:latest'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f $CONTAINER || true
                docker run -d --name $CONTAINER -p $PORT:80 httpd
                '''
            }
        }

        stage('Deploy Page') {
            steps {
                sh '''
                docker exec $CONTAINER bash -c "
                echo 'This is ${BRANCH_NAME}' > /usr/local/apache2/htdocs/index.html
                "
                '''
            }
        }
    }
}
