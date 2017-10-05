---
title: "Poskytovatelé prostředků Azure a typy prostředků | Microsoft Docs"
description: "Popisuje zprostředkovatele prostředků, které podporují Resource Manager, jejich schémata a dostupné verze rozhraní API a oblastí, které může být hostitelem prostředky."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 6a9128f45d4199404019cee594842d59c7f1aaf3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="resource-providers-and-types"></a><span data-ttu-id="471bd-103">Poskytovatelé prostředků a typy</span><span class="sxs-lookup"><span data-stu-id="471bd-103">Resource providers and types</span></span>

<span data-ttu-id="471bd-104">Při nasazení prostředků, můžete často potřebují k načtení informací o zprostředkovatelé prostředků a typy.</span><span class="sxs-lookup"><span data-stu-id="471bd-104">When deploying resources, you frequently need to retrieve information about the resource providers and types.</span></span> <span data-ttu-id="471bd-105">V tomto článku se dozvíte na:</span><span class="sxs-lookup"><span data-stu-id="471bd-105">In this article, you learn to:</span></span>

* <span data-ttu-id="471bd-106">Zobrazení všech poskytovatelů prostředků v Azure</span><span class="sxs-lookup"><span data-stu-id="471bd-106">View all resource providers in Azure</span></span>
* <span data-ttu-id="471bd-107">Zkontrolovat stav registrace poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="471bd-107">Check registration status of a resource provider</span></span>
* <span data-ttu-id="471bd-108">Registrace poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="471bd-108">Register a resource provider</span></span>
* <span data-ttu-id="471bd-109">Zobrazit typy prostředků pro poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="471bd-109">View resource types for a resource provider</span></span>
* <span data-ttu-id="471bd-110">Zobrazení platná umístění pro typ prostředku</span><span class="sxs-lookup"><span data-stu-id="471bd-110">View valid locations for a resource type</span></span>
* <span data-ttu-id="471bd-111">Zobrazit platná verze rozhraní API pro typ prostředku</span><span class="sxs-lookup"><span data-stu-id="471bd-111">View valid API versions for a resource type</span></span>

<span data-ttu-id="471bd-112">Abyste mohli provést tyto kroky prostřednictvím portálu, prostředí PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="471bd-112">You can perform these steps through the portal, PowerShell, or Azure CLI.</span></span>

## <a name="powershell"></a><span data-ttu-id="471bd-113">PowerShell</span><span class="sxs-lookup"><span data-stu-id="471bd-113">PowerShell</span></span>

<span data-ttu-id="471bd-114">Pokud chcete zobrazit všech poskytovatelů prostředků v Azure a stav registrace pro vaše předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="471bd-114">To see all resource providers in Azure, and the registration status for your subscription, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

<span data-ttu-id="471bd-115">Které vrátí výsledky podobné:</span><span class="sxs-lookup"><span data-stu-id="471bd-115">Which returns results similar to:</span></span>

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="471bd-116">Registruje se poskytovatel prostředků nakonfiguruje vaše předplatné pro práci s poskytovatelem prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-116">Registering a resource provider configures your subscription to work with the resource provider.</span></span> <span data-ttu-id="471bd-117">Rozsah pro registraci je vždy předplatné.</span><span class="sxs-lookup"><span data-stu-id="471bd-117">The scope for registration is always the subscription.</span></span> <span data-ttu-id="471bd-118">Ve výchozím nastavení se automaticky registruje mnoho poskytovatelů prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-118">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="471bd-119">Můžete však ručně zaregistrovat někteří poskytovatelé prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-119">However, you may need to manually register some resource providers.</span></span> <span data-ttu-id="471bd-120">Registrace poskytovatele prostředků, musíte mít oprávnění k provedení `/register/action` operace pro poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-120">To register a resource provider, you must have permission to perform the `/register/action` operation for the resource provider.</span></span> <span data-ttu-id="471bd-121">Tato operace je součástí role Přispěvatel a vlastník.</span><span class="sxs-lookup"><span data-stu-id="471bd-121">This operation is included in the Contributor and Owner roles.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="471bd-122">Které vrátí výsledky podobné:</span><span class="sxs-lookup"><span data-stu-id="471bd-122">Which returns results similar to:</span></span>

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

