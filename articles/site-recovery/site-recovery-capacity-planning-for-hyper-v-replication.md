---
title: "nástroje Plánovač kapacity aaaRun hello technologie Hyper-V pro obnovení lokality | Microsoft Docs"
description: "Tento článek popisuje, jak toorun hello nástroje Plánovač kapacity technologie Hyper-V pro Azure Site Recovery"
services: site-recovery
documentationcenter: na
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 2bc3832f-4d6e-458d-bf0c-f00567200ca0
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: b853598e5cd290c48b59794ba48eefc72ac8ded6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hello-hyper-v-capacity-planner-tool-for-site-recovery"></a>Spuštění nástroje Plánovač kapacity hello technologie Hyper-V pro obnovení lokality

Jako součást nasazení Azure Site Recovery je nutné toofigure replikace a nároky na šířku pásma. nástroje Plánovač kapacity Hello technologie Hyper-V pro obnovení lokality pomáhá vám toodo, pro replikaci virtuálního počítače technologie Hyper-V.

Tento článek popisuje, jak toorun hello nástroje Plánovač kapacity technologie Hyper-V. Tento nástroj slouží spolu s informacemi hello v [plánování kapacity pro Site Recovery](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Než začnete
Spuštění nástroje hello na uzlu serveru nebo clusteru Hyper-V v primární lokalitě. potřebuje toorun hello nástroj hello technologie Hyper-V hostitelské servery:

* Operační systém: Windows Server 2012 nebo 2012 R2
* Paměť: 20 MB (minimálně)
* Procesoru: zatížení 5 procent (minimálně)
* Místo na disku: 5 MB (minimálně)

Před spuštěním nástroje hello, je nutné tooprepare hello primární lokality. Pokud replikujete mezi dvěma místními lokalitami a chcete, aby toocheck šířky pásma, musíte tooprepare i server repliky.

## <a name="step-1-prepare-hello-primary-site"></a>Krok 1: Příprava hello primární lokality

1. V primární lokalitě hello zkontrolujte seznam všech virtuálních počítačů technologie Hyper-V, které chcete hello tooreplicate a na kterých se nachází hostitele nebo clustery hello Hyper-V. Hello nástroj můžete spustit pro více samostatných hostitelů, nebo pro jeden cluster, ale ne obojí společně. Také musí toorun odděleně pro každý operační systém, takže byste měli získat informace o serverech technologie Hyper-V následujícím způsobem:

   * Samostatné servery systému Windows Server 2012
   * Clustery systému Windows Server 2012
   * Samostatné servery Windows Server 2012 R2
   * Clustery systému Windows Server 2012 R2
2. Povolte tooWMI vzdáleného přístupu na všech hostitelích hello Hyper-V a clustery. Tento příkaz spustit na každém serveru clusteru, zda pravidla brány firewall toomake a oprávnění uživatele jsou nastaveny:

        netsh firewall set service RemoteAdmin enable
3. Povolte monitorování výkonu u serverů a clusterů, následujícím způsobem:

   * Otevřete hello brány Windows Firewall s hello **pokročilým zabezpečením** modul snap-in, a potom povolit hello následující příchozí pravidla: **síťového přístupu COM + (DCOM-IN)** a všechna pravidla v hello **vzdáleného protokolu událostí Skupina pro správu**.

## <a name="step-2-prepare-a-replica-server-on-premises-tooon-premises-replication"></a>Krok 2: Příprava serveru repliky (místní tooon místní replikace)
Nebudete potřebovat, toodo Pokud replikujete tooAzure.

Doporučujeme že nastavit jeden hostitel Hyper-V jako server pro obnovení, tak, aby fiktivní virtuální počítač může být replikované tooit toocheck šířky pásma.  To můžete přeskočit, ale pokud se nebudete moct toomeasure šířky pásma.

1. Pokud chcete, aby toouse uzlu clusteru jako repliky hello konfigurace zprostředkovatele replik technologie Hyper-V:

   * V **správce serveru**, otevřete **Správce clusteru převzetí služeb při selhání**.
   * Připojení clusteru toohello, zvýrazněte hello název clusteru a klikněte na tlačítko **akce** > **konfigurovat roli** Průvodce vysokou dostupností tooopen hello.
   * V **vybrat roli**, klikněte na tlačítko **zprostředkovatele replik technologie Hyper-V**. V Průvodci hello poskytují **název pro rozhraní NetBIOS** a hello **IP adresu** toobe použít jako hello připojení toohello bod clusteru (označovaný jako klientský přístupový bod). Hello **zprostředkovatele replik technologie Hyper-V** bude nakonfigurován, výsledkem je název klientského přístupového bodu, který si všimněte.
   * Ověřte, že hello role zprostředkovatele replik technologie Hyper-V úspěšně přepne do online a mohou přecházet mezi všemi uzly v clusteru hello. toodo, klikněte pravým tlačítkem na roli hello, přejděte příliš**přesunout**a potom klikněte na **vybrat uzel**. Vyberte uzel > **OK**.
   * Pokud používáte ověřování pomocí certifikátů, zkontrolujte, že každý uzel clusteru a hello klientský přístupový bod všechny mají nainstalovaný certifikát hello.
2. Povolte serveru repliky:

   * Pro cluster s podporou otevřete Správce clusteru selhání, připojte toohello cluster a klikněte na tlačítko **role** > vyberte role > **nastavení replikace** > **povolit tento cluster jako replika Server**. Pokud používáte cluster jako hello repliky, musíte toohave hello zprostředkovatele replik technologie Hyper-V role přítomná v clusteru hello v hello primární lokality.
   * Samostatný server otevřete Správce technologie Hyper-V. V hello **akce** podokně klikněte na tlačítko **nastavení technologie Hyper-V** pro hello server chcete tooenable a v **konfiguraci replikace** klikněte na tlačítko **povolit počítač jako server repliky**.
3. Nastavení ověřování:

   * V **ověřování a porty**vyberte, jak tooauthenticate hello primární server a porty ověřování hello. Pokud používáte klikněte na certifikát **vybrat certifikát** tooselect jeden. Používat protokol Kerberos, pokud jsou hello primárními a obnovovacími hostitelů Hyper-V v hello stejné doméně, nebo v důvěryhodných doménách. Použijte certifikáty pro jiné domény nebo pracovní skupiny nasazení.
   * V **ověřování a úložiště**, povolit **žádné** ověřeného (primárního) serveru toosend replikace dat toothis serveru repliky.

     ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)
   * Spustit **netsh http show servicestate**, toocheck pro hello protokolu nebo je zadaný port je spuštěný naslouchací proces této hello:  
