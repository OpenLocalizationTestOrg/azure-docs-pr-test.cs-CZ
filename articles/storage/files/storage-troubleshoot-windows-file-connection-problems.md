---
title: "Poradce při potížích úložiště Azure File ve Windows | Microsoft Docs"
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
ms.openlocfilehash: daaf7d0589f5e2d82b43dad879bffd23370a2c81
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-windows"></a><span data-ttu-id="42806-103">Poradce při potížích úložiště Azure File ve Windows</span><span class="sxs-lookup"><span data-stu-id="42806-103">Troubleshoot Azure File storage problems in Windows</span></span>

<span data-ttu-id="42806-104">V tomto článku jsou uvedeny běžné problémy, které se vztahují k Microsoft Azure File storage při připojení klientů se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="42806-104">This article lists common problems that are related to Microsoft Azure File storage when you connect from Windows clients.</span></span> <span data-ttu-id="42806-105">Umožňuje také možné příčiny a řešení těchto problémů.</span><span class="sxs-lookup"><span data-stu-id="42806-105">It also provides possible causes and resolutions for these problems.</span></span> <span data-ttu-id="42806-106">Kromě řešení problémů s kroky v tomto článku, můžete také použít [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) zajistit, že klienta prostředí systému Windows má správný předpoklady.</span><span class="sxs-lookup"><span data-stu-id="42806-106">In addition to the troubleshooting steps in this article, you can also use [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) to ensure that the Windows client environment has correct prerequisites.</span></span> <span data-ttu-id="42806-107">AzFileDiagnostics automatizuje detekce většina příznaků uvedených v tomto článku a pomáhá nastavit svoje prostředí, abyste získali optimální výkon.</span><span class="sxs-lookup"><span data-stu-id="42806-107">AzFileDiagnostics automates detection of most of the symptoms mentioned in this article and helps set up your environment to get optimal performance.</span></span> <span data-ttu-id="42806-108">Můžete také najít tyto informace [Azure sdíleným Poradce při potížích s](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) , který poskytuje postup vám pomůže s připojením, mapování/připojení Azure sdíleným problémy.</span><span class="sxs-lookup"><span data-stu-id="42806-108">You can also find this information in the [Azure Files shares Troubleshooter](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) that provides steps to assist you with problems connecting/mapping/mounting Azure Files shares.</span></span>


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a><span data-ttu-id="42806-109">Chyba 53, chyba 67 nebo 87 Chyba při připojení nebo odpojení sdílenou složku Azure</span><span class="sxs-lookup"><span data-stu-id="42806-109">Error 53, Error 67, or Error 87 when you mount or unmount an Azure file share</span></span>

<span data-ttu-id="42806-110">Při pokusu připojit sdílenou složku z místní nebo jiném datovém centru, může se zobrazit následující chyby:</span><span class="sxs-lookup"><span data-stu-id="42806-110">When you try to mount a file share from on-premises or from a different datacenter, you might receive the following errors:</span></span>

- <span data-ttu-id="42806-111">Došlo k systémové chybě 53.</span><span class="sxs-lookup"><span data-stu-id="42806-111">System error 53 has occurred.</span></span> <span data-ttu-id="42806-112">Cesta v síti nebyla nalezena.</span><span class="sxs-lookup"><span data-stu-id="42806-112">The network path was not found.</span></span>
- <span data-ttu-id="42806-113">Došlo k systémové chybě 67.</span><span class="sxs-lookup"><span data-stu-id="42806-113">System error 67 has occurred.</span></span> <span data-ttu-id="42806-114">Síťový název nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="42806-114">The network name cannot be found.</span></span>
- <span data-ttu-id="42806-115">Došlo k systémové chybě 87.</span><span class="sxs-lookup"><span data-stu-id="42806-115">System error 87 has occurred.</span></span> <span data-ttu-id="42806-116">Parametr je nesprávný.</span><span class="sxs-lookup"><span data-stu-id="42806-116">The parameter is incorrect.</span></span>

### <a name="cause-1-unencrypted-communication-channel"></a><span data-ttu-id="42806-117">Příčina 1: Nezašifrované komunikační kanál</span><span class="sxs-lookup"><span data-stu-id="42806-117">Cause 1: Unencrypted communication channel</span></span>

