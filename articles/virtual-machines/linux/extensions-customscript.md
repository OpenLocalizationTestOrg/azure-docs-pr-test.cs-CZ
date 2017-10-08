---
title: "vlastní skripty aaaRun na virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Automatizovat úkoly konfigurace virtuálního počítače s Linuxem pomocí hello rozšíření vlastních skriptů"
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: nepeters
ms.openlocfilehash: f2c273a5fbd4cd1695aea48fa4bd08e691511e5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-custom-script-extension-with-linux-virtual-machines"></a>Pomocí hello rozšíření vlastních skriptů Azure s virtuálním počítačům s Linuxem
Hello rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure. Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy. Skripty můžete stáhnout z úložiště Azure nebo jiné dostupné umístění v Internetu nebo zadat čas spuštění rozšíření toohello. Hello rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí hello rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo hello REST API pro virtuální počítač Azure.

Tato dokument podrobně popisuje, jak toouse hello rozšíření vlastních skriptů z hello rozhraní příkazového řádku Azure a šablonu Azure Resource Manager a také podrobnosti o řešení potíží s kroky v systémech Linux.

## <a name="extension-configuration"></a>Konfigurace rozšíření
Konfigurace rozšíření vlastních skriptů Hello určuje takové věci, jako umístění skriptu a toobe hello příkaz spustit. Tato konfigurace mohou být uloženy v konfiguračních souborech zadané na hello příkazového řádku, nebo šablonu Azure Resource Manager. Citlivá data se uloží v chráněném konfigurace, který je šifrovaný a dešifrovat jenom uvnitř hello virtuálního počítače. Hello chráněné konfigurace je užitečná při provádění příkazu hello zahrnuje tajné klíče, jako například heslo.

### <a name="public-configuration"></a>Veřejná konfigurace
Schéma:

**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena. Pomocí názvů hello, jak je vidět níže tooavoid problémy při nasazení.

* **commandToExecute**: (vyžaduje se, řetězce) hello tooexecute skriptu vstupního bodu
* **fileUris**: (volitelné, pole řetězců) adresy URL hello toobe soubory stáhnout.
* **časové razítko** (volitelné, celé číslo) používat tato pole pouze tootrigger opětovné spuštění skriptu hello tak, že změníte hodnotu tohoto pole.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Chráněné konfigurace
Schéma:

**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena. Pomocí názvů hello, jak je vidět níže tooavoid problémy při nasazení.

* **commandToExecute**: (volitelné, string) hello tooexecute skriptu vstupní bod. Toto pole použijte místo toho, pokud váš příkaz obsahuje tajné klíče, jako jsou hesla.
* **storageAccountName**: (volitelné, string) hello název účtu úložiště. Pokud chcete zadat pověření pro úložiště, musí být všechny fileUris adresy URL pro objekty BLOB Azure.
* **storageAccountKey**: (volitelné, string) hello přístupový klíč účtu úložiště.

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI
Pokud používáte hello rozhraní příkazového řádku Azure toorun hello rozšíření vlastních skriptů, vytvořte konfigurační soubor nebo soubory obsahující na minimální hello souboru uri a hello příkaz provádění skriptu.

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

Volitelně hello nastavení můžete zadat v příkazu hello jako řetězec formátu JSON. To umožňuje toobe hello konfigurace zadané během provádění a bez jiný konfigurační soubor.

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],"commandToExecute": "./test.sh"}'
```

### <a name="azure-cli-examples"></a>Příklady rozhraní příkazového řádku Azure

**Příklad 1** -veřejné konfigurace pomocí souboru skriptu.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],
  "commandToExecute": "./test.sh"
}
```

Azure CLI příkaz:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Příklad 2** -veřejné konfigurace s žádný soubor skriptu.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI příkaz:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Příklad 3** – veřejné konfigurační soubor je soubor skriptu hello použité toospecify URI a chráněné konfigurační soubor je použité toospecify hello příkaz toobe provést.

Veřejné konfigurační soubor:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

Chráněné konfigurační soubor:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI příkaz:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a>Šablona Resource Manageru
Hello rozšíření vlastních skriptů Azure můžete spustit v době nasazení virtuálního počítače pomocí šablony Resource Manageru. toodo tedy přidání správně formátovaný JSON toohello nasazení šablony.

### <a name="resource-manager-examples"></a>Příklady Resource Manager
**Příklad 1** -veřejné konfigurace.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Příklad 2** -spuštění příkazu v chráněné konfigurace.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
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
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Najdete v části hello .net Core Hudba úložiště ukázkové kompletní příklad - [Hudba úložiště ukázkové](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Řešení potíží
Po spuštění hello rozšíření vlastních skriptů hello skript se vytvoří nebo stáhli do adresáře podobné toohello následující ukázka. výstup příkazu Hello je také uložen do tohoto adresáře `stdout` a `stderr` souboru.

```bash
/var/lib/waagent/custom-script/download/0/
```

Hello rozšíření skriptů Azure vytvoří protokol, který je zde uveden.

```bash
/var/log/azure/custom-script/handler.log
```

také je možné načíst stav spuštění Hello hello rozšíření vlastních skriptů s hello rozhraní příkazového řádku Azure.

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

výstup Hello vypadá hello následující text:

```azurecli
info:    Executing command vm extension get
+ Looking up hello VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Další kroky
Informace o skriptu rozšíření jiných virtuálních počítačů, v tématu [přehled Azure rozšíření skriptů pro Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

