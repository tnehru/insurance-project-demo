node{
    stage('code checkout'){
        git 'https://github.com/tnehru/insurance-project-demo.git'
    }
    stage('mvn build'){
        sh 'mvn clean package'
    }
    stage('Deploy'){
        //sh 'docker build -t tnehru/insure-me:1.0 .'
    }
    stage('Release'){
        withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
    // some block
        //sh "docker login -u tnehru -p ${dockerhubpwd}"
        //sh 'docker push tnehru/insure-me:1.0'
        }
    }
    stage('Deploy to Test env'){
        ansiblePlaybook become: true, credentialsId: 'anisble-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    stage('code checkout'){
        git 'https://github.com/tnehru/my-selemnium-test-app.git'
    }
    stage('build test scripts'){
        sh 'mvn clean package assembly:single'
    }
    stage('Execute test scripts'){
        sh 'java -jar target/my-app-test-1.0-SNAPSHOT-jar-with-dependencies.jar'
    }
    stage('code checkout'){
        git 'https://github.com/tnehru/insurance-project-demo.git'
    }
    stage('Deploy to Test env'){
        ansiblePlaybook become: true, credentialsId: 'anisble-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }
}
