---
title: "aaaTroubleshoot ochrany selhání VMware nebo fyzický tooAzure | Microsoft Docs"
description: "Tento článek popisuje selhání replikace počítač běžné VMware hello a jak tootroubleshoot je"
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
ms.openlocfilehash: b821e9aa2610482ba1900645fb75e75744dc442f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-on-premises-vmwarephysical-server-replication-issues"></a><span data-ttu-id="0f44e-103">Řešení problémů s replikací místní VMware nebo fyzický server</span><span class="sxs-lookup"><span data-stu-id="0f44e-103">Troubleshoot on-premises VMware/Physical server replication issues</span></span>
<span data-ttu-id="0f44e-104">Při ochraně virtuálních počítačů VMware nebo fyzických serverů pomocí Azure Site Recovery, může se zobrazit konkrétní chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="0f44e-104">You may receive a specific error message when protecting your VMware virtual machines or physical servers using Azure Site Recovery.</span></span> <span data-ttu-id="0f44e-105">Tento článek podrobnosti některé z nejběžnějších chybových zpráv došlo, společně s řešení potíží s kroky tooresolve hello je.</span><span class="sxs-lookup"><span data-stu-id="0f44e-105">This article details some of hello more common error messages encountered, along with troubleshooting steps tooresolve them.</span></span>


## <a name="initial-replication-is-stuck-at-0"></a><span data-ttu-id="0f44e-106">Počáteční replikace se zasekla v umístění % 0</span><span class="sxs-lookup"><span data-stu-id="0f44e-106">Initial replication is stuck at 0%</span></span>
<span data-ttu-id="0f44e-107">Většina selhání hello počáteční replikace, které jsme na podporu je z důvodu problémů tooconnectivity mezi zdrojový server proces serveru nebo proces serveru do Azure.</span><span class="sxs-lookup"><span data-stu-id="0f44e-107">Most of hello initial replication failures that we encounter at support are due tooconnectivity issues between source server-to-process server or process server-to-Azure.</span></span>
<span data-ttu-id="0f44e-108">Pro většině případů sám sebou můžete tyto problémy vyřešit pomocí následujících hello kroků uvedených níže.</span><span class="sxs-lookup"><span data-stu-id="0f44e-108">For most cases, you can self troubleshoot these issues by following hello steps listed below.</span></span>

###<a name="check-hello-following-on-source-machine"></a><span data-ttu-id="0f44e-109">Zkontrolujte následující hello na ZDROJOVÉM počítači</span><span class="sxs-lookup"><span data-stu-id="0f44e-109">Check hello following on SOURCE MACHINE</span></span>
* <span data-ttu-id="0f44e-110">Z příkazového řádku počítače zdrojového serveru použijte Telnet tooping hello procesový Server s port https (standardně 9443), jak je uvedeno níže toosee, pokud jsou nějaké problémy se síťovým připojením nebo problémy s blokováním port brány firewall.</span><span class="sxs-lookup"><span data-stu-id="0f44e-110">From Source Server machine command line, use Telnet tooping hello Process Server with https port (default 9443) as shown below toosee if there are any network connectivity issues or firewall port blocking issues.</span></span>
     
    `telnet <PS IP address> <port>`
> [!NOTE]
    > <span data-ttu-id="0f44e-111">Pomocí služby Telnet, nepoužívejte příkaz PING tootest připojení.</span><span class="sxs-lookup"><span data-stu-id="0f44e-111">Use Telnet, don’t use PING tootest connectivity.</span></span>  <span data-ttu-id="0f44e-112">Pokud Telnet není nainstalovaná, postupujte podle kroků seznamu hello [sem](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)</span><span class="sxs-lookup"><span data-stu-id="0f44e-112">If Telnet is not installed, follow hello steps list [here](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)</span></span>

