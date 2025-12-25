
25-December-2025

Copilot


---

# 📖 Notes on `local_file`

## 🔹 What is `local_file`?
- It is used to **create a file on your local machine** (where you run `terraform apply`).  
- **Shortcut definition:**  
  > `local_file` = Terraform writes a file on your laptop or CI/CD runner.  

---

## 🔹 Why Use It?
- To save outputs, IDs, or configs automatically instead of writing them manually.  
- To generate configuration files for other tools.  
- To keep logs or records of important values from Terraform runs.  

---

## 🧩 Example 1: Simple File
```hcl
resource "local_file" "example" {
  content  = "Hello Bilal!"
  filename = "${path.module}/hello.txt"
}
```

👉 After `terraform apply`, you’ll see a file called `hello.txt` in your project folder with the text:  
```
Hello Bilal!
```

---

## 🧩 Example 2: Workflow with Packer + Terraform
This shows how `null_resource` + `local-exec` + `local_file` work together to build an AMI and save its ID.

```hcl
provider "aws" {
  region = "ap-south-1"
}

# Step 1: Run Packer locally
resource "null_resource" "build_ami" {
  provisioner "local-exec" {
    command = "packer build -machine-readable aws-ami.pkr.hcl | awk -F, '/artifact,0,id/ {print $6}' > ami_id.txt"
  }
}

# Step 2: Save AMI ID into a local file
resource "local_file" "ami_id" {
  content  = file("ami_id.txt")
  filename = "${path.module}/ami_id_output.txt"
}

# Step 3: Use AMI ID to launch EC2
resource "aws_instance" "web" {
  ami           = local_file.ami_id.content
  instance_type = "t2.micro"

  tags = {
    Name = "Bilal-Packer-Terraform-Instance"
  }
}
```

---

## 🔎 Explanation
- **`null_resource` + `local-exec`** → runs `packer build` locally (on your laptop or CI/CD runner).  
- **`local_file`** → saves the AMI ID into a text file (`ami_id_output.txt`).  
- **`aws_instance`** → reads the AMI ID from that file and launches an EC2 instance with it.  

---

## ✅ Super Simple Takeaway
> **`null_resource` runs commands, `local-exec` runs them locally, and `local_file` saves outputs into a file.**

---

Bilal, this polished version is now structured like a **ready-to-use study note**. Would you like me to also create a **diagram workflow** (Terraform → null_resource → local-exec → Packer → AMI → local_file → EC2) so you can visualize this chain quickly?
