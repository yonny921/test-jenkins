pipeline {
  agent any
    
 parameters {
        string(name: 'name_container', defaultValue: 'asertec-qa')
        string(name: 'name_imagen', defaultValue: 'img-asertec-qa')
        string(name: 'tag_imagen', defaultValue: 'latest')
        string(name: 'puerto_imagen', defaultValue: '3000')
    }
    environment {
        name_final = '${name_container}${tag_imagen}'
    }
    stages {
          stage('stop/rm') {
            when {
                expression {
                    DOCKER_EXIST = sh(returnStdout: true, script: 'echo "$(docker ps -q --filter name=${name_final})"')
                    return DOCKER_EXIST != ''
                }
            }
            steps {
                script{
                    sh '''
                        docker stop ${name_final}
                    '''
                }
            }
        }

        stage('build') {
            steps {
                script{
                    sh '''
                        docker build . -t ${name_imagen}:${tag_imagen}
                    '''
                    }
                }

        }
        stage('run') {
            steps {
                script{
                    sh '''
                        docker run -dp ${puerto_imagen}:80 --name ${name_final} ${name_imagen}:${tag_imagen}
                    '''
                }
            }

        }        
    
    }
}
