

rolling update


Go to Kubernetes cluster

Create cluster

go to CLOUD SHELL




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


![image](https://user-images.githubusercontent.com/33985509/103161786-35c43280-47e7-11eb-8273-5fbd6e7108c2.png)



