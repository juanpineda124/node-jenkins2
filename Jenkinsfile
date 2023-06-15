pipeline{
    agent any

    stages{
        stage('Clonar el repositorio'){
            steps{
                git branch: 'main', credentialsId: 'git-jenkins1', url: 'https://github.com/juanpineda124/node-jenkins2.git'
            }
        }
        stage('Contruir imagen Docker'){
            steps{
                script{
                    withCredentials([
                    string(credentialsId: 'MONGO_URI', variable: 'MONGO_URI' )
                    ]) {
                    docker.build('proyectos-backend-micro:v1', '--build-arg MONGO_URI=${MONGO_URI} .')
                    }
                 }
            }
        }
        stage('Desplegar contenedor Docker'){
            steps{
                script{
                    withCredentials([
                             string(credentialsId: 'MONGO_URI', variable: 'MONGO_URI' )
                        ]) {  
                            sh """
                                sed 's|\\${MONGO_URI}|${MONGO_URI}|g' docker-compose.yml > docker-compose-update.yml
                                docker-compose -f  docker-compose-update.yml up -d   
                         """
                        }
                }
            }
         }
    }
}