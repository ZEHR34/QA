pipeline {
    agent any
    environment {
            DB_URL = credentials('db_url_192.168.1.109')
            PASSWORD = credentials('db_password_for192.168.1.109_1234')
            USERNAME = credentials('DB_uername_entropia')
        }
//     parameters {
//             string(name: 'URL', description: 'DB_URL')
//
//             string(name: 'USERNAME', description: 'DB_NAME')
//
//             password(name: 'PASSWORD', description: 'DB_PASSWORD')
//         }
    stages {
        stage('package') {
            steps {
                echo 'Hello world!'
                sh "ls -al"
                sh "mvn package -D PORT=9636 -D JDBC_DATABASE_USERNAME=$USERNAME -D JDBC_DATABASE_PASSWORD=$PASSWORD -D JDBC_DATABASE_URL=$DB_URL"
                sh "ls -al target/"
                echo "------------------URA-----------------------"
            }
        }
        node {
            def remote = [:]
            remote.allowAnyHosts = true
            remote.name = "node-1"
            remote.host = "192.168.1.109"
            remote.port = "2225"
            withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key-for-app-server', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
                remote.user = userName
                remote.identityFile = identity
                stage("SSH Steps Rocks!") {
                    sshPut remote: remote, from: 'target/qa-0.0.1-SNAPSHOT.war', into: '.'
                }
            }
        }
    }
}
