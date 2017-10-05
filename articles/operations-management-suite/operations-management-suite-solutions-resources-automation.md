---
title: "Prostředky služby Azure Automation v OMS řešení | Microsoft Docs"
description: "Řešení v OMS by měl obvykle zahrnovat sady runbook ve službě Azure Automation k automatizaci procesů, jako je shromažďování a zpracování dat monitorování.  Tento článek popisuje, jak se zahrnuje sady runbook a jejich související prostředky v řešení."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 5281462e-f480-4e5e-9c19-022f36dce76d
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c1909183a33ed03d8165671cff25cc8b83b77733
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="adding-azure-automation-resources-to-an-oms-management-solution-preview"></a><span data-ttu-id="90ad4-104">Přidání prostředky Azure Automation OMS řešení pro správu (Preview)</span><span class="sxs-lookup"><span data-stu-id="90ad4-104">Adding Azure Automation resources to an OMS management solution (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="90ad4-105">Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="90ad4-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="90ad4-106">Žádné schéma popsané níže se mohou změnit.</span><span class="sxs-lookup"><span data-stu-id="90ad4-106">Any schema described below is subject to change.</span></span>   


<span data-ttu-id="90ad4-107">[Řešení pro správu v OMS](operations-management-suite-solutions.md) by měl obvykle zahrnovat sady runbook ve službě Azure Automation k automatizaci procesů, jako je shromažďování a zpracování dat monitorování.</span><span class="sxs-lookup"><span data-stu-id="90ad4-107">[Management solutions in OMS](operations-management-suite-solutions.md) will typically include runbooks in Azure Automation to automate processes such as collecting and processing monitoring data.</span></span>  <span data-ttu-id="90ad4-108">Účty Automation kromě sady runbook, obsahuje prostředky, jako jsou proměnné a plány, které podporují sady runbook používá v řešení.</span><span class="sxs-lookup"><span data-stu-id="90ad4-108">In addition to runbooks, Automation accounts includes assets such as variables and schedules that support the runbooks used in the solution.</span></span>  <span data-ttu-id="90ad4-109">Tento článek popisuje, jak se zahrnuje sady runbook a jejich související prostředky v řešení.</span><span class="sxs-lookup"><span data-stu-id="90ad4-109">This article describes how to include runbooks and their related resources in a solution.</span></span>

> [!NOTE]
> <span data-ttu-id="90ad4-110">Ukázky v tomto článku použít parametry a proměnné, které jsou nutné nebo společné pro řešení pro správu a jsou popsány v [vytváření řešení pro správu v Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="90ad4-110">The samples in this article use parameters and variables that are either required or common to management solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="90ad4-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="90ad4-111">Prerequisites</span></span>
<span data-ttu-id="90ad4-112">Tento článek předpokládá, že jste již obeznámeni s následujícími informacemi.</span><span class="sxs-lookup"><span data-stu-id="90ad4-112">This article assumes that you're already familiar with the following information.</span></span>

- <span data-ttu-id="90ad4-113">Postup [vytvoření řešení správy](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="90ad4-113">How to [create a management solution](operations-management-suite-solutions-creating.md).</span></span>
- <span data-ttu-id="90ad4-114">Struktura [soubor řešení](operations-management-suite-solutions-solution-file.md).</span><span class="sxs-lookup"><span data-stu-id="90ad4-114">The structure of a [solution file](operations-management-suite-solutions-solution-file.md).</span></span>
- <span data-ttu-id="90ad4-115">Postup [vytváření šablon Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="90ad4-115">How to [author Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md)</span></span>

## <a name="automation-account"></a><span data-ttu-id="90ad4-116">Účet Automation</span><span class="sxs-lookup"><span data-stu-id="90ad4-116">Automation account</span></span>
<span data-ttu-id="90ad4-117">Všechny prostředky ve službě Azure Automation jsou součástí [účet Automation](../automation/automation-security-overview.md#automation-account-overview).</span><span class="sxs-lookup"><span data-stu-id="90ad4-117">All resources in Azure Automation are contained in an [Automation account](../automation/automation-security-overview.md#automation-account-overview).</span></span>  <span data-ttu-id="90ad4-118">Jak je popsáno v [OMS pracovní prostor a účet Automation](operations-management-suite-solutions.md#oms-workspace-and-automation-account) účet Automation není zahrnutý v řešení pro správu, ale musí existovat před instalací řešení.</span><span class="sxs-lookup"><span data-stu-id="90ad4-118">As described in [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) the Automation account isn't included in the management solution but must exist before the solution is installed.</span></span>  <span data-ttu-id="90ad4-119">Pokud není k dispozici, se nezdaří instalace řešení.</span><span class="sxs-lookup"><span data-stu-id="90ad4-119">If it isn't available, then the solution install will fail.</span></span>

<span data-ttu-id="90ad4-120">Název každého prostředku automatizace obsahuje název svůj účet Automation.</span><span class="sxs-lookup"><span data-stu-id="90ad4-120">The name of each Automation resource includes the name of its Automation account.</span></span>  <span data-ttu-id="90ad4-121">To se provádí v řešení s **accountName** parametr jako v následujícím příkladu runbook prostředku.</span><span class="sxs-lookup"><span data-stu-id="90ad4-121">This is done in the solution with the **accountName** parameter as in the following example of a runbook resource.</span></span>

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a><span data-ttu-id="90ad4-122">Runbooky</span><span class="sxs-lookup"><span data-stu-id="90ad4-122">Runbooks</span></span>
<span data-ttu-id="90ad4-123">By měly obsahovat všechny sady runbook používá řešení v souboru řešení tak, aby se vytváří při instalaci řešení.</span><span class="sxs-lookup"><span data-stu-id="90ad4-123">You should include any runbooks used by the solution in the solution file so that they're created when the solution is installed.</span></span>  <span data-ttu-id="90ad4-124">Text sady runbook v šabloně nemůže obsahovat Přestože, proto byste měli publikovat sadu runbook na veřejném místě, kde je přístupná žádný uživatel instalaci řešení.</span><span class="sxs-lookup"><span data-stu-id="90ad4-124">You cannot contain the body of the runbook in the template though, so you should publish the runbook to a public location where it can be accessed by any user installing your solution.</span></span>

<span data-ttu-id="90ad4-125">[Azure Automation runbook](../automation/automation-runbook-types.md) prostředky mít typ **Microsoft.Automation/automationAccounts/runbooks** a mít následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-125">[Azure Automation runbook](../automation/automation-runbook-types.md) resources have a type of **Microsoft.Automation/automationAccounts/runbooks** and have the following structure.</span></span> <span data-ttu-id="90ad4-126">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="90ad4-126">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

    {
        "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
        "type": "Microsoft.Automation/automationAccounts/runbooks",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "location": "[parameters('regionId')]",
        "tags": { },
        "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
                "uri": "[variables('Runbook').Uri]",
                "version": [variables('Runbook').Version]"
            }
        }
    }


<span data-ttu-id="90ad4-127">Vlastnosti pro sady runbook jsou popsány v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="90ad4-127">The properties for runbooks are described in the following table.</span></span>

| <span data-ttu-id="90ad4-128">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="90ad4-128">Property</span></span> | <span data-ttu-id="90ad4-129">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-129">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="90ad4-130">runbookType</span><span class="sxs-lookup"><span data-stu-id="90ad4-130">runbookType</span></span> |<span data-ttu-id="90ad4-131">Určuje typy sady runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-131">Specifies the types of the runbook.</span></span> <br><br> <span data-ttu-id="90ad4-132">Skript - skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="90ad4-132">Script - PowerShell script</span></span> <br><span data-ttu-id="90ad4-133">PowerShell – pracovní postup prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="90ad4-133">PowerShell - PowerShell workflow</span></span> <br> <span data-ttu-id="90ad4-134">GraphPowerShell - runbook skriptu grafické prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="90ad4-134">GraphPowerShell - Graphical PowerShell script runbook</span></span> <br> <span data-ttu-id="90ad4-135">GraphPowerShellWorkflow - runbook pracovního postupu grafické prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="90ad4-135">GraphPowerShellWorkflow - Graphical PowerShell workflow runbook</span></span> |
| <span data-ttu-id="90ad4-136">logProgress</span><span class="sxs-lookup"><span data-stu-id="90ad4-136">logProgress</span></span> |<span data-ttu-id="90ad4-137">Určuje, zda [průběhu záznamy](../automation/automation-runbook-output-and-messages.md) by měl být vygenerován pro sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-137">Specifies whether [progress records](../automation/automation-runbook-output-and-messages.md) should be generated for the runbook.</span></span> |
| <span data-ttu-id="90ad4-138">logVerbose</span><span class="sxs-lookup"><span data-stu-id="90ad4-138">logVerbose</span></span> |<span data-ttu-id="90ad4-139">Určuje, zda [podrobných záznamů](../automation/automation-runbook-output-and-messages.md) by měl být vygenerován pro sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-139">Specifies whether [verbose records](../automation/automation-runbook-output-and-messages.md) should be generated for the runbook.</span></span> |
| <span data-ttu-id="90ad4-140">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-140">description</span></span> |<span data-ttu-id="90ad4-141">Volitelný popis pro sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-141">Optional description for the runbook.</span></span> |
| <span data-ttu-id="90ad4-142">publishContentLink</span><span class="sxs-lookup"><span data-stu-id="90ad4-142">publishContentLink</span></span> |<span data-ttu-id="90ad4-143">Určuje obsah sady runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-143">Specifies the content of the runbook.</span></span> <br><br><span data-ttu-id="90ad4-144">identifikátor URI – identifikátor Uri, který obsah sady runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-144">uri - Uri to the content of the runbook.</span></span>  <span data-ttu-id="90ad4-145">Bude jím souboru s příponou .ps1 pro prostředí PowerShell a skript sady runbook a soubor exportovaný grafický runbook pro sadu runbook grafu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-145">This will be a .ps1 file for PowerShell and Script runbooks, and an exported graphical runbook file for a Graph runbook.</span></span>  <br> <span data-ttu-id="90ad4-146">verze - verze sady runbook pro vlastní sledování.</span><span class="sxs-lookup"><span data-stu-id="90ad4-146">version - Version of the runbook for your own tracking.</span></span> |


## <a name="automation-jobs"></a><span data-ttu-id="90ad4-147">Automatizace úloh</span><span class="sxs-lookup"><span data-stu-id="90ad4-147">Automation jobs</span></span>
<span data-ttu-id="90ad4-148">Když spustíte runbook ve službě Azure Automation, vytvoří úlohu automatizace.</span><span class="sxs-lookup"><span data-stu-id="90ad4-148">When you start a runbook in Azure Automation, it creates an automation job.</span></span>  <span data-ttu-id="90ad4-149">Prostředek automatizace úloh můžete přidat do vašeho řešení na automatické spuštění sady runbook, když je nainstalován do řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-149">You can add an automation job resource to your solution to automatically start a runbook when the management solution is installed.</span></span>  <span data-ttu-id="90ad4-150">Tato metoda se obvykle používá ke spuštění sady runbook, které se používají pro počáteční konfiguraci řešení.</span><span class="sxs-lookup"><span data-stu-id="90ad4-150">This method is typically used to start runbooks that are used for initial configuration of the solution.</span></span>  <span data-ttu-id="90ad4-151">Chcete-li spuštění sady runbook v pravidelných intervalech, vytvořte [plán](#schedules) a [plán úloh](#job-schedules)</span><span class="sxs-lookup"><span data-stu-id="90ad4-151">To start a runbook at regular intervals, create a [schedule](#schedules) and a [job schedule](#job-schedules)</span></span>

<span data-ttu-id="90ad4-152">Úloha prostředky mít typ **Microsoft.Automation/automationAccounts/jobs** a mít následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-152">Job resources have a type of **Microsoft.Automation/automationAccounts/jobs** and have the following structure.</span></span>  <span data-ttu-id="90ad4-153">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="90ad4-153">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', parameters('Runbook').JobGuid)]",
      "type": "Microsoft.Automation/automationAccounts/jobs",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
      ],
      "tags": { },
      "properties": {
        "runbook": {
          "name": "[variables('Runbook').Name]"
        },
        "parameters": {
          "Parameter1": "[[variables('Runbook').Parameter1]",
          "Parameter2": "[[variables('Runbook').Parameter2]"
        }
      }
    }

<span data-ttu-id="90ad4-154">Vlastnosti pro automatizaci úloh jsou popsané v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="90ad4-154">The properties for automation jobs are described in the following table.</span></span>

| <span data-ttu-id="90ad4-155">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="90ad4-155">Property</span></span> | <span data-ttu-id="90ad4-156">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-156">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="90ad4-157">sady runbook</span><span class="sxs-lookup"><span data-stu-id="90ad4-157">runbook</span></span> |<span data-ttu-id="90ad4-158">Jeden název entity s názvem runbook spustit.</span><span class="sxs-lookup"><span data-stu-id="90ad4-158">Single name entity with the name of the runbook to start.</span></span> |
| <span data-ttu-id="90ad4-159">Parametry</span><span class="sxs-lookup"><span data-stu-id="90ad4-159">parameters</span></span> |<span data-ttu-id="90ad4-160">Entita pro každou hodnotu parametru vyžaduje sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-160">Entity for each parameter value required by the runbook.</span></span> |

<span data-ttu-id="90ad4-161">Úloha obsahuje název sady runbook a všechny hodnoty parametrů pro odeslání do runbooku.</span><span class="sxs-lookup"><span data-stu-id="90ad4-161">The job includes the runbook name and any parameter values to be sent to the runbook.</span></span>  <span data-ttu-id="90ad4-162">Úloha by měla [závisí na](operations-management-suite-solutions-solution-file.md#resources) před úlohy je třeba vytvořit runbook, která se spouští od runbooku.</span><span class="sxs-lookup"><span data-stu-id="90ad4-162">The job should [depend on](operations-management-suite-solutions-solution-file.md#resources) the runbook that it's starting since the runbook must be created before the job.</span></span>  <span data-ttu-id="90ad4-163">Pokud máte více sad runbook, který by měl být spuštěn můžete definovat jejich pořadí tak, že úloha závisí na jiné úlohy, které by měl být spuštěn první.</span><span class="sxs-lookup"><span data-stu-id="90ad4-163">If you have multiple runbooks that should be started you can define their order by having a job depend on any other jobs that should be run first.</span></span>

<span data-ttu-id="90ad4-164">Název prostředku úlohy musí obsahovat identifikátor GUID, který je obvykle přiřadila parametr.</span><span class="sxs-lookup"><span data-stu-id="90ad4-164">The name of a job resource must contain a GUID which is typically assigned by a parameter.</span></span>  <span data-ttu-id="90ad4-165">Další informace o parametrech identifikátor GUID v [vytváření řešení v Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="90ad4-165">You can read more about GUID parameters in [Creating solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).</span></span>  


## <a name="certificates"></a><span data-ttu-id="90ad4-166">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="90ad4-166">Certificates</span></span>
<span data-ttu-id="90ad4-167">[Azure Automation certifikáty](../automation/automation-certificates.md) mít typ **Microsoft.Automation/automationAccounts/certificates** a mít následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-167">[Azure Automation certificates](../automation/automation-certificates.md) have a type of **Microsoft.Automation/automationAccounts/certificates** and have the following structure.</span></span> <span data-ttu-id="90ad4-168">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="90ad4-168">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
      "type": "Microsoft.Automation/automationAccounts/certificates",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "base64Value": "[variables('Certificate').Base64Value]",
        "thumbprint": "[variables('Certificate').Thumbprint]"
      }
    }



<span data-ttu-id="90ad4-169">Vlastnosti pro certifikáty prostředky jsou popsány v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="90ad4-169">The properties for Certificates resources are described in the following table.</span></span>

| <span data-ttu-id="90ad4-170">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="90ad4-170">Property</span></span> | <span data-ttu-id="90ad4-171">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-171">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="90ad4-172">base64Value</span><span class="sxs-lookup"><span data-stu-id="90ad4-172">base64Value</span></span> |<span data-ttu-id="90ad4-173">Hodnota Base 64 pro certifikát.</span><span class="sxs-lookup"><span data-stu-id="90ad4-173">Base 64 value for the certificate.</span></span> |
| <span data-ttu-id="90ad4-174">Kryptografický otisk</span><span class="sxs-lookup"><span data-stu-id="90ad4-174">thumbprint</span></span> |<span data-ttu-id="90ad4-175">Kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-175">Thumbprint for the certificate.</span></span> |



## <a name="credentials"></a><span data-ttu-id="90ad4-176">Přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="90ad4-176">Credentials</span></span>
<span data-ttu-id="90ad4-177">[Přihlašovací údaje Azure Automation](../automation/automation-credentials.md) mít typ **Microsoft.Automation/automationAccounts/credentials** a mít následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-177">[Azure Automation credentials](../automation/automation-credentials.md) have a type of **Microsoft.Automation/automationAccounts/credentials** and have the following structure.</span></span>  <span data-ttu-id="90ad4-178">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="90ad4-178">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 


    {
      "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
      "type": "Microsoft.Automation/automationAccounts/credentials",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "userName": "[parameters('credentialUsername')]",
        "password": "[parameters('credentialPassword')]"
      }
    }

