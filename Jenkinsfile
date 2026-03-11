node {

    checkout scm

    // deploy env dev
    stage("Build"){
        docker.image('shippingdocker/php-composer:8.2').inside('-u root') {

            sh 'rm -f composer.lock'
            sh 'composer install'
        }
    }

    // Testing
    stage("Test"){
        docker.image('ubuntu').inside('-u root') {
            sh 'echo "Ini adalah test"'
        }
    }

}