utawebapp pipeline:
pipeline {
    agent any

    tools {
        // Install the Maven version configured as "maven" and add it to the path.
        maven "maven"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nicoleannhargrove/edu.uta.webapp.git']]])
                // Run Maven on a Unix agent.
                sh "mvn clean package"
            }

           
        }
        
        stage('Deploy') {
            steps {
                echo 'in deploy'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible Control Node', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'sudo ansible-playbook --limit ansible_CNU /opt/docker2/webapp.yaml -vvv;', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/opt/docker2', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
            }

           
        }
    }
}
