
```bash
output "helloworld" {
value = "Hello World"
}

---

```bash
resource "aws_instance" "ec2_example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = t2.micro

  tags = {
    Name = "Terraform EC2"
  }
}

output "instance_id" {
  value = aws_instance.ec2_example.public_ip
}
```