<span data-ttu-id="90ad4-179">Vlastnosti přihlašovacích údajů prostředky jsou popsané v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="90ad4-179">The properties for Credential resources are described in the following table.</span></span>

| <span data-ttu-id="90ad4-180">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="90ad4-180">Property</span></span> | <span data-ttu-id="90ad4-181">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-181">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="90ad4-182">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="90ad4-182">userName</span></span> |<span data-ttu-id="90ad4-183">Uživatelské jméno pro přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="90ad4-183">User name for the credential.</span></span> |
| <span data-ttu-id="90ad4-184">heslo</span><span class="sxs-lookup"><span data-stu-id="90ad4-184">password</span></span> |<span data-ttu-id="90ad4-185">Heslo pro přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="90ad4-185">Password for the credential.</span></span> |


## <a name="schedules"></a><span data-ttu-id="90ad4-186">Plány</span><span class="sxs-lookup"><span data-stu-id="90ad4-186">Schedules</span></span>
<span data-ttu-id="90ad4-187">[Azure Automation plány](../automation/automation-schedules.md) mít typ **Microsoft.Automation/automationAccounts/schedules** a mají následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-187">[Azure Automation schedules](../automation/automation-schedules.md) have a type of **Microsoft.Automation/automationAccounts/schedules** and have the the following structure.</span></span> <span data-ttu-id="90ad4-188">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="90ad4-188">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
      "type": "microsoft.automation/automationAccounts/schedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Schedule').Description]",
        "startTime": "[parameters('scheduleStartTime')]",
        "timeZone": "[parameters('scheduleTimeZone')]",
        "isEnabled": "[variables('Schedule').IsEnabled]",
        "interval": "[variables('Schedule').Interval]",
        "frequency": "[variables('Schedule').Frequency]"
      }
    }

