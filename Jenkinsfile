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
                script{

                    def NexusRepo = Version.endsWith("SNAPSHOT") ? "DfDevopsLab-snapshot": "DFDevopsLab-Release"
                    nexusArtifactUploader artifacts: 
                    [[artifactId: "${ArtifactId}", classifier: '', 
                    file: 'target/'+ "${Name}" + '-'+ "${Version}.war", type: 'war']], 
                    credentialsId: '687c73af-242d-4f23-808d-935b04e7cfb9', 
                    groupId: "${GroupID}" , 
                    nexusUrl: '172.20.10.79:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: "${NexusRepo}", 
                    version: "${Version}"
                }
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
                sshPublisher(publishers: [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: [sshTransfer(
                        cleanRemote: false, 
                        execCommand: 'ansible-playbook -i /opt/playbooks/hosts /opt/playbooks/download_deploy_artifact.yml', 
                        execTimeout: 120000, 
                      )], 
                        usePromotionTimestamp: false, 
                        useWorkspaceInPromotion: false, 
                        verbose: false)])

            }
        }

        
        
    }

}