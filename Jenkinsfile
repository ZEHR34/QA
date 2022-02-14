pipeline {
    agent any
    environment {
        DB_URL = credentials('db_url_192.168.1.109')
        PASSWORD = credentials('db_password_for192.168.1.109_1234')
        USERNAMEI = credentials('DB_uername_entropia')
    }
    stages {
        stage('Example Username/Password') {
            steps {
                echo 'Hello world!'
                sh "ls -al"
                sh "mvn package -D PORT=9636 -D JDBC_DATABASE_USERNAME=${USERNAMEI} -D JDBC_DATABASE_PASSWORD=${PASSWORD} -D JDBC_DATABASE_URL=${DB_URL}"
                sh "ls -al target/"
                echo "------------------URA-----------------------"
            }
        }
    }
}

def remote = [:]
remote.name = "node-1"
remote.host = "192.168.1.109"
remote.port = 2225
remote.allowAnyHosts = true
node {
    withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key-for-app-server', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
        remote.user = userName
//         remote.identityFile = identity
        remote.password = '1234'
        stage("deploy") {
            sshPut remote: remote, from: 'target/qa-0.0.1-SNAPSHOT.war', into: '/home/ubuntu/qa.war'
        }
        stage("run app") {
            sshCommand remote: remote, command: '''
                                                    if ssh -i /home/id_rsa -p 2225 ubuntu@192.168.1.109 "pkill java"
                                                    then echo 1
                                                    else echo 2
                                                    fi
                                                    ssh -i /home/id_rsa -p 2225 ubuntu@192.168.1.109 "nohup java -jar qa.war --JDBC_DATABASE_URL=$URL --JDBC_DATABASE_USERNAME=$NAME --JDBC_DATABASE_PASSWORD=$PASSWORD" > /dev/null &
                                                '''
        }
    }
}


// pipeline {
//     agent any
//     environment {
//         DB_URL = credentials('db_url_192.168.1.109')
//         PASSWORD = credentials('db_password_for192.168.1.109_1234')
//         USERNAMEI = credentials('DB_uername_entropia')
//     }
//     stages {
//         stage('Example Username/Password') {
//             steps {
//                 echo 'Hello world!'
//                 sh '''
//                     if ssh -i /home/id_rsa -p 2225 ubuntu@192.168.1.109 "pkill java"
//                     then echo 1
//                     else echo 2
//                     fi
//                     ssh -i /home/id_rsa -p 2225 ubuntu@192.168.1.109 "nohup java -jar qa.war --JDBC_DATABASE_URL=$URL --JDBC_DATABASE_USERNAME=$NAME --JDBC_DATABASE_PASSWORD=$PASSWORD" > /dev/null &
//                 '''
//                 sh "mvn package -D PORT=9636 -D JDBC_DATABASE_USERNAME=${USERNAMEI} -D JDBC_DATABASE_PASSWORD=${PASSWORD} -D JDBC_DATABASE_URL=${DB_URL}"
//                 sh "ls -al target/"
//                 echo "------------------URA-----------------------"
//             }
//         }
//     }
// }