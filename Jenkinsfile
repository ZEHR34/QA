def remote = [:]
remote.name = "node-1"
remote.host = "192.168.1.109"
remote.port = 2225
remote.allowAnyHosts = true

node {

        environment {
            DB_URL = credentials('db_url_192.168.1.109')
            PASSWORD = credentials('db_password_for192.168.1.109_1234')
            USERNAME = credentials('DB_uername_entropia')
        }
        stage('package') {
            echo 'Hello world!'
            sh "ls -al"
            sh "mvn package -D PORT=9636 -D JDBC_DATABASE_USERNAME=$USERNAME -D JDBC_DATABASE_PASSWORD=$PASSWORD -D JDBC_DATABASE_URL=$DB_URL"
            sh "ls -al target/"
            echo "------------------URA-----------------------"
        }


    withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key-for-app-server', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
        remote.user = userName
        remote.identityFile = identity
        stage("SSH Steps Rocks!") {
            writeFile file: 'abc.sh', text: 'ls'
        }
    }
}