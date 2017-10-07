---
title: "aaaFail zálohování virtuálních počítačů VMware z Azure na portálu classic hello | Microsoft Docs"
description: "Informace o selhání toohello zpět na místní lokalitu po převzetí služeb při selhání virtuální počítače VMware a fyzické servery tooAzure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 7ca86e21-7a5d-45ab-8f4b-e42da90f199a
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 80bc3ab2708a5df953c6532b353da19a4c44ac34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-toohello-on-premises-site-classic-portal"></a>Selhání back virtuální počítače VMware a fyzické servery toohello místní lokality (portálu classic)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-failback-azure-to-vmware.md)
> * [Portál Azure Classic](site-recovery-failback-azure-to-vmware-classic.md)
> * [Portál Azure Classic (zastaralé)](site-recovery-failback-azure-to-vmware-classic-legacy.md)
>
>

Tento článek popisuje, jak toofail zálohování virtuálních počítačů Azure z Azure toohello místní lokality. Postupujte podle pokynů hello v tomto článku, když jste připravené toofail zálohování virtuálních počítačů VMware nebo fyzických serverů Windows nebo Linuxem po jste při selhání z hello místní lokality tooAzure pomocí této [kurzu](site-recovery-vmware-to-azure-classic.md).

## <a name="overview"></a>Přehled
Tento diagram zobrazuje hello architektura navrácení služeb po obnovení pro tento scénář.

Tato architektura použijte v případě hello procesový server je na místě a používáte ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Tato architektura použijte v případě hello procesový server je v Azure a máte síť VPN nebo připojení typu ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

toosee hello úplný seznam portů a hello navrácení služeb po obnovení architechture diagramu najdete následující toohello obrázek

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Zde je, jak funguje navrácení služeb po obnovení:

* Poté, co jste převzal tooAzure, selhání tooyour zpět na místní lokalitu v několika fázích:
  * **Fáze 1**: tak, aby jejich spuštění replikace zpět tooVMware virtuálních počítačů spuštěných ve vaší místní lokalitě nastavte znovu hello virtuálních počítačích Azure. Povolení nové provedení ochrany přesune hello virtuálních počítačů do skupiny ochrany navrácení služeb po obnovení, který byl automaticky vytvořen, když jste původně vytvořili skupinu ochrany hello převzetí služeb při selhání. Pokud jste přidali váš plán převzetí služeb při selhání ochrany. skupina tooa obnovení pak hello navrácení služeb po obnovení ochrany byla také automaticky přidána skupina toohello plán.  Při opětovné ochrany zadejte, jak tooplan toofail zpět.
  * **Fáze 2**: po virtuální počítače Azure se replikace tooyour na místní lokalitu, spuštění selhání přes toofail zpět z Azure.
  * **Fáze 3**: po vašich dat se nezdařila zpět, je znovu nastavte ochranu hello místní virtuální počítače, které při selhání zálohování, tak, aby jejich spuštění replikace tooAzure.


  [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Failback/player]

### <a name="failback-toohello-original-or-alternate-location"></a>Navrácení služeb po obnovení toohello původního nebo alternativního umístění
Pokud při selhání virtuálního počítače VMware můžete služeb při selhání back toohello stejný zdroj virtuálního počítače, pokud stále existuje místní. V tomto scénáři se jenom hello rozdílové změny, bude zpět. Poznámky:

* Pokud při selhání fyzických serverů a navrácení služeb po obnovení je vždy tooa nového virtuálního počítače VMware.
  * Před selháním fyzický počítač zpět Všimněte si, že
  * Fyzický počítač chráněný vrátí se znovu jako virtuální počítač při převzetí služeb při selhání zpátky do Azure tooVMware
  * Ujistěte se, že zjistíte alespoň jedno hlavní cílový server společně s hello nezbytné ESX/ESXi hostitelů toowhich potřebujete toofailback.