<span data-ttu-id="90ad4-189">Vlastnosti pro plán prostředky jsou popsány v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="90ad4-189">The properties for schedule resources are described in the following table.</span></span>

| <span data-ttu-id="90ad4-190">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="90ad4-190">Property</span></span> | <span data-ttu-id="90ad4-191">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-191">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="90ad4-192">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-192">description</span></span> |<span data-ttu-id="90ad4-193">Volitelný popis pro plán.</span><span class="sxs-lookup"><span data-stu-id="90ad4-193">Optional description for the schedule.</span></span> |
| <span data-ttu-id="90ad4-194">startTime</span><span class="sxs-lookup"><span data-stu-id="90ad4-194">startTime</span></span> |<span data-ttu-id="90ad4-195">Určuje počáteční čas plánu jako objekt data a času.</span><span class="sxs-lookup"><span data-stu-id="90ad4-195">Specifies the start time of a schedule as a DateTime object.</span></span> <span data-ttu-id="90ad4-196">Řetězec lze zadat, pokud je možné ji převést na platný datový typ DateTime.</span><span class="sxs-lookup"><span data-stu-id="90ad4-196">A string can be provided if it can be converted to a valid DateTime.</span></span> |
| <span data-ttu-id="90ad4-197">Hodnotu IsEnabled</span><span class="sxs-lookup"><span data-stu-id="90ad4-197">isEnabled</span></span> |<span data-ttu-id="90ad4-198">Určuje, zda je povoleno plán.</span><span class="sxs-lookup"><span data-stu-id="90ad4-198">Specifies whether the schedule is enabled.</span></span> |
| <span data-ttu-id="90ad4-199">Interval</span><span class="sxs-lookup"><span data-stu-id="90ad4-199">interval</span></span> |<span data-ttu-id="90ad4-200">Typ intervalu pro plán.</span><span class="sxs-lookup"><span data-stu-id="90ad4-200">The type of interval for the schedule.</span></span><br><br><span data-ttu-id="90ad4-201">Den</span><span class="sxs-lookup"><span data-stu-id="90ad4-201">day</span></span><br><span data-ttu-id="90ad4-202">Hodina</span><span class="sxs-lookup"><span data-stu-id="90ad4-202">hour</span></span> |
| <span data-ttu-id="90ad4-203">frekvence</span><span class="sxs-lookup"><span data-stu-id="90ad4-203">frequency</span></span> |<span data-ttu-id="90ad4-204">Četnost plán by měl fire počtu dnů nebo hodin.</span><span class="sxs-lookup"><span data-stu-id="90ad4-204">Frequency that the schedule should fire in number of days or hours.</span></span> |

