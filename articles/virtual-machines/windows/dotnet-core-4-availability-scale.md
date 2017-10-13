---
title: "aaaAvailability a škálování v šablonách Azure Resource Manageru | Microsoft Docs"
description: "Virtuální počítač Azure DotNet základní kurz"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 494724b5-06af-4dea-a774-ba580cf27527
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a122d8e9536ea5fc2dc9c3f84042ed5c5179d783
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-scale-in-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="900cc-103">Dostupnost a dosah v šablonách Azure Resource Manageru pro virtuální počítače Windows</span><span class="sxs-lookup"><span data-stu-id="900cc-103">Availability and scale in Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="900cc-104">Dostupnost a dosah naleznete toouptime a hello možnost toomeet poptávky.</span><span class="sxs-lookup"><span data-stu-id="900cc-104">Availability and scale refer toouptime and hello ability toomeet demand.</span></span> <span data-ttu-id="900cc-105">Pokud aplikace musí být 99,9 % času hello, musí toohave architekturu, která umožňuje více souběžných výpočetní prostředky.</span><span class="sxs-lookup"><span data-stu-id="900cc-105">If an application must be up 99.9% of hello time, it needs toohave an architecture that allows for multiple concurrent compute resources.</span></span> <span data-ttu-id="900cc-106">Například místo jen jeden web, zahrnuje konfigurace s vyšší úrovní dostupnosti více instancí hello stejné vyrovnávání technologie před jejich lokality.</span><span class="sxs-lookup"><span data-stu-id="900cc-106">For instance, rather than having a single website, a configuration with a higher level of availability includes multiple instances of hello same site, with balancing technology in front of them.</span></span> <span data-ttu-id="900cc-107">V této konfiguraci jednu instanci aplikace hello může být kvůli údržbě vypnutá, zatímco hello zbývající dále toofunction.</span><span class="sxs-lookup"><span data-stu-id="900cc-107">In this configuration, one instance of hello application can be taken down for maintenance, while hello remaining continue toofunction.</span></span> <span data-ttu-id="900cc-108">Škálování na hello druhé straně odkazuje tooan aplikace možnost tooserve vyžádání.</span><span class="sxs-lookup"><span data-stu-id="900cc-108">Scale on hello other hand refers tooan applications ability tooserve demand.</span></span> <span data-ttu-id="900cc-109">S zatížení vyrovnáváním aplikace, přidání nebo odebrání instance z fondu hello umožňuje vyžádání toomeet tooscale aplikace.</span><span class="sxs-lookup"><span data-stu-id="900cc-109">With a load balanced application, adding or removing instances from hello pool allows an application tooscale toomeet demand.</span></span>

<span data-ttu-id="900cc-110">Tento dokument podrobně popisuje, jak je hello Hudba úložiště Ukázka nasazení nakonfigurované pro dostupnost a škálovatelnost.</span><span class="sxs-lookup"><span data-stu-id="900cc-110">This document details how hello Music Store sample deployment is configured for availability and scale.</span></span> <span data-ttu-id="900cc-111">Jsou vyznačené všechny závislosti a unikátní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="900cc-111">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="900cc-112">Nejvhodnější hello předem nasaďte instanci hello řešení tooyour předplatného Azure a pracovní společně s hello šablony Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="900cc-112">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="900cc-113">úplnou šablonu Hello naleznete zde – [Hudba nasazení úložiště v systému Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="900cc-113">hello complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="availability-set"></a><span data-ttu-id="900cc-114">Skupina dostupnosti</span><span class="sxs-lookup"><span data-stu-id="900cc-114">Availability Set</span></span>
<span data-ttu-id="900cc-115">Skupiny dostupnosti logicky platí virtuálních počítačů Azure pro fyzické hostitele a jiné infrastruktury komponenty, například napájení a fyzický síťový hardware.</span><span class="sxs-lookup"><span data-stu-id="900cc-115">An Availability Set logically spans Azure Virtual Machines across physical hosts and other infrastructural components such as power supplies and physical networking hardware.</span></span> <span data-ttu-id="900cc-116">Skupiny dostupnosti Ujistěte se, že během údržby, selhání zařízení nebo jiné výpadek, ne všechny virtuální počítače, budou provedeny.</span><span class="sxs-lookup"><span data-stu-id="900cc-116">Availability sets ensure that during maintenance, device failure, or other down time, not all virtual machines are effected.</span></span> <span data-ttu-id="900cc-117">Skupiny dostupnosti mohou být přidány tooan šablony Azure Resource Manageru pomocí hello Visual Studio nový Průvodce přidáním prostředku nebo vkládání platný kód JSON do šablony.</span><span class="sxs-lookup"><span data-stu-id="900cc-117">An Availability Set can be added tooan Azure Resource Manager template using hello Visual Studio Add New Resource Wizard, or inserting valid JSON into a template.</span></span>

<span data-ttu-id="900cc-118">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [sadu dostupnosti](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L368).</span><span class="sxs-lookup"><span data-stu-id="900cc-118">Follow this link toosee hello JSON sample within hello Resource Manager template – [Availability Set](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L368).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "availability-set"
  },
  "properties": {
  }
}
```

<span data-ttu-id="900cc-119">Skupiny dostupnosti je deklarován jako vlastnost prostředku virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="900cc-119">An Availability Set is declared as a property of a Virtual Machine resource.</span></span> 

<span data-ttu-id="900cc-120">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [dostupnost nastavená přidružení virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L302).</span><span class="sxs-lookup"><span data-stu-id="900cc-120">Follow this link toosee hello JSON sample within hello Resource Manager template – [Availability Set association with Virtual Machine](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L302).</span></span>

```json
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
<span data-ttu-id="900cc-121">dostupnost Hello nastavit, jak je vidět z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="900cc-121">hello availability set as seen from hello Azure portal.</span></span> <span data-ttu-id="900cc-122">Každý virtuální počítač a podrobnosti o konfiguraci hello jsou podrobně popsané v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="900cc-122">Each virtual machine and details about hello configuration are detailed here.</span></span>

