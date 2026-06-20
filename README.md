Here is your README in **ready-to-copy `.md` format**:

```md
# 🚀 Azure Claude Code Sandbox (Bicep)

This repository contains a **Bicep template** to deploy a secure, disposable Ubuntu VM sandbox on Azure for experimenting with tools like Claude Code.

The VM is configured with:

- ✅ RDP-enabled Ubuntu desktop (XFCE)
- ✅ Visual Studio Code (native)
- ✅ Python and Git
- ✅ Public IP restricted to **your IP only**
- ✅ Daily **auto-shutdown at 22:00 CET**
- ✅ Lightweight and cost-efficient setup

---

# 🏗️ Architecture Overview

```

Azure Resource Group
│
├── Virtual Network (VNet)
│   └── Subnet
│
├── Network Security Group (NSG)
│   └── Allows RDP (3389) only from your IP
│
├── Public IP (Static)
│
├── Network Interface
│
├── Virtual Machine (Ubuntu 22.04)
│   ├── XFCE Desktop
│   ├── XRDP (RDP access)
│   ├── VS Code
│   ├── Python + pip
│   └── Git
│
└── Auto-shutdown schedule (22:00 CET)

````

---

# 🔐 Security Model

- ✅ Only **RDP (port 3389)** is exposed  
- ✅ Access restricted to **your public IP**  
- ✅ No open public access  
- ✅ Password authentication enabled (required for RDP)  
- ✅ Optional SSH access via key  

---

# 📦 Prerequisites

- Azure CLI installed  
- Logged in to Azure:
  ```bash
  az login
````

* Existing resource group:
  ```bash
  az group create -n rg-claude-sandbox -l northeurope
  ```
* SSH key:
  ```bash
  ssh-keygen -t rsa -b 4096
  ```

***

# 📁 Repository Structure

```
.
├── main.bicep        # Main infrastructure template
├── cloud-init.txt    # VM bootstrap configuration
└── README.md
```

***

# ⚙️ Deployment

## 1. Clone the repo

```bash
git clone https://github.com/<your-repo>.git
cd <your-repo>
```

***

## 2. Get your public IP

```bash
MY_IP=$(curl -s ifconfig.me)/32
```

***

## 3. Deploy the template

```bash
az deployment group create \
  --resource-group rg-claude-sandbox \
  --template-file main.bicep \
  --parameters sshPublicKey="$(cat ~/.ssh/id_rsa.pub)" \
  --parameters myIpAddress=$MY_IP \
  --parameters adminPassword='YourStrongPassword123!'
```

***

# 🖥️ Connect via RDP

## 1. Get VM public IP

```bash
az vm show -d -g rg-claude-sandbox -n vm-claude-sandbox --query publicIps -o tsv
```

***

## 2. Open RDP client

On Windows:

```bash
mstsc
```

***

## 3. Connect

* **Host:** `<public-ip>`
* **Username:** `azureuser`
* **Password:** (the one you provided)

***

# 💻 What’s Installed

## ✅ Core tools

* Python 3 + pip
* Git

## ✅ GUI

* XFCE desktop environment
* XRDP server

## ✅ Developer tooling

* Visual Studio Code (official Microsoft build)

***

# ⏱️ Auto-Shutdown

The VM is configured to automatically shut down:

* 🕙 **Time:** 22:00
* 🌍 **Timezone:** Central Europe (handles DST automatically)

***

# 🧠 Usage Recommendations

This VM is designed for:

* Safe experimentation with generated code
* Running unknown scripts in isolation
* Testing development environments
* Prototyping automation

***

# ⚠️ Known Limitations

* XRDP performance is good but not identical to native desktop
* First login may take longer (\~20–30 seconds)
* Occasional reconnect may be needed

***

# 🔧 Customization

## Change VM size

Edit in `main.bicep`:

```bicep
vmSize: 'Standard_B2s'
```

***

## Allow additional ports

Modify NSG rules (e.g., add SSH):

```bicep
destinationPortRange: '22'
```

***

## Disable public access (more secure)

* Remove Public IP
* Use Azure Bastion instead

***

# 💸 Cost Optimization

* Uses **B-series VM** (low cost)
* Auto-shutdown enabled
* Suitable for on-demand usage

***

# 🧹 Cleanup

Delete all resources:

```bash
az group delete -n rg-claude-sandbox --yes --no-wait
```

***

# ✅ Summary

This setup provides:

* 🔐 Secure access (IP-restricted RDP)
* ⚡ Fast deployment (Bicep)
* 💻 Full Linux desktop with VS Code
* 🧪 Safe sandbox for experimentation
* 💸 Cost control via auto-shutdown
