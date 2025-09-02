@Library("shared") _
pipeline {
    agent {label 'majdoor'}

    stages {
        
        stage('hello')
        {
            steps {
                script{
                    hello()
                }
            }
        }
        stage('code') {
            steps {
               script{
                   clone2("https://github.com/pythonic06/django-notes-app.git", "main")
               }
            }
        }
        stage('build') {
            steps {
                echo 'this is for building code'
                bat 'docker build -t notes_app:latest .'
            }
        }
         stage('docker push') {
            steps {
               withCredentials([usernamePassword(credentialsId: 'dockerHubCreds', usernameVariable: 'dockerhubUser', passwordVariable: 'dockerhubPass')]) {
    bat ' docker login -u %dockerhubUser% -p%dockerhubPass%'
    bat 'docker image tag notes_app:latest %dockerhubUser%/notes_app:latest'
    bat 'docker push %dockerhubUser%/notes_app:latest'
                }
                echo 'image successfully push to docker hub'
                
            }
        }
         stage('deploy') {
            steps {
                echo 'this is for deploying code'
                bat 'docker compose up -d'
                
            }
        }
    }
}
