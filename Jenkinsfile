pipeline {
 agent {
    node {
        label 'jenkins-agent-1'
    }
 }
//  environment {
//       versioncheck = ''
//       nexus_url = '172.31.16.69:8081'
//  }

   parameters {
//         // booleanParam, choice, file, text, password, run, or string
//         booleanParam(defaultValue: true, description: '', name: 'booleanExample')
        string(defaultValue: '', description: 'What is version?', name: "version")
        string(defaultValue: '', description: 'What is environment?', name: "environment")
//         text(defaultValue: "This is a multiline\n text", description: "Multiline Text", name: "textExample")
//         choice(choices: 'US-EAST-1\nUS-WEST-2', description: 'What AWS region?', name: 'choiceExample')
//         password(defaultValue: "Password", description: "Password Parameter", name: "passwordExample")
   }
//  parameters {
//       choice(choices: 'PROD\nDEV', description: 'Choose PROD or DEV?', name: 'choice')
//  }
   
   options {
                timeout(time: 1, unit: 'HOURS')
                ansiColor('xtrem')
           }
    

    stages {
   //   stage('checkout_from_scm') {
   //   steps {
   //       git branch: 'main', changelog: false, poll: false, url: 'https://github.com/Mygit-Naresh/catalogue.git'
   //     }
   //  }
    stage('print version and enviroment') {
      steps {
       script {
        echo  params.version
        echo  params.environment
       }

    }
    }
       stage('terraform init from tfvars') {
      steps {
      
        sh """
          cd terraform
          terraform init  --backend-config=${params.environment}/backend.tf -reconfigure
          

        """
       

    }
    }
   stage('terraform plan from tfvars') {
      steps {
       script {
        sh """
          cd terraform
          terraform plan -var-file=${params.environment}/${params.environment}.tfvars -var="appversion=${params.version}"
          """
       }

    }
    }
     stage('terraform apply from tfvars') {
      steps {
       script {
        sh """
          cd terraform
          terraform apply -var-file=${params.environment}/${params.environment}.tfvars -var="appversion=${params.version} -auto-approve"
          """
       }

    }
    }

    }
      post {
   always {
      echo "Check you status below failure or success"
      
   }
    failure {
        echo "your build failed"
    }
    success {
        echo "your build is success thumbs up"
        
    }
  }
}



