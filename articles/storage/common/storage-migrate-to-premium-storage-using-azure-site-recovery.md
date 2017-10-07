---
title: "aaaMigrating tooAzure Premium Storage pomocí Azure Site Recovery (nespravované disky) | Microsoft Docs"
description: "Migrujte existující virtuální počítače tooAzure Premium Storage pomocí Site Recovery (nespravované disky)."
services: storage
documentationcenter: 
author: luywang
manager: kavithag
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: luywang
ms.openlocfilehash: 2c82fffaa38baeeb4a676748125bd85d6e0500f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-toopremium-storage-using-azure-site-recovery-unmanaged-disks"></a>Migrace tooPremium úložiště pomocí Azure Site Recovery (nespravované disků)

[Azure Premium Storage](storage-premium-storage.md) nabízí podporu vysoce výkonné, nízkou latencí disků pro virtuální počítače (VM), které jsou spuštěny I náročnými úlohy. Hello účel tohoto průvodce je uživatelé toohelp migraci jejich disky virtuálních počítačů z tooa účet standardního úložiště prémiový účet úložiště pomocí [Azure Site Recovery](../../site-recovery/site-recovery-overview.md).

Site Recovery je služba Azure, která přispívá tooyour provozní kontinuitu a strategie zotavení po havárii tím, že orchestruje hello replikace místní fyzických serverů a virtuálních počítačů toohello cloudu (Azure) nebo tooa sekundárního datacentra. Pokud dojde k výpadkům ve vašem primárním umístění, můžete převzít toohello sekundárního umístění tookeep aplikace a úlohy zůstaly dostupné. Primární umístění back tooyour nezdaří, až se obnoví toonormal operaci. Site Recovery poskytuje testovací převzetí služeb při selhání projde obnovení po havárii toosupport aniž by to ovlivňovalo produkční prostředí. Když spustíte převzetí služeb při selhání s minimálními ztrátami dat (podle četnosti replikací) pro neočekávané havárie. V hello scénáře migrace tooPremium úložiště, můžete použít hello [převzetí služeb při selhání ve službě Site Recovery](../../site-recovery/site-recovery-failover.md) v Azure Site Recovery toomigrate cílové disky tooa prémiový účet úložiště.

Doporučujeme migraci tooPremium úložiště pomocí Site Recovery, protože tato možnost nabízí minimální dobou výpadku a zabraňuje hello provádění ruční kopírování disků a vytvoření nové virtuální počítače. Site Recovery bude systematičtěji zkopírujte vaše disky a vytvořte nové virtuální počítače během převzetí služeb při selhání. Site Recovery podporuje několik typů převzetí služeb při selhání s minimální nebo žádný výpadek. tooplan vaše ztrátě dat výpadky a odhad, najdete v části hello [typy převzetí služeb při selhání](../../site-recovery/site-recovery-failover.md) tabulky v Site Recovery. Pokud jste [Příprava tooconnect tooAzure virtuální počítače po převzetí služeb při selhání](../../site-recovery/vmware-walkthrough-overview.md), byste měli mít tooconnect toohello virtuálního počítače Azure pomocí protokolu RDP po převzetí služeb při selhání.

![][1]

## <a name="azure-site-recovery-components"></a>Součásti Azure Site Recovery

Toto jsou součásti Site Recovery hello, které jsou relevantní toothis scénář migrace.

* **Konfigurační server** je virtuální počítač Azure, která koordinuje komunikaci a spravuje procesy data replikace a obnovení. Na tomto virtuálním počítači budete spouštět nastavení jedním souboru tooinstall hello konfigurační server a další komponentu, názvem procesní server, jako replikační brána. Přečtěte si informace o [požadavky konfigurace serveru](../../site-recovery/vmware-walkthrough-overview.md). Konfigurační server pouze musí toobe nakonfigurovaný jednou a lze použít pro všechny migrace toohello stejné oblasti.

