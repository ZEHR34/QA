pipeline {
    agent any
    parameters {
            string(name: 'URL', description: 'DB_URL')

            string(name: 'USERNAME', description: 'DB_NAME')

            password(name: 'PASSWORD', description: 'DB_PASSWORD')
        }
    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!'
                sh "ls -al"
                sh "mvn package -D PORT=9636 -D JDBC_DATABASE_USERNAME=$URL -D JDBC_DATABASE_PASSWORD=${params.USERNAME} -D JDBC_DATABASE_URL=${URL}"
                sh "ls -al /target"
            }
        }
    }
}
