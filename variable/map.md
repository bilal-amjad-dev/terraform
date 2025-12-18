


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
