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
      restartPolicy: Never
''') {
  node(POD_LABEL) {
    stage('k8s') {
      git 'https://github.com/dlambrig/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git'
      container('centos') {
        stage('start calculator') {
          sh '''
          cd Chapter08/sample1

          curl -ik -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBRNETES_SERVICE_PORT/api/v1/namespaces/devops-tools/pods

          curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt) /bin/linux/amd64/kubectl"

          chmod +x ./kubectl

          curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://kubernetes.default.svc.cluster.local/apis/apps/v1/namespaces/devops-tools/deployments -XPOST -H "Content-type: application/yaml" --data-binary @calculator.yaml

          '''
        }
      }
    }
  }
}


// kubectl apiVersion
// ''') {
//   node(POD_LABEL) {
//     stage('k8s') {
//       git 'https://github.com/dlambrig/Continuous-Delivery-with-Docker-and-Jenkins-Second-Edition.git'
//       container('centos') {
//         stage('start calculator') {
//           sh '''
//           cd Chapter08/sample1
//           curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
//           chmod +x ./kubectl
//           ./kubectl apply -f calculator.yaml
//           ./kubectl apply -f hazelcast.yaml
//           '''
//         }
//       }
//     }
//   }
// }
