pipeline
{
    agent any
    stages
    {
        stage('Build')
        {
            steps
            {
                echo 'Running Build Automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        
        stage('Deploy to Staging')
        {
            when
            {
                branch 'master'
            }
            
            steps
            {
                echo 'Deploying to Staging'
                withCredentials([usernamePassword(credentialsId: 'webserver_login',
                                                   usernameVariable: 'USERNAME',
                                                   passwordVariable: 'USERPASS')])
                {
                    sshPublisher(failOnError: true,
                                 continueOnError: false)
                }
            }
        }
    }
}
