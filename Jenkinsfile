//Jenkins file for jenkins server on amazon ec2 instance to work with Rancher CD
pipeline {
    agent any
    environment {
        PROJECT_ID = 'PROJECT-ID'
        CLUSTER_NAME = 'CLUSTER-NAME'
        LOCATION = 'CLUSTER-LOCATION'
        CREDENTIALS_ID = 'gke'
          
    }
    stages {
//         stage("Checkout code") {
//             steps {
//                 checkout scm
//             }
//         }
        stage("Build image") {
            steps {
                script {
//                       sh "until docker ps; do sleep 3; done && docker build -t hergi2004/argocd-demo:${env.GIT_COMMIT} ."
                    myapp = docker.build("hergi2004/argocd-demo:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
    
    stage('Deploy E2E') {
      environment {
        GIT_CREDS = credentials('git')
      }
      steps {

//                 checkout([$class: 'GitSCM', branches: [[name: '*/branchname']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'jenkins-user-github', url: 'https://github.com/aakashsehgal/FMU.git']]])
//                 sh "ls -lart ./*"
         script {
           // The below will clone your repo and will be checked out to master branch by default.
//            git credentialsId: 'git', url: 'https://github.com/hergi2004/nginx.git'
             // Git clone helm chart repo
             git credentialsId: 'ssh', url: 'git@github.com:hergi2004/nginx.git'
//           git config --global http.sslVerify false
//            git config --global user.email 'hergi2004@gmail.com'
//            git config --global user.name 'hergi2004'
//               git config --global credential.username {GIT_USERNAME}
            // Make image version changes on deployment
           dir("templates") { 
            sh "sed -i 's/argocd-demo:latest/argocd-demo:${env.BUILD_ID}/g' deployment.yaml"
//            sed -i 's/argocd-demo:latest/argocd-demo:${env.BUILD_ID}/g' deployment.yaml"
//              sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
//              sh "git config --global --edit"
//              sh "git commit --amend --reset-author"
//                sh "git commit -am 'Publish new version' && git push git@github.com:hergi2004/nginx.git || echo 'no changes'"
                   sh "git commit -am 'Publish new version' || true"
//                 sshagent(['git-credentials-id']) {
//                   sh "git push origin master"
//                 }
               // push latest Helm chart to trigger deployment
               sshagent(['ssh']) {
                sh("git push git@github.com:hergi2004/nginx.git")
            }
//         withCredentials([sshUserPrivateKey(credentialsId: CODECOMMIT_CREDENTIALS_ID, keyFileVariable: 'SSH_KEY', usernameVariable: 'SSH_USER')]) {
//         withEnv(["GIT_SSH_COMMAND=ssh -o StrictHostKeyChecking=no -o User=${SSH_USER} -i ${SSH_KEY}"]) {
//         sh 'git push origin git@github.com:hergi2004/nginx.git:master'
//     }
// }
}
//             sh "git commit -am 'Publish new version' && git push --set-upstream origin master || echo 'no changes'"
//                sh "git commit -am 'Publish new version' --amend --author="hergi2004 hergi004@gmail.com" && git push --set-upstream origin master || echo 'no changes'"
           // Do a ls -lart to view all the files are cloned. It will be clonned. This is just for you to be sure about it.
//            // List all branches in your repo. 
//            sh "git branch -a"
//            // Checkout to a specific branch in your repo.
//            sh "git checkout branchname"
           }
          }
            }
        }   
    }
//     }
//     stage('Deploy to Prod') {
//       steps {
//         input message:'Approve deployment?'
//         container('tools') {
//           dir("argocd-demo-deploy") {
//             sh "cd ./prod && kustomize edit set image hergi2004/argocd-demo:${env.GIT_COMMIT}"
//             sh "git commit -am 'Publish new version' && git push https://hergi2004@github.com/hergi2004/argocd-demo-deploy.git || echo 'no changes'"
//           }
//         }
//       }
//     }
//   }
// }
