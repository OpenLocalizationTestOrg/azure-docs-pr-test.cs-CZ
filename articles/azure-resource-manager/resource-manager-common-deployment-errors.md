---
title: "běžné chyby nasazení Azure aaaTroubleshoot | Microsoft Docs"
description: "Popisuje, jak tooresolve běžných chyb při nasazování tooAzure prostředků pomocí Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Chyba nasazení, nasazení azure nasazení tooazure"
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: tomfitz
ms.openlocfilehash: 8571e9941879eb5586e4258a785b6a09247da771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a><span data-ttu-id="4fad8-104">Odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="4fad8-104">Troubleshoot common Azure deployment errors with Azure Resource Manager</span></span>
<span data-ttu-id="4fad8-105">Toto téma popisuje, jak můžete vyřešit některé běžné chyby nasazení Azure se můžete setkat.</span><span class="sxs-lookup"><span data-stu-id="4fad8-105">This topic describes how you can resolve some common Azure deployment errors you may encounter.</span></span>

<span data-ttu-id="4fad8-106">v tomto tématu jsou popsány Hello následující kódy chyb:</span><span class="sxs-lookup"><span data-stu-id="4fad8-106">hello following error codes are described in this topic:</span></span>

* [<span data-ttu-id="4fad8-107">AccountNameInvalid</span><span class="sxs-lookup"><span data-stu-id="4fad8-107">AccountNameInvalid</span></span>](#accountnameinvalid)
* [<span data-ttu-id="4fad8-108">Ověření se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="4fad8-108">Authorization failed</span></span>](#authorization-failed)
* [<span data-ttu-id="4fad8-109">Struktura BadRequest</span><span class="sxs-lookup"><span data-stu-id="4fad8-109">BadRequest</span></span>](#badrequest)
* [<span data-ttu-id="4fad8-110">DeploymentFailed</span><span class="sxs-lookup"><span data-stu-id="4fad8-110">DeploymentFailed</span></span>](#deploymentfailed)
* [<span data-ttu-id="4fad8-111">DisallowedOperation</span><span class="sxs-lookup"><span data-stu-id="4fad8-111">DisallowedOperation</span></span>](#disallowedoperation)
* [<span data-ttu-id="4fad8-112">InvalidContentLink</span><span class="sxs-lookup"><span data-stu-id="4fad8-112">InvalidContentLink</span></span>](#invalidcontentlink)
* [<span data-ttu-id="4fad8-113">InvalidTemplate</span><span class="sxs-lookup"><span data-stu-id="4fad8-113">InvalidTemplate</span></span>](#invalidtemplate)
* [<span data-ttu-id="4fad8-114">MissingSubscriptionRegistration</span><span class="sxs-lookup"><span data-stu-id="4fad8-114">MissingSubscriptionRegistration</span></span>](#noregisteredproviderfound)
* [<span data-ttu-id="4fad8-115">NotFound</span><span class="sxs-lookup"><span data-stu-id="4fad8-115">NotFound</span></span>](#notfound)
* [<span data-ttu-id="4fad8-116">NoRegisteredProviderFound</span><span class="sxs-lookup"><span data-stu-id="4fad8-116">NoRegisteredProviderFound</span></span>](#noregisteredproviderfound)
* [<span data-ttu-id="4fad8-117">OperationNotAllowed</span><span class="sxs-lookup"><span data-stu-id="4fad8-117">OperationNotAllowed</span></span>](#quotaexceeded)
* [<span data-ttu-id="4fad8-118">ParentResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="4fad8-118">ParentResourceNotFound</span></span>](#parentresourcenotfound)
* [<span data-ttu-id="4fad8-119">QuotaExceeded</span><span class="sxs-lookup"><span data-stu-id="4fad8-119">QuotaExceeded</span></span>](#quotaexceeded)
* [<span data-ttu-id="4fad8-120">RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="4fad8-120">RequestDisallowedByPolicy</span></span>](#requestdisallowedbypolicy)
* [<span data-ttu-id="4fad8-121">ResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="4fad8-121">ResourceNotFound</span></span>](#notfound)
* [<span data-ttu-id="4fad8-122">SkuNotAvailable</span><span class="sxs-lookup"><span data-stu-id="4fad8-122">SkuNotAvailable</span></span>](#skunotavailable)
* [<span data-ttu-id="4fad8-123">StorageAccountAlreadyExists</span><span class="sxs-lookup"><span data-stu-id="4fad8-123">StorageAccountAlreadyExists</span></span>](#storagenamenotunique)
* [<span data-ttu-id="4fad8-124">StorageAccountAlreadyTaken</span><span class="sxs-lookup"><span data-stu-id="4fad8-124">StorageAccountAlreadyTaken</span></span>](#storagenamenotunique)

## <a name="deploymentfailed"></a><span data-ttu-id="4fad8-125">DeploymentFailed</span><span class="sxs-lookup"><span data-stu-id="4fad8-125">DeploymentFailed</span></span>

<span data-ttu-id="4fad8-126">Tento kód chyby označuje Chyba obecné nasazení, ale není potřebujete toostart řešení potíží s kódem chyby hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-126">This error code indicates a general deployment error, but it is not hello error code you need toostart troubleshooting.</span></span> <span data-ttu-id="4fad8-127">Kód chyby Hello, která ve skutečnosti pomáhá hello problém vyřešit, je obvykle o jednu úroveň pod tuto chybu.</span><span class="sxs-lookup"><span data-stu-id="4fad8-127">hello error code that actually helps you resolve hello issue is usually one level below this error.</span></span> <span data-ttu-id="4fad8-128">Například hello následující obrázek ukazuje hello **RequestDisallowedByPolicy** kód chyby, který je pod hello chyba nasazení.</span><span class="sxs-lookup"><span data-stu-id="4fad8-128">For example, hello following image shows hello **RequestDisallowedByPolicy** error code that is under hello deployment error.</span></span>

![Zobrazí kód chyby](./media/resource-manager-common-deployment-errors/error-code.png)

## <a name="skunotavailable"></a><span data-ttu-id="4fad8-130">SkuNotAvailable</span><span class="sxs-lookup"><span data-stu-id="4fad8-130">SkuNotAvailable</span></span>

<span data-ttu-id="4fad8-131">Při nasazování prostředku (obvykle virtuální počítač), může se zobrazit následující chyba kódu a chybové zprávy hello:</span><span class="sxs-lookup"><span data-stu-id="4fad8-131">When deploying a resource (typically a virtual machine), you may receive hello following error code and error message:</span></span>

```
Code: SkuNotAvailable
Message: hello requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy tooa different location.
```

<span data-ttu-id="4fad8-132">Tato chyba se zobrazí, když prostředek hello SKU, které jste vybrali (například velikost virtuálního počítače) není k dispozici pro umístění hello, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="4fad8-132">You receive this error when hello resource SKU you have selected (such as VM size) is not available for hello location you have selected.</span></span> <span data-ttu-id="4fad8-133">tooresolve tento problém, je nutné toodetermine, které SKU jsou k dispozici v oblasti.</span><span class="sxs-lookup"><span data-stu-id="4fad8-133">tooresolve this issue, you need toodetermine which SKUs are available in a region.</span></span> <span data-ttu-id="4fad8-134">Můžete použít prostředí PowerShell, hello portálu nebo toofind operace REST dostupné edice.</span><span class="sxs-lookup"><span data-stu-id="4fad8-134">You can use PowerShell, hello portal, or a REST operation toofind available SKUs.</span></span>

- <span data-ttu-id="4fad8-135">Pro prostředí PowerShell, použijte [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) a filtrovat podle umístění.</span><span class="sxs-lookup"><span data-stu-id="4fad8-135">For PowerShell, use [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) and filter by location.</span></span> <span data-ttu-id="4fad8-136">Musíte mít hello nejnovější verzi prostředí PowerShell pro tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="4fad8-136">You must have hello latest version of PowerShell for this command.</span></span>

  ```powershell
  Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
  ```

  <span data-ttu-id="4fad8-137">Hello výsledky obsahovat seznam SKU pro hello umístění a nějaká omezení pro tento SKU.</span><span class="sxs-lookup"><span data-stu-id="4fad8-137">hello results include a list of SKUs for hello location and any restrictions for that SKU.</span></span>

  ```powershell
  ResourceType                Name      Locations Restriction                      Capability Value
  ------------                ----      --------- -----------                      ---------- -----
  availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
  availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
  virtualMachines      Standard_A0 southcentralus
  virtualMachines      Standard_A1 southcentralus
  virtualMachines      Standard_A2 southcentralus
  ```

- <span data-ttu-id="4fad8-138">toouse hello [portál](https://portal.azure.com), přihlaste se toohello portál a přidat prostředek prostřednictvím rozhraní hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-138">toouse hello [portal](https://portal.azure.com), log in toohello portal and add a resource through hello interface.</span></span> <span data-ttu-id="4fad8-139">Můžete nastavit hello hodnoty, jako vidíte hello k dispozici SKU pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="4fad8-139">As you set hello values, you see hello available SKUs for that resource.</span></span> <span data-ttu-id="4fad8-140">Není nutné toocomplete hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="4fad8-140">You do not need toocomplete hello deployment.</span></span>

    ![dostupné edice](./media/resource-manager-common-deployment-errors/view-sku.png)

- <span data-ttu-id="4fad8-142">toouse hello REST API pro virtuální počítače, odesílat hello následující požadavek:</span><span class="sxs-lookup"><span data-stu-id="4fad8-142">toouse hello REST API for virtual machines, send hello following request:</span></span>

  ```HTTP 
  GET
  https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
  ```

  <span data-ttu-id="4fad8-143">Vrátí k dispozici SKU a oblastí, ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="4fad8-143">It returns available SKUs and regions in hello following format:</span></span>

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

<span data-ttu-id="4fad8-144">Pokud jste nelze toofind vhodný SKU v této oblasti nebo alternativní oblast, která vyhovuje vašim obchodním potřebám, odeslání [SKU požadavek](https://aka.ms/skurestriction) tooAzure podpory.</span><span class="sxs-lookup"><span data-stu-id="4fad8-144">If you are unable toofind a suitable SKU in that region or an alternative region that meets your business needs, submit a [SKU request](https://aka.ms/skurestriction) tooAzure Support.</span></span>

## <a name="disallowedoperation"></a><span data-ttu-id="4fad8-145">DisallowedOperation</span><span class="sxs-lookup"><span data-stu-id="4fad8-145">DisallowedOperation</span></span>

```
Code: DisallowedOperation
Message: hello current subscription type is not permitted tooperform operations on any provider 
namespace. Please use a different subscription.
```

<span data-ttu-id="4fad8-146">Pokud se zobrazí tato chyba, kterou používáte odběr, který není povolen tooaccess služby Azure než Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4fad8-146">If you receive this error, you are using a subscription that is not permitted tooaccess any Azure services other than Azure Active Directory.</span></span> <span data-ttu-id="4fad8-147">Pokud potřebujete tooaccess hello klasický portál ale nejsou povoleny toodeploy prostředků můžete mít tento typ předplatného.</span><span class="sxs-lookup"><span data-stu-id="4fad8-147">You might have this type of subscription when you need tooaccess hello classic portal but are not permitted toodeploy resources.</span></span> <span data-ttu-id="4fad8-148">tooresolve tento problém, je nutné použít odběr, který má oprávnění toodeploy prostředky.</span><span class="sxs-lookup"><span data-stu-id="4fad8-148">tooresolve this issue, you must use a subscription that has permission toodeploy resources.</span></span>  

<span data-ttu-id="4fad8-149">tooview předplatné k dispozici v prostředí PowerShell, použijte:</span><span class="sxs-lookup"><span data-stu-id="4fad8-149">tooview your available subscriptions with PowerShell, use:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="4fad8-150">A, tooset hello aktuální předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="4fad8-150">And, tooset hello current subscription, use:</span></span>

```powershell
Set-AzureRmContext -SubscriptionName {subscription-name}
```

<span data-ttu-id="4fad8-151">tooview vaše dostupných předplatných s 2.0 rozhraní příkazového řádku Azure, použijte:</span><span class="sxs-lookup"><span data-stu-id="4fad8-151">tooview your available subscriptions with Azure CLI 2.0, use:</span></span>

```azurecli
az account list
```

<span data-ttu-id="4fad8-152">A, tooset hello aktuální předplatné, použijte:</span><span class="sxs-lookup"><span data-stu-id="4fad8-152">And, tooset hello current subscription, use:</span></span>

```azurecli
az account set --subscription {subscription-name}
```

## <a name="invalidtemplate"></a><span data-ttu-id="4fad8-153">InvalidTemplate</span><span class="sxs-lookup"><span data-stu-id="4fad8-153">InvalidTemplate</span></span>
<span data-ttu-id="4fad8-154">Tato chyba může způsobit z několika různých typů chyb.</span><span class="sxs-lookup"><span data-stu-id="4fad8-154">This error can result from several different types of errors.</span></span>

- <span data-ttu-id="4fad8-155">Chyba syntaxe</span><span class="sxs-lookup"><span data-stu-id="4fad8-155">Syntax error</span></span>

   <span data-ttu-id="4fad8-156">Pokud se zobrazí chybovou zprávu, která určuje hello šablony se nezdařilo ověření, bude pravděpodobně problém syntaxe ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="4fad8-156">If you receive an error message that indicates hello template failed validation, you may have a syntax problem in your template.</span></span>

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed
  ```

   <span data-ttu-id="4fad8-157">Tato chyba je snadno toomake, protože může být složité výrazy šablony.</span><span class="sxs-lookup"><span data-stu-id="4fad8-157">This error is easy toomake because template expressions can be intricate.</span></span> <span data-ttu-id="4fad8-158">Například hello následující přiřazení název pro účet úložiště obsahuje jednu sadu závorky, tři funkce, tři sady závorky, jednu sadu jednoduchých uvozovek a jednu vlastnost:</span><span class="sxs-lookup"><span data-stu-id="4fad8-158">For example, hello following name assignment for a storage account contains one set of brackets, three functions, three sets of parentheses, one set of single quotes, and one property:</span></span>

  ```json
  "name": "[concat('storage', uniqueString(resourceGroup().id))]",
  ```

   <span data-ttu-id="4fad8-159">Pokud nezadáte syntaxe porovnávání hello, vytvoří šablona hello hodnotu, která se liší od svůj záměr.</span><span class="sxs-lookup"><span data-stu-id="4fad8-159">If you do not provide hello matching syntax, hello template produces a value that is different than your intention.</span></span>

   <span data-ttu-id="4fad8-160">Po zobrazení tohoto typu chyby, pečlivě zkontrolujte syntaxi výrazu hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-160">When you receive this type of error, carefully review hello expression syntax.</span></span> <span data-ttu-id="4fad8-161">Zvažte použití editor JSON jako [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) nebo [Visual Studio Code](resource-manager-vs-code.md), která vás upozorní chyby syntaxe.</span><span class="sxs-lookup"><span data-stu-id="4fad8-161">Consider using a JSON editor like [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) or [Visual Studio Code](resource-manager-vs-code.md), which can warn you about syntax errors.</span></span>

- <span data-ttu-id="4fad8-162">Segment nesprávné délky</span><span class="sxs-lookup"><span data-stu-id="4fad8-162">Incorrect segment lengths</span></span>

   <span data-ttu-id="4fad8-163">Další neplatný šablony chyba nastane, když hello název prostředku není ve správném formátu hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-163">Another invalid template error occurs when hello resource name is not in hello correct format.</span></span>

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed: 'hello template resource {resource-name}'
  for type {resource-type} has incorrect segment lengths.
  ```

   <span data-ttu-id="4fad8-164">Kořenové úrovně prostředku musí mít jeden menší segmentu v hello název než v hello typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="4fad8-164">A root level resource must have one less segment in hello name than in hello resource type.</span></span> <span data-ttu-id="4fad8-165">Každý segment je rozlišené pomocí lomítko.</span><span class="sxs-lookup"><span data-stu-id="4fad8-165">Each segment is differentiated by a slash.</span></span> <span data-ttu-id="4fad8-166">V následujícím příkladu hello, typ hello má dva segmenty a hello název jednoho segmentu, tak, aby byl **platný název**.</span><span class="sxs-lookup"><span data-stu-id="4fad8-166">In hello following example, hello type has two segments and hello name has one segment, so it is a **valid name**.</span></span>

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "myHostingPlanName",
    ...
  }
  ```

   <span data-ttu-id="4fad8-167">Ale je další příklad hello **není platný název** protože má hello stejný počet segmentů jako typ hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-167">But hello next example is **not a valid name** because it has hello same number of segments as hello type.</span></span>

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "appPlan/myHostingPlanName",
    ...
  }
  ```

   <span data-ttu-id="4fad8-168">Pro podřízené prostředky typu hello a název hello mít stejný počet segmentů.</span><span class="sxs-lookup"><span data-stu-id="4fad8-168">For child resources, hello type and name have hello same number of segments.</span></span> <span data-ttu-id="4fad8-169">Tento počet segmentů dává smysl, protože hello úplný název a typ pro podřízené hello zahrnuje hello nadřazené názvem a typem.</span><span class="sxs-lookup"><span data-stu-id="4fad8-169">This number of segments makes sense because hello full name and type for hello child includes hello parent name and type.</span></span> <span data-ttu-id="4fad8-170">Úplný název hello proto má stále jednoho menší segmentu než úplné typ hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-170">Therefore, hello full name still has one less segment than hello full type.</span></span>

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

   <span data-ttu-id="4fad8-171">Získávání hello segmenty správné může být složité s typy Resource Manager, které se použijí napříč zprostředkovatelé prostředků.</span><span class="sxs-lookup"><span data-stu-id="4fad8-171">Getting hello segments right can be tricky with Resource Manager types that are applied across resource providers.</span></span> <span data-ttu-id="4fad8-172">Například použití webu prostředek zámku tooa vyžaduje typ s čtyři segmenty.</span><span class="sxs-lookup"><span data-stu-id="4fad8-172">For example, applying a resource lock tooa web site requires a type with four segments.</span></span> <span data-ttu-id="4fad8-173">Název hello je proto tři segmenty:</span><span class="sxs-lookup"><span data-stu-id="4fad8-173">Therefore, hello name is three segments:</span></span>

  ```json
  {
      "type": "Microsoft.Web/sites/providers/locks",
      "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
      ...
  }
  ```

- <span data-ttu-id="4fad8-174">Kopírování indexu není očekáván.</span><span class="sxs-lookup"><span data-stu-id="4fad8-174">Copy index is not expected</span></span>

   <span data-ttu-id="4fad8-175">To narazíte **InvalidTemplate** chyba, když jste použili hello **kopie** element tooa součástí hello šablony, která nepodporuje tento element.</span><span class="sxs-lookup"><span data-stu-id="4fad8-175">You encounter this **InvalidTemplate** error when you have applied hello **copy** element tooa part of hello template that does not support this element.</span></span> <span data-ttu-id="4fad8-176">Lze použít pouze typ prostředku tooa element kopie hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-176">You can only apply hello copy element tooa resource type.</span></span> <span data-ttu-id="4fad8-177">Nelze použít vlastnost tooa kopírování v rámci typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="4fad8-177">You cannot apply copy tooa property within a resource type.</span></span> <span data-ttu-id="4fad8-178">Například použijete kopie tooa virtuálního počítače, ale jej nelze použít toohello OS disky pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="4fad8-178">For example, you apply copy tooa virtual machine, but you cannot apply it toohello OS disks for a virtual machine.</span></span> <span data-ttu-id="4fad8-179">V některých případech můžete převést podřízených prostředků tooa nadřazený prostředek toocreate kopírovací smyčkou.</span><span class="sxs-lookup"><span data-stu-id="4fad8-179">In some cases, you can convert a child resource tooa parent resource toocreate a copy loop.</span></span> <span data-ttu-id="4fad8-180">Další informace o používání kopírování najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="4fad8-180">For more information about using copy, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

- <span data-ttu-id="4fad8-181">Parametr není platný</span><span class="sxs-lookup"><span data-stu-id="4fad8-181">Parameter is not valid</span></span>

   <span data-ttu-id="4fad8-182">Pokud šablona hello určuje povolené hodnoty pro parametr, a zadáte hodnotu, která není jedním z těchto hodnot, obdržíte zprávu podobné toohello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="4fad8-182">If hello template specifies permitted values for a parameter, and you provide a value that is not one of those values, you receive a message similar toohello following error:</span></span>

  ```
  Code=InvalidTemplate;
  Message=Deployment template validation failed: 'hello provided value {parameter value}
  for hello template parameter {parameter name} is not valid. hello parameter value is not
  part of hello allowed values
  ``` 

   <span data-ttu-id="4fad8-183">Překontrolujte hello povolené hodnoty v šabloně hello a zadejte jeden během nasazení.</span><span class="sxs-lookup"><span data-stu-id="4fad8-183">Double check hello allowed values in hello template, and provide one during deployment.</span></span>

- <span data-ttu-id="4fad8-184">Zjistila se cyklická závislost</span><span class="sxs-lookup"><span data-stu-id="4fad8-184">Circular dependency detected</span></span>

   <span data-ttu-id="4fad8-185">Tato chyba se zobrazí, když prostředky jsou vzájemně závislé způsobem, který brání hello nasazení spustit.</span><span class="sxs-lookup"><span data-stu-id="4fad8-185">You receive this error when resources depend on each other in a way that prevents hello deployment from starting.</span></span> <span data-ttu-id="4fad8-186">Kombinace vzájemné závislosti díky dvou nebo více prostředků čekat na další prostředky, které jsou také čekání.</span><span class="sxs-lookup"><span data-stu-id="4fad8-186">A combination of interdependencies makes two or more resource wait for other resources that are also waiting.</span></span> <span data-ttu-id="4fad8-187">Například resource1 závisí na resource3, resource2 závisí na resource1 a resource3 závisí na resource2.</span><span class="sxs-lookup"><span data-stu-id="4fad8-187">For example, resource1 depends on resource3, resource2 depends on resource1, and resource3 depends on resource2.</span></span> <span data-ttu-id="4fad8-188">Obvykle můžete tento problém vyřešit odstraněním nepotřebných závislosti.</span><span class="sxs-lookup"><span data-stu-id="4fad8-188">You can usually solve this problem by removing unnecessary dependencies.</span></span> 

<a id="notfound" />
### <a name="notfound-and-resourcenotfound"></a><span data-ttu-id="4fad8-189">NotFound a ResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="4fad8-189">NotFound and ResourceNotFound</span></span>
<span data-ttu-id="4fad8-190">Pokud vaše šablona zahrnuje hello název prostředku, které nelze vyřešit, obdržíte chybu podobně jako:</span><span class="sxs-lookup"><span data-stu-id="4fad8-190">When your template includes hello name of a resource that cannot be resolved, you receive an error similar to:</span></span>

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

<span data-ttu-id="4fad8-191">Pokud se pokoušíte toodeploy hello chybí prostředek v šabloně hello, zkontrolujte, zda je nutné tooadd závislost.</span><span class="sxs-lookup"><span data-stu-id="4fad8-191">If you are attempting toodeploy hello missing resource in hello template, check whether you need tooadd a dependency.</span></span> <span data-ttu-id="4fad8-192">Správce prostředků optimalizuje nasazení tak, že vytvoříte prostředky paralelně, pokud je to možné.</span><span class="sxs-lookup"><span data-stu-id="4fad8-192">Resource Manager optimizes deployment by creating resources in parallel, when possible.</span></span> <span data-ttu-id="4fad8-193">Pokud po jiný prostředek musí být nasazený jeden prostředek, je třeba toouse hello **dependsOn** element ve vaší šablony toocreate a závislost na hello jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="4fad8-193">If one resource must be deployed after another resource, you need toouse hello **dependsOn** element in your template toocreate a dependency on hello other resource.</span></span> <span data-ttu-id="4fad8-194">Například Pokud nasazujete webovou aplikaci, musí existovat hello plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="4fad8-194">For example, when deploying a web app, hello App Service plan must exist.</span></span> <span data-ttu-id="4fad8-195">Pokud jste nezadali, že webová aplikace hello závisí na hello plán služby App Service, Resource Manager vytvoří v hello oba současně.</span><span class="sxs-lookup"><span data-stu-id="4fad8-195">If you have not specified that hello web app depends on hello App Service plan, Resource Manager creates both resources at hello same time.</span></span> <span data-ttu-id="4fad8-196">Můžete zobrazit chyba oznamující tuto hello, které nelze najít prostředek plánu služby App Service, protože neexistuje ještě při pokusu o tooset vlastnost hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fad8-196">You receive an error stating that hello App Service plan resource cannot be found, because it does not exist yet when attempting tooset a property on hello web app.</span></span> <span data-ttu-id="4fad8-197">Tuto chybu můžete zabránit nastavením hello závislostí v hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4fad8-197">You prevent this error by setting hello dependency in hello web app.</span></span>

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

<span data-ttu-id="4fad8-198">Návrhy na řešení potíží s chybami závislostí, najdete v části [Zkontrolujte pořadí nasazení](#check-deployment-sequence).</span><span class="sxs-lookup"><span data-stu-id="4fad8-198">For suggestions on troubleshooting dependency errors, see [Check deployment sequence](#check-deployment-sequence).</span></span>

<span data-ttu-id="4fad8-199">Můžete také zobrazit chyba při hello prostředek existuje v jiné skupině prostředků než hello jeden nasazován na.</span><span class="sxs-lookup"><span data-stu-id="4fad8-199">You also see this error when hello resource exists in a different resource group than hello one being deployed to.</span></span> <span data-ttu-id="4fad8-200">V takovém případě použijte hello [resourceId funkce](resource-group-template-functions-resource.md#resourceid) tooget hello plně kvalifikovaný název prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-200">In that case, use hello [resourceId function](resource-group-template-functions-resource.md#resourceid) tooget hello fully qualified name of hello resource.</span></span>

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

<span data-ttu-id="4fad8-201">Pokud se pokusíte toouse hello [odkaz](resource-group-template-functions-resource.md#reference) nebo [listKeys](resource-group-template-functions-resource.md#listkeys) funkce s prostředky, které nelze vyřešit, obdržíte hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="4fad8-201">If you attempt toouse hello [reference](resource-group-template-functions-resource.md#reference) or [listKeys](resource-group-template-functions-resource.md#listkeys) functions with a resource that cannot be resolved, you receive hello following error:</span></span>

```
Code=ResourceNotFound;
Message=hello Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

<span data-ttu-id="4fad8-202">Vyhledat výraz, který zahrnuje hello **odkaz** funkce.</span><span class="sxs-lookup"><span data-stu-id="4fad8-202">Look for an expression that includes hello **reference** function.</span></span> <span data-ttu-id="4fad8-203">Dvojitý zkontrolujte, zda text hello hodnoty parametrů jsou správné.</span><span class="sxs-lookup"><span data-stu-id="4fad8-203">Double check that hello parameter values are correct.</span></span>

## <a name="parentresourcenotfound"></a><span data-ttu-id="4fad8-204">ParentResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="4fad8-204">ParentResourceNotFound</span></span>

<span data-ttu-id="4fad8-205">Pokud jeden prostředek je prostředek tooanother nadřazené, hello nadřazený prostředek musí existovat před vytvořením hello podřízených prostředků.</span><span class="sxs-lookup"><span data-stu-id="4fad8-205">When one resource is a parent tooanother resource, hello parent resource must exist before creating hello child resource.</span></span> <span data-ttu-id="4fad8-206">Pokud ještě neexistuje, zobrazí hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="4fad8-206">If it does not yet exist, you receive hello following error:</span></span>

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

<span data-ttu-id="4fad8-207">Hello název prostředku podřízené hello obsahuje název nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-207">hello name of hello child resource includes hello parent name.</span></span> <span data-ttu-id="4fad8-208">Například může být definovaná SQL Database jako:</span><span class="sxs-lookup"><span data-stu-id="4fad8-208">For example, a SQL Database might be defined as:</span></span>

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

<span data-ttu-id="4fad8-209">Ale pokud nezadáte závislost na nadřazeném prostředku hello, může získat hello podřízených prostředků nasadit před nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-209">But, if you do not specify a dependency on hello parent resource, hello child resource may get deployed before hello parent.</span></span> <span data-ttu-id="4fad8-210">tooresolve této chyby patří závislost.</span><span class="sxs-lookup"><span data-stu-id="4fad8-210">tooresolve this error, include a dependency.</span></span>

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

<a id="storagenamenotunique" />

## <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a><span data-ttu-id="4fad8-211">StorageAccountAlreadyExists a StorageAccountAlreadyTaken</span><span class="sxs-lookup"><span data-stu-id="4fad8-211">StorageAccountAlreadyExists and StorageAccountAlreadyTaken</span></span>
<span data-ttu-id="4fad8-212">Pro účty úložiště je nutné zadat název pro hello prostředek, který je v rámci Azure jedinečný.</span><span class="sxs-lookup"><span data-stu-id="4fad8-212">For storage accounts, you must provide a name for hello resource that is unique across Azure.</span></span> <span data-ttu-id="4fad8-213">Pokud nezadáte jedinečný název, zobrazí chybu jako:</span><span class="sxs-lookup"><span data-stu-id="4fad8-213">If you do not provide a unique name, you receive an error like:</span></span>

```
Code=StorageAccountAlreadyTaken
Message=hello storage account named mystorage is already taken.
```

<span data-ttu-id="4fad8-214">Můžete vytvořit jedinečný název vaší zásady vytváření názvů s hello výsledek hello zřetězení [uniqueString](resource-group-template-functions-string.md#uniquestring) funkce.</span><span class="sxs-lookup"><span data-stu-id="4fad8-214">You can create a unique name by concatenating your naming convention with hello result of hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function.</span></span>

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

<span data-ttu-id="4fad8-215">Pokud nasadíte účet úložiště s hello stejný název jako existující účet úložiště v rámci vašeho předplatného, ale zadejte jiné umístění, obdržíte chybu, což značí hello účet úložiště už existuje v jiném umístění.</span><span class="sxs-lookup"><span data-stu-id="4fad8-215">If you deploy a storage account with hello same name as an existing storage account in your subscription, but provide a different location, you receive an error indicating hello storage account already exists in a different location.</span></span> <span data-ttu-id="4fad8-216">Odstraňte stávající účet úložiště hello nebo zadat hello stejné umístění jako hello stávající účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="4fad8-216">Either delete hello existing storage account, or provide hello same location as hello existing storage account.</span></span>

## <a name="accountnameinvalid"></a><span data-ttu-id="4fad8-217">AccountNameInvalid</span><span class="sxs-lookup"><span data-stu-id="4fad8-217">AccountNameInvalid</span></span>
<span data-ttu-id="4fad8-218">Zobrazí hello **AccountNameInvalid** Chyba při pokusu o toogive úložiště účet název, který obsahuje zakázané znaky.</span><span class="sxs-lookup"><span data-stu-id="4fad8-218">You see hello **AccountNameInvalid** error when attempting toogive a storage account a name that includes prohibited characters.</span></span> <span data-ttu-id="4fad8-219">Názvy účtů úložiště musí být v rozmezí 3 až 24 znaků a používat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="4fad8-219">Storage account names must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span> <span data-ttu-id="4fad8-220">Hello [uniqueString](resource-group-template-functions-string.md#uniquestring) funkce vrátí 13 znaků.</span><span class="sxs-lookup"><span data-stu-id="4fad8-220">hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function returns 13 characters.</span></span> <span data-ttu-id="4fad8-221">Pokud řetězení předponu toohello **uniqueString** vést, zadejte předponu, která je 11 znaků nebo méně.</span><span class="sxs-lookup"><span data-stu-id="4fad8-221">If you concatenate a prefix toohello **uniqueString** result, provide a prefix that is 11 characters or less.</span></span>

## <a name="badrequest"></a><span data-ttu-id="4fad8-222">Struktura BadRequest</span><span class="sxs-lookup"><span data-stu-id="4fad8-222">BadRequest</span></span>

<span data-ttu-id="4fad8-223">Struktura BadRequest stav se můžete setkat, když zadáte neplatnou hodnotu pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="4fad8-223">You may encounter a BadRequest status when you provide an invalid value for a property.</span></span> <span data-ttu-id="4fad8-224">Například pokud jste zadali nesprávnou hodnotu SKU pro účet úložiště, hello nasazení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4fad8-224">For example, if you provide an incorrect SKU value for a storage account, hello deployment fails.</span></span> <span data-ttu-id="4fad8-225">toodetermine platné hodnoty pro vlastnost, podívejte se na hello [REST API](/rest/api) pro typ prostředku hello nasazujete.</span><span class="sxs-lookup"><span data-stu-id="4fad8-225">toodetermine valid values for property, look at hello [REST API](/rest/api) for hello resource type you are deploying.</span></span>

<a id="noregisteredproviderfound" />

## <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a><span data-ttu-id="4fad8-226">NoRegisteredProviderFound a MissingSubscriptionRegistration</span><span class="sxs-lookup"><span data-stu-id="4fad8-226">NoRegisteredProviderFound and MissingSubscriptionRegistration</span></span>
<span data-ttu-id="4fad8-227">Při nasazování prostředků, může přijímat hello následující kód chyby a zpráva:</span><span class="sxs-lookup"><span data-stu-id="4fad8-227">When deploying resource, you may receive hello following error code and message:</span></span>

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

<span data-ttu-id="4fad8-228">Nebo se může zobrazit podobné zpráva, že:</span><span class="sxs-lookup"><span data-stu-id="4fad8-228">Or, you may receive a similar message that states:</span></span>

```
Code: MissingSubscriptionRegistration
Message: hello subscription is not registered toouse namespace {resource-provider-namespace}
```

<span data-ttu-id="4fad8-229">Zobrazí tyto chyby pro jednu ze tří důvodů:</span><span class="sxs-lookup"><span data-stu-id="4fad8-229">You receive these errors for one of three reasons:</span></span>

1. <span data-ttu-id="4fad8-230">Zprostředkovatel prostředků Hello není zaregistrován pro vaše předplatné</span><span class="sxs-lookup"><span data-stu-id="4fad8-230">hello resource provider has not been registered for your subscription</span></span>
2. <span data-ttu-id="4fad8-231">Verze rozhraní API není podporována pro typ prostředku hello</span><span class="sxs-lookup"><span data-stu-id="4fad8-231">API version not supported for hello resource type</span></span>
3. <span data-ttu-id="4fad8-232">Umístění není podporováno pro typ prostředku hello</span><span class="sxs-lookup"><span data-stu-id="4fad8-232">Location not supported for hello resource type</span></span>

<span data-ttu-id="4fad8-233">Hello chybová zpráva by měla dát návrhy hello podporované umístění a verze rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="4fad8-233">hello error message should give you suggestions for hello supported locations and API versions.</span></span> <span data-ttu-id="4fad8-234">Vaše šablona tooone Dobrý den, můžete změnit navrhované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4fad8-234">You can change your template tooone of hello suggested values.</span></span> <span data-ttu-id="4fad8-235">Většina poskytovatelů automaticky registrovaných v hello portálu nebo hello rozhraní příkazového řádku Azure, kterou používáte, ale ne všechny.</span><span class="sxs-lookup"><span data-stu-id="4fad8-235">Most providers are registered automatically by hello Azure portal or hello command-line interface you are using, but not all.</span></span> <span data-ttu-id="4fad8-236">Pokud jste nepoužili poskytovatele určitému prostředku před, musíte tooregister tohoto zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="4fad8-236">If you have not used a particular resource provider before, you may need tooregister that provider.</span></span> <span data-ttu-id="4fad8-237">Může zjišťovat informace o poskytovatelích prostředků prostřednictvím PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4fad8-237">You can discover more about resource providers through PowerShell or Azure CLI.</span></span>

<span data-ttu-id="4fad8-238">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="4fad8-238">**Portal**</span></span>

<span data-ttu-id="4fad8-239">Můžete zobrazit stav registrace hello a zaregistrovat obor názvů zprostředkovatele prostředků prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-239">You can see hello registration status and register a resource provider namespace through hello portal.</span></span>

1. <span data-ttu-id="4fad8-240">Pro vaše předplatné, vyberte **zprostředkovatelé prostředků**.</span><span class="sxs-lookup"><span data-stu-id="4fad8-240">For your subscription, select **Resource providers**.</span></span>

   ![Vyberte poskytovatele prostředků](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. <span data-ttu-id="4fad8-242">Podívejte se na hello seznam poskytovatelů prostředků a v případě potřeby vyberte hello **zaregistrovat** poskytovatele prostředků hello tooregister propojení typu hello se pokoušíte toodeploy.</span><span class="sxs-lookup"><span data-stu-id="4fad8-242">Look at hello list of resource providers, and if necessary, select hello **Register** link tooregister hello resource provider of hello type you are trying toodeploy.</span></span>

   ![Zobrazit seznam poskytovatelů prostředků](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

<span data-ttu-id="4fad8-244">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="4fad8-244">**PowerShell**</span></span>

<span data-ttu-id="4fad8-245">toosee stav registrace, použijte **Get-AzureRmResourceProvider**.</span><span class="sxs-lookup"><span data-stu-id="4fad8-245">toosee your registration status, use **Get-AzureRmResourceProvider**.</span></span>

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

<span data-ttu-id="4fad8-246">použít tooregister zprostředkovatele, **Register-AzureRmResourceProvider** a zadejte název hello zprostředkovatele prostředků hello chcete tooregister.</span><span class="sxs-lookup"><span data-stu-id="4fad8-246">tooregister a provider, use **Register-AzureRmResourceProvider** and provide hello name of hello resource provider you wish tooregister.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

<span data-ttu-id="4fad8-247">tooget hello podporované umístění pro konkrétní typ prostředku, použijte:</span><span class="sxs-lookup"><span data-stu-id="4fad8-247">tooget hello supported locations for a particular type of resource, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

<span data-ttu-id="4fad8-248">tooget hello podporované verze rozhraní API pro konkrétní typ prostředku, použijte:</span><span class="sxs-lookup"><span data-stu-id="4fad8-248">tooget hello supported API versions for a particular type of resource, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

<span data-ttu-id="4fad8-249">**Azure CLI**</span><span class="sxs-lookup"><span data-stu-id="4fad8-249">**Azure CLI**</span></span>

<span data-ttu-id="4fad8-250">toosee, zda je zaregistrován hello poskytovatele, používat hello `azure provider list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4fad8-250">toosee whether hello provider is registered, use hello `azure provider list` command.</span></span>

```azurecli
az provider list
```

<span data-ttu-id="4fad8-251">tooregister zprostředkovatel prostředků, použijte hello `azure provider register` příkaz a zadejte hello *obor názvů* tooregister.</span><span class="sxs-lookup"><span data-stu-id="4fad8-251">tooregister a resource provider, use hello `azure provider register` command, and specify hello *namespace* tooregister.</span></span>

```azurecli
az provider register --namespace Microsoft.Cdn
```

<span data-ttu-id="4fad8-252">toosee hello podporované umístění a verze rozhraní API pro typ prostředku, použijte:</span><span class="sxs-lookup"><span data-stu-id="4fad8-252">toosee hello supported locations and API versions for a resource type, use:</span></span>

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

<a id="quotaexceeded" />

## <a name="quotaexceeded-and-operationnotallowed"></a><span data-ttu-id="4fad8-253">QuotaExceeded a OperationNotAllowed</span><span class="sxs-lookup"><span data-stu-id="4fad8-253">QuotaExceeded and OperationNotAllowed</span></span>
<span data-ttu-id="4fad8-254">Pokud nasazení překročí kvótu, která může být na skupinu prostředků, odběry, účty a další obory, může mít problémy.</span><span class="sxs-lookup"><span data-stu-id="4fad8-254">You might have issues when deployment exceeds a quota, which could be per resource group, subscriptions, accounts, and other scopes.</span></span> <span data-ttu-id="4fad8-255">Vaše předplatné může být například nakonfigurován toolimit hello počet jader pro oblast.</span><span class="sxs-lookup"><span data-stu-id="4fad8-255">For example, your subscription may be configured toolimit hello number of cores for a region.</span></span> <span data-ttu-id="4fad8-256">Když zkusíte toodeploy virtuální počítač s více jader, než hello povolená výše, obdržíte chybu, která byla překročena kvóta stanovení hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-256">If you attempt toodeploy a virtual machine with more cores than hello permitted amount, you receive an error stating hello quota has been exceeded.</span></span>
<span data-ttu-id="4fad8-257">Dokončení kvóty informace najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="4fad8-257">For complete quota information, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="4fad8-258">tooexamine vaše předplatné kvóty pro počet jader, můžete použít hello `azure vm list-usage` v hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4fad8-258">tooexamine your subscription's quotas for cores, you can use hello `azure vm list-usage` command in hello Azure CLI.</span></span> <span data-ttu-id="4fad8-259">Hello následující příklad znázorňuje tento hello základní kvóta pro 4 je bezplatný zkušební účet:</span><span class="sxs-lookup"><span data-stu-id="4fad8-259">hello following example illustrates that hello core quota for a free trial account is 4:</span></span>

```azurecli
az vm list-usage --location "South Central US"
```

<span data-ttu-id="4fad8-260">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="4fad8-260">Which returns:</span></span>

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

<span data-ttu-id="4fad8-261">Pokud nasadíte šablonu, která vytvoří více než čtyři jader v oblasti západní USA hello, dojde k chybě nasazení, bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="4fad8-261">If you deploy a template that creates more than four cores in hello West US region, you get a deployment error that looks like:</span></span>

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

<span data-ttu-id="4fad8-262">Nebo v prostředí PowerShell, můžete použít hello **Get-AzureRmVMUsage** rutiny.</span><span class="sxs-lookup"><span data-stu-id="4fad8-262">Or in PowerShell, you can use hello **Get-AzureRmVMUsage** cmdlet.</span></span>

```powershell
Get-AzureRmVMUsage
```

<span data-ttu-id="4fad8-263">Která vrací:</span><span class="sxs-lookup"><span data-stu-id="4fad8-263">Which returns:</span></span>

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

<span data-ttu-id="4fad8-264">V těchto případech by měl přejděte toohello portál a souborů podporu problém tooraise vaší kvóty pro hello oblast, do kterého chcete toodeploy.</span><span class="sxs-lookup"><span data-stu-id="4fad8-264">In these cases, you should go toohello portal and file a support issue tooraise your quota for hello region into which you want toodeploy.</span></span>

> [!NOTE]
> <span data-ttu-id="4fad8-265">Mějte na paměti, že pro skupiny prostředků, kvóty hello je pro každou oblast jednotlivých, ne pro celé předplatné hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-265">Remember that for resource groups, hello quota is for each individual region, not for hello entire subscription.</span></span> <span data-ttu-id="4fad8-266">Pokud potřebujete toodeploy 30 jader v západní USA, máte tooask pro 30 Resource Manager jader v západní USA.</span><span class="sxs-lookup"><span data-stu-id="4fad8-266">If you need toodeploy 30 cores in West US, you have tooask for 30 Resource Manager cores in West US.</span></span> <span data-ttu-id="4fad8-267">Pokud potřebujete toodeploy 30 jader v některém z oblasti toowhich hello máte přístup, je třeba požádat o 30 jader Resource Manager ve všech oblastech.</span><span class="sxs-lookup"><span data-stu-id="4fad8-267">If you need toodeploy 30 cores in any of hello regions toowhich you have access, you should ask for 30 Resource Manager cores in all regions.</span></span>
>
>

## <a name="invalidcontentlink"></a><span data-ttu-id="4fad8-268">InvalidContentLink</span><span class="sxs-lookup"><span data-stu-id="4fad8-268">InvalidContentLink</span></span>
<span data-ttu-id="4fad8-269">Když hello chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="4fad8-269">When you receive hello error message:</span></span>

```
Code=InvalidContentLink
Message=Unable toodownload deployment content from ...
```

<span data-ttu-id="4fad8-270">Pokusili jste s největší pravděpodobností tooa toolink vnořené se šablonu, která není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="4fad8-270">You have most likely attempted toolink tooa nested template that is not available.</span></span> <span data-ttu-id="4fad8-271">Překontrolujte hello URI, které jste zadali pro vnořené šablonu hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-271">Double check hello URI you provided for hello nested template.</span></span> <span data-ttu-id="4fad8-272">Pokud šablona hello existuje v účtu úložiště, zajistěte, aby hello identifikátor URI je přístupný.</span><span class="sxs-lookup"><span data-stu-id="4fad8-272">If hello template exists in a storage account, make sure hello URI is accessible.</span></span> <span data-ttu-id="4fad8-273">Může být nutné toopass SAS token.</span><span class="sxs-lookup"><span data-stu-id="4fad8-273">You may need toopass a SAS token.</span></span> <span data-ttu-id="4fad8-274">Další informace najdete v tématu věnovaném [použití propojených šablon s Azure Resource Managerem](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="4fad8-274">For more information, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="requestdisallowedbypolicy"></a><span data-ttu-id="4fad8-275">RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="4fad8-275">RequestDisallowedByPolicy</span></span>
<span data-ttu-id="4fad8-276">Tato chyba se zobrazí, když vaše předplatné zahrnuje zásady prostředků, které brání akce se pokoušíte tooperform během nasazení.</span><span class="sxs-lookup"><span data-stu-id="4fad8-276">You receive this error when your subscription includes a resource policy that prevents an action you are trying tooperform during deployment.</span></span> <span data-ttu-id="4fad8-277">V hello chybová zpráva vyhledejte identifikátor zásady hello.</span><span class="sxs-lookup"><span data-stu-id="4fad8-277">In hello error message, look for hello policy identifier.</span></span>

```
Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'
```

<span data-ttu-id="4fad8-278">V **prostředí PowerShell**, poskytovat identifikátor zásady jako hello **Id** parametr tooretrieve podrobnosti o hello zásady, které blokované vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="4fad8-278">In **PowerShell**, provide that policy identifier as hello **Id** parameter tooretrieve details about hello policy that blocked your deployment.</span></span>

```powershell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

<span data-ttu-id="4fad8-279">V **rozhraní příkazového řádku Azure**, poskytovat hello název definice zásady hello:</span><span class="sxs-lookup"><span data-stu-id="4fad8-279">In **Azure CLI**, provide hello name of hello policy definition:</span></span>

```azurecli
az policy definition show --name regionPolicyAssignment
```

<span data-ttu-id="4fad8-280">Další informace najdete v tématu hello následující články:</span><span class="sxs-lookup"><span data-stu-id="4fad8-280">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="4fad8-281">Chyba RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="4fad8-281">RequestDisallowedByPolicy error</span></span>](resource-manager-policy-requestdisallowedbypolicy-error.md)
- <span data-ttu-id="4fad8-282">[Použijte zásady toomanage prostředků a řízení přístupu](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="4fad8-282">[Use Policy toomanage resources and control access](resource-manager-policy.md).</span></span>

## <a name="authorization-failed"></a><span data-ttu-id="4fad8-283">Ověření se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="4fad8-283">Authorization failed</span></span>
<span data-ttu-id="4fad8-284">Může zobrazit chybová během nasazení, protože hello účet nebo instanční objekt pokus toodeploy hello prostředků nemá přístup k tooperform tyto akce.</span><span class="sxs-lookup"><span data-stu-id="4fad8-284">You may receive an error during deployment because hello account or service principal attempting toodeploy hello resources does not have access tooperform those actions.</span></span> <span data-ttu-id="4fad8-285">Azure Active Directory umožňuje vám nebo vaší toocontrol správce identit, které mají přístup k jaké prostředkům s vysoký stupeň přesnosti.</span><span class="sxs-lookup"><span data-stu-id="4fad8-285">Azure Active Directory enables you or your administrator toocontrol which identities can access what resources with a great degree of precision.</span></span> <span data-ttu-id="4fad8-286">Například pokud váš účet je přiřadit role Čtenář toohello, nemůžete se může toocreate prostředky.</span><span class="sxs-lookup"><span data-stu-id="4fad8-286">For example, if your account is assigned toohello Reader role, you are not able toocreate resources.</span></span> <span data-ttu-id="4fad8-287">V takovém případě zobrazí chybová zpráva oznamující, že autorizace se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="4fad8-287">In that case, you see an error message indicating that authorization failed.</span></span>

<span data-ttu-id="4fad8-288">Další informace o řízení přístupu na základě rolí najdete v tématu [řízení přístupu](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="4fad8-288">For more information about role-based access control, see [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="4fad8-289">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4fad8-289">Next steps</span></span>
* <span data-ttu-id="4fad8-290">toolearn o auditování akce, najdete v části [auditovat operace s Resource Managerem](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="4fad8-290">toolearn about auditing actions, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="4fad8-291">toolearn o akce toodetermine hello chyb během nasazení, najdete v části [zobrazit operace nasazení](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="4fad8-291">toolearn about actions toodetermine hello errors during deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
