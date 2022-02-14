pipeline {
    agent any
//     environment {
//             URK = credentials('jenkins-aws-secret-key-id')
//             AWS_SECRET_ACCESS_KEY = credentials('jenkins-aws-secret-access-key')
//         }
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
                sh "mvn package -D PORT=9636 -D JDBC_DATABASE_USERNAME=${params.USERNAME} -D JDBC_DATABASE_PASSWORD=${params.PASSWORD} -D JDBC_DATABASE_URL=${params.URL}"
                sh "ls -al /target"
                echo "------------------URA-----------------------"
            }
        }
    }
}
