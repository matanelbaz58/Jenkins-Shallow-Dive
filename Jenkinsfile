pipeline{
    agent {label 'workers'}
        stages {
            stage('Build') {
                steps {
                    echo 'Building..'
                    sleep 5
                    }
                }
                stage('Test') {
                    steps {
                            echo 'Testing..'
                                sleep 5

                        }
                    }
                stage('Deploy') {
                    steps {
                            echo 'Deploying....'
                                        sleep 5

                        }
                    }
            }
}