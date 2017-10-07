---
title: "fyzické servery tooAzure aaaReplicate | Microsoft Docs"
description: "Popisuje, jak toodeploy Azure Site Recovery tooorchestrate replikace, převzetí služeb při selhání a obnovení místní tooAzure fyzických serverů Windows nebo Linuxem pomocí portálu Azure hello"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: physical-walkthrough-overview
ms.openlocfilehash: cf5928fb631f6858d57b27f6f21babc312714e21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
---
# <a name="replicate-physical-machines-tooazure-by-using-site-recovery"></a>Replikovat fyzické počítače tooAzure pomocí Site Recovery


Tento článek popisuje, jak tooreplicate místní tooAzure fyzické počítače pomocí služby Azure Site Recovery hello v hello portálu Azure.

Pokud chcete toomigrate fyzické počítače tooAzure (převzetí služeb při selhání pouze), přečtěte si [migrovat tooAzure službou Site Recovery](site-recovery-migrate-to-azure.md) toolearn Další.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites"></a>Požadavky

**Požadavek na podporu** | **Podrobnosti**
--- | ---
**Azure** | Přečtěte si o [požadavcích na Azure](site-recovery-prereq.md#azure-requirements).
**Lokální konfigurační server** | Na místním počítači (fyzické nebo virtuální počítač VMware) s Windows serverem 2012 R2 nebo novější. Během nasazování Site Recovery nastavíte hello konfigurační server.<br/><br/> Ve výchozím nastavení hello procesový server a hlavní cílový server jsou také v tomto počítači nainstalován. Když jste vertikální navýšení kapacity, může být nutné samostatný procesový server a má hello stejné požadavky jako hello konfigurační server.<br/><br/> Další informace o těchto součástí v [nastavit hello zdrojové prostředí](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements).
**Místní virtuální počítače** | Počítače, které chcete tooreplicate by měla být spuštěná [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) a v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Adresy URL** | konfigurační server Hello potřebuje přístup k toothese adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte pravidla brány firewall založená na adresu IP, ujistěte se, že umožňují tooAzure komunikace.<br/></br> Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a hello port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro přístup k řízení a identity management).<br/><br/> Povolit tuto adresu URL pro stahování MySQL hello: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Služba mobility** | Tato služba je nainstalována na každém počítači chcete tooreplicate.

## <a name="limitations"></a>Omezení

