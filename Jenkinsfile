pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }
        //stage 3: publish artifacts

        stage{
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/com.vinaysdevopslab-0.0.12-SNAPSHOT.war', type: 'war']], credentialsId: '687c73af-242d-4f23-808d-935b04e7cfb9', groupId: 'com.vinaysdevopslab', nexusUrl: '172.20.10.79', nexusVersion: 'nexus3', protocol: 'http', repository: 'DfDevopsLab-snapshot', version: '0.0.12-SNAPSHOT'
            }
        }


        // Stage3 : Publish the source code to Sonarqube
        stage ('deploy'){
            steps {
                echo ' deploying......'
                // withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
                //      sh 'mvn sonar:sonar'
                // }

            }
        }

        
        
    }

}