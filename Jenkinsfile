def withPod(body) {
 podTemplate(containers: [
   containerTemplate(name: 'docker', image: 'docker', command: 'cat', ➥ttyEnabled: true),
  ]
 ) {
  body()
 }
}

withPod {
 node(POD_LABEL) {
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