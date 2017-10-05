---
title: "Dostupnost a dosah v šablonách Azure Resource Manageru | Microsoft Docs"
description: "Virtuální počítač Azure DotNet základní kurz"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8fcfea79-f017-4658-8c51-74242fcfb7f6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0c0250b8152ed31b9a5d8b42ae139c9b38da0984
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="availability-and-scale-in-azure-resource-manager-templates-for-linux-vms"></a><span data-ttu-id="58bc6-103">Dostupnost a dosah v šablonách Azure Resource Manageru pro virtuální počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="58bc6-103">Availability and scale in Azure Resource Manager templates for Linux VMs</span></span>

<span data-ttu-id="58bc6-104">Dostupnost a dosah viz provozu a schopnost potřeby.</span><span class="sxs-lookup"><span data-stu-id="58bc6-104">Availability and scale refer to uptime and the ability to meet demand.</span></span> <span data-ttu-id="58bc6-105">Pokud aplikace musí být 99,9 % času, musí mít architekturu, která umožňuje více souběžných výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="58bc6-105">If an application must be up 99.9% of the time, it needs to have an architecture that allows for multiple concurrent compute resources.</span></span> <span data-ttu-id="58bc6-106">Například místo jen jeden web, konfigurace s vyšší úrovní dostupnosti obsahuje více instancí stejného webu, vyrovnávání technologie před jejich.</span><span class="sxs-lookup"><span data-stu-id="58bc6-106">For instance, rather than having a single website, a configuration with a higher level of availability includes multiple instances of the same site, with balancing technology in front of them.</span></span> <span data-ttu-id="58bc6-107">V této konfiguraci jednu instanci aplikace můžete ukončena kvůli údržbě, zatímco zbývající i nadále fungovat.</span><span class="sxs-lookup"><span data-stu-id="58bc6-107">In this configuration, one instance of the application can be taken down for maintenance, while the remaining continue to function.</span></span> <span data-ttu-id="58bc6-108">Škálování na straně druhé odkazuje aplikace schopnost poskytovat vyžádání.</span><span class="sxs-lookup"><span data-stu-id="58bc6-108">Scale on the other hand refers to an applications ability to serve demand.</span></span> <span data-ttu-id="58bc6-109">S zatížení vyrovnáváním aplikace, přidání nebo odebrání instance z fondu umožňuje aplikaci škálovat podle požadavků.</span><span class="sxs-lookup"><span data-stu-id="58bc6-109">With a load balanced application, adding or removing instances from the pool allows an application to scale to meet demand.</span></span>

