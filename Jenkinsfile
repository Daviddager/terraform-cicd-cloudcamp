node('linux'){
    stage('Checkout'){
        checkout scmGit(
            branches: [[name: 'main']],
            extensions: [],
            userRemoteConfigs: [[
                url: 'https://github.com/Daviddager/terraform-cicd-cloudcamp.git'
            ]]
        )
    }
    
    withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-cloudcamp', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
        stage('Terraform Init'){
            sh 'terraform init'
        }
        stage('Terraform Validate'){
            sh 'terraform validate && terraform fmt'
        }

        withEnv(['PATH+CHECKOV=/home/admin/checkov-venv/bin/']) {
            stage('Checkov Scan'){
                sh 'checkov -d .'
            }
        }

        stage('Terraform Plan'){
            sh 'terraform plan'
        }
        stage('Terraform Apply'){
            echo 'terraform apply -auto-approve'
        }
    }
    
}