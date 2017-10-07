---
title: "virtuální počítače VMware tooAzure aaaReplicate | Microsoft Docs"
description: "Shrnuje hello kroky, které potřebujete pro replikaci úloh běžících na virtuálních počítačích VMware tooAzure úložiště"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: vmware-walkthrough-overview
ms.openlocfilehash: f800e7d8a3b59a86809a995748eacf87630a1713
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-virtual-machines-tooazure-with-site-recovery"></a>Replikovat tooAzure virtuální počítače VMware s Site Recovery

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmware-to-azure.md)
> * [Azure Classic](site-recovery-vmware-to-azure-classic.md)


Tento článek popisuje, jak tooreplicate místní tooAzure virtuální počítače VMware, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

Pokud chcete virtuální počítače VMware tooAzure toomigrate (převzetí služeb při selhání pouze), přečtěte si [v tomto článku](site-recovery-migrate-to-azure.md) toolearn Další.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="deployment-steps"></a>Kroky nasazení

Zde je budete potřebovat toodo:

1. Ověřte požadavky a omezení.
2. Nastavte síť Azure a účty úložiště.
3. Příprava hello na místním počítači, který chcete toodeploy jako hello konfigurační server.
4. Připravte účty VMware toobe použité pro automatické zjišťování virtuálních počítačů a volitelně pro nabízenou instalaci služby Mobility hello.
4. Vytvořte trezor služby Recovery Services. Trezor Hello obsahuje nastavení konfigurace a orchestruje replikaci.
5. Zadejte nastavení zdroje, cíl a replikace.
6. Nasazení služby Mobility hello na virtuálních počítačích chcete tooreplicate.
7. Povolení replikace pro virtuální počítače hello.
7. Spusťte toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.

## <a name="prerequisites"></a>Požadavky

**Požadavek na podporu** | **Podrobnosti**
--- | ---
**Azure** | Další informace o [požadavky pro Azure](site-recovery-prereq.md#azure-requirements)
**Lokální konfigurační server** | Je třeba virtuální počítače VMware s Windows serverem 2012 R2 nebo novější. Nastavit tento server během nasazování Site Recovery.<br/><br/> Výchozí proces hello jsou server a hlavní cílový server také nainstalované na tomto virtuálním počítači. Když jste vertikální navýšení kapacity, může být nutné samostatný procesový server a má hello stejné požadavky jako hello konfigurační server.<br/><br/> Další informace o těchto součástí [sem](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)
**Na místní servery VMware** | Jeden nebo více VMware vSphere serverů, 6.0, 5.5, 5.1 s nejnovější aktualizace. Servery musí nacházet ve stejné sítě jako konfigurační server hello (nebo samostatný procesový server) hello.<br/><br/> Doporučujeme systému vCenter server toomanage hostitele, kde běží 6.0 nebo 5.5 s nejnovějšími aktualizacemi hello. Při nasazení verze 6.0 jsou podporovány pouze funkce, které jsou k dispozici v 5.5.
**Místní virtuální počítače** | Virtuální počítače chcete tooreplicate by měla být spuštěná [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)a v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). Virtuální počítač by měl mít spuštění nástroje VMware.
**Adresy URL** | konfigurační server Hello potřebuje přístup k toothese adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.<br/></br> Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).<br/><br/> Povolit tuto adresu URL pro stahování MySQL hello: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Služba mobility** | Nainstalovat na každý replikovaných virtuálních počítačů.




## <a name="limitations"></a>Omezení

