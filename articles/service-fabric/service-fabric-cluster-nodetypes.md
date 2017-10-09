---
title: "aaaService Fabric typů uzlů a škálovatelné sady virtuálních počítačů | Microsoft Docs"
description: "Popisuje, jak Service Fabric typy uzlů vztahují tooVM sady škálování a připojení tooremote tooa instance Škálovací sady virtuálního počítače nebo uzel clusteru."
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
ms.openlocfilehash: 830ea2816f5864de146a77483c85de26f91c2425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a><span data-ttu-id="abcca-103">Hello vztah mezi typy uzlů Service Fabric a sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="abcca-103">hello relationship between Service Fabric node types and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="abcca-104">Sady škálování virtuálního počítače jsou prostředek Azure Compute můžete použít toodeploy a spravovat kolekci virtuálních počítačů jako sada.</span><span class="sxs-lookup"><span data-stu-id="abcca-104">Virtual Machine Scale Sets are an Azure Compute resource you can use toodeploy and manage a collection of virtual machines as a set.</span></span> <span data-ttu-id="abcca-105">Každý typ uzlu, který je definován v clusteru Service Fabric je nastavený jako samostatné sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="abcca-105">Every node type that is defined in a Service Fabric cluster is set up as a separate VM Scale Set.</span></span> <span data-ttu-id="abcca-106">Každý typ uzlu je možné škálovat pak nebo dolů nezávisle, mají různé sady otevřené porty a může mít různé kapacity metriky.</span><span class="sxs-lookup"><span data-stu-id="abcca-106">Each node type can then be scaled up or down independently, have different sets of ports open, and can have different capacity metrics.</span></span>

<span data-ttu-id="abcca-107">Hello následující snímek obrazovky ukazuje cluster, který má dva typy uzlů: front-endové a back-end.</span><span class="sxs-lookup"><span data-stu-id="abcca-107">hello following screen shot shows a cluster that has two node types: FrontEnd and BackEnd.</span></span>  <span data-ttu-id="abcca-108">Každý typ uzlu má pět uzlů.</span><span class="sxs-lookup"><span data-stu-id="abcca-108">Each node type has five nodes each.</span></span>

![Snímek obrazovky, který má dva typy uzlů clusteru][NodeTypes]

## <a name="mapping-vm-scale-set-instances-toonodes"></a><span data-ttu-id="abcca-110">Mapování toonodes instance Škálovací sady virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="abcca-110">Mapping VM Scale Set instances toonodes</span></span>
<span data-ttu-id="abcca-111">Jak je uvedeno výše, spusťte z instance 0 hello instance Škálovací sady virtuálního počítače a pak se zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="abcca-111">As you can see above, hello VM Scale Set instances start from instance 0 and then goes up.</span></span> <span data-ttu-id="abcca-112">číslování Hello se projeví v názvech hello.</span><span class="sxs-lookup"><span data-stu-id="abcca-112">hello numbering is reflected in hello names.</span></span> <span data-ttu-id="abcca-113">Například BackEnd_0 je instance 0 hello sady škálování virtuálních počítačů v back-end.</span><span class="sxs-lookup"><span data-stu-id="abcca-113">For example, BackEnd_0 is instance 0 of hello BackEnd VM Scale Set.</span></span> <span data-ttu-id="abcca-114">Tato konkrétní Škálovací sadě virtuálních počítačů má pět instancí s názvem BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 a BackEnd_4.</span><span class="sxs-lookup"><span data-stu-id="abcca-114">This particular VM Scale Set has five instances, named BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 and BackEnd_4.</span></span>

