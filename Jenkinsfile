node {

    checkout scm

    stage("Build") {
        docker.image('composer:2.7').inside('--entrypoint="" -u root') {
            sh 'rm -f composer.lock'
            sh 'composer install --ignore-platform-reqs'
        }
    }

    stage("Test") {
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }

    stage('Deploy to Prod') {
    agent any
    steps {
        withDockerContainer(image: 'agung3wi/alpine-rsync:1.1') {
            sshagent(credentials: ['dj']) {
                sh 'mkdir -p /root/.ssh'
                sh 'ssh-keyscan -H 172.20.209.222 >> /root/.ssh/known_hosts'  // <-- ganti di sini
                sh 'rsync -avz --delete ./ dj@172.20.209.222:/path/to/prod/'  // <-- dan di sini
            }
        }
    }
}