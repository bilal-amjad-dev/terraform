

25-December-2025

## `resource "null_resource"`
- **What it does**: A “dummy” resource that doesn’t create anything in AWS. It’s just a placeholder to attach actions (like provisioners).  
- **Analogy**: Think of it as an empty box. You don’t care about the box itself, but you can tape instructions to it.  
- **Example use**: Use a `null_resource` to run a script (via `local-exec`) after another resource is created. For example, after building a VPC, run a script that updates your config files.

- `null_resource` → acts as a trigger step.  



