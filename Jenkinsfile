node {

    checkout scm

    // deploy env dev
    stage("Build"){
        docker.image('composer:2.7').inside('-u root') {

            sh 'rm -f composer.lock'
            sh 'composer install --ignore-platform-reqs'
        }
    }

    // Testing
    stage("Test"){
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }

}