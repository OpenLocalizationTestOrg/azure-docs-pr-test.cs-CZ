---
title: "chyby nasazení Azure aaaUnderstand | Microsoft Docs"
description: "Popisuje, jak toolearn o chybách nasazení Azure."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Chyba nasazení, nasazení azure nasazení tooazure"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.openlocfilehash: a335e121e9b908a763374907e34b1f6e823d6e96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-deployment-errors"></a><span data-ttu-id="980e7-104">Pochopení chyby nasazení Azure</span><span class="sxs-lookup"><span data-stu-id="980e7-104">Understand Azure deployment errors</span></span>
<span data-ttu-id="980e7-105">Toto téma popisuje chyby nasazení a jak můžete zjistit další informace o chybě.</span><span class="sxs-lookup"><span data-stu-id="980e7-105">This topic describes deployment errors and how you can discover more information about an error.</span></span> <span data-ttu-id="980e7-106">Chyby nasazení toocommon řešení, najdete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="980e7-106">For resolutions toocommon deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>

## <a name="two-types-of-errors"></a><span data-ttu-id="980e7-107">Dva typy chyb</span><span class="sxs-lookup"><span data-stu-id="980e7-107">Two types of errors</span></span>
<span data-ttu-id="980e7-108">Existují dva typy chyb, které dostanete:</span><span class="sxs-lookup"><span data-stu-id="980e7-108">There are two types of errors you can receive:</span></span>

* <span data-ttu-id="980e7-109">Chyby ověření</span><span class="sxs-lookup"><span data-stu-id="980e7-109">validation errors</span></span>
* <span data-ttu-id="980e7-110">Chyby nasazení</span><span class="sxs-lookup"><span data-stu-id="980e7-110">deployment errors</span></span>

<span data-ttu-id="980e7-111">Hello následující obrázek znázorňuje hello protokol aktivit pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="980e7-111">hello following image shows hello activity log for a subscription.</span></span> <span data-ttu-id="980e7-112">Reprezentuje dvě nasazení.</span><span class="sxs-lookup"><span data-stu-id="980e7-112">It represents two deployments.</span></span> <span data-ttu-id="980e7-113">V nasazení jedné hello šablony se nezdařilo ověření (**ověřením**) a neproběhla.</span><span class="sxs-lookup"><span data-stu-id="980e7-113">In one deployment, hello template failed validation (**Validate**) and did not proceed.</span></span> <span data-ttu-id="980e7-114">V hello další nasazení, šablony hello ověření proběhlo úspěšně, ale při vytváření prostředků hello (**zápisu nasazení**).</span><span class="sxs-lookup"><span data-stu-id="980e7-114">In hello other deployment, hello template passed validation but failed when creating hello resources (**Write Deployments**).</span></span> 

