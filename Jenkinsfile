pipeline {
  agent {
    docker {
      image 'localhost:5000/hpe/maven'
      args '-v /var/run/docker.sock:/var/run/docker.sock  -v maven_repo:/root/.m2'
    }
    
  }
  stages {
    stage('Init') {
      steps {
        sh '''#!/bin/bash
echo PATH = ${PATH}
mvn -version
docker -v
pom_groupid=`mvn org.apache.maven.plugins:maven-help-plugin:2.2:evaluate -Dexpression=project.groupId |grep -Ev '(^\\[|Download\\w+:)'`
pom_artifactid=`mvn org.apache.maven.plugins:maven-help-plugin:2.2:evaluate -Dexpression=project.artifactId |grep -Ev '(^\\[|Download\\w+:)'`
pom_version=`mvn org.apache.maven.plugins:maven-help-plugin:2.2:evaluate -Dexpression=project.version |grep -Ev '(^\\[|Download\\w+:)'`
echo "Maven Artifcat: ${pom_groupid}.${pom_artifactid}:${pom_version}"
docker-compose -p app down

docker rmi app
docker rmi  app_web

        '''
      }
    }
    stage('Build') {
      steps {
        sh '''#!/bin/bash
mvn clean package
        '''
      }
    }
    stage('Test') {
      steps {
        sh '''#!/bin/bash
mvn test
        '''
      }
    }
    stage('Publish') {
      steps {
        sh '''#!/bin/bash
docker images
        '''
      }
    }
    stage('Deployment') {
      steps {
        sh '''#!/bin/bash
echo "Start docker..."
docker-compose -p app up -d
echo "Succesful start services!"
        '''
      }
    }
  }
}
