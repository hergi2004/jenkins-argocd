// pipeline {
//   agent any
//   stages {

//     stage('Build') {
//       environment {
//         DOCKERHUB_CREDS = credentials('dockerhub')
//       }
//       steps {
//           // Build new image
//           sh "until docker ps; do sleep 3; done && docker build -t hergi2004/argocd-demo:${env.GIT_COMMIT} ."
//           // Publish new image
//         sh "docker login --u ${DOCKERHUB_CREDS_USR} --p ${DOCKERHUB_CREDS_PSW} && docker push hergi2004/argocd-demo:${env.GIT_COMMIT}"
      
//         }
//       }
agent {
    node {
        label 'node1'
        customWorkspace '/some/other/path'

    }
}
        stage('Push Docker Image to Docker Registry') {
        container('docker'){
            withCredentials([[$class: 'UsernamePasswordMultiBinding',
            credentialsId: env.dockerhub,
            usernameVariable: 'USERNAME',
            passwordVariable: 'PASSWORD']]) {
                docker.withRegistry(env.DOCEKR_REGISTRY, env.DOCKER_CREDENTIALS_ID) {
                    sh("docker push ${app1_image_tag}")
                    sh("docker push ${app2_image_tag}")
                }
            }
        }
        }

    
//     stage('Deploy E2E') {
//       environment {
//         GIT_CREDS = credentials('git')
//       }
//       steps {
//         container('tools') {
//        //     sh "git config --global user.email 'hergi2004@gmail.com'"
//       //      sh "git clone https://github.com/hergi2004/argocd-demo-deploy.git"
//             sh "git config --global http.sslVerify false"
//             sh "git config --global user.email 'hergi2004@gmail.com'"
//             sh "git config --global user.name 'hergi2004'"
//      //       sh "git clone https://${GIT_CREDS_USR}:${GIT_CREDS_PSW}@github.com/${GIT_CREDS_USR}/argocd-demo-deploy.git"
//             sh "git clone https://github.com/hergi2004/argocd-demo-deploy.git"
//           dir("argocd-demo-deploy") {
//             sh "cd ./e2e && kustomize edit set image hergi2004/argocd-demo:${env.GIT_COMMIT}"
//             sh "git commit -am 'Publish new version' && git push || echo 'no changes'"
//           }
//         }
//       }
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
