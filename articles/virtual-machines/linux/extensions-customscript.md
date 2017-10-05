---
title: "Spustit vlastní skripty na virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Automatizovat úkoly konfigurace virtuálního počítače s Linuxem pomocí rozšíření vlastních skriptů"
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
ms.openlocfilehash: 1dde64aac72c11ccfccf4fdb676279692befaadd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a><span data-ttu-id="4f0f4-103">Virtuální počítače s Linuxem pomocí rozšíření Azure vlastních skriptů</span><span class="sxs-lookup"><span data-stu-id="4f0f4-103">Using the Azure Custom Script Extension with Linux Virtual Machines</span></span>
<span data-ttu-id="4f0f4-104">Rozšíření vlastních skriptů stahuje a spouští skripty na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-104">The Custom Script Extension downloads and executes scripts on Azure virtual machines.</span></span> <span data-ttu-id="4f0f4-105">Toto rozšíření je užitečné pro konfiguraci nasazení post, instalace softwaru nebo jakoukoli jinou konfiguraci, nebo úlohu správy.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-105">This extension is useful for post deployment configuration, software installation, or any other configuration / management task.</span></span> <span data-ttu-id="4f0f4-106">Skripty můžete stáhnout z úložiště Azure nebo jiné dostupné umístění v Internetu nebo zadat čas spuštění rozšíření.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-106">Scripts can be downloaded from Azure storage or other accessible internet location, or provided to the extension run time.</span></span> <span data-ttu-id="4f0f4-107">Rozšíření vlastních skriptů se integruje s šablon Azure Resource Manageru a můžete také spustit pomocí rozhraní příkazového řádku Azure, PowerShell, portálu Azure nebo REST API pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-107">The Custom Script extension integrates with Azure Resource Manager templates, and can also be run using the Azure CLI, PowerShell, Azure portal, or the Azure Virtual Machine REST API.</span></span>

<span data-ttu-id="4f0f4-108">Tento dokument podrobnosti o tom, jak používat rozšíření vlastních skriptů z rozhraní příkazového řádku Azure a šablonu Azure Resource Manager a také podrobnosti o řešení potíží v systémech Linux.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-108">This document details how to use the Custom Script Extension from the Azure CLI, and an Azure Resource Manager template, and also details troubleshooting steps on Linux systems.</span></span>

## <a name="extension-configuration"></a><span data-ttu-id="4f0f4-109">Konfigurace rozšíření</span><span class="sxs-lookup"><span data-stu-id="4f0f4-109">Extension Configuration</span></span>
<span data-ttu-id="4f0f4-110">Konfigurace rozšíření vlastních skriptů určuje takové věci, jako umístění skriptu a příkaz ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-110">The Custom Script Extension configuration specifies things like script location and the command to be run.</span></span> <span data-ttu-id="4f0f4-111">Tato konfigurace může být uložen v konfiguračních souborů, zadané na příkazovém řádku, nebo šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-111">This configuration can be stored in configuration files, specified on the command line, or in an Azure Resource Manager template.</span></span> <span data-ttu-id="4f0f4-112">Citlivá data se uloží v chráněném konfigurace, který je šifrovaný a dešifrovat jenom ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-112">Sensitive data can be stored in a protected configuration, which is encrypted and only decrypted inside the virtual machine.</span></span> <span data-ttu-id="4f0f4-113">Chráněné konfigurace je užitečné při provádění příkazu zahrnuje tajné klíče, jako například heslo.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-113">The protected configuration is useful when the execution command includes secrets such as a password.</span></span>

### <a name="public-configuration"></a><span data-ttu-id="4f0f4-114">Veřejná konfigurace</span><span class="sxs-lookup"><span data-stu-id="4f0f4-114">Public Configuration</span></span>
<span data-ttu-id="4f0f4-115">Schéma:</span><span class="sxs-lookup"><span data-stu-id="4f0f4-115">Schema:</span></span>

<span data-ttu-id="4f0f4-116">**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-116">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="4f0f4-117">Používejte názvy, jak vidíte níže, abyste předešli problémům s nasazení.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-117">Use the names as seen below to avoid deployment issues.</span></span>

