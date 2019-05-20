pipeline{
   // tool name: 'Groovy_Home', type: 'hudson.plugins.groovy.GroovyInstallation'
    agent any
    tools{
        maven 'Maven3.6.0'
        jdk 'JAVA'
        //groovy 'Groovy_Home'
    }
    stages{
        stage('checkout'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'admin', url: 'https://github.com/antresastefi/PetClinic.git']]])
            }
        }
       stage('Build'){
            steps{
              bat '''cd %WORKSPACE%\\Petclinic\\spring-petclinic-master\\main
              mvn install'''
            }
        }
        stage('Test Publish'){
            steps{
                bat '''cd %WORKSPACE%\\Petclinic\\spring-petclinic-master\\main
                mvn cobertura:cobertura'''
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: '\\Petclinic\\spring-petclinic-master\\main\\target\\site\\cobertura', reportFiles: 'index.html', reportName: 'Cobertura Report', reportTitles: ''])
            }
        }
        stage('read pom'){
             
            steps{
                script{
                 def readpom = readMavenPom file: 'Petclinic\\spring-petclinic-master\\main\\pom.xml';
                 def Version = readpom.version;
                 println "$Version"
                 //def file1=new file('properties.txt')
                 //file1<<"$Version"
                 writeFile file: 'D:\\properties.txt', text: "${Version}"
                }
               
            }
            
        }
       
}
}
