---
title: "Vytvořte prostředí Linux s 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Vytvoření úložiště, virtuální počítač s Linuxem, virtuální síť a podsíť, nástroj pro vyrovnávání zatížení, seskupování, veřejnou IP adresu a skupinu zabezpečení sítě, všechny od základů pomocí Azure CLI 2.0."
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
ms.openlocfilehash: e5c4785428b2150e951923e98079e00808a82d87
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-complete-linux-virtual-machine-with-the-azure-cli"></a><span data-ttu-id="a69f6-103">Vytvoření kompletní virtuální počítač Linux pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a69f6-103">Create a complete Linux virtual machine with the Azure CLI</span></span>
<span data-ttu-id="a69f6-104">Rychle vytvořit virtuální počítač (VM) v Azure, můžete jeden příkaz rozhraní příkazového řádku Azure, kterou použije výchozí hodnoty pro vytvoření všechny požadované podpůrné prostředky.</span><span class="sxs-lookup"><span data-stu-id="a69f6-104">To quickly create a virtual machine (VM) in Azure, you can use a single Azure CLI command that uses default values to create any required supporting resources.</span></span> <span data-ttu-id="a69f6-105">Prostředky, jako je například virtuální sítě, veřejnou IP adresu a pravidel skupiny zabezpečení sítě se vytvářejí automaticky.</span><span class="sxs-lookup"><span data-stu-id="a69f6-105">Resources such as a virtual network, public IP address, and network security group rules are automatically created.</span></span> <span data-ttu-id="a69f6-106">Pro další ovládací prvek v provozním prostředí použít, můžete vytvořit tyto prostředky předem a pak do nich přidat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="a69f6-106">For more control of your environment in production use, you may create these resources ahead of time and then add your VMs to them.</span></span> <span data-ttu-id="a69f6-107">Tento článek vás provede postup vytvoření virtuálního počítače a každý z doprovodné materiály po jednom.</span><span class="sxs-lookup"><span data-stu-id="a69f6-107">This article guides you through how to create a VM and each of the supporting resources one by one.</span></span>

