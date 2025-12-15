
15-Dec-2025.

2 Public Subnets using smartly:

we do countindex, it understand that it has used list 

> Yani variable ki type list honi chahia and count = length hona chahia
- type = list
- count = length()
- [count.index[



`main.tf`

```bash
# Smart Public Subnets (creates exactly 2)
resource "aws_subnet" "public" {
  count = length(var.public_subnet_cidrs)

  vpc_id                  = aws_vpc.main.id
  cidr_block              = var.public_subnet_cidrs[count.index]
  availability_zone       = var.availability_zones[count.index]
  map_public_ip_on_launch = true

  tags = {
    Name = "${var.project_name}-public-subnet-${count.index + 1}"
    Type = "Public"
  }
}
```

`variables.tf`

```bash
variable "public_subnet_cidrs" {
  default = ["10.0.1.0/24", "10.0.2.0/24"]
}

variable "availability_zones" {
  default = ["ap-south-1a", "ap-south-1b"]
}

variable "project_name" {
  default = "main"
}
```

---


```bash
variable "bucket_names" {
  description = "A list of S3 bucket names"
  type        = list(string)
  default     = ["bucket-one", "bucket-two", "bucket-three"]
}

resource "aws_s3_bucket" "example" {
  # The length of the list determines how many buckets to create (3 in this case)
  count = length(var.bucket_names)

  # count.index is a special variable (0, 1, 2) used to access the list items
  bucket = var.bucket_names[count.index]
}

```



