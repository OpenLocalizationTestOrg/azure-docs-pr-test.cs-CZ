---
title: "aaaCapture virtuální počítač Azure s Linuxem toouse jako šablona | Microsoft Docs"
description: "Zjistěte, jak toocapture a generalize bitové kopie založené na systému Linux Azure virtuálního počítače (VM) vytvořené pomocí modelu nasazení Azure Resource Manager hello."
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
ms.openlocfilehash: 877eee5c842bebe80e755c2240cdaaef4ade6ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="capture-a-linux-virtual-machine-running-on-azure"></a><span data-ttu-id="a2047-103">Zachytit virtuální počítač Linux spuštěné v Azure</span><span class="sxs-lookup"><span data-stu-id="a2047-103">Capture a Linux virtual machine running on Azure</span></span>
<span data-ttu-id="a2047-104">Postupujte podle kroků hello v toogeneralize Tento článek a zachycení Azure Linux virtuálního počítače (VM) v modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="a2047-104">Follow hello steps in this article toogeneralize and capture your Azure Linux virtual machine (VM) in hello Resource Manager deployment model.</span></span> <span data-ttu-id="a2047-105">Při průchodu generalize hello virtuálních počítačů, můžete odebrat informace o osobní účet a příprava toobe hello virtuálních počítačů použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="a2047-105">When you generalize hello VM, you remove personal account information and prepare hello VM toobe used as an image.</span></span> <span data-ttu-id="a2047-106">Můžete potom zachycení bitové kopie zobecněný virtuální pevný disk (VHD) pro hello operačního systému, virtuální pevné disky pro připojené datových disků, a [šablony Resource Manageru](../../azure-resource-manager/resource-group-overview.md) pro nová nasazení virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a2047-106">You then capture a generalized virtual hard disk (VHD) image for hello OS, VHDs for attached data disks, and a [Resource Manager template](../../azure-resource-manager/resource-group-overview.md) for new VM deployments.</span></span> <span data-ttu-id="a2047-107">Tento článek popisuje, jak toocapture virtuálního počítače bitové kopie s hello 1.0 rozhraní příkazového řádku Azure pro virtuální počítač pomocí nespravované disků.</span><span class="sxs-lookup"><span data-stu-id="a2047-107">This article details how toocapture a VM image with hello Azure CLI 1.0 for a VM using unmanaged disks.</span></span> <span data-ttu-id="a2047-108">Můžete také [zachycení virtuálního počítače pomocí Azure spravované disky s hello Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a2047-108">You can also [capture a VM using Azure Managed Disks with hello Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="a2047-109">Spravované disky jsou zpracovávány hello platformy Azure a nevyžadují, aby všechny přípravné nebo umístění toostore je.</span><span class="sxs-lookup"><span data-stu-id="a2047-109">Managed disks are handled by hello Azure platform and do not require any preparation or location toostore them.</span></span> <span data-ttu-id="a2047-110">Další informace najdete v tématu [Přehled služby Azure Managed Disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a2047-110">For more information, see [Azure Managed Disks overview](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="a2047-111">toocreate virtuální počítače pomocí bitové kopie hello, nastavit síťovým prostředkům pro každý nový virtuální počítač a použijte hello šablony (soubor JavaScript Object Notation nebo formát JSON,) toodeploy z hello zachycení bitové kopie virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="a2047-111">toocreate VMs using hello image, set up network resources for each new VM, and use hello template (a JavaScript Object Notation, or JSON, file) toodeploy it from hello captured VHD images.</span></span> <span data-ttu-id="a2047-112">Tímto způsobem můžete replikovat virtuální počítač s své aktuální konfiguraci softwaru, podobně jako toohello způsob, jak používat Image v hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="a2047-112">In this way, you can replicate a VM with its current software configuration, similar toohello way you use images in hello Azure Marketplace.</span></span>

> [!TIP]
> <span data-ttu-id="a2047-113">Pokud chcete toocreate kopii existující virtuální počítač Linux s jejím specializované stavu pro zálohování nebo ladění, najdete v části [vytvoření kopie virtuálního počítače Linux spuštěné v Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a2047-113">If you want toocreate a copy of your existing Linux VM with its specialized state for backup or debugging, see [Create a copy of a Linux virtual machine running on Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="a2047-114">A pokud chcete tooupload Linux virtuální pevný disk z virtuálního počítače místní, najdete v části [odesílání a vytvoření virtuálního počítače s Linuxem z bitové kopie disku vlastní](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a2047-114">And if you want tooupload a Linux VHD from an on-premises VM, see [Upload and create a Linux VM from custom disk image](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>  

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="a2047-115">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="a2047-115">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="a2047-116">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="a2047-116">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="a2047-117">[Azure CLI 1.0](#before-you-begin) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="a2047-117">[Azure CLI 1.0](#before-you-begin) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="a2047-118">[Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="a2047-118">[Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a2047-119">Než začnete</span><span class="sxs-lookup"><span data-stu-id="a2047-119">Before you begin</span></span>
<span data-ttu-id="a2047-120">Ujistěte se, že splňujete hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="a2047-120">Ensure that you meet hello following prerequisites:</span></span>

* <span data-ttu-id="a2047-121">**Virtuální počítač Azure vytvořené v modelu nasazení Resource Manager hello** – Pokud jste nevytvořili virtuální počítač s Linuxem, můžete použít hello [portál](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hello [rozhraní příkazového řádku Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), nebo [Resource Manager šablony](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="a2047-121">**Azure VM created in hello Resource Manager deployment model** - If you haven't created a Linux VM, you can use hello [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hello [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), or [Resource Manager templates](create-ssh-secured-vm-from-template.md).</span></span> 
  
    <span data-ttu-id="a2047-122">Nakonfigurujte hello virtuální počítač podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="a2047-122">Configure hello VM as needed.</span></span> <span data-ttu-id="a2047-123">Například [přidat datových disků](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), aktualizace a instalovat aplikace.</span><span class="sxs-lookup"><span data-stu-id="a2047-123">For example, [add data disks](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), apply updates, and install applications.</span></span> 
* <span data-ttu-id="a2047-124">**Rozhraní příkazového řádku Azure** -instalace hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="a2047-124">**Azure CLI** - Install hello [Azure CLI](../../cli-install-nodejs.md) on a local computer.</span></span>

## <a name="step-1-remove-hello-azure-linux-agent"></a><span data-ttu-id="a2047-125">Krok 1: Odebrání hello Azure Linux agent</span><span class="sxs-lookup"><span data-stu-id="a2047-125">Step 1: Remove hello Azure Linux agent</span></span>
<span data-ttu-id="a2047-126">Nejprve spustit hello **příkaz waagent** s hello **deprovision** parametr na hello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="a2047-126">First, run hello **waagent** command with hello **deprovision** parameter on hello Linux VM.</span></span> <span data-ttu-id="a2047-127">Tento příkaz odstraní všechny soubory a data toomake hello připravené pro generalizací virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a2047-127">This command deletes files and data toomake hello VM ready for generalizing.</span></span> <span data-ttu-id="a2047-128">Podrobnosti najdete v tématu hello [Azure Linux Agent uživatelská příručka](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a2047-128">For details, see hello [Azure Linux Agent user guide](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

1. <span data-ttu-id="a2047-129">Připojte tooyour virtuálního počítače s Linuxem pomocí klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="a2047-129">Connect tooyour Linux VM using an SSH client.</span></span>
2. <span data-ttu-id="a2047-130">V okně hello SSH zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="a2047-130">In hello SSH window, type hello following command:</span></span>
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > <span data-ttu-id="a2047-131">Spusťte pouze na virtuálním počítači tento příkaz, že máte v úmyslu toocapture jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="a2047-131">Only run this command on a VM that you intend toocapture as an image.</span></span> <span data-ttu-id="a2047-132">Nezaručuje se této bitové kopie hello vymaže všechny citlivých informací nebo je vhodný pro opětovnou distribuci.</span><span class="sxs-lookup"><span data-stu-id="a2047-132">It does not guarantee that hello image is cleared of all sensitive information or is suitable for redistribution.</span></span>
 
3. <span data-ttu-id="a2047-133">Typ **y** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="a2047-133">Type **y** toocontinue.</span></span> <span data-ttu-id="a2047-134">Můžete přidat hello **-force** parametr tooavoid tento krok potvrzení.</span><span class="sxs-lookup"><span data-stu-id="a2047-134">You can add hello **-force** parameter tooavoid this confirmation step.</span></span>
4. <span data-ttu-id="a2047-135">Po dokončení příkazu hello zadejte **ukončete**.</span><span class="sxs-lookup"><span data-stu-id="a2047-135">After hello command completes, type **exit**.</span></span> <span data-ttu-id="a2047-136">Tento krok zavře klient SSH hello.</span><span class="sxs-lookup"><span data-stu-id="a2047-136">This step closes hello SSH client.</span></span>

## <a name="step-2-capture-hello-vm"></a><span data-ttu-id="a2047-137">Krok 2: Zachycení hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a2047-137">Step 2: Capture hello VM</span></span>
<span data-ttu-id="a2047-138">Pomocí rozhraní příkazového řádku Azure toogeneralize hello a zachycení hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a2047-138">Use hello Azure CLI toogeneralize and capture hello VM.</span></span> <span data-ttu-id="a2047-139">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="a2047-139">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="a2047-140">Zahrnout názvy parametrů příklad **myResourceGroup**, **myVnet**, a **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="a2047-140">Example parameter names include **myResourceGroup**, **myVnet**, and **myVM**.</span></span>

1. <span data-ttu-id="a2047-141">Z místního počítače, otevřete hello rozhraní příkazového řádku Azure a [přihlášení tooyour předplatné](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="a2047-141">From your local computer, open hello Azure CLI and [login tooyour Azure subscription](../../xplat-cli-connect.md).</span></span> 
2. <span data-ttu-id="a2047-142">Ujistěte se, že jste v režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a2047-142">Make sure you are in Resource Manager mode.</span></span>
   
    ```azurecli
    azure config mode arm
    ```
3. <span data-ttu-id="a2047-143">Vypněte hello virtuální počítač, který již zrušit pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a2047-143">Shut down hello VM that you already deprovisioned by using hello following command:</span></span>
   
    ```azurecli
    azure vm deallocate -g myResourceGroup -n myVM
    ```
4. <span data-ttu-id="a2047-144">Generalize hello virtuálních počítačů s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a2047-144">Generalize hello VM with hello following command:</span></span>
   
    ```azurecli
    azure vm generalize -g myResourceGroup -n myVM
    ```
5. <span data-ttu-id="a2047-145">Teď spustit hello **zachycení virtuálního počítače azure** příkaz, který zachytává hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a2047-145">Now run hello **azure vm capture** command, which captures hello VM.</span></span> <span data-ttu-id="a2047-146">V následujícím příkladu hello, bitové kopie hello virtuální pevné disky jsou zachyceny s názvy počínaje **MyVHDNamePrefix**a hello **-t** možnost určuje šablonu cesty toohello **MyTemplate.json**.</span><span class="sxs-lookup"><span data-stu-id="a2047-146">In hello following example, hello image VHDs are captured with names beginning with **MyVHDNamePrefix**, and hello **-t** option specifies a path toohello template **MyTemplate.json**.</span></span> 
   
    ```azurecli
    azure vm capture -g myResourceGroup -n myVM -p myVHDNamePrefix -t myTemplate.json
    ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="a2047-147">soubory virtuálního pevného disku image Hello vytvářeny ve výchozím nastavení v hello používá stejný účet úložiště, který hello původní virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a2047-147">hello image VHD files get created by default in hello same storage account that hello original VM used.</span></span> <span data-ttu-id="a2047-148">Použití hello *stejný účet úložiště* toostore hello virtuální pevné disky pro všechny nové virtuální počítače, můžete vytvořit z hello image.</span><span class="sxs-lookup"><span data-stu-id="a2047-148">Use hello *same storage account* toostore hello VHDs for any new VMs you create from hello image.</span></span> 

6. <span data-ttu-id="a2047-149">umístění hello toofind zaznamenané bitové kopie, otevřete hello šablony JSON v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="a2047-149">toofind hello location of a captured image, open hello JSON template in a text editor.</span></span> <span data-ttu-id="a2047-150">V hello **storageProfile**, najde hello **uri** z hello **image** umístěný v hello **systému** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a2047-150">In hello **storageProfile**, find hello **uri** of hello **image** located in hello **system** container.</span></span> <span data-ttu-id="a2047-151">Například hello URI image disku hello operačního systému je podobný příliš`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span><span class="sxs-lookup"><span data-stu-id="a2047-151">For example, hello URI of hello OS disk image is similar too`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span></span>

## <a name="step-3-create-a-vm-from-hello-captured-image"></a><span data-ttu-id="a2047-152">Krok 3: Vytvoření virtuálního počítače z hello zachycení image</span><span class="sxs-lookup"><span data-stu-id="a2047-152">Step 3: Create a VM from hello captured image</span></span>
<span data-ttu-id="a2047-153">Teď použijte bitovou kopii hello s toocreate šablony virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="a2047-153">Now use hello image with a template toocreate a Linux VM.</span></span> <span data-ttu-id="a2047-154">Tyto kroky ukazují, jak toouse hello rozhraní příkazového řádku Azure a hello šablona souboru JSON zachycené toocreate hello virtuálních počítačů v nové virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a2047-154">These steps show you how toouse hello Azure CLI and hello JSON file template you captured toocreate hello VM in a new virtual network.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="a2047-155">Vytvoření síťové prostředky</span><span class="sxs-lookup"><span data-stu-id="a2047-155">Create network resources</span></span>
<span data-ttu-id="a2047-156">toouse hello šablony, je nutné nejprve tooset virtuální sítě a síťovou kartu pro nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="a2047-156">toouse hello template, you first need tooset up a virtual network and NIC for your new VM.</span></span> <span data-ttu-id="a2047-157">Doporučujeme že vytvořit skupinu prostředků pro tyto prostředky v hello umístění, kde jsou uložené vaše image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2047-157">We recommend you create a resource group for these resources in hello location where your VM image is stored.</span></span> <span data-ttu-id="a2047-158">Spusťte příkazy podobné toohello následující, nahraďte názvy pro vaše prostředky a příslušné umístění Azure ("centralus" v těchto příkazech):</span><span class="sxs-lookup"><span data-stu-id="a2047-158">Run commands similar toohello following, substituting names for your resources and an appropriate Azure location ("centralus" in these commands):</span></span>

```azurecli
azure group create myResourceGroup1 -l "centralus"

azure network vnet create myResourceGroup1 myVnet -l "centralus"

azure network vnet subnet create myResourceGroup1 myVnet mySubnet

azure network public-ip create myResourceGroup1 myPublicIP -l "centralus"

azure network nic create myResourceGroup1 myNIC -k mySubnet -m myVnet -p myPublicIP -l "centralus"
```

### <a name="get-hello-id-of-hello-nic"></a><span data-ttu-id="a2047-159">Získat hello Id hello síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="a2047-159">Get hello Id of hello NIC</span></span>
<span data-ttu-id="a2047-160">virtuální počítač z bitové kopie hello pomocí hello JSON, které jste uložili během zachycení toodeploy, budete potřebovat hello Id hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="a2047-160">toodeploy a VM from hello image by using hello JSON you saved during capture, you need hello Id of hello NIC.</span></span> <span data-ttu-id="a2047-161">Ho můžete získejte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a2047-161">Obtain it by running hello following command:</span></span>

```azurecli
azure network nic show myResourceGroup1 myNIC
```

<span data-ttu-id="a2047-162">Hello **Id** v hello výstup se podobá příliš`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`</span><span class="sxs-lookup"><span data-stu-id="a2047-162">hello **Id** in hello output is similar too`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`</span></span>

### <a name="create-a-vm"></a><span data-ttu-id="a2047-163">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a2047-163">Create a VM</span></span>
<span data-ttu-id="a2047-164">Teď hello spusťte následující příkaz toocreate virtuálního počítače z hello zachycenou image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2047-164">Now run hello following command toocreate your VM from hello captured VM image.</span></span> <span data-ttu-id="a2047-165">Použití hello **-f** parametr toospecify hello cesta JSON toohello šablony soubor uložit.</span><span class="sxs-lookup"><span data-stu-id="a2047-165">Use hello **-f** parameter toospecify hello path toohello template JSON file you saved.</span></span>

```azurecli
azure group deployment create myResourceGroup1 MyDeployment -f MyTemplate.json
```

<span data-ttu-id="a2047-166">Ve výstupu příkazu hello jsou výzvami toosupply nový název virtuálního počítače, hello správce uživatelské jméno a heslo a hello Id hello seskupování, které jste předtím vytvořili.</span><span class="sxs-lookup"><span data-stu-id="a2047-166">In hello command output, you are prompted toosupply a new VM name, hello admin user name and password, and hello Id of hello NIC you created previously.</span></span>

```bash
info:    Executing command group deployment create
info:    Supply values for hello following parameters
vmName: myNewVM
adminUserName: myAdminuser
adminPassword: ********
networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/myResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
```

<span data-ttu-id="a2047-167">Hello následující ukázka uvádí, co vidíte pro úspěšné nasazení:</span><span class="sxs-lookup"><span data-stu-id="a2047-167">hello following sample shows what you see for a successful deployment:</span></span>

```bash
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment xxxxxxx
+ Waiting for deployment toocomplete
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

### <a name="verify-hello-deployment"></a><span data-ttu-id="a2047-168">Ověření nasazení hello</span><span class="sxs-lookup"><span data-stu-id="a2047-168">Verify hello deployment</span></span>
<span data-ttu-id="a2047-169">Nyní SSH toohello virtuálního počítače jste vytvořili tooverify hello nasazení a spuštění pomocí hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2047-169">Now SSH toohello virtual machine you created tooverify hello deployment and start using hello new VM.</span></span> <span data-ttu-id="a2047-170">tooconnect pomocí protokolu SSH, najít IP adresu hello hello virtuálních počítačů, které jste vytvořili spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a2047-170">tooconnect via SSH, find hello IP address of hello VM you created by running hello following command:</span></span>

```azurecli
azure network public-ip show myResourceGroup1 myPublicIP
```

<span data-ttu-id="a2047-171">Hello veřejná IP adresa je uvedena ve výstupu příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="a2047-171">hello public IP address is listed in hello command output.</span></span> <span data-ttu-id="a2047-172">Ve výchozím nastavení je připojit toohello virtuálního počítače s Linuxem pomocí SSH na port 22.</span><span class="sxs-lookup"><span data-stu-id="a2047-172">By default you connect toohello Linux VM by SSH on port 22.</span></span>

## <a name="create-additional-vms"></a><span data-ttu-id="a2047-173">Vytvoření dalších virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a2047-173">Create additional VMs</span></span>
<span data-ttu-id="a2047-174">Použití hello zachycení bitové kopie a šablony toodeploy dalších virtuálních počítačů pomocí kroků hello v předcházející části hello.</span><span class="sxs-lookup"><span data-stu-id="a2047-174">Use hello captured image and template toodeploy additional VMs using hello steps in hello preceding section.</span></span> <span data-ttu-id="a2047-175">Další možnosti toocreate virtuální počítače z hello image zahrnout pomocí šabloně pro rychlý start nebo systémem hello **vytvoření virtuálního počítače azure** příkaz.</span><span class="sxs-lookup"><span data-stu-id="a2047-175">Other options toocreate VMs from hello image include using a quickstart template or running hello **azure vm create** command.</span></span>

### <a name="use-hello-captured-template"></a><span data-ttu-id="a2047-176">Použít šablonu zaznamenané hello</span><span class="sxs-lookup"><span data-stu-id="a2047-176">Use hello captured template</span></span>
<span data-ttu-id="a2047-177">toouse hello zachycení bitové kopie a šablony, postupujte podle těchto kroků (podrobně v předcházející části hello):</span><span class="sxs-lookup"><span data-stu-id="a2047-177">toouse hello captured image and template, follow these steps (detailed in hello preceding section):</span></span>

* <span data-ttu-id="a2047-178">Zajistěte, aby byl váš image virtuálního počítače v hello stejný účet úložiště, který je hostitelem virtuálního pevného disku Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2047-178">Ensure that your VM image is in hello same storage account that hosts your VM's VHD.</span></span>
* <span data-ttu-id="a2047-179">Zkopírujte soubor JSON hello šablonu a zadejte jedinečný název pro disk s operačním systémem hello hello nového Virtuálního počítače virtuální pevný disk (nebo virtuální pevné disky).</span><span class="sxs-lookup"><span data-stu-id="a2047-179">Copy hello template JSON file and specify a unique name for hello OS disk of hello new VM's VHD (or VHDs).</span></span> <span data-ttu-id="a2047-180">Například v hello **storageProfile**v části **virtuálního pevného disku**v **uri**, zadejte jedinečný název pro hello **osDisk** virtuálního pevného disku, podobně jako příliš`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span><span class="sxs-lookup"><span data-stu-id="a2047-180">For example, in hello **storageProfile**, under **vhd**, in **uri**, specify a unique name for hello **osDisk** VHD, similar too`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span></span>
* <span data-ttu-id="a2047-181">Vytvořte síťovou kartu v buď hello stejný nebo jiný virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a2047-181">Create a NIC in either hello same or a different virtual network.</span></span>
* <span data-ttu-id="a2047-182">Ve skupině prostředků hello, ve kterém můžete nastavit hello virtuální sítě pomocí souboru JSON šablony hello změnit, vytvořte nasazení.</span><span class="sxs-lookup"><span data-stu-id="a2047-182">Using hello modified template JSON file, create a deployment in hello resource group in which you set up hello virtual network.</span></span>

### <a name="use-a-quickstart-template"></a><span data-ttu-id="a2047-183">Použít šabloně pro rychlý start</span><span class="sxs-lookup"><span data-stu-id="a2047-183">Use a quickstart template</span></span>
<span data-ttu-id="a2047-184">Pokud chcete nastavit automaticky při vytváření virtuálního počítače z hello image sítě hello, můžete tyto prostředky v šabloně.</span><span class="sxs-lookup"><span data-stu-id="a2047-184">If you want hello network set up automatically when you create a VM from hello image, you can specify those resources in a template.</span></span> <span data-ttu-id="a2047-185">Například v tématu hello [101-vm z – uživatele – image šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) z Githubu.</span><span class="sxs-lookup"><span data-stu-id="a2047-185">For example, see hello [101-vm-from-user-image template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) from GitHub.</span></span> <span data-ttu-id="a2047-186">Tato šablona vytvoří virtuální počítač pomocí vlastní image a hello potřebná virtuální sítě, veřejnou IP adresu a Síťových prostředků.</span><span class="sxs-lookup"><span data-stu-id="a2047-186">This template creates a VM from your custom image and hello necessary virtual network, public IP address, and NIC resources.</span></span> <span data-ttu-id="a2047-187">Návod k používání hello šablony v hello portálu Azure, najdete v části [jak toocreate virtuálního počítače z vlastní image pomocí šablony Resource Manageru](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).</span><span class="sxs-lookup"><span data-stu-id="a2047-187">For a walkthrough of using hello template in hello Azure portal, see [How toocreate a virtual machine from a custom image using a Resource Manager template](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).</span></span>

### <a name="use-hello-azure-vm-create-command"></a><span data-ttu-id="a2047-188">Vytvoření virtuálního počítače hello azure použití příkazu</span><span class="sxs-lookup"><span data-stu-id="a2047-188">Use hello azure vm create command</span></span>
<span data-ttu-id="a2047-189">Obvykle je nejjednodušší toouse toocreate správce prostředků šablony virtuálního počítače z hello image.</span><span class="sxs-lookup"><span data-stu-id="a2047-189">Usually it's easiest toouse a Resource Manager template toocreate a VM from hello image.</span></span> <span data-ttu-id="a2047-190">Ale můžete vytvořit hello virtuálních počítačů *imperativní* pomocí hello **vytvoření virtuálního počítače azure** s hello **-Q** (**– image urn**) parametr .</span><span class="sxs-lookup"><span data-stu-id="a2047-190">However, you can create hello VM *imperatively* by using hello **azure vm create** command with hello **-Q** (**--image-urn**) parameter.</span></span> <span data-ttu-id="a2047-191">Pokud použijete tuto metodu, předáte hello **-d** (**vhd disk operačního systému –**) parametr toospecify hello umístění souboru VHD hello operačního systému pro hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2047-191">If you use this method, you also pass hello **-d** (**--os-disk-vhd**) parameter toospecify hello location of hello OS .vhd file for hello new VM.</span></span> <span data-ttu-id="a2047-192">Tento soubor musí být v kontejneru virtuální pevné disky hello účtu úložiště hello, kde je uložený soubor VHD hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a2047-192">This file must be in hello vhds container of hello storage account where hello image VHD file is stored.</span></span> <span data-ttu-id="a2047-193">příkaz kopie hello virtuálního pevného disku pro hello Hello nový virtuální počítač automaticky toohello **virtuální pevné disky** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="a2047-193">hello command copies hello VHD for hello new VM automatically toohello **vhds** container.</span></span>

<span data-ttu-id="a2047-194">Dřív, než spustíte **vytvoření virtuálního počítače azure** hello Image, dokončete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a2047-194">Before running **azure vm create** with hello image, complete hello following steps:</span></span>

1. <span data-ttu-id="a2047-195">Vytvořte skupinu prostředků nebo identifikovat existující skupinu prostředků pro nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="a2047-195">Create a resource group, or identify an existing resource group for hello deployment.</span></span>
2. <span data-ttu-id="a2047-196">Vytvoření veřejné IP adresu prostředku a Síťových prostředků pro hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2047-196">Create a public IP address resource and a NIC resource for hello new VM.</span></span> <span data-ttu-id="a2047-197">Pro kroky toocreate virtuální sítě, veřejnou IP adresu a síťový adaptér pomocí hello rozhraní příkazového řádku, viz výše v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="a2047-197">For steps toocreate a virtual network, public IP address, and NIC by using hello CLI, see earlier in this article.</span></span> <span data-ttu-id="a2047-198">(**vytvoření virtuálního počítače azure** můžete také vytvořit síťovou kartu, ale potřebujete toopass další parametry pro virtuální síť a podsíť.)</span><span class="sxs-lookup"><span data-stu-id="a2047-198">(**azure vm create** can also create a NIC, but you need toopass additional parameters for a virtual network and subnet.)</span></span>

<span data-ttu-id="a2047-199">Spusťte příkaz, který předává identifikátory URI tooboth hello nového souboru virtuálního pevného disku operačního systému a hello existující bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="a2047-199">Then run a command that passes URIs tooboth hello new OS VHD file and hello existing image.</span></span> <span data-ttu-id="a2047-200">V tomto příkladu je vytvořen s velikostí virtuálních počítačů Standard_A1 v oblasti Východ USA hello.</span><span class="sxs-lookup"><span data-stu-id="a2047-200">In this example, a size Standard_A1 VM is created in hello East US region.</span></span>

```azurecli
azure vm create -g myResourceGroup1 -n myNewVM -l eastus -y Linux \
-z Standard_A1 -u myAdminname -p myPassword -f myNIC \
-d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" \
-Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.vhd"
```

<span data-ttu-id="a2047-201">Další příkaz Možnosti spustit `azure help vm create`.</span><span class="sxs-lookup"><span data-stu-id="a2047-201">For additional command options, run `azure help vm create`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a2047-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2047-202">Next steps</span></span>
<span data-ttu-id="a2047-203">toomanage virtuální počítače s hello CLI, najdete v části úlohy hello v [nasadit a spravovat virtuální počítače pomocí šablony Azure Resource Manager a hello rozhraní příkazového řádku Azure](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="a2047-203">toomanage your VMs with hello CLI, see hello tasks in [Deploy and manage virtual machines by using Azure Resource Manager templates and hello Azure CLI](create-ssh-secured-vm-from-template.md).</span></span>

