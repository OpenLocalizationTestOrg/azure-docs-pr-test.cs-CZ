---
title: "aaaDelete Azure cluster a jeho prostředky | Microsoft Docs"
description: "Zjistěte, jak odstranit toocompletely Service Fabric clusteru buď odstraňování skupiny prostředků hello obsahující hello clusteru nebo selektivně odstraněním prostředky."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: de422950-2d22-4ddb-ac47-dd663a946a7e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/24/2017
ms.author: chackdan
ms.openlocfilehash: 5c15a4184644da715cd69397f2150de86ab433ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-service-fabric-cluster-on-azure-and-hello-resources-it-uses"></a><span data-ttu-id="facc8-103">Odstranění clusteru Service Fabric na Azure a hello prostředky, které používá</span><span class="sxs-lookup"><span data-stu-id="facc8-103">Delete a Service Fabric cluster on Azure and hello resources it uses</span></span>
<span data-ttu-id="facc8-104">Cluster Service Fabric se skládá z mnoha dalším prostředkům služby Azure kromě toohello prostředek clusteru, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="facc8-104">A Service Fabric cluster is made up of many other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="facc8-105">Takže toocompletely odstranit cluster Service Fabric je také nutné toodelete všechny prostředky, které se provádí z hello.</span><span class="sxs-lookup"><span data-stu-id="facc8-105">So toocompletely delete a Service Fabric cluster you also need toodelete all hello resources it is made of.</span></span>
<span data-ttu-id="facc8-106">Máte dvě možnosti: Skupina prostředků buď odstranit hello hello clusteru je v (která odstraňuje hello prostředku clusteru a všechny další prostředky ve skupině prostředků hello) nebo konkrétně odstranit prostředek clusteru hello a je přiřazen prostředky (ale ne jiné prostředky ve skupině prostředků hello).</span><span class="sxs-lookup"><span data-stu-id="facc8-106">You have two options: Either delete hello resource group that hello cluster is in (which deletes hello cluster resource and any other resources in hello resource group) or specifically delete hello cluster resource and it's associated resources (but not other resources in hello resource group).</span></span>

> [!NOTE]
> <span data-ttu-id="facc8-107">Odstranění prostředku clusteru hello **nemá** odstranit všechny další prostředky, které váš cluster Service Fabric tvoří hello.</span><span class="sxs-lookup"><span data-stu-id="facc8-107">Deleting hello cluster resource **does not** delete all hello other resources that your Service Fabric cluster is composed of.</span></span>
> 
> 

## <a name="delete-hello-entire-resource-group-rg-that-hello-service-fabric-cluster-is-in"></a><span data-ttu-id="facc8-108">Odstraňte skupinu prostředků celý hello (RG), který hello Cluster Service Fabric</span><span class="sxs-lookup"><span data-stu-id="facc8-108">Delete hello entire resource group (RG) that hello Service Fabric cluster is in</span></span>
<span data-ttu-id="facc8-109">Toto je hello nejjednodušší způsob, jak tooensure odstranit všechny prostředky hello přidruženého k vašemu clusteru, včetně hello skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="facc8-109">This is hello easiest way tooensure that you delete all hello resources associated with your cluster, including hello resource group.</span></span> <span data-ttu-id="facc8-110">Odstraněním skupiny prostředků hello pomocí prostředí PowerShell nebo prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="facc8-110">You can delete hello resource group using PowerShell or through hello Azure portal.</span></span> <span data-ttu-id="facc8-111">Pokud vaše skupina prostředků má prostředky, které nejsou clusteru související tooService infrastruktury, můžete odstranit konkrétní prostředky.</span><span class="sxs-lookup"><span data-stu-id="facc8-111">If your resource group has resources that are not related tooService fabric cluster, then you can delete specific resources.</span></span>

