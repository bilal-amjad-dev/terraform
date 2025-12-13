
13-Dec-2025.


Sure 🙂 I’ll explain it in the **simplest possible way**, step by step.

Imagine this folder structure:

```
my-project/
│
├─ main.tf
└─ modules/
   └─ ec2/
      └─ main.tf
```

### 1️⃣ What `path.module` means

`path.module` = **“the folder where THIS file is located”**

---

### 2️⃣ Example in the root module

`my-project/main.tf`

```hcl
output "where_am_i" {
  value = path.module
}
```

📌 Output:

```
/my-project
```

Why?
Because this file is in the **root folder**.

---

### 3️⃣ Example in a child module

`my-project/modules/ec2/main.tf`

```hcl
output "where_am_i" {
  value = path.module
}
```

📌 Output:

```
/my-project/modules/ec2
```

Why?
Because this file is inside the **ec2 module**.

---

### 4️⃣ Why is this useful? (real reason)

Suppose you have a file inside the module:

```
modules/ec2/
├─ main.tf
└─ script.sh
```

To read `script.sh` **from the module**, you do:

```hcl
file("${path.module}/script.sh")
```

✔ This always works
✔ Even if the module is reused somewhere else
✔ Industry standard

---

### 🧠 One-line rule (remember this)

> **`path.module` always points to the current module’s folder.**

That’s it. No complexity 😊
