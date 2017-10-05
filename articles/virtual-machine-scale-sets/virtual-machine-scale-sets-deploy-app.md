---
title: "Nasazení aplikace na sady škálování virtuálního počítače"
description: "Rozšíření do depoy aplikace použijte na sady škálování virtuálního počítače Azure."
services: virtual-machine-scale-sets
documentationcenter: 
author: thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.openlocfilehash: fa7d9d3bef4cb326844ede76171e8c566e87116b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a><span data-ttu-id="1db11-103">Nasazení aplikace na sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1db11-103">Deploy your application on virtual machine scale sets</span></span>

<span data-ttu-id="1db11-104">Tento článek popisuje různé způsoby instalace softwaru v době byly sadou škálování je zřízený.</span><span class="sxs-lookup"><span data-stu-id="1db11-104">This article describes different ways of how to install software at the time the scale set is provisioned.</span></span>

<span data-ttu-id="1db11-105">Můžete zkontrolovat [přehled návrhu nastavení škálování](virtual-machine-scale-sets-design-overview.md) článek, který popisuje některé z omezení, která sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1db11-105">You may want to review the [Scale Set Design Overview](virtual-machine-scale-sets-design-overview.md) article, which describes some of the limits imposed by virtual machine scale sets.</span></span>

## <a name="capture-and-reuse-an-image"></a><span data-ttu-id="1db11-106">Zaznamenání a znovu použít bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="1db11-106">Capture and reuse an image</span></span>

<span data-ttu-id="1db11-107">Můžete vytvořit virtuální počítač nastavené v Azure a připravit bitovou kopii základní pro vaši škálování.</span><span class="sxs-lookup"><span data-stu-id="1db11-107">You can use a virtual machine you have in Azure to prepare a base-image for your scale set.</span></span> <span data-ttu-id="1db11-108">Tento proces vytvoří ve vašem účtu úložiště, které můžete použít jako základní bitovou kopii pro škálovací sadu spravovaných disků.</span><span class="sxs-lookup"><span data-stu-id="1db11-108">This process creates a managed disk in your storage account, which you can reference as the base image for your scale set.</span></span> 

<span data-ttu-id="1db11-109">Proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1db11-109">Do the following steps:</span></span>

1. <span data-ttu-id="1db11-110">Vytvořit virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="1db11-110">Create an Azure Virtual Machine</span></span>
   * <span data-ttu-id="1db11-111">[Linux][linux-vm-create]</span><span class="sxs-lookup"><span data-stu-id="1db11-111">[Linux][linux-vm-create]</span></span>
   * <span data-ttu-id="1db11-112">[Windows][windows-vm-create]</span><span class="sxs-lookup"><span data-stu-id="1db11-112">[Windows][windows-vm-create]</span></span>

2. <span data-ttu-id="1db11-113">Vzdálené do virtuálního počítače a přizpůsobení systému libovolně.</span><span class="sxs-lookup"><span data-stu-id="1db11-113">Remote into the virtual machine and customize the system to your liking.</span></span>

   <span data-ttu-id="1db11-114">Pokud chcete, můžete nainstalovat aplikace hned.</span><span class="sxs-lookup"><span data-stu-id="1db11-114">If you want, you can install your application now.</span></span> <span data-ttu-id="1db11-115">Však vědět, že nainstalováním aplikace teď, může být upgrade vaší aplikace složitější, protože je nutné nejprve odeberte.</span><span class="sxs-lookup"><span data-stu-id="1db11-115">However, know that by installing your application now, you may make upgrading your application more complicated because you may need to remove it first.</span></span> <span data-ttu-id="1db11-116">Místo toho můžete tento krok nainstalovat všechny požadované součásti, které aplikace může vyžadovat, podobně jako u konkrétní funkce runtime nebo operačního systému.</span><span class="sxs-lookup"><span data-stu-id="1db11-116">Instead, you can use this step to install any prerequisites your application may need, like a specific runtime or operating system feature.</span></span>

3. <span data-ttu-id="1db11-117">Postupujte podle kurzu "zachytit počítač" pro buď [Linux] [ linux-vm-capture] nebo [Windows][windows-vm-capture].</span><span class="sxs-lookup"><span data-stu-id="1db11-117">Follow the "capture a machine" tutorial for either [Linux][linux-vm-capture] or [Windows][windows-vm-capture].</span></span>

4. <span data-ttu-id="1db11-118">Vytvoření [sadu škálování virtuálního počítače] [ vmss-create] s bitovou kopií URI zachytili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="1db11-118">Create a [Virtual Machine Scale Set][vmss-create] with the image URI you captured in the previous step.</span></span>

