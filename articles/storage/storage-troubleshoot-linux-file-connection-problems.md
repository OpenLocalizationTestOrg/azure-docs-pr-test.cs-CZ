---
title: "Řešení potíží s Azure File storage problémy v systému Linux | Microsoft Docs"
description: "Řešení potíží s Azure File storage problémy v systému Linux"
services: storage
documentationcenter: 
author: genlin
manager: willchen
editor: na
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: genli
ms.openlocfilehash: 62cd62ec3a2900f06acacc0852a48b5e3ff1c8cd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a><span data-ttu-id="15d53-103">Řešení potíží s Azure File storage problémy v systému Linux</span><span class="sxs-lookup"><span data-stu-id="15d53-103">Troubleshoot Azure File storage problems in Linux</span></span>

<span data-ttu-id="15d53-104">V tomto článku jsou uvedeny běžné problémy, které se vztahují k Microsoft Azure File storage při připojení z klientů Linux.</span><span class="sxs-lookup"><span data-stu-id="15d53-104">This article lists common problems that are related to Microsoft Azure File storage when you connect from Linux clients.</span></span> <span data-ttu-id="15d53-105">Umožňuje také možné příčiny a řešení těchto problémů.</span><span class="sxs-lookup"><span data-stu-id="15d53-105">It also provides possible causes and resolutions for these problems.</span></span>

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a><span data-ttu-id="15d53-106">"[bylo odepřeno oprávnění] byla překročena disková kvóta" při pokusu o otevření souboru</span><span class="sxs-lookup"><span data-stu-id="15d53-106">"[permission denied] Disk quota exceeded" when you try to open a file</span></span>

<span data-ttu-id="15d53-107">V systému Linux obdržíte chybovou zprávu, která vypadá přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="15d53-107">In Linux, you receive an error message that resembles the following:</span></span>

<span data-ttu-id="15d53-108">**<filename>[bylo odepřeno oprávnění] Byla překročena kvóta disku**</span><span class="sxs-lookup"><span data-stu-id="15d53-108">**<filename> [permission denied] Disk quota exceeded**</span></span>

### <a name="cause"></a><span data-ttu-id="15d53-109">Příčina</span><span class="sxs-lookup"><span data-stu-id="15d53-109">Cause</span></span>

<span data-ttu-id="15d53-110">Bylo dosaženo horní limit počtu souběžných otevřete obslužných rutin, které jsou povoleny pro soubor.</span><span class="sxs-lookup"><span data-stu-id="15d53-110">You have reached the upper limit of concurrent open handles that are allowed for a file.</span></span>

### <a name="solution"></a><span data-ttu-id="15d53-111">Řešení</span><span class="sxs-lookup"><span data-stu-id="15d53-111">Solution</span></span>

