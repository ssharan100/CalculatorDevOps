pipeline {
    // The “agent” section configures on which nodes the pipeline can be run.
    // Specifying “agent any” means that Jenkins will run the job on any of the
    // available nodes.
// 	environment {
// 	    registryCredential = 'dockerhub'
// 	    imagename = "1651078/calculator_dev_ops"
// 	}

	agent any

    stages {
        stage('Git Pull') {
            steps {
                // Get code from a GitHub repository
                // Make sure to add your own git url and credentialsId
				git url: 'https://github.com/ssharan100/CalculatorDevOps.git',
				branch: 'master',
                credentialsId: 'github'
            }
        }
        stage('Maven Build') {
            steps {
                // Maven build, 'sh' specifies it is a shell command
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Images') {
            steps {
                sh 'docker build -t 1651078/calculator_dev_ops:latest .'
            }
        }
        stage('Publish Docker Images') {
            steps {
                withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
                    sh 'docker push 1651078/calculator_dev_ops:latest'
                }
            }
        }
        stage('Clean Docker Images') {
            steps {
                sh 'docker rmi -f 1651078/calculator_dev_ops:latest'
            }
        }
        stage('Deploy and Run Image'){
            steps {
                ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'p2.yml', sudoUser: null
            }
        }
        // stage('Attach docker container') {
        //     steps {

        //     }
        // }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}