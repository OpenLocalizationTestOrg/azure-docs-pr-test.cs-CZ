---
title: "Migrace na Azure Premium Storage pomocí Azure Site Recovery | Microsoft Docs"
description: "Migrace stávajících virtuálních počítačů do Azure Premium Storage pomocí Site Recovery. Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro I náročnými úlohy běžící na virtuálních počítačích Azure."
services: storage
cloud: Azure
documentationcenter: na
author: luywang
manager: kavithag
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2017
ms.author: luywang
ms.openlocfilehash: cc364bdae49068a50ec86c537c3b878670b8b8b7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="migrating-to-premium-storage-using-azure-site-recovery"></a>Migrace na Storage úrovně Premium pomocí Azure Site Recovery

[Azure Premium Storage](storage-premium-storage.md) nabízí podporu vysoce výkonné, nízkou latencí disků pro virtuální počítače (VM), které jsou spuštěny I náročnými úlohy. Účelem této příručky je pomoci uživatelům migraci jejich disky virtuálních počítačů z standardní účet úložiště na prémiový účet úložiště pomocí [Azure Site Recovery](../site-recovery/site-recovery-overview.md).

Site Recovery je služba Azure, která přispívá ke strategii obnovení kontinuity a po havárii obchodní orchestrací replikace místní fyzických serverů a virtuálních počítačů do cloudu (Azure) nebo do sekundárního datacentra. Pokud dojde k výpadkům ve vašem primárním umístění, můžete převzetí služeb při selhání do sekundárního umístění, aby aplikace a úlohy zůstaly dostupné. Nedodržíte zpět do primárního umístění, až se obnoví do normálního provozu. Site Recovery poskytuje testovací převzetí služeb při selhání na Nácvik zotavení po havárii aniž by to ovlivňovalo produkční prostředí. Když spustíte převzetí služeb při selhání s minimálními ztrátami dat (podle četnosti replikací) pro neočekávané havárie. Ve scénáři migrace na Storage úrovně Premium, můžete použít [převzetí služeb při selhání ve službě Site Recovery](../site-recovery/site-recovery-failover.md) v Azure Site Recovery k migraci disků target na prémiový účet úložiště.

Doporučujeme, abyste migrace na Storage úrovně Premium pomocí Site Recovery, protože tato možnost nabízí minimální dobou výpadku a zabraňuje provádění ruční kopírování disků a vytvoření nové virtuální počítače. Site Recovery bude systematičtěji zkopírujte vaše disky a vytvořte nové virtuální počítače během převzetí služeb při selhání. Site Recovery podporuje několik typů převzetí služeb při selhání s minimální nebo žádný výpadek. Naplánovat odstávka a odhadnout ztrátě dat, přečtěte si téma [typy převzetí služeb při selhání](../site-recovery/site-recovery-failover.md) tabulky v Site Recovery. Pokud jste [Příprava připojení k virtuálním počítačům Azure po převzetí služeb při selhání](../site-recovery/site-recovery-vmware-to-azure.md), musí být možné se připojit k virtuálnímu počítači Azure po převzetí služeb při selhání pomocí protokolu RDP.

![][1]

## <a name="azure-site-recovery-components"></a>Součásti Azure Site Recovery

Toto jsou součásti Site Recovery, které jsou relevantní pro tento scénář migrace.

* **Konfigurační server** je virtuální počítač Azure, která koordinuje komunikaci a spravuje procesy data replikace a obnovení. Na tomto virtuálním počítači spustíte souboru jednoho instalačního programu nainstalujte konfigurační server a další komponentu, názvem procesní server, jako replikační brána. Přečtěte si informace o [požadavky konfigurace serveru](../site-recovery/site-recovery-vmware-to-azure.md). Konfigurační server jenom je potřeba nakonfigurovat jednou a lze použít pro všechny migrace do stejné oblasti.

