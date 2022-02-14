pipeline {
    agent any
    environment {
        DB_URL = credentials('db_url_192.168.1.109')
        PASSWORD = credentials('db_password_for192.168.1.109_1234')
        USERNAMEI = credentials('DB_uername_entropia')
        SSHKEY = credentials('ssh_privat_file')
    }
    stages {
        stage('build / test') {
            steps {
                echo 'Hello world!'
                sh "ls -al"
//                 sh "mvn package -D PORT=9636 -D JDBC_DATABASE_USERNAME=${USERNAMEI} -D JDBC_DATABASE_PASSWORD=${PASSWORD} -D JDBC_DATABASE_URL=${DB_URL}"
                sh "ls -al target/"
                sh ''' echo "
                       if pkill java
                       then echo 1
                       else echo 2
                       fi
                       java -jar qa.war --JDBC_DATABASE_URL=${DB_URL} --JDBC_DATABASE_USERNAME=${USERNAMEI} --JDBC_DATABASE_PASSWORD=${PASSWORD} > log
                       " > start.sh  '''
                echo "------------------URA-----------------------"
            }
        }
        stage("deploy"){
            steps{
                sh "scp -P 2225 -i /home/id_rsa target/qa-0.0.1-SNAPSHOT.war ubuntu@192.168.1.109:qa.war"
            }
        }
        stage('run') {
            steps {
                sh "echo ${SSHKEY}"
                sh '''
                    if ssh -i /home/id_rsa -p 2225 ubuntu@192.168.1.109 "pkill java"
                    then echo 1
                    else echo 2
                    fi
                    ssh -i /home/id_rsa -p 2225 ubuntu@192.168.1.109 "nohup java -jar qa.war --JDBC_DATABASE_URL=$URL --JDBC_DATABASE_USERNAME=$NAME --JDBC_DATABASE_PASSWORD=$PASSWORD" > /dev/null &
                '''
//                 sh "mvn package -D PORT=9636 -D JDBC_DATABASE_USERNAME=${USERNAMEI} -D JDBC_DATABASE_PASSWORD=${PASSWORD} -D JDBC_DATABASE_URL=${DB_URL}"
//                 sh "ls -al target/"
//                 echo "------------------URA-----------------------"
            }
        }
    }
}

// def remote = [:]
// remote.name = "node-1"
// remote.host = "192.168.1.109"
// remote.port = 2225
// remote.allowAnyHosts = true
// node {
//     withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key-for-app-server', keyFileVariable: 'identity', passphraseVariable: 'pass', usernameVariable: 'userName')]) {
//         remote.user = userName
//         echo "---------------000--------------"
//         echo identity
//         echo "---------------000--------------"
//         echo userName
//         echo "---------------000--------------"
//         echo pass
//         echo "---------------000--------------"
// //         remote.identity = identity
//         remote.password = pass
//         stage("deploy") {
//             sshPut remote: remote, from: 'target/qa-0.0.1-SNAPSHOT.war', into: '/home/ubuntu/qa.war'
//             sshPut remote: remote, from: 'start.sh', into: '/home/ubuntu/'
//
//         }
//         environment {
//             DB_URL = credentials('db_url_192.168.1.109')
//             PASSWORD = credentials('db_password_for192.168.1.109_1234')
//             USERNAMEI = credentials('DB_uername_entropia')
//         }
//         stage("run app") {
//             sshCommand remote: remote, command: "bash start.sh >&- 2>&- <&- & " // $USERNAMEI $PASSWORD > test.txt"
//         }
//     }
// }
//
//
// // pipeline {
// //     agent any
// //     environment {
// //         DB_URL = credentials('db_url_192.168.1.109')
// //         PASSWORD = credentials('db_password_for192.168.1.109_1234')
// //         USERNAMEI = credentials('DB_uername_entropia')
// //     }
// //     stages {
// //         stage('Example Username/Password') {
// //             steps {
// //                 echo 'Hello world!'
// //                 sh '''
// //                     if ssh -i /home/id_rsa -p 2225 ubuntu@192.168.1.109 "pkill java"
// //                     then echo 1
// //                     else echo 2
// //                     fi
// //                     ssh -i /home/id_rsa -p 2225 ubuntu@192.168.1.109 "nohup java -jar qa.war --JDBC_DATABASE_URL=$URL --JDBC_DATABASE_USERNAME=$NAME --JDBC_DATABASE_PASSWORD=$PASSWORD" > /dev/null &
// //                 '''
// //                 sh "mvn package -D PORT=9636 -D JDBC_DATABASE_USERNAME=${USERNAMEI} -D JDBC_DATABASE_PASSWORD=${PASSWORD} -D JDBC_DATABASE_URL=${DB_URL}"
// //                 sh "ls -al target/"
// //                 echo "------------------URA-----------------------"
// //             }
// //         }
// //     }
// // }