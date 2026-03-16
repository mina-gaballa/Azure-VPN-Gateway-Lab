# Azure Networking Lab 🌍
Building a real-world Azure network from scratch — VNets, VPN Gateways, Virtual WAN, and Private DNS across 3 global regions. All deployed with ARM templates & PowerShell.

---

## 📋 Lab Overview
This lab covers end-to-end Azure networking configuration for the fictional company **Contoso**, deploying and connecting resources across 3 global regions.

| Region | Resource |
|--------|----------|
| Canada Central | CoreServicesVnet, CoreServicesVM, VPN Gateway |
| Norway East | ManufacturingVnet, ManufacturingVM, VPN Gateway |
| Central India | ResearchVnet, Virtual WAN Hub |

---

## 🏗️ Architecture
- 3 Virtual Networks across 3 regions
- 2 Virtual Machines (Windows Server 2019)
- 2 VPN Gateways connected VNet-to-VNet
- 1 Virtual WAN with a hub in Central India
- Private DNS Zone (contoso.com)

---

## 📁 Files

| File | Description |
|------|-------------|
| `azuredeploy.json` | ARM template to create all 3 VNets and subnets |
| `azuredeploy.parameters.json` | Parameters for VNets deployment |
| `CoreServicesVMazuredeploy.json` | ARM template for CoreServicesVM |
| `CoreServicesVMazuredeploy.parameters.json` | Parameters for CoreServicesVM |
| `ManufacturingVMazuredeploy.json` | ARM template for ManufacturingVM |
| `ManufacturingVMazuredeploy.parameters.json` | Parameters for ManufacturingVM |

---

## 🚀 Deployment Steps

### Task 1 — Create VNets
```powershell
$RGName = "ContosoResourceGroup"
New-AzResourceGroup -Name $RGName -Location "canadacentral"
New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
```

### Task 2 — Create CoreServicesVM
```powershell
New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile CoreServicesVMazuredeploy.json -TemplateParameterFile CoreServicesVMazuredeploy.parameters.json
```

### Task 3 — Create ManufacturingVM
```powershell
New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
```

### Task 4 — Create VPN Gateways
| Gateway | Region | VNet |
|---------|--------|------|
| CoreServicesVnetGateway | Canada Central | CoreServicesVnet |
| ManufacturingVnetGateway | Norway East | ManufacturingVnet |

### Task 5 — Connect VPN Gateways
| Connection | From | To | PSK |
|------------|------|----|-----|
| CoreServicesGW-to-ManufacturingGW | Canada Central | Norway East | abc123 |
| ManufacturingGW-to-CoreServicesGW | Norway East | Canada Central | abc123 |

### Task 6 — Create Virtual WAN
- **Name:** ContosoVirtualWAN
- **Type:** Standard
- **Hub Region:** Central India
- **Connected VNet:** ResearchVnet

---

## 📸 Screenshots

### All Resources
![All Resources](All-Resources-Overview.png)

### VPN Connections — Connected ✅
![VPN Connections](VPN-Connections-Connected.png)

### Virtual WAN Overview
![Virtual WAN](ContosoVirtualWAN-Overview.png)

---

## 🔧 Technologies Used
- Azure Virtual Networks (VNet)
- Azure VPN Gateway
- Azure Virtual WAN
- ARM Templates
- PowerShell (Az Module)
- Azure Private DNS

---

## 👤 Author
**Mina Gaballa**
