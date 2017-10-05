---
title: "Zachycení virtuálního počítače Azure Linux chcete použít jako šablonu | Microsoft Docs"
description: "Informace o zaznamenání a generalize bitové kopie založené na systému Linux Azure virtuálního počítače (VM) vytvořené pomocí modelu nasazení Azure Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: b1164fbd816eea5189786850f096438e32f8f802
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="capture-a-linux-virtual-machine-running-on-azure"></a><span data-ttu-id="97a1f-103">Zachytit virtuální počítač Linux spuštěné v Azure</span><span class="sxs-lookup"><span data-stu-id="97a1f-103">Capture a Linux virtual machine running on Azure</span></span>
<span data-ttu-id="97a1f-104">Postupujte podle kroků v tomto článku generalize a zachycení Azure Linux virtuálního počítače (VM) v modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="97a1f-104">Follow the steps in this article to generalize and capture your Azure Linux virtual machine (VM) in the Resource Manager deployment model.</span></span> <span data-ttu-id="97a1f-105">Při průchodu generalize virtuálního počítače, můžete odebrat informace o osobní účet a připravit virtuální počítač, který se má použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="97a1f-105">When you generalize the VM, you remove personal account information and prepare the VM to be used as an image.</span></span> <span data-ttu-id="97a1f-106">Můžete potom zachycení bitové kopie zobecněný virtuální pevný disk (VHD) pro operační systém, virtuální pevné disky pro připojené datových disků, a [šablony Resource Manageru](../../azure-resource-manager/resource-group-overview.md) pro nová nasazení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="97a1f-106">You then capture a generalized virtual hard disk (VHD) image for the OS, VHDs for attached data disks, and a [Resource Manager template](../../azure-resource-manager/resource-group-overview.md) for new VM deployments.</span></span> <span data-ttu-id="97a1f-107">Tento článek popisuje, jak zachytit image virtuálního počítače s 1.0 rozhraní příkazového řádku Azure pro virtuální počítač pomocí nespravované disků.</span><span class="sxs-lookup"><span data-stu-id="97a1f-107">This article details how to capture a VM image with the Azure CLI 1.0 for a VM using unmanaged disks.</span></span> <span data-ttu-id="97a1f-108">Můžete také [zachytit virtuální počítač Azure spravované disky pomocí Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="97a1f-108">You can also [capture a VM using Azure Managed Disks with the Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="97a1f-109">Spravované disky jsou zpracovávány platformy Azure a nevyžadují, aby všechny přípravné nebo umístění pro uložení.</span><span class="sxs-lookup"><span data-stu-id="97a1f-109">Managed disks are handled by the Azure platform and do not require any preparation or location to store them.</span></span> <span data-ttu-id="97a1f-110">Další informace najdete v tématu [Přehled služby Azure Managed Disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="97a1f-110">For more information, see [Azure Managed Disks overview](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="97a1f-111">Pokud chcete vytvořit virtuální počítače pomocí bitové kopie, nastavit síťovým prostředkům pro každý nový virtuální počítač a nasazení ze zaznamenané bitové kopie virtuálního pevného disku pomocí šablony (soubor JavaScript Object Notation nebo formát JSON,).</span><span class="sxs-lookup"><span data-stu-id="97a1f-111">To create VMs using the image, set up network resources for each new VM, and use the template (a JavaScript Object Notation, or JSON, file) to deploy it from the captured VHD images.</span></span> <span data-ttu-id="97a1f-112">Tímto způsobem můžete replikovat virtuální počítač s aktuální konfigurace softwaru, podobně jako pomocí bitové kopie v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="97a1f-112">In this way, you can replicate a VM with its current software configuration, similar to the way you use images in the Azure Marketplace.</span></span>

> [!TIP]
> <span data-ttu-id="97a1f-113">Pokud chcete vytvořit kopii existující virtuální počítač Linux s jejím specializované stavu pro zálohování nebo ladění, přečtěte si téma [vytvoření kopie virtuálního počítače Linux spuštěné v Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="97a1f-113">If you want to create a copy of your existing Linux VM with its specialized state for backup or debugging, see [Create a copy of a Linux virtual machine running on Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="97a1f-114">A pokud chcete nahrát Linux virtuální pevný disk z virtuálního počítače místní, přečtěte si téma [nahrát a vytvoření virtuálního počítače s Linuxem z bitové kopie disku vlastní](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="97a1f-114">And if you want to upload a Linux VHD from an on-premises VM, see [Upload and create a Linux VM from custom disk image](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>  

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="97a1f-115">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="97a1f-115">CLI versions to complete the task</span></span>
<span data-ttu-id="97a1f-116">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="97a1f-116">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="97a1f-117">[Azure CLI 1.0](#before-you-begin) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="97a1f-117">[Azure CLI 1.0](#before-you-begin) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="97a1f-118">[Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="97a1f-118">[Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="97a1f-119">Než začnete</span><span class="sxs-lookup"><span data-stu-id="97a1f-119">Before you begin</span></span>
<span data-ttu-id="97a1f-120">Ujistěte se, že splňujete následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="97a1f-120">Ensure that you meet the following prerequisites:</span></span>

* <span data-ttu-id="97a1f-121">**Virtuální počítač Azure vytvořené v modelu nasazení Resource Manager** – Pokud jste nevytvořili virtuální počítač s Linuxem, můžete použít [portál](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [rozhraní příkazového řádku Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), nebo [šablony Resource Manageru ](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="97a1f-121">**Azure VM created in the Resource Manager deployment model** - If you haven't created a Linux VM, you can use the [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), the [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), or [Resource Manager templates](create-ssh-secured-vm-from-template.md).</span></span> 
  
    <span data-ttu-id="97a1f-122">Podle potřeby nakonfigurujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="97a1f-122">Configure the VM as needed.</span></span> <span data-ttu-id="97a1f-123">Například [přidat datových disků](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), aktualizace a instalovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="97a1f-123">For example, [add data disks](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), apply updates, and install applications.</span></span> 
* <span data-ttu-id="97a1f-124">**Rozhraní příkazového řádku Azure** -nainstalovat [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="97a1f-124">**Azure CLI** - Install the [Azure CLI](../../cli-install-nodejs.md) on a local computer.</span></span>

## <a name="step-1-remove-the-azure-linux-agent"></a><span data-ttu-id="97a1f-125">Krok 1: Odebrání agenta Azure Linux</span><span class="sxs-lookup"><span data-stu-id="97a1f-125">Step 1: Remove the Azure Linux agent</span></span>
<span data-ttu-id="97a1f-126">Nejprve spustit **příkaz waagent** s **deprovision** parametr na virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="97a1f-126">First, run the **waagent** command with the **deprovision** parameter on the Linux VM.</span></span> <span data-ttu-id="97a1f-127">Tento příkaz odstraní všechny soubory a data tak, aby virtuální počítač připraven na generalizací.</span><span class="sxs-lookup"><span data-stu-id="97a1f-127">This command deletes files and data to make the VM ready for generalizing.</span></span> <span data-ttu-id="97a1f-128">Podrobnosti najdete v tématu [Azure Linux Agent uživatelská příručka](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="97a1f-128">For details, see the [Azure Linux Agent user guide](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

1. <span data-ttu-id="97a1f-129">Připojte k virtuálním počítačům s Linuxem pomocí klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="97a1f-129">Connect to your Linux VM using an SSH client.</span></span>
2. <span data-ttu-id="97a1f-130">V okně SSH zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="97a1f-130">In the SSH window, type the following command:</span></span>
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > <span data-ttu-id="97a1f-131">Tento příkaz lze spusťte pouze na virtuální počítač, který máte v úmyslu bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="97a1f-131">Only run this command on a VM that you intend to capture as an image.</span></span> <span data-ttu-id="97a1f-132">Není zaručeno, že bitovou kopii vymaže všechny citlivých informací nebo je vhodný pro opětovnou distribuci.</span><span class="sxs-lookup"><span data-stu-id="97a1f-132">It does not guarantee that the image is cleared of all sensitive information or is suitable for redistribution.</span></span>
 
3. <span data-ttu-id="97a1f-133">Typ **y** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="97a1f-133">Type **y** to continue.</span></span> <span data-ttu-id="97a1f-134">Můžete přidat **-force** parametr předejdete tento krok potvrzení.</span><span class="sxs-lookup"><span data-stu-id="97a1f-134">You can add the **-force** parameter to avoid this confirmation step.</span></span>
4. <span data-ttu-id="97a1f-135">Po dokončení příkazu, zadejte **ukončete**.</span><span class="sxs-lookup"><span data-stu-id="97a1f-135">After the command completes, type **exit**.</span></span> <span data-ttu-id="97a1f-136">Tento krok zavře použije klient SSH.</span><span class="sxs-lookup"><span data-stu-id="97a1f-136">This step closes the SSH client.</span></span>

## <a name="step-2-capture-the-vm"></a><span data-ttu-id="97a1f-137">Krok 2: Zachycení virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="97a1f-137">Step 2: Capture the VM</span></span>
<span data-ttu-id="97a1f-138">Pomocí rozhraní příkazového řádku Azure generalize a zachycení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="97a1f-138">Use the Azure CLI to generalize and capture the VM.</span></span> <span data-ttu-id="97a1f-139">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="97a1f-139">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="97a1f-140">Zahrnout názvy parametrů příklad **myResourceGroup**, **myVnet**, a **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="97a1f-140">Example parameter names include **myResourceGroup**, **myVnet**, and **myVM**.</span></span>

1. <span data-ttu-id="97a1f-141">Z místního počítače, otevřete rozhraní příkazového řádku Azure a [přihlášení k předplatnému Azure](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="97a1f-141">From your local computer, open the Azure CLI and [login to your Azure subscription](../../xplat-cli-connect.md).</span></span> 
2. <span data-ttu-id="97a1f-142">Ujistěte se, že jste v režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="97a1f-142">Make sure you are in Resource Manager mode.</span></span>
   
    ```azurecli
    azure config mode arm
    ```
3. <span data-ttu-id="97a1f-143">Vypněte virtuální počítač, který již zrušit pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="97a1f-143">Shut down the VM that you already deprovisioned by using the following command:</span></span>
   
    ```azurecli
    azure vm deallocate -g myResourceGroup -n myVM
    ```
4. <span data-ttu-id="97a1f-144">Generalize virtuálního počítače pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="97a1f-144">Generalize the VM with the following command:</span></span>
   
    ```azurecli
    azure vm generalize -g myResourceGroup -n myVM
    ```
5. <span data-ttu-id="97a1f-145">Teď spustit **zachycení virtuálního počítače azure** příkaz, který zachycuje virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="97a1f-145">Now run the **azure vm capture** command, which captures the VM.</span></span> <span data-ttu-id="97a1f-146">V následujícím příkladu, bitovou kopii virtuální pevné disky jsou zachyceny s názvy počínaje **MyVHDNamePrefix**a **-t** možnost určuje cestu k šabloně **MyTemplate.json**.</span><span class="sxs-lookup"><span data-stu-id="97a1f-146">In the following example, the image VHDs are captured with names beginning with **MyVHDNamePrefix**, and the **-t** option specifies a path to the template **MyTemplate.json**.</span></span> 
   
    ```azurecli
    azure vm capture -g myResourceGroup -n myVM -p myVHDNamePrefix -t myTemplate.json
    ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="97a1f-147">Soubory VHD image vytvářeny ve výchozím nastavení ve stejném účtu úložiště, který používá původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="97a1f-147">The image VHD files get created by default in the same storage account that the original VM used.</span></span> <span data-ttu-id="97a1f-148">Použití *stejný účet úložiště* k ukládání virtuálních pevných disků pro všechny nové virtuální počítače, můžete vytvořit z image.</span><span class="sxs-lookup"><span data-stu-id="97a1f-148">Use the *same storage account* to store the VHDs for any new VMs you create from the image.</span></span> 

6. <span data-ttu-id="97a1f-149">Najít umístění zaznamenané bitové kopie, otevřete šablonu JSON v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="97a1f-149">To find the location of a captured image, open the JSON template in a text editor.</span></span> <span data-ttu-id="97a1f-150">V **storageProfile**, Najít **uri** z **image** umístěný v **systému** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="97a1f-150">In the **storageProfile**, find the **uri** of the **image** located in the **system** container.</span></span> <span data-ttu-id="97a1f-151">Například je podobná identifikátor URI bitové kopie disku operačního systému`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span><span class="sxs-lookup"><span data-stu-id="97a1f-151">For example, the URI of the OS disk image is similar to `https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span></span>

## <a name="step-3-create-a-vm-from-the-captured-image"></a><span data-ttu-id="97a1f-152">Krok 3: Vytvoření virtuálního počítače ze zaznamenané bitové kopie</span><span class="sxs-lookup"><span data-stu-id="97a1f-152">Step 3: Create a VM from the captured image</span></span>
<span data-ttu-id="97a1f-153">Chcete-li vytvořit virtuální počítač s Linuxem teď použijte bitovou kopii pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="97a1f-153">Now use the image with a template to create a Linux VM.</span></span> <span data-ttu-id="97a1f-154">Tyto kroky ukazují, jak používat rozhraní příkazového řádku Azure a šablona souboru JSON, kterou zachycené vytvořte virtuální počítač v nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="97a1f-154">These steps show you how to use the Azure CLI and the JSON file template you captured to create the VM in a new virtual network.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="97a1f-155">Vytvoření síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="97a1f-155">Create network resources</span></span>
<span data-ttu-id="97a1f-156">Abyste mohli použít šablonu, musíte nejprve nastavit virtuální sítě a síťovou kartu pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="97a1f-156">To use the template, you first need to set up a virtual network and NIC for your new VM.</span></span> <span data-ttu-id="97a1f-157">Doporučujeme že vytvořit skupinu prostředků pro tyto prostředky v umístění, kde jsou uložené vaše image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="97a1f-157">We recommend you create a resource group for these resources in the location where your VM image is stored.</span></span> <span data-ttu-id="97a1f-158">Příkazy spouštějte podobná následující, nahraďte názvy pro vaše prostředky a příslušné umístění Azure ("centralus" v těchto příkazech):</span><span class="sxs-lookup"><span data-stu-id="97a1f-158">Run commands similar to the following, substituting names for your resources and an appropriate Azure location ("centralus" in these commands):</span></span>

```azurecli
azure group create myResourceGroup1 -l "centralus"

azure network vnet create myResourceGroup1 myVnet -l "centralus"

azure network vnet subnet create myResourceGroup1 myVnet mySubnet

azure network public-ip create myResourceGroup1 myPublicIP -l "centralus"

azure network nic create myResourceGroup1 myNIC -k mySubnet -m myVnet -p myPublicIP -l "centralus"
```

### <a name="get-the-id-of-the-nic"></a><span data-ttu-id="97a1f-159">Získání Id síťového adaptéru</span><span class="sxs-lookup"><span data-stu-id="97a1f-159">Get the Id of the NIC</span></span>
<span data-ttu-id="97a1f-160">Pokud chcete nasadit virtuální počítač z bitové kopie pomocí JSON, který jste uložili během zachycení, je třeba Id na síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="97a1f-160">To deploy a VM from the image by using the JSON you saved during capture, you need the Id of the NIC.</span></span> <span data-ttu-id="97a1f-161">Ho můžete získejte spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="97a1f-161">Obtain it by running the following command:</span></span>

```azurecli
azure network nic show myResourceGroup1 myNIC
```

<span data-ttu-id="97a1f-162">**Id** ve výstupu je podobná`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`</span><span class="sxs-lookup"><span data-stu-id="97a1f-162">The **Id** in the output is similar to `/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`</span></span>

### <a name="create-a-vm"></a><span data-ttu-id="97a1f-163">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="97a1f-163">Create a VM</span></span>
<span data-ttu-id="97a1f-164">Nyní spusťte následující příkaz pro vytvoření virtuálního počítače ze zaznamenané bitové kopie virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="97a1f-164">Now run the following command to create your VM from the captured VM image.</span></span> <span data-ttu-id="97a1f-165">Použití **-f** parametru určete cestu k souboru šablony JSON jste uložili.</span><span class="sxs-lookup"><span data-stu-id="97a1f-165">Use the **-f** parameter to specify the path to the template JSON file you saved.</span></span>

```azurecli
azure group deployment create myResourceGroup1 MyDeployment -f MyTemplate.json
```

<span data-ttu-id="97a1f-166">Ve výstupu příkazu zobrazí se výzva k zadejte nový název virtuálního počítače, uživatelské jméno správce a heslo a Id síťového adaptéru, který jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="97a1f-166">In the command output, you are prompted to supply a new VM name, the admin user name and password, and the Id of the NIC you created previously.</span></span>

```bash
info:    Executing command group deployment create
info:    Supply values for the following parameters
vmName: myNewVM
adminUserName: myAdminuser
adminPassword: ********
networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/myResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
```

<span data-ttu-id="97a1f-167">Následující příklad ukazuje, co vidíte pro úspěšné nasazení:</span><span class="sxs-lookup"><span data-stu-id="97a1f-167">The following sample shows what you see for a successful deployment:</span></span>

```bash
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment xxxxxxx
+ Waiting for deployment to complete
data:    DeploymentName     : MyDeployment
data:    ResourceGroupName  : MyResourceGroup1
data:    ProvisioningState  : Succeeded
data:    Timestamp          : xxxxxxx
data:    Mode               : Incremental
data:    Name                Type          Value

data:    ------------------  ------------  -------------------------------------

data:    vmName              String        myNewVM

data:    vmSize              String        Standard_D1

data:    adminUserName       String        myAdminuser

data:    adminPassword       SecureString  undefined

data:    networkInterfaceId  String        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
info:    group deployment create command OK
```

### <a name="verify-the-deployment"></a><span data-ttu-id="97a1f-168">Ověření nasazení</span><span class="sxs-lookup"><span data-stu-id="97a1f-168">Verify the deployment</span></span>
<span data-ttu-id="97a1f-169">Nyní SSH k virtuálnímu počítači, který jste vytvořili pro ověření nasazení a začít používat nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="97a1f-169">Now SSH to the virtual machine you created to verify the deployment and start using the new VM.</span></span> <span data-ttu-id="97a1f-170">Pro připojení pomocí protokolu SSH, najít IP adresu virtuálního počítače, které jste vytvořili pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="97a1f-170">To connect via SSH, find the IP address of the VM you created by running the following command:</span></span>

```azurecli
azure network public-ip show myResourceGroup1 myPublicIP
```

<span data-ttu-id="97a1f-171">Veřejná IP adresa je uvedena ve výstupu příkazu.</span><span class="sxs-lookup"><span data-stu-id="97a1f-171">The public IP address is listed in the command output.</span></span> <span data-ttu-id="97a1f-172">Ve výchozím nastavení připojení k virtuálního počítače s Linuxem pomocí SSH na port 22.</span><span class="sxs-lookup"><span data-stu-id="97a1f-172">By default you connect to the Linux VM by SSH on port 22.</span></span>

## <a name="create-additional-vms"></a><span data-ttu-id="97a1f-173">Vytvoření dalších virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="97a1f-173">Create additional VMs</span></span>
<span data-ttu-id="97a1f-174">Pomocí zaznamenané bitové kopie a šablony můžete nasadit další virtuální počítače pomocí kroků v předchozím oddílu.</span><span class="sxs-lookup"><span data-stu-id="97a1f-174">Use the captured image and template to deploy additional VMs using the steps in the preceding section.</span></span> <span data-ttu-id="97a1f-175">Další možnosti, jak vytvořit virtuální počítače z image zahrnout pomocí šabloně pro rychlý start nebo spuštěné **vytvoření virtuálního počítače azure** příkaz.</span><span class="sxs-lookup"><span data-stu-id="97a1f-175">Other options to create VMs from the image include using a quickstart template or running the **azure vm create** command.</span></span>

### <a name="use-the-captured-template"></a><span data-ttu-id="97a1f-176">Použití zaznamenané šablony</span><span class="sxs-lookup"><span data-stu-id="97a1f-176">Use the captured template</span></span>
<span data-ttu-id="97a1f-177">Zaznamenané bitové kopie a šablony, postupujte podle těchto kroků (popsané v předchozí části):</span><span class="sxs-lookup"><span data-stu-id="97a1f-177">To use the captured image and template, follow these steps (detailed in the preceding section):</span></span>

* <span data-ttu-id="97a1f-178">Zajistěte, aby vaše image virtuálního počítače ve stejném účtu úložiště, který je hostitelem virtuálního pevného disku Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="97a1f-178">Ensure that your VM image is in the same storage account that hosts your VM's VHD.</span></span>
* <span data-ttu-id="97a1f-179">Zkopírujte soubor šablony JSON a zadejte jedinečný název pro disk operačního systému nový virtuální počítač virtuálního pevného disku (nebo virtuální pevné disky).</span><span class="sxs-lookup"><span data-stu-id="97a1f-179">Copy the template JSON file and specify a unique name for the OS disk of the new VM's VHD (or VHDs).</span></span> <span data-ttu-id="97a1f-180">Například v **storageProfile**v části **virtuálního pevného disku**v **uri**, zadejte jedinečný název pro **osDisk** virtuálního pevného disku, podobně jako`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span><span class="sxs-lookup"><span data-stu-id="97a1f-180">For example, in the **storageProfile**, under **vhd**, in **uri**, specify a unique name for the **osDisk** VHD, similar to `https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span></span>
* <span data-ttu-id="97a1f-181">Vytvořte síťový adaptér ve stejné nebo jiné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="97a1f-181">Create a NIC in either the same or a different virtual network.</span></span>
* <span data-ttu-id="97a1f-182">Ve skupině prostředků, ve kterém můžete nastavit virtuální sítě pomocí souboru JSON změny šablony, vytvořte nasazení.</span><span class="sxs-lookup"><span data-stu-id="97a1f-182">Using the modified template JSON file, create a deployment in the resource group in which you set up the virtual network.</span></span>

### <a name="use-a-quickstart-template"></a><span data-ttu-id="97a1f-183">Použít šabloně pro rychlý start</span><span class="sxs-lookup"><span data-stu-id="97a1f-183">Use a quickstart template</span></span>
<span data-ttu-id="97a1f-184">Pokud chcete nastavit automaticky při vytváření virtuálního počítače z image sítě, můžete tyto prostředky v šabloně.</span><span class="sxs-lookup"><span data-stu-id="97a1f-184">If you want the network set up automatically when you create a VM from the image, you can specify those resources in a template.</span></span> <span data-ttu-id="97a1f-185">Například najdete v článku [101-vm z – uživatele – image šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) z Githubu.</span><span class="sxs-lookup"><span data-stu-id="97a1f-185">For example, see the [101-vm-from-user-image template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) from GitHub.</span></span> <span data-ttu-id="97a1f-186">Tato šablona vytvoří virtuální počítač pomocí vlastní image a potřebná virtuální sítě, veřejnou IP adresu a Síťových prostředků.</span><span class="sxs-lookup"><span data-stu-id="97a1f-186">This template creates a VM from your custom image and the necessary virtual network, public IP address, and NIC resources.</span></span> <span data-ttu-id="97a1f-187">Návod, pomocí šablony na portálu Azure, najdete v části [postup vytvoření virtuálního počítače z vlastní image pomocí šablony Resource Manageru](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).</span><span class="sxs-lookup"><span data-stu-id="97a1f-187">For a walkthrough of using the template in the Azure portal, see [How to create a virtual machine from a custom image using a Resource Manager template](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).</span></span>

### <a name="use-the-azure-vm-create-command"></a><span data-ttu-id="97a1f-188">Příkaz vytváření použít virtuální počítač azure</span><span class="sxs-lookup"><span data-stu-id="97a1f-188">Use the azure vm create command</span></span>
<span data-ttu-id="97a1f-189">Obvykle je to nejjednodušší provádět vytvořit virtuální počítač z bitové kopie pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="97a1f-189">Usually it's easiest to use a Resource Manager template to create a VM from the image.</span></span> <span data-ttu-id="97a1f-190">Však můžete vytvořit virtuální počítač *imperativní* pomocí **vytvoření virtuálního počítače azure** s **-Q** (**– image urn**) parametr.</span><span class="sxs-lookup"><span data-stu-id="97a1f-190">However, you can create the VM *imperatively* by using the **azure vm create** command with the **-Q** (**--image-urn**) parameter.</span></span> <span data-ttu-id="97a1f-191">Pokud použijete tuto metodu, můžete předat také **-d** (**vhd disk operačního systému –**) parametr k určení umístění souboru VHD operačního systému pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="97a1f-191">If you use this method, you also pass the **-d** (**--os-disk-vhd**) parameter to specify the location of the OS .vhd file for the new VM.</span></span> <span data-ttu-id="97a1f-192">Tento soubor musí být v kontejneru virtuální pevné disky účtu úložiště, kde je uložen soubor bitové kopie virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="97a1f-192">This file must be in the vhds container of the storage account where the image VHD file is stored.</span></span> <span data-ttu-id="97a1f-193">Příkaz zkopíruje virtuálního pevného disku pro nový virtuální počítač automaticky **virtuální pevné disky** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="97a1f-193">The command copies the VHD for the new VM automatically to the **vhds** container.</span></span>

<span data-ttu-id="97a1f-194">Dřív, než spustíte **vytvoření virtuálního počítače azure** Image, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="97a1f-194">Before running **azure vm create** with the image, complete the following steps:</span></span>

1. <span data-ttu-id="97a1f-195">Vytvořte skupinu prostředků nebo identifikovat existující skupinu prostředků pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="97a1f-195">Create a resource group, or identify an existing resource group for the deployment.</span></span>
2. <span data-ttu-id="97a1f-196">Vytvořte prostředek veřejné IP adresy a Síťových prostředků pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="97a1f-196">Create a public IP address resource and a NIC resource for the new VM.</span></span> <span data-ttu-id="97a1f-197">Pokyny k vytvoření virtuální sítě, veřejnou IP adresu a síťový adaptér pomocí rozhraní příkazového řádku najdete v části výše v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="97a1f-197">For steps to create a virtual network, public IP address, and NIC by using the CLI, see earlier in this article.</span></span> <span data-ttu-id="97a1f-198">(**vytvoření virtuálního počítače azure** můžete také vytvořit síťovou kartu, ale je třeba předat další parametry pro virtuální síť a podsíť.)</span><span class="sxs-lookup"><span data-stu-id="97a1f-198">(**azure vm create** can also create a NIC, but you need to pass additional parameters for a virtual network and subnet.)</span></span>

<span data-ttu-id="97a1f-199">Spusťte příkaz, který předává identifikátory URI do nového souboru virtuálního pevného disku operačního systému a existující bitová kopie.</span><span class="sxs-lookup"><span data-stu-id="97a1f-199">Then run a command that passes URIs to both the new OS VHD file and the existing image.</span></span> <span data-ttu-id="97a1f-200">V tomto příkladu je vytvořen s velikostí virtuálních počítačů Standard_A1 v oblasti Východ USA.</span><span class="sxs-lookup"><span data-stu-id="97a1f-200">In this example, a size Standard_A1 VM is created in the East US region.</span></span>

```azurecli
azure vm create -g myResourceGroup1 -n myNewVM -l eastus -y Linux \
-z Standard_A1 -u myAdminname -p myPassword -f myNIC \
-d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" \
-Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.vhd"
```

<span data-ttu-id="97a1f-201">Další příkaz Možnosti spustit `azure help vm create`.</span><span class="sxs-lookup"><span data-stu-id="97a1f-201">For additional command options, run `azure help vm create`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97a1f-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="97a1f-202">Next steps</span></span>
<span data-ttu-id="97a1f-203">Pokud chcete spravovat virtuální počítače pomocí rozhraní příkazového řádku, najdete v části úlohy v [nasadit a spravovat virtuální počítače pomocí šablony Azure Resource Manager a rozhraní příkazového řádku Azure](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="97a1f-203">To manage your VMs with the CLI, see the tasks in [Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI](create-ssh-secured-vm-from-template.md).</span></span>

