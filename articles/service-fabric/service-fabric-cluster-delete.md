---
title: "Odstranění clusteru služby Azure a jeho prostředků | Microsoft Docs"
description: "Zjistěte, jak úplně odstranit cluster Service Fabric odstranění skupiny prostředků obsahující cluster nebo selektivně odstraněním prostředky."
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
ms.openlocfilehash: 7672aa12421fbe4ad86e7315d6a7a06c2ff5124d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="delete-a-service-fabric-cluster-on-azure-and-the-resources-it-uses"></a><span data-ttu-id="7c51a-103">Odstranění clusteru Service Fabric na Azure a prostředky, které používá</span><span class="sxs-lookup"><span data-stu-id="7c51a-103">Delete a Service Fabric cluster on Azure and the resources it uses</span></span>
<span data-ttu-id="7c51a-104">Cluster Service Fabric se skládá z mnoha dalším prostředkům služby Azure kromě prostředek clusteru sám sebe.</span><span class="sxs-lookup"><span data-stu-id="7c51a-104">A Service Fabric cluster is made up of many other Azure resources in addition to the cluster resource itself.</span></span> <span data-ttu-id="7c51a-105">Proto je ke kompletnímu odstranění clusteru Service Fabric potřeba odstranit taky prostředky, které ho tvoří.</span><span class="sxs-lookup"><span data-stu-id="7c51a-105">So to completely delete a Service Fabric cluster you also need to delete all the resources it is made of.</span></span>
<span data-ttu-id="7c51a-106">Máte dvě možnosti: buď odstranit skupinu prostředků, který je cluster v (která odstraňuje prostředek clusteru a všechny další prostředky ve skupině prostředků) nebo konkrétně odstranit prostředek clusteru a je přiřazen prostředky (ale ne další prostředky ve skupině prostředků).</span><span class="sxs-lookup"><span data-stu-id="7c51a-106">You have two options: Either delete the resource group that the cluster is in (which deletes the cluster resource and any other resources in the resource group) or specifically delete the cluster resource and it's associated resources (but not other resources in the resource group).</span></span>

> [!NOTE]
> <span data-ttu-id="7c51a-107">Odstranění prostředku clusteru **nemá** odstranit všechny ostatní prostředky, které váš cluster Service Fabric tvoří.</span><span class="sxs-lookup"><span data-stu-id="7c51a-107">Deleting the cluster resource **does not** delete all the other resources that your Service Fabric cluster is composed of.</span></span>
> 
> 

## <a name="delete-the-entire-resource-group-rg-that-the-service-fabric-cluster-is-in"></a><span data-ttu-id="7c51a-108">Odstraňte skupinu prostředků celý (RG), který je cluster Service Fabric v</span><span class="sxs-lookup"><span data-stu-id="7c51a-108">Delete the entire resource group (RG) that the Service Fabric cluster is in</span></span>
<span data-ttu-id="7c51a-109">Toto je nejjednodušší způsob, jak zajistit, že odstraníte všechny prostředky přidruženého k vašemu clusteru, včetně skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c51a-109">This is the easiest way to ensure that you delete all the resources associated with your cluster, including the resource group.</span></span> <span data-ttu-id="7c51a-110">Odstraněním skupiny prostředků pomocí prostředí PowerShell nebo prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7c51a-110">You can delete the resource group using PowerShell or through the Azure portal.</span></span> <span data-ttu-id="7c51a-111">Pokud vaše skupina prostředků obsahuje prostředky, které nesouvisí se Service fabric cluster, můžete odstranit konkrétní prostředky.</span><span class="sxs-lookup"><span data-stu-id="7c51a-111">If your resource group has resources that are not related to Service fabric cluster, then you can delete specific resources.</span></span>

### <a name="delete-the-resource-group-using-azure-powershell"></a><span data-ttu-id="7c51a-112">Odstraňte skupinu prostředků pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7c51a-112">Delete the resource group using Azure PowerShell</span></span>
<span data-ttu-id="7c51a-113">Můžete také odstranit skupinu prostředků spuštěním následující rutiny prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7c51a-113">You can also delete the resource group by running the following Azure PowerShell cmdlets.</span></span> <span data-ttu-id="7c51a-114">Ujistěte se, že Azure PowerShell 1.0 nebo vyšší je v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="7c51a-114">Make sure Azure PowerShell 1.0 or greater is installed on your computer.</span></span> <span data-ttu-id="7c51a-115">Pokud jste to před neudělali, postupujte podle kroků uvedených v [postup instalace a konfigurace prostředí Azure PowerShell.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="7c51a-115">If you have not done this before, follow the steps outlined in [How to install and Configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="7c51a-116">Otevřete okno prostředí PowerShell a spusťte následující rutiny PS:</span><span class="sxs-lookup"><span data-stu-id="7c51a-116">Open a PowerShell window and run the following PS cmdlets:</span></span>

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