* <span data-ttu-id="4f0f4-118">**commandToExecute**: (vyžaduje se, řetězce) k provedení skriptu bodu položka</span><span class="sxs-lookup"><span data-stu-id="4f0f4-118">**commandToExecute**: (required, string) the entry point script to execute</span></span>
* <span data-ttu-id="4f0f4-119">**fileUris**: (volitelné, pole řetězců) adresy URL pro soubory ke stažení.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-119">**fileUris**: (optional, string array) the URLs for files to be downloaded.</span></span>
* <span data-ttu-id="4f0f4-120">**časové razítko** (volitelné, celé číslo) můžete do tohoto pole pouze pro aktivaci spusťte skript tím, že změníte hodnotu tohoto pole.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-120">**timestamp** (optional, integer) use this field only to trigger a rerun of the script by changing value of this field.</span></span>

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a><span data-ttu-id="4f0f4-121">Chráněné konfigurace</span><span class="sxs-lookup"><span data-stu-id="4f0f4-121">Protected Configuration</span></span>
<span data-ttu-id="4f0f4-122">Schéma:</span><span class="sxs-lookup"><span data-stu-id="4f0f4-122">Schema:</span></span>

<span data-ttu-id="4f0f4-123">**Poznámka:** -názvy těchto vlastností jsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-123">**Note** - these property names are case sensitive.</span></span> <span data-ttu-id="4f0f4-124">Používejte názvy, jak vidíte níže, abyste předešli problémům s nasazení.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-124">Use the names as seen below to avoid deployment issues.</span></span>

