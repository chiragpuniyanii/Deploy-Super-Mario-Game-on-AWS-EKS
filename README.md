# Deploy Super Mario Game on AWS EKS

Super Mario is a classic game loved by many. In this guide, we’ll explore how to deploy a Super Mario game on Amazon’s Elastic Kubernetes Service (EKS). Utilizing Kubernetes, we can orchestrate the game’s deployment on AWS EKS, allowing for scalability, reliability, and easy management.

## Prerequisites:
- An Ubuntu Instance
- IAM Role
- Terraform should be installed on the instance
- AWS CLI and KUBECTL on the instance

## Let’s Deploy

### Step 1: Launch Ubuntu Instance

1. **Sign in to AWS Console**: Log in to your AWS Management Console.
2. **Navigate to EC2 Dashboard**: Go to the EC2 Dashboard by selecting “Services” in the top menu and then choosing “EC2” under the Compute section.
3. **Launch Instance**: Click on the “Launch Instance” button to start the instance creation process.
4. **Choose an Amazon Machine Image (AMI)**: Select an appropriate AMI for your instance (e.g., Ubuntu image).
5. **Choose an Instance Type**: In the “Choose Instance Type” step, select `t2.micro`. Proceed by clicking “Next: Configure Instance Details.”
6. **Configure Instance Details**:
   - Set “Number of Instances” to 1 (unless you need multiple instances).
   - Configure additional settings like network, subnets, IAM role, etc., if necessary.
   - For “Storage,” click “Add New Volume” and set the size to 8GB (or modify the existing storage to 8GB).
   - Click “Next: Add Tags” when you’re done.
7. **Add Tags (Optional)**: Add any desired tags to your instance. This step is optional but helps in organizing instances.
8. **Configure Security Group**: Configure your security group to allow inbound traffic on necessary ports (e.g., port 22 for SSH).
9. **Review and Launch**: Review the configuration details and click “Launch Instances.”
10. **Access the EC2 Instance**: Once the instance is launched, access it using SSH with the key pair and instance’s public IP or DNS.

---

### Step 2: Create IAM Role

