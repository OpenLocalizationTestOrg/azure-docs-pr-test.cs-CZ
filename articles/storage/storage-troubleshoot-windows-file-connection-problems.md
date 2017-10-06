---
title: "aaaTroubleshoot Azure File storage problémy v systému Windows | Microsoft Docs"
description: "Řešení potíží s Azure File storage problémy v systému Windows"
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
ms.date: 06/28/2017
ms.author: genli
ms.openlocfilehash: ba0315d1add76a27fec93a9aee3aa99ccb7fa164
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-windows"></a><span data-ttu-id="e2021-103">Poradce při potížích úložiště Azure File ve Windows</span><span class="sxs-lookup"><span data-stu-id="e2021-103">Troubleshoot Azure File storage problems in Windows</span></span>

<span data-ttu-id="e2021-104">V tomto článku jsou uvedeny běžné problémy, které jsou související tooMicrosoft Azure File storage při připojení klientů se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="e2021-104">This article lists common problems that are related tooMicrosoft Azure File storage when you connect from Windows clients.</span></span> <span data-ttu-id="e2021-105">Umožňuje také možné příčiny a řešení těchto problémů.</span><span class="sxs-lookup"><span data-stu-id="e2021-105">It also provides possible causes and resolutions for these problems.</span></span> <span data-ttu-id="e2021-106">Kromě toho toohello řešení potíží s kroky v tomto článku, můžete také použít [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) zajistit, že Windows hello klienta prostředí má správné požadavky.</span><span class="sxs-lookup"><span data-stu-id="e2021-106">In addition toohello troubleshooting steps in this article, you can also use [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) to ensure that hello Windows client environment has correct prerequisites.</span></span> <span data-ttu-id="e2021-107">AzFileDiagnostics automatizuje detekce většinu hello příznaků uvedených v tomto článku a pomáhá nastavení vašeho prostředí tooget optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="e2021-107">AzFileDiagnostics automates detection of most of hello symptoms mentioned in this article and helps set up your environment tooget optimal performance.</span></span> <span data-ttu-id="e2021-108">Tyto informace můžete také najít v hello [Azure sdíleným Poradce při potížích s](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) poskytující tooassist kroky můžete s problémy sdílí soubory Azure připojení, mapování/připojení.</span><span class="sxs-lookup"><span data-stu-id="e2021-108">You can also find this information in hello [Azure Files shares Troubleshooter](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) that provides steps tooassist you with problems connecting/mapping/mounting Azure Files shares.</span></span>


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a><span data-ttu-id="e2021-109">Chyba 53, chyba 67 nebo 87 Chyba při připojení nebo odpojení sdílenou složku Azure</span><span class="sxs-lookup"><span data-stu-id="e2021-109">Error 53, Error 67, or Error 87 when you mount or unmount an Azure file share</span></span>

<span data-ttu-id="e2021-110">Pokusíte-li toomount sdílenou z místní nebo jiném datovém centru, může se zobrazit hello následujícím chybám:</span><span class="sxs-lookup"><span data-stu-id="e2021-110">When you try toomount a file share from on-premises or from a different datacenter, you might receive hello following errors:</span></span>

- <span data-ttu-id="e2021-111">Došlo k systémové chybě 53.</span><span class="sxs-lookup"><span data-stu-id="e2021-111">System error 53 has occurred.</span></span> <span data-ttu-id="e2021-112">Hello síťová cesta nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="e2021-112">hello network path was not found.</span></span>
- <span data-ttu-id="e2021-113">Došlo k systémové chybě 67.</span><span class="sxs-lookup"><span data-stu-id="e2021-113">System error 67 has occurred.</span></span> <span data-ttu-id="e2021-114">Název sítě Hello nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="e2021-114">hello network name cannot be found.</span></span>
- <span data-ttu-id="e2021-115">Došlo k systémové chybě 87.</span><span class="sxs-lookup"><span data-stu-id="e2021-115">System error 87 has occurred.</span></span> <span data-ttu-id="e2021-116">Hello parametr je nesprávný.</span><span class="sxs-lookup"><span data-stu-id="e2021-116">hello parameter is incorrect.</span></span>

