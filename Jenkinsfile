pipeline {
agent {
label 'Build-server'
}

stages {

properties([parameters([choice(choices: ['account-services', 'customer-service', 'discovery-services', 'zipkin-services', 'gateway-service'], description: 'select a service name to build and deploy', name: 'Service_name')])])

stage ('Checkout') 
{
steps
    {
    
        checkout scm
        
    }
    
}
stage ('Build') 
{
    steps
    {
       sh "cd /home/ubuntu/workspace/Jenkins-Pipeline/${params.Service_name} ; mvn clean install " 
    }
}
    stage ('dockerbuild') 
{
    steps
    {
        sh "cd /home/ubuntu/workspace/Jenkins-Pipeline/${params.Service_name} ; sudo docker build t $(params.Service_name) . " 
    }
}
     stage ('dockerimagepush ') 
{
    steps
    {
       sh "cd /home/ubuntu/workspace/Jenkins-Pipeline-java-ms/${param.Service_name} ; sudo  docker login -uankit1111 -pmiet@1234 "
        sh "cd /home/ubuntu/workspace/Jenkins-Pipeline-java-ms/$(param.Service_name} ; sudo docker tag account-service ankit1111/$(param.Service_name)  "
        sh "cd /home/ubuntu/workspace/Jenkins-Pipeline-java-ms/$(param.Service_name) ; sudo docker push ankit1111/$(param.Service_name)  "
        
        
    }
}
    
    
stage ('k8sdeployment') 
    {
        steps {
            node (' Ansible-server') {
             sh " sudo ansible-playbook /root/k8s.yaml"
   
    }
}
}
}
    
    
}
