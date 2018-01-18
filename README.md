# chart-jenkins
This repository contains 2 charts that are used to deploy jenkins to kubernetes.
- jenkins-storage
- jenkins

## Installing
First install `jenkins-storage` chart
```
helm install --name jenkins-storage chartmuseum/jenkins-storage
```

After that, install `jenkins` chart
```
helm install --name jenkins chartmuseum/jenkins
```