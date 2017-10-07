---
title: "virtuální počítače VMware aaaFailback z Azure tooon místní | Microsoft Docs"
description: "Informace o selhání toohello zpět na místní lokalitu po převzetí služeb při selhání virtuální počítače VMware a fyzické servery tooAzure."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 5a47337f-89a9-43e8-8fdc-7da373c0fb0f
ms.service: site-recovery
ms.devlang: na
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: ruturajd
ms.openlocfilehash: 258f5a55252083135b2040e5a235fa1ffbf3b9d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-vmware-virtual-machines-and-physical-servers-toohello-on-premises-site"></a>Selhání back virtuální počítače VMware a fyzické servery toohello místního serveru


Tento článek popisuje, jak toofailback Azure virtuálních počítačů z Azure toohello místní lokality. Postupujte podle pokynů hello zde až budete připraveni toofail zálohování virtuálních počítačů VMware nebo fyzických serverů systému Windows nebo Linux po opětovné zajištění ochrany vašich počítačů pomocí této [odkaz](site-recovery-how-to-reprotect.md).

>[!NOTE]
>Pokud používáte portál Azure classic hello, naleznete v tooinstructions uvedených [sem](site-recovery-failback-azure-to-vmware-classic.md) pro rozšířené architekturu tooAzure VMware a [sem](site-recovery-failback-azure-to-vmware-classic-legacy.md) pro starší verze architektury hello

## <a name="overview"></a>Přehled
diagramy Hello v této části ukazují hello architektura navrácení služeb po obnovení pro tento scénář.

Když používáte připojení k Azure ExpressRoute hello procesový Server je místní, použijte tato architektura:

