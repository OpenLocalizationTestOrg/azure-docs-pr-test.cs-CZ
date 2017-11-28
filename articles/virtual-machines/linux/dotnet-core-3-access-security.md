---
title: "aaaAccess a zabezpečení v šablonách Azure pro virtuální počítače s Linuxem | Microsoft Docs"
description: "Virtuální počítač Azure DotNet základní kurz"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 07e47189-680e-4102-a8d4-5a8eb9c00213
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 88fedc8287c1f8ab8397a03ddefe1e60a686815e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="access-and-security-in-azure-resource-manager-templates-for-linux-vms"></a><span data-ttu-id="cd006-103">Přístup a zabezpečení v šablonách Azure Resource Manageru pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="cd006-103">Access and security in Azure Resource Manager templates for Linux VMs</span></span>

<span data-ttu-id="cd006-104">Aplikace hostované v Azure pravděpodobně nutné toobe přístup přes hello internet nebo síť VPN nebo připojení Express Route s Azure.</span><span class="sxs-lookup"><span data-stu-id="cd006-104">Applications hosted in Azure likely need toobe access over hello internet or a VPN / Express Route connection with Azure.</span></span> <span data-ttu-id="cd006-105">S ukázkou aplikace hello Hudba úložiště, je k dispozici na webu hello hello internet s veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="cd006-105">With hello Music Store application sample, hello web site is made available on hello internet with a public IP address.</span></span> <span data-ttu-id="cd006-106">S přístupem k vytvoření by měly být zabezpečeny připojení toohello aplikací a přístupu toohello prostředky virtuálního počítače sami.</span><span class="sxs-lookup"><span data-stu-id="cd006-106">With access established, connections toohello application and access toohello virtual machine resources themselves should be secured.</span></span> <span data-ttu-id="cd006-107">Tento přístup zabezpečení je součástí skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="cd006-107">This access security is provided with a Network Security Group.</span></span> 

<span data-ttu-id="cd006-108">Tento dokument podrobně popisuje, jak jsou zabezpečená hello aplikaci Store Hudba v šablony Azure Resource Manageru ukázkový hello.</span><span class="sxs-lookup"><span data-stu-id="cd006-108">This document details how hello Music Store application is secured in hello sample Azure Resource Manager template.</span></span> <span data-ttu-id="cd006-109">Jsou vyznačené všechny závislosti a unikátní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="cd006-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="cd006-110">Nejvhodnější hello předem nasaďte instanci hello řešení tooyour předplatného Azure a pracovní společně s hello šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="cd006-110">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="cd006-111">úplnou šablonu Hello naleznete zde – [Hudba úložiště nasazení na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="cd006-111">hello complete template can be found here – [Music Store Deployment on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span> 

## <a name="public-ip-address"></a><span data-ttu-id="cd006-112">Veřejná IP adresa</span><span class="sxs-lookup"><span data-stu-id="cd006-112">Public IP Address</span></span>
<span data-ttu-id="cd006-113">tooprovide veřejný přístup tooan prostředků Azure, prostředek veřejné IP adresy lze použít.</span><span class="sxs-lookup"><span data-stu-id="cd006-113">tooprovide public access tooan Azure resource, a public IP address resource can be used.</span></span> <span data-ttu-id="cd006-114">Veřejnou IP adresu můžete nakonfigurovat statickou nebo dynamickou adresu IP.</span><span class="sxs-lookup"><span data-stu-id="cd006-114">Public IP address can be configured with a static or dynamic IP address.</span></span> <span data-ttu-id="cd006-115">Pokud se používá s dynamickou adresou a hello virtuální počítač je zastaven a navrácena, odeberou se hello adresy.</span><span class="sxs-lookup"><span data-stu-id="cd006-115">If a dynamic address is used, and hello virtual machine is stopped and deallocated, hello addresses is removed.</span></span> <span data-ttu-id="cd006-116">Když se počítač hello znovu spustí, může přiřadit jinou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="cd006-116">When hello machine is started again, it may be assigned a different public IP address.</span></span> <span data-ttu-id="cd006-117">tooprevent na IP adresu z změna, můžete použít vyhrazenou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="cd006-117">tooprevent an IP address from changing, a reserved IP address can be used.</span></span> 

<span data-ttu-id="cd006-118">Šablony Azure Resource Manageru tooan pomocí hello Visual Studio nový Průvodce přidáním prostředku, lze přidat na veřejnou IP adresu nebo vložením platný kód JSON do šablony.</span><span class="sxs-lookup"><span data-stu-id="cd006-118">A Public IP Address can be added tooan Azure Resource Manager template using hello Visual Studio Add New Resource Wizard, or by inserting valid JSON into a template.</span></span> 

