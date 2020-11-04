def builderDocker
def CommitHash

pipeline {

    agent any

    parameters {
        booleanParam(name: 'RUNTEST', defaultValue: true, description: 'Toggle this value from testing')
        choice(name: 'Deploy', choices: ['Deploy Stagging', 'Deploy Deployment', 'Deploy Fullstack'], description: 'Pick something')
    }

    stages {
        stage('Deploy on Deployment') {
            when {
                expression {
                    params.CICD == 'Deploy Deployment' || BRANCH_NAME == 'dev'
                }
            }
            steps {
                script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'ansible-server',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'cd ansible && ansible-playbook -i hosts deployment-frontend.yml',
                                        execTimeout: 120000,
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
        stage('Deploy on stagging') {
            when {
                expression {
                    params.CICD == 'Deploy Stagging' || BRANCH_NAME == 'stagging'
                }
            }
            steps {
                script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'ansible-server',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'cd ansible && ansible-playbook -i hosts stagging-frontend.yml',
                                        execTimeout: 120000,
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
        stage('Deploy on fullstack') {
            when {
                expression {
                    params.CICD == 'Deploy Fullstack' || BRANCH_NAME == 'main'
                }
            }
            steps {
                script {
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: 'ansible-server',
                                verbose: false,
                                transfers: [
                                    sshTransfer(
                                        execCommand: 'cd ansible && ansible-playbook -i hosts fullstack-frontend.yml',
                                        execTimeout: 120000,
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}