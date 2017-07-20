node('docker-compose') {
  stage('start database') {
    sh 'docker network create flyway'
    sh 'docker-compose up -d db'
  }
  stage('flyway init') {
    checkout scm
    sh 'docker-compose run --rm init-db'
    sh 'docker-compose exec -T db PGPASSWORD=password psql -h db -U postgres \d+ test_emp'
  }
  stage('flyway update') {
    checkout scm
    sh 'docker-compose run --rm update-db'
    sh 'docker-compose exec -T db PGPASSWORD=password psql -h db -U postgres \d+ test_emp'
  }
  stage('clean-up') {
    sh 'docker-compose down'        
  }   
}
