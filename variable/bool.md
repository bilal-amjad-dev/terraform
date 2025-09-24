```bash
resource "aws_instance" "ec2_example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  count         = 1
  associate_public_ip_address = var.enable_public_ip
  
  tags = {
    Name = "Terraform EC2"
  }
}



variable "enable_public_ip" {
  description = "Enable public IP address"
  type        = bool
  default     = true  
}
```
