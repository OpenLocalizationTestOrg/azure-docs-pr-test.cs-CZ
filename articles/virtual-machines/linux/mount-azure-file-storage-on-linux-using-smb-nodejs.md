---
title: "Připojení Azure File storage na virtuální počítače s Linuxem pomocí protokolu SMB 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tom, jak připojit Azure File storage na virtuální počítače s Linuxem pomocí protokolu SMB"
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
ms.openlocfilehash: 4951860630f0aad107d0846d52ebe4423ee0b91c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-by-using-smb-with-azure-cli-10"></a><span data-ttu-id="c063d-103">Připojení Azure File storage na virtuální počítače s Linuxem pomocí protokolu SMB 1.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c063d-103">Mount Azure File storage on Linux VMs by using SMB with Azure CLI 1.0</span></span>

<span data-ttu-id="c063d-104">Tento článek ukazuje, jak připojit Azure File storage na virtuální počítač s Linuxem pomocí protokolu Server Message Block (SMB).</span><span class="sxs-lookup"><span data-stu-id="c063d-104">This article shows how to mount Azure File storage on a Linux VM by using the Server Message Block (SMB) protocol.</span></span> <span data-ttu-id="c063d-105">File storage nabízí sdílené složky v cloudu přes standardní protokol SMB.</span><span class="sxs-lookup"><span data-stu-id="c063d-105">File storage offers file shares in the cloud via the standard SMB protocol.</span></span> <span data-ttu-id="c063d-106">Požadavky:</span><span class="sxs-lookup"><span data-stu-id="c063d-106">The requirements are:</span></span>

