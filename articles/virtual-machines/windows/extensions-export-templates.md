---
title: "Export skupiny prostředků Azure, které obsahují rozšíření virtuálního počítače | Microsoft Docs"
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
ms.openlocfilehash: cc3c705f1c9123de75ced016a5b39eb1a86b0f73
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a><span data-ttu-id="2eefd-103">Export skupiny prostředků, které obsahují rozšíření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2eefd-103">Exporting Resource Groups that contain VM extensions</span></span>

<span data-ttu-id="2eefd-104">Skupiny prostředků Azure je možné exportovat do nové šablony Resource Manager, který lze potom znovu nasadit.</span><span class="sxs-lookup"><span data-stu-id="2eefd-104">Azure Resource Groups can be exported into a new Resource Manager template that can then be redeployed.</span></span> <span data-ttu-id="2eefd-105">Proces exportu interpretuje existující prostředky a vytvoří šablonu Resource Manager, která při nasazení výsledkem podobné skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2eefd-105">The export process interprets existing resources, and creates a Resource Manager template that when deployed results in a similar Resource Group.</span></span> <span data-ttu-id="2eefd-106">Při použití možnosti export skupiny prostředků pro skupinu prostředků obsahující rozšíření virtuálního počítače, je potřeba zvážit například rozšíření kompatibility několik položek a chráněné nastavení.</span><span class="sxs-lookup"><span data-stu-id="2eefd-106">When using the Resource Group export option against a Resource Group containing Virtual Machine extensions, several items need to be considered such as extension compatibility and protected settings.</span></span>

<span data-ttu-id="2eefd-107">Tento dokument podrobnosti, jak funguje skupina prostředků procesu exportu týkající se rozšíření virtuálního počítače, včetně seznamu podporované rozšíření a informace o zpracování zabezpečená data.</span><span class="sxs-lookup"><span data-stu-id="2eefd-107">This document details how the Resource Group export process works regarding virtual machine extensions, including a list of supported extensions, and details on handling secured data.</span></span>

## <a name="supported-virtual-machine-extensions"></a><span data-ttu-id="2eefd-108">Rozšíření podporované virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2eefd-108">Supported Virtual Machine Extensions</span></span>

<span data-ttu-id="2eefd-109">Jsou k dispozici mnoho rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2eefd-109">Many Virtual Machine extensions are available.</span></span> <span data-ttu-id="2eefd-110">Ne všechny rozšíření je možné exportovat do šablony Resource Manageru pomocí funkce "Skriptu pro automatizaci".</span><span class="sxs-lookup"><span data-stu-id="2eefd-110">Not all extensions can be exported into a Resource Manager template using the “Automation Script” feature.</span></span> <span data-ttu-id="2eefd-111">Pokud rozšíření virtuálního počítače není podporován, je nutné ručně umístí zpět do vyexportované šablony.</span><span class="sxs-lookup"><span data-stu-id="2eefd-111">If a virtual machine extension is not supported, it needs to be manually placed back into the exported template.</span></span>

<span data-ttu-id="2eefd-112">Následující rozšíření je možné exportovat s funkcí skript automatizace.</span><span class="sxs-lookup"><span data-stu-id="2eefd-112">The following extensions can be exported with the automation script feature.</span></span>