1. **Search for IAM** in the AWS Console search bar and click on "Roles".

  ![image](https://github.com/user-attachments/assets/229b1a73-51f8-423c-a746-fbbb27eaf1c8)

2. **Create Role**: 

  ![image](https://github.com/user-attachments/assets/deb71699-5ab1-4c0f-a2b3-719ad7f750e3)

   - Select "AWS service" as the trusted entity type.
   - Choose "EC2" as the use case, then click "Next".
    ![image](https://github.com/user-attachments/assets/9335c26c-6699-4afc-b2cd-db501d8fc068)

   - Select **Administrator Access** (for learning purposes), then click "Next".
   ![image](https://github.com/user-attachments/assets/4a5b96ff-8c93-47b5-87c1-575ec97c2f0d)
   - Provide a name for the role (e.g., `Mario`) and click "Create Role".
    ![image](https://github.com/user-attachments/assets/d4cfd0fa-4ade-4803-916a-06e3eb0c7fa1)
   ![image](https://github.com/user-attachments/assets/fb7bdb00-14d9-450b-bb09-f0a5eb1228d1)

3. **Attach Role to EC2 Instance**:
   - Go to the EC2 Dashboard and select the instance.
   - Click on **Actions → Security → Modify IAM role**.
  ![image](https://github.com/user-attachments/assets/f7ff12cc-45b3-4b72-b821-4abd2ef2cd8b)

   - Choose the role you just created and click "Update IAM role".
   ![image](https://github.com/user-attachments/assets/fc5130fd-1d9d-4a23-8a2b-7be486b10b20)
   - Connect the instance to Mobaxtreme or Putty

   


---

### Step 3: Cluster Provision

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/chiragpuniyanii/Deploy-Super-Mario-Game-on-AWS-EKS.git
   cd k8s-mario
2. **Make the Script Executable:**

```bash
sudo chmod +x script.sh
```

3. **Run the Script:**


![image](https://github.com/user-attachments/assets/32923546-f3e8-4ef7-9067-64d004e68184)

```bash

./script.sh
```
4. **Check Versions:**

```bash

docker --version
aws --version
kubectl version --client
terraform --version
```
![image](https://github.com/user-attachments/assets/932ce75f-a2ee-4a15-87b7-96574016a06a)

5. **Change Directory to EKS-TF:**

```bash

cd EKS-TF
```

6. **Initialize Terraform:**

```bash

terraform init
```

NOTE: Don’t forgot to change the s3 bucket name in the backend.tf file
![image](https://github.com/user-attachments/assets/5a6db117-1e7d-4185-ba26-218715c2a449)

7. **Validate and Plan Terraform:**

```bash

terraform validate
terraform plan
```
![image](https://github.com/user-attachments/assets/8a23f0df-40a4-4eae-8855-33ef91d9895b)


8. **Provision the Cluster:**

```bash

terraform apply --auto-approve
```
![image](https://github.com/user-attachments/assets/d74ec49d-bcde-4fdd-8170-08fb6402986b)



Completed in 10mins

![image](https://github.com/user-attachments/assets/4b28602c-6312-4d6c-b36d-fc733fea0031)

9. **Update Kubernetes Configuration:**

```bash

aws eks update-kubeconfig --name EKS_CLOUD --region ap-south-1
```
![image](https://github.com/user-attachments/assets/5d9cde6d-9f6f-48e6-a3a1-b9e77bf6fb9b)




## Step 4: Apply the Deployment and Service
1. **Change Directory Back to k8s-mario:**

```bash

cd ../k8s-mario
```

2. **Apply Deployment:**

```bash

kubectl apply -f deployment.yaml
```
3. **Check the Deployment:**

```bash
kubectl get all
```
![image](https://github.com/user-attachments/assets/497c6ac2-cee5-4503-b934-7ddc68db505b)


4. **Apply Service:**

```bash

kubectl apply -f service.yaml
kubectl get all
```

![image](https://github.com/user-attachments/assets/7cb23f35-9bdc-4668-8285-cd7d3975ebe1)


5. **Describe Service and Get the LoadBalancer Ingress:**

```bash

kubectl describe service mario-service
```
![image](https://github.com/user-attachments/assets/693c6c20-9a38-4574-aac2-b61ada6bcafc)


Access the Game: Paste the LoadBalancer Ingress URL in your browser to play the Super Mario game!


Let’s Go back to 1985 and play the game like children.


![image](https://github.com/user-attachments/assets/0dec59b3-d6f4-46dc-a929-8481318203f8)

You can check in Docker-hub as well chiragg619/mario:latest


## Destruction
**Delete the Service and Deployment:**

```bash

kubectl get all
kubectl delete service mario-service
kubectl delete deployment mario-deployment
```

**Destroy the Cluster:**

```bash

terraform destroy --auto-approve
```
![image](https://github.com/user-attachments/assets/701183ad-dc3c-43bc-9a22-41747cbf35e4)

Resources will be removed in 10 minutes.

![image](https://github.com/user-attachments/assets/f58f3607-cd07-4856-a27b-d4b967d9eda6)


## Docker Image
This project uses the official Docker image chiragg619/mario:latest, available on Docker Hub.

## Conclusion
Thank you for joining this nostalgic journey to the 90s! We hope you enjoyed rekindling your love for gaming with the deployment of the iconic Mario game on Kubernetes. Embracing the past while exploring new technologies is a true testament to the timeless allure of classic games. Until next time!


## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Additional Resources
- Kubernetes Documentation
- AWS EKS Documentation
- Docker Hub: chiragg619/mario
- Terraform Documentation
- AWS CLI Documentation

## Contact
For any questions, suggestions, or contributions, feel free to contact:

- GitHub: @chiragpuniyanii
- Email: chiragpuniyani7@gmail.com



### Key Update:
- Changed the Docker image reference to `chiragg619/mario:latest` as provided.

This should now accurately reflect the correct Docker image and make the README complete!





