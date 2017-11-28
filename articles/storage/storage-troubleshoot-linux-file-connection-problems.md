---
title: "aaaTroubleshoot Azure File storage problémy v systému Linux | Microsoft Docs"
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
ms.openlocfilehash: 3dc537d714244451a5ff8e01f4a27f03873cf2fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a><span data-ttu-id="0362a-103">Řešení potíží s Azure File storage problémy v systému Linux</span><span class="sxs-lookup"><span data-stu-id="0362a-103">Troubleshoot Azure File storage problems in Linux</span></span>

<span data-ttu-id="0362a-104">V tomto článku jsou uvedeny běžné problémy, které jsou související tooMicrosoft Azure File storage při připojení z klientů Linux.</span><span class="sxs-lookup"><span data-stu-id="0362a-104">This article lists common problems that are related tooMicrosoft Azure File storage when you connect from Linux clients.</span></span> <span data-ttu-id="0362a-105">Umožňuje také možné příčiny a řešení těchto problémů.</span><span class="sxs-lookup"><span data-stu-id="0362a-105">It also provides possible causes and resolutions for these problems.</span></span>

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-tooopen-a-file"></a><span data-ttu-id="0362a-106">"[bylo odepřeno oprávnění] byla překročena disková kvóta" když se pokusíte tooopen soubor</span><span class="sxs-lookup"><span data-stu-id="0362a-106">"[permission denied] Disk quota exceeded" when you try tooopen a file</span></span>

<span data-ttu-id="0362a-107">V systému Linux obdržíte chybovou zprávu, která se podobá následující hello:</span><span class="sxs-lookup"><span data-stu-id="0362a-107">In Linux, you receive an error message that resembles hello following:</span></span>

<span data-ttu-id="0362a-108">**<filename>[bylo odepřeno oprávnění] Byla překročena kvóta disku**</span><span class="sxs-lookup"><span data-stu-id="0362a-108">**<filename> [permission denied] Disk quota exceeded**</span></span>

### <a name="cause"></a><span data-ttu-id="0362a-109">Příčina</span><span class="sxs-lookup"><span data-stu-id="0362a-109">Cause</span></span>

<span data-ttu-id="0362a-110">Dosáhli jste hello horní limit počtu souběžných otevřete obslužných rutin, které jsou povoleny pro soubor.</span><span class="sxs-lookup"><span data-stu-id="0362a-110">You have reached hello upper limit of concurrent open handles that are allowed for a file.</span></span>

### <a name="solution"></a><span data-ttu-id="0362a-111">Řešení</span><span class="sxs-lookup"><span data-stu-id="0362a-111">Solution</span></span>

