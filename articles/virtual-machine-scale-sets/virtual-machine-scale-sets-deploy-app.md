---
title: "Nastaví aaaDeploy aplikace na škálování virtuálních počítačů"
description: "Pomocí rozšíření toodepoy aplikace na sady škálování virtuálního počítače Azure."
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
ms.openlocfilehash: 5f3988b9511d80370a8be1fc042c21fee212506e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a><span data-ttu-id="a7258-103">Nasazení aplikace na sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a7258-103">Deploy your application on virtual machine scale sets</span></span>

<span data-ttu-id="a7258-104">Tento článek popisuje různé způsoby, jak nastavit tooinstall softwaru škálované hello čas hello je zřízený.</span><span class="sxs-lookup"><span data-stu-id="a7258-104">This article describes different ways of how tooinstall software at hello time hello scale set is provisioned.</span></span>

<span data-ttu-id="a7258-105">Může být vhodné tooreview hello [přehled návrhu nastavení škálování](virtual-machine-scale-sets-design-overview.md) článek, který popisuje některé z hello omezení, která sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7258-105">You may want tooreview hello [Scale Set Design Overview](virtual-machine-scale-sets-design-overview.md) article, which describes some of hello limits imposed by virtual machine scale sets.</span></span>

## <a name="capture-and-reuse-an-image"></a><span data-ttu-id="a7258-106">Zaznamenání a znovu použít bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="a7258-106">Capture and reuse an image</span></span>

<span data-ttu-id="a7258-107">Můžete vytvořit virtuální počítač nastavené v Azure tooprepare bitovou kopii základní pro vaši škálování.</span><span class="sxs-lookup"><span data-stu-id="a7258-107">You can use a virtual machine you have in Azure tooprepare a base-image for your scale set.</span></span> <span data-ttu-id="a7258-108">Tento proces vytvoří ve vašem účtu úložiště, které můžete použít jako základní bitová kopie hello pro škálovací sadu spravovaných disků.</span><span class="sxs-lookup"><span data-stu-id="a7258-108">This process creates a managed disk in your storage account, which you can reference as hello base image for your scale set.</span></span> 

<span data-ttu-id="a7258-109">Hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a7258-109">Do hello following steps:</span></span>

1. <span data-ttu-id="a7258-110">Vytvořit virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="a7258-110">Create an Azure Virtual Machine</span></span>
   * <span data-ttu-id="a7258-111">[Linux][linux-vm-create]</span><span class="sxs-lookup"><span data-stu-id="a7258-111">[Linux][linux-vm-create]</span></span>
   * <span data-ttu-id="a7258-112">[Windows][windows-vm-create]</span><span class="sxs-lookup"><span data-stu-id="a7258-112">[Windows][windows-vm-create]</span></span>

2. <span data-ttu-id="a7258-113">Vzdálené do hello virtuálního počítače a přizpůsobení hello systému tooyour libosti.</span><span class="sxs-lookup"><span data-stu-id="a7258-113">Remote into hello virtual machine and customize hello system tooyour liking.</span></span>

   <span data-ttu-id="a7258-114">Pokud chcete, můžete nainstalovat aplikace hned.</span><span class="sxs-lookup"><span data-stu-id="a7258-114">If you want, you can install your application now.</span></span> <span data-ttu-id="a7258-115">Však vědět, že nainstalováním aplikace teď může provedete upgrade vaší aplikace složitější, protože může být nutné tooremove ho první.</span><span class="sxs-lookup"><span data-stu-id="a7258-115">However, know that by installing your application now, you may make upgrading your application more complicated because you may need tooremove it first.</span></span> <span data-ttu-id="a7258-116">Tento krok tooinstall místo toho můžete použít všechny požadované součásti, které aplikace může vyžadovat, podobně jako u konkrétní funkce runtime nebo operačního systému.</span><span class="sxs-lookup"><span data-stu-id="a7258-116">Instead, you can use this step tooinstall any prerequisites your application may need, like a specific runtime or operating system feature.</span></span>

