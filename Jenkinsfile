pipeline{
    agent any
    tools{
        nodejs 'NodeJS19.8.1'
    }
    stages{
        stage('Clone repository'){
            steps{
                git 'https://github.com/WanjikuN/gallery.git'
            }
        }
        stage('Install dependancies'){
            steps{
                sh 'npm install'
            }
        }
        stage('Tests'){
            post{
                failure{
                  mail bcc: '', body: 'Jenkins Test Failure', cc: '', from: '', replyTo: '', subject: 'Test Failed', to: 'patricia.njoroge@student.moringaschool.com'  
                }
            }
            steps{
                sh 'npm test'
            }
        }
        stage('Deploy to Heroku'){
            steps{
               withCredentials([gitUsernamePassword(credentialsId: 'heroku', gitToolName: 'PASS')]) {
                sh 'git push https://${PASS}@git.heroku.com/gallerypat.git master'
                } 
            }
        }
        stage('Slack Notifications'){
            steps{
                slackSend channel: '#ip1-devops', color: '#008000', message: "Successful deployment of ${env.JOB_NAME} ,Build number ${env.BUILD_NUMBER} .  (<https://gallerypat.herokuapp.com/|Gallery url>)", teamDomain: 'patriciaip1', tokenCredentialId: 'slackIP'
            }
        }
    }
}
