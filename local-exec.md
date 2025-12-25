

25-December-2025

Copilot


### `provisioner "local-exec"`**
- **Purpose**: Runs a command *locally* on the machine where you execute `terraform apply`.  
- **Use cases**:
  - Triggering external scripts or tools (e.g., running `packer build` after Terraform creates a resource).  
  - Sending notifications (e.g., curl to a webhook).  
  - Running CLI commands that aren’t directly supported by Terraform providers.  
- **Key point**: It doesn’t run inside the cloud resource — it runs on *your local workstation or CI/CD runner*.  


---


Exactly right, Bilal 👌 — you’ve got the intuition: **`local-exec` runs commands on the same machine where you run `terraform apply`**. That could be your **laptop**, or a **CI/CD runner** (like GitLab CI, GitHub Actions, Jenkins, etc.).

Let’s break it down super simple:

---

## 🔹 What is `local-exec`?

- Terraform normally creates cloud resources (EC2, S3, etc.).  
- Sometimes you also want to run a **local command** right after a resource is created.  
- That’s where `local-exec` comes in.  
- It’s like saying: *“Terraform, after you finish this step, please run this command on my computer.”*

---

## 🔹 Simple Example

Imagine you create an EC2 instance, and after it’s ready you want to **print its public IP** into your terminal.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  provisioner "local-exec" {
    command = "echo The instance IP is ${self.public_ip}"
  }
}
```

### 👉 What happens:
1. Terraform creates the EC2 instance.  
2. After it’s done, Terraform runs the local command:  
   ```
   echo The instance IP is 54.210.123.45
   ```
3. You see the IP printed in your terminal.  

---

## 🔹 Another Example (CI/CD)

In GitLab CI, you might want Terraform to **trigger a Packer build** after creating a resource:

```hcl
resource "null_resource" "build_ami" {
  provisioner "local-exec" {
    command = "packer build aws-ami.pkr.hcl"
  }
}
```

Here:
- Terraform doesn’t create a real resource (we use `null_resource` as a placeholder).  
- But it runs `packer build` locally — on your laptop or CI/CD runner.  

---

## 📖 Notes (Cheat Sheet)

- Runs **locally** (your laptop or CI/CD runner).  
- Useful for:  
  - Triggering scripts (`bash`, `python`, `packer build`)  
  - Printing outputs  
  - Sending notifications (curl, echo, etc.)  
- Not for cloud actions — it’s purely local.  

---

Bilal, would you like me to also show you a **step-by-step mini workflow** where Terraform uses `local-exec` to run `packer build` and then launches an EC2 with that AMI? That would tie it directly to your series.