3. <span data-ttu-id="a7258-117">Postupujte podle kurzu hello "zachytit počítač" pro buď [Linux] [ linux-vm-capture] nebo [Windows][windows-vm-capture].</span><span class="sxs-lookup"><span data-stu-id="a7258-117">Follow hello "capture a machine" tutorial for either [Linux][linux-vm-capture] or [Windows][windows-vm-capture].</span></span>

4. <span data-ttu-id="a7258-118">Vytvoření [sadu škálování virtuálního počítače] [ vmss-create] s hello bitové kopie URI zachytili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="a7258-118">Create a [Virtual Machine Scale Set][vmss-create] with hello image URI you captured in hello previous step.</span></span>

<span data-ttu-id="a7258-119">Další informace o discích najdete v tématu [přehled disky spravované](../virtual-machines/windows/managed-disks-overview.md) a [datové disky připojené k použití](virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="a7258-119">For more information about disks, see [Managed Disks Overview](../virtual-machines/windows/managed-disks-overview.md) and [Use Attached Data Disks](virtual-machine-scale-sets-attached-disks.md).</span></span>

## <a name="install-when-hello-scale-set-is-provisioned"></a><span data-ttu-id="a7258-120">Nainstaluje, když je zřízený sadě škálování hello</span><span class="sxs-lookup"><span data-stu-id="a7258-120">Install when hello scale set is provisioned</span></span>

<span data-ttu-id="a7258-121">Rozšíření virtuálního počítače může být použita škálovací sadu virtuálních počítačů tooa.</span><span class="sxs-lookup"><span data-stu-id="a7258-121">Virtual machine extensions can be applied tooa virtual machine scale set.</span></span> <span data-ttu-id="a7258-122">S rozšířením virtuálního počítače můžete přizpůsobit hello virtuálních počítačů v škálování nastavit jako pro celou skupinu.</span><span class="sxs-lookup"><span data-stu-id="a7258-122">With a virtual machine extension, you can customize hello virtual machines in a scale set as a whole group.</span></span> <span data-ttu-id="a7258-123">Další informace o rozšířeních najdete v tématu [rozšíření virtuálního počítače](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a7258-123">For more information about extensions, see [Virtual Machine Extensions](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="a7258-124">Existují tři hlavní rozšíření, které můžete použít, podle toho, pokud váš operační systém se systémem Linux nebo systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a7258-124">There are three main extensions you can use, depending on if your operating system is Linux-based or Windows-based.</span></span>

### <a name="windows"></a><span data-ttu-id="a7258-125">Windows</span><span class="sxs-lookup"><span data-stu-id="a7258-125">Windows</span></span>

<span data-ttu-id="a7258-126">Pro operační systém na základě systému Windows, použijte buď hello **vlastní skript v1.8** rozšíření nebo hello **DSC Powershellu** rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a7258-126">For a Windows-based operating system, use either hello **Custom Script v1.8** extension, or hello **PowerShell DSC** extension.</span></span>

#### <a name="custom-script"></a><span data-ttu-id="a7258-127">Vlastní skript</span><span class="sxs-lookup"><span data-stu-id="a7258-127">Custom Script</span></span>

<span data-ttu-id="a7258-128">Hello rozšíření vlastních skriptů spouští skript na každou instanci virtuálního počítače ve škálovací sadě hello.</span><span class="sxs-lookup"><span data-stu-id="a7258-128">hello Custom Script extension runs a script on each virtual machine instance in hello scale set.</span></span> <span data-ttu-id="a7258-129">Konfigurační soubor nebo proměnná Určuje, které soubory jsou stažené toohello virtuálního počítače, a poté co příkaz spustí.</span><span class="sxs-lookup"><span data-stu-id="a7258-129">A config file or variable indicates which files are downloaded toohello virtual machine, and then what command runs.</span></span> <span data-ttu-id="a7258-130">Můžete použít tento toorun instalační program, skript, dávkového souboru jakéhokoliv spustitelného souboru, třeba.</span><span class="sxs-lookup"><span data-stu-id="a7258-130">You could use this toorun an installer, a script, a batch file, any executable for example.</span></span>

<span data-ttu-id="a7258-131">Zatřiďovací tabulku používá prostředí PowerShell pro nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="a7258-131">PowerShell uses a hashtable for hello settings.</span></span> <span data-ttu-id="a7258-132">Tento příklad konfiguruje toorun rozšíření vlastních skriptů hello Powershellový skript, který nainstaluje službu IIS.</span><span class="sxs-lookup"><span data-stu-id="a7258-132">This example configures hello custom script extension toorun a PowerShell script that installs IIS.</span></span>

```powershell
# Setup extension configuration hashtable variable
$customConfig = @{
  "fileUris" = @("https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1");
  "commandToExecute" = "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> `"%TEMP%\StartupLog.txt`" 2>&1";
};

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Compute -Type CustomScriptExtension -TypeHandlerVersion 1.8 -Name "customscript1" -Setting $customConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "MyVmssTest143"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
><span data-ttu-id="a7258-133">Použití hello `-ProtectedSetting` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="a7258-133">Use hello `-ProtectedSetting` switch for any settings that may contain sensitive information.</span></span>

---------


<span data-ttu-id="a7258-134">Rozhraní příkazového řádku Azure používá soubor json pro nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="a7258-134">Azure CLI uses a json file for hello settings.</span></span> <span data-ttu-id="a7258-135">Tento příklad konfiguruje toorun rozšíření vlastních skriptů hello Powershellový skript, který nainstaluje službu IIS.</span><span class="sxs-lookup"><span data-stu-id="a7258-135">This example configures hello custom script extension toorun a PowerShell script that installs IIS.</span></span> <span data-ttu-id="a7258-136">Uložte následující soubor json jako hello _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="a7258-136">Save hello following json file as _settings.json_.</span></span>

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/temp/install-iis.ps1"
  ],
  "commandToExecute": "PowerShell -ExecutionPolicy Unrestricted .\install-iis.ps1 >> \"%TEMP%\StartupLog.txt\" 2>&1"
}
```

<span data-ttu-id="a7258-137">Potom spusťte tento příkaz rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="a7258-137">Then, run this Azure CLI command.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Compute --version 1.8 --name CustomScriptExtension --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="a7258-138">Použití hello `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="a7258-138">Use hello `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

### <a name="powershell-dsc"></a><span data-ttu-id="a7258-139">Prostředí PowerShell DSC</span><span class="sxs-lookup"><span data-stu-id="a7258-139">PowerShell DSC</span></span>

<span data-ttu-id="a7258-140">Můžete použít PowerShell DSC toocustomize hello škálovací sadu virtuálních počítačů instance.</span><span class="sxs-lookup"><span data-stu-id="a7258-140">You can use PowerShell DSC toocustomize hello scale set vm instances.</span></span> <span data-ttu-id="a7258-141">Hello **DSC** rozšíření, které zveřejnil **Microsoft.Powershell** nasadí a spustí konfigurace DSC hello zadaný na každou instanci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a7258-141">hello **DSC** extension published by **Microsoft.Powershell** deploys and runs hello provided DSC configuration on each virtual machine instance.</span></span> <span data-ttu-id="a7258-142">Konfiguračním souboru nebo proměnná informuje hello rozšíření kde *.zip* balíček je a který _funkce skriptu_ toorun kombinaci.</span><span class="sxs-lookup"><span data-stu-id="a7258-142">A config file or variable tells hello extension where *.zip* package is, and which _script-function_ combination toorun.</span></span>

<span data-ttu-id="a7258-143">Zatřiďovací tabulku používá prostředí PowerShell pro nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="a7258-143">PowerShell uses a hashtable for hello settings.</span></span> <span data-ttu-id="a7258-144">Tento příklad nasadí DSC balíček, který nainstaluje službu IIS.</span><span class="sxs-lookup"><span data-stu-id="a7258-144">This example deploys a DSC package that installs IIS.</span></span>

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

# Add hello extension toohello config
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig -Publisher Microsoft.Powershell -Type DSC -TypeHandlerVersion 2.24 -Name "dsc1" -Setting $dscConfig

# Send hello new config tooAzure
Update-AzureRmVmss -ResourceGroupName $rg -Name "myscaleset1"  -VirtualMachineScaleSet $vmssConfig
```

