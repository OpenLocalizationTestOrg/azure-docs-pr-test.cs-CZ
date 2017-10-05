---
title: "Připojení Azure File storage na virtuální počítače s Linuxem pomocí protokolu SMB | Microsoft Docs"
description: "Tom, jak připojit Azure File storage na virtuální počítače s Linuxem pomocí protokolu SMB 2.0 rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: 9eae17b304f8a987b44ebed8906dabd8ff3a36a8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a><span data-ttu-id="c6349-103">Připojení Azure File storage na virtuální počítače s Linuxem pomocí protokolu SMB</span><span class="sxs-lookup"><span data-stu-id="c6349-103">Mount Azure File storage on Linux VMs using SMB</span></span>

<span data-ttu-id="c6349-104">Tento článek ukazuje, jak využívat službu Azure File storage na virtuální počítač s Linuxem pomocí připojení protokolu SMB 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="c6349-104">This article shows you how to utilize the Azure File storage service on a Linux VM using an SMB mount with the Azure CLI 2.0.</span></span> <span data-ttu-id="c6349-105">Azure File storage nabízí sdílené složky v cloudu přes standardní protokol SMB.</span><span class="sxs-lookup"><span data-stu-id="c6349-105">Azure File storage offers file shares in the cloud using the standard SMB protocol.</span></span> <span data-ttu-id="c6349-106">K provedení těchto kroků můžete také využít [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c6349-106">You can also perform these steps with the [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="c6349-107">Požadavky:</span><span class="sxs-lookup"><span data-stu-id="c6349-107">The requirements are:</span></span>

- [<span data-ttu-id="c6349-108">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="c6349-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="c6349-109">Soubory veřejného a privátního klíče SSH</span><span class="sxs-lookup"><span data-stu-id="c6349-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

## <a name="quick-commands"></a><span data-ttu-id="c6349-110">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="c6349-110">Quick Commands</span></span>

* <span data-ttu-id="c6349-111">Skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c6349-111">A resource group</span></span>
* <span data-ttu-id="c6349-112">Virtuální síť Azure</span><span class="sxs-lookup"><span data-stu-id="c6349-112">An Azure virtual network</span></span>
* <span data-ttu-id="c6349-113">Skupina zabezpečení sítě pomocí SSH příchozí</span><span class="sxs-lookup"><span data-stu-id="c6349-113">A network security group with an SSH inbound</span></span>
* <span data-ttu-id="c6349-114">Podsíť</span><span class="sxs-lookup"><span data-stu-id="c6349-114">A subnet</span></span>
* <span data-ttu-id="c6349-115">Účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="c6349-115">An Azure storage account</span></span>
* <span data-ttu-id="c6349-116">Klíče účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="c6349-116">Azure storage account keys</span></span>
* <span data-ttu-id="c6349-117">Sdílenou složku Azure File storage</span><span class="sxs-lookup"><span data-stu-id="c6349-117">An Azure File storage share</span></span>
* <span data-ttu-id="c6349-118">Virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="c6349-118">A Linux VM</span></span>

<span data-ttu-id="c6349-119">Nahradí všechny příklady s vlastním nastavením.</span><span class="sxs-lookup"><span data-stu-id="c6349-119">Replace any examples with your own settings.</span></span>

### <a name="create-a-directory-for-the-local-mount"></a><span data-ttu-id="c6349-120">Vytvořte adresář pro místního připojení</span><span class="sxs-lookup"><span data-stu-id="c6349-120">Create a directory for the local mount</span></span>

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-the-file-storage-smb-share-to-the-mount-point"></a><span data-ttu-id="c6349-121">Připojte soubor úložiště do přípojného bodu sdílená složka SMB</span><span class="sxs-lookup"><span data-stu-id="c6349-121">Mount the File storage SMB share to the mount point</span></span>

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-the-mount-after-a-reboot"></a><span data-ttu-id="c6349-122">Zachovat připojení po restartu systému</span><span class="sxs-lookup"><span data-stu-id="c6349-122">Persist the mount after a reboot</span></span>
<span data-ttu-id="c6349-123">Uděláte to tak, přidejte následující řádek na `/etc/fstab`:</span><span class="sxs-lookup"><span data-stu-id="c6349-123">To do so, add the following line to the `/etc/fstab`:</span></span>

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="c6349-124">Podrobný postup</span><span class="sxs-lookup"><span data-stu-id="c6349-124">Detailed walkthrough</span></span>

<span data-ttu-id="c6349-125">File storage nabízí sdílené složky v cloudu, které používají standardní protokol SMB.</span><span class="sxs-lookup"><span data-stu-id="c6349-125">File storage offers file shares in the cloud that use the standard SMB protocol.</span></span> <span data-ttu-id="c6349-126">Nejnovější verze služby úložiště File můžete také připojit sdílenou složku z jakékoli operační systém, který podporuje protokol SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="c6349-126">With the latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.</span></span> <span data-ttu-id="c6349-127">Pokud používáte připojení protokolu SMB v systému Linux, získáte snadno zálohy robustní, trvalé archivováním umístění úložiště podporovaný SLA.</span><span class="sxs-lookup"><span data-stu-id="c6349-127">When you use an SMB mount on Linux, you get easy backups to a robust, permanent archiving storage location that is supported by an SLA.</span></span>

<span data-ttu-id="c6349-128">Přesunutí souborů z virtuálního počítače připojení protokolu SMB, který je hostován úložiště souborů je skvělým způsobem, jak ladit protokoly.</span><span class="sxs-lookup"><span data-stu-id="c6349-128">Moving files from a VM to an SMB mount that's hosted on File storage is a great way to debug logs.</span></span> <span data-ttu-id="c6349-129">Je to způsobeno téže sdílené složky protokolu SMB může být připojen místně do pracovní stanice se systémem Mac, Linux nebo Windows.</span><span class="sxs-lookup"><span data-stu-id="c6349-129">That's because the same SMB share can be mounted locally to your Mac, Linux, or Windows workstation.</span></span> <span data-ttu-id="c6349-130">SMB není nejlepší řešení pro streamování Linux nebo aplikace protokolů v reálném čase, protože není protokol SMB vytvořené pro zpracování těchto funkcí velkou protokolování.</span><span class="sxs-lookup"><span data-stu-id="c6349-130">SMB isn't the best solution for streaming Linux or application logs in real time, because the SMB protocol is not built to handle such heavy logging duties.</span></span> <span data-ttu-id="c6349-131">Nástroje protokolování vyhrazené, jednotná vrstvy, jako je Fluentd bude vhodnější než SMB pro shromažďování Linux a aplikace protokolování výstupu.</span><span class="sxs-lookup"><span data-stu-id="c6349-131">A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.</span></span>

<span data-ttu-id="c6349-132">Pro tento podrobný návod jsme vytvořte součásti potřebné nejprve vytvořit sdílenou složku úložiště a pak připojte prostřednictvím protokolu SMB na virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="c6349-132">For this detailed walkthrough, we create the prerequisites needed to first create the File storage share, and then mount it via SMB on a Linux VM.</span></span>

1. <span data-ttu-id="c6349-133">Vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create) pro sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="c6349-133">Create a resource group with [az group create](/cli/azure/group#create) to hold the file share.</span></span>

    <span data-ttu-id="c6349-134">Chcete-li vytvořit skupinu prostředků s názvem `myResourceGroup` v umístění "Západní USA", pomocí následujícího příkladu:</span><span class="sxs-lookup"><span data-stu-id="c6349-134">To create a resource group named `myResourceGroup` in the "West US" location, use the following example:</span></span>

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. <span data-ttu-id="c6349-135">Vytvoření účtu úložiště Azure s [vytvořit účet úložiště az](/cli/azure/storage/account#create) ukládat soubory.</span><span class="sxs-lookup"><span data-stu-id="c6349-135">Create an Azure storage account with [az storage account create](/cli/azure/storage/account#create) to store the actual files.</span></span>

    <span data-ttu-id="c6349-136">Chcete-li vytvořit účet úložiště s názvem můj_účet_úložiště pomocí úložiště Standard_LRS SKU, použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="c6349-136">To create a storage account named mystorageaccount by using the Standard_LRS storage SKU, use the following example:</span></span>

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. <span data-ttu-id="c6349-137">Zobrazit klíče účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c6349-137">Show the storage account keys.</span></span>

    <span data-ttu-id="c6349-138">Když vytvoříte účet úložiště, klíče účtu jsou vytvořeny v párech, aby se mohou otáčet bez výpadku služby.</span><span class="sxs-lookup"><span data-stu-id="c6349-138">When you create a storage account, the account keys are created in pairs so that they can be rotated without any service interruption.</span></span> <span data-ttu-id="c6349-139">Když přepnete na druhý klíč v páru, můžete vytvořit nový pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="c6349-139">When you switch to the second key in the pair, you create a new key pair.</span></span> <span data-ttu-id="c6349-140">Nových klíčů účtu úložiště se vytváří vždy v párech, a že jste vždy k dispozici alespoň jeden nepoužívané klíč účtu úložiště připravené přepnout do.</span><span class="sxs-lookup"><span data-stu-id="c6349-140">New storage account keys are always created in pairs, ensuring that you always have at least one unused storage account key ready to switch to.</span></span>

    <span data-ttu-id="c6349-141">Zobrazit klíče účtu úložiště s [seznam klíčů účtu úložiště az](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="c6349-141">View the storage account keys with the [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span> <span data-ttu-id="c6349-142">Účet úložiště klíčů pro pojmenované `mystorageaccount` jsou uvedené v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c6349-142">The storage account keys for the named `mystorageaccount` are listed in the following example:</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    <span data-ttu-id="c6349-143">Chcete-li extrahovat jeden klíč, použijte `--query` příznak.</span><span class="sxs-lookup"><span data-stu-id="c6349-143">To extract a single key, use the `--query` flag.</span></span> <span data-ttu-id="c6349-144">Následující příklad extrahuje první klíč (`[0]`):</span><span class="sxs-lookup"><span data-stu-id="c6349-144">The following example extracts the first key (`[0]`):</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. <span data-ttu-id="c6349-145">Vytvořte sdílenou složku úložiště.</span><span class="sxs-lookup"><span data-stu-id="c6349-145">Create the File storage share.</span></span>

    <span data-ttu-id="c6349-146">Sdílenou složku úložiště obsahuje sdílené složky SMB s [vytvořit sdílenou složku úložiště az](/cli/azure/storage/share#create).</span><span class="sxs-lookup"><span data-stu-id="c6349-146">The File storage share contains the SMB share with [az storage share create](/cli/azure/storage/share#create).</span></span> <span data-ttu-id="c6349-147">Kvóta je vždy vyjádřené v gigabajtech (GB).</span><span class="sxs-lookup"><span data-stu-id="c6349-147">The quota is always expressed in gigabytes (GB).</span></span> <span data-ttu-id="c6349-148">Předejte jí jeden z klíčů z předchozí `az storage account keys list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="c6349-148">Pass in one of the keys from the preceding `az storage account keys list` command.</span></span> <span data-ttu-id="c6349-149">Vytvořte sdílenou složku s názvem mystorageshare s kvótou 10 GB s použitím v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c6349-149">Create a share named mystorageshare with a 10-GB quota by using the following example:</span></span>

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. <span data-ttu-id="c6349-150">Vytvořte adresář přípojného bodu.</span><span class="sxs-lookup"><span data-stu-id="c6349-150">Create a mount-point directory.</span></span>

    <span data-ttu-id="c6349-151">Vytvořte místní adresář v souborovém systému Linux připojit sdílenou složku SMB.</span><span class="sxs-lookup"><span data-stu-id="c6349-151">Create a local directory in the Linux file system to mount the SMB share to.</span></span> <span data-ttu-id="c6349-152">Nic zapsané nebo čtení z adresáře místní připojení se předají do složky SMB, který je hostován na úložiště File.</span><span class="sxs-lookup"><span data-stu-id="c6349-152">Anything written or read from the local mount directory is forwarded to the SMB share that's hosted on File storage.</span></span> <span data-ttu-id="c6349-153">K vytvoření místního adresáře v /mnt/mymountdirectory, použijte následující příklad:</span><span class="sxs-lookup"><span data-stu-id="c6349-153">To create a local directory at /mnt/mymountdirectory, use the following example:</span></span>

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. <span data-ttu-id="c6349-154">Připojení sdílené složky SMB do místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="c6349-154">Mount the SMB share to the local directory.</span></span>

    <span data-ttu-id="c6349-155">Zadejte vlastní uživatelské jméno účtu úložiště a klíč účtu úložiště přihlašovacích údajů k připojení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c6349-155">Provide your own storage account username and storage account key for the mount credentials as follows:</span></span>

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. <span data-ttu-id="c6349-156">Zachovat připojení SMB prostřednictvím restartování počítače.</span><span class="sxs-lookup"><span data-stu-id="c6349-156">Persist the SMB mount through reboots.</span></span>

    <span data-ttu-id="c6349-157">Po restartování virtuálního počítače s Linuxem, je při vypnutí nepřipojené připojené sdílenou složku SMB.</span><span class="sxs-lookup"><span data-stu-id="c6349-157">When you reboot the Linux VM, the mounted SMB share is unmounted during shutdown.</span></span> <span data-ttu-id="c6349-158">Pro opětovné připojení do sdílené složky protokolu SMB na spuštění, přidejte řádek na Linux /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="c6349-158">To remount the SMB share on boot, add a line to the Linux /etc/fstab.</span></span> <span data-ttu-id="c6349-159">Linux používá soubor fstab zobrazte seznam systémů souborů, které je potřeba připojit během spouštění.</span><span class="sxs-lookup"><span data-stu-id="c6349-159">Linux uses the fstab file to list the file systems that it needs to mount during the boot process.</span></span> <span data-ttu-id="c6349-160">Přidání sdílené složky SMB zajistí, že sdílené složky úložiště bude trvale připojeného souboru systém pro virtuální počítač s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="c6349-160">Adding the SMB share ensures that the File storage share is a permanently mounted file system for the Linux VM.</span></span> <span data-ttu-id="c6349-161">Přidání úložiště File sdílená složka SMB na nový virtuální počítač je možné, pokud používáte cloudové init.</span><span class="sxs-lookup"><span data-stu-id="c6349-161">Adding the File storage SMB share to a new VM is possible when you use cloud-init.</span></span>

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a><span data-ttu-id="c6349-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c6349-162">Next steps</span></span>

- [<span data-ttu-id="c6349-163">Přizpůsobení virtuálního počítače s Linuxem během vytváření pomocí init cloudu</span><span class="sxs-lookup"><span data-stu-id="c6349-163">Using cloud-init to customize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="c6349-164">Přidání disku do virtuálního počítače s Linuxem</span><span class="sxs-lookup"><span data-stu-id="c6349-164">Add a disk to a Linux VM</span></span>](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="c6349-165">Šifrování disky na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="c6349-165">Encrypt disks on a Linux VM by using the Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