<span data-ttu-id="abcca-115">Při škálování Škálovací sadě virtuálních počítačů je vytvořena nová instance.</span><span class="sxs-lookup"><span data-stu-id="abcca-115">When you scale up a VM Scale Set a new instance is created.</span></span> <span data-ttu-id="abcca-116">Hello nové sady škálování virtuálního počítače název instance bude obvykle název sady škálování virtuálního počítače hello + hello další instance číslo.</span><span class="sxs-lookup"><span data-stu-id="abcca-116">hello new VM Scale Set instance name will typically be hello VM Scale Set name + hello next instance number.</span></span> <span data-ttu-id="abcca-117">V našem příkladu je BackEnd_5.</span><span class="sxs-lookup"><span data-stu-id="abcca-117">In our example, it is BackEnd_5.</span></span>

## <a name="mapping-vm-scale-set-load-balancers-tooeach-node-typevm-scale-set"></a><span data-ttu-id="abcca-118">Mapování virtuálních počítačů sady škálování zatížení nástroje pro vyrovnávání tooeach uzel typu nebo virtuálních počítačů sady škálování</span><span class="sxs-lookup"><span data-stu-id="abcca-118">Mapping VM scale set load balancers tooeach node type/VM Scale Set</span></span>
<span data-ttu-id="abcca-119">Pokud jste nasadili cluster z portálu hello nebo použili šablony Resource Manageru ukázka hello, který jsme zadali, pak když získáte seznam všech prostředků ve skupině prostředků, uvidíte hello služby Vyrovnávání zatížení pro každý typ Škálovací sady virtuálního počítače nebo uzel.</span><span class="sxs-lookup"><span data-stu-id="abcca-119">If you have deployed your cluster from hello portal or have used hello sample Resource Manager template that we provided, then when you get a list of all resources under a Resource Group then you will see hello load balancers for each VM Scale Set or node type.</span></span>

<span data-ttu-id="abcca-120">Hello název by podobný: **LB -&lt;NodeType název&gt;**.</span><span class="sxs-lookup"><span data-stu-id="abcca-120">hello name would something like: **LB-&lt;NodeType name&gt;**.</span></span> <span data-ttu-id="abcca-121">Například LB-sfcluster4doc-0, jak je vidět na tomto snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="abcca-121">For example, LB-sfcluster4doc-0, as shown in this screenshot:</span></span>

![Zdroje][Resources]

## <a name="remote-connect-tooa-vm-scale-set-instance-or-a-cluster-node"></a><span data-ttu-id="abcca-123">Vzdálené připojení tooa instance Škálovací sady virtuálního počítače nebo uzlu clusteru</span><span class="sxs-lookup"><span data-stu-id="abcca-123">Remote connect tooa VM Scale Set instance or a cluster node</span></span>
<span data-ttu-id="abcca-124">Každý typ uzlu, který je definován v clusteru s podporou je nastavený jako samostatné sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="abcca-124">Every Node type that is defined in a cluster is set up as a separate VM Scale Set.</span></span>  <span data-ttu-id="abcca-125">Aby znamená hello typy uzlů je možné rozšířit až nebo nezávisle a může být z jiné SKU virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="abcca-125">That means hello node types can be scaled up or down independently and can be made of different VM SKUs.</span></span> <span data-ttu-id="abcca-126">Na rozdíl od jedné instance virtuálních počítačů instance Škálovací sady virtuálního počítače hello Nezískávat virtuální IP adresy s vlastními.</span><span class="sxs-lookup"><span data-stu-id="abcca-126">Unlike single instance VMs, hello VM Scale Set instances do not get a virtual IP address of their own.</span></span> <span data-ttu-id="abcca-127">Aby mohli být chvíli náročné, když hledáte IP adresu a port, který můžete použít tooremote připojit tooa konkrétní instanci.</span><span class="sxs-lookup"><span data-stu-id="abcca-127">So it can be a bit challenging when you are looking for an IP address and port that you can use tooremote connect tooa specific instance.</span></span>

<span data-ttu-id="abcca-128">Tady jsou kroky hello toodiscover můžete postupovat podle nich.</span><span class="sxs-lookup"><span data-stu-id="abcca-128">Here are hello steps you can follow toodiscover them.</span></span>

