pipeline {
    agent any
    tools{
        maven 'maven-3.9.6'
    }
    stages {
        stage('Pull code from github') {
            steps {
                git changelog: false, poll: false, url: 'https://github.com/Shekhawat123/hello-world.git'
            }
        }
        
        stage('Build the application') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true clean install"
            }
        }
        
        stage('send artifact to ansible server') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: 'webapp/target/*war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        
        stage('Remove old images') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker rmi -f sankalpshekhawat/reg-app:v1 reg-app:v1', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        
        stage('Build and push image to docker hub') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''cd /opt/docker;
                ansible-playbook -i /opt/docker/hosts create-image.yml;''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        
        stage('Run the reg-app container') {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''sleep 5;
                ansible-playbook -i /opt/docker/hosts /opt/docker/run-container.yml''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}
