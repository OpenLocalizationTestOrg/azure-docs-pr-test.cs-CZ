---
title: aaaUse Azure File storage s Linuxem | Microsoft Docs
description: "Zjistěte, jak sdílet toomount soubor Azure prostřednictvím protokolu SMB v systému Linux."
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
ms.openlocfilehash: eeaa24b7f9e646724c5d86ae1e80dfdadaff34fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-file-storage-with-linux"></a><span data-ttu-id="d070a-103">Používání Azure File storage s Linuxem</span><span class="sxs-lookup"><span data-stu-id="d070a-103">Use Azure File storage with Linux</span></span>
<span data-ttu-id="d070a-104">[Úložiště Azure File](../storage-dotnet-how-to-use-files.md) je systém souborů cloudu snadno toouse společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d070a-104">[Azure File storage](../storage-dotnet-how-to-use-files.md) is Microsoft's easy toouse cloud file system.</span></span> <span data-ttu-id="d070a-105">Sdílené složky Azure může být připojen v Linuxových distribucích pomocí hello [cifs utils balíček](https://wiki.samba.org/index.php/LinuxCIFS_utils) z hello [Samba projektu](https://www.samba.org/).</span><span class="sxs-lookup"><span data-stu-id="d070a-105">Azure File shares can be mounted in Linux distributions using hello [cifs-utils package](https://wiki.samba.org/index.php/LinuxCIFS_utils) from hello [Samba project](https://www.samba.org/).</span></span> <span data-ttu-id="d070a-106">Tento článek ukazuje dva způsoby toomount sdílenou složku Azure File: na vyžádání pomocí hello `mount` příkazů a na spouštění pomocí vytváření položku v `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="d070a-106">This article shows two ways toomount an Azure File share: on-demand with hello `mount` command and on-boot by creating an entry in `/etc/fstab`.</span></span>

> [!NOTE]  
> <span data-ttu-id="d070a-107">V pořadí toomount Azure sdílenou mimo hello oblast Azure, který je hostovaný, například místně nebo v jiné oblasti Azure, hello operačního systému musí podporovat funkci hello šifrování protokolu SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="d070a-107">In order toomount an Azure File share outside of hello Azure region it is hosted in, such as on-premises or in a different Azure region, hello OS must support hello encryption functionality of SMB 3.0.</span></span> <span data-ttu-id="d070a-108">Šifrování funkce pro protokol SMB 3.0 pro Linux byla zavedena v 4.11 jádra.</span><span class="sxs-lookup"><span data-stu-id="d070a-108">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="d070a-109">Tato funkce umožňuje připojení Azure sdílené položky z místní nebo jiné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="d070a-109">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="d070a-110">V době hello publikování tato funkce byla tooUbuntu přeneseny zpět z 16.04 a vyšší.</span><span class="sxs-lookup"><span data-stu-id="d070a-110">At hello time of publishing, this functionality has been backported tooUbuntu from 16.04 and above.</span></span>


## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-hello-cifs-utils-package"></a><span data-ttu-id="d070a-111">Prerequisities pro připojení Azure sdílené složky pro Linux a hello cifs utils balíček</span><span class="sxs-lookup"><span data-stu-id="d070a-111">Prerequisities for mounting an Azure File share with Linux and hello cifs-utils package</span></span>
* <span data-ttu-id="d070a-112">**Vyberte distribuce systému Linux, která může mít hello cifs utils balíček nainstalován**: Společnost Microsoft doporučuje hello následující Linuxových distribucích v galerii Azure image hello:</span><span class="sxs-lookup"><span data-stu-id="d070a-112">**Pick a Linux distribution that can have hello cifs-utils package installed**: Microsoft recommends hello following Linux distributions in hello Azure image gallery:</span></span>

    * <span data-ttu-id="d070a-113">Ubuntu Server 14.04 +</span><span class="sxs-lookup"><span data-stu-id="d070a-113">Ubuntu Server 14.04+</span></span>
    * <span data-ttu-id="d070a-114">RHEL 7 +</span><span class="sxs-lookup"><span data-stu-id="d070a-114">RHEL 7+</span></span>
    * <span data-ttu-id="d070a-115">CentOS 7 +</span><span class="sxs-lookup"><span data-stu-id="d070a-115">CentOS 7+</span></span>
    * <span data-ttu-id="d070a-116">Debian 8</span><span class="sxs-lookup"><span data-stu-id="d070a-116">Debian 8</span></span>
    * <span data-ttu-id="d070a-117">openSUSE 13.2 +</span><span class="sxs-lookup"><span data-stu-id="d070a-117">openSUSE 13.2+</span></span>
    * <span data-ttu-id="d070a-118">SUSE Linux Enterprise Server 12</span><span class="sxs-lookup"><span data-stu-id="d070a-118">SUSE Linux Enterprise Server 12</span></span>

