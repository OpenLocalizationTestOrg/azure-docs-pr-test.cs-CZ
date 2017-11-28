---
title: "aaaExporting skupiny prostředků Azure, které obsahují rozšíření virtuálního počítače | Microsoft Docs"
description: "Exportujte šablony Resource Manager, které zahrnují rozšíření virtuálního počítače."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7f4e2ca6-f1c7-4f59-a2cc-8f63132de279
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: nepeters
ms.openlocfilehash: cdbc2030988a19fe68429e8733dc60536c264abf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a><span data-ttu-id="57773-103">Export skupiny prostředků, které obsahují rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="57773-103">Exporting Resource Groups that contain VM extensions</span></span>

<span data-ttu-id="57773-104">Skupiny prostředků Azure je možné exportovat do nové šablony Resource Manager, který lze potom znovu nasadit.</span><span class="sxs-lookup"><span data-stu-id="57773-104">Azure Resource Groups can be exported into a new Resource Manager template that can then be redeployed.</span></span> <span data-ttu-id="57773-105">Hello procesu exportu interpretuje existující prostředky a vytvoří šablonu Resource Manager, která při nasazení výsledkem podobné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="57773-105">hello export process interprets existing resources, and creates a Resource Manager template that when deployed results in a similar Resource Group.</span></span> <span data-ttu-id="57773-106">Při použití možnosti exportu hello skupinu prostředků pro skupinu prostředků obsahující rozšíření virtuálního počítače, několik položek nutné toobe považována za jako je například rozšíření kompatibility a chráněné nastavení.</span><span class="sxs-lookup"><span data-stu-id="57773-106">When using hello Resource Group export option against a Resource Group containing Virtual Machine extensions, several items need toobe considered such as extension compatibility and protected settings.</span></span>

<span data-ttu-id="57773-107">Tato dokument podrobně popisuje, jak funguje hello procesu exportu skupiny prostředků týkající se rozšíření virtuálního počítače, včetně seznamu podporované rozšíření a informace o zpracování zabezpečená data.</span><span class="sxs-lookup"><span data-stu-id="57773-107">This document details how hello Resource Group export process works regarding virtual machine extensions, including a list of supported extensions, and details on handling secured data.</span></span>

## <a name="supported-virtual-machine-extensions"></a><span data-ttu-id="57773-108">Rozšíření podporované virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="57773-108">Supported Virtual Machine Extensions</span></span>

<span data-ttu-id="57773-109">Jsou k dispozici mnoho rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="57773-109">Many Virtual Machine extensions are available.</span></span> <span data-ttu-id="57773-110">Ne všechny rozšíření je možné exportovat do šablony Resource Manageru pomocí funkce "Skriptu pro automatizaci" hello.</span><span class="sxs-lookup"><span data-stu-id="57773-110">Not all extensions can be exported into a Resource Manager template using hello “Automation Script” feature.</span></span> <span data-ttu-id="57773-111">Pokud rozšíření virtuálního počítače není podporován, musí toobe ručně umístí zpět do vyexportované šablony hello.</span><span class="sxs-lookup"><span data-stu-id="57773-111">If a virtual machine extension is not supported, it needs toobe manually placed back into hello exported template.</span></span>

<span data-ttu-id="57773-112">Hello následující rozšíření je možné exportovat pomocí funkce skriptu automatizace hello.</span><span class="sxs-lookup"><span data-stu-id="57773-112">hello following extensions can be exported with hello automation script feature.</span></span>

