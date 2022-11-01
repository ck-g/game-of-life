pipeline
{
	agent
	{
		label 
		{
			label 'Master'
			
		}
	}
	stages
	{
		stage('Docker-Cleanup')
		{
			steps
			{
				script
				{
					
					def exitCode = sh script: ' docker ps -a | grep -w mytomcat-8081', returnStatus: true
					if ( exitCode == 0 ) 
					{
						sh 'docker stop mytomcat-8081'
					}					
					def exitCode3 = sh script: ' docker images | grep -w mycentos:1.0', returnStatus: true
					if ( exitCode3== 0 ) 
					{
						sh 'docker rmi mycentos:1.0'
						sh 'docker system prune -a -f'
                    			}
					
				}
			}
		}
		stage('Git-Checkout')
		{
			steps
			{
				script
				{
					sh '''
						docker system prune -a -f
						git checkout -f master
						source /root/.bash_profile
            					mvn clean package 
						chmod 755 *
						docker build -t mycentos:1.0 .
						docker run -itdp 8088:8080 --name mytomcat-8081 mycentos:1.0
						chmod 755 gameoflife-web/target/gameoflife.war
						docker cp gameoflife-web/target/gameoflife.war mytomcat-8081:/usr/local/tomcat/webapps
						
						
					'''
				}
			}
		}
	}
}
