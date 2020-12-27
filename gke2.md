

# Rolling Update


Go to Kubernetes cluster



go to CLOUD SHELL



create below files 


app.py

```
# The Docker image contains the following code
from flask import Flask
import os
import socket

app = Flask(__name__)

@app.route("/")
def showPinehead():
    html = "<div style='text-align:center;margin:20px;'><h1>Greetings from Linux Academy!</h1><h2>Version 1</h2><img src='https://storage.googleapis.com/la-gcp-labs-resources/essentials/Logo-Pinehead-NVY.png' width='40%' alt='Pinehead @ Linux Academy'></div>"
    return html

if __name__ == "__main__":
  app.run(host='0.0.0.0', port=80)

```



deploy-rolling.yaml



```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: rolling
spec:
  replicas: 4
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  minReadySeconds: 5
  template:
    metadata:
      labels:
        app: rolling
        track: stable
    spec:
      containers:
        - name: rollingv1cd 
          image: linuxacademycontent/content-gcp-labs:rolling-v1
          ports:
            - name: http
              containerPort: 80

```


dockerfile

```
# The Dockerfile defines the image's environment
# Import Python runtime and set up working directory
FROM python:3.7-slim
WORKDIR /app
ADD . /app

# Install any necessary dependencies
RUN pip install -r requirements.txt

# Open port 80 for serving the webpage
EXPOSE 80

# Run app.py when the container launches
CMD ["python", "app.py"]


```

gcloud config set compute/zone us-central1-a



gcloud container clusters create gke-cluster1 --num-nodes=4


![image](https://user-images.githubusercontent.com/33985509/103161808-a703e580-47e7-11eb-8dbc-1245918f228d.png)


![image](https://user-images.githubusercontent.com/33985509/103161834-22659700-47e8-11eb-9fad-0d5b24326a0c.png)


gcloud container clusters get-credentials gke-cluster1





# Need to check finding error

no matches for kind “Deployment” in version "extensions/v1beta1

ValidationError: missing required field “selector” in io.k8s.api.v1.DeploymentSpec



next steps

kubectl expose deployment rolling --type=LoadBalancer --name=rolling-service --port=80 --target-port=80

kubectl get services rolling-service


then make changes in the deploy.yaml file to rolling-v2


kubectl apply -f deploy-rolling.yaml
