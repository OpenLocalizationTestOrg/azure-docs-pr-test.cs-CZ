---
title: "Označit prostředky Azure pro logické organizaci | Microsoft Docs"
description: "Ukazuje způsob použití značek k uspořádání prostředků Azure pro fakturace a správy."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 003a78e5-2ff8-4685-93b4-e94d6fb8ed5b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: AzurePortal
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: tomfitz
ms.openlocfilehash: 4f52c30614ad39da8a34ff6ecfb707b75400517f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-tags-to-organize-your-azure-resources"></a><span data-ttu-id="8cb97-103">Používání značek k uspořádání prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="8cb97-103">Use tags to organize your Azure resources</span></span>
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

> [!NOTE]
> <span data-ttu-id="8cb97-104">Značky můžete použít pouze na prostředky, které podporují operace Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8cb97-104">You can apply tags only to resources that support Azure Resource Manager operations.</span></span> <span data-ttu-id="8cb97-105">Pokud jste vytvořili virtuální počítač, virtuální sítě nebo účet úložiště pomocí modelu nasazení classic (například prostřednictvím portálu Azure classic), značku nelze použít k danému prostředku.</span><span class="sxs-lookup"><span data-stu-id="8cb97-105">If you created a virtual machine, virtual network, or storage account through the classic deployment model (such as through the Azure classic portal), you cannot apply a tag to that resource.</span></span> <span data-ttu-id="8cb97-106">Pro podporu označování, znovu nasaďte tyto prostředky prostřednictvím Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8cb97-106">To support tagging, redeploy these resources through Resource Manager.</span></span> <span data-ttu-id="8cb97-107">Všechny ostatní prostředky podporují označování.</span><span class="sxs-lookup"><span data-stu-id="8cb97-107">All other resources support tagging.</span></span>
> 
> 

## <a name="policies-for-tag-consistency"></a><span data-ttu-id="8cb97-108">Zásady pro značka konzistence</span><span class="sxs-lookup"><span data-stu-id="8cb97-108">Policies for tag consistency</span></span>

<span data-ttu-id="8cb97-109">Zásady prostředků můžete vytvořit standardní pravidla pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="8cb97-109">You can use resource policies to create standard rules for your organization.</span></span> <span data-ttu-id="8cb97-110">Můžete vytvořit zásady, které zajišťují, že prostředky jsou označené odpovídající hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8cb97-110">You can create policies that ensure resources are tagged with the appropriate values.</span></span> <span data-ttu-id="8cb97-111">Další informace najdete v tématu [zásad prostředků pro značky](resource-manager-policy-tags.md).</span><span class="sxs-lookup"><span data-stu-id="8cb97-111">For more information, see [Apply resource policies for tags](resource-manager-policy-tags.md).</span></span>