### <a name="step-1-find-out-hello-virtual-ip-address-for-hello-node-type-and-then-inbound-nat-rules-for-rdp"></a><span data-ttu-id="abcca-129">Krok 1: Zjistěte hello virtuální IP adresy pro typ uzlu hello a potom pravidla příchozí NAT pro protokol RDP</span><span class="sxs-lookup"><span data-stu-id="abcca-129">Step 1: Find out hello virtual IP address for hello node type and then Inbound NAT rules for RDP</span></span>
<span data-ttu-id="abcca-130">V pořadí tooget, je nutné, aby tooget hello příchozích pravidel NAT pravidla hodnoty, které byly definované jako součást hello definice prostředků pro **Microsoft.Network/loadBalancers**.</span><span class="sxs-lookup"><span data-stu-id="abcca-130">In order tooget that, you need tooget hello inbound NAT rules values that were defined as a part of hello resource definition for **Microsoft.Network/loadBalancers**.</span></span>

<span data-ttu-id="abcca-131">Hello portálu, přejděte okno nástroje pro vyrovnávání zatížení toohello a potom **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="abcca-131">In hello portal, navigate toohello Load balancer blade and then **Settings**.</span></span>

![LBBlade][LBBlade]

<span data-ttu-id="abcca-133">V **nastavení**, klikněte na **pravidla příchozí NAT**.</span><span class="sxs-lookup"><span data-stu-id="abcca-133">In **Settings**, click on **Inbound NAT rules**.</span></span> <span data-ttu-id="abcca-134">Tato teď poskytuje hello IP adresu a port, které můžete použít tooremote připojit toohello první instance Škálovací sady virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="abcca-134">This now gives you hello IP address and port that you can use tooremote connect toohello first VM Scale Set instance.</span></span> <span data-ttu-id="abcca-135">Na snímku obrazovky hello níže, je **104.42.106.156** a **3389**</span><span class="sxs-lookup"><span data-stu-id="abcca-135">In hello screenshot below, it is **104.42.106.156** and **3389**</span></span>

![NATRules][NATRules]

### <a name="step-2-find-out-hello-port-that-you-can-use-tooremote-connect-toohello-specific-vm-scale-set-instancenode"></a><span data-ttu-id="abcca-137">Krok 2: Připojení zjistěte hello portu, které můžete použít tooremote toohello konkrétní sady škálování virtuálního počítače instance/uzlu</span><span class="sxs-lookup"><span data-stu-id="abcca-137">Step 2: Find out hello port that you can use tooremote connect toohello specific VM Scale Set instance/node</span></span>
<span data-ttu-id="abcca-138">Dříve v tomto dokumentu I věnovala tomu, jak toohello uzlů mapování hello instance Škálovací sady virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="abcca-138">Earlier in this document, I talked about how hello VM Scale Set instances map toohello nodes.</span></span> <span data-ttu-id="abcca-139">Budeme používat tento toofigure hello přesný portu.</span><span class="sxs-lookup"><span data-stu-id="abcca-139">We will use that toofigure out hello exact port.</span></span>

<span data-ttu-id="abcca-140">Hello porty jsou přiděleny ve vzestupném pořadí hello instance Škálovací sady virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="abcca-140">hello ports are allocated in ascending order of hello VM Scale Set instance.</span></span> <span data-ttu-id="abcca-141">v mé příklad hello typ uzlu front-endu, tak hello porty pro každou hello pět instancí jsou následující hello.</span><span class="sxs-lookup"><span data-stu-id="abcca-141">so in my example for hello FrontEnd node type, hello ports for each of hello five instances are hello following.</span></span> <span data-ttu-id="abcca-142">je teď třeba toodo hello stejné mapování pro instance Škálovací sady virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="abcca-142">you now need toodo hello same mapping for your VM Scale Set instance.</span></span>

