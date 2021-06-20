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
         script {
             git credentialsId: 'ssh', url: 'git@github.com:hergi2004/nginx.git'

           dir("templates") { 
            sh "sed -i 's/argocd-demo:latest/argocd-demo:${env.BUILD_ID}/g' deployment.yaml"
                   sh "git commit -am 'Publish new version' || true"
               sshagent(['ssh']) {
                sh("git push git@github.com:hergi2004/nginx.git || echo 'no changes'")
                   
            }

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