* <span data-ttu-id="4f0f4-125">**commandToExecute**: (volitelné, string) k provedení skriptu bodu položku.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-125">**commandToExecute**: (optional, string) the entry point script to execute.</span></span> <span data-ttu-id="4f0f4-126">Toto pole použijte místo toho, pokud váš příkaz obsahuje tajné klíče, jako jsou hesla.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-126">Use this field instead if your command contains secrets such as passwords.</span></span>
* <span data-ttu-id="4f0f4-127">**storageAccountName**: (volitelné, string) název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-127">**storageAccountName**: (optional, string) the name of storage account.</span></span> <span data-ttu-id="4f0f4-128">Pokud chcete zadat pověření pro úložiště, musí být všechny fileUris adresy URL pro objekty BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-128">If you specify storage credentials, all fileUris must be URLs for Azure Blobs.</span></span>
* <span data-ttu-id="4f0f4-129">**storageAccountKey**: (volitelné, string) přístupový klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-129">**storageAccountKey**: (optional, string) the access key of storage account.</span></span>

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a><span data-ttu-id="4f0f4-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4f0f4-130">Azure CLI</span></span>
<span data-ttu-id="4f0f4-131">Pokud používáte Azure CLI ke spuštění rozšíření vlastních skriptů, vytvořte konfigurační soubor nebo soubory obsahující alespoň identifikátor uri souboru a spuštění příkazu skriptu.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-131">When using the Azure CLI to run the Custom Script Extension, create a configuration file or files containing at minimum the file uri, and the script execution command.</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="4f0f4-132">Volitelně můžete nastavení zadané v příkazu jako řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-132">Optionally the settings can be specified in the command as a JSON formatted string.</span></span> <span data-ttu-id="4f0f4-133">To umožňuje konfiguraci zadat během provádění a bez jiný konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-133">This allows the configuration to be specified during execution and without a separate configuration file.</span></span>

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],"commandToExecute": "./test.sh"}'
```

### <a name="azure-cli-examples"></a><span data-ttu-id="4f0f4-134">Příklady rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="4f0f4-134">Azure CLI Examples</span></span>

<span data-ttu-id="4f0f4-135">**Příklad 1** -veřejné konfigurace pomocí souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-135">**Example 1** - Public configuration with script file.</span></span>

```json
{
  "fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],
  "commandToExecute": "./test.sh"
}
```

<span data-ttu-id="4f0f4-136">Azure CLI příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f0f4-136">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="4f0f4-137">**Příklad 2** -veřejné konfigurace s žádný soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-137">**Example 2** - Public configuration with no script file.</span></span>

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

<span data-ttu-id="4f0f4-138">Azure CLI příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f0f4-138">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

<span data-ttu-id="4f0f4-139">**Příklad 3** – veřejné konfigurační soubor se používá k určení souboru skriptu URI a chráněné konfigurační soubor se používá k určení příkaz, který má být proveden.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-139">**Example 3** - A public configuration file is used to specify the script file URI, and a protected configuration file is used to specify the command to be executed.</span></span>

<span data-ttu-id="4f0f4-140">Veřejné konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="4f0f4-140">Public configuration file:</span></span>

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

<span data-ttu-id="4f0f4-141">Chráněné konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="4f0f4-141">Protected configuration file:</span></span>  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

<span data-ttu-id="4f0f4-142">Azure CLI příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f0f4-142">Azure CLI command:</span></span>

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a><span data-ttu-id="4f0f4-143">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="4f0f4-143">Resource Manager Template</span></span>
<span data-ttu-id="4f0f4-144">Rozšíření vlastních skriptů Azure můžete spustit v době nasazení virtuálního počítače pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-144">The Azure Custom Script Extension can be run at Virtual Machine deployment time using a Resource Manager template.</span></span> <span data-ttu-id="4f0f4-145">Uděláte to tak, přidejte do šablony nasazení správně formátovaný JSON.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-145">To do so, add properly formatted JSON to the deployment template.</span></span>

### <a name="resource-manager-examples"></a><span data-ttu-id="4f0f4-146">Příklady Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4f0f4-146">Resource Manager Examples</span></span>
<span data-ttu-id="4f0f4-147">**Příklad 1** -veřejné konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-147">**Example 1** - public configuration.</span></span>

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

<span data-ttu-id="4f0f4-148">**Příklad 2** -spuštění příkazu v chráněné konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-148">**Example 2** - execution command in protected configuration.</span></span>

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

<span data-ttu-id="4f0f4-149">Najdete v článku .net Core Hudba úložiště ukázkové kompletní příklad - [Hudba úložiště ukázkové](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).</span><span class="sxs-lookup"><span data-stu-id="4f0f4-149">See the .Net Core Music Store Demo for a complete example - [Music Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="4f0f4-150">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="4f0f4-150">Troubleshooting</span></span>
<span data-ttu-id="4f0f4-151">Při spuštění rozšíření vlastních skriptů, skript se vytvoří nebo stáhli do adresáře podobně jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-151">When the Custom Script Extension runs, the script is created or downloaded into a directory similar to the following example.</span></span> <span data-ttu-id="4f0f4-152">Výstup příkazu je také uložen do tohoto adresáře `stdout` a `stderr` souboru.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-152">The command output is also saved into this directory in `stdout` and `stderr` file.</span></span>

```bash
/var/lib/waagent/custom-script/download/0/
```

<span data-ttu-id="4f0f4-153">Přípona skriptu Azure vytvoří protokol, který je zde uveden.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-153">The Azure Script Extension produces a log, which can be found here.</span></span>

```bash
/var/log/azure/custom-script/handler.log
```

<span data-ttu-id="4f0f4-154">Provádění stav rozšíření vlastních skriptů je také možné načíst pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="4f0f4-154">The execution state of the Custom Script Extension can also be retrieved with the Azure CLI.</span></span>

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

<span data-ttu-id="4f0f4-155">Výstup vypadá následující text:</span><span class="sxs-lookup"><span data-stu-id="4f0f4-155">The output looks like the following text:</span></span>

```azurecli
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a><span data-ttu-id="4f0f4-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f0f4-156">Next Steps</span></span>
<span data-ttu-id="4f0f4-157">Informace o skriptu rozšíření jiných virtuálních počítačů, v tématu [přehled Azure rozšíření skriptů pro Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4f0f4-157">For information on other VM Script Extensions, see [Azure Script Extension overview for Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

