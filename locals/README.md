
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
