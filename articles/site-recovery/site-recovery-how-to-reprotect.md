---
title: "aaaReprotect z místní lokality Azure tooan | Microsoft Docs"
description: "Po převzetí služeb při selhání tooAzure virtuální počítače můžete zahájit toobring navrácení služeb po obnovení zpět tooon místní virtuální počítače. Zjistěte, jak tooreprotect před navrácení služeb po obnovení."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 94c86e79cba4cd3f0c5821fdd5509cca1d0e7820
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="reprotect-from-azure-tooan-on-premises-site"></a>Nastavte znovu z místní lokality Azure tooan



## <a name="overview"></a>Přehled
Tento článek popisuje, jak tooreprotect Azure virtuálních počítačů z Azure tooan místní lokality. Postupujte podle pokynů hello v tomto článku, když jste připravené toofail zálohování virtuálních počítačů VMware nebo fyzických serverů Windows nebo Linuxem po jste při selhání z hello místní lokality tooAzure (jak je popsáno v [replikovat virtuální VMware počítače a fyzické servery tooAzure s Azure Site Recovery](site-recovery-failover.md)).

> [!WARNING]
> Nelze označit zpět po [dokončení migrace](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), přesunout skupiny prostředků tooanother virtuálního počítače nebo odstranění virtuálního počítače Azure.


Po dokončení vytvoření a hello chráněné virtuální počítače jsou replikací, můžete zahájit navrácení služeb po obnovení na virtuální počítače toobring hello je toohello místního webu.

POST dotazy nebo připomínky můžete na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Rychlý přehled sledujte hello následující video o tom, jak toofail přes z Azure tooan místní lokality.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="prerequisites"></a>Požadavky
Při přípravě tooreprotect virtuálních počítačů trvat nebo zvažte hello následující požadované akce:

* Pokud vCenter server spravuje hello virtuálních počítačů, které chcete toofail zpět do, ujistěte se, že máte hello [požadovaná oprávnění](site-recovery-vmware-to-azure-classic.md) zjišťování virtuálních počítačů na servery vCenter server.

  > [!WARNING]
  > Pokud jsou k dispozici na hello snímky místní hlavní cílový počítač nebo hello virtuální počítač, vytvoření nezdaří. Než budete pokračovat tooreprotect můžete odstranit snímky hello na hlavním cíli hello. snímky Hello na virtuálním počítači hello sloučeny automaticky během úloh opětovné ochrany.

* Předtím, než jste navrácení služeb po obnovení, vytvořte dva další součásti:

  * **Procesový server**: hello [procesový server](site-recovery-vmware-setup-azure-ps-resource-manager.md) přijímá data z hello chráněný virtuální počítač v Azure a odesílá data toohello místního webu. Je vyžadován mezi hello procesový server a hello chráněný virtuální počítač k síti s nízkou latencí. Pokud používáte připojení k Azure ExpressRoute nebo využitím Azure procesového serveru a sítě VPN, proto může obsahovat serveru místní proces.
  
  * **Hlavní cílový server**: hello hlavní cílový server přijímá data navrácení služeb po obnovení. hlavní cílový server nainstalované ve výchozím nastavení má server správy místní Hello, kterou jste vytvořili. V závislosti na hello objem provozu zpět se nezdařilo, však může být zapotřebí toocreate samostatné hlavní cílový server navrácení služeb po obnovení.
    * [Virtuální počítač s Linuxem musí hlavní cílový server Linux](site-recovery-how-to-install-linux-master-target.md).
    * Virtuální počítač Windows musí hlavní cílový server systému Windows. Hello místní proces server a hlavní cílových počítačů můžete použít znovu.

    hlavní cíl Hello má jiné požadavky, které jsou uvedeny v [běžné toocheck věcí na hlavním cíli před opětovné ochrany](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server).

* Konfigurační server je místní, když jste navrácení služeb po obnovení. Během navrácení služeb po obnovení musí existovat hello virtuálního počítače v databázi serveru konfigurace hello. Jinak navrácení služeb po obnovení neúspěšné. 

> [!IMPORTANT]
> Zajistěte, aby podniknout pravidelně naplánovaných záloh konfigurace serveru. Pokud dojde k havárii, obnovte server hello s hello stejné IP adres, který navrácení služeb po obnovení fungovat.

