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
			stage('Acceptance Test') {
			  sh '''
			  pwd
			  cd Chapter09/sample3
			  chmod +x gradlew
              ./gradlew acceptanceTest -Dcalculator.url=http://calculator-service:8080			  
			  '''
			}
			stage("Test Result") {
			  echo 'Generating test results'
			  publishHTML(target: [
              reportDir: 'Chapter09/sample3/build/reports/tests/acceptanceTest',
              reportFiles: 'index.html',
              reportName: "AcceptanceTestResult"
              ])
            }
        }	
	}
  }
}

