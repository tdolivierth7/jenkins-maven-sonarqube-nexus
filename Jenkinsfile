pipeline {
  agent any // This specifies that the pipeline can run on any available agent

  environment {
    // <<<--- Allow reflective access needed by Sonar
    MAVEN_OPTS = "--add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.lang.reflect=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED" // <<<--- ADDED
  }

  stages {
    stage('Validate Project') {
        steps {
            sh 'mvn validate'
        }
    }
    stage('Unit Test'){
        steps {
            sh 'mvn test'
        }
    }
    stage('App Packaging | Build'){
        steps {
            sh 'mvn package'
        }
    }
    stage('Integration Test'){
        steps {
            sh 'mvn verify -DskipUnitTests'
        }
    }
    stage ('Checkstyle Code Analysis'){
        steps {
            sh 'mvn checkstyle:checkstyle'
        }
    }
    stage('SonarQube Inspection') {
        steps {
            sh  """mvn sonar:sonar \
                    -Dsonar.projectKey=java-webapp-project \
                    -Dsonar.host.url=http://172.31.84.142:9000 \
                    -Dsonar.login=6c3c372a060f5b4840b14be78fa4bfd94bcd2821"""
        }
    } 
    stage("Upload Artifact To Nexus"){
        steps{
             sh 'mvn deploy'
        } 
        post {
            success {
              echo 'Successfully Uploaded Artifact to Nexus Artifactory'
        }
      }
    }
  }
}
