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
# <a name="using-hello-azure-custom-script-extension-with-linux-virtual-machines"></a><span data-ttu-id="87578-103">Pomocí hello rozšíření vlastních skriptů Azure s virtuálním počítačům s Linuxem</span><span class="sxs-lookup"><span data-stu-id="87578-103">Using hello Azure Custom Script Extension with Linux Virtual Machines</span></span>
<span data-ttu-id="87578-104">Hello rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="87578-104">hello Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="87578-105">Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy.</span><span class="sxs-lookup"><span data-stu-id="87578-105">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="87578-106">Skripty můžete stáhnout z úložiště Azure nebo jiné dostupné umístění v Internetu nebo zadat čas spuštění rozšíření toohello.</span><span class="sxs-lookup"><span data-stu-id="87578-106">Scripts can be downloaded from Azure storage or other accessible internet location, or provided toohello extension run time.</span></span> <span data-ttu-id="87578-107">Hello rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí hello rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo hello REST API pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="87578-107">hello Custom Script extension integrates with Azure Resource Manager templates, and can also be run using hello Azure CLI, PowerShell, Azure portal, or hello Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="87578-108">Tato dokument podrobně popisuje, jak toouse hello rozšíření vlastních skriptů z hello rozhraní příkazového řádku Azure a šablonu Azure Resource Manager a také podrobnosti o řešení potíží s kroky v systémech Linux.</span><span class="sxs-lookup"><span data-stu-id="87578-108">This document details how toouse hello Custom Script Extension from hello Azure CLI, and an Azure Resource Manager template, and also details troubleshooting steps on Linux systems.</span></span>

## <a name="extension-configuration"></a><span data-ttu-id="87578-109">Konfigurace rozšíření</span><span class="sxs-lookup"><span data-stu-id="87578-109">Extension Configuration</span></span>
<span data-ttu-id="87578-110">Konfigurace rozšíření vlastních skriptů Hello určuje takové věci, jako umístění skriptu a toobe hello příkaz spustit.</span><span class="sxs-lookup"><span data-stu-id="87578-110">hello Custom Script Extension configuration specifies things like script location and hello command toobe run.</span></span> <span data-ttu-id="87578-111">Tato konfigurace mohou být uloženy v konfiguračních souborech zadané na hello příkazového řádku, nebo šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="87578-111">This configuration can be stored in configuration files, specified on hello command line, or in an Azure Resource Manager template.</span></span> <span data-ttu-id="87578-112">Citlivá data se uloží v chráněném konfigurace, který je šifrovaný a dešifrovat jenom uvnitř hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="87578-112">Sensitive data can be stored in a protected configuration, which is encrypted and only decrypted inside hello virtual machine.</span></span> <span data-ttu-id="87578-113">Hello chráněné konfigurace je užitečná při provádění příkazu hello zahrnuje tajné klíče, jako například heslo.</span><span class="sxs-lookup"><span data-stu-id="87578-113">hello protected configuration is useful when hello execution command includes secrets such as a password.</span></span>

### <a name="public-configuration"></a><span data-ttu-id="87578-114">Veřejná konfigurace</span><span class="sxs-lookup"><span data-stu-id="87578-114">Public Configuration</span></span>
<span data-ttu-id="87578-115">Schéma:</span><span class="sxs-lookup"><span data-stu-id="87578-115">Schema:</span></span>

<span data-ttu-id="87578-116">**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="87578-116">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="87578-117">Pomocí názvů hello, jak je vidět níže tooavoid problémy při nasazení.</span><span class="sxs-lookup"><span data-stu-id="87578-117">Use hello names as seen below tooavoid deployment issues.</span></span>

