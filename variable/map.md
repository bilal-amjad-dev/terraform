


### map consists of key-value pairs


```bash
variable "project_environment" {
  description = "project name and environment"
  type = map(string)
  default = {
    project = "project-alpha"
    environment = "dev"
  }
}
```

here:

- **key** is project and environment
- **value** is project-alpha and dev

---

18-December-2025

# Terraform Map Variable - Perfect Notes

## **Map = Key-Value Pairs** (like a dictionary)

```
Key    → Value
project → "project-alpha"
environment → "dev"
```

## **Your Example (Cleaned Up)**

```hcl
variable "project_environment" {
  description = "Project name and environment settings"
  type        = map(string)
  default     = {
    project     = "project-alpha"
    environment = "dev"
  }
}
```

## **How to Use It**
```hcl
# Access specific values
resource "aws_instance" "web" {
  tags = {
    Name        = var.project_environment["project"]     # = "project-alpha"
    Environment = var.project_environment["environment"] # = "dev"
  }
}
```

## **Quick Rules**
- **Key** = Always string (`project`, `environment`)
- **Value** = Any type (string, number, list)
- **Access** = `var.map_name["key"]`
- **One map** = Multiple related settings together

**Perfect for:** Tags, regions, environments, configs in one place!

**Your notes format is excellent - crisp and clear!** 👍


---

Tag?

What it is: A Key-Value Pair

In AWS or Azure, a tag isn't just a word; it is always a Key and a Value.

Key: Environment

Value: Dev

In Terraform, a map(string) is just a list of these pairs.

Why we do it this way (The Logic)

1. The "Default ={}" (The Empty Box)

When you see default = {}, it means the "box" is empty. If you don't provide any tags, Terraform won't crash; it will just apply zero tags. It’s like saying, "I have a label maker, but I haven't typed anything yet."

---

Copilot:

Perfect Bilal 🙌 — here’s a **structured set of notes on Terraform maps** that you can maintain in your docs. I’ll keep them simple, clear, and example‑driven so they’re easy to revisit later.

---

# 📘 Notes on Terraform Map

## 1. What is a Map?
- A **map** is a collection of **key → value pairs**.  
- In Terraform, `map(string)` means both the key and the value are text.  
- Think of it like a **dictionary** or a **shopping bag with labeled items**.

---

## 2. Why Use a Map?
- Cloud resources often need **tags** or settings in key/value format.  
- A map makes it easy to group them together and pass them around.  
- Instead of writing each tag separately, you define them once and reuse.

---

## 3. Example: Tags Map
```hcl
variable "tags" {
  type        = map(string)
  description = "Tags to apply to resources"
  default     = {}
}

tags = {
  Environment = "Development"
  Team_Name   = "Engineering"
  Cost_Center = "IT"
}
```

---

## 4. How Terraform Applies It
```hcl
resource "azurerm_windows_virtual_machine" "example" {
  name     = "demo-vm"
  location = "East US 2"

  tags = var.tags
}
```

👉 Terraform attaches **all tags** from the map to the VM:
- Environment = Development  
- Team_Name = Engineering  
- Cost_Center = IT  

---

## 5. Benefits of Using Map
- **Consistency** → Same tags applied across multiple resources.  
- **Flexibility** → Different tfvars files can override tags per environment.  
- **Organization** → Helps track cost, ownership, and environment.  
- **Governance** → Policies can enforce required tags.

---

## 6. Environment Example
```hcl
# dev.tfvars
tags = {
  Environment = "Development"
  Owner       = "Bilal"
}

# prod.tfvars
tags = {
  Environment = "Production"
  Owner       = "OpsTeam"
}
```

👉 Same Terraform code, but tags change depending on which tfvars file you use.

---

## 7. Default Value
```hcl
default = {}
```
- Means “empty map.”  
- If no tags are provided, Terraform won’t apply any.  
- Safe fallback to avoid errors.

---

## 8. Analogy
- **Phone contacts**: Name = Key, Number = Value.  
- **Labels on folders**: Label = Key, Text = Value.  
- **Tags in Terraform**: Key = Tag name, Value = Tag value.

---

## ✅ Crux
- A map is a **dictionary of text pairs**.  
- Perfect for tags because tags are always **key → value**.  
- You load them from `.tfvars` files and apply them with `tags = var.tags`.  
- Keeps your infrastructure **organized, reusable, and safe**.

---

Bilal, would you like me to also add a **section on advanced map tricks** (like merging maps or using `lookup()` to fetch a single tag) so your notes cover both beginner and intermediate use cases?
