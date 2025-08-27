# 🚀 Ansible Node.js Deployment

This project automates the deployment of a **Node.js application with Nginx** on AWS EC2 instances using **Ansible**.  
It supports **rolling deployments** (one server at a time) for zero downtime.


---
````markdown
## 📂 Project Structure

ansible-proj/
├── hosts.yml                # Ansible inventory file (with node1, node2)
├── deploy.yml               # Main Ansible playbook
├── files/
│   └── index.js             # Sample Node.js app
├── templates/
│   └── nginx.conf.j2        # Nginx configuration template
├── .gitignore               # Ignored files (keys, env, logs, etc.)
└── README.md                # Project documentation
````

---

## 🔑 Requirements

* AWS EC2 instances (Amazon Linux 2 or Amazon Linux AMI)
* SSH key (`practice.pem`) with access to the instances
* Ansible installed on the control node (your workstation or a separate EC2)

---

## ⚙️ Setup

1. **Copy your SSH key to the control node**

   On your **local machine**:

   ```bash
   scp -i ~/Downloads/practice.pem ~/Downloads/practice.pem ec2-user@<CONTROL_NODE_IP>:/home/ec2-user/
   ```

   On your **control node (EC2)**:

   ```bash
   mv ~/practice.pem ~/.ssh/practice.pem
   chmod 400 ~/.ssh/practice.pem
   ```

2. **Edit `hosts.yml`** and add your application server IPs:

   ```yaml
   all:
     hosts:
       node1:
         ansible_host: <NODE1_PUBLIC_IP>
         ansible_user: ec2-user
         ansible_ssh_private_key_file: ~/.ssh/practice.pem
       node2:
         ansible_host: <NODE2_PUBLIC_IP>
         ansible_user: ec2-user
         ansible_ssh_private_key_file: ~/.ssh/practice.pem
   ```

---

## ▶️ Run Playbook

Run the deployment playbook:

```bash
ansible-playbook -i hosts.yml deploy.yml
```

---

## ✅ Verification

1. SSH into a node and check if the app is running with **PM2**:

   ```bash
   pm2 list
   ```

2. Check Nginx service:

   ```bash
   systemctl status nginx
   ```

3. Open the server public IP in your browser:

   ```
   http://<NODE_PUBLIC_IP>
   ```

🎉 You should see your Node.js app response.

---

## 🔒 Security Notes

* Do **not commit private keys (`.pem`)** – `.gitignore` already ignores them.
* Use **Ansible Vault** for storing sensitive variables like DB credentials.
* Restrict SSH access in AWS Security Groups.

---

## 🛠 Future Enhancements

* Add CI/CD pipeline for auto-deployment
* Configure HTTPS with Let’s Encrypt
* Add monitoring/alerts with Prometheus & Grafana
