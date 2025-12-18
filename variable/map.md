


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