* Pokud jste navrácení služeb po obnovení se vyžaduje toohello původní virtuální počítač hello následující:

  * Pokud hello virtuálního počítače je spravován systémem vCenter server musí mít hostitel ESX hello hlavního cíle úložiště virtuálních počítačů toohello přístup.
  * Pokud hello virtuálního počítače na hostiteli ESX, ale není spravován nástrojem vCenter pak hello hello pevný disk virtuálního počítače musí být v dostupné úložiště dat hello MT na hostitele.
  * Pokud virtuální počítač na hostiteli ESX a nepoužívá vCenter byste měli předtím, než můžete znovu nastavte ochranu dokončit zjišťování hostitele ESX hello hello MT. To platí, pokud po obnovení zpět fyzických serverů příliš.
  * Další možností (Pokud hello místní virtuální počítač existuje) je toodelete ji před provedením navrácení služeb po obnovení. Potom navrácení služeb po obnovení pak vytvoří nový virtuální počítač na hello stejné hostitele jako hostitele ESX hello hlavní cíl.
* Při navrácení služeb po obnovení tooan alternativního umístění hello dat bude obnovená toohello stejné úložiště dat a hello stejného hostitele ESX, který používá hello místní hlavní cílový server.

## <a name="prerequisites"></a>Požadavky
* Budete potřebovat prostředí VMware v pořadí toofail zpět virtuální počítače VMware a fyzické servery. Není-li back tooa fyzický server není podporován.
* V pořadí toofail zpět měli jste vytvořili síť Azure při počátečním nastavení ochrany. Navrácení služeb po obnovení musí připojení VPN nebo ExpressRoute z hello Azure sítě, ve kterém hello virtuálních počítačích Azure se nachází toohello místního webu.
* Pokud virtuální počítače hello chcete toofail back tooare spravuje vCenter server budete potřebovat toomake se, že máte oprávnění hello vyžaduje pro zjišťování virtuálních počítačů na servery vCenter server. [Přečtěte si další informace](site-recovery-vmware-to-azure-classic.md).
* Pokud jsou k dispozici na virtuálním počítači snímky vytvoření selže. Můžete odstranit snímky hello nebo hello disky.
* Než budete navrácení služeb po obnovení budete potřebovat toocreate počet součástí:
  * **Vytvořte procesový server v Azure**. Toto je virtuální počítač Azure budete potřebovat toocreate a dál běžet během navrácení služeb po obnovení. Po dokončení navrácení služeb po obnovení můžete odstranit hello počítače.
  * **Vytvořit hlavní cílový server**: hello hlavní cílový server odesílá a přijímá data navrácení služeb po obnovení. má server pro správu Hello jste vytvořili místní hlavní cílový server ve výchozím nastavení nainstalovaná. V závislosti na svazku hello selhání back provozu však může být zapotřebí toocreate samostatné hlavní cílový server navrácení služeb po obnovení.
  * Pokud chcete toocreate další hlavního cílového serveru se systémem Linux, musíte před instalací hello hlavní cílový server, jak je popsáno níže tooset až hello virtuálního počítače s Linuxem.
* Konfigurační server je místní, jakmile to uděláte navrácení služeb po obnovení. Během navrácení služeb po obnovení musí existovat hello virtuálního počítače v databázi serveru konfigurace hello, nejsou-li být které navrácení služeb po obnovení nebude úspěšné. Proto zajistěte podniknout pravidelné plánované zálohy vašeho serveru. V případě, že z havárie, budete potřebovat toorestore její hello stejné IP adres, aby navrácení služeb po obnovení budou pracovat.

## <a name="set-up-hello-process-server-in-azure"></a>Nastavit hello procesní server v Azure
Budete potřebovat tooinstall procesní server v Azure, aby virtuální počítače Azure hello může odesílat hello data zpět tooon místní hlavní cílový server.

1. Na portálu Site Recovery hello > **konfigurační servery** vyberte tooadd nový procesový server.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)
2. Zadejte název serveru, který proces a zadejte název a heslo, které budete používat tooconnect toohello virtuálního počítače Azure jako správce. V **konfigurační Server** vyberte server pro správu místní hello a zadejte hello Azure sítě, ve které hello procesový server nasadit. To by měl být hello sítě, ve které se nacházejí virtuální počítače Azure hello. Zadejte jedinečnou IP adresu z podsítě vyberte hello a zahájit nasazení.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

   Aktivuje se úloha toodeploy hello procesový server

   ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

   Po hello je procesový server nasadit v Azure, můžete se přihlásit pomocí přihlašovacích údajů hello, které jste zadali. Hello při prvním přihlášení v dialogovém okně server hello proces se spustí. Zadejte IP adresu hello hello místní server pro správu a jeho heslo. Ponechejte výchozí nastavení portu 443 hello. Můžete také ponechat hello výchozí 9443 port pro replikaci dat pokud konkrétně změnit toto nastavení při nastavování serveru pro správu místní hello.

   > [!NOTE]
   > Hello server nebudou viditelné v rámci **vlastnosti virtuálního počítače**. Je viditelný v části hello pouze **servery** ve hello management serveru toowhich je zaregistrován. Může trvat o 10 až 15 minut pro tooappear server hello procesu.
   >
   >