<span data-ttu-id="15d53-112">Snižte počet souběžných otevřených obslužných rutin ukončením některé obslužné rutiny a potom operaci opakujte.</span><span class="sxs-lookup"><span data-stu-id="15d53-112">Reduce the number of concurrent open handles by closing some handles, and then retry the operation.</span></span> <span data-ttu-id="15d53-113">Další informace najdete v tématu [kontrolní seznam výkonu a škálovatelnosti služby Microsoft Azure Storage](storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="15d53-113">For more information, see [Microsoft Azure Storage performance and scalability checklist](storage-performance-checklist.md).</span></span>

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-file-storage-in-linux"></a><span data-ttu-id="15d53-114">Pomalé kopírování souborů do a z Azure File storage v systému Linux</span><span class="sxs-lookup"><span data-stu-id="15d53-114">Slow file copying to and from Azure File storage in Linux</span></span>

-   <span data-ttu-id="15d53-115">Pokud nemáte konkrétní požadavky minimální velikost vstupně-výstupních operací, doporučujeme použít 1 MB jako velikost vstupně-výstupní operace pro zajištění optimálního výkonu.</span><span class="sxs-lookup"><span data-stu-id="15d53-115">If you don’t have a specific minimum I/O size requirement, we recommend that you use 1 MB as the I/O size for optimal performance.</span></span>
-   <span data-ttu-id="15d53-116">Pokud znáte konečné velikosti souboru, který bude rozšiřovat s použitím zápisy a váš software nebude zaznamenat problémy s kompatibilitou, při unwritten poškozené databáze za na soubor obsahuje nuly, potom nastavte velikost souboru předem místo provedení každém zápisu rozšíření zápisu.</span><span class="sxs-lookup"><span data-stu-id="15d53-116">If you know the final size of a file that you are extending by using writes, and your software doesn’t experience compatibility problems when an unwritten tail on the file contains zeros, then set the file size in advance instead of making every write an extending write.</span></span>
-   <span data-ttu-id="15d53-117">Použijte metodu pravé kopie:</span><span class="sxs-lookup"><span data-stu-id="15d53-117">Use the right copy method:</span></span>
    -   <span data-ttu-id="15d53-118">Použití [AzCopy](storage-use-azcopy.md#file-copy) pro všechny přenosy mezi dvěma sdílenými složkami.</span><span class="sxs-lookup"><span data-stu-id="15d53-118">Use [AzCopy](storage-use-azcopy.md#file-copy) for any transfer between two file shares.</span></span>
    -   <span data-ttu-id="15d53-119">Použití [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mezi sdílené složky na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="15d53-119">Use [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) between file shares on an on-premises computer.</span></span>

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a><span data-ttu-id="15d53-120">"Připojit error(112): hostitel je dolů" z důvodu vypršení časového limitu opětovné připojení</span><span class="sxs-lookup"><span data-stu-id="15d53-120">"Mount error(112): Host is down" because of a reconnection time-out</span></span>

<span data-ttu-id="15d53-121">"112" připojení dojde k chybě na straně klienta Linux klienta bylo nečinné po dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="15d53-121">A "112" mount error occurs on the Linux client when the client has been idle for a long time.</span></span> <span data-ttu-id="15d53-122">Po delší dobu nečinnosti se klient neodpojí a časový limit připojení.</span><span class="sxs-lookup"><span data-stu-id="15d53-122">After extended idle time, the client disconnects and the connection times out.</span></span>  

### <a name="cause"></a><span data-ttu-id="15d53-123">Příčina</span><span class="sxs-lookup"><span data-stu-id="15d53-123">Cause</span></span>

<span data-ttu-id="15d53-124">Připojení může být nečinnosti z následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="15d53-124">The connection can be idle for the following reasons:</span></span>

-   <span data-ttu-id="15d53-125">Selhání komunikace sítě, které zabraňují znovu navázání připojení TCP k serveru, pokud je použita výchozí možnost "soft" připojení</span><span class="sxs-lookup"><span data-stu-id="15d53-125">Network communication failures that prevent re-establishing a TCP connection to the server when the default “soft” mount option is used</span></span>
-   <span data-ttu-id="15d53-126">Nejnovější opravy opětovné připojení, které se nenacházejí v starší jádra</span><span class="sxs-lookup"><span data-stu-id="15d53-126">Recent reconnection fixes that are not present in older kernels</span></span>

### <a name="solution"></a><span data-ttu-id="15d53-127">Řešení</span><span class="sxs-lookup"><span data-stu-id="15d53-127">Solution</span></span>

<span data-ttu-id="15d53-128">Tento problém opětovné připojení v systému Linux jádra vyřešen nyní jako součást následující změny:</span><span class="sxs-lookup"><span data-stu-id="15d53-128">This reconnection problem in the Linux kernel is now fixed as part of the following changes:</span></span>

- [<span data-ttu-id="15d53-129">Oprava připojení není odložení smb3 relace znovu připojit po obnovení připojení soketu</span><span class="sxs-lookup"><span data-stu-id="15d53-129">Fix reconnect to not defer smb3 session reconnect long after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [<span data-ttu-id="15d53-130">Volání služby echo ihned po obnovení připojení soketu</span><span class="sxs-lookup"><span data-stu-id="15d53-130">Call echo service immediately after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [<span data-ttu-id="15d53-131">CIFS: Oprava poškození možné paměti při volání metody reconnect</span><span class="sxs-lookup"><span data-stu-id="15d53-131">CIFS: Fix a possible memory corruption during reconnect</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [<span data-ttu-id="15d53-132">CIFS: Opravte možné dvojité uzamykání mutex během opětovného připojení (pro v4.9 jádra a novější)</span><span class="sxs-lookup"><span data-stu-id="15d53-132">CIFS: Fix a possible double locking of mutex during reconnect (for kernel v4.9 and later)</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

<span data-ttu-id="15d53-133">Ale tyto změny nemusí být přesně ještě do Linuxových distribucích.</span><span class="sxs-lookup"><span data-stu-id="15d53-133">However, these changes might not be ported yet to all the Linux distributions.</span></span> <span data-ttu-id="15d53-134">Tato oprava a ostatní opravy opětovné připojení jsou vytvářeny v jádrech následující oblíbených Linux: 4.4.40, 4.8.16 a 4.9.1.</span><span class="sxs-lookup"><span data-stu-id="15d53-134">This fix and other reconnection fixes are made in the following popular Linux kernels: 4.4.40, 4.8.16, and 4.9.1.</span></span> <span data-ttu-id="15d53-135">Tato oprava získáte po upgradu na jednu z těchto verzí doporučené jádra.</span><span class="sxs-lookup"><span data-stu-id="15d53-135">You can get this fix by upgrading to one of these recommended kernel versions.</span></span>

### <a name="workaround"></a><span data-ttu-id="15d53-136">Alternativní řešení</span><span class="sxs-lookup"><span data-stu-id="15d53-136">Workaround</span></span>

<span data-ttu-id="15d53-137">Tento problém můžete vyřešit tak, že zadáte pevné připojení.</span><span class="sxs-lookup"><span data-stu-id="15d53-137">You can work around this problem by specifying a hard mount.</span></span> <span data-ttu-id="15d53-138">Vynutí se klient čekat, dokud nebude navázáno připojení, nebo dokud ji explicitně přerušení a je možné zabránit chybám z důvodu vypršení časových limitů sítě.</span><span class="sxs-lookup"><span data-stu-id="15d53-138">This forces the client to wait until a connection is established or until it’s explicitly interrupted and can be used to prevent errors because of network time-outs.</span></span> <span data-ttu-id="15d53-139">Toto řešení však může způsobit neomezené čeká.</span><span class="sxs-lookup"><span data-stu-id="15d53-139">However, this workaround might cause indefinite waits.</span></span> <span data-ttu-id="15d53-140">Připravte se na zastavení připojení v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="15d53-140">Be prepared to stop connections as necessary.</span></span>

<span data-ttu-id="15d53-141">Pokud nemůžete provést upgrade na nejnovější verze jádra, můžete tento problém obejít tím, že soubor v Azure sdílené složky, zapisovat na každých 30 sekund nebo méně.</span><span class="sxs-lookup"><span data-stu-id="15d53-141">If you cannot upgrade to the latest kernel versions, you can work around this problem by keeping a file in the Azure file share that you write to every 30 seconds or less.</span></span> <span data-ttu-id="15d53-142">Toto musí být operace zápisu, jako je například přepisování vytvořené nebo upravené datum v souboru.</span><span class="sxs-lookup"><span data-stu-id="15d53-142">This must be a write operation, such as rewriting the created or modified date on the file.</span></span> <span data-ttu-id="15d53-143">Jinak může získat výsledky uložené v mezipaměti a operaci nemusí spustit obnovení připojení.</span><span class="sxs-lookup"><span data-stu-id="15d53-143">Otherwise, you might get cached results, and your operation might not trigger the reconnection.</span></span>

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a><span data-ttu-id="15d53-144">"Připojit error(115): operace právě probíhá" při připojení Azure File storage pomocí protokolu SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="15d53-144">"Mount error(115): Operation now in progress" when you mount Azure File storage by using SMB 3.0</span></span>

### <a name="cause"></a><span data-ttu-id="15d53-145">Příčina</span><span class="sxs-lookup"><span data-stu-id="15d53-145">Cause</span></span>

<span data-ttu-id="15d53-146">Některé Linuxových distribucích zatím nepodporují funkcí šifrování v SMB 3.0 a uživatelé se může zobrazit zprávu "115" Chyba při pokusu o připojení Azure File storage pomocí protokolu SMB 3.0 z důvodu chybějící funkce.</span><span class="sxs-lookup"><span data-stu-id="15d53-146">Some Linux distributions do not yet support encryption features in SMB 3.0 and users might receive a "115" error message if they try to mount Azure File storage by using SMB 3.0 because of a missing feature.</span></span>

### <a name="solution"></a><span data-ttu-id="15d53-147">Řešení</span><span class="sxs-lookup"><span data-stu-id="15d53-147">Solution</span></span>

<span data-ttu-id="15d53-148">Šifrování funkce pro protokol SMB 3.0 pro Linux byla zavedena v 4.11 jádra.</span><span class="sxs-lookup"><span data-stu-id="15d53-148">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="15d53-149">Tato funkce umožňuje připojení Azure sdílené položky z místní nebo jiné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="15d53-149">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="15d53-150">V době publikování tato funkce byla přeneseny zpět do č. 17.04 Ubuntu a Ubuntu 16.10.</span><span class="sxs-lookup"><span data-stu-id="15d53-150">At the time of publishing, this functionality has been backported to Ubuntu 17.04 and Ubuntu 16.10.</span></span> <span data-ttu-id="15d53-151">Pokud váš klient Linux SMB nepodporuje šifrování, připojte pomocí protokolu SMB 2.1 z Linux virtuálního počítače Azure, který je ve stejném datovém centru jako soubor účet úložiště Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="15d53-151">If your Linux SMB client does not support encryption, mount Azure File storage by using SMB 2.1 from an Azure Linux VM that's in the same datacenter as the File storage account.</span></span>

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a><span data-ttu-id="15d53-152">Nízký výkon na sdílenou složku Azure připojit na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="15d53-152">Slow performance on an Azure file share mounted on a Linux VM</span></span>

### <a name="cause"></a><span data-ttu-id="15d53-153">Příčina</span><span class="sxs-lookup"><span data-stu-id="15d53-153">Cause</span></span>

<span data-ttu-id="15d53-154">Možnou příčinou nízký výkon je zakázána, ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="15d53-154">One possible cause of slow performance is disabled caching.</span></span>

### <a name="solution"></a><span data-ttu-id="15d53-155">Řešení</span><span class="sxs-lookup"><span data-stu-id="15d53-155">Solution</span></span>

<span data-ttu-id="15d53-156">Chcete-li zkontrolovat, zda je zakáže ukládání do mezipaměti, vyhledejte **mezipaměti =** položku.</span><span class="sxs-lookup"><span data-stu-id="15d53-156">To check whether caching is disabled, look for the **cache=** entry.</span></span> 

<span data-ttu-id="15d53-157">**Mezipaměť = none** označuje, že se zakáže ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="15d53-157">**Cache=none** indicates that caching is disabled.</span></span>  <span data-ttu-id="15d53-158">Pomocí příkazu výchozí připojení nebo explicitně přidáním znovu připojit sdílenou složku **mezipaměti = striktní** je povolena možnost příkazu připojit k zajištění, že ukládání do mezipaměti výchozí nebo "striktní" režim ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="15d53-158">Remount the share by using the default mount command or by explicitly adding the **cache=strict** option to the mount command to ensure that default caching or "strict" caching mode is enabled.</span></span>

<span data-ttu-id="15d53-159">V některých případech **serverino** možnost připojení může způsobit **ls** příkaz ke spuštění stat pro každou položku adresáře.</span><span class="sxs-lookup"><span data-stu-id="15d53-159">In some scenarios, the **serverino** mount option can cause the **ls** command to run stat against every directory entry.</span></span> <span data-ttu-id="15d53-160">Toto chování výsledkem snížení výkonu, když jste výpis velký adresář.</span><span class="sxs-lookup"><span data-stu-id="15d53-160">This behavior results in performance degradation when you're listing a big directory.</span></span> <span data-ttu-id="15d53-161">Možnosti připojení můžete zkontrolovat ve vaší **/etc/fstab** položku:</span><span class="sxs-lookup"><span data-stu-id="15d53-161">You can check the mount options in your **/etc/fstab** entry:</span></span>

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

<span data-ttu-id="15d53-162">Můžete také zkontrolovat, zda se správnými možnostmi jsou používány spuštěním **sudo připojení | grep cifs** příkazu a kontrola její výstup, jako je například výstupu v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="15d53-162">You can also check whether the correct options are being used by running the  **sudo mount | grep cifs** command and checking its output, such as the following example output:</span></span>

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

<span data-ttu-id="15d53-163">Pokud **mezipaměti = striktní** nebo **serverino** možnost je k dispozici, odpojte a znovu připojit Azure File storage spuštěním příkazu připojení z [dokumentaci](storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="15d53-163">If the **cache=strict** or **serverino** option is not present, unmount and mount Azure File storage again by running the mount command from the [documentation](storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="15d53-164">Potom znovu zkontrolovat, **/etc/fstab** položka má se správnými možnostmi.</span><span class="sxs-lookup"><span data-stu-id="15d53-164">Then, recheck that the **/etc/fstab** entry has the correct options.</span></span>

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a><span data-ttu-id="15d53-165">Časová razítka ztratily v kopírování souborů ze systému Windows do systému Linux</span><span class="sxs-lookup"><span data-stu-id="15d53-165">Time stamps were lost in copying files from Windows to Linux</span></span>

<span data-ttu-id="15d53-166">Na platformách, Linux a Unix **cp -p** příkaz se nezdaří, pokud soubor 1 a 2 jsou vlastněny různé uživatele.</span><span class="sxs-lookup"><span data-stu-id="15d53-166">On Linux/Unix platforms, the **cp -p** command fails if file 1 and file 2 are owned by different users.</span></span>

### <a name="cause"></a><span data-ttu-id="15d53-167">Příčina</span><span class="sxs-lookup"><span data-stu-id="15d53-167">Cause</span></span>

<span data-ttu-id="15d53-168">Příznak force **f** v COPYFILE výsledkem provádění **cp -p -f** v systému Unix.</span><span class="sxs-lookup"><span data-stu-id="15d53-168">The force flag **f** in COPYFILE results in executing **cp -p -f** on Unix.</span></span> <span data-ttu-id="15d53-169">Tento příkaz taky nedaří zachovat časové razítko souboru, kterou nevlastníte.</span><span class="sxs-lookup"><span data-stu-id="15d53-169">This command also fails to preserve the time stamp of the file that you don't own.</span></span>

### <a name="workaround"></a><span data-ttu-id="15d53-170">Alternativní řešení</span><span class="sxs-lookup"><span data-stu-id="15d53-170">Workaround</span></span>

<span data-ttu-id="15d53-171">Použijte uživatelský účet úložiště pro kopírování souborů:</span><span class="sxs-lookup"><span data-stu-id="15d53-171">Use the storage account user for copying the files:</span></span>

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="need-help-contact-support"></a><span data-ttu-id="15d53-172">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="15d53-172">Need help?</span></span> <span data-ttu-id="15d53-173">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="15d53-173">Contact support.</span></span>

<span data-ttu-id="15d53-174">Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="15d53-174">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your problem resolved quickly.</span></span>