* **Procesový server** replikační brána, která přijímá data replikace z zdrojové virtuální počítače optimalizuje hello dat pomocí ukládání do mezipaměti, komprese a šifrování, a odešle ji tooa účet úložiště. Také obstará nabízenou instalaci hello mobility service toosource virtuální počítače a provádí automatického zjišťování zdrojové virtuální počítače. Hello Výchozí procesový server je nainstalována na hello konfiguračním serveru. Můžete nasadit další samostatný proces servery tooscale vaše nasazení. Přečtěte si informace o [osvědčené postupy pro nasazení procesového serveru](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) a [nasazení serverů pro další proces](../../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers). Procesový server pouze potřebuje toobe nakonfigurovaný jednou a lze použít pro všechny migrace toohello stejné oblasti.

* **Služba mobility** je komponenta, která je nasazena v každé standardní virtuální počítač má tooreplicate. Zachycení datové zápisy na hello standardní virtuální počítač a předává je toohello procesový server. Přečtěte si informace o [replikovat počítače požadavky](../../site-recovery/vmware-walkthrough-overview.md).

Tento obrázek znázorňuje interakci těchto součástí.

![][15]

> [!NOTE]
> Site Recovery nepodporuje migraci hello disků prostorů úložiště.

Další součásti pro další scénáře naleznete příliš[architekturu scénáře](../../site-recovery/vmware-walkthrough-overview.md).

## <a name="azure-essentials"></a>Azure essentials

Tyto jsou hello Azure požadavky pro tento scénář migrace.

* Předplatné Azure
* Toostore účet úložiště Azure Premium replikovaná data
* Pokud jste vytvořili v převzetí služeb při selhání se budou připojovat virtuální síti Azure (VNet) toowhich virtuálních počítačů. Hello virtuální síť Azure musí být v hello stejné oblasti jako jeden ve které hello Site Recovery běží hello
* Azure standardní účet úložiště v které toostore protokoly replikace. Může se jednat jako disky hello virtuálních počítačů se migruje hello stejný účet úložiště

## <a name="prerequisites"></a>Požadavky

* Pochopit součásti scénáře hello relevantní migrace v předcházející části hello
* Naplánovat odstávka metodou učení o hello [převzetí služeb při selhání ve službě Site Recovery](../../site-recovery/site-recovery-failover.md)

## <a name="setup-and-migration-steps"></a>Postup instalace a migrace

Site Recovery toomigrate virtuální počítače Azure IaaS můžete použít mezi regiony nebo v rámci stejné oblasti. Hello následující pokyny mít byla přizpůsobit pro tento scénář migrace z článku hello [replikovat virtuální počítače VMware nebo fyzických serverů tooAzure](../../site-recovery/vmware-walkthrough-overview.md). Postupujte podle hello odkazy podrobný popis kroků v další toohello pokyny v tomto článku.

