node {

    def PROD_HOST = "host.docker.internal"
    def COMPOSER_IMAGE = "composer:2.7"

    stage("Checkout Source Code") {
        checkout scm
    }

    // Build Laravel Dependencies
    stage("Build") {
        docker.image(COMPOSER_IMAGE).inside('-u root --entrypoint=""') {
            sh '''
                rm -f composer.lock
                composer install --ignore-platform-reqs \
                --prefer-dist \
                --no-interaction \
                --no-progress
            '''
        }
    }

    // Testing
    stage("Test") {
        docker.image('php:8.2-cli').inside('-u root') {
            sh '''
                php -v
                echo "Menjalankan test sederhana"
            '''
        }
    }

    // Deploy ke Production
    stage("Deploy to Production") {
        docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
            sshagent(credentials: ['ssh-prod']) {
                sh '''
                    mkdir -p ~/.ssh
                    ssh-keyscan -H ${PROD_HOST} >> ~/.ssh/known_hosts

                    rsync -rav --delete ./ dj@${PROD_HOST}:/home/dj/prod.kelasdevops.xyz/ \
                    --exclude=.env \
                    --exclude=storage \
                    --exclude=.git \
                    --exclude=node_modules
                '''
            }
        }
    }

}