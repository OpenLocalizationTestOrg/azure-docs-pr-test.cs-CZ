---
title: "aaaConvert Azure tooa sady škálování virtuálního počítače | Microsoft Docs"
description: "Vytvořit a nasadit škálování virtuálního počítače Linux Azure s hello rozhraní příkazového řádku Azure."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 04/05/2017
ms.author: adegeo
ms.openlocfilehash: e228282ac4855cef589b8500e74e9d461f9aed84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="convert-an-existing-azure-virtual-machine-tooa-scale-set"></a><span data-ttu-id="88e48-103">Převést existující sady škálování virtuálního počítače Azure tooa</span><span class="sxs-lookup"><span data-stu-id="88e48-103">Convert an existing Azure virtual machine tooa scale set</span></span>

<span data-ttu-id="88e48-104">Tento kurz ukazuje, jak tooconvert toouse 2.0 rozhraní příkazového řádku Azure virtuální počítač tooa škálovací sady virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="88e48-104">This tutorial shows you how toouse Azure CLI 2.0 tooconvert a virtual machine tooa virtual machine scale set.</span></span> <span data-ttu-id="88e48-105">Také zjistíte, jak nastavit konfiguraci hello tooautomate hello virtuální počítače ve škálovací hello.</span><span class="sxs-lookup"><span data-stu-id="88e48-105">You also learn how tooautomate hello configuration of hello virtual machines in hello scale set.</span></span> <span data-ttu-id="88e48-106">Další informace o tom, jak tooinstall rozhraní příkazového řádku Azure 2.0, najdete v části [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="88e48-106">For more information on how tooinstall Azure CLI 2.0, see [Getting Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span> <span data-ttu-id="88e48-107">Další informace o sadách škálování najdete v tématu [sadách škálování virtuálního počítače](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="88e48-107">For more information about scale sets, see [Virtual Machine Scale Sets](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span></span>

## <a name="step-1---deprovision-hello-vm"></a><span data-ttu-id="88e48-108">Krok 1 - Deprovision hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="88e48-108">Step 1 - Deprovision hello VM</span></span>

<span data-ttu-id="88e48-109">Pomocí SSH tooconnect toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="88e48-109">Use SSH tooconnect toohello VM.</span></span>

<span data-ttu-id="88e48-110">Deprovision hello virtuálních počítačů pomocí souborů toodelete agenta virtuálního počítače Azure hello a data.</span><span class="sxs-lookup"><span data-stu-id="88e48-110">Deprovision hello VM using hello Azure VM agent toodelete files and data.</span></span> <span data-ttu-id="88e48-111">Podrobný přehled o zrušení zřízení, najdete v části [zachytit virtuální počítač s Linuxem](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="88e48-111">For a detailed overview of deprovisioning, see [Capture a Linux virtual machine](capture-image.md).</span></span>

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-hello-vm"></a><span data-ttu-id="88e48-112">Krok 2 – Vytvoření bitové kopie hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="88e48-112">Step 2 - Capture an image of hello VM</span></span>

<span data-ttu-id="88e48-113">Podrobný přehled o zachycení, najdete v části [zachytit virtuální počítač s Linuxem](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="88e48-113">For a detailed overview of capturing, see [Capture a Linux virtual machine](capture-image.md).</span></span>

<span data-ttu-id="88e48-114">Deallocate hello virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate):</span><span class="sxs-lookup"><span data-stu-id="88e48-114">Deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate):</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="88e48-115">Generalize hello virtuálního počítače s [az virtuálních počítačů zobecní](/cli/azure/vm#generalize):</span><span class="sxs-lookup"><span data-stu-id="88e48-115">Generalize hello VM with [az vm generalize](/cli/azure/vm#generalize):</span></span>

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="88e48-116">Vytvoření image z hello prostředků virtuálního počítače s [vytvoření bitové kopie az](/cli/azure/image#create):</span><span class="sxs-lookup"><span data-stu-id="88e48-116">Create an image from hello VM resource with [az image create](/cli/azure/image#create):</span></span>

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-hello-scale-set"></a><span data-ttu-id="88e48-117">Krok 3 – vytvoření sadě škálování hello</span><span class="sxs-lookup"><span data-stu-id="88e48-117">Step 3 - Create hello scale set</span></span>

<span data-ttu-id="88e48-118">Získat hello **id** hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="88e48-118">Get hello **id** of hello image.</span></span>

```azurecli
az image show --resource-group myResourceGroup --name myImage --query id
```

```json
"/subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage"
```

<span data-ttu-id="88e48-119">Vytvořte virtuální počítač z bitové kopie prostředku s [vytvořit az vmss](/cli/azure/vmss#create):</span><span class="sxs-lookup"><span data-stu-id="88e48-119">Create a VM from your image resource with [az vmss create](/cli/azure/vmss#create):</span></span>

```azurecli
az vmss create --resource-group myResourceGroup --name myScaleSet --image /subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage --upgrade-policy-mode automatic --vm-sku Standard_DS1_v2 --data-disk-sizes-gb 10 --admin-username azureuser --generate-ssh-keys
```

<span data-ttu-id="88e48-120">Tento příkaz také připojit datový disk 10gb.</span><span class="sxs-lookup"><span data-stu-id="88e48-120">This command also attached a 10gb data disk.</span></span> <span data-ttu-id="88e48-121">Mějte na paměti, který v závislosti na hello virtuálních počítačů velikost zvolenou (jsme použili **Standard_DS1_v2**), se liší hello počet datových disků povolená.</span><span class="sxs-lookup"><span data-stu-id="88e48-121">Keep in mind that depending on hello VM size chosen (we used **Standard_DS1_v2**), hello number of data disks allowed is different.</span></span> <span data-ttu-id="88e48-122">Další informace najdete v tématu hello [velikostí virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="88e48-122">For more information, review hello [virtual machine sizes](sizes.md).</span></span>

<span data-ttu-id="88e48-123">Po dokončení nastavení škálování hello připojte tooit.</span><span class="sxs-lookup"><span data-stu-id="88e48-123">Once hello scale set finishes, connect tooit.</span></span> <span data-ttu-id="88e48-124">Získat seznam IP adres pro instance hello SSH s [az vmss seznamu--připojení-informace o instanci](/cli/azure/vmss#list-instance-connection-info):</span><span class="sxs-lookup"><span data-stu-id="88e48-124">Get a list of IP addresses for hello instances for SSH with [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info):</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

<span data-ttu-id="88e48-125">Teď se můžete připojit toohello virtuální počítač instance tooinitialize hello datový disk</span><span class="sxs-lookup"><span data-stu-id="88e48-125">Now you can connect toohello virtual machine instance tooinitialize hello data disk</span></span>

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-hello-data-disk"></a><span data-ttu-id="88e48-126">Krok 4 – inicializovat hello datový disk</span><span class="sxs-lookup"><span data-stu-id="88e48-126">Step 4 - Initialize hello data disk</span></span>

<span data-ttu-id="88e48-127">Při připojené toohello virtuálního počítače, oddílu hello disk s `fdisk`:</span><span class="sxs-lookup"><span data-stu-id="88e48-127">While connected toohello virtual machine, partition hello disk with `fdisk`:</span></span>

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

<span data-ttu-id="88e48-128">Zápis systému souborů toohello oddílu s hello `mkfs` příkaz:</span><span class="sxs-lookup"><span data-stu-id="88e48-128">Write a file system toohello partition with hello `mkfs` command:</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="88e48-129">Připojte nový disk hello tak, aby se v hello operačního systému:</span><span class="sxs-lookup"><span data-stu-id="88e48-129">Mount hello new disk so that it is accessible in hello operating system:</span></span>

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="88e48-130">Hello disk může být nyní přistupuje prostřednictvím přípojný bod datadrive hello, který lze ověřit pomocí `ls /datadrive/`.</span><span class="sxs-lookup"><span data-stu-id="88e48-130">hello disk can now be accesses through hello datadrive mountpoint, which can be verified with `ls /datadrive/`.</span></span>

<span data-ttu-id="88e48-131">Ukončení relace SSH hello.</span><span class="sxs-lookup"><span data-stu-id="88e48-131">End hello SSH session.</span></span>


## <a name="step-5---configure-firewall"></a><span data-ttu-id="88e48-132">Krok 5: Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="88e48-132">Step 5 - Configure firewall</span></span>

<span data-ttu-id="88e48-133">Děrování díru prostřednictvím brány firewall webový server toohello hello hostované hello škálovací sada.</span><span class="sxs-lookup"><span data-stu-id="88e48-133">Punch a hole through hello firewall toohello webserver hosted by hello scale set.</span></span> <span data-ttu-id="88e48-134">Pokud byla vytvořena hello škálovací sadu, byla vytvořena i nástroj pro vyrovnávání zatížení a ho používají **SSH** toohello jednotlivé virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="88e48-134">When hello scale set was created, a load balancer was also created, and you used it **SSH** toohello individual virtual machines.</span></span> <span data-ttu-id="88e48-135">tooopen port, budete potřebovat dva kusy informace, které můžete získat pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="88e48-135">tooopen a port, you need two pieces of information, which you can get using Azure CLI.</span></span>

* <span data-ttu-id="88e48-136">**Fond adres IP front-endu**</span><span class="sxs-lookup"><span data-stu-id="88e48-136">**Frontend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* <span data-ttu-id="88e48-137">**Fond back-end IP adres**</span><span class="sxs-lookup"><span data-stu-id="88e48-137">**Backend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

<span data-ttu-id="88e48-138">S těmito dva názvy, můžete otevřít port **80**.</span><span class="sxs-lookup"><span data-stu-id="88e48-138">With those two names, you can open port **80**.</span></span>

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a><span data-ttu-id="88e48-139">Krok 6 – automatické konfiguraci</span><span class="sxs-lookup"><span data-stu-id="88e48-139">Step 6 - Automate configuration</span></span>

<span data-ttu-id="88e48-140">datový disk Hello musí toobe nakonfigurované na každou instanci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="88e48-140">hello data disk needs toobe configured on each virtual machine instance.</span></span> <span data-ttu-id="88e48-141">Jsme můžete automatizovat konfiguraci hello hello virtuálního počítače s hello **CustomScript** rozšíření.</span><span class="sxs-lookup"><span data-stu-id="88e48-141">We can automate hello configuration of hello virtual machine with hello **CustomScript** extension.</span></span>

<span data-ttu-id="88e48-142">Nejprve vytvořte *.sh* skript, který obsahuje příkazy formát disku hello.</span><span class="sxs-lookup"><span data-stu-id="88e48-142">First, create a *.sh* script that includes hello disk format commands.</span></span>

```sh
#!/bin/bash

# Setup hello data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

<span data-ttu-id="88e48-143">V dalším kroku nahrát tento skript soubor toowhere hello **CustomScript** rozšíření k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="88e48-143">Next, upload that script file toowhere hello **CustomScript** extension can access it.</span></span> <span data-ttu-id="88e48-144">Je k dispozici kopii [zde](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span><span class="sxs-lookup"><span data-stu-id="88e48-144">A copy is available [here](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span></span>

<span data-ttu-id="88e48-145">Vytvořte místní soubor s názvem **settings.json** a put hello následující bloku JSON v ní.</span><span class="sxs-lookup"><span data-stu-id="88e48-145">Create a local file named **settings.json** and put hello following JSON block in it.</span></span> <span data-ttu-id="88e48-146">Hello `flieUris` vlastnost musí být nastavená toowhere nahranému v souboru skriptu.</span><span class="sxs-lookup"><span data-stu-id="88e48-146">hello `flieUris` property should be set toowhere your script file was uploaded to.</span></span>

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

<span data-ttu-id="88e48-147">Nasazení tohoto měřítka tooyour příkaz s hello **CustomScript** rozšíření, odkazující na hello **settings.json** souboru jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="88e48-147">Deploy this command tooyour scale set with hello **CustomScript** extension, referencing hello **settings.json** file we just created.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

<span data-ttu-id="88e48-148">Toto rozšíření se automaticky spustí na všechny aktuální instance a všechny instance později vytvořené škálování.</span><span class="sxs-lookup"><span data-stu-id="88e48-148">This extension automatically runs on all current instances, and any instances later created by scaling.</span></span>

## <a name="step-7---configure-autoscale-rules"></a><span data-ttu-id="88e48-149">Krok 7: Konfigurace pravidel automatického škálování</span><span class="sxs-lookup"><span data-stu-id="88e48-149">Step 7 - Configure autoscale rules</span></span>

<span data-ttu-id="88e48-150">Škálování pravidla se v současné době nelze nastavit v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="88e48-150">Currently, autoscale rules cannot be set in Azure CLI.</span></span> <span data-ttu-id="88e48-151">Použití hello [portál Azure](https://portal.azure.com) tooconfigure škálování.</span><span class="sxs-lookup"><span data-stu-id="88e48-151">Use hello [Azure portal](https://portal.azure.com) tooconfigure autoscale.</span></span>

## <a name="step-8---management-tasks"></a><span data-ttu-id="88e48-152">Krok 8 – úlohy správy</span><span class="sxs-lookup"><span data-stu-id="88e48-152">Step 8 - Management tasks</span></span>

<span data-ttu-id="88e48-153">V průběhu cyklu hello škálovací sady hello, může být nutné toorun jedné nebo více úloh správy.</span><span class="sxs-lookup"><span data-stu-id="88e48-153">Throughout hello lifecycle of hello scale set, you may need toorun one or more management tasks.</span></span> <span data-ttu-id="88e48-154">Kromě toho může být vhodné toocreate skripty, které automatizují různé úlohy životního cyklu a hello rozhraní příkazového řádku Azure poskytuje rychlý způsob toodo tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="88e48-154">Additionally, you may want toocreate scripts that automate various lifecycle-tasks, and hello Azure CLI provides a quick way toodo those tasks.</span></span> <span data-ttu-id="88e48-155">Tady jsou několik běžných úloh.</span><span class="sxs-lookup"><span data-stu-id="88e48-155">Here are a few common tasks.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="88e48-156">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="88e48-156">Get connection info</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

### <a name="set-instance-count-manual-scale"></a><span data-ttu-id="88e48-157">Nastavit počet instancí (ruční škálování)</span><span class="sxs-lookup"><span data-stu-id="88e48-157">Set instance count (manual scale)</span></span>

```azurecli
az vmss scale --resource-group myResourceGroup --name myScaleSet --new-capacity 4
```

### <a name="delete-resource-group"></a><span data-ttu-id="88e48-158">Odstraňte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="88e48-158">Delete resource group</span></span>

<span data-ttu-id="88e48-159">Odstranění skupiny prostředků se také odstraní všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="88e48-159">Deleting a resource group also deletes all resources contained within.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="88e48-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="88e48-160">Next steps</span></span>
<span data-ttu-id="88e48-161">toolearn informace o některých škálování virtuálních počítačů hello nastavit funkce zavedená v tomto kurzu, najdete v části hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="88e48-161">toolearn more about some of hello virtual machine scale set features introduced in this tutorial, see hello following information:</span></span>

- [<span data-ttu-id="88e48-162">Přehled sady škálování virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="88e48-162">Azure Virtual Machine Scale Sets overview</span></span>](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [<span data-ttu-id="88e48-163">Azure Load Balancer – přehled</span><span class="sxs-lookup"><span data-stu-id="88e48-163">Azure Load Balancer overview</span></span>](../../load-balancer/load-balancer-overview.md)
- [<span data-ttu-id="88e48-164">Řízení toku provozu sítě s skupin zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="88e48-164">Control network traffic flow with network security groups</span></span>](../../virtual-network/virtual-networks-nsg.md)