## <a name="set-up-hello-master-target-server-on-premises"></a>Nastavit hello hlavního cílového serveru na místě
hlavní cílový server Hello přijímá data hello navrácení služeb po obnovení. Hlavní cílový server se automaticky nainstaluje na server pro správu místní hello, ale pokud po obnovení zpět velké množství dat může být nutné tooset nahoru Další hlavní cílový server. Proveďte to následujícím způsobem:

> [!NOTE]
> Pokud chcete tooinstall hlavní cílový server v systému Linux, postupujte podle pokynů hello v dalším postupu hello.
>
>

1. Pokud instalujete hello hlavní cílový server v systému Windows, otevřete stránku rychlý Start hello z hello virtuálního počítače, na kterém jste instalaci hello hlavní cílový server a stáhněte instalační soubor hello Průvodce hello Unified instalace nástroje Azure Site Recovery.
2. Spusťte instalační program a v **před zahájením** vyberte **přidat další proces servery tooscale se nasazení**.
3. Dokončení hello průvodce v hello stejně jako jste to udělali při jste [nastavení serveru správy hello](site-recovery-vmware-to-azure-classic.md). Na hello **podrobnosti o konfiguraci serveru** stránky, zadejte IP adresu hello tento hlavní cílový server a přístupové heslo tooaccess hello virtuálních počítačů.

### <a name="set-up-a-linux-vm-as-hello-master-target-server"></a>Nastavení virtuálního počítače s Linuxem jako hlavní cílový server hello
tooset se serverem pro správu hello používajícího hello hlavní cílový server jako Linux virtuálního počítače budete potřebovat tooinstall hello centů) S 6.6 minimální operační systém, načíst hello SCSI identifikátory pro každý SCSI pevný disk, nainstalujte některé další balíčky a vlastní změny.

#### <a name="install-centos-66"></a>Nainstalujte CentOS 6.6
1. Nainstalujte hello CentOS 6.6 minimální operační systém na serveru pro správu hello virtuálních počítačů. Zachovat hello ISO v systému DVD jednotku a spouštěcí hello. Přeskočit hello média testování, vyberte v hello jazyk, vyberte angličtinu **základní zařízení úložiště**, zkontrolujte, zda text hello pevný disk nemá žádné důležitých dat a klikněte na tlačítko **Ano**, zahodit žádná data. Zadejte název hostitele hello hello serveru pro správu a vyberte hello serveru síťový adaptér.  V hello **úpravy systému** dialogovém okně vyberte ** připojit automaticky, ** a přidejte statickou IP adresu, sítě a nastavení služby DNS. Zadejte časové pásmo a server pro správu kořenového hesla tooaccess hello.
2. Pokud dotaz hello typ instalace, byste chtěli vyberte **vytvořit vlastní rozložení** jako hello oddíl. Po kliknutí na tlačítko **Další** vyberte **volné** a klikněte na tlačítko vytvořit. Vytvoření  **/** , **/var/havárií** a **/home oddíly** s **FS typ:** **ext4**. Vytvoření partion hello odkládacího souboru jako **FS typ: prohození**.
3. Pokud již existující zařízení nalezeny, že zobrazí se zpráva upozornění. Klikněte na tlačítko **formátu** tooformat hello jednotku d s nastaveními oddílu hello. Klikněte na tlačítko **zápisu změnit toodisk** tooapply hello oddílu změny.
4. Vyberte **instalace spouštěcí zavaděč** > **Další** tooinstall hello spouštěcí zavaděč na hello kořenové oddílu.
5. Po dokončení instalace klikněte na tlačítko **restartovat**.