<span data-ttu-id="0362a-112">Snižte hello počet souběžných otevřených obslužných rutin ukončením některé obslužné rutiny a zopakujte operaci hello.</span><span class="sxs-lookup"><span data-stu-id="0362a-112">Reduce hello number of concurrent open handles by closing some handles, and then retry hello operation.</span></span> <span data-ttu-id="0362a-113">Další informace najdete v tématu [kontrolní seznam výkonu a škálovatelnosti služby Microsoft Azure Storage](storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="0362a-113">For more information, see [Microsoft Azure Storage performance and scalability checklist](storage-performance-checklist.md).</span></span>

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-linux"></a><span data-ttu-id="0362a-114">Pomalé souboru kopírování tooand z Azure File storage v systému Linux</span><span class="sxs-lookup"><span data-stu-id="0362a-114">Slow file copying tooand from Azure File storage in Linux</span></span>

-   <span data-ttu-id="0362a-115">Pokud nemáte konkrétní požadavky minimální velikost vstupně-výstupních operací, doporučujeme použít 1 MB, protože hello velikost vstupně-výstupní operace pro zajištění optimálního výkonu.</span><span class="sxs-lookup"><span data-stu-id="0362a-115">If you don’t have a specific minimum I/O size requirement, we recommend that you use 1 MB as hello I/O size for optimal performance.</span></span>
-   <span data-ttu-id="0362a-116">Pokud znáte hello konečné velikosti souboru, který bude rozšiřovat s použitím zápisy a váš software není zaznamenat problémy s kompatibilitou, když unwritten poškozené databáze za na hello soubor obsahuje nulami, nastavte velikost souboru hello předem místo provedení každém zápisu rozšíření zápis.</span><span class="sxs-lookup"><span data-stu-id="0362a-116">If you know hello final size of a file that you are extending by using writes, and your software doesn’t experience compatibility problems when an unwritten tail on hello file contains zeros, then set hello file size in advance instead of making every write an extending write.</span></span>
-   <span data-ttu-id="0362a-117">Použijte metodu pravé kopie hello:</span><span class="sxs-lookup"><span data-stu-id="0362a-117">Use hello right copy method:</span></span>
    -   <span data-ttu-id="0362a-118">Použití [AzCopy](storage-use-azcopy.md#file-copy) pro všechny přenosy mezi dvěma sdílenými složkami.</span><span class="sxs-lookup"><span data-stu-id="0362a-118">Use [AzCopy](storage-use-azcopy.md#file-copy) for any transfer between two file shares.</span></span>
    -   <span data-ttu-id="0362a-119">Použití [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mezi sdílené složky na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="0362a-119">Use [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) between file shares on an on-premises computer.</span></span>

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a><span data-ttu-id="0362a-120">"Připojit error(112): hostitel je dolů" z důvodu vypršení časového limitu opětovné připojení</span><span class="sxs-lookup"><span data-stu-id="0362a-120">"Mount error(112): Host is down" because of a reconnection time-out</span></span>

<span data-ttu-id="0362a-121">"112" připojení dojde k chybě v klientovi Linux hello hello klienta bylo nečinné po dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="0362a-121">A "112" mount error occurs on hello Linux client when hello client has been idle for a long time.</span></span> <span data-ttu-id="0362a-122">Po delší dobu nečinnosti hello klient odpojí a časový limit připojení hello.</span><span class="sxs-lookup"><span data-stu-id="0362a-122">After extended idle time, hello client disconnects and hello connection times out.</span></span>  

### <a name="cause"></a><span data-ttu-id="0362a-123">Příčina</span><span class="sxs-lookup"><span data-stu-id="0362a-123">Cause</span></span>

<span data-ttu-id="0362a-124">Hello připojení může být v nečinnosti hello následujících důvodů:</span><span class="sxs-lookup"><span data-stu-id="0362a-124">hello connection can be idle for hello following reasons:</span></span>

-   <span data-ttu-id="0362a-125">Selhání komunikace sítě, které zabraňují znovu zřízení serveru toohello připojení TCP, při použití možnosti "soft" připojení výchozí hello</span><span class="sxs-lookup"><span data-stu-id="0362a-125">Network communication failures that prevent re-establishing a TCP connection toohello server when hello default “soft” mount option is used</span></span>
-   <span data-ttu-id="0362a-126">Nejnovější opravy opětovné připojení, které se nenacházejí v starší jádra</span><span class="sxs-lookup"><span data-stu-id="0362a-126">Recent reconnection fixes that are not present in older kernels</span></span>

### <a name="solution"></a><span data-ttu-id="0362a-127">Řešení</span><span class="sxs-lookup"><span data-stu-id="0362a-127">Solution</span></span>

<span data-ttu-id="0362a-128">Tento problém opětovným připojením hello Linux jádra systému vyřešen nyní jako součást hello následující změny:</span><span class="sxs-lookup"><span data-stu-id="0362a-128">This reconnection problem in hello Linux kernel is now fixed as part of hello following changes:</span></span>

- [<span data-ttu-id="0362a-129">Oprava znovu připojit toonot odložení smb3 relace znovu připojit po obnovení připojení soketu</span><span class="sxs-lookup"><span data-stu-id="0362a-129">Fix reconnect toonot defer smb3 session reconnect long after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [<span data-ttu-id="0362a-130">Volání služby echo ihned po obnovení připojení soketu</span><span class="sxs-lookup"><span data-stu-id="0362a-130">Call echo service immediately after socket reconnect</span></span>](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [<span data-ttu-id="0362a-131">CIFS: Oprava poškození možné paměti při volání metody reconnect</span><span class="sxs-lookup"><span data-stu-id="0362a-131">CIFS: Fix a possible memory corruption during reconnect</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [<span data-ttu-id="0362a-132">CIFS: Opravte možné dvojité uzamykání mutex během opětovného připojení (pro v4.9 jádra a novější)</span><span class="sxs-lookup"><span data-stu-id="0362a-132">CIFS: Fix a possible double locking of mutex during reconnect (for kernel v4.9 and later)</span></span>](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

<span data-ttu-id="0362a-133">Tyto změny však nemusí přesně, ještě tooall hello Linuxových distribucích.</span><span class="sxs-lookup"><span data-stu-id="0362a-133">However, these changes might not be ported yet tooall hello Linux distributions.</span></span> <span data-ttu-id="0362a-134">Tato oprava a ostatní opravy opětovné připojení jsou vytvářeny v hello následující oblíbených jádra Linux: 4.4.40, 4.8.16 a 4.9.1.</span><span class="sxs-lookup"><span data-stu-id="0362a-134">This fix and other reconnection fixes are made in hello following popular Linux kernels: 4.4.40, 4.8.16, and 4.9.1.</span></span> <span data-ttu-id="0362a-135">Můžete získat tuto opravu upgradem tooone tyto verze doporučené jádra.</span><span class="sxs-lookup"><span data-stu-id="0362a-135">You can get this fix by upgrading tooone of these recommended kernel versions.</span></span>

### <a name="workaround"></a><span data-ttu-id="0362a-136">Alternativní řešení</span><span class="sxs-lookup"><span data-stu-id="0362a-136">Workaround</span></span>

<span data-ttu-id="0362a-137">Tento problém můžete vyřešit tak, že zadáte pevné připojení.</span><span class="sxs-lookup"><span data-stu-id="0362a-137">You can work around this problem by specifying a hard mount.</span></span> <span data-ttu-id="0362a-138">Dokud nebude navázáno připojení, nebo dokud ji explicitně přerušení a může být použité tooprevent chyb z důvodu vypršení časových limitů sítě vynutí toowait hello klienta.</span><span class="sxs-lookup"><span data-stu-id="0362a-138">This forces hello client toowait until a connection is established or until it’s explicitly interrupted and can be used tooprevent errors because of network time-outs.</span></span> <span data-ttu-id="0362a-139">Toto řešení však může způsobit neomezené čeká.</span><span class="sxs-lookup"><span data-stu-id="0362a-139">However, this workaround might cause indefinite waits.</span></span> <span data-ttu-id="0362a-140">Mít připravený toostop připojení v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="0362a-140">Be prepared toostop connections as necessary.</span></span>

<span data-ttu-id="0362a-141">Pokud nemůžete provést upgrade toohello nejnovější verze jádra, můžete tento problém obejít tím, že soubor v hello sdílenou složku Azure zápisu tooevery 30 sekund nebo méně.</span><span class="sxs-lookup"><span data-stu-id="0362a-141">If you cannot upgrade toohello latest kernel versions, you can work around this problem by keeping a file in hello Azure file share that you write tooevery 30 seconds or less.</span></span> <span data-ttu-id="0362a-142">Operace zápisu, musí se jednat například přepisování hello vytvořit nebo změnit data v souboru hello.</span><span class="sxs-lookup"><span data-stu-id="0362a-142">This must be a write operation, such as rewriting hello created or modified date on hello file.</span></span> <span data-ttu-id="0362a-143">Jinak může získat výsledky uložené v mezipaměti a operaci nemusí aktivovat hello opětovné připojení.</span><span class="sxs-lookup"><span data-stu-id="0362a-143">Otherwise, you might get cached results, and your operation might not trigger hello reconnection.</span></span>

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a><span data-ttu-id="0362a-144">"Připojit error(115): operace právě probíhá" při připojení Azure File storage pomocí protokolu SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="0362a-144">"Mount error(115): Operation now in progress" when you mount Azure File storage by using SMB 3.0</span></span>

### <a name="cause"></a><span data-ttu-id="0362a-145">Příčina</span><span class="sxs-lookup"><span data-stu-id="0362a-145">Cause</span></span>

<span data-ttu-id="0362a-146">Některých distribucích systému Linux zatím nepodporují funkcí šifrování v SMB 3.0 a uživatelé mohou zobrazit "115" chybová zpráva, pokud se uživatel pokusí toomount Azure File storage pomocí protokolu SMB 3.0 z důvodu chybějící funkce.</span><span class="sxs-lookup"><span data-stu-id="0362a-146">Some Linux distributions do not yet support encryption features in SMB 3.0 and users might receive a "115" error message if they try toomount Azure File storage by using SMB 3.0 because of a missing feature.</span></span>

### <a name="solution"></a><span data-ttu-id="0362a-147">Řešení</span><span class="sxs-lookup"><span data-stu-id="0362a-147">Solution</span></span>

<span data-ttu-id="0362a-148">Šifrování funkce pro protokol SMB 3.0 pro Linux byla zavedena v 4.11 jádra.</span><span class="sxs-lookup"><span data-stu-id="0362a-148">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="0362a-149">Tato funkce umožňuje připojení Azure sdílené položky z místní nebo jiné oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="0362a-149">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="0362a-150">V době hello publikování tato funkce byla přeneseny zpět tooUbuntu č. 17.04 a Ubuntu 16.10.</span><span class="sxs-lookup"><span data-stu-id="0362a-150">At hello time of publishing, this functionality has been backported tooUbuntu 17.04 and Ubuntu 16.10.</span></span> <span data-ttu-id="0362a-151">Pokud váš klient Linux SMB nepodporuje šifrování, připojit pomocí protokolu SMB 2.1 z Linux virtuálního počítače Azure, který je v Azure File storage hello stejném datovém centru jako hello účet úložiště souborů.</span><span class="sxs-lookup"><span data-stu-id="0362a-151">If your Linux SMB client does not support encryption, mount Azure File storage by using SMB 2.1 from an Azure Linux VM that's in hello same datacenter as hello File storage account.</span></span>

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a><span data-ttu-id="0362a-152">Nízký výkon na sdílenou složku Azure připojit na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="0362a-152">Slow performance on an Azure file share mounted on a Linux VM</span></span>

### <a name="cause"></a><span data-ttu-id="0362a-153">Příčina</span><span class="sxs-lookup"><span data-stu-id="0362a-153">Cause</span></span>

<span data-ttu-id="0362a-154">Možnou příčinou nízký výkon je zakázána, ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="0362a-154">One possible cause of slow performance is disabled caching.</span></span>

### <a name="solution"></a><span data-ttu-id="0362a-155">Řešení</span><span class="sxs-lookup"><span data-stu-id="0362a-155">Solution</span></span>

<span data-ttu-id="0362a-156">toocheck jestli ukládání do mezipaměti je zakázaná, vyhledejte hello **mezipaměti =** položku.</span><span class="sxs-lookup"><span data-stu-id="0362a-156">toocheck whether caching is disabled, look for hello **cache=** entry.</span></span> 

<span data-ttu-id="0362a-157">**Mezipaměť = none** označuje, že se zakáže ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="0362a-157">**Cache=none** indicates that caching is disabled.</span></span>  <span data-ttu-id="0362a-158">Sdílené složky hello opětovné připojení pomocí příkazu připojit výchozí hello nebo explicitně přidáním hello **mezipaměti = striktní** je povolená možnost toohello připojení příkaz tooensure, který je výchozí režim ukládání do mezipaměti nebo "striktní" ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="0362a-158">Remount hello share by using hello default mount command or by explicitly adding hello **cache=strict** option toohello mount command tooensure that default caching or "strict" caching mode is enabled.</span></span>

<span data-ttu-id="0362a-159">V některých scénářích hello **serverino** možnost připojení může způsobit hello **ls** stat toorun příkaz pro každou položku adresáře.</span><span class="sxs-lookup"><span data-stu-id="0362a-159">In some scenarios, hello **serverino** mount option can cause hello **ls** command toorun stat against every directory entry.</span></span> <span data-ttu-id="0362a-160">Toto chování výsledkem snížení výkonu, když jste výpis velký adresář.</span><span class="sxs-lookup"><span data-stu-id="0362a-160">This behavior results in performance degradation when you're listing a big directory.</span></span> <span data-ttu-id="0362a-161">Možnosti připojení hello můžete zkontrolovat ve vaší **/etc/fstab** položku:</span><span class="sxs-lookup"><span data-stu-id="0362a-161">You can check hello mount options in your **/etc/fstab** entry:</span></span>

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

<span data-ttu-id="0362a-162">Můžete také zkontrolovat, zda se správnými možnostmi hello používají spuštěním hello **sudo připojení | grep cifs** příkazu a kontrola její výstup, jako je například hello následující příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="0362a-162">You can also check whether hello correct options are being used by running hello  **sudo mount | grep cifs** command and checking its output, such as hello following example output:</span></span>

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

<span data-ttu-id="0362a-163">Pokud hello **mezipaměti = striktní** nebo **serverino** možnost je k dispozici, odpojte a znovu připojit Azure File storage spuštěním příkazu hello připojení z hello [dokumentaci](storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="0362a-163">If hello **cache=strict** or **serverino** option is not present, unmount and mount Azure File storage again by running hello mount command from hello [documentation](storage-how-to-use-files-linux.md).</span></span> <span data-ttu-id="0362a-164">Potom spusťte opětovnou kontrolu této hello **/etc/fstab** položka má hello správnými možnostmi.</span><span class="sxs-lookup"><span data-stu-id="0362a-164">Then, recheck that hello **/etc/fstab** entry has hello correct options.</span></span>

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-toolinux"></a><span data-ttu-id="0362a-165">Časová razítka ztratily v kopírování souborů z Windows tooLinux</span><span class="sxs-lookup"><span data-stu-id="0362a-165">Time stamps were lost in copying files from Windows tooLinux</span></span>

<span data-ttu-id="0362a-166">Na platformách, Linux a Unix, hello **cp -p** příkaz se nezdaří, pokud soubor 1 a 2 jsou vlastněny různé uživatele.</span><span class="sxs-lookup"><span data-stu-id="0362a-166">On Linux/Unix platforms, hello **cp -p** command fails if file 1 and file 2 are owned by different users.</span></span>

### <a name="cause"></a><span data-ttu-id="0362a-167">Příčina</span><span class="sxs-lookup"><span data-stu-id="0362a-167">Cause</span></span>

<span data-ttu-id="0362a-168">Hello příznak force **f** v COPYFILE výsledkem provádění **cp -p -f** v systému Unix.</span><span class="sxs-lookup"><span data-stu-id="0362a-168">hello force flag **f** in COPYFILE results in executing **cp -p -f** on Unix.</span></span> <span data-ttu-id="0362a-169">Tento příkaz se nezdaří ani toopreserve hello časové razítko hello souboru, kterou nevlastníte.</span><span class="sxs-lookup"><span data-stu-id="0362a-169">This command also fails toopreserve hello time stamp of hello file that you don't own.</span></span>

### <a name="workaround"></a><span data-ttu-id="0362a-170">Alternativní řešení</span><span class="sxs-lookup"><span data-stu-id="0362a-170">Workaround</span></span>

<span data-ttu-id="0362a-171">Použijte hello uživatelský účet úložiště pro kopírování souborů hello:</span><span class="sxs-lookup"><span data-stu-id="0362a-171">Use hello storage account user for copying hello files:</span></span>

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="need-help-contact-support"></a><span data-ttu-id="0362a-172">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="0362a-172">Need help?</span></span> <span data-ttu-id="0362a-173">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="0362a-173">Contact support.</span></span>

<span data-ttu-id="0362a-174">Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit váš problém.</span><span class="sxs-lookup"><span data-stu-id="0362a-174">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your problem resolved quickly.</span></span>
