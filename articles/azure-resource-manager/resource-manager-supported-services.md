---
title: "aaaAzure zprostředkovatelé prostředků a typy prostředků | Microsoft Docs"
description: "Popisuje zprostředkovatele hello prostředků, které podporují Resource Manager, jejich schémat a k dispozici verze rozhraní API a hello oblasti, které může být hostitelem hello prostředky."
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
ms.openlocfilehash: 23db1d3808a20166f3b44ec801e1bcc46fbb9bd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resource-providers-and-types"></a><span data-ttu-id="64ea1-103">Poskytovatelé prostředků a typy</span><span class="sxs-lookup"><span data-stu-id="64ea1-103">Resource providers and types</span></span>

<span data-ttu-id="64ea1-104">Pokud nasazujete prostředky, musíte často tooretrieve informace o poskytovatelích prostředků hello a typy.</span><span class="sxs-lookup"><span data-stu-id="64ea1-104">When deploying resources, you frequently need tooretrieve information about hello resource providers and types.</span></span> <span data-ttu-id="64ea1-105">V tomto článku se dozvíte na:</span><span class="sxs-lookup"><span data-stu-id="64ea1-105">In this article, you learn to:</span></span>

* <span data-ttu-id="64ea1-106">Zobrazení všech poskytovatelů prostředků v Azure</span><span class="sxs-lookup"><span data-stu-id="64ea1-106">View all resource providers in Azure</span></span>
* <span data-ttu-id="64ea1-107">Zkontrolovat stav registrace poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="64ea1-107">Check registration status of a resource provider</span></span>
* <span data-ttu-id="64ea1-108">Registrace poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="64ea1-108">Register a resource provider</span></span>
* <span data-ttu-id="64ea1-109">Zobrazit typy prostředků pro poskytovatele prostředků</span><span class="sxs-lookup"><span data-stu-id="64ea1-109">View resource types for a resource provider</span></span>
* <span data-ttu-id="64ea1-110">Zobrazení platná umístění pro typ prostředku</span><span class="sxs-lookup"><span data-stu-id="64ea1-110">View valid locations for a resource type</span></span>
* <span data-ttu-id="64ea1-111">Zobrazit platná verze rozhraní API pro typ prostředku</span><span class="sxs-lookup"><span data-stu-id="64ea1-111">View valid API versions for a resource type</span></span>

<span data-ttu-id="64ea1-112">Abyste mohli provést tyto kroky prostřednictvím portálu hello, prostředí PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="64ea1-112">You can perform these steps through hello portal, PowerShell, or Azure CLI.</span></span>

## <a name="powershell"></a><span data-ttu-id="64ea1-113">PowerShell</span><span class="sxs-lookup"><span data-stu-id="64ea1-113">PowerShell</span></span>

<span data-ttu-id="64ea1-114">toosee všech poskytovatelů prostředků v Azure a hello stav registrace pro vaše předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="64ea1-114">toosee all resource providers in Azure, and hello registration status for your subscription, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

<span data-ttu-id="64ea1-115">Které vrátí výsledky podobné:</span><span class="sxs-lookup"><span data-stu-id="64ea1-115">Which returns results similar to:</span></span>

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="64ea1-116">Registruje se poskytovatel prostředků nakonfiguruje vaše předplatné toowork s poskytovatelem prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-116">Registering a resource provider configures your subscription toowork with hello resource provider.</span></span> <span data-ttu-id="64ea1-117">Hello oboru pro registraci je vždy hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="64ea1-117">hello scope for registration is always hello subscription.</span></span> <span data-ttu-id="64ea1-118">Ve výchozím nastavení se automaticky registruje mnoho poskytovatelů prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-118">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="64ea1-119">Ale musíte toomanually zaregistrovat někteří poskytovatelé prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-119">However, you may need toomanually register some resource providers.</span></span> <span data-ttu-id="64ea1-120">tooregister poskytovatele prostředků, musíte mít oprávnění tooperform hello `/register/action` operace pro poskytovatele prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-120">tooregister a resource provider, you must have permission tooperform hello `/register/action` operation for hello resource provider.</span></span> <span data-ttu-id="64ea1-121">Tato operace je součástí hello Přispěvatel a vlastníka role.</span><span class="sxs-lookup"><span data-stu-id="64ea1-121">This operation is included in hello Contributor and Owner roles.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="64ea1-122">Které vrátí výsledky podobné:</span><span class="sxs-lookup"><span data-stu-id="64ea1-122">Which returns results similar to:</span></span>

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