| <span data-ttu-id="57773-113">Linka</span><span class="sxs-lookup"><span data-stu-id="57773-113">Extension</span></span> ||||
|---|---|---|---|
| <span data-ttu-id="57773-114">Acronis zálohování</span><span class="sxs-lookup"><span data-stu-id="57773-114">Acronis Backup</span></span> | <span data-ttu-id="57773-115">Agent webu Datadog Windows</span><span class="sxs-lookup"><span data-stu-id="57773-115">Datadog Windows Agent</span></span> | <span data-ttu-id="57773-116">Opravy chyb pro Linux operačního systému</span><span class="sxs-lookup"><span data-stu-id="57773-116">OS Patching For Linux</span></span> | <span data-ttu-id="57773-117">Linux snímek virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="57773-117">VM Snapshot Linux</span></span>
| <span data-ttu-id="57773-118">Linux Acronis zálohování</span><span class="sxs-lookup"><span data-stu-id="57773-118">Acronis Backup Linux</span></span> | <span data-ttu-id="57773-119">Rozšíření docker</span><span class="sxs-lookup"><span data-stu-id="57773-119">Docker Extension</span></span> | <span data-ttu-id="57773-120">Puppet agenta</span><span class="sxs-lookup"><span data-stu-id="57773-120">Puppet Agent</span></span> |
| <span data-ttu-id="57773-121">Informace o BG</span><span class="sxs-lookup"><span data-stu-id="57773-121">Bg Info</span></span> | <span data-ttu-id="57773-122">Rozšíření DSC</span><span class="sxs-lookup"><span data-stu-id="57773-122">DSC Extension</span></span> | <span data-ttu-id="57773-123">Přehled Apm lokalita 24 x 7</span><span class="sxs-lookup"><span data-stu-id="57773-123">Site 24x7 Apm Insight</span></span> |
| <span data-ttu-id="57773-124">Linux Agent BMC CTM</span><span class="sxs-lookup"><span data-stu-id="57773-124">BMC CTM Agent Linux</span></span> | <span data-ttu-id="57773-125">Dynatrace Linux</span><span class="sxs-lookup"><span data-stu-id="57773-125">Dynatrace Linux</span></span> | <span data-ttu-id="57773-126">Server lokality 24 x 7 Linux</span><span class="sxs-lookup"><span data-stu-id="57773-126">Site 24x7 Linux Server</span></span> |
| <span data-ttu-id="57773-127">BMC CTM agenta Windows</span><span class="sxs-lookup"><span data-stu-id="57773-127">BMC CTM Agent Windows</span></span> | <span data-ttu-id="57773-128">Dynatrace Windows</span><span class="sxs-lookup"><span data-stu-id="57773-128">Dynatrace Windows</span></span> | <span data-ttu-id="57773-129">24 x 7 Windows serveru lokality</span><span class="sxs-lookup"><span data-stu-id="57773-129">Site 24x7 Windows Server</span></span> |
| <span data-ttu-id="57773-130">Chef klienta</span><span class="sxs-lookup"><span data-stu-id="57773-130">Chef Client</span></span> | <span data-ttu-id="57773-131">HPE zabezpečení aplikace Defender</span><span class="sxs-lookup"><span data-stu-id="57773-131">HPE Security Application Defender</span></span> | <span data-ttu-id="57773-132">Trend malých DSA</span><span class="sxs-lookup"><span data-stu-id="57773-132">Trend Micro DSA</span></span> |
| <span data-ttu-id="57773-133">Vlastní skript</span><span class="sxs-lookup"><span data-stu-id="57773-133">Custom Script</span></span> | <span data-ttu-id="57773-134">Antimalwarové IaaS</span><span class="sxs-lookup"><span data-stu-id="57773-134">IaaS Antimalware</span></span> | <span data-ttu-id="57773-135">Trend malých DSA Linux</span><span class="sxs-lookup"><span data-stu-id="57773-135">Trend Micro DSA Linux</span></span> |
| <span data-ttu-id="57773-136">Rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="57773-136">Custom Script Extension</span></span> | <span data-ttu-id="57773-137">Diagnostika IaaS</span><span class="sxs-lookup"><span data-stu-id="57773-137">IaaS Diagnostics</span></span> | <span data-ttu-id="57773-138">Virtuální počítač přístup pro Linux</span><span class="sxs-lookup"><span data-stu-id="57773-138">VM Access For Linux</span></span> |
| <span data-ttu-id="57773-139">Vlastní skript pro Linux</span><span class="sxs-lookup"><span data-stu-id="57773-139">Custom Script for Linux</span></span> | <span data-ttu-id="57773-140">Linux Chef klienta</span><span class="sxs-lookup"><span data-stu-id="57773-140">Linux Chef Client</span></span> | <span data-ttu-id="57773-141">Virtuální počítač přístup pro Linux</span><span class="sxs-lookup"><span data-stu-id="57773-141">VM Access For Linux</span></span> |
| <span data-ttu-id="57773-142">Datadog agenta systému Linux</span><span class="sxs-lookup"><span data-stu-id="57773-142">Datadog Linux Agent</span></span> | <span data-ttu-id="57773-143">Linux diagnostiky</span><span class="sxs-lookup"><span data-stu-id="57773-143">Linux Diagnostic</span></span> | <span data-ttu-id="57773-144">Snímku virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="57773-144">VM Snapshot</span></span> |

## <a name="export-hello-resource-group"></a><span data-ttu-id="57773-145">Hello export skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="57773-145">Export hello Resource Group</span></span>

<span data-ttu-id="57773-146">tooexport skupiny prostředků do opakovatelně použitelných šablony, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="57773-146">tooexport a Resource Group into a reusable template, complete hello following steps:</span></span>