| <span data-ttu-id="2eefd-113">Linka</span><span class="sxs-lookup"><span data-stu-id="2eefd-113">Extension</span></span> ||||
|---|---|---|---|
| <span data-ttu-id="2eefd-114">Acronis zálohování</span><span class="sxs-lookup"><span data-stu-id="2eefd-114">Acronis Backup</span></span> | <span data-ttu-id="2eefd-115">Agent webu Datadog Windows</span><span class="sxs-lookup"><span data-stu-id="2eefd-115">Datadog Windows Agent</span></span> | <span data-ttu-id="2eefd-116">Opravy chyb pro Linux operačního systému</span><span class="sxs-lookup"><span data-stu-id="2eefd-116">OS Patching For Linux</span></span> | <span data-ttu-id="2eefd-117">Linux snímek virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2eefd-117">VM Snapshot Linux</span></span>
| <span data-ttu-id="2eefd-118">Linux Acronis zálohování</span><span class="sxs-lookup"><span data-stu-id="2eefd-118">Acronis Backup Linux</span></span> | <span data-ttu-id="2eefd-119">Rozšíření docker</span><span class="sxs-lookup"><span data-stu-id="2eefd-119">Docker Extension</span></span> | <span data-ttu-id="2eefd-120">Puppet agenta</span><span class="sxs-lookup"><span data-stu-id="2eefd-120">Puppet Agent</span></span> |
| <span data-ttu-id="2eefd-121">Informace o BG</span><span class="sxs-lookup"><span data-stu-id="2eefd-121">Bg Info</span></span> | <span data-ttu-id="2eefd-122">Rozšíření DSC</span><span class="sxs-lookup"><span data-stu-id="2eefd-122">DSC Extension</span></span> | <span data-ttu-id="2eefd-123">Přehled Apm lokalita 24 x 7</span><span class="sxs-lookup"><span data-stu-id="2eefd-123">Site 24x7 Apm Insight</span></span> |
| <span data-ttu-id="2eefd-124">Linux Agent BMC CTM</span><span class="sxs-lookup"><span data-stu-id="2eefd-124">BMC CTM Agent Linux</span></span> | <span data-ttu-id="2eefd-125">Dynatrace Linux</span><span class="sxs-lookup"><span data-stu-id="2eefd-125">Dynatrace Linux</span></span> | <span data-ttu-id="2eefd-126">Server lokality 24 x 7 Linux</span><span class="sxs-lookup"><span data-stu-id="2eefd-126">Site 24x7 Linux Server</span></span> |
| <span data-ttu-id="2eefd-127">BMC CTM agenta Windows</span><span class="sxs-lookup"><span data-stu-id="2eefd-127">BMC CTM Agent Windows</span></span> | <span data-ttu-id="2eefd-128">Dynatrace Windows</span><span class="sxs-lookup"><span data-stu-id="2eefd-128">Dynatrace Windows</span></span> | <span data-ttu-id="2eefd-129">24 x 7 Windows serveru lokality</span><span class="sxs-lookup"><span data-stu-id="2eefd-129">Site 24x7 Windows Server</span></span> |
| <span data-ttu-id="2eefd-130">Chef klienta</span><span class="sxs-lookup"><span data-stu-id="2eefd-130">Chef Client</span></span> | <span data-ttu-id="2eefd-131">HPE zabezpečení aplikace Defender</span><span class="sxs-lookup"><span data-stu-id="2eefd-131">HPE Security Application Defender</span></span> | <span data-ttu-id="2eefd-132">Trend malých DSA</span><span class="sxs-lookup"><span data-stu-id="2eefd-132">Trend Micro DSA</span></span> |
| <span data-ttu-id="2eefd-133">Vlastní skript</span><span class="sxs-lookup"><span data-stu-id="2eefd-133">Custom Script</span></span> | <span data-ttu-id="2eefd-134">Antimalwarové IaaS</span><span class="sxs-lookup"><span data-stu-id="2eefd-134">IaaS Antimalware</span></span> | <span data-ttu-id="2eefd-135">Trend malých DSA Linux</span><span class="sxs-lookup"><span data-stu-id="2eefd-135">Trend Micro DSA Linux</span></span> |
| <span data-ttu-id="2eefd-136">Rozšíření vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="2eefd-136">Custom Script Extension</span></span> | <span data-ttu-id="2eefd-137">Diagnostika IaaS</span><span class="sxs-lookup"><span data-stu-id="2eefd-137">IaaS Diagnostics</span></span> | <span data-ttu-id="2eefd-138">Virtuální počítač přístup pro Linux</span><span class="sxs-lookup"><span data-stu-id="2eefd-138">VM Access For Linux</span></span> |
| <span data-ttu-id="2eefd-139">Vlastní skript pro Linux</span><span class="sxs-lookup"><span data-stu-id="2eefd-139">Custom Script for Linux</span></span> | <span data-ttu-id="2eefd-140">Linux Chef klienta</span><span class="sxs-lookup"><span data-stu-id="2eefd-140">Linux Chef Client</span></span> | <span data-ttu-id="2eefd-141">Virtuální počítač přístup pro Linux</span><span class="sxs-lookup"><span data-stu-id="2eefd-141">VM Access For Linux</span></span> |
| <span data-ttu-id="2eefd-142">Datadog agenta systému Linux</span><span class="sxs-lookup"><span data-stu-id="2eefd-142">Datadog Linux Agent</span></span> | <span data-ttu-id="2eefd-143">Linux diagnostiky</span><span class="sxs-lookup"><span data-stu-id="2eefd-143">Linux Diagnostic</span></span> | <span data-ttu-id="2eefd-144">Snímku virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="2eefd-144">VM Snapshot</span></span> |

## <a name="export-the-resource-group"></a><span data-ttu-id="2eefd-145">Export skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="2eefd-145">Export the Resource Group</span></span>