<span data-ttu-id="0f44e-113">Pokud nelze tooconnect povolit příchozí port 9443 hello procesový Server a zkontrolujte, zda hello problém pořád ukončí.</span><span class="sxs-lookup"><span data-stu-id="0f44e-113">If unable tooconnect, allow inbound port 9443 on hello Process Server and check if hello problem still exits.</span></span> <span data-ttu-id="0f44e-114">Byl některých případech, kdy byl procesový server za hraniční sítě, který byl příčinou tohoto problému.</span><span class="sxs-lookup"><span data-stu-id="0f44e-114">There has been some cases where process server was behind DMZ, which was causing this problem.</span></span>

* <span data-ttu-id="0f44e-115">Zkontrolujte stav služby hello `InMage Scout VX Agent – Sentinel/OutpostStart` Pokud není spuštěná a kontrola Pokud hello problém stále existuje.</span><span class="sxs-lookup"><span data-stu-id="0f44e-115">Check hello status of service `InMage Scout VX Agent – Sentinel/OutpostStart` if it is not running and check if hello problem still exists.</span></span>   
 
###<a name="check-hello-following-on-process-server"></a><span data-ttu-id="0f44e-116">Zkontrolujte následující hello na PROCESOVÉM serveru</span><span class="sxs-lookup"><span data-stu-id="0f44e-116">Check hello following on PROCESS SERVER</span></span>

* <span data-ttu-id="0f44e-117">**Zkontrolujte, pokud je procesový server aktivně vkládání dat tooAzure**</span><span class="sxs-lookup"><span data-stu-id="0f44e-117">**Check if process server is actively pushing data tooAzure**</span></span> 

<span data-ttu-id="0f44e-118">Z počítače procesový Server otevřete Správce úloh (stiskněte kombinaci kláves Ctrl-Shift-Esc) hello.</span><span class="sxs-lookup"><span data-stu-id="0f44e-118">From Process Server machine, open hello Task Manager (press Ctrl-Shift-Esc ).</span></span> <span data-ttu-id="0f44e-119">Přejděte na kartu toohello výkon a klikněte na odkaz sledování otevřete prostředků.</span><span class="sxs-lookup"><span data-stu-id="0f44e-119">Go toohello Performance tab and click ‘Open Resource Monitor’ link.</span></span> <span data-ttu-id="0f44e-120">Ze Správce prostředků přejděte tooNetwork kartě. Zkontrolujte, pokud cbengine.exe v "Procesy s síťové aktivity" aktivně odeslání velkého objemu dat (v MB).</span><span class="sxs-lookup"><span data-stu-id="0f44e-120">From Resource Manager, go tooNetwork tab. Check if cbengine.exe in ‘Processes with Network Activity’ is actively sending large volume (in Mbs) of data.</span></span>

![Povolení replikace](./media/site-recovery-protection-common-errors/cbengine.png)

<span data-ttu-id="0f44e-122">Pokud ne, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="0f44e-122">If not follow hello steps listed below:</span></span>

* <span data-ttu-id="0f44e-123">**Zkontrolujte, jestli procesový server je možné tooconnect objektů Blob v Azure**: vyberte a zkontrolujte cbengine.exe tooview hello 'připojení TCP, toosee, pokud je k dispozici připojení z adresy URL pro proces serveru tooAzure úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="0f44e-123">**Check if Process server is able tooconnect Azure Blob**: Select and check cbengine.exe tooview hello ‘TCP Connections’ toosee if there is connectivity from Process server tooAzure Storage blob URL.</span></span>

![Povolení replikace](./media/site-recovery-protection-common-errors/rmonitor.png)

<span data-ttu-id="0f44e-125">Není-li poté přejděte tooControl panely > služeb, zkontrolujte, zda jsou následující služby hello provozu:</span><span class="sxs-lookup"><span data-stu-id="0f44e-125">If not then go tooControl Panel > Services, check if hello following services are up and running:</span></span>

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     * 
<span data-ttu-id="0f44e-126">(Re) Spustit žádné služby, která není spuštěna a zkontrolujte, zda text hello problém stále existuje.</span><span class="sxs-lookup"><span data-stu-id="0f44e-126">(Re)Start any service which is not running and check if hello problem still exists.</span></span>

* <span data-ttu-id="0f44e-127">**Zkontrolujte, jestli procesový server je možné tooconnect tooAzure veřejnou IP adresu pomocí portu 443**</span><span class="sxs-lookup"><span data-stu-id="0f44e-127">**Check if Process server is able tooconnect tooAzure Public IP address using port 443**</span></span>