![Skupina dostupnosti](./media/dotnet-core-4-availability-scale/ase-win.png)

<span data-ttu-id="900cc-124">Podrobné informace o dostupnosti sadách najdete v tématu [Správa dostupnosti virtuálních počítačů](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="900cc-124">For in-depth information on Availability Sets, see [Manage availability of virtual machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

## <a name="network-load-balancer"></a><span data-ttu-id="900cc-125">Nástroj pro vyrovnávání zatížení sítě</span><span class="sxs-lookup"><span data-stu-id="900cc-125">Network Load Balancer</span></span>
<span data-ttu-id="900cc-126">Vzhledem k tomu, skupinu dostupnosti aplikace odolnost proti chybám, Vyrovnávání zatížení zpřístupní velký počet instancí aplikace hello na jednu síťovou adresu.</span><span class="sxs-lookup"><span data-stu-id="900cc-126">Whereas an availability set provides application fault tolerance, a load balancer makes many instances of hello application available on a single network address.</span></span> <span data-ttu-id="900cc-127">Několik instancí aplikace může být hostovaný na velký počet virtuálních počítačů, každý z nich připojený tooa nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="900cc-127">Multiple instances of an application can be hosted on many virtual machines, each one connected tooa load balancer.</span></span> <span data-ttu-id="900cc-128">Jako aplikace hello přistupuje, směrování pro vyrovnávání zatížení hello hello příchozí žádost mezi členy hello připojen.</span><span class="sxs-lookup"><span data-stu-id="900cc-128">As hello application is accessed, hello load balancer routes hello incoming request across hello attached members.</span></span> <span data-ttu-id="900cc-129">Nástroj pro vyrovnávání zatížení lze přidat pomocí hello Visual Studio nový Průvodce přidáním prostředku nebo vložením správně naformátovaný JSON prostředků do šablony Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="900cc-129">A Load Balancer can be added using hello Visual Studio Add New Resource Wizard, or by inserting properly formatted JSON resource into hello Azure Resource Manager template.</span></span>

<span data-ttu-id="900cc-130">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [nástroj pro vyrovnávání zatížení sítě](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L198).</span><span class="sxs-lookup"><span data-stu-id="900cc-130">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L198).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer"
  },
  ........<truncated>
}
```

<span data-ttu-id="900cc-131">Protože hello vzorová aplikace je zveřejněné toohello internet s veřejnou IP adresu, tato adresa je přidružená Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="900cc-131">Because hello sample application is exposed toohello internet with a public IP address, this address is associated with hello load balancer.</span></span> 

<span data-ttu-id="900cc-132">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [přidružení nástroj pro vyrovnávání zatížení sítě s veřejnou IP adresou](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).</span><span class="sxs-lookup"><span data-stu-id="900cc-132">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Load Balancer association with Public IP Address](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).</span></span>

```json
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIpAddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

