---
title: "aaaCreate prostředí Linux s hello 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření úložiště, virtuální počítač s Linuxem, virtuální síť a podsíť, nástroj pro vyrovnávání zatížení, seskupování, veřejnou IP adresu a skupinu zabezpečení sítě z hello pozadí pomocí hello 2.0 rozhraní příkazového řádku Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/06/2017
ms.author: iainfou
ms.openlocfilehash: 7287ea178e76001b84dade628ead04a59dc27f40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="51c27-103">Vytvoření kompletní virtuální počítač Linux s hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="51c27-103">Create a complete Linux virtual machine with hello Azure CLI</span></span>
<span data-ttu-id="51c27-104">tooquickly vytvoření virtuálního počítače (VM) v Azure, můžete použít jeden příkaz rozhraní příkazového řádku Azure, který používá výchozí hodnoty toocreate všech požadovaných podpora prostředky.</span><span class="sxs-lookup"><span data-stu-id="51c27-104">tooquickly create a virtual machine (VM) in Azure, you can use a single Azure CLI command that uses default values toocreate any required supporting resources.</span></span> <span data-ttu-id="51c27-105">Prostředky, jako je například virtuální sítě, veřejnou IP adresu a pravidel skupiny zabezpečení sítě se vytvářejí automaticky.</span><span class="sxs-lookup"><span data-stu-id="51c27-105">Resources such as a virtual network, public IP address, and network security group rules are automatically created.</span></span> <span data-ttu-id="51c27-106">Pro další ovládací prvek v provozním prostředí použít, můžete vytvořit tyto prostředky předem a pak přidat toothem vaše virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="51c27-106">For more control of your environment in production use, you may create these resources ahead of time and then add your VMs toothem.</span></span> <span data-ttu-id="51c27-107">Tento článek vás provede jak toocreate virtuálního počítače a každý z hello doprovodné materiály po jednom.</span><span class="sxs-lookup"><span data-stu-id="51c27-107">This article guides you through how toocreate a VM and each of hello supporting resources one by one.</span></span>