<span data-ttu-id="a69f6-108">Ujistěte se, že jste nainstalovali nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlášení k účtu Azure s [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="a69f6-108">Make sure that you have installed the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and logged to an Azure account in with [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="a69f6-109">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a69f6-109">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="a69f6-110">Zahrnout názvy parametrů příklad *myResourceGroup*, *myVnet*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="a69f6-110">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="a69f6-111">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a69f6-111">Create resource group</span></span>
<span data-ttu-id="a69f6-112">Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="a69f6-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="a69f6-113">Před virtuální počítač a doprovodné materiály virtuální sítě je třeba vytvořit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a69f6-113">A resource group must be created before a virtual machine and supporting virtual network resources.</span></span> <span data-ttu-id="a69f6-114">Vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="a69f6-114">Create the resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="a69f6-115">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="a69f6-115">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="a69f6-116">Ve výchozím nastavení výstup rozhraní příkazového řádku Azure je ve formátu JSON (JavaScript Object Notation).</span><span class="sxs-lookup"><span data-stu-id="a69f6-116">By default, the output of Azure CLI commands is in JSON (JavaScript Object Notation).</span></span> <span data-ttu-id="a69f6-117">Chcete-li změnit výchozí výstup do seznamu nebo tabulka, například použít [konfigurace az--výstup](/cli/azure/#configure).</span><span class="sxs-lookup"><span data-stu-id="a69f6-117">To change the default output to a list or table, for example, use [az configure --output](/cli/azure/#configure).</span></span> <span data-ttu-id="a69f6-118">Můžete také přidat `--output` na libovolný příkaz po dobu jednoho změnit ve výstupním formátu.</span><span class="sxs-lookup"><span data-stu-id="a69f6-118">You can also add `--output` to any command for a one time change in output format.</span></span> <span data-ttu-id="a69f6-119">Následující příklad ukazuje výstup JSON `az group create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="a69f6-119">The following example shows the JSON output from the `az group create` command:</span></span>

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

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="a69f6-120">Vytvoření virtuální sítě a podsítě</span><span class="sxs-lookup"><span data-stu-id="a69f6-120">Create a virtual network and subnet</span></span>
<span data-ttu-id="a69f6-121">Další, které vytvoření virtuální sítě v Azure a podsíť v do které můžete vytvořit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="a69f6-121">Next you create a virtual network in Azure and a subnet in to which you can create your VMs.</span></span> <span data-ttu-id="a69f6-122">Použití [vytvoření sítě vnet az](/cli/azure/network/vnet#create) vytvořit virtuální síť s názvem *myVnet* s *192.168.0.0/16* předponu adresy.</span><span class="sxs-lookup"><span data-stu-id="a69f6-122">Use [az network vnet create](/cli/azure/network/vnet#create) to create a virtual network named *myVnet* with the *192.168.0.0/16* address prefix.</span></span> <span data-ttu-id="a69f6-123">Můžete také přidat podsíť s názvem *mySubnet* s předponou adresy z *192.168.1.0/24*:</span><span class="sxs-lookup"><span data-stu-id="a69f6-123">You also add a subnet named *mySubnet* with the address prefix of *192.168.1.0/24*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

<span data-ttu-id="a69f6-124">Výstup ukazuje podsíť jako logicky vytvořená ve virtuální síti:</span><span class="sxs-lookup"><span data-stu-id="a69f6-124">The output shows the subnet as logically created inside the virtual network:</span></span>

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


## <a name="create-a-public-ip-address"></a><span data-ttu-id="a69f6-125">Vytvoření veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="a69f6-125">Create a public IP address</span></span>
<span data-ttu-id="a69f6-126">Nyní vytvoříme veřejnou IP adresu s [vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="a69f6-126">Now let's create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="a69f6-127">Tato veřejná IP adresa umožňuje připojení k virtuální počítače z Internetu.</span><span class="sxs-lookup"><span data-stu-id="a69f6-127">This public IP address enables you to connect to your VMs from the Internet.</span></span> <span data-ttu-id="a69f6-128">Protože výchozí adresa je dynamická, jsme také vytvořit položku DNS s názvem s `--domain-name-label` možnost.</span><span class="sxs-lookup"><span data-stu-id="a69f6-128">Because the default address is dynamic, we also create a named DNS entry with the `--domain-name-label` option.</span></span> <span data-ttu-id="a69f6-129">Následující příklad vytvoří veřejnou IP adresu s názvem *myPublicIP* s názvem DNS *mypublicdns*.</span><span class="sxs-lookup"><span data-stu-id="a69f6-129">The following example creates a public IP named *myPublicIP* with the DNS name of *mypublicdns*.</span></span> <span data-ttu-id="a69f6-130">Vzhledem k tomu, že název DNS musí být jedinečný, zadejte svůj vlastní jedinečný název DNS:</span><span class="sxs-lookup"><span data-stu-id="a69f6-130">Because the DNS name must be unique, provide your own unique DNS name:</span></span>

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

<span data-ttu-id="a69f6-131">Výstup:</span><span class="sxs-lookup"><span data-stu-id="a69f6-131">Output:</span></span>

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


## <a name="create-a-network-security-group"></a><span data-ttu-id="a69f6-132">Vytvořit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="a69f6-132">Create a network security group</span></span>
<span data-ttu-id="a69f6-133">K řízení toku provozu do a z virtuálních počítačů, vytvořte skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="a69f6-133">To control the flow of traffic in and out of your VMs, create a network security group.</span></span> <span data-ttu-id="a69f6-134">Skupina zabezpečení sítě je použít pro síťový adaptér nebo podsíť.</span><span class="sxs-lookup"><span data-stu-id="a69f6-134">A network security group can be applied to a NIC or subnet.</span></span> <span data-ttu-id="a69f6-135">Následující příklad používá [vytvořit az sítě nsg](/cli/azure/network/nsg#create) vytvoříte skupinu zabezpečení sítě s názvem *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="a69f6-135">The following example uses [az network nsg create](/cli/azure/network/nsg#create) to create a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="a69f6-136">Můžete definovat pravidla, která povolují nebo odepírají konkrétní přenosy.</span><span class="sxs-lookup"><span data-stu-id="a69f6-136">You define rules that allow or deny the specific traffic.</span></span> <span data-ttu-id="a69f6-137">Povolit příchozí připojení na portu 22 (pro podporu SSH), vytvoření příchozího pravidla pro skupinu zabezpečení sítě s [vytvořit pravidla nsg sítě az](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="a69f6-137">To allow inbound connections on port 22 (to support SSH), create an inbound rule for the network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="a69f6-138">Následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRuleSSH*:</span><span class="sxs-lookup"><span data-stu-id="a69f6-138">The following example creates a rule named *myNetworkSecurityGroupRuleSSH*:</span></span>

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

<span data-ttu-id="a69f6-139">Chcete-li povolit příchozí připojení na portu 80 (pro podporu webový provoz), přidejte jiná pravidla skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="a69f6-139">To allow inbound connections on port 80 (to support web traffic), add another network security group rule.</span></span> <span data-ttu-id="a69f6-140">Následující příklad vytvoří pravidlo s názvem *myNetworkSecurityGroupRuleHTTP*:</span><span class="sxs-lookup"><span data-stu-id="a69f6-140">The following example creates a rule named *myNetworkSecurityGroupRuleHTTP*:</span></span>

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

<span data-ttu-id="a69f6-141">Podívejte se na skupinu zabezpečení sítě a pravidla s [az sítě nsg zobrazit](/cli/azure/network/nsg#show):</span><span class="sxs-lookup"><span data-stu-id="a69f6-141">Examine the network security group and rules with [az network nsg show](/cli/azure/network/nsg#show):</span></span>

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

<span data-ttu-id="a69f6-142">Výstup:</span><span class="sxs-lookup"><span data-stu-id="a69f6-142">Output:</span></span>

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
      "description": "Allow outbound traffic from all VMs to all VMs in VNET",
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
      "description": "Allow outbound traffic from all VMs to Internet",
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

## <a name="create-a-virtual-nic"></a><span data-ttu-id="a69f6-143">Vytvořit virtuální síťovou kartu</span><span class="sxs-lookup"><span data-stu-id="a69f6-143">Create a virtual NIC</span></span>
<span data-ttu-id="a69f6-144">Virtuální síťové adaptéry (NIC) jsou prostřednictvím kódu programu k dispozici, protože pravidla můžete použít pro jejich použití.</span><span class="sxs-lookup"><span data-stu-id="a69f6-144">Virtual network interface cards (NICs) are programmatically available because you can apply rules to their use.</span></span> <span data-ttu-id="a69f6-145">Také můžete mít více než jednu.</span><span class="sxs-lookup"><span data-stu-id="a69f6-145">You can also have more than one.</span></span> <span data-ttu-id="a69f6-146">V následujícím [vytvořit síťových adaptérů sítě az](/cli/azure/network/nic#create) příkazu vytvoříte síťový adaptér s názvem *myNic* a přidružte ji k skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="a69f6-146">In the following [az network nic create](/cli/azure/network/nic#create) command, you create a NIC named *myNic* and associate it with the network security group.</span></span> <span data-ttu-id="a69f6-147">Veřejná IP adresa *myPublicIP* je taky přiřazený virtuální síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="a69f6-147">The public IP address *myPublicIP* is also associated with the virtual NIC.</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="a69f6-148">Výstup:</span><span class="sxs-lookup"><span data-stu-id="a69f6-148">Output:</span></span>

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


## <a name="create-an-availability-set"></a><span data-ttu-id="a69f6-149">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="a69f6-149">Create an availability set</span></span>
<span data-ttu-id="a69f6-150">Dostupnost nastaví nápovědu šíření virtuální počítače napříč domén selhání a aktualizace domény.</span><span class="sxs-lookup"><span data-stu-id="a69f6-150">Availability sets help spread your VMs across fault domains and update domains.</span></span> <span data-ttu-id="a69f6-151">I když můžete vytvořit pouze jeden virtuální počítač nyní, je osvědčeným postupem použít skupiny dostupnosti, aby bylo snazší v budoucnu rozšířit.</span><span class="sxs-lookup"><span data-stu-id="a69f6-151">Even though you only create one VM right now, it's best practice to use availability sets to make it easier to expand in the future.</span></span> 

<span data-ttu-id="a69f6-152">Domén selhání definovat seskupování virtuálních počítačů, které sdílejí společné přepínač zdroje a sítě power.</span><span class="sxs-lookup"><span data-stu-id="a69f6-152">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="a69f6-153">Ve výchozím nastavení jsou oddělené virtuální počítače, které jsou nakonfigurované v rámci vaší skupiny dostupnosti v rámci až tři domény selhání.</span><span class="sxs-lookup"><span data-stu-id="a69f6-153">By default, the virtual machines that are configured within your availability set are separated across up to three fault domains.</span></span> <span data-ttu-id="a69f6-154">Problémem hardwaru v jednom z těchto domén selhání nemá vliv na každý virtuální počítač, který běží vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="a69f6-154">A hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span>

<span data-ttu-id="a69f6-155">Aktualizace domén označují skupiny virtuálních počítačů a základní fyzický hardware, který může být restartován ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="a69f6-155">Update domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at the same time.</span></span> <span data-ttu-id="a69f6-156">Během plánované údržby pořadí, ve které aktualizace se restartují domény nemusí být po sobě jdoucích, ale po restartu pouze jednu aktualizační doménu najednou.</span><span class="sxs-lookup"><span data-stu-id="a69f6-156">During planned maintenance, the order in which update domains are rebooted might not be sequential, but only one update domain is rebooted at a time.</span></span>

<span data-ttu-id="a69f6-157">Virtuální počítače Azure automaticky distribuuje mezi doménami selhání a aktualizace, když umístění do nastavení dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="a69f6-157">Azure automatically distributes VMs across the fault and update domains when placing them in an availability set.</span></span> <span data-ttu-id="a69f6-158">Další informace najdete v tématu [Správa dostupnosti virtuálních počítačů](manage-availability.md).</span><span class="sxs-lookup"><span data-stu-id="a69f6-158">For more information, see [managing the availability of VMs](manage-availability.md).</span></span>

<span data-ttu-id="a69f6-159">Vytvořit sadu dostupnosti pro virtuální počítač s [az virtuálních počítačů sady dostupnosti. vytváření](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="a69f6-159">Create an availability set for your VM with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="a69f6-160">Následující příklad vytvoří sadu s názvem dostupnosti *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="a69f6-160">The following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

<span data-ttu-id="a69f6-161">Výstup domén selhání poznámky a aktualizace domény:</span><span class="sxs-lookup"><span data-stu-id="a69f6-161">The output notes fault domains and update domains:</span></span>

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


## <a name="create-the-linux-vms"></a><span data-ttu-id="a69f6-162">Vytvořit virtuální počítače Linux</span><span class="sxs-lookup"><span data-stu-id="a69f6-162">Create the Linux VMs</span></span>
<span data-ttu-id="a69f6-163">Jste vytvořili síťovým prostředkům pro podporu přístupné z Internetu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a69f6-163">You've created the network resources to support Internet-accessible VMs.</span></span> <span data-ttu-id="a69f6-164">Teď vytvořte virtuální počítač a zabezpečte ji pomocí klíče SSH.</span><span class="sxs-lookup"><span data-stu-id="a69f6-164">Now create a VM and secure it with an SSH key.</span></span> <span data-ttu-id="a69f6-165">V tomto případě vytvoříme vytvořit Ubuntu podle nejnovější LTS virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a69f6-165">In this case, we're going to create an Ubuntu VM based on the most recent LTS.</span></span> <span data-ttu-id="a69f6-166">Můžete najít další Image s [seznamu obrázků virtuálních počítačů az](/cli/azure/vm/image#list), jak je popsáno v [hledání Image virtuálního počítače Azure](cli-ps-findimage.md).</span><span class="sxs-lookup"><span data-stu-id="a69f6-166">You can find additional images with [az vm image list](/cli/azure/vm/image#list), as described in [finding Azure VM images](cli-ps-findimage.md).</span></span>

<span data-ttu-id="a69f6-167">Můžeme také zadejte klíč SSH pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="a69f6-167">We also specify an SSH key to use for authentication.</span></span> <span data-ttu-id="a69f6-168">Pokud nemáte páru veřejného klíče SSH, můžete [jejich vytvoření](mac-create-ssh-keys.md) nebo použít `--generate-ssh-keys` parametr, aby pro vás vytvořil.</span><span class="sxs-lookup"><span data-stu-id="a69f6-168">If you do not have an SSH public key pair, you can [create them](mac-create-ssh-keys.md) or use the `--generate-ssh-keys` parameter to create them for you.</span></span> <span data-ttu-id="a69f6-169">Pokud jste již pár klíčů, tento parametr používá existující klíče v `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="a69f6-169">If you already a key pair, this parameter uses existing keys in `~/.ssh`.</span></span>

<span data-ttu-id="a69f6-170">Vytvoření virtuálního počítače tak, že převedou všechny naše prostředky a informace o společně s [vytvořit virtuální počítač az](/cli/azure/vm#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="a69f6-170">Create the VM by bringing all our resources and information together with the [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="a69f6-171">Následující příklad vytvoří virtuální počítač s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="a69f6-171">The following example creates a VM named *myVM*:</span></span>

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

<span data-ttu-id="a69f6-172">SSH pro virtuální počítač s položkou DNS, kterou jste zadali při vytvoření veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="a69f6-172">SSH to your VM with the DNS entry you provided when you created the public IP address.</span></span> <span data-ttu-id="a69f6-173">To `fqdn` je zobrazené ve výstupu, jako je vytváření virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="a69f6-173">This `fqdn` is shown in the output as you create your VM:</span></span>

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

<span data-ttu-id="a69f6-174">Výstup:</span><span class="sxs-lookup"><span data-stu-id="a69f6-174">Output:</span></span>

```bash
The authenticity of host 'mypublicdns.eastus.cloudapp.azure.com (13.90.94.252)' can't be established.
ECDSA key fingerprint is SHA256:SylINP80Um6XRTvWiFaNz+H+1jcrKB1IiNgCDDJRj6A.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.eastus.cloudapp.azure.com,13.90.94.252' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-81-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@myVM:~$
```

<span data-ttu-id="a69f6-175">Můžete nainstalovat NGINX a sledovat provoz toku k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="a69f6-175">You can install NGINX and see the traffic flow to the VM.</span></span> <span data-ttu-id="a69f6-176">Nainstalujte NGINX následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a69f6-176">Install NGINX as follows:</span></span>

```bash
sudo apt-get install -y nginx
```

<span data-ttu-id="a69f6-177">Pokud chcete zobrazit výchozí web NGINX v akci, otevřete webový prohlížeč a zadejte vaše plně kvalifikovaný název domény:</span><span class="sxs-lookup"><span data-stu-id="a69f6-177">To see the default NGINX site in action, open your web browser and enter your FQDN:</span></span>

![Výchozí web NGINX na vašem virtuálním počítači](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a><span data-ttu-id="a69f6-179">Exportovat jako šablonu</span><span class="sxs-lookup"><span data-stu-id="a69f6-179">Export as a template</span></span>
<span data-ttu-id="a69f6-180">Co když teď chcete vytvořit další vývoj prostředí se stejnými parametry nebo produkčním prostředí odpovídající ho?</span><span class="sxs-lookup"><span data-stu-id="a69f6-180">What if you now want to create an additional development environment with the same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="a69f6-181">Správce prostředků používá šablony JSON, které definují všech parametrů pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="a69f6-181">Resource Manager uses JSON templates that define all the parameters for your environment.</span></span> <span data-ttu-id="a69f6-182">Můžete vytvořit na celé prostředí podle odkazující na tuto šablonu JSON.</span><span class="sxs-lookup"><span data-stu-id="a69f6-182">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="a69f6-183">Můžete [vytvořit šablony JSON ručně](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo exportovat stávajícího prostředí pro vytvoření šablony JSON pro vás.</span><span class="sxs-lookup"><span data-stu-id="a69f6-183">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment to create the JSON template for you.</span></span> <span data-ttu-id="a69f6-184">Použití [export skupiny az](/cli/azure/group#export) export skupiny prostředků následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a69f6-184">Use [az group export](/cli/azure/group#export) to export your resource group as follows:</span></span>

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

<span data-ttu-id="a69f6-185">Tento příkaz vytvoří `myResourceGroup.json` souboru v aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="a69f6-185">This command creates the `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="a69f6-186">Při vytváření prostředí z této šablony, zobrazí se výzva pro všechny názvy prostředků.</span><span class="sxs-lookup"><span data-stu-id="a69f6-186">When you create an environment from this template, you are prompted for all the resource names.</span></span> <span data-ttu-id="a69f6-187">Tyto názvy v souboru šablony můžete naplnit tak, že přidáte `--include-parameter-default-value` parametru `az group export` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a69f6-187">You can populate these names in your template file by adding the `--include-parameter-default-value` parameter to the `az group export` command.</span></span> <span data-ttu-id="a69f6-188">Upravte svou šablonu JSON zadat názvy prostředků, nebo [vytvoření souboru parameters.JSON tímto kódem](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) určující názvy prostředků.</span><span class="sxs-lookup"><span data-stu-id="a69f6-188">Edit your JSON template to specify the resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies the resource names.</span></span>

<span data-ttu-id="a69f6-189">K vytvoření prostředí ze šablony, použijte [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a69f6-189">To create an environment from your template, use [az group deployment create](/cli/azure/group/deployment#create) as follows:</span></span>

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

<span data-ttu-id="a69f6-190">Můžete chtít číst [Další informace o tom, jak nasadit ze šablon](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a69f6-190">You might want to read [more about how to deploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="a69f6-191">Další informace o tom, jak přírůstkově aktualizovat prostředí, použijte soubor parametrů a přístup k šablony z jedno umístění úložiště.</span><span class="sxs-lookup"><span data-stu-id="a69f6-191">Learn about how to incrementally update environments, use the parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a69f6-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a69f6-192">Next steps</span></span>
<span data-ttu-id="a69f6-193">Nyní jste připraveni začít pracovat s více síťovými součástmi a virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="a69f6-193">Now you're ready to begin working with multiple networking components and VMs.</span></span> <span data-ttu-id="a69f6-194">Tato ukázka prostředí můžete vytváří vaše aplikace pomocí základních komponent zavedená sem.</span><span class="sxs-lookup"><span data-stu-id="a69f6-194">You can use this sample environment to build out your application by using the core components introduced here.</span></span>