<span data-ttu-id="1db11-119">Další informace o discích najdete v tématu [přehled disky spravované](../virtual-machines/windows/managed-disks-overview.md) a [datové disky připojené k použití](virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="1db11-119">For more information about disks, see [Managed Disks Overview](../virtual-machines/windows/managed-disks-overview.md) and [Use Attached Data Disks](virtual-machine-scale-sets-attached-disks.md).</span></span>

## <a name="install-when-the-scale-set-is-provisioned"></a><span data-ttu-id="1db11-120">Nainstaluje, když byly sadou škálování je zřízený.</span><span class="sxs-lookup"><span data-stu-id="1db11-120">Install when the scale set is provisioned</span></span>

<span data-ttu-id="1db11-121">Rozšíření virtuálního počítače je použít pro škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1db11-121">Virtual machine extensions can be applied to a virtual machine scale set.</span></span> <span data-ttu-id="1db11-122">S rozšířením virtuálního počítače můžete přizpůsobit virtuálních počítačů ve škále nastavit jako pro celou skupinu.</span><span class="sxs-lookup"><span data-stu-id="1db11-122">With a virtual machine extension, you can customize the virtual machines in a scale set as a whole group.</span></span> <span data-ttu-id="1db11-123">Další informace o rozšířeních najdete v tématu [rozšíření virtuálního počítače](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1db11-123">For more information about extensions, see [Virtual Machine Extensions](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="1db11-124">Existují tři hlavní rozšíření, které můžete použít, podle toho, pokud váš operační systém se systémem Linux nebo systému Windows.</span><span class="sxs-lookup"><span data-stu-id="1db11-124">There are three main extensions you can use, depending on if your operating system is Linux-based or Windows-based.</span></span>

### <a name="windows"></a><span data-ttu-id="1db11-125">Windows</span><span class="sxs-lookup"><span data-stu-id="1db11-125">Windows</span></span>

<span data-ttu-id="1db11-126">Pro operační systém na základě systému Windows, použijte buď **vlastní skript v1.8** rozšíření, nebo **DSC Powershellu** rozšíření.</span><span class="sxs-lookup"><span data-stu-id="1db11-126">For a Windows-based operating system, use either the **Custom Script v1.8** extension, or the **PowerShell DSC** extension.</span></span>

#### <a name="custom-script"></a><span data-ttu-id="1db11-127">Vlastní skript</span><span class="sxs-lookup"><span data-stu-id="1db11-127">Custom Script</span></span>

<span data-ttu-id="1db11-128">Rozšíření vlastních skriptů spouští skript na každou instanci virtuálního počítače v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="1db11-128">The Custom Script extension runs a script on each virtual machine instance in the scale set.</span></span> <span data-ttu-id="1db11-129">Konfigurační soubor nebo proměnná označuje soubory, které se stáhnou do virtuálního počítače, a poté co příkaz spustí.</span><span class="sxs-lookup"><span data-stu-id="1db11-129">A config file or variable indicates which files are downloaded to the virtual machine, and then what command runs.</span></span> <span data-ttu-id="1db11-130">Můžete to použít ke spuštění instalačního programu, skript, dávkového souboru, jakéhokoliv spustitelného souboru, třeba.</span><span class="sxs-lookup"><span data-stu-id="1db11-130">You could use this to run an installer, a script, a batch file, any executable for example.</span></span>

<span data-ttu-id="1db11-131">Zatřiďovací tabulku používá prostředí PowerShell pro nastavení.</span><span class="sxs-lookup"><span data-stu-id="1db11-131">PowerShell uses a hashtable for the settings.</span></span> <span data-ttu-id="1db11-132">Tento příklad konfiguruje rozšíření vlastních skriptů spustit skript prostředí PowerShell, který nainstaluje službu IIS.</span><span class="sxs-lookup"><span data-stu-id="1db11-132">This example configures the custom script extension to run a PowerShell script that installs IIS.</span></span>

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
><span data-ttu-id="1db11-133">Použití `-ProtectedSetting` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="1db11-133">Use the `-ProtectedSetting` switch for any settings that may contain sensitive information.</span></span>

---------


<span data-ttu-id="1db11-134">Rozhraní příkazového řádku Azure používá soubor json pro nastavení.</span><span class="sxs-lookup"><span data-stu-id="1db11-134">Azure CLI uses a json file for the settings.</span></span> <span data-ttu-id="1db11-135">Tento příklad konfiguruje rozšíření vlastních skriptů spustit skript prostředí PowerShell, který nainstaluje službu IIS.</span><span class="sxs-lookup"><span data-stu-id="1db11-135">This example configures the custom script extension to run a PowerShell script that installs IIS.</span></span> <span data-ttu-id="1db11-136">Uložte následující soubor json jako _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="1db11-136">Save the following json file as _settings.json_.</span></span>

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1"
  ],
  "commandToExecute": "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> \"%TEMP%\StartupLog.txt\" 2>&1"
}
```

<span data-ttu-id="1db11-137">Potom spusťte tento příkaz rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1db11-137">Then, run this Azure CLI command.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Compute --version 1.8 --name CustomScriptExtension --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="1db11-138">Použití `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="1db11-138">Use the `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

### <a name="powershell-dsc"></a><span data-ttu-id="1db11-139">Prostředí PowerShell DSC</span><span class="sxs-lookup"><span data-stu-id="1db11-139">PowerShell DSC</span></span>

<span data-ttu-id="1db11-140">DSC prostředí PowerShell můžete použít k přizpůsobení instance škálovací sady virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1db11-140">You can use PowerShell DSC to customize the scale set vm instances.</span></span> <span data-ttu-id="1db11-141">**DSC** rozšíření, které zveřejnil **Microsoft.Powershell** nasadí a běží Zadaná konfigurace DSC na každou instanci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1db11-141">The **DSC** extension published by **Microsoft.Powershell** deploys and runs the provided DSC configuration on each virtual machine instance.</span></span> <span data-ttu-id="1db11-142">Konfiguračním souboru nebo proměnná informuje rozšíření kde *.zip* balíček je a který _funkce skriptu_ kombinace ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="1db11-142">A config file or variable tells the extension where *.zip* package is, and which _script-function_ combination to run.</span></span>

<span data-ttu-id="1db11-143">Zatřiďovací tabulku používá prostředí PowerShell pro nastavení.</span><span class="sxs-lookup"><span data-stu-id="1db11-143">PowerShell uses a hashtable for the settings.</span></span> <span data-ttu-id="1db11-144">Tento příklad nasadí DSC balíček, který nainstaluje službu IIS.</span><span class="sxs-lookup"><span data-stu-id="1db11-144">This example deploys a DSC package that installs IIS.</span></span>

```powershell
# Setup extension configuration hashtable variable
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Add the extension to the config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send the new config to Azure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
><span data-ttu-id="1db11-145">Použití `-ProtectedSetting` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="1db11-145">Use the `-ProtectedSetting` switch for any settings that may contain sensitive information.</span></span>

