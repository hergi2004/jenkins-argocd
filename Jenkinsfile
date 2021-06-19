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
                    myapp = docker.build("hergi2004/argocd-demo:${env.GIT_COMMIT}")
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
           git credentialsId: 'git', url: 'https://github.com/hergi2004/nginx.git'
           dir("nginx") {
           sh "ls -nginx ./*" 
            sh "cd ./templates && sed -i 's/argocd-demo:latest/argocd-demo:${env.BUILD_ID}/g' deployment.yaml"
//              sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
            sh "git commit -am 'Publish new version' && git push || echo 'no changes'"
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
    }
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