<span data-ttu-id="90ad4-205">Plány musí mít počáteční čas s hodnotou větší než aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="90ad4-205">Schedules must have a start time with a value greater than the current time.</span></span>  <span data-ttu-id="90ad4-206">Tuto hodnotu nelze poskytnout proměnné, vzhledem k tomu, že by měla mít žádný způsob, jak zjistit, kdy se bude nainstalována.</span><span class="sxs-lookup"><span data-stu-id="90ad4-206">You cannot provide this value with a variable since you would have no way of knowing when it's going to be installed.</span></span>

<span data-ttu-id="90ad4-207">Při použití plánu prostředky v řešení, použijte jednu z následujících dvou strategií.</span><span class="sxs-lookup"><span data-stu-id="90ad4-207">Use one of the following two strategies when using schedule resources in a solution.</span></span>

- <span data-ttu-id="90ad4-208">Použijte parametr pro čas spuštění plánu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-208">Use a parameter for the start time of the schedule.</span></span>  <span data-ttu-id="90ad4-209">To se zobrazí výzva k zadání hodnoty při instalaci řešení.</span><span class="sxs-lookup"><span data-stu-id="90ad4-209">This will prompt the user to provide a value when they install the solution.</span></span>  <span data-ttu-id="90ad4-210">Pokud máte více plánů, můžete použít jeden parametr hodnotu pro více než jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="90ad4-210">If you have multiple schedules, you could use a single parameter value for more than one of them.</span></span>
- <span data-ttu-id="90ad4-211">Vytvořte plány pomocí sady runbook, který se spustí při řešení je nainstalováno.</span><span class="sxs-lookup"><span data-stu-id="90ad4-211">Create the schedules using a runbook that starts when the solution is installed.</span></span>  <span data-ttu-id="90ad4-212">To eliminuje požadavek uživatele na určují dobu, ale nemůže obsahovat plán ve vašem řešení, když dojde k odebrání řešení bude odebrán.</span><span class="sxs-lookup"><span data-stu-id="90ad4-212">This removes the requirement of the user to specify a time, but you can't contain the schedule in your solution so it will be removed when the solution is removed.</span></span>