<span data-ttu-id="471bd-123">Pokud máte pořád typy prostředků z tohoto zprostředkovatele prostředků v rámci vašeho předplatného, nelze zrušit registraci poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-123">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="471bd-124">Pokud chcete zobrazit informace pro určitý prostředek zprostředkovatele, použijte:</span><span class="sxs-lookup"><span data-stu-id="471bd-124">To see information for a particular resource provider, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="471bd-125">Které vrátí výsledky podobné:</span><span class="sxs-lookup"><span data-stu-id="471bd-125">Which returns results similar to:</span></span>

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

<span data-ttu-id="471bd-126">Chcete-li zobrazit typy prostředků pro poskytovatele prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="471bd-126">To see the resource types for a resource provider, use:</span></span>

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

<span data-ttu-id="471bd-127">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="471bd-127">Which returns:</span></span>

```powershell
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="471bd-128">Verze rozhraní API odpovídá verzi operace REST API, které vydávají poskytovatelem prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-128">The API version corresponds to a version of REST API operations that are released by the resource provider.</span></span> <span data-ttu-id="471bd-129">Zprostředkovatel prostředků umožňuje nové funkce, uvolní novou verzi rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="471bd-129">As a resource provider enables new features, it releases a new version of the REST API.</span></span> 

<span data-ttu-id="471bd-130">K dispozici verze rozhraní API pro typ prostředku, použijte:</span><span class="sxs-lookup"><span data-stu-id="471bd-130">To get the available API versions for a resource type, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

<span data-ttu-id="471bd-131">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="471bd-131">Which returns:</span></span>

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="471bd-132">Správce prostředků se podporuje ve všech oblastech, ale nemusí být podporován prostředky, které můžete nasadit ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="471bd-132">Resource Manager is supported in all regions, but the resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="471bd-133">Kromě toho může být omezení na vaše předplatné, které zabránit vám v použití některé oblasti, které podporují prostředku.</span><span class="sxs-lookup"><span data-stu-id="471bd-133">In addition, there may be limitations on your subscription that prevent you from using some regions that support the resource.</span></span> 

<span data-ttu-id="471bd-134">Podporovaná umístění pro typ prostředku, použijte.</span><span class="sxs-lookup"><span data-stu-id="471bd-134">To get the supported locations for a resource type, use.</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

<span data-ttu-id="471bd-135">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="471bd-135">Which returns:</span></span>

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a><span data-ttu-id="471bd-136">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="471bd-136">Azure CLI</span></span>
<span data-ttu-id="471bd-137">Pokud chcete zobrazit všech poskytovatelů prostředků v Azure a stav registrace pro vaše předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="471bd-137">To see all resource providers in Azure, and the registration status for your subscription, use:</span></span>

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

<span data-ttu-id="471bd-138">Které vrátí výsledky podobné:</span><span class="sxs-lookup"><span data-stu-id="471bd-138">Which returns results similar to:</span></span>

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="471bd-139">Registruje se poskytovatel prostředků nakonfiguruje vaše předplatné pro práci s poskytovatelem prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-139">Registering a resource provider configures your subscription to work with the resource provider.</span></span> <span data-ttu-id="471bd-140">Rozsah pro registraci je vždy předplatné.</span><span class="sxs-lookup"><span data-stu-id="471bd-140">The scope for registration is always the subscription.</span></span> <span data-ttu-id="471bd-141">Ve výchozím nastavení se automaticky registruje mnoho poskytovatelů prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-141">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="471bd-142">Můžete však ručně zaregistrovat někteří poskytovatelé prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-142">However, you may need to manually register some resource providers.</span></span> <span data-ttu-id="471bd-143">Registrace poskytovatele prostředků, musíte mít oprávnění k provedení `/register/action` operace pro poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-143">To register a resource provider, you must have permission to perform the `/register/action` operation for the resource provider.</span></span> <span data-ttu-id="471bd-144">Tato operace je součástí role Přispěvatel a vlastník.</span><span class="sxs-lookup"><span data-stu-id="471bd-144">This operation is included in the Contributor and Owner roles.</span></span>

```azurecli
az provider register --namespace Microsoft.Batch
```

<span data-ttu-id="471bd-145">Které vrátí zprávu, že registrace se průběžné.</span><span class="sxs-lookup"><span data-stu-id="471bd-145">Which returns a message that registration is on-going.</span></span>

<span data-ttu-id="471bd-146">Pokud máte pořád typy prostředků z tohoto zprostředkovatele prostředků v rámci vašeho předplatného, nelze zrušit registraci poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-146">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="471bd-147">Pokud chcete zobrazit informace pro určitý prostředek zprostředkovatele, použijte:</span><span class="sxs-lookup"><span data-stu-id="471bd-147">To see information for a particular resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch
```

