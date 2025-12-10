# Best Buy Cloud-Native Application
 
## **Student Name:** Jigarkumar Patel<br> **Student ID:** 041169204


## Project Overview  
This project is a **microservices-based Best Buy e-commerce application** deployed on **Kubernetes**.

It simulates a **Best Buy online shopping system** where:

- Customers place orders using the **Store Front**
- Employees manage orders using the **Store Admin**
- Orders move through **RabbitMQ**
- Orders are stored in **MongoDB**
- All services run as **Docker containers**
- **CI/CD is implemented using GitHub Actions**

This project demonstrates a **real-world cloud-based retail system using Kubernetes**.

---

##  Architecture Diagram  
The architecture diagram is created using **Draw.io** and shows:


📂 **Diagram File Location:**  
`/assets/BestBuy-Architecture.drawio.png`  
(Add your exported Draw.io image here)

---

## Application Services

| Service Name | Purpose |
|--------------|--------|
| Store Front | Customer website to place orders |
| Store Admin | Employee dashboard to manage orders |
| Order Service | Sends orders to the message queue |
| Product Service | Manages product data |
| Makeline Service | Reads orders from queue and saves to database |
| RabbitMQ | Message queue system |
| MongoDB | Database to store orders |

---




## Deployment Instructions

This section explains the exact steps I followed to deploy my customized Besy Buy application to a Kubernetes cluster on Azure. All UI changes were completed before containerization so that the deployed version matched my final design.

---

### Step 1: Update Product Instructions and Images

I first updated the product descriptions. After that, I replaced the old product images with new images inside the frontend project. This step was completed before building any containers so that the UI changes were included in the deployment.

---

### Step 2: Update CSS Design

I modified the CSS files to change the visual design of the application. This included updating colors, spacing, and the overall layout so that the user interface matched my customized design instead of the default template.

---

### Step 3: Build Docker Images for All Services

After completing all UI and design changes, I built Docker images for all the microservices. Each service was converted into its own container image so that it could be deployed to Kubernetes.

```
docker build -t jigarkumar25/store-front-l8 .   

docker build -t jigarkumar25/store-admin-l8 .

docker build -t jigarkumar25/order-service-l8 .

docker build -t jigarkumar25/product-service-l8 .

docker build -t jigarkumar25/makeline-service-l8 .
```
---

### Step 4: Push Images to Docker Registry

Once the images were built, I pushed all of them to the Docker registry. This allowed the Kubernetes cluster to pull the images during deployment.
```
docker push jigarkumar25/store-front-l8

docker push jigarkumar25/store-admin-l8

docker push jigarkumar25/order-service-l8

docker push jigarkumar25/product-service-l8

docker push jigarkumar25/makeline-service-l8
```
---

### Step 5: Create Kubernetes Cluster

I created a Kubernetes cluster in Azure to host the application. The cluster was configured with 2 worker nodes, and I connected it to my local machine using kubectl so that I could control the deployment.

```
az aks get-credentials --resource-group myRG --name myAKS
```

---

### Step 6: Edit Previous YAML Files from the Last Lab

Instead of creating new YAML files, I reused the YAML files from my previous lab. I only updated the Docker image names to match the new images that I built and pushed to the registry.
I added that **allservice.yaml** file into **/Deployment Files**

---

### Step 7: Apply ConfigMaps File and allservices.yaml

After updating the YAML files, I applied the config-maps.yaml file first. This created all the required environment variables and shared configuration needed by the microservices.
```
kubectl apply -f config-maps.yaml
```
After the ConfigMaps were created successfully, I applied the allservices.yaml file to deploy all the microservices together. This deployed both the backend and frontend services in a single step.
```
kubectl apply -f config-maps.yaml
```
---

### Step 8: Verify Pod Status

I checked the status of all the pods to make sure that they were created successfully and were running without any errors.
```
kubectl get pods
```
---

### Step 9: Verify Service Logs

I reviewed the logs of the services to confirm that all the microservices were communicating properly and that there were no startup or runtime errors.
```
kubectl logs deployment/makeline-service
kubectl logs deployment/order-service
```
---

### Step 10: Access the Application

I accessed the application using the external service IP provided by Kubernetes. The storefront loaded successfully in the browser and all features were working as expected.
```
kubectl get svc
```
After all services were deployed and running, I accessed the RabbitMQ management interface by forwarding the RabbitMQ service port to my local machine. This allowed me to open the RabbitMQ dashboard in the browser and verify that message queues and connections were working correctly.

```
kubectl port-forward service/rabbitmq 15672:15672
```
I connected directly to the MongoDB pod running inside the Kubernetes cluster using an interactive terminal session. From there, I accessed the MongoDB shell to verify that the database was running correctly and that application data was being stored properly.
```
kubectl exec -it statefulset/mongodb -- mongo
use orderdb
db.orders.find()
```
---

### Step 11: Final Verification

I performed final validation to confirm that:
- The storefront user interface loads correctly  
- Product data displays properly  
- Orders can be placed successfully  
- The makeline service processes orders correctly  
- RabbitMQ is running and handling messages  
- MongoDB is accessible and storing data  
- No pods are crashing or restarting  
---

### Deployment Completed

The application was successfully customized, containerized, deployed to Kubernetes, and verified for full functionality.

---

### Configure CI/CD Workflow Using GitHub Actions:
The CI/CD workflow file was already present in the cloned repository from the previous lab. I did not create a new workflow file. Instead, I added the new secret keys required for deployment into the GitHub repository secrets for each service. 
```
cat ~/.kube/config | base64 -w 0 > kube_config_base64.txt
```
After updating the secrets, I committed all my modified files, including UI changes, image updates, and configuration updates, and pushed everything to GitHub. This triggered the existing CI/CD workflow to automatically build and deploy the updated application.

---
## Repository & Docker Hub Links


| Service Name        | GitHub Repository Link                                                                 | Docker Hub Image Link                                                                 |
|---------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| Store Front         | [Store Front](https://github.com/Jigarkumar25/store-front-L8)                            | [Store Front Image](https://hub.docker.com/r/jigarkumar25/store-front-l8)               |
| Store Admin         | [Store Admin](https://github.com/Jigarkumar25/store-admin-L8)                            | [Store Admin Image](https://hub.docker.com/r/jigarkumar25/store-admin-l8)               |
| Order Service       | [Order Service](https://github.com/Jigarkumar25/order-service-L8)                        | [Order Service Image](https://hub.docker.com/r/jigarkumar25/order-service-l8)           |
| Product Service     | [Product Service](https://github.com/Jigarkumar25/product-service-L8)                    | [Product Service Image](https://hub.docker.com/r/jigarkumar25/product-service-l8)       |
| Makeline Service    | [Makeline Service](https://github.com/Jigarkumar25/makeline-service-L8)                 | [Makeline Service Image](https://hub.docker.com/r/jigarkumar25/makeline-service-l8)     |

---

## Youtube Video: 
<a href="https://youtu.be/3CrCeDGPPFY">Video Presentation</a>