<span data-ttu-id="900cc-133">Z hello portálu Azure hello přehled nástroje pro vyrovnávání zatížení sítě zobrazuje hello přidružení hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="900cc-133">From hello Azure portal, hello network load balancer overview shows hello association with hello public IP address.</span></span>

![Nástroj pro vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/nlb-win.png)

## <a name="load-balancer-rule"></a><span data-ttu-id="900cc-135">Pravidlo Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="900cc-135">Load Balancer Rule</span></span>
<span data-ttu-id="900cc-136">Při použití služby Vyrovnávání zatížení, konfigurovaná pravidla, které řídí, jak se data jsou rovnoměrně mezi prostředky hello určený.</span><span class="sxs-lookup"><span data-stu-id="900cc-136">When using a load balancer, rules are configured that control how traffic is balanced across hello intended resources.</span></span> <span data-ttu-id="900cc-137">S hello ukázkovou aplikaci Store Hudba provoz dorazí na portu 80 hello veřejné IP adresy a je distribuován do portu 80 všech virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="900cc-137">With hello sample Music Store application, traffic arrives on port 80 of hello public IP address and is distributed across port 80 of all virtual machines.</span></span> 

<span data-ttu-id="900cc-138">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [pravidlo Vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L226).</span><span class="sxs-lookup"><span data-stu-id="900cc-138">Follow this link toosee hello JSON sample within hello Resource Manager template – [Load Balancer Rule](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L226).</span></span>

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

<span data-ttu-id="900cc-139">Zobrazení hello sítě pravidlo nástroje pro vyrovnávání zatížení z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="900cc-139">A view of hello network load balancer rule from hello portal.</span></span>

![Pravidlo Vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/lbrule-win.png)

## <a name="load-balancer-probe"></a><span data-ttu-id="900cc-141">Sondu nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="900cc-141">Load Balancer Probe</span></span>
<span data-ttu-id="900cc-142">Nástroj pro vyrovnávání zatížení Hello také musí toomonitor každý virtuální počítač tak, aby se zpracovat požadavky jenom toorunning systémy.</span><span class="sxs-lookup"><span data-stu-id="900cc-142">hello load balancer also needs toomonitor each virtual machine so that requests are served only toorunning systems.</span></span> <span data-ttu-id="900cc-143">Toto monitorování probíhá podle konstantní zjišťování předem definovaný port.</span><span class="sxs-lookup"><span data-stu-id="900cc-143">This monitoring takes place by constant probing of a pre-defined port.</span></span> <span data-ttu-id="900cc-144">nasazení Hudba úložiště Hello je nakonfigurované tooprobe portu 80 pro všechny zahrnuté virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="900cc-144">hello Music Store deployment is configured tooprobe port 80 on all included virtual machines.</span></span> 

<span data-ttu-id="900cc-145">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [sběru dat služby Vyrovnávání zatížení](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L247).</span><span class="sxs-lookup"><span data-stu-id="900cc-145">Follow this link toosee hello JSON sample within hello Resource Manager template – [Load Balancer Probe](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L247).</span></span>

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

<span data-ttu-id="900cc-146">sondu nástroje pro vyrovnávání zatížení Hello vidět z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="900cc-146">hello load balancer probe seen from hello Azure portal.</span></span>

![Sondu nástroje pro vyrovnávání zatížení sítě](./media/dotnet-core-4-availability-scale/lbprobe-win.png)

## <a name="inbound-nat-rules"></a><span data-ttu-id="900cc-148">Příchozí pravidla NAT</span><span class="sxs-lookup"><span data-stu-id="900cc-148">Inbound NAT Rules</span></span>
<span data-ttu-id="900cc-149">Pokud používáte nástroj pro vyrovnávání zatížení, třeba pravidla toobe zavést, které poskytují tooeach vyrovnáváním přístup bez zatížení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="900cc-149">When using a Load Balancer, rules need toobe put into place that provide non-load balanced access tooeach Virtual Machine.</span></span> <span data-ttu-id="900cc-150">Například při vytváření připojení ke vzdálené ploše pomocí každý virtuální počítač, tato komunikace by neměl být skupinu s vyrovnáváním zatížení, místo by měl být nakonfigurovaný předem určené cesty.</span><span class="sxs-lookup"><span data-stu-id="900cc-150">For instance, when creating an RDP connection with each virtual machine, this traffic should not be load balanced, rather a pre-determined path should be configured.</span></span> <span data-ttu-id="900cc-151">Předem určené cesty se konfiguruje pomocí prostředek příchozí pravidlo NAT.</span><span class="sxs-lookup"><span data-stu-id="900cc-151">Pre-determined paths are configured using an Inbound NAT Rule resource.</span></span> <span data-ttu-id="900cc-152">Pomocí tohoto prostředku, příchozí komunikace může být namapované tooindividual virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="900cc-152">Using this resource, inbound communication can be mapped tooindividual Virtual Machines.</span></span> 