* Sada hello `disk.EnableUUID=true` nastavení v parametry konfigurace hello hello hlavního cílového virtuálního počítače v prostředí VMware. Pokud tento řádek neexistuje, přidejte ji. Toto nastavení je požadovaná tooprovide konzistentní disku virtuálního počítače UUID toohello (VMDK) tak, aby ji připojí správně.

* *Řešení Storage vMotion nelze použít na hlavní cílový server hello*. To může způsobit toofail hello navrácení služeb po obnovení. Hello virtuální počítač nelze spustit, protože hello disky nejsou k dispozici tooit. tooprevent tyto potíže odstranit, vyloučit hello hlavních cílových serverů ze seznamu řešení vMotion.

* Přidejte nový hlavní cílový server toohello jednotky: jednotka pro uchování. Přidejte nový disk a formát hello jednotku.


### <a name="frequently-asked-questions"></a>Nejčastější dotazy

#### <a name="why-do-i-need-a-s2s-vpn-or-an-expressroute-connection-tooreplicate-data-back-toohello-on-premises-site"></a>Proč musím S2S VPN nebo ExpressRoute připojení tooreplicate data zpět toohello místní web?
Vzhledem k tomu může dojít replikace z místní tooAzure přes hello Internetu nebo ExpressRoute připojení, který má veřejného partnerského vztahu, vytvoření a navrácení služeb po obnovení vyžaduje site-to-site (S2S) VPN tooreplicate data. Zadejte síť hello tak, aby hello při selhání virtuálních počítačů v Azure dosáhnout (ping) hello místní konfigurační server. Můžete také nasadit procesový server v hello síť Azure hello při selhání virtuálního počítače. Tento proces server by měl být také možné toocommunicate s hello místní konfigurační server.

#### <a name="when-should-i-install-a-process-server-in-azure"></a>Při instalaci procesový server v Azure?


Hello virtuální počítače na platformě Azure, které chcete tooreprotect odesílají replikace dat tooa procesový server. Nastavení sítě tak, aby hello virtuální počítače na platformě Azure může kontaktovat server hello procesu.

Můžete nasadit procesový server v Azure nebo použít hello existující procesového serveru, které jste použili při převzetí služeb při selhání. důležité tooconsider bodu Hello je hello latence toosend hello data z hello virtuální počítače v Azure toohello procesový server.

Pokud máte připojení typu ExpressRoute nastavení, můžete proces místní server toosend data, protože je nízkou latencí hello mezi hello virtuálního počítače a hello procesového serveru.

 ![Diagram architektury pro ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)



Pokud máte pouze S2S VPN, doporučujeme však nasazení hello procesní server v Azure.

 ![Diagram architektury pro síť VPN](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)


Mějte na paměti, že se stane, že replikace jen přes hello S2S VPN nebo hello privátní partnerský vztah ExpressRoute sítě. Ujistěte se, že dostatečná šířka pásma je k dispozici v daném kanálu sítě.

Informace o instalaci serveru pro postup založené na Azure najdete v tématu [spravovat procesový server běží v Azure](site-recovery-vmware-setup-azure-ps-resource-manager.md).

> [!TIP]
> Doporučujeme používat využitím Azure procesového serveru během navrácení služeb po obnovení. Hello replikace výkon je vyšší, pokud hello procesový server je blíž toohello replikace virtuálního počítače (hello počítače při selhání v Azure). Během testování konceptu (POC) nebo ukázku, ale můžete použít hello místní proces serveru spolu s ExpressRoute s soukromého partnerského vztahu hello toocomplete POC rychlejší.



#### <a name="what-ports-should-i-open-on-different-components-so-that-reprotection-can-work"></a>Jaké porty by měla otevřít na různých komponent, aby mohl pracovat nové provedení ochrany?

