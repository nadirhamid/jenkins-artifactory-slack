pipeline {
    agent any
    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/nadirhamid/jenkins-artifactory-slack"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: "http://artifactory:8081/artifactory",
                    //credentialsId: CREDENTIALS
                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'mvn3', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean package',
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }

        stage ('Notify slack') {
            steps {
                rtPublishBuildInfo (
                        slackSend channel: '#artifactory-channel'
                          message: 'Image was pushed to artifactory!'
            }
        }
    }
}