### <a name="cause-1-unencrypted-communication-channel"></a><span data-ttu-id="e2021-117">Příčina 1: Nezašifrované komunikační kanál</span><span class="sxs-lookup"><span data-stu-id="e2021-117">Cause 1: Unencrypted communication channel</span></span>

<span data-ttu-id="e2021-118">Připojení tooAzure sdílené složky jsou zablokované, pokud není šifrován hello komunikační kanál, a pokud se pokus o připojení hello není z bezpečnostních důvodů hello stejného datového centra, kde jsou umístěny hello sdílené složky Azure.</span><span class="sxs-lookup"><span data-stu-id="e2021-118">For security reasons, connections tooAzure file shares are blocked if hello communication channel isn’t encrypted and if hello connection attempt isn't made from hello same datacenter where hello Azure file shares reside.</span></span> <span data-ttu-id="e2021-119">Šifrování kanálu komunikace je k dispozici pouze v případě, že hello uživatele klientského operačního systému podporuje šifrování protokolu SMB.</span><span class="sxs-lookup"><span data-stu-id="e2021-119">Communication channel encryption is provided only if hello user’s client OS supports SMB encryption.</span></span>

<span data-ttu-id="e2021-120">Windows 8, Windows Server 2012 a novějších verzích každý systém vyjednávat požadavků, které zahrnují protokolu SMB 3.0, který podporuje šifrování.</span><span class="sxs-lookup"><span data-stu-id="e2021-120">Windows 8, Windows Server 2012, and later versions of each system negotiate requests that include SMB 3.0, which supports encryption.</span></span>

### <a name="solution-for-cause-1"></a><span data-ttu-id="e2021-121">Řešení pro příčina 1</span><span class="sxs-lookup"><span data-stu-id="e2021-121">Solution for cause 1</span></span>

<span data-ttu-id="e2021-122">Připojení z klienta, který provede některou z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="e2021-122">Connect from a client that does one of hello following:</span></span>

- <span data-ttu-id="e2021-123">Splňuje požadavky hello Windows 8 a Windows Server 2012 nebo novější verze</span><span class="sxs-lookup"><span data-stu-id="e2021-123">Meets hello requirements of Windows 8 and Windows Server 2012 or later versions</span></span>
- <span data-ttu-id="e2021-124">Připojí z virtuálního počítače v hello stejné datovém centru jako hello účtu úložiště Azure, který se používá pro sdílenou složku hello Azure</span><span class="sxs-lookup"><span data-stu-id="e2021-124">Connects from a virtual machine in hello same datacenter as hello Azure storage account that is used for hello Azure file share</span></span>

### <a name="cause-2-port-445-is-blocked"></a><span data-ttu-id="e2021-125">2 příčina: Port 445 je blokován.</span><span class="sxs-lookup"><span data-stu-id="e2021-125">Cause 2: Port 445 is blocked</span></span>

<span data-ttu-id="e2021-126">Systémová chyba 53 nebo systémové chybě 67 může dojít, pokud je blokovaný port 445 odchozí komunikaci tooan Azure File storage datacenter.</span><span class="sxs-lookup"><span data-stu-id="e2021-126">System error 53 or system error 67 can occur if port 445 outbound communication tooan Azure File storage datacenter is blocked.</span></span> <span data-ttu-id="e2021-127">Souhrn hello toosee poskytovatelů internetových služeb, které povolí nebo zakáže přístup z port 445, přejděte příliš[TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).</span><span class="sxs-lookup"><span data-stu-id="e2021-127">toosee hello summary of ISPs that allow or disallow access from port 445, go too[TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).</span></span>

