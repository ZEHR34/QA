def remote = [:]
remote.name = "node-1"
remote.host = "192.168.1.109"
remote.port = 2225
remote.allowAnyHosts = true

node {
    withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key-for-app-server', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
        remote.user = userName
        remote.identityFile = identity
        stage("SSH Steps Rocks!") {
            writeFile file: 'abc.sh', text: 'ls'
        }
    }
}