>[!IMPORTANT]
><span data-ttu-id="a7258-145">Použití hello `-ProtectedSetting` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="a7258-145">Use hello `-ProtectedSetting` switch for any settings that may contain sensitive information.</span></span>

-----------

<span data-ttu-id="a7258-146">Rozhraní příkazového řádku Azure používá json pro nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="a7258-146">Azure CLI uses a json for hello settings.</span></span> <span data-ttu-id="a7258-147">Tento příklad nasadí DSC balíček, který nainstaluje službu IIS.</span><span class="sxs-lookup"><span data-stu-id="a7258-147">This example deploys a DSC package that installs IIS.</span></span> <span data-ttu-id="a7258-148">Uložte následující soubor json jako hello _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="a7258-148">Save hello following json file as _settings.json_.</span></span>

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

<span data-ttu-id="a7258-149">Potom spusťte tento příkaz rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="a7258-149">Then, run this Azure CLI command.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Powershell --version 2.24 --name DSC --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="a7258-150">Použití hello `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="a7258-150">Use hello `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

### <a name="linux"></a><span data-ttu-id="a7258-151">Linux</span><span class="sxs-lookup"><span data-stu-id="a7258-151">Linux</span></span>

<span data-ttu-id="a7258-152">Linux můžete použít buď hello **vlastní skript v2.0** rozšíření nebo použijte **cloudu init** během vytváření.</span><span class="sxs-lookup"><span data-stu-id="a7258-152">Linux can use either hello **Custom Script v2.0** extension or use **cloud-init** during creation.</span></span>

