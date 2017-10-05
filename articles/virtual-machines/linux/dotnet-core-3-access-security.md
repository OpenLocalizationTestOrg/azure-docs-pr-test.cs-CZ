---
title: "Přístup a zabezpečení v šablonách Azure pro virtuální počítače s Linuxem | Microsoft Docs"
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
ms.openlocfilehash: 4ef4fc4109d4ccf4be273608bacacce820deb281
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="access-and-security-in-azure-resource-manager-templates-for-linux-vms"></a><span data-ttu-id="badea-103">Přístup a zabezpečení v šablonách Azure Resource Manageru pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="badea-103">Access and security in Azure Resource Manager templates for Linux VMs</span></span>

<span data-ttu-id="badea-104">Aplikace hostované v Azure pravděpodobně muset být přístup přes internet nebo síť VPN nebo připojení Express Route s Azure.</span><span class="sxs-lookup"><span data-stu-id="badea-104">Applications hosted in Azure likely need to be access over the internet or a VPN / Express Route connection with Azure.</span></span> <span data-ttu-id="badea-105">Pomocí aplikace Ukázky hudby úložiště je webový server k dispozici na Internetu s veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="badea-105">With the Music Store application sample, the web site is made available on the internet with a public IP address.</span></span> <span data-ttu-id="badea-106">S přístupem k vytvoření by měly být zabezpečeny připojení k aplikaci a přístup k prostředkům virtuálního počítače, sami.</span><span class="sxs-lookup"><span data-stu-id="badea-106">With access established, connections to the application and access to the virtual machine resources themselves should be secured.</span></span> <span data-ttu-id="badea-107">Tento přístup zabezpečení je součástí skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="badea-107">This access security is provided with a Network Security Group.</span></span> 

<span data-ttu-id="badea-108">Tento dokument podrobně popisuje, jak jsou zabezpečená aplikaci Store Hudba v ukázkové šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="badea-108">This document details how the Music Store application is secured in the sample Azure Resource Manager template.</span></span> <span data-ttu-id="badea-109">Jsou vyznačené všechny závislosti a unikátní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="badea-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="badea-110">Pro dosažení co nejlepších výsledků, předem nasaďte instanci řešení, aby vaše předplatné Azure a pracovní společně s šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="badea-110">For the best experience, pre-deploy an instance of the solution to your Azure subscription and work along with the Azure Resource Manager template.</span></span> <span data-ttu-id="badea-111">Úplnou šablonu naleznete zde – [Hudba úložiště nasazení na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="badea-111">The complete template can be found here – [Music Store Deployment on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span> 

## <a name="public-ip-address"></a><span data-ttu-id="badea-112">Veřejná IP adresa</span><span class="sxs-lookup"><span data-stu-id="badea-112">Public IP Address</span></span>
<span data-ttu-id="badea-113">Pokud chcete zadat veřejný přístup k prostředku Azure, lze prostředek veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="badea-113">To provide public access to an Azure resource, a public IP address resource can be used.</span></span> <span data-ttu-id="badea-114">Veřejnou IP adresu můžete nakonfigurovat statickou nebo dynamickou adresu IP.</span><span class="sxs-lookup"><span data-stu-id="badea-114">Public IP address can be configured with a static or dynamic IP address.</span></span> <span data-ttu-id="badea-115">Pokud se používá s dynamickou adresou a virtuální počítač je zastaven a navrácena, odeberou se adresy.</span><span class="sxs-lookup"><span data-stu-id="badea-115">If a dynamic address is used, and the virtual machine is stopped and deallocated, the addresses is removed.</span></span> <span data-ttu-id="badea-116">Když se počítač spustí znovu, může přiřadit jinou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="badea-116">When the machine is started again, it may be assigned a different public IP address.</span></span> <span data-ttu-id="badea-117">Abyste zabránili změna IP adresy, lze použít vyhrazenou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="badea-117">To prevent an IP address from changing, a reserved IP address can be used.</span></span> 

<span data-ttu-id="badea-118">Veřejnou IP adresu lze přidat do šablony Azure Resource Manager pomocí Visual Studio nový Průvodce přidáním prostředku nebo vložením platný kód JSON do šablony.</span><span class="sxs-lookup"><span data-stu-id="badea-118">A Public IP Address can be added to an Azure Resource Manager template using the Visual Studio Add New Resource Wizard, or by inserting valid JSON into a template.</span></span> 

<span data-ttu-id="badea-119">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [veřejnou IP adresu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).</span><span class="sxs-lookup"><span data-stu-id="badea-119">Follow this link to see the JSON sample within the Resource Manager template – [Public IP Address](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).</span></span>

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

<span data-ttu-id="badea-120">Veřejnou IP adresu lze přidružit k virtuální síťový adaptér, nebo nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="badea-120">A Public IP Address can be associated with a Virtual Network Adapter, or a Load Balancer.</span></span> <span data-ttu-id="badea-121">V tomto příkladu protože web Hudba úložiště je vyrovnáváno zatížení napříč několika virtuálních počítačů, je veřejná IP adresa připojený k vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="badea-121">In this example, because the Music Store website is load balanced across several virtual machines, the Public IP Address is attached to the Load Balancer.</span></span>

