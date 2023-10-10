pipeline {
    agent any
    
    stages {
        stage("Clone Code") {
            steps {
                echo "cloning code..."
                git url:"https://github.com/vihankapil/django-notes-app.git", branch: "main"
            }
        }
        stage('Build') {
            steps {
                echo "Building image..."
                sh "docker build -t my-note-app ."
            }
        }
        stage ("push to docker hub"){
            steps {
                echo "Pushing image to docker hub..."
                script {
                    def registry = 'docker.io'
                    def username = 'kapilvihan'
                    def password = 'Siwaya@123'

                    def expectScript = """
                    spawn docker login $registry
                    expect \"Username:\"
                    send \"$username\\r\"
                    expect \"Password:\"
                    send \"$password\\r\"
                    expect eof
                    """
                    sh("expect -c '$expectScript'")
                    sh "docker tag my-note-app ${username}/my-note.app:latest"
                    sh "docker push ${username}/my-note.app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying container..."
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    }
