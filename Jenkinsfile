def withPod(body) {
 podTemplate(label: 'pod', containers: [
   containerTemplate(name: 'docker', image: 'docker', command: 'cat', ➥ttyEnabled: true),
  ]
 ) {
  body()
 }
}

withPod {
 node('pod') {
  def tag = "${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
  def service = "market-data:${tag}"
  checkout scm
  container('docker') {
   stage('Build') {
    sh("docker build -t ${service} .")
   }
  }
 }
}