![Diagram architektury pro ExpressRoute](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Pokud mají hello, které procesový Server je v Azure a síť VPN nebo připojení ExpressRoute, použijte tato architektura:

![Diagram architektury pro síť VPN](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Úplný seznam portů a diagram architektury hello navrácení služeb po obnovení najdete v části toohello následující bitové kopie:

![Převzetí služeb při selhání-navrácení služeb po obnovení seznamu všechny porty](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Poté, co jste převzal tooAzure, nedodržíte tooyour zpět na místní lokalitu ve třech fázích:

* **Fáze 1**: tak, aby jejich spuštění replikace virtuálních počítačů VMware back toohello, na místní lokalitu nastavte znovu hello virtuálních počítačích Azure.
* **Fáze 2**: po virtuální počítače Azure jsou replikované tooyour místního serveru, je spustit převzetí služeb při selhání toofail zpět z Azure.
* **Fáze 3**: po vašich dat se nezdařila zpět, je znovu nastavte ochranu hello místní virtuální počítače, které při selhání zálohování, tak, aby jejich spuštění replikace tooAzure.

### <a name="fail-back-toohello-original-location-or-an-alternate-location"></a>Selhání back toohello původního umístění nebo alternativního umístění
Poté, co jste převzetí služeb při selhání virtuálního počítače VMware, může selhat back toohello stejný zdroj virtuálního počítače, pokud stále existuje místní. V tomto scénáři pouze hello rozdílů došlo k selhání zpět.

Pokud jste při selhání fyzické servery, navrácení služeb po obnovení je vždy tooa nového virtuálního počítače VMware. Před selháním zpět fyzického počítače, Všimněte si, že:
* Chráněné fyzický počítač vrátí se znovu jako virtuální počítač, pokud je při selhání zpátky do Azure tooVMware. Fyzický počítač systému Windows Server 2008 R2 SP1, pokud je chráněný a převzal tooAzure, nemůže se zpět. Windows Server 2008 R2 SP1 počítač, který spuštěna jako virtuální počítač na místě bude možné toofail zpět.
* Ujistěte se, že zjistíte alespoň jeden hlavní cílový server společně s hello nezbytné hostitelů ESX/ESXi, je nutné, aby toofail zpět do.

Pokud selže back toohello původní virtuální počítač, hello vyžadují se následující věci:

* Pokud hello virtuálního počítače je spravovaná službou vCenter server, hello hostitele ESX hlavní cíl měli přístup toohello virtuální počítače úložiště.
* Pokud hello virtuálního počítače na hostiteli ESX, ale není spravován nástrojem vCenter, hello hello pevný disk virtuálního počítače musí být v úložišti dat, která je přístupná pomocí hello MT na hostiteli.
* Pokud virtuální počítač je na hostiteli ESX a nepoužívá vCenter, byste měli dokončit zjišťování hostitele ESX hello hello MT předtím, než můžete znovu nastavte ochranu. To platí, pokud po obnovení zpět fyzických serverů příliš.
* Další možností (Pokud hello místní virtuální počítač existuje) je toodelete ji před provedením navrácení služeb po obnovení. Navrácení služeb po obnovení pak vytvoří nový virtuální počítač na hello stejné hostitele jako hostitele ESX hello hlavní cíl.

Pokud žádnou back tooan alternativní umístění, hello dat je obnovené toohello stejné úložiště dat a hello stejného hostitele ESX, který používá hello místní hlavní cílový server.

## <a name="prerequisites"></a>Požadavky
* toofail zpět virtuální počítače VMware a fyzické servery, budete potřebovat prostředí VMware. Není-li back tooa fyzický server není podporován.
* zpět toofail musí jste vytvořili síť Azure při počátečním nastavení ochrany. Navrácení služeb po obnovení musí připojení VPN nebo ExpressRoute z hello Azure sítě, ve kterém hello virtuálních počítačích Azure se nachází toohello místního webu.
* Pokud hello virtuálních počítačů, které chcete toofail zpět tooare spravuje vCenter server, ujistěte se, zda máte oprávnění hello vyžaduje pro zjišťování virtuálních počítačů na servery vCenter server. Další informace najdete v tématu [VMware replikovat virtuální počítače a fyzické servery tooAzure s Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).
* Pokud snímky jsou v něm na virtuálním počítači, nové provedení ochrany se nezdaří. Můžete odstranit snímky hello nebo hello disky.
* Předtím, než jste navrácení služeb po obnovení, vytvořte tyto komponenty:
  * **Vytvořte procesový Server v Azure**. Tato součást je virtuální počítač Azure, které vytvoříte a dál běžet během navrácení služeb po obnovení. Po dokončení navrácení služeb po obnovení můžete odstranit hello virtuálních počítačů.
  * **Vytvořit hlavní cílový server**: hello hlavní cílový server odesílá a přijímá data navrácení služeb po obnovení. hlavní cílový server, který je nainstalován ve výchozím nastavení má server správy Hello, které jste vytvořili místní. V závislosti na hello objem provozu zpět se nezdařilo, však může být zapotřebí toocreate samostatné hlavní cílový server navrácení služeb po obnovení.
  * toocreate další hlavní cílový server, který běží na systému Linux, nastavte hello virtuálního počítače s Linuxem před instalací hello hlavní cílový server, jak je popsáno dále.
* Konfigurační server je místní, jakmile to uděláte navrácení služeb po obnovení. Během navrácení služeb po obnovení musí existovat hello virtuálního počítače v databázi serveru konfigurace hello. Pokud hello konfigurační server databáze obsahuje žádné virtuálních počítačů, nelze úspěšně dokončit navrácení služeb po obnovení. Proto zajistěte, abyste vytvořili pravidelných naplánovaných záloh serveru. Po havárii, je nutné toorestore její hello stejnou IP adresu, že navrácení služeb po obnovení funguje.
* Nastavení disk.enableUUID=true hello **parametry konfigurace** hello hlavního cíle virtuálních počítačů v prostředí VMware. Pokud tento řádek neexistuje, přidejte ji. Toto nastavení je požadovaná tooprovide konzistentní identifikátor UUID (UUID) toohello virtuálního počítače (VMDK) souboru na disku tak, aby se správně připojené.
* Mějte "hlavní cílový server nemůže být úložiště vMotioned" stav, který může způsobit toofail hello navrácení služeb po obnovení. Hello virtuálních počítačů nelze vyvolat, protože disky hello nejsou provedeny tooit k dispozici.
* Přidáním jednotky, nazývá jednotka pro uchování, do hello hlavní cílový server. Přidejte disk a formátování disku hello.

## <a name="failback-policy"></a>Navrácení služeb po obnovení zásad
tooreplicate back tooon místní, musíte zásadu navrácení služeb po obnovení. zásady Hello se automaticky vytvoří, když vytvoříte zásadu směru dopředu a se automaticky přidruží hello konfigurační server. Se nedá upravovat. zásady Hello je hello následující nastavení replikace:

* Prahová hodnota plánovaného bodu obnovení = 15 minut
* Uchování bodu obnovení = 24 hodin
* Frekvence snímků konzistentní vzhledem = 60 minut

 ![Nastavení replikace hello navrácení služeb po obnovení zásad](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

## <a name="set-up-a-process-server-in-azure"></a>Nastavit procesní Server v Azure
Nainstalujte procesní Server v Azure tak, aby virtuální počítače Azure hello můžete odesílat hello data zpět toohello místní hlavní cílový server.

Pokud chráníte virtuální počítače jako klasické prostředky (hello obnovit virtuální počítač v Azure je virtuální počítač, který byl vytvořen pomocí modelu nasazení classic hello), budete potřebovat procesový Server v Azure. Pokud jste obnovili hello virtuálních počítačů pomocí Azure Resource Manageru jako typ nasazení hello, hello procesový Server musí mít správce prostředků jako typ nasazení hello. Hello typ nasazení je vybrána ve hello Azure hello virtuální síť, kterou můžete nasadit procesový Server na.

1. V **trezoru** > **nastavení** > **infrastruktury obnovení lokality** (v části **spravovat**) > **Konfigurační servery** (v části **pro VMware a fyzické počítače**), vyberte hello konfigurační server.
2. Klikněte na tlačítko **zpracovat serveru**.

  ![Proces serveru stisknutím tlačítka](./media/site-recovery-failback-azure-to-vmware-classic/add-processserver.png)
3. Zvolte toodeploy hello procesový Server jako **nasadit procesový Server v Azure, navrácení služeb po obnovení**.
4. Vyberte hello předplatné, které jste obnovili hello virtuální počítače.
5. Vyberte síť Azure, kterou jste obnovili hello virtuálních počítačů do hello. Hello procesový Server potřebuje toobe v hello stejné sítě, aby hello obnovit virtuální počítače a hello procesový Server může komunikovat.
6. Pokud jste vybrali *modelu nasazení classic* sítě, vytvoření virtuálního počítače prostřednictvím hello Azure Marketplace a poté nainstalujte hello procesový Server v něm.

 ![okno Hello "Přidat Server proces"](./media/site-recovery-failback-azure-to-vmware-classic/add-classic.png)

 Jak vytváříte hello procesový Server, věnovat pozornost toohello následující:
 * Název Hello hello bitové kopie je *Microsoft Azure Site obnovení proces serveru V2*. Vyberte **Classic** jako model nasazení hello.

       ![Select "Classic" as hello Process Server deployment model](./media/site-recovery-failback-azure-to-vmware-classic/templatename.png)
 * Nainstalujte hello procesový Server podle toohello pokyny v [VMware replikovat virtuální počítače a fyzické servery tooAzure s Azure Site Recovery](site-recovery-vmware-to-azure-classic.md).
7. Pokud vyberete hello *Resource Manager* Azure síť, nasazení hello procesového serveru tím, že poskytuje hello následující informace:

  * Hello název skupiny prostředků hello, který chcete toodeploy hello server
  * Hello název serveru hello
  * Uživatelské jméno a heslo pro přihlášení toohello serveru
  * Hello účtu úložiště, které chcete toodeploy hello server
  * Hello podsíť a hello síťové rozhraní, které chcete tooconnect tooit
   >[!NOTE]
   >Musíte vytvořit vlastní [síťové rozhraní](../virtual-network/virtual-networks-multiple-nics.md) (NIC) a vyberte ho při nasazování hello procesový Server.

    ![Zadejte informace v dialogovém okně hello "Přidat Server proces"](./media/site-recovery-failback-azure-to-vmware-classic/psinputsadd.png)

8. Klikněte na **OK**. Tato akce spustí úlohu, která vytvoří virtuální počítač typu nasazení Správce prostředků během instalace procesový Server hello. hello tooregister hello serveru toohello konfigurační server, instalační program spusťte hello uvnitř virtuálního počítače podle pokynů hello v [VMware replikovat virtuální počítače a fyzické servery tooAzure s Azure Site Recovery](site-recovery-vmware-to-azure-classic.md). Aktivuje se také úlohy toodeploy hello procesový Server.

  Hello procesový Server je uveden na hello **konfigurační servery** > **přidružené servery** > **proces servery** kartě.

    ![](./media/site-recovery-failback-azure-to-vmware-new/pslistingincs.png)

    > [!NOTE]
    > Hello procesový Server není viditelné v rámci **vlastnosti virtuálního počítače**. Je zobrazeno pouze na hello **servery** kartě v serveru pro správu hello, který je zaregistrován. Může trvat 10 minut too15 tooappear procesový Server hello.


## <a name="set-up-hello-master-target-server-on-premises"></a>Nastavit hello hlavního cílového serveru na místě
hlavní cílový server Hello přijímá data hello navrácení služeb po obnovení. Hello server se automaticky nainstaluje na server pro správu místní hello, ale pokud po obnovení zpět příliš velké množství dat, může být nutné tooset nahoru Další hlavní cílový server. tooset až hlavního cíle místní server, hello následující:

> [!NOTE]
> tooset hlavní cílový server v systému Linux, přeskočte toohello další postup. Použijte pouze CentOS 6.6 minimální operační systém jako hello hlavní cílový operační systém.

1. Pokud nastavujete hello hlavní cílový server v systému Windows, otevřete stránku hello úvodní z hello virtuální počítač, který instalujete na hello hlavní cílový server.
2. Stáhněte instalační soubor hello Průvodce hello Unified instalace nástroje Azure Site Recovery.
3. Spusťte instalační program hello a v **před zahájením**, vyberte **přidat další servery proces tooscale se nasazení**.
4. Dokončení hello průvodce v hello stejně jako jste to udělali při jste [nastavení serveru správy hello](site-recovery-vmware-to-azure-classic.md). Na hello **podrobnosti o konfiguraci serveru** stránky, zadejte IP adresu hello hello hlavní cílový server a zadejte přístupové heslo tooaccess hello virtuálních počítačů.

### <a name="set-up-a-linux-vm-as-hello-master-target-server"></a>Nastavení virtuálního počítače s Linuxem jako hlavní cílový server hello
tooset se serverem pro správu hello používajícího hello hlavní cílový server jako virtuální počítač s Linuxem, nainstalujte hello CentOS 6.6 minimální operační systém. V dalším kroku načíst hello SCSI identifikátory pro každý SCSI pevný disk, nainstalujte některé další balíčky a vlastní změny.

#### <a name="install-centos-66"></a>Nainstalujte CentOS 6.6

1. Nainstalujte hello CentOS 6.6 minimální operační systém na serveru pro správu hello virtuálních počítačů. Ponechte hello ISO v jednotce DVD a spouštěcím systémem hello. Přeskočte testování hello média. Vyberte **angličtinu** jako hello jazyk, vyberte **základní zařízení úložiště**, zkontrolujte, zda text hello pevný disk nemá žádná důležitá data, klikněte na **Ano**a zrušení žádná data. Zadejte název hostitele hello hello serveru pro správu a vyberte hello serveru síťový adaptér.  V hello **úpravy systému** dialogové okno, vyberte **připojit automaticky,**a poté přidejte statickou IP adresu, sítě a nastavení služby DNS. Zadejte časové pásmo. tooaccess hello serveru pro správu, zadejte hello kořenové heslo.
2. Jakmile se zobrazí výzva, jaký typ instalace chcete, vyberte **vytvořit vlastní rozložení** jako hello oddíl. Klikněte na **Další**. Vyberte **volné**a potom klikněte na **vytvořit**. Vytvoření  **/** , **/var/havárií**, a **/home oddíly** s **FS typ:** **ext4**. Vytvořit oddíl hello odkládacího souboru jako **FS typ: prohození**.
3. Pokud se najde existující zařízení, zobrazí se zpráva s upozorněním. Klikněte na tlačítko **formátu** tooformat hello jednotku d s nastaveními oddílu hello. Klikněte na tlačítko **zápisu změnit toodisk** tooapply hello oddílu změny.
4. Vyberte **instalace spouštěcí zavaděč** > **Další** tooinstall hello spouštěcí zavaděč na hello kořenové oddílu.
5. Po dokončení instalace hello klikněte na tlačítko **restartovat**.

#### <a name="retrieve-hello-scsi-ids"></a>Načtení ID SCSI hello

1. Po instalaci hello načíst hello SCSI identifikátory pro každý pevný disk SCSI v hello virtuálních počítačů. toodo tedy vypnout server pro správu hello virtuálních počítačů. Ve vlastnostech hello virtuálních počítačů v prostředí VMware, klikněte pravým tlačítkem na položku hello virtuálního počítače > **upravit nastavení** > **možnosti**.
2. Vyberte **Upřesnit** > **obecné položky**a potom klikněte na **parametry konfigurace**. Tato možnost není dostupná, když hello počítač běží. Pro možnost toobe hello k dispozici musí být hello počítač vypnout.
3. Proveďte jednu z následujících hello:
 * Pokud hello řádek **disku. EnableUUID** existuje, ujistěte se, že hodnota hello nastavena příliš**True** (malá a velká písmena). Pokud hodnota hello už je nastavené tooTrue, můžete zrušit a otestovat hello SCSI příkaz uvnitř hostovaného operačního systému po je při spuštění.
 * Pokud hello řádek **disku. EnableUUID** neexistuje, klikněte na tlačítko **přidat řádek**a poté je přidejte s hello **True** hodnotu. Nepoužívejte dvojitých uvozovek.

#### <a name="install-additional-packages"></a>Nainstalujte další balíčky
Stáhněte a nainstalujte další balíčky.

1. Zkontrolujte, zda text hello hlavní cílový server je připojený toohello Internetu.
2. toodownload a instalační balíčky 15 z úložiště hello CentOS, spusťte tento příkaz: `# yum install –y xfsprogs perl lsscsi rsync wget kexec-tools`.
3. Pokud chráníte hello zdrojového počítače se systémem Linux s Reiser nebo XFS systém pro kořenovou nebo spouštěcí zařízení hello souborů, stáhněte a nainstalujte další balíčky následujícím způsobem:

   * \#/usr/local disku CD
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
   * \#wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
   * \#reiserfs modulu RPM – ivh kmod reiserfs 0,0 1.el6.elrepo.x86_64.rpm-utils-3.6.21-1.el6.elrepo.x86_64.rpm
   * \#wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
   * \#ot. / min – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm
   * \#YUM instalaci zařízení multipath mapper (vícenásobný balíčků pro požadované tooenable na hello hlavní cílový server)

#### <a name="apply-custom-changes"></a>Použít vlastní změny
Po dokončení kroků po instalaci hello a hello nainstalované balíčky, použít vlastní změny pomocí tohoto postupu hello následující:

1. Zkopírujte binární toohello hello RHEL 6-64 Unified Agent virtuálního počítače. toountar hello binární, spusťte tento příkaz: `tar –zxvf <file name>`.
2. toogive oprávnění, spusťte tento příkaz: `# chmod 755 ./ApplyCustomChanges.sh`.
3. Spuštění hello následující skript: `# ./ApplyCustomChanges.sh`. Spusťte jenom jednou. Po úspěšném spuštění, restartujte hello server.

## <a name="run-hello-failback"></a>Spustit hello navrácení služeb po obnovení
### <a name="reprotect-hello-azure-vms"></a>Znovu nastavte ochranu virtuálních počítačů Azure hello
1. V **trezoru**v **replikované položky**, klikněte pravým tlačítkem na hello virtuální počítač, který se po selhání a pak vyberte **znovu nastavit ochranu**.
2. V okně hello, zobrazí tento směr hello ochrany **Azure tooOn místní** je již vybrána.
3. V **hlavní cílový Server** a **procesový Server**vyberte hello místní hlavní cílový server a hello serveru procesu virtuálního počítače Azure.
4. Vyberte hello úložiště dat, které chcete toorecover hello disky místním nasazením a. Tuto možnost použijte, když hello místní virtuální počítač se odstraní a potřebujete toocreate nové disky. Možnost hello ignorujte, pokud hello disky již existují, ale stále potřebujete toospecify hodnotu.
5. Použijte uchování disku toostop hello body v čas, kdy hello virtuálního počítače je replikované back tooon místní. Jsou zde uvedeny některé kritéria jednotka pro uchování. Bez těchto kritérií není uvedené hello jednotky pro hello hlavní cílový server.

  * Svazek by neměl být používán k žádnému jinému účelu (Cílem replikace a tak dále).
  * Svazek by neměl být v režimu zámku.
  * Svazek by neměl být svazkem mezipaměti. (instalace hlavní cíl hello nesmí existovat na tomto svazku. Hello procesový Server a hlavní cílový svazek vlastní instalaci nejsou vhodné pro svazek pro uchovávání. Zde hello nainstalován procesový Server a hlavní cílový svazek je svazek mezipaměti hello hello hlavního cíle.)
  * Typ systému souborů svazku Hello by neměl být FAT a FAT32.
  * kapacita svazku Hello by měl být nulová.
  * svazek pro uchovávání dat Hello výchozí pro Windows je R svazku.
  * svazek pro uchovávání dat výchozí Hello pro Linux je /mnt/retention.

6. navrácení služeb po obnovení zásad Hello je automaticky vybrán.
7. Po kliknutí na tlačítko **OK** toobegin došlo, úlohu začne tooreplicate hello virtuální počítač z Azure toohello místního webu. Hello průběh můžete sledovat na hello **úlohy** kartě.

Pokud chcete toorecover tooan alternativní umístění, vyberte jednotka pro uchování hello a úložiště dat, které jsou nakonfigurované pro hello hlavní cílový server. Pokud žádnou toohello zpět na místní lokalitu, hello virtuální počítače VMware v plánu ochrany navrácení služeb po obnovení hello použít hello stejné úložiště dat jako hello hlavní cílový server. Pokud chcete, aby toorecover hello repliky virtuálního počítače Azure toohello stejné místní virtuální počítač, hello místní virtuální počítač musí již být v hello stejné úložiště dat jako hello hlavní cílový server. Pokud žádný virtuální počítač na místě, vytvoří se během vytvoření novou.

![V rozevírací nabídce hello vyberte "Azure místní tooon"](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Znovu nastavte ochranu můžete také na úrovni plánu obnovení. Pokud máte skupinu replikace, můžete jen pomocí plán obnovení nastavte ji znovu. Pokud jste znovu nastavte ochranu pomocí plán obnovení, pro každý chráněný počítač použijte hello předchozí hodnoty.

> [!NOTE]
> Replikační skupiny by měly být chráněné zpět s hello stejný hlavní cíl. Pokud jsou chráněny zpět k jiným hlavním cílovým serverům, společný bod v čase nelze určit pro ně.

### <a name="run-a-failover-toohello-on-premises-site"></a>Provedení místní toohello převzetí služeb při selhání
Po znovu nastavte ochranu hello virtuálních počítačů, můžete spustit převzetí služeb při selhání z Azure tooon místní.

1. Na stránce hello replikované položky, klikněte pravým tlačítkem na hello virtuálního počítače a potom vyberte **neplánované převzetí služeb při selhání**.
2. V **potvrzení převzetí služeb při selhání**, ověřte hello směr převzetí služeb při selhání (z Azure) a pak vyberte bod obnovení hello má toouse pro převzetí hello (hello nejnovější nebo hello nejnovějším konzistentní bodem obnovení). Bod obnovení konzistentních s aplikací dojde před hello nejnovější bod v čase a může to způsobit ztrátu dat.
3. Během převzetí služeb při selhání Site Recovery vypne hello virtuálních počítačích Azure. Po můžete zkontrolovat, že tento navrácení služeb po obnovení se dokončila podle očekávání, můžete zkontrolovat tooensure, která byla vypnuta virtuálních počítačích Azure hello podle očekávání.

### <a name="reprotect-hello-on-premises-site"></a>Znovu nastavte ochranu hello místního serveru
Po dokončení navrácení služeb po obnovení, jsou odstraněny tooensure potvrzení hello virtuální počítač, který hello virtuální počítače v Azure. toodo Ano, klikněte pravým tlačítkem na hello chráněné položky a pak klikněte na **potvrdit**. Tato akce spustí úlohu, která odebere hello bývalé obnovené virtuální počítače v Azure.

Po dokončení hello potvrzení data by měla být zpět na hello místní lokality, ale nebudou chráněné. replikaci tooAzure toostart znovu hello následující:

1. V **trezoru**v **nastavení** > **replikované položky**, vyberte hello virtuálních počítačů, které selhaly zpět a pak klikněte na tlačítko **znovu nastavit ochranu**.
2. Zadejte hodnotu hello hello procesového serveru, které musí použít toobe toosend data zpět tooAzure.
3. Klikněte na **OK**.

Po dokončení vytvoření hello back tooAzure replikuje hello virtuálních počítačů a můžete provést převzetí služeb při selhání.

### <a name="resolve-common-failback-issues"></a>Řešení běžných potíží navrácení služeb po obnovení
* Pokud provádět zjišťování vCenter uživatele jen pro čtení a chránit virtuální počítače, bude úspěšný a funguje převzetí služeb při selhání. Při vytvoření převzetí služeb při selhání se nezdaří, protože nemůže být zjištěny hello datastores. Jako příznakem neuvidíte hello datastores uvedené během vytvoření. tooresolve-li tento problém, můžete aktualizovat pověření vCenter hello odpovídajícímu účtu, který má oprávnění a opakujte úlohu hello. Další informace najdete v tématu [VMware replikovat virtuální počítače a fyzické servery tooAzure s Azure Site Recovery](site-recovery-vmware-to-azure-classic.md)
* Při navrácení služeb po obnovení virtuálního počítače s Linuxem a spusťte ho místně, uvidíte, že byl tento balíček správce sítě hello odinstalován z počítače hello. Tato odinstalace se stane, protože balíček správce sítě hello odebrán, pokud je obnovena hello virtuálních počítačů v Azure.
* Když virtuální počítač je nakonfigurovaný se statickou IP adresou a při selhání tooAzure, hello IP adresa se získávají pomocí protokolu DHCP. Při selhání back tooon místní hello virtuální počítač dál toouse DHCP tooacquire hello IP adresu. Ručně přihlásit toohello počítač a v případě potřeby nastavte hello IP adresu zpět tooa statickou adresu.
* Pokud používáte edice free ESXi 5.5 nebo edice free Hypervisor vSphere 6, by úspěšné převzetí služeb při selhání, ale navrácení služeb po obnovení nebude úspěšné. tooenable navrácení služeb po obnovení, program upgradu tooeither zkušební licence.
* Pokud není možné dosáhnout hello konfigurační server z hello procesového serveru, zkontrolujte připojení toohello konfigurační server Telnet toohello konfigurace serveru počítač na portu 443. Můžete také zkusit tooping hello konfigurační server z počítače procesový Server hello. Procesový Server musí být také prezenční signál, pokud je připojený toohello konfigurační server.
* Pokud se pokoušíte toofail back tooan alternativní vCenter, ujistěte se, že vaše nové vCenter zjišťuje a že hello hlavní cílový server je také zjistit. Typické symptomem je, že hello datastores nejsou přístupné nebo viditelné v hello **znovu nastavte ochranu** dialogové okno.
* WS2008R2SP1 počítač, který je chráněn jako fyzický na místním počítači nemůže být zpět z Azure tooon místní se nezdařilo.

## <a name="fail-back-with-expressroute"></a>Neúspěšné a zobrazí zpět se ExpressRoute
Může selhat zpět prostřednictvím připojení VPN nebo pomocí připojení ExpressRoute. Pokud chcete toouse připojení ExpressRoute, vezměte na vědomí následující hello:

* Hello připojení ExpressRoute měli nastavit na hello virtuální síť Azure, který hello zdrojového počítače při selhání tooand kde nacházejí hello virtuální počítače Azure po převzetí služeb při selhání hello.
* Data jsou replikované tooan účtu úložiště Azure na veřejný koncový bod. toouse připojení ExpressRoute, nastavte veřejný partnerský vztah v ExpressRoute s hello cílového datového centra pro replikaci Site Recovery.