<span data-ttu-id="0f44e-128">Otevřete hello nejnovější CBEngineCurr.errlog z `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` a vyhledejte řetězec: 443 a připojení pokus se nezdařil.</span><span class="sxs-lookup"><span data-stu-id="0f44e-128">Open hello latest CBEngineCurr.errlog from `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` and search for :443  and connection attempt failed.</span></span>

![Povolení replikace](./media/site-recovery-protection-common-errors/logdetails1.png)

<span data-ttu-id="0f44e-130">Pokud se problémy objevily, z příkazového řádku procesový Server, použijte telnet tooping vaše Azure veřejné IP adresy (maskovat ve výše image) v hello CBEngineCurr.currLog přes port 443.</span><span class="sxs-lookup"><span data-stu-id="0f44e-130">If there are issues, then from Process Server command line, use telnet tooping your Azure Public IP address (masked in above image) found in hello CBEngineCurr.currLog using port 443.</span></span>

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
<span data-ttu-id="0f44e-131">Pokud jste nelze tooconnect, potom zkontrolujte, zda problém přístup hello je z důvodu toofirewall nebo Proxy, jak je popsáno v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="0f44e-131">If you are unable tooconnect, then check if hello access issue is due toofirewall or Proxy as described in next step.</span></span>


* <span data-ttu-id="0f44e-132">**Zkontrolujte, pokud IP adresa brána firewall na procesní server neblokují přístup**: Používáte-li na serveru hello pravidla brány firewall založená na adresu IP, pak stáhnout úplný seznam hello Microsoft Azure Datacenter rozsahy IP adres z [sem ](https://www.microsoft.com/download/details.aspx?id=41653) a přidat je tooensure konfigurace brány firewall tooyour umožňují komunikaci tooAzure (a hello port HTTPS (443)).</span><span class="sxs-lookup"><span data-stu-id="0f44e-132">**Check if IP address-based firewall on Process server are not blocking access**: If you are using an IP address-based firewall rules on hello server, then download hello complete list of Microsoft Azure Datacenter IP Ranges from [here](https://www.microsoft.com/download/details.aspx?id=41653) and add them tooyour firewall configuration tooensure they allow communication tooAzure (and hello HTTPS (443) port).</span></span>  <span data-ttu-id="0f44e-133">Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).</span><span class="sxs-lookup"><span data-stu-id="0f44e-133">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

* <span data-ttu-id="0f44e-134">**Zkontrolujte, pokud adresa URL brána firewall na procesový server není přístup blokován**: Pokud používáte adresu URL na základě pravidel brány firewall na serveru hello, ujistěte se, hello následující adresy URL se přidají toofirewall konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0f44e-134">**Check if URL-based firewall on Process server is not blocking access**:  If you are using a URL based firewall rules on hello server, ensure hello following URLs are added toofirewall configuration.</span></span> 
     
  <span data-ttu-id="0f44e-135">`*.accesscontrol.windows.net:` Používá se k řízení přístupu a správě identit.</span><span class="sxs-lookup"><span data-stu-id="0f44e-135">`*.accesscontrol.windows.net:` Used for access control and identity management</span></span>

  <span data-ttu-id="0f44e-136">`*.backup.windowsazure.com:` Používá se k orchestraci a přenosu dat replikace.</span><span class="sxs-lookup"><span data-stu-id="0f44e-136">`*.backup.windowsazure.com:` Used for replication data transfer and orchestration</span></span>

  <span data-ttu-id="0f44e-137">`*.blob.core.windows.net:`Použít pro přístup k toohello účet úložiště, který ukládá replikovaná data</span><span class="sxs-lookup"><span data-stu-id="0f44e-137">`*.blob.core.windows.net:` Used for access toohello storage account that stores replicated data</span></span>

  <span data-ttu-id="0f44e-138">`*.hypervrecoverymanager.windowsazure.com:` Používá se pro orchestraci a operace správy replikací.</span><span class="sxs-lookup"><span data-stu-id="0f44e-138">`*.hypervrecoverymanager.windowsazure.com:` Used for replication management operations and orchestration</span></span>

  <span data-ttu-id="0f44e-139">`time.nist.gov`a `time.windows.com`: používá synchronizaci času toocheck mezi systémem a globálním časem.</span><span class="sxs-lookup"><span data-stu-id="0f44e-139">`time.nist.gov` and `time.windows.com`: Used toocheck time synchronization between system and global time.</span></span>

<span data-ttu-id="0f44e-140">Adresy URL pro **cloudu Azure Government**:</span><span class="sxs-lookup"><span data-stu-id="0f44e-140">URLs for **Azure Government Cloud**:</span></span>

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us` 

* <span data-ttu-id="0f44e-141">**Zkontrolujte, pokud nastavení proxy serveru na Procesovém serveru neblokují přístup**.</span><span class="sxs-lookup"><span data-stu-id="0f44e-141">**Check if Proxy Settings on Process server are not blocking access**.</span></span>  <span data-ttu-id="0f44e-142">Pokud používáte Proxy Server, ověřte, že je server DNS hello řešení hello název proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="0f44e-142">If you are using a Proxy Server, ensure hello proxy server name is resolving by hello DNS server.</span></span>
<span data-ttu-id="0f44e-143">toocheck co jste zadali v době hello nastavení konfigurace serveru.</span><span class="sxs-lookup"><span data-stu-id="0f44e-143">toocheck what you have provided at hello time of Configuration Server setup.</span></span> <span data-ttu-id="0f44e-144">Přejděte tooregistry klíč</span><span class="sxs-lookup"><span data-stu-id="0f44e-144">Go tooregistry key</span></span>

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

<span data-ttu-id="0f44e-145">Teď zajistěte, že hello stejné nastavení jsou používány data toosend agenta Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="0f44e-145">Now ensure that hello same settings are being used by Azure Site Recovery agent toosend data.</span></span>
<span data-ttu-id="0f44e-146">Zálohování Microsoft Azure Search</span><span class="sxs-lookup"><span data-stu-id="0f44e-146">Search Microsoft Azure  Backup</span></span> 

![Povolení replikace](./media/site-recovery-protection-common-errors/mab.png)

<span data-ttu-id="0f44e-148">Otevřete ho a klikněte na akci > změnit vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0f44e-148">Open it and click on Action > Change Properties.</span></span> <span data-ttu-id="0f44e-149">Karta Konfigurace proxy serveru měli byste vidět hello proxy adresu, která by měla být stejná, jak je uvedeno nastavení registru hello.</span><span class="sxs-lookup"><span data-stu-id="0f44e-149">Under Proxy Configuration tab, you should see hello proxy address, which should be same as shown by hello registry settings.</span></span> <span data-ttu-id="0f44e-150">Pokud ne, změňte ho toohello stejnou adresu.</span><span class="sxs-lookup"><span data-stu-id="0f44e-150">If not, please change it toohello same address.</span></span>

![Povolení replikace](./media/site-recovery-protection-common-errors/mabproxy.png)

* <span data-ttu-id="0f44e-152">**Zkontrolujte, pokud omezení šířky pásma není omezen na Procesovém serveru**: zvýšení hello šířky pásma a zkontrolujte, zda text hello problém stále existuje.</span><span class="sxs-lookup"><span data-stu-id="0f44e-152">**Check if Throttle bandwidth is not constrained on Process server**:  Increase hello bandwidth  and check if hello problem still exists.</span></span>

##<a name="next-steps"></a><span data-ttu-id="0f44e-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f44e-153">Next steps</span></span>
<span data-ttu-id="0f44e-154">Pokud potřebujete další pomoc, pak odeslat dotaz příliš[fórum automatické obnovení systému](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="0f44e-154">If you need more help, then post your query too[ASR forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).</span></span> <span data-ttu-id="0f44e-155">Máme aktivní komunitě a jeden z našich technici bude možné tooassist je.</span><span class="sxs-lookup"><span data-stu-id="0f44e-155">We have an active community and one of our engineers will be able tooassist you.</span></span>