#### <a name="retrieve-hello-scsi-ids"></a>Načtení ID SCSI hello
1. Po instalaci načíst hello SCSI identifikátory pro každý pevný disk SCSI v hello virtuálních počítačů. toodo tento vypnout server pro správu hello virtuálních počítačů v hello virtuálních počítačů vlastnosti v prostředí VMware klikněte pravým tlačítkem myši virtuálního počítače položka hello > **upravit nastavení** > **možnosti**.
2. Vyberte **Upřesnit** > **obecné položky** a klikněte na tlačítko **parametry konfigurace**. Tato možnost bude de-active, když hello počítač běží. toomake it active hello počítače musí být vypnuté.
3. Pokud hello řádek **disku. EnableUUID** existuje ověřte hodnotu hello nastavena příliš**True** (malá a velká písmena). Pokud už je můžete zrušit a otestovat hello SCSI příkaz uvnitř hostovaného operačního systému po je při spuštění.
4. Pokud hello řádek není existující klikněte na tlačítko **přidat řádek** – a přidejte ji s hello **True** hodnotu. Nepoužívejte uvozovky.

#### <a name="install-additional-packages"></a>Nainstalujte další balíčky
Budete potřebovat toodownload a některé další balíčky nainstalovat.

1. Zkontrolujte, zda hello hlavní cílový server je připojený toohello Internetu.
2. Spusťte tento příkaz toodownload a nainstalujte 15 balíčky z úložiště CentOS hello: **# yum nainstalovat – y xfsprogs perl lsscsi rsync wget kexec nástroje**.
3. Pokud hello zdrojový počítač, který chráníte běží Linux wit Reiser XFS systém pro kořenový adresář hello souborů nebo spouštěcí zařízení, pak by měla stáhnout a nainstalovat další balíčky následujícím způsobem:

   * # <a name="cd-usrlocal"></a>/usr/local disku CD
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>reiserfs modulu RPM – ivh kmod reiserfs 0,0 1.el6.elrepo.x86_64.rpm-utils-3.6.21-1.el6.elrepo.x86_64.rpm
   * # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>ot. / min – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Použít vlastní změny
Proveďte následující změny vlastní tooapply po jste kroky dokončení hello po instalaci a balíčky nainstalované hello hello:

1. Zkopírujte binární toohello hello RHEL 6-64 Unified Agent virtuálního počítače. Spusťte tento příkaz hello toountar binární: **– zxvf funkce vkládání<file name>**
2. Spusťte tento příkaz toogive oprávnění: **./ApplyCustomChanges.sh chmod &#755;**
3. Spusťte skript hello: **#./ApplyCustomChanges.sh**. Hello skriptu měli spustit jenom jednou. Restartujte hello server po úspěšném spuštění skriptu hello.

## <a name="run-hello-failback"></a>Spustit hello navrácení služeb po obnovení
### <a name="reprotect-hello-azure-vms"></a>Znovu nastavte ochranu virtuálních počítačů Azure hello
1. Na portálu Site Recovery hello > **počítače** Výběr karty hello virtuální počítač, který při selhání a klikněte na tlačítko **znovu nastavit ochranu**.
2. V **hlavní cílový Server** a **procesový Server** vyberte hello místní hlavní cílový server a procesový server hello virtuálního počítače Azure.
3. Vyberte účet hello, které jste nakonfigurovali pro připojení toohello virtuálních počítačů.
4. Vyberte verzi navrácení služeb po obnovení hello hello skupiny ochrany. Například pokud hello virtuální počítač je chráněn v so1, pak musíte tooselect PG1_Failback.
5. Pokud chcete toorecover tooan alternativní umístění, vyberte jednotka pro uchování hello a úložiště dat, které jsou nakonfigurované pro hello hlavní cílový server. Při selhání back toohello místní lokality hello virtuální počítače VMware v plánu ochrany navrácení služeb po obnovení hello hello používat stejné úložiště dat jako hello hlavní cílový server. Pokud chcete, aby toorecover hello repliky virtuálního počítače Azure toohello stejné místní virtuální počítač potom hello místního virtuálního počítače by již měla být v hello stejné úložiště dat, jako hello hlavní cíl serveru. Pokud neexistuje žádný virtuální počítač místně, vytvoří se při vytvoření nového.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)
6. Po kliknutí na tlačítko **OK** toobegin vytvoření úlohy začne tooreplicate hello virtuální počítač z Azure toohello místního webu. Hello průběh můžete sledovat na hello **úlohy** kartě.

   ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-toohello-on-premises-site"></a>Provedení místní toohello převzetí služeb při selhání
