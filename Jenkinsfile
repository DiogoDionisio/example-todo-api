node('php'){
    stage('Clean'){
        deleteDir()
        sh 'ls -la'
    }
    //sempre vai pegar na branch corrente. resolve o problema de nao ter que ficar editando o jenkinsfile e trocando a branch ou puxando da master.
    stage('Fetch') {
        checkout scm
    }
    
    stage('Build'){
        sh 'composer install --prefer-dist --no-dev --ignore-platform-reqs'
        sh 'php artisan config:cache'
        // sh 'php artisan route:cache'
    }
    
    stage('Docker Build') {
        sh 'docker build -t dmdionisio/todoapi:$BUILD_NUMBER .'
    }
    
    stage('config') {
        parallel(
            'config cache': {
                sh 'php artisan config:cache'
            },
            'config route': {
                sh 'php artisan'
            }
        )
    }
    
    stage('Docker Ship') {
        sh 'docker push dmdionisio/todoapi:$BUILD_NUMBER'
    }
}
