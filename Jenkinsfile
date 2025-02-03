pipeline {
    agent any
    tools {
        maven "M3"
    }

    environment {
        pom = readMavenPom(file: 'calculator/pom.xml')
        artifactId = pom.getArtifactId()
        version = pom.getVersion()
        name = pom.getName()
        groupId = pom.getGroupId()
    }

    stages {
        stage('git check') {
            steps {
                sh "ls -l"
                sh 'rm -rf /root/.m2/repository/*'   
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

    stage('Send to Nexus snapshot repo') {
        steps {
            dir('calculator') {
              echo 'Nexus upload'
              nexusArtifactUploader(
                credentialsId: 'nexus-jenkins',
                groupId: "${groupId}",
                nexusUrl: 'obscure-parakeet-9xx46vrxqwwfg45-8081.app.github.dev',
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

    }
}