<span data-ttu-id="42806-118">Z bezpečnostních důvodů připojení ke sdílené složky Azure jsou zablokovány, pokud není šifrován komunikační kanál, a pokud není proveden pokus o připojení ze stejného datového centra kde jsou umístěny sdílené složky Azure.</span><span class="sxs-lookup"><span data-stu-id="42806-118">For security reasons, connections to Azure file shares are blocked if the communication channel isn’t encrypted and if the connection attempt isn't made from the same datacenter where the Azure file shares reside.</span></span> <span data-ttu-id="42806-119">Šifrování kanálu komunikace je k dispozici pouze v případě, že uživatele klientského operačního systému podporuje šifrování protokolu SMB.</span><span class="sxs-lookup"><span data-stu-id="42806-119">Communication channel encryption is provided only if the user’s client OS supports SMB encryption.</span></span>

<span data-ttu-id="42806-120">Windows 8, Windows Server 2012 a novějších verzích každý systém vyjednávat požadavků, které zahrnují protokolu SMB 3.0, který podporuje šifrování.</span><span class="sxs-lookup"><span data-stu-id="42806-120">Windows 8, Windows Server 2012, and later versions of each system negotiate requests that include SMB 3.0, which supports encryption.</span></span>

### <a name="solution-for-cause-1"></a><span data-ttu-id="42806-121">Řešení pro příčina 1</span><span class="sxs-lookup"><span data-stu-id="42806-121">Solution for cause 1</span></span>

<span data-ttu-id="42806-122">Připojení z klienta, která provádí jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="42806-122">Connect from a client that does one of the following:</span></span>

- <span data-ttu-id="42806-123">Splňuje požadavky na systém Windows 8 a Windows Server 2012 nebo novější verze</span><span class="sxs-lookup"><span data-stu-id="42806-123">Meets the requirements of Windows 8 and Windows Server 2012 or later versions</span></span>
- <span data-ttu-id="42806-124">Připojí se z virtuálního počítače ve stejném datovém centru jako účet úložiště Azure, který se používá pro sdílenou složkou Azure</span><span class="sxs-lookup"><span data-stu-id="42806-124">Connects from a virtual machine in the same datacenter as the Azure storage account that is used for the Azure file share</span></span>

### <a name="cause-2-port-445-is-blocked"></a><span data-ttu-id="42806-125">2 příčina: Port 445 je blokován.</span><span class="sxs-lookup"><span data-stu-id="42806-125">Cause 2: Port 445 is blocked</span></span>

<span data-ttu-id="42806-126">Systémová chyba 53 nebo systémové chybě 67 může dojít, pokud je blokován port 445 odchozí komunikaci datacentrum Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="42806-126">System error 53 or system error 67 can occur if port 445 outbound communication to an Azure File storage datacenter is blocked.</span></span> <span data-ttu-id="42806-127">Chcete-li zobrazit seznam poskytovatelů internetových služeb, které povolí nebo zakáže přístup z port 445, přejděte na [TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).</span><span class="sxs-lookup"><span data-stu-id="42806-127">To see the summary of ISPs that allow or disallow access from port 445, go to [TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).</span></span>

<span data-ttu-id="42806-128">Zjistit, jestli je z důvodu za zpráva "Chyba systému 53", můžete Portqry dotazovat TCP:445 koncový bod.</span><span class="sxs-lookup"><span data-stu-id="42806-128">To understand whether this is the reason behind the "System error 53" message, you can use Portqry to query the TCP:445 endpoint.</span></span> <span data-ttu-id="42806-129">Pokud koncový bod TCP:445 se zobrazí jako filtrované, TCP port je blokován.</span><span class="sxs-lookup"><span data-stu-id="42806-129">If the TCP:445 endpoint is displayed as filtered, the TCP port is blocked.</span></span> <span data-ttu-id="42806-130">Tady je příklad dotazu:</span><span class="sxs-lookup"><span data-stu-id="42806-130">Here is an example query:</span></span>

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

<span data-ttu-id="42806-131">Pokud TCP port 445 je blokován pravidlem v síťové cestě, zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="42806-131">If TCP port 445 is blocked by a rule along the network path, you will see the following output:</span></span>

  `TCP port 445 (microsoft-ds service): FILTERED`

