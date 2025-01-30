pipeline {
  agent any
  tools {
      maven 'M3'
  }

  environment {
      pom = readMavenPom(file: 'calculator/pom.xml')
      artifactId = pom.getArtifactId()
      version = pom.getVersion()
      name = pom.getName()
      groupId = pom.getGroupId()
  }

  stages {
    stage('Git Fetched and .m2 repo cleaned') {
      steps {
        sh 'ls -l'
        // sh 'rm -rf /root/.m2/repository/*'   // clean when there is issues
        // echo 'Repo /root/.m2/repository/ cleaned'
      }
    }

    stage('Environment details') {
      steps {
        dir('calculator') {
          script {
            echo "Version: ${version}"
            echo "Group ID: ${groupId}"
            echo "Artifact ID: ${artifactId}"
          }
        }
      }
    }

    stage('Build') {
      steps {
        dir('calculator') {
          sh 'cat src/main/java/com/echios/App.java '
          sh 'mvn clean install'
        }
      }
    }

    stage('SonarQube Analysis...') {
        steps {
            dir('calculator') {
          script {
            withSonarQubeEnv(credentialsId: 'sonar-scan') {
              sh 'mvn clean package sonar:sonar -Dsonar.coverage.exclusions=**/*'
            }
          }
            }
        }
    }

    stage('Send to Nexus snapshot rep') {
        steps {
            dir('calculator') {
              echo 'Nexus upload'
              nexusArtifactUploader(
                credentialsId: 'nexus-jenkins',
                groupId: "${groupId}",
                nexusUrl: 'fictional-goldfish-w44jv6p44p72569g-8081.app.github.dev',
                nexusVersion: 'nexus3',
                protocol: 'https',
                repository: 'echop',
                version: "${version}",
                artifacts: [
                    [
                        artifactId: "${artifactId}",
                        classifier: '',
                        file: "target/${artifactId}-${version}.jar",
                        type: 'jar'
                    ]
                ]
              )
            }
        }
    }

    stage('JiraUpdate') {
        steps {
            jiraComment body: "Sonar Passed & pushed to Nexus (Job: ${env.JOB_NAME}, Build: ${env.BUILD_NUMBER})", issueKey: 'RJNSA-5'
        }
    }

  }
}
