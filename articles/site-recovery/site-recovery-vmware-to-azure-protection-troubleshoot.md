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
# <a name="troubleshoot-on-premises-vmwarephysical-server-replication-issues"></a>Řešení problémů s replikací místní VMware nebo fyzický server
Při ochraně virtuálních počítačů VMware nebo fyzických serverů pomocí Azure Site Recovery, může se zobrazit konkrétní chybová zpráva. Tento článek podrobnosti některé z nejběžnějších chybových zpráv došlo, společně s řešení potíží s kroky tooresolve hello je.


## <a name="initial-replication-is-stuck-at-0"></a>Počáteční replikace se zasekla v umístění % 0
Většina selhání hello počáteční replikace, které jsme na podporu je z důvodu problémů tooconnectivity mezi zdrojový server proces serveru nebo proces serveru do Azure.
Pro většině případů sám sebou můžete tyto problémy vyřešit pomocí následujících hello kroků uvedených níže.

###<a name="check-hello-following-on-source-machine"></a>Zkontrolujte následující hello na ZDROJOVÉM počítači
* Z příkazového řádku počítače zdrojového serveru použijte Telnet tooping hello procesový Server s port https (standardně 9443), jak je uvedeno níže toosee, pokud jsou nějaké problémy se síťovým připojením nebo problémy s blokováním port brány firewall.
     
    `telnet <PS IP address> <port>`
> [!NOTE]
    > Pomocí služby Telnet, nepoužívejte příkaz PING tootest připojení.  Pokud Telnet není nainstalovaná, postupujte podle kroků seznamu hello [sem](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx)

Pokud nelze tooconnect povolit příchozí port 9443 hello procesový Server a zkontrolujte, zda hello problém pořád ukončí. Byl některých případech, kdy byl procesový server za hraniční sítě, který byl příčinou tohoto problému.

* Zkontrolujte stav služby hello `InMage Scout VX Agent – Sentinel/OutpostStart` Pokud není spuštěná a kontrola Pokud hello problém stále existuje.   
 
###<a name="check-hello-following-on-process-server"></a>Zkontrolujte následující hello na PROCESOVÉM serveru

* **Zkontrolujte, pokud je procesový server aktivně vkládání dat tooAzure** 

Z počítače procesový Server otevřete Správce úloh (stiskněte kombinaci kláves Ctrl-Shift-Esc) hello. Přejděte na kartu toohello výkon a klikněte na odkaz sledování otevřete prostředků. Ze Správce prostředků přejděte tooNetwork kartě. Zkontrolujte, pokud cbengine.exe v "Procesy s síťové aktivity" aktivně odeslání velkého objemu dat (v MB).

![Povolení replikace](./media/site-recovery-protection-common-errors/cbengine.png)

Pokud ne, postupujte podle následujících kroků hello:

* **Zkontrolujte, jestli procesový server je možné tooconnect objektů Blob v Azure**: vyberte a zkontrolujte cbengine.exe tooview hello 'připojení TCP, toosee, pokud je k dispozici připojení z adresy URL pro proces serveru tooAzure úložiště objektů blob.

![Povolení replikace](./media/site-recovery-protection-common-errors/rmonitor.png)

Není-li poté přejděte tooControl panely > služeb, zkontrolujte, zda jsou následující služby hello provozu:

     * cxprocessserver
     * InMage Scout VX Agent – Sentinel/Outpost
     * Microsoft Azure Recovery Services Agent
     * Microsoft Azure Site Recovery Service
     * tmansvc
     * 
(Re) Spustit žádné služby, která není spuštěna a zkontrolujte, zda text hello problém stále existuje.

* **Zkontrolujte, jestli procesový server je možné tooconnect tooAzure veřejnou IP adresu pomocí portu 443**

Otevřete hello nejnovější CBEngineCurr.errlog z `%programfiles%\Microsoft Azure Recovery Services Agent\Temp` a vyhledejte řetězec: 443 a připojení pokus se nezdařil.

![Povolení replikace](./media/site-recovery-protection-common-errors/logdetails1.png)

Pokud se problémy objevily, z příkazového řádku procesový Server, použijte telnet tooping vaše Azure veřejné IP adresy (maskovat ve výše image) v hello CBEngineCurr.currLog přes port 443.

      telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443
Pokud jste nelze tooconnect, potom zkontrolujte, zda problém přístup hello je z důvodu toofirewall nebo Proxy, jak je popsáno v dalším kroku.


* **Zkontrolujte, pokud IP adresa brána firewall na procesní server neblokují přístup**: Používáte-li na serveru hello pravidla brány firewall založená na adresu IP, pak stáhnout úplný seznam hello Microsoft Azure Datacenter rozsahy IP adres z [sem ](https://www.microsoft.com/download/details.aspx?id=41653) a přidat je tooensure konfigurace brány firewall tooyour umožňují komunikaci tooAzure (a hello port HTTPS (443)).  Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).

* **Zkontrolujte, pokud adresa URL brána firewall na procesový server není přístup blokován**: Pokud používáte adresu URL na základě pravidel brány firewall na serveru hello, ujistěte se, hello následující adresy URL se přidají toofirewall konfigurace. 
     
  `*.accesscontrol.windows.net:` Používá se k řízení přístupu a správě identit.

  `*.backup.windowsazure.com:` Používá se k orchestraci a přenosu dat replikace.

  `*.blob.core.windows.net:`Použít pro přístup k toohello účet úložiště, který ukládá replikovaná data

  `*.hypervrecoverymanager.windowsazure.com:` Používá se pro orchestraci a operace správy replikací.

  `time.nist.gov`a `time.windows.com`: používá synchronizaci času toocheck mezi systémem a globálním časem.

Adresy URL pro **cloudu Azure Government**:

`* .ugv.hypervrecoverymanager.windowsazure.us`

`* .ugv.backup.windowsazure.us`

`* .ugi.hypervrecoverymanager.windowsazure.us`

`* .ugi.backup.windowsazure.us` 

* **Zkontrolujte, pokud nastavení proxy serveru na Procesovém serveru neblokují přístup**.  Pokud používáte Proxy Server, ověřte, že je server DNS hello řešení hello název proxy serveru.
toocheck co jste zadali v době hello nastavení konfigurace serveru. Přejděte tooregistry klíč

    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings`

Teď zajistěte, že hello stejné nastavení jsou používány data toosend agenta Azure Site Recovery.
Zálohování Microsoft Azure Search 

![Povolení replikace](./media/site-recovery-protection-common-errors/mab.png)

Otevřete ho a klikněte na akci > změnit vlastnosti. Karta Konfigurace proxy serveru měli byste vidět hello proxy adresu, která by měla být stejná, jak je uvedeno nastavení registru hello. Pokud ne, změňte ho toohello stejnou adresu.

![Povolení replikace](./media/site-recovery-protection-common-errors/mabproxy.png)

* **Zkontrolujte, pokud omezení šířky pásma není omezen na Procesovém serveru**: zvýšení hello šířky pásma a zkontrolujte, zda text hello problém stále existuje.

##<a name="next-steps"></a>Další kroky
Pokud potřebujete další pomoc, pak odeslat dotaz příliš[fórum automatické obnovení systému](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Máme aktivní komunitě a jeden z našich technici bude možné tooassist je.
