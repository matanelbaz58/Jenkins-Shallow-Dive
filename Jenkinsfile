pipeline{
    agent {label 'deb'}
        stages {
            stage('Pre-Build') {
                steps {
                    sh '''
                    sudo apt-get update && sudo apt-get install -y  wget \
                                                                    curl \
                                                                    python3 \
                                                                    python3-pip \
                                                                    pipx \
                                                                    python3-flask \
                                                                    pipenv  \
                                                                    pylint  \
                                                                    python3-poetry
                    pipx install pyinstaller
                    '''
                    }
                }
                stage('Linter') {
                    steps {
                            sh '''
                            pylint   --disable=missing-docstring,invalid-name app.py
                            '''
                        }
                    }
                stage('Test') {
    steps {
        sh '''
            python3 ./app.py > app.log 2>&1 &
            APP_PID=$!
            sleep 5

            if curl -f http://127.0.0.1:8000 > /dev/null 2>&1; then
                echo "Test SUCCESSFUL"
            else
                echo "Test FAILED"
                cat app.log
                kill $APP_PID || true
                exit 1
            fi

            kill $APP_PID || true
        '''
    }
}

                stage('Build'){
                    steps {
                    sh '''
                        chmod +x -R /home/jenkins/.local/pipx/venvs/pyinstaller
                        /home/jenkins/.local/pipx/venvs/pyinstaller -y  app.py
                    '''
                    }
                }
                stage('Archive'){
                            steps {
                    archiveArtifacts artifacts: 'dist', onlyIfSuccessful: true
                        }
                }
            }
        }
