# 🚀 AWS EC2 Web Server using CloudFormation (CFT)

> This is my **Day 1 hands-on project** as a complete beginner learning AWS and DevOps from scratch.  
> I created an EC2 instance on AWS using **Infrastructure as Code (CloudFormation)** — no manual clicking in the console.

---

## 📌 What This Project Does

- Creates a **VPC** (Virtual Private Cloud) — my own private network on AWS
- Creates a **Public Subnet** inside the VPC
- Creates a **Security Group** — acts as a firewall (allows SSH on port 22, HTTP on port 80)
- Launches an **EC2 Instance** (virtual server) inside the subnet
- All of this happens **automatically** from a single YAML template file

---

## 🏗️ Architecture

```
CloudFormation Template (.yaml)
            │
            ▼
    ┌───────────────────────────────┐
    │         AWS VPC               │
    │   (my private network)        │
    │                               │
    │   ┌───────────────────────┐   │
    │   │    Public Subnet      │   │
    │   │                       │   │
    │   │   ┌───────────────┐   │   │
    │   │   │  EC2 Instance │   │   │
    │   │   │  (Web Server) │   │   │
    │   │   └───────────────┘   │   │
    │   │          │            │   │
    │   │   Security Group      │   │
    │   │   Port 22  (SSH)      │   │
    │   │   Port 80  (HTTP)     │   │
    │   └───────────────────────┘   │
    └───────────────────────────────┘
```

---

## 🛠️ AWS Services Used

| Service | Purpose |
|---------|---------|
| **CloudFormation (CFT)** | Write infrastructure as code using YAML |
| **EC2** | Virtual server to host the web application |
| **VPC** | Private isolated network on AWS |
| **Subnet** | A smaller network inside the VPC |
| **Security Group** | Firewall — controls what traffic is allowed |
| **Internet Gateway** | Connects the VPC to the internet |
| **Route Table** | Directs traffic from subnet to internet |

---

## 📄 Files in This Repo

```
aws-ec2-webserver-cft/
│
├── cloudformation/
│   ├── ec2-basic.yaml              # Simple EC2 with existing subnet & SG
│   └── ec2-with-vpc.yaml          # Full stack — VPC + Subnet + SG + EC2
│
└── README.md                       # This file
```

---

## 🚀 How to Deploy

### Prerequisites
- An AWS account (free tier works)
- An existing **EC2 Key Pair** in your AWS account
- AWS region: `ap-south-1` (Mumbai) — change AMI ID if using another region

### Step 1 — Clone this repo
```bash
git clone https://github.com/tejapadigala/aws-ec2-webserver-cft.git
cd aws-ec2-webserver-cft
```

### Step 2 — Deploy via AWS Console
1. Go to **AWS Console → CloudFormation → Create Stack**
2. Choose **"With new resources (standard)"**
3. Select **"Upload a template file"**
4. Upload `cloudformation/ec2-with-vpc.yaml`
5. Click **Next**
6. Fill in the Parameters:
   ```
   Stack Name     → my-ec2-stack
   InstanceType   → t2.micro
   KeyName        → (select your existing key pair)
   Environment    → dev
   ```
7. Click **Next → Next → Create Stack**
8. Watch the **Events tab** — resources are created automatically
9. Once done, check the **Outputs tab** for your EC2 Public IP

### Step 3 — Verify EC2 is Running
- Go to **EC2 Console → Instances**
- Instance State → 🟢 Running
- Status Checks → 2/2 checks passed

### Step 4 — Clean Up (Important!)
To avoid AWS charges, delete the stack when done:
> CloudFormation → Select your stack → **Delete** → Confirm

Deleting the stack automatically removes **all resources** created by it.

---

## 💡 Key Concepts I Learned Today

### What is CloudFormation?
Instead of manually clicking in the AWS Console to create servers,  
CloudFormation lets you **write your infrastructure as a YAML file**.  
AWS reads the file and creates everything automatically — same way, every time.

### What is a VPC?
A VPC is your **own private network inside AWS**.  
Think of AWS as a large apartment building — your VPC is your private apartment.  
Other AWS customers cannot see or access your resources.

### What is a Subnet?
A subnet is a **smaller network carved out of your VPC**.  
A Public Subnet has internet access. A Private Subnet does not.  
EC2 web servers go in Public Subnets. Databases go in Private Subnets.

### What is a Security Group?
A Security Group is a **firewall for your EC2 instance**.  
It controls which ports and IP addresses can connect to your server.  
- Port `22` → SSH (to log into the server)
- Port `80` → HTTP (for website traffic)

### CFT Functions I Used
| Function | What it does |
|----------|-------------|
| `!Ref` | Gets the value of a parameter or resource |
| `!Sub` | Substitutes a variable into a string |
| `!GetAtt` | Gets a specific attribute of a resource |

---

## ❗ Errors I Faced & How I Fixed Them

### Error 1 — `No subnets found for the default VPC`
**Cause:** I hardcoded a subnet ID that no longer existed in my account.  
**Fix:** Used `AWS::EC2::Subnet::Id` parameter type so AWS shows a dropdown  
of real existing subnets, or created the subnet inside the CFT template itself.

### Error 2 — VS Code showing `Unresolved tag: !Ref`
**Cause:** VS Code's CloudFormation extension cannot connect to AWS,  
so it cannot verify what `!Ref` will resolve to at deploy time.  
**Fix:** This is just a VS Code warning — not a real error.  
The template deploys and works perfectly fine on AWS.

---

## 🧠 What I Learned

- Infrastructure as Code (IaC) is a core DevOps practice
- CloudFormation creates resources in the correct order automatically by reading `!Ref` links
- Never hardcode subnet or security group IDs — use parameters instead
- Always delete your CloudFormation stack after practice to avoid charges
- VS Code warnings ≠ AWS errors — always test by actually deploying

---



---

*Built with 💪 on Day 1 of my AWS DevOps journey*
