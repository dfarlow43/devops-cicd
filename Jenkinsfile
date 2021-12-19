pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    }
    environment{
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupID = readMavenPom().getGroupId()

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

        stage ('Publish to nexus') {
            steps{
                nexusArtifactUploader artifacts: 
                [[artifactId: ${ArtifactId}, classifier: '', 
                file: 'target/'+ ${Name} + '-'+ ${Version}, type: 'war']], 
                credentialsId: '687c73af-242d-4f23-808d-935b04e7cfb9', 
                groupId: ${GroupID} , 
                nexusUrl: '172.20.10.79:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'DfDevopsLab-snapshot', 
                version: ${Version}
            }
        }

        stage ('Print Environment Variables'){
            steps{
                echo "ArtifactId: '${ArtifactId}'"
                echo "Version: '${Version}'"
                echo "GroupID: '${GroupID}'"
                echo "Name: '${Name}'"
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