1. <span data-ttu-id="57773-147">Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="57773-147">Sign in toohello Azure portal</span></span>
2. <span data-ttu-id="57773-148">V nabídce centra hello klikněte na skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="57773-148">On hello Hub Menu, click Resource Groups</span></span>
3. <span data-ttu-id="57773-149">Vyberte ze seznamu hello hello cílová skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="57773-149">Select hello target resource group from hello list</span></span>
4. <span data-ttu-id="57773-150">V okně skupiny prostředků hello klikněte na příkaz skriptu pro automatizaci</span><span class="sxs-lookup"><span data-stu-id="57773-150">In hello Resource Group blade, click Automation Script</span></span>

![Export šablony](./media/extensions-export-templates/template-export.png)

<span data-ttu-id="57773-152">Hello skriptu pro automatizaci Azure Resource Manager vytvoří šablony Resource Manageru, soubor parametrů a několik ukázkové skripty nasazení například prostředí PowerShell a rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="57773-152">hello Azure Resource Manager automations script produces a Resource Manager template, a parameters file, and several sample deployment scripts such as PowerShell and Azure CLI.</span></span> <span data-ttu-id="57773-153">V tomto okamžiku hello vyexportované šablony lze stáhnout pomocí hello tlačítko Stáhnout, přidat jako nový knihovna šablon toohello šablony, nebo znovu nasadit pomocí hello tlačítko nasadit.</span><span class="sxs-lookup"><span data-stu-id="57773-153">At this point, hello exported template can be downloaded using hello download button, added as a new template toohello template library, or redeployed using hello deploy button.</span></span>

## <a name="configure-protected-settings"></a><span data-ttu-id="57773-154">Konfigurace nastavení chráněných</span><span class="sxs-lookup"><span data-stu-id="57773-154">Configure protected settings</span></span>

<span data-ttu-id="57773-155">Mnoho rozšíření virtuálního počítače Azure zahrnují konfiguraci chráněných nastavení, který šifruje citlivá data, jako je například přihlašovací údaje a konfigurace řetězce.</span><span class="sxs-lookup"><span data-stu-id="57773-155">Many Azure virtual machine extensions include a protected settings configuration, that encrypts sensitive data such as credentials and configuration strings.</span></span> <span data-ttu-id="57773-156">Chráněné nastavení se neexportují pomocí skriptu pro automatizaci hello.</span><span class="sxs-lookup"><span data-stu-id="57773-156">Protected settings are not exported with hello automation script.</span></span> <span data-ttu-id="57773-157">Pokud nastavení potřebné, chráněné potřebovat toobe znovu vložit do hello exportovali podle šablony.</span><span class="sxs-lookup"><span data-stu-id="57773-157">If necessary, protected settings need toobe reinserted into hello exported templated.</span></span>

### <a name="step-1---remove-template-parameter"></a><span data-ttu-id="57773-158">Krok 1 – odebrání parametru šablony</span><span class="sxs-lookup"><span data-stu-id="57773-158">Step 1 - Remove template parameter</span></span>

<span data-ttu-id="57773-159">Při vytváření tooprovide hello, který je exportován skupinu prostředků, parametr jednu šablonu toohello hodnota exportovali chráněné nastavení.</span><span class="sxs-lookup"><span data-stu-id="57773-159">When hello Resource Group is exported, a single template parameter is created tooprovide a value toohello exported protected settings.</span></span> <span data-ttu-id="57773-160">Tento parametr lze odebrat.</span><span class="sxs-lookup"><span data-stu-id="57773-160">This parameter can be removed.</span></span> <span data-ttu-id="57773-161">Parametr hello tooremove, projděte seznam parametrů hello a odstranit hello parametr, který vypadá podobně jako příklad toothis JSON.</span><span class="sxs-lookup"><span data-stu-id="57773-161">tooremove hello parameter, look through hello parameter list and delete hello parameter that looks similar toothis JSON example.</span></span>

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a><span data-ttu-id="57773-162">Krok 2 – Get chráněné vlastnosti nastavení</span><span class="sxs-lookup"><span data-stu-id="57773-162">Step 2 - Get protected settings properties</span></span>

<span data-ttu-id="57773-163">Protože každé chráněné nastavení obsahuje sadu požadované vlastnosti, musí seznam těchto vlastností toobe shromáždit.</span><span class="sxs-lookup"><span data-stu-id="57773-163">Because each protected setting has a set of required properties, a list of these properties need toobe gathered.</span></span> <span data-ttu-id="57773-164">Každý parametr hello chráněné nastavení konfigurace lze nalézt v hello [Azure Resource Manager schématu na Githubu](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json).</span><span class="sxs-lookup"><span data-stu-id="57773-164">Each parameter of hello protected settings configuration can be found in hello [Azure Resource Manager schema on GitHub](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json).</span></span> <span data-ttu-id="57773-165">Toto schéma zahrnuje jenom hello sady parametrů pro rozšíření hello uvedené v části Přehled hello tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="57773-165">This schema only includes hello parameter sets for hello extensions listed in hello overview section of this document.</span></span> 