1. **Vytvoření trezoru služeb zotavení**. Vytvoření a správa hello trezoru Site Recovery prostřednictvím hello [portál Azure](https://portal.azure.com). Klikněte na tlačítko **nové** > **správy** > **zálohování** a **lokality obnovení (OMS)**. Případně můžete kliknout na **Procházet** > **trezoru služeb zotavení** > **přidat**. Virtuální počítače budou replikované toohello oblasti, které určíte v tomto kroku. Hello za účelem migrace v hello stejné oblasti, vyberte hello oblast, kdy jsou vaše zdrojové virtuální počítače a účty zdrojové úložiště. 

2. Hello následující postup vám pomůže **volba cílů ochrany**.

    2a. Na hello virtuálního počítače, kam chcete tooinstall hello konfigurační server, otevřete hello [portál Azure](https://portal.azure.com). Přejděte příliš**trezory služeb zotavení** > **nastavení**. V části **nastavení**, vyberte **Site Recovery**. V části **Site Recovery**, vyberte **krok 1: připravte infrastrukturu**. V části **připravit infrastrukturu**, vyberte **cíl ochrany**.

    ![][2]

    2b. V části **cíl ochrany**, v prvním rozevíracím seznamu text hello, vyberte **tooAzure**. V rozevíracím seznamu druhý hello vyberte **není virtualizované / jiné**a potom klikněte na **OK**.

    ![][3]

3. Hello následující postup vám pomůže **nastavit hello zdrojové prostředí (konfigurační server)**.

    3a. Stáhnout hello **Unified instalace nástroje Azure Site Recovery** a hello **registrační klíč trezoru** podle budete toohello **připravit infrastrukturu**  >  **Připravit zdroj** > **přidat Server** okno. Budete potřebovat hello nastavení hello unified klíče toorun registrace trezoru. Hello klíč je platný 5 dní od jeho vygenerování.

    ![][4]

    3b. Přidání konfigurace serveru v hello **přidat Server** okno.

    ![][5]

    3c. Na hello virtuálního počítače, který používáte jako hello konfigurační server spusťte instalační program Unified tooinstall hello konfigurační server a procesový server hello. Si můžete projít snímky obrazovky hello [sem](../../site-recovery/vmware-walkthrough-overview.md) toocomplete hello instalace. Je možné odkazovat toohello následující snímky obrazovky pro kroky zadaný pro tento scénář migrace.

    V **před zahájením**, vyberte **nainstalovat hello konfigurační server a procesový server**.

    ![][6]

    3D. V **registrace**, procházet a vyberte registrační klíč hello jste si stáhli z trezoru hello.

    ![][7]

    3E. V **prostředí podrobnosti**vyberte, jestli budete virtuální počítače VMware tooreplicate. Pro tento scénář migrace zvolte **ne**.

    ![][8]

    3f. Po dokončení instalace hello uvidíte hello **Microsoft Azure Site Recovery konfigurační Server** okno. Použít hello **Správa účtů** kartě toocreate hello účet, který Site Recovery můžete použít pro automatické zjišťování. (V případě hello o ochraně fyzické počítače, nastavení účtu hello není relevantní, ale musíte mít alespoň jeden tooenable účet, jednu z následujících kroků hello. V tomto případě můžete pojmenovat hello účet a heslo jako kterákoli.) Použití hello **registrace trezoru** soubor s přihlašovacími údaji trezoru karta tooupload hello.

    ![][9]

4. **Nastavení cílového prostředí hello**. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**a zadejte model nasazení hello toouse chcete pro virtuální počítače po převzetí služeb při selhání. Můžete zvolit **Classic** nebo **Resource Manager**, v závislosti na vašem scénáři.

    ![][10]

    Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure. Všimněte si, že pokud pro replikovaná data používáte účet úložiště Premium, je třeba tooset replikaci toostore účtu další standardní úložiště protokolů.

5. **Nakonfigurování nastavení replikace**. Postupujte podle [nakonfigurování nastavení replikace](../../site-recovery/vmware-walkthrough-overview.md) tooverify, který je úspěšně přidružený k zásadě replikace hello vytvoříte konfigurační server.

6. **Plánování kapacity**. Použití hello [Plánovač kapacity](../../site-recovery/site-recovery-capacity-planner.md) tooaccurately odhad šířky pásma sítě, úložiště a další požadavky toomeet musí vaší replikace. Až budete hotoví, vyberte **Ano** v **dokončili jste plánování kapacity?**.

    ![][11]

7. Hello následující postup vám pomůže **instalaci služby mobility a zapnout replikaci**.

    7a. Můžete zvolit příliš[nabízená instalace](../../site-recovery/vmware-walkthrough-overview.md) tooyour zdrojové virtuální počítače nebo příliš[ručně instalaci služby mobility](../../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md) na zdrojové virtuální počítače. Požadavek hello vkládání instalace a cestu hello hello ruční Instalační služby systému můžete najít v hello uvedeného odkazu. Pokud provádíte ruční instalace, bude pravděpodobně nutné toouse interní IP adresu toofind hello konfigurace serveru.

    ![][12]

    Hello virtuálních počítačů při selhání bude mít dva dočasné disky: jeden z hello primární virtuální počítač a hello jiných vytvořené při zřizování hello virtuálního počítače v oblasti obnovení hello. tooexclude hello dočasným diskovým před replikace, nainstalujte službu mobility hello před povolením replikace. toolearn Další informace o jak tooexclude hello dočasného disku odkazovat příliš[z replikace vyloučit disky](../../site-recovery/vmware-walkthrough-overview.md).

    7b. Teď následujícím způsobem povolte replikaci:
      * Klikněte na tlačítko **replikujte aplikaci** > **zdroj**. Po povolení replikace pro hello poprvé, klikněte na tlačítko + replikovat v hello trezoru tooenable replikaci pro další počítače.
      * V kroku 1 nastavte zdroj procesový server.
      * V kroku 2 zadejte model nasazení hello post-převzetí služeb při selhání, toomigrate účet úložiště Premium informace k, protokoly toosave účet standardního úložiště a toofail virtuální sítě k.
      * V kroku 3, přidejte chráněných virtuálních počítačů pomocí IP adresy (může být nutné k interní IP adresu toofind je).
      * V kroku 4 konfigurujte vlastnosti hello výběrem hello účty, které jste dřív nastavili na hello procesový server.
      * V kroku 5 zvolte hello replikace zásadu, kterou jste vytvořili dříve, nastavte nastavení replikace.
      Klikněte na tlačítko **OK** a zapnout replikaci.

    > [!NOTE]
    > Když virtuální počítač Azure je navrácena a spustit znovu, neexistuje žádná záruka, které se budou získávat hello stejnou IP adresu. Pokud IP adresa hello hello konfigurace proces serveru nebo serveru nebo hello chráněné virtuální počítače Azure změnu, hello replikace v tomto scénáři nemusí fungovat správně.

    ![][13]

    Když navrhujete prostředí Azure Storage, doporučujeme použít účty samostatné úložiště pro každý virtuální počítač v nastavení dostupnosti. Doporučujeme podle hello osvědčený postup ve vrstvě úložiště hello příliš[používat více účtů úložiště pro každou skupinu dostupnosti](../../virtual-machines/windows/manage-availability.md). Distribuce účtů úložiště toomultiple disky virtuálních počítačů pomáhá dostupnost úložiště tooimprove a rozděluje hello vstupně-výstupních operací na hello infrastruktury úložiště Azure. Pokud jsou vaše virtuální počítače v nastavení dostupnosti, namísto replikace disků všechny virtuální počítače do jeden účet úložiště, důrazně doporučujeme migraci víc virtuálních počítačů vícekrát, tak, aby hello hello virtuální počítače ve stejné sady dostupnosti. nesdílejí účet jednoho úložiště. Použití hello **povolit replikaci** okno tooset až cílový účet úložiště pro každý virtuální počítač, po jednom. Můžete podle potřeby tooyour model nasazení post-převzetí služeb při selhání. Pokud si zvolíte Resource Manager (RM) jako model nasazení post-převzetí služeb při selhání, můžete převzít tooan RM virtuálních počítačů RM VM nebo přebírány classic tooan virtuálních počítačů RM VM.

8. **Spustit testovací převzetí služeb**. toocheck jestli vaší replikace skončí, klikněte na možnost obnovení lokality a pak klikněte na tlačítko **nastavení** > **replikované položky**. Zobrazí se stav hello a procento replikační proces. Po počáteční replikaci je kompletní, spustit testovací převzetí služeb při selhání toovalidate strategie replikace. Podrobný postup testovacího převzetí služeb při selhání naleznete příliš[spustit testovací převzetí služeb ve službě Site Recovery](../../site-recovery/vmware-walkthrough-overview.md). Zobrazí se stav hello testovací převzetí služeb při selhání v **nastavení** > **úlohy** > **YOUR_FAILOVER_PLAN_NAME**. V okně hello uvidíte rozpis hello kroků a výsledky úspěch nebo selhání. Pokud se nezdaří hello testovací převzetí služeb při selhání v kterémkoliv kroku, klikněte na tlačítko hello krok toocheck hello chybová zpráva. Zajistěte, aby že virtuální počítače a strategie replikace splňovat požadavky hello před spuštěním převzetí služeb při selhání. Čtení [tooAzure testovací převzetí služeb při selhání ve službě Site Recovery](../../site-recovery/site-recovery-test-failover-to-azure.md) pro další informace a pokyny testovací převzetí služeb při selhání.

9. **Spuštění převzetí služeb při selhání**. Po dokončení hello testovací převzetí služeb při selhání, spusťte převzetí služeb při selhání toomigrate tooPremium vaše disky úložiště a replikovat hello instance virtuálních počítačů. Postupujte podle hello podrobné kroky v [spustit převzetí služeb při selhání](../../site-recovery/site-recovery-failover.md#run-a-failover). Zkontrolujte, zda jste vybrali **vypněte virtuální počítače a synchronizovat nejnovější data hello** toospecify, že by měl Site Recovery zkuste tooshut dolů hello chráněné virtuální počítače a synchronizovat hello data tak, aby hello nejnovější verzi dat hello převezme služby při selhání. Pokud tuto možnost nevyberete nebo pokus o hello k neúspěchu hello převzetí služeb při selhání bude od hello nejnovější dostupný bod obnovení pro hello virtuálních počítačů. Site Recovery vytvoří instanci virtuálního počítače, jejichž typ je hello stejný jako nebo podobné tooa Premium Storage – výkonný virtuálních počítačů. Můžete zkontrolovat hello výkon a cenu různých instancí virtuálních počítačů tak, že přejdete příliš[ceny virtuálních počítačů Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) nebo [ceny virtuálních počítačů Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="post-migration-steps"></a>Kroky po migraci

1. **Konfigurace replikované toohello dostupnosti virtuálních počítačů, nastavit, pokud je k dispozici**. Site Recovery nepodporuje migraci virtuálních počítačů společně se skupinou dostupnosti hello. V závislosti na nasazení hello replikované virtuální počítač proveďte jednu z následujících hello:
  * Pro virtuální počítač, vytvořené pomocí modelu nasazení classic hello: přidejte hello virtuálních počítačů toohello skupinou dostupnosti ve hello portálu Azure. Podrobný postup je uveden příliš[přidat stávající sadu dostupnosti virtuálního počítače tooan](../../virtual-machines/windows/classic/configure-availability.md#addmachine).
  * Pro model nasazení Resource Manager hello: Uložit konfiguraci hello virtuálních počítačů a potom odstraňte a znovu vytvořit hello virtuálních počítačů v nastavení dostupnosti hello. toodo Ano, použít skript hello v [nastavit Azure Resource Manager virtuálních počítačů sady dostupnosti](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4). Zkontrolujte hello omezení tento skript a plánování výpadku před spuštěním skriptu hello.

2. **Odstranit staré virtuální počítače a disky**. Před odstraněním těchto, zkontrolujte, zda text hello prémiové disky jsou konzistentní s zdrojové disky a hello nové virtuální počítače provést hello stejné funkce jako zdroj hello virtuálních počítačů. V modelu nasazení Resource Manager (RM) hello odstraňte hello virtuálních počítačů a odstranit hello disky ze zdrojového účty úložiště v hello portálu Azure. V modelu nasazení classic hello můžete odstranit hello virtuálních počítačů a disky v portálu classic hello nebo portálu Azure. Pokud nastane problém v které hello disku neodstraní, i když jste odstranili hello virtuálních počítačů, naleznete v tématu [řešení chyb při odstranění virtuální pevné disky](storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

3. **Vyčištění hello infrastruktury Azure Site Recovery**. Pokud Site Recovery je již nepotřebujete, můžete odstranit replikované položky, hello konfigurační server a hello obnovení zásad, a pak odstranění trezoru Azure Site Recovery hello smazat svoji infrastrukturu.

## <a name="troubleshooting"></a>Řešení potíží

* [Monitorování a řešení ochrany pro virtuální počítače a fyzické servery](../../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [Fórum Microsoft Azure Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

## <a name="next-steps"></a>Další kroky

V tématu hello následující prostředků u konkrétních scénářů pro migraci virtuálních počítačů:

* [Migrovat virtuální počítače, které jsou mezi účty úložiště Azure](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Vytvoření a nahrání virtuálního pevného disku serveru Windows tooAzure.](../../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Vytvoření a nahrání virtuálního pevného disku tohoto hello obsahuje operační systém Linux](../../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Migrace virtuálních počítačů z Amazon AWS tooMicrosoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Viz také hello následující prostředky toolearn Další informace o Azure Storage a virtuálních počítačích Azure:

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
