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
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a>Export skupiny prostředků, které obsahují rozšíření virtuálního počítače

Skupiny prostředků Azure je možné exportovat do nové šablony Resource Manager, který lze potom znovu nasadit. Hello procesu exportu interpretuje existující prostředky a vytvoří šablonu Resource Manager, která při nasazení výsledkem podobné skupiny prostředků. Při použití možnosti exportu hello skupinu prostředků pro skupinu prostředků obsahující rozšíření virtuálního počítače, několik položek nutné toobe považována za jako je například rozšíření kompatibility a chráněné nastavení.

Tato dokument podrobně popisuje, jak funguje hello procesu exportu skupiny prostředků týkající se rozšíření virtuálního počítače, včetně seznamu podporované rozšíření a informace o zpracování zabezpečená data.

## <a name="supported-virtual-machine-extensions"></a>Rozšíření podporované virtuálního počítače

Jsou k dispozici mnoho rozšíření virtuálního počítače. Ne všechny rozšíření je možné exportovat do šablony Resource Manageru pomocí funkce "Skriptu pro automatizaci" hello. Pokud rozšíření virtuálního počítače není podporován, musí toobe ručně umístí zpět do vyexportované šablony hello.

Hello následující rozšíření je možné exportovat pomocí funkce skriptu automatizace hello.

| Linka ||||
|---|---|---|---|
| Acronis zálohování | Agent webu Datadog Windows | Opravy chyb pro Linux operačního systému | Linux snímek virtuálního počítače
| Linux Acronis zálohování | Rozšíření docker | Puppet agenta |
| Informace o BG | Rozšíření DSC | Přehled Apm lokalita 24 x 7 |
| Linux Agent BMC CTM | Dynatrace Linux | Server lokality 24 x 7 Linux |
| BMC CTM agenta Windows | Dynatrace Windows | 24 x 7 Windows serveru lokality |
| Chef klienta | HPE zabezpečení aplikace Defender | Trend malých DSA |
| Vlastní skript | Antimalwarové IaaS | Trend malých DSA Linux |
| Rozšíření vlastních skriptů | Diagnostika IaaS | Virtuální počítač přístup pro Linux |
| Vlastní skript pro Linux | Linux Chef klienta | Virtuální počítač přístup pro Linux |
| Datadog agenta systému Linux | Linux diagnostiky | Snímku virtuálního počítače |

## <a name="export-hello-resource-group"></a>Hello export skupiny prostředků

tooexport skupiny prostředků do opakovatelně použitelných šablony, dokončení hello následující kroky:

1. Přihlaste se toohello portálu Azure
2. V nabídce centra hello klikněte na skupiny prostředků
3. Vyberte ze seznamu hello hello cílová skupina prostředků
4. V okně skupiny prostředků hello klikněte na příkaz skriptu pro automatizaci

![Export šablony](./media/extensions-export-templates/template-export.png)

Hello skriptu pro automatizaci Azure Resource Manager vytvoří šablony Resource Manageru, soubor parametrů a několik ukázkové skripty nasazení například prostředí PowerShell a rozhraní příkazového řádku Azure. V tomto okamžiku hello vyexportované šablony lze stáhnout pomocí hello tlačítko Stáhnout, přidat jako nový knihovna šablon toohello šablony, nebo znovu nasadit pomocí hello tlačítko nasadit.

## <a name="configure-protected-settings"></a>Konfigurace nastavení chráněných

Mnoho rozšíření virtuálního počítače Azure zahrnují konfiguraci chráněných nastavení, který šifruje citlivá data, jako je například přihlašovací údaje a konfigurace řetězce. Chráněné nastavení se neexportují pomocí skriptu pro automatizaci hello. Pokud nastavení potřebné, chráněné potřebovat toobe znovu vložit do hello exportovali podle šablony.

### <a name="step-1---remove-template-parameter"></a>Krok 1 – odebrání parametru šablony

Při vytváření tooprovide hello, který je exportován skupinu prostředků, parametr jednu šablonu toohello hodnota exportovali chráněné nastavení. Tento parametr lze odebrat. Parametr hello tooremove, projděte seznam parametrů hello a odstranit hello parametr, který vypadá podobně jako příklad toothis JSON.

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a>Krok 2 – Get chráněné vlastnosti nastavení

Protože každé chráněné nastavení obsahuje sadu požadované vlastnosti, musí seznam těchto vlastností toobe shromáždit. Každý parametr hello chráněné nastavení konfigurace lze nalézt v hello [Azure Resource Manager schématu na Githubu](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json). Toto schéma zahrnuje jenom hello sady parametrů pro rozšíření hello uvedené v části Přehled hello tohoto dokumentu. 

Z v rámci hello schéma úložiště, vyhledejte hello potřeby rozšíření, v tomto příkladu `IaaSDiagnostics`. Jednou hello rozšíření `protectedSettings` byl objekt nachází, poznamenejte si jednotlivé parametry. V příkladu hello hello `IaasDiagnostic` rozšíření, hello vyžadují parametry jsou `storageAccountName`, `storageAccountKey`, a `storageAccountEndPoint`.

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

### <a name="step-3---re-create-hello-protected-configuration"></a>Krok 3 – konfigurace hello chráněné znovu vytvořit

Na hello vyexportované šablony, vyhledejte `protectedSettings` a nahraďte hello exportovaný chráněné nastavování objektu novou, který obsahuje parametry hello požadované rozšíření a hodnota pro každé z nich.

V příkladu hello hello `IaasDiagnostic` hello novou konfiguraci chráněných nastavení rozšíření, může vypadat třeba hello následující ukázka:

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

Hello konečné rozšíření prostředků vypadá podobně jako toohello následující ukázka JSON:

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

Pokud používáte hodnoty vlastností tooprovide parametry šablony, tyto potřebovat toobe vytvořili. Při vytváření parametry šablony chráněné nastavení hodnot, ujistěte se, že toouse hello `SecureString` parametr zadejte tak, aby citlivé hodnoty jsou zabezpečené. Další informace o použití parametrů najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../../resource-group-authoring-templates.md).

V příkladu hello hello `IaasDiagnostic` rozšíření, by se vytvořily hello následující parametry v části hello parametrů šablony Resource Manageru hello.

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

V tomto okamžiku hello šablonu můžete nasadit pomocí libovolné metody nasazení šablony.
