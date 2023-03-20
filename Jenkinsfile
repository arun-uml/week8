podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: centos
        image: centos
        command:
        - sleep
        args:
        - 99d
        volumeMounts:
        - name: shared-storage
          mountPath: /mnt              
      restartPolicy: Never
      volumes:
      - name: shared-storage
        persistentVolumeClaim:
          claimName: jenkins-pv-claim
''') 
{
  node(POD_LABEL) {
    stage('k8s') {
		git 'https://github.com/arun-uml/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git'
		container('centos') {			
			stage('Start calculator') {
			  sh '''
			  curl -ik -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"
			  https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/api/v1/namespaces/devops-tools/pods
			  '''
			}
		}
	}
  }
}

