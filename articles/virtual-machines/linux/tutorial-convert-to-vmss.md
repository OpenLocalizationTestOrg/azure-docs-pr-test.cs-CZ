---
title: "Převést na sadu škálování virtuálního počítače Azure | Microsoft Docs"
description: "Vytvořit a nasadit škálování virtuálního počítače Linux Azure nastavit pomocí Azure CLI."
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
ms.openlocfilehash: 8d3376d2791b1349298db618d475ce5573083702
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="convert-an-existing-azure-virtual-machine-to-a-scale-set"></a><span data-ttu-id="00d41-103">Převést stávající virtuální počítač Azure na škálovací sadě</span><span class="sxs-lookup"><span data-stu-id="00d41-103">Convert an existing Azure virtual machine to a scale set</span></span>

<span data-ttu-id="00d41-104">V tomto kurzu se dozvíte, jak používat Azure CLI 2.0 převést virtuální počítač na škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="00d41-104">This tutorial shows you how to use Azure CLI 2.0 to convert a virtual machine to a virtual machine scale set.</span></span> <span data-ttu-id="00d41-105">Můžete také informace o automatizaci konfigurace virtuálních počítačů v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="00d41-105">You also learn how to automate the configuration of the virtual machines in the scale set.</span></span> <span data-ttu-id="00d41-106">Další informace o tom, jak nainstalovat Azure CLI 2.0, naleznete v části [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="00d41-106">For more information on how to install Azure CLI 2.0, see [Getting Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span> <span data-ttu-id="00d41-107">Další informace o sadách škálování najdete v tématu [sadách škálování virtuálního počítače](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="00d41-107">For more information about scale sets, see [Virtual Machine Scale Sets](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span></span>

## <a name="step-1---deprovision-the-vm"></a><span data-ttu-id="00d41-108">Krok 1 – zrušení zřízení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="00d41-108">Step 1 - Deprovision the VM</span></span>

<span data-ttu-id="00d41-109">Použití SSH se připojit k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="00d41-109">Use SSH to connect to the VM.</span></span>

<span data-ttu-id="00d41-110">Zrušení zřízení virtuálního počítače pomocí agenta virtuálního počítače Azure se odstranit soubory a data.</span><span class="sxs-lookup"><span data-stu-id="00d41-110">Deprovision the VM using the Azure VM agent to delete files and data.</span></span> <span data-ttu-id="00d41-111">Podrobný přehled o zrušení zřízení, najdete v části [zachytit virtuální počítač s Linuxem](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="00d41-111">For a detailed overview of deprovisioning, see [Capture a Linux virtual machine](capture-image.md).</span></span>

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-the-vm"></a><span data-ttu-id="00d41-112">Krok 2 – Vytvoření bitové kopie virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="00d41-112">Step 2 - Capture an image of the VM</span></span>

<span data-ttu-id="00d41-113">Podrobný přehled o zachycení, najdete v části [zachytit virtuální počítač s Linuxem](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="00d41-113">For a detailed overview of capturing, see [Capture a Linux virtual machine](capture-image.md).</span></span>

<span data-ttu-id="00d41-114">Zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate):</span><span class="sxs-lookup"><span data-stu-id="00d41-114">Deallocate the VM with [az vm deallocate](/cli/azure/vm#deallocate):</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="00d41-115">Generalize virtuálního počítače s [az virtuálních počítačů zobecní](/cli/azure/vm#generalize):</span><span class="sxs-lookup"><span data-stu-id="00d41-115">Generalize the VM with [az vm generalize](/cli/azure/vm#generalize):</span></span>

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="00d41-116">Vytvoření image z prostředků virtuálního počítače s [vytvoření bitové kopie az](/cli/azure/image#create):</span><span class="sxs-lookup"><span data-stu-id="00d41-116">Create an image from the VM resource with [az image create](/cli/azure/image#create):</span></span>

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-the-scale-set"></a><span data-ttu-id="00d41-117">Krok 3 – vytvoření sady škálování</span><span class="sxs-lookup"><span data-stu-id="00d41-117">Step 3 - Create the scale set</span></span>

<span data-ttu-id="00d41-118">Získat **id** bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="00d41-118">Get the **id** of the image.</span></span>

```azurecli
az image show --resource-group myResourceGroup --name myImage --query id
```

```json
"/subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage"
```

<span data-ttu-id="00d41-119">Vytvořte virtuální počítač z bitové kopie prostředku s [vytvořit az vmss](/cli/azure/vmss#create):</span><span class="sxs-lookup"><span data-stu-id="00d41-119">Create a VM from your image resource with [az vmss create](/cli/azure/vmss#create):</span></span>

```azurecli
az vmss create --resource-group myResourceGroup --name myScaleSet --image /subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage --upgrade-policy-mode automatic --vm-sku Standard_DS1_v2 --data-disk-sizes-gb 10 --admin-username azureuser --generate-ssh-keys
```

<span data-ttu-id="00d41-120">Tento příkaz také připojit datový disk 10gb.</span><span class="sxs-lookup"><span data-stu-id="00d41-120">This command also attached a 10gb data disk.</span></span> <span data-ttu-id="00d41-121">Mějte na paměti, který v závislosti na virtuální počítač velikost zvolenou (jsme použili **Standard_DS1_v2**), počet datových disků povolená, se liší.</span><span class="sxs-lookup"><span data-stu-id="00d41-121">Keep in mind that depending on the VM size chosen (we used **Standard_DS1_v2**), the number of data disks allowed is different.</span></span> <span data-ttu-id="00d41-122">Další informace najdete v článku [velikostí virtuálních počítačů](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="00d41-122">For more information, review the [virtual machine sizes](sizes.md).</span></span>

<span data-ttu-id="00d41-123">Po dokončení nastavení měřítka připojte se k němu.</span><span class="sxs-lookup"><span data-stu-id="00d41-123">Once the scale set finishes, connect to it.</span></span> <span data-ttu-id="00d41-124">Získat seznam IP adres pro instance pro SSH s [az vmss seznamu--připojení-informace o instanci](/cli/azure/vmss#list-instance-connection-info):</span><span class="sxs-lookup"><span data-stu-id="00d41-124">Get a list of IP addresses for the instances for SSH with [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info):</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

<span data-ttu-id="00d41-125">Nyní můžete připojit k instanci virtuálního počítače k chybě při inicializaci datový disk</span><span class="sxs-lookup"><span data-stu-id="00d41-125">Now you can connect to the virtual machine instance to initialize the data disk</span></span>

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-the-data-disk"></a><span data-ttu-id="00d41-126">Krok 4 – inicializovat datový disk</span><span class="sxs-lookup"><span data-stu-id="00d41-126">Step 4 - Initialize the data disk</span></span>

<span data-ttu-id="00d41-127">Při připojení k virtuálnímu počítači, oddílu disku spolu s `fdisk`:</span><span class="sxs-lookup"><span data-stu-id="00d41-127">While connected to the virtual machine, partition the disk with `fdisk`:</span></span>

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

<span data-ttu-id="00d41-128">Zápis systém souborů k oddílu s `mkfs` příkaz:</span><span class="sxs-lookup"><span data-stu-id="00d41-128">Write a file system to the partition with the `mkfs` command:</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="00d41-129">Připojte nový disk, takže bude přístupný v operačním systému:</span><span class="sxs-lookup"><span data-stu-id="00d41-129">Mount the new disk so that it is accessible in the operating system:</span></span>

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="00d41-130">Disk může být nyní přistupuje prostřednictvím datadrive přípojný bod, který lze ověřit pomocí `ls /datadrive/`.</span><span class="sxs-lookup"><span data-stu-id="00d41-130">The disk can now be accesses through the datadrive mountpoint, which can be verified with `ls /datadrive/`.</span></span>

<span data-ttu-id="00d41-131">Ukončení relace SSH.</span><span class="sxs-lookup"><span data-stu-id="00d41-131">End the SSH session.</span></span>


## <a name="step-5---configure-firewall"></a><span data-ttu-id="00d41-132">Krok 5: Konfigurace brány firewall</span><span class="sxs-lookup"><span data-stu-id="00d41-132">Step 5 - Configure firewall</span></span>

<span data-ttu-id="00d41-133">Děrování díru přes bránu firewall, aby webový server hostované byly sadou škálování.</span><span class="sxs-lookup"><span data-stu-id="00d41-133">Punch a hole through the firewall to the webserver hosted by the scale set.</span></span> <span data-ttu-id="00d41-134">Pokud byly sadou škálování byl vytvořen, byla vytvořena i nástroj pro vyrovnávání zatížení a ho používají **SSH** pro jednotlivé virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="00d41-134">When the scale set was created, a load balancer was also created, and you used it **SSH** to the individual virtual machines.</span></span> <span data-ttu-id="00d41-135">Chcete-li otevřít port, je třeba dva kusy informace, které můžete získat pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="00d41-135">To open a port, you need two pieces of information, which you can get using Azure CLI.</span></span>

* <span data-ttu-id="00d41-136">**Fond adres IP front-endu**</span><span class="sxs-lookup"><span data-stu-id="00d41-136">**Frontend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* <span data-ttu-id="00d41-137">**Fond back-end IP adres**</span><span class="sxs-lookup"><span data-stu-id="00d41-137">**Backend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

<span data-ttu-id="00d41-138">S těmito dva názvy, můžete otevřít port **80**.</span><span class="sxs-lookup"><span data-stu-id="00d41-138">With those two names, you can open port **80**.</span></span>

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a><span data-ttu-id="00d41-139">Krok 6 – automatické konfiguraci</span><span class="sxs-lookup"><span data-stu-id="00d41-139">Step 6 - Automate configuration</span></span>

<span data-ttu-id="00d41-140">Datový disk musí být nakonfigurované na každou instanci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="00d41-140">The data disk needs to be configured on each virtual machine instance.</span></span> <span data-ttu-id="00d41-141">Jsme můžete automatizovat konfiguraci virtuálního počítače s **CustomScript** rozšíření.</span><span class="sxs-lookup"><span data-stu-id="00d41-141">We can automate the configuration of the virtual machine with the **CustomScript** extension.</span></span>

<span data-ttu-id="00d41-142">Nejprve vytvořte *.sh* skript, který obsahuje příkazy formátu disku.</span><span class="sxs-lookup"><span data-stu-id="00d41-142">First, create a *.sh* script that includes the disk format commands.</span></span>

```sh
#!/bin/bash

# Setup the data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

<span data-ttu-id="00d41-143">V dalším kroku nahrát daného souboru skriptu k umístění, kde **CustomScript** rozšíření k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="00d41-143">Next, upload that script file to where the **CustomScript** extension can access it.</span></span> <span data-ttu-id="00d41-144">Je k dispozici kopii [zde](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span><span class="sxs-lookup"><span data-stu-id="00d41-144">A copy is available [here](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span></span>

<span data-ttu-id="00d41-145">Vytvořte místní soubor s názvem **settings.json** a uveďte následující blok JSON.</span><span class="sxs-lookup"><span data-stu-id="00d41-145">Create a local file named **settings.json** and put the following JSON block in it.</span></span> <span data-ttu-id="00d41-146">`flieUris` Musí být vlastnost nastavena, kde byla nahrát váš soubor skriptu do.</span><span class="sxs-lookup"><span data-stu-id="00d41-146">The `flieUris` property should be set to where your script file was uploaded to.</span></span>

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

<span data-ttu-id="00d41-147">Nasadit tento příkaz vaší škálování s **CustomScript** rozšíření, odkazující **settings.json** souboru jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="00d41-147">Deploy this command to your scale set with the **CustomScript** extension, referencing the **settings.json** file we just created.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

<span data-ttu-id="00d41-148">Toto rozšíření se automaticky spustí na všechny aktuální instance a všechny instance později vytvořené škálování.</span><span class="sxs-lookup"><span data-stu-id="00d41-148">This extension automatically runs on all current instances, and any instances later created by scaling.</span></span>

## <a name="step-7---configure-autoscale-rules"></a><span data-ttu-id="00d41-149">Krok 7: Konfigurace pravidel automatického škálování</span><span class="sxs-lookup"><span data-stu-id="00d41-149">Step 7 - Configure autoscale rules</span></span>

<span data-ttu-id="00d41-150">Škálování pravidla se v současné době nelze nastavit v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="00d41-150">Currently, autoscale rules cannot be set in Azure CLI.</span></span> <span data-ttu-id="00d41-151">Použití [portál Azure](https://portal.azure.com) ke konfiguraci automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="00d41-151">Use the [Azure portal](https://portal.azure.com) to configure autoscale.</span></span>

## <a name="step-8---management-tasks"></a><span data-ttu-id="00d41-152">Krok 8 – úlohy správy</span><span class="sxs-lookup"><span data-stu-id="00d41-152">Step 8 - Management tasks</span></span>

<span data-ttu-id="00d41-153">V průběhu cyklu škálovací sady můžete spustit jeden nebo více úloh správy.</span><span class="sxs-lookup"><span data-stu-id="00d41-153">Throughout the lifecycle of the scale set, you may need to run one or more management tasks.</span></span> <span data-ttu-id="00d41-154">Kromě toho můžete chtít vytvořit skripty, které automatizují různé úlohy životního cyklu a rozhraní příkazového řádku Azure poskytuje rychlý způsob, jak provést tyto úlohy.</span><span class="sxs-lookup"><span data-stu-id="00d41-154">Additionally, you may want to create scripts that automate various lifecycle-tasks, and the Azure CLI provides a quick way to do those tasks.</span></span> <span data-ttu-id="00d41-155">Tady jsou několik běžných úloh.</span><span class="sxs-lookup"><span data-stu-id="00d41-155">Here are a few common tasks.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="00d41-156">Získání informací o připojení</span><span class="sxs-lookup"><span data-stu-id="00d41-156">Get connection info</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

### <a name="set-instance-count-manual-scale"></a><span data-ttu-id="00d41-157">Nastavit počet instancí (ruční škálování)</span><span class="sxs-lookup"><span data-stu-id="00d41-157">Set instance count (manual scale)</span></span>

```azurecli
az vmss scale --resource-group myResourceGroup --name myScaleSet --new-capacity 4
```

### <a name="delete-resource-group"></a><span data-ttu-id="00d41-158">Odstraňte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="00d41-158">Delete resource group</span></span>

<span data-ttu-id="00d41-159">Odstranění skupiny prostředků se také odstraní všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="00d41-159">Deleting a resource group also deletes all resources contained within.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="00d41-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="00d41-160">Next steps</span></span>
<span data-ttu-id="00d41-161">Další informace o některých funkcí sady škálování virtuálního počítače byla zavedená v tomto kurzu, přečtěte si následující informace:</span><span class="sxs-lookup"><span data-stu-id="00d41-161">To learn more about some of the virtual machine scale set features introduced in this tutorial, see the following information:</span></span>

- [<span data-ttu-id="00d41-162">Přehled sady škálování virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="00d41-162">Azure Virtual Machine Scale Sets overview</span></span>](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [<span data-ttu-id="00d41-163">Azure Load Balancer – přehled</span><span class="sxs-lookup"><span data-stu-id="00d41-163">Azure Load Balancer overview</span></span>](../../load-balancer/load-balancer-overview.md)
- [<span data-ttu-id="00d41-164">Řízení toku provozu sítě s skupin zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="00d41-164">Control network traffic flow with network security groups</span></span>](../../virtual-network/virtual-networks-nsg.md)