<span data-ttu-id="a7258-153">Vlastní skript je jednoduchý rozšíření, která stáhne instancí virtuálního počítače toohello soubory a spustí příkaz.</span><span class="sxs-lookup"><span data-stu-id="a7258-153">Custom script is a simple extension that downloads files toohello virtual machine instances, and runs a command.</span></span>

#### <a name="custom-script"></a><span data-ttu-id="a7258-154">Vlastní skript</span><span class="sxs-lookup"><span data-stu-id="a7258-154">Custom Script</span></span>

<span data-ttu-id="a7258-155">Uložte následující soubor json jako hello _settings.json_.</span><span class="sxs-lookup"><span data-stu-id="a7258-155">Save hello following json file as _settings.json_.</span></span>

```json
{
  "fileUris": [
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
    "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
  ],
  "commandToExecute": "bash installserver.sh"
}
```

<span data-ttu-id="a7258-156">Pomocí rozhraní příkazového řádku Azure tooadd hello toto rozšíření tooan existující škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a7258-156">Use hello Azure CLI tooadd this extension tooan existing virtual machine scale set.</span></span> <span data-ttu-id="a7258-157">Každý virtuální počítač ve škálovací hello nastavit automaticky spustí hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a7258-157">Each virtual machine in hello scale set automatically runs hello extension.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

>[!IMPORTANT]
><span data-ttu-id="a7258-158">Použití hello `--protected-settings` přepínač pro všechna nastavení, která mohou obsahovat citlivé informace.</span><span class="sxs-lookup"><span data-stu-id="a7258-158">Use hello `--protected-settings` switch for any settings that may contain sensitive information.</span></span>

#### <a name="cloud-init"></a><span data-ttu-id="a7258-159">Init cloudu</span><span class="sxs-lookup"><span data-stu-id="a7258-159">Cloud-Init</span></span>

<span data-ttu-id="a7258-160">Init cloudu se používá, když se vytvoří sada škálování hello.</span><span class="sxs-lookup"><span data-stu-id="a7258-160">Cloud-Init is used when hello scale set is created.</span></span> <span data-ttu-id="a7258-161">Nejprve vytvořte místní soubor s názvem _cloudu init.txt_ a přidejte tooit vaší konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a7258-161">First, create a local file named _cloud-init.txt_ and add your configuration tooit.</span></span> <span data-ttu-id="a7258-162">Například v tématu [tento gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)</span><span class="sxs-lookup"><span data-stu-id="a7258-162">For example, see [this gist](https://gist.github.com/Thraka/27bd66b1fb79e11904fb62b7de08a8a6#file-cloud-init-txt)</span></span>

