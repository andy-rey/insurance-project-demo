node{
    
    stage('checkout'){
        git 'https://github.com/andy-rey/insurance-project-demo.git'
    }
    
    stage('maven build'){
        sh 'mvn clean package'
    }
    
    stage('containerize'){
      // sh 'docker build -t amarixrey/insure-me:1.0 .'
    }
    
    stage('Release'){
        withCredentials([string(credentialsId: 'dockerHubPwd', variable: 'dockerHubPwd')]) {
       sh "docker login -u amarixrey -p ${dockerHubPwd}"
       sh 'docker push amarixrey/insure-me:1.0'
        }
    }
    
    stage('Deploy to Test'){
     ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    
    stage('checkout regression test source code'){
        git 'https://github.com/andy-rey/devOpsSeleniumHW.git'
    }
    
    stage('build test scripts'){
        sh 'mvn clean package assembly:single'
    }
    
    stage('execute selenium test script'){
        sh 'java -jar target/assignment-0.0.1-SNAPSHOT-jar-with-dependencies.jar'
    }

    stage('checkout'){
        git 'https://github.com/andy-rey/insurance-project-demo.git'
    }
    
     stage('Deploy to Prod'){
     ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }
    
    
    
}
