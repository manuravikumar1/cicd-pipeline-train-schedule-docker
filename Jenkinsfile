pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage ('Build Docker Image'){
            when{
                branch 'master'
            }
            steps{
                script{
                app = docker.build("manu1/train-schedule")
                app.inside{
                    sh 'echo $(curl localhost:8080)'
                }
            }
        }
    }
    stage('Push Docker Image') {
        when{
            branch 'master'
        }
        steps{
            script{
               docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
    withCredentials([usernamePassword(credentialsId: 'docker_hub_login', usernameVariable: 'manuravi', passwordVariable: 'Galvatron@123')]) {
        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
        app.push("${env.BUILD_NUMBER}")
        app.push("latest")
    }
}
                }
            }
        }
    }
}
}

