---
title: "aaaMount Azure File storage ve virtuální počítače s Linuxem pomocí protokolu SMB 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Jak toomount Azure File storage ve virtuální počítače s Linuxem pomocí protokolu SMB"
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
ms.date: 12/07/2016
ms.author: v-livech
ms.openlocfilehash: 14a4224228cadb0ae2f05e8e5c8022ee84f138a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-by-using-smb-with-azure-cli-10"></a><span data-ttu-id="9bb41-103">Připojení Azure File storage na virtuální počítače s Linuxem pomocí protokolu SMB 1.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="9bb41-103">Mount Azure File storage on Linux VMs by using SMB with Azure CLI 1.0</span></span>

<span data-ttu-id="9bb41-104">Tento článek ukazuje, jak toomount Azure File storage na virtuální počítač s Linuxem pomocí hello zpráva bloku protokol Server (SMB).</span><span class="sxs-lookup"><span data-stu-id="9bb41-104">This article shows how toomount Azure File storage on a Linux VM by using hello Server Message Block (SMB) protocol.</span></span> <span data-ttu-id="9bb41-105">File storage nabízí sdílené složky v cloudu hello prostřednictvím hello standardní protokol SMB.</span><span class="sxs-lookup"><span data-stu-id="9bb41-105">File storage offers file shares in hello cloud via hello standard SMB protocol.</span></span> <span data-ttu-id="9bb41-106">Hello požadavky jsou:</span><span class="sxs-lookup"><span data-stu-id="9bb41-106">hello requirements are:</span></span>