4. Nastavení brány firewall. Během instalace technologie Hyper-V se pravidla brány firewall na hello výchozí porty (HTTPS na 443, protokolu Kerberos na 80) vytvoří tooallow provoz. Tato pravidla povolte následujícím způsobem:
  - Ověření certifikátu v clusteru (443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
  - Ověřování protokolem Kerberos v clusteru (80):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
  - Ověření certifikátu na samostatný server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
  - Ověřování protokolu Kerberos na samostatný server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-hello-capacity-planner-tool"></a>Krok 3: Spuštění nástroje Plánovač kapacity hello
Poté, co jste připravili primární lokalitu a nastavit server pro obnovení, mohou spouštět nástroj hello.

1. [Stáhněte si](https://www.microsoft.com/download/details.aspx?id=39057) hello nástroj z hello Microsoft Download Center.
2. Spusťte nástroj hello z jedním z primárních serverů hello (nebo jeden z hello uzlů z clusteru primární hello). Klikněte pravým tlačítkem na soubor .exe hello a potom vyberte **spustit jako správce**.
3. V **před zahájením**, zadejte, jak dlouho chcete toocollect data. Doporučujeme, že spusťte nástroj hello během provozní hodiny tooensure, že data jsou zástupce. Pokud zkoušíte pouze toovalidate připojení k síti, můžete shromáždit pro pouze několik minut.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)
4. V **podrobnosti primární lokality**, zadejte název serveru hello nebo plně kvalifikovaný název domény pro samostatný hostitel nebo pro cluster s podporou hello plně kvalifikovaný název domény klienta hello přijmout bodu, název clusteru nebo libovolného uzlu v clusteru hello a pak klikněte na **Další**. Nástroj Hello automaticky rozpozná hello název hello serveru, který je spuštěn na. Hello nástroj příjmem virtuálních počítačů, které jde monitorovat hello zadané servery.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)
5. V **podrobnosti lokality repliky**, pokud se při replikaci tooAzure, nebo pokud se při replikaci tooa sekundárního datového centra a nenastavili server repliky, vyberte **zahrnující lokality repliky testy přeskočte**. Pokud replikujete tooa sekundárního datového centra a nastavili jste si typ repliky, zadejte plně kvalifikovaný název domény hello samostatný server nebo hello klientský přístupový bod pro hello clusteru v **Server name (nebo) CAP zprostředkovatele repliky technologie Hyper-V**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)
6. V **rozšířené repliky podrobnosti**, povolit **přeskočit hello testy zahrnující lokality rozšířených replik**. Tyto testy nejsou podporovány službou Site Recovery.
7. V **vyberte virtuální počítače tooReplicate**, nástroje hello připojí toohello serveru nebo clusteru a zobrazí virtuálních počítačů, a disky spuštěných na primárním serveru hello, v souladu s nastavením hello jste zadali na hello **podrobnosti primární lokality**  stránky. Virtuální počítače, které již jsou povoleny pro replikaci nebo který neběží, nebudou zobrazeny. Vyberte hello virtuální počítače, pro které chcete toocollect metriky. Výběr virtuálních pevných disků hello automaticky příliš shromažďuje data pro hello virtuálních počítačů.
8. Pokud jste nakonfigurovali na serveru repliky nebo clusteru, v **sítě informace**, hello přibližnou šířky pásma sítě WAN si myslíte, bude použit mezi lokalitami hello primárním serverem a repliky a certifikáty vyberte hello, pokud jste nakonfigurovali ověřování pomocí certifikátu.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)
9. V **Souhrn**, zkontrolujte hello nastavení a klikněte na **Další** toobegin shromažďování metrik. Nástroj průběh a stav se zobrazí na hello **vypočítat kapacitu** stránky. Po ukončení běhu hello nástroje, klikněte na tlačítko **zobrazit sestavu** tooview hello výstup. Ve výchozím nastavení, sestavy a protokoly jsou uloženy v **%systemdrive%\Users\Public\Documents\Capacity Planner**.

   ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-hello-results"></a>Krok 4: Interpretovat výsledky hello

