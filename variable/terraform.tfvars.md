
`main.tf`

```bash
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "ec2_example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type

  tags = {
    Name = "Terraform EC2"
  }
}
```


`variable.tf`

We are not going to store any value, we just going to define a variable over here.

```bash
variable "instance_type" {
}
```


`terraform.tfvars`

```bash
default     = "t2.micro"
```


