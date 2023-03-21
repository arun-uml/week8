podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: gradle
        image: gradle:jdk8
        command:
        - sleep
        args:
        - 99d
        restartPolicy: Never
''') 
{
  node(POD_LABEL) {
    stage('gradle') {
		git 'https://github.com/arun-uml/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git'
		container('gradle') {			
			stage('Build gradle') {
			  sh '''
			  pwd
			  cd Chapter09/sample3
			  chmod +x gradlew			  
			  '''
			}
			stage("Acceptance Test") {
			  sh '''
			  ./gradlew acceptanceTest -Dcalculator.url=http://calculator-service:8080
			  '''
			}
			stage("Test Result") {
			  echo 'Generating test results'
			  publishHTML(target: [
              reportDir: 'build/reports/tests/acceptanceTest',
              reportFiles: 'index.html',
              reportName: "Acceptance Test Result"
              ])
            }
        }	
	}
  }
}