* <span data-ttu-id="9bb41-107">[Účet Azure](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="9bb41-107">An [Azure account](https://azure.microsoft.com/pricing/free-trial/)</span></span>
* [<span data-ttu-id="9bb41-108">Secure Shell (SSH) soubory veřejného a privátního klíče</span><span class="sxs-lookup"><span data-stu-id="9bb41-108">Secure Shell (SSH) public and private key files</span></span>](mac-create-ssh-keys.md)

## <a name="cli-versions-toouse"></a><span data-ttu-id="9bb41-109">Toouse verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9bb41-109">CLI versions toouse</span></span>
<span data-ttu-id="9bb41-110">Hello úloh můžete dokončit pomocí jedné z hello následující verze rozhraní příkazového řádku (CLI):</span><span class="sxs-lookup"><span data-stu-id="9bb41-110">You can complete hello task by using one of hello following command-line interface (CLI) versions:</span></span>

- <span data-ttu-id="9bb41-111">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="9bb41-111">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="9bb41-112">[Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="9bb41-112">[Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)- our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="9bb41-113">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="9bb41-113">Quick commands</span></span>
<span data-ttu-id="9bb41-114">Úloha hello tooaccomplish rychle, postupujte podle kroků hello v této části.</span><span class="sxs-lookup"><span data-stu-id="9bb41-114">tooaccomplish hello task quickly, follow hello steps in this section.</span></span> <span data-ttu-id="9bb41-115">Podrobné informace a kontext, začít ve hello ["Podrobný návod"](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) části.</span><span class="sxs-lookup"><span data-stu-id="9bb41-115">For more detailed information and context, begin at hello ["Detailed walkthrough"](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) section.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="9bb41-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9bb41-116">Prerequisites</span></span>
* <span data-ttu-id="9bb41-117">Skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="9bb41-117">A resource group</span></span>
* <span data-ttu-id="9bb41-118">Virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="9bb41-118">An Azure virtual network</span></span>
* <span data-ttu-id="9bb41-119">Skupina zabezpečení sítě pomocí SSH příchozí</span><span class="sxs-lookup"><span data-stu-id="9bb41-119">A network security group with an SSH inbound</span></span>
* <span data-ttu-id="9bb41-120">Podsíť</span><span class="sxs-lookup"><span data-stu-id="9bb41-120">A subnet</span></span>
* <span data-ttu-id="9bb41-121">Účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="9bb41-121">An Azure storage account</span></span>
* <span data-ttu-id="9bb41-122">Klíče účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="9bb41-122">Azure storage account keys</span></span>
* <span data-ttu-id="9bb41-123">Sdílenou složku Azure File storage</span><span class="sxs-lookup"><span data-stu-id="9bb41-123">An Azure File storage share</span></span>
* <span data-ttu-id="9bb41-124">Virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="9bb41-124">A Linux VM</span></span>

<span data-ttu-id="9bb41-125">Nahradí všechny příklady s vlastním nastavením.</span><span class="sxs-lookup"><span data-stu-id="9bb41-125">Replace any examples with your own settings.</span></span>

### <a name="create-a-directory-for-hello-local-mount"></a><span data-ttu-id="9bb41-126">Vytvořte adresář pro hello místního připojení</span><span class="sxs-lookup"><span data-stu-id="9bb41-126">Create a directory for hello local mount</span></span>

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-hello-file-storage-smb-share-toohello-mount-point"></a><span data-ttu-id="9bb41-127">Připojit hello soubor úložiště SMB sdílenou složku toohello přípojný bod</span><span class="sxs-lookup"><span data-stu-id="9bb41-127">Mount hello File storage SMB share toohello mount point</span></span>

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-hello-mount-after-a-reboot"></a><span data-ttu-id="9bb41-128">Zachovat připojení hello po restartu systému</span><span class="sxs-lookup"><span data-stu-id="9bb41-128">Persist hello mount after a reboot</span></span>
<span data-ttu-id="9bb41-129">Přidejte následující řádek příliš hello`/etc/fstab`:</span><span class="sxs-lookup"><span data-stu-id="9bb41-129">Add hello following line too`/etc/fstab`:</span></span>

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="9bb41-130">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="9bb41-130">Detailed walkthrough</span></span>

<span data-ttu-id="9bb41-131">File storage nabízí sdílené složky v cloudu hello, které používají standardní protokol SMB hello.</span><span class="sxs-lookup"><span data-stu-id="9bb41-131">File storage offers file shares in hello cloud that use hello standard SMB protocol.</span></span> <span data-ttu-id="9bb41-132">Hello nejnovější verze služby úložiště File můžete také připojit sdílenou složku z jakékoli operační systém, který podporuje protokol SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="9bb41-132">With hello latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.</span></span> <span data-ttu-id="9bb41-133">Pokud používáte připojení protokolu SMB v systému Linux, získáte snadno zálohy tooa robustní, trvalé archivováním umístění úložiště podporovaný SLA.</span><span class="sxs-lookup"><span data-stu-id="9bb41-133">When you use an SMB mount on Linux, you get easy backups tooa robust, permanent archiving storage location that is supported by an SLA.</span></span>

<span data-ttu-id="9bb41-134">Přesunutí souborů z připojení SMB tooan virtuálního počítače, který je hostován na soubor úložiště je že skvělým způsobem toodebug protokoly.</span><span class="sxs-lookup"><span data-stu-id="9bb41-134">Moving files from a VM tooan SMB mount that's hosted on File storage is a great way toodebug logs.</span></span> <span data-ttu-id="9bb41-135">Je to způsobeno hello sdílet stejný protokol SMB může být připojen místně tooyour Mac, Linux nebo Windows pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="9bb41-135">That's because hello same SMB share can be mounted locally tooyour Mac, Linux, or Windows workstation.</span></span> <span data-ttu-id="9bb41-136">SMB není hello nejlepší řešení pro streamování Linux nebo aplikace protokolů v reálném čase, protože není hello protokolu SMB vytvořené toohandle těchto funkcí velkou protokolování.</span><span class="sxs-lookup"><span data-stu-id="9bb41-136">SMB isn't hello best solution for streaming Linux or application logs in real time, because hello SMB protocol is not built toohandle such heavy logging duties.</span></span> <span data-ttu-id="9bb41-137">Nástroje protokolování vyhrazené, jednotná vrstvy, jako je Fluentd bude vhodnější než SMB pro shromažďování Linux a aplikace protokolování výstupu.</span><span class="sxs-lookup"><span data-stu-id="9bb41-137">A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.</span></span>

<span data-ttu-id="9bb41-138">Pro tento podrobný návod vytvoříme hello požadavky potřeby toofirst vytvořit hello sdílenou složku úložiště a následné připojení prostřednictvím protokolu SMB na virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="9bb41-138">For this detailed walkthrough, we create hello prerequisites needed toofirst create hello File storage share, and then mount it via SMB on a Linux VM.</span></span>

1. <span data-ttu-id="9bb41-139">Vytvořte účet úložiště Azure pomocí hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="9bb41-139">Create an Azure storage account by using hello following code:</span></span>

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. <span data-ttu-id="9bb41-140">Zobrazit hello klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9bb41-140">Show hello storage account keys.</span></span>

    <span data-ttu-id="9bb41-141">Když vytvoříte účet úložiště, klíče účtu hello jsou vytvořeny v párech, aby mohou otáčet bez výpadku služby.</span><span class="sxs-lookup"><span data-stu-id="9bb41-141">When you create a storage account, hello account keys are created in pairs so that they can be rotated without any service interruption.</span></span> <span data-ttu-id="9bb41-142">Když přepnete toohello druhý klíč v páru hello, můžete vytvořit nový pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="9bb41-142">When you switch toohello second key in hello pair, you create a new key pair.</span></span> <span data-ttu-id="9bb41-143">Nových klíčů účtu úložiště se vytváří vždy v párech, a zajistit, abyste měli vždy alespoň jeden nepoužívané úložiště klíčů tooswitch připravené k.</span><span class="sxs-lookup"><span data-stu-id="9bb41-143">New storage account keys are always created in pairs, ensuring that you always have at least one unused storage key ready tooswitch to.</span></span> <span data-ttu-id="9bb41-144">klíče účtu úložiště tooshow hello, použijte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="9bb41-144">tooshow hello storage account keys, use hello following code:</span></span>

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. <span data-ttu-id="9bb41-145">Vytvořte sdílenou složku File storage hello.</span><span class="sxs-lookup"><span data-stu-id="9bb41-145">Create hello File storage share.</span></span>

    <span data-ttu-id="9bb41-146">Hello sdílenou složku úložiště obsahuje hello sdílenou složku SMB.</span><span class="sxs-lookup"><span data-stu-id="9bb41-146">hello File storage share contains hello SMB share.</span></span> <span data-ttu-id="9bb41-147">kvóta Hello je vždy vyjádřené v gigabajtech (GB).</span><span class="sxs-lookup"><span data-stu-id="9bb41-147">hello quota is always expressed in gigabytes (GB).</span></span> <span data-ttu-id="9bb41-148">toocreate hello sdílenou složku úložiště, použijte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="9bb41-148">toocreate hello File storage share, use hello following code:</span></span>

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. <span data-ttu-id="9bb41-149">Vytvořte adresář hello přípojného bodu.</span><span class="sxs-lookup"><span data-stu-id="9bb41-149">Create hello mount-point directory.</span></span>

    <span data-ttu-id="9bb41-150">Je nutné vytvořit místní adresář v hello Linux souboru systému toomount hello k sdílená složka SMB.</span><span class="sxs-lookup"><span data-stu-id="9bb41-150">You must create a local directory in hello Linux file system toomount hello SMB share to.</span></span> <span data-ttu-id="9bb41-151">Nic zapsané nebo čtení z adresáře místní připojení hello se předají toohello sdílená složka SMB, který je hostován na úložiště File.</span><span class="sxs-lookup"><span data-stu-id="9bb41-151">Anything written or read from hello local mount directory is forwarded toohello SMB share that's hosted on File storage.</span></span> <span data-ttu-id="9bb41-152">toocreate hello adresář, použijte hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="9bb41-152">toocreate hello directory, use hello following code:</span></span>

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. <span data-ttu-id="9bb41-153">Připojte hello sdílená složka SMB pomocí hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="9bb41-153">Mount hello SMB share by using hello following code:</span></span>

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. <span data-ttu-id="9bb41-154">Zachovat hello SMB připojit prostřednictvím restartování počítače.</span><span class="sxs-lookup"><span data-stu-id="9bb41-154">Persist hello SMB mount through reboots.</span></span>

    <span data-ttu-id="9bb41-155">Když restartujete hello virtuálního počítače s Linuxem, hello připojené sdílené složky SMB nepřipojené během vypnutí.</span><span class="sxs-lookup"><span data-stu-id="9bb41-155">When you reboot hello Linux VM, hello mounted SMB share is unmounted during shutdown.</span></span> <span data-ttu-id="9bb41-156">tooremount hello složce SMB na spouštěcí, je nutné přidat řádku toohello Linux /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="9bb41-156">tooremount hello SMB share on boot, you must add a line toohello Linux /etc/fstab.</span></span> <span data-ttu-id="9bb41-157">Linux používá hello fstab souboru toolist hello systémy souborů je nutné toomount během procesu spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="9bb41-157">Linux uses hello fstab file toolist hello file systems that it needs toomount during hello boot process.</span></span> <span data-ttu-id="9bb41-158">Přidání sdílené složky SMB hello zajišťuje, že aby hello sdílená úložiště je trvale připojeného souboru systém pro virtuální počítač s Linuxem hello.</span><span class="sxs-lookup"><span data-stu-id="9bb41-158">Adding hello SMB share ensures that hello File storage share is a permanently mounted file system for hello Linux VM.</span></span> <span data-ttu-id="9bb41-159">Přidání hello soubor úložiště SMB sdílenou složku tooa nového virtuálního počítače je možné, pokud používáte cloudové init.</span><span class="sxs-lookup"><span data-stu-id="9bb41-159">Adding hello File storage SMB share tooa new VM is possible when you use cloud-init.</span></span>

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a><span data-ttu-id="9bb41-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9bb41-160">Next steps</span></span>

- [<span data-ttu-id="9bb41-161">Během vytváření pomocí toocustomize init cloudu virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="9bb41-161">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="9bb41-162">Přidat tooa disku virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="9bb41-162">Add a disk tooa Linux VM</span></span>](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="9bb41-163">Šifrování disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure hello</span><span class="sxs-lookup"><span data-stu-id="9bb41-163">Encrypt disks on a Linux VM by using hello Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
