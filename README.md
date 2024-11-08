# Deploy Super Mario Game on AWS EKS

This project demonstrates how to deploy a Super Mario game on an AWS EKS (Elastic Kubernetes Service) cluster. Follow the instructions below to set up your environment and deploy the game.

## Prerequisites

Ensure the following are set up before starting:

- **An Ubuntu Instance**: Use an EC2 instance with Ubuntu as the operating system.
- **IAM Role**: Attach an IAM role to your EC2 instance with the required permissions to manage AWS resources.
- **Terraform**: Terraform should be installed on the instance for provisioning the EKS cluster.
- **AWS CLI and kubectl**: AWS CLI and kubectl should be installed on the instance for managing the AWS infrastructure and Kubernetes cluster.

## Let's Deploy

### STEP 1: Launch Ubuntu Instance

1. **Sign in to AWS Console**: Log in to your AWS Management Console.
2. **Navigate to EC2 Dashboard**: Go to the EC2 Dashboard by selecting “Services” in the top menu and then choosing “EC2” under the Compute section.
3. **Launch Instance**: Click on the “Launch Instance” button to start the instance creation process.
4. **Choose an Amazon Machine Image (AMI)**: Select an appropriate AMI for your instance, such as an Ubuntu image.
5. **Choose an Instance Type**: In the “Choose Instance Type” step, select `t2.micro` as your instance type. Proceed by clicking “Next: Configure Instance Details.”
6. **Configure Instance Details**:
   - Set “Number of Instances” to 1 (or as needed).
   - Configure additional settings like network, subnets, IAM role, etc., if necessary.
   - For “Storage,” click “Add New Volume” and set the size to 8GB (or modify the existing storage to 8GB).
   - Click “Next: Add Tags” when you’re done.
7. **Add Tags (Optional)**: Add any desired tags to your instance for easier management and organization.
8. **Configure Security Group**:
   - Ensure security group rules allow access as needed (e.g., SSH, HTTP).
9. **Review and Launch**:
   - Review the configuration details to ensure everything is correct.
10. **Select Key Pair**:
    - Select “Choose an existing key pair” and choose the key pair from the dropdown.
    - Acknowledge that you have access to the selected private key file.
11. **Launch Instances**: Click “Launch Instances” to create the instance.
12. **Access the EC2 Instance**:
    - Once launched, you can access the instance using SSH with the key pair and the instance’s public IP or DNS.
    - Ensure you follow best practices for security when configuring security groups and key pairs.

---

### STEP 2: Create IAM Role

1. **Search for IAM**: In the AWS Console, search for “IAM” and click on “Roles.”

![image](https://github.com/user-attachments/assets/e6a99503-10bb-480b-9b83-dde1ce3c1729)

2. **Create Role**:
 
![image](https://github.com/user-attachments/assets/c382c325-2d74-4fac-a419-c23c86f8c927)
   - Click on “Create Role.”
   - Select entity type as “AWS service.”
   - Choose “EC2” as the use case and click “Next.”

![image](https://github.com/user-attachments/assets/d417011b-3749-4a4c-b4f3-97585990595c)

3. **Select Permissions**:
   - For the permission policy, select **Administrator Access** (only for learning purposes).
   - Click “Next.”

![image](https://github.com/user-attachments/assets/8c3a44b5-74b1-451d-86cb-fd2538248f00)

4. **Name the Role**: Provide a name for the role and click “Create Role.”

![image](https://github.com/user-attachments/assets/e39d0eb7-e9da-4ec9-87c2-8e49601ff80d)

Role is Created

![image](https://github.com/user-attachments/assets/dafea52c-e3bc-4c7d-82a0-7c3509f6619a)

5. **Attach Role to EC2 Instance**:
   - Go to the EC2 Dashboard and select your instance.
   - Click on “Actions” → “Security” → “Modify IAM role.”

![image](https://github.com/user-attachments/assets/e508d73e-36e3-400d-a8e1-4833d9eb0f97)
   - Select the role you just created and click “Update IAM role.”

![image](https://github.com/user-attachments/assets/9f473a89-b180-4f96-8ff5-4ede3330b059)



6. **Connect to the Instance**: Connect to your EC2 instance using tools like MobaXterm or PuTTY.



## STEP 3: Cluster Provision

1. **Clone the Repository**:

       git clone https://github.com/chiragpuniyanii/Deploy-Super-Mario-Game-on-AWS-EKS.git

2. **Change Directory**:
   
       cd Deploy-Super-Mario-Game-on-AWS-EKS/

3.  **Provide Executable Permissions and Run Script**:

         sudo chmod +x script.sh
        ./script.sh
  ![image](https://github.com/user-attachments/assets/3a2cab73-458a-4e6f-8aaf-f227ddd80bd2)
 
  This script will install Terraform, AWS CLI, kubectl, and Docker.

4. **Check Versions**:

        docker --version
        aws --version
        kubectl version --client
        terraform --version

![image](https://github.com/user-attachments/assets/de5c1384-f64c-4abb-b29d-2e5ae1b0c0ae)

5. **Initialize Terraform:**
   -Change directory to EKS-TF:

        cd EKS-TF
   -Important: Don’t forget to change the S3 bucket name in the backend.tf file.

   -Run the following commands:

        terraform init
        terraform validate
        terraform plan

   ![image](https://github.com/user-attachments/assets/88940666-ac65-4cee-a590-539d6ddbf8b4)
   


6. **Apply Terraform Configuration:**
   
       terraform apply --auto-approve

   ![image](https://github.com/user-attachments/assets/a967c5fe-e656-4747-9119-190a07879712)


This step provisions the cluster and takes about 10 minutes to complete.

  ![image](https://github.com/user-attachments/assets/43755889-2c54-40e5-922b-0a10778bd02b)

 7. **Update Kubernetes Configuration**:

         aws eks update-kubeconfig --name EKS_CLOUD --region us-east-1
   ![image](https://github.com/user-attachments/assets/5256268b-ed00-4bf7-acdb-d37756d74109)

   **Make sure to replace us-east-1 with your desired region if necessary.**


 8. **Deploy the Application**:
    -Change back to the Deploy-Super-Mario-Game-on-AWS-EKS directory:
    
           cd Deploy-Super-Mario-Game-on-AWS-EKS/

    -Apply the deployment

            kubectl apply -f deployment.yaml
            kubectl get all  # to check the deployment status
    
    ![image](https://github.com/user-attachments/assets/273d7c61-1184-404c-8234-b8469888549f)


10. **Apply the Service:**

            kubectl apply -f service.yaml
            kubectl get all  # to verify service creation

    ![image](https://github.com/user-attachments/assets/af441144-c3fe-47a9-a53d-7fd2fbff6451)

    -Describe the service and copy the LoadBalancer Ingress:
    
            kubectl describe service mario-service

    -Paste the ingress link in a browser to access the Mario game.








## Destruction

1. **Delete the Service and Deployment:**
   
           kubectl delete service mario-service
           kubectl delete deployment mario-deployment

2. **Destroy the Cluster:**
   
           terraform destroy --auto-approve

   Resources provisioned will be removed in about 10 minutes.






