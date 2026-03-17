### Stateful-vs-Stateless-Firewalls
Understanding Stateful vs Stateless Firewalls in AWS
### Overview

This lab demonstrates the difference between stateful and stateless firewalls in AWS using:

Security Groups (Stateful)

Network ACLs (Stateless)

You will build a custom VPC, deploy an EC2 instance, and analyze how traffic is controlled at both layers.
## Architecture 
          Internet
              |
      +-----------------+
      | Internet Gateway|
      +--------+--------+
               |
        +-------------+
        |   Public    |
        |   Subnet    |
        +------+------+ 
               |
        +-------------+
        |    EC2      |
        +-------------+
               |
     -----------------------
     |                     |
Security Group        Network ACL
(Stateful)           (Stateless)



---

## ☁️ Key AWS Services

### Amazon VPC
A logically isolated network where you control IP ranges, routing, and security.

### Amazon EC2
Virtual server used to simulate real-world traffic and test firewall behavior.

### Security Groups (Stateful Firewall)
- Tracks connection state
- Automatically allows return traffic
- Works at **instance level**

**Example:**
If inbound SSH (port 22) is allowed → return traffic is automatically allowed.

---

### Network ACLs (Stateless Firewall)
- Does NOT track connection state
- Requires explicit inbound AND outbound rules
- Works at **subnet level**

**Example:**
If inbound SSH is allowed → outbound must also be explicitly allowed.

---
#### Key Differences
| Feature        | Security Group | Network ACL       |
| -------------- | -------------- | ----------------- |
| State          | Stateful       | Stateless         |
| Level          | Instance       | Subnet            |
| Rules          | Allow only     | Allow & Deny      |
| Return Traffic | Automatic      | Manual            |
| Evaluation     | All rules      | Rule number order |


---

## Lab Implementation

### Step 1 — Create VPC
- Create custom VPC
- Define CIDR block

### Step 2 — Create Public Subnet
- Associate with VPC
- Enable auto-assign public IP

### Step 3 — Internet Connectivity
- Attach Internet Gateway
- Create Route Table
- Add route:
0.0.0.0/0 → Internet Gateway

---

### Step 4 — Security Group (Stateful)

**Inbound Rules:**
SSH (22) → Your IP
HTTP (80) → Anywhere

**Outbound Rules:**
Allow all traffic (default)

---

### Step 5 — Launch EC2 Instance
- Attach Security Group
- Place in Public Subnet

---

### Step 6 — Network ACL (Stateless)

**Inbound Rules:**
ALLOW 22 (SSH)
ALLOW 80 (HTTP)
ALLOW 1024-65535 (Ephemeral Ports)

**Outbound Rules:**
ALLOW 22
ALLOW 80
ALLOW 1024-65535


**Important:**  
Rules are evaluated in ascending order → first match wins.

---

### Step 7 — Testing

- SSH into EC2
- Test web access
- Modify NACL rules to:
  - Block outbound → observe failure
  - Remove ephemeral ports → observe broken communication

---

## Security Insight

This lab highlights a critical cloud security concept:

- **Security Groups = Primary defense (stateful, simple)**
- **NACLs = Secondary defense (stateless, strict control)**

This layered approach supports:

- Defense-in-depth
- Network segmentation
- Zero Trust architecture

---

## Real-World Use Cases

- Blocking malicious IP ranges at subnet level
- Enforcing strict compliance policies
- Preventing lateral movement inside VPC
- Segmenting sensitive workloads

---

## Key Takeaways

- Stateful firewalls simplify management
- Stateless firewalls provide granular control
- Both are essential for secure cloud architecture
- Misconfigurations can break connectivity or expose systems