Po vytvoření hello virtuálního počítače je verze přesunutý toohello navrácení služeb po obnovení skupiny ochrany a je automaticky přidán toohello plánu obnovení, která jste použili pro tooAzure hello převzetí služeb při selhání, pokud je.

1. V hello **plány obnovení** plán obnovení vyberte hello stránku obsahující skupiny hello navrácení služeb po obnovení a klikněte na tlačítko **převzetí služeb při selhání** > **neplánované převzetí služeb při selhání**.
2. V **potvrzení převzetí služeb při selhání** ověřte hello směr převzetí služeb při selhání (tooAzure) a vyberte bod obnovení hello chcete toouse hello převzetí služeb při selhání (nejnovější). Pokud jste povolili **více virtuálních počítačů** při konfiguraci vlastností replikace můžete obnovit toohello nejnovější aplikace nebo bod konzistentní při selhání obnovení. Klikněte na tlačítko hello zaškrtnutí toostart hello převzetí služeb při selhání.
3. Site Recovery při převzetí služeb při selhání se zastaví hello virtuálních počítačích Azure. Po zkontrolujte, že tento navrácení služeb po obnovení je dokončená, podle očekávání, které můžete můžete zkontrolovat, že hello byl vypnut virtuálních počítačích Azure podle očekávání.

### <a name="reprotect-hello--on-premises-site"></a>Znovu nastavte ochranu hello místního serveru
Po dokončení navrácení služeb po obnovení dat bude zpět na hello místní lokality, ale nebudou chráněné. replikaci tooAzure toostart znovu hello následující:

1. Na portálu Site Recovery hello > **počítače** vyberte hello virtuálních počítačů, které selhaly zpět a klikněte na kartě **znovu nastavit ochranu**.
2. Po ověření, že tento tooAzure replikace funguje podle očekávání, v Azure, můžete odstranit virtuální počítače hello Azure (aktuálně není spuštěn), které byly zpět se nezdařilo.

### <a name="common-issues-in-failback"></a>Běžné problémy při navrácení služeb po obnovení
1. Pokud provádět zjišťování vCenter uživatelů jen pro čtení a chránit virtuální počítače, bude úspěšný a funguje převzetí služeb při selhání. V době hello opětovné ochrany se nezdaří, protože hello datastores nemůže být zjištěny. Jako příznakem neuvidíte hello datastores uvedené při ochranu znovu. tooresolve, můžete aktualizovat pověření vCenter hello odpovídajícímu účtu, který má oprávnění a opakujte úlohu hello. [Další informace](site-recovery-vmware-to-azure-classic.md)
2. Pokud jste navrácení služeb po obnovení virtuálního počítače s Linuxem a potom ho spusťte místní, zobrazí se, že hello správce sítě balíček odinstalován z počítače hello. Je to proto, že při obnovení hello virtuálních počítačů v Azure je odebrán balíček hello správce sítě.
3. Když virtuální počítač je nakonfigurovaný se statickou IP adresou a při selhání tooAzure, hello IP adresa se získávají pomocí protokolu DHCP. Při selhání back tooOn místní hello virtuální počítač dál toouse DHCP tooacquire hello IP adresu. Budete potřebovat toomanually přihlášení do počítače hello a nastavit hello IP adresu zpět tooStatic adresu v případě potřeby.
4. Pokud používáte edice free ESXi 5.5 nebo edice free Hypervisor vSphere 6, by úspěšné převzetí služeb při selhání, ale navrácení služeb po obnovení nebude úspěšné. Budete ned tooupgrade tooeither zkušební licence tooenable navrácení služeb po obnovení.

## <a name="failing-back-with-expressroute"></a>Chybě zpět s ExpressRoute
Vám může selhat zpět přes připojení VPN nebo Azure ExpressRoute. Pokud chcete, aby toouse ExpressRoute Poznámka hello následující:

* ExpressRoute měli nastavit na hello virtuální síť Azure toowhich zdrojového počítače selhání přes a ve kterém virtuální počítače Azure jsou umístěné po převzetí služeb při selhání hello.
* Data jsou replikované tooan účtu úložiště Azure na veřejný koncový bod. Měli byste nastavit veřejný partnerský vztah v ExpressRoute s hello cíl datové centrum pro Site Recovery replikaci toouse ExpressRoute.
