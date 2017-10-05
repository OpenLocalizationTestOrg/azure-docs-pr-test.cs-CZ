---
title: "Používání Azure File storage s Linuxem | Microsoft Docs"
description: "Zjistěte, jak připojit sdílenou složku Azure přes protokol SMB v systému Linux."
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/8/2017
ms.author: renash
ms.openlocfilehash: 27b393a899c60a3a0393619f338a396dff659498
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-azure-file-storage-with-linux"></a><span data-ttu-id="a24a0-103">Používání Azure File storage s Linuxem</span><span class="sxs-lookup"><span data-stu-id="a24a0-103">Use Azure File storage with Linux</span></span>
<span data-ttu-id="a24a0-104">[Azure File Storage](storage-dotnet-how-to-use-files.md) je snadno použitelný cloudový systém souborů od Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="a24a0-104">[Azure File storage](storage-dotnet-how-to-use-files.md) is Microsoft's easy to use cloud file system.</span></span> <span data-ttu-id="a24a0-105">Sdílené složky Azure může být připojen v Linuxových distribucích pomocí [cifs utils balíček](https://wiki.samba.org/index.php/LinuxCIFS_utils) z [Samba projektu](https://www.samba.org/).</span><span class="sxs-lookup"><span data-stu-id="a24a0-105">Azure File shares can be mounted in Linux distributions using the [cifs-utils package](https://wiki.samba.org/index.php/LinuxCIFS_utils) from the [Samba project](https://www.samba.org/).</span></span> <span data-ttu-id="a24a0-106">Tento článek ukazuje dva způsoby, jak připojit sdílenou složku Azure: na vyžádání pomocí `mount` příkazů a na spouštění pomocí vytváření položku v `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="a24a0-106">This article shows two ways to mount an Azure File share: on-demand with the `mount` command and on-boot by creating an entry in `/etc/fstab`.</span></span>

> [!NOTE]  
> <span data-ttu-id="a24a0-107">Aby bylo možné připojit Azure sdílenou složku mimo Azure oblast, kterou je hostovaná v, jako jsou místní nebo v jiné oblasti Azure, operačního systému musí podporovat funkci šifrování protokolu SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="a24a0-107">In order to mount an Azure File share outside of the Azure region it is hosted in, such as on-premises or in a different Azure region, the OS must support the encryption functionality of SMB 3.0.</span></span> <span data-ttu-id="a24a0-108">Šifrování funkce pro protokol SMB 3.0 pro Linux byla zavedena v 4.11 jádra.</span><span class="sxs-lookup"><span data-stu-id="a24a0-108">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="a24a0-109">Tato funkce umožňuje připojení Azure sdílené položky z místní nebo jiné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="a24a0-109">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="a24a0-110">V době publikování tato funkce byla přeneseny zpět do Ubuntu z 16.04 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="a24a0-110">At the time of publishing, this functionality has been backported to Ubuntu from 16.04 and above.</span></span>


## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a><span data-ttu-id="a24a0-111">Prerequisities pro připojení Azure File sdílet s Linux a cifs utils balíček</span><span class="sxs-lookup"><span data-stu-id="a24a0-111">Prerequisities for mounting an Azure File share with Linux and the cifs-utils package</span></span>
* <span data-ttu-id="a24a0-112">**Vyberte distribuce systému Linux, která může mít tento balíček cifs utils nainstalován**: Společnost Microsoft doporučuje následující Linuxových distribucích v galerii Azure bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="a24a0-112">**Pick a Linux distribution that can have the cifs-utils package installed**: Microsoft recommends the following Linux distributions in the Azure image gallery:</span></span>

    * <span data-ttu-id="a24a0-113">Ubuntu Server 14.04 +</span><span class="sxs-lookup"><span data-stu-id="a24a0-113">Ubuntu Server 14.04+</span></span>
    * <span data-ttu-id="a24a0-114">RHEL 7 +</span><span class="sxs-lookup"><span data-stu-id="a24a0-114">RHEL 7+</span></span>
    * <span data-ttu-id="a24a0-115">CentOS 7 +</span><span class="sxs-lookup"><span data-stu-id="a24a0-115">CentOS 7+</span></span>
    * <span data-ttu-id="a24a0-116">Debian 8</span><span class="sxs-lookup"><span data-stu-id="a24a0-116">Debian 8</span></span>
    * <span data-ttu-id="a24a0-117">openSUSE 13.2 +</span><span class="sxs-lookup"><span data-stu-id="a24a0-117">openSUSE 13.2+</span></span>
    * <span data-ttu-id="a24a0-118">SUSE Linux Enterprise Server 12</span><span class="sxs-lookup"><span data-stu-id="a24a0-118">SUSE Linux Enterprise Server 12</span></span>

* <span data-ttu-id="a24a0-119"><a id="install-cifs-utils"></a>**Instalaci balíčku cifs utils**: cifs-utils můžete nainstalovat pomocí Správce balíčků na distribučním Linux podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="a24a0-119"><a id="install-cifs-utils"></a>**The cifs-utils package is installed**: The cifs-utils can be installed using the package manager on the Linux distribution of your choice.</span></span> 

    <span data-ttu-id="a24a0-120">Na **Ubuntu** a **na základě Debian** distribuce, použijte `apt-get` Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="a24a0-120">On **Ubuntu** and **Debian-based** distributions, use the `apt-get` package manager:</span></span>

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    <span data-ttu-id="a24a0-121">Na **RHEL** a **CentOS**, použijte `yum` Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="a24a0-121">On **RHEL** and **CentOS**, use the `yum` package manager:</span></span>

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    <span data-ttu-id="a24a0-122">Na **openSUSE**, použijte `zypper` Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="a24a0-122">On **openSUSE**, use the `zypper` package manager:</span></span>

    ```
    sudo zypper install samba*
    ```

    <span data-ttu-id="a24a0-123">Na další distribuce pomocí příslušné package manager nebo [zkompilovat ze zdroje](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span><span class="sxs-lookup"><span data-stu-id="a24a0-123">On other distributions, use the appropriate package manager or [compile from source](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span></span>

* <span data-ttu-id="a24a0-124">**Rozhodněte o oprávnění souboru nebo adresáře připojenou složku**: V následujících příkladech používáme 0777, aby číst, zapisovat a spouštět oprávnění pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="a24a0-124">**Decide on the directory/file permissions of the mounted share**: In the examples below, we use 0777, to give read, write, and execute permissions to all users.</span></span> <span data-ttu-id="a24a0-125">Nahraďte ji s jinými [oprávnění chmod](https://en.wikipedia.org/wiki/Chmod) podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="a24a0-125">You can replace it with other [chmod permissions](https://en.wikipedia.org/wiki/Chmod) as desired.</span></span> 

* <span data-ttu-id="a24a0-126">**Název účtu úložiště:** Pro připojení sdílené složky Azure budete potřebovat název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="a24a0-126">**Storage Account Name**: To mount an Azure File share, you will need the name of the storage account.</span></span>

* <span data-ttu-id="a24a0-127">**Klíč účtu úložiště:** Pro připojení sdílené složky Azure budete potřebovat primární (nebo sekundární) klíč úložiště.</span><span class="sxs-lookup"><span data-stu-id="a24a0-127">**Storage Account Key**: To mount an Azure File share, you will need the primary (or secondary) storage key.</span></span> <span data-ttu-id="a24a0-128">Klíče SAS aktuálně nejsou pro připojení podporovány.</span><span class="sxs-lookup"><span data-stu-id="a24a0-128">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="a24a0-129">**Ujistěte se, je otevřený port 445**: SMB komunikuje přes port TCP 445 - zkontrolujte, pokud chcete zobrazit, pokud brána firewall neblokuje TCP porty 445 z klientského počítače.</span><span class="sxs-lookup"><span data-stu-id="a24a0-129">**Ensure port 445 is open**: SMB communicates over TCP port 445 - check to see if your firewall is not blocking TCP ports 445 from client machine.</span></span>

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a><span data-ttu-id="a24a0-130">Připojení Azure File sdílet na vyžádání pomocí`mount`</span><span class="sxs-lookup"><span data-stu-id="a24a0-130">Mount the Azure File share on-demand with `mount`</span></span>
1. <span data-ttu-id="a24a0-131">**[Nainstalovat balíček cifs utils pro Linux distribuční](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="a24a0-131">**[Install the cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="a24a0-132">**Vytvořte složku pro přípojného bodu**: to můžete provést libovolné místo v systému souborů.</span><span class="sxs-lookup"><span data-stu-id="a24a0-132">**Create a folder for the mount point**: This can be done anywhere on the file system.</span></span>

    ```
    mkdir mymountpoint
    ```

3. <span data-ttu-id="a24a0-133">**Použijte příkaz připojení připojit sdílenou složku Azure File**: Nezapomeňte nahradit `<storage-account-name>`, `<share-name>`, a `<storage-account-key>` správné informace.</span><span class="sxs-lookup"><span data-stu-id="a24a0-133">**Use the mount command to mount the Azure File share**: Remember to replace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with the proper information.</span></span>

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> <span data-ttu-id="a24a0-134">Po dokončení použití sdílené složky Azure File, můžete použít `sudo umount ./mymountpoint` o odpojení sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="a24a0-134">When you are done using the Azure File share, you may use `sudo umount ./mymountpoint` to unmount the share.</span></span>

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a><span data-ttu-id="a24a0-135">Vytvořit bod trvalé připojení pro Azure sdílené složky`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="a24a0-135">Create a persistent mount point for the Azure File share with `/etc/fstab`</span></span>
1. <span data-ttu-id="a24a0-136">**[Nainstalovat balíček cifs utils pro Linux distribuční](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="a24a0-136">**[Install the cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="a24a0-137">**Vytvořte složku pro přípojného bodu**: to můžete provést libovolné místo v systému souborů, ale je potřeba si absolutní cestu ke složce.</span><span class="sxs-lookup"><span data-stu-id="a24a0-137">**Create a folder for the mount point**: This can be done anywhere on the file system, but you need to note the absolute path of the folder.</span></span> <span data-ttu-id="a24a0-138">Následující příklad vytvoří složku v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="a24a0-138">The following example creates a folder under root.</span></span>

    ```
    sudo mkdir /mymountpoint
    ```

3. <span data-ttu-id="a24a0-139">**Použijte následující příkaz pro připojení následující řádek do `/etc/fstab`** : Nezapomeňte nahradit `<storage-account-name>`, `<share-name>`, a `<storage-account-key>` správné informace.</span><span class="sxs-lookup"><span data-stu-id="a24a0-139">**Use the following command to append the following line to `/etc/fstab`**: Remember to replace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with the proper information.</span></span>

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> <span data-ttu-id="a24a0-140">Můžete použít `sudo mount -a` připojit sdílenou složku Azure File po dokončení úprav `/etc/fstab` místo restartování.</span><span class="sxs-lookup"><span data-stu-id="a24a0-140">You can use `sudo mount -a` to mount the Azure File share after editing `/etc/fstab` instead of rebooting.</span></span>

## <a name="feedback"></a><span data-ttu-id="a24a0-141">Váš názor</span><span class="sxs-lookup"><span data-stu-id="a24a0-141">Feedback</span></span>
<span data-ttu-id="a24a0-142">Linux uživatele, chceme slyšet váš názor!</span><span class="sxs-lookup"><span data-stu-id="a24a0-142">Linux users, we want to hear from you!</span></span>

<span data-ttu-id="a24a0-143">Azure File storage pro skupiny uživatelů Linux poskytuje fórum můžete sdílet zpětnou vazbu, jak vyhodnotit a přijmout soubor úložiště v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="a24a0-143">The Azure File storage for Linux users' group provides a forum for you to share feedback as you evaluate and adopt File storage on Linux.</span></span> <span data-ttu-id="a24a0-144">E-mailu [Azure File storage Linux uživatelé](mailto:azurefileslinuxusers@microsoft.com) o připojení ke skupině uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a24a0-144">Email [Azure File storage Linux Users](mailto:azurefileslinuxusers@microsoft.com) to join the users' group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a24a0-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a24a0-145">Next steps</span></span>
<span data-ttu-id="a24a0-146">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="a24a0-146">See these links for more information about Azure File storage.</span></span>
* [<span data-ttu-id="a24a0-147">REST API služby File – referenční informace</span><span class="sxs-lookup"><span data-stu-id="a24a0-147">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [<span data-ttu-id="a24a0-148">Použití Azure PowerShell s Azure storage</span><span class="sxs-lookup"><span data-stu-id="a24a0-148">Using Azure PowerShell with Azure storage</span></span>](storage-powershell-guide-full.md)
* [<span data-ttu-id="a24a0-149">Použití nástroje AzCopy s Microsoft Azure storage</span><span class="sxs-lookup"><span data-stu-id="a24a0-149">How to use AzCopy with Microsoft Azure storage</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="a24a0-150">Použití Azure CLI s Azure storage</span><span class="sxs-lookup"><span data-stu-id="a24a0-150">Using the Azure CLI with Azure storage</span></span>](storage-azure-cli.md#create-and-manage-file-shares)
* [<span data-ttu-id="a24a0-151">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="a24a0-151">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="a24a0-152">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="a24a0-152">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)
