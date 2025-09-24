


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
