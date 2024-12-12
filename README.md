# CloudFormation VPC with Public and Private Subnets

This project involves creating a VPC with 1 public subnet and 1 private subnet using AWS CloudFormation. It provisions EC2 instances in both subnets, installs Apache on the public EC2 instance, and installs Git on the private EC2 instance. The solution also demonstrates secure access to the private EC2 instance via the public EC2 instance (Bastion Host) using SSH-Agent in Windows OS.

## Steps

### 1. **Create CloudFormation Template**
- **Template Overview**:
  - Creates a VPC with two subnets:
    - **Public Subnet** with an EC2 instance running Apache and a public IP.
    - **Private Subnet** with a private EC2 instance running Git and accessed via a NAT Gateway.
- The CloudFormation stack was deployed using the template from this [GitHub repository](https://github.com/ArashMHD91/CloudFormation-Templates.git).

### 2. **Deploy the CloudFormation Stack**
- Deploy the CloudFormation stack using the AWS Management Console or AWS CLI with the template file.

### 3. **Secure SSH Access to Private EC2**
To securely access the private EC2 instance via the public EC2 instance (Bastion Host), follow these steps:

#### **Without SSH-Agent (Not Recommended for Security)**:
- **Copy the private key to the public EC2 instance**:
  ```bash
  scp -i "YourKeyPair.pem" YourKeyPair.pem ec2-user@<Public_IP>:~
  ```

- **SSH into the public EC2 instance**:
  ```bash
  ssh -i "YourKeyPair.pem" ec2-user@<Public_IP>
  ```

- **SSH into the private EC2 instance**:
  ```bash
  ssh -i "YourKeyPair.pem" ec2-user@<Private_IP>
  ```

- **Verify Git installation on the private EC2 instance**:
  ```bash
  git --version
  ```

#### **With SSH-Agent (Secure Method)**:
To avoid leaving the key pair on the EC2 instance, use **SSH-Agent** to securely access the private EC2 instance.

1. **Start the SSH-Agent**:
   ```bash
   ssh-agent
   ```

2. **Set up your environment**:
   ```bash
   eval "$(ssh-agent -s)"
   ```

3. **Add your private key to the SSH-Agent**:
   ```bash
   ssh-add YourKeyPair.pem
   ```

4. **SSH into the public EC2 instance with agent forwarding**:
   ```bash
   ssh -A ec2-user@<Public_IP>
   ```

5. **SSH into the private EC2 instance using agent forwarding**:
   ```bash
   ssh -A ec2-user@<Private_IP>
   ```

6. **Verify Git installation on the private EC2 instance**:
   ```bash
   git --version
   ```

---

### 4. **Outcome**
- The **public EC2 instance** is successfully running Apache and accessible via its public IP.
- The **private EC2 instance** is running Git and can be securely accessed from the public EC2 instance using SSH-Agent forwarding.

---

### Requirements:
- **AWS CLI** (if using the command line to deploy).
- **SSH Client** for Windows (e.g., Git Bash or Windows Subsystem for Linux) to interact with the EC2 instances.

---

### Conclusion
This solution ensures secure access to the private EC2 instance without compromising the key pair on the public EC2 instance. Using SSH-Agent forwarding enhances security by keeping the private key off the EC2 instances.