<span data-ttu-id="2eefd-146">Pokud chcete exportovat skupiny prostředků do opakovatelně použitelných šablony, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2eefd-146">To export a Resource Group into a reusable template, complete the following steps:</span></span>

1. <span data-ttu-id="2eefd-147">Přihlášení k webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2eefd-147">Sign in to the Azure portal</span></span>
2. <span data-ttu-id="2eefd-148">V nabídce centra klikněte na skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="2eefd-148">On the Hub Menu, click Resource Groups</span></span>
3. <span data-ttu-id="2eefd-149">Vyberte cílová skupina prostředků ze seznamu</span><span class="sxs-lookup"><span data-stu-id="2eefd-149">Select the target resource group from the list</span></span>
4. <span data-ttu-id="2eefd-150">V okně skupiny prostředků klikněte na příkaz skriptu pro automatizaci</span><span class="sxs-lookup"><span data-stu-id="2eefd-150">In the Resource Group blade, click Automation Script</span></span>

![Export šablony](./media/extensions-export-templates/template-export.png)

<span data-ttu-id="2eefd-152">Skript automatizaci Azure Resource Manager vytvoří šablony Resource Manageru, soubor parametrů a několik ukázkové skripty nasazení například prostředí PowerShell a rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2eefd-152">The Azure Resource Manager automations script produces a Resource Manager template, a parameters file, and several sample deployment scripts such as PowerShell and Azure CLI.</span></span> <span data-ttu-id="2eefd-153">V tomto okamžiku vyexportované šablony lze stáhnout pomocí tlačítko Stáhnout, přidány jako novou šablonu do šablony knihovny, nebo znovu nasadit pomocí tlačítka nasadit.</span><span class="sxs-lookup"><span data-stu-id="2eefd-153">At this point, the exported template can be downloaded using the download button, added as a new template to the template library, or redeployed using the deploy button.</span></span>

## <a name="configure-protected-settings"></a><span data-ttu-id="2eefd-154">Konfigurace nastavení chráněných</span><span class="sxs-lookup"><span data-stu-id="2eefd-154">Configure protected settings</span></span>

<span data-ttu-id="2eefd-155">Mnoho rozšíření virtuálního počítače Azure zahrnují konfiguraci chráněných nastavení, který šifruje citlivá data, jako je například přihlašovací údaje a konfigurace řetězce.</span><span class="sxs-lookup"><span data-stu-id="2eefd-155">Many Azure virtual machine extensions include a protected settings configuration, that encrypts sensitive data such as credentials and configuration strings.</span></span> <span data-ttu-id="2eefd-156">Chráněné nastavení se neexportují pomocí skriptu pro automatizaci.</span><span class="sxs-lookup"><span data-stu-id="2eefd-156">Protected settings are not exported with the automation script.</span></span> <span data-ttu-id="2eefd-157">Pokud nastavení potřebné, chráněné je třeba znovu vložit do exportovaný podle šablony.</span><span class="sxs-lookup"><span data-stu-id="2eefd-157">If necessary, protected settings need to be reinserted into the exported templated.</span></span>

### <a name="step-1---remove-template-parameter"></a><span data-ttu-id="2eefd-158">Krok 1 – odebrání parametru šablony</span><span class="sxs-lookup"><span data-stu-id="2eefd-158">Step 1 - Remove template parameter</span></span>

<span data-ttu-id="2eefd-159">Při exportu skupinu prostředků, parametr jednu šablonu se vytvoří pro poskytnutí hodnoty exportovaných nastaveních chráněný.</span><span class="sxs-lookup"><span data-stu-id="2eefd-159">When the Resource Group is exported, a single template parameter is created to provide a value to the exported protected settings.</span></span> <span data-ttu-id="2eefd-160">Tento parametr lze odebrat.</span><span class="sxs-lookup"><span data-stu-id="2eefd-160">This parameter can be removed.</span></span> <span data-ttu-id="2eefd-161">Odeberte parametr, projděte seznam parametrů a odstraňovat parametr, který vypadá podobně jako v tomto příkladu JSON.</span><span class="sxs-lookup"><span data-stu-id="2eefd-161">To remove the parameter, look through the parameter list and delete the parameter that looks similar to this JSON example.</span></span>

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a><span data-ttu-id="2eefd-162">Krok 2 – Get chráněné vlastnosti nastavení</span><span class="sxs-lookup"><span data-stu-id="2eefd-162">Step 2 - Get protected settings properties</span></span>

