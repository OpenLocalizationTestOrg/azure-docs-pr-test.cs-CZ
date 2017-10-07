---
title: "prostředky služby Automation aaaAzure OMS řešení | Microsoft Docs"
description: "Řešení v OMS by měl obvykle zahrnovat sady runbook v Azure Automation tooautomate procesy, jako je shromažďování a zpracování dat monitorování.  Tento článek popisuje, jak tooinclude sady runbook a jejich související prostředky v řešení."
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
ms.openlocfilehash: 82a156f89bf77ce25e52e5e4596261ec07a24dae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-automation-resources-tooan-oms-management-solution-preview"></a><span data-ttu-id="753f8-104">Přidání řešení správy OMS tooan prostředky Azure Automation (Preview)</span><span class="sxs-lookup"><span data-stu-id="753f8-104">Adding Azure Automation resources tooan OMS management solution (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="753f8-105">Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="753f8-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="753f8-106">Žádné schéma níže popsané je toochange subjektu.</span><span class="sxs-lookup"><span data-stu-id="753f8-106">Any schema described below is subject toochange.</span></span>   


<span data-ttu-id="753f8-107">[Řešení pro správu v OMS](operations-management-suite-solutions.md) by měl obvykle zahrnovat sady runbook v Azure Automation tooautomate procesy, jako je shromažďování a zpracování dat monitorování.</span><span class="sxs-lookup"><span data-stu-id="753f8-107">[Management solutions in OMS](operations-management-suite-solutions.md) will typically include runbooks in Azure Automation tooautomate processes such as collecting and processing monitoring data.</span></span>  <span data-ttu-id="753f8-108">Kromě toorunbooks, účty služby Automation obsahuje prostředky, jako jsou proměnné a plány, které podporují sady runbook hello používá v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-108">In addition toorunbooks, Automation accounts includes assets such as variables and schedules that support hello runbooks used in hello solution.</span></span>  <span data-ttu-id="753f8-109">Tento článek popisuje, jak tooinclude sady runbook a jejich související prostředky v řešení.</span><span class="sxs-lookup"><span data-stu-id="753f8-109">This article describes how tooinclude runbooks and their related resources in a solution.</span></span>

