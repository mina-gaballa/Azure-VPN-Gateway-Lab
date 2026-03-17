# Azure Networking Lab 🌍
Building a real-world Azure Application Gateway infrastructure for Contoso in Central India using ARM templates & PowerShell.

---

## 📋 Lab Overview
This lab demonstrates deploying backend web servers behind an Azure Application Gateway and validating traffic distribution and backend health.

| Region | Resource |
|--------|----------|
| Central India | ContosoVNet, ContosoAppGateway, BackendVM1, BackendVM2 |

---

## 🏗️ Architecture
- 1 Virtual Network (ContosoVNet)
- 1 Application Gateway (ContosoAppGateway)
- 2 Backend Virtual Machines (BackendVM1, BackendVM2)
- Backend subnet (BackendSubnet)
- Backend Pool (HTTP - Port 80)
- IIS installed on both VMs

---

## 📁 Files

| File | Description |
|------|-------------|
| `backend-centralindia.json` | ARM template for backend VMs and networking |
| `backend-centralindia.parameters.json` | Parameters file |
| `install-iis-centralindia.ps1` | Script to install IIS |

---

## 🚀 Deployment Steps

### Task 1 — Deploy Backend Resources
```powershell
$RGName = "ContosoResourceGroup"
New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile backend-centralindia.json -TemplateParameterFile backend-centralindia.parameters.json
```

This deployment creates:
- BackendVM1
- BackendVM2
- Network Interfaces
- Supporting resources in Central India

---

### Task 2 — Install IIS on BackendVM1
```powershell
Invoke-AzVMRunCommand -ResourceGroupName 'ContosoResourceGroup' -Name 'BackendVM1' -CommandId 'RunPowerShellScript' -ScriptPath 'install-iis-centralindia.ps1'
```

---

### Task 3 — Install IIS on BackendVM2
```powershell
Invoke-AzVMRunCommand -ResourceGroupName 'ContosoResourceGroup' -Name 'BackendVM2' -CommandId 'RunPowerShellScript' -ScriptPath 'install-iis-centralindia.ps1'
```

The script installs IIS and customizes each VM page to display its name.

---

### Task 4 — Validate Backend Health

- Open **ContosoAppGateway**
- Go to **Backend Health**
- Ensure both VMs are **Healthy**
- Status must be **200 OK**

Expected IPs:
- 10.0.1.4
- 10.0.1.5

---

### Task 5 — Test Load Balancing

Open in browser:
```
http://20.219.244.30
```

Refresh multiple times → should switch between:
- BackendVM1
- BackendVM2

---

## 📸 Screenshots

### All Resources
![All Resources](all-resources-centralindia.PNG)

### Backend Health
![Backend Health](app-gateway-backend-health.PNG)

### Browser Test — BackendVM1
![BackendVM1](app-gateway-test-backendvm1.PNG)

### Browser Test — BackendVM2
![BackendVM2](app-gateway-test-backendvm2.PNG)

---

## 🔧 Technologies Used
- Azure Application Gateway
- Azure Virtual Network
- Azure Virtual Machines
- ARM Templates
- PowerShell (Az Module)
- IIS

---

## 👤 Author
Mina Gaballa