-----------

<span data-ttu-id="1db11-146">Rozhraní příkazového řádku Azure používá json pro nastavení.</span><span class="sxs-lookup"><span data-stu-id="1db11-146">Azure CLI uses a json for the settings.</span></span> <span data-ttu-id="1db11-147">Tento příklad nasadí DSC balíček, který nainstaluje službu IIS.</span><span class="sxs-lookup"><span data-stu-id="1db11-147">This example deploys a DSC package that installs IIS.</span></span> <span data-ttu-id="1db11-148">Uložte následující soubor json jako _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="1db11-148">Save the following json file as _settings.json_.</span></span>

```json
{
  "wmfVersion": "latest",
  "configuration": {
    "url": "https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/dsc.zip",
    "script": "configure-http.ps1",
    "function": "WebsiteTest"
  }
}
```

<span data-ttu-id="1db11-149">Potom spusťte tento příkaz rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1db11-149">Then, run this Azure CLI command.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Powershell --version 2.24 --name DSC --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="1db11-150">Použití `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="1db11-150">Use the `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

### <a name="linux"></a><span data-ttu-id="1db11-151">Linux</span><span class="sxs-lookup"><span data-stu-id="1db11-151">Linux</span></span>

<span data-ttu-id="1db11-152">Linux můžete použít buď **vlastní skript v2.0** rozšíření nebo použijte **cloudu init** během vytváření.</span><span class="sxs-lookup"><span data-stu-id="1db11-152">Linux can use either the **Custom Script v2.0** extension or use **cloud-init** during creation.</span></span>

<span data-ttu-id="1db11-153">Vlastní skript je jednoduchý rozšíření, které se stahování souborů do instance virtuálních počítačů a spustí příkaz.</span><span class="sxs-lookup"><span data-stu-id="1db11-153">Custom script is a simple extension that downloads files to the virtual machine instances, and runs a command.</span></span>

