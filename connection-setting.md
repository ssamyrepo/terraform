 **Git SSH authentication error** while Terraform tries to download the `terraform-aws-consul` module from GitHub via SSH.

---

### 🔍 **What the error means:**

```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

This means:

* You're trying to access a **GitHub repo via SSH**
* But **your local machine doesn’t have the right SSH key added to your GitHub account**, or
* You're using the wrong repo URL/protocol

---

### ✅ **Fix Options:**

#### **Option 1: Use HTTPS Instead of SSH (Easiest)**

Modify your module source URL to use HTTPS instead of SSH in your `main.tf`:

```hcl
module "consul" {
  source = "https://github.com/hashicorp/terraform-aws-consul.git"
  ...
}
```

Then rerun:

```bash
terraform init
```

#### **Option 2: Fix SSH Access to GitHub**

If you want to use SSH:

1. **Check for existing SSH keys:**

```bash
ls ~/.ssh/id_rsa.pub
```

2. **If no key, generate one:**

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

3. **Add your public key to GitHub:**

```bash
cat ~/.ssh/id_rsa.pub
```

Copy the output and add it to GitHub:
**GitHub → Settings → SSH and GPG keys → New SSH key**

4. **Test connection:**

```bash
ssh -T git@github.com
```

You should see:

> Hi <username>! You've successfully authenticated...

Then try:

```bash
terraform init
```

---

### ✅ **Recommendation**

Unless you **need to use SSH** (e.g., for private repos), go with **HTTPS** for public modules — it’s simpler and avoids SSH headaches.

Let me know if you want help setting up SSH or switching to HTTPS in your config.