<span data-ttu-id="a7258-163">Nastavit hello pomocí rozhraní příkazového řádku Azure toocreate škálování.</span><span class="sxs-lookup"><span data-stu-id="a7258-163">Use hello Azure CLI toocreate a scale set.</span></span> <span data-ttu-id="a7258-164">Hello `--custom-data` pole lze zadat název souboru hello cloudu init skriptu.</span><span class="sxs-lookup"><span data-stu-id="a7258-164">hello `--custom-data` field accepts hello file name of a cloud-init script.</span></span>

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

## <a name="how-do-i-manage-application-updates"></a><span data-ttu-id="a7258-165">Jak lze spravovat aktualizace aplikace?</span><span class="sxs-lookup"><span data-stu-id="a7258-165">How do I manage application updates?</span></span>

<span data-ttu-id="a7258-166">Pokud jste nasadili aplikaci prostřednictvím rozšíření, změnit definice rozšíření hello nějakým způsobem.</span><span class="sxs-lookup"><span data-stu-id="a7258-166">If you deployed your application through an extension, alter hello extension definition in some way.</span></span> <span data-ttu-id="a7258-167">Tato změna způsobí, že instance virtuálních počítačů pro tooall hello rozšíření toobe znovu nasazena.</span><span class="sxs-lookup"><span data-stu-id="a7258-167">This change causes hello extension toobe redeployed tooall virtual machine instances.</span></span> <span data-ttu-id="a7258-168">Něco **musí** změnit o hello rozšíření, jako je například přejmenování odkazované souboru, jinak, Azure nemá došlo ke změně není najdete, který hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a7258-168">Something **must** be changed about hello extension, such as renaming a referenced file, otherwise, Azure does not see that hello extension has changed.</span></span>

<span data-ttu-id="a7258-169">Pokud jste zaručená aplikace hello do vlastní bitovou kopii operačního systému, použijte kanál automatického nasazení aktualizací aplikací.</span><span class="sxs-lookup"><span data-stu-id="a7258-169">If you baked hello application into your own operating system image, use an automated deployment pipeline for application updates.</span></span> <span data-ttu-id="a7258-170">Návrh vaší architektury toofacilitate rychlé odkládací dvoufázové instalace měřítka nastavit do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="a7258-170">Design your architecture toofacilitate rapid swapping of a staged scale set into production.</span></span> <span data-ttu-id="a7258-171">Dobrým příkladem tohoto přístupu je hello [Azure Spinnaker ovladač pracovní](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).</span><span class="sxs-lookup"><span data-stu-id="a7258-171">A good example of this approach is hello [Azure Spinnaker driver work](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).</span></span>