* <span data-ttu-id="c063d-107">[Účet Azure](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="c063d-107">An [Azure account](https://azure.microsoft.com/pricing/free-trial/)</span></span>
* [<span data-ttu-id="c063d-108">Secure Shell (SSH) soubory veřejného a privátního klíče</span><span class="sxs-lookup"><span data-stu-id="c063d-108">Secure Shell (SSH) public and private key files</span></span>](mac-create-ssh-keys.md)

## <a name="cli-versions-to-use"></a><span data-ttu-id="c063d-109">Verze rozhraní příkazového řádku používat</span><span class="sxs-lookup"><span data-stu-id="c063d-109">CLI versions to use</span></span>
<span data-ttu-id="c063d-110">Úlohu můžete dokončit pomocí jedné z následujících verzí rozhraní příkazového řádku (CLI):</span><span class="sxs-lookup"><span data-stu-id="c063d-110">You can complete the task by using one of the following command-line interface (CLI) versions:</span></span>

- <span data-ttu-id="c063d-111">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="c063d-111">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="c063d-112">[Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="c063d-112">[Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)- our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="c063d-113">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="c063d-113">Quick commands</span></span>
<span data-ttu-id="c063d-114">K provedení úlohy rychle, postupujte podle kroků v této části.</span><span class="sxs-lookup"><span data-stu-id="c063d-114">To accomplish the task quickly, follow the steps in this section.</span></span> <span data-ttu-id="c063d-115">Podrobné informace a kontext, začít ve ["Podrobný návod"](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) části.</span><span class="sxs-lookup"><span data-stu-id="c063d-115">For more detailed information and context, begin at the ["Detailed walkthrough"](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) section.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="c063d-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c063d-116">Prerequisites</span></span>
* <span data-ttu-id="c063d-117">Skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c063d-117">A resource group</span></span>
* <span data-ttu-id="c063d-118">Virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="c063d-118">An Azure virtual network</span></span>
* <span data-ttu-id="c063d-119">Skupina zabezpečení sítě pomocí SSH příchozí</span><span class="sxs-lookup"><span data-stu-id="c063d-119">A network security group with an SSH inbound</span></span>
* <span data-ttu-id="c063d-120">Podsíť</span><span class="sxs-lookup"><span data-stu-id="c063d-120">A subnet</span></span>
* <span data-ttu-id="c063d-121">Účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="c063d-121">An Azure storage account</span></span>
* <span data-ttu-id="c063d-122">Klíče účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="c063d-122">Azure storage account keys</span></span>
* <span data-ttu-id="c063d-123">Sdílenou složku Azure File storage</span><span class="sxs-lookup"><span data-stu-id="c063d-123">An Azure File storage share</span></span>
* <span data-ttu-id="c063d-124">Virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="c063d-124">A Linux VM</span></span>

<span data-ttu-id="c063d-125">Nahradí všechny příklady s vlastním nastavením.</span><span class="sxs-lookup"><span data-stu-id="c063d-125">Replace any examples with your own settings.</span></span>

### <a name="create-a-directory-for-the-local-mount"></a><span data-ttu-id="c063d-126">Vytvořte adresář pro místního připojení</span><span class="sxs-lookup"><span data-stu-id="c063d-126">Create a directory for the local mount</span></span>

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-the-file-storage-smb-share-to-the-mount-point"></a><span data-ttu-id="c063d-127">Připojte soubor úložiště do přípojného bodu sdílená složka SMB</span><span class="sxs-lookup"><span data-stu-id="c063d-127">Mount the File storage SMB share to the mount point</span></span>

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-the-mount-after-a-reboot"></a><span data-ttu-id="c063d-128">Zachovat připojení po restartu systému</span><span class="sxs-lookup"><span data-stu-id="c063d-128">Persist the mount after a reboot</span></span>
<span data-ttu-id="c063d-129">Přidejte následující řádek na `/etc/fstab`:</span><span class="sxs-lookup"><span data-stu-id="c063d-129">Add the following line to `/etc/fstab`:</span></span>

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="c063d-130">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="c063d-130">Detailed walkthrough</span></span>

<span data-ttu-id="c063d-131">File storage nabízí sdílené složky v cloudu, které používají standardní protokol SMB.</span><span class="sxs-lookup"><span data-stu-id="c063d-131">File storage offers file shares in the cloud that use the standard SMB protocol.</span></span> <span data-ttu-id="c063d-132">Nejnovější verze služby úložiště File můžete také připojit sdílenou složku z jakékoli operační systém, který podporuje protokol SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="c063d-132">With the latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.</span></span> <span data-ttu-id="c063d-133">Pokud používáte připojení protokolu SMB v systému Linux, získáte snadno zálohy robustní, trvalé archivováním umístění úložiště podporovaný SLA.</span><span class="sxs-lookup"><span data-stu-id="c063d-133">When you use an SMB mount on Linux, you get easy backups to a robust, permanent archiving storage location that is supported by an SLA.</span></span>

<span data-ttu-id="c063d-134">Přesunutí souborů z virtuálního počítače připojení protokolu SMB, který je hostován úložiště souborů je skvělým způsobem, jak ladit protokoly.</span><span class="sxs-lookup"><span data-stu-id="c063d-134">Moving files from a VM to an SMB mount that's hosted on File storage is a great way to debug logs.</span></span> <span data-ttu-id="c063d-135">Je to způsobeno téže sdílené složky protokolu SMB může být připojen místně do pracovní stanice se systémem Mac, Linux nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="c063d-135">That's because the same SMB share can be mounted locally to your Mac, Linux, or Windows workstation.</span></span> <span data-ttu-id="c063d-136">SMB není nejlepší řešení pro streamování Linux nebo aplikace protokolů v reálném čase, protože není protokol SMB vytvořené pro zpracování těchto funkcí velkou protokolování.</span><span class="sxs-lookup"><span data-stu-id="c063d-136">SMB isn't the best solution for streaming Linux or application logs in real time, because the SMB protocol is not built to handle such heavy logging duties.</span></span> <span data-ttu-id="c063d-137">Nástroje protokolování vyhrazené, jednotná vrstvy, jako je Fluentd bude vhodnější než SMB pro shromažďování Linux a aplikace protokolování výstupu.</span><span class="sxs-lookup"><span data-stu-id="c063d-137">A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.</span></span>

<span data-ttu-id="c063d-138">Pro tento podrobný návod jsme vytvořte součásti potřebné nejprve vytvořit sdílenou složku úložiště a pak připojte prostřednictvím protokolu SMB na virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="c063d-138">For this detailed walkthrough, we create the prerequisites needed to first create the File storage share, and then mount it via SMB on a Linux VM.</span></span>

1. <span data-ttu-id="c063d-139">Vytvořte účet úložiště Azure pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="c063d-139">Create an Azure storage account by using the following code:</span></span>

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. <span data-ttu-id="c063d-140">Zobrazit klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c063d-140">Show the storage account keys.</span></span>

    <span data-ttu-id="c063d-141">Když vytvoříte účet úložiště, klíče účtu jsou vytvořeny v párech, aby se mohou otáčet bez výpadku služby.</span><span class="sxs-lookup"><span data-stu-id="c063d-141">When you create a storage account, the account keys are created in pairs so that they can be rotated without any service interruption.</span></span> <span data-ttu-id="c063d-142">Když přepnete na druhý klíč v páru, můžete vytvořit nový pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="c063d-142">When you switch to the second key in the pair, you create a new key pair.</span></span> <span data-ttu-id="c063d-143">Nových klíčů účtu úložiště se vytváří vždy v párech, a že jste vždy k dispozici alespoň jeden klíč nepoužívané úložiště připravené přepnout do.</span><span class="sxs-lookup"><span data-stu-id="c063d-143">New storage account keys are always created in pairs, ensuring that you always have at least one unused storage key ready to switch to.</span></span> <span data-ttu-id="c063d-144">Chcete-li zobrazit klíče účtu úložiště, použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="c063d-144">To show the storage account keys, use the following code:</span></span>

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. <span data-ttu-id="c063d-145">Vytvořte sdílenou složku úložiště.</span><span class="sxs-lookup"><span data-stu-id="c063d-145">Create the File storage share.</span></span>

    <span data-ttu-id="c063d-146">Sdílenou složku úložiště obsahuje sdílenou složku SMB.</span><span class="sxs-lookup"><span data-stu-id="c063d-146">The File storage share contains the SMB share.</span></span> <span data-ttu-id="c063d-147">Kvóta je vždy vyjádřené v gigabajtech (GB).</span><span class="sxs-lookup"><span data-stu-id="c063d-147">The quota is always expressed in gigabytes (GB).</span></span> <span data-ttu-id="c063d-148">Pokud chcete vytvořit sdílenou složku úložiště, použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="c063d-148">To create the File storage share, use the following code:</span></span>

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. <span data-ttu-id="c063d-149">Vytvořte adresář přípojného bodu.</span><span class="sxs-lookup"><span data-stu-id="c063d-149">Create the mount-point directory.</span></span>

    <span data-ttu-id="c063d-150">Je nutné vytvořit místní adresář v souborovém systému Linux připojit sdílenou složku SMB.</span><span class="sxs-lookup"><span data-stu-id="c063d-150">You must create a local directory in the Linux file system to mount the SMB share to.</span></span> <span data-ttu-id="c063d-151">Nic zapsané nebo čtení z adresáře místní připojení se předají do složky SMB, který je hostován na úložiště File.</span><span class="sxs-lookup"><span data-stu-id="c063d-151">Anything written or read from the local mount directory is forwarded to the SMB share that's hosted on File storage.</span></span> <span data-ttu-id="c063d-152">Chcete-li vytvořit adresář, použijte následující kód:</span><span class="sxs-lookup"><span data-stu-id="c063d-152">To create the directory, use the following code:</span></span>

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. <span data-ttu-id="c063d-153">Připojte sdílenou složku SMB pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="c063d-153">Mount the SMB share by using the following code:</span></span>

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. <span data-ttu-id="c063d-154">Zachovat připojení SMB prostřednictvím restartování počítače.</span><span class="sxs-lookup"><span data-stu-id="c063d-154">Persist the SMB mount through reboots.</span></span>

    <span data-ttu-id="c063d-155">Po restartování virtuálního počítače s Linuxem, je při vypnutí nepřipojené připojené sdílenou složku SMB.</span><span class="sxs-lookup"><span data-stu-id="c063d-155">When you reboot the Linux VM, the mounted SMB share is unmounted during shutdown.</span></span> <span data-ttu-id="c063d-156">Pro opětovné připojení do sdílené složky protokolu SMB na spouštění, musí přidá řádek do Linux /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="c063d-156">To remount the SMB share on boot, you must add a line to the Linux /etc/fstab.</span></span> <span data-ttu-id="c063d-157">Linux používá soubor fstab zobrazte seznam systémů souborů, které je potřeba připojit během spouštění.</span><span class="sxs-lookup"><span data-stu-id="c063d-157">Linux uses the fstab file to list the file systems that it needs to mount during the boot process.</span></span> <span data-ttu-id="c063d-158">Přidání sdílené složky SMB zajistí, že sdílené složky úložiště bude trvale připojeného souboru systém pro virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="c063d-158">Adding the SMB share ensures that the File storage share is a permanently mounted file system for the Linux VM.</span></span> <span data-ttu-id="c063d-159">Přidání úložiště File sdílená složka SMB na nový virtuální počítač je možné, pokud používáte cloudové init.</span><span class="sxs-lookup"><span data-stu-id="c063d-159">Adding the File storage SMB share to a new VM is possible when you use cloud-init.</span></span>

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a><span data-ttu-id="c063d-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c063d-160">Next steps</span></span>

- [<span data-ttu-id="c063d-161">Přizpůsobení virtuálního počítače s Linuxem během vytváření pomocí init cloudu</span><span class="sxs-lookup"><span data-stu-id="c063d-161">Using cloud-init to customize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="c063d-162">Přidání disku do virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="c063d-162">Add a disk to a Linux VM</span></span>](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="c063d-163">Šifrování disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c063d-163">Encrypt disks on a Linux VM by using the Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