<span data-ttu-id="64ea1-123">Pokud máte pořád typy prostředků z tohoto zprostředkovatele prostředků v rámci vašeho předplatného, nelze zrušit registraci poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-123">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="64ea1-124">informace o toosee pro určitý prostředek zprostředkovatele, použijte:</span><span class="sxs-lookup"><span data-stu-id="64ea1-124">toosee information for a particular resource provider, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="64ea1-125">Které vrátí výsledky podobné:</span><span class="sxs-lookup"><span data-stu-id="64ea1-125">Which returns results similar to:</span></span>

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

<span data-ttu-id="64ea1-126">toosee hello typy prostředků pro poskytovatele prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="64ea1-126">toosee hello resource types for a resource provider, use:</span></span>

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

<span data-ttu-id="64ea1-127">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="64ea1-127">Which returns:</span></span>

```powershell
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="64ea1-128">verze rozhraní API Hello odpovídá verzi tooa operací REST API, které jsou vydané poskytovatelem prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-128">hello API version corresponds tooa version of REST API operations that are released by hello resource provider.</span></span> <span data-ttu-id="64ea1-129">Zprostředkovatel prostředků umožňuje nové funkce, uvolní novou verzi hello REST API.</span><span class="sxs-lookup"><span data-stu-id="64ea1-129">As a resource provider enables new features, it releases a new version of hello REST API.</span></span> 

<span data-ttu-id="64ea1-130">tooget hello k dispozici rozhraní API verze pro typ prostředku, použijte:</span><span class="sxs-lookup"><span data-stu-id="64ea1-130">tooget hello available API versions for a resource type, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

<span data-ttu-id="64ea1-131">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="64ea1-131">Which returns:</span></span>

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="64ea1-132">Správce prostředků se podporuje ve všech oblastech, ale nemusí být hello prostředky, které nasazujete podporován ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="64ea1-132">Resource Manager is supported in all regions, but hello resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="64ea1-133">Kromě toho může být omezení na vaše předplatné, které zabránit vám v použití některé oblasti, které podporují hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-133">In addition, there may be limitations on your subscription that prevent you from using some regions that support hello resource.</span></span> 

<span data-ttu-id="64ea1-134">umístění tooget hello podporované pro typ prostředku použít.</span><span class="sxs-lookup"><span data-stu-id="64ea1-134">tooget hello supported locations for a resource type, use.</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

<span data-ttu-id="64ea1-135">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="64ea1-135">Which returns:</span></span>

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a><span data-ttu-id="64ea1-136">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="64ea1-136">Azure CLI</span></span>
<span data-ttu-id="64ea1-137">toosee všech poskytovatelů prostředků v Azure a hello stav registrace pro vaše předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="64ea1-137">toosee all resource providers in Azure, and hello registration status for your subscription, use:</span></span>

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

<span data-ttu-id="64ea1-138">Které vrátí výsledky podobné:</span><span class="sxs-lookup"><span data-stu-id="64ea1-138">Which returns results similar to:</span></span>

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="64ea1-139">Registruje se poskytovatel prostředků nakonfiguruje vaše předplatné toowork s poskytovatelem prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-139">Registering a resource provider configures your subscription toowork with hello resource provider.</span></span> <span data-ttu-id="64ea1-140">Hello oboru pro registraci je vždy hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="64ea1-140">hello scope for registration is always hello subscription.</span></span> <span data-ttu-id="64ea1-141">Ve výchozím nastavení se automaticky registruje mnoho poskytovatelů prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-141">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="64ea1-142">Ale musíte toomanually zaregistrovat někteří poskytovatelé prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-142">However, you may need toomanually register some resource providers.</span></span> <span data-ttu-id="64ea1-143">tooregister poskytovatele prostředků, musíte mít oprávnění tooperform hello `/register/action` operace pro poskytovatele prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-143">tooregister a resource provider, you must have permission tooperform hello `/register/action` operation for hello resource provider.</span></span> <span data-ttu-id="64ea1-144">Tato operace je součástí hello Přispěvatel a vlastníka role.</span><span class="sxs-lookup"><span data-stu-id="64ea1-144">This operation is included in hello Contributor and Owner roles.</span></span>

```azurecli
az provider register --namespace Microsoft.Batch
```

<span data-ttu-id="64ea1-145">Které vrátí zprávu, že registrace se průběžné.</span><span class="sxs-lookup"><span data-stu-id="64ea1-145">Which returns a message that registration is on-going.</span></span>

<span data-ttu-id="64ea1-146">Pokud máte pořád typy prostředků z tohoto zprostředkovatele prostředků v rámci vašeho předplatného, nelze zrušit registraci poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-146">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="64ea1-147">informace o toosee pro určitý prostředek zprostředkovatele, použijte:</span><span class="sxs-lookup"><span data-stu-id="64ea1-147">toosee information for a particular resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch
```