* <span data-ttu-id="87578-118">**commandToExecute**: (vyžaduje se, řetězce) hello tooexecute skriptu vstupního bodu</span><span class="sxs-lookup"><span data-stu-id="87578-118">**commandToExecute**: (required, string) hello entry point script tooexecute</span></span>
* <span data-ttu-id="87578-119">**fileUris**: (volitelné, pole řetězců) adresy URL hello toobe soubory stáhnout.</span><span class="sxs-lookup"><span data-stu-id="87578-119">**fileUris**: (optional, string array) hello URLs for files toobe downloaded.</span></span>
* <span data-ttu-id="87578-120">**časové razítko** (volitelné, celé číslo) používat tato pole pouze tootrigger opětovné spuštění skriptu hello tak, že změníte hodnotu tohoto pole.</span><span class="sxs-lookup"><span data-stu-id="87578-120">**timestamp** (optional, integer) use this field only tootrigger a rerun of hello script by changing value of this field.</span></span>

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a><span data-ttu-id="87578-121">Chráněné konfigurace</span><span class="sxs-lookup"><span data-stu-id="87578-121">Protected Configuration</span></span>
<span data-ttu-id="87578-122">Schéma:</span><span class="sxs-lookup"><span data-stu-id="87578-122">Schema:</span></span>

<span data-ttu-id="87578-123">**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="87578-123">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="87578-124">Pomocí názvů hello, jak je vidět níže tooavoid problémy při nasazení.</span><span class="sxs-lookup"><span data-stu-id="87578-124">Use hello names as seen below tooavoid deployment issues.</span></span>