<span data-ttu-id="badea-122">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [přidružení veřejné IP adresy pomocí nástroje pro vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).</span><span class="sxs-lookup"><span data-stu-id="badea-122">Follow this link to see the JSON sample within the Resource Manager template – [Public IP Address association with Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).</span></span>

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

<span data-ttu-id="badea-123">Veřejnou IP adresu jako zaznamenané z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="badea-123">The public IP Address as seen from the Azure portal.</span></span> <span data-ttu-id="badea-124">Všimněte si, že veřejná IP adresa je přidružená k vyrovnávání zatížení a není virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="badea-124">Notice that the public IP address is associated to a load balancer and not a virtual machine.</span></span> <span data-ttu-id="badea-125">Nástroje pro vyrovnávání zatížení sítě jsou podrobně popsané v další dokument Tato řada.</span><span class="sxs-lookup"><span data-stu-id="badea-125">Network load balancers are detailed in the next document of this series.</span></span>

![Veřejná IP adresa](./media/dotnet-core-3-access-security/pubip.png)

<span data-ttu-id="badea-127">Další informace o Azure veřejné IP adresy najdete v tématu [IP adresách v Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="badea-127">For more information on Azure Public IP Addresses, see [IP addresses in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span></span>

## <a name="network-security-group"></a><span data-ttu-id="badea-128">Skupina zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="badea-128">Network Security Group</span></span>
<span data-ttu-id="badea-129">Po vytvoření přístup k prostředkům Azure tento přístup by měla být omezená.</span><span class="sxs-lookup"><span data-stu-id="badea-129">Once access has been established to Azure resources, this access should be limited.</span></span> <span data-ttu-id="badea-130">Pro virtuální počítače Azure se provádí zabezpečený přístup pomocí skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="badea-130">For Azure virtual machines, secure access is accomplished using a network security group.</span></span> <span data-ttu-id="badea-131">Pomocí aplikace Ukázky hudby úložiště je veškerý přístup k virtuálnímu počítači s omezeným přístupem, s výjimkou přes port 80 pro přístup protokolu http a port 22 pro SSH přístup.</span><span class="sxs-lookup"><span data-stu-id="badea-131">With the Music Store application sample, all access to the virtual machine is restricted except for over port 80 for http access, and port 22 for SSH access.</span></span> <span data-ttu-id="badea-132">Skupina zabezpečení sítě lze přidat do šablony Azure Resource Manager pomocí Visual Studio nový Průvodce přidáním prostředku nebo vložením platný kód JSON do šablony.</span><span class="sxs-lookup"><span data-stu-id="badea-132">A Network Security Group can be added to an Azure Resource Manager template using the Visual Studio Add New Resource Wizard, or by inserting valid JSON into a template.</span></span>

<span data-ttu-id="badea-133">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [skupinu zabezpečení sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).</span><span class="sxs-lookup"><span data-stu-id="badea-133">Follow this link to see the JSON sample within the Resource Manager template – [Network Security Group](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).</span></span>

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

<span data-ttu-id="badea-134">Skupina zabezpečení sítě v tomto příkladu je spojený s objektu podsítě, které jsou deklarované v rámci virtuální sítě prostředku.</span><span class="sxs-lookup"><span data-stu-id="badea-134">In this example, the network security group is associate with the subnet object declared in the Virtual Network resource.</span></span> 

<span data-ttu-id="badea-135">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [přidružení skupiny zabezpečení sítě pomocí virtuální sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).</span><span class="sxs-lookup"><span data-stu-id="badea-135">Follow this link to see the JSON sample within the Resource Manager template – [Network Security Group association with Virtual Network](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).</span></span>

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

<span data-ttu-id="badea-136">Zde je, jak skupinu zabezpečení sítě vypadá z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="badea-136">Here is what the network security group looks like from the Azure portal.</span></span> <span data-ttu-id="badea-137">Všimněte si, že skupina NSG můžete přidružit podsítě a / nebo síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="badea-137">Notice that an NSG can be associate with a subnet and / or network interface.</span></span> <span data-ttu-id="badea-138">V takovém případě je přidružena k podsíti NSG.</span><span class="sxs-lookup"><span data-stu-id="badea-138">In this case, the NSG is associated to a subnet.</span></span> <span data-ttu-id="badea-139">V této konfiguraci příchozích pravidel platí pro všechny virtuální počítače připojené k podsíti.</span><span class="sxs-lookup"><span data-stu-id="badea-139">In this configuration, the inbound rules apply to all virtual machines connected to the subnet.</span></span>

![Skupina zabezpečení sítě](./media/dotnet-core-3-access-security/nsg.png)

<span data-ttu-id="badea-141">Podrobné informace na skupiny zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).</span><span class="sxs-lookup"><span data-stu-id="badea-141">For in-depth information on Network Security Groups, see [What is a Network Security Group](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).</span></span>

## <a name="next-step"></a><span data-ttu-id="badea-142">Další krok</span><span class="sxs-lookup"><span data-stu-id="badea-142">Next step</span></span>
<hr>

[<span data-ttu-id="badea-143">Krok 3 – dostupnost a dosah v šablonách Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="badea-143">Step 3 - Availability and Scale in Azure Resource Manager Templates</span></span>](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

