pipeline {
    agent { label 'JDK_17' }
    triggers { pollSCM ('* * * * *') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal')
    }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/Nagababudevopsjun28/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage('package') {
            tools {
                jdk 'JDK_17'
            }
            steps {
                sh "mvn ${params.MAVEN_GOAL}"
            }
        }
        stage('sonar analysis') {
            environment {
                SCANNER_HOME = tool 'sonar-scanner'
            }

              steps {
                  withSonarQubeEnv('SONAR_CLOUD') {
                       sh '${SCANNER_HOME**}**}/bin/sonar-scanner mvn clean package sonar:sonar -Dsonar.projectkey=spring786 -Dsonar.organization=spring786 -Dsonar.token=f53559ab41622c1375e45d72cdb431b6aaa9b2e3'
                }
            }
        }
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.1.0-SNAPSHOT.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}

 