### <a name="job-schedules"></a><span data-ttu-id="90ad4-213">Plány úlohy</span><span class="sxs-lookup"><span data-stu-id="90ad4-213">Job schedules</span></span>
<span data-ttu-id="90ad4-214">Prostředky plán úlohy propojit sady runbook s plánem.</span><span class="sxs-lookup"><span data-stu-id="90ad4-214">Job schedule resources link a runbook with a schedule.</span></span>  <span data-ttu-id="90ad4-215">Mají typ **Microsoft.Automation/automationAccounts/jobSchedules** a mají následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-215">They have a type of **Microsoft.Automation/automationAccounts/jobSchedules** and have the the following structure.</span></span>  <span data-ttu-id="90ad4-216">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="90ad4-216">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
      "type": "microsoft.automation/automationAccounts/jobSchedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
        "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
      ],
      "tags": {
      },
      "properties": {
        "schedule": {
          "name": "[variables('Schedule').Name]"
        },
        "runbook": {
          "name": "[variables('Runbook').Name]"
        }
      }
    }


<span data-ttu-id="90ad4-217">Vlastnosti pro plány úloh jsou popsané v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="90ad4-217">The properties for job schedules are described in the following table.</span></span>

| <span data-ttu-id="90ad4-218">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="90ad4-218">Property</span></span> | <span data-ttu-id="90ad4-219">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-219">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="90ad4-220">Název plánu</span><span class="sxs-lookup"><span data-stu-id="90ad4-220">schedule name</span></span> |<span data-ttu-id="90ad4-221">Jeden **název** entity s názvem plánu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-221">Single **name** entity with the name of the schedule.</span></span> |
| <span data-ttu-id="90ad4-222">Název sady Runbook</span><span class="sxs-lookup"><span data-stu-id="90ad4-222">runbook name</span></span>  |<span data-ttu-id="90ad4-223">Jeden **název** entity se název sady runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-223">Single **name** entity with the name of the runbook.</span></span>  |



## <a name="variables"></a><span data-ttu-id="90ad4-224">Proměnné</span><span class="sxs-lookup"><span data-stu-id="90ad4-224">Variables</span></span>
<span data-ttu-id="90ad4-225">[Azure Automation proměnné](../automation/automation-variables.md) mít typ **Microsoft.Automation/automationAccounts/variables** a mít následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-225">[Azure Automation variables](../automation/automation-variables.md) have a type of **Microsoft.Automation/automationAccounts/variables** and have the following structure.</span></span>  <span data-ttu-id="90ad4-226">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="90ad4-226">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span>

    {
      "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
      "type": "microsoft.automation/automationAccounts/variables",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Variable').Description]",
        "isEncrypted": "[variables('Variable').Encrypted]",
        "type": "[variables('Variable').Type]",
        "value": "[variables('Variable').Value]"
      }
    }

<span data-ttu-id="90ad4-227">Vlastnosti pro proměnné prostředky jsou popsány v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="90ad4-227">The properties for variable resources are described in the following table.</span></span>

| <span data-ttu-id="90ad4-228">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="90ad4-228">Property</span></span> | <span data-ttu-id="90ad4-229">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-229">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="90ad4-230">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-230">description</span></span> | <span data-ttu-id="90ad4-231">Volitelný popis pro proměnnou.</span><span class="sxs-lookup"><span data-stu-id="90ad4-231">Optional description for the variable.</span></span> |
| <span data-ttu-id="90ad4-232">isEncrypted</span><span class="sxs-lookup"><span data-stu-id="90ad4-232">isEncrypted</span></span> | <span data-ttu-id="90ad4-233">Určuje, jestli by se proměnná šifrovat.</span><span class="sxs-lookup"><span data-stu-id="90ad4-233">Specifies whether the variable should be encrypted.</span></span> |
| <span data-ttu-id="90ad4-234">type</span><span class="sxs-lookup"><span data-stu-id="90ad4-234">type</span></span> | <span data-ttu-id="90ad4-235">Tato vlastnost aktuálně nemá žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="90ad4-235">This property currently has no effect.</span></span>  <span data-ttu-id="90ad4-236">Datový typ proměnné určí počáteční hodnota.</span><span class="sxs-lookup"><span data-stu-id="90ad4-236">The data type of the variable will be determined by the initial value.</span></span> |
| <span data-ttu-id="90ad4-237">hodnota</span><span class="sxs-lookup"><span data-stu-id="90ad4-237">value</span></span> | <span data-ttu-id="90ad4-238">Hodnota proměnné.</span><span class="sxs-lookup"><span data-stu-id="90ad4-238">Value for the variable.</span></span> |

> [!NOTE]
> <span data-ttu-id="90ad4-239">**Typ** vlastnost aktuálně nemá žádný vliv na proměnnou vytváří.</span><span class="sxs-lookup"><span data-stu-id="90ad4-239">The **type** property currently has no effect on the variable being created.</span></span>  <span data-ttu-id="90ad4-240">Hodnota závisí na datový typ pro proměnnou.</span><span class="sxs-lookup"><span data-stu-id="90ad4-240">The data type for the variable will be determined by the value.</span></span>  

