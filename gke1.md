


![image](https://user-images.githubusercontent.com/33985509/103160491-37392f00-47d6-11eb-9537-23dbc632fe4e.png)


gcloud auth configure-docker

docker tag cluster1-image gcr.io/&lt;deploying-to-27-9014ee10/cluster1-image:v1

docker push gcr.io/deploying-to-27-9014ee10/cluster1-image:v1

![image](https://user-images.githubusercontent.com/33985509/103160829-859bfd00-47d9-11eb-8411-50920cc534a7.png)





app.py

```
# The Docker image contains the following code
from flask import Flask
import os
import socket

app = Flask(__name__)

@app.route("/")
def hello():
    html = "<h1 style='text-align:center;margin:20px;'>Greetings from Linux Academy!</h1>"
    return html

if __name__ == "__main__":
  app.run(host='0.0.0.0', port=80)


```




config.yaml

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: 2018-08-20T19:49:31Z
  generation: 2
  labels:
    app: nginx-1
  name: nginx-1
  namespace: default
  resourceVersion: "904"
  selfLink: /apis/extensions/v1beta1/namespaces/default/deployments/nginx-1
  uid: 2805a653-a4b2-11e8-a16d-42010a960fc7
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-1
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx-1
    spec:
      containers:
      - image: gcr.io/[PROJECT_ID]/la-container-image:v1
        imagePullPolicy: IfNotPresent
        name: la-container-image
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status:
  availableReplicas: 1
  conditions:
  - lastTransitionTime: 2018-08-20T19:49:38Z
    lastUpdateTime: 2018-08-20T19:49:38Z
    message: Deployment has minimum availability.
    reason: MinimumReplicasAvailable
    status: "True"
    type: Available
  observedGeneration: 2
  readyReplicas: 1
  replicas: 1
  updatedReplicas: 1


```






dockerfile

```
# The Dockerfile defines the image's environment
# Import Python runtime and set up working directory
FROM python:2.7-slim
WORKDIR /app
ADD . /app

# Install any necessary dependencies
RUN pip install -r requirements.txt

# Open port 80 for serving the webpage
EXPOSE 80

# Run app.py when the container launches
CMD ["python", "app.py"]

```



Expose the deployment.
On the Deployment details page, click Expose.
Set the Service type to Load balancer.
Click Expose.
Confirm by clicking the External endpoints IP address.