<span data-ttu-id="900cc-153">S hello aplikaci Store Hudba je port od 5000 namapované tooport 3389 na každém virtuálním počítači pro přístup k protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="900cc-153">With hello Music Store application, a port starting at 5000 is mapped tooport 3389 on each Virtual Machine for RDP access.</span></span> <span data-ttu-id="900cc-154">Hello `copyindex()` funkce je použité tooincrement hello příchozí port, tak, aby se hello druhý virtuální počítač přijímá port pro příchozí 5001, hello třetí 5002 a tak dále.</span><span class="sxs-lookup"><span data-stu-id="900cc-154">hello `copyindex()` function is used tooincrement hello incoming port, such that hello second Virtual Machine receives an incoming port of 5001, hello third 5002, and so on.</span></span>

<span data-ttu-id="900cc-155">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [příchozí pravidla NAT](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L260).</span><span class="sxs-lookup"><span data-stu-id="900cc-155">Follow this link toosee hello JSON sample within hello Resource Manager template – [Inbound NAT Rules](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L260).</span></span> 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'RDP-VM', copyIndex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
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
    "backendPort": 3389,
    "enableFloatingIP": false
  }
}
```

<span data-ttu-id="900cc-156">Jedním z příkladů příchozí pravidlo NAT jako hello zaznamenané v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="900cc-156">One example inbound NAT rule as seen in hello Azure portal.</span></span> <span data-ttu-id="900cc-157">Pro každý virtuální počítač v hello nasazení se vytvoří pravidlo RDP NAT.</span><span class="sxs-lookup"><span data-stu-id="900cc-157">An RDP NAT rule is created for each virtual machine in hello deployment.</span></span>

![Příchozí pravidlo NAT](./media/dotnet-core-4-availability-scale/natrule-win.png)

<span data-ttu-id="900cc-159">Podrobnější informace o hello Vyrovnávání zatížení sítě Azure, najdete v části [pro infrastrukturu Azure služby Vyrovnávání zatížení](load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="900cc-159">For in-depth information on hello Azure Network Load Balancer, see [Load balancing for Azure infrastructure services](load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="deploy-multiple-vms"></a><span data-ttu-id="900cc-160">Nasazení více virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="900cc-160">Deploy multiple VMs</span></span>
<span data-ttu-id="900cc-161">Nakonec pro funkci tooeffectively sady dostupnosti nebo služby Vyrovnávání zatížení, jsou požadovány více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="900cc-161">Finally, for an Availability Set or Load Balancer tooeffectively function, multiple virtual machines are required.</span></span> <span data-ttu-id="900cc-162">Víc virtuálních počítačů můžete nasadit pomocí funkce kopírování šablony Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="900cc-162">Multiple VMs can be deployed using hello Azure Resource Manager template copy function.</span></span> <span data-ttu-id="900cc-163">Pomocí funkce kopírování hello, není nutné toodefine omezený počet virtuálních počítačů, místo tuto hodnotu můžete dynamicky poskytovat hello během nasazení.</span><span class="sxs-lookup"><span data-stu-id="900cc-163">Using hello copy function, it is not necessary toodefine a finite number of Virtual Machines, rather this value can be dynamically provided at hello time of deployment.</span></span> <span data-ttu-id="900cc-164">funkci kopírování do Hello spotřebuje hello počet instancí toobe vytvoření a nasazení hello správný počet virtuálních počítačů a přidružených prostředků obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="900cc-164">hello copy function consumes hello number of instances toobe created and handles deploying hello proper number of virtual machines and associated resources.</span></span>

<span data-ttu-id="900cc-165">V šabloně Hudba úložiště ukázkový text hello je definován parametr, který přebírá počet instancí.</span><span class="sxs-lookup"><span data-stu-id="900cc-165">In hello Music Store Sample template, a parameter is defined that takes in an instance count.</span></span> <span data-ttu-id="900cc-166">Toto číslo se používá v rámci hello šablonu při vytváření virtuálních počítačů a související prostředky.</span><span class="sxs-lookup"><span data-stu-id="900cc-166">This number is used throughout hello template when creating virtual machines and related resources.</span></span>

```json
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 2,
  "metadata": {
    "description": "Number of VM instances toobe created behind load balancer."
  }
},
```

<span data-ttu-id="900cc-167">Na hello prostředek virtuálního počítače hello kopírovací smyčkou je zadaný název a používá toocontrol hello počet kopií výsledné hello počet instancí parametru.</span><span class="sxs-lookup"><span data-stu-id="900cc-167">On hello Virtual Machine resource, hello copy loop is given a name and hello number of instances parameter used toocontrol hello number of resulting copies.</span></span>

<span data-ttu-id="900cc-168">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [funkci kopírování do virtuálního počítače](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L290).</span><span class="sxs-lookup"><span data-stu-id="900cc-168">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine Copy Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L290).</span></span> 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  }
```

