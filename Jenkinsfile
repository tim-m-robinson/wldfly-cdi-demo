node {
    stage('Prep') {
        deleteDir()
        git url: 'https://github.com/tim-m-robinson/wldfly-cdi-demo.git'
    }
    // Maven build steps
    withDockerContainer(image: 'maven:3-jdk-8',
          args: '''--network="citools" 
                   -v /var/run/docker.sock:/var/run/docker.sock
                   --group-add 994''') {

        stage('Build') {
          sh 'mvn -B compile'
        }
        
        stage('Test') {
          sh 'mvn -B test'
        }
        
        stage('Dependency Check') {
          sh 'mvn org.owasp:dependency-check-maven:2.1.0:check'
        }

        stage('Sonar Check') {
          sh '''mvn sonar:sonar \
                -Dsonar.host.url=http://sonar:9000 \
                -Dsonar.login=admin \
                -Dsonar.password=admin'''
        }

        stage('Package') {
          sh 'mvn -B package'
        }

        stage('Containerise') {
          sh 'mvn docker:build'
        }
    }
    stage('Release') {
        //
    }
}
