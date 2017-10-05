---
title: "Řešení potíží s ochrany selhání VMware nebo fyzický do Azure | Microsoft Docs"
description: "Tento článek popisuje běžné chyby replikace počítače VMware a řešení potíží s nimi"
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/26/2017
ms.author: asgang
ms.openlocfilehash: 6ebec2e06566b1e2d6834fdd81c0d8b2801b80b9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-on-premises-vmwarephysical-server-replication-issues"></a><span data-ttu-id="1078e-103">Řešení problémů s replikací místní VMware nebo fyzický server</span><span class="sxs-lookup"><span data-stu-id="1078e-103">Troubleshoot on-premises VMware/Physical server replication issues</span></span>
<span data-ttu-id="1078e-104">Při ochraně virtuálních počítačů VMware nebo fyzických serverů pomocí Azure Site Recovery, může se zobrazit konkrétní chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="1078e-104">You may receive a specific error message when protecting your VMware virtual machines or physical servers using Azure Site Recovery.</span></span> <span data-ttu-id="1078e-105">Tento článek podrobně popisuje některé z nejběžnějších chybových zpráv došlo, společně s řešení potíží s kroky k jejich řešení.</span><span class="sxs-lookup"><span data-stu-id="1078e-105">This article details some of the more common error messages encountered, along with troubleshooting steps to resolve them.</span></span>


## <a name="initial-replication-is-stuck-at-0"></a><span data-ttu-id="1078e-106">Počáteční replikace se zasekla v umístění % 0</span><span class="sxs-lookup"><span data-stu-id="1078e-106">Initial replication is stuck at 0%</span></span>
<span data-ttu-id="1078e-107">Většina selhání je počáteční replikace, které jsme na podporu jsou způsobeny problémy s připojením mezi zdrojový server proces serveru nebo proces serveru do Azure.</span><span class="sxs-lookup"><span data-stu-id="1078e-107">Most of the initial replication failures that we encounter at support are due to connectivity issues between source server-to-process server or process server-to-Azure.</span></span>
<span data-ttu-id="1078e-108">Pro většině případů sám sebou můžete tyto problémy vyřešit pomocí následujících kroků uvedených níže.</span><span class="sxs-lookup"><span data-stu-id="1078e-108">For most cases, you can self troubleshoot these issues by following the steps listed below.</span></span>

###<a name="check-the-following-on-source-machine"></a><span data-ttu-id="1078e-109">Zkontrolujte následující na ZDROJOVÉM počítači</span><span class="sxs-lookup"><span data-stu-id="1078e-109">Check the following on SOURCE MACHINE</span></span>
* <span data-ttu-id="1078e-110">Z příkazového řádku počítače zdrojového serveru použijte Telnet na příkaz ping procesový Server s port https (standardně 9443), jak vidíte níže, pokud jsou k dispozici žádné problémy se síťovým připojením nebo blokující problémy port brány firewall.</span><span class="sxs-lookup"><span data-stu-id="1078e-110">From Source Server machine command line, use Telnet to ping the Process Server with https port (default 9443) as shown below to see if there are any network connectivity issues or firewall port blocking issues.</span></span>
     
    `telnet <PS IP address> <port>`
> [!NOTE]
    > <span data-ttu-id="1078e-111">Pomocí služby Telnet, nepoužívejte příkaz PING k testování připojení.</span><span class="sxs-lookup"><span data-stu-id="1078e-111">Use Telnet, don’t use PING to test connectivity.</span></span>  <span data-ttu-id="1078e-112">Pokud Telnet není nainstalovaná, postupujte podle kroků seznamu [sem](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)</span><span class="sxs-lookup"><span data-stu-id="1078e-112">If Telnet is not installed, follow the steps list [here](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)</span></span>

<span data-ttu-id="1078e-113">Pokud se nelze připojit, povolit příchozí port 9443 na Procesovém serveru a zkontrolujte, pokud tento problém pořád ukončí.</span><span class="sxs-lookup"><span data-stu-id="1078e-113">If unable to connect, allow inbound port 9443 on the Process Server and check if the problem still exits.</span></span> <span data-ttu-id="1078e-114">Byl některých případech, kdy byl procesový server za hraniční sítě, který byl příčinou tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="1078e-114">There has been some cases where process server was behind DMZ, which was causing this problem.</span></span>