Tady jsou důležité metriky hello. Metriky, které zde nejsou uvedeny, můžete ignorovat. Nejsou relevantní pro Site Recovery.

### <a name="on-premises-tooon-premises-replication"></a>Místní tooon místní replikace

* Dopad replikace na výpočty hello primárním hostitelem, a paměti
* Dopad replikace na hello primární, místo na disku úložiště obnovení hostitelů, IOPS
* Celková šířka pásma vyžadovaná pro replikaci rozdílů (MB/s)
* Zjištěnou šířku pásma sítě mezi primárním hostitelem hello a hello hostiteli obnovení (MB/s)
* Návrh pro hello ideální počet active paralelní přenosů mezi dvěma hello hostitele nebo clustery

### <a name="on-premises-tooazure-replication"></a>Místní tooAzure replikace

* Dopad replikace na výpočty hello primárním hostitelem, a paměti
* Dopad replikace hello primárním hostitelem úložiště místa na disku, IOPS
* Celková šířka pásma vyžadovaná pro replikaci rozdílů (MB/s)

## <a name="more-resources"></a>Další zdroje informací
* Podrobné informace o nástroji hello přečíst hello dokumentu, který doprovází hello nástroj pro stažení.
* Podívejte se na návod hello nástroj na Keithema Mayera [blog na TechNetu](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [Získat výsledky hello](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) z našich testování výkonu pro replikaci mezi lokálními tooon místní technologie Hyper-V

## <a name="next-steps"></a>Další kroky

Po dokončení plánování kapacity můžete začít nasazovat Site Recovery:

* [Replikace virtuálních počítačů technologie Hyper-V v tooAzure cloudy VMM](site-recovery-vmm-to-azure.md)
* [Replikovat virtuální počítače Hyper-V (bez VMM) tooAzure](site-recovery-hyper-v-site-to-azure.md)
* [Replikace virtuálních počítačů technologie Hyper-V mezi lokalitami VMM](site-recovery-vmm-to-vmm.md)
