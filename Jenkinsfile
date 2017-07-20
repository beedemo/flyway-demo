node('docker-compose') {
  properties([[$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5']]])
  stage('start database') {
    checkout scm
    sh 'docker-compose up -d db'
  }
  stage('flyway init') {
    checkout scm
    sh 'docker-compose run --rm init-db'
    sh """docker-compose exec -T db env PGPASSWORD=password psql -h db -U postgres postgres -c \"\\\\d+ test_emp\""""
  }
  stage('flyway update') {
    sh 'docker-compose run --rm update-db'
    sh """docker-compose exec -T db env PGPASSWORD=password psql -h db -U postgres postgres -c \"\\\\d+ test_emp\""""
  }
  stage('clean-up') {
    sh 'docker-compose down'     
  }   
}
