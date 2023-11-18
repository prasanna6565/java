@Library('my-shared-library') _

pipeline{

    agent any

    parameters {
        string(name: 'ArtifactoryUrl', description: 'Artifactory URL', defaultValue: 'https://your-artifactory-instance/artifactory')
        string(name: 'ArtifactoryRepo', description: 'Artifactory repository', defaultValue: 'your-repo')
    }

    stages{
         
        stage('Git Checkout'){
                    when { expression {  params.action == 'create' } }
            steps{
            gitCheckout(
                branch: "main",
                url: "https://github.com/prasanna6565/Java_app_3.0.git"
            )
            }
        }
         stage('Unit Test maven'){
         
         when { expression {  params.action == 'create' } }

            steps{
               script{
                   
                   mvnTest()
               }
            }
        }
         stage('Integration Test maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   mvnIntegrationTest()
               }
            }
        }
        
        stage('Maven Build : maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   mvnBuild()
               }
            }
        }
         stage('Deploy to Artifactory') {
            steps {
                script {
                    // Deploy the JAR file to Artifactory using HTTP request
                    def artifactoryUrl = "${params.ArtifactoryUrl}/${params.ArtifactoryRepo}/<GROUP_ID>/<ARTIFACT_ID>/<VERSION>/<ARTIFACT_ID>-<VERSION>.jar"
                    def fileToUpload = "target/*.jar"

                    sh "curl -u your-username:your-password -X PUT ${artifactoryUrl} --upload-file ${fileToUpload}"
                }
          }
     }
}
