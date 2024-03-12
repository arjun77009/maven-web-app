node {
    
    stage ('Git Clone'){
        git credentialsId: 'c87aff7e-f5f1-4756-978f-3379694978e6', url: 'https://github.com/ashokitschool/maven-web-app.git'
    }
    
    stage ('Maven Clean Build'){
        def mavenHome = tool name: "Maven-3.8.6", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
    
    stage ('Build Docker Image'){
       sh "docker build -t nvreddy124/mavenwebapp ."
    }
    
    stage ('Docker Push'){
       withCredentials([string(credentialsId: '278c300d-d845-4989-9169-0a00ea37a64c', variable: 'DOCKER_CREDENTIALS')]) {
       sh "docker login -u ashokit -p ${DOCKER_CREDENTIALS}"
    }

        sh "docker push ashokit/mavenwebapp"
    }
    
   /** stage('Deploy App in K8S Cluster'){
        kubernetesDeploy(
            configs: 'maven-web-app-deploy.yml',
            kubeconfigId: 'KUBERNETES_CONFIGURATION'
        )
    }**/
    
    stage('Deploy App in K8S Cluster'){
       sh 'kubectl apply -f maven-web-app-deploy.yml'
    }
    
}