<span data-ttu-id="cd006-119">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [veřejnou IP adresu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).</span><span class="sxs-lookup"><span data-stu-id="cd006-119">Follow this link toosee hello JSON sample within hello Resource Manager template – [Public IP Address](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicipaddressName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "public-ip-front"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

<span data-ttu-id="cd006-120">Veřejnou IP adresu lze přidružit k virtuální síťový adaptér, nebo nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="cd006-120">A Public IP Address can be associated with a Virtual Network Adapter, or a Load Balancer.</span></span> <span data-ttu-id="cd006-121">V tomto příkladu protože hello Hudba úložiště webu vyrovnáváno zatížení napříč několika virtuálních počítačů, je hello veřejnou IP adresu připojené toohello Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="cd006-121">In this example, because hello Music Store website is load balanced across several virtual machines, hello Public IP Address is attached toohello Load Balancer.</span></span>

<span data-ttu-id="cd006-122">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [přidružení veřejné IP adresy pomocí nástroje pro vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).</span><span class="sxs-lookup"><span data-stu-id="cd006-122">Follow this link toosee hello JSON sample within hello Resource Manager template – [Public IP Address association with Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).</span></span>

```json
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

<span data-ttu-id="cd006-123">Hello veřejnou IP adresu jako z pohledu hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cd006-123">hello public IP Address as seen from hello Azure portal.</span></span> <span data-ttu-id="cd006-124">Všimněte si, že hello veřejná IP adresa je přidružená tooa nástroj pro vyrovnávání zatížení a není virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cd006-124">Notice that hello public IP address is associated tooa load balancer and not a virtual machine.</span></span> <span data-ttu-id="cd006-125">Nástroje pro vyrovnávání zatížení sítě jsou podrobně popsané v dokumentu další hello této řady.</span><span class="sxs-lookup"><span data-stu-id="cd006-125">Network load balancers are detailed in hello next document of this series.</span></span>

![Veřejná IP adresa](./media/dotnet-core-3-access-security/pubip.png)

<span data-ttu-id="cd006-127">Další informace o Azure veřejné IP adresy najdete v tématu [IP adresách v Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="cd006-127">For more information on Azure Public IP Addresses, see [IP addresses in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span></span>

## <a name="network-security-group"></a><span data-ttu-id="cd006-128">Skupina zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="cd006-128">Network Security Group</span></span>
<span data-ttu-id="cd006-129">Jakmile přístup zavedených tooAzure prostředky, už musí být tento přístup omezený.</span><span class="sxs-lookup"><span data-stu-id="cd006-129">Once access has been established tooAzure resources, this access should be limited.</span></span> <span data-ttu-id="cd006-130">Pro virtuální počítače Azure se provádí zabezpečený přístup pomocí skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="cd006-130">For Azure virtual machines, secure access is accomplished using a network security group.</span></span> <span data-ttu-id="cd006-131">S ukázkou aplikace hello úložiště Hudba všechny přístup toohello virtuální počítač je omezena kromě přes port 80 pro přístup protokolu http a port 22 pro SSH přístup.</span><span class="sxs-lookup"><span data-stu-id="cd006-131">With hello Music Store application sample, all access toohello virtual machine is restricted except for over port 80 for http access, and port 22 for SSH access.</span></span> <span data-ttu-id="cd006-132">Šablony Azure Resource Manageru tooan pomocí hello Visual Studio nový Průvodce přidáním prostředku, lze přidat skupinu zabezpečení sítě, nebo vložením platný kód JSON do šablony.</span><span class="sxs-lookup"><span data-stu-id="cd006-132">A Network Security Group can be added tooan Azure Resource Manager template using hello Visual Studio Add New Resource Wizard, or by inserting valid JSON into a template.</span></span>

<span data-ttu-id="cd006-133">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [skupinu zabezpečení sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).</span><span class="sxs-lookup"><span data-stu-id="cd006-133">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Security Group](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).</span></span>

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('nsgfront')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "nsg-front"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
}
```

<span data-ttu-id="cd006-134">V tomto příkladu je skupina zabezpečení sítě hello spojený s objektu podsítě hello deklarované v prostředku hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="cd006-134">In this example, hello network security group is associate with hello subnet object declared in hello Virtual Network resource.</span></span> 

<span data-ttu-id="cd006-135">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [přidružení skupiny zabezpečení sítě pomocí virtuální sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).</span><span class="sxs-lookup"><span data-stu-id="cd006-135">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Security Group association with Virtual Network](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).</span></span>

```json
"subnets": [
  {
    "name": "[variables('subnetName')]",
    "properties": {
      "addressPrefix": "10.0.0.0/24",
      "networkSecurityGroup": {
        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
      }
    }
  }
```

<span data-ttu-id="cd006-136">Zde je, jakou skupinu zabezpečení sítě hello vypadá z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cd006-136">Here is what hello network security group looks like from hello Azure portal.</span></span> <span data-ttu-id="cd006-137">Všimněte si, že skupina NSG můžete přidružit podsítě a / nebo síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cd006-137">Notice that an NSG can be associate with a subnet and / or network interface.</span></span> <span data-ttu-id="cd006-138">V takovém případě je hello NSG přidružené tooa podsíť.</span><span class="sxs-lookup"><span data-stu-id="cd006-138">In this case, hello NSG is associated tooa subnet.</span></span> <span data-ttu-id="cd006-139">V této konfiguraci hello příchozí pravidla použít tooall virtuální počítače připojené toohello podsítě.</span><span class="sxs-lookup"><span data-stu-id="cd006-139">In this configuration, hello inbound rules apply tooall virtual machines connected toohello subnet.</span></span>

![Skupina zabezpečení sítě](./media/dotnet-core-3-access-security/nsg.png)

<span data-ttu-id="cd006-141">Podrobné informace na skupiny zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).</span><span class="sxs-lookup"><span data-stu-id="cd006-141">For in-depth information on Network Security Groups, see [What is a Network Security Group](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).</span></span>

## <a name="next-step"></a><span data-ttu-id="cd006-142">Další krok</span><span class="sxs-lookup"><span data-stu-id="cd006-142">Next step</span></span>
<hr>

[<span data-ttu-id="cd006-143">Krok 3 – dostupnost a dosah v šablonách Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="cd006-143">Step 3 - Availability and Scale in Azure Resource Manager Templates</span></span>](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

