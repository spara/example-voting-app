node {
    def resultImage
    def voteImage
    def workerImage
    docker.withRegistry("https://index.docker.io/v1/", "spara" ) { 
      stage('Clone repo') {
        checkout scm
      }
      stage('Build result') {
        resultImage = docker.build("spara/result", "./result")
      } 
      stage('Build vote') {
        voteImage = docker.build("spara/vote", "./vote")
      }
      stage('Build worker dotnet') {
        workerImage = docker.build("spara/worker", "./worker")
      }
      stage('Push result image') {
          resultImage.push("${env.BUILD_NUMBER}")
          resultImage.push()
      }
      stage('Push vote image') {
          voteImage.push("${env.BUILD_NUMBER}")
          voteImage.push()
      }
      stage('Push worker image') {
          workerImage.push("${env.BUILD_NUMBER}")
          workerImage.push()
      }
      stage('Test deploy') {
        sh "docker-compose -f docker-compose-simple-jenkins.yml up -d --verbose --abort-on-container-exit"
      }
      post {
        always {
            sh "docker-compose down -v"
        }
      }
    }
}