**Omezení** | **Podrobnosti**
--- | ---
**Azure** | Účty úložiště a sítě musí být v hello stejné oblasti jako trezor hello.<br/><br/> Pokud používáte prémiový účet úložiště, musíte také standardní ukládat protokoly replikace toostore účtu.<br/><br/> Nelze replikovat toopremium účty ve střední a – Jih, Indie.
**Lokální konfigurační server** | Pokud nainstalujete hello konfigurační server na virtuální počítače VMware, hello typ adaptéru virtuálního počítače musí být VMXNET3. Pokud tomu tak není, [nainstalovat tuto aktualizaci](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1).<br/><br/> Pokud používáte virtuální počítače VMware, musí se do něj nainstalovat vSphere PowerCLI 6.0.<br/><br> Hello počítač by neměl být řadič domény.<br/><br/> Hello počítač by měl mít statickou IP adresu.<br/><br/> název hostitele Hello by měl být maximálně 15 znaků nebo méně, a hello operačního systému by měl být v angličtině.
**Replikované počítače** | Ověřte [virtuálního počítače Azure omezení](site-recovery-prereq.md#azure-requirements).<br/><br/> Pokud chcete konzistence tooenable více virtuálních počítačů, která umožňuje počítačům, které běží stejné zatížení toobe obnovit hello společně tooa konzistentní datový bod, otevřete port 20004 na počítači hello.<br/><br/> Konkrétní typy [Linux úložiště](site-recovery-support-matrix-to-azure.md#support-for-storage) jsou podporovány.
**Navrácení služeb po obnovení** | Nelze označit zpět z Azure tooa fyzického počítače. Pokud chcete po převzetí služeb při selhání toobe možné toofail back tooon místní, musíte v prostředí VMware tak, aby může selhat back tooa virtuálního počítače VMware.


## <a name="set-up-azure"></a>Nastavení Azure

1. [Nastavit síť Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

      a. Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.

      b. Můžete nastavit síť v Azure [Resource Manager](../resource-manager-deployment-model.md) nebo v klasickém režimu.

2. Nastavení [účtu úložiště Azure](../storage/storage-create-storage-account.md#create-a-storage-account) pro replikovaná data.

    a. Hello účet může být standardní nebo [premium](../storage/storage-premium-storage.md).

    b. Můžete nastavit účet ve službě Správce prostředků nebo v klasickém režimu.

## <a name="prepare-hello-configuration-server"></a>Příprava hello konfigurace serveru

1. Nainstalujte Windows Server 2012 R2 nebo později na serveru místní fyzické nebo virtuální počítače VMware.

2. Ověřte, zda má počítač hello přístup toohello adresy URL uvedené v [požadavky](#prerequisites).

## <a name="prepare-for-mobility-service-installation"></a>Příprava pro instalaci služby Mobility

Pokud chcete, aby toopush hello Mobility service tooa fyzický počítač, je třeba účet, který lze použít hello proces serveru tooaccess hello počítače. Hello účet se používá pouze pro hello nabízenou instalaci. Můžete použít domény nebo místní účet:

  - Pro Windows Pokud nepoužíváte účet domény, budete potřebovat toodisable řízení vzdáleného přístupu uživatele v místním počítači hello. toodo to v klíči registru hello **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD hello **LocalAccountTokenFilterPolicy**, s hodnotou 1. Pokud chcete položku registru hello tooadd pro Windows z rozhraní příkazového řádku, zadejte:

        ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
  - Pro systémy Linux hello účet by měl být kořenový uživatel na zdrojovém serveru se Linux hello.


## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="select-hello-protection-goal"></a>Vyberte cíl ochrany hello

Vyberte, co chcete tooreplicate a místo, kam chcete tooreplicate k.

1. Klikněte na tlačítko **trezory služeb zotavení** > **trezoru**.
2. Na hello **prostředků** nabídky, klikněte na tlačítko **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-vmware-to-azure/choose-goal-physical.PNG)

3. V **cíl ochrany**, vyberte **tooAzure** > **není virtualizované / jiné**.


## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí

Nastavení konfigurace serveru hello, zaregistrovat v trezoru hello a vyhledání virtuálních počítačů.

1. Klikněte na tlačítko **lokality obnovení** > **Příprava infrastruktury** > **zdroj**.
2. Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační Server**.

    ![Nastavení zdroje](./media/site-recovery-vmware-to-azure/set-source1.png)

3. V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
4. Stáhnout hello **Unified instalace nástroje Site Recovery** instalační soubor.
5. Stáhnout hello **registrační klíč trezoru**. Když spustíte instalační program Unified potřebujete tento klíč. Hello klíč je platný pět dní od jeho vygenerování.

   ![Nastavení zdroje](./media/site-recovery-vmware-to-azure/set-source2.png)


## <a name="run-site-recovery-unified-setup"></a>Sjednocený instalační program spusťte Site Recovery

Než začnete, hello následující:

- Vám zajistí rychlý přehled videa. (hello video popisuje, jak zpracovat tooreplicate virtuální počítače VMware, ale hello je podobné pro replikaci fyzického počítače.)

    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

- Na počítači serveru hello konfigurace, ujistěte se, že hello systémové hodiny synchronizované s [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Pokud je 15 minut před nebo za instalace může selhat.
- Spusťte instalaci jako místní správce na počítači serveru konfigurace hello.
- Zkontrolujte, zda že je na počítači hello povolená TLS 1.0.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Hello konfigurační server lze také nainstalovat [z příkazového řádku hello](http://aka.ms/installconfigsrv).


## <a name="set-up-hello-target-environment"></a>Nastavení cílového prostředí hello

Před nastavením hello cílové prostředí, zkontrolujte toomake se, že máte [účtu úložiště Azure a sítě](#set-up-azure).

1. Klikněte na tlačítko **Příprava infrastruktury** > **cíl**, a vyberte hello chcete toouse předplatného Azure.
2. Určete, zda je váš cíl model nasazení Resource Manager nebo classic.
3. Site Recovery zkontroluje, zda máte jeden nebo více účtů kompatibilní úložiště Azure a sítě toomake.

   ![cíl](./media/site-recovery-vmware-to-azure/gs-target.png)

4. Pokud jste dosud nevytvořili účet úložiště nebo sítě, klikněte na tlačítko **+ účet úložiště** nebo **+ síť** toocreate správce prostředků účtu nebo síti vložené.

## <a name="set-up-replication-settings"></a>Nakonfigurování nastavení replikace

Než začnete, vám zajistí rychlý přehled videa. (hello video popisuje, jak zpracovat tooreplicate virtuální počítače VMware, ale hello je podobné pro replikaci fyzického počítače.)

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. Klikněte na tlačítko toocreate novou zásadu replikace, **infrastruktura Site Recovery** > **zásady replikace** > **+ zásady replikace**.
2. V **vytvořit zásadu replikace**, zadejte název zásady.
3. V **prahovou hodnotu RPO**, zadejte limit hello plánovaný bod obnovení. Tato hodnota určuje, jak často jsou vytvořeny body obnovení data. Pokud tento limit překračuje průběžnou replikaci, je vygenerována výstraha.
4. V **uchování bodu obnovení**, určit, jak dlouho (v hodinách) pro každý bod obnovení je okno uchování hello. Replikované virtuální počítače mohou být obnovena tooany bodu v okně. Pro úložiště replikované toopremium počítače je podporováno až too24 čas uchování. Pro úložiště replikované toostandard počítače je podporováno až too72 čas uchování.
5. V **frekvence snímkování konzistentní aplikace vzhledem**, určit, jak často (v minutách) jsou vytvořeny body obnovení, které obsahují snímky konzistentní s aplikacemi. Klikněte na tlačítko **OK** toocreate hello zásad.

    ![Zásady replikace](./media/site-recovery-vmware-to-azure/gs-replication2.png)

6. Když vytvoříte novou zásadu, je automaticky přidružená hello konfigurační server. Ve výchozím nastavení je pro navrácení služeb po obnovení automaticky vytvoří odpovídající zásady. Například pokud hello zásady replikace je **rep zásad**, pak je hello navrácení služeb po obnovení zásad **rep. zásady navrácení**. Tyto zásady se nepoužije, dokud nespustíte navrácení služeb po obnovení z Azure.  


## <a name="plan-capacity"></a>Plánování kapacity

1. Teď, když máte infrastruktury základní nastavení, můžete myslíte o plánování kapacity a zjistit, jestli nepotřebujete další prostředky. [Další informace](site-recovery-plan-capacity-vmware.md).
2. Když jste hotovi s plánování kapacity, vyberte **Ano** v **dokončili jste plánování kapacity?**

   ![Plánování kapacity](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)


## <a name="prepare-vms-for-replication"></a>Příprava pro replikaci virtuálních počítačů

Všechny počítače, které chcete tooreplicate musí mít nainstalovaná služba Mobility hello. Služba Mobility hello můžete nainstalovat v několika způsoby:

- Nainstalujte službu hello nabízenou instalaci z hello procesový server. Je potřeba tooprepare počítače v předstihu toouse tuto metodu.
- Nainstalujte službu hello pomocí nástroje nasazení, například System Center Configuration Manager nebo konfigurace požadovaného stavu aplikace Azure Automation.
- Nainstalujte službu hello ručně.

[Další informace](site-recovery-vmware-to-azure-install-mob-svc.md).


## <a name="enable-replication"></a>Povolení replikace

Než začnete, potřebujete:

- Azure uživatelský účet potřebuje toohave určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci nové tooAzure virtuálního počítače.
- Když přidáváte nebo odebíráte virtuálních počítačů, může trvat až too15 minut nebo déle pro změny tootake účinek a je tooappear hello portálu.
- Čas poslední zjištěné hello můžete zkontrolovat pro virtuální počítače v **konfigurační servery** > **poslední obraťte se na**.
- tooadd virtuálních počítačů bez čekání na hello naplánovaného zjišťování, zvýraznění hello konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat**.
- Virtuální počítač připravený na nabízenou instalaci, hello procesový server automaticky nainstaluje služba Mobility hello při povolení replikace.
- Vám zajistí rychlý přehled videa. (hello video popisuje, jak zpracovat tooreplicate virtuální počítače VMware, ale hello je podobné pro replikaci fyzického počítače.)

    >[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]


### <a name="exclude-disks-from-replication"></a>Vyloučení disků z replikace

Ve výchozím nastavení se replikují všechny disky virtuálního počítače. Disky můžete z replikace vyloučit. Například chcete nemusí tooreplicate disků se dočasná data nebo data, která se má aktualizovat každý čas restartování počítače nebo aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server).

### <a name="replicate-vms"></a>Replikace virtuálních počítačů

1. Klikněte na tlačítko **replikujte aplikaci** > **zdroj**.
2. V **zdroj**, vyberte **místní**.
3. V **umístění zdroje**, vyberte název serveru konfigurace hello.
4. V **počítač typ**, vyberte **fyzických počítačů**.
5. V **procesový server**, zvolte hello procesový server. Pokud jste nevytvořili žádné další proces servery, je tento server hello konfigurační server. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-physical-to-azure/chooseVM.png)

6. V **cíl**, vyberte hello **předplatné** a hello **skupiny prostředků** chcete toocreate hello virtuální počítače Azure po převzetí služeb při selhání. Zvolte hello nasazení modelu, který má toouse v Azure (classic nebo Resource Manager) pro hello při selhání virtuálních počítačů.

7. Vyberte účet úložiště Azure hello, že který má toouse pro replikaci dat. Pokud nechcete, aby toouse účet, který jste již nastavili, můžete vytvořit nový.

8. Vyberte hello **síť Azure** a **podsíť** toowhich virtuální počítače Azure po převzetí služeb při selhání připojit. Vyberte **nakonfigurovat pro vybrané počítače** tooapply hello nastavení tooall počítače sítě je vybrat pro ochranu. Vyberte **nakonfigurovat později** tooselect hello síť Azure pro konkrétní počítač. Pokud nechcete, aby toouse existující síť, můžete vytvořit jeden.

    ![Povolení replikace](./media/site-recovery-physical-to-azure/targetsettings.png)

9. V **fyzické počítače**, klikněte na tlačítko **+ fyzický počítač** a zadejte hello **název** a **IP adresu**. Vyberte hello operační systém počítače hello chcete tooreplicate. Jak dlouho trvá několik minut, dokud nebudou zjištěny a zobrazí v seznamu hello počítače.

    ![Povolení replikace](./media/site-recovery-physical-to-azure/machineselect.png)

10. V **vlastnosti** > **konfigurovat vlastnosti**, vyberte účet hello toobe používané hello proces serveru tooautomatically instalaci služby Mobility hello na počítači hello.
11. Ve výchozím nastavení se replikují všechny disky. Klikněte na tlačítko **všechny disky**a zrušte zaškrtnutí všech disků, které nechcete tooreplicate. Pak klikněte na **OK**. Později můžete nastavit další vlastnosti virtuálního počítače.

    ![Povolení replikace](./media/site-recovery-physical-to-azure/configprop.png)

12. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, že hello správné zásady replikace je vybrána. Pokud změníte zásady, změny jsou použité toohello replikovat počítače a toonew počítače.
13. Povolit **konzistence pro víc Virtuálních** Pokud mají toogather počítače do replikační skupiny a zadejte název pro skupinu hello. Pak klikněte na **OK**. Poznámky:

    a. Počítače v replikační skupiny replikovat společně a mít sdílené body obnovení konzistentní při selhání a konzistentní s aplikací, když se převzetí služeb při selhání.

    b. Doporučujeme vám, že shromáždíte virtuálních počítačů a fyzických serverů společně, aby odpovídaly vašich úloh. Povolení konzistence více virtuálních počítačů může ovlivnit výkon zatížení. By měl použít, pouze pokud počítače hello běží stejné zatížení a potřebujete konzistenci.

    ![Povolení replikace](./media/site-recovery-physical-to-azure/policy.png)

14. Klikněte na tlačítko **povolit replikaci**. Můžete sledovat průběh hello **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**. Po hello **dokončení ochrany** úloha spustí, počítač hello je připraven na převzetí služeb při selhání.

Po povolení replikace hello služba Mobility je nainstalován, pokud jste nastavili nabízenou instalaci. Po hello služba Mobility je nainstalován na počítači, spustí úlohu ochrany a selže. Po selhání hello, je nutné toomanually každý počítač restartovat. Potom úlohu ochrany hello začne znovu a dojde k počáteční replikaci.


### <a name="view-and-manage-azure-vm-properties"></a>Zobrazení a Správa vlastností virtuálního počítače Azure

Doporučujeme, abyste ověřte vlastnosti virtuálního počítače hello a proveďte změny, které potřebujete.

1. Klikněte na tlačítko **replikované položky**a vyberte hello počítač. Hello **Essentials** okno zobrazuje informace o nastavení počítače a stav.
2. V **vlastnosti**, můžete zobrazit replikace a informace o převzetí služeb při selhání pro hello virtuálních počítačů.
3. V **výpočty a síť** > **výpočetní vlastnosti**, můžete zadat název a cílovou velikost virtuálního počítače Azure hello. Upravit název toocomply hello s [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) Pokud potřebujete.
4. Upravte nastavení pro hello Cílová síť, podsíť a IP adresu, které jsou přiřazeny toohello virtuálního počítače Azure:

    a. Můžete nastavit hello cílová IP adresa.

    b.  Pokud adresu nezadáte, počítače při selhání hello používá protokol DHCP.

    c. Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, převzetí služeb při selhání nebude fungovat.

    d. Dobrý den, které lze použít stejnou cílovou IP adresu pro testovací převzetí služeb při selhání, pokud není k dispozici v hello testovací převzetí služeb při selhání sítě hello adresa.

    e. Hello počet síťových adaptérů je závisí na velikosti hello, které zadáte pro hello cílového virtuálního počítače:

     - Pokud hello počet síťových adaptérů na zdrojovém počítači hello se hello stejné jako nebo menší než hello počet adaptérů povoleno pro velikost cílového počítače hello, pak hello cíl má hello stejný počet adaptérů jako zdroj hello.
     - Hello počet adaptérů pro hello zdrojového virtuálního počítače, překročí-li hello počet povolených pro hello cílovou velikost, maximální velikost cíle hello je použita.
     - Například pokud má zdrojový počítač dva síťové adaptéry a velikost hello cílového počítače podporuje čtyři, má hello cílový počítač dva adaptéry. Pokud má hello zdrojový počítač dva adaptéry, ale velikost cílového hello podporované podporuje pouze jeden, hello cílový počítač má jenom jeden adaptér.     
   - Pokud hello virtuální počítač má několik síťových adaptérů, budou všechny připojit toohello stejné síti.
   - Pokud hello virtuální počítač má několik síťových adaptérů, pak hello jeden uvedené v seznamu hello se stal hello *výchozí* síťový adaptér v hello virtuálního počítače Azure.
5. V **disky**, uvidíte hello virtuálních počítačů operačního systému a hello datových disků, které se replikují.

## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

Po nastavili jste si vše, spusťte toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání. Než začnete, podívejte se na video rychlý přehled.

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


1. toofail přes jeden počítač, v **nastavení** > **replikované položky**, klikněte na tlačítko **testovací převzetí služeb při selhání**.

    ![Testovací převzetí služeb při selhání](./media/site-recovery-vmware-to-azure/TestFailover.png)

2. plánování toofail přes obnovení, **nastavení** > **plány obnovení**, klikněte pravým tlačítkem na hello plán > **testovací převzetí služeb při selhání**. plán obnovení toocreate [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).  
3. V **testovací převzetí služeb při selhání**, vyberte síť Azure toowhich hello virtuálních počítačích Azure připojeni po převzetí služeb při selhání.
4. Klikněte na tlačítko **OK** toobegin hello převzetí služeb při selhání. Průběh můžete sledovat kliknutím tooopen hello virtuální počítač jeho vlastnosti, nebo kliknutím hello **testovací převzetí služeb při selhání** úlohy v název trezoru > **nastavení** > **úlohy**  >  **Úlohy site Recovery**.
5. Po dokončení převzetí služeb při selhání hello, měli byste také mít možnost toosee hello repliky počítač Azure se zobrazí v hello portálu Azure > **virtuální počítače**. Zajistěte, aby byl tento hello virtuálních počítačů hello odpovídající velikost, byl připojený toohello příslušnou síť, a zda je spuštěna.
6. Pokud jste [připravili připojení po převzetí služeb při selhání](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover), byste měli mít tooconnect toohello virtuálního počítače Azure.
7. Jakmile jste hotovi, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** v plánu obnovení hello. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání. Tento krok odstraní hello virtuálních počítačů, které byly vytvořeny během testovacího převzetí služeb při selhání.

Další informace najdete v tématu hello [testovací převzetí služeb při selhání tooAzure](site-recovery-test-failover-to-azure.md) dokumentu.

## <a name="next-steps"></a>Další kroky

Po můžete zprovoznění replikace a používá, když dojde k výpadku převzít tooAzure a virtuálních počítačích Azure jsou vytvořené pomocí hello replikovat data. Pak můžete přistupovat úlohy a aplikace v Azure, dokud se selhání primárního umístění back tooyour až se obnoví toonormal operace.

- [Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak toorun je.
- Pokud se migrace počítačů namísto replikace a selhání zpět, [Další](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- Při replikaci fyzické počítače, můžete pouze navrácení služeb po obnovení prostředí VMware tooa. [Přečtěte si informace o navrácení služeb po obnovení](site-recovery-failback-azure-to-vmware.md).

## <a name="third-party-software-notices-and-information"></a>Oznámení software třetích stran a informace
Nechcete převede nebo lokalizaci

Hello software a firmware spuštěné v hello produktů společnosti Microsoft nebo služba je založena na nebo zahrnuje materiálu z hello projekty uvedené níže (dále souhrnně nazývané "kód třetí strany"). Společnost Microsoft není hello původní autor hello kód třetích stran. Hello původní autorská práva a licenci, pod kterým společnost Microsoft takový kód třetích stran, jsou nastavené níže uvedenými.

Hello informace v části A týká níže uvedené součásti z projektů hello kód třetích stran. Tyto licence a informace jsou uvedené pro pouze pro informační účely. Tento kód třetích stran, která má být relicensed tooyou společností Microsoft v rámci licenční podmínky pro produkty Microsoft hello nebo služby společnosti Microsoft software.  

Hello informace v části B je týkající se součástí kód třetích stran, které jsou určené k dispozici tooyou společností Microsoft v rámci hello původní licenční podmínky.

Hello dokončení soubor se nachází na hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Společnost Microsoft si vyhrazuje všechna práva, která nejsou výslovně udělena, zda implicitně, jako překážku uplatnění nároku, nebo jinak.
