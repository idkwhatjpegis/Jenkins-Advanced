pipeline 

{

    agent 

    {

        label 'docker-node'

    }
    environment 

    {

        DOCKERHUB_CREDENTIALS=credentials('docker-hub')

    }



    stages 

    {

        stage('Clone') 

        {

            steps 

            {

                git branch: 'main', url: 'http://192.168.99.102:3000/semerdzhiev/jenkins-advanced'

            }

        }
        stage('Build ')

        {

            steps

            {
                
                sh 'docker compose up -d'

            }

        }
        
        stage('Test')

        {

            steps

            {

                script 

                {

                    echo 'Test #1 - reachability'

                    sh 'echo $(curl --write-out "%{http_code}" --silent --output /dev/null http://192.168.99.102:8080) | grep 200'


                }

            }

        }
        
        stage('Login') 

        {

            steps 

            {

                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'

            }

        }

        stage('Push') 

        {

            steps 

            {

                sh 'docker image tag homework-5-2-web semerdzhiev/jenkins-advanced'
                sh 'docker image tag homework-5-2-db semerdzhiev/jenkins-advanced'
                sh 'docker push semerdzhiev/jenkins-advanced'

            }

        }


    

        stage('Clean')

        {

            steps

            {

                sh 'docker container rm --force homework-5-db-1 homework-5-2-web-1 homework-5-2-db-1'

                sh 'docker network rm homework-5-2_app-network'

            }

        }

            stage('Deploy')

        {

                steps

            {

                sh 'docker container rm -f jenkins-advanced || true'

                sh 'docker container run -d -p 80:80 --name homework-5-2-web semerdzhiev/jenkins-advanced'

                sh 'docker container run -d -p 3306:3306 --name homework-5-2-db semerdzhiev/jenkins-advanced'

            }

        }
    }
    post 

    { 

        always 

        { 

            cleanWs()

        }

    }

}