* <span data-ttu-id="1078e-115">Zkontrolujte stav služby `InMage Scout VX Agent – Sentinel/OutpostStart` Pokud není spuštěná a kontrola Pokud problém přetrvává.</span><span class="sxs-lookup"><span data-stu-id="1078e-115">Check the status of service `InMage Scout VX Agent – Sentinel/OutpostStart` if it is not running and check if the problem still exists.</span></span>   
 
###<a name="check-the-following-on-process-server"></a><span data-ttu-id="1078e-116">Zkontrolujte následující na PROCESOVÉM serveru</span><span class="sxs-lookup"><span data-stu-id="1078e-116">Check the following on PROCESS SERVER</span></span>

* <span data-ttu-id="1078e-117">**Zkontrolujte, pokud procesový server není aktivně předání dat do Azure**</span><span class="sxs-lookup"><span data-stu-id="1078e-117">**Check if process server is actively pushing data to Azure**</span></span> 

<span data-ttu-id="1078e-118">Z počítače procesový Server otevřete Správce úloh (stiskněte kombinaci kláves Ctrl-Shift-Esc).</span><span class="sxs-lookup"><span data-stu-id="1078e-118">From Process Server machine, open the Task Manager (press Ctrl-Shift-Esc ).</span></span> <span data-ttu-id="1078e-119">Přejděte na kartu výkonu a klikněte na odkaz sledování otevřete prostředků.</span><span class="sxs-lookup"><span data-stu-id="1078e-119">Go to the Performance tab and click ‘Open Resource Monitor’ link.</span></span> <span data-ttu-id="1078e-120">Ze Správce prostředků, přejděte na kartu síť.</span><span class="sxs-lookup"><span data-stu-id="1078e-120">From Resource Manager, go to Network tab.</span></span> <span data-ttu-id="1078e-121">Zkontrolujte, pokud cbengine.exe v "Procesy s síťové aktivity" aktivně odeslání velkého objemu dat (v MB).</span><span class="sxs-lookup"><span data-stu-id="1078e-121">Check if cbengine.exe in ‘Processes with Network Activity’ is actively sending large volume (in Mbs) of data.</span></span>

![Povolení replikace](./media/site-recovery-protection-common-errors/cbengine.png)

<span data-ttu-id="1078e-123">Pokud ne, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="1078e-123">If not follow the steps listed below:</span></span>

* <span data-ttu-id="1078e-124">**Zkontrolujte, jestli je procesový server připojit objektů Blob v Azure**: vyberte a zkontrolujte cbengine.exe zobrazení TCP připojení chcete zobrazit, pokud je k dispozici připojení z procesového serveru do adresy URL objektu blob úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="1078e-124">**Check if Process server is able to connect Azure Blob**: Select and check cbengine.exe to view the ‘TCP Connections’ to see if there is connectivity from Process server to Azure Storage blob URL.</span></span>

![Povolení replikace](./media/site-recovery-protection-common-errors/rmonitor.png)

<span data-ttu-id="1078e-126">Pokud není pak přejděte do ovládacích panelů > služeb, zkontrolujte, zda tyto služby jsou spuštěná:</span><span class="sxs-lookup"><span data-stu-id="1078e-126">If not then go to Control Panel > Services, check if the following services are up and running:</span></span>

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     * 
<span data-ttu-id="1078e-127">(Re) Spustit žádné služby, která není spuštěna a zkontrolujte, zda stále existuje problém.</span><span class="sxs-lookup"><span data-stu-id="1078e-127">(Re)Start any service which is not running and check if the problem still exists.</span></span>

* <span data-ttu-id="1078e-128">**Zkontrolujte, jestli je možné se připojit k Azure veřejnou IP adresu přes port 443 procesového serveru**</span><span class="sxs-lookup"><span data-stu-id="1078e-128">**Check if Process server is able to connect to Azure Public IP address using port 443**</span></span>

