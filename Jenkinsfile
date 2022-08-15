pipeline{
    agent any

    parameters{
        string(name: 'ansible_server', defaultValue: '172.31.42.119', description: 'docker Server')
    }

    stages{
        stage('build'){
            steps{
                sh 'mvn clean install package'
            }
        }
        stage('copy war file and dockerfile'){
            steps{
                sh "scp /var/lib/jenkins/workspace/hello_private/webapp/target/*.war jenkins@${params.ansible_server}:/home/ansadmin"
                sh "scp Dockerfile jenkins@${params.ansible_server}:/home/ansadmin"
            }
        }
        stage('run ansible-playbook'){
            steps{
                sh """ssh 172.31.42.119 << EOF
                ansible-playbook pushimage.yaml --connection=local
                exit
                EOF"""

            }
        }
    }

    }
