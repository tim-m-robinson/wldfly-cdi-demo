node {
    stage('Prep') {
        deleteDir()
        git url: 'https://github.com/tim-m-robinson/wldfly-cdi-demo.git'
    }
    // Maven build steps
    withDockerContainer('maven:3-jdk-8') {

        stage('Build')
        sh 'mvn compile'
        
        stage('Test')
        sh 'mvn test'
        
        stage('Dependency Check')
        //sh 'mvn org.owasp:dependency-check-maven:2.1.0:check'

        stage('Sonar Check') 
        sh '''mvn sonar:sonar \
                -Dsonar.host.url=http://sonar:9000 \
                -Dsonar.login=admin \
                -Dsonar.password=admin'''

        stage('Package')
        sh 'mvn package'

        stage('Containerise')
        sh 'mvn docker:build'
    }
    stage('Release') {
        //
    }
}
