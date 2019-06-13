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
        
        stage('Staging')
        {
            when
            {
                branch 'master'
            }
            
            steps
            {
                echo 'Running Staging Automation'
                withCredentials([usernamePassword(credentialsID: 'webserver_login',
                                                  usernameVariable: 'USERNAME',
                                                  passwordVariable: 'USERPASS')])
                {
                    sshPublisher(failOnError: true, 
                                 continueOnError: false,
                                 publishers: [sshPublisherDesc(configName: 'staging',
                                                               sshCredentials: [username: "$USERNAME",
                                                                                encryptedPassphrase: "$USERPASS"],
                                                               transfers: [sshTransfer(sourceFiles: 'dist/trainSchedule.zip',
                                                                                       removePrefix: 'dist/',
                                                                                       remoteDirectory: '/tmp',
                                                                                       excCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                                                                      )]
                                                              )]
                                )
                }
            }
        }
    }
}
