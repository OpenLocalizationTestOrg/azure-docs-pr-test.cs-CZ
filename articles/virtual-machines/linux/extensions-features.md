---
title: "aaaVirtual počítače rozšíření a funkce pro Linux | Microsoft Docs"
description: "Zjistěte, jaká rozšíření jsou k dispozici pro virtuální počítače Azure, seskupené podle co se poskytují nebo zvýšit."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: e0d2ce794c76815ccc6743e8788ee5d9d931e9a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Rozšíření virtuálního počítače a funkce pro Linux

Rozšíření virtuálního počítače Azure se malých aplikacích, které poskytují konfiguraci a automatizaci úloh po nasazení na virtuálních počítačích Azure. Například pokud virtuální počítač vyžaduje instalace softwaru, ochrana proti virům nebo Docker konfigurace, rozšíření virtuálního počítače může být použité toocomplete tyto úlohy. Rozšíření virtuálního počítače Azure lze spouštět s využitím hello rozhraní příkazového řádku Azure, prostředí PowerShell, šablon Azure Resource Manageru a hello portálu Azure. Rozšíření můžete dodávat s nové nasazení virtuálního počítače nebo spouštění všechny existující systém.

Tento dokument obsahuje přehled rozšíření virtuálního počítače, požadavky na tom, jak spravovat toodetect a odeberte rozšíření virtuálního počítače pomocí rozšíření virtuálního počítače Azure a pokyny. Tento dokument obsahuje zobecněný informace, protože mnoho rozšíření virtuálního počítače nejsou k dispozici, každý s konfigurací potenciálně jedinečný. Podrobnosti o konkrétní rozšíření najdete v každé jednotlivé rozšíření konkrétní toohello dokumentu.

## <a name="use-cases-and-samples"></a>Případy použití a ukázky

Jsou k dispozici několik různých rozšíření virtuálního počítače Azure, každý s konkrétní případ použití. Tady je několik příkladů:

