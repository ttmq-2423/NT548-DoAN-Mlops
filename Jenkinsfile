pipeline {
    agent { docker { image 'python:3.9' } }

    stages {
        stage('build model serving') {
<<<<<<< HEAD
            when {changeset "model_serving/**" }

            steps {
                echo 'Building model serving..'
                sh 'cd model_serving && make build_image'
=======
            when {changeset "model_deployment/**" }

            steps {
                echo 'Building model serving..'
                sh 'make build_image'
>>>>>>> main
            }
        }

        stage('test model serving') {
<<<<<<< HEAD
            when {changeset "model_serving/**" }

            steps {
                echo 'Testing model serving..' // (1)
            }
        }

        stage('deploy model serving') {
            parallel { // (2)
                stage('batch serving pipeline') {
                    when {changeset "model_serving/**" }

                    steps {
                        sh 'cd model_serving && make deploy_dags'
                    }
                }

                stage('online serving API') {
                    when {changeset "model_serving/**" }

                    steps {
                        sh 'cd model_serving && make compose_up'
                    }
=======
            when {changeset "model_deployment/**" }

            steps {
                echo 'Testing model serving..' # (1)
            }
        }

        parallel { # (2)
            stage('deploy serving pipeline') {
                when {changeset "model_deployment/**" }

                steps {
                    sh 'make deploy_dags'
                }
            }

            stage('deploy online serving API') {
                // when {changeset "model_serving/**" }

                steps {
                    sh 'make compose_up'
>>>>>>> main
                }
            }
        }
    }
}