pipeline {
    agent any
    
    environment {
        
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Arijit094/10-MicroService-Appliction-CiCd-pipeline.git'
            }
        }
        stage('sonarQube') {
            steps {
                withSonarQubeEnv('sonar') {
                     sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=10-tier -Dsonar.projectName=10-tier -Dsonar.java.binaries=. '''
                }

            }
        }
        stage('adservice') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-new', toolName: 'docker') {
                        dir('/var/lib/jenkins/workspace/10-tier/src/adservice') {
                                sh "docker build -t arijit094/adservice:latest ."
                                sh "docker push arijit094/adservice:latest"
                                sh " docker rmi arijit094/adservice:latest"
                            
                        }
                    }
                }
                
            }
        }
        stage('cartservice') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-new', toolName: 'docker') {
                          dir('/var/lib/jenkins/workspace/10-tier/src/cartservice/src/') {
                                 sh "docker build -t arijit094/cartservice:latest ."
                                 sh "docker push arijit094/cartservice:latest"
								 sh " docker rmi arijit094/cartservice:latest"
                        }
                    }
                }
            }
        }
		
		stage('checkoutservice') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-new', toolName: 'docker') {
                          dir('/var/lib/jenkins/workspace/10-tier/src/checkoutservice/') {
                                 sh "docker build -t arijit094/checkoutservice:latest ."
                                 sh "docker push arijit094/checkoutservice:latest"
								 sh " docker rmi arijit094/checkoutservice:latest"
                        }
                    }
                }
            }
        }
		
		stage('currencyservice') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-new', toolName: 'docker') {
                          dir('/var/lib/jenkins/workspace/10-tier/src/currencyservice/') {
                                 sh "docker build -t arijit094/currencyservice:latest ."
                                 sh "docker push arijit094/currencyservice:latest"
								 sh " docker rmi arijit094/currencyservice:latest"
                        }
                    }
                }
            }
        }
        
		stage('emailservice') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-new', toolName: 'docker') {
                          dir('/var/lib/jenkins/workspace/10-tier/src/emailservice/') {
                                 sh "docker build -t arijit094/emailservice:latest ."
                                 sh "docker push arijit094/emailservice:latest"
								 sh " docker rmi arijit094/emailservice:latest"
                        }
                    }
                }
            }
        }
		
		stage('frontend') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-new', toolName: 'docker') {
                          dir('/var/lib/jenkins/workspace/10-tier/src/frontend/') {
                                 sh "docker build -t arijit094/frontend:latest ."
                                 sh "docker push arijit094/frontend:latest"
								 sh " docker rmi arijit094/frontend:latest"
                        }
                    }
                }
            }
        }
		
		stage('loadgenerator') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-new', toolName: 'docker') {
                          dir('/var/lib/jenkins/workspace/10-tier/src/loadgenerator/') {
                                 sh "docker build -t arijit094/loadgenerator:latest ."
                                 sh "docker push arijit094/loadgenerator:latest"
								 sh " docker rmi arijit094/loadgenerator:latest"
                        }
                    }
                }
            }
        }
		
		stage('paymentservice') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-new', toolName: 'docker') {
                          dir('/var/lib/jenkins/workspace/10-tier/src/paymentservice/') {
                                 sh "docker build -t arijit094/paymentservice:latest ."
                                 sh "docker push arijit094/paymentservice:latest"
								  sh " docker rmi arijit094/paymentservice:latest"
                        }
                    }
                }
            }
        }
        
		stage('productcatalogservice') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-new', toolName: 'docker') {
                          dir('/var/lib/jenkins/workspace/10-tier/src/productcatalogservice/') {
                                 sh "docker build -t arijit094/productcatalogservice:latest ."
                                 sh "docker push arijit094/productcatalogservice:latest"
								 sh " docker rmi arijit094/productcatalogservice:latest"
                        }
                    }
                }
            }
        }
		
		stage('recommendationservice') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-new', toolName: 'docker') {
                          dir('/var/lib/jenkins/workspace/10-tier/src/recommendationservice/') {
                                 sh "docker build -t arijit094/recommendationservice:latest ."
                                 sh "docker push arijit094/recommendationservice:latest"
								 sh " docker rmi arijit094/recommendationservice:latest"
                        }
                    }
                }
            }
        }
		
		stage('shippingservice') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-new', toolName: 'docker') {
                          dir('/var/lib/jenkins/workspace/10-tier/src/shippingservice/') {
                                 sh "docker build -t arijit094/shippingservice:latest ."
                                 sh "docker push arijit094/shippingservice:latest"
								 sh " docker rmi arijit094/shippingservice:latest"
                        }
                    }
                }
            }
        }
        stage('k8s-deploy') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: 'my-eks22', contextName: '', credentialsId: 'kube-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://0807809959F99FAFDB2965BE21E68D03.yl4.ap-south-1.eks.amazonaws.com') {
                    
                         sh 'kubectl apply -f deployment-service.yml'
                         sh 'kubectl get pods '
                         sh 'kubectl get svc'
                    
                }

            }
        }
    }
}
