node('docker-cloud') {
  stage('start database) {
    sh 'docker network create flyway'
    sh 'docker run --name flyway-postgres -e POSTGRES_PASSWORD=password -d postgres'
  }
  stage('flyway init') {
    checkout scm
    sh 'docker-compose run --rm init-db'
    
  }
}