### <a name="delete-hello-resource-group-using-azure-powershell"></a><span data-ttu-id="facc8-112">Odstraňte skupinu prostředků hello pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="facc8-112">Delete hello resource group using Azure PowerShell</span></span>
<span data-ttu-id="facc8-113">Můžete také odstranit skupinu prostředků hello spuštěním následující rutiny prostředí Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="facc8-113">You can also delete hello resource group by running hello following Azure PowerShell cmdlets.</span></span> <span data-ttu-id="facc8-114">Ujistěte se, že Azure PowerShell 1.0 nebo vyšší je v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="facc8-114">Make sure Azure PowerShell 1.0 or greater is installed on your computer.</span></span> <span data-ttu-id="facc8-115">Pokud jste to před neudělali, postupujte podle kroků hello uvedených v [jak tooinstall a konfigurace prostředí Azure PowerShell.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="facc8-115">If you have not done this before, follow hello steps outlined in [How tooinstall and Configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="facc8-116">Otevřete okno prostředí PowerShell a spusťte následující rutiny PS hello:</span><span class="sxs-lookup"><span data-stu-id="facc8-116">Open a PowerShell window and run hello following PS cmdlets:</span></span>

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

<span data-ttu-id="facc8-117">Zobrazí se výzva tooconfirm odstranění hello, pokud jste nepoužili hello *-Force* možnost.</span><span class="sxs-lookup"><span data-stu-id="facc8-117">You will get a prompt tooconfirm hello deletion if you did not use hello *-Force* option.</span></span> <span data-ttu-id="facc8-118">Na potvrzovací hello RG a jsou odstraněny všechny hello prostředky, které obsahuje.</span><span class="sxs-lookup"><span data-stu-id="facc8-118">On confirmation hello RG and all hello resources it contains are deleted.</span></span>

### <a name="delete-a-resource-group-in-hello-azure-portal"></a><span data-ttu-id="facc8-119">Odstranění skupiny prostředků v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="facc8-119">Delete a resource group in hello Azure portal</span></span>
1. <span data-ttu-id="facc8-120">Přihlášení toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="facc8-120">Login toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="facc8-121">Přejděte cluster Service Fabric toohello chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="facc8-121">Navigate toohello Service Fabric cluster you want toodelete.</span></span>
3. <span data-ttu-id="facc8-122">Klikněte na hello název skupiny prostředků na stránce essentials hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="facc8-122">Click on hello Resource Group name on hello cluster essentials page.</span></span>
4. <span data-ttu-id="facc8-123">Otevře hello **Essentials skupiny prostředků** stránky.</span><span class="sxs-lookup"><span data-stu-id="facc8-123">This brings up hello **Resource Group Essentials** page.</span></span>
5. <span data-ttu-id="facc8-124">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="facc8-124">Click **Delete**.</span></span>
6. <span data-ttu-id="facc8-125">Postupujte podle pokynů hello na této stránce toocomplete hello odstranění skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="facc8-125">Follow hello instructions on that page toocomplete hello deletion of hello resource group.</span></span>

![Odstranění skupiny prostředků][ResourceGroupDelete]

## <a name="delete-hello-cluster-resource-and-hello-resources-it-uses-but-not-other-resources-in-hello-resource-group"></a><span data-ttu-id="facc8-127">Odstranit prostředek clusteru hello a hello prostředky, které používá, ale ne další prostředky ve skupině prostředků hello</span><span class="sxs-lookup"><span data-stu-id="facc8-127">Delete hello cluster resource and hello resources it uses, but not other resources in hello resource group</span></span>
<span data-ttu-id="facc8-128">Pokud skupina prostředků má jenom prostředky, které se cluster Service Fabric související toohello chcete toodelete, pak je snazší toodelete hello celé skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="facc8-128">If your resource group has only resources that are related toohello Service Fabric cluster you want toodelete, then it is easier toodelete hello entire resource group.</span></span> <span data-ttu-id="facc8-129">Pokud chcete tooselectively odstranění hello prostředky po jednom ve vaší skupině prostředků, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="facc8-129">If you want tooselectively delete hello resources one-by-one in your resource group, then follow these steps.</span></span>

<span data-ttu-id="facc8-130">Pokud jste nasadili cluster pomocí hello portálu nebo pomocí jednoho ze šablony služby Správce prostředků infrastruktury hello z Galerie šablon hello, pak všechny hello prostředky, které hello používá clusteru jsou označené hello následující dvě značky.</span><span class="sxs-lookup"><span data-stu-id="facc8-130">If you deployed your cluster using hello portal or using one of hello Service Fabric Resource Manager templates from hello template gallery, then all hello resources that hello cluster uses are tagged with hello following two tags.</span></span> <span data-ttu-id="facc8-131">Můžete je použít toodecide prostředky, ke kterým chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="facc8-131">You can use them toodecide which resources you want toodelete.</span></span>

<span data-ttu-id="facc8-132">***Značka č. 1:*** klíč = clusterName, hodnota = "název clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="facc8-132">***Tag#1:*** Key = clusterName, Value = 'name of hello cluster'</span></span>

<span data-ttu-id="facc8-133">***Značka č. 2:*** klíč = resourceName, hodnota = ServiceFabric</span><span class="sxs-lookup"><span data-stu-id="facc8-133">***Tag#2:*** Key = resourceName, Value = ServiceFabric</span></span>

### <a name="delete-specific-resources-in-hello-azure-portal"></a><span data-ttu-id="facc8-134">Odstranit konkrétní prostředky v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="facc8-134">Delete specific resources in hello Azure portal</span></span>
1. <span data-ttu-id="facc8-135">Přihlášení toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="facc8-135">Login toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="facc8-136">Přejděte cluster Service Fabric toohello chcete toodelete.</span><span class="sxs-lookup"><span data-stu-id="facc8-136">Navigate toohello Service Fabric cluster you want toodelete.</span></span>
3. <span data-ttu-id="facc8-137">Přejděte příliš**všechna nastavení** v okně essentials hello.</span><span class="sxs-lookup"><span data-stu-id="facc8-137">Go too**All settings** on hello essentials blade.</span></span>
4. <span data-ttu-id="facc8-138">Klikněte na **značky** pod **Správa prostředků** v okně Nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="facc8-138">Click on **Tags** under **Resource Management** in hello settings blade.</span></span>
5. <span data-ttu-id="facc8-139">Klikněte na jednu z hello **značky** v hello značky okno tooget seznam všech prostředků hello s touto značkou.</span><span class="sxs-lookup"><span data-stu-id="facc8-139">Click on one of hello **Tags** in hello tags blade tooget a list of all hello resources with that tag.</span></span>
   
    ![Značky prostředku][ResourceTags]
6. <span data-ttu-id="facc8-141">Jakmile máte seznam hello s příznakem prostředků, klikněte na každou hello prostředků a odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="facc8-141">Once you have hello list of tagged resources, click on each of hello resources and delete them.</span></span>
   
    ![Prostředky s příznakem][TaggedResources]

### <a name="delete-hello-resources-using-azure-powershell"></a><span data-ttu-id="facc8-143">Odstranit hello prostředků pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="facc8-143">Delete hello resources using Azure PowerShell</span></span>
<span data-ttu-id="facc8-144">Spuštěním následující rutiny prostředí Azure PowerShell hello můžete odstranit hello prostředky po jednom.</span><span class="sxs-lookup"><span data-stu-id="facc8-144">You can delete hello resources one-by-one by running hello following Azure PowerShell cmdlets.</span></span> <span data-ttu-id="facc8-145">Ujistěte se, že Azure PowerShell 1.0 nebo vyšší je v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="facc8-145">Make sure Azure PowerShell 1.0 or greater is installed on your computer.</span></span> <span data-ttu-id="facc8-146">Pokud jste to před neudělali, postupujte podle kroků hello uvedených v [jak tooinstall a konfigurace prostředí Azure PowerShell.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="facc8-146">If you have not done this before, follow hello steps outlined in [How tooinstall and Configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="facc8-147">Otevřete okno prostředí PowerShell a spusťte následující rutiny PS hello:</span><span class="sxs-lookup"><span data-stu-id="facc8-147">Open a PowerShell window and run hello following PS cmdlets:</span></span>

```powershell
Login-AzureRmAccount
```
<span data-ttu-id="facc8-148">Pro každou hello prostředků má toodelete, spusťte následující hello:</span><span class="sxs-lookup"><span data-stu-id="facc8-148">For each of hello resources you want toodelete, run hello following:</span></span>

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of hello resource group>" -Force
```

<span data-ttu-id="facc8-149">toodelete hello prostředek clusteru, spusťte následující hello:</span><span class="sxs-lookup"><span data-stu-id="facc8-149">toodelete hello cluster resource, run hello following:</span></span>

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of hello resource group>" -Force
```

## <a name="next-steps"></a><span data-ttu-id="facc8-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="facc8-150">Next steps</span></span>
<span data-ttu-id="facc8-151">Čtení hello následující tooalso Další informace o upgradu clusteru a rozdělení do oddílů služby:</span><span class="sxs-lookup"><span data-stu-id="facc8-151">Read hello following tooalso learn about upgrading a cluster and partitioning services:</span></span>

* [<span data-ttu-id="facc8-152">Další informace o upgrade clusteru</span><span class="sxs-lookup"><span data-stu-id="facc8-152">Learn about cluster upgrades</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="facc8-153">Další informace o vytváření oddílů stavové služby pro maximální měřítko</span><span class="sxs-lookup"><span data-stu-id="facc8-153">Learn about partitioning stateful services for maximum scale</span></span>](service-fabric-concepts-partitioning.md)

<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
