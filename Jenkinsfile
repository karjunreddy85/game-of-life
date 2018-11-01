node('master') {
environment {
  BUILD_ID = "$BUILD_ID"
}

stage('Checkout') {
	git 'https://github.com/arjunkundur/game-of-life.git'
}

stage('Build') {
    withMaven(maven: 'Maven') {
    sh "mvn install -DskipTests"
}
}

stage('Nexus_Deploy') {
	nexusArtifactUploader artifacts: [[artifactId: 'dev', classifier: '', file: 'gameoflife-web/target/gameoflife.war', type: 'war']], credentialsId: '44e5c24c-2ec4-4ac4-a599-58183becda76', groupId: 'dev', nexusUrl: '34.209.226.14:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'red', version: '$BUILD_ID'	
}

stage('Ansible_Playbook') {
	def BUILD_ID="${BUILD_ID}"
	ansiblePlaybook extras: 'BUILD_ID', inventory: '/home/ubuntu/inventory', playbook: 'playbook.yml'
	
}


}
