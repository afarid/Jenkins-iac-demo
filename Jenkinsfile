import groovy.json.*

pipeline {
    agent {
        label 'docker'
    }
    options {
         ansiColor('xterm')
    }
    environment {
        SLACK_URL = 'https://hooks.slack.com/services/SDKJSDKS/SDSDJSDK/SDKJSDKDS23434SDSDLCMLC'
        SLACK_NOTIFICATION_CHANNEL = 'random'
        JENKINS_SERVER_URL = 'http://host.docker.internal:8080/'
        TERRAFROM_VERSION = '0.11.10'
        SLACK_HOOK = credentials("slack_hook")
        AWS_CREDENTIALS = credentials("AWS_CREDENTIALS")
        AWS_ACCESS_KEY_ID = "${env.AWS_CREDENTIALS_USR}"
        AWS_SECRET_ACCESS_KEY= "${env.AWS_CREDENTIALS_PSW}"
    }
    stages {
        stage('Checkout code') {
            steps {
                checkout scm
            }
        }
        stage('init_and_plan') {
            steps {
                sh """
                env
                cd terraform
                terraform init && terraform plan -out=tfplan
                """
            }
        }

        stage("approve") {
            steps {
                sh "env"
                notifySlack("Do you approve deployment? ${BUILD_URL}console", "${env.SLACK_NOTIFICATION_CHANNEL}", [])
                input 'Do you approve deployment?'
            }
        }

         stage("apply") {
             steps {
                 sh """
                 cd terraform
                 terraform apply "tfplan"
                 """
                 notifySlack("Deployment Done", "${env.SLACK_NOTIFICATION_CHANNEL}", [])
             }
         }
    }
    post {
        always {
            cleanWs()
        }
    }
}

def notifySlack(text, channel, attachments) {
    def payload = JsonOutput.toJson([text: text,
        channel: channel,
        username: "Jenkins",
        attachments: attachments
    ])
    sh "curl -X POST --data-urlencode \'payload=${payload}\' ${env.SLACK_HOOK}"
}