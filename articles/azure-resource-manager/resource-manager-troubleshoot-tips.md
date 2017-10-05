---
title: "Pochopení chyby nasazení Azure | Microsoft Docs"
description: "Popisuje, jak Další informace o nasazení Azure chyby."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Chyba nasazení, nasazení azure nasazení do azure"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.openlocfilehash: b67bb30fa259fa08e37e11afec724c8b8c3eb633
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="understand-azure-deployment-errors"></a><span data-ttu-id="bce4b-104">Pochopení chyby nasazení Azure</span><span class="sxs-lookup"><span data-stu-id="bce4b-104">Understand Azure deployment errors</span></span>
<span data-ttu-id="bce4b-105">Toto téma popisuje chyby nasazení a jak můžete zjistit další informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="bce4b-105">This topic describes deployment errors and how you can discover more information about an error.</span></span> <span data-ttu-id="bce4b-106">Řešení pro běžné chyby nasazení najdete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="bce4b-106">For resolutions to common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>

## <a name="two-types-of-errors"></a><span data-ttu-id="bce4b-107">Dva typy chyb</span><span class="sxs-lookup"><span data-stu-id="bce4b-107">Two types of errors</span></span>
<span data-ttu-id="bce4b-108">Existují dva typy chyb, které dostanete:</span><span class="sxs-lookup"><span data-stu-id="bce4b-108">There are two types of errors you can receive:</span></span>

* <span data-ttu-id="bce4b-109">Chyby ověření</span><span class="sxs-lookup"><span data-stu-id="bce4b-109">validation errors</span></span>
* <span data-ttu-id="bce4b-110">Chyby nasazení</span><span class="sxs-lookup"><span data-stu-id="bce4b-110">deployment errors</span></span>

<span data-ttu-id="bce4b-111">Následující obrázek znázorňuje protokol aktivit pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="bce4b-111">The following image shows the activity log for a subscription.</span></span> <span data-ttu-id="bce4b-112">Reprezentuje dvě nasazení.</span><span class="sxs-lookup"><span data-stu-id="bce4b-112">It represents two deployments.</span></span> <span data-ttu-id="bce4b-113">V jedné nasazení, šablony se nezdařilo ověření (**ověřením**) a neproběhla.</span><span class="sxs-lookup"><span data-stu-id="bce4b-113">In one deployment, the template failed validation (**Validate**) and did not proceed.</span></span> <span data-ttu-id="bce4b-114">V druhé nasazení, šablony ověření proběhlo úspěšně, ale při vytváření prostředků (**zápisu nasazení**).</span><span class="sxs-lookup"><span data-stu-id="bce4b-114">In the other deployment, the template passed validation but failed when creating the resources (**Write Deployments**).</span></span> 

