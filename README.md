# Flask DateTime API Deployment

## Deployment Workflow
1. **Provision Infrastructure with Terraform**
2. **Build and Push Docker Image to AWS ECR using Gitlab CI**
3. **Deploy to AWS ECS using GitLab CD**

---

## 1. Provisioning Infrastructure with Terraform

### **Steps to Deploy ECS Fargate Cluster**
1. **Initialize Terraform**
   ```sh
   terraform init
   ```
2. **Validate Terraform Configuration**
   ```sh
   terraform validate
   ```
3. **Plan Infrastructure Changes**
   ```sh
   terraform plan
   ```
4. **Apply Infrastructure Changes**
   ```sh
   terraform apply -auto-approve
   ```

This creates:
 **VPC, Subnet, and Security Group**
 **ECS Cluster and Fargate Service**
 **IAM Roles for ECS and GitLab Runner**

---

## 2. Building & Pushing Docker Image

### **GitLab CI/CD Pipeline**
The pipeline builds the Docker image and pushes it to AWS ECR.

#### **Steps**
1. **GitLab Runner logs into AWS ECR**
2. **Builds the Flask app Docker image**
3. **Pushes the image to AWS ECR**

#### **Trigger Manually**
```sh
git push origin main
```

---

## 3. Deploying to AWS ECS

After pushing the Docker image, GitLab CI/CD updates the **ECS service** to use the new image.

#### **Deployment Steps**
1. **Updates ECS service with new task definition**
2. **Forces a new deployment**

#### **Trigger Manually**
```sh
git push origin main
```

---

##  Verifying the Deployment
Once deployed, access the API:
```sh
curl http://<ECS_PUBLIC_IP>:5000/datetime
```

If everything is set up correctly, you should receive:
```json
{"datetime": "2024-03-24T12:34:56.789Z"}
```

##Assumptions 
1. Runner is running inside EC2 in order for it to assume the role and talk to ECR and relevant services.
