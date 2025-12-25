

25-December-2025

Copilot

> **Tip:** `local-exec` runs *where Terraform runs*, not inside the AWS resource itself.  


Runs locally (your laptop or CI/CD runner).it runs on your local workstation or CI/CD runner.

example:
```bash
resource "null_resource" "build_ami" {
  provisioner "local-exec" {
    command = "packer build aws-ami.pkr.hcl"
  }
}
```


---

Great, Bilal 👍 — let’s now contrast **`local-exec`** with **`remote-exec`** so you can see the difference clearly.

---

## 🔹 `local-exec`
- Runs **on your local machine or CI/CD runner** (where you run `terraform apply`).  
- Useful for triggering external tools like `packer build`, `ansible-playbook`, or printing outputs.  
- Example:

```hcl
resource "null_resource" "build_ami" {
  provisioner "local-exec" {
    command = "packer build aws-ami.pkr.hcl"
  }
}
```

👉 Here, Terraform runs `packer build` **on your laptop or pipeline runner**, not inside AWS.

---

## 🔹 `remote-exec`
- Runs **inside the cloud resource itself** (like an EC2 instance).  
- Useful for configuring servers after they’re created (installing packages, running scripts).  
- Example:

```hcl
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  provisioner "remote-exec" {
    inline = [
      "sudo yum update -y",
      "sudo yum install -y httpd",
      "sudo systemctl start httpd"
    ]
  }
}
```

👉 Here, Terraform connects via SSH to the EC2 instance and runs commands **inside the server**.

---

## 📖 Cheat Sheet

- **`local-exec`** → Runs commands **where Terraform runs** (your laptop or CI/CD).  
- **`remote-exec`** → Runs commands **inside the resource** (e.g., inside EC2 via SSH).  

---


