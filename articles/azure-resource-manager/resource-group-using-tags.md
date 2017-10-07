---
title: "aaaTag Azure prostředky pro logické organizace | Microsoft Docs"
description: "Ukazuje, jak tooapply značky tooorganize Azure prostředky pro fakturace a správy."
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
ms.openlocfilehash: e07470463d160f8cefe5c80bc91e66a96af6ca45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-tags-tooorganize-your-azure-resources"></a><span data-ttu-id="6a4a8-103">Pomocí značky tooorganize vašich prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="6a4a8-103">Use tags tooorganize your Azure resources</span></span>
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

> [!NOTE]
> <span data-ttu-id="6a4a8-104">Můžete použít pouze tooresources značky, které podporují operace Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-104">You can apply tags only tooresources that support Azure Resource Manager operations.</span></span> <span data-ttu-id="6a4a8-105">Pokud jste vytvořili virtuální počítač, virtuální sítě nebo účet úložiště pomocí modelu nasazení classic hello (hello například jako prostřednictvím portálu Azure classic), nemůžete použít prostředek toothat značky.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-105">If you created a virtual machine, virtual network, or storage account through hello classic deployment model (such as through hello Azure classic portal), you cannot apply a tag toothat resource.</span></span> <span data-ttu-id="6a4a8-106">toosupport označování, znovu nasadit tyto prostředky prostřednictvím Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-106">toosupport tagging, redeploy these resources through Resource Manager.</span></span> <span data-ttu-id="6a4a8-107">Všechny ostatní prostředky podporují označování.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-107">All other resources support tagging.</span></span>
> 
> 

## <a name="policies-for-tag-consistency"></a><span data-ttu-id="6a4a8-108">Zásady pro značka konzistence</span><span class="sxs-lookup"><span data-stu-id="6a4a8-108">Policies for tag consistency</span></span>

<span data-ttu-id="6a4a8-109">Prostředek zásady toocreate standardní pravidla můžete použít pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-109">You can use resource policies toocreate standard rules for your organization.</span></span> <span data-ttu-id="6a4a8-110">Můžete vytvořit zásady, které zajišťují, že prostředky jsou označené hello příslušné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-110">You can create policies that ensure resources are tagged with hello appropriate values.</span></span> <span data-ttu-id="6a4a8-111">Další informace najdete v tématu [zásad prostředků pro značky](resource-manager-policy-tags.md).</span><span class="sxs-lookup"><span data-stu-id="6a4a8-111">For more information, see [Apply resource policies for tags](resource-manager-policy-tags.md).</span></span>

