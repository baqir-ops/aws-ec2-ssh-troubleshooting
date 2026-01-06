# Case 001 – EC2 SSH Connectivity Troubleshooting

## 📌 Overview
This case study documents a real-world AWS Support Engineer–style investigation into an EC2 SSH connectivity failure. The issue was caused by a misconfigured security group that did not allow inbound SSH (port 22) traffic.

The goal was to identify the root cause, apply the correct fix, and restore secure access to the instance.

---

## 🧩 Problem Statement
An Amazon EC2 instance launched in a public subnet was unreachable via SSH.  
The user received repeated **connection timed out** errors when attempting to connect from a local machine.

---

## 🔍 Investigation & Troubleshooting Steps

### 1️⃣ Verify Instance State
- Confirmed the EC2 instance was in a **running** state.
- Verified the correct **public IPv4 address** was being used.

---

### 2️⃣ Validate SSH Key Usage
- Initial attempts failed due to incorrect key path:
Warning: Identity file not accessible

r
Copy code
- Resolved by using the **absolute path** to the `.pem` file:
```bash
ssh -i "C:\Users\Baryal Laptops\Downloads\baqir-ssh-key.pem" ec2-user@<PUBLIC_IP>
3️⃣ Check Network Connectivity
SSH attempts resulted in:

pgsql
Copy code
ssh: connect to host <IP> port 22: Connection timed out
This indicated a network-level issue rather than authentication.

4️⃣ Inspect Security Group Inbound Rules
Reviewed the attached security group.

Found that port 22 (SSH) was NOT allowed.

Only HTTP/HTTPS rules were present.

5️⃣ Apply Fix
Added a new inbound rule:

Type: SSH

Protocol: TCP

Port: 22

Source: My IP (for security)

6️⃣ Re-test SSH Access
Retried the SSH command:

bash
Copy code
ssh -i "C:\Users\Baryal Laptops\Downloads\baqir-ssh-key.pem" ec2-user@<PUBLIC_IP>
SSH connection succeeded.

Logged into Amazon Linux 2023 successfully.

🛠 Root Cause Analysis
The EC2 instance’s security group did not allow inbound SSH traffic on port 22, causing all connection attempts to time out.

✅ Resolution
Added the correct SSH inbound rule to the security group.

Verified successful remote access.

📚 Key Learnings
Security group misconfigurations are a common root cause of EC2 connectivity issues.

“Connection timed out” usually indicates network or firewall problems.

Always verify:

Instance state

Correct key pair

Security group rules

Source IP restrictions

🎯 Skills Demonstrated
EC2 troubleshooting

Security group analysis

SSH debugging

AWS networking fundamentals

Incident-style root cause analysis

🧹 Cleanup
The EC2 instance was terminated after validation to avoid unnecessary costs.

🏁 Conclusion
This case study demonstrates a structured, AWS Support Engineer–style approach to diagnosing and resolving EC2 SSH connectivity issues using methodical investigation and best practices.