**Omezení** | **Podrobnosti**
--- | ---
**Azure** | Účty úložiště a sítě musí být v hello stejné oblasti jako trezor hello<br/><br/> Pokud používáte prémiový účet úložiště, musíte také standardní ukládat protokoly replikace toostore účtu<br/><br/> Nelze replikovat toopremium účty ve střední a – Jih, Indie.
**Lokální konfigurační server** | Typ adaptéru virtuálního počítače VMware musí být VMXNET3. Pokud tomu tak není, [tuto aktualizaci nainstalovat](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> vSphere PowerCLI 6.0 by měly být nainstalovány.<br/><br> Hello počítač by neměl být řadič domény. Hello počítač by měl mít statickou IP adresu.<br/><br/> název hostitele Hello by měl být maximálně 15 znaků nebo méně, a operační systém by měl být v angličtině.
**VMware** | Jsou podporovány pouze verzi 5.5 funkce v systému vCenter 6.0. Site Recovery nepodporuje nové vCenter a vSphere 6.0 funkce, jako křížové řešení vMotion vCenter, virtuální svazky a úložiště DRS.
**Virtuální počítače** | Ověřte [omezení virtuálního počítače Azure](site-recovery-prereq.md#azure-requirements)<br/><br/> Nelze replikovat virtuální počítače s šifrované disky nebo virtuální počítače pomocí rozhraní UEFI nebo EFI.<br/><br> Sdílený disk, clustery nejsou podporované. Pokud hello zdrojového virtuálního počítače má seskupování síťových adaptérů, převede se tooa jeden síťový adaptér po převzetí služeb při selhání.<br/><br/> Pokud virtuální počítače iSCSI disk, Site Recovery převede souboru virtuálního pevného disku tooa po převzetí služeb při selhání. Pokud hello cíle iSCSI, dosažitelný z hello virtuálního počítače Azure, připojí tooit a zobrazí se i hello virtuálního pevného disku. Pokud k tomu dojde, odpojte hello cíle iSCSI.<br/><br/> Pokud chcete, aby tooenable více virtuálních počítačů konzistence, která umožňuje virtuální počítače se systémem hello stejné zatížení toobe obnovit společně tooa konzistentní datový bod, otevřete port 20004 hello virtuálních počítačů.<br/><br/> Windows musí být nainstalován na jednotce hello C. disk s operačním systémem Hello by měl být základní a nikoli dynamické. Hello datový disk může být dynamické.<br/><br/> Soubory Linux/etc/hosts na virtuálních počítačích by měly obsahovat položky, které mapují hello místního hostitele název tooIP adres přidružených všechny síťové adaptéry. Hello název hostitele, přípojné body, název zařízení, systémové cesty a názvy souborů (/ etc; USR) by měly být pouze v angličtině.<br/><br/> Konkrétní typy [Linux úložiště](site-recovery-support-matrix-to-azure.md#support-for-storage) jsou podporovány.<br/><br/>Vytvořit nebo nastavit **disk.enableUUID=true** v nastavení virtuálního počítače hello. To poskytuje konzistentní UUID toohello VMDK, tak, aby správně připojí a zajišťuje, aby jenom rozdílové změny přenášená back místní tooon během navrácení služeb po obnovení bez úplná replikace.

## <a name="set-up-azure"></a>Nastavení Azure

1. [Nastavit síť Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).
    - Virtuální počítače Azure budou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.
    - Můžete nastavit síť v [Resource Manager](../resource-manager-deployment-model.md), nebo v klasickém režimu.

2. Nastavení [účtu úložiště Azure](../storage/storage-create-storage-account.md#create-a-storage-account) pro replikovaná data.
    - Hello účet může být standardní nebo [premium](../storage/storage-premium-storage.md).
    - Můžete nastavit účet ve službě Správce prostředků, nebo v klasickém režimu.

3. [Příprava účet](#prepare-for-automatic-discovery-and-push-installation) hello vCenter server nebo hostitelů vSphere, tak obnovení lokality může automaticky zjistit virtuální počítače VMware.

## <a name="prepare-hello-configuration-server"></a>Příprava hello konfigurace serveru

1. Nainstalujte Windows Server 2012 R2 nebo novější, na virtuální počítače VMware.
2. Ověřte, zda text hello virtuálních počítačů má přístup toohello adresy URL uvedené v [požadavky](#prerequisites).
3. Nainstalujte [VMware vSphere PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1).


## <a name="prepare-for-automatic-discovery-and-push-installation"></a>Příprava pro automatické zjišťování a vynucená instalace

- **Příprava účtu pro automatické zjišťování**: hello Site Recovery procesový server automaticky vyhledá virtuální počítače. toodo potřebám přihlašovací údaje se Site Recovery, kteří mohou přistupovat k servery vCenter server a hostitelů vSphere ESXi.

    1. toouse vyhrazený účet, vytvoření role (na úrovni vCenter hello, pomocí těchto [oprávnění](#vmware-account-permissions). Pojmenujte ji jako **Azure_Site_Recovery**.
    2. Potom vytvořte uživatele na serveru hostitele/vCenter vSphere hello a přiřaďte hello role toohello uživatele. Tento uživatelský účet je zadat během nasazování Site Recovery.

- **Příprava toopush hello účtu služby Mobility**: Pokud chcete toopush hello Mobility service tooVMs, potřebujete účet, který mohou být využívána hello proces serveru tooaccess hello virtuálních počítačů. Hello účet se používá pouze pro hello nabízenou instalaci. Můžete použít domény nebo místní účet:

    - Pro Windows Pokud nepoužíváte účet domény, budete potřebovat toodisable řízení vzdáleného přístupu uživatele v místním počítači hello. toodo, informace v hello zaregistrovat v rámci **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD hello **LocalAccountTokenFilterPolicy**, s hodnotou 1.
    - Pokud chcete položku registru hello tooadd pro Windows z rozhraní příkazového řádku, zadejte:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
    - Pro systémy Linux hello účet by měl být kořenový na zdrojovém serveru se Linux hello.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-hello-protection-goal"></a>Vyberte cíl ochrany hello

Vyberte, co chcete tooreplicate, a místo, kam chcete tooreplicate k.

1. Klikněte na tlačítko **trezory služeb zotavení** > trezoru.
2. V hello prostředků nabídky, klikněte na **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-vmware-to-azure/choose-goals.png)
3. V **cíl ochrany**, vyberte **tooAzure** > **Ano, s hypervisoru VMware vSphere**.

    ![Zvolte cíle.](./media/site-recovery-vmware-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí

Nastavení konfigurace serveru hello, zaregistrovat v trezoru hello a vyhledání virtuálních počítačů.

1. Klikněte na tlačítko **lokality obnovení** > **krok 1: Příprava infrastruktury** > **zdroj**.
2. Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server**.

    ![Nastavení zdroje](./media/site-recovery-vmware-to-azure/set-source1.png)
3. V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
4. Stáhněte si soubor instalace hello Unified instalace nástroje Site Recovery.
5. Stáhněte si registrační klíč trezoru hello. Tuto funkci potřebujete po spuštění Unified instalace. Hello klíč je platný pět dní od jeho vygenerování.

   ![Nastavení zdroje](./media/site-recovery-vmware-to-azure/set-source2.png)


## <a name="run-site-recovery-unified-setup"></a>Sjednocený instalační program spusťte Site Recovery

Následující hello před start a potom spusťte tooinstall hello konfigurační server, hello procesový server a hlavní cílový server hello sjednocený instalační program.
    - Vám zajistí rychlý přehled video

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - Ujistěte se, že hello systémové hodiny synchronizované s na konfigurační server hello virtuálních počítačů, [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Měla by se shodovat. Pokud je 15 minut před nebo za instalace může selhat.
    - Spusťte instalační program jako místní správce na konfigurační server hello virtuálních počítačů.
    - Ujistěte se, že je povolená TLS 1.0 na hello virtuálních počítačů.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Hello konfigurační server lze také nainstalovat [z příkazového řádku hello](http://aka.ms/installconfigsrv).



### <a name="add-hello-account-for-automatic-discovery"></a>Přidat účet hello pro automatické zjišťování

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="connect-toovmware-servers"></a>Připojte tooVMware servery

Připojte hostitele ESXi toovSphere nebo servery vCenter server, virtuální počítače VMware toodiscover.

- Pokud přidáte hello vCenter server nebo hostitelů vSphere pomocí účtu bez oprávnění správce na serveru hello, potřebuje účet hello těchto oprávnění povoleno:
    - Datacenter, úložiště, složka, hostitele, sítě, prostředků, virtuální počítač, vSphere distribuované přepínače.
    - Hello vCenter server potřebuje oprávnění úložišť zobrazení.
- Když přidáte servery VMware, může trvat 15 minut nebo déle pro ně tooappear hello portálu.
tooallow Azure Site Recovery toodiscover virtuální počítače běžící na vašem místním prostředí, musíte tooconnect serveru VMware vCenter nebo hostitelů vSphere ESXi službou Site Recovery.

Vyberte **+ vCenter** toostart připojuje VMware vCenter server nebo hostiteli ESXi VMware vSphere.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

Obnovení lokality se připojuje tooVMware servery pomocí hello zadané nastavení a vyhledá virtuální počítače.

## <a name="set-up-hello-target"></a>Nastavení cílového hello

Před nastavením hello cílové prostředí, zkontrolujte, máte [účtu úložiště Azure a sítě](#set-up-azure)

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**, a vyberte hello chcete toouse předplatného Azure.
2. Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

   ![cíl](./media/site-recovery-vmware-to-azure/gs-target.png)
4. Pokud jste dosud nevytvořili účet úložiště nebo sítě, klikněte na tlačítko **+ účet úložiště** nebo **+ síť**, toocreate správce prostředků účtu nebo síti vložené.

## <a name="set-up-replication-settings"></a>Nakonfigurování nastavení replikace

Vám zajistí rychlý přehled videa, než začnete:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. Klikněte na tlačítko toocreate novou zásadu replikace, **infrastruktura Site Recovery** > **zásady replikace** > **+ zásady replikace**.
2. V **vytvořit zásadu replikace**, zadejte název zásady.
3. V **prahovou hodnotu RPO**, zadejte limit hello plánovaný bod obnovení. Tato hodnota určuje, jak často jsou vytvořeny body obnovení data. Pokud tento limit překračuje průběžnou replikaci, je vygenerována výstraha.
4. V **uchování bodu obnovení**, určit, jak dlouho (v hodinách) pro každý bod obnovení je okno uchování hello. Replikované virtuální počítače mohou být obnovena tooany bodu v okně. Až too24 čas uchování je podporována pro počítače replikují toopremium úložiště a 72 hodin, než standardní úložiště.
5. V **frekvence snímkování konzistentní aplikace vzhledem**, určit, jak často (v minutách) budou vytvořeny body obnovení obsahující snímky konzistentní s aplikacemi. Klikněte na tlačítko **OK** toocreate hello zásad.

    ![Zásady replikace](./media/site-recovery-vmware-to-azure/gs-replication2.png)
8. Když vytvoříte novou zásadu je přidružená automaticky hello konfigurační server. Ve výchozím nastavení je pro navrácení služeb po obnovení automaticky vytvoří odpovídající zásady. Například pokud hello zásady replikace je **rep zásad** bude hello navrácení služeb po obnovení zásad **rep. zásady navrácení**. Tyto zásady se nepoužije, dokud nespustíte navrácení služeb po obnovení z Azure.  



## <a name="plan-capacity"></a>Plánování kapacity

1. Teď, když máte základní infrastrukturu, můžete nastavit můžete myslíte o plánování kapacity a zjistit, jestli nepotřebujete další prostředky. [Další informace](site-recovery-plan-capacity-vmware.md).
2. Když jste hotovi s plánování kapacity, vyberte **Ano** v **dokončili jste plánování kapacity?**

   ![Plánování kapacity](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)


## <a name="prepare-vms-for-replication"></a>Příprava pro replikaci virtuálních počítačů

Služba Mobility Hello musí být nainstalován na všech virtuálních počítačích VMware, které chcete tooreplicate. Služba Mobility hello můžete nainstalovat v několika způsoby:

1. Instalace pomocí nabízené instalace z hello procesový server. Tato metoda musíte toouse tooprepare virtuálních počítačů.
2. Instalace pomocí nástroje nasazení, například System Center Configuration Manager nebo služby Azure automation DSC.
3.  Nainstalujte ručně.

[Další informace](site-recovery-vmware-to-azure-install-mob-svc.md)


## <a name="enable-replication"></a>Povolení replikace

Než začnete, potřebujete:

- Azure uživatelský účet potřebuje toohave určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci nové tooAzure virtuálního počítače.
- Když přidáváte nebo odebíráte virtuálních počítačů, může trvat až too15 minut nebo déle pro změny tootake účinek a jejich tooappear hello portálu.
- Čas poslední zjištěné hello můžete zkontrolovat pro virtuální počítače v **konfigurační servery** > **poslední obraťte se na**.
- tooadd virtuálních počítačů bez čekání na hello naplánovaného zjišťování, zvýraznění hello konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat**.
- Virtuální počítač připravený na nabízenou instalaci, hello procesový server automaticky nainstaluje služba Mobility hello při povolení replikace.


### <a name="exclude-disks-from-replication"></a>Vyloučení disků z replikace

Ve výchozím nastavení jsou všechny disky na počítači replikují. Disky můžete z replikace vyloučit. Například chcete nemusí tooreplicate disků se dočasná data nebo data, která se má aktualizovat pokaždé, když je počítač nebo restartování aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server).

### <a name="replicate-vms"></a>Replikace virtuálních počítačů

Než začnete, podívejte se na rychlé video s přehledem

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**.
2. V **zdroj**vyberte hello konfigurační server.
3. V **počítač typ**, vyberte **virtuální počítače**.
4. V **vCenter vSphere Hypervisor**, vyberte hello vCenter server, který spravuje hostitelů vSphere hello nebo vyberte hello hostitele.
5. Vyberte procesový server hello. Pokud jste nevytvořili žádné servery další proces bude hello konfigurační server. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. V **cíl**, vyberte předplatné hello a hello skupinu prostředků, ve kterém chcete toocreate hello převzal virtuálních počítačů. Vyberte model nasazení hello, který chcete toouse v Azure (klasický nebo prostředek management), pro hello převzal virtuální počítače.


7. Vyberte účet úložiště Azure hello, že který má toouse pro replikaci dat. Pokud nechcete, aby toouse účet, který jste již nastavili, můžete vytvořit nový.

8. Vyberte hello Azure toowhich síť a podsíť virtuálních počítačů Azure se připojí, když jste vytvořili po převzetí služeb při selhání. Vyberte **nakonfigurovat pro vybrané počítače**, tooapply hello nastavení tooall počítače sítě je vybrat pro ochranu. Vyberte **nakonfigurovat později** tooselect hello síť Azure pro konkrétní počítač. Pokud nechcete, aby toouse existující síť, můžete vytvořit jeden.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. V **virtuální počítače** > **vybrat virtuální počítače**, klikněte na tlačítko a vyberte každý počítač má tooreplicate. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. V **vlastnosti** > **konfigurovat vlastnosti**vyberte hello účet, který se použije hello proces serveru tooautomatically instalaci služby Mobility hello na počítači hello.
11. Ve výchozím nastavení jsou všechny disky replikovat. Klikněte na tlačítko **všechny disky** a zrušte zaškrtnutí všech disků, které nechcete tooreplicate. Pak klikněte na **OK**. Později můžete nastavit další vlastnosti virtuálního počítače.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication6.png)
11. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, že hello správné zásady replikace je vybrána. Pokud změníte zásady, změny budou použité tooreplicating počítač a toonew počítače.
12. Povolit **konzistence pro víc Virtuálních** Pokud mají toogather počítače do replikační skupiny a zadejte název pro skupinu hello. Pak klikněte na **OK**. Poznámky:

    * Počítače v replikační skupiny replikovat společně a mít sdílené body obnovení konzistentní při selhání a konzistentní s aplikací, když se převzetí služeb při selhání.
    * Doporučujeme vám, že shromáždíte virtuálních počítačů a fyzických serverů společně, aby odpovídaly vašich úloh. Povolení konzistence více virtuálních počítačů může ovlivnit výkon pracovního vytížení a musí být použit pouze pokud počítače běží hello stejné úlohy a potřebujete konzistence.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication7.png)
13. Klikněte na tlačítko **povolit replikaci**. Můžete sledovat průběh hello **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**. Po hello **dokončení ochrany** úloha spustí počítač hello je připraven na převzetí služeb při selhání.

### <a name="view-and-manage-vm-properties"></a>Zobrazení a správa vlastností virtuálního počítače

Doporučujeme, abyste ověřte vlastnosti virtuálního počítače hello a proveďte změny, které potřebujete.

1. Klikněte na tlačítko **replikované položky** > a vyberte hello počítač. Hello **Essentials** okno zobrazuje informace o nastavení počítače a stav.
2. V **vlastnosti**, můžete zobrazit replikace a informace o převzetí služeb při selhání pro hello virtuálních počítačů.
3. V **výpočty a síť** > **výpočetní vlastnosti**, můžete zadat název a cílovou velikost virtuálního počítače Azure hello. Upravit název toocomply hello s [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) Pokud potřebujete.
4. Upravte nastavení pro hello Cílová síť, podsíť a IP adresu, která bude přiřazena toohello virtuálního počítače Azure:

   - Můžete nastavit hello cílová IP adresa.

    - Pokud adresu nezadáte, použije hello převzal počítač DHCP.
    - Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, převzetí služeb při selhání nebude fungovat.
    - Dobrý den, lze použít stejnou cílovou IP adresu pro převzetí služeb při selhání, pokud není k dispozici v hello testovací převzetí služeb při selhání sítě hello adresa.

   - Hello počet síťových adaptérů je závisí na velikosti hello, které zadáte pro hello cílového virtuálního počítače:

     - Pokud hello počet síťových adaptérů na zdrojovém počítači hello se hello stejné jako nebo menší než hello počet adaptérů povoleno pro velikost cílového počítače hello, pak bude mít cíl hello hello stejný počet adaptérů jako zdroj hello.
     - Pokud hello počet adaptérů pro hello zdrojového virtuálního počítače překračuje hello počet povolený pro cílovou velikost hello, pak se použije maximální velikost cíle hello.
     - Například pokud má zdrojový počítač dva síťové adaptéry a velikost hello cílového počítače podporuje čtyři, bude mít hello cílový počítač dva adaptéry. Pokud má hello zdrojový počítač dva adaptéry, ale hello podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač hello jenom jeden adaptér.     
   - Pokud hello virtuální počítač více síťových adaptérů připojí se všechny toohello stejné síti.
   - Pokud hello virtuální počítač více síťových adaptérů pak hello jeden uvedené v seznamu hello se stal hello *výchozí* síťový adaptér v hello virtuální počítač Azure.
4. V **disky**, uvidíte hello virtuálních počítačů operačního systému a dat hello disky, které bude replikován.

#### <a name="managed-disks"></a>Managed Disks

V **výpočty a síť** > **výpočetní vlastnosti**, "Použití spravovaných disků" nastavení příliš "Ano" hello virtuálních počítačů můžete nastavit, pokud chcete počítač tooyour tooattach spravovaných disků na tooAzure převzetí služeb při selhání. Spravované disky zjednodušuje Správa disku pro virtuální počítače Azure IaaS pomocí správy hello účty úložiště přidružené ke hello disky virtuálních počítačů. [Další informace o spravovaných disky.](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview)

   - Spravované disky jsou vytvořené a připojené toohello virtuální počítač jenom na tooAzure převzetí služeb při selhání. K povolení ochrany, data z místního počítače bude tooreplicate toostorage účty.  Spravované disky se dají vytvořit jenom pro virtuální počítače nasazené pomocí modelu nasazení Resource manager hello.  

   - Při nastavení "Použití spravovaných disků" příliš "Ano", pouze skupiny dostupnosti ve skupině prostředků hello sadou "Použití spravovaných disků" příliš "Ano" by být k dispozici pro výběr. Je to proto, že virtuální počítače s spravované disky lze pouze součástí skupiny dostupnosti s "Použití spravovaných disků" vlastností nastavenou příliš "Ano". Zajistěte, aby vytváření skupiny dostupnosti s nastavenou vlastnost "Použití spravovaných disků" založené na discích záměrné toouse spravované na převzetí služeb při selhání.  Podobně když nastavíte "Použití spravovaných disků" příliš "žádný", pouze skupiny dostupnosti ve skupině prostředků hello s "Použití spravovaných disků" vlastností nastavenou příliš "žádný" by být k dispozici pro výběr. [Přečtěte si další informace o spravovaných discích a skupinách dostupnosti](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Pokud účet úložiště hello používané pro replikaci byla zašifrovaná pomocí šifrování služby úložiště v libovolném bodě v čase, vytvoření spravovaných disků během převzetí služeb při selhání se nezdaří. Můžete buď nastavit "Použití spravovaných disků" příliš "žádný" a zkuste zopakovat převzetí služeb při selhání nebo zakažte ochranu pro virtuální počítač hello a chránit tooa účet úložiště, který nemá povolené v libovolném bodě v čase šifrování služby úložiště.
  > [Další informace o šifrování služby Storage a o spravovaných discích](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption)


## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání


Poté, co jste nastavili vše, spusťte toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání. Vám zajistí rychlý přehled videa, než začnete
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


1. toofail přes jeden počítač, v **nastavení** > **replikované položky**, klikněte na tlačítko hello virtuálního počítače > **+ testovací převzetí služeb při selhání** ikonu.

    ![Testovací převzetí služeb při selhání](./media/site-recovery-vmware-to-azure/TestFailover.png)

1. plánování toofail přes obnovení, **nastavení** > **plány obnovení**, klikněte pravým tlačítkem na hello plán > **testovací převzetí služeb při selhání**. plán obnovení toocreate [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).  

1. V **testovací převzetí služeb při selhání**, vyberte síť Azure toowhich hello virtuální počítače Azure připojí po převzetí služeb při selhání.

1. Klikněte na tlačítko **OK** toobegin hello převzetí služeb při selhání. Průběh můžete sledovat, když kliknete na virtuální počítač tooopen hello jeho vlastnosti, nebo na hello **testovací převzetí služeb při selhání** úlohy v název trezoru > **nastavení** > **úlohy**  >  **Úlohy site Recovery**.

1. Po dokončení převzetí služeb při selhání hello, měli byste také mít možnost toosee hello repliky počítač Azure se zobrazí v hello portálu Azure > **virtuální počítače**. Měli byste si ověřit, že hello virtuálního počítače je hello odpovídající velikost, byl připojený toohello příslušnou síť, a zda je spuštěna.

1. Pokud jste [připravili připojení po převzetí služeb při selhání](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover), byste měli mít tooconnect toohello virtuálního počítače Azure.

1. Jakmile jste hotovi, klikněte na **vyčistit testovací převzetí služeb při selhání** v plánu obnovení hello. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání. Tato akce odstraní hello virtuálních počítačů, které byly vytvořeny během testovacího převzetí služeb při selhání.

[Další informace](site-recovery-test-failover-to-azure.md) o testovací převzetí služeb při selhání.


## <a name="vmware-account-permissions"></a>Oprávnění pro uživatelský účet VMware

Site Recovery je potřeba tooVMware přístup pro hello proces serveru tooautomatically zjišťování virtuálních počítačů a pro převzetí služeb při selhání a navrácení služeb po obnovení virtuálních počítačů.

- **Migrace**: Pokud chcete pouze toomigrate tooAzure virtuální počítače VMware, bez někdy je selhání zpět, je použít účet VMware s rolí jen pro čtení. Tyto role můžete spustit převzetí služeb při selhání, ale nelze vypnout chráněných zdrojových počítačů. Tato akce není nezbytné pro migraci.
- **Replikovat nebo obnovit**: Pokud chcete účet hello toodeploy úplná replikace (replikace, převzetí služeb při selhání, navrácení služeb po obnovení) musí být schopný toorun operací, jako je vytváření a odebrání disků, zapínání virtuální počítače atd.
- **Automatické zjišťování**: je požadován alespoň účet jen pro čtení.


**Úkol** | **Požadovaný účet nebo role** | **Oprávnění** | **Podrobnosti**
--- | --- | --- | ---
**Procesový server automaticky vyhledá virtuální počítače VMware** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate tooChild objekt, role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup k objektům hello tooall v datovém centru hello.<br/><br/> toorestrict přístup, přiřaďte hello **žádný přístup** role s hello **rozšířit toochild** objektu toohello podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).
**Převzetí služeb při selhání** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate tooChild objekt, role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup k objektům hello tooall v datovém centru hello.<br/><br/> toorestrict přístup, přiřaďte hello **žádný přístup** role s hello **rozšířit toochild** objektu toohello podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).<br/><br/> Tato možnost je užitečná pro účely migrace, ale není úplná replikace, převzetí služeb při selhání a navrácení služeb po obnovení.
**Převzetí služeb při selhání a navrácení služeb po obnovení** | Doporučujeme vytvořit roli (Azure_Site_Recovery) s hello vyžaduje oprávnění a pak mu přiřaďte hello role tooa VMware uživatel nebo skupina | Objekt datového centra –> Propagate tooChild objekt, role = Azure_Site_Recovery<br/><br/> Úložiště dat -> přidělte místo, procházet úložiště dat, operace se soubory nízké úrovně, odstraňte soubor, aktualizovat soubory virtuálního počítače<br/><br/> Síť -> přiřazení sítě<br/><br/> Prostředků -> fondu tooresource přiřazení virtuálního počítače, migraci napájený vypnout virtuální počítač, migrace napájený na virtuálním počítači<br/><br/> Úlohy -> Vytvořit úlohu, úloha aktualizace<br/><br/> Virtuální počítač -> Konfigurace<br/><br/> Virtuální počítač -> interakcí -> odpovědí otázku, připojení zařízení, nakonfigurovat média CD, nakonfigurovat disketová média, vypnout, zapnutí, instalaci nástroje VMware<br/><br/> Virtuální počítač -> inventáře -> vytvořit, registraci, zrušení registrace<br/><br/> Virtuální počítač -> zřizování -> Povolit stahování virtuálního počítače, povolí nahrát soubory virtuálního počítače<br/><br/> Virtuální počítač -> snímky -> odebrat snímky | Uživatel přiřazené úrovni datacenter a má přístup k objektům hello tooall v datovém centru hello.<br/><br/> toorestrict přístup, přiřaďte hello **žádný přístup** role s hello **rozšířit toochild** objektu toohello podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).


## <a name="next-steps"></a>Další kroky

Po můžete zprovoznění replikace a používá, když dojde k výpadku převzít tooAzure a virtuálních počítačích Azure jsou vytvořené pomocí hello replikovat data. Pak můžete přistupovat úlohy a aplikace v Azure, dokud se selhání primárního umístění back tooyour až se obnoví toonormal operace.

- [Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak toorun je.
- Pokud se migrace počítačů namísto replikace a selhání zpět, [Další](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- [Přečtěte si informace o navrácení služeb po obnovení](site-recovery-failback-azure-to-vmware.md), toofail zpět a replikaci virtuálních počítačů Azure zpět toohello primární místního webu.

## <a name="third-party-software-notices-and-information"></a>Oznámení software třetích stran a informace
Nechcete převede nebo lokalizaci

Hello software a firmware spuštěné v hello produktů společnosti Microsoft nebo služba je založena na nebo zahrnuje materiálu z hello projekty uvedené níže (dále souhrnně nazývané "kód třetí strany").  Společnost Microsoft se hello není původní autor hello kód třetích stran.  Hello původní autorská práva a licenci, pod kterým společnost Microsoft takový kód třetích stran, jsou nastavené níže uvedenými.

Hello informace v části A týká níže uvedené součásti z projektů hello kód třetích stran. Tyto licence a informace jsou uvedené pro pouze pro informační účely.  Tento kód třetích stran, která má být relicensed tooyou společností Microsoft v rámci licenční podmínky pro produkty Microsoft hello nebo služby společnosti Microsoft software.  

Hello informace v části B je týkající se součástí kód třetích stran, které jsou určené k dispozici tooyou společností Microsoft v rámci hello původní licenční podmínky.

Hello celého souboru může být nalezen na hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Společnost Microsoft si vyhrazuje všechna práva, která nejsou výslovně udělena, zda implicitně, jako překážku uplatnění nároku či jiným způsobem.