<span data-ttu-id="90ad4-241">Pokud jste nastavili počáteční hodnotu pro proměnnou, musí být nakonfigurované jako správného datového typu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-241">If you set the initial value for the variable, it must be configured as the correct data type.</span></span>  <span data-ttu-id="90ad4-242">Následující tabulka obsahuje různé datové typy, které jsou povolené a jejich syntaxi.</span><span class="sxs-lookup"><span data-stu-id="90ad4-242">The following table provides the different data types allowable and their syntax.</span></span>  <span data-ttu-id="90ad4-243">Všimněte si, že se hodnoty ve formátu JSON očekává vždycky být uzavřena v uvozovkách s žádné speciální znaky v rámci uvozovky.</span><span class="sxs-lookup"><span data-stu-id="90ad4-243">Note that values in JSON are expected to always be enclosed in quotes with any special characters within the quotes.</span></span>  <span data-ttu-id="90ad4-244">Například by být řetězcová hodnota určena řetězec v uvozovkách (pomocí řídicí znak (\\)) číselnou hodnotu by zadán s jednu sadu uvozovky.</span><span class="sxs-lookup"><span data-stu-id="90ad4-244">For example, a string value would be specified by quotes around the string (using the escape character (\\)) while a numeric value would be specified with one set of quotes.</span></span>

| <span data-ttu-id="90ad4-245">Datový typ</span><span class="sxs-lookup"><span data-stu-id="90ad4-245">Data type</span></span> | <span data-ttu-id="90ad4-246">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-246">Description</span></span> | <span data-ttu-id="90ad4-247">Příklad</span><span class="sxs-lookup"><span data-stu-id="90ad4-247">Example</span></span> | <span data-ttu-id="90ad4-248">Přeloží na</span><span class="sxs-lookup"><span data-stu-id="90ad4-248">Resolves to</span></span> |
|:--|:--|:--|:--|
| <span data-ttu-id="90ad4-249">Řetězec</span><span class="sxs-lookup"><span data-stu-id="90ad4-249">string</span></span>   | <span data-ttu-id="90ad4-250">Vložte hodnotu do dvojitých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="90ad4-250">Enclose value in double quotes.</span></span>  | <span data-ttu-id="90ad4-251">"\"Hello, world\""</span><span class="sxs-lookup"><span data-stu-id="90ad4-251">"\"Hello world\""</span></span> | <span data-ttu-id="90ad4-252">"Hello, world"</span><span class="sxs-lookup"><span data-stu-id="90ad4-252">"Hello world"</span></span> |
| <span data-ttu-id="90ad4-253">číselné</span><span class="sxs-lookup"><span data-stu-id="90ad4-253">numeric</span></span>  | <span data-ttu-id="90ad4-254">Číselná hodnota se jednoduchých uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="90ad4-254">Numeric value with single quotes.</span></span>| <span data-ttu-id="90ad4-255">"64"</span><span class="sxs-lookup"><span data-stu-id="90ad4-255">"64"</span></span> | <span data-ttu-id="90ad4-256">64</span><span class="sxs-lookup"><span data-stu-id="90ad4-256">64</span></span> |
| <span data-ttu-id="90ad4-257">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="90ad4-257">boolean</span></span>  | <span data-ttu-id="90ad4-258">**Hodnota TRUE,** nebo **false** v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="90ad4-258">**true** or **false** in quotes.</span></span>  <span data-ttu-id="90ad4-259">Všimněte si, že tato hodnota musí být malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="90ad4-259">Note that this value must be lowercase.</span></span> | <span data-ttu-id="90ad4-260">"true"</span><span class="sxs-lookup"><span data-stu-id="90ad4-260">"true"</span></span> | <span data-ttu-id="90ad4-261">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="90ad4-261">true</span></span> |
| <span data-ttu-id="90ad4-262">Data a času</span><span class="sxs-lookup"><span data-stu-id="90ad4-262">datetime</span></span> | <span data-ttu-id="90ad4-263">Hodnota serializovaná data.</span><span class="sxs-lookup"><span data-stu-id="90ad4-263">Serialized date value.</span></span><br><span data-ttu-id="90ad4-264">Můžete použít rutinu ConvertTo-Json v prostředí PowerShell k vygenerování této hodnoty pro konkrétní datum.</span><span class="sxs-lookup"><span data-stu-id="90ad4-264">You can use the ConvertTo-Json cmdlet in PowerShell to generate this value for a particular date.</span></span><br><span data-ttu-id="90ad4-265">Příklad: get datum "5/24/2017 13:14:57" \\</span><span class="sxs-lookup"><span data-stu-id="90ad4-265">Example: get-date "5/24/2017 13:14:57" \\</span></span>| <span data-ttu-id="90ad4-266">ConvertTo-Json</span><span class="sxs-lookup"><span data-stu-id="90ad4-266">ConvertTo-Json</span></span> | <span data-ttu-id="90ad4-267">"\\/Date(1495656897378)\\/"</span><span class="sxs-lookup"><span data-stu-id="90ad4-267">"\\/Date(1495656897378)\\/"</span></span> | <span data-ttu-id="90ad4-268">2017-05-24 13:14:57</span><span class="sxs-lookup"><span data-stu-id="90ad4-268">2017-05-24 13:14:57</span></span> |

## <a name="modules"></a><span data-ttu-id="90ad4-269">Moduly</span><span class="sxs-lookup"><span data-stu-id="90ad4-269">Modules</span></span>
<span data-ttu-id="90ad4-270">Řešení pro správu není nutné definovat [globální moduly](../automation/automation-integration-modules.md) použít ve vašich sadách runbook, protože se budou vždy k dispozici ve vašem účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="90ad4-270">Your management solution does not need to define [global modules](../automation/automation-integration-modules.md) used by your runbooks because they will always be available in your Automation account.</span></span>  <span data-ttu-id="90ad4-271">Musíte zahrnout prostředku pro ostatní moduly používané vaší sady runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-271">You do need to include a resource for any other module used by your runbooks.</span></span>