> [!NOTE]
> <span data-ttu-id="753f8-110">Použijte parametry a proměnné, které jsou buď požadované, nebo běžné toomanagement řešení a popsané v zprostředkovatele Hello ukázky v tomto článku [vytváření řešení pro správu v Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="753f8-110">hello samples in this article use parameters and variables that are either required or common toomanagement solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="753f8-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="753f8-111">Prerequisites</span></span>
<span data-ttu-id="753f8-112">Tento článek předpokládá, že jste již obeznámeni s hello následující informace.</span><span class="sxs-lookup"><span data-stu-id="753f8-112">This article assumes that you're already familiar with hello following information.</span></span>

- <span data-ttu-id="753f8-113">Jak příliš[vytvoření řešení správy](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="753f8-113">How too[create a management solution](operations-management-suite-solutions-creating.md).</span></span>
- <span data-ttu-id="753f8-114">Hello struktura [soubor řešení](operations-management-suite-solutions-solution-file.md).</span><span class="sxs-lookup"><span data-stu-id="753f8-114">hello structure of a [solution file](operations-management-suite-solutions-solution-file.md).</span></span>
- <span data-ttu-id="753f8-115">Jak příliš[vytváření šablon Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="753f8-115">How too[author Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md)</span></span>

## <a name="automation-account"></a><span data-ttu-id="753f8-116">Účet Automation</span><span class="sxs-lookup"><span data-stu-id="753f8-116">Automation account</span></span>
<span data-ttu-id="753f8-117">Všechny prostředky ve službě Azure Automation jsou součástí [účet Automation](../automation/automation-security-overview.md#automation-account-overview).</span><span class="sxs-lookup"><span data-stu-id="753f8-117">All resources in Azure Automation are contained in an [Automation account](../automation/automation-security-overview.md#automation-account-overview).</span></span>  <span data-ttu-id="753f8-118">Jak je popsáno v [OMS pracovní prostor a účet Automation](operations-management-suite-solutions.md#oms-workspace-and-automation-account) není zahrnutý v řešení pro správu hello hello účet Automation, ale musí existovat před instalací hello řešení.</span><span class="sxs-lookup"><span data-stu-id="753f8-118">As described in [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello Automation account isn't included in hello management solution but must exist before hello solution is installed.</span></span>  <span data-ttu-id="753f8-119">Pokud není k dispozici, se nezdaří instalace řešení hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-119">If it isn't available, then hello solution install will fail.</span></span>

<span data-ttu-id="753f8-120">Hello název každého prostředku automatizace obsahuje název hello jeho účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="753f8-120">hello name of each Automation resource includes hello name of its Automation account.</span></span>  <span data-ttu-id="753f8-121">To se provádí v hello řešení s hello **accountName** parametr jako hello následující ukázka runbook prostředku.</span><span class="sxs-lookup"><span data-stu-id="753f8-121">This is done in hello solution with hello **accountName** parameter as in hello following example of a runbook resource.</span></span>

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a><span data-ttu-id="753f8-122">Runbooky</span><span class="sxs-lookup"><span data-stu-id="753f8-122">Runbooks</span></span>
<span data-ttu-id="753f8-123">By měly obsahovat všechny sady runbook používá hello řešení v souboru hello řešení tak, aby se vytváří při instalaci hello řešení.</span><span class="sxs-lookup"><span data-stu-id="753f8-123">You should include any runbooks used by hello solution in hello solution file so that they're created when hello solution is installed.</span></span>  <span data-ttu-id="753f8-124">Hello textu hello sady runbook v šabloně hello nemůže obsahovat Přestože, proto byste měli publikovat hello runbook tooa veřejném místě kde je přístupná žádný uživatel instalace řešení.</span><span class="sxs-lookup"><span data-stu-id="753f8-124">You cannot contain hello body of hello runbook in hello template though, so you should publish hello runbook tooa public location where it can be accessed by any user installing your solution.</span></span>

<span data-ttu-id="753f8-125">[Azure Automation runbook](../automation/automation-runbook-types.md) prostředky mít typ **Microsoft.Automation/automationAccounts/runbooks** a mít hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="753f8-125">[Azure Automation runbook](../automation/automation-runbook-types.md) resources have a type of **Microsoft.Automation/automationAccounts/runbooks** and have hello following structure.</span></span> <span data-ttu-id="753f8-126">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-126">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

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


<span data-ttu-id="753f8-127">Hello vlastnosti pro sady runbook jsou popsány v následující tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-127">hello properties for runbooks are described in hello following table.</span></span>

| <span data-ttu-id="753f8-128">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="753f8-128">Property</span></span> | <span data-ttu-id="753f8-129">Popis</span><span class="sxs-lookup"><span data-stu-id="753f8-129">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="753f8-130">runbookType</span><span class="sxs-lookup"><span data-stu-id="753f8-130">runbookType</span></span> |<span data-ttu-id="753f8-131">Určuje typy hello hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-131">Specifies hello types of hello runbook.</span></span> <br><br> <span data-ttu-id="753f8-132">Skript - skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="753f8-132">Script - PowerShell script</span></span> <br><span data-ttu-id="753f8-133">PowerShell – pracovní postup prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="753f8-133">PowerShell - PowerShell workflow</span></span> <br> <span data-ttu-id="753f8-134">GraphPowerShell - runbook skriptu grafické prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="753f8-134">GraphPowerShell - Graphical PowerShell script runbook</span></span> <br> <span data-ttu-id="753f8-135">GraphPowerShellWorkflow - runbook pracovního postupu grafické prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="753f8-135">GraphPowerShellWorkflow - Graphical PowerShell workflow runbook</span></span> |
| <span data-ttu-id="753f8-136">logProgress</span><span class="sxs-lookup"><span data-stu-id="753f8-136">logProgress</span></span> |<span data-ttu-id="753f8-137">Určuje, zda [průběhu záznamy](../automation/automation-runbook-output-and-messages.md) by měl být vygenerován hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-137">Specifies whether [progress records](../automation/automation-runbook-output-and-messages.md) should be generated for hello runbook.</span></span> |
| <span data-ttu-id="753f8-138">logVerbose</span><span class="sxs-lookup"><span data-stu-id="753f8-138">logVerbose</span></span> |<span data-ttu-id="753f8-139">Určuje, zda [podrobných záznamů](../automation/automation-runbook-output-and-messages.md) by měl být vygenerován hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-139">Specifies whether [verbose records](../automation/automation-runbook-output-and-messages.md) should be generated for hello runbook.</span></span> |
| <span data-ttu-id="753f8-140">description</span><span class="sxs-lookup"><span data-stu-id="753f8-140">description</span></span> |<span data-ttu-id="753f8-141">Volitelný popis pro sadu runbook hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-141">Optional description for hello runbook.</span></span> |
| <span data-ttu-id="753f8-142">publishContentLink</span><span class="sxs-lookup"><span data-stu-id="753f8-142">publishContentLink</span></span> |<span data-ttu-id="753f8-143">Určuje hello obsah sady runbook hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-143">Specifies hello content of hello runbook.</span></span> <br><br><span data-ttu-id="753f8-144">identifikátor URI - toohello obsah identifikátoru Uri hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-144">uri - Uri toohello content of hello runbook.</span></span>  <span data-ttu-id="753f8-145">Bude jím souboru s příponou .ps1 pro prostředí PowerShell a skript sady runbook a soubor exportovaný grafický runbook pro sadu runbook grafu.</span><span class="sxs-lookup"><span data-stu-id="753f8-145">This will be a .ps1 file for PowerShell and Script runbooks, and an exported graphical runbook file for a Graph runbook.</span></span>  <br> <span data-ttu-id="753f8-146">verze - verzi hello runbook pro vlastní sledování.</span><span class="sxs-lookup"><span data-stu-id="753f8-146">version - Version of hello runbook for your own tracking.</span></span> |


## <a name="automation-jobs"></a><span data-ttu-id="753f8-147">Automatizace úloh</span><span class="sxs-lookup"><span data-stu-id="753f8-147">Automation jobs</span></span>
<span data-ttu-id="753f8-148">Když spustíte runbook ve službě Azure Automation, vytvoří úlohu automatizace.</span><span class="sxs-lookup"><span data-stu-id="753f8-148">When you start a runbook in Azure Automation, it creates an automation job.</span></span>  <span data-ttu-id="753f8-149">Můžete přidat automatizaci úlohy prostředků tooyour řešení tooautomatically spuštění sady runbook při instalaci hello řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="753f8-149">You can add an automation job resource tooyour solution tooautomatically start a runbook when hello management solution is installed.</span></span>  <span data-ttu-id="753f8-150">Tato metoda je obvykle používanými toostart sady runbook, které se používají pro počáteční konfiguraci hello řešení.</span><span class="sxs-lookup"><span data-stu-id="753f8-150">This method is typically used toostart runbooks that are used for initial configuration of hello solution.</span></span>  <span data-ttu-id="753f8-151">Vytvoření toostart sady runbook v pravidelných intervalech [plán](#schedules) a [plán úloh](#job-schedules)</span><span class="sxs-lookup"><span data-stu-id="753f8-151">toostart a runbook at regular intervals, create a [schedule](#schedules) and a [job schedule](#job-schedules)</span></span>

<span data-ttu-id="753f8-152">Úloha prostředky mít typ **Microsoft.Automation/automationAccounts/jobs** a mít hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="753f8-152">Job resources have a type of **Microsoft.Automation/automationAccounts/jobs** and have hello following structure.</span></span>  <span data-ttu-id="753f8-153">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-153">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

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

<span data-ttu-id="753f8-154">Hello vlastnosti pro automatizaci úloh jsou popsané v následující tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-154">hello properties for automation jobs are described in hello following table.</span></span>

| <span data-ttu-id="753f8-155">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="753f8-155">Property</span></span> | <span data-ttu-id="753f8-156">Popis</span><span class="sxs-lookup"><span data-stu-id="753f8-156">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="753f8-157">sady runbook</span><span class="sxs-lookup"><span data-stu-id="753f8-157">runbook</span></span> |<span data-ttu-id="753f8-158">Jeden název entity s názvem hello hello runbook toostart.</span><span class="sxs-lookup"><span data-stu-id="753f8-158">Single name entity with hello name of hello runbook toostart.</span></span> |
| <span data-ttu-id="753f8-159">parameters</span><span class="sxs-lookup"><span data-stu-id="753f8-159">parameters</span></span> |<span data-ttu-id="753f8-160">Entita pro každou hodnotu parametru vyžadovanou hello runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-160">Entity for each parameter value required by hello runbook.</span></span> |

<span data-ttu-id="753f8-161">Úloha Hello zahrnuje hello název sady runbook a všechny toobe hodnoty parametru odeslané toohello runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-161">hello job includes hello runbook name and any parameter values toobe sent toohello runbook.</span></span>  <span data-ttu-id="753f8-162">úlohy Hello by [závisí na](operations-management-suite-solutions-solution-file.md#resources) hello runbook, která se spouští od hello runbook musí být vytvořeny před hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="753f8-162">hello job should [depend on](operations-management-suite-solutions-solution-file.md#resources) hello runbook that it's starting since hello runbook must be created before hello job.</span></span>  <span data-ttu-id="753f8-163">Pokud máte více sad runbook, který by měl být spuštěn můžete definovat jejich pořadí tak, že úloha závisí na jiné úlohy, které by měl být spuštěn první.</span><span class="sxs-lookup"><span data-stu-id="753f8-163">If you have multiple runbooks that should be started you can define their order by having a job depend on any other jobs that should be run first.</span></span>

<span data-ttu-id="753f8-164">Hello název prostředku úlohy musí obsahovat identifikátor GUID, který je obvykle přiřadila parametr.</span><span class="sxs-lookup"><span data-stu-id="753f8-164">hello name of a job resource must contain a GUID which is typically assigned by a parameter.</span></span>  <span data-ttu-id="753f8-165">Další informace o parametrech identifikátor GUID v [vytváření řešení v Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="753f8-165">You can read more about GUID parameters in [Creating solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).</span></span>  


## <a name="certificates"></a><span data-ttu-id="753f8-166">Certifikáty</span><span class="sxs-lookup"><span data-stu-id="753f8-166">Certificates</span></span>
<span data-ttu-id="753f8-167">[Azure Automation certifikáty](../automation/automation-certificates.md) mít typ **Microsoft.Automation/automationAccounts/certificates** a mít hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="753f8-167">[Azure Automation certificates](../automation/automation-certificates.md) have a type of **Microsoft.Automation/automationAccounts/certificates** and have hello following structure.</span></span> <span data-ttu-id="753f8-168">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-168">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

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



<span data-ttu-id="753f8-169">Hello vlastnosti prostředků certifikáty jsou popsané v následující tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-169">hello properties for Certificates resources are described in hello following table.</span></span>

| <span data-ttu-id="753f8-170">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="753f8-170">Property</span></span> | <span data-ttu-id="753f8-171">Popis</span><span class="sxs-lookup"><span data-stu-id="753f8-171">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="753f8-172">base64Value</span><span class="sxs-lookup"><span data-stu-id="753f8-172">base64Value</span></span> |<span data-ttu-id="753f8-173">Hodnota Base 64 hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="753f8-173">Base 64 value for hello certificate.</span></span> |
| <span data-ttu-id="753f8-174">Kryptografický otisk</span><span class="sxs-lookup"><span data-stu-id="753f8-174">thumbprint</span></span> |<span data-ttu-id="753f8-175">Kryptografický otisk certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-175">Thumbprint for hello certificate.</span></span> |



## <a name="credentials"></a><span data-ttu-id="753f8-176">Přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="753f8-176">Credentials</span></span>
<span data-ttu-id="753f8-177">[Přihlašovací údaje Azure Automation](../automation/automation-credentials.md) mít typ **Microsoft.Automation/automationAccounts/credentials** a mít hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="753f8-177">[Azure Automation credentials](../automation/automation-credentials.md) have a type of **Microsoft.Automation/automationAccounts/credentials** and have hello following structure.</span></span>  <span data-ttu-id="753f8-178">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-178">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 


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

<span data-ttu-id="753f8-179">Hello vlastnosti prostředků přihlašovacích údajů jsou popsané v následující tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-179">hello properties for Credential resources are described in hello following table.</span></span>

| <span data-ttu-id="753f8-180">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="753f8-180">Property</span></span> | <span data-ttu-id="753f8-181">Popis</span><span class="sxs-lookup"><span data-stu-id="753f8-181">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="753f8-182">Uživatelské jméno</span><span class="sxs-lookup"><span data-stu-id="753f8-182">userName</span></span> |<span data-ttu-id="753f8-183">Uživatelské jméno pro přihlašovací údaje hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-183">User name for hello credential.</span></span> |
| <span data-ttu-id="753f8-184">heslo</span><span class="sxs-lookup"><span data-stu-id="753f8-184">password</span></span> |<span data-ttu-id="753f8-185">Heslo pro přihlašovací údaje hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-185">Password for hello credential.</span></span> |


## <a name="schedules"></a><span data-ttu-id="753f8-186">Plány</span><span class="sxs-lookup"><span data-stu-id="753f8-186">Schedules</span></span>
<span data-ttu-id="753f8-187">[Azure Automation plány](../automation/automation-schedules.md) mít typ **Microsoft.Automation/automationAccounts/schedules** a mít hello hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="753f8-187">[Azure Automation schedules](../automation/automation-schedules.md) have a type of **Microsoft.Automation/automationAccounts/schedules** and have hello hello following structure.</span></span> <span data-ttu-id="753f8-188">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-188">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

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

<span data-ttu-id="753f8-189">Hello vlastnosti pro plán prostředky jsou popsané v následující tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-189">hello properties for schedule resources are described in hello following table.</span></span>

| <span data-ttu-id="753f8-190">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="753f8-190">Property</span></span> | <span data-ttu-id="753f8-191">Popis</span><span class="sxs-lookup"><span data-stu-id="753f8-191">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="753f8-192">description</span><span class="sxs-lookup"><span data-stu-id="753f8-192">description</span></span> |<span data-ttu-id="753f8-193">Volitelný popis pro plán hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-193">Optional description for hello schedule.</span></span> |
| <span data-ttu-id="753f8-194">startTime</span><span class="sxs-lookup"><span data-stu-id="753f8-194">startTime</span></span> |<span data-ttu-id="753f8-195">Určuje hello čas spuštění plánu jako objekt data a času.</span><span class="sxs-lookup"><span data-stu-id="753f8-195">Specifies hello start time of a schedule as a DateTime object.</span></span> <span data-ttu-id="753f8-196">Řetězec lze zadat, pokud lze převedený tooa platný datový typ DateTime.</span><span class="sxs-lookup"><span data-stu-id="753f8-196">A string can be provided if it can be converted tooa valid DateTime.</span></span> |
| <span data-ttu-id="753f8-197">Hodnotu IsEnabled</span><span class="sxs-lookup"><span data-stu-id="753f8-197">isEnabled</span></span> |<span data-ttu-id="753f8-198">Určuje, zda je povoleno hello plán.</span><span class="sxs-lookup"><span data-stu-id="753f8-198">Specifies whether hello schedule is enabled.</span></span> |
| <span data-ttu-id="753f8-199">interval</span><span class="sxs-lookup"><span data-stu-id="753f8-199">interval</span></span> |<span data-ttu-id="753f8-200">Hello typ intervalu pro plán hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-200">hello type of interval for hello schedule.</span></span><br><br><span data-ttu-id="753f8-201">Den</span><span class="sxs-lookup"><span data-stu-id="753f8-201">day</span></span><br><span data-ttu-id="753f8-202">Hodina</span><span class="sxs-lookup"><span data-stu-id="753f8-202">hour</span></span> |
| <span data-ttu-id="753f8-203">frequency</span><span class="sxs-lookup"><span data-stu-id="753f8-203">frequency</span></span> |<span data-ttu-id="753f8-204">Četnost hello plán by měl fire počtu dnů nebo hodin.</span><span class="sxs-lookup"><span data-stu-id="753f8-204">Frequency that hello schedule should fire in number of days or hours.</span></span> |

<span data-ttu-id="753f8-205">Plány musí mít počáteční čas s hodnotou větší než hello aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="753f8-205">Schedules must have a start time with a value greater than hello current time.</span></span>  <span data-ttu-id="753f8-206">Tuto hodnotu nelze poskytnout proměnné, vzhledem k tomu, že by měla mít žádný způsob, jak zjistit, kdy je toobe má nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="753f8-206">You cannot provide this value with a variable since you would have no way of knowing when it's going toobe installed.</span></span>

<span data-ttu-id="753f8-207">Použijte jeden z následujících dvou strategie při použití plánu prostředky v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-207">Use one of hello following two strategies when using schedule resources in a solution.</span></span>

- <span data-ttu-id="753f8-208">Použijte parametr pro hello čas spuštění plánu hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-208">Use a parameter for hello start time of hello schedule.</span></span>  <span data-ttu-id="753f8-209">Zobrazí se výzva hello uživatele tooprovide hodnotu při instalaci hello řešení.</span><span class="sxs-lookup"><span data-stu-id="753f8-209">This will prompt hello user tooprovide a value when they install hello solution.</span></span>  <span data-ttu-id="753f8-210">Pokud máte více plánů, můžete použít jeden parametr hodnotu pro více než jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="753f8-210">If you have multiple schedules, you could use a single parameter value for more than one of them.</span></span>
- <span data-ttu-id="753f8-211">Vytvořte plány hello pomocí sady runbook, který se spustí při řešení hello je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="753f8-211">Create hello schedules using a runbook that starts when hello solution is installed.</span></span>  <span data-ttu-id="753f8-212">Odebere hello požadavek hello toospecify uživatele na dobu, ale nemůže obsahovat hello plán ve vašem řešení, když dojde k odebrání hello řešení bude odebrán.</span><span class="sxs-lookup"><span data-stu-id="753f8-212">This removes hello requirement of hello user toospecify a time, but you can't contain hello schedule in your solution so it will be removed when hello solution is removed.</span></span>


### <a name="job-schedules"></a><span data-ttu-id="753f8-213">Plány úlohy</span><span class="sxs-lookup"><span data-stu-id="753f8-213">Job schedules</span></span>
<span data-ttu-id="753f8-214">Prostředky plán úlohy propojit sady runbook s plánem.</span><span class="sxs-lookup"><span data-stu-id="753f8-214">Job schedule resources link a runbook with a schedule.</span></span>  <span data-ttu-id="753f8-215">Mají typ **Microsoft.Automation/automationAccounts/jobSchedules** a mít hello hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="753f8-215">They have a type of **Microsoft.Automation/automationAccounts/jobSchedules** and have hello hello following structure.</span></span>  <span data-ttu-id="753f8-216">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-216">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

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


<span data-ttu-id="753f8-217">Hello vlastnosti pro plány úloh jsou popsané v následující tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-217">hello properties for job schedules are described in hello following table.</span></span>

| <span data-ttu-id="753f8-218">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="753f8-218">Property</span></span> | <span data-ttu-id="753f8-219">Popis</span><span class="sxs-lookup"><span data-stu-id="753f8-219">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="753f8-220">Název plánu</span><span class="sxs-lookup"><span data-stu-id="753f8-220">schedule name</span></span> |<span data-ttu-id="753f8-221">Jeden **název** entity s názvem hello hello plánu.</span><span class="sxs-lookup"><span data-stu-id="753f8-221">Single **name** entity with hello name of hello schedule.</span></span> |
| <span data-ttu-id="753f8-222">Název sady Runbook</span><span class="sxs-lookup"><span data-stu-id="753f8-222">runbook name</span></span>  |<span data-ttu-id="753f8-223">Jeden **název** entity s názvem hello hello sady runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-223">Single **name** entity with hello name of hello runbook.</span></span>  |



## <a name="variables"></a><span data-ttu-id="753f8-224">Proměnné</span><span class="sxs-lookup"><span data-stu-id="753f8-224">Variables</span></span>
<span data-ttu-id="753f8-225">[Azure Automation proměnné](../automation/automation-variables.md) mít typ **Microsoft.Automation/automationAccounts/variables** a mít hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="753f8-225">[Azure Automation variables](../automation/automation-variables.md) have a type of **Microsoft.Automation/automationAccounts/variables** and have hello following structure.</span></span>  <span data-ttu-id="753f8-226">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-226">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span>

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

<span data-ttu-id="753f8-227">Hello vlastnosti pro proměnné prostředky jsou popsané v následující tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-227">hello properties for variable resources are described in hello following table.</span></span>

| <span data-ttu-id="753f8-228">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="753f8-228">Property</span></span> | <span data-ttu-id="753f8-229">Popis</span><span class="sxs-lookup"><span data-stu-id="753f8-229">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="753f8-230">description</span><span class="sxs-lookup"><span data-stu-id="753f8-230">description</span></span> | <span data-ttu-id="753f8-231">Volitelný popis pro proměnnou hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-231">Optional description for hello variable.</span></span> |
| <span data-ttu-id="753f8-232">isEncrypted</span><span class="sxs-lookup"><span data-stu-id="753f8-232">isEncrypted</span></span> | <span data-ttu-id="753f8-233">Určuje, jestli by se šifrovat hello proměnné.</span><span class="sxs-lookup"><span data-stu-id="753f8-233">Specifies whether hello variable should be encrypted.</span></span> |
| <span data-ttu-id="753f8-234">type</span><span class="sxs-lookup"><span data-stu-id="753f8-234">type</span></span> | <span data-ttu-id="753f8-235">Tato vlastnost aktuálně nemá žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="753f8-235">This property currently has no effect.</span></span>  <span data-ttu-id="753f8-236">datový typ Hello hello proměnné určí hello počáteční hodnota.</span><span class="sxs-lookup"><span data-stu-id="753f8-236">hello data type of hello variable will be determined by hello initial value.</span></span> |
| <span data-ttu-id="753f8-237">hodnota</span><span class="sxs-lookup"><span data-stu-id="753f8-237">value</span></span> | <span data-ttu-id="753f8-238">Hodnotu pro proměnnou hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-238">Value for hello variable.</span></span> |

> [!NOTE]
> <span data-ttu-id="753f8-239">Hello **typ** vlastnost aktuálně nemá žádný vliv na proměnnou hello vytváří.</span><span class="sxs-lookup"><span data-stu-id="753f8-239">hello **type** property currently has no effect on hello variable being created.</span></span>  <span data-ttu-id="753f8-240">Hello datový typ pro proměnnou hello určí hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-240">hello data type for hello variable will be determined by hello value.</span></span>  

<span data-ttu-id="753f8-241">Pokud jste nastavili hello počáteční hodnotu pro proměnnou hello, musí být nakonfigurované jako hello správného datového typu.</span><span class="sxs-lookup"><span data-stu-id="753f8-241">If you set hello initial value for hello variable, it must be configured as hello correct data type.</span></span>  <span data-ttu-id="753f8-242">Hello následující tabulka obsahuje různé datové typy hello povolená a jejich syntaxi.</span><span class="sxs-lookup"><span data-stu-id="753f8-242">hello following table provides hello different data types allowable and their syntax.</span></span>  <span data-ttu-id="753f8-243">Všimněte si, jestli jsou hodnoty ve formátu JSON očekávané tooalways být uzavřena v uvozovkách s žádné speciální znaky v rámci hello uvozovky.</span><span class="sxs-lookup"><span data-stu-id="753f8-243">Note that values in JSON are expected tooalways be enclosed in quotes with any special characters within hello quotes.</span></span>  <span data-ttu-id="753f8-244">Například by být řetězcová hodnota určena hello řetězec v uvozovkách (pomocí hello řídicí znak (\\)) číselnou hodnotu by zadán s jednu sadu uvozovky.</span><span class="sxs-lookup"><span data-stu-id="753f8-244">For example, a string value would be specified by quotes around hello string (using hello escape character (\\)) while a numeric value would be specified with one set of quotes.</span></span>

| <span data-ttu-id="753f8-245">Datový typ</span><span class="sxs-lookup"><span data-stu-id="753f8-245">Data type</span></span> | <span data-ttu-id="753f8-246">Popis</span><span class="sxs-lookup"><span data-stu-id="753f8-246">Description</span></span> | <span data-ttu-id="753f8-247">Příklad</span><span class="sxs-lookup"><span data-stu-id="753f8-247">Example</span></span> | <span data-ttu-id="753f8-248">Přeloží příliš</span><span class="sxs-lookup"><span data-stu-id="753f8-248">Resolves too</span></span>|
|:--|:--|:--|:--|
| <span data-ttu-id="753f8-249">Řetězec</span><span class="sxs-lookup"><span data-stu-id="753f8-249">string</span></span>   | <span data-ttu-id="753f8-250">Vložte hodnotu do dvojitých uvozovek.</span><span class="sxs-lookup"><span data-stu-id="753f8-250">Enclose value in double quotes.</span></span>  | <span data-ttu-id="753f8-251">"\"Hello, world\""</span><span class="sxs-lookup"><span data-stu-id="753f8-251">"\"Hello world\""</span></span> | <span data-ttu-id="753f8-252">"Hello, world"</span><span class="sxs-lookup"><span data-stu-id="753f8-252">"Hello world"</span></span> |
| <span data-ttu-id="753f8-253">číselné</span><span class="sxs-lookup"><span data-stu-id="753f8-253">numeric</span></span>  | <span data-ttu-id="753f8-254">Číselná hodnota se jednoduchých uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="753f8-254">Numeric value with single quotes.</span></span>| <span data-ttu-id="753f8-255">"64"</span><span class="sxs-lookup"><span data-stu-id="753f8-255">"64"</span></span> | <span data-ttu-id="753f8-256">64</span><span class="sxs-lookup"><span data-stu-id="753f8-256">64</span></span> |
| <span data-ttu-id="753f8-257">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="753f8-257">boolean</span></span>  | <span data-ttu-id="753f8-258">**Hodnota TRUE,** nebo **false** v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="753f8-258">**true** or **false** in quotes.</span></span>  <span data-ttu-id="753f8-259">Všimněte si, že tato hodnota musí být malými písmeny.</span><span class="sxs-lookup"><span data-stu-id="753f8-259">Note that this value must be lowercase.</span></span> | <span data-ttu-id="753f8-260">"true"</span><span class="sxs-lookup"><span data-stu-id="753f8-260">"true"</span></span> | <span data-ttu-id="753f8-261">Hodnota TRUE</span><span class="sxs-lookup"><span data-stu-id="753f8-261">true</span></span> |
| <span data-ttu-id="753f8-262">Data a času</span><span class="sxs-lookup"><span data-stu-id="753f8-262">datetime</span></span> | <span data-ttu-id="753f8-263">Hodnota serializovaná data.</span><span class="sxs-lookup"><span data-stu-id="753f8-263">Serialized date value.</span></span><br><span data-ttu-id="753f8-264">Tato hodnota hello ConvertTo-Json rutiny v prostředí PowerShell toogenerate můžete použít pro konkrétní datum.</span><span class="sxs-lookup"><span data-stu-id="753f8-264">You can use hello ConvertTo-Json cmdlet in PowerShell toogenerate this value for a particular date.</span></span><br><span data-ttu-id="753f8-265">Příklad: get datum "5/24/2017 13:14:57" \\</span><span class="sxs-lookup"><span data-stu-id="753f8-265">Example: get-date "5/24/2017 13:14:57" \\</span></span>| <span data-ttu-id="753f8-266">ConvertTo-Json</span><span class="sxs-lookup"><span data-stu-id="753f8-266">ConvertTo-Json</span></span> | <span data-ttu-id="753f8-267">"\\/Date(1495656897378)\\/"</span><span class="sxs-lookup"><span data-stu-id="753f8-267">"\\/Date(1495656897378)\\/"</span></span> | <span data-ttu-id="753f8-268">2017-05-24 13:14:57</span><span class="sxs-lookup"><span data-stu-id="753f8-268">2017-05-24 13:14:57</span></span> |

## <a name="modules"></a><span data-ttu-id="753f8-269">Moduly</span><span class="sxs-lookup"><span data-stu-id="753f8-269">Modules</span></span>
<span data-ttu-id="753f8-270">Řešení pro správu nemusí toodefine [globální moduly](../automation/automation-integration-modules.md) použít ve vašich sadách runbook, protože se budou vždy k dispozici ve vašem účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="753f8-270">Your management solution does not need toodefine [global modules](../automation/automation-integration-modules.md) used by your runbooks because they will always be available in your Automation account.</span></span>  <span data-ttu-id="753f8-271">Potřebujete tooinclude prostředek pro ostatní moduly používané vaší sady runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-271">You do need tooinclude a resource for any other module used by your runbooks.</span></span>

<span data-ttu-id="753f8-272">[Integrační moduly](../automation/automation-integration-modules.md) mít typ **Microsoft.Automation/automationAccounts/modules** a mít hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="753f8-272">[Integration modules](../automation/automation-integration-modules.md) have a type of **Microsoft.Automation/automationAccounts/modules** and have hello following structure.</span></span>  <span data-ttu-id="753f8-273">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-273">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span>

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


<span data-ttu-id="753f8-274">Hello vlastnosti pro modul prostředky jsou popsané v následující tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-274">hello properties for module resources are described in hello following table.</span></span>

| <span data-ttu-id="753f8-275">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="753f8-275">Property</span></span> | <span data-ttu-id="753f8-276">Popis</span><span class="sxs-lookup"><span data-stu-id="753f8-276">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="753f8-277">contentLink</span><span class="sxs-lookup"><span data-stu-id="753f8-277">contentLink</span></span> |<span data-ttu-id="753f8-278">Určuje obsah hello hello modulu.</span><span class="sxs-lookup"><span data-stu-id="753f8-278">Specifies hello content of hello module.</span></span> <br><br><span data-ttu-id="753f8-279">identifikátor URI - toohello obsah identifikátoru Uri hello modulu.</span><span class="sxs-lookup"><span data-stu-id="753f8-279">uri - Uri toohello content of hello module.</span></span>  <span data-ttu-id="753f8-280">Bude jím souboru s příponou .ps1 pro prostředí PowerShell a skript sady runbook a soubor exportovaný grafický runbook pro sadu runbook grafu.</span><span class="sxs-lookup"><span data-stu-id="753f8-280">This will be a .ps1 file for PowerShell and Script runbooks, and an exported graphical runbook file for a Graph runbook.</span></span>  <br> <span data-ttu-id="753f8-281">verze - verze hello modulu pro vlastní sledování.</span><span class="sxs-lookup"><span data-stu-id="753f8-281">version - Version of hello module for your own tracking.</span></span> |

<span data-ttu-id="753f8-282">Hello runbook by měl závisí na hello modulu prostředků tooensure vytvořené před hello runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-282">hello runbook should depend on hello module resource tooensure that it's created before hello runbook.</span></span>

### <a name="updating-modules"></a><span data-ttu-id="753f8-283">Aktualizace moduly</span><span class="sxs-lookup"><span data-stu-id="753f8-283">Updating modules</span></span>
<span data-ttu-id="753f8-284">Pokud aktualizujete řešení pro správu, která zahrnuje sadu runbook, která používá plánu a hello novou verzi řešení má nové modulu používá dané sady runbook, může používat hello runbook hello starší verzi modulu hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-284">If you update a management solution that includes a runbook that uses a schedule, and hello new version of your solution has a new module used by that runbook, then hello runbook may use hello old version of hello module.</span></span>  <span data-ttu-id="753f8-285">Musí zahrnovat následující sady runbook ve vašem řešení hello a vytvořit toorun úlohy je před jiné runbooky.</span><span class="sxs-lookup"><span data-stu-id="753f8-285">You should include hello following runbooks in your solution and create a job toorun them before any other runbooks.</span></span>  <span data-ttu-id="753f8-286">Tím bude zajištěno, že jsou všechny moduly aktualizovat, protože požadované před hello, které se načítají sady runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-286">This will ensure that any modules are updated as required before hello runbooks are loaded.</span></span>

* <span data-ttu-id="753f8-287">[Aktualizace ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) zajistí, že všechny hello moduly používané v sadách runbook ve vašem řešení hello nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="753f8-287">[Update-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) will ensure that all of hello modules used by runbooks in your solution are hello latest version.</span></span>  
* <span data-ttu-id="753f8-288">[ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) bude znovu registrovat všechny hello plán prostředky tooensure že hello runbooky propojené toothem s použití hello nejnovější moduly.</span><span class="sxs-lookup"><span data-stu-id="753f8-288">[ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) will reregister all of hello schedule resources tooensure that hello runbooks linked toothem with use hello latest modules.</span></span>




## <a name="sample"></a><span data-ttu-id="753f8-289">Ukázka</span><span class="sxs-lookup"><span data-stu-id="753f8-289">Sample</span></span>
<span data-ttu-id="753f8-290">Tady je ukázka řešení, které zahrnují zahrnující hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="753f8-290">Following is a sample of a solution that include that includes hello following resources:</span></span>

- <span data-ttu-id="753f8-291">Sady Runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-291">Runbook.</span></span>  <span data-ttu-id="753f8-292">Toto je uložen v úložišti GitHub, které veřejné vzorové sady runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-292">This is a sample runbook stored in a public GitHub repository.</span></span>
- <span data-ttu-id="753f8-293">Úlohu automatizace, který se spustí hello runbook při řešení hello je nainstalována.</span><span class="sxs-lookup"><span data-stu-id="753f8-293">Automation job that starts hello runbook when hello solution is installed.</span></span>
- <span data-ttu-id="753f8-294">Plán a úlohy naplánovat v pravidelných intervalech toostart hello runbook.</span><span class="sxs-lookup"><span data-stu-id="753f8-294">Schedule and job schedule toostart hello runbook at regular intervals.</span></span>
- <span data-ttu-id="753f8-295">Certifikát.</span><span class="sxs-lookup"><span data-stu-id="753f8-295">Certificate.</span></span>
- <span data-ttu-id="753f8-296">Přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="753f8-296">Credential.</span></span>
- <span data-ttu-id="753f8-297">Proměnná.</span><span class="sxs-lookup"><span data-stu-id="753f8-297">Variable.</span></span>
- <span data-ttu-id="753f8-298">Modul.</span><span class="sxs-lookup"><span data-stu-id="753f8-298">Module.</span></span>  <span data-ttu-id="753f8-299">Toto je hello [OMSIngestionAPI modulu](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) pro zápis dat tooLog Analytics.</span><span class="sxs-lookup"><span data-stu-id="753f8-299">This is hello [OMSIngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) for writing data tooLog Analytics.</span></span> 

<span data-ttu-id="753f8-300">Hello používá ukázka [standardní řešení parametry](operations-management-suite-solutions-solution-file.md#parameters) proměnné, které by běžně používané v řešení, jako byl proti toohardcoding hodnoty v definicích prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="753f8-300">hello sample uses [standard solution parameters](operations-management-suite-solutions-solution-file.md#parameters) variables that would commonly be used in a solution as opposed toohardcoding values in hello resource definitions.</span></span>


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
            "description": "GUID for hello schedule link toorunbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for hello runbook job.",
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




## <a name="next-steps"></a><span data-ttu-id="753f8-301">Další kroky</span><span class="sxs-lookup"><span data-stu-id="753f8-301">Next steps</span></span>
* <span data-ttu-id="753f8-302">[Přidat řešení tooyour zobrazení](operations-management-suite-solutions-resources-views.md) toovisualize shromažďovat data.</span><span class="sxs-lookup"><span data-stu-id="753f8-302">[Add a view tooyour solution](operations-management-suite-solutions-resources-views.md) toovisualize collected data.</span></span>
