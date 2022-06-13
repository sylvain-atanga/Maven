pipeline 
{
    agent any
    stages 
    {
        stage('ContinuousDownload_master') 
        {
            steps 
            {
                script
                {
                    try
                    {
                        git 'https://github.com/sylvain-atanga/Maven.git'    
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins failed to download code from github repository', cc: '', from: '', replyTo: '', subject: 'Download failed', to: 'git.team@hbc.org'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousBuild_master') 
        {
            steps 
            {
                script
                {
                    try
                    {
                        sh 'mvn package'    
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins failed to build artifact from code', cc: '', from: '', replyTo: '', subject: 'Build failed', to: 'dev.team@hbc.org'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousDeployment_master') 
        {
            steps 
            {
                script
                {
                    try
                    {
                        deploy adapters: [tomcat9(credentialsId: 'cfadbe0d-44a4-46f4-9cbe-a5af5c3446c4', path: '', url: 'http://172.31.27.194:8080')], contextPath: 'QA-app', war: '**/*war'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins failed to deploy artifact to QA environment', cc: '', from: '', replyTo: '', subject: 'Build failed', to: 'midleware.team@hbc.org'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousTesting_master') 
        {
            steps 
            {
                script
                {
                    try
                    {
                        git 'https://github.com/sylvain-atanga/FunctionalTesting.git'
                        sh 'java -jar /var/lib/jenkins/workspace/DeclarativePipeline/testing.jar'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins shows fail after executing selenium scripts', cc: '', from: '', replyTo: '', subject: 'testing failed', to: 'testing.team@hbc.org'
                        exit(1)
                    }
                }
            }
        }
        stage('ContinuousDelivery_master') 
        {
            steps 
            {
                script
                {
                    try
                    {
                        deploy adapters: [tomcat9(credentialsId: 'cfadbe0d-44a4-46f4-9cbe-a5af5c3446c4', path: '', url: 'http://172.31.26.111:8080')], contextPath: 'prodapp war: '**/*war'
                    }
                    catch(Exception e1)
                    {
                        mail bcc: '', body: 'Jenkins fail to deliver app to production environment', cc: '', from: '', replyTo: '', subject: 'delivery failed', to: 'delivery.team@hbc.org'
                        exit(1)
                    }
                }
            }
        }
    }
}