| <span data-ttu-id="abcca-143">**Instance sady škálování virtuálního počítače**</span><span class="sxs-lookup"><span data-stu-id="abcca-143">**VM Scale Set Instance**</span></span> | <span data-ttu-id="abcca-144">**Port**</span><span class="sxs-lookup"><span data-stu-id="abcca-144">**Port**</span></span> |
| --- | --- |
| <span data-ttu-id="abcca-145">FrontEnd_0</span><span class="sxs-lookup"><span data-stu-id="abcca-145">FrontEnd_0</span></span> |<span data-ttu-id="abcca-146">3389</span><span class="sxs-lookup"><span data-stu-id="abcca-146">3389</span></span> |
| <span data-ttu-id="abcca-147">FrontEnd_1</span><span class="sxs-lookup"><span data-stu-id="abcca-147">FrontEnd_1</span></span> |<span data-ttu-id="abcca-148">3390</span><span class="sxs-lookup"><span data-stu-id="abcca-148">3390</span></span> |
| <span data-ttu-id="abcca-149">FrontEnd_2</span><span class="sxs-lookup"><span data-stu-id="abcca-149">FrontEnd_2</span></span> |<span data-ttu-id="abcca-150">3391</span><span class="sxs-lookup"><span data-stu-id="abcca-150">3391</span></span> |
| <span data-ttu-id="abcca-151">FrontEnd_3</span><span class="sxs-lookup"><span data-stu-id="abcca-151">FrontEnd_3</span></span> |<span data-ttu-id="abcca-152">3392</span><span class="sxs-lookup"><span data-stu-id="abcca-152">3392</span></span> |
| <span data-ttu-id="abcca-153">FrontEnd_4</span><span class="sxs-lookup"><span data-stu-id="abcca-153">FrontEnd_4</span></span> |<span data-ttu-id="abcca-154">3393</span><span class="sxs-lookup"><span data-stu-id="abcca-154">3393</span></span> |
| <span data-ttu-id="abcca-155">FrontEnd_5</span><span class="sxs-lookup"><span data-stu-id="abcca-155">FrontEnd_5</span></span> |<span data-ttu-id="abcca-156">3394</span><span class="sxs-lookup"><span data-stu-id="abcca-156">3394</span></span> |

### <a name="step-3-remote-connect-toohello-specific-vm-scale-set-instance"></a><span data-ttu-id="abcca-157">Krok 3: Vzdáleného připojení toohello konkrétní instance Škálovací sady virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="abcca-157">Step 3: Remote connect toohello specific VM Scale Set instance</span></span>
<span data-ttu-id="abcca-158">V následující snímek obrazovky hello použít připojení ke vzdálené ploše tooconnect toohello FrontEnd_1:</span><span class="sxs-lookup"><span data-stu-id="abcca-158">In hello screenshot below I use Remote Desktop Connection tooconnect toohello FrontEnd_1:</span></span>

![PROTOKOLU RDP][RDP]

## <a name="how-toochange-hello-rdp-port-range-values"></a><span data-ttu-id="abcca-160">Jak toochange hello portu RDP rozsahu hodnot</span><span class="sxs-lookup"><span data-stu-id="abcca-160">How toochange hello RDP port range values</span></span>
### <a name="before-cluster-deployment"></a><span data-ttu-id="abcca-161">Před nasazením clusteru</span><span class="sxs-lookup"><span data-stu-id="abcca-161">Before cluster deployment</span></span>
<span data-ttu-id="abcca-162">Když nastavujete hello clusteru pomocí šablony Resource Manager, můžete zadat rozsah hello v hello **inboundNatPools**.</span><span class="sxs-lookup"><span data-stu-id="abcca-162">When you are setting up hello cluster using an Resource Manager template, you can specify hello range in hello **inboundNatPools**.</span></span>

