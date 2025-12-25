

25-December-2025

Copiot

## `resource "null_resource"`
- **What it does**: A “dummy” resource that doesn’t create anything in AWS. It’s just a placeholder to attach actions (like provisioners).  
- **Analogy**: Think of it as an empty box. You don’t care about the box itself, but you can tape instructions to it.  
- **Example use**: Use a `null_resource` to run a script (via `local-exec`) after another resource is created. For example, after building a VPC, run a script that updates your config files.

- `null_resource` → acts as a trigger step.  



Alright Bilal, let’s make **`null_resource`** super simple and clear for your notes 👌  

---

## 🔹 What is `null_resource`?
- It’s a **dummy resource** in Terraform.  
- It doesn’t create anything in AWS, Azure, or GCP.  
- Instead, it’s used as a **placeholder** to attach actions (like provisioners).  

Think of it like an **empty box**: the box itself doesn’t matter, but you can tape instructions to it.  

---

## 🔹 Why do we use it?
- To run commands or scripts when no “real” resource exists.  
- To orchestrate tasks (e.g., run a script after another resource is created).  
- To glue Terraform with external tools (like **Packer**, **Ansible**, or shell scripts).  

---

## 🔹 Example 1: Run a local command
```hcl
resource "null_resource" "build_ami" {
  provisioner "local-exec" {
    command = "packer build aws-ami.pkr.hcl"
  }
}
```
👉 Here, Terraform doesn’t create anything in AWS.  
It just runs `packer build` locally when you do `terraform apply`.

---

## 🔹 Example 2: Run a remote command on EC2
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
    type     = "ssh"
    user     = "ec2-user"
    host     = aws_instance.web.public_ip
    private_key = file("~/.ssh/id_rsa")
  }
}
```
👉 Here, `null_resource` connects to an EC2 instance and runs commands inside it.  

---

## 📖 Cheat Sheet
- **`null_resource`** = placeholder resource.  
- Use it when you need to run provisioners (`local-exec` or `remote-exec`) but don’t have a real resource.  
- Common in workflows like:  
  - Triggering **Packer builds**  
  - Running **scripts** after infra creation  
  - Orchestrating **CI/CD steps**  

---

Bilal, the key takeaway:  
> **`null_resource` doesn’t create infrastructure. It’s just a hook to run commands.**  

---


Exactly, Bilal ✅ — you’ve nailed the core idea.

- **`null_resource`** by itself does **not** create any infrastructure (no EC2, no S3, nothing in AWS).  
- It’s basically a **placeholder** or “dummy resource.”  
- Its main purpose is to give you a place to attach **provisioners** (like `local-exec` or `remote-exec`).  

So when you combine it with **`local-exec`**:

```hcl
resource "null_resource" "build_ami" {
  provisioner "local-exec" {
    command = "packer build aws-ami.pkr.hcl"
  }
}
```

👉 What happens:
- Terraform applies the `null_resource`.  
- Since it has no real cloud resource to create, it just runs the **local command** (`packer build`) on your laptop or CI/CD runner.  
- This is how you “glue” Terraform with external tools like Packer.

---

### 📖 Cheat Sheet
- **`null_resource`** → dummy resource, doesn’t create infra.  
- **Attach `local-exec`** → run commands locally (your workstation or CI/CD).  
- **Attach `remote-exec`** → run commands inside a cloud resource (like EC2 via SSH).  

---

So yes, your understanding is correct:  
> **`null_resource` is used not to create any resource, but to run commands (often via `local-exec`).**

Would you like me to also show you a **real workflow example** where `null_resource` + `local-exec` runs `packer build`, and then Terraform automatically uses the AMI ID to launch EC2? That would tie it directly to your AMI series.