## <a name="powershell"></a><span data-ttu-id="6a4a8-112">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a4a8-112">PowerShell</span></span>
[!INCLUDE [resource-manager-tag-resources-powershell](../../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a><span data-ttu-id="6a4a8-113">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6a4a8-113">Azure CLI</span></span>

<span data-ttu-id="6a4a8-114">toosee hello stávající značky pro *skupiny prostředků*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6a4a8-114">toosee hello existing tags for a *resource group*, use:</span></span>

```azurecli
az group show -n examplegroup --query tags
```

<span data-ttu-id="6a4a8-115">Tento skript vrátí hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="6a4a8-115">That script returns hello following format:</span></span>

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

<span data-ttu-id="6a4a8-116">toosee hello stávající značky pro *prostředek, který má zadaný prostředek ID*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6a4a8-116">toosee hello existing tags for a *resource that has a specified resource ID*, use:</span></span>

```azurecli
az resource show --id {resource-id} --query tags
```

<span data-ttu-id="6a4a8-117">Nebo toosee hello stávající značky pro *prostředek, který má zadaný název, typ a prostředků skupinu*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6a4a8-117">Or, toosee hello existing tags for a *resource that has a specified name, type, and resource group*, use:</span></span>

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

<span data-ttu-id="6a4a8-118">použití skupin prostředků tooget, které mají s konkrétní značkou tag `az group list`:</span><span class="sxs-lookup"><span data-stu-id="6a4a8-118">tooget resource groups that have a specific tag, use `az group list`:</span></span>

```azurecli
az group list --tag Dept=IT
```

<span data-ttu-id="6a4a8-119">tooget všechny hello prostředky, které mají konkrétní značku a hodnota, používají `az resource list`:</span><span class="sxs-lookup"><span data-stu-id="6a4a8-119">tooget all hello resources that have a particular tag and value, use `az resource list`:</span></span>

```azurecli
az resource list --tag Dept=Finance
```

<span data-ttu-id="6a4a8-120">Pokaždé, když použijete značky tooa prostředek nebo skupina zdrojů, můžete přepsat hello existující značek pro daný prostředek nebo skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-120">Every time you apply tags tooa resource or a resource group, you overwrite hello existing tags on that resource or resource group.</span></span> <span data-ttu-id="6a4a8-121">Proto je nutné použít jiný přístup na základě toho, jestli hello prostředek nebo skupina prostředků má stávající značky.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-121">Therefore, you must use a different approach based on whether hello resource or resource group has existing tags.</span></span> 

<span data-ttu-id="6a4a8-122">tooadd značky tooa *skupiny prostředků bez existující značky*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6a4a8-122">tooadd tags tooa *resource group without existing tags*, use:</span></span>

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

<span data-ttu-id="6a4a8-123">tooadd značky tooa *prostředku bez existující značky*, použijte:</span><span class="sxs-lookup"><span data-stu-id="6a4a8-123">tooadd tags tooa *resource without existing tags*, use:</span></span>

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
``` 

<span data-ttu-id="6a4a8-124">tooadd značky tooa prostředek, který již má značky, načtení hello existující značky, přeformátujte tuto hodnotu a znovu zásadu použijte hello stávající a nové značky:</span><span class="sxs-lookup"><span data-stu-id="6a4a8-124">tooadd tags tooa resource that already has tags, retrieve hello existing tags, reformat that value, and reapply hello existing and new tags:</span></span> 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

<span data-ttu-id="6a4a8-125">tooapply všechny značky ze zdroje tooits skupiny prostředků, a *není zachována stávající značky na prostředcích hello*, použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="6a4a8-125">tooapply all tags from a resource group tooits resources, and *not retain existing tags on hello resources*, use hello following script:</span></span>

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

<span data-ttu-id="6a4a8-126">tooapply všechny značky ze zdroje tooits skupiny prostředků, a *zachovat stávající značky na prostředcích*, použijte hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="6a4a8-126">tooapply all tags from a resource group tooits resources, and *retain existing tags on resources*, use hello following script:</span></span>

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


## <a name="templates"></a><span data-ttu-id="6a4a8-127">Šablony</span><span class="sxs-lookup"><span data-stu-id="6a4a8-127">Templates</span></span>

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a><span data-ttu-id="6a4a8-128">Portál</span><span class="sxs-lookup"><span data-stu-id="6a4a8-128">Portal</span></span>
[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]


## <a name="rest-api"></a><span data-ttu-id="6a4a8-129">REST API</span><span class="sxs-lookup"><span data-stu-id="6a4a8-129">REST API</span></span>
<span data-ttu-id="6a4a8-130">Hello portál Azure a prostředí PowerShell použít hello [REST API Resource Manageru](https://docs.microsoft.com/rest/api/resources/) pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-130">hello Azure portal and PowerShell both use hello [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) behind hello scenes.</span></span> <span data-ttu-id="6a4a8-131">Pokud potřebujete toointegrate označování do jiného prostředí, můžete získat značky pomocí **získat** hello prostředků ID a aktualizace hello sadu pomocí značek **oprava** volání.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-131">If you need toointegrate tagging into another environment, you can get tags by using **GET** on hello resource ID and update hello set of tags by using a **PATCH** call.</span></span>

## <a name="tags-and-billing"></a><span data-ttu-id="6a4a8-132">Značky a fakturace</span><span class="sxs-lookup"><span data-stu-id="6a4a8-132">Tags and billing</span></span>
<span data-ttu-id="6a4a8-133">Můžete vytvořit značky toogroup fakturační údaje.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-133">You can use tags toogroup your billing data.</span></span> <span data-ttu-id="6a4a8-134">Například pokud používáte víc virtuálních počítačů pro jiné organizace, použijte hello značky toogroup využití podle nákladové středisko.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-134">For example, if you are running multiple VMs for different organizations, use hello tags toogroup usage by cost center.</span></span> <span data-ttu-id="6a4a8-135">Můžete taky náklady toocategorize značky prostředí runtime, jako je například hello fakturace využití pro virtuální počítače spuštěné v provozním prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-135">You can also use tags toocategorize costs by runtime environment, such as hello billing usage for VMs running in hello production environment.</span></span>


<span data-ttu-id="6a4a8-136">Můžete načíst informace o značkách prostřednictvím hello [využití prostředků Azure a rozhraní API RateCard](../billing/billing-usage-rate-card-overview.md) nebo soubor hodnot oddělených čárkami (CSV) využití hello.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-136">You can retrieve information about tags through hello [Azure Resource Usage and RateCard APIs](../billing/billing-usage-rate-card-overview.md) or hello usage comma-separated values (CSV) file.</span></span> <span data-ttu-id="6a4a8-137">Stáhnout soubor využití hello z hello [účet Azure portal](https://account.windowsazure.com/) nebo [EA portál](https://ea.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6a4a8-137">You download hello usage file from hello [Azure account portal](https://account.windowsazure.com/) or [EA portal](https://ea.azure.com).</span></span> <span data-ttu-id="6a4a8-138">Další informace o programový přístup toobilling informace najdete v tématu [proniknout do vaší spotřeby prostředků Microsoft Azure](../billing/billing-usage-rate-card-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a4a8-138">For more information about programmatic access toobilling information, see [Gain insights into your Microsoft Azure resource consumption](../billing/billing-usage-rate-card-overview.md).</span></span> <span data-ttu-id="6a4a8-139">Operace REST API, najdete v části [referenční dokumentace rozhraní API Azure fakturace REST](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).</span><span class="sxs-lookup"><span data-stu-id="6a4a8-139">For REST API operations, see [Azure Billing REST API Reference](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).</span></span>


<span data-ttu-id="6a4a8-140">Při stahování hello použití sdíleného svazku clusteru pro služby, které podporují značky s fakturace hello značky se zobrazují v hello **značky** sloupce.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-140">When you download hello usage CSV for services that support tags with billing, hello tags appear in hello **Tags** column.</span></span> <span data-ttu-id="6a4a8-141">Další informace najdete v tématu [porozumět vaší faktuře pro Microsoft Azure](../billing/billing-understand-your-bill.md).</span><span class="sxs-lookup"><span data-stu-id="6a4a8-141">For more information, see [Understand your bill for Microsoft Azure](../billing/billing-understand-your-bill.md).</span></span>

![Najdete v části značky k fakturaci](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a><span data-ttu-id="6a4a8-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a4a8-143">Next steps</span></span>
* <span data-ttu-id="6a4a8-144">Pomocí vlastních zásad, můžete použít omezení a pravidla týkající se vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-144">You can apply restrictions and conventions across your subscription by using customized policies.</span></span> <span data-ttu-id="6a4a8-145">Zásady, které definujete může vyžadovat, že všechny prostředky obsahovat hodnotu pro konkrétní značku.</span><span class="sxs-lookup"><span data-stu-id="6a4a8-145">A policy that you define might require that all resources have a value for a particular tag.</span></span> <span data-ttu-id="6a4a8-146">Další informace najdete v tématu [používají zásady toomanage prostředky a řízení přístupu](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="6a4a8-146">For more information, see [Use policies toomanage resources and control access](resource-manager-policy.md).</span></span>
* <span data-ttu-id="6a4a8-147">Úvod toousing prostředí Azure PowerShell Pokud nasazujete prostředky, najdete v části [použití Azure Powershellu s Azure Resource Manager](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="6a4a8-147">For an introduction toousing Azure PowerShell when you're deploying resources, see [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="6a4a8-148">Úvod toousing hello rozhraní příkazového řádku Azure Pokud nasazujete prostředky, najdete v části [hello pomocí Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="6a4a8-148">For an introduction toousing hello Azure CLI when you're deploying resources, see [Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>
* <span data-ttu-id="6a4a8-149">Úvod toousing hello portálu, najdete v části [pomocí hello Azure portálu toomanage vašich prostředků Azure](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6a4a8-149">For an introduction toousing hello portal, see [Using hello Azure portal toomanage your Azure resources](resource-group-portal.md).</span></span>  
* <span data-ttu-id="6a4a8-150">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="6a4a8-150">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