<span data-ttu-id="abcca-163">Přejděte toohello definice prostředků **Microsoft.Network/loadBalancers**.</span><span class="sxs-lookup"><span data-stu-id="abcca-163">Go toohello resource definition for **Microsoft.Network/loadBalancers**.</span></span> <span data-ttu-id="abcca-164">V části, který najdete popis hello **inboundNatPools**.</span><span class="sxs-lookup"><span data-stu-id="abcca-164">Under that you find hello description for **inboundNatPools**.</span></span>  <span data-ttu-id="abcca-165">Nahraďte hello *frontendPortRangeStart* a *frontendPortRangeEnd* hodnoty.</span><span class="sxs-lookup"><span data-stu-id="abcca-165">Replace hello *frontendPortRangeStart* and *frontendPortRangeEnd* values.</span></span>

![InboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a><span data-ttu-id="abcca-167">Po nasazení clusteru</span><span class="sxs-lookup"><span data-stu-id="abcca-167">After cluster deployment</span></span>
<span data-ttu-id="abcca-168">Toto je složitější a může vést k virtuálním počítačům hello recyklaci.</span><span class="sxs-lookup"><span data-stu-id="abcca-168">This is a bit more involved and may result in hello VMs getting recycled.</span></span> <span data-ttu-id="abcca-169">Nyní máte tooset nových hodnot, pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="abcca-169">You will now have tooset new values using Azure PowerShell.</span></span> <span data-ttu-id="abcca-170">Ujistěte se, že je v počítači nainstalován prostředí Azure PowerShell 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="abcca-170">Make sure that Azure PowerShell 1.0 or later is installed on your machine.</span></span> <span data-ttu-id="abcca-171">Pokud jste to před neučinili, I důrazně doporučujeme postupujte podle kroků hello uvedených v [jak tooinstall a konfigurace prostředí Azure PowerShell.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="abcca-171">If you have not done this before, I strongly suggest that you follow hello steps outlined in [How tooinstall and configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="abcca-172">Přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="abcca-172">Sign in tooyour Azure account.</span></span> <span data-ttu-id="abcca-173">Pokud z nějakého důvodu selže tento příkaz prostředí PowerShell, byste měli zkontrolovat, jestli máte Azure PowerShell správně nainstalován.</span><span class="sxs-lookup"><span data-stu-id="abcca-173">If this PowerShell command fails for some reason, you should check whether you have Azure PowerShell installed correctly.</span></span>

```
Login-AzureRmAccount
```

<span data-ttu-id="abcca-174">Spustit hello následujících podrobnostech tooget na nástroj pro vyrovnávání zatížení a zobrazí hello hodnoty pro popis hello **inboundNatPools**:</span><span class="sxs-lookup"><span data-stu-id="abcca-174">Run hello following tooget details on your load balancer and you see hello values for hello description for **inboundNatPools**:</span></span>

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

<span data-ttu-id="abcca-175">Nastaví *frontendPortRangeEnd* a *frontendPortRangeStart* toohello hodnoty, které mají.</span><span class="sxs-lookup"><span data-stu-id="abcca-175">Now set *frontendPortRangeEnd* and *frontendPortRangeStart* toohello values you want.</span></span>

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use hello API version that get returned> -Force
```


## <a name="next-steps"></a><span data-ttu-id="abcca-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="abcca-176">Next steps</span></span>
* [<span data-ttu-id="abcca-177">Přehled funkce "Kdekoli nasadit" hello a porovnání s Azure spravované clustery</span><span class="sxs-lookup"><span data-stu-id="abcca-177">Overview of hello "Deploy anywhere" feature and a comparison with Azure-managed clusters</span></span>](service-fabric-deploy-anywhere.md)
* [<span data-ttu-id="abcca-178">Zabezpečení clusteru</span><span class="sxs-lookup"><span data-stu-id="abcca-178">Cluster security</span></span>](service-fabric-cluster-security.md)
* [<span data-ttu-id="abcca-179">Service Fabric SDK a Začínáme</span><span class="sxs-lookup"><span data-stu-id="abcca-179"> Service Fabric SDK and getting started</span></span>](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
