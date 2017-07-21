pipeline {
  options { 
    buildDiscarder(logRotator(numToKeepStr: '5')) 
  }
  
  agent{ label 'docker-compose' }
  environment {
    DOCKER_HUB_USER = 'beedemo'
    DOCKER_CREDENTIAL_ID = 'docker-hub-beedemo'
  }

  stages {
    stage('create build cache image') {
      when {
        branch 'maven-build-cache'
      }
      steps {
        buildMavenCacheImage("${DOCKER_HUB_USER}", "flyway-demo-mvn-cache", "${DOCKER_CREDENTIAL_ID}")
      }
    }
    stage('start database') {
      when {
        not {
          branch 'maven-build-cache'
        }
      }
      steps {
        sh 'docker-compose up -d db'
      }
    }
    stage('flyway init') {
      when {
        not {
          branch 'maven-build-cache'
        }
      }
      steps {
        sh 'docker-compose run --rm init-db'
        sh """docker-compose exec -T db env PGPASSWORD=password psql -h db -U postgres postgres -c \"\\\\d+ test_emp\""""
      }
    }
    stage('flyway update') {
      when {
        not {
          branch 'maven-build-cache'
        }
      }
      steps {
        sh 'docker-compose run --rm update-db'
        sh """docker-compose exec -T db env PGPASSWORD=password psql -h db -U postgres postgres -c \"\\\\d+ test_emp\""""
      }
    }
    stage('flyway alter column') {
      when {
        not {
          branch 'maven-build-cache'
        }
      }
      steps {
        sh 'docker-compose run --rm alter-db'
        sh """docker-compose exec -T db env PGPASSWORD=password psql -h db -U postgres postgres -c \"\\\\d+ test_emp\""""
      }
    }  
  }
  post {
    always {
      sh 'docker-compose down'     
    }
  } 
}
