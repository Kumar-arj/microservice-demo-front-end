def label = "any"
def mvn_version = 'M2'
{
   node (label) {
       stage ('Checkout SCM'){
         git credentialsId: 'git', url: 'https://github.com/Kumar-arj/microservice-demo-front-end.git', branch: 'master'
       }
       stage ('Docker Build'){
         container('build') {
               stage('Build Image') {
                   docker.withRegistry( 'https://registry.hub.docker.com', 'docker' ) {
                   def customImage = docker.build("kumararj/microservice-demo-front:latest")
                   customImage.push()            
                   }
               }
           }
       }
       stage ('Helm Chart') {
         container('build') {
           dir('charts') {
             withCredentials([usernamePassword(credentialsId: 'jfrog', usernameVariable: 'username', passwordVariable: 'password')]) {
             sh '/usr/local/bin/helm package webapp'
             sh '/usr/local/bin/helm push-artifactory webapp-1.0.tgz
https://eosartifact.jfrog.io/artifactory/eos-helm-local
 --username $username --password $password'
             }
           }
         }
      }
   }
}