<span data-ttu-id="42806-132">Další informace o tom, jak používat Portqry najdete v tématu [Popis nástroje příkazového řádku Portqry.exe](https://support.microsoft.com/help/310099).</span><span class="sxs-lookup"><span data-stu-id="42806-132">For more information about how to use Portqry, see [Description of the Portqry.exe command-line utility](https://support.microsoft.com/help/310099).</span></span>

### <a name="solution-for-cause-2"></a><span data-ttu-id="42806-133">Řešení pro příčina 2</span><span class="sxs-lookup"><span data-stu-id="42806-133">Solution for cause 2</span></span>

<span data-ttu-id="42806-134">Práce s vaším IT oddělením otevřít port 445 odchozí do [rozsahy Azure IP](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="42806-134">Work with your IT department to open port 445 outbound to [Azure IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

### <a name="cause-3-ntlmv1-is-enabled"></a><span data-ttu-id="42806-135">Příčina 3: NTLMv1 je povolen.</span><span class="sxs-lookup"><span data-stu-id="42806-135">Cause 3: NTLMv1 is enabled</span></span>

<span data-ttu-id="42806-136">Systémová chyba 53 nebo systémové chybě 87 může dojít, pokud je povoleno NTLMv1 komunikace na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="42806-136">System error 53 or system error 87 can occur if NTLMv1 communication is enabled on the client.</span></span> <span data-ttu-id="42806-137">Úložiště Azure File podporuje jenom ověřování NTLMv2.</span><span class="sxs-lookup"><span data-stu-id="42806-137">Azure File storage supports only NTLMv2 authentication.</span></span> <span data-ttu-id="42806-138">S NTLMv1 povoleno vytvoří klienta méně bezpečné.</span><span class="sxs-lookup"><span data-stu-id="42806-138">Having NTLMv1 enabled creates a less-secure client.</span></span> <span data-ttu-id="42806-139">Proto je blokován komunikace pro Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="42806-139">Therefore, communication is blocked for Azure File storage.</span></span> 

<span data-ttu-id="42806-140">Pokud chcete zjistit, jestli se jedná o příčinu chyby, ověřte, zda následující podklíč registru je nastavena na hodnotu 3:</span><span class="sxs-lookup"><span data-stu-id="42806-140">To determine whether this is the cause of the error, verify that the following registry subkey is set to a value of 3:</span></span>

<span data-ttu-id="42806-141">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**</span><span class="sxs-lookup"><span data-stu-id="42806-141">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**</span></span>

<span data-ttu-id="42806-142">Další informace najdete v tématu [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) tématu na webu TechNet.</span><span class="sxs-lookup"><span data-stu-id="42806-142">For more information, see the [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) topic on TechNet.</span></span>

### <a name="solution-for-cause-3"></a><span data-ttu-id="42806-143">Řešení pro příčina 3</span><span class="sxs-lookup"><span data-stu-id="42806-143">Solution for cause 3</span></span>

<span data-ttu-id="42806-144">Vrátit zpět **LmCompatibilityLevel** hodnota na výchozí hodnotu 3 v následujícím podklíči registru:</span><span class="sxs-lookup"><span data-stu-id="42806-144">Revert the **LmCompatibilityLevel** value to the default value of 3 in the following registry subkey:</span></span>

  <span data-ttu-id="42806-145">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa**</span><span class="sxs-lookup"><span data-stu-id="42806-145">**HKLM\SYSTEM\CurrentControlSet\Control\Lsa**</span></span>

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a><span data-ttu-id="42806-146">Chyba 1816 "není dostatek kvóty je k dispozici pro zpracování tohoto příkazu" Pokud zkopírujete do Azure sdílené složky</span><span class="sxs-lookup"><span data-stu-id="42806-146">Error 1816 “Not enough quota is available to process this command” when you copy to an Azure file share</span></span>

### <a name="cause"></a><span data-ttu-id="42806-147">Příčina</span><span class="sxs-lookup"><span data-stu-id="42806-147">Cause</span></span>

<span data-ttu-id="42806-148">Při dosažení horní limit počtu souběžných otevřete obslužných rutin, které jsou povoleny pro soubor v počítači, kde připojené sdílené složky dochází k chybě 1816.</span><span class="sxs-lookup"><span data-stu-id="42806-148">Error 1816 happens when you reach the upper limit of concurrent open handles that are allowed for a file on the computer where the file share is being mounted.</span></span>

### <a name="solution"></a><span data-ttu-id="42806-149">Řešení</span><span class="sxs-lookup"><span data-stu-id="42806-149">Solution</span></span>

<span data-ttu-id="42806-150">Snižte počet souběžných otevřených obslužných rutin ukončením některé obslužné rutiny a poté opakujte.</span><span class="sxs-lookup"><span data-stu-id="42806-150">Reduce the number of concurrent open handles by closing some handles, and then retry.</span></span> <span data-ttu-id="42806-151">Další informace najdete v tématu [kontrolní seznam výkonu a škálovatelnosti služby Microsoft Azure Storage](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="42806-151">For more information, see [Microsoft Azure Storage performance and scalability checklist](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span></span>

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-file-storage-in-windows"></a><span data-ttu-id="42806-152">Pomalé kopírování souborů do a z Azure File storage ve Windows</span><span class="sxs-lookup"><span data-stu-id="42806-152">Slow file copying to and from Azure File storage in Windows</span></span>

<span data-ttu-id="42806-153">Při pokusu o přenos souborů do služby Azure File, může se zobrazit nízký výkon.</span><span class="sxs-lookup"><span data-stu-id="42806-153">You might see slow performance when you try to transfer files to the Azure File service.</span></span>

- <span data-ttu-id="42806-154">Pokud nemáte konkrétní požadavky minimální velikost vstupně-výstupních operací, doporučujeme použít 1 MB jako velikost vstupně-výstupní operace pro zajištění optimálního výkonu.</span><span class="sxs-lookup"><span data-stu-id="42806-154">If you don’t have a specific minimum I/O size requirement, we recommend that you use 1 MB as the I/O size for optimal performance.</span></span>
-   <span data-ttu-id="42806-155">Pokud znáte konečné velikosti souboru, který bude rozšiřovat s zápisy a váš software nebude mít problémy s kompatibilitou, pokud unwritten tail na soubor obsahuje nuly, potom nastavte velikost souboru předem místo provedení každém zápisu rozšíření zápisu.</span><span class="sxs-lookup"><span data-stu-id="42806-155">If you know the final size of a file that you are extending with writes, and your software doesn’t have compatibility problems when the unwritten tail on the file contains zeros, then set the file size in advance instead of making every write an extending write.</span></span>
-   <span data-ttu-id="42806-156">Použijte metodu pravé kopie:</span><span class="sxs-lookup"><span data-stu-id="42806-156">Use the right copy method:</span></span>
    -   <span data-ttu-id="42806-157">Použití [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) pro všechny přenosy mezi dvěma sdílenými složkami.</span><span class="sxs-lookup"><span data-stu-id="42806-157">Use [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) for any transfer between two file shares.</span></span>
    -   <span data-ttu-id="42806-158">Použití [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mezi sdílené složky na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="42806-158">Use [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) between file shares on an on-premises computer.</span></span>

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a><span data-ttu-id="42806-159">Důležité informace pro Windows 8.1 nebo Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="42806-159">Considerations for Windows 8.1 or Windows Server 2012 R2</span></span>

<span data-ttu-id="42806-160">Pro klienty se systémem Windows 8.1 nebo Windows Server 2012 R2, ujistěte se, že [KB3114025](https://support.microsoft.com/help/3114025) je nainstalována oprava hotfix.</span><span class="sxs-lookup"><span data-stu-id="42806-160">For clients that are running Windows 8.1 or Windows Server 2012 R2, make sure that the [KB3114025](https://support.microsoft.com/help/3114025) hotfix is installed.</span></span> <span data-ttu-id="42806-161">Tato oprava hotfix zvyšuje výkon vytvořit a zavřete obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="42806-161">This hotfix improves the performance of create and close handles.</span></span>

<span data-ttu-id="42806-162">Můžete spustit následující skript, který chcete zkontrolovat, zda byla nainstalována oprava hotfix:</span><span class="sxs-lookup"><span data-stu-id="42806-162">You can run the following script to check whether the hotfix has been installed:</span></span>

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

<span data-ttu-id="42806-163">Pokud je nainstalována oprava hotfix, zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="42806-163">If hotfix is installed, the following output is displayed:</span></span>

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> <span data-ttu-id="42806-164">Bitové kopie systému Windows Server 2012 R2 v Azure Marketplace nainstalována oprava hotfix KB3114025 nainstalované ve výchozím nastavení, od prosince 2015.</span><span class="sxs-lookup"><span data-stu-id="42806-164">Windows Server 2012 R2 images in Azure Marketplace have hotfix KB3114025 installed by default, starting in December 2015.</span></span>

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a><span data-ttu-id="42806-165">Žádná složka s písmenem jednotky v **tento počítač**</span><span class="sxs-lookup"><span data-stu-id="42806-165">No folder with a drive letter in **My Computer**</span></span>

<span data-ttu-id="42806-166">Pokud namapujete sdílenou složku Azure jako správce pomocí příkazu net use, sdílené složky pravděpodobně chybí.</span><span class="sxs-lookup"><span data-stu-id="42806-166">If you map an Azure file share as an administrator by using net use, the share appears to be missing.</span></span>

### <a name="cause"></a><span data-ttu-id="42806-167">Příčina</span><span class="sxs-lookup"><span data-stu-id="42806-167">Cause</span></span>

<span data-ttu-id="42806-168">Ve výchozím prohlížeči souborů Windows nejde spustit jako správce.</span><span class="sxs-lookup"><span data-stu-id="42806-168">By default, Windows File Explorer does not run as an administrator.</span></span> <span data-ttu-id="42806-169">Pokud spustíte příkazu net use z příkazového řádku pro správu, mapovat síťovou jednotku jako správce.</span><span class="sxs-lookup"><span data-stu-id="42806-169">If you run net use from an administrative command prompt, you map the network drive as an administrator.</span></span> <span data-ttu-id="42806-170">Protože připojené jednotky jsou zaměřené na uživatele, nejsou zobrazeny uživatelský účet, který je přihlášen jednotky, pokud jsou připojeny v jiného uživatelského účtu.</span><span class="sxs-lookup"><span data-stu-id="42806-170">Because mapped drives are user-centric, the user account that is logged in does not display the drives if they are mounted under a different user account.</span></span>

### <a name="solution"></a><span data-ttu-id="42806-171">Řešení</span><span class="sxs-lookup"><span data-stu-id="42806-171">Solution</span></span>
<span data-ttu-id="42806-172">Připojte sdílenou složku z příkazového řádku bez oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="42806-172">Mount the share from a non-administrator command line.</span></span> <span data-ttu-id="42806-173">Alternativně můžete postupovat podle [Toto téma TechNet](https://technet.microsoft.com/library/ee844140.aspx) nakonfigurovat **EnableLinkedConnections** hodnotu registru.</span><span class="sxs-lookup"><span data-stu-id="42806-173">Alternatively, you can follow [this TechNet topic](https://technet.microsoft.com/library/ee844140.aspx) to configure the **EnableLinkedConnections** registry value.</span></span>

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a><span data-ttu-id="42806-174">Pomocí příkazu net use selže, pokud účet úložiště obsahuje lomítkem</span><span class="sxs-lookup"><span data-stu-id="42806-174">Net use command fails if the storage account contains a forward slash</span></span>

### <a name="cause"></a><span data-ttu-id="42806-175">Příčina</span><span class="sxs-lookup"><span data-stu-id="42806-175">Cause</span></span>

<span data-ttu-id="42806-176">Pomocí příkazu net use interpretuje jako možnost příkazového řádku lomítkem (/).</span><span class="sxs-lookup"><span data-stu-id="42806-176">The net use command interprets a forward slash (/) as a command-line option.</span></span> <span data-ttu-id="42806-177">Pokud vaše uživatelské jméno účtu začíná lomítkem, mapování jednotky se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="42806-177">If your user account name starts with a forward slash, the drive mapping fails.</span></span>

### <a name="solution"></a><span data-ttu-id="42806-178">Řešení</span><span class="sxs-lookup"><span data-stu-id="42806-178">Solution</span></span>

<span data-ttu-id="42806-179">Chcete-li vyřešit tento problém můžete použít některý z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="42806-179">You can use either of the following steps to work around the problem:</span></span>

- <span data-ttu-id="42806-180">Spusťte následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="42806-180">Run the following PowerShell command:</span></span>

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  <span data-ttu-id="42806-181">Z dávkového souboru můžete spustit příkaz tímto způsobem:</span><span class="sxs-lookup"><span data-stu-id="42806-181">From a batch file, you can run the command this way:</span></span>

  `Echo new-smbMapping ... | powershell -command –`

- <span data-ttu-id="42806-182">Dvojité uvozovky klíč, který chcete vyřešit tento problém – uvedena, pokud je první znak dopředné lomítko.</span><span class="sxs-lookup"><span data-stu-id="42806-182">Put double quotation marks around the key to work around this problem--unless the forward slash is the first character.</span></span> <span data-ttu-id="42806-183">Pokud se jedná, použijte interaktivní režim a zadejte své heslo samostatně nebo znovu vygenerovat klíče získat klíč, který nezačíná lomítkem.</span><span class="sxs-lookup"><span data-stu-id="42806-183">If it is, either use the interactive mode and enter your password separately or regenerate your keys to get a key that doesn't start with a forward slash.</span></span>

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-file-storage-drive"></a><span data-ttu-id="42806-184">Aplikace nebo služba nedostupná připojenou jednotku úložiště Azure File</span><span class="sxs-lookup"><span data-stu-id="42806-184">Application or service cannot access a mounted Azure File storage drive</span></span>

### <a name="cause"></a><span data-ttu-id="42806-185">Příčina</span><span class="sxs-lookup"><span data-stu-id="42806-185">Cause</span></span>

<span data-ttu-id="42806-186">Na uživatele jsou připojené jednotky.</span><span class="sxs-lookup"><span data-stu-id="42806-186">Drives are mounted per user.</span></span> <span data-ttu-id="42806-187">Pokud vaše aplikace nebo služba běží pod účtem uživatele jiný než ten, který připojené jednotky, aplikace se nezobrazí na jednotku.</span><span class="sxs-lookup"><span data-stu-id="42806-187">If your application or service is running under a different user account than the one that mounted the drive, the application will not see the drive.</span></span>

### <a name="solution"></a><span data-ttu-id="42806-188">Řešení</span><span class="sxs-lookup"><span data-stu-id="42806-188">Solution</span></span>

<span data-ttu-id="42806-189">Použijte jedno z následujících řešení:</span><span class="sxs-lookup"><span data-stu-id="42806-189">Use one of the following solutions:</span></span>

-   <span data-ttu-id="42806-190">Připojte jednotku ze stejného uživatelského účtu, který obsahuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="42806-190">Mount the drive from the same user account that contains the application.</span></span> <span data-ttu-id="42806-191">Můžete použít nástroje, jako je PsExec.</span><span class="sxs-lookup"><span data-stu-id="42806-191">You can use a tool such as PsExec.</span></span>
- <span data-ttu-id="42806-192">Předejte název účtu úložiště a klíč uživatelské jméno a heslo parametry sítě, použijte příkaz.</span><span class="sxs-lookup"><span data-stu-id="42806-192">Pass the storage account name and key in the user name and password parameters of the net use command.</span></span>

<span data-ttu-id="42806-193">Až budete postupovat podle těchto pokynů, při spuštění příkazu net use pro účet služby systému nebo síti může zobrazit následující chybová zpráva: "došlo k systémové chybě 1312.</span><span class="sxs-lookup"><span data-stu-id="42806-193">After you follow these instructions, you might receive the following error message when you run net use for the system/network service account: "System error 1312 has occurred.</span></span> <span data-ttu-id="42806-194">Zadané přihlašovací relace neexistuje.</span><span class="sxs-lookup"><span data-stu-id="42806-194">A specified logon session does not exist.</span></span> <span data-ttu-id="42806-195">Ho může již byla ukončena."</span><span class="sxs-lookup"><span data-stu-id="42806-195">It may already have been terminated."</span></span> <span data-ttu-id="42806-196">Pokud k tomu dojde, ujistěte se, že zadané uživatelské jméno, který je předán příkazu net use obsahuje informace o doméně (například: "[název účtu úložiště]. file.core.windows .net").</span><span class="sxs-lookup"><span data-stu-id="42806-196">If this occurs, make sure that the username that is passed to net use includes domain information (for example: "[storage account name].file.core.windows.net").</span></span>

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a><span data-ttu-id="42806-197">Chyba "Kopírujete soubor do cílového umístění, která nepodporuje šifrování"</span><span class="sxs-lookup"><span data-stu-id="42806-197">Error "You are copying a file to a destination that does not support encryption"</span></span>

<span data-ttu-id="42806-198">Při kopírování souboru přes síť, je soubor dešifrovat na zdrojovém počítači, odesílané informace ve formátu prostého textu a znovu zašifrována v cílovém umístění.</span><span class="sxs-lookup"><span data-stu-id="42806-198">When a file is copied over the network, the file is decrypted on the source computer, transmitted in plaintext, and re-encrypted at the destination.</span></span> <span data-ttu-id="42806-199">Ale když se pokoušíte zkopírovat šifrovaný soubor se může zobrazit následující chyby: "Jsou kopírování souboru do cílového umístění, které nepodporuje šifrování."</span><span class="sxs-lookup"><span data-stu-id="42806-199">However, you might see the following error when you're trying to copy an encrypted file: "You are copying the file to a destination that does not support encryption."</span></span>

### <a name="cause"></a><span data-ttu-id="42806-200">Příčina</span><span class="sxs-lookup"><span data-stu-id="42806-200">Cause</span></span>
<span data-ttu-id="42806-201">Tomuto problému může dojít, pokud používáte systému souborů EFS (ENCRYPTING File System).</span><span class="sxs-lookup"><span data-stu-id="42806-201">This problem can occur if you are using Encrypting File System (EFS).</span></span> <span data-ttu-id="42806-202">Šifrované nástrojem BitLocker soubory je možné zkopírovat do úložiště Azure File.</span><span class="sxs-lookup"><span data-stu-id="42806-202">BitLocker-encrypted files can be copied to Azure File storage.</span></span> <span data-ttu-id="42806-203">Azure File storage však nepodporuje systém souborů EFS systému souborů NTFS.</span><span class="sxs-lookup"><span data-stu-id="42806-203">However, Azure File storage does not support NTFS EFS.</span></span>

### <a name="workaround"></a><span data-ttu-id="42806-204">Alternativní řešení</span><span class="sxs-lookup"><span data-stu-id="42806-204">Workaround</span></span>
<span data-ttu-id="42806-205">Kopírování souboru přes síť, můžete ji nejprve dešifrovat.</span><span class="sxs-lookup"><span data-stu-id="42806-205">To copy a file over the network, you must first decrypt it.</span></span> <span data-ttu-id="42806-206">Použijte jednu z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="42806-206">Use one of the following methods:</span></span>

- <span data-ttu-id="42806-207">Použití **zkopírujte /d** příkaz.</span><span class="sxs-lookup"><span data-stu-id="42806-207">Use the **copy /d** command.</span></span> <span data-ttu-id="42806-208">To umožňuje šifrované soubory uložit jako dešifrované soubory v cílovém umístění.</span><span class="sxs-lookup"><span data-stu-id="42806-208">It allows the encrypted files to be saved as decrypted files at the destination.</span></span>
- <span data-ttu-id="42806-209">Nastavte následující klíč registru:</span><span class="sxs-lookup"><span data-stu-id="42806-209">Set the following registry key:</span></span>
  - <span data-ttu-id="42806-210">Cesta = HKLM\Software\Policies\Microsoft\Windows\System</span><span class="sxs-lookup"><span data-stu-id="42806-210">Path = HKLM\Software\Policies\Microsoft\Windows\System</span></span>
  - <span data-ttu-id="42806-211">Typ hodnoty = DWORD</span><span class="sxs-lookup"><span data-stu-id="42806-211">Value type = DWORD</span></span>
  - <span data-ttu-id="42806-212">Název = CopyFileAllowDecryptedRemoteDestination</span><span class="sxs-lookup"><span data-stu-id="42806-212">Name = CopyFileAllowDecryptedRemoteDestination</span></span>
  - <span data-ttu-id="42806-213">Hodnota = 1</span><span class="sxs-lookup"><span data-stu-id="42806-213">Value = 1</span></span>

<span data-ttu-id="42806-214">Upozorňujeme, že nastavení klíče registru ovlivní všechny operace kopírování, které jsou vytvářeny do sdílené síťové složky.</span><span class="sxs-lookup"><span data-stu-id="42806-214">Be aware that setting the registry key affects all copy operations that are made to network shares.</span></span>

## <a name="need-help-contact-support"></a><span data-ttu-id="42806-215">Potřebujete pomoct?</span><span class="sxs-lookup"><span data-stu-id="42806-215">Need help?</span></span> <span data-ttu-id="42806-216">Obraťte se na podporu.</span><span class="sxs-lookup"><span data-stu-id="42806-216">Contact support.</span></span>
<span data-ttu-id="42806-217">Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.</span><span class="sxs-lookup"><span data-stu-id="42806-217">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your problem resolved quickly.</span></span>