<span data-ttu-id="e2021-128">toounderstand ať to je důvod hello za zpráva "Chyba systému 53" hello, můžete použít Portqry tooquery hello TCP:445 koncový bod.</span><span class="sxs-lookup"><span data-stu-id="e2021-128">toounderstand whether this is hello reason behind hello "System error 53" message, you can use Portqry tooquery hello TCP:445 endpoint.</span></span> <span data-ttu-id="e2021-129">Pokud koncový bod TCP:445 hello se zobrazí jako filtrované, hello TCP port je blokován.</span><span class="sxs-lookup"><span data-stu-id="e2021-129">If hello TCP:445 endpoint is displayed as filtered, hello TCP port is blocked.</span></span> <span data-ttu-id="e2021-130">Tady je příklad dotazu:</span><span class="sxs-lookup"><span data-stu-id="e2021-130">Here is an example query:</span></span>

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

<span data-ttu-id="e2021-131">Pokud TCP port 445 je blokován pravidlem podél hello síťovou cestu, zobrazí se následující výstup hello:</span><span class="sxs-lookup"><span data-stu-id="e2021-131">If TCP port 445 is blocked by a rule along hello network path, you will see hello following output:</span></span>

  `TCP port 445 (microsoft-ds service): FILTERED`

<span data-ttu-id="e2021-132">Další informace o tom, najdete v části toouse Portqry, [Popis nástroje příkazového řádku Portqry.exe hello](https://support.microsoft.com/help/310099).</span><span class="sxs-lookup"><span data-stu-id="e2021-132">For more information about how toouse Portqry, see [Description of hello Portqry.exe command-line utility](https://support.microsoft.com/help/310099).</span></span>

### <a name="solution-for-cause-2"></a><span data-ttu-id="e2021-133">Řešení pro příčina 2</span><span class="sxs-lookup"><span data-stu-id="e2021-133">Solution for cause 2</span></span>

<span data-ttu-id="e2021-134">Práce s vaše IT oddělení tooopen port 445 odchozí příliš[rozsahy Azure IP](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="e2021-134">Work with your IT department tooopen port 445 outbound too[Azure IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

### <a name="cause-3-ntlmv1-is-enabled"></a><span data-ttu-id="e2021-135">Příčina 3: NTLMv1 je povolen.</span><span class="sxs-lookup"><span data-stu-id="e2021-135">Cause 3: NTLMv1 is enabled</span></span>

<span data-ttu-id="e2021-136">Systémová chyba 53 nebo systémové chybě 87 může dojít, pokud NTLMv1 komunikace je zapnuta hello klienta.</span><span class="sxs-lookup"><span data-stu-id="e2021-136">System error 53 or system error 87 can occur if NTLMv1 communication is enabled on hello client.</span></span> <span data-ttu-id="e2021-137">Úložiště Azure File podporuje jenom ověřování NTLMv2.</span><span class="sxs-lookup"><span data-stu-id="e2021-137">Azure File storage supports only NTLMv2 authentication.</span></span> <span data-ttu-id="e2021-138">S NTLMv1 povoleno vytvoří klienta méně bezpečné.</span><span class="sxs-lookup"><span data-stu-id="e2021-138">Having NTLMv1 enabled creates a less-secure client.</span></span> <span data-ttu-id="e2021-139">Proto je blokován komunikace pro Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="e2021-139">Therefore, communication is blocked for Azure File storage.</span></span> 

<span data-ttu-id="e2021-140">toodetermine jestli je to hello příčinu chyby hello, ověřte, že tento hello následující podklíč registru je nastavena hodnota tooa 3:</span><span class="sxs-lookup"><span data-stu-id="e2021-140">toodetermine whether this is hello cause of hello error, verify that hello following registry subkey is set tooa value of 3:</span></span>

<span data-ttu-id="e2021-141">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**</span><span class="sxs-lookup"><span data-stu-id="e2021-141">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**</span></span>

<span data-ttu-id="e2021-142">Další informace najdete v tématu hello [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) tématu na webu TechNet.</span><span class="sxs-lookup"><span data-stu-id="e2021-142">For more information, see hello [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) topic on TechNet.</span></span>

### <a name="solution-for-cause-3"></a><span data-ttu-id="e2021-143">Řešení pro příčina 3</span><span class="sxs-lookup"><span data-stu-id="e2021-143">Solution for cause 3</span></span>

<span data-ttu-id="e2021-144">Vrátit hello **LmCompatibilityLevel** hodnotu toohello výchozí hodnotu 3 v hello následující podklíč registru:</span><span class="sxs-lookup"><span data-stu-id="e2021-144">Revert hello **LmCompatibilityLevel** value toohello default value of 3 in hello following registry subkey:</span></span>

  <span data-ttu-id="e2021-145">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa**</span><span class="sxs-lookup"><span data-stu-id="e2021-145">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa**</span></span>

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-tooprocess-this-command-when-you-copy-tooan-azure-file-share"></a><span data-ttu-id="e2021-146">Chyba 1816 "není dostatek kvóty je k dispozici tooprocess tento příkaz" Pokud zkopírujete tooan Azure sdílené složky</span><span class="sxs-lookup"><span data-stu-id="e2021-146">Error 1816 “Not enough quota is available tooprocess this command” when you copy tooan Azure file share</span></span>

### <a name="cause"></a><span data-ttu-id="e2021-147">Příčina</span><span class="sxs-lookup"><span data-stu-id="e2021-147">Cause</span></span>

<span data-ttu-id="e2021-148">Chyba 1816 se stane, když dostanete hello horní limit počtu souběžných otevřete obslužných rutin, které jsou povoleny pro soubor v počítači hello kde připojené hello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e2021-148">Error 1816 happens when you reach hello upper limit of concurrent open handles that are allowed for a file on hello computer where hello file share is being mounted.</span></span>

### <a name="solution"></a><span data-ttu-id="e2021-149">Řešení</span><span class="sxs-lookup"><span data-stu-id="e2021-149">Solution</span></span>

<span data-ttu-id="e2021-150">Snižte hello počet souběžných otevřených obslužných rutin ukončením některé obslužné rutiny a poté opakujte.</span><span class="sxs-lookup"><span data-stu-id="e2021-150">Reduce hello number of concurrent open handles by closing some handles, and then retry.</span></span> <span data-ttu-id="e2021-151">Další informace najdete v tématu [kontrolní seznam výkonu a škálovatelnosti služby Microsoft Azure Storage](storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="e2021-151">For more information, see [Microsoft Azure Storage performance and scalability checklist](storage-performance-checklist.md).</span></span>

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-windows"></a><span data-ttu-id="e2021-152">Pomalé souboru kopírování tooand z Azure File storage ve Windows</span><span class="sxs-lookup"><span data-stu-id="e2021-152">Slow file copying tooand from Azure File storage in Windows</span></span>

<span data-ttu-id="e2021-153">Při pokusu o tootransfer soubory toohello Azure File service, může se zobrazit nízký výkon.</span><span class="sxs-lookup"><span data-stu-id="e2021-153">You might see slow performance when you try tootransfer files toohello Azure File service.</span></span>

- <span data-ttu-id="e2021-154">Pokud nemáte konkrétní požadavky minimální velikost vstupně-výstupních operací, doporučujeme použít 1 MB, protože hello velikost vstupně-výstupní operace pro zajištění optimálního výkonu.</span><span class="sxs-lookup"><span data-stu-id="e2021-154">If you don’t have a specific minimum I/O size requirement, we recommend that you use 1 MB as hello I/O size for optimal performance.</span></span>
-   <span data-ttu-id="e2021-155">Pokud znáte hello konečné velikosti souboru, který bude rozšiřovat s zapíše a váš software nemá problémy s kompatibilitou, když hello unwritten tail na hello soubor obsahuje nuly, potom sadu hello velikost souboru předem místo provedení každém zápisu rozšíření zápisu.</span><span class="sxs-lookup"><span data-stu-id="e2021-155">If you know hello final size of a file that you are extending with writes, and your software doesn’t have compatibility problems when hello unwritten tail on hello file contains zeros, then set hello file size in advance instead of making every write an extending write.</span></span>
-   <span data-ttu-id="e2021-156">Použijte metodu pravé kopie hello:</span><span class="sxs-lookup"><span data-stu-id="e2021-156">Use hello right copy method:</span></span>
    -   <span data-ttu-id="e2021-157">Použití [AzCopy](storage-use-azcopy.md#file-copy) pro všechny přenosy mezi dvěma sdílenými složkami.</span><span class="sxs-lookup"><span data-stu-id="e2021-157">Use [AzCopy](storage-use-azcopy.md#file-copy) for any transfer between two file shares.</span></span>
    -   <span data-ttu-id="e2021-158">Použití [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mezi sdílené složky na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="e2021-158">Use [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) between file shares on an on-premises computer.</span></span>

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a><span data-ttu-id="e2021-159">Důležité informace pro Windows 8.1 nebo Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="e2021-159">Considerations for Windows 8.1 or Windows Server 2012 R2</span></span>

<span data-ttu-id="e2021-160">Pro klienty se systémem Windows 8.1 nebo Windows Server 2012 R2, ujistěte se, že hello [KB3114025](https://support.microsoft.com/help/3114025) je nainstalována oprava hotfix.</span><span class="sxs-lookup"><span data-stu-id="e2021-160">For clients that are running Windows 8.1 or Windows Server 2012 R2, make sure that hello [KB3114025](https://support.microsoft.com/help/3114025) hotfix is installed.</span></span> <span data-ttu-id="e2021-161">Tato oprava hotfix zlepšuje hello Create a zavřete obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="e2021-161">This hotfix improves hello performance of create and close handles.</span></span>

<span data-ttu-id="e2021-162">Můžete spustit následující skript toocheck, zda byla nainstalována oprava hotfix hello hello:</span><span class="sxs-lookup"><span data-stu-id="e2021-162">You can run hello following script toocheck whether hello hotfix has been installed:</span></span>

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

<span data-ttu-id="e2021-163">Pokud je nainstalována oprava hotfix, hello následující výstup se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="e2021-163">If hotfix is installed, hello following output is displayed:</span></span>

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> <span data-ttu-id="e2021-164">Bitové kopie systému Windows Server 2012 R2 v Azure Marketplace nainstalována oprava hotfix KB3114025 nainstalované ve výchozím nastavení, od prosince 2015.</span><span class="sxs-lookup"><span data-stu-id="e2021-164">Windows Server 2012 R2 images in Azure Marketplace have hotfix KB3114025 installed by default, starting in December 2015.</span></span>

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a><span data-ttu-id="e2021-165">Žádná složka s písmenem jednotky v **tento počítač**</span><span class="sxs-lookup"><span data-stu-id="e2021-165">No folder with a drive letter in **My Computer**</span></span>

<span data-ttu-id="e2021-166">Pokud namapujete sdílenou složku Azure jako správce pomocí příkazu net use, sdílené složky hello se zobrazí toobe chybí.</span><span class="sxs-lookup"><span data-stu-id="e2021-166">If you map an Azure file share as an administrator by using net use, hello share appears toobe missing.</span></span>

### <a name="cause"></a><span data-ttu-id="e2021-167">Příčina</span><span class="sxs-lookup"><span data-stu-id="e2021-167">Cause</span></span>

<span data-ttu-id="e2021-168">Ve výchozím prohlížeči souborů Windows nejde spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="e2021-168">By default, Windows File Explorer does not run as an administrator.</span></span> <span data-ttu-id="e2021-169">Spuštěním příkazu net use z příkazového řádku pro správu, mapovat síťovou jednotku hello jako správce.</span><span class="sxs-lookup"><span data-stu-id="e2021-169">If you run net use from an administrative command prompt, you map hello network drive as an administrator.</span></span> <span data-ttu-id="e2021-170">Protože připojené jednotky jsou zaměřené na uživatele, nejsou zobrazeny hello uživatelský účet, který je přihlášen hello jednotky, pokud jsou připojeny v jiného uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="e2021-170">Because mapped drives are user-centric, hello user account that is logged in does not display hello drives if they are mounted under a different user account.</span></span>

### <a name="solution"></a><span data-ttu-id="e2021-171">Řešení</span><span class="sxs-lookup"><span data-stu-id="e2021-171">Solution</span></span>
<span data-ttu-id="e2021-172">Připojte sdílenou složku hello z příkazového řádku bez oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="e2021-172">Mount hello share from a non-administrator command line.</span></span> <span data-ttu-id="e2021-173">Alternativně můžete postupovat podle [Toto téma TechNet](https://technet.microsoft.com/library/ee844140.aspx) tooconfigure hello **EnableLinkedConnections** hodnotu registru.</span><span class="sxs-lookup"><span data-stu-id="e2021-173">Alternatively, you can follow [this TechNet topic](https://technet.microsoft.com/library/ee844140.aspx) tooconfigure hello **EnableLinkedConnections** registry value.</span></span>

<a id="netuse"></a>
## <a name="net-use-command-fails-if-hello-storage-account-contains-a-forward-slash"></a><span data-ttu-id="e2021-174">Pomocí příkazu net use selže, pokud účet úložiště hello obsahuje lomítkem</span><span class="sxs-lookup"><span data-stu-id="e2021-174">Net use command fails if hello storage account contains a forward slash</span></span>

### <a name="cause"></a><span data-ttu-id="e2021-175">Příčina</span><span class="sxs-lookup"><span data-stu-id="e2021-175">Cause</span></span>

<span data-ttu-id="e2021-176">pomocí příkazu net use Hello interpretuje jako možnost příkazového řádku lomítkem (/).</span><span class="sxs-lookup"><span data-stu-id="e2021-176">hello net use command interprets a forward slash (/) as a command-line option.</span></span> <span data-ttu-id="e2021-177">Pokud vaše uživatelské jméno účtu začíná lomítkem, mapování jednotky hello se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="e2021-177">If your user account name starts with a forward slash, hello drive mapping fails.</span></span>

### <a name="solution"></a><span data-ttu-id="e2021-178">Řešení</span><span class="sxs-lookup"><span data-stu-id="e2021-178">Solution</span></span>

<span data-ttu-id="e2021-179">Můžete použít některý z následujících kroků toowork vyřešit problém hello hello:</span><span class="sxs-lookup"><span data-stu-id="e2021-179">You can use either of hello following steps toowork around hello problem:</span></span>

- <span data-ttu-id="e2021-180">Spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="e2021-180">Run hello following PowerShell command:</span></span>

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  <span data-ttu-id="e2021-181">Z dávkového souboru můžete spustit příkaz hello tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="e2021-181">From a batch file, you can run hello command this way:</span></span>

  `Echo new-smbMapping ... | powershell -command –`

- <span data-ttu-id="e2021-182">Uveďte dvojité uvozovky hello klíče toowork vyřešit tento problém – pokud je první znak hello hello lomítkem.</span><span class="sxs-lookup"><span data-stu-id="e2021-182">Put double quotation marks around hello key toowork around this problem--unless hello forward slash is hello first character.</span></span> <span data-ttu-id="e2021-183">Pokud se jedná, buď použijte hello interaktivním režimu a zadejte své heslo samostatně nebo obnovit vaše klíče tooget klíč, který nezačíná lomítkem.</span><span class="sxs-lookup"><span data-stu-id="e2021-183">If it is, either use hello interactive mode and enter your password separately or regenerate your keys tooget a key that doesn't start with a forward slash.</span></span>

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-file-storage-drive"></a><span data-ttu-id="e2021-184">Aplikace nebo služba nedostupná připojenou jednotku úložiště Azure File</span><span class="sxs-lookup"><span data-stu-id="e2021-184">Application or service cannot access a mounted Azure File storage drive</span></span>

### <a name="cause"></a><span data-ttu-id="e2021-185">Příčina</span><span class="sxs-lookup"><span data-stu-id="e2021-185">Cause</span></span>

<span data-ttu-id="e2021-186">Na uživatele jsou připojené jednotky.</span><span class="sxs-lookup"><span data-stu-id="e2021-186">Drives are mounted per user.</span></span> <span data-ttu-id="e2021-187">Pokud vaše aplikace nebo služba běží pod jiným uživatelským účtem než hello ten, který připojenou jednotku hello, nezobrazí aplikace hello hello jednotky.</span><span class="sxs-lookup"><span data-stu-id="e2021-187">If your application or service is running under a different user account than hello one that mounted hello drive, hello application will not see hello drive.</span></span>

### <a name="solution"></a><span data-ttu-id="e2021-188">Řešení</span><span class="sxs-lookup"><span data-stu-id="e2021-188">Solution</span></span>

<span data-ttu-id="e2021-189">Použijte jednu z následujících řešení hello:</span><span class="sxs-lookup"><span data-stu-id="e2021-189">Use one of hello following solutions:</span></span>

-   <span data-ttu-id="e2021-190">Připojit jednotku hello z hello stejným uživatelským účtem, který obsahuje aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="e2021-190">Mount hello drive from hello same user account that contains hello application.</span></span> <span data-ttu-id="e2021-191">Můžete použít nástroje, jako je PsExec.</span><span class="sxs-lookup"><span data-stu-id="e2021-191">You can use a tool such as PsExec.</span></span>
- <span data-ttu-id="e2021-192">Předejte hello název účtu úložiště a klíč uživatele hello jméno a heslo parametry příkazu net use hello.</span><span class="sxs-lookup"><span data-stu-id="e2021-192">Pass hello storage account name and key in hello user name and password parameters of hello net use command.</span></span>

<span data-ttu-id="e2021-193">Po budete postupovat podle těchto pokynů, může se zobrazit následující chybová zpráva při spuštění příkazu net use pro účet služby systému nebo síťových hello hello: "došlo k systémové chybě 1312.</span><span class="sxs-lookup"><span data-stu-id="e2021-193">After you follow these instructions, you might receive hello following error message when you run net use for hello system/network service account: "System error 1312 has occurred.</span></span> <span data-ttu-id="e2021-194">Zadané přihlašovací relace neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e2021-194">A specified logon session does not exist.</span></span> <span data-ttu-id="e2021-195">Ho může již byla ukončena."</span><span class="sxs-lookup"><span data-stu-id="e2021-195">It may already have been terminated."</span></span> <span data-ttu-id="e2021-196">Pokud k tomu dojde, ujistěte se, tímto uživatelským jménem, který je předán toonet použití hello obsahuje informace o doméně (například: "[název účtu úložiště]. file.core.windows .net").</span><span class="sxs-lookup"><span data-stu-id="e2021-196">If this occurs, make sure that hello username that is passed toonet use includes domain information (for example: "[storage account name].file.core.windows.net").</span></span>

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-tooa-destination-that-does-not-support-encryption"></a><span data-ttu-id="e2021-197">Chyba "Kopírujete cíl tooa souboru, který nepodporuje šifrování"</span><span class="sxs-lookup"><span data-stu-id="e2021-197">Error "You are copying a file tooa destination that does not support encryption"</span></span>

<span data-ttu-id="e2021-198">Po zkopírování souboru přes síť hello hello souboru jsou dešifrována na hello zdrojový počítač, přenáší ve formátu prostého textu a znovu zašifrována v cílovém umístění hello.</span><span class="sxs-lookup"><span data-stu-id="e2021-198">When a file is copied over hello network, hello file is decrypted on hello source computer, transmitted in plaintext, and re-encrypted at hello destination.</span></span> <span data-ttu-id="e2021-199">Však může se zobrazit následující chyby, pokud se pokoušíte toocopy zašifrovaný soubor hello: "Kopírujete hello cíl tooa souboru, který nepodporuje šifrování."</span><span class="sxs-lookup"><span data-stu-id="e2021-199">However, you might see hello following error when you're trying toocopy an encrypted file: "You are copying hello file tooa destination that does not support encryption."</span></span>

### <a name="cause"></a><span data-ttu-id="e2021-200">Příčina</span><span class="sxs-lookup"><span data-stu-id="e2021-200">Cause</span></span>
<span data-ttu-id="e2021-201">Tomuto problému může dojít, pokud používáte systému souborů EFS (ENCRYPTING File System).</span><span class="sxs-lookup"><span data-stu-id="e2021-201">This problem can occur if you are using Encrypting File System (EFS).</span></span> <span data-ttu-id="e2021-202">Šifrované nástrojem BitLocker soubory mohou být tooAzure zkopírovaný soubor úložiště.</span><span class="sxs-lookup"><span data-stu-id="e2021-202">BitLocker-encrypted files can be copied tooAzure File storage.</span></span> <span data-ttu-id="e2021-203">Azure File storage však nepodporuje systém souborů EFS systému souborů NTFS.</span><span class="sxs-lookup"><span data-stu-id="e2021-203">However, Azure File storage does not support NTFS EFS.</span></span>

### <a name="workaround"></a><span data-ttu-id="e2021-204">Alternativní řešení</span><span class="sxs-lookup"><span data-stu-id="e2021-204">Workaround</span></span>
<span data-ttu-id="e2021-205">toocopy soubor hello síti, můžete musí nejprve ho dešifrovat.</span><span class="sxs-lookup"><span data-stu-id="e2021-205">toocopy a file over hello network, you must first decrypt it.</span></span> <span data-ttu-id="e2021-206">Použijte jednu z následujících metod hello:</span><span class="sxs-lookup"><span data-stu-id="e2021-206">Use one of hello following methods:</span></span>

- <span data-ttu-id="e2021-207">Použití hello **zkopírujte /d** příkaz.</span><span class="sxs-lookup"><span data-stu-id="e2021-207">Use hello **copy /d** command.</span></span> <span data-ttu-id="e2021-208">Umožňuje hello šifrované soubory toobe uložena jako dešifrované soubory v cílovém umístění hello.</span><span class="sxs-lookup"><span data-stu-id="e2021-208">It allows hello encrypted files toobe saved as decrypted files at hello destination.</span></span>
- <span data-ttu-id="e2021-209">Nastavte hello následující klíč registru:</span><span class="sxs-lookup"><span data-stu-id="e2021-209">Set hello following registry key:</span></span>
  - <span data-ttu-id="e2021-210">Cesta = HKLM\Software\Policies\Microsoft\Windows\System</span><span class="sxs-lookup"><span data-stu-id="e2021-210">Path = HKLM\Software\Policies\Microsoft\Windows\System</span></span>
  - <span data-ttu-id="e2021-211">Typ hodnoty = DWORD</span><span class="sxs-lookup"><span data-stu-id="e2021-211">Value type = DWORD</span></span>
  - <span data-ttu-id="e2021-212">Název = CopyFileAllowDecryptedRemoteDestination</span><span class="sxs-lookup"><span data-stu-id="e2021-212">Name = CopyFileAllowDecryptedRemoteDestination</span></span>
  - <span data-ttu-id="e2021-213">Hodnota = 1</span><span class="sxs-lookup"><span data-stu-id="e2021-213">Value = 1</span></span>

<span data-ttu-id="e2021-214">Mějte na paměti, že tento klíč registru hello nastavení ovlivňuje všechny operace kopírování, které jsou vytvářeny toonetwork sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="e2021-214">Be aware that setting hello registry key affects all copy operations that are made toonetwork shares.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="e2021-215">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="e2021-215">Need help?</span></span> <span data-ttu-id="e2021-216">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="e2021-216">Contact support.</span></span>
<span data-ttu-id="e2021-217">Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit váš problém.</span><span class="sxs-lookup"><span data-stu-id="e2021-217">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your problem resolved quickly.</span></span>