<span data-ttu-id="57773-166">Z v rámci hello schéma úložiště, vyhledejte hello potřeby rozšíření, v tomto příkladu `IaaSDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="57773-166">From within hello schema repository, search for hello desired extension, for this example `IaaSDiagnostics`.</span></span> <span data-ttu-id="57773-167">Jednou hello rozšíření `protectedSettings` byl objekt nachází, poznamenejte si jednotlivé parametry.</span><span class="sxs-lookup"><span data-stu-id="57773-167">Once hello extensions `protectedSettings` object has been located, take note of each parameter.</span></span> <span data-ttu-id="57773-168">V příkladu hello hello `IaasDiagnostic` rozšíření, hello vyžadují parametry jsou `storageAccountName`, `storageAccountKey`, a `storageAccountEndPoint`.</span><span class="sxs-lookup"><span data-stu-id="57773-168">In hello example of hello `IaasDiagnostic` extension, hello require parameters are `storageAccountName`, `storageAccountKey`, and `storageAccountEndPoint`.</span></span>

```json
"protectedSettings": {
    "type": "object",
    "properties": {
        "storageAccountName": {
            "type": "string"
        },
        "storageAccountKey": {
            "type": "string"
        },
        "storageAccountEndPoint": {
            "type": "string"
        }
    },
    "required": [
        "storageAccountName",
        "storageAccountKey",
        "storageAccountEndPoint"
    ]
}
```

### <a name="step-3---re-create-hello-protected-configuration"></a><span data-ttu-id="57773-169">Krok 3 – konfigurace hello chráněné znovu vytvořit</span><span class="sxs-lookup"><span data-stu-id="57773-169">Step 3 - Re-create hello protected configuration</span></span>

<span data-ttu-id="57773-170">Na hello vyexportované šablony, vyhledejte `protectedSettings` a nahraďte hello exportovaný chráněné nastavování objektu novou, který obsahuje parametry hello požadované rozšíření a hodnota pro každé z nich.</span><span class="sxs-lookup"><span data-stu-id="57773-170">On hello exported template, search for `protectedSettings` and replace hello exported protected setting object with a new one that includes hello required extension parameters and a value for each one.</span></span>

<span data-ttu-id="57773-171">V příkladu hello hello `IaasDiagnostic` hello novou konfiguraci chráněných nastavení rozšíření, může vypadat třeba hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="57773-171">In hello example of hello `IaasDiagnostic` extension, hello new protected setting configuration would look like hello following example:</span></span>

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

<span data-ttu-id="57773-172">Hello konečné rozšíření prostředků vypadá podobně jako toohello následující ukázka JSON:</span><span class="sxs-lookup"><span data-stu-id="57773-172">hello final extension resource looks similar toohello following JSON example:</span></span>

```json
{
    "name": "Microsoft.Insights.VMDiagnosticsSettings",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "[variables('apiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "tags": {
        "displayName": "AzureDiagnostics"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Diagnostics",
        "type": "IaaSDiagnostics",
        "typeHandlerVersion": "1.5",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
        },
        "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]",
            "storageAccountEndPoint": "https://core.windows.net"
        }
    }
}
```

<span data-ttu-id="57773-173">Pokud používáte hodnoty vlastností tooprovide parametry šablony, tyto potřebovat toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="57773-173">If using template parameters tooprovide property values, these need toobe created.</span></span> <span data-ttu-id="57773-174">Při vytváření parametry šablony chráněné nastavení hodnot, ujistěte se, že toouse hello `SecureString` parametr zadejte tak, aby citlivé hodnoty jsou zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="57773-174">When creating template parameters for protected setting values, make sure toouse hello `SecureString` parameter type so that sensitive values are secured.</span></span> <span data-ttu-id="57773-175">Další informace o použití parametrů najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="57773-175">For more information on using parameters, see [Authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="57773-176">V příkladu hello hello `IaasDiagnostic` rozšíření, by se vytvořily hello následující parametry v části hello parametrů šablony Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="57773-176">In hello example of hello `IaasDiagnostic` extension, hello following parameters would be created in hello parameters section of hello Resource Manager template.</span></span>

```json
"storageAccountName": {
    "defaultValue": null,
    "type": "SecureString"
},
"storageAccountKey": {
    "defaultValue": null,
    "type": "SecureString"
}
```

<span data-ttu-id="57773-177">V tomto okamžiku hello šablonu můžete nasadit pomocí libovolné metody nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="57773-177">At this point, hello template can be deployed using any template deployment method.</span></span>