<span data-ttu-id="58bc6-110">Tento dokument podrobně popisuje konfiguraci nasazení ukázkové Hudba úložiště pro dostupnost a škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="58bc6-110">This document details how the Music Store sample deployment is configured for availability and scale.</span></span> <span data-ttu-id="58bc6-111">Jsou vyznačené všechny závislosti a unikátní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="58bc6-111">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="58bc6-112">Pro dosažení co nejlepších výsledků, předem nasaďte instanci řešení, aby vaše předplatné Azure a pracovní společně s šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="58bc6-112">For the best experience, pre-deploy an instance of the solution to your Azure subscription and work along with the Azure Resource Manager template.</span></span> <span data-ttu-id="58bc6-113">Úplnou šablonu naleznete zde – [Hudba úložiště nasazení na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span><span class="sxs-lookup"><span data-stu-id="58bc6-113">The complete template can be found here – [Music Store Deployment on Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).</span></span>

## <a name="availability-set"></a><span data-ttu-id="58bc6-114">Skupina dostupnosti</span><span class="sxs-lookup"><span data-stu-id="58bc6-114">Availability Set</span></span>
<span data-ttu-id="58bc6-115">Skupiny dostupnosti logicky platí virtuálních počítačů Azure pro fyzické hostitele a jiné infrastruktury komponenty, například napájení a fyzický síťový hardware.</span><span class="sxs-lookup"><span data-stu-id="58bc6-115">An Availability Set logically spans Azure Virtual Machines across physical hosts and other infrastructural components such as power supplies and physical networking hardware.</span></span> <span data-ttu-id="58bc6-116">Skupiny dostupnosti Ujistěte se, že během údržby, selhání zařízení nebo jiné výpadek, ne všechny virtuální počítače, budou provedeny.</span><span class="sxs-lookup"><span data-stu-id="58bc6-116">Availability sets ensure that during maintenance, device failure, or other down time, not all virtual machines are effected.</span></span> <span data-ttu-id="58bc6-117">Skupiny dostupnosti mohou být přidány do šablony Azure Resource Manager pomocí Visual Studio nový Průvodce přidáním prostředku nebo vkládání platný kód JSON do šablony.</span><span class="sxs-lookup"><span data-stu-id="58bc6-117">An Availability Set can be added to an Azure Resource Manager template using the Visual Studio Add New Resource Wizard, or inserting valid JSON into a template.</span></span>

<span data-ttu-id="58bc6-118">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [sadu dostupnosti](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).</span><span class="sxs-lookup"><span data-stu-id="58bc6-118">Follow this link to see the JSON sample within the Resource Manager template – [Availability Set](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

<span data-ttu-id="58bc6-119">Skupiny dostupnosti je deklarován jako vlastnost prostředku virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="58bc6-119">An Availability Set is declared as a property of a Virtual Machine resource.</span></span> 

<span data-ttu-id="58bc6-120">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [dostupnost nastavená přidružení virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).</span><span class="sxs-lookup"><span data-stu-id="58bc6-120">Follow this link to see the JSON sample within the Resource Manager template – [Availability Set association with Virtual Machine](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).</span></span>

```json
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
<span data-ttu-id="58bc6-121">Dostupnost nastavit, jak je vidět na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="58bc6-121">The availability set as seen from the Azure portal.</span></span> <span data-ttu-id="58bc6-122">Každý virtuální počítač a údaje o konfiguraci jsou podrobně popsané v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="58bc6-122">Each virtual machine and details about the configuration are detailed here.</span></span>

![Skupina dostupnosti](./media/dotnet-core-4-availability-scale/aset.png)

<span data-ttu-id="58bc6-124">Podrobné informace o dostupnosti sadách najdete v tématu [Správa dostupnosti virtuálních počítačů](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="58bc6-124">For in-depth information on Availability Sets, see [Manage availability of virtual machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

## <a name="network-load-balancer"></a><span data-ttu-id="58bc6-125">Nástroj pro vyrovnávání zatížení sítě</span><span class="sxs-lookup"><span data-stu-id="58bc6-125">Network Load Balancer</span></span>
<span data-ttu-id="58bc6-126">Vzhledem k tomu, skupinu dostupnosti aplikace odolnost proti chybám, Vyrovnávání zatížení zpřístupní velký počet instancí aplikace na jednu síťovou adresu.</span><span class="sxs-lookup"><span data-stu-id="58bc6-126">Whereas an availability set provides application fault tolerance, a load balancer makes many instances of the application available on a single network address.</span></span> <span data-ttu-id="58bc6-127">Několik instancí aplikace může být hostovaný na velký počet virtuálních počítačů, každý z nich připojený ke službě Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="58bc6-127">Multiple instances of an application can be hosted on many virtual machines, each one connected to a load balancer.</span></span> <span data-ttu-id="58bc6-128">Protože přistupuje k aplikaci, nástroje pro vyrovnávání zatížení směrování příchozího požadavku napříč připojení členové.</span><span class="sxs-lookup"><span data-stu-id="58bc6-128">As the application is accessed, the load balancer routes the incoming request across the attached members.</span></span> <span data-ttu-id="58bc6-129">Nástroj pro vyrovnávání zatížení lze přidat pomocí Visual Studio nový Průvodce přidáním prostředku nebo vložením správně naformátovaný JSON prostředků do šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="58bc6-129">A Load Balancer can be added using the Visual Studio Add New Resource Wizard, or by inserting properly formatted JSON resource into the Azure Resource Manager template.</span></span>

<span data-ttu-id="58bc6-130">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [nástroj pro vyrovnávání zatížení sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).</span><span class="sxs-lookup"><span data-stu-id="58bc6-130">Follow this link to see the JSON sample within the Resource Manager template – [Network Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

<span data-ttu-id="58bc6-131">Protože ukázkové aplikace je přístup k Internetu s veřejnou IP adresu, tato adresa je přidružena s nástrojem pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="58bc6-131">Because the sample application is exposed to the internet with a public IP address, this address is associated with the load balancer.</span></span> 

<span data-ttu-id="58bc6-132">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [přidružení nástroj pro vyrovnávání zatížení sítě s veřejnou IP adresou](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).</span><span class="sxs-lookup"><span data-stu-id="58bc6-132">Follow this link to see the JSON sample within the Resource Manager template – [Network Load Balancer association with Public IP Address](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).</span></span>

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

<span data-ttu-id="58bc6-133">Z portálu Azure zobrazuje přehled nástroje pro vyrovnávání zatížení sítě přidružení veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="58bc6-133">From the Azure portal, the network load balancer overview shows the association with the public IP address.</span></span>

![Nástroj pro vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/nlb.png)

## <a name="load-balancer-rule"></a><span data-ttu-id="58bc6-135">Pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="58bc6-135">Load Balancer Rule</span></span>
<span data-ttu-id="58bc6-136">Při použití služby Vyrovnávání zatížení, jsou nakonfigurované pravidla, které řídí, jak se data jsou rovnoměrně mezi předpokládaných prostředků.</span><span class="sxs-lookup"><span data-stu-id="58bc6-136">When using a load balancer, rules are configured that control how traffic is balanced across the intended resources.</span></span> <span data-ttu-id="58bc6-137">S ukázkovou aplikaci Store Hudba provoz dorazí na portu 80 veřejnou IP adresu a je distribuován do portu 80 všech virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="58bc6-137">With the sample Music Store application, traffic arrives on port 80 of the public IP address and is distributed across port 80 of all virtual machines.</span></span> 

<span data-ttu-id="58bc6-138">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [pravidlo Vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).</span><span class="sxs-lookup"><span data-stu-id="58bc6-138">Follow this link to see the JSON sample within the Resource Manager template – [Load Balancer Rule](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).</span></span>

```json
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

<span data-ttu-id="58bc6-139">Zobrazení pravidlo Vyrovnávání zatížení sítě z portálu.</span><span class="sxs-lookup"><span data-stu-id="58bc6-139">A view of the network load balancer rule from the portal.</span></span>

![Pravidlo Vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/lbrule.png)

## <a name="load-balancer-probe"></a><span data-ttu-id="58bc6-141">Sondu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="58bc6-141">Load Balancer Probe</span></span>
<span data-ttu-id="58bc6-142">Vyrovnávání zatížení musí také monitorovat každý virtuální počítač tak, aby se zpracovat požadavky pouze na systémy.</span><span class="sxs-lookup"><span data-stu-id="58bc6-142">The load balancer also needs to monitor each virtual machine so that requests are served only to running systems.</span></span> <span data-ttu-id="58bc6-143">Toto monitorování probíhá podle konstantní zjišťování předem definovaný port.</span><span class="sxs-lookup"><span data-stu-id="58bc6-143">This monitoring takes place by constant probing of a pre-defined port.</span></span> <span data-ttu-id="58bc6-144">Hudba úložiště nasazení se konfiguruje testovat na zahrnuté všechny virtuální počítače, port 80.</span><span class="sxs-lookup"><span data-stu-id="58bc6-144">The Music Store deployment is configured to probe port 80 on all included virtual machines.</span></span> 

<span data-ttu-id="58bc6-145">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [sběru dat služby Vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).</span><span class="sxs-lookup"><span data-stu-id="58bc6-145">Follow this link to see the JSON sample within the Resource Manager template – [Load Balancer Probe](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).</span></span>

```json
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

<span data-ttu-id="58bc6-146">Sondu nástroje pro vyrovnávání zatížení vidět na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="58bc6-146">The load balancer probe seen from the Azure portal.</span></span>

![Sondu nástroje pro vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/lbprobe.png)

## <a name="inbound-nat-rules"></a><span data-ttu-id="58bc6-148">Příchozí pravidla NAT</span><span class="sxs-lookup"><span data-stu-id="58bc6-148">Inbound NAT Rules</span></span>
<span data-ttu-id="58bc6-149">Pokud používáte nástroj pro vyrovnávání zatížení, pravidla potřebovat k uvedení do místo, které poskytují vyrovnáváním přístup bez zatížení pro každý virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="58bc6-149">When using a Load Balancer, rules need to be put into place that provide non-load balanced access to each Virtual Machine.</span></span> <span data-ttu-id="58bc6-150">Například při vytváření připojení SSH pomocí každý virtuální počítač, tato komunikace by neměl být skupinu s vyrovnáváním zatížení, místo by měl být nakonfigurovaný předem určené cesty.</span><span class="sxs-lookup"><span data-stu-id="58bc6-150">For instance, when creating an SSH connection with each virtual machine, this traffic should not be load balanced, rather a pre-determined path should be configured.</span></span> <span data-ttu-id="58bc6-151">Předem určené cesty se konfiguruje pomocí prostředek příchozí pravidlo NAT.</span><span class="sxs-lookup"><span data-stu-id="58bc6-151">pre-determined paths are configured using an Inbound NAT Rule resource.</span></span> <span data-ttu-id="58bc6-152">Pomocí tohoto prostředku, příchozí komunikaci lze mapovat na jednotlivé virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="58bc6-152">Using this resource, inbound communication can be mapped to individual Virtual Machines.</span></span> 

<span data-ttu-id="58bc6-153">Se k aplikaci Store Hudba je port od 5000 namapovat port 22 na každém virtuálním počítači pro přístup SSH.</span><span class="sxs-lookup"><span data-stu-id="58bc6-153">With the Music Store application, a port starting at 5000 is mapped to port 22 on each Virtual Machine for SSH access.</span></span> <span data-ttu-id="58bc6-154">`copyindex()` Funkce slouží k zvýšit příchozí port, tak, aby druhý virtuální počítač přijímá port pro příchozí 5001, třetí 5002 a tak dále.</span><span class="sxs-lookup"><span data-stu-id="58bc6-154">The `copyindex()` function is used to increment the incoming port, such that the second Virtual Machine receives an incoming port of 5001, the third 5002, and so on.</span></span> 

<span data-ttu-id="58bc6-155">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [příchozí pravidla NAT](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).</span><span class="sxs-lookup"><span data-stu-id="58bc6-155">Follow this link to see the JSON sample within the Resource Manager template – [Inbound NAT Rules](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).</span></span> 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

<span data-ttu-id="58bc6-156">Jedním z příkladů příchozí pravidlo NAT, jak je vidět na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="58bc6-156">One example inbound NAT rule as seen in the Azure portal.</span></span> <span data-ttu-id="58bc6-157">Pro každý virtuální počítač v nasazení se vytvoří pravidlo SSH NAT.</span><span class="sxs-lookup"><span data-stu-id="58bc6-157">An SSH NAT rule is created for each virtual machine in the deployment.</span></span>

![Příchozí pravidlo NAT](./media/dotnet-core-4-availability-scale/natrule.png)

<span data-ttu-id="58bc6-159">Podrobnější informace o vyrovnávání zatížení sítě Azure, najdete v části [pro infrastrukturu Azure služby Vyrovnávání zatížení](../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="58bc6-159">For in-depth information on the Azure Network Load Balancer, see [Load balancing for Azure infrastructure services](../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="deploy-multiple-vms"></a><span data-ttu-id="58bc6-160">Nasazení více virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="58bc6-160">Deploy multiple VMs</span></span>
<span data-ttu-id="58bc6-161">Nakonec sadu dostupnosti nebo nástroj pro vyrovnávání zatížení efektivně fungovat, jsou požadovány více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="58bc6-161">Finally, for an Availability Set or Load Balancer to effectively function, multiple virtual machines are required.</span></span> <span data-ttu-id="58bc6-162">Víc virtuálních počítačů můžete nasadit pomocí funkce kopírování šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="58bc6-162">Multiple VMs can be deployed using the Azure Resource Manager template copy function.</span></span> <span data-ttu-id="58bc6-163">Pomocí funkce kopírování, není nutné definovat omezený počet virtuálních počítačů, místo tuto hodnotu můžete dynamicky poskytovat v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="58bc6-163">Using the copy function, it is not necessary to define a finite number of Virtual Machines, rather this value can be dynamically provided at the time of deployment.</span></span> <span data-ttu-id="58bc6-164">Funkci kopírování spotřebuje počet instancí pro vytvoření a nasazení správný počet virtuálních počítačů a přidružených prostředků obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="58bc6-164">The copy function consumes the number of instances to created and handles deploying the proper number of virtual machines and associated resources.</span></span>

<span data-ttu-id="58bc6-165">V šabloně Hudba úložiště ukázkové je definován parametr, který přebírá počet instancí.</span><span class="sxs-lookup"><span data-stu-id="58bc6-165">In the Music Store Sample template, a parameter is defined that takes in an instance count.</span></span> <span data-ttu-id="58bc6-166">Toto číslo se používá v rámci šablony při vytváření virtuálních počítačů a související prostředky.</span><span class="sxs-lookup"><span data-stu-id="58bc6-166">This number is used throughout the template when creating virtual machines and related resources.</span></span>

```json
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
}
```

<span data-ttu-id="58bc6-167">Na prostředek virtuálního počítače je kopírovací smyčkou zadaný název a číslo instance parametru použít k řízení počtu kopií výsledné.</span><span class="sxs-lookup"><span data-stu-id="58bc6-167">On the Virtual Machine resource, the copy loop is given a name and the number of instances parameter used to control the number of resulting copies.</span></span>

<span data-ttu-id="58bc6-168">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [funkci kopírování do virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300).</span><span class="sxs-lookup"><span data-stu-id="58bc6-168">Follow this link to see the JSON sample within the Resource Manager template – [Virtual Machine Copy Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300).</span></span> 

```json
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

<span data-ttu-id="58bc6-169">Na aktuální iteraci funkce kopii lze přistupovat pomocí `copyIndex()` funkce.</span><span class="sxs-lookup"><span data-stu-id="58bc6-169">The current iteration of the copy function can be accessed with the `copyIndex()` function.</span></span> <span data-ttu-id="58bc6-170">Hodnota indexu kopírování slouží k názvu virtuální počítače a další prostředky.</span><span class="sxs-lookup"><span data-stu-id="58bc6-170">The value of the copy index function can be used to name virtual machines and other resources.</span></span> <span data-ttu-id="58bc6-171">Například pokud se nasazuje dvě instance virtuálního počítače, potřebují mít odlišné názvy.</span><span class="sxs-lookup"><span data-stu-id="58bc6-171">For instance, if two instances of a virtual machine are deployed, they need different names.</span></span> <span data-ttu-id="58bc6-172">`copyIndex()` Funkci lze použít jako součást název virtuálního počítače vytvoří jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="58bc6-172">The `copyIndex()` function can be used as part of the virtual machine name to create a unique name.</span></span> <span data-ttu-id="58bc6-173">Příkladem `copyindex()` funkce použité pro účely pojmenování si můžete prohlédnout ve prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="58bc6-173">An example of the `copyindex()` function used for naming purposes can be seen in the Virtual Machine resource.</span></span> <span data-ttu-id="58bc6-174">Zde, název počítače je tvořen `vmName` parametr a `copyIndex()` funkce.</span><span class="sxs-lookup"><span data-stu-id="58bc6-174">Here, the computer name is a concatenation of the `vmName` parameter, and the `copyIndex()` function.</span></span> 

<span data-ttu-id="58bc6-175">Tento odkaz zobrazíte vzorku JSON šablony Resource Manageru – [funkci kopírování do indexu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319).</span><span class="sxs-lookup"><span data-stu-id="58bc6-175">Follow this link to see the JSON sample within the Resource Manager template – [Copy Index Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319).</span></span> 

```json
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

<span data-ttu-id="58bc6-176">`copyIndex` Funkce se používá v šabloně Hudba úložiště ukázkové několikrát.</span><span class="sxs-lookup"><span data-stu-id="58bc6-176">The `copyIndex` function is used several times in the Music Store sample template.</span></span> <span data-ttu-id="58bc6-177">Prostředky a funkce využívá `copyIndex` obsahovat vše specifické pro jednu instanci virtuálního počítače jako síťové rozhraní, pravidla nástroje pro vyrovnávání zatížení, a všechny závisí na funkce.</span><span class="sxs-lookup"><span data-stu-id="58bc6-177">Resources and functions utilizing `copyIndex` include anything specific to a single instance of the virtual machine such as network interface, load balancer rules, and any depends on functions.</span></span> 

<span data-ttu-id="58bc6-178">Další informace o funkci kopírování najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="58bc6-178">For more information on the copy function, see [Create multiple instances of resources in Azure Resource Manager](../../resource-group-create-multiple.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="58bc6-179">Další krok</span><span class="sxs-lookup"><span data-stu-id="58bc6-179">Next step</span></span>
<hr>

[<span data-ttu-id="58bc6-180">Krok 4 – nasazení aplikace pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="58bc6-180">Step 4 - Application Deployment with Azure Resource Manager Templates</span></span>](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