<span data-ttu-id="90ad4-272">[Integrační moduly](../automation/automation-integration-modules.md) mít typ **Microsoft.Automation/automationAccounts/modules** a mít následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-272">[Integration modules](../automation/automation-integration-modules.md) have a type of **Microsoft.Automation/automationAccounts/modules** and have the following structure.</span></span>  <span data-ttu-id="90ad4-273">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="90ad4-273">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span>

    {
      "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
      "type": "Microsoft.Automation/automationAccounts/modules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "dependsOn": [
      ],
      "properties": {
        "contentLink": {
          "uri": "[variables('Module').Uri]"
        }
      }
    }


<span data-ttu-id="90ad4-274">Vlastnosti modulu prostředky jsou popsané v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="90ad4-274">The properties for module resources are described in the following table.</span></span>

| <span data-ttu-id="90ad4-275">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="90ad4-275">Property</span></span> | <span data-ttu-id="90ad4-276">Popis</span><span class="sxs-lookup"><span data-stu-id="90ad4-276">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="90ad4-277">contentLink</span><span class="sxs-lookup"><span data-stu-id="90ad4-277">contentLink</span></span> |<span data-ttu-id="90ad4-278">Určuje obsah modulu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-278">Specifies the content of the module.</span></span> <br><br><span data-ttu-id="90ad4-279">identifikátor URI – identifikátor Uri obsahu modulu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-279">uri - Uri to the content of the module.</span></span>  <span data-ttu-id="90ad4-280">Bude jím souboru s příponou .ps1 pro prostředí PowerShell a skript sady runbook a soubor exportovaný grafický runbook pro sadu runbook grafu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-280">This will be a .ps1 file for PowerShell and Script runbooks, and an exported graphical runbook file for a Graph runbook.</span></span>  <br> <span data-ttu-id="90ad4-281">verze - verze modulu pro vlastní sledování.</span><span class="sxs-lookup"><span data-stu-id="90ad4-281">version - Version of the module for your own tracking.</span></span> |

<span data-ttu-id="90ad4-282">Sada runbook by měl závisí na modulu prostředků a ověřte, že je vytvořen před sady runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-282">The runbook should depend on the module resource to ensure that it's created before the runbook.</span></span>

### <a name="updating-modules"></a><span data-ttu-id="90ad4-283">Aktualizace moduly</span><span class="sxs-lookup"><span data-stu-id="90ad4-283">Updating modules</span></span>
<span data-ttu-id="90ad4-284">Pokud aktualizujete řešení pro správu, která zahrnuje sadu runbook, která používá plánu a novou verzi řešení má nové modulu používá dané sady runbook, může používat runbook stará verze modulu.</span><span class="sxs-lookup"><span data-stu-id="90ad4-284">If you update a management solution that includes a runbook that uses a schedule, and the new version of your solution has a new module used by that runbook, then the runbook may use the old version of the module.</span></span>  <span data-ttu-id="90ad4-285">Musí zahrnovat následující sady runbook ve vašem řešení a vytvořit úlohu, abyste je mohli spustit před jiné runbooky.</span><span class="sxs-lookup"><span data-stu-id="90ad4-285">You should include the following runbooks in your solution and create a job to run them before any other runbooks.</span></span>  <span data-ttu-id="90ad4-286">To zajistí, že jsou všechny moduly, aktualizovat, protože požadované než sady runbook jsou načtena.</span><span class="sxs-lookup"><span data-stu-id="90ad4-286">This will ensure that any modules are updated as required before the runbooks are loaded.</span></span>

* <span data-ttu-id="90ad4-287">[Aktualizace ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) zajistí, že jsou všechny moduly používané v sadách runbook ve vašem řešení na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="90ad4-287">[Update-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) will ensure that all of the modules used by runbooks in your solution are the latest version.</span></span>  
* <span data-ttu-id="90ad4-288">[ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) bude znovu registrovat všechny prostředky plán zajistit, že sady runbook s nimi propojené s použitím nejnovější moduly.</span><span class="sxs-lookup"><span data-stu-id="90ad4-288">[ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) will reregister all of the schedule resources to ensure that the runbooks linked to them with use the latest modules.</span></span>




## <a name="sample"></a><span data-ttu-id="90ad4-289">Ukázka</span><span class="sxs-lookup"><span data-stu-id="90ad4-289">Sample</span></span>
<span data-ttu-id="90ad4-290">Tady je ukázka řešení, které zahrnují, který obsahuje následující zdroje:</span><span class="sxs-lookup"><span data-stu-id="90ad4-290">Following is a sample of a solution that include that includes the following resources:</span></span>

- <span data-ttu-id="90ad4-291">Sady Runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-291">Runbook.</span></span>  <span data-ttu-id="90ad4-292">Toto je uložen v úložišti GitHub, které veřejné vzorové sady runbook.</span><span class="sxs-lookup"><span data-stu-id="90ad4-292">This is a sample runbook stored in a public GitHub repository.</span></span>
- <span data-ttu-id="90ad4-293">Úlohu automatizace, který se spustí sada runbook při řešení je nainstalováno.</span><span class="sxs-lookup"><span data-stu-id="90ad4-293">Automation job that starts the runbook when the solution is installed.</span></span>
- <span data-ttu-id="90ad4-294">Plán a plán úlohy pro spuštění sady runbook v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="90ad4-294">Schedule and job schedule to start the runbook at regular intervals.</span></span>
- <span data-ttu-id="90ad4-295">Certifikát.</span><span class="sxs-lookup"><span data-stu-id="90ad4-295">Certificate.</span></span>
- <span data-ttu-id="90ad4-296">Přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="90ad4-296">Credential.</span></span>
- <span data-ttu-id="90ad4-297">Proměnná.</span><span class="sxs-lookup"><span data-stu-id="90ad4-297">Variable.</span></span>
- <span data-ttu-id="90ad4-298">Modul.</span><span class="sxs-lookup"><span data-stu-id="90ad4-298">Module.</span></span>  <span data-ttu-id="90ad4-299">Toto je [OMSIngestionAPI modulu](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) pro zápis dat k analýze protokolů.</span><span class="sxs-lookup"><span data-stu-id="90ad4-299">This is the [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) for writing data to Log Analytics.</span></span> 

