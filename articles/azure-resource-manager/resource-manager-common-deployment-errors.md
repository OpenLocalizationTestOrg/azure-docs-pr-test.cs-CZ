---
title: "Odstraňování běžných chyb nasazení Azure | Microsoft Docs"
description: "Popisuje postup řešení běžných chyb při nasazování prostředků do Azure pomocí Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Chyba nasazení, nasazení azure nasazení do azure"
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: tomfitz
ms.openlocfilehash: 30adc10d01290f14a3e116813b19916fa36ab0bc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a><span data-ttu-id="0206e-104">Odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="0206e-104">Troubleshoot common Azure deployment errors with Azure Resource Manager</span></span>
<span data-ttu-id="0206e-105">Toto téma popisuje, jak můžete vyřešit některé běžné chyby nasazení Azure se můžete setkat.</span><span class="sxs-lookup"><span data-stu-id="0206e-105">This topic describes how you can resolve some common Azure deployment errors you may encounter.</span></span>

<span data-ttu-id="0206e-106">V tomto tématu jsou popsány následující kódy chyb:</span><span class="sxs-lookup"><span data-stu-id="0206e-106">The following error codes are described in this topic:</span></span>

* [<span data-ttu-id="0206e-107">AccountNameInvalid</span><span class="sxs-lookup"><span data-stu-id="0206e-107">AccountNameInvalid</span></span>](#accountnameinvalid)
* [<span data-ttu-id="0206e-108">Ověření se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="0206e-108">Authorization failed</span></span>](#authorization-failed)
* [<span data-ttu-id="0206e-109">Struktura BadRequest</span><span class="sxs-lookup"><span data-stu-id="0206e-109">BadRequest</span></span>](#badrequest)
* [<span data-ttu-id="0206e-110">DeploymentFailed</span><span class="sxs-lookup"><span data-stu-id="0206e-110">DeploymentFailed</span></span>](#deploymentfailed)
* [<span data-ttu-id="0206e-111">DisallowedOperation</span><span class="sxs-lookup"><span data-stu-id="0206e-111">DisallowedOperation</span></span>](#disallowedoperation)
* [<span data-ttu-id="0206e-112">InvalidContentLink</span><span class="sxs-lookup"><span data-stu-id="0206e-112">InvalidContentLink</span></span>](#invalidcontentlink)
* [<span data-ttu-id="0206e-113">InvalidTemplate</span><span class="sxs-lookup"><span data-stu-id="0206e-113">InvalidTemplate</span></span>](#invalidtemplate)
* [<span data-ttu-id="0206e-114">MissingSubscriptionRegistration</span><span class="sxs-lookup"><span data-stu-id="0206e-114">MissingSubscriptionRegistration</span></span>](#noregisteredproviderfound)
* [<span data-ttu-id="0206e-115">NotFound</span><span class="sxs-lookup"><span data-stu-id="0206e-115">NotFound</span></span>](#notfound)
* [<span data-ttu-id="0206e-116">NoRegisteredProviderFound</span><span class="sxs-lookup"><span data-stu-id="0206e-116">NoRegisteredProviderFound</span></span>](#noregisteredproviderfound)
* [<span data-ttu-id="0206e-117">OperationNotAllowed</span><span class="sxs-lookup"><span data-stu-id="0206e-117">OperationNotAllowed</span></span>](#quotaexceeded)
* [<span data-ttu-id="0206e-118">ParentResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="0206e-118">ParentResourceNotFound</span></span>](#parentresourcenotfound)
* [<span data-ttu-id="0206e-119">QuotaExceeded</span><span class="sxs-lookup"><span data-stu-id="0206e-119">QuotaExceeded</span></span>](#quotaexceeded)
* [<span data-ttu-id="0206e-120">RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="0206e-120">RequestDisallowedByPolicy</span></span>](#requestdisallowedbypolicy)
* [<span data-ttu-id="0206e-121">ResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="0206e-121">ResourceNotFound</span></span>](#notfound)
* [<span data-ttu-id="0206e-122">SkuNotAvailable</span><span class="sxs-lookup"><span data-stu-id="0206e-122">SkuNotAvailable</span></span>](#skunotavailable)
* [<span data-ttu-id="0206e-123">StorageAccountAlreadyExists</span><span class="sxs-lookup"><span data-stu-id="0206e-123">StorageAccountAlreadyExists</span></span>](#storagenamenotunique)
* [<span data-ttu-id="0206e-124">StorageAccountAlreadyTaken</span><span class="sxs-lookup"><span data-stu-id="0206e-124">StorageAccountAlreadyTaken</span></span>](#storagenamenotunique)

## <a name="deploymentfailed"></a><span data-ttu-id="0206e-125">DeploymentFailed</span><span class="sxs-lookup"><span data-stu-id="0206e-125">DeploymentFailed</span></span>

<span data-ttu-id="0206e-126">Tento kód chyby označuje Chyba obecné nasazení, ale není kód chyby, které je třeba spustit Poradce při potížích s.</span><span class="sxs-lookup"><span data-stu-id="0206e-126">This error code indicates a general deployment error, but it is not the error code you need to start troubleshooting.</span></span> <span data-ttu-id="0206e-127">Kód chyby, která ve skutečnosti pomáhá vyřešit tento problém je obvykle jednu úroveň pod tuto chybu.</span><span class="sxs-lookup"><span data-stu-id="0206e-127">The error code that actually helps you resolve the issue is usually one level below this error.</span></span> <span data-ttu-id="0206e-128">Například na následujícím obrázku **RequestDisallowedByPolicy** kód chyby, který je pod chyba nasazení.</span><span class="sxs-lookup"><span data-stu-id="0206e-128">For example, the following image shows the **RequestDisallowedByPolicy** error code that is under the deployment error.</span></span>

![Zobrazí kód chyby](./media/resource-manager-common-deployment-errors/error-code.png)

## <a name="skunotavailable"></a><span data-ttu-id="0206e-130">SkuNotAvailable</span><span class="sxs-lookup"><span data-stu-id="0206e-130">SkuNotAvailable</span></span>

<span data-ttu-id="0206e-131">Pokud nasazujete prostředku (obvykle virtuální počítač), může se zobrazit následující kód chyby a chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="0206e-131">When deploying a resource (typically a virtual machine), you may receive the following error code and error message:</span></span>

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

<span data-ttu-id="0206e-132">Tato chyba se zobrazí, když prostředek SKU, které jste vybrali (například velikost virtuálního počítače) není k dispozici pro umístění, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="0206e-132">You receive this error when the resource SKU you have selected (such as VM size) is not available for the location you have selected.</span></span> <span data-ttu-id="0206e-133">Chcete-li vyřešit tento problém, musíte určit, která SKU jsou k dispozici v oblasti.</span><span class="sxs-lookup"><span data-stu-id="0206e-133">To resolve this issue, you need to determine which SKUs are available in a region.</span></span> <span data-ttu-id="0206e-134">Operaci REST, na portálu nebo prostředí PowerShell můžete použít k vyhledání dostupné edice.</span><span class="sxs-lookup"><span data-stu-id="0206e-134">You can use PowerShell, the portal, or a REST operation to find available SKUs.</span></span>

- <span data-ttu-id="0206e-135">Pro prostředí PowerShell, použijte [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) a filtrovat podle umístění.</span><span class="sxs-lookup"><span data-stu-id="0206e-135">For PowerShell, use [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) and filter by location.</span></span> <span data-ttu-id="0206e-136">Musíte mít nejnovější verzi prostředí PowerShell pro tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="0206e-136">You must have the latest version of PowerShell for this command.</span></span>

  ```powershell
  Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
  ```

  <span data-ttu-id="0206e-137">Výsledky budou zahrnovat seznam SKU pro umístění a nějaká omezení pro tento SKU.</span><span class="sxs-lookup"><span data-stu-id="0206e-137">The results include a list of SKUs for the location and any restrictions for that SKU.</span></span>

  ```powershell
  ResourceType                Name      Locations Restriction                      Capability Value
  ------------                ----      --------- -----------                      ---------- -----
  availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
  availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
  virtualMachines      Standard_A0 southcentralus
  virtualMachines      Standard_A1 southcentralus
  virtualMachines      Standard_A2 southcentralus
  ```

- <span data-ttu-id="0206e-138">Použít [portál](https://portal.azure.com), přihlaste se k portálu a přidat prostředek přes rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0206e-138">To use the [portal](https://portal.azure.com), log in to the portal and add a resource through the interface.</span></span> <span data-ttu-id="0206e-139">Při nastavování hodnoty, uvidíte dostupné edice pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="0206e-139">As you set the values, you see the available SKUs for that resource.</span></span> <span data-ttu-id="0206e-140">Není nutné k dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="0206e-140">You do not need to complete the deployment.</span></span>

    ![dostupné edice](./media/resource-manager-common-deployment-errors/view-sku.png)

- <span data-ttu-id="0206e-142">Použití rozhraní REST API pro virtuální počítače, odešlete požadavek na následující:</span><span class="sxs-lookup"><span data-stu-id="0206e-142">To use the REST API for virtual machines, send the following request:</span></span>

  ```HTTP 
  GET
  https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
  ```

  <span data-ttu-id="0206e-143">Vrátí k dispozici SKU a oblastí, v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="0206e-143">It returns available SKUs and regions in the following format:</span></span>

  ```json
  {
    "value": [
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A0",
        "tier": "Standard",
        "size": "A0",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A1",
        "tier": "Standard",
        "size": "A1",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      ...
    ]
  }    
  ```

<span data-ttu-id="0206e-144">Pokud jste se nepodařilo najít vhodnou SKU v této oblasti nebo alternativní oblast, která splňuje vaše obchodní potřebuje, odeslání [SKU požadavek](https://aka.ms/skurestriction) pro podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="0206e-144">If you are unable to find a suitable SKU in that region or an alternative region that meets your business needs, submit a [SKU request](https://aka.ms/skurestriction) to Azure Support.</span></span>

## <a name="disallowedoperation"></a><span data-ttu-id="0206e-145">DisallowedOperation</span><span class="sxs-lookup"><span data-stu-id="0206e-145">DisallowedOperation</span></span>

```
Code: DisallowedOperation
Message: The current subscription type is not permitted to perform operations on any provider 
namespace. Please use a different subscription.
```

<span data-ttu-id="0206e-146">Pokud se zobrazí tato chyba, používáte odběr, který nemá oprávnění k přístupu služby Azure než Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0206e-146">If you receive this error, you are using a subscription that is not permitted to access any Azure services other than Azure Active Directory.</span></span> <span data-ttu-id="0206e-147">Pokud potřebujete přístup k portálu classic, ale nejsou povoleny pro nasazení prostředků můžete mít tento typ předplatného.</span><span class="sxs-lookup"><span data-stu-id="0206e-147">You might have this type of subscription when you need to access the classic portal but are not permitted to deploy resources.</span></span> <span data-ttu-id="0206e-148">Chcete-li vyřešit tento problém, musíte použít odběr, který má oprávnění k nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="0206e-148">To resolve this issue, you must use a subscription that has permission to deploy resources.</span></span>  

<span data-ttu-id="0206e-149">Chcete-li zobrazit předplatné k dispozici v prostředí PowerShell, použijte:</span><span class="sxs-lookup"><span data-stu-id="0206e-149">To view your available subscriptions with PowerShell, use:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="0206e-150">A pokud chcete nastavit aktuální předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="0206e-150">And, to set the current subscription, use:</span></span>

```powershell
Set-AzureRmContext -SubscriptionName {subscription-name}
```

<span data-ttu-id="0206e-151">Chcete-li zobrazit dostupné předplatné s Azure CLI 2.0, použijte:</span><span class="sxs-lookup"><span data-stu-id="0206e-151">To view your available subscriptions with Azure CLI 2.0, use:</span></span>

```azurecli
az account list
```

<span data-ttu-id="0206e-152">A pokud chcete nastavit aktuální předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="0206e-152">And, to set the current subscription, use:</span></span>

```azurecli
az account set --subscription {subscription-name}
```

## <a name="invalidtemplate"></a><span data-ttu-id="0206e-153">InvalidTemplate</span><span class="sxs-lookup"><span data-stu-id="0206e-153">InvalidTemplate</span></span>
<span data-ttu-id="0206e-154">Tato chyba může způsobit z několika různých typů chyb.</span><span class="sxs-lookup"><span data-stu-id="0206e-154">This error can result from several different types of errors.</span></span>

- <span data-ttu-id="0206e-155">Chyba syntaxe</span><span class="sxs-lookup"><span data-stu-id="0206e-155">Syntax error</span></span>

   <span data-ttu-id="0206e-156">Pokud se zobrazí chybovou zprávu, která určuje ověření šablony se nezdařilo, bude pravděpodobně problém syntaxe ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="0206e-156">If you receive an error message that indicates the template failed validation, you may have a syntax problem in your template.</span></span>

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed
  ```

   <span data-ttu-id="0206e-157">Tato chyba je snadné provést, protože může být složité výrazy šablony.</span><span class="sxs-lookup"><span data-stu-id="0206e-157">This error is easy to make because template expressions can be intricate.</span></span> <span data-ttu-id="0206e-158">Například následující přiřazení název pro účet úložiště obsahuje jednu sadu závorky, tři funkce, tři sady závorky, jednu sadu jednoduchých uvozovek a jednu vlastnost:</span><span class="sxs-lookup"><span data-stu-id="0206e-158">For example, the following name assignment for a storage account contains one set of brackets, three functions, three sets of parentheses, one set of single quotes, and one property:</span></span>

  ```json
  "name": "[concat('storage', uniqueString(resourceGroup().id))]",
  ```

   <span data-ttu-id="0206e-159">Pokud nezadáte syntaxe porovnávání, vytvoří šablona hodnotu, která se liší od svůj záměr.</span><span class="sxs-lookup"><span data-stu-id="0206e-159">If you do not provide the matching syntax, the template produces a value that is different than your intention.</span></span>

   <span data-ttu-id="0206e-160">Po zobrazení tohoto typu chyby, pečlivě zkontrolujte syntaxi výrazu.</span><span class="sxs-lookup"><span data-stu-id="0206e-160">When you receive this type of error, carefully review the expression syntax.</span></span> <span data-ttu-id="0206e-161">Zvažte použití editor JSON jako [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) nebo [Visual Studio Code](resource-manager-vs-code.md), která vás upozorní chyby syntaxe.</span><span class="sxs-lookup"><span data-stu-id="0206e-161">Consider using a JSON editor like [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) or [Visual Studio Code](resource-manager-vs-code.md), which can warn you about syntax errors.</span></span>

- <span data-ttu-id="0206e-162">Segment nesprávné délky</span><span class="sxs-lookup"><span data-stu-id="0206e-162">Incorrect segment lengths</span></span>

   <span data-ttu-id="0206e-163">Další neplatný šablony chyba nastane, když název prostředku není ve správném formátu.</span><span class="sxs-lookup"><span data-stu-id="0206e-163">Another invalid template error occurs when the resource name is not in the correct format.</span></span>

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed: 'The template resource {resource-name}'
  for type {resource-type} has incorrect segment lengths.
  ```

   <span data-ttu-id="0206e-164">Kořenové úrovně prostředku musí mít jeden menší segmentu v názvu než v typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="0206e-164">A root level resource must have one less segment in the name than in the resource type.</span></span> <span data-ttu-id="0206e-165">Každý segment je rozlišené pomocí lomítko.</span><span class="sxs-lookup"><span data-stu-id="0206e-165">Each segment is differentiated by a slash.</span></span> <span data-ttu-id="0206e-166">V následujícím příkladu, typ má dva segmenty a název jednoho segmentu, tak, aby byl **platný název**.</span><span class="sxs-lookup"><span data-stu-id="0206e-166">In the following example, the type has two segments and the name has one segment, so it is a **valid name**.</span></span>

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "myHostingPlanName",
    ...
  }
  ```

   <span data-ttu-id="0206e-167">Dalším příkladem je, ale **není platný název** protože má stejný počet segmentů jako typ.</span><span class="sxs-lookup"><span data-stu-id="0206e-167">But the next example is **not a valid name** because it has the same number of segments as the type.</span></span>

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "appPlan/myHostingPlanName",
    ...
  }
  ```

   <span data-ttu-id="0206e-168">Pro podřízené prostředky typ a název mít stejný počet segmentů.</span><span class="sxs-lookup"><span data-stu-id="0206e-168">For child resources, the type and name have the same number of segments.</span></span> <span data-ttu-id="0206e-169">Tento počet segmentů dává smysl, protože úplný název a typ pro podřízený objekt zahrnuje název nadřazeného a typu.</span><span class="sxs-lookup"><span data-stu-id="0206e-169">This number of segments makes sense because the full name and type for the child includes the parent name and type.</span></span> <span data-ttu-id="0206e-170">Úplný název proto má stále jednoho menší segmentu než úplné typu.</span><span class="sxs-lookup"><span data-stu-id="0206e-170">Therefore, the full name still has one less segment than the full type.</span></span>

  ```json
  "resources": [
      {
          "type": "Microsoft.KeyVault/vaults",
          "name": "contosokeyvault",
          ...
          "resources": [
              {
                  "type": "secrets",
                  "name": "appPassword",
                  ...
              }
          ]
      }
  ]
  ```

   <span data-ttu-id="0206e-171">Získávání segmentů správné může být složité s typy Resource Manager, které se použijí napříč zprostředkovatelé prostředků.</span><span class="sxs-lookup"><span data-stu-id="0206e-171">Getting the segments right can be tricky with Resource Manager types that are applied across resource providers.</span></span> <span data-ttu-id="0206e-172">Například použití prostředků zámku na web vyžaduje typ s čtyři segmenty.</span><span class="sxs-lookup"><span data-stu-id="0206e-172">For example, applying a resource lock to a web site requires a type with four segments.</span></span> <span data-ttu-id="0206e-173">Název je tedy tři segmenty:</span><span class="sxs-lookup"><span data-stu-id="0206e-173">Therefore, the name is three segments:</span></span>

  ```json
  {
      "type": "Microsoft.Web/sites/providers/locks",
      "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
      ...
  }
  ```

- <span data-ttu-id="0206e-174">Kopírování indexu není očekáván.</span><span class="sxs-lookup"><span data-stu-id="0206e-174">Copy index is not expected</span></span>

   <span data-ttu-id="0206e-175">Narazíte na to **InvalidTemplate** chyba, když jste použili **kopie** element součástí šablony, která nepodporuje tento element.</span><span class="sxs-lookup"><span data-stu-id="0206e-175">You encounter this **InvalidTemplate** error when you have applied the **copy** element to a part of the template that does not support this element.</span></span> <span data-ttu-id="0206e-176">Element kopie lze použít pouze pro typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="0206e-176">You can only apply the copy element to a resource type.</span></span> <span data-ttu-id="0206e-177">Kopírování nelze použít na vlastnost v rámci typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="0206e-177">You cannot apply copy to a property within a resource type.</span></span> <span data-ttu-id="0206e-178">Například použijete kopie virtuálního počítače, ale nelze ho použít pro disky operačního systému pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="0206e-178">For example, you apply copy to a virtual machine, but you cannot apply it to the OS disks for a virtual machine.</span></span> <span data-ttu-id="0206e-179">V některých případech může převést prostředek podřízené na nadřazený prostředek pro vytvoření kopie smyčky.</span><span class="sxs-lookup"><span data-stu-id="0206e-179">In some cases, you can convert a child resource to a parent resource to create a copy loop.</span></span> <span data-ttu-id="0206e-180">Další informace o používání kopírování najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="0206e-180">For more information about using copy, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

- <span data-ttu-id="0206e-181">Parametr není platný</span><span class="sxs-lookup"><span data-stu-id="0206e-181">Parameter is not valid</span></span>

   <span data-ttu-id="0206e-182">Pokud šablona specifikuje povolené hodnoty pro parametr, a zadáte hodnotu, která není jedním z těchto hodnot, zobrazí se zpráva podobná následující chybě:</span><span class="sxs-lookup"><span data-stu-id="0206e-182">If the template specifies permitted values for a parameter, and you provide a value that is not one of those values, you receive a message similar to the following error:</span></span>

  ```
  Code=InvalidTemplate;
  Message=Deployment template validation failed: 'The provided value {parameter value}
  for the template parameter {parameter name} is not valid. The parameter value is not
  part of the allowed values
  ``` 

   <span data-ttu-id="0206e-183">Double zkontrolujte povolené hodnoty v šabloně a zadejte jeden během nasazení.</span><span class="sxs-lookup"><span data-stu-id="0206e-183">Double check the allowed values in the template, and provide one during deployment.</span></span>

- <span data-ttu-id="0206e-184">Zjistila se cyklická závislost</span><span class="sxs-lookup"><span data-stu-id="0206e-184">Circular dependency detected</span></span>

   <span data-ttu-id="0206e-185">Tato chyba se zobrazí, když prostředky jsou vzájemně závislé způsobem, který zabrání spuštění nasazení.</span><span class="sxs-lookup"><span data-stu-id="0206e-185">You receive this error when resources depend on each other in a way that prevents the deployment from starting.</span></span> <span data-ttu-id="0206e-186">Kombinace vzájemné závislosti díky dvou nebo více prostředků čekat na další prostředky, které jsou také čekání.</span><span class="sxs-lookup"><span data-stu-id="0206e-186">A combination of interdependencies makes two or more resource wait for other resources that are also waiting.</span></span> <span data-ttu-id="0206e-187">Například resource1 závisí na resource3, resource2 závisí na resource1 a resource3 závisí na resource2.</span><span class="sxs-lookup"><span data-stu-id="0206e-187">For example, resource1 depends on resource3, resource2 depends on resource1, and resource3 depends on resource2.</span></span> <span data-ttu-id="0206e-188">Obvykle můžete tento problém vyřešit odstraněním nepotřebných závislosti.</span><span class="sxs-lookup"><span data-stu-id="0206e-188">You can usually solve this problem by removing unnecessary dependencies.</span></span> 

<a id="notfound" />
### <a name="notfound-and-resourcenotfound"></a><span data-ttu-id="0206e-189">NotFound a ResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="0206e-189">NotFound and ResourceNotFound</span></span>
<span data-ttu-id="0206e-190">Pokud vaše šablona zahrnuje název prostředku, které nelze vyřešit, obdržíte chybu podobně jako:</span><span class="sxs-lookup"><span data-stu-id="0206e-190">When your template includes the name of a resource that cannot be resolved, you receive an error similar to:</span></span>

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

<span data-ttu-id="0206e-191">Pokud se pokoušíte nasadit chybějící prostředek v šabloně, zkontrolujte, jestli je potřeba přidat závislost.</span><span class="sxs-lookup"><span data-stu-id="0206e-191">If you are attempting to deploy the missing resource in the template, check whether you need to add a dependency.</span></span> <span data-ttu-id="0206e-192">Správce prostředků optimalizuje nasazení tak, že vytvoříte prostředky paralelně, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="0206e-192">Resource Manager optimizes deployment by creating resources in parallel, when possible.</span></span> <span data-ttu-id="0206e-193">Pokud po jiný prostředek musí být nasazený jeden prostředek, budete muset použít **dependsOn** element v šabloně pro vytvoření závislosti na jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="0206e-193">If one resource must be deployed after another resource, you need to use the **dependsOn** element in your template to create a dependency on the other resource.</span></span> <span data-ttu-id="0206e-194">Například Pokud nasazujete webovou aplikaci, musí existovat plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0206e-194">For example, when deploying a web app, the App Service plan must exist.</span></span> <span data-ttu-id="0206e-195">Pokud jste nezadali, že webová aplikace závisí na plán služby App Service, vytvoří Resource Manager i prostředky ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="0206e-195">If you have not specified that the web app depends on the App Service plan, Resource Manager creates both resources at the same time.</span></span> <span data-ttu-id="0206e-196">Zobrazí se chyba oznamující, že nelze najít prostředek plánu služby App Service, protože neexistuje ještě při pokusu o nastavení vlastnosti ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0206e-196">You receive an error stating that the App Service plan resource cannot be found, because it does not exist yet when attempting to set a property on the web app.</span></span> <span data-ttu-id="0206e-197">Tuto chybu můžete zabránit nastavením závislost ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0206e-197">You prevent this error by setting the dependency in the web app.</span></span>

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

<span data-ttu-id="0206e-198">Návrhy na řešení potíží s chybami závislostí, najdete v části [Zkontrolujte pořadí nasazení](#check-deployment-sequence).</span><span class="sxs-lookup"><span data-stu-id="0206e-198">For suggestions on troubleshooting dependency errors, see [Check deployment sequence](#check-deployment-sequence).</span></span>

<span data-ttu-id="0206e-199">Můžete také zobrazit chyba při prostředek existuje v jiné skupině prostředků než ten, který nasazován.</span><span class="sxs-lookup"><span data-stu-id="0206e-199">You also see this error when the resource exists in a different resource group than the one being deployed to.</span></span> <span data-ttu-id="0206e-200">V takovém případě použijte [resourceId funkce](resource-group-template-functions-resource.md#resourceid) získat plně kvalifikovaný název prostředku.</span><span class="sxs-lookup"><span data-stu-id="0206e-200">In that case, use the [resourceId function](resource-group-template-functions-resource.md#resourceid) to get the fully qualified name of the resource.</span></span>

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

<span data-ttu-id="0206e-201">Pokud pokus o použití [odkaz](resource-group-template-functions-resource.md#reference) nebo [listKeys](resource-group-template-functions-resource.md#listkeys) funkce s prostředky, které nelze vyřešit, zobrazí se následující chyba:</span><span class="sxs-lookup"><span data-stu-id="0206e-201">If you attempt to use the [reference](resource-group-template-functions-resource.md#reference) or [listKeys](resource-group-template-functions-resource.md#listkeys) functions with a resource that cannot be resolved, you receive the following error:</span></span>

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

<span data-ttu-id="0206e-202">Vyhledat výraz, který zahrnuje **odkaz** funkce.</span><span class="sxs-lookup"><span data-stu-id="0206e-202">Look for an expression that includes the **reference** function.</span></span> <span data-ttu-id="0206e-203">Zkontrolujte, zda hodnoty parametrů jsou správné.</span><span class="sxs-lookup"><span data-stu-id="0206e-203">Double check that the parameter values are correct.</span></span>

## <a name="parentresourcenotfound"></a><span data-ttu-id="0206e-204">ParentResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="0206e-204">ParentResourceNotFound</span></span>

<span data-ttu-id="0206e-205">Pokud jeden prostředek je nadřazený na jiný prostředek, musí existovat nadřazený prostředek před vytvořením podřízené prostředků.</span><span class="sxs-lookup"><span data-stu-id="0206e-205">When one resource is a parent to another resource, the parent resource must exist before creating the child resource.</span></span> <span data-ttu-id="0206e-206">Pokud ještě neexistuje, zobrazí se následující chyba:</span><span class="sxs-lookup"><span data-stu-id="0206e-206">If it does not yet exist, you receive the following error:</span></span>

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

<span data-ttu-id="0206e-207">Název prostředku podřízené obsahuje název nadřazené.</span><span class="sxs-lookup"><span data-stu-id="0206e-207">The name of the child resource includes the parent name.</span></span> <span data-ttu-id="0206e-208">Například může být definovaná SQL Database jako:</span><span class="sxs-lookup"><span data-stu-id="0206e-208">For example, a SQL Database might be defined as:</span></span>

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

<span data-ttu-id="0206e-209">Ale pokud nezadáte závislost na nadřazeném prostředku, může získat prostředek podřízené nasadit před nadřazený.</span><span class="sxs-lookup"><span data-stu-id="0206e-209">But, if you do not specify a dependency on the parent resource, the child resource may get deployed before the parent.</span></span> <span data-ttu-id="0206e-210">Chcete-li tuto chybu vyřešit, obsahovat závislost.</span><span class="sxs-lookup"><span data-stu-id="0206e-210">To resolve this error, include a dependency.</span></span>

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

<a id="storagenamenotunique" />

## <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a><span data-ttu-id="0206e-211">StorageAccountAlreadyExists a StorageAccountAlreadyTaken</span><span class="sxs-lookup"><span data-stu-id="0206e-211">StorageAccountAlreadyExists and StorageAccountAlreadyTaken</span></span>
<span data-ttu-id="0206e-212">Pro účty úložiště je nutné zadat název pro prostředek, který je v rámci Azure jedinečný.</span><span class="sxs-lookup"><span data-stu-id="0206e-212">For storage accounts, you must provide a name for the resource that is unique across Azure.</span></span> <span data-ttu-id="0206e-213">Pokud nezadáte jedinečný název, zobrazí chybu jako:</span><span class="sxs-lookup"><span data-stu-id="0206e-213">If you do not provide a unique name, you receive an error like:</span></span>

```
Code=StorageAccountAlreadyTaken
Message=The storage account named mystorage is already taken.
```

<span data-ttu-id="0206e-214">Můžete vytvořit jedinečný název zřetězení vaše zásady vytváření názvů s výsledkem [uniqueString](resource-group-template-functions-string.md#uniquestring) funkce.</span><span class="sxs-lookup"><span data-stu-id="0206e-214">You can create a unique name by concatenating your naming convention with the result of the [uniqueString](resource-group-template-functions-string.md#uniquestring) function.</span></span>

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

<span data-ttu-id="0206e-215">Pokud nasazení účtu úložiště se stejným názvem jako stávající účet úložiště v rámci vašeho předplatného, ale zadejte jiné umístění, zobrazí se chyba oznamující, že účet úložiště již existuje v jiném umístění.</span><span class="sxs-lookup"><span data-stu-id="0206e-215">If you deploy a storage account with the same name as an existing storage account in your subscription, but provide a different location, you receive an error indicating the storage account already exists in a different location.</span></span> <span data-ttu-id="0206e-216">Buď existující účet úložiště odstranit, nebo zadejte stejné umístění jako stávající účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="0206e-216">Either delete the existing storage account, or provide the same location as the existing storage account.</span></span>

## <a name="accountnameinvalid"></a><span data-ttu-id="0206e-217">AccountNameInvalid</span><span class="sxs-lookup"><span data-stu-id="0206e-217">AccountNameInvalid</span></span>
<span data-ttu-id="0206e-218">Zobrazí **AccountNameInvalid** Chyba při pokusu o účtu úložiště poskytnout název, který obsahuje zakázané znaky.</span><span class="sxs-lookup"><span data-stu-id="0206e-218">You see the **AccountNameInvalid** error when attempting to give a storage account a name that includes prohibited characters.</span></span> <span data-ttu-id="0206e-219">Názvy účtů úložiště musí být v rozmezí 3 až 24 znaků a používat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="0206e-219">Storage account names must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span> <span data-ttu-id="0206e-220">[UniqueString](resource-group-template-functions-string.md#uniquestring) funkce vrátí 13 znaků.</span><span class="sxs-lookup"><span data-stu-id="0206e-220">The [uniqueString](resource-group-template-functions-string.md#uniquestring) function returns 13 characters.</span></span> <span data-ttu-id="0206e-221">Pokud řetězení předpony **uniqueString** vést, zadejte předponu, která je 11 znaků nebo méně.</span><span class="sxs-lookup"><span data-stu-id="0206e-221">If you concatenate a prefix to the **uniqueString** result, provide a prefix that is 11 characters or less.</span></span>

## <a name="badrequest"></a><span data-ttu-id="0206e-222">Struktura BadRequest</span><span class="sxs-lookup"><span data-stu-id="0206e-222">BadRequest</span></span>

<span data-ttu-id="0206e-223">Struktura BadRequest stav se můžete setkat, když zadáte neplatnou hodnotu pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="0206e-223">You may encounter a BadRequest status when you provide an invalid value for a property.</span></span> <span data-ttu-id="0206e-224">Například pokud jste zadali nesprávnou hodnotu SKU pro účet úložiště, nasazení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="0206e-224">For example, if you provide an incorrect SKU value for a storage account, the deployment fails.</span></span> <span data-ttu-id="0206e-225">Chcete-li určit platné hodnoty pro vlastnost, podívejte se [REST API](/rest/api) pro typ prostředku, kterou nasazujete.</span><span class="sxs-lookup"><span data-stu-id="0206e-225">To determine valid values for property, look at the [REST API](/rest/api) for the resource type you are deploying.</span></span>

<a id="noregisteredproviderfound" />

## <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a><span data-ttu-id="0206e-226">NoRegisteredProviderFound a MissingSubscriptionRegistration</span><span class="sxs-lookup"><span data-stu-id="0206e-226">NoRegisteredProviderFound and MissingSubscriptionRegistration</span></span>
<span data-ttu-id="0206e-227">Při nasazování prostředků, může se zobrazit následující kód chyby a zpráva:</span><span class="sxs-lookup"><span data-stu-id="0206e-227">When deploying resource, you may receive the following error code and message:</span></span>

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

<span data-ttu-id="0206e-228">Nebo se může zobrazit podobné zpráva, že:</span><span class="sxs-lookup"><span data-stu-id="0206e-228">Or, you may receive a similar message that states:</span></span>

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

<span data-ttu-id="0206e-229">Zobrazí tyto chyby pro jednu ze tří důvodů:</span><span class="sxs-lookup"><span data-stu-id="0206e-229">You receive these errors for one of three reasons:</span></span>

1. <span data-ttu-id="0206e-230">Zprostředkovatel prostředků není zaregistrován pro vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="0206e-230">The resource provider has not been registered for your subscription</span></span>
2. <span data-ttu-id="0206e-231">Verze rozhraní API není podporována pro typ prostředku</span><span class="sxs-lookup"><span data-stu-id="0206e-231">API version not supported for the resource type</span></span>
3. <span data-ttu-id="0206e-232">Umístění není podporováno pro typ prostředku</span><span class="sxs-lookup"><span data-stu-id="0206e-232">Location not supported for the resource type</span></span>

<span data-ttu-id="0206e-233">Chybová zpráva by měla dát návrhy na podporovaném umístění a verze rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0206e-233">The error message should give you suggestions for the supported locations and API versions.</span></span> <span data-ttu-id="0206e-234">Šablony můžete změnit na jednu z Tyhle navrhované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0206e-234">You can change your template to one of the suggested values.</span></span> <span data-ttu-id="0206e-235">Většina poskytovatelé jsou registrované automaticky pomocí portálu Azure nebo rozhraní příkazového řádku, který používáte, ale ne všechny.</span><span class="sxs-lookup"><span data-stu-id="0206e-235">Most providers are registered automatically by the Azure portal or the command-line interface you are using, but not all.</span></span> <span data-ttu-id="0206e-236">Pokud jste nepoužili poskytovatele určitému prostředku před, musíte k registraci tohoto zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="0206e-236">If you have not used a particular resource provider before, you may need to register that provider.</span></span> <span data-ttu-id="0206e-237">Může zjišťovat informace o poskytovatelích prostředků prostřednictvím PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="0206e-237">You can discover more about resource providers through PowerShell or Azure CLI.</span></span>

<span data-ttu-id="0206e-238">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="0206e-238">**Portal**</span></span>

<span data-ttu-id="0206e-239">Můžete zobrazit stav registrace a zaregistrovat obor názvů zprostředkovatele prostředků prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="0206e-239">You can see the registration status and register a resource provider namespace through the portal.</span></span>

1. <span data-ttu-id="0206e-240">Pro vaše předplatné, vyberte **zprostředkovatelé prostředků**.</span><span class="sxs-lookup"><span data-stu-id="0206e-240">For your subscription, select **Resource providers**.</span></span>

   ![Vyberte poskytovatele prostředků](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. <span data-ttu-id="0206e-242">Podívejte se na seznam poskytovatelů prostředků a v případě potřeby vyberte **zaregistrovat** odkaz k registraci poskytovatele prostředků typu, který se snažíte nasadit.</span><span class="sxs-lookup"><span data-stu-id="0206e-242">Look at the list of resource providers, and if necessary, select the **Register** link to register the resource provider of the type you are trying to deploy.</span></span>

   ![Zobrazit seznam poskytovatelů prostředků](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

<span data-ttu-id="0206e-244">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="0206e-244">**PowerShell**</span></span>

<span data-ttu-id="0206e-245">Pokud chcete zobrazit stav vaší registrace, použijte **Get-AzureRmResourceProvider**.</span><span class="sxs-lookup"><span data-stu-id="0206e-245">To see your registration status, use **Get-AzureRmResourceProvider**.</span></span>

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

<span data-ttu-id="0206e-246">Chcete-li zaregistrovat zprostředkovatele, použijte **Register-AzureRmResourceProvider** a zadejte název zprostředkovatele prostředků, které chcete zaregistrovat.</span><span class="sxs-lookup"><span data-stu-id="0206e-246">To register a provider, use **Register-AzureRmResourceProvider** and provide the name of the resource provider you wish to register.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

<span data-ttu-id="0206e-247">Chcete-li získat podporovaná umístění pro konkrétní typ prostředku, použijte:</span><span class="sxs-lookup"><span data-stu-id="0206e-247">To get the supported locations for a particular type of resource, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

<span data-ttu-id="0206e-248">Podporované verze rozhraní API pro konkrétní typ prostředku, použijte:</span><span class="sxs-lookup"><span data-stu-id="0206e-248">To get the supported API versions for a particular type of resource, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

<span data-ttu-id="0206e-249">**Azure CLI**</span><span class="sxs-lookup"><span data-stu-id="0206e-249">**Azure CLI**</span></span>

<span data-ttu-id="0206e-250">Chcete-li zjistit, zda zprostředkovatel registroval, použijte `azure provider list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0206e-250">To see whether the provider is registered, use the `azure provider list` command.</span></span>

```azurecli
az provider list
```

<span data-ttu-id="0206e-251">Registrace poskytovatele prostředků, použijte `azure provider register` příkaz a zadejte *obor názvů* k registraci.</span><span class="sxs-lookup"><span data-stu-id="0206e-251">To register a resource provider, use the `azure provider register` command, and specify the *namespace* to register.</span></span>

```azurecli
az provider register --namespace Microsoft.Cdn
```

<span data-ttu-id="0206e-252">Pokud chcete zobrazit podporovaná umístění a verze rozhraní API pro typ prostředku, použijte:</span><span class="sxs-lookup"><span data-stu-id="0206e-252">To see the supported locations and API versions for a resource type, use:</span></span>

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

<a id="quotaexceeded" />

## <a name="quotaexceeded-and-operationnotallowed"></a><span data-ttu-id="0206e-253">QuotaExceeded a OperationNotAllowed</span><span class="sxs-lookup"><span data-stu-id="0206e-253">QuotaExceeded and OperationNotAllowed</span></span>
<span data-ttu-id="0206e-254">Pokud nasazení překročí kvótu, která může být na skupinu prostředků, odběry, účty a další obory, může mít problémy.</span><span class="sxs-lookup"><span data-stu-id="0206e-254">You might have issues when deployment exceeds a quota, which could be per resource group, subscriptions, accounts, and other scopes.</span></span> <span data-ttu-id="0206e-255">Omezit počet jader pro oblast lze nakonfigurovat například vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="0206e-255">For example, your subscription may be configured to limit the number of cores for a region.</span></span> <span data-ttu-id="0206e-256">Pokud se pokusíte nasazení virtuálního počítače s více jader, než povolené množství, obdržíte chybu oznamující, že byla překročena kvóta.</span><span class="sxs-lookup"><span data-stu-id="0206e-256">If you attempt to deploy a virtual machine with more cores than the permitted amount, you receive an error stating the quota has been exceeded.</span></span>
<span data-ttu-id="0206e-257">Dokončení kvóty informace najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="0206e-257">For complete quota information, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="0206e-258">Chcete-li prozkoumat vaše předplatné kvóty pro počet jader, můžete použít `azure vm list-usage` v Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="0206e-258">To examine your subscription's quotas for cores, you can use the `azure vm list-usage` command in the Azure CLI.</span></span> <span data-ttu-id="0206e-259">Následující příklad ukazuje, že základní kvóta pro bezplatný zkušební účet je 4:</span><span class="sxs-lookup"><span data-stu-id="0206e-259">The following example illustrates that the core quota for a free trial account is 4:</span></span>

```azurecli
az vm list-usage --location "South Central US"
```

<span data-ttu-id="0206e-260">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="0206e-260">Which returns:</span></span>

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

<span data-ttu-id="0206e-261">Pokud nasadíte šablonu, která vytvoří více než čtyři jader v oblasti západní USA, dojde k chybě nasazení, bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="0206e-261">If you deploy a template that creates more than four cores in the West US region, you get a deployment error that looks like:</span></span>

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

<span data-ttu-id="0206e-262">Nebo v prostředí PowerShell, můžete použít **Get-AzureRmVMUsage** rutiny.</span><span class="sxs-lookup"><span data-stu-id="0206e-262">Or in PowerShell, you can use the **Get-AzureRmVMUsage** cmdlet.</span></span>

```powershell
Get-AzureRmVMUsage
```

<span data-ttu-id="0206e-263">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="0206e-263">Which returns:</span></span>

```powershell
...
CurrentValue : 0
Limit        : 4
Name         : {
                 "value": "cores",
                 "localizedValue": "Total Regional Cores"
               }
Unit         : null
...
```

<span data-ttu-id="0206e-264">V těchto případech by měl přejděte na portál a souborů podpory problém ke zvýšení vaší kvóty pro oblast, do které chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="0206e-264">In these cases, you should go to the portal and file a support issue to raise your quota for the region into which you want to deploy.</span></span>

> [!NOTE]
> <span data-ttu-id="0206e-265">Nezapomeňte, že pro skupiny prostředků, se kvóty pro každou oblast jednotlivých, ne pro celé předplatné.</span><span class="sxs-lookup"><span data-stu-id="0206e-265">Remember that for resource groups, the quota is for each individual region, not for the entire subscription.</span></span> <span data-ttu-id="0206e-266">Pokud potřebujete nasadit 30 jader v západní USA, budete muset požádat o 30 Resource Manager jader v západní USA.</span><span class="sxs-lookup"><span data-stu-id="0206e-266">If you need to deploy 30 cores in West US, you have to ask for 30 Resource Manager cores in West US.</span></span> <span data-ttu-id="0206e-267">Pokud potřebujete nasadit 30 jader v některém z oblasti, ke které máte přístup, je třeba požádat o 30 jader Resource Manager ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="0206e-267">If you need to deploy 30 cores in any of the regions to which you have access, you should ask for 30 Resource Manager cores in all regions.</span></span>
>
>

## <a name="invalidcontentlink"></a><span data-ttu-id="0206e-268">InvalidContentLink</span><span class="sxs-lookup"><span data-stu-id="0206e-268">InvalidContentLink</span></span>
<span data-ttu-id="0206e-269">Když zobrazí chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="0206e-269">When you receive the error message:</span></span>

```
Code=InvalidContentLink
Message=Unable to download deployment content from ...
```

<span data-ttu-id="0206e-270">Pokusili jste s největší pravděpodobností propojení vnořené šablony, která není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0206e-270">You have most likely attempted to link to a nested template that is not available.</span></span> <span data-ttu-id="0206e-271">Překontrolujte identifikátor URI, který jste zadali pro vnořené šablonu.</span><span class="sxs-lookup"><span data-stu-id="0206e-271">Double check the URI you provided for the nested template.</span></span> <span data-ttu-id="0206e-272">Pokud šablona již existuje v účtu úložiště, zkontrolujte, zda že identifikátor URI je přístupný.</span><span class="sxs-lookup"><span data-stu-id="0206e-272">If the template exists in a storage account, make sure the URI is accessible.</span></span> <span data-ttu-id="0206e-273">Potřebujete předat SAS token.</span><span class="sxs-lookup"><span data-stu-id="0206e-273">You may need to pass a SAS token.</span></span> <span data-ttu-id="0206e-274">Další informace najdete v tématu věnovaném [použití propojených šablon s Azure Resource Managerem](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="0206e-274">For more information, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="requestdisallowedbypolicy"></a><span data-ttu-id="0206e-275">RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="0206e-275">RequestDisallowedByPolicy</span></span>
<span data-ttu-id="0206e-276">Tato chyba se zobrazí, když vaše předplatné zahrnuje zásady prostředků, které brání akce, které se pokoušíte provést během nasazení.</span><span class="sxs-lookup"><span data-stu-id="0206e-276">You receive this error when your subscription includes a resource policy that prevents an action you are trying to perform during deployment.</span></span> <span data-ttu-id="0206e-277">V chybové zprávě vyhledejte identifikátor zásad.</span><span class="sxs-lookup"><span data-stu-id="0206e-277">In the error message, look for the policy identifier.</span></span>

```
Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'
```

<span data-ttu-id="0206e-278">V **prostředí PowerShell**, poskytovat identifikátor zásady jako **Id** parametr načíst podrobnosti o zásady, které blokované vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="0206e-278">In **PowerShell**, provide that policy identifier as the **Id** parameter to retrieve details about the policy that blocked your deployment.</span></span>

```powershell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

<span data-ttu-id="0206e-279">V **rozhraní příkazového řádku Azure**, zadejte název definice zásady:</span><span class="sxs-lookup"><span data-stu-id="0206e-279">In **Azure CLI**, provide the name of the policy definition:</span></span>

```azurecli
az policy definition show --name regionPolicyAssignment
```

<span data-ttu-id="0206e-280">Další informace najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="0206e-280">For more information, see the following articles:</span></span>

- [<span data-ttu-id="0206e-281">Chyba RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="0206e-281">RequestDisallowedByPolicy error</span></span>](resource-manager-policy-requestdisallowedbypolicy-error.md)
- <span data-ttu-id="0206e-282">[Použití zásad ke správě prostředků a řízení přístupu](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="0206e-282">[Use Policy to manage resources and control access](resource-manager-policy.md).</span></span>

## <a name="authorization-failed"></a><span data-ttu-id="0206e-283">Ověření se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="0206e-283">Authorization failed</span></span>
<span data-ttu-id="0206e-284">Může zobrazit chybová během nasazení, protože účet nebo instanční objekt pokusu o nasazení prostředků nemá přístup k provedení těchto akcí.</span><span class="sxs-lookup"><span data-stu-id="0206e-284">You may receive an error during deployment because the account or service principal attempting to deploy the resources does not have access to perform those actions.</span></span> <span data-ttu-id="0206e-285">Azure Active Directory, které vám umožní nebo správce pro řízení identit, které můžete přístup prostředky s vysoký stupeň přesnosti.</span><span class="sxs-lookup"><span data-stu-id="0206e-285">Azure Active Directory enables you or your administrator to control which identities can access what resources with a great degree of precision.</span></span> <span data-ttu-id="0206e-286">Například pokud váš účet je přiřazen k roli čtečky, nejste schopni vytvořit prostředky.</span><span class="sxs-lookup"><span data-stu-id="0206e-286">For example, if your account is assigned to the Reader role, you are not able to create resources.</span></span> <span data-ttu-id="0206e-287">V takovém případě zobrazí chybová zpráva oznamující, že autorizace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="0206e-287">In that case, you see an error message indicating that authorization failed.</span></span>

<span data-ttu-id="0206e-288">Další informace o řízení přístupu na základě rolí najdete v tématu [řízení přístupu](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="0206e-288">For more information about role-based access control, see [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="0206e-289">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0206e-289">Next steps</span></span>
* <span data-ttu-id="0206e-290">Další informace o auditování akce najdete v tématu [auditovat operace s Resource Managerem](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="0206e-290">To learn about auditing actions, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="0206e-291">Další informace o akce k určení chyby během nasazení najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="0206e-291">To learn about actions to determine the errors during deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