<span data-ttu-id="900cc-169">aktuální iteraci Hello funkci kopírování do hello lze přistupovat pomocí hello `copyIndex()` funkce.</span><span class="sxs-lookup"><span data-stu-id="900cc-169">hello current iteration of hello copy function can be accessed with hello `copyIndex()` function.</span></span> <span data-ttu-id="900cc-170">Hodnota Hello hello kopie index funkce může být použité tooname virtuální počítače a další prostředky.</span><span class="sxs-lookup"><span data-stu-id="900cc-170">hello value of hello copy index function can be used tooname virtual machines and other resources.</span></span> <span data-ttu-id="900cc-171">Například pokud se nasazuje dvě instance virtuálního počítače, potřebují mít odlišné názvy.</span><span class="sxs-lookup"><span data-stu-id="900cc-171">For instance, if two instances of a virtual machine are deployed, they need different names.</span></span> <span data-ttu-id="900cc-172">Hello `copyIndex()` funkci lze použít jako součást hello virtuálního počítače názvu toocreate jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="900cc-172">hello `copyIndex()` function can be used as part of hello virtual machine name toocreate a unique name.</span></span> <span data-ttu-id="900cc-173">Příkladem hello `copyindex()` funkce použité pro účely pojmenování si můžete prohlédnout ve hello prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="900cc-173">An example of hello `copyindex()` function used for naming purposes can be seen in hello Virtual Machine resource.</span></span> <span data-ttu-id="900cc-174">Zde je název počítače hello zřetězení hello `vmName` parametr a hello `copyIndex()` funkce.</span><span class="sxs-lookup"><span data-stu-id="900cc-174">Here, hello computer name is a concatenation of hello `vmName` parameter, and hello `copyIndex()` function.</span></span> 

<span data-ttu-id="900cc-175">Použijte tento odkaz toosee hello JSON ukázka v rámci šablony Resource Manageru hello – [funkci kopírování do indexu](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L309).</span><span class="sxs-lookup"><span data-stu-id="900cc-175">Follow this link toosee hello JSON sample within hello Resource Manager template – [Copy Index Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L309).</span></span> 

```json
"osProfile": {
  "computerName": "[concat(variables('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "adminPassword": "[parameters('adminPassword')]"
}
```

<span data-ttu-id="900cc-176">Hello `copyIndex` funkce je používán několikrát hello Hudba úložiště vzorové šablony.</span><span class="sxs-lookup"><span data-stu-id="900cc-176">hello `copyIndex` function is used several times in hello Music Store sample template.</span></span> <span data-ttu-id="900cc-177">Prostředky a funkce využívá `copyIndex` obsahovat cokoli konkrétní tooa jednu instanci virtuálního počítače hello takové a síťového rozhraní, pravidla nástroje pro vyrovnávání zatížení, a všechny závisí na funkce.</span><span class="sxs-lookup"><span data-stu-id="900cc-177">Resources and functions utilizing `copyIndex` include anything specific tooa single instance of hello virtual machine such and network interface, load balancer rules, and any depends on functions.</span></span> 

<span data-ttu-id="900cc-178">Další informace o funkci kopírování do hello najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="900cc-178">For more information on hello copy function, see [Create multiple instances of resources in Azure Resource Manager](../../resource-group-create-multiple.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="900cc-179">Další krok</span><span class="sxs-lookup"><span data-stu-id="900cc-179">Next step</span></span>
<hr>

[<span data-ttu-id="900cc-180">Krok 4 – nasazení aplikace pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="900cc-180">Step 4 - Application Deployment with Azure Resource Manager Templates</span></span>](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