![Zobrazí kód chyby](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

<span data-ttu-id="980e7-116">Chyby ověření jsou vyvolány scénáře, které lze určit před nasazením.</span><span class="sxs-lookup"><span data-stu-id="980e7-116">Validation errors arise from scenarios that can be determined before deployment.</span></span> <span data-ttu-id="980e7-117">Obsahují chyby syntaxe v šabloně, nebo se snažíme toodeploy prostředky, které překročí vaší kvóty předplatného.</span><span class="sxs-lookup"><span data-stu-id="980e7-117">They include syntax errors in your template, or trying toodeploy resources that would exceed your subscription quotas.</span></span> <span data-ttu-id="980e7-118">Chyby nasazení jsou vyvolány podmínky, které ke kterým došlo během nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-118">Deployment errors arise from conditions that occur during hello deployment process.</span></span> <span data-ttu-id="980e7-119">Patří mezi ně při tooaccess na prostředek, který je nasazený paralelně.</span><span class="sxs-lookup"><span data-stu-id="980e7-119">They include trying tooaccess a resource that is being deployed in parallel.</span></span>

<span data-ttu-id="980e7-120">Oba typy chyb návratový kód chyby, že používáte tootroubleshoot hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="980e7-120">Both types of errors return an error code that you use tootroubleshoot hello deployment.</span></span> <span data-ttu-id="980e7-121">Oba typy chyb se zobrazí v hello [protokol aktivit](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="980e7-121">Both types of errors appear in hello [activity log](resource-group-audit.md).</span></span> <span data-ttu-id="980e7-122">Ale chyb při ověřování se nezobrazí v historii nasazení protože hello nasazení nikdy spuštěn.</span><span class="sxs-lookup"><span data-stu-id="980e7-122">However, validation errors do not appear in your deployment history because hello deployment never started.</span></span>

## <a name="determine-error-code"></a><span data-ttu-id="980e7-123">Určete kód chyby</span><span class="sxs-lookup"><span data-stu-id="980e7-123">Determine error code</span></span>

<span data-ttu-id="980e7-124">Informace o chybě najdete prohlížením hello chybovou zprávu a hello kód chyby.</span><span class="sxs-lookup"><span data-stu-id="980e7-124">You can learn about an error by looking at hello error message and hello error code.</span></span> <span data-ttu-id="980e7-125">Hello [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md) článku jsou uvedené řešení v kódu chyby.</span><span class="sxs-lookup"><span data-stu-id="980e7-125">hello [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md) article lists resolutions by error code.</span></span> <span data-ttu-id="980e7-126">Toto téma ukazuje, jak toouse hello Azure portálu toodiscover hello chybový kód.</span><span class="sxs-lookup"><span data-stu-id="980e7-126">This topic shows how toouse hello Azure portal toodiscover hello error code.</span></span>

### <a name="validation-errors"></a><span data-ttu-id="980e7-127">Chyby ověření</span><span class="sxs-lookup"><span data-stu-id="980e7-127">Validation errors</span></span>

<span data-ttu-id="980e7-128">Pokud nasazujete prostřednictvím portálu hello, zobrazí chybu ověření po odeslání svoje hodnoty.</span><span class="sxs-lookup"><span data-stu-id="980e7-128">When deploying through hello portal, you see a validation error after submitting your values.</span></span>

![zobrazit chyba portálu ověření](./media/resource-manager-troubleshoot-tips/validation-error.png)

<span data-ttu-id="980e7-130">Vyberte uvítací zprávu další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="980e7-130">Select hello message for more details.</span></span> <span data-ttu-id="980e7-131">Hello následující bitové kopie, se zobrazuje **InvalidTemplateDeployment** chyba a zprávu, která určuje zásadu blokován nasazení.</span><span class="sxs-lookup"><span data-stu-id="980e7-131">In hello following image, you see an **InvalidTemplateDeployment** error and a message that indicates a policy blocked deployment.</span></span>

![Zobrazit podrobnosti o ověření](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a><span data-ttu-id="980e7-133">Chyby nasazení</span><span class="sxs-lookup"><span data-stu-id="980e7-133">Deployment errors</span></span>

<span data-ttu-id="980e7-134">Při operaci hello ověřením úspěšně projde, ale během nasazení se nezdaří, zobrazí se chyba hello v oznámeních hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-134">When hello operation passes validation, but fails during deployment, you see hello error in hello notifications.</span></span> <span data-ttu-id="980e7-135">Vyberte oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-135">Select hello notification.</span></span>

![Chyba oznámení](./media/resource-manager-troubleshoot-tips/notification.png)

<span data-ttu-id="980e7-137">Zobrazí podrobnosti o nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-137">You see more details about hello deployment.</span></span> <span data-ttu-id="980e7-138">Vyberte možnost toofind hello Další informace o chybě hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-138">Select hello option toofind more information about hello error.</span></span>

![nasazení se nezdařilo.](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

<span data-ttu-id="980e7-140">Zobrazí zprávu a chybové kódy chyb hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-140">You see hello error message and error codes.</span></span> <span data-ttu-id="980e7-141">Všimněte si, že jsou dva kódy chyb.</span><span class="sxs-lookup"><span data-stu-id="980e7-141">Notice there are two error codes.</span></span> <span data-ttu-id="980e7-142">Hello první kód chyby (**DeploymentFailed**) je obecná chyba, která nenabízí hello podrobnosti můžete potřebovat toosolve hello chyby.</span><span class="sxs-lookup"><span data-stu-id="980e7-142">hello first error code (**DeploymentFailed**) is a general error that does not provide hello details you need toosolve hello error.</span></span> <span data-ttu-id="980e7-143">Hello druhý kód chyby (**StorageAccountNotFound**) poskytuje podrobnosti hello potřebujete.</span><span class="sxs-lookup"><span data-stu-id="980e7-143">hello second error code (**StorageAccountNotFound**) provides hello details you need.</span></span> 

![Podrobnosti o chybě](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a><span data-ttu-id="980e7-145">Povolit protokolování ladění</span><span class="sxs-lookup"><span data-stu-id="980e7-145">Enable debug logging</span></span>
<span data-ttu-id="980e7-146">Někdy můžete potřebovat další informace o požadavku a odpovědi toodiscover hello kde došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="980e7-146">Sometimes you need more information about hello request and response toodiscover what went wrong.</span></span> <span data-ttu-id="980e7-147">Pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure, můžete požádat, že další informace o protokolu během nasazení.</span><span class="sxs-lookup"><span data-stu-id="980e7-147">By using PowerShell or Azure CLI, you can request that additional information is logged during a deployment.</span></span>

- <span data-ttu-id="980e7-148">PowerShell</span><span class="sxs-lookup"><span data-stu-id="980e7-148">PowerShell</span></span>

   <span data-ttu-id="980e7-149">V prostředí PowerShell, nastavte hello **DeploymentDebugLogLevel** parametr tooAll, ResponseContent nebo RequestContent.</span><span class="sxs-lookup"><span data-stu-id="980e7-149">In PowerShell, set hello **DeploymentDebugLogLevel** parameter tooAll, ResponseContent, or RequestContent.</span></span>

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   <span data-ttu-id="980e7-150">Zkontrolujte obsah žádosti hello s hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="980e7-150">Examine hello request content with hello following cmdlet:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   <span data-ttu-id="980e7-151">Nebo hello obsahu s odpovědí:</span><span class="sxs-lookup"><span data-stu-id="980e7-151">Or, hello response content with:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   <span data-ttu-id="980e7-152">Tyto informace můžete určit, zda hodnotu v šabloně hello je správně nastaveno.</span><span class="sxs-lookup"><span data-stu-id="980e7-152">This information can help you determine whether a value in hello template is being incorrectly set.</span></span>

- <span data-ttu-id="980e7-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="980e7-153">Azure CLI</span></span>

   <span data-ttu-id="980e7-154">Zkontrolujte hello operace nasazení se hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="980e7-154">Examine hello deployment operations with hello following command:</span></span>

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- <span data-ttu-id="980e7-155">Vnořené šablony</span><span class="sxs-lookup"><span data-stu-id="980e7-155">Nested template</span></span>

   <span data-ttu-id="980e7-156">informace o ladění toolog vnořené šablony, použijte hello **debugSetting** element.</span><span class="sxs-lookup"><span data-stu-id="980e7-156">toolog debug information for a nested template, use hello **debugSetting** element.</span></span>

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


## <a name="create-a-troubleshooting-template"></a><span data-ttu-id="980e7-157">Vytvořit šablonu řešení potíží</span><span class="sxs-lookup"><span data-stu-id="980e7-157">Create a troubleshooting template</span></span>
<span data-ttu-id="980e7-158">V některých případech hello nejjednodušší způsob, jak tootroubleshoot šablona je tootest části.</span><span class="sxs-lookup"><span data-stu-id="980e7-158">In some cases, hello easiest way tootroubleshoot your template is tootest parts of it.</span></span> <span data-ttu-id="980e7-159">Zjednodušená šablonu, která vám umožní můžete vytvořit toofocus na hello část, která budete mít dojem, že je příčinou chyby hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-159">You can create a simplified template that enables you toofocus on hello part that you believe is causing hello error.</span></span> <span data-ttu-id="980e7-160">Předpokládejme například, že jste obdrželi chybu při odkazování na prostředek.</span><span class="sxs-lookup"><span data-stu-id="980e7-160">For example, suppose you are receiving an error when referencing a resource.</span></span> <span data-ttu-id="980e7-161">Místo práci s celou šablony, vytvořte šablonu, která vrátí hello část, která může být příčinou problému.</span><span class="sxs-lookup"><span data-stu-id="980e7-161">Rather than dealing with an entire template, create a template that returns hello part that may be causing your problem.</span></span> <span data-ttu-id="980e7-162">Může pomoct určit, zda jsou předávání v hello správné parametry, pomocí šablony funkce správně, a získávání prostředků hello očekáváte.</span><span class="sxs-lookup"><span data-stu-id="980e7-162">It can help you determine whether you are passing in hello right parameters, using template functions correctly, and getting hello resource you expect.</span></span>

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

<span data-ttu-id="980e7-163">Nebo Předpokládejme, že dochází k chybám nasazení, které budete mít dojem, že se vztahují tooincorrectly nastavit závislosti.</span><span class="sxs-lookup"><span data-stu-id="980e7-163">Or, suppose you are encountering deployment errors that you believe are related tooincorrectly set dependencies.</span></span> <span data-ttu-id="980e7-164">Testování šablony porušením do zjednodušené šablony.</span><span class="sxs-lookup"><span data-stu-id="980e7-164">Test your template by breaking it into simplified templates.</span></span> <span data-ttu-id="980e7-165">Nejprve vytvořte šablonu, která nasadí pouze jeden prostředek (např. SQL Server).</span><span class="sxs-lookup"><span data-stu-id="980e7-165">First, create a template that deploys only a single resource (like a SQL Server).</span></span> <span data-ttu-id="980e7-166">Pokud jste si jistí, že máte správně definované prostředku, přidejte na prostředek, který na něm závisí (např. SQL Database).</span><span class="sxs-lookup"><span data-stu-id="980e7-166">When you are sure you have that resource correctly defined, add a resource that depends on it (like a SQL Database).</span></span> <span data-ttu-id="980e7-167">Až budete mít tyto dva prostředky správně definované, přidejte další závislé prostředky (jako jsou zásady auditu).</span><span class="sxs-lookup"><span data-stu-id="980e7-167">When you have those two resources correctly defined, add other dependent resources (like auditing policies).</span></span> <span data-ttu-id="980e7-168">Mezi každou testovací nasazení odstraňte hello skupiny toomake zda jste adekvátní testování hello závislosti prostředků.</span><span class="sxs-lookup"><span data-stu-id="980e7-168">In between each test deployment, delete hello resource group toomake sure you adequately testing hello dependencies.</span></span> 

## <a name="check-deployment-sequence"></a><span data-ttu-id="980e7-169">Zkontrolujte pořadí nasazení</span><span class="sxs-lookup"><span data-stu-id="980e7-169">Check deployment sequence</span></span>

<span data-ttu-id="980e7-170">Mnoho chyb nasazení dojít, když se prostředky nasadí neočekávané pořadí.</span><span class="sxs-lookup"><span data-stu-id="980e7-170">Many deployment errors happen when resources are deployed in an unexpected sequence.</span></span> <span data-ttu-id="980e7-171">Tyto chyby jsou vyvolány při závislosti nejsou správně nastavené.</span><span class="sxs-lookup"><span data-stu-id="980e7-171">These errors arise when dependencies are not correctly set.</span></span> <span data-ttu-id="980e7-172">Pokud chybí potřebné závislosti, jeden prostředek pokusí o toouse hodnotu pro jiný prostředek, ale hello jiných ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="980e7-172">When you are missing a needed dependency, one resource attempts toouse a value for another resource but hello other does not yet exist.</span></span> <span data-ttu-id="980e7-173">Zobrazí chyba oznamující, že prostředek nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="980e7-173">You get an error stating that a resource is not found.</span></span> <span data-ttu-id="980e7-174">Setkat tohoto typu chyby občas může lišit hello času nasazení pro každého prostředku.</span><span class="sxs-lookup"><span data-stu-id="980e7-174">You may encounter this type of error intermittently because hello deployment time for each resource can vary.</span></span> <span data-ttu-id="980e7-175">Například vaše první pokus o toodeploy vašich prostředků bude úspěšné, protože požadovaný prostředek náhodně dokončí v čase.</span><span class="sxs-lookup"><span data-stu-id="980e7-175">For example, your first attempt toodeploy your resources succeeds because a required resource randomly completes in time.</span></span> <span data-ttu-id="980e7-176">Však druhý pokus selže, protože hello požadované prostředku se nedokončila v čase.</span><span class="sxs-lookup"><span data-stu-id="980e7-176">However, your second attempt fails because hello required resource did not complete in time.</span></span> 

<span data-ttu-id="980e7-177">Ale chcete tooavoid nastavení závislosti, které nejsou potřebné.</span><span class="sxs-lookup"><span data-stu-id="980e7-177">But, you want tooavoid setting dependencies that are not needed.</span></span> <span data-ttu-id="980e7-178">Až budete mít nepotřebné závislosti, můžete prodloužit doba trvání hello hello nasazení tím, že prostředky, které nejsou na sobě navzájem závislé z nasazuje paralelně.</span><span class="sxs-lookup"><span data-stu-id="980e7-178">When you have unnecessary dependencies, you prolong hello duration of hello deployment by preventing resources that are not dependent on each other from being deployed in parallel.</span></span> <span data-ttu-id="980e7-179">Kromě toho můžete vytvořit cyklické závislosti, které blokují nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-179">In addition, you may create circular dependencies that block hello deployment.</span></span> <span data-ttu-id="980e7-180">Hello [odkaz](resource-group-template-functions-resource.md#reference) funkce vytvoří implicitní závislostí v hello odkazuje prostředků při tento prostředek je nasazena v hello stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="980e7-180">hello [reference](resource-group-template-functions-resource.md#reference) function creates an implicit dependency on hello referenced resource, when that resource is deployed in hello same template.</span></span> <span data-ttu-id="980e7-181">Proto můžete mít další závislosti než hello závislosti zadaný v hello **dependsOn** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="980e7-181">Therefore, you may have more dependencies than hello dependencies specified in hello **dependsOn** property.</span></span> <span data-ttu-id="980e7-182">Hello [resourceId](resource-group-template-functions-resource.md#resourceid) funkce vytvořit implicitní závislostí nebo ověřit, zda text hello prostředků existuje.</span><span class="sxs-lookup"><span data-stu-id="980e7-182">hello [resourceId](resource-group-template-functions-resource.md#resourceid) function does not create an implicit dependency or validate that hello resource exists.</span></span>

<span data-ttu-id="980e7-183">Pokud narazíte na potíže závislostí, je potřeba toogain aspekty hello pořadí nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="980e7-183">When you encounter dependency problems, you need toogain insight into hello order of resource deployment.</span></span> <span data-ttu-id="980e7-184">pořadí hello tooview operace nasazení:</span><span class="sxs-lookup"><span data-stu-id="980e7-184">tooview hello order of deployment operations:</span></span>

1. <span data-ttu-id="980e7-185">Vyberte hello historii nasazení skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="980e7-185">Select hello deployment history for your resource group.</span></span>

   ![Vyberte historii nasazení](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. <span data-ttu-id="980e7-187">Vyberte nasazení z historie hello a vyberte **události**.</span><span class="sxs-lookup"><span data-stu-id="980e7-187">Select a deployment from hello history, and select **Events**.</span></span>

   ![Vybrat nasazení události](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. <span data-ttu-id="980e7-189">Zkontrolujte hello posloupnost událostí pro každého prostředku.</span><span class="sxs-lookup"><span data-stu-id="980e7-189">Examine hello sequence of events for each resource.</span></span> <span data-ttu-id="980e7-190">Věnujte pozornost toohello stav jednotlivých operací.</span><span class="sxs-lookup"><span data-stu-id="980e7-190">Pay attention toohello status of each operation.</span></span> <span data-ttu-id="980e7-191">Například hello následující obrázek ukazuje tři účty úložiště, které nasadí současně.</span><span class="sxs-lookup"><span data-stu-id="980e7-191">For example, hello following image shows three storage accounts that deployed in parallel.</span></span> <span data-ttu-id="980e7-192">Všimněte si, že hello tři účty úložiště jsou zahájená hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="980e7-192">Notice that hello three storage accounts are started at hello same time.</span></span>

   ![Paralelní nasazení](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   <span data-ttu-id="980e7-194">Hello další obrázek ukazuje tři účty úložiště, které nejsou nasazené paralelně.</span><span class="sxs-lookup"><span data-stu-id="980e7-194">hello next image shows three storage accounts that are not deployed in parallel.</span></span> <span data-ttu-id="980e7-195">Hello druhého účtu úložiště závisí na hello první účet úložiště, a účet úložiště třetí hello na hello druhého účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="980e7-195">hello second storage account depends on hello first storage account, and hello third storage account depends on hello second storage account.</span></span> <span data-ttu-id="980e7-196">Proto hello první účet úložiště spuštěna, přijmout a dokončit před příštím spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-196">Therefore, hello first storage account is started, accepted, and completed before hello next is started.</span></span>

   ![sekvenční nasazení](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

<span data-ttu-id="980e7-198">I pro složitější scénáře, můžete použít hello toodiscover stejným způsobem, když je nasazení spuštěno a dokončeno pro každý zdroj.</span><span class="sxs-lookup"><span data-stu-id="980e7-198">Even for more complicated scenarios, you can use hello same technique toodiscover when deployment is started and completed for each resource.</span></span> <span data-ttu-id="980e7-199">Pokud je jiné než byste očekávali hello pořadí projděte toosee události vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="980e7-199">Look through your deployment events toosee if hello sequence is different than you would expect.</span></span> <span data-ttu-id="980e7-200">Pokud ano, přehodnocovat hello závislosti pro tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="980e7-200">If so, reevaluate hello dependencies for this resource.</span></span>

<span data-ttu-id="980e7-201">Správce prostředků identifikuje cyklické závislosti během ověřování šablony.</span><span class="sxs-lookup"><span data-stu-id="980e7-201">Resource Manager identifies circular dependencies during template validation.</span></span> <span data-ttu-id="980e7-202">Vrátí, zda existuje chybovou zprávu, která konkrétně stavy cyklickou závislost.</span><span class="sxs-lookup"><span data-stu-id="980e7-202">It returns an error message that specifically states a circular dependency exists.</span></span> <span data-ttu-id="980e7-203">toosolve cyklická závislost:</span><span class="sxs-lookup"><span data-stu-id="980e7-203">toosolve a circular dependency:</span></span>

1. <span data-ttu-id="980e7-204">V šabloně najdete hello zdroje identifikovaného v hello cyklická závislost.</span><span class="sxs-lookup"><span data-stu-id="980e7-204">In your template, find hello resource identified in hello circular dependency.</span></span> 
2. <span data-ttu-id="980e7-205">Pro tento prostředek, zkontrolujte hello **dependsOn** vlastnost a všechny používá hello **odkaz** toosee prostředky, ke kterým závisí na funkce.</span><span class="sxs-lookup"><span data-stu-id="980e7-205">For that resource, examine hello **dependsOn** property and any uses of hello **reference** function toosee which resources it depends on.</span></span> 
3. <span data-ttu-id="980e7-206">Zkontrolujte toosee tyto prostředky, které prostředky, které jsou závislé na.</span><span class="sxs-lookup"><span data-stu-id="980e7-206">Examine those resources toosee which resources they depend on.</span></span> <span data-ttu-id="980e7-207">Postupujte podle hello závislosti, dokud si všimnete na prostředek, který závisí na původní prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-207">Follow hello dependencies until you notice a resource that depends on hello original resource.</span></span>
5. <span data-ttu-id="980e7-208">Pro hello prostředků zahrnutých v hello cyklická závislost, pečlivě zkontrolujte všechny používá hello **dependsOn** vlastnost tooidentify všechny závislosti, které nejsou potřebné.</span><span class="sxs-lookup"><span data-stu-id="980e7-208">For hello resources involved in hello circular dependency, carefully examine all uses of hello **dependsOn** property tooidentify any dependencies that are not needed.</span></span> <span data-ttu-id="980e7-209">Odeberte tyto závislosti.</span><span class="sxs-lookup"><span data-stu-id="980e7-209">Remove those dependencies.</span></span> <span data-ttu-id="980e7-210">Pokud si nejste jistí, že je potřeba závislost, zkuste ji odebrat.</span><span class="sxs-lookup"><span data-stu-id="980e7-210">If you are unsure that a dependency is needed, try removing it.</span></span> 
6. <span data-ttu-id="980e7-211">Znovu nasaďte šablonu hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-211">Redeploy hello template.</span></span>

<span data-ttu-id="980e7-212">Odebírání hodnot z hello **dependsOn** vlastnost mohou způsobit chyby, když nasazujete hello šablony.</span><span class="sxs-lookup"><span data-stu-id="980e7-212">Removing values from hello **dependsOn** property can cause errors when you deploy hello template.</span></span> <span data-ttu-id="980e7-213">Pokud dojde k chybě, přidejte závislosti hello zpět do šablony hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-213">If you encounter an error, add hello dependency back into hello template.</span></span> 

<span data-ttu-id="980e7-214">Pokud tento přístup nevyřeší hello cyklická závislost, zvažte přesunutí součást logiky nasazení do podřízené prostředky (například rozšíření nebo nastavení konfigurace).</span><span class="sxs-lookup"><span data-stu-id="980e7-214">If that approach does not solve hello circular dependency, consider moving part of your deployment logic into child resources (such as extensions or configuration settings).</span></span> <span data-ttu-id="980e7-215">Nakonfigurujte tyto podřízené prostředky toodeploy po hello prostředků zahrnutých v hello cyklická závislost.</span><span class="sxs-lookup"><span data-stu-id="980e7-215">Configure those child resources toodeploy after hello resources involved in hello circular dependency.</span></span> <span data-ttu-id="980e7-216">Předpokládejme například, nasazujete dva virtuální počítače, ale je nutné nastavit na každé z nich najdete toohello jiné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="980e7-216">For example, suppose you are deploying two virtual machines but you must set properties on each one that refer toohello other.</span></span> <span data-ttu-id="980e7-217">Můžete je nasadit v hello následující pořadí:</span><span class="sxs-lookup"><span data-stu-id="980e7-217">You can deploy them in hello following order:</span></span>

1. <span data-ttu-id="980e7-218">vm1</span><span class="sxs-lookup"><span data-stu-id="980e7-218">vm1</span></span>
2. <span data-ttu-id="980e7-219">virtuálního počítače 2</span><span class="sxs-lookup"><span data-stu-id="980e7-219">vm2</span></span>
3. <span data-ttu-id="980e7-220">Rozšíření na vm1 závisí na vm1 a virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="980e7-220">Extension on vm1 depends on vm1 and vm2.</span></span> <span data-ttu-id="980e7-221">rozšíření Hello nastaví hodnoty vm1, který získá z virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="980e7-221">hello extension sets values on vm1 that it gets from vm2.</span></span>
4. <span data-ttu-id="980e7-222">Rozšíření na virtuálního počítače 2 závisí na vm1 a virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="980e7-222">Extension on vm2 depends on vm1 and vm2.</span></span> <span data-ttu-id="980e7-223">rozšíření Hello nastaví hodnoty virtuálního počítače 2, který získá ze vm1.</span><span class="sxs-lookup"><span data-stu-id="980e7-223">hello extension sets values on vm2 that it gets from vm1.</span></span>

<span data-ttu-id="980e7-224">Hello stejný přístup funguje pro aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="980e7-224">hello same approach works for App Service apps.</span></span> <span data-ttu-id="980e7-225">Zvažte přesunutí hodnoty konfigurace do podřízených prostředkem prostředek aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="980e7-225">Consider moving configuration values into a child resource of hello app resource.</span></span> <span data-ttu-id="980e7-226">Můžete nasadit dva webové aplikace v hello následující pořadí:</span><span class="sxs-lookup"><span data-stu-id="980e7-226">You can deploy two web apps in hello following order:</span></span>

1. <span data-ttu-id="980e7-227">webapp1</span><span class="sxs-lookup"><span data-stu-id="980e7-227">webapp1</span></span>
2. <span data-ttu-id="980e7-228">webapp2</span><span class="sxs-lookup"><span data-stu-id="980e7-228">webapp2</span></span>
3. <span data-ttu-id="980e7-229">konfigurace pro webapp1 závisí na webapp1 a webapp2.</span><span class="sxs-lookup"><span data-stu-id="980e7-229">config for webapp1 depends on webapp1 and webapp2.</span></span> <span data-ttu-id="980e7-230">Obsahuje nastavení aplikace s hodnotami z webapp2.</span><span class="sxs-lookup"><span data-stu-id="980e7-230">It contains app settings with values from webapp2.</span></span>
4. <span data-ttu-id="980e7-231">konfigurace pro webapp2 závisí na webapp1 a webapp2.</span><span class="sxs-lookup"><span data-stu-id="980e7-231">config for webapp2 depends on webapp1 and webapp2.</span></span> <span data-ttu-id="980e7-232">Obsahuje nastavení aplikace s hodnotami z webapp1.</span><span class="sxs-lookup"><span data-stu-id="980e7-232">It contains app settings with values from webapp1.</span></span>


## <a name="next-steps"></a><span data-ttu-id="980e7-233">Další kroky</span><span class="sxs-lookup"><span data-stu-id="980e7-233">Next steps</span></span>
* <span data-ttu-id="980e7-234">Chyby nasazení toocommon řešení, najdete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="980e7-234">For resolutions toocommon deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="980e7-235">toolearn o auditování akce, najdete v části [auditovat operace s Resource Managerem](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="980e7-235">toolearn about auditing actions, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="980e7-236">toolearn o akce toodetermine hello chyb během nasazení, najdete v části [zobrazit operace nasazení](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="980e7-236">toolearn about actions toodetermine hello errors during deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
