pipeline {
    agent any
   
    stages {

       stage('Run Selenium Tests with pytest') {
    steps {
        echo "Running Selenium Tests using pytest"

        script {
            def pythonExe = 'C:\\Users\\Tanzila\\AppData\\Local\\Programs\\Python\\Python310\\python.exe'
            bat "\"${pythonExe}\" -m pip install -r requirements.txt"
        }

        bat 'start /B python app.py'
        bat 'ping 127.0.0.1 -n 5 > nul'
        bat 'python -m pytest -v'
    }
}
        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat "docker build -t seleniumdemoapp:v1 ."
            }
        }
        stage('Docker Login') {
            steps {
                  bat 'docker login -u tanzila123 -p Tan@1234'
                }
            }
        stage('push Docker Image to Docker Hub') {
            steps {
                echo "push Docker Image to Docker Hub"
                bat "docker tag seleniumdemoapp:v1 tanzila123/sample2026:seleniumtestimage"               
                    
                bat "docker push tanzila123/sample2026:seleniumtestimage"
                
            }
        }
        stage('Deploy to Kubernetes') { 
            steps { 
                    // apply deployment & service 
                    bat 'kubectl apply -f deployment.yaml --validate=false' 
                    bat 'kubectl apply -f service.yaml' 
            } 
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
