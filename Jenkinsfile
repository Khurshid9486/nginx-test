pipeline {
    agent any
    stages {

           stage ("Set up") {
               steps {
                    sh """
                    sudo pip3 install molecule
                    sudo pip3 install docker
                """
               }//steps
           }//stage
            stage ("starting deamon and enable") {
               steps {
                   sh """
                   sudo systemctl start docker
                   sudo systemctl enable docker
                   sudo systemctl restart docker
                   """
           
            } //steps
        } //stage


        stage("Create docker image for testing") {
            steps {
                sh """
                    python3 -m molecule create
                """
            } //steps
        } //stage
        stage("Apply Ansible role to docker image") {
            steps {
                sh """
                    python3 -m molecule converge
                """
            } //steps
        } //stage
        stage("Check idempotency") {
            steps {
                sh """
                    python3 -m molecule idempotence
                """
            } //steps
        } //stage
        stage("Cleanup molecule") {
            steps {
                sh """
                    python3 -m molecule cleanup
                """
            } //steps
        } //stage
        stage("Destroy molecule instance") {
            steps {
                sh """
                    python3 -m molecule destroy
                """
            } //steps
        } //stage
    } //stages
    post {
        always {
            sh """
                sudo pip3 uninstall docker -y
                sudo pip3 uninstall molecule -y
            """
        }
    }
} //pipeline
