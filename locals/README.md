
```bash
locals {
  instance_name = "my-ec2-instance"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = local.instance_name
  }
}
```
---

18-December-2025

Claude

OK! Let me make it **super simple**.



## `locals` = **Nickname** or **Shortcut**

Think of `locals` like giving a **nickname** to something long, so you don't have to type it again and again.

`locals` = **Give a name to something, then use it many times**


---

## Example 1: Without locals (repeating)

```hcl
resource "aws_s3_bucket" "bucket1" {
  bucket = "my-company-dev-data"
}

resource "aws_s3_bucket" "bucket2" {
  bucket = "my-company-dev-logs"
}

resource "aws_s3_bucket" "bucket3" {
  bucket = "my-company-dev-backups"
}
```

You typed `"my-company-dev"` **three times**! 😫

---

## Example 2: With locals (shortcut)

```hcl
locals {
  prefix = "my-company-dev"
}

resource "aws_s3_bucket" "bucket1" {
  bucket = "${local.prefix}-data"
}

resource "aws_s3_bucket" "bucket2" {
  bucket = "${local.prefix}-logs"
}

resource "aws_s3_bucket" "bucket3" {
  bucket = "${local.prefix}-backups"
}
```

Now you only typed `"my-company-dev"` **once**! 😊

---

## Real Life Example:

**Without locals:**
```hcl
name = "mstv-backend-ingestion"
tag  = "mstv-backend-ingestion"
id   = "mstv-backend-ingestion"
```
Typing the same thing 3 times! 😩

**With locals:**
```hcl
locals {
  app_name = "mstv-backend-ingestion"
}

name = local.app_name
tag  = local.app_name
id   = local.app_name
```
Type once, use many times! 😎

---

## Summary:

`locals` = **Give a name to something, then use it many times**

Like:
- Nickname for a person
- Shortcut on your desktop
- Contact name in your phone

**That's it!** 

Does this make sense now? 🤔


---

Copilot:

Perfect Bilal 👌 — let’s make **Terraform locals** super simple.

---

## 🛠️ What are `locals` in Terraform?
- `locals` are like **shortcuts or reusable variables** inside your Terraform code.  
- They let you define a value once and then use it many times, without repeating yourself.  
- Unlike input `variables`, locals are **internal only** — you don’t pass them from outside, they live inside your `.tf` files.

---

## 🔍 Example
```hcl
locals {
  project_name = "BilalLabs"
  location     = "East US"
  vm_size      = "Standard_DS2_v2"
}

resource "azurerm_resource_group" "rg" {
  name     = "${local.project_name}-rg"
  location = local.location
}

resource "azurerm_virtual_machine" "vm" {
  name     = "${local.project_name}-vm"
  location = local.location
  vm_size  = local.vm_size
}
```

👉 What happens:
- You define `locals` once at the top.  
- Then you reuse them in multiple resources.  
- If you change `project_name` in locals, it updates everywhere automatically.

---

## 🧩 Why Use Locals?
- **Avoid repetition** → write once, use everywhere.  
- **Consistency** → same value across resources.  
- **Readability** → makes code cleaner and easier to maintain.  

---

## ✅ Crux
- **variables** = values you pass in from outside (like `terraform.tfvars`).  
- **locals** = internal shortcuts you define inside your Terraform code.  

---

👉 Analogy:  
- **Variables** = ingredients you bring from outside into the kitchen.  
- **Locals** = nicknames you give inside the recipe to avoid writing “olive oil” ten times — you just say `oil`.

---

Bilal, would you like me to show you a **real lab snippet** where locals are used to simplify your **SIG + Packer workflow** (like reusing gallery name, location, and image definition across resources)? That way you’ll see locals in action in your exact use case.