<span data-ttu-id="471bd-148">Které vrátí výsledky podobné:</span><span class="sxs-lookup"><span data-stu-id="471bd-148">Which returns results similar to:</span></span>

```azurecli
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

<span data-ttu-id="471bd-149">Chcete-li zobrazit typy prostředků pro poskytovatele prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="471bd-149">To see the resource types for a resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

<span data-ttu-id="471bd-150">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="471bd-150">Which returns:</span></span>

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="471bd-151">Verze rozhraní API odpovídá verzi operace REST API, které vydávají poskytovatelem prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-151">The API version corresponds to a version of REST API operations that are released by the resource provider.</span></span> <span data-ttu-id="471bd-152">Zprostředkovatel prostředků umožňuje nové funkce, uvolní novou verzi rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="471bd-152">As a resource provider enables new features, it releases a new version of the REST API.</span></span> 

<span data-ttu-id="471bd-153">K dispozici verze rozhraní API pro typ prostředku, použijte:</span><span class="sxs-lookup"><span data-stu-id="471bd-153">To get the available API versions for a resource type, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

<span data-ttu-id="471bd-154">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="471bd-154">Which returns:</span></span>

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="471bd-155">Správce prostředků se podporuje ve všech oblastech, ale nemusí být podporován prostředky, které můžete nasadit ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="471bd-155">Resource Manager is supported in all regions, but the resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="471bd-156">Kromě toho může být omezení na vaše předplatné, které zabránit vám v použití některé oblasti, které podporují prostředku.</span><span class="sxs-lookup"><span data-stu-id="471bd-156">In addition, there may be limitations on your subscription that prevent you from using some regions that support the resource.</span></span> 

<span data-ttu-id="471bd-157">Podporovaná umístění pro typ prostředku, použijte.</span><span class="sxs-lookup"><span data-stu-id="471bd-157">To get the supported locations for a resource type, use.</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

<span data-ttu-id="471bd-158">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="471bd-158">Which returns:</span></span>

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a><span data-ttu-id="471bd-159">Portál</span><span class="sxs-lookup"><span data-stu-id="471bd-159">Portal</span></span>

<span data-ttu-id="471bd-160">Pokud chcete zobrazit všech poskytovatelů prostředků v Azure a stav registrace pro vaše předplatné, vyberte **odběry**.</span><span class="sxs-lookup"><span data-stu-id="471bd-160">To see all resource providers in Azure, and the registration status for your subscription, select **Subscriptions**.</span></span>

![Vyberte předplatná](./media/resource-manager-supported-services/select-subscriptions.png)

<span data-ttu-id="471bd-162">Zvolte předplatné, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="471bd-162">Choose the subscription to view.</span></span>

![Zadejte předplatné](./media/resource-manager-supported-services/subscription.png)

<span data-ttu-id="471bd-164">Vyberte **zprostředkovatelé prostředků** a zobrazit seznam dostupných poskytovatelů prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-164">Select **Resource providers** and view the list of available resource providers.</span></span>

![Zobrazit zprostředkovatelé prostředků](./media/resource-manager-supported-services/show-resource-providers.png)

<span data-ttu-id="471bd-166">Registruje se poskytovatel prostředků nakonfiguruje vaše předplatné pro práci s poskytovatelem prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-166">Registering a resource provider configures your subscription to work with the resource provider.</span></span> <span data-ttu-id="471bd-167">Rozsah pro registraci je vždy předplatné.</span><span class="sxs-lookup"><span data-stu-id="471bd-167">The scope for registration is always the subscription.</span></span> <span data-ttu-id="471bd-168">Ve výchozím nastavení se automaticky registruje mnoho poskytovatelů prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-168">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="471bd-169">Můžete však ručně zaregistrovat někteří poskytovatelé prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-169">However, you may need to manually register some resource providers.</span></span> <span data-ttu-id="471bd-170">Registrace poskytovatele prostředků, musíte mít oprávnění k provedení `/register/action` operace pro poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-170">To register a resource provider, you must have permission to perform the `/register/action` operation for the resource provider.</span></span> <span data-ttu-id="471bd-171">Tato operace je součástí role Přispěvatel a vlastník.</span><span class="sxs-lookup"><span data-stu-id="471bd-171">This operation is included in the Contributor and Owner roles.</span></span> <span data-ttu-id="471bd-172">Registrace poskytovatele prostředků, vyberte **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="471bd-172">To register a resource provider, select **Register**.</span></span>

![registrace poskytovatele prostředků](./media/resource-manager-supported-services/register-provider.png)

<span data-ttu-id="471bd-174">Pokud máte pořád typy prostředků z tohoto zprostředkovatele prostředků v rámci vašeho předplatného, nelze zrušit registraci poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-174">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="471bd-175">Chcete-li zobrazit informace pro určitý prostředek poskytovatele, vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="471bd-175">To see information for a particular resource provider, select **More services**.</span></span>

![Vyberte další služby](./media/resource-manager-supported-services/more-services.png)

<span data-ttu-id="471bd-177">Vyhledejte **Průzkumníka prostředků** a vyberte z dostupných možností.</span><span class="sxs-lookup"><span data-stu-id="471bd-177">Search for **Resource Explorer** and select it from the available options.</span></span>

![Vyberte Průzkumník prostředků](./media/resource-manager-supported-services/select-resource-explorer.png)

<span data-ttu-id="471bd-179">Vyberte **zprostředkovatelé**.</span><span class="sxs-lookup"><span data-stu-id="471bd-179">Select **Providers**.</span></span>

![Vyberte zprostředkovatele](./media/resource-manager-supported-services/select-providers.png)

<span data-ttu-id="471bd-181">Vyberte zprostředkovatele prostředků a typ prostředku, který chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="471bd-181">Select the resource provider and resource type that you want to view.</span></span>

![Vyberte typ prostředku](./media/resource-manager-supported-services/select-resource-type.png)

<span data-ttu-id="471bd-183">Správce prostředků se podporuje ve všech oblastech, ale nemusí být podporován prostředky, které můžete nasadit ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="471bd-183">Resource Manager is supported in all regions, but the resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="471bd-184">Kromě toho může být omezení na vaše předplatné, které zabránit vám v použití některé oblasti, které podporují prostředku.</span><span class="sxs-lookup"><span data-stu-id="471bd-184">In addition, there may be limitations on your subscription that prevent you from using some regions that support the resource.</span></span> <span data-ttu-id="471bd-185">Průzkumník prostředků zobrazí platná umístění pro typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="471bd-185">The resource explorer displays valid locations for the resource type.</span></span>

![Zobrazit umístění](./media/resource-manager-supported-services/show-locations.png)

<span data-ttu-id="471bd-187">Verze rozhraní API odpovídá verzi operace REST API, které vydávají poskytovatelem prostředků.</span><span class="sxs-lookup"><span data-stu-id="471bd-187">The API version corresponds to a version of REST API operations that are released by the resource provider.</span></span> <span data-ttu-id="471bd-188">Zprostředkovatel prostředků umožňuje nové funkce, uvolní novou verzi rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="471bd-188">As a resource provider enables new features, it releases a new version of the REST API.</span></span> <span data-ttu-id="471bd-189">Průzkumník prostředků zobrazí platná verze rozhraní API pro typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="471bd-189">The resource explorer displays valid API versions for the resource type.</span></span>

![Zobrazit verze rozhraní API](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a><span data-ttu-id="471bd-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="471bd-191">Next steps</span></span>
* <span data-ttu-id="471bd-192">Další informace o vytváření šablon Resource Manageru, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="471bd-192">To learn about creating Resource Manager templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="471bd-193">Další informace o nasazení prostředků najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="471bd-193">To learn about deploying resources, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="471bd-194">Operace pro poskytovatele prostředků najdete v tématu [rozhraní REST API Azure](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="471bd-194">To view the operations for a resource provider, see [Azure REST API](/rest/api/).</span></span>