<span data-ttu-id="1078e-129">Otevřete nejnovější CBEngineCurr.errlog z `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` a vyhledejte řetězec: 443 a připojení pokus se nezdařil.</span><span class="sxs-lookup"><span data-stu-id="1078e-129">Open the latest CBEngineCurr.errlog from `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` and search for :443  and connection attempt failed.</span></span>

![Povolení replikace](./media/site-recovery-protection-common-errors/logdetails1.png)

<span data-ttu-id="1078e-131">Pokud se problémy objevily, z příkazového řádku procesový Server, použijte telnet na příkaz ping vaší Azure veřejné IP adresy (maskovat ve výše image) v CBEngineCurr.currLog přes port 443.</span><span class="sxs-lookup"><span data-stu-id="1078e-131">If there are issues, then from Process Server command line, use telnet to ping your Azure Public IP address (masked in above image) found in the CBEngineCurr.currLog using port 443.</span></span>

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
<span data-ttu-id="1078e-132">Pokud se nemůžete připojit, potom zkontrolujte, jestli se problém přístup je z důvodu brány firewall nebo Proxy, jak je popsáno v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="1078e-132">If you are unable to connect, then check if the access issue is due to firewall or Proxy as described in next step.</span></span>


* <span data-ttu-id="1078e-133">**Zkontrolujte, pokud IP adresa brána firewall na procesní server neblokují přístup**: Pokud používáte pravidla brány firewall založená na adresu IP na serveru, pak stáhnout úplný seznam Microsoft Azure Datacenter rozsahy IP adres z [sem](https://www.microsoft.com/download/details.aspx?id=41653) a přidat je do vaší konfiguraci brány firewall a ujistěte se, že umožňují komunikaci s Azure (a port HTTPS (443)).</span><span class="sxs-lookup"><span data-stu-id="1078e-133">**Check if IP address-based firewall on Process server are not blocking access**: If you are using an IP address-based firewall rules on the server, then download the complete list of Microsoft Azure Datacenter IP Ranges from [here](https://www.microsoft.com/download/details.aspx?id=41653) and add them to your firewall configuration to ensure they allow communication to Azure (and the HTTPS (443) port).</span></span>  <span data-ttu-id="1078e-134">Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).</span><span class="sxs-lookup"><span data-stu-id="1078e-134">Allow IP address ranges for the Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

* <span data-ttu-id="1078e-135">**Zkontrolujte, pokud adresa URL brána firewall na procesový server není přístup blokován**: Pokud používáte adresu URL na základě pravidel brány firewall na serveru, zkontrolujte následující adresy URL se přidají do konfigurace brány firewall.</span><span class="sxs-lookup"><span data-stu-id="1078e-135">**Check if URL-based firewall on Process server is not blocking access**:  If you are using a URL based firewall rules on the server, ensure the following URLs are added to firewall configuration.</span></span> 
     
  <span data-ttu-id="1078e-136">`*.accesscontrol.windows.net:` Používá se k řízení přístupu a správě identit.</span><span class="sxs-lookup"><span data-stu-id="1078e-136">`*.accesscontrol.windows.net:` Used for access control and identity management</span></span>

  <span data-ttu-id="1078e-137">`*.backup.windowsazure.com:` Používá se k orchestraci a přenosu dat replikace.</span><span class="sxs-lookup"><span data-stu-id="1078e-137">`*.backup.windowsazure.com:` Used for replication data transfer and orchestration</span></span>

  <span data-ttu-id="1078e-138">`*.blob.core.windows.net:`Slouží pro přístup k účtu úložiště, že úložiště replikovaná data</span><span class="sxs-lookup"><span data-stu-id="1078e-138">`*.blob.core.windows.net:` Used for access to the storage account that stores replicated data</span></span>

  <span data-ttu-id="1078e-139">`*.hypervrecoverymanager.windowsazure.com:` Používá se pro orchestraci a operace správy replikací.</span><span class="sxs-lookup"><span data-stu-id="1078e-139">`*.hypervrecoverymanager.windowsazure.com:` Used for replication management operations and orchestration</span></span>

  <span data-ttu-id="1078e-140">`time.nist.gov`a `time.windows.com`: používá k ověření synchronizaci času mezi systémem a globálním časem.</span><span class="sxs-lookup"><span data-stu-id="1078e-140">`time.nist.gov` and `time.windows.com`: Used to check time synchronization between system and global time.</span></span>

<span data-ttu-id="1078e-141">Adresy URL pro **cloudu Azure Government**:</span><span class="sxs-lookup"><span data-stu-id="1078e-141">URLs for **Azure Government Cloud**:</span></span>

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us` 

* <span data-ttu-id="1078e-142">**Zkontrolujte, pokud nastavení proxy serveru na Procesovém serveru neblokují přístup**.</span><span class="sxs-lookup"><span data-stu-id="1078e-142">**Check if Proxy Settings on Process server are not blocking access**.</span></span>  <span data-ttu-id="1078e-143">Pokud používáte Proxy Server, zajistěte, aby že název proxy serveru je řešení serverem DNS.</span><span class="sxs-lookup"><span data-stu-id="1078e-143">If you are using a Proxy Server, ensure the proxy server name is resolving by the DNS server.</span></span>
<span data-ttu-id="1078e-144">Chcete-li zkontrolovat, co jste zadali v době instalace konfigurační Server.</span><span class="sxs-lookup"><span data-stu-id="1078e-144">To check what you have provided at the time of Configuration Server setup.</span></span> <span data-ttu-id="1078e-145">Přejděte ke klíči registru</span><span class="sxs-lookup"><span data-stu-id="1078e-145">Go to registry key</span></span>

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

<span data-ttu-id="1078e-146">Teď zajistíte, že stejné nastavení jsou používány agentem Azure Site Recovery k odesílání dat.</span><span class="sxs-lookup"><span data-stu-id="1078e-146">Now ensure that the same settings are being used by Azure Site Recovery agent to send data.</span></span>
<span data-ttu-id="1078e-147">Zálohování Microsoft Azure Search</span><span class="sxs-lookup"><span data-stu-id="1078e-147">Search Microsoft Azure  Backup</span></span> 

![Povolení replikace](./media/site-recovery-protection-common-errors/mab.png)

<span data-ttu-id="1078e-149">Otevřete ho a klikněte na akci > změnit vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1078e-149">Open it and click on Action > Change Properties.</span></span> <span data-ttu-id="1078e-150">Karta Konfigurace proxy serveru měli byste vidět adresu proxy serveru, která by měla být stejná, jak je uvedeno nastavení registru.</span><span class="sxs-lookup"><span data-stu-id="1078e-150">Under Proxy Configuration tab, you should see the proxy address, which should be same as shown by the registry settings.</span></span> <span data-ttu-id="1078e-151">V opačném případě změňte se stejnou adresou.</span><span class="sxs-lookup"><span data-stu-id="1078e-151">If not, please change it to the same address.</span></span>

![Povolení replikace](./media/site-recovery-protection-common-errors/mabproxy.png)

* <span data-ttu-id="1078e-153">**Zkontrolujte, pokud omezení šířky pásma není omezen na Procesovém serveru**: zvětšit šířku pásma a zkontrolujte, zda stále existuje problém.</span><span class="sxs-lookup"><span data-stu-id="1078e-153">**Check if Throttle bandwidth is not constrained on Process server**:  Increase the bandwidth  and check if the problem still exists.</span></span>

##<a name="next-steps"></a><span data-ttu-id="1078e-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1078e-154">Next steps</span></span>
<span data-ttu-id="1078e-155">Pokud potřebujete další pomoc, následně je publikovat dotazu [fórum automatické obnovení systému](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="1078e-155">If you need more help, then post your query to [ASR forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).</span></span> <span data-ttu-id="1078e-156">Máme aktivní komunitě a jeden z našich technici bude moct vám pomůže.</span><span class="sxs-lookup"><span data-stu-id="1078e-156">We have an active community and one of our engineers will be able to assist you.</span></span>