<span data-ttu-id="2eefd-163">Protože každý chráněný nastavení obsahuje sadu požadované vlastnosti, seznam těchto vlastností, které je třeba shromáždit.</span><span class="sxs-lookup"><span data-stu-id="2eefd-163">Because each protected setting has a set of required properties, a list of these properties need to be gathered.</span></span> <span data-ttu-id="2eefd-164">Každý parametr konfiguraci chráněných nastavení najdete v [Azure Resource Manager schématu na Githubu](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json).</span><span class="sxs-lookup"><span data-stu-id="2eefd-164">Each parameter of the protected settings configuration can be found in the [Azure Resource Manager schema on GitHub](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json).</span></span> <span data-ttu-id="2eefd-165">Toto schéma zahrnuje jenom sady parametrů pro rozšíření uvedených v části Přehled tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="2eefd-165">This schema only includes the parameter sets for the extensions listed in the overview section of this document.</span></span> 

<span data-ttu-id="2eefd-166">Z v úložišti schémat vyhledejte požadovanou příponu, v tomto příkladu `IaaSDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="2eefd-166">From within the schema repository, search for the desired extension, for this example `IaaSDiagnostics`.</span></span> <span data-ttu-id="2eefd-167">Po rozšíření `protectedSettings` byl objekt nachází, poznamenejte si jednotlivé parametry.</span><span class="sxs-lookup"><span data-stu-id="2eefd-167">Once the extensions `protectedSettings` object has been located, take note of each parameter.</span></span> <span data-ttu-id="2eefd-168">V příkladu `IaasDiagnostic` rozšíření, vyžadovat parametry jsou `storageAccountName`, `storageAccountKey`, a `storageAccountEndPoint`.</span><span class="sxs-lookup"><span data-stu-id="2eefd-168">In the example of the `IaasDiagnostic` extension, the require parameters are `storageAccountName`, `storageAccountKey`, and `storageAccountEndPoint`.</span></span>

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

### <a name="step-3---re-create-the-protected-configuration"></a><span data-ttu-id="2eefd-169">Krok 3 – znovu vytvořit chráněné konfigurace</span><span class="sxs-lookup"><span data-stu-id="2eefd-169">Step 3 - Re-create the protected configuration</span></span>

<span data-ttu-id="2eefd-170">V šabloně exportovaný, vyhledejte `protectedSettings` a nahraďte objekt exportovaný chráněné nastavení novou, který obsahuje parametry požadované rozšíření a hodnota pro každé z nich.</span><span class="sxs-lookup"><span data-stu-id="2eefd-170">On the exported template, search for `protectedSettings` and replace the exported protected setting object with a new one that includes the required extension parameters and a value for each one.</span></span>

<span data-ttu-id="2eefd-171">V příkladu `IaasDiagnostic` rozšíření, nové chráněné nastavení konfigurace vypadat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2eefd-171">In the example of the `IaasDiagnostic` extension, the new protected setting configuration would look like the following example:</span></span>

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

<span data-ttu-id="2eefd-172">Vypadá podobně jako v následujícím příkladu JSON posledním rozšíření prostředků:</span><span class="sxs-lookup"><span data-stu-id="2eefd-172">The final extension resource looks similar to the following JSON example:</span></span>

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

<span data-ttu-id="2eefd-173">Pokud používáte parametry šablony k poskytování hodnot vlastností, tyto musí vytvořit.</span><span class="sxs-lookup"><span data-stu-id="2eefd-173">If using template parameters to provide property values, these need to be created.</span></span> <span data-ttu-id="2eefd-174">Při vytváření parametry šablony chráněné nastavení hodnot, nezapomeňte použít `SecureString` parametr zadejte tak, aby citlivé hodnoty jsou zabezpečené.</span><span class="sxs-lookup"><span data-stu-id="2eefd-174">When creating template parameters for protected setting values, make sure to use the `SecureString` parameter type so that sensitive values are secured.</span></span> <span data-ttu-id="2eefd-175">Další informace o použití parametrů najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2eefd-175">For more information on using parameters, see [Authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="2eefd-176">V příkladu `IaasDiagnostic` rozšíření, by se vytvořily následující parametry v sekci parametrů šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="2eefd-176">In the example of the `IaasDiagnostic` extension, the following parameters would be created in the parameters section of the Resource Manager template.</span></span>

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

<span data-ttu-id="2eefd-177">V tomto okamžiku šabloně se dá nasadit pomocí libovolné metody nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="2eefd-177">At this point, the template can be deployed using any template deployment method.</span></span>