#### <a name="custom-script"></a><span data-ttu-id="1db11-154">Vlastní skript</span><span class="sxs-lookup"><span data-stu-id="1db11-154">Custom Script</span></span>

<span data-ttu-id="1db11-155">Uložte následující soubor json jako _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="1db11-155">Save the following json file as _settings.json_.</span></span>

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

<span data-ttu-id="1db11-156">Pomocí rozhraní příkazového řádku Azure přidáte toto rozšíření do existující sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1db11-156">Use the Azure CLI to add this extension to an existing virtual machine scale set.</span></span> <span data-ttu-id="1db11-157">Každý virtuální počítač ve škálovací nastavit automaticky spustí rozšíření.</span><span class="sxs-lookup"><span data-stu-id="1db11-157">Each virtual machine in the scale set automatically runs the extension.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="1db11-158">Použití `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="1db11-158">Use the `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

#### <a name="cloud-init"></a><span data-ttu-id="1db11-159">Init cloudu</span><span class="sxs-lookup"><span data-stu-id="1db11-159">Cloud-Init</span></span>

<span data-ttu-id="1db11-160">Init cloudu se používá, když se vytvoří sada škálování.</span><span class="sxs-lookup"><span data-stu-id="1db11-160">Cloud-Init is used when the scale set is created.</span></span> <span data-ttu-id="1db11-161">Nejprve vytvořte místní soubor s názvem _cloudu init.txt_ a přidejte do ní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="1db11-161">First, create a local file named _cloud-init.txt_ and add your configuration to it.</span></span> <span data-ttu-id="1db11-162">Například v tématu [tento gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)</span><span class="sxs-lookup"><span data-stu-id="1db11-162">For example, see [this gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)</span></span>

<span data-ttu-id="1db11-163">Pomocí rozhraní příkazového řádku Azure k vytvoření sady škálování.</span><span class="sxs-lookup"><span data-stu-id="1db11-163">Use the Azure CLI to create a scale set.</span></span> <span data-ttu-id="1db11-164">`--custom-data` Pole lze zadat název souboru skriptu init cloudu.</span><span class="sxs-lookup"><span data-stu-id="1db11-164">The `--custom-data` field accepts the file name of a cloud-init script.</span></span>