- Použití prostředí PowerShell požadovaná stav konfigurace tooa virtuálního počítače pomocí hello rozšíření DSC pro Linux. Další informace najdete v tématu [stavu Azure požadovaná konfigurace rozšíření](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- Konfigurace monitorování virtuálního počítače s hello rozšíření Microsoft Monitoring Agent virtuálního počítače. Další informace najdete v tématu [jak toomonitor virtuálního počítače s Linuxem](tutorial-monitoring.md).
- Konfigurace sledování infrastruktury Azure s hello Datadog rozšíření. Další informace najdete v tématu hello [Datadog blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- Konfigurace hostitele Docker na virtuální počítač Azure pomocí rozšíření virtuálního počítače Docker hello. Další informace najdete v tématu [rozšíření virtuálního počítače Docker](dockerextension.md).

Kromě toho tooprocess konkrétní rozšíření, rozšíření vlastních skriptů je k dispozici pro virtuální počítače Windows a Linux. Hello rozšíření vlastních skriptů pro Linux umožňuje žádné Bash toobe skript spustit na virtuálním počítači. Vlastní skripty jsou užitečné pro návrh Azure nasazení, které vyžadují konfiguraci nad rámec jaké nativní Azure nástrojů může poskytnout. Další informace najdete v tématu [rozšíření skriptů vlastní virtuálních počítačů Linux](extensions-customscript.md).


## <a name="prerequisites"></a>Požadavky

Každé rozšíření virtuálního počítače může mít vlastní sadu požadavků. Například hello rozšíření virtuálního počítače Docker má předpokladem podporované distribuce systému Linux. Požadavky jednotlivých rozšíření jsou podrobně popsané v dokumentaci pro konkrétní rozšíření hello.

### <a name="azure-vm-agent"></a>Agent virtuálního počítače Azure

agent virtuálního počítače Azure Hello spravuje interakce mezi virtuální počítač Azure a prostředků infrastruktury Azure řadiče hello. agent virtuálního počítače Hello je zodpovědná za funkční aspekty nasazení a správa virtuálních počítačích Azure, včetně spuštění rozšíření virtuálního počítače. agent virtuálního počítače Azure Hello je předinstalován v Azure Marketplace bitové kopie a může být nainstalován ručně na podporovaných operačních systémů.

Informace o podporovaných operačních systémů a pokyny k instalaci najdete v tématu [agent virtuálního počítače Azure](../windows/classic/agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Zjistit rozšíření virtuálního počítače

Mnoho různých rozšíření virtuálního počítače jsou k dispozici pro použití s virtuálními počítači Azure. toosee úplný seznam, spusťte následující příkaz s hello rozhraní příkazového řádku Azure, nahraďte hello Příklad umístění hello umístění podle vaší volby hello.

```azurecli
az vm extension image list --location westus -o table
```

## <a name="run-vm-extensions"></a>Spuštění rozšíření virtuálního počítače

Rozšíření virtuálního počítače Azure můžete spustit na existující virtuální počítače, které jsou užitečné, když potřebujete toomake změny konfigurace nebo obnovit připojení již nasazené virtuálního počítače. Rozšíření virtuálního počítače můžete také dodávat s nasazení šablony Azure Resource Manager. Virtuální počítače Azure můžete pomocí rozšíření šablony Resource Manageru, nasazení a nakonfigurování bez zásahu po nasazení.

Hello následující metody se dá použít toorun rozšíření proti existujícího virtuálního počítače.

### <a name="azure-cli"></a>Azure CLI

Pro existující virtuální počítač můžete spouštět rozšíření virtuálního počítače Azure pomocí hello `az vm extension set` příkaz. Tento příklad spouští rozšíření vlastních skriptů hello virtuálního počítače.

```azurecli
az vm extension set `
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

vytváří skript Hello výstup podobný toohello následující text:

```azurecli
info:    Executing command vm extension set
+ Looking up hello VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>portál Azure

Rozšíření virtuálního počítače může být použité tooan existujícího virtuálního počítače prostřednictvím hello portálu Azure. toodo tedy vyberte hello virtuálního počítače, zvolte **rozšíření**a klikněte na tlačítko **přidat**. Vyberte rozšíření hello ze hello seznam dostupných rozšíření a postupujte podle pokynů hello v Průvodci hello.

Hello následující obrázek znázorňuje instalaci hello hello rozšíření vlastních skriptů Linux z hello portálu Azure.

![Instalace rozšíření vlastních skriptů](./media/extensions-features/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Šablony Azure Resource Manageru

Rozšíření virtuálního počítače může být přidané tooan šablony Azure Resource Manageru a provést s hello nasazení šablony hello. Když nasadíte rozšíření pomocí šablony, můžete vytvořit plně nakonfigurované Azure nasazení. Například následující JSON hello je převzat ze šablony Resource Manageru. Šablona Hello nasadí sadu Vyrovnávání zatížení sítě virtuálních počítačů a Azure SQL database a potom nainstaluje aplikace .NET Core na každý virtuální počítač. Instalace softwaru hello postará Hello rozšíření virtuálního počítače.

Další informace najdete v tématu hello úplné [šablony Resource Manageru](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

Další informace najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).

## <a name="secure-vm-extension-data"></a>Zabezpečení dat rozšíření virtuálního počítače

Když používáte rozšíření virtuálního počítače, může být nutné tooinclude citlivé informace, jako je například přihlašovací údaje, názvy účtů úložiště a přístupových klíčů k účtu úložiště. Mnoho rozšíření virtuálního počítače zahrnují chráněné konfigurace, která data šifruje a dešifruje ji pouze uvnitř hello cílového virtuálního počítače. Každé rozšíření má schéma konkrétní chráněné konfigurace a každý je podrobně popsaná v dokumentaci konkrétní rozšíření.

Hello následující příklad ukazuje instanci hello rozšíření vlastních skriptů pro Linux. Všimněte si, že tento příkaz tooexecute hello zahrnuje sadu přihlašovacích údajů. V tomto příkladu hello příkaz tooexecute se šifrovat nebude.


```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Přesunutí hello **příkaz tooexecute** vlastnost toohello **chráněné** konfigurace zabezpečuje hello provádění řetězec.

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

## <a name="troubleshoot-vm-extensions"></a>Řešení potíží s rozšířeními virtuálního počítače

Každé rozšíření virtuálního počítače může mít řešení potíží s kroky konkrétní toohello rozšíření. Například pokud používáte rozšíření vlastních skriptů hello, podrobnosti provádění skriptu najdete místně na hello virtuálním počítači, na kterém byl spuštěn hello rozšíření. Kroky řešení potíží konkrétní rozšíření jsou podrobně popsané v dokumentaci k konkrétní rozšíření.

Hello následující kroky řešení potíží použít tooall rozšíření virtuálního počítače.

### <a name="view-extension-status"></a>Zobrazit stav rozšíření

Po spuštění rozšíření virtuálního počítače pro virtuální počítač, použijte následující stav rozšíření tooreturn příkazu příkazového řádku Azure CLI hello. Názvy parametrů příkladu nahraďte vlastními hodnotami.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

výstup Hello vypadá hello následující text:

```azurecli
AutoUpgradeMinorVersion    Location    Name          ProvisioningState    Publisher                   ResourceGroup      TypeHandlerVersion  VirtualMachineExtensionType
-------------------------  ----------  ------------  -------------------  --------------------------  ---------------  --------------------  -----------------------------
True                       westus      customScript  Succeeded            Microsoft.Azure.Extensions  exttest                             2  customScript
```

Stav spuštění rozšíření naleznete také v hello portálu Azure. Stav hello tooview rozšíření, vyberte hello virtuálního počítače, zvolte **rozšíření**, a vyberte hello požadované rozšíření.

### <a name="rerun-a-vm-extension"></a>Znovu spustit, rozšíření virtuálního počítače

Můžou nastat případy, ve kterých rozšíření virtuálního počítače potřebuje toobe spusťte znovu. Rozšíření může znovu odebrat, a pak znovu spustit hello rozšíření s metodu provádění podle svého výběru. tooremove rozšíření, spusťte následující příkaz s hello rozhraní příkazového řádku Azure hello. Názvy parametrů příkladu nahraďte vlastními hodnotami.

```azurecli
az vm extension delete --name customScript --resource-group myResourceGroup --vm-name myVM
```

Rozšíření můžete odebrat pomocí hello proveďte kroky v hello portálu Azure:

1. Vyberte virtuální počítač.
2. Zvolte **rozšíření**.
3. Vyberte hello požadované rozšíření.
4. Zvolte **odinstalovat**.

## <a name="common-vm-extension-reference"></a>Běžné odkaz na rozšíření virtuálního počítače
| Název rozšíření | Popis | Další informace |
| --- | --- | --- |
| Rozšíření vlastních skriptů pro Linux |Spouštění skriptů na virtuálním počítači Azure |[Rozšíření vlastních skriptů pro Linux](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Rozšíření docker |Nainstalujte hello Docker démon toosupport vzdálených Docker příkazů. |[Rozšíření Docker VM](dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Rozšíření přístupu virtuálních počítačů |Obnoví přístup tooan virtuální počítač Azure |[Rozšíření přístupu virtuálních počítačů](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Rozšíření Azure Diagnostics |Správa Azure Diagnostics |[Rozšíření Azure Diagnostics](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Rozšíření přístup virtuálních počítačů Azure |Spravovat uživatele a přihlašovací údaje |[Rozšíření pro přístup virtuálních počítačů pro Linux](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |
