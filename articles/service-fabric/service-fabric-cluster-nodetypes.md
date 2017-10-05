---
title: "Typy uzlů Service Fabric a škálovatelné sady virtuálních počítačů | Microsoft Docs"
description: "Popisuje, jak Service Fabric typy uzlů se týkají škálovatelné sady virtuálních počítačů a jak pro vzdálené připojení k do instance Škálovací sady virtuálního počítače nebo uzel clusteru."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: chackdan
ms.openlocfilehash: 3b1a22bb3653abb68fc73645ad2cb623fabc7736
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a><span data-ttu-id="e2606-103">Vztah mezi typy uzlů Service Fabric a sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e2606-103">The relationship between Service Fabric node types and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="e2606-104">Sady škálování virtuálního počítače jsou prostředek Azure Compute, které můžete použít k nasazení a správě kolekci jako sada virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e2606-104">Virtual Machine Scale Sets are an Azure Compute resource you can use to deploy and manage a collection of virtual machines as a set.</span></span> <span data-ttu-id="e2606-105">Každý typ uzlu, který je definován v clusteru Service Fabric je nastavený jako samostatné sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2606-105">Every node type that is defined in a Service Fabric cluster is set up as a separate VM Scale Set.</span></span> <span data-ttu-id="e2606-106">Každý typ uzlu je možné škálovat pak nebo dolů nezávisle, mají různé sady otevřené porty a může mít různé kapacity metriky.</span><span class="sxs-lookup"><span data-stu-id="e2606-106">Each node type can then be scaled up or down independently, have different sets of ports open, and can have different capacity metrics.</span></span>

<span data-ttu-id="e2606-107">Následující snímek obrazovky ukazuje cluster, který má dva typy uzlů: front-endové a back-end.</span><span class="sxs-lookup"><span data-stu-id="e2606-107">The following screen shot shows a cluster that has two node types: FrontEnd and BackEnd.</span></span>  <span data-ttu-id="e2606-108">Každý typ uzlu má pět uzlů.</span><span class="sxs-lookup"><span data-stu-id="e2606-108">Each node type has five nodes each.</span></span>