<span data-ttu-id="7c51a-117">Zobrazí se výzva k potvrzení odstranění, pokud jste nepoužili *-Force* možnost.</span><span class="sxs-lookup"><span data-stu-id="7c51a-117">You will get a prompt to confirm the deletion if you did not use the *-Force* option.</span></span> <span data-ttu-id="7c51a-118">Na potvrzení RG a všechny prostředky, které obsahuje, se odstraní.</span><span class="sxs-lookup"><span data-stu-id="7c51a-118">On confirmation the RG and all the resources it contains are deleted.</span></span>

### <a name="delete-a-resource-group-in-the-azure-portal"></a><span data-ttu-id="7c51a-119">Odstranit skupinu prostředků na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7c51a-119">Delete a resource group in the Azure portal</span></span>
1. <span data-ttu-id="7c51a-120">Přihlášení k [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7c51a-120">Login to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7c51a-121">Přejděte ke clusteru Service Fabric, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="7c51a-121">Navigate to the Service Fabric cluster you want to delete.</span></span>
3. <span data-ttu-id="7c51a-122">Klikněte na název skupiny prostředků na stránce essentials clusteru.</span><span class="sxs-lookup"><span data-stu-id="7c51a-122">Click on the Resource Group name on the cluster essentials page.</span></span>
4. <span data-ttu-id="7c51a-123">Po výběru této možnosti **Essentials skupiny prostředků** stránky.</span><span class="sxs-lookup"><span data-stu-id="7c51a-123">This brings up the **Resource Group Essentials** page.</span></span>
5. <span data-ttu-id="7c51a-124">Klikněte na **Odstranit**.</span><span class="sxs-lookup"><span data-stu-id="7c51a-124">Click **Delete**.</span></span>
6. <span data-ttu-id="7c51a-125">Postupujte podle pokynů na této stránce dokončení odstranění skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7c51a-125">Follow the instructions on that page to complete the deletion of the resource group.</span></span>

![Odstranění skupiny prostředků][ResourceGroupDelete]

## <a name="delete-the-cluster-resource-and-the-resources-it-uses-but-not-other-resources-in-the-resource-group"></a><span data-ttu-id="7c51a-127">Odstranit prostředek clusteru a prostředky, které používá, ale ne další prostředky ve skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="7c51a-127">Delete the cluster resource and the resources it uses, but not other resources in the resource group</span></span>
<span data-ttu-id="7c51a-128">Pokud vaše skupina prostředků má jenom prostředky, které se vztahují k cluster Service Fabric, které chcete odstranit, je snazší Odstraňte skupinu prostředků celý.</span><span class="sxs-lookup"><span data-stu-id="7c51a-128">If your resource group has only resources that are related to the Service Fabric cluster you want to delete, then it is easier to delete the entire resource group.</span></span> <span data-ttu-id="7c51a-129">Pokud chcete selektivně odstranění prostředků po jednom ve vaší skupině prostředků, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="7c51a-129">If you want to selectively delete the resources one-by-one in your resource group, then follow these steps.</span></span>

<span data-ttu-id="7c51a-130">Pokud jste nasadili cluster pomocí portálu nebo pomocí jedné z šablon služby Správce prostředků infrastruktury z Galerie šablon, jsou všechny prostředky, které cluster používá označené s těmito dvěma značkami.</span><span class="sxs-lookup"><span data-stu-id="7c51a-130">If you deployed your cluster using the portal or using one of the Service Fabric Resource Manager templates from the template gallery, then all the resources that the cluster uses are tagged with the following two tags.</span></span> <span data-ttu-id="7c51a-131">Můžete je používat k rozhodování o prostředky, ke kterým chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="7c51a-131">You can use them to decide which resources you want to delete.</span></span>

<span data-ttu-id="7c51a-132">***Značka č. 1:*** klíč = clusterName, hodnota = "název clusteru.</span><span class="sxs-lookup"><span data-stu-id="7c51a-132">***Tag#1:*** Key = clusterName, Value = 'name of the cluster'</span></span>

<span data-ttu-id="7c51a-133">***Značka č. 2:*** klíč = resourceName, hodnota = ServiceFabric</span><span class="sxs-lookup"><span data-stu-id="7c51a-133">***Tag#2:*** Key = resourceName, Value = ServiceFabric</span></span>

### <a name="delete-specific-resources-in-the-azure-portal"></a><span data-ttu-id="7c51a-134">Odstranit konkrétní prostředky na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7c51a-134">Delete specific resources in the Azure portal</span></span>
1. <span data-ttu-id="7c51a-135">Přihlášení k [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7c51a-135">Login to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7c51a-136">Přejděte ke clusteru Service Fabric, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="7c51a-136">Navigate to the Service Fabric cluster you want to delete.</span></span>
3. <span data-ttu-id="7c51a-137">Přejděte na **všechna nastavení** v okně essentials.</span><span class="sxs-lookup"><span data-stu-id="7c51a-137">Go to **All settings** on the essentials blade.</span></span>
4. <span data-ttu-id="7c51a-138">Klikněte na **značky** pod **Správa prostředků** v okně nastavení.</span><span class="sxs-lookup"><span data-stu-id="7c51a-138">Click on **Tags** under **Resource Management** in the settings blade.</span></span>
5. <span data-ttu-id="7c51a-139">Klikněte na jednu z **značky** v okně značky k získání seznamu všech prostředků, které jsou s touto značkou.</span><span class="sxs-lookup"><span data-stu-id="7c51a-139">Click on one of the **Tags** in the tags blade to get a list of all the resources with that tag.</span></span>
   
    ![Značky prostředku][ResourceTags]
6. <span data-ttu-id="7c51a-141">Jakmile máte seznam prostředků s příznakem, klikněte na jednotlivých zdrojích a odstraňte je.</span><span class="sxs-lookup"><span data-stu-id="7c51a-141">Once you have the list of tagged resources, click on each of the resources and delete them.</span></span>
   
    ![Prostředky s příznakem][TaggedResources]

### <a name="delete-the-resources-using-azure-powershell"></a><span data-ttu-id="7c51a-143">Odstranění prostředků pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7c51a-143">Delete the resources using Azure PowerShell</span></span>
<span data-ttu-id="7c51a-144">Spuštěním následující rutiny prostředí Azure PowerShell můžete odstranit prostředky po jednom.</span><span class="sxs-lookup"><span data-stu-id="7c51a-144">You can delete the resources one-by-one by running the following Azure PowerShell cmdlets.</span></span> <span data-ttu-id="7c51a-145">Ujistěte se, že Azure PowerShell 1.0 nebo vyšší je v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="7c51a-145">Make sure Azure PowerShell 1.0 or greater is installed on your computer.</span></span> <span data-ttu-id="7c51a-146">Pokud jste to před neudělali, postupujte podle kroků uvedených v [postup instalace a konfigurace prostředí Azure PowerShell.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="7c51a-146">If you have not done this before, follow the steps outlined in [How to install and Configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="7c51a-147">Otevřete okno prostředí PowerShell a spusťte následující rutiny PS:</span><span class="sxs-lookup"><span data-stu-id="7c51a-147">Open a PowerShell window and run the following PS cmdlets:</span></span>

```powershell
Login-AzureRmAccount
```
<span data-ttu-id="7c51a-148">Pro každou prostředků chcete odstranit, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="7c51a-148">For each of the resources you want to delete, run the following:</span></span>

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of the resource group>" -Force
```

<span data-ttu-id="7c51a-149">Pokud chcete odstranit prostředek clusteru, spusťte následující:</span><span class="sxs-lookup"><span data-stu-id="7c51a-149">To delete the cluster resource, run the following:</span></span>

```powershell
Remove-AzureRmResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of the resource group>" -Force
```

## <a name="next-steps"></a><span data-ttu-id="7c51a-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7c51a-150">Next steps</span></span>
<span data-ttu-id="7c51a-151">Přečtěte si následující také další informace o upgradu clusteru a rozdělení do oddílů služby:</span><span class="sxs-lookup"><span data-stu-id="7c51a-151">Read the following to also learn about upgrading a cluster and partitioning services:</span></span>

* [<span data-ttu-id="7c51a-152">Další informace o upgrade clusteru</span><span class="sxs-lookup"><span data-stu-id="7c51a-152">Learn about cluster upgrades</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="7c51a-153">Další informace o vytváření oddílů stavové služby pro maximální měřítko</span><span class="sxs-lookup"><span data-stu-id="7c51a-153">Learn about partitioning stateful services for maximum scale</span></span>](service-fabric-concepts-partitioning.md)

<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