<span data-ttu-id="51c27-108">Ujistěte se, že jste nainstalovali hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a protokolu tooan Azure účet s [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="51c27-108">Make sure that you have installed hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and logged tooan Azure account in with [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="51c27-109">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="51c27-109">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="51c27-110">Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="51c27-110">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="51c27-111">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="51c27-111">Create resource group</span></span>
<span data-ttu-id="51c27-112">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="51c27-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="51c27-113">Před virtuální počítač a doprovodné materiály virtuální sítě je třeba vytvořit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="51c27-113">A resource group must be created before a virtual machine and supporting virtual network resources.</span></span> <span data-ttu-id="51c27-114">Vytvořte skupinu prostředků hello s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="51c27-114">Create hello resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="51c27-115">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="51c27-115">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="51c27-116">Ve výchozím nastavení výstup hello rozhraní příkazového řádku Azure je ve formátu JSON (JavaScript Object Notation).</span><span class="sxs-lookup"><span data-stu-id="51c27-116">By default, hello output of Azure CLI commands is in JSON (JavaScript Object Notation).</span></span> <span data-ttu-id="51c27-117">toochange hello výchozí výstup tooa seznamu nebo tabulka, například pomocí [konfigurace az--výstup](/cli/azure/#configure).</span><span class="sxs-lookup"><span data-stu-id="51c27-117">toochange hello default output tooa list or table, for example, use [az configure --output](/cli/azure/#configure).</span></span> <span data-ttu-id="51c27-118">Můžete také přidat `--output` tooany příkaz po dobu jednoho změnit ve výstupním formátu.</span><span class="sxs-lookup"><span data-stu-id="51c27-118">You can also add `--output` tooany command for a one time change in output format.</span></span> <span data-ttu-id="51c27-119">Hello následující příklad ukazuje výstup JSON hello hello `az group create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="51c27-119">hello following example shows hello JSON output from hello `az group create` command:</span></span>

```json                       
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "location": "eastus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="51c27-120">Vytvoření virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="51c27-120">Create a virtual network and subnet</span></span>
<span data-ttu-id="51c27-121">Dále vytvoříte virtuální síť v Azure a podsíť v toowhich můžete vytvořit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="51c27-121">Next you create a virtual network in Azure and a subnet in toowhich you can create your VMs.</span></span> <span data-ttu-id="51c27-122">Použití [vytvoření sítě vnet az](/cli/azure/network/vnet#create) toocreate virtuální síť s názvem *myVnet* s hello *192.168.0.0/16* předponu adresy.</span><span class="sxs-lookup"><span data-stu-id="51c27-122">Use [az network vnet create](/cli/azure/network/vnet#create) toocreate a virtual network named *myVnet* with hello *192.168.0.0/16* address prefix.</span></span> <span data-ttu-id="51c27-123">Můžete také přidat podsíť s názvem *mySubnet* s předponou adresy hello z *192.168.1.0/24*:</span><span class="sxs-lookup"><span data-stu-id="51c27-123">You also add a subnet named *mySubnet* with hello address prefix of *192.168.1.0/24*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

<span data-ttu-id="51c27-124">výstup Hello ukazuje podsíť hello jako logicky vytvořená ve virtuální síti hello:</span><span class="sxs-lookup"><span data-stu-id="51c27-124">hello output shows hello subnet as logically created inside hello virtual network:</span></span>

```json
{
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "location": "eastus",
  "name": "myVnet",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "ed62fd03-e9de-430b-84df-8a3b87cacdbb",
  "subnets": [
    {
      "addressPrefix": "192.168.1.0/24",
      "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
      "ipConfigurations": null,
      "name": "mySubnet",
      "networkSecurityGroup": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  "tags": {},
  "type": "Microsoft.Network/virtualNetworks",
  "virtualNetworkPeerings": null
}
```


## <a name="create-a-public-ip-address"></a><span data-ttu-id="51c27-125">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="51c27-125">Create a public IP address</span></span>
<span data-ttu-id="51c27-126">Nyní vytvoříme veřejnou IP adresu s [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="51c27-126">Now let's create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="51c27-127">Tato veřejná IP adresa umožňuje vám tooconnect tooyour virtuální počítače z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="51c27-127">This public IP address enables you tooconnect tooyour VMs from hello Internet.</span></span> <span data-ttu-id="51c27-128">Protože hello výchozí adresa je dynamická, jsme také vytvořit položku DNS s názvem hello `--domain-name-label` možnost.</span><span class="sxs-lookup"><span data-stu-id="51c27-128">Because hello default address is dynamic, we also create a named DNS entry with hello `--domain-name-label` option.</span></span> <span data-ttu-id="51c27-129">Hello následující příklad vytvoří veřejnou IP adresu s názvem *myPublicIP* s názvem DNS hello *mypublicdns*.</span><span class="sxs-lookup"><span data-stu-id="51c27-129">hello following example creates a public IP named *myPublicIP* with hello DNS name of *mypublicdns*.</span></span> <span data-ttu-id="51c27-130">Vzhledem k tomu, že název DNS hello musí být jedinečný, zadejte svůj vlastní jedinečný název DNS:</span><span class="sxs-lookup"><span data-stu-id="51c27-130">Because hello DNS name must be unique, provide your own unique DNS name:</span></span>

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

<span data-ttu-id="51c27-131">Výstup:</span><span class="sxs-lookup"><span data-stu-id="51c27-131">Output:</span></span>

```json
{
  "publicIp": {
    "dnsSettings": {
      "domainNameLabel": "mypublicdns",
      "fqdn": "mypublicdns.eastus.cloudapp.azure.com",
      "reverseFqdn": null
    },
    "etag": "W/\"2632aa72-3d2d-4529-b38e-b622b4202925\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": null,
    "ipConfiguration": null,
    "location": "eastus",
    "name": "myPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Dynamic",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "4c65de38-71f5-4684-be10-75e605b3e41f",
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses"
  }
}
```


## <a name="create-a-network-security-group"></a><span data-ttu-id="51c27-132">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="51c27-132">Create a network security group</span></span>
<span data-ttu-id="51c27-133">toocontrol hello tok provozu do a z virtuálních počítačů, vytvořte skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="51c27-133">toocontrol hello flow of traffic in and out of your VMs, create a network security group.</span></span> <span data-ttu-id="51c27-134">Skupina zabezpečení sítě může být použité tooa síťového adaptéru nebo podsítě.</span><span class="sxs-lookup"><span data-stu-id="51c27-134">A network security group can be applied tooa NIC or subnet.</span></span> <span data-ttu-id="51c27-135">Hello následující příklad používá [vytvořit az sítě nsg](/cli/azure/network/nsg#create) toocreate skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="51c27-135">hello following example uses [az network nsg create](/cli/azure/network/nsg#create) toocreate a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="51c27-136">Můžete definovat pravidla, která povolují nebo odepírají hello konkrétní přenosy.</span><span class="sxs-lookup"><span data-stu-id="51c27-136">You define rules that allow or deny hello specific traffic.</span></span> <span data-ttu-id="51c27-137">tooallow příchozí připojení na portu 22 (toosupport SSH), vytvoření příchozího pravidla pro skupinu zabezpečení sítě hello s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="51c27-137">tooallow inbound connections on port 22 (toosupport SSH), create an inbound rule for hello network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="51c27-138">Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRuleSSH*:</span><span class="sxs-lookup"><span data-stu-id="51c27-138">hello following example creates a rule named *myNetworkSecurityGroupRuleSSH*:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 22 \
    --access allow
```

<span data-ttu-id="51c27-139">tooallow příchozí připojení na portu 80 (toosupport webových přenosů), přidejte další pravidla skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="51c27-139">tooallow inbound connections on port 80 (toosupport web traffic), add another network security group rule.</span></span> <span data-ttu-id="51c27-140">Hello následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRuleHTTP*:</span><span class="sxs-lookup"><span data-stu-id="51c27-140">hello following example creates a rule named *myNetworkSecurityGroupRuleHTTP*:</span></span>

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleWeb \
    --protocol tcp \
    --priority 1001 \
    --destination-port-range 80 \
    --access allow
```

<span data-ttu-id="51c27-141">Zkontrolujte hello skupinu zabezpečení sítě a pravidla s [az sítě nsg zobrazit](/cli/azure/network/nsg#show):</span><span class="sxs-lookup"><span data-stu-id="51c27-141">Examine hello network security group and rules with [az network nsg show](/cli/azure/network/nsg#show):</span></span>

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

<span data-ttu-id="51c27-142">Výstup:</span><span class="sxs-lookup"><span data-stu-id="51c27-142">Output:</span></span>

```json
{
  "defaultSecurityRules": [
    {
      "access": "Allow",
      "description": "Allow inbound traffic from all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetInBound",
      "name": "AllowVnetInBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow inbound traffic from azure load balancer",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowAzureLoadBalancerInBou
      "name": "AllowAzureLoadBalancerInBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "AzureLoadBalancer",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all inbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllInBound",
      "name": "DenyAllInBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs tooall VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetOutBound",
      "name": "AllowVnetOutBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs tooInternet",
      "destinationAddressPrefix": "Internet",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowInternetOutBound",
      "name": "AllowInternetOutBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all outbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllOutBound",
      "name": "DenyAllOutBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
  "location": "eastus",
  "name": "myNetworkSecurityGroup",
  "networkInterfaces": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "47a9964e-23a3-438a-a726-8d60ebbb1c3c",
  "securityRules": [
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "22",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleSSH",
      "name": "myNetworkSecurityGroupRuleSSH",
      "priority": 1000,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "80",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleWeb",
      "name": "myNetworkSecurityGroupRuleWeb",
      "priority": 1001,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "subnets": null,
  "tags": null,
  "type": "Microsoft.Network/networkSecurityGroups"
}
```

## <a name="create-a-virtual-nic"></a><span data-ttu-id="51c27-143">Vytvořit virtuální síťovou kartu</span><span class="sxs-lookup"><span data-stu-id="51c27-143">Create a virtual NIC</span></span>
<span data-ttu-id="51c27-144">Virtuální síťové adaptéry (NIC) jsou prostřednictvím kódu programu k dispozici, protože můžete použít pravidla tootheir použití.</span><span class="sxs-lookup"><span data-stu-id="51c27-144">Virtual network interface cards (NICs) are programmatically available because you can apply rules tootheir use.</span></span> <span data-ttu-id="51c27-145">Také můžete mít více než jednu.</span><span class="sxs-lookup"><span data-stu-id="51c27-145">You can also have more than one.</span></span> <span data-ttu-id="51c27-146">V následující hello [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create) příkazu vytvoříte síťový adaptér s názvem *myNic* a přidružte ji k hello skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="51c27-146">In hello following [az network nic create](/cli/azure/network/nic#create) command, you create a NIC named *myNic* and associate it with hello network security group.</span></span> <span data-ttu-id="51c27-147">Hello veřejnou IP adresu *myPublicIP* je taky přiřazený hello virtuální síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="51c27-147">hello public IP address *myPublicIP* is also associated with hello virtual NIC.</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="51c27-148">Výstup:</span><span class="sxs-lookup"><span data-stu-id="51c27-148">Output:</span></span>

```json
{
  "NewNIC": {
    "dnsSettings": {
      "appliedDnsServers": [],
      "dnsServers": [],
      "internalDnsNameLabel": null,
      "internalDomainNameSuffix": "brqlt10lvoxedgkeuomc4pm5tb.bx.internal.cloudapp.net",
      "internalFqdn": null
    },
    "enableAcceleratedNetworking": false,
    "enableIpForwarding": false,
    "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
    "ipConfigurations": [
      {
        "applicationGatewayBackendAddressPools": null,
        "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic/ipConfigurations/ipconfig1",
        "loadBalancerBackendAddressPools": null,
        "loadBalancerInboundNatRules": null,
        "name": "ipconfig1",
        "primary": true,
        "privateIpAddress": "192.168.1.4",
        "privateIpAddressVersion": "IPv4",
        "privateIpAllocationMethod": "Dynamic",
        "provisioningState": "Succeeded",
        "publicIpAddress": {
          "dnsSettings": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
          "idleTimeoutInMinutes": null,
          "ipAddress": null,
          "ipConfiguration": null,
          "location": null,
          "name": null,
          "provisioningState": null,
          "publicIpAddressVersion": null,
          "publicIpAllocationMethod": null,
          "resourceGroup": "myResourceGroup",
          "resourceGuid": null,
          "tags": null,
          "type": null
        },
        "resourceGroup": "myResourceGroup",
        "subnet": {
          "addressPrefix": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
          "ipConfigurations": null,
          "name": null,
          "networkSecurityGroup": null,
          "provisioningState": null,
          "resourceGroup": "myResourceGroup",
          "resourceNavigationLinks": null,
          "routeTable": null
        }
      }
    ],
    "location": "eastus",
    "macAddress": null,
    "name": "myNic",
    "networkSecurityGroup": {
      "defaultSecurityRules": null,
      "etag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
      "location": null,
      "name": null,
      "networkInterfaces": null,
      "provisioningState": null,
      "resourceGroup": "myResourceGroup",
      "resourceGuid": null,
      "securityRules": null,
      "subnets": null,
      "tags": null,
      "type": null
    },
    "primary": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "b3dbaa0e-2cf2-43be-a814-5cc49fea3304",
    "tags": null,
    "type": "Microsoft.Network/networkInterfaces",
    "virtualMachine": null
  }
}
```


## <a name="create-an-availability-set"></a><span data-ttu-id="51c27-149">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="51c27-149">Create an availability set</span></span>
<span data-ttu-id="51c27-150">Dostupnost nastaví nápovědu šíření virtuální počítače napříč domén selhání a aktualizace domény.</span><span class="sxs-lookup"><span data-stu-id="51c27-150">Availability sets help spread your VMs across fault domains and update domains.</span></span> <span data-ttu-id="51c27-151">I když můžete vytvořit pouze jeden virtuální počítač nyní, je nejlepší postup toouse dostupnost sady toomake je snazší tooexpand v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="51c27-151">Even though you only create one VM right now, it's best practice toouse availability sets toomake it easier tooexpand in hello future.</span></span> 

<span data-ttu-id="51c27-152">Domén selhání definovat seskupování virtuálních počítačů, které sdílejí společné přepínač zdroje a sítě power.</span><span class="sxs-lookup"><span data-stu-id="51c27-152">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="51c27-153">Ve výchozím nastavení hello virtuálních počítačů, které jsou nakonfigurované v rámci vaší sady dostupnosti jsou oddělené v rámci až toothree domén selhání.</span><span class="sxs-lookup"><span data-stu-id="51c27-153">By default, hello virtual machines that are configured within your availability set are separated across up toothree fault domains.</span></span> <span data-ttu-id="51c27-154">Problémem hardwaru v jednom z těchto domén selhání nemá vliv na každý virtuální počítač, který běží vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="51c27-154">A hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span>

<span data-ttu-id="51c27-155">Aktualizace domén označují skupiny virtuálních počítačů a základní fyzický hardware, který může být restartován v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="51c27-155">Update domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="51c27-156">Během plánované údržby hello pořadí, ve které aktualizace se restartují domény nemusí být po sobě jdoucích, ale po restartu pouze jednu aktualizační doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="51c27-156">During planned maintenance, hello order in which update domains are rebooted might not be sequential, but only one update domain is rebooted at a time.</span></span>

<span data-ttu-id="51c27-157">Virtuální počítače Azure automaticky distribuuje mezi doménami hello selhání a aktualizace, když umístění do nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="51c27-157">Azure automatically distributes VMs across hello fault and update domains when placing them in an availability set.</span></span> <span data-ttu-id="51c27-158">Další informace najdete v tématu [Správa hello dostupnosti virtuálních počítačů](manage-availability.md).</span><span class="sxs-lookup"><span data-stu-id="51c27-158">For more information, see [managing hello availability of VMs](manage-availability.md).</span></span>

<span data-ttu-id="51c27-159">Vytvořit sadu dostupnosti pro virtuální počítač s [az virtuálních počítačů sady dostupnosti. vytváření](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="51c27-159">Create an availability set for your VM with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="51c27-160">Hello následující příklad vytvoří sadu s názvem dostupnosti *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="51c27-160">hello following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

<span data-ttu-id="51c27-161">Hello domén selhání poznámky výstup a aktualizace domény:</span><span class="sxs-lookup"><span data-stu-id="51c27-161">hello output notes fault domains and update domains:</span></span>

```json
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet",
  "location": "eastus",
  "managed": null,
  "name": "myAvailabilitySet",
  "platformFaultDomainCount": 2,
  "platformUpdateDomainCount": 5,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": null,
    "managed": true,
    "tier": null
  },
  "statuses": null,
  "tags": {},
  "type": "Microsoft.Compute/availabilitySets",
  "virtualMachines": []
}
```


## <a name="create-hello-linux-vms"></a><span data-ttu-id="51c27-162">Vytvořit virtuální počítače s Linuxem hello</span><span class="sxs-lookup"><span data-stu-id="51c27-162">Create hello Linux VMs</span></span>
<span data-ttu-id="51c27-163">Hello síťové prostředky toosupport jste vytvořili přístupné z Internetu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51c27-163">You've created hello network resources toosupport Internet-accessible VMs.</span></span> <span data-ttu-id="51c27-164">Teď vytvořte virtuální počítač a zabezpečte ji pomocí klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="51c27-164">Now create a VM and secure it with an SSH key.</span></span> <span data-ttu-id="51c27-165">V tomto případě vytvoříme toocreate virtuálního počítače s Ubuntu podle hello nejnovější LTS.</span><span class="sxs-lookup"><span data-stu-id="51c27-165">In this case, we're going toocreate an Ubuntu VM based on hello most recent LTS.</span></span> <span data-ttu-id="51c27-166">Můžete najít další Image s [seznamu obrázků virtuálních počítačů az](/cli/azure/vm/image#list), jak je popsáno v [hledání Image virtuálního počítače Azure](cli-ps-findimage.md).</span><span class="sxs-lookup"><span data-stu-id="51c27-166">You can find additional images with [az vm image list](/cli/azure/vm/image#list), as described in [finding Azure VM images](cli-ps-findimage.md).</span></span>

<span data-ttu-id="51c27-167">Můžeme také určit klíče toouse SSH pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="51c27-167">We also specify an SSH key toouse for authentication.</span></span> <span data-ttu-id="51c27-168">Pokud nemáte páru veřejného klíče SSH, můžete [jejich vytvoření](mac-create-ssh-keys.md) nebo použijte hello `--generate-ssh-keys` toocreate parametr je pro vás.</span><span class="sxs-lookup"><span data-stu-id="51c27-168">If you do not have an SSH public key pair, you can [create them](mac-create-ssh-keys.md) or use hello `--generate-ssh-keys` parameter toocreate them for you.</span></span> <span data-ttu-id="51c27-169">Pokud jste již pár klíčů, tento parametr používá existující klíče v `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="51c27-169">If you already a key pair, this parameter uses existing keys in `~/.ssh`.</span></span>

<span data-ttu-id="51c27-170">Vytvoření hello virtuálních počítačů tak, že převedou všechny naše prostředky a informace o společně s hello [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="51c27-170">Create hello VM by bringing all our resources and information together with hello [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="51c27-171">Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="51c27-171">hello following example creates a VM named *myVM*:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --availability-set myAvailabilitySet \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="51c27-172">SSH tooyour virtuálních počítačů s hello záznam DNS, které jste zadali při vytváření hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="51c27-172">SSH tooyour VM with hello DNS entry you provided when you created hello public IP address.</span></span> <span data-ttu-id="51c27-173">To `fqdn` se zobrazí ve výstupu hello při vytváření virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="51c27-173">This `fqdn` is shown in hello output as you create your VM:</span></span>

```json
{
  "fqdns": "mypublicdns.eastus.cloudapp.azure.com",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-13-71-C8",
  "powerState": "VM running",
  "privateIpAddress": "192.168.1.5",
  "publicIpAddress": "13.90.94.252",
  "resourceGroup": "myResourceGroup"
}
```

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

<span data-ttu-id="51c27-174">Výstup:</span><span class="sxs-lookup"><span data-stu-id="51c27-174">Output:</span></span>

```bash
hello authenticity of host 'mypublicdns.eastus.cloudapp.azure.com (13.90.94.252)' can't be established.
ECDSA key fingerprint is SHA256:SylINP80Um6XRTvWiFaNz+H+1jcrKB1IiNgCDDJRj6A.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.eastus.cloudapp.azure.com,13.90.94.252' (ECDSA) toohello list of known hosts.
Welcome tooUbuntu 16.04.2 LTS (GNU/Linux 4.4.0-81-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


hello programs included with hello Ubuntu system are free software;
hello exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, toohello extent permitted by
applicable law.

toorun a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@myVM:~$
```

<span data-ttu-id="51c27-175">Můžete nainstalovat NGINX a najdete v části hello provoz toku toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51c27-175">You can install NGINX and see hello traffic flow toohello VM.</span></span> <span data-ttu-id="51c27-176">Nainstalujte NGINX následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="51c27-176">Install NGINX as follows:</span></span>

```bash
sudo apt-get install -y nginx
```

<span data-ttu-id="51c27-177">toosee hello výchozí NGINX lokality v akci, otevřete webový prohlížeč a zadejte vaše plně kvalifikovaný název domény:</span><span class="sxs-lookup"><span data-stu-id="51c27-177">toosee hello default NGINX site in action, open your web browser and enter your FQDN:</span></span>

![Výchozí web NGINX na vašem virtuálním počítači](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a><span data-ttu-id="51c27-179">Exportovat jako šablonu</span><span class="sxs-lookup"><span data-stu-id="51c27-179">Export as a template</span></span>
<span data-ttu-id="51c27-180">Co když teď chcete toocreate další vývoj prostředí s hello stejnými parametry nebo produkčním prostředí, který odpovídá jeho?</span><span class="sxs-lookup"><span data-stu-id="51c27-180">What if you now want toocreate an additional development environment with hello same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="51c27-181">Správce prostředků používá šablony JSON, které definují všech hello parametrů pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="51c27-181">Resource Manager uses JSON templates that define all hello parameters for your environment.</span></span> <span data-ttu-id="51c27-182">Můžete vytvořit na celé prostředí podle odkazující na tuto šablonu JSON.</span><span class="sxs-lookup"><span data-stu-id="51c27-182">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="51c27-183">Můžete [vytvořit šablony JSON ručně](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo exportovat stávající šablonu JSON toocreate hello prostředí pro vás.</span><span class="sxs-lookup"><span data-stu-id="51c27-183">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment toocreate hello JSON template for you.</span></span> <span data-ttu-id="51c27-184">Použití [export skupiny az](/cli/azure/group#export) tooexport prostředku skupiny takto:</span><span class="sxs-lookup"><span data-stu-id="51c27-184">Use [az group export](/cli/azure/group#export) tooexport your resource group as follows:</span></span>

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

<span data-ttu-id="51c27-185">Tento příkaz vytvoří hello `myResourceGroup.json` souboru v aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="51c27-185">This command creates hello `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="51c27-186">Při vytváření prostředí z této šablony, zobrazí se výzva pro všechny názvy prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="51c27-186">When you create an environment from this template, you are prompted for all hello resource names.</span></span> <span data-ttu-id="51c27-187">Tyto názvy v souboru šablony může vyplnit přidáním hello `--include-parameter-default-value` parametr toohello `az group export` příkaz.</span><span class="sxs-lookup"><span data-stu-id="51c27-187">You can populate these names in your template file by adding hello `--include-parameter-default-value` parameter toohello `az group export` command.</span></span> <span data-ttu-id="51c27-188">Upravit vaše JSON šablony toospecify hello názvy prostředků, nebo [vytvoření souboru parameters.JSON tímto kódem](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) určující názvy prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="51c27-188">Edit your JSON template toospecify hello resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies hello resource names.</span></span>

<span data-ttu-id="51c27-189">toocreate prostředí z šablony, použijte [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="51c27-189">toocreate an environment from your template, use [az group deployment create](/cli/azure/group/deployment#create) as follows:</span></span>

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

<span data-ttu-id="51c27-190">Můžete chtít tooread [více o toodeploy ze šablon](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="51c27-190">You might want tooread [more about how toodeploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="51c27-191">Další informace o tom, jak použít soubor parametrů hello tooincrementally aktualizace prostředí a přístup k šablony jedno umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="51c27-191">Learn about how tooincrementally update environments, use hello parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51c27-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51c27-192">Next steps</span></span>
<span data-ttu-id="51c27-193">Nyní jste připravené toobegin práce s více síťovými součástmi a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51c27-193">Now you're ready toobegin working with multiple networking components and VMs.</span></span> <span data-ttu-id="51c27-194">Tato ukázka toobuild prostředí můžete použít se vaše aplikace pomocí sem zavedl hello základní součásti služby.</span><span class="sxs-lookup"><span data-stu-id="51c27-194">You can use this sample environment toobuild out your application by using hello core components introduced here.</span></span>
