```bash
resource "aws_iam_user" "example" {
    count = length(var.user_names)
    name  = var.user_names[count.index]
}

variable "user_names" {
  description = "List of user names"
  type        = list(string)
  default     = ["user1", "user2", "user3"]
}
```