<span data-ttu-id="90ad4-300">Příklad používá [standardní řešení parametry](operations-management-suite-solutions-solution-file.md#parameters) proměnné, které by běžně používané v řešení oproti hardcoding hodnoty v definicích prostředků.</span><span class="sxs-lookup"><span data-stu-id="90ad4-300">The sample uses [standard solution parameters](operations-management-suite-solutions-solution-file.md#parameters) variables that would commonly be used in a solution as opposed to hardcoding values in the resource definitions.</span></span>


    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Log Analytics workspace."
          }
        },
        "accountName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Automation account."
          }
        },
        "workspaceregionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Log Analytics workspace."
          }
        },
        "regionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Automation account."
          }
        },
        "pricingTier": {
          "type": "string",
          "metadata": {
            "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account."
          }
        },
        "certificateBase64Value": {
          "type": "string",
          "metadata": {
            "Description": "Base 64 value for certificate."
          }
        },
        "certificateThumbprint": {
          "type": "securestring",
          "metadata": {
            "Description": "Thumbprint for certificate."
          }
        },
        "credentialUsername": {
          "type": "string",
          "metadata": {
            "Description": "Username for credential."
          }
        },
        "credentialPassword": {
          "type": "securestring",
          "metadata": {
            "Description": "Password for credential."
          }
        },
        "scheduleStartTime": {
          "type": "string",
          "metadata": {
            "Description": "Start time for shedule."
          }
        },
        "scheduleTimeZone": {
          "type": "string",
          "metadata": {
            "Description": "Time zone for schedule."
          }
        },
        "scheduleLinkGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the schedule link to runbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for the runbook job.",
            "control": "guid"
          }
        }
      },
      "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
    
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31",
    
        "Runbook": {
          "Name": "MyRunbook",
          "Description": "Sample runbook",
          "Type": "PowerShell",
          "Uri": "https://raw.githubusercontent.com/user/myrepo/master/samples/MyRunbook.ps1",
          "JobGuid": "[parameters('runbookJobGuid')]"
        },
    
        "Certificate": {
          "Name": "MyCertificate",
          "Base64Value": "[parameters('certificateBase64Value')]",
          "Thumbprint": "[parameters('certificateThumbprint')]"
        },
    
        "Credential": {
          "Name": "MyCredential",
          "UserName": "[parameters('credentialUsername')]",
          "Password": "[parameters('credentialPassword')]"
        },
    
        "Schedule": {
          "Name": "MySchedule",
          "Description": "Sample schedule",
          "IsEnabled": "true",
          "Interval": "1",
          "Frequency": "hour",
          "StartTime": "[parameters('scheduleStartTime')]",
          "TimeZone": "[parameters('scheduleTimeZone')]",
          "LinkGuid": "[parameters('scheduleLinkGuid')]"
        },
    
        "Variable": {
          "Name": "MyVariable",
          "Description": "Sample variable",
          "Encrypted": 0,
          "Type": "string",
          "Value": "'This is my string value.'"
        },
    
        "Module": {
          "Name": "OMSIngestionAPI",
          "Uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
        }
      },
      "resources": [
        {
          "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
          "location": "[parameters('workspaceRegionId')]",
          "tags": { },
          "type": "Microsoft.OperationsManagement/solutions",
          "apiVersion": "[variables('LogAnalyticsApiVersion')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
            "referencedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
            ],
            "containedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]"
            ]
          },
          "plan": {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
            "Version": "[variables('SolutionVersion')]",
            "product": "[variables('ProductName')]",
            "publisher": "[variables('SolutionPublisher')]",
            "promotionCode": ""
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
              "uri": "[variables('Runbook').Uri]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').JobGuid)]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('Runbook').Name]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
          "type": "Microsoft.Automation/automationAccounts/certificates",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "Base64Value": "[variables('Certificate').Base64Value]",
            "Thumbprint": "[variables('Certificate').Thumbprint]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
          "type": "Microsoft.Automation/automationAccounts/credentials",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "userName": "[variables('Credential').UserName]",
            "password": "[variables('Credential').Password]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
          "type": "microsoft.automation/automationAccounts/schedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Schedule').Description]",
            "startTime": "[variables('Schedule').StartTime]",
            "timeZone": "[variables('Schedule').TimeZone]",
            "isEnabled": "[variables('Schedule').IsEnabled]",
            "interval": "[variables('Schedule').Interval]",
            "frequency": "[variables('Schedule').Frequency]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
          "type": "microsoft.automation/automationAccounts/jobSchedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
          ],
          "tags": {
          },
          "properties": {
            "schedule": {
              "name": "[variables('Schedule').Name]"
            },
            "runbook": {
              "name": "[variables('Runbook').Name]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
          "type": "microsoft.automation/automationAccounts/variables",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Variable').Description]",
            "isEncrypted": "[variables('Variable').Encrypted]",
            "type": "[variables('Variable').Type]",
            "value": "[variables('Variable').Value]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
          "type": "Microsoft.Automation/automationAccounts/modules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "properties": {
            "contentLink": {
              "uri": "[variables('Module').Uri]"
            }
          }
        }
    
      ],
      "outputs": { }
    }




## <a name="next-steps"></a><span data-ttu-id="90ad4-301">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90ad4-301">Next steps</span></span>
* <span data-ttu-id="90ad4-302">[Přidat zobrazení do řešení](operations-management-suite-solutions-resources-views.md) k vizualizaci shromážděná data.</span><span class="sxs-lookup"><span data-stu-id="90ad4-302">[Add a view to your solution](operations-management-suite-solutions-resources-views.md) to visualize collected data.</span></span>
