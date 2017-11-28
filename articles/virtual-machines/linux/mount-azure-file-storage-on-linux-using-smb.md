---
title: "aaaMount Azure File storage ve virtuální počítače s Linuxem pomocí protokolu SMB | Microsoft Docs"
description: "Jak toomount Azure File storage ve virtuální počítače s Linuxem pomocí protokolu SMB s hello 2.0 rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/13/2017
ms.author: v-livech
ms.openlocfilehash: 7b34c81e602748b78c924f44a54b876744c28f56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a><span data-ttu-id="54136-103">Připojení Azure File storage na virtuální počítače s Linuxem pomocí protokolu SMB</span><span class="sxs-lookup"><span data-stu-id="54136-103">Mount Azure File storage on Linux VMs using SMB</span></span>

<span data-ttu-id="54136-104">Tento článek ukazuje, jak připojit tooutilize hello službu úložiště Azure File na virtuální počítač s Linuxem pomocí SMB s hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="54136-104">This article shows you how tooutilize hello Azure File storage service on a Linux VM using an SMB mount with hello Azure CLI 2.0.</span></span> <span data-ttu-id="54136-105">Azure File storage nabízí sdílené složky v cloudu hello pomocí standardního protokol SMB hello.</span><span class="sxs-lookup"><span data-stu-id="54136-105">Azure File storage offers file shares in hello cloud using hello standard SMB protocol.</span></span> <span data-ttu-id="54136-106">Můžete také provést tyto kroky hello [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="54136-106">You can also perform these steps with hello [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="54136-107">Hello požadavky jsou:</span><span class="sxs-lookup"><span data-stu-id="54136-107">hello requirements are:</span></span>

- [<span data-ttu-id="54136-108">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="54136-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="54136-109">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="54136-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

## <a name="quick-commands"></a><span data-ttu-id="54136-110">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="54136-110">Quick Commands</span></span>

* <span data-ttu-id="54136-111">Skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="54136-111">A resource group</span></span>
* <span data-ttu-id="54136-112">Virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="54136-112">An Azure virtual network</span></span>
* <span data-ttu-id="54136-113">Skupina zabezpečení sítě pomocí SSH příchozí</span><span class="sxs-lookup"><span data-stu-id="54136-113">A network security group with an SSH inbound</span></span>
* <span data-ttu-id="54136-114">Podsíť</span><span class="sxs-lookup"><span data-stu-id="54136-114">A subnet</span></span>
* <span data-ttu-id="54136-115">Účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="54136-115">An Azure storage account</span></span>
* <span data-ttu-id="54136-116">Klíče účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="54136-116">Azure storage account keys</span></span>
* <span data-ttu-id="54136-117">Sdílenou složku Azure File storage</span><span class="sxs-lookup"><span data-stu-id="54136-117">An Azure File storage share</span></span>
* <span data-ttu-id="54136-118">Virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="54136-118">A Linux VM</span></span>

<span data-ttu-id="54136-119">Nahradí všechny příklady s vlastním nastavením.</span><span class="sxs-lookup"><span data-stu-id="54136-119">Replace any examples with your own settings.</span></span>

### <a name="create-a-directory-for-hello-local-mount"></a><span data-ttu-id="54136-120">Vytvořte adresář pro hello místního připojení</span><span class="sxs-lookup"><span data-stu-id="54136-120">Create a directory for hello local mount</span></span>

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-hello-file-storage-smb-share-toohello-mount-point"></a><span data-ttu-id="54136-121">Připojit hello soubor úložiště SMB sdílenou složku toohello přípojný bod</span><span class="sxs-lookup"><span data-stu-id="54136-121">Mount hello File storage SMB share toohello mount point</span></span>

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-hello-mount-after-a-reboot"></a><span data-ttu-id="54136-122">Zachovat připojení hello po restartu systému</span><span class="sxs-lookup"><span data-stu-id="54136-122">Persist hello mount after a reboot</span></span>
<span data-ttu-id="54136-123">toodo Ano, přidejte následující řádek toohello hello `/etc/fstab`:</span><span class="sxs-lookup"><span data-stu-id="54136-123">toodo so, add hello following line toohello `/etc/fstab`:</span></span>

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="54136-124">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="54136-124">Detailed walkthrough</span></span>

<span data-ttu-id="54136-125">File storage nabízí sdílené složky v cloudu hello, které používají standardní protokol SMB hello.</span><span class="sxs-lookup"><span data-stu-id="54136-125">File storage offers file shares in hello cloud that use hello standard SMB protocol.</span></span> <span data-ttu-id="54136-126">Hello nejnovější verze služby úložiště File můžete také připojit sdílenou složku z jakékoli operační systém, který podporuje protokol SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="54136-126">With hello latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.</span></span> <span data-ttu-id="54136-127">Pokud používáte připojení protokolu SMB v systému Linux, získáte snadno zálohy tooa robustní, trvalé archivováním umístění úložiště podporovaný SLA.</span><span class="sxs-lookup"><span data-stu-id="54136-127">When you use an SMB mount on Linux, you get easy backups tooa robust, permanent archiving storage location that is supported by an SLA.</span></span>

<span data-ttu-id="54136-128">Přesunutí souborů z připojení SMB tooan virtuálního počítače, který je hostován na soubor úložiště je že skvělým způsobem toodebug protokoly.</span><span class="sxs-lookup"><span data-stu-id="54136-128">Moving files from a VM tooan SMB mount that's hosted on File storage is a great way toodebug logs.</span></span> <span data-ttu-id="54136-129">Je to způsobeno hello sdílet stejný protokol SMB může být připojen místně tooyour Mac, Linux nebo Windows pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="54136-129">That's because hello same SMB share can be mounted locally tooyour Mac, Linux, or Windows workstation.</span></span> <span data-ttu-id="54136-130">SMB není hello nejlepší řešení pro streamování Linux nebo aplikace protokolů v reálném čase, protože není hello protokolu SMB vytvořené toohandle těchto funkcí velkou protokolování.</span><span class="sxs-lookup"><span data-stu-id="54136-130">SMB isn't hello best solution for streaming Linux or application logs in real time, because hello SMB protocol is not built toohandle such heavy logging duties.</span></span> <span data-ttu-id="54136-131">Nástroje protokolování vyhrazené, jednotná vrstvy, jako je Fluentd bude vhodnější než SMB pro shromažďování Linux a aplikace protokolování výstupu.</span><span class="sxs-lookup"><span data-stu-id="54136-131">A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.</span></span>

<span data-ttu-id="54136-132">Pro tento podrobný návod vytvoříme hello požadavky potřeby toofirst vytvořit hello sdílenou složku úložiště a následné připojení prostřednictvím protokolu SMB na virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="54136-132">For this detailed walkthrough, we create hello prerequisites needed toofirst create hello File storage share, and then mount it via SMB on a Linux VM.</span></span>

1. <span data-ttu-id="54136-133">Vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create) toohold hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="54136-133">Create a resource group with [az group create](/cli/azure/group#create) toohold hello file share.</span></span>

    <span data-ttu-id="54136-134">skupinu prostředků s názvem toocreate `myResourceGroup` v umístění "Západní USA" hello, použijte následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="54136-134">toocreate a resource group named `myResourceGroup` in hello "West US" location, use hello following example:</span></span>

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. <span data-ttu-id="54136-135">Vytvoření účtu úložiště Azure s [vytvořit účet úložiště az](/cli/azure/storage/account#create) toostore hello skutečné soubory.</span><span class="sxs-lookup"><span data-stu-id="54136-135">Create an Azure storage account with [az storage account create](/cli/azure/storage/account#create) toostore hello actual files.</span></span>

    <span data-ttu-id="54136-136">toocreate účet úložiště s názvem můj_účet_úložiště pomocí hello Standard_LRS úložiště SKU, použijte hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="54136-136">toocreate a storage account named mystorageaccount by using hello Standard_LRS storage SKU, use hello following example:</span></span>

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. <span data-ttu-id="54136-137">Zobrazit hello klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="54136-137">Show hello storage account keys.</span></span>

    <span data-ttu-id="54136-138">Když vytvoříte účet úložiště, klíče účtu hello jsou vytvořeny v párech, aby mohou otáčet bez výpadku služby.</span><span class="sxs-lookup"><span data-stu-id="54136-138">When you create a storage account, hello account keys are created in pairs so that they can be rotated without any service interruption.</span></span> <span data-ttu-id="54136-139">Když přepnete toohello druhý klíč v páru hello, můžete vytvořit nový pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="54136-139">When you switch toohello second key in hello pair, you create a new key pair.</span></span> <span data-ttu-id="54136-140">Nových klíčů účtu úložiště se vytváří vždy v párech, a zajistit, abyste měli vždy alespoň jeden nepoužívané úložiště účet klíče připravené tooswitch k.</span><span class="sxs-lookup"><span data-stu-id="54136-140">New storage account keys are always created in pairs, ensuring that you always have at least one unused storage account key ready tooswitch to.</span></span>

    <span data-ttu-id="54136-141">Zobrazit hello klíče účtu úložiště s hello [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="54136-141">View hello storage account keys with hello [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span> <span data-ttu-id="54136-142">Hello klíče účtu úložiště pro hello s názvem `mystorageaccount` jsou uvedeny v následující ukázka hello:</span><span class="sxs-lookup"><span data-stu-id="54136-142">hello storage account keys for hello named `mystorageaccount` are listed in hello following example:</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    <span data-ttu-id="54136-143">tooextract jeden klíč, použijte hello `--query` příznak.</span><span class="sxs-lookup"><span data-stu-id="54136-143">tooextract a single key, use hello `--query` flag.</span></span> <span data-ttu-id="54136-144">Hello následující příklad extrahuje první klíč hello (`[0]`):</span><span class="sxs-lookup"><span data-stu-id="54136-144">hello following example extracts hello first key (`[0]`):</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. <span data-ttu-id="54136-145">Vytvořte sdílenou složku File storage hello.</span><span class="sxs-lookup"><span data-stu-id="54136-145">Create hello File storage share.</span></span>

    <span data-ttu-id="54136-146">Hello sdílenou složku úložiště obsahuje hello se sdílená složka SMB [vytvořit sdílenou složku úložiště az](/cli/azure/storage/share#create).</span><span class="sxs-lookup"><span data-stu-id="54136-146">hello File storage share contains hello SMB share with [az storage share create](/cli/azure/storage/share#create).</span></span> <span data-ttu-id="54136-147">kvóta Hello je vždy vyjádřené v gigabajtech (GB).</span><span class="sxs-lookup"><span data-stu-id="54136-147">hello quota is always expressed in gigabytes (GB).</span></span> <span data-ttu-id="54136-148">Jeden z klíčů hello předat z předchozí hello `az storage account keys list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="54136-148">Pass in one of hello keys from hello preceding `az storage account keys list` command.</span></span> <span data-ttu-id="54136-149">Vytvořte sdílenou složku s názvem mystorageshare s kvótou 10 GB s použitím hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="54136-149">Create a share named mystorageshare with a 10-GB quota by using hello following example:</span></span>

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. <span data-ttu-id="54136-150">Vytvořte adresář přípojného bodu.</span><span class="sxs-lookup"><span data-stu-id="54136-150">Create a mount-point directory.</span></span>

    <span data-ttu-id="54136-151">Vytvořte místní adresář v hello Linux souboru systému toomount hello k sdílená složka SMB.</span><span class="sxs-lookup"><span data-stu-id="54136-151">Create a local directory in hello Linux file system toomount hello SMB share to.</span></span> <span data-ttu-id="54136-152">Nic zapsané nebo čtení z adresáře místní připojení hello se předají toohello sdílená složka SMB, který je hostován na úložiště File.</span><span class="sxs-lookup"><span data-stu-id="54136-152">Anything written or read from hello local mount directory is forwarded toohello SMB share that's hosted on File storage.</span></span> <span data-ttu-id="54136-153">toocreate do místního adresáře v/mnt/mymountdirectory, použijte hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="54136-153">toocreate a local directory at /mnt/mymountdirectory, use hello following example:</span></span>

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. <span data-ttu-id="54136-154">Připojte hello SMB sdílenou složku toohello místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="54136-154">Mount hello SMB share toohello local directory.</span></span>

    <span data-ttu-id="54136-155">Zadejte vlastní uživatelské jméno účtu úložiště a klíč účtu úložiště pro přihlašovací údaje pro připojení k hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="54136-155">Provide your own storage account username and storage account key for hello mount credentials as follows:</span></span>

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. <span data-ttu-id="54136-156">Zachovat hello SMB připojit prostřednictvím restartování počítače.</span><span class="sxs-lookup"><span data-stu-id="54136-156">Persist hello SMB mount through reboots.</span></span>

    <span data-ttu-id="54136-157">Když restartujete hello virtuálního počítače s Linuxem, hello připojené sdílené složky SMB nepřipojené během vypnutí.</span><span class="sxs-lookup"><span data-stu-id="54136-157">When you reboot hello Linux VM, hello mounted SMB share is unmounted during shutdown.</span></span> <span data-ttu-id="54136-158">hello tooremount sdílená složka SMB na spuštění, přidejte řádek toohello Linux /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="54136-158">tooremount hello SMB share on boot, add a line toohello Linux /etc/fstab.</span></span> <span data-ttu-id="54136-159">Linux používá hello fstab souboru toolist hello systémy souborů je nutné toomount během procesu spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="54136-159">Linux uses hello fstab file toolist hello file systems that it needs toomount during hello boot process.</span></span> <span data-ttu-id="54136-160">Přidání sdílené složky SMB hello zajišťuje, že aby hello sdílená úložiště je trvale připojeného souboru systém pro virtuální počítač s Linuxem hello.</span><span class="sxs-lookup"><span data-stu-id="54136-160">Adding hello SMB share ensures that hello File storage share is a permanently mounted file system for hello Linux VM.</span></span> <span data-ttu-id="54136-161">Přidání hello soubor úložiště SMB sdílenou složku tooa nového virtuálního počítače je možné, pokud používáte cloudové init.</span><span class="sxs-lookup"><span data-stu-id="54136-161">Adding hello File storage SMB share tooa new VM is possible when you use cloud-init.</span></span>

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a><span data-ttu-id="54136-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54136-162">Next steps</span></span>

- [<span data-ttu-id="54136-163">Během vytváření pomocí toocustomize init cloudu virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="54136-163">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="54136-164">Přidat tooa disku virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="54136-164">Add a disk tooa Linux VM</span></span>](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="54136-165">Šifrování disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure hello</span><span class="sxs-lookup"><span data-stu-id="54136-165">Encrypt disks on a Linux VM by using hello Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