![Snímek obrazovky, který má dva typy uzlů clusteru][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a><span data-ttu-id="e2606-110">Mapování instance Škálovací sady virtuálního počítače na uzly</span><span class="sxs-lookup"><span data-stu-id="e2606-110">Mapping VM Scale Set instances to nodes</span></span>
<span data-ttu-id="e2606-111">Jak je uvedeno výše, spustit instance Škálovací sady virtuálního počítače z instance, 0 a poté přejde nahoru.</span><span class="sxs-lookup"><span data-stu-id="e2606-111">As you can see above, the VM Scale Set instances start from instance 0 and then goes up.</span></span> <span data-ttu-id="e2606-112">Číslování se projeví v názvech.</span><span class="sxs-lookup"><span data-stu-id="e2606-112">The numbering is reflected in the names.</span></span> <span data-ttu-id="e2606-113">Například BackEnd_0 je instance 0 sady škálování virtuálního počítače back-end.</span><span class="sxs-lookup"><span data-stu-id="e2606-113">For example, BackEnd_0 is instance 0 of the BackEnd VM Scale Set.</span></span> <span data-ttu-id="e2606-114">Tato konkrétní Škálovací sadě virtuálních počítačů má pět instancí s názvem BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 a BackEnd_4.</span><span class="sxs-lookup"><span data-stu-id="e2606-114">This particular VM Scale Set has five instances, named BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 and BackEnd_4.</span></span>

<span data-ttu-id="e2606-115">Při škálování Škálovací sadě virtuálních počítačů je vytvořena nová instance.</span><span class="sxs-lookup"><span data-stu-id="e2606-115">When you scale up a VM Scale Set a new instance is created.</span></span> <span data-ttu-id="e2606-116">Nový název instance Škálovací sady virtuálního počítače bude obvykle název sady škálování virtuálního počítače + číslo další instance.</span><span class="sxs-lookup"><span data-stu-id="e2606-116">The new VM Scale Set instance name will typically be the VM Scale Set name + the next instance number.</span></span> <span data-ttu-id="e2606-117">V našem příkladu je BackEnd_5.</span><span class="sxs-lookup"><span data-stu-id="e2606-117">In our example, it is BackEnd_5.</span></span>

## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a><span data-ttu-id="e2606-118">Mapování škálování virtuálních počítačů pro každý uzel typu nebo virtuální počítač Škálovací sadu nastavení nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="e2606-118">Mapping VM scale set load balancers to each node type/VM Scale Set</span></span>
<span data-ttu-id="e2606-119">Pokud jste nasadili cluster z portálu nebo jste použili šablony Resource Manageru ukázkový, který jsme zadali, pak když získáte seznam všech prostředků ve skupině prostředků, se zobrazí u každé sady škálování virtuálního počítače nebo uzel typu služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="e2606-119">If you have deployed your cluster from the portal or have used the sample Resource Manager template that we provided, then when you get a list of all resources under a Resource Group then you will see the load balancers for each VM Scale Set or node type.</span></span>

<span data-ttu-id="e2606-120">Název by podobný: **LB -&lt;NodeType název&gt;**.</span><span class="sxs-lookup"><span data-stu-id="e2606-120">The name would something like: **LB-&lt;NodeType name&gt;**.</span></span> <span data-ttu-id="e2606-121">Například LB-sfcluster4doc-0, jak je vidět na tomto snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="e2606-121">For example, LB-sfcluster4doc-0, as shown in this screenshot:</span></span>

![Zdroje][Resources]

## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a><span data-ttu-id="e2606-123">Vzdálené připojení k do instance Škálovací sady virtuálního počítače nebo uzlu clusteru</span><span class="sxs-lookup"><span data-stu-id="e2606-123">Remote connect to a VM Scale Set instance or a cluster node</span></span>
<span data-ttu-id="e2606-124">Každý typ uzlu, který je definován v clusteru s podporou je nastavený jako samostatné sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2606-124">Every Node type that is defined in a cluster is set up as a separate VM Scale Set.</span></span>  <span data-ttu-id="e2606-125">To znamená, že je možné rozšířit typy uzlů až nebo nezávisle a může být z jiné SKU virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e2606-125">That means the node types can be scaled up or down independently and can be made of different VM SKUs.</span></span> <span data-ttu-id="e2606-126">Na rozdíl od jedné instance virtuálních počítačů Nezískávat instance Škálovací sady virtuálního počítače virtuální IP adresy s vlastními.</span><span class="sxs-lookup"><span data-stu-id="e2606-126">Unlike single instance VMs, the VM Scale Set instances do not get a virtual IP address of their own.</span></span> <span data-ttu-id="e2606-127">Proto může být trochu náročné když hledáte IP adresu a port, který můžete použít pro vzdálené připojení ke konkrétní instanci.</span><span class="sxs-lookup"><span data-stu-id="e2606-127">So it can be a bit challenging when you are looking for an IP address and port that you can use to remote connect to a specific instance.</span></span>

<span data-ttu-id="e2606-128">Tady jsou kroky, pomocí kterých je zjišťovat.</span><span class="sxs-lookup"><span data-stu-id="e2606-128">Here are the steps you can follow to discover them.</span></span>

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a><span data-ttu-id="e2606-129">Krok 1: Zjistěte virtuální IP adresu pro typ uzlu a potom pravidla příchozí NAT pro protokol RDP</span><span class="sxs-lookup"><span data-stu-id="e2606-129">Step 1: Find out the virtual IP address for the node type and then Inbound NAT rules for RDP</span></span>
<span data-ttu-id="e2606-130">Chcete-li získat, který, potřebujete získat hodnoty příchozích pravidel NAT, které byly definované jako součást definice prostředků pro **Microsoft.Network/loadBalancers**.</span><span class="sxs-lookup"><span data-stu-id="e2606-130">In order to get that, you need to get the inbound NAT rules values that were defined as a part of the resource definition for **Microsoft.Network/loadBalancers**.</span></span>

<span data-ttu-id="e2606-131">Na portálu, přejděte do okna nástroje pro vyrovnávání zatížení a potom **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="e2606-131">In the portal, navigate to the Load balancer blade and then **Settings**.</span></span>

![LBBlade][LBBlade]

<span data-ttu-id="e2606-133">V **nastavení**, klikněte na **pravidla příchozí NAT**.</span><span class="sxs-lookup"><span data-stu-id="e2606-133">In **Settings**, click on **Inbound NAT rules**.</span></span> <span data-ttu-id="e2606-134">Tuto chybu získáte IP adresu a port, který můžete použít pro vzdálené připojení k první instance Škálovací sady virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2606-134">This now gives you the IP address and port that you can use to remote connect to the first VM Scale Set instance.</span></span> <span data-ttu-id="e2606-135">Na tomto snímku obrazovky je **104.42.106.156** a **3389**</span><span class="sxs-lookup"><span data-stu-id="e2606-135">In the screenshot below, it is **104.42.106.156** and **3389**</span></span>

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a><span data-ttu-id="e2606-137">Krok 2: Najít se port, který můžete použít vzdálené připojení k instanci/uzlu konkrétní Škálovací sady virtuálního počítače na více systémů</span><span class="sxs-lookup"><span data-stu-id="e2606-137">Step 2: Find out the port that you can use to remote connect to the specific VM Scale Set instance/node</span></span>
<span data-ttu-id="e2606-138">Dříve v tomto dokumentu I věnovala mapování instance Škálovací sady virtuálního počítače do uzlu.</span><span class="sxs-lookup"><span data-stu-id="e2606-138">Earlier in this document, I talked about how the VM Scale Set instances map to the nodes.</span></span> <span data-ttu-id="e2606-139">Použijeme, a pokuste se zjistit přesnou port.</span><span class="sxs-lookup"><span data-stu-id="e2606-139">We will use that to figure out the exact port.</span></span>

<span data-ttu-id="e2606-140">Porty jsou přiděleny ve vzestupném pořadí instance Škálovací sady virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2606-140">The ports are allocated in ascending order of the VM Scale Set instance.</span></span> <span data-ttu-id="e2606-141">proto v tomto příkladu pro typ uzlu front-endu jsou tyto porty pro jednotlivé pět instancí.</span><span class="sxs-lookup"><span data-stu-id="e2606-141">so in my example for the FrontEnd node type, the ports for each of the five instances are the following.</span></span> <span data-ttu-id="e2606-142">Teď je potřeba provést stejné mapování pro instance Škálovací sady virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e2606-142">you now need to do the same mapping for your VM Scale Set instance.</span></span>

| <span data-ttu-id="e2606-143">**Instance sady škálování virtuálního počítače**</span><span class="sxs-lookup"><span data-stu-id="e2606-143">**VM Scale Set Instance**</span></span> | <span data-ttu-id="e2606-144">**Port**</span><span class="sxs-lookup"><span data-stu-id="e2606-144">**Port**</span></span> |
| --- | --- |
| <span data-ttu-id="e2606-145">FrontEnd_0</span><span class="sxs-lookup"><span data-stu-id="e2606-145">FrontEnd_0</span></span> |<span data-ttu-id="e2606-146">3389</span><span class="sxs-lookup"><span data-stu-id="e2606-146">3389</span></span> |
| <span data-ttu-id="e2606-147">FrontEnd_1</span><span class="sxs-lookup"><span data-stu-id="e2606-147">FrontEnd_1</span></span> |<span data-ttu-id="e2606-148">3390</span><span class="sxs-lookup"><span data-stu-id="e2606-148">3390</span></span> |
| <span data-ttu-id="e2606-149">FrontEnd_2</span><span class="sxs-lookup"><span data-stu-id="e2606-149">FrontEnd_2</span></span> |<span data-ttu-id="e2606-150">3391</span><span class="sxs-lookup"><span data-stu-id="e2606-150">3391</span></span> |
| <span data-ttu-id="e2606-151">FrontEnd_3</span><span class="sxs-lookup"><span data-stu-id="e2606-151">FrontEnd_3</span></span> |<span data-ttu-id="e2606-152">3392</span><span class="sxs-lookup"><span data-stu-id="e2606-152">3392</span></span> |
| <span data-ttu-id="e2606-153">FrontEnd_4</span><span class="sxs-lookup"><span data-stu-id="e2606-153">FrontEnd_4</span></span> |<span data-ttu-id="e2606-154">3393</span><span class="sxs-lookup"><span data-stu-id="e2606-154">3393</span></span> |
| <span data-ttu-id="e2606-155">FrontEnd_5</span><span class="sxs-lookup"><span data-stu-id="e2606-155">FrontEnd_5</span></span> |<span data-ttu-id="e2606-156">3394</span><span class="sxs-lookup"><span data-stu-id="e2606-156">3394</span></span> |

### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a><span data-ttu-id="e2606-157">Krok 3: Vzdálené připojení k konkrétní instance Škálovací sady virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e2606-157">Step 3: Remote connect to the specific VM Scale Set instance</span></span>
<span data-ttu-id="e2606-158">Na tomto snímku obrazovky I pro připojení k FrontEnd_1 použijte připojení ke vzdálené ploše:</span><span class="sxs-lookup"><span data-stu-id="e2606-158">In the screenshot below I use Remote Desktop Connection to connect to the FrontEnd_1:</span></span>

![PROTOKOLU RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a><span data-ttu-id="e2606-160">Jak změnit hodnoty rozsahu portu protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="e2606-160">How to change the RDP port range values</span></span>
### <a name="before-cluster-deployment"></a><span data-ttu-id="e2606-161">Před nasazením clusteru</span><span class="sxs-lookup"><span data-stu-id="e2606-161">Before cluster deployment</span></span>
<span data-ttu-id="e2606-162">Když nastavujete clusteru pomocí šablony Resource Manager, můžete zadat rozsah v **inboundNatPools**.</span><span class="sxs-lookup"><span data-stu-id="e2606-162">When you are setting up the cluster using an Resource Manager template, you can specify the range in the **inboundNatPools**.</span></span>

<span data-ttu-id="e2606-163">Přejděte do definice prostředků pro **Microsoft.Network/loadBalancers**.</span><span class="sxs-lookup"><span data-stu-id="e2606-163">Go to the resource definition for **Microsoft.Network/loadBalancers**.</span></span> <span data-ttu-id="e2606-164">V části, který najdete popis **inboundNatPools**.</span><span class="sxs-lookup"><span data-stu-id="e2606-164">Under that you find the description for **inboundNatPools**.</span></span>  <span data-ttu-id="e2606-165">Nahraďte *frontendPortRangeStart* a *frontendPortRangeEnd* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e2606-165">Replace the *frontendPortRangeStart* and *frontendPortRangeEnd* values.</span></span>

![InboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a><span data-ttu-id="e2606-167">Po nasazení clusteru</span><span class="sxs-lookup"><span data-stu-id="e2606-167">After cluster deployment</span></span>
<span data-ttu-id="e2606-168">Toto je složitější a může vést k recyklaci virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e2606-168">This is a bit more involved and may result in the VMs getting recycled.</span></span> <span data-ttu-id="e2606-169">Nyní máte nastavit nové hodnoty pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e2606-169">You will now have to set new values using Azure PowerShell.</span></span> <span data-ttu-id="e2606-170">Ujistěte se, že je v počítači nainstalován prostředí Azure PowerShell 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e2606-170">Make sure that Azure PowerShell 1.0 or later is installed on your machine.</span></span> <span data-ttu-id="e2606-171">Pokud jste to před neučinili, I důrazně doporučujeme postupujte podle kroků uvedených v [postup instalace a konfigurace prostředí Azure PowerShell.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="e2606-171">If you have not done this before, I strongly suggest that you follow the steps outlined in [How to install and configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="e2606-172">Přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="e2606-172">Sign in to your Azure account.</span></span> <span data-ttu-id="e2606-173">Pokud z nějakého důvodu selže tento příkaz prostředí PowerShell, byste měli zkontrolovat, jestli máte Azure PowerShell správně nainstalován.</span><span class="sxs-lookup"><span data-stu-id="e2606-173">If this PowerShell command fails for some reason, you should check whether you have Azure PowerShell installed correctly.</span></span>

```
Login-AzureRmAccount
```

<span data-ttu-id="e2606-174">Spusťte následující příkaz a zobrazí podrobnosti o nástroj pro vyrovnávání zatížení a zobrazí hodnoty pro popis **inboundNatPools**:</span><span class="sxs-lookup"><span data-stu-id="e2606-174">Run the following to get details on your load balancer and you see the values for the description for **inboundNatPools**:</span></span>

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

<span data-ttu-id="e2606-175">Nastaví *frontendPortRangeEnd* a *frontendPortRangeStart* na požadované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e2606-175">Now set *frontendPortRangeEnd* and *frontendPortRangeStart* to the values you want.</span></span>

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a><span data-ttu-id="e2606-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e2606-176">Next steps</span></span>
* [<span data-ttu-id="e2606-177">Přehled funkce "Nasazení odkudkoli" a porovnání s Azure spravované clustery</span><span class="sxs-lookup"><span data-stu-id="e2606-177">Overview of the "Deploy anywhere" feature and a comparison with Azure-managed clusters</span></span>](service-fabric-deploy-anywhere.md)
* [<span data-ttu-id="e2606-178">Zabezpečení clusteru</span><span class="sxs-lookup"><span data-stu-id="e2606-178">Cluster security</span></span>](service-fabric-cluster-security.md)
* [<span data-ttu-id="e2606-179">Service Fabric SDK a Začínáme</span><span class="sxs-lookup"><span data-stu-id="e2606-179"> Service Fabric SDK and getting started</span></span>](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