* **Procesový server** replikační brána, která přijímá data replikace z zdrojové virtuální počítače optimalizuje dat pomocí ukládání do mezipaměti, komprese a šifrování, a odešle ji do účtu úložiště. Také obstará nabízenou instalaci služby mobility na zdrojový virtuální počítače a provádí automatického zjišťování zdrojové virtuální počítače. Výchozí proces server je nainstalována na konfiguračním serveru. Můžete nasadit další samostatných serverů proces škálování vašeho nasazení. Přečtěte si informace o [osvědčené postupy pro nasazení procesového serveru](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) a [nasazení serverů pro další proces](../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers). Procesový server jenom je potřeba nakonfigurovat jednou a lze použít pro všechny migrace do stejné oblasti.

* **Služba mobility** je komponenta, která je nasazena v každé standardní virtuálních počítačů, které chcete replikovat. Se zaznamenává datové zápisy na standardní virtuální počítač a předává je na procesní server. Přečtěte si informace o [replikovat počítače požadavky](../site-recovery/site-recovery-vmware-to-azure.md).

Tento obrázek znázorňuje interakci těchto součástí.

![][15]

> [!NOTE]
> Site Recovery nepodporuje migraci disků prostorů úložiště.

Další součásti pro další scénáře, naleznete v [architekturu scénáře](../site-recovery/site-recovery-vmware-to-azure.md).

## <a name="azure-essentials"></a>Azure essentials

Toto jsou požadavky na Azure pro tento scénář migrace.

* Předplatné Azure
* Účet služby Azure Premium storage k ukládání replikovaných dat
* Virtuální sítě Azure (VNet) ke kterému se připojí virtuální počítače, když jste vytvořili v převzetí služeb při selhání. Virtuální síť Azure musí být ve stejné oblasti jako ta, ve kterém běží služby Site Recovery
* Azure standardní účet úložiště pro uložení protokoly replikace. To může být stejný účet úložiště jako migrovaného disky virtuálních počítačů

## <a name="prerequisites"></a>Požadavky

* Seznámení s komponentami scénář relevantní migrace v předchozí části
* Naplánovat odstávka metodou učení o [převzetí služeb při selhání ve službě Site Recovery](../site-recovery/site-recovery-failover.md)

## <a name="setup-and-migration-steps"></a>Postup instalace a migrace

Site Recovery můžete použít k migraci virtuálních počítačů Azure IaaS mezi regiony nebo v rámci stejné oblasti. Mají se přizpůsobit podle následujících pokynů pro tento scénář migrace z článku [replikovat virtuální počítače VMware nebo fyzických serverů do Azure](../site-recovery/site-recovery-vmware-to-azure.md). Podrobné odkazy podle kroků v další pokyny v tomto článku.