<span data-ttu-id="64ea1-148">Které vrátí výsledky podobné:</span><span class="sxs-lookup"><span data-stu-id="64ea1-148">Which returns results similar to:</span></span>

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

<span data-ttu-id="64ea1-149">toosee hello typy prostředků pro poskytovatele prostředků, použijte:</span><span class="sxs-lookup"><span data-stu-id="64ea1-149">toosee hello resource types for a resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

<span data-ttu-id="64ea1-150">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="64ea1-150">Which returns:</span></span>

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="64ea1-151">verze rozhraní API Hello odpovídá verzi tooa operací REST API, které jsou vydané poskytovatelem prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-151">hello API version corresponds tooa version of REST API operations that are released by hello resource provider.</span></span> <span data-ttu-id="64ea1-152">Zprostředkovatel prostředků umožňuje nové funkce, uvolní novou verzi hello REST API.</span><span class="sxs-lookup"><span data-stu-id="64ea1-152">As a resource provider enables new features, it releases a new version of hello REST API.</span></span> 

<span data-ttu-id="64ea1-153">tooget hello k dispozici rozhraní API verze pro typ prostředku, použijte:</span><span class="sxs-lookup"><span data-stu-id="64ea1-153">tooget hello available API versions for a resource type, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

<span data-ttu-id="64ea1-154">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="64ea1-154">Which returns:</span></span>

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="64ea1-155">Správce prostředků se podporuje ve všech oblastech, ale nemusí být hello prostředky, které nasazujete podporován ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="64ea1-155">Resource Manager is supported in all regions, but hello resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="64ea1-156">Kromě toho může být omezení na vaše předplatné, které zabránit vám v použití některé oblasti, které podporují hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-156">In addition, there may be limitations on your subscription that prevent you from using some regions that support hello resource.</span></span> 

<span data-ttu-id="64ea1-157">umístění tooget hello podporované pro typ prostředku použít.</span><span class="sxs-lookup"><span data-stu-id="64ea1-157">tooget hello supported locations for a resource type, use.</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

<span data-ttu-id="64ea1-158">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="64ea1-158">Which returns:</span></span>

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a><span data-ttu-id="64ea1-159">Portál</span><span class="sxs-lookup"><span data-stu-id="64ea1-159">Portal</span></span>

<span data-ttu-id="64ea1-160">Vyberte všechny poskytovatele prostředků v Azure a hello stav registrace pro vaše předplatné toosee **odběry**.</span><span class="sxs-lookup"><span data-stu-id="64ea1-160">toosee all resource providers in Azure, and hello registration status for your subscription, select **Subscriptions**.</span></span>

![Vyberte předplatná](./media/resource-manager-supported-services/select-subscriptions.png)

<span data-ttu-id="64ea1-162">Zvolte předplatné tooview hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-162">Choose hello subscription tooview.</span></span>

![Zadejte předplatné](./media/resource-manager-supported-services/subscription.png)

<span data-ttu-id="64ea1-164">Vyberte **zprostředkovatelé prostředků** a hello zobrazit seznam dostupných poskytovatelů prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-164">Select **Resource providers** and view hello list of available resource providers.</span></span>

![Zobrazit zprostředkovatelé prostředků](./media/resource-manager-supported-services/show-resource-providers.png)

