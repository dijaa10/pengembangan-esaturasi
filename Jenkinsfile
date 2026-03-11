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
        docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
            sshagent(credentials: ['dj']) {
                sh 'mkdir -p /root/.ssh'
                sh 'ssh-keyscan -H 172.20.0.1 >> /root/.ssh/known_hosts'
                sh 'rsync -avz --delete ./ dj@172.20.0.1:/home/dj/prod.kelasdevops.xyz/'
            }
        }
    }

}