* <span data-ttu-id="d070a-119"><a id="install-cifs-utils"></a>**Hello cifs utils je nainstalovaný balíček**: hello cifs-utils můžete nainstalovat pomocí Správce balíčků hello hello distribucí Linux podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="d070a-119"><a id="install-cifs-utils"></a>**hello cifs-utils package is installed**: hello cifs-utils can be installed using hello package manager on hello Linux distribution of your choice.</span></span> 

    <span data-ttu-id="d070a-120">Na **Ubuntu** a **na základě Debian** distribuce, použijte hello `apt-get` Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="d070a-120">On **Ubuntu** and **Debian-based** distributions, use hello `apt-get` package manager:</span></span>

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    <span data-ttu-id="d070a-121">Na **RHEL** a **CentOS**, použijte hello `yum` Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="d070a-121">On **RHEL** and **CentOS**, use hello `yum` package manager:</span></span>

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    <span data-ttu-id="d070a-122">Na **openSUSE**, použijte hello `zypper` Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="d070a-122">On **openSUSE**, use hello `zypper` package manager:</span></span>

    ```
    sudo zypper install samba*
    ```

    <span data-ttu-id="d070a-123">Na další distribuce pomocí hello odpovídající package manager nebo [zkompilovat ze zdroje](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span><span class="sxs-lookup"><span data-stu-id="d070a-123">On other distributions, use hello appropriate package manager or [compile from source](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span></span>

* <span data-ttu-id="d070a-124">**Rozhodněte o oprávnění souboru nebo adresáře hello připojenou složku hello**: V následujících příkladech hello, použijeme 0777, toogive číst, zapisovat a spouštět uživatelé tooall oprávnění.</span><span class="sxs-lookup"><span data-stu-id="d070a-124">**Decide on hello directory/file permissions of hello mounted share**: In hello examples below, we use 0777, toogive read, write, and execute permissions tooall users.</span></span> <span data-ttu-id="d070a-125">Nahraďte ji s jinými [oprávnění chmod](https://en.wikipedia.org/wiki/Chmod) podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="d070a-125">You can replace it with other [chmod permissions](https://en.wikipedia.org/wiki/Chmod) as desired.</span></span> 

* <span data-ttu-id="d070a-126">**Název účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="d070a-126">**Storage Account Name**: toomount an Azure File share, you will need hello name of hello storage account.</span></span>

* <span data-ttu-id="d070a-127">**Klíč účtu úložiště**: sdílenou složku Azure File toomount, bude nutné hello klíč primární (nebo sekundární) úložiště.</span><span class="sxs-lookup"><span data-stu-id="d070a-127">**Storage Account Key**: toomount an Azure File share, you will need hello primary (or secondary) storage key.</span></span> <span data-ttu-id="d070a-128">Klíče SAS aktuálně nejsou pro připojení podporovány.</span><span class="sxs-lookup"><span data-stu-id="d070a-128">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="d070a-129">**Ujistěte se, je otevřený port 445**: SMB komunikuje přes port TCP 445 - zkontrolujte porty toosee, pokud brána firewall neblokuje TCP 445 z klientského počítače.</span><span class="sxs-lookup"><span data-stu-id="d070a-129">**Ensure port 445 is open**: SMB communicates over TCP port 445 - check toosee if your firewall is not blocking TCP ports 445 from client machine.</span></span>

## <a name="mount-hello-azure-file-share-on-demand-with-mount"></a><span data-ttu-id="d070a-130">Připojit hello Azure sdílená složka na vyžádání pomocí`mount`</span><span class="sxs-lookup"><span data-stu-id="d070a-130">Mount hello Azure File share on-demand with `mount`</span></span>
1. <span data-ttu-id="d070a-131">**[Instalovat balíček hello cifs utils pro Linux distribuční](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="d070a-131">**[Install hello cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="d070a-132">**Vytvořte složku pro hello přípojného bodu**: to můžete provést libovolné místo v systému souborů hello.</span><span class="sxs-lookup"><span data-stu-id="d070a-132">**Create a folder for hello mount point**: This can be done anywhere on hello file system.</span></span>

    ```
    mkdir mymountpoint
    ```

3. <span data-ttu-id="d070a-133">**Použití hello připojení příkaz toomount hello sdílenou složku Azure File**: Mějte na paměti, tooreplace `<storage-account-name>`, `<share-name>`, a `<storage-account-key>` hello správné informace.</span><span class="sxs-lookup"><span data-stu-id="d070a-133">**Use hello mount command toomount hello Azure File share**: Remember tooreplace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with hello proper information.</span></span>

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> <span data-ttu-id="d070a-134">Po dokončení pomocí hello sdílenou složku Azure, můžete použít `sudo umount ./mymountpoint` toounmount hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="d070a-134">When you are done using hello Azure File share, you may use `sudo umount ./mymountpoint` toounmount hello share.</span></span>

## <a name="create-a-persistent-mount-point-for-hello-azure-file-share-with-etcfstab"></a><span data-ttu-id="d070a-135">Vytvořit bod trvalé připojení pro sdílenou složku Azure File hello s`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="d070a-135">Create a persistent mount point for hello Azure File share with `/etc/fstab`</span></span>
1. <span data-ttu-id="d070a-136">**[Instalovat balíček hello cifs utils pro Linux distribuční](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="d070a-136">**[Install hello cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="d070a-137">**Vytvořte složku pro hello přípojného bodu**: to můžete provést libovolné místo v systému souborů hello, ale potřebujete toonote hello absolutní cestu složky pro hello.</span><span class="sxs-lookup"><span data-stu-id="d070a-137">**Create a folder for hello mount point**: This can be done anywhere on hello file system, but you need toonote hello absolute path of hello folder.</span></span> <span data-ttu-id="d070a-138">Hello následující ukázka vytvoří složku v kořenovém adresáři.</span><span class="sxs-lookup"><span data-stu-id="d070a-138">hello following example creates a folder under root.</span></span>

    ```
    sudo mkdir /mymountpoint
    ```

3. <span data-ttu-id="d070a-139">**Použití hello následující příkaz tooappend hello následující řádek příliš`/etc/fstab`**: Mějte na paměti, tooreplace `<storage-account-name>`, `<share-name>`, a `<storage-account-key>` hello správné informace.</span><span class="sxs-lookup"><span data-stu-id="d070a-139">**Use hello following command tooappend hello following line too`/etc/fstab`**: Remember tooreplace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with hello proper information.</span></span>

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> <span data-ttu-id="d070a-140">Můžete použít `sudo mount -a` sdílenou složku Azure File hello toomount po dokončení úprav `/etc/fstab` místo restartování.</span><span class="sxs-lookup"><span data-stu-id="d070a-140">You can use `sudo mount -a` toomount hello Azure File share after editing `/etc/fstab` instead of rebooting.</span></span>

## <a name="feedback"></a><span data-ttu-id="d070a-141">Váš názor</span><span class="sxs-lookup"><span data-stu-id="d070a-141">Feedback</span></span>
<span data-ttu-id="d070a-142">Uživatelé systému Linux, chceme toohear od vás!</span><span class="sxs-lookup"><span data-stu-id="d070a-142">Linux users, we want toohear from you!</span></span>

<span data-ttu-id="d070a-143">Hello Azure File storage pro skupiny uživatelů Linux poskytuje fórum pro vás zpětnou vazbu tooshare vyhodnotit a přijmout soubor úložiště v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="d070a-143">hello Azure File storage for Linux users' group provides a forum for you tooshare feedback as you evaluate and adopt File storage on Linux.</span></span> <span data-ttu-id="d070a-144">E-mailu [Azure File storage Linux uživatelé](mailto:azurefileslinuxusers@microsoft.com) skupiny toojoin hello uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d070a-144">Email [Azure File storage Linux Users](mailto:azurefileslinuxusers@microsoft.com) toojoin hello users' group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d070a-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d070a-145">Next steps</span></span>
<span data-ttu-id="d070a-146">Další informace o úložišti Azure File jsou dostupné na těchto odkazech.</span><span class="sxs-lookup"><span data-stu-id="d070a-146">See these links for more information about Azure File storage.</span></span>
* [<span data-ttu-id="d070a-147">REST API služby File – referenční informace</span><span class="sxs-lookup"><span data-stu-id="d070a-147">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [<span data-ttu-id="d070a-148">Jak toouse AzCopy s Microsoft Azure storage</span><span class="sxs-lookup"><span data-stu-id="d070a-148">How toouse AzCopy with Microsoft Azure storage</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [<span data-ttu-id="d070a-149">Použití hello rozhraní příkazového řádku Azure s Azure storage</span><span class="sxs-lookup"><span data-stu-id="d070a-149">Using hello Azure CLI with Azure storage</span></span>](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [<span data-ttu-id="d070a-150">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="d070a-150">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="d070a-151">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="d070a-151">Troubleshooting</span></span>](storage-troubleshoot-linux-file-connection-problems.md)