1. **Vytvoření trezoru služeb zotavení**. Vytvoření a správa trezoru Site Recovery prostřednictvím [portál Azure](https://portal.azure.com). Klikněte na tlačítko **nové** > **správy** > **zálohování** a **lokality obnovení (OMS)**. Případně můžete kliknout na **Procházet** > **trezoru služeb zotavení** > **přidat**. Virtuální počítače budou replikovány do oblasti, které určíte v tomto kroku. Pro účely migrace ve stejné oblasti vyberte oblast, kde jsou zdrojové virtuální počítače a účty zdrojové úložiště. Všimněte si, že migrace na účty úložiště Premium se podporuje jenom v [portál Azure](https://portal.azure.com), nikoli [portálu classic](https://manage.windowsazure.com).

2. Následující postup vám pomůže **volba cílů ochrany**.

    2a. Na virtuálním počítači, kam chcete nainstalovat konfigurační server, otevřete [portál Azure](https://portal.azure.com). Přejděte na **trezory služeb zotavení** > **nastavení**. V části **nastavení**, vyberte **Site Recovery**. V části **Site Recovery**, vyberte **krok 1: připravte infrastrukturu**. V části **připravit infrastrukturu**, vyberte **cíl ochrany**.

    ![][2]

    2b. V části **cíl ochrany**, v prvním rozevíracím seznamu vyberte **do Azure**. Vyberte v rozevíracím seznamu druhý **není virtualizované / jiné**a potom klikněte na **OK**.

    ![][3]

3. Následující postup vám pomůže **nastavení zdrojového prostředí (konfigurační server)**.

    3a. Stažení **Unified instalace nástroje Azure Site Recovery** a **registrační klíč trezoru** přechodem na **připravit infrastrukturu** > **připravit zdroj** > **přidat Server** okno. Budete potřebovat registrační klíč trezoru spustit jednotnou instalaci. Klíč je platný 5 dní od jeho vygenerování.

    ![][4]

    3b. Přidejte konfigurační Server v **přidat Server** okno.

    ![][5]

    3c. Ve virtuálním počítači, který používáte jako konfigurační server spusťte Unified instalačního programu nainstalujte konfigurační server a procesový server. Si můžete projít na snímcích obrazovky [sem](../site-recovery/site-recovery-vmware-to-azure.md) k dokončení instalace. Najdete na následujících snímcích obrazovky kroky zadaný pro tento scénář migrace.

    Na stránce **Než začnete** vyberte **Nainstalovat konfigurační server a procesový server**.

    ![][6]

    3D. V **registrace**, vyhledejte a vyberte registrační klíč stažený z trezoru.

    ![][7]

    3E. Na stránce **Podrobnosti o prostředí** vyberte, zda se chystáte replikovat virtuální počítače VMware. Pro tento scénář migrace zvolte **ne**.

    ![][8]

    3f. Po dokončení instalace se zobrazí **Microsoft Azure Site Recovery konfigurační Server** okno. Použít **Správa účtů** pro automatické zjišťování můžete použít kartu k vytvoření účtu obnovení lokality. (Ve scénáři o ochraně fyzické počítače, nastavení účtu není relevantní, ale musíte mít alespoň jeden účet, aby měla jedna z následujících kroků. V tomto případě můžete pojmenovat účet a heslo jako kterákoli.) Použití **registrace trezoru** kartě nahrát soubor s přihlašovacími údaji trezoru.

    ![][9]

4. **Nastavení cílového prostředí**. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**a zadejte model nasazení, kterou chcete použít pro virtuální počítače po převzetí služeb při selhání. Můžete zvolit **Classic** nebo **Resource Manager**, v závislosti na vašem scénáři.

    ![][10]

    Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure. Poznámka: Pokud pro replikovaná data používáte účet úložiště Premium, budete muset nastavit další standardní účet úložiště k ukládání protokolů replikace.

5. **Nakonfigurování nastavení replikace**. Postupujte podle [nakonfigurování nastavení replikace](../site-recovery/site-recovery-vmware-to-azure.md) k ověřte, zda je konfigurační server úspěšně přidružené k zásadě replikace, který vytvoříte.

6. **Plánování kapacity**. Použití [Plánovač kapacity](../site-recovery/site-recovery-capacity-planner.md) přesně odhadnout šířku pásma sítě, úložiště a dalších požadavků na provádění vaší replikace potřebuje. Až budete hotoví, vyberte **Ano** v **dokončili jste plánování kapacity?**.

    ![][11]

7. Následující postup vám pomůže **instalaci služby mobility a zapnout replikaci**.

    7a. Můžete se rozhodnout [nabízená instalace](../site-recovery/site-recovery-vmware-to-azure.md) pro vaše zdrojové virtuální počítače nebo na [ručně instalaci služby mobility](../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md) na zdrojové virtuální počítače. Můžete najít požadavek nabízené instalace a cestu ruční instalační program v odkazu. Při provádění ruční instalace, možná budete muset použít interní IP adresu najít konfigurační server.

    ![][12]

    Virtuální počítač při selhání bude mít dva dočasné disky: jedné z primárního virtuálního počítače a dalších vytvořené při zřizování virtuálních počítačů v oblasti obnovení. Vyloučit dočasným diskovým před replikaci, nainstalujte službu mobility před povolením replikace. Další informace o tom, jak vyloučit dočasného disku, najdete v tématu [z replikace vyloučit disky](../site-recovery/site-recovery-vmware-to-azure.md).

    7b. Teď následujícím způsobem povolte replikaci:
      * Klikněte na tlačítko **replikujte aplikaci** > **zdroj**. Po povolení replikace poprvé, klikněte na tlačítko + replikovat v trezoru povolíte replikaci pro další počítače.
      * V kroku 1 nastavte zdroj procesový server.
      * V kroku 2 zadejte model nasazení post-převzetí služeb při selhání, prémiový účet úložiště k migraci na standardní účet úložiště k ukládání protokolů a virtuální sítě, aby v případě selhání.
      * V kroku 3 přidejte chráněných virtuálních počítačů pomocí IP adresy (může být nutné interní IP adresu, kde je najít).
      * V kroku 4 nakonfigurujte vlastnosti výběrem účty, které jste dřív nastavili na procesovém serveru.
      * V kroku 5 vyberte zásadu replikace, kterou jste vytvořili dříve, nastavte nastavení replikace.
      Klikněte na tlačítko **OK** a zapnout replikaci.

    > [!NOTE]
    > Když virtuální počítač Azure je navrácena a spustit znovu, není zaručeno, že se budou získávat stejnou IP adresu. Pokud se změní IP adresu serveru, proces serveru nebo konfigurace nebo chráněné virtuální počítače Azure, replikace v tomto scénáři nemusí fungovat správně.

    ![][13]

    Když navrhujete prostředí Azure Storage, doporučujeme použít účty samostatné úložiště pro každý virtuální počítač v nastavení dostupnosti. Doporučujeme vám postupovat podle osvědčených postupů ve vrstvě úložiště pro [používat více účtů úložiště pro každou skupinu dostupnosti](../virtual-machines/windows/manage-availability.md). Distribuci disky virtuálních počítačů k několika účtům úložiště pomáhá zvýšit dostupnost úložiště a rozděluje vstupy/výstupy na infrastruktuře úložiště Azure. Pokud jsou vaše virtuální počítače v nastavení dostupnosti, namísto replikace disků všechny virtuální počítače do jeden účet úložiště, důrazně doporučujeme migraci víc virtuálních počítačů vícekrát, tak, aby virtuální počítače ve stejné sadě dostupnosti nesdílejí účet jednoho úložiště. Použití **povolit replikaci** okno Nastavit cílový účet úložiště pro každý virtuální počítač, po jednom. Podle potřeby vašeho můžete vybrat model nasazení post-převzetí služeb při selhání. Pokud si zvolíte Resource Manager (RM) jako model nasazení post-převzetí služeb při selhání, můžete převzít RM virtuálního počítače na virtuální počítač RM nebo přebírány classic virtuálního počítače na virtuální počítač RM.

8. **Spustit testovací převzetí služeb**. Pokud chcete zkontrolovat, jestli se váš replikace nedokončí, klikněte na možnost obnovení lokality a pak klikněte na **nastavení** > **replikované položky**. Zobrazí se stav a procento replikační proces. Po dokončení počáteční replikace spustit testovací převzetí služeb při selhání ověření strategie replikace. Podrobný postup testovacího převzetí služeb při selhání, naleznete v [spustit testovací převzetí služeb ve službě Site Recovery](../site-recovery/site-recovery-vmware-to-azure.md). Zobrazí se stav převzetí služeb při selhání v **nastavení** > **úlohy** > **YOUR_FAILOVER_PLAN_NAME**. V okně uvidíte rozpis kroků a výsledky úspěch nebo selhání. V případě selhání testu převzetí služeb v kterémkoliv kroku klikněte na tlačítko kroku najdete v chybové zprávě. Zajistěte, aby že virtuální počítače a strategie replikace požadavkům před spuštěním převzetí služeb při selhání. Čtení [testovací převzetí služeb při selhání do Azure ve službě Site Recovery](../site-recovery/site-recovery-test-failover-to-azure.md) pro další informace a pokyny testovací převzetí služeb při selhání.

9. **Spuštění převzetí služeb při selhání**. Po testu je dokončit převzetí služeb při selhání, spusťte převzetí služeb při selhání k migraci disků do úložiště úrovně Premium a replikovat instance virtuálních počítačů. Postupujte podle podrobných pokynů v [spustit převzetí služeb při selhání](../site-recovery/site-recovery-failover.md#run-a-failover). Zkontrolujte, zda jste vybrali **vypněte virtuální počítače a synchronizovat nejnovější data** k určení, že Site Recovery opakovat vypněte chráněných virtuálních počítačů a synchronizace dat tak, aby nejnovější verzi dat převezme služby při selhání. Pokud tuto možnost nevyberete nebo neúspěchu pokus převzetí služeb při selhání bude z posledního bodu obnovení k dispozici pro virtuální počítač. Site Recovery se vytvoří instance virtuálního počítače, jejichž typ je stejný jako nebo podobné k virtuálnímu počítači Premium Storage – výkonný. Můžete zkontrolovat výkon a cenu instancí virtuálních počítačů v různých přechodem na [ceny virtuálních počítačů Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) nebo [ceny virtuálních počítačů Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="post-migration-steps"></a>Kroky po migraci

1. **Nakonfigurovat replikované virtuální počítače pro skupinu dostupnosti případně**. Site Recovery nepodporuje migraci virtuálních počítačů spolu s skupiny dostupnosti. V závislosti na nasazení replikované virtuální počítač proveďte jednu z následujících akcí:
  * Pro virtuální počítač, vytvořené pomocí modelu nasazení classic: Přidání virtuálního počítače pro skupinu dostupnosti na portálu Azure. Podrobné kroky, přejděte na [přidat existující virtuální počítač do skupiny dostupnosti](../virtual-machines/windows/classic/configure-availability.md#addmachine).
  * Pro model nasazení Resource Manager: Uložit konfiguraci virtuálního počítače a pak odstraňte a znovu vytvořit virtuální počítače v sadě dostupnosti. Uděláte to tak, použijte skript v [nastavit Azure Resource Manager virtuálních počítačů sady dostupnosti](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4). Zkontrolujte omezení tento skript a plánování výpadku před spuštěním skriptu.

2. **Odstranit staré virtuální počítače a disky**. Před odstraněním těchto, zkontrolujte, zda jsou konzistentní s zdrojové disky prémiové disky a nové virtuální počítače provádí stejnou funkci jako zdrojové virtuální počítače. V modelu nasazení Resource Manager (RM) odstraňte virtuální počítač a odstraňte disky ze zdrojového účtů úložiště na portálu Azure. V modelu nasazení classic můžete odstranit virtuální počítač a disky v portálu classic nebo portálu Azure. Pokud nastane problém neodstraní disku i v případě, že jste odstranili virtuálního počítače, naleznete v tématu [řešení chyb při odstranění virtuální pevné disky](storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

3. **Vyčištění infrastruktury Azure Site Recovery**. Pokud Site Recovery je již nepotřebujete, můžete smazat jeho infrastruktury odstranit replikované položky, že konfigurační server a obnovení zásad, a pak odstranění trezoru Azure Site Recovery.

## <a name="troubleshooting"></a>Řešení potíží

* [Monitorování a řešení ochrany pro virtuální počítače a fyzické servery](../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [Fórum Microsoft Azure Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

## <a name="next-steps"></a>Další kroky

Najdete v následujících materiálech u konkrétních scénářů pro migraci virtuálních počítačů:

* [Migrovat virtuální počítače, které jsou mezi účty úložiště Azure](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Vytvoření a nahrání virtuálního pevného disku serveru Windows do Azure.](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Vytváření a odesílání virtuální pevný Disk, který obsahuje operační systém Linux](../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Migrace virtuálních počítačů z Amazon AWS k Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Zkontrolujte také, další informace o Azure Storage a virtuální počítače Azure v následujících zdrojích:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Storage úrovně Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md)

[1]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-1.png
[2]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-2.png
[3]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-3.png
[4]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-4.png
[5]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-5.png
[6]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-6.PNG
[7]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-7.PNG
[8]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-8.PNG
[9]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-9.PNG
[10]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-10.png
[11]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-11.PNG
[12]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-12.PNG
[13]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-13.png
[14]:../site-recovery/media/site-recovery-vmware-to-azure/v2a-architecture-henry.png
[15]:./media/storage-migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-14.png