* <span data-ttu-id="87578-125">**commandToExecute**: (volitelné, string) hello tooexecute skriptu vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="87578-125">**commandToExecute**: (optional, string) hello entry point script tooexecute.</span></span> <span data-ttu-id="87578-126">Toto pole použijte místo toho, pokud váš příkaz obsahuje tajné klíče, jako jsou hesla.</span><span class="sxs-lookup"><span data-stu-id="87578-126">Use this field instead if your command contains secrets such as passwords.</span></span>
* <span data-ttu-id="87578-127">**storageAccountName**: (volitelné, string) hello název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="87578-127">**storageAccountName**: (optional, string) hello name of storage account.</span></span> <span data-ttu-id="87578-128">Pokud chcete zadat pověření pro úložiště, musí být všechny fileUris adresy URL pro objekty BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="87578-128">If you specify storage credentials, all fileUris must be URLs for Azure Blobs.</span></span>
* <span data-ttu-id="87578-129">**storageAccountKey**: (volitelné, string) hello přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="87578-129">**storageAccountKey**: (optional, string) hello access key of storage account.</span></span>

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a><span data-ttu-id="87578-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="87578-130">Azure CLI</span></span>
<span data-ttu-id="87578-131">Pokud používáte hello rozhraní příkazového řádku Azure toorun hello rozšíření vlastních skriptů, vytvořte konfigurační soubor nebo soubory obsahující na minimální hello souboru uri a hello příkaz provádění skriptu.</span><span class="sxs-lookup"><span data-stu-id="87578-131">When using hello Azure CLI toorun hello Custom Script Extension, create a configuration file or files containing at minimum hello file uri, and hello script execution command.</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="87578-132">Volitelně hello nastavení můžete zadat v příkazu hello jako řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="87578-132">Optionally hello settings can be specified in hello command as a JSON formatted string.</span></span> <span data-ttu-id="87578-133">To umožňuje toobe hello konfigurace zadané během provádění a bez jiný konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="87578-133">This allows hello configuration toobe specified during execution and without a separate configuration file.</span></span>

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],"commandToExecute": "./test.sh"}'
```

### <a name="azure-cli-examples"></a><span data-ttu-id="87578-134">Příklady rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="87578-134">Azure CLI Examples</span></span>

<span data-ttu-id="87578-135">**Příklad 1** -veřejné konfigurace pomocí souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="87578-135">**Example 1** - Public configuration with script file.</span></span>

```json
{
  "fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],
  "commandToExecute": "./test.sh"
}
```

<span data-ttu-id="87578-136">Azure CLI příkaz:</span><span class="sxs-lookup"><span data-stu-id="87578-136">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="87578-137">**Příklad 2** -veřejné konfigurace s žádný soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="87578-137">**Example 2** - Public configuration with no script file.</span></span>

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

<span data-ttu-id="87578-138">Azure CLI příkaz:</span><span class="sxs-lookup"><span data-stu-id="87578-138">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="87578-139">**Příklad 3** – veřejné konfigurační soubor je soubor skriptu hello použité toospecify URI a chráněné konfigurační soubor je použité toospecify hello příkaz toobe provést.</span><span class="sxs-lookup"><span data-stu-id="87578-139">**Example 3** - A public configuration file is used toospecify hello script file URI, and a protected configuration file is used toospecify hello command toobe executed.</span></span>

<span data-ttu-id="87578-140">Veřejné konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="87578-140">Public configuration file:</span></span>

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

<span data-ttu-id="87578-141">Chráněné konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="87578-141">Protected configuration file:</span></span>  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

<span data-ttu-id="87578-142">Azure CLI příkaz:</span><span class="sxs-lookup"><span data-stu-id="87578-142">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a><span data-ttu-id="87578-143">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="87578-143">Resource Manager Template</span></span>
<span data-ttu-id="87578-144">Hello rozšíření vlastních skriptů Azure můžete spustit v době nasazení virtuálního počítače pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="87578-144">hello Azure Custom Script Extension can be run at Virtual Machine deployment time using a Resource Manager template.</span></span> <span data-ttu-id="87578-145">toodo tedy přidání správně formátovaný JSON toohello nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="87578-145">toodo so, add properly formatted JSON toohello deployment template.</span></span>

### <a name="resource-manager-examples"></a><span data-ttu-id="87578-146">Příklady Resource Manager</span><span class="sxs-lookup"><span data-stu-id="87578-146">Resource Manager Examples</span></span>
<span data-ttu-id="87578-147">**Příklad 1** -veřejné konfigurace.</span><span class="sxs-lookup"><span data-stu-id="87578-147">**Example 1** - public configuration.</span></span>

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

<span data-ttu-id="87578-148">**Příklad 2** -spuštění příkazu v chráněné konfigurace.</span><span class="sxs-lookup"><span data-stu-id="87578-148">**Example 2** - execution command in protected configuration.</span></span>

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

<span data-ttu-id="87578-149">Najdete v části hello .net Core Hudba úložiště ukázkové kompletní příklad - [Hudba úložiště ukázkové](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).</span><span class="sxs-lookup"><span data-stu-id="87578-149">See hello .Net Core Music Store Demo for a complete example - [Music Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="87578-150">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="87578-150">Troubleshooting</span></span>
<span data-ttu-id="87578-151">Po spuštění hello rozšíření vlastních skriptů hello skript se vytvoří nebo stáhli do adresáře podobné toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="87578-151">When hello Custom Script Extension runs, hello script is created or downloaded into a directory similar toohello following example.</span></span> <span data-ttu-id="87578-152">výstup příkazu Hello je také uložen do tohoto adresáře `stdout` a `stderr` souboru.</span><span class="sxs-lookup"><span data-stu-id="87578-152">hello command output is also saved into this directory in `stdout` and `stderr` file.</span></span>

```bash
/var/lib/waagent/custom-script/download/0/
```

<span data-ttu-id="87578-153">Hello rozšíření skriptů Azure vytvoří protokol, který je zde uveden.</span><span class="sxs-lookup"><span data-stu-id="87578-153">hello Azure Script Extension produces a log, which can be found here.</span></span>

```bash
/var/log/azure/custom-script/handler.log
```

<span data-ttu-id="87578-154">také je možné načíst stav spuštění Hello hello rozšíření vlastních skriptů s hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="87578-154">hello execution state of hello Custom Script Extension can also be retrieved with hello Azure CLI.</span></span>

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

<span data-ttu-id="87578-155">výstup Hello vypadá hello následující text:</span><span class="sxs-lookup"><span data-stu-id="87578-155">hello output looks like hello following text:</span></span>

```azurecli
info:    Executing command vm extension get
+ Looking up hello VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a><span data-ttu-id="87578-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87578-156">Next Steps</span></span>
<span data-ttu-id="87578-157">Informace o skriptu rozšíření jiných virtuálních počítačů, v tématu [přehled Azure rozšíření skriptů pro Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="87578-157">For information on other VM Script Extensions, see [Azure Script Extension overview for Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

