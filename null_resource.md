
---

# Notes on `null_resource`

**Date:** 25‑December‑2025  
**Author:** Copilot  

---

## 🔹 What is `null_resource`?
- It doesn’t create anything in AWS, Azure, or GCP.  
- **`null_resource`** is a dummy resource in Terraform.  
- Its purpose is to run commands using provisioners (`local-exec` or `remote-exec`).  

---

## 🔹 How It Works
- **With `local-exec`** → runs commands locally (on your laptop or CI/CD runner).  
- **With `remote-exec`** → runs commands inside a cloud resource (e.g., after creating an EC2 instance).  

---

## 🧩 Example 1: Run a Local Command
```hcl
resource "null_resource" "build_ami" {
  provisioner "local-exec" {
    command = "packer build aws-ami.pkr.hcl"
  }
}
```
👉 Here, Terraform applies the `null_resource` and runs `packer build` **on your local machine or CI/CD runner**.

---

## 🧩 Example 2: Run a Remote Command on EC2
```hcl
resource "null_resource" "configure_server" {
  provisioner "remote-exec" {
    inline = [
      "sudo yum update -y",
      "sudo yum install -y httpd",
      "sudo systemctl start httpd"
    ]
  }

  connection {
    type        = "ssh"
    user        = "ec2-user"
    host        = aws_instance.web.public_ip
    private_key = file("~/.ssh/id_rsa")
  }
}
```
👉 Here, `null_resource` connects to an EC2 instance via SSH and runs commands **inside the server**.

---

## 🔎 What Happens
- Terraform applies the `null_resource`.  
- Since it has no real cloud resource to create, it simply executes the attached provisioner.  
- With `local-exec`, commands run locally.  
- With `remote-exec`, commands run inside the target resource.  
- This makes `null_resource` a powerful way to **glue Terraform with external tools** like Packer or Ansible.  

---

## 📖 Cheat Sheet
- **`null_resource`** → dummy resource, doesn’t create infra.  
- **Attach `local-exec`** → run commands locally (workstation or CI/CD).  
- **Attach `remote-exec`** → run commands inside a cloud resource (like EC2 via SSH).  

---

✅ Your understanding is correct:  
> **`null_resource` is not used to create infrastructure, but to run commands — most often via `local-exec`.**

---

Bilal, this polished version is now structured like a **study note** you can reuse in your series or personal knowledge base. Would you like me to also add a **diagram workflow** (Terraform → null_resource → local-exec → Packer → AMI → EC2) so you can visualize it quickly?
