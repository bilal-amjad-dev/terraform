
13-Dec-2025.

Here is your **fixed, clean, and well-structured version of the notes**, with no extra noise and easy to understand:

---

### Quick Reference

| Variable      | Kya deta hai?                 | Kab use karte hain?                                                                        |
| ------------- | ----------------------------- | ------------------------------------------------------------------------------------------ |
| `path.module` | Current module ka folder path | Child modules mein files ya templates read karne ke liye (**99% cases mein yahi chahiye**) |

---

```
my-project/
│
├─ main.tf
└─ modules/
   └─ ec2/
      └─ main.tf
```

---

### Example: `my-project/main.tf` (Root Module)

```hcl
output "where_am_i" {
  value = path.module
}
```

**Output:**

```
/my-project
```

---

### Example: `my-project/modules/ec2/main.tf` (EC2 Child Module)

```hcl
output "where_am_i" {
  value = path.module
}
```

**Output:**

```
/my-project/modules/ec2
```

---

### Real Example with a Script

```
my-project/
│
├─ main.tf              ← Root module
└─ modules/
   └─ ec2/
      ├─ main.tf        ← EC2 module
      └─ script.sh
```

📍 `script.sh` is located **inside the EC2 module folder**.

To use `script.sh`, write this **inside `modules/ec2/main.tf`**:

📍 `script.sh` is **inside the ec2 folder**

So the rule is:

> **The file that uses `script.sh` must be in the SAME folder (module).**

---


```hcl
user_data = file("${path.module}/script.sh")
```

---

### Simple rule to remember

> **Script jahan ho, `path.module` wahi use karo.**

That’s it — your notes are now clean, correct, and beginner-friendly 👍


That’s all 😊

