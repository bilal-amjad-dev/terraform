`main.tf`

```bash
resource "aws_instance" "ec2_example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type

  tags = {
    Name = "Terraform EC2"
  }
}
```


`variable.tf`
```bash
variable "instance_type" {
}
```

---

```bash
terraform plan -var="instance_type=t2.micro"
```

```bash
terraform apply -var="instance_type=t2.micro"
```
