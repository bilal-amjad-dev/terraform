
18-December-2025

Copilot:

Exactly Bilal ✅ — you’ve got the right idea. Let me show you how **SIG + Packer + Terraform** can be wired together so that when you run `terraform apply`, it automatically triggers Packer to build the image, then publishes it into the **Shared Image Gallery (SIG)**.

---

## 🧩 Integration Pattern: Terraform + Packer

### 1. Packer Template (build golden image)
`packer.json`
```json
{
  "builders": [{
    "type": "azure-arm",
    "subscription_id": "<sub-id>",
    "client_id": "<app-id>",
    "client_secret": "<password>",
    "tenant_id": "<tenant-id>",

    "managed_image_resource_group_name": "rg-image-gallery",
    "managed_image_name": "myManagedImage",
    "managed_image_location": "East US",

    "os_type": "Windows",
    "image_publisher": "MicrosoftWindowsDesktop",
    "image_offer": "windows-11",
    "image_sku": "win11-22h2-pro"
  }],
  "provisioners": [{
    "type": "powershell",
    "inline": [
      "Write-Host 'Installing FSLogix...'",
      "choco install fslogix -y"
    ]
  }]
}
```

---

### 2. Terraform Code (trigger Packer + publish to SIG)

```hcl
provider "azurerm" {
  features {}
}

# Resource Group
resource "azurerm_resource_group" "rg" {
  name     = "rg-image-gallery"
  location = "East US"
}

# Run Packer automatically before SIG setup
resource "null_resource" "packer_build" {
  provisioner "local-exec" {
    command = "powershell ./scripts/build-image.ps1"
  }
}

# Shared Image Gallery
resource "azurerm_shared_image_gallery" "gallery" {
  name                = "myGallery"
  resource_group_name = azurerm_resource_group.rg.name
  location            = azurerm_resource_group.rg.location
  description         = "Golden Image Gallery for AVD"
}

# Shared Image Definition
resource "azurerm_shared_image" "image_def" {
  name                = "myGoldenImage"
  gallery_name        = azurerm_shared_image_gallery.gallery.name
  resource_group_name = azurerm_resource_group.rg.name
  location            = azurerm_resource_group.rg.location
  os_type             = "Windows"

  identifier {
    publisher = "BilalLabs"
    offer     = "GoldenImageOffer"
    sku       = "v1"
  }
}

# Shared Image Version (depends on Packer run)
resource "azurerm_shared_image_version" "image_version" {
  depends_on          = [null_resource.packer_build]
  name                = "1.0.0"
  gallery_name        = azurerm_shared_image_gallery.gallery.name
  image_name          = azurerm_shared_image.image_def.name
  resource_group_name = azurerm_resource_group.rg.name
  location            = azurerm_resource_group.rg.location

  target_region {
    name                   = "East US"
    regional_replica_count = 1
    storage_account_type   = "Standard_LRS"
  }

  managed_image_id = "/subscriptions/<sub-id>/resourceGroups/rg-image-gallery/providers/Microsoft.Compute/images/myManagedImage"
}
```

---

### 3. PowerShell Script (`scripts/build-image.ps1`)
```powershell
Write-Host "Running Packer build..."
packer build ./packer.json
```

---

## 🔍 Flow
1. You run `terraform apply`.  
2. Terraform executes the `null_resource` → runs `build-image.ps1`.  
3. That script calls **Packer** → builds the Managed Image.  
4. Terraform then publishes that image into **SIG** as a version.  

---

## ✅ Crux
- **Packer** = builds the golden image.  
- **Terraform** = organizes it into SIG.  
- **null_resource + local-exec** = glue that makes Packer run automatically during `terraform apply`.  

---

👉 This is exactly what you saw in your lab: the `.ps1` script was wired into Terraform, so Packer got triggered automatically.  

Bilal, would you like me to extend this with a **bonus step**: deploy a VM from the SIG version, so you can actually boot a machine from your golden image and confirm everything works end‑to‑end?