<span data-ttu-id="a7258-172">[Balírna](https://www.packer.io/) a [Terraform](https://www.terraform.io/) podpory Azure Resource Manager, tak můžete také definovat obrázků "jako kód" a sestavení je v Azure, pak použít hello virtuální pevný disk ve škálovací sadě.</span><span class="sxs-lookup"><span data-stu-id="a7258-172">[Packer](https://www.packer.io/) and [Terraform](https://www.terraform.io/) support Azure Resource Manager, so you can also define your images "as code" and build them in Azure, then use hello VHD in your scale set.</span></span> <span data-ttu-id="a7258-173">Ale to proto by se stal problematické pro Image marketplace, kde rozšíření nebo vlastní skripty se jsou důležitější vzhledem k tomu, že nemáte přímo upravit bits z webu marketplace.</span><span class="sxs-lookup"><span data-stu-id="a7258-173">However, doing so would become problematic for marketplace images, where extensions/custom scripts become more important since you don’t directly manipulate bits from marketplace.</span></span>

## <a name="what-happens-when-a-scale-set-scales-out"></a><span data-ttu-id="a7258-174">Co se stane, když sady měřítek škálování se?</span><span class="sxs-lookup"><span data-stu-id="a7258-174">What happens when a scale set scales out?</span></span>
<span data-ttu-id="a7258-175">Při přidání jedné nebo více virtuálních počítačů tooa škálovací sadu aplikace hello je automaticky nainstalována.</span><span class="sxs-lookup"><span data-stu-id="a7258-175">When you add one or more virtual machines tooa scale set, hello application is automatically installed.</span></span> <span data-ttu-id="a7258-176">Pro příklad nastaveného hello škálování má rozšíření definovaná, poběží na novém virtuálním počítači pokaždé, když je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="a7258-176">For example if hello scale set has extensions defined, they run on a new virtual machine each time it is created.</span></span> <span data-ttu-id="a7258-177">Pokud hello škálovací sada je založená na vlastní image, všechny nové virtuální počítač je kopii hello zdroj vlastní image.</span><span class="sxs-lookup"><span data-stu-id="a7258-177">If hello scale set is based on a custom image, any new virtual machine is a copy of hello source custom image.</span></span> <span data-ttu-id="a7258-178">Pokud hello škálovací sadu virtuálních počítačů jsou hostitelé kontejneru, může být spuštění kódu tooload hello kontejnery v rozšíření vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="a7258-178">If hello scale set virtual machines are container hosts, then you might have startup code tooload hello containers in a Custom Script Extension.</span></span> <span data-ttu-id="a7258-179">Nebo rozšíření může nainstalovat agenta, který registruje clusteru orchestrator, jako je například Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="a7258-179">Or, an extension might install an agent that registers with a cluster orchestrator, such as Azure Container Service.</span></span>


## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a><span data-ttu-id="a7258-180">Jak budete zavádět aktualizaci operačního systému napříč doménami aktualizace?</span><span class="sxs-lookup"><span data-stu-id="a7258-180">How do you roll out an OS update across update domains?</span></span>
<span data-ttu-id="a7258-181">Předpokládejme, že chcete bitové kopie operačního systému pro tooupdate zachováním škálování virtuálních počítačů hello nastavit systémem.</span><span class="sxs-lookup"><span data-stu-id="a7258-181">Suppose you want tooupdate your OS image while keeping hello virtual machine scale set running.</span></span> <span data-ttu-id="a7258-182">Prostředí PowerShell a hello rozhraní příkazového řádku Azure můžete aktualizovat hello Image virtuálního počítače, jeden virtuální počítač najednou.</span><span class="sxs-lookup"><span data-stu-id="a7258-182">PowerShell and hello Azure CLI can update hello virtual machine images, one virtual machine at a time.</span></span> <span data-ttu-id="a7258-183">Hello [upgradovat sadu škálování virtuálního počítače](./virtual-machine-scale-sets-upgrade-scale-set.md) článek obsahuje také další informace o jaké možnosti jsou k dispozici tooperform operační systém upgradovat napříč škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a7258-183">hello [Upgrade a Virtual Machine Scale Set](./virtual-machine-scale-sets-upgrade-scale-set.md) article also provides further information on what options are available tooperform an operating system upgrade across a virtual machine scale set.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7258-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7258-184">Next steps</span></span>

* [<span data-ttu-id="a7258-185">Pomocí prostředí PowerShell toomanage škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="a7258-185">Use PowerShell toomanage your scale set.</span></span>](virtual-machine-scale-sets-windows-manage.md)
* [<span data-ttu-id="a7258-186">Vytvořte šablonu sady škálování.</span><span class="sxs-lookup"><span data-stu-id="a7258-186">Create a scale set template.</span></span>](virtual-machine-scale-sets-mvss-start.md)


[linux-vm-create]: ../virtual-machines/linux/tutorial-manage-vm.md
[windows-vm-create]: ../virtual-machines/windows/tutorial-manage-vm.md
[linux-vm-capture]: ../virtual-machines/linux/capture-image.md
[windows-vm-capture]: ../virtual-machines/windows/capture-image.md 
[vmss-create]: virtual-machine-scale-sets-create.md

