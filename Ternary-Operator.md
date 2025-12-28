
28-December-2025

Great question Bilal 👌 — let’s clear up the confusion.

### Why a Default AMI Is Used
The line in your Terraform code:

```hcl
ami = var.ami_id != "" ? var.ami_id : "ami-00ca570c1b6d79f36"
```

is basically a **safety net**.  

- **Goal**: You want to use your **custom AMI** (built with Packer).  
- **Problem**: When you first run `terraform plan`, your custom AMI might not exist yet (because the pipeline hasn’t built it).  
- **Solution**: Terraform uses a **default AMI** so the plan can still run without errors.  

👉 Once your GitLab CI/CD pipeline runs the **Packer stage**, it creates the custom AMI and passes its ID into `var.ami_id`. At that point, Terraform will use your **custom image** instead of the default.

---

### Simple Analogy
Think of it like booking a hotel room:
- If your **custom room** (AMI) is ready, you stay there.  
- If not, you temporarily get a **default room** so your booking doesn’t fail.  

---

### Workflow in Your Lab
1. **Packer builds custom AMI** → outputs AMI ID.  
2. **Terraform pipeline runs** → picks up that AMI ID (`var.ami_id`).  
3. **If AMI ID exists** → Terraform uses the custom AMI.  
4. **If AMI ID doesn’t exist yet** → Terraform falls back to the default AMI so the plan stage doesn’t break.  

---

✅ So the default AMI is **only a placeholder**. In practice, once your custom AMI is built, Terraform will always use that.

Bilal, do you want me to show you how to **connect Packer’s AMI output directly into Terraform variables** so the pipeline automatically switches from default → custom without manual edits? That’s the real industry trick.