![Zobrazí kód chyby](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

<span data-ttu-id="bce4b-116">Chyby ověření jsou vyvolány scénáře, které lze určit před nasazením.</span><span class="sxs-lookup"><span data-stu-id="bce4b-116">Validation errors arise from scenarios that can be determined before deployment.</span></span> <span data-ttu-id="bce4b-117">Obsahují chyby syntaxe v šabloně nebo pokusu o nasazení prostředky, které překročí vaší kvóty předplatného.</span><span class="sxs-lookup"><span data-stu-id="bce4b-117">They include syntax errors in your template, or trying to deploy resources that would exceed your subscription quotas.</span></span> <span data-ttu-id="bce4b-118">Chyby nasazení jsou vyvolány podmínky, které se vyskytují během procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="bce4b-118">Deployment errors arise from conditions that occur during the deployment process.</span></span> <span data-ttu-id="bce4b-119">Patří mezi ně pokusu o přístup k prostředku, který je nasazován paralelně.</span><span class="sxs-lookup"><span data-stu-id="bce4b-119">They include trying to access a resource that is being deployed in parallel.</span></span>

<span data-ttu-id="bce4b-120">Oba typy chyb vrátí kód chyby, který můžete použít k nasazení řešení.</span><span class="sxs-lookup"><span data-stu-id="bce4b-120">Both types of errors return an error code that you use to troubleshoot the deployment.</span></span> <span data-ttu-id="bce4b-121">Oba typy chyb se zobrazí v [protokol aktivit](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="bce4b-121">Both types of errors appear in the [activity log](resource-group-audit.md).</span></span> <span data-ttu-id="bce4b-122">Ale chyb při ověřování se nezobrazí v historii nasazení protože nasazení nikdy spuštěn.</span><span class="sxs-lookup"><span data-stu-id="bce4b-122">However, validation errors do not appear in your deployment history because the deployment never started.</span></span>

## <a name="determine-error-code"></a><span data-ttu-id="bce4b-123">Určete kód chyby</span><span class="sxs-lookup"><span data-stu-id="bce4b-123">Determine error code</span></span>

<span data-ttu-id="bce4b-124">Informace o chybě najdete prohlížením chybovou zprávu a kód chyby.</span><span class="sxs-lookup"><span data-stu-id="bce4b-124">You can learn about an error by looking at the error message and the error code.</span></span> <span data-ttu-id="bce4b-125">[Odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md) článku jsou uvedené řešení v kódu chyby.</span><span class="sxs-lookup"><span data-stu-id="bce4b-125">The [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md) article lists resolutions by error code.</span></span> <span data-ttu-id="bce4b-126">Toto téma ukazuje, jak pomocí portálu Azure ke zjištění kód chyby.</span><span class="sxs-lookup"><span data-stu-id="bce4b-126">This topic shows how to use the Azure portal to discover the error code.</span></span>

### <a name="validation-errors"></a><span data-ttu-id="bce4b-127">Chyby ověření</span><span class="sxs-lookup"><span data-stu-id="bce4b-127">Validation errors</span></span>

<span data-ttu-id="bce4b-128">Pokud nasazujete prostřednictvím portálu, zobrazí chybu ověření po odeslání svoje hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bce4b-128">When deploying through the portal, you see a validation error after submitting your values.</span></span>

![zobrazit chyba portálu ověření](./media/resource-manager-troubleshoot-tips/validation-error.png)

<span data-ttu-id="bce4b-130">Vyberte další podrobnosti najdete v zprávě.</span><span class="sxs-lookup"><span data-stu-id="bce4b-130">Select the message for more details.</span></span> <span data-ttu-id="bce4b-131">Na následujícím obrázku vidíte **InvalidTemplateDeployment** chyba a zprávu, která určuje zásadu blokován nasazení.</span><span class="sxs-lookup"><span data-stu-id="bce4b-131">In the following image, you see an **InvalidTemplateDeployment** error and a message that indicates a policy blocked deployment.</span></span>

![Zobrazit podrobnosti o ověření](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a><span data-ttu-id="bce4b-133">Chyby nasazení</span><span class="sxs-lookup"><span data-stu-id="bce4b-133">Deployment errors</span></span>

<span data-ttu-id="bce4b-134">Při operaci ověřením úspěšně projde, ale během nasazení se nezdaří, zobrazí se tato chyba v oznámení.</span><span class="sxs-lookup"><span data-stu-id="bce4b-134">When the operation passes validation, but fails during deployment, you see the error in the notifications.</span></span> <span data-ttu-id="bce4b-135">Vyberte oznámení.</span><span class="sxs-lookup"><span data-stu-id="bce4b-135">Select the notification.</span></span>

![Chyba oznámení](./media/resource-manager-troubleshoot-tips/notification.png)

<span data-ttu-id="bce4b-137">Zobrazí podrobnosti o tomto nasazení.</span><span class="sxs-lookup"><span data-stu-id="bce4b-137">You see more details about the deployment.</span></span> <span data-ttu-id="bce4b-138">Vyberte možnost Najít další informace o této chybě.</span><span class="sxs-lookup"><span data-stu-id="bce4b-138">Select the option to find more information about the error.</span></span>

![nasazení se nezdařilo.](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

<span data-ttu-id="bce4b-140">Zobrazí chybovou zprávu a kódy chyb.</span><span class="sxs-lookup"><span data-stu-id="bce4b-140">You see the error message and error codes.</span></span> <span data-ttu-id="bce4b-141">Všimněte si, že jsou dva kódy chyb.</span><span class="sxs-lookup"><span data-stu-id="bce4b-141">Notice there are two error codes.</span></span> <span data-ttu-id="bce4b-142">První kód chyby (**DeploymentFailed**) je obecná chyba, která neobsahuje podrobnosti, budete muset vyřešit chyby.</span><span class="sxs-lookup"><span data-stu-id="bce4b-142">The first error code (**DeploymentFailed**) is a general error that does not provide the details you need to solve the error.</span></span> <span data-ttu-id="bce4b-143">Druhý kód chyby (**StorageAccountNotFound**) poskytuje informace potřebné.</span><span class="sxs-lookup"><span data-stu-id="bce4b-143">The second error code (**StorageAccountNotFound**) provides the details you need.</span></span> 

![Podrobnosti o chybě](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a><span data-ttu-id="bce4b-145">Povolit protokolování ladění</span><span class="sxs-lookup"><span data-stu-id="bce4b-145">Enable debug logging</span></span>
<span data-ttu-id="bce4b-146">Někdy potřebovat další informace o požadavku a odpovědi, které chcete zjistit, kde došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="bce4b-146">Sometimes you need more information about the request and response to discover what went wrong.</span></span> <span data-ttu-id="bce4b-147">Pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure, můžete požádat, že další informace o protokolu během nasazení.</span><span class="sxs-lookup"><span data-stu-id="bce4b-147">By using PowerShell or Azure CLI, you can request that additional information is logged during a deployment.</span></span>

- <span data-ttu-id="bce4b-148">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bce4b-148">PowerShell</span></span>

   <span data-ttu-id="bce4b-149">V prostředí PowerShell nastavená **DeploymentDebugLogLevel** parametr ke všem, ResponseContent nebo RequestContent.</span><span class="sxs-lookup"><span data-stu-id="bce4b-149">In PowerShell, set the **DeploymentDebugLogLevel** parameter to All, ResponseContent, or RequestContent.</span></span>

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   <span data-ttu-id="bce4b-150">Zkontrolujte požadavek obsahu pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="bce4b-150">Examine the request content with the following cmdlet:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   <span data-ttu-id="bce4b-151">Nebo obsahu se odpovědi:</span><span class="sxs-lookup"><span data-stu-id="bce4b-151">Or, the response content with:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   <span data-ttu-id="bce4b-152">Tyto informace můžete určit, zda probíhá nesprávně nastavení na hodnotu v šabloně.</span><span class="sxs-lookup"><span data-stu-id="bce4b-152">This information can help you determine whether a value in the template is being incorrectly set.</span></span>

- <span data-ttu-id="bce4b-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bce4b-153">Azure CLI</span></span>

   <span data-ttu-id="bce4b-154">Zkontrolujte operace nasazení pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="bce4b-154">Examine the deployment operations with the following command:</span></span>

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- <span data-ttu-id="bce4b-155">Vnořené šablony</span><span class="sxs-lookup"><span data-stu-id="bce4b-155">Nested template</span></span>

   <span data-ttu-id="bce4b-156">Do protokolu informace o ladění vnořené šablony, pomocí **debugSetting** element.</span><span class="sxs-lookup"><span data-stu-id="bce4b-156">To log debug information for a nested template, use the **debugSetting** element.</span></span>

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```


## <a name="create-a-troubleshooting-template"></a><span data-ttu-id="bce4b-157">Vytvořit šablonu řešení potíží</span><span class="sxs-lookup"><span data-stu-id="bce4b-157">Create a troubleshooting template</span></span>
<span data-ttu-id="bce4b-158">V některých případech je nejjednodušší způsob, jak vyřešit šablony otestovat části.</span><span class="sxs-lookup"><span data-stu-id="bce4b-158">In some cases, the easiest way to troubleshoot your template is to test parts of it.</span></span> <span data-ttu-id="bce4b-159">Můžete vytvořit zjednodušená šablonu, která umožňuje zaměřit se na část, která budete mít dojem, je příčinou chyby.</span><span class="sxs-lookup"><span data-stu-id="bce4b-159">You can create a simplified template that enables you to focus on the part that you believe is causing the error.</span></span> <span data-ttu-id="bce4b-160">Předpokládejme například, že jste obdrželi chybu při odkazování na prostředek.</span><span class="sxs-lookup"><span data-stu-id="bce4b-160">For example, suppose you are receiving an error when referencing a resource.</span></span> <span data-ttu-id="bce4b-161">Místo práci s celou šablony, vytvořte šablonu, která vrátí část, která může být příčinou problému.</span><span class="sxs-lookup"><span data-stu-id="bce4b-161">Rather than dealing with an entire template, create a template that returns the part that may be causing your problem.</span></span> <span data-ttu-id="bce4b-162">Může pomoct určit, zda jsou předávání v správné parametry, pomocí šablony funkce správně, a získávání prostředek očekáváte.</span><span class="sxs-lookup"><span data-stu-id="bce4b-162">It can help you determine whether you are passing in the right parameters, using template functions correctly, and getting the resource you expect.</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

<span data-ttu-id="bce4b-163">Nebo Předpokládejme, že dochází k chybám nasazení, které budete mít dojem, že se vztahují k nesprávné sady závislosti.</span><span class="sxs-lookup"><span data-stu-id="bce4b-163">Or, suppose you are encountering deployment errors that you believe are related to incorrectly set dependencies.</span></span> <span data-ttu-id="bce4b-164">Testování šablony porušením do zjednodušené šablony.</span><span class="sxs-lookup"><span data-stu-id="bce4b-164">Test your template by breaking it into simplified templates.</span></span> <span data-ttu-id="bce4b-165">Nejprve vytvořte šablonu, která nasadí pouze jeden prostředek (např. SQL Server).</span><span class="sxs-lookup"><span data-stu-id="bce4b-165">First, create a template that deploys only a single resource (like a SQL Server).</span></span> <span data-ttu-id="bce4b-166">Pokud jste si jistí, že máte správně definované prostředku, přidejte na prostředek, který na něm závisí (např. SQL Database).</span><span class="sxs-lookup"><span data-stu-id="bce4b-166">When you are sure you have that resource correctly defined, add a resource that depends on it (like a SQL Database).</span></span> <span data-ttu-id="bce4b-167">Až budete mít tyto dva prostředky správně definované, přidejte další závislé prostředky (jako jsou zásady auditu).</span><span class="sxs-lookup"><span data-stu-id="bce4b-167">When you have those two resources correctly defined, add other dependent resources (like auditing policies).</span></span> <span data-ttu-id="bce4b-168">Mezi každou testovací nasazení Odstraňte skupinu prostředků. Ověřte, že je adekvátní testování závislosti.</span><span class="sxs-lookup"><span data-stu-id="bce4b-168">In between each test deployment, delete the resource group to make sure you adequately testing the dependencies.</span></span> 

## <a name="check-deployment-sequence"></a><span data-ttu-id="bce4b-169">Zkontrolujte pořadí nasazení</span><span class="sxs-lookup"><span data-stu-id="bce4b-169">Check deployment sequence</span></span>

<span data-ttu-id="bce4b-170">Mnoho chyb nasazení dojít, když se prostředky nasadí neočekávané pořadí.</span><span class="sxs-lookup"><span data-stu-id="bce4b-170">Many deployment errors happen when resources are deployed in an unexpected sequence.</span></span> <span data-ttu-id="bce4b-171">Tyto chyby jsou vyvolány při závislosti nejsou správně nastavené.</span><span class="sxs-lookup"><span data-stu-id="bce4b-171">These errors arise when dependencies are not correctly set.</span></span> <span data-ttu-id="bce4b-172">Pokud chybí potřebné závislosti, jeden prostředek se pokusí použít hodnotu pro jiný prostředek, ale druhá ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="bce4b-172">When you are missing a needed dependency, one resource attempts to use a value for another resource but the other does not yet exist.</span></span> <span data-ttu-id="bce4b-173">Zobrazí chyba oznamující, že prostředek nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="bce4b-173">You get an error stating that a resource is not found.</span></span> <span data-ttu-id="bce4b-174">Setkat tohoto typu chyby občas může lišit v době nasazení pro každý zdroj.</span><span class="sxs-lookup"><span data-stu-id="bce4b-174">You may encounter this type of error intermittently because the deployment time for each resource can vary.</span></span> <span data-ttu-id="bce4b-175">Například první pokus o nasazení vašich prostředků úspěšné, protože požadovaný prostředek náhodně dokončí v čase.</span><span class="sxs-lookup"><span data-stu-id="bce4b-175">For example, your first attempt to deploy your resources succeeds because a required resource randomly completes in time.</span></span> <span data-ttu-id="bce4b-176">Však druhý pokus selže, protože požadovaný prostředek se nedokončila v čase.</span><span class="sxs-lookup"><span data-stu-id="bce4b-176">However, your second attempt fails because the required resource did not complete in time.</span></span> 

<span data-ttu-id="bce4b-177">Ale budete chtít vyhnout nastavení závislosti, které nejsou potřebné.</span><span class="sxs-lookup"><span data-stu-id="bce4b-177">But, you want to avoid setting dependencies that are not needed.</span></span> <span data-ttu-id="bce4b-178">Až budete mít nepotřebné závislosti, můžete prodloužit doba trvání nasazení tím, že prostředky, které nejsou na sobě navzájem závislé z nasazuje paralelně.</span><span class="sxs-lookup"><span data-stu-id="bce4b-178">When you have unnecessary dependencies, you prolong the duration of the deployment by preventing resources that are not dependent on each other from being deployed in parallel.</span></span> <span data-ttu-id="bce4b-179">Kromě toho můžete vytvořit cyklické závislosti, které blokují nasazení.</span><span class="sxs-lookup"><span data-stu-id="bce4b-179">In addition, you may create circular dependencies that block the deployment.</span></span> <span data-ttu-id="bce4b-180">[Odkaz](resource-group-template-functions-resource.md#reference) funkce vytvoří implicitní závislostí v odkazovaném prostředku, když tento prostředek je nasazen do stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="bce4b-180">The [reference](resource-group-template-functions-resource.md#reference) function creates an implicit dependency on the referenced resource, when that resource is deployed in the same template.</span></span> <span data-ttu-id="bce4b-181">Proto můžete mít další závislosti, než je určeno závislosti **dependsOn** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bce4b-181">Therefore, you may have more dependencies than the dependencies specified in the **dependsOn** property.</span></span> <span data-ttu-id="bce4b-182">[ResourceId](resource-group-template-functions-resource.md#resourceid) funkce vytvořit implicitní závislostí nebo ověřit, zda prostředek existuje.</span><span class="sxs-lookup"><span data-stu-id="bce4b-182">The [resourceId](resource-group-template-functions-resource.md#resourceid) function does not create an implicit dependency or validate that the resource exists.</span></span>

<span data-ttu-id="bce4b-183">Pokud narazíte na potíže závislostí, budete muset získat přehled o pořadí nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="bce4b-183">When you encounter dependency problems, you need to gain insight into the order of resource deployment.</span></span> <span data-ttu-id="bce4b-184">Chcete-li zobrazit pořadí operace nasazení:</span><span class="sxs-lookup"><span data-stu-id="bce4b-184">To view the order of deployment operations:</span></span>

1. <span data-ttu-id="bce4b-185">Vyberte historii nasazení skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="bce4b-185">Select the deployment history for your resource group.</span></span>

   ![Vyberte historii nasazení](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. <span data-ttu-id="bce4b-187">Vyberte nasazení z historie a vyberte **události**.</span><span class="sxs-lookup"><span data-stu-id="bce4b-187">Select a deployment from the history, and select **Events**.</span></span>

   ![Vybrat nasazení události](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. <span data-ttu-id="bce4b-189">Zkontrolujte posloupnost událostí pro každý zdroj.</span><span class="sxs-lookup"><span data-stu-id="bce4b-189">Examine the sequence of events for each resource.</span></span> <span data-ttu-id="bce4b-190">Věnujte pozornost stav jednotlivých operací.</span><span class="sxs-lookup"><span data-stu-id="bce4b-190">Pay attention to the status of each operation.</span></span> <span data-ttu-id="bce4b-191">Například následující obrázek ukazuje tři účty úložiště, které nasadí paralelně.</span><span class="sxs-lookup"><span data-stu-id="bce4b-191">For example, the following image shows three storage accounts that deployed in parallel.</span></span> <span data-ttu-id="bce4b-192">Všimněte si, že tři účty úložiště jsou spuštěné ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="bce4b-192">Notice that the three storage accounts are started at the same time.</span></span>

   ![Paralelní nasazení](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   <span data-ttu-id="bce4b-194">Další obrázek ukazuje tři účty úložiště, které nejsou nasazené paralelně.</span><span class="sxs-lookup"><span data-stu-id="bce4b-194">The next image shows three storage accounts that are not deployed in parallel.</span></span> <span data-ttu-id="bce4b-195">Druhý účtu úložiště závisí na první účet úložiště, a třetí účet úložiště na druhý účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="bce4b-195">The second storage account depends on the first storage account, and the third storage account depends on the second storage account.</span></span> <span data-ttu-id="bce4b-196">Proto první účet úložiště spuštěna, přijat a dokončit před zahájením na další.</span><span class="sxs-lookup"><span data-stu-id="bce4b-196">Therefore, the first storage account is started, accepted, and completed before the next is started.</span></span>

   ![sekvenční nasazení](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

<span data-ttu-id="bce4b-198">I pro složitější scénáře můžete použít stejný postup na zjišťování, když je nasazení spuštěno a dokončeno pro každý zdroj.</span><span class="sxs-lookup"><span data-stu-id="bce4b-198">Even for more complicated scenarios, you can use the same technique to discover when deployment is started and completed for each resource.</span></span> <span data-ttu-id="bce4b-199">Projděte nasazení událostí zobrazíte, pokud je pořadí je jiné než byste očekávali.</span><span class="sxs-lookup"><span data-stu-id="bce4b-199">Look through your deployment events to see if the sequence is different than you would expect.</span></span> <span data-ttu-id="bce4b-200">Pokud ano, přehodnocovat závislosti pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="bce4b-200">If so, reevaluate the dependencies for this resource.</span></span>

<span data-ttu-id="bce4b-201">Správce prostředků identifikuje cyklické závislosti během ověřování šablony.</span><span class="sxs-lookup"><span data-stu-id="bce4b-201">Resource Manager identifies circular dependencies during template validation.</span></span> <span data-ttu-id="bce4b-202">Vrátí, zda existuje chybovou zprávu, která konkrétně stavy cyklickou závislost.</span><span class="sxs-lookup"><span data-stu-id="bce4b-202">It returns an error message that specifically states a circular dependency exists.</span></span> <span data-ttu-id="bce4b-203">K vyřešení cyklická závislost:</span><span class="sxs-lookup"><span data-stu-id="bce4b-203">To solve a circular dependency:</span></span>

1. <span data-ttu-id="bce4b-204">V šabloně najdete prostředek určený v cyklická závislost.</span><span class="sxs-lookup"><span data-stu-id="bce4b-204">In your template, find the resource identified in the circular dependency.</span></span> 
2. <span data-ttu-id="bce4b-205">Pro tento prostředek, zkontrolujte **dependsOn** vlastnost a jakékoli použití **odkaz** funkce na prostředcích, které závisí na.</span><span class="sxs-lookup"><span data-stu-id="bce4b-205">For that resource, examine the **dependsOn** property and any uses of the **reference** function to see which resources it depends on.</span></span> 
3. <span data-ttu-id="bce4b-206">Zkontrolujte na prostředcích, které jsou závislé na tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="bce4b-206">Examine those resources to see which resources they depend on.</span></span> <span data-ttu-id="bce4b-207">Postupujte podle závislosti, dokud si všimnete na prostředek, který závisí na původní prostředků.</span><span class="sxs-lookup"><span data-stu-id="bce4b-207">Follow the dependencies until you notice a resource that depends on the original resource.</span></span>
5. <span data-ttu-id="bce4b-208">Pro prostředků zahrnutých v cyklická závislost, pečlivě zkontrolujte všechna jeho použití **dependsOn** vlastnost k identifikaci všechny závislosti, které nejsou potřebné.</span><span class="sxs-lookup"><span data-stu-id="bce4b-208">For the resources involved in the circular dependency, carefully examine all uses of the **dependsOn** property to identify any dependencies that are not needed.</span></span> <span data-ttu-id="bce4b-209">Odeberte tyto závislosti.</span><span class="sxs-lookup"><span data-stu-id="bce4b-209">Remove those dependencies.</span></span> <span data-ttu-id="bce4b-210">Pokud si nejste jistí, že je potřeba závislost, zkuste ji odebrat.</span><span class="sxs-lookup"><span data-stu-id="bce4b-210">If you are unsure that a dependency is needed, try removing it.</span></span> 
6. <span data-ttu-id="bce4b-211">Znovu nasaďte šablonu.</span><span class="sxs-lookup"><span data-stu-id="bce4b-211">Redeploy the template.</span></span>

<span data-ttu-id="bce4b-212">Odebírání hodnot **dependsOn** vlastnost mohou způsobit chyby při nasazování šablony.</span><span class="sxs-lookup"><span data-stu-id="bce4b-212">Removing values from the **dependsOn** property can cause errors when you deploy the template.</span></span> <span data-ttu-id="bce4b-213">Pokud dojde k chybě, přidejte závislosti zpět do šablony.</span><span class="sxs-lookup"><span data-stu-id="bce4b-213">If you encounter an error, add the dependency back into the template.</span></span> 

<span data-ttu-id="bce4b-214">Pokud tento přístup nevyřeší cyklická závislost, zvažte přesunutí součást logiky nasazení do podřízené prostředky (například rozšíření nebo nastavení konfigurace).</span><span class="sxs-lookup"><span data-stu-id="bce4b-214">If that approach does not solve the circular dependency, consider moving part of your deployment logic into child resources (such as extensions or configuration settings).</span></span> <span data-ttu-id="bce4b-215">Nakonfigurujte tyto podřízené prostředky k nasazení po prostředků zahrnutých v cyklická závislost.</span><span class="sxs-lookup"><span data-stu-id="bce4b-215">Configure those child resources to deploy after the resources involved in the circular dependency.</span></span> <span data-ttu-id="bce4b-216">Předpokládejme například, kterou nasazujete dva virtuální počítače, ale je nutné nastavit vlastnosti u každé z nich odkazovat na druhý.</span><span class="sxs-lookup"><span data-stu-id="bce4b-216">For example, suppose you are deploying two virtual machines but you must set properties on each one that refer to the other.</span></span> <span data-ttu-id="bce4b-217">Je můžete nasadit v následujícím pořadí:</span><span class="sxs-lookup"><span data-stu-id="bce4b-217">You can deploy them in the following order:</span></span>

1. <span data-ttu-id="bce4b-218">vm1</span><span class="sxs-lookup"><span data-stu-id="bce4b-218">vm1</span></span>
2. <span data-ttu-id="bce4b-219">virtuálního počítače 2</span><span class="sxs-lookup"><span data-stu-id="bce4b-219">vm2</span></span>
3. <span data-ttu-id="bce4b-220">Rozšíření na vm1 závisí na vm1 a virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="bce4b-220">Extension on vm1 depends on vm1 and vm2.</span></span> <span data-ttu-id="bce4b-221">Rozšíření nastaví hodnoty vm1, který získá z virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="bce4b-221">The extension sets values on vm1 that it gets from vm2.</span></span>
4. <span data-ttu-id="bce4b-222">Rozšíření na virtuálního počítače 2 závisí na vm1 a virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="bce4b-222">Extension on vm2 depends on vm1 and vm2.</span></span> <span data-ttu-id="bce4b-223">Rozšíření virtuálního počítače 2, který získá ze vm1 nastaví hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bce4b-223">The extension sets values on vm2 that it gets from vm1.</span></span>

<span data-ttu-id="bce4b-224">Ve stejný přístup funguje pro aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="bce4b-224">The same approach works for App Service apps.</span></span> <span data-ttu-id="bce4b-225">Zvažte přesunutí hodnoty konfigurace do podřízených prostředkem prostředek aplikace.</span><span class="sxs-lookup"><span data-stu-id="bce4b-225">Consider moving configuration values into a child resource of the app resource.</span></span> <span data-ttu-id="bce4b-226">Můžete nasadit dva webové aplikace v následujícím pořadí:</span><span class="sxs-lookup"><span data-stu-id="bce4b-226">You can deploy two web apps in the following order:</span></span>

1. <span data-ttu-id="bce4b-227">webapp1</span><span class="sxs-lookup"><span data-stu-id="bce4b-227">webapp1</span></span>
2. <span data-ttu-id="bce4b-228">webapp2</span><span class="sxs-lookup"><span data-stu-id="bce4b-228">webapp2</span></span>
3. <span data-ttu-id="bce4b-229">konfigurace pro webapp1 závisí na webapp1 a webapp2.</span><span class="sxs-lookup"><span data-stu-id="bce4b-229">config for webapp1 depends on webapp1 and webapp2.</span></span> <span data-ttu-id="bce4b-230">Obsahuje nastavení aplikace s hodnotami z webapp2.</span><span class="sxs-lookup"><span data-stu-id="bce4b-230">It contains app settings with values from webapp2.</span></span>
4. <span data-ttu-id="bce4b-231">konfigurace pro webapp2 závisí na webapp1 a webapp2.</span><span class="sxs-lookup"><span data-stu-id="bce4b-231">config for webapp2 depends on webapp1 and webapp2.</span></span> <span data-ttu-id="bce4b-232">Obsahuje nastavení aplikace s hodnotami z webapp1.</span><span class="sxs-lookup"><span data-stu-id="bce4b-232">It contains app settings with values from webapp1.</span></span>


## <a name="next-steps"></a><span data-ttu-id="bce4b-233">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bce4b-233">Next steps</span></span>
* <span data-ttu-id="bce4b-234">Řešení pro běžné chyby nasazení najdete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="bce4b-234">For resolutions to common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="bce4b-235">Další informace o auditování akce najdete v tématu [auditovat operace s Resource Managerem](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="bce4b-235">To learn about auditing actions, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="bce4b-236">Další informace o akce k určení chyby během nasazení najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="bce4b-236">To learn about actions to determine the errors during deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