![Porty pro převzetí služeb při selhání a navrácení služeb po obnovení](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

#### <a name="which-master-target-server-should-i-use-for-reprotection"></a>Které hlavní cílový server by měl použít pro nové provedení ochrany?
Místní hlavní cílový server se vyžaduje tooreceive data z hello procesový server a pak napište VMDK toohello místního virtuálního počítače. Pokud chráníte virtuální počítače s Windows, potřebujete hlavní cílový server systému Windows. Můžete opakovaně použít hello místně procesový server a hlavní cíl<!-- !todo component -->. Pro virtuální počítače s Linuxem budete potřebovat tooset si dalších místních hlavního cíle Linuxu.


Informace o instalaci hlavní cílový server najdete v tématu:

* [Jak tooinstall Windows hlavní cílový server](site-recovery-vmware-to-azure.md)
* [Jak tooinstall Linux hlavní cílový server](site-recovery-how-to-install-linux-master-target.md)


#### <a name="what-datastore-types-are-supported-on-hello-on-premises-esxi-host-during-failback"></a>Jaké typy úložiště jsou podporovány na hostiteli ESXi místní hello během navrácení služeb po obnovení?

V současné době podporuje Azure Site Recovery selhání zpět pouze tooa systém souborů virtuálního počítače (VMFS) nebo sítě vSAN úložiště. Úložiště dat systému souborů NFS se nepodporuje. Z důvodu omezení toothis zadejte výběr úložiště hello na hello opětovné ochrany obrazovky je prázdný v případě hello datastores systému souborů NFS, nebo zobrazuje hello sítě vSAN úložiště, ale selže během hello úlohy. Pokud máte v úmyslu toofail zpět, můžete vytvořit místní úložiště VMFS a selhání back tooit. Úplné stažení hello VMDK způsobí, že tento navrácení služeb po obnovení.

### <a name="common-things-toocheck-after-completing-installation-of-hello-master-target-server"></a>Běžné věcí toocheck po dokončení instalace hlavní cílový server hello

* Pokud hello je virtuální počítač na místní hello vCenter server, hello hlavní cílový server potřebuje přístup k VMDK toohello místního virtuálního počítače. Přístup je potřebné toowrite hello replikovaných dat toohello disky virtuálního počítače. Zajistěte, aby hello místní virtuální počítač na úložiště dat připojené v hostiteli hello hlavní cíl s přístupem pro čtení a zápis.

* Pokud hello virtuálního počítače není přítomen místně na hello vCenter server, musí hello služba Site Recovery toocreate nového virtuálního počítače během vytvoření. Tento virtuální počítač se vytvoří na hostiteli ESX hello, ve kterém vytvoříte hello hlavní cíl. Vyberte hostitele ESX hello pečlivě, tak, aby na hostiteli hello, který chcete, aby při vytvoření hello navrácení služeb po obnovení virtuálního počítače.

* *Řešení Storage vMotion nelze použít pro hlavní cílový server hello*. To může způsobit toofail hello navrácení služeb po obnovení. Hello virtuální počítač nelze spustit, protože hello disky nejsou k dispozici tooit.

  > [!WARNING]
  > Pokud hlavní cíl zde nevyskytlo úlohu řešení vMotion úložiště po vytvoření, disky hello chráněné virtuální počítače, které jsou připojené toohello hlavní cíl migrovat toohello cíl hello řešení vMotion úloh. Pokud se pokusíte toofail po této, odpojení disku hello nezdaří, protože nejsou disky hello. Pak stává toofind pevné disky hello v účty úložiště. Je třeba toofind je ručně a připojte je toohello virtuálního počítače. Potom můžete spustit hello místní virtuální počítač.

* Přidáte uchování disku tooyour existující Windows hlavní cílový server. Přidejte nový disk a formát hello jednotku. jednotka pro uchování Hello je použité toostop hello body v čase, kdy hello virtuální počítač se replikují toohello zpět na místní lokalitu. Toto jsou některé kritéria jednotka pro uchování. Bez těchto kritérií nebude hello jednotky uvedené pro hello hlavní cílový server.

   * Hello svazku se nepoužívá k žádnému jinému účelu, jako je cílem replikace.

   * Hello svazek není v režimu zamknutí.

   * Hello svazek není svazek mezipaměti. instalace hlavní cíl Hello nesmí existovat na tomto svazku. Hello vlastní instalaci svazek pro cílový server a hlavní proces hello nejsou vhodné pro svazek pro uchovávání. Při instalaci hello procesový server a hlavní cílový svazek hello svazek je svazek mezipaměti hello hlavního cíle.

   * Typ systému souborů Hello hello svazku není FAT nebo FAT32.

   * kapacita svazku Hello je nenulové hodnoty.

   * svazek pro uchovávání dat Hello výchozí pro Windows je hello R svazku.

   * svazek pro uchovávání dat výchozí Hello pro Linux je /mnt/retention.

  > [!IMPORTANT]
  > Pokud používáte existující počítač proces serveru nebo konfigurační server nebo škálování nebo proces serveru/hlavní cílový server počítač musíte tooadd na nový disk. nový disk Hello by měl splňovat hello předcházející požadavky. Jednotka pro uchování hello není přítomna, se nezobrazí v rozevíracím seznamu výběru hello na portálu hello. Po přidání hlavní cíl místní jednotku toohello zabírají too15 minut hello tooappear jednotku v hello výběru na portálu hello. Můžete taky aktualizovat hello konfigurační server, pokud hello jednotka nezobrazí po 15 minutách.

* Virtuální počítač s Linuxem při selhání vyžaduje hlavní cílový server Linux. Při selhání virtuálního počítače s Windows vyžaduje hlavní cílový server systému Windows.

* Nainstalujte nástroje VMware na hello hlavní cílový server. Bez hello nástroje VMware nelze zjistit hello datastores na hostiteli ESXi hello hlavní cíl.

* Povolit hello `disk.EnableUUID=true` parametr hello hlavního cílového virtuálního počítače pomocí vlastností vCenter hello. <!-- !todo Needs link. -->

* hlavní cíl Hello by měl mít alespoň jeden VMFS úložiště připojené. Pokud je none hello **úložiště** vstup na stránku hello opětovné ochrany bude prázdný a nemůže pokračovat.

* hlavní cílový server Hello nemůže mít snímky na discích hello. Pokud existují snímky, vytvoření a navrácení služeb po obnovení nezdaří.

* hlavní cíl Hello nemůže mít řadič Paravirtual SCSI. Hello řadič může být pouze řadič LSI Logic. Bez řadič LSI Logic nové provedení ochrany se nezdaří.

<!--
### Failback policy
tooreplicate back tooon-premises, you will need a failback policy. This policy get automatically created when you create a forward direction policy. Note that

1. This policy gets auto associated with hello configuration server during creation.
2. This policy is not editable.
3. hello set values of hello policy are (RPO Threshold = 15 Mins, Recovery Point Retention = 24 Hours, App Consistency Snapshot Frequency = 60 Mins)
   ![](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

-->

## <a name="steps-tooreprotect"></a>Kroky tooreprotect

> [!NOTE]
> Po spuštění virtuálního počítače v Azure, trvá nějakou dobu, než hello agent tooregister back toohello konfigurační server (až too15 minut). Během této doby došlo selže a chybová zpráva znamená, že tento hello agent není nainstalován. Počkejte několik minut a opakujte akci vytvoření.



1. V **trezoru** > **replikované položky**, klikněte pravým tlačítkem na hello virtuální počítač, který při selhání a pak vyberte **znovu nastavit ochranu**. Můžete také kliknout na hello počítače a vyberte možnost **znovu nastavit ochranu** z hello příkazová tlačítka.
2. V okně hello, Všimněte si, že směr hello ochrany, **Azure tooOn místní**, je již vybrána.
3. V **hlavní cílový Server** a **procesový Server**vyberte hello místní hlavní cílový server a hello procesový server.
4. Pro **úložiště**, vyberte hello toowhich úložiště dat, které chcete toorecover hello disky místní. Tato možnost se používá, když hello místní virtuální počítač odstraní a potřebujete toocreate nové disky. Tato možnost je ignorována, pokud hello disky již existují, ale stále potřebujete toospecify hodnotu.
5. Zvolte jednotka pro uchování hello.
6. navrácení služeb po obnovení zásad Hello je automaticky vybrán.
7. Klikněte na tlačítko **OK** toobegin došlo. Úlohu začne tooreplicate hello virtuální počítač z Azure toohello místního webu. Hello průběh můžete sledovat na hello **úlohy** kartě.

Pokud chcete, aby toorecover tooan alternativního umístění (při odstranění hello místní virtuální počítače), vyberte jednotka pro uchování hello a úložiště dat, které jsou nakonfigurované pro hello hlavní cílový server. Pokud žádnou toohello zpět na místní lokalitu, hello VMware s virtuálními počítači používán plán ochranu navrácení služeb po obnovení hello hello stejné úložiště dat, jako hello hlavní cíl serveru. Nový virtuální počítač se pak vytvoří v vCenter.

Pokud chcete toorecover hello virtuálního počítače na Azure tooan existující místní virtuální počítač, hello připojení místní datastores virtuální počítač s přístupem pro čtení a zápis na hostiteli ESXi hello hlavní cílový server.
    ![Dialogové okno znovu nastavit ochranu](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Znovu nastavte ochranu můžete také na úrovni hello plánu obnovení. Replikační skupiny můžete reprotected pouze prostřednictvím plán obnovení. Pokud jste znovu nastavte ochranu pomocí plán obnovení, je potřeba tooprovide hello hodnoty pro každý chráněný počítač.

> [!NOTE]
> Použití hello stejný hlavní cílový server tooreprotect replikační skupiny. Pokud chcete použít jiném hlavním cílovém serveru tooreprotect skupiny replikace, nemůže hello server poskytovat společný bod v čase.

> [!NOTE]
> Hello místní virtuální počítač je vypnutý během vytvoření. To pomáhá zajistit konzistenci dat během replikace. Nezapínejte hello virtuálního počítače, po dokončení vytvoření.

Po úspěšném vytvoření hello hello virtuální počítač přejde chráněném stavu.

## <a name="next-steps"></a>Další kroky

Po zadání chráněném stavu hello virtuálního počítače můžete [zahájení navrácení služeb po obnovení](site-recovery-how-to-failback-azure-to-vmware.md#steps-to-fail-back). 

navrácení služeb po obnovení Hello vypne hello virtuálního počítače v Azure a spouštěcí hello místní virtuální počítač. Očekávané výpadky aplikace hello. Vyberte čas pro navrácení služeb po obnovení, pokud hello aplikace může tolerovat výpadku.

## <a name="common-problems"></a>Běžné problémy

* Pokud jste použili toocreate šablony virtuálních počítačů, ujistěte se, že každý virtuální počítač má svou vlastní UUID hello disků. Pokud hello místní střetům UUID virtuálního počítače s u hlavního cílového hello vzhledem k tomu, jak byly vytvořeny z hello stejné vytvoření šablony, se nezdaří. Nasadit další hlavní cíl, který ještě nebyl vytvořen z hello stejné šablony.

* Pokud provedení zjišťování vCenter uživatele jen pro čtení a ochrana virtuálních počítačů, je úspěšné ochrany a funguje převzetí služeb při selhání. Při vytvoření převzetí služeb při selhání se nezdaří, protože nemůže být zjištěny hello datastores. Příznakem je, že hello, které nejsou na seznamu datastores během vytvoření. tooresolve tento problém, můžete aktualizovat pověření vCenter hello odpovídajícímu účtu, který má oprávnění, a opakujte úlohu hello. Další informace najdete v tématu [VMware replikovat virtuální počítače a fyzické servery tooAzure s Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).

* Při navrácení služeb po obnovení virtuální počítač s Linuxem a spusťte ho místně, uvidíte, že byl tento balíček správce sítě hello odinstalován z počítače hello. Tato odinstalace se stane, protože balíček správce sítě hello odebrán, pokud hello virtuální počítač bude obnoven v Azure.

* Když virtuální počítač s Linuxem je nakonfigurovaný se statickou IP adresou a při selhání tooAzure, se získávají hello IP adresu z DHCP. Při selhání tooon místní hello virtuální počítač dál toouse DHCP tooacquire hello IP adresu. Ručně přihlásit toohello počítače a v případě potřeby nastavte hello IP adresu zpět tooa statickou adresu. Virtuálního počítače s Windows můžete znovu získat jeho statickou IP adresu.

* Pokud používáte edice free ESXi 5.5 nebo edice free Hypervisor vSphere 6, převzetí služeb při selhání úspěšné, ale navrácení služeb po obnovení nebude úspěšná. tooenable navrácení služeb po obnovení, program upgradu tooeither zkušební licence.

* Pokud není možné dosáhnout hello konfigurační server z hello procesového serveru, použijte Telnet toocheck připojení toohello konfigurační server na portu 443. Můžete také zkusit tooping hello konfigurační server z hello procesový server. Procesový server musí být také prezenční signál, pokud je připojený toohello konfigurační server.

* Pokud se pokoušíte toofail back tooan alternativní vCenter, ujistěte se, zjištěné nové vCenter a hello hlavní cílový server. Typické symptomem je, že hello datastores nejsou přístupné nebo viditelné v hello **znovu nastavte ochranu** dialogové okno.

* Server Windows Server 2008 R2 SP1, který je chráněn jako fyzický místní server nelze zpět z Azure tooan na místní lokalitu se nezdařil.