## <a name="powershell"></a><span data-ttu-id="8cb97-112">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8cb97-112">PowerShell</span></span>
[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a><span data-ttu-id="8cb97-113">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8cb97-113">Azure CLI</span></span>

<span data-ttu-id="8cb97-114">Pokud chcete zobrazit existující značky pro *skupinu prostředků*, použijte:</span><span class="sxs-lookup"><span data-stu-id="8cb97-114">To see the existing tags for a *resource group*, use:</span></span>

```azurecli
az group show -n examplegroup --query tags
```

<span data-ttu-id="8cb97-115">Výstup tohoto skriptu bude v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="8cb97-115">That script returns the following format:</span></span>

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

<span data-ttu-id="8cb97-116">Pokud chcete zobrazit existující značky pro *prostředek s konkrétním ID prostředku*, použijte:</span><span class="sxs-lookup"><span data-stu-id="8cb97-116">To see the existing tags for a *resource that has a specified resource ID*, use:</span></span>

```azurecli
az resource show --id {resource-id} --query tags
```

<span data-ttu-id="8cb97-117">Nebo, pokud chcete zobrazit stávající značky pro *prostředek, který má zadaný název, typ a prostředků skupinu*, použijte:</span><span class="sxs-lookup"><span data-stu-id="8cb97-117">Or, to see the existing tags for a *resource that has a specified name, type, and resource group*, use:</span></span>

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

<span data-ttu-id="8cb97-118">Skupiny prostředků, které mají s konkrétní značkou tag, použijte `az group list`:</span><span class="sxs-lookup"><span data-stu-id="8cb97-118">To get resource groups that have a specific tag, use `az group list`:</span></span>

```azurecli
az group list --tag Dept=IT
```

<span data-ttu-id="8cb97-119">Všechny prostředky, které mají určitý značky a hodnoty, použijte `az resource list`:</span><span class="sxs-lookup"><span data-stu-id="8cb97-119">To get all the resources that have a particular tag and value, use `az resource list`:</span></span>

```azurecli
az resource list --tag Dept=Finance
```

<span data-ttu-id="8cb97-120">Pokaždé, když použijete značky na prostředek nebo skupinu prostředků, přepíšete pro daný prostředek nebo skupinu prostředků existující značky.</span><span class="sxs-lookup"><span data-stu-id="8cb97-120">Every time you apply tags to a resource or a resource group, you overwrite the existing tags on that resource or resource group.</span></span> <span data-ttu-id="8cb97-121">Proto je nutné použít jiný přístup na základě toho, jestli prostředek nebo skupina prostředků má existující značky.</span><span class="sxs-lookup"><span data-stu-id="8cb97-121">Therefore, you must use a different approach based on whether the resource or resource group has existing tags.</span></span> 

<span data-ttu-id="8cb97-122">Pokud chcete přidat značky ke *skupině prostředků bez existujících značek*, použijte:</span><span class="sxs-lookup"><span data-stu-id="8cb97-122">To add tags to a *resource group without existing tags*, use:</span></span>

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

<span data-ttu-id="8cb97-123">Pokud chcete přidat značky k *prostředku bez existujících značek*, použijte:</span><span class="sxs-lookup"><span data-stu-id="8cb97-123">To add tags to a *resource without existing tags*, use:</span></span>

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
``` 

<span data-ttu-id="8cb97-124">Přidání značek k prostředku, který již má značky, načíst existující značky, přeformátujte tuto hodnotu a znovu použít stávající a nové značky:</span><span class="sxs-lookup"><span data-stu-id="8cb97-124">To add tags to a resource that already has tags, retrieve the existing tags, reformat that value, and reapply the existing and new tags:</span></span> 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

<span data-ttu-id="8cb97-125">Pokud chcete použít všechny značky ze skupiny prostředků na prostředky a *nezachovat existující značky u prostředků*, použijte tento skript:</span><span class="sxs-lookup"><span data-stu-id="8cb97-125">To apply all tags from a resource group to its resources, and *not retain existing tags on the resources*, use the following script:</span></span>

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    az resource tag --tags $t --id $resid
  done 
done
```

<span data-ttu-id="8cb97-126">Použít všechny značky ze skupiny zdrojů na jeho prostředky, a *zachovat stávající značky na prostředcích*, použijte tento skript:</span><span class="sxs-lookup"><span data-stu-id="8cb97-126">To apply all tags from a resource group to its resources, and *retain existing tags on resources*, use the following script:</span></span>

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups 
do 
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv) 
  for resid in $r 
  do 
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done 
done
```


## <a name="templates"></a><span data-ttu-id="8cb97-127">Šablony</span><span class="sxs-lookup"><span data-stu-id="8cb97-127">Templates</span></span>

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a><span data-ttu-id="8cb97-128">Portál</span><span class="sxs-lookup"><span data-stu-id="8cb97-128">Portal</span></span>
[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]


## <a name="rest-api"></a><span data-ttu-id="8cb97-129">REST API</span><span class="sxs-lookup"><span data-stu-id="8cb97-129">REST API</span></span>
<span data-ttu-id="8cb97-130">Portál Azure a prostředí PowerShell, jak používat [REST API Resource Manageru](https://docs.microsoft.com/rest/api/resources/) na pozadí.</span><span class="sxs-lookup"><span data-stu-id="8cb97-130">The Azure portal and PowerShell both use the [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) behind the scenes.</span></span> <span data-ttu-id="8cb97-131">Pokud potřebujete integrovat označování do jiného prostředí, můžete získat značky pomocí **získat** na ID prostředku a aktualizace sadu značky pomocí **oprava** volání.</span><span class="sxs-lookup"><span data-stu-id="8cb97-131">If you need to integrate tagging into another environment, you can get tags by using **GET** on the resource ID and update the set of tags by using a **PATCH** call.</span></span>

## <a name="tags-and-billing"></a><span data-ttu-id="8cb97-132">Značky a fakturace</span><span class="sxs-lookup"><span data-stu-id="8cb97-132">Tags and billing</span></span>
<span data-ttu-id="8cb97-133">Značky můžete použít k seskupení fakturační údaje.</span><span class="sxs-lookup"><span data-stu-id="8cb97-133">You can use tags to group your billing data.</span></span> <span data-ttu-id="8cb97-134">Například pokud používáte víc virtuálních počítačů pro jiné organizace, použití značek k použití skupiny podle nákladové středisko.</span><span class="sxs-lookup"><span data-stu-id="8cb97-134">For example, if you are running multiple VMs for different organizations, use the tags to group usage by cost center.</span></span> <span data-ttu-id="8cb97-135">Značky lze použít také ke kategorizaci náklady prostředí runtime, jako je fakturace využití pro virtuální počítače spuštěné v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8cb97-135">You can also use tags to categorize costs by runtime environment, such as the billing usage for VMs running in the production environment.</span></span>


<span data-ttu-id="8cb97-136">Můžete načíst informace o značkách prostřednictvím [využití prostředků Azure a rozhraní API RateCard](../billing/billing-usage-rate-card-overview.md) nebo využití soubor hodnot oddělených čárkami (CSV).</span><span class="sxs-lookup"><span data-stu-id="8cb97-136">You can retrieve information about tags through the [Azure Resource Usage and RateCard APIs](../billing/billing-usage-rate-card-overview.md) or the usage comma-separated values (CSV) file.</span></span> <span data-ttu-id="8cb97-137">Stáhnout soubor využití z [účet Azure portal](https://account.windowsazure.com/) nebo [EA portál](https://ea.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8cb97-137">You download the usage file from the [Azure account portal](https://account.windowsazure.com/) or [EA portal](https://ea.azure.com).</span></span> <span data-ttu-id="8cb97-138">Další informace o programový přístup k fakturační informace najdete v tématu [proniknout do vaší spotřeby prostředků Microsoft Azure](../billing/billing-usage-rate-card-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8cb97-138">For more information about programmatic access to billing information, see [Gain insights into your Microsoft Azure resource consumption](../billing/billing-usage-rate-card-overview.md).</span></span> <span data-ttu-id="8cb97-139">Operace REST API, najdete v části [referenční dokumentace rozhraní API Azure fakturace REST](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).</span><span class="sxs-lookup"><span data-stu-id="8cb97-139">For REST API operations, see [Azure Billing REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).</span></span>


<span data-ttu-id="8cb97-140">Když si stáhnete použití sdíleného svazku clusteru pro služby, které podporují značky s fakturace, značky se zobrazí v **značky** sloupce.</span><span class="sxs-lookup"><span data-stu-id="8cb97-140">When you download the usage CSV for services that support tags with billing, the tags appear in the **Tags** column.</span></span> <span data-ttu-id="8cb97-141">Další informace najdete v tématu [porozumět vaší faktuře pro Microsoft Azure](../billing/billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="8cb97-141">For more information, see [Understand your bill for Microsoft Azure](../billing/billing-understand-your-bill.md).</span></span>

![Najdete v části značky k fakturaci](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a><span data-ttu-id="8cb97-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8cb97-143">Next steps</span></span>
* <span data-ttu-id="8cb97-144">Pomocí vlastních zásad, můžete použít omezení a pravidla týkající se vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="8cb97-144">You can apply restrictions and conventions across your subscription by using customized policies.</span></span> <span data-ttu-id="8cb97-145">Zásady, které definujete může vyžadovat, že všechny prostředky obsahovat hodnotu pro konkrétní značku.</span><span class="sxs-lookup"><span data-stu-id="8cb97-145">A policy that you define might require that all resources have a value for a particular tag.</span></span> <span data-ttu-id="8cb97-146">Další informace najdete v tématu [použití zásad ke správě prostředků a řízení přístupu](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="8cb97-146">For more information, see [Use policies to manage resources and control access](resource-manager-policy.md).</span></span>
* <span data-ttu-id="8cb97-147">Úvod do pomocí Azure PowerShell, pokud nasazujete prostředky, najdete v části [použití Azure Powershellu s Azure Resource Manager](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="8cb97-147">For an introduction to using Azure PowerShell when you're deploying resources, see [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="8cb97-148">Úvod do používání rozhraní příkazového řádku Azure, pokud nasazujete prostředky, najdete v části [pomocí rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="8cb97-148">For an introduction to using the Azure CLI when you're deploying resources, see [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>
* <span data-ttu-id="8cb97-149">Úvod do portálu, najdete v části [použití portálu Azure ke správě prostředků Azure](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8cb97-149">For an introduction to using the portal, see [Using the Azure portal to manage your Azure resources](resource-group-portal.md).</span></span>  
* <span data-ttu-id="8cb97-150">Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="8cb97-150">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