```azurecli
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image Canonical:UbuntuServer:14.04.4-LTS:latest \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

## <a name="how-do-i-manage-application-updates"></a><span data-ttu-id="1db11-165">Jak lze spravovat aktualizace aplikace?</span><span class="sxs-lookup"><span data-stu-id="1db11-165">How do I manage application updates?</span></span>

<span data-ttu-id="1db11-166">Pokud jste nasadili aplikaci prostřednictvím rozšíření, změnit definici rozšíření nějakým způsobem.</span><span class="sxs-lookup"><span data-stu-id="1db11-166">If you deployed your application through an extension, alter the extension definition in some way.</span></span> <span data-ttu-id="1db11-167">Tato změna způsobí, že rozšíření znovu nasadit na všechny instance virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1db11-167">This change causes the extension to be redeployed to all virtual machine instances.</span></span> <span data-ttu-id="1db11-168">Něco **musí** změnit o rozšíření, jako je například přejmenování odkazované souboru, jinak, Azure nemá není najdete, které se změnily rozšíření.</span><span class="sxs-lookup"><span data-stu-id="1db11-168">Something **must** be changed about the extension, such as renaming a referenced file, otherwise, Azure does not see that the extension has changed.</span></span>

<span data-ttu-id="1db11-169">Pokud jste zaručená aplikace do vlastní bitovou kopii operačního systému, použijte kanál automatického nasazení aktualizací aplikací.</span><span class="sxs-lookup"><span data-stu-id="1db11-169">If you baked the application into your own operating system image, use an automated deployment pipeline for application updates.</span></span> <span data-ttu-id="1db11-170">Návrh vaší architektury pro usnadnění rychlé odkládací dvoufázové instalace měřítka nastavit do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="1db11-170">Design your architecture to facilitate rapid swapping of a staged scale set into production.</span></span> <span data-ttu-id="1db11-171">Dobrým příkladem tohoto přístupu je [Azure Spinnaker ovladač pracovní](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).</span><span class="sxs-lookup"><span data-stu-id="1db11-171">A good example of this approach is the [Azure Spinnaker driver work](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).</span></span>

<span data-ttu-id="1db11-172">[Balírna](https://www.packer.io/) a [Terraform](https://www.terraform.io/) podpory Azure Resource Manager, tak můžete také definovat obrázků "jako kód" a sestavení je v Azure, pak použít virtuální pevný disk ve škálovací sadě.</span><span class="sxs-lookup"><span data-stu-id="1db11-172">[Packer](https://www.packer.io/) and [Terraform](https://www.terraform.io/) support Azure Resource Manager, so you can also define your images "as code" and build them in Azure, then use the VHD in your scale set.</span></span> <span data-ttu-id="1db11-173">Ale to proto by se stal problematické pro Image marketplace, kde rozšíření nebo vlastní skripty se jsou důležitější vzhledem k tomu, že nemáte přímo upravit bits z webu marketplace.</span><span class="sxs-lookup"><span data-stu-id="1db11-173">However, doing so would become problematic for marketplace images, where extensions/custom scripts become more important since you don’t directly manipulate bits from marketplace.</span></span>

## <a name="what-happens-when-a-scale-set-scales-out"></a><span data-ttu-id="1db11-174">Co se stane, když sady měřítek škálování se?</span><span class="sxs-lookup"><span data-stu-id="1db11-174">What happens when a scale set scales out?</span></span>
<span data-ttu-id="1db11-175">Při přidání jedné nebo více virtuálních počítačů k sadě škálování, je automaticky nainstalována aplikace.</span><span class="sxs-lookup"><span data-stu-id="1db11-175">When you add one or more virtual machines to a scale set, the application is automatically installed.</span></span> <span data-ttu-id="1db11-176">Pro příklad nastaveného měřítka má rozšíření definovaná, poběží na novém virtuálním počítači pokaždé, když je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="1db11-176">For example if the scale set has extensions defined, they run on a new virtual machine each time it is created.</span></span> <span data-ttu-id="1db11-177">Pokud byly sadou škálování je založená na vlastní image, všechny nové virtuální počítač je kopie zdrojové vlastní Image.</span><span class="sxs-lookup"><span data-stu-id="1db11-177">If the scale set is based on a custom image, any new virtual machine is a copy of the source custom image.</span></span> <span data-ttu-id="1db11-178">Pokud virtuální počítače sady škálování hostitele kontejneru, může mít kód spuštění načíst kontejnery v rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="1db11-178">If the scale set virtual machines are container hosts, then you might have startup code to load the containers in a Custom Script Extension.</span></span> <span data-ttu-id="1db11-179">Nebo rozšíření může nainstalovat agenta, který registruje clusteru orchestrator, jako je například Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="1db11-179">Or, an extension might install an agent that registers with a cluster orchestrator, such as Azure Container Service.</span></span>


## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a><span data-ttu-id="1db11-180">Jak budete zavádět aktualizaci operačního systému napříč doménami aktualizace?</span><span class="sxs-lookup"><span data-stu-id="1db11-180">How do you roll out an OS update across update domains?</span></span>
<span data-ttu-id="1db11-181">Předpokládejme, že chcete aktualizaci bitové kopie operačního systému a zachovat přitom sad systémem škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1db11-181">Suppose you want to update your OS image while keeping the virtual machine scale set running.</span></span> <span data-ttu-id="1db11-182">Prostředí PowerShell a rozhraní příkazového řádku Azure můžete aktualizovat Image virtuálního počítače, jeden virtuální počítač najednou.</span><span class="sxs-lookup"><span data-stu-id="1db11-182">PowerShell and the Azure CLI can update the virtual machine images, one virtual machine at a time.</span></span> <span data-ttu-id="1db11-183">[Upgradovat sadu škálování virtuálního počítače](./virtual-machine-scale-sets-upgrade-scale-set.md) článek obsahuje také další informace o jaké možnosti jsou dostupné k provedení upgradu operačního systému přes škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1db11-183">The [Upgrade a Virtual Machine Scale Set](./virtual-machine-scale-sets-upgrade-scale-set.md) article also provides further information on what options are available to perform an operating system upgrade across a virtual machine scale set.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1db11-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1db11-184">Next steps</span></span>

* [<span data-ttu-id="1db11-185">Použití Powershellu ke správě škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="1db11-185">Use PowerShell to manage your scale set.</span></span>](virtual-machine-scale-sets-windows-manage.md)
* [<span data-ttu-id="1db11-186">Vytvořte šablonu sady škálování.</span><span class="sxs-lookup"><span data-stu-id="1db11-186">Create a scale set template.</span></span>](virtual-machine-scale-sets-mvss-start.md)


[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md