<span data-ttu-id="64ea1-166">Registruje se poskytovatel prostředků nakonfiguruje vaše předplatné toowork s poskytovatelem prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-166">Registering a resource provider configures your subscription toowork with hello resource provider.</span></span> <span data-ttu-id="64ea1-167">Hello oboru pro registraci je vždy hello předplatného.</span><span class="sxs-lookup"><span data-stu-id="64ea1-167">hello scope for registration is always hello subscription.</span></span> <span data-ttu-id="64ea1-168">Ve výchozím nastavení se automaticky registruje mnoho poskytovatelů prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-168">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="64ea1-169">Ale musíte toomanually zaregistrovat někteří poskytovatelé prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-169">However, you may need toomanually register some resource providers.</span></span> <span data-ttu-id="64ea1-170">tooregister poskytovatele prostředků, musíte mít oprávnění tooperform hello `/register/action` operace pro poskytovatele prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-170">tooregister a resource provider, you must have permission tooperform hello `/register/action` operation for hello resource provider.</span></span> <span data-ttu-id="64ea1-171">Tato operace je součástí hello Přispěvatel a vlastníka role.</span><span class="sxs-lookup"><span data-stu-id="64ea1-171">This operation is included in hello Contributor and Owner roles.</span></span> <span data-ttu-id="64ea1-172">Vyberte tooregister zprostředkovatel prostředků, **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="64ea1-172">tooregister a resource provider, select **Register**.</span></span>

![registrace poskytovatele prostředků](./media/resource-manager-supported-services/register-provider.png)

<span data-ttu-id="64ea1-174">Pokud máte pořád typy prostředků z tohoto zprostředkovatele prostředků v rámci vašeho předplatného, nelze zrušit registraci poskytovatele prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-174">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="64ea1-175">informace o toosee pro určitý prostředek poskytovatele, vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="64ea1-175">toosee information for a particular resource provider, select **More services**.</span></span>

![Vyberte další služby](./media/resource-manager-supported-services/more-services.png)

<span data-ttu-id="64ea1-177">Vyhledejte **Průzkumníka prostředků** a vyberte z dostupných možností hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-177">Search for **Resource Explorer** and select it from hello available options.</span></span>

![Vyberte Průzkumník prostředků](./media/resource-manager-supported-services/select-resource-explorer.png)

<span data-ttu-id="64ea1-179">Vyberte **zprostředkovatelé**.</span><span class="sxs-lookup"><span data-stu-id="64ea1-179">Select **Providers**.</span></span>

![Vyberte zprostředkovatele](./media/resource-manager-supported-services/select-providers.png)

<span data-ttu-id="64ea1-181">Vyberte hello poskytovatele prostředků a prostředků zadejte, že chcete tooview.</span><span class="sxs-lookup"><span data-stu-id="64ea1-181">Select hello resource provider and resource type that you want tooview.</span></span>

![Vyberte typ prostředku](./media/resource-manager-supported-services/select-resource-type.png)

<span data-ttu-id="64ea1-183">Správce prostředků se podporuje ve všech oblastech, ale nemusí být hello prostředky, které nasazujete podporován ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="64ea1-183">Resource Manager is supported in all regions, but hello resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="64ea1-184">Kromě toho může být omezení na vaše předplatné, které zabránit vám v použití některé oblasti, které podporují hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="64ea1-184">In addition, there may be limitations on your subscription that prevent you from using some regions that support hello resource.</span></span> <span data-ttu-id="64ea1-185">Průzkumník prostředků Hello zobrazí platná umístění pro typ prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-185">hello resource explorer displays valid locations for hello resource type.</span></span>

![Zobrazit umístění](./media/resource-manager-supported-services/show-locations.png)

<span data-ttu-id="64ea1-187">verze rozhraní API Hello odpovídá verzi tooa operací REST API, které jsou vydané poskytovatelem prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-187">hello API version corresponds tooa version of REST API operations that are released by hello resource provider.</span></span> <span data-ttu-id="64ea1-188">Zprostředkovatel prostředků umožňuje nové funkce, uvolní novou verzi hello REST API.</span><span class="sxs-lookup"><span data-stu-id="64ea1-188">As a resource provider enables new features, it releases a new version of hello REST API.</span></span> <span data-ttu-id="64ea1-189">Průzkumník prostředků Hello zobrazí platná verze rozhraní API pro typ prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="64ea1-189">hello resource explorer displays valid API versions for hello resource type.</span></span>

![Zobrazit verze rozhraní API](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a><span data-ttu-id="64ea1-191">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64ea1-191">Next steps</span></span>
* <span data-ttu-id="64ea1-192">toolearn o vytváření šablon Resource Manageru, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="64ea1-192">toolearn about creating Resource Manager templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="64ea1-193">toolearn o nasazení prostředků, najdete v části [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="64ea1-193">toolearn about deploying resources, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="64ea1-194">operace hello tooview pro poskytovatele prostředků, najdete v části [rozhraní REST API Azure](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="64ea1-194">tooview hello operations for a resource provider, see [Azure REST API](/rest/api/).</span></span>

