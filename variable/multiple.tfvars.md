 In industry it is common to use multiple .tfvars files to manage different environments (like dev, stage, prod). Each file contains variable values specific to that environment. For example:

- dev.tfvars
- stage.tfvars
- production.tfvars

This approach helps keep environment configurations separate, organized, and easy to manage.





`main.tf`

```bash
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "ec2_example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type

  tags = {
    Name = var.environment_name
  }
}
```

`variable.tf`


```bash
variable "instance_type" {
}

variable "environment_name" {
}


`stage.tfvars`

```bash
instance_type    = "t2.micro"
environment_name = "stage"
```


`production.tfvars`
```bash
instance_type    = "t2.micro"
environment_name = "production"
```

```bash
terraform plan -var-file="stage.tfvars"
terraform apply -var-file="stage.tfvars"
```

```bash
terraform plan -var-file="production.tfvars"
terraform apply -var-file="production.tfvars"
```

