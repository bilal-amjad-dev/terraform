
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

Got it Bilal 👍 — let’s simplify that long command step by step so it’s easy to understand:

---
---
---
---
---
---


### Original Command
```bash
packer build -machine-readable aws-ami.pkr.hcl | awk -F, '/artifact,0,id/ {print $6}' > ami_id.txt
```

---

### 🔎 What Each Part Does
1. **`packer build -machine-readable aws-ami.pkr.hcl`**  
   → Runs Packer to build an AMI using the template `aws-ami.pkr.hcl`.  
   → The `-machine-readable` flag makes the output structured (CSV‑like).

2. **`| awk -F, '/artifact,0,id/ {print $6}'`**  
   → Filters the output to find the AMI ID line.  
   → `-F,` means “split by commas.”  
   → `/artifact,0,id/` finds the line with the AMI ID.  
   → `{print $6}` prints the 6th column (which is the AMI ID).

3. **`> ami_id.txt`**  
   → Saves the AMI ID into a file called `ami_id.txt`.

---

### 🧩 Super Simple Version
```bash
packer build aws-ami.pkr.hcl > ami_id.txt
```

👉 This simpler command just runs Packer and saves **all output** into a file.  
But if you only want the **AMI ID**, you need the longer version with `awk`.

---

✅ Takeaway for your notes:  
- **Long command** = extracts only the AMI ID and saves it.  
- **Short command** = saves all Packer output into a file.  

---

Bilal, would you like me to write a **beginner‑friendly version using Terraform output** (instead of awk) so you don’t have to parse text manually? That’s often cleaner for teaching.
