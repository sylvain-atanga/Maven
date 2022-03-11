pipeline 
{
    agent any
    stages 
    {
        stage('ContinuousDownload_loans') 
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
        stage('ContinuousBuild_loans') 
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
  }
}
