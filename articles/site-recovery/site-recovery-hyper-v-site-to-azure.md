---
title: "virtuální počítače Hyper-V tooAzure aaaReplicate | Microsoft Docs"
description: "Popisuje, jak tooorchestrate replikace, převzetí služeb při selhání a obnovení místní technologie Hyper-V virtuální počítače tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 04/05/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: hyper-v-site-walkthrough-overview
ms.openlocfilehash: 6fba41e43823fc57511d51ea2e09691159693982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-tooazure-using-azure-site-recovery-with-hello-azure-portal"></a>Replikaci technologie Hyper-V virtuální počítače (bez VMM) tooAzure hello portálu Azure pomocí Azure Site Recovery

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Azure Classic](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

Tento článek popisuje, jak tooreplicate místní tooAzure virtuální počítače Hyper-V, pomocí [Azure Site Recovery](site-recovery-overview.md) v hello portálu Azure. Ve virtuálních počítačích této nasazení technologie Hyper-V nejsou spravovány nástrojem System Center Virtual Machine Manager (VMM).

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Pokud chcete, aby počítače tooAzure toomigrate (bez navrácení služeb po obnovení), přečtěte si informace v [v tomto článku](site-recovery-migrate-to-azure.md).



## <a name="deployment-steps"></a>Kroky nasazení

Postupujte podle článku toocomplete hello tyto kroky nasazení:

1. [Další informace](site-recovery-components.md#hyper-v-to-azure) o architektuře hello pro toto nasazení. Dále si [můžete přečíst](site-recovery-hyper-v-azure-architecture.md), jak ve službě Site Recovery funguje replikace Hyper-V.
2. Ověřte požadavky a omezení.
3. Nastavte síť Azure a účty úložiště.
4. Příprava hostitele Hyper-V.
5. Vytvořte trezor služby Recovery Services. Trezor Hello obsahuje nastavení konfigurace a orchestruje replikaci.
6. Zadejte nastavení zdroje. Vytvořit web technologie Hyper-V, který obsahuje hello hostitelů Hyper-V a zaregistrujte hello lokality v trezoru hello. Nainstalujte hello zprostředkovatele Azure Site Recovery a agenta služeb zotavení Microsoft hello, na hostitelích hello technologie Hyper-V.
7. Nastavte cíl a replikaci.
8. Povolení replikace pro virtuální počítače hello.
9. Spusťte toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.



## <a name="prerequisites"></a>Požadavky


**Požadavek** | **Podrobnosti** |
--- | --- |
**Azure** | Přečtěte si o [požadavcích na Azure](site-recovery-prereq.md#azure-requirements).
**Místní servery** | [Další informace](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager) o požadavcích pro hostitele Hyper-V místní hello.
**Místní virtuální počítače Hyper-V** | Virtuální počítače chcete tooreplicate by měla být spuštěná [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)a v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Adresy URL Azure** | Hostitelé Hyper-V, potřebujete přístup toothese adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.<br/></br> Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).



## <a name="prepare-for-deployment"></a>Příprava nasazení

tooprepare pro nasazení, které potřebujete:

1. [Nastavit síť Azure](#set-up-an-azure-network) ve kterém virtuální počítače Azure budou umístěné když jste vytvořili po převzetí služeb při selhání.
2. [Nastavit účet úložiště Azure](#set-up-an-azure-storage-account) pro replikovaná data.
3. [Příprava hello technologie Hyper-V hosts](#prepare-the-hyper-v-hosts) tooensure přístupem hello požadované adresy URL.

### <a name="set-up-an-azure-network"></a>Nastavení sítě Azure

Nastavte síť Azure. Musíte to tak, aby virtuální počítače Azure hello vytvořené po převzetí služeb při selhání připojené tooa sítě.

* Hello síť musí být ve hello stejné oblasti jako hello trezoru služeb zotavení.
* V závislosti na modelu prostředků hello chcete toouse pro převzetí služeb virtuálních počítačích Azure při selhání, nastavíte hello síť Azure [režimu Resource Manager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md), nebo [klasickém režimu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* Doporučujeme nastavit síť ještě před tím, než začnete. Pokud to neuděláte, musíte toodo ji během nasazování Site Recovery.
- Účty úložiště používané službou Site Recovery nemůže být [přesunout](../azure-resource-manager/resource-group-move-resources.md) v rámci hello stejné, nebo v různých, předplatných.


### <a name="set-up-an-azure-storage-account"></a>Nastavení účtu úložiště Azure

- Je nutné účtu úložiště Azure, že toohold data replikují tooAzure standard nebo premium. [Storage úrovně premium](../storage/storage-premium-storage.md) se obvykle používá u virtuálních počítačů, které je třeba konzistentně vysoký výkon vstupně-výstupní operace a s nízkou latencí toohost vstupně-výstupní operace náročné úlohy.
- Pokud chcete toouse toostore účtu premium replikovaná data, musíte taky standardní úložiště účet toostore protokoly replikace, zachycení probíhající změny tooon místní data.
- V závislosti na modelu prostředků hello chcete toouse pro převzetí služeb virtuálních počítačích Azure při selhání, nastavíte účet v [režimu Resource Manager](../storage/storage-create-storage-account.md), nebo [klasickém režimu](../storage/storage-create-storage-account-classic-portal.md).
- Doporučujeme, abyste před zahájením nastavení účtu úložiště. Pokud to neuděláte, budete potřebovat toodo ji během nasazování Site Recovery. Hello účty musí být v hello stejné oblasti jako hello trezoru služeb zotavení.
- Nelze přesunout, účty úložiště používané při Site Recovery přes skupiny prostředků v rámci hello stejné předplatné, nebo v různých předplatných.


### <a name="prepare-hello-hyper-v-hosts"></a>Příprava hello hostitelů Hyper-V

* Ujistěte se, že hello technologie Hyper-V hostitelé splňují hello [požadavky](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager).
- Ujistěte se, že hello hostitelé mají přístup k hello požadované adresy URL.


## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na **Nový** > **Monitorování a správa** > **Backup a Site Recovery (OMS)**.  

    ![Nový trezor](./media/site-recovery-hyper-v-site-to-azure/new-vault.png)

3. V **název**, zadejte popisný název tooidentify hello trezoru. Máte-li více předplatných, vyberte jedno z nich.

4. [Vytvořit novou skupinu prostředků](../azure-resource-manager/resource-group-template-deploy-portal.md) nebo vyberte existující šablonu a zadejte oblast Azure. Počítače budou replikované toothis oblast. toocheck podporované oblasti, najdete v části geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).

5. Pokud chcete, aby tooquickly přístup hello trezoru z hello řídicí panel, klikněte na tlačítko **Pin toodashboard**a potom klikněte na **vytvořit**.

    ![Nový trezor](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Hello nový trezor se zobrazí v hello **řídicí panel** > **všechny prostředky** seznamu a na hello hlavní **trezory služeb zotavení** okno.



## <a name="select-hello-protection-goal"></a>Vyberte cíl ochrany hello

Vyberte, co chcete tooreplicate, a místo, kam chcete tooreplicate k.

1. V hello **trezory služeb zotavení**vyberte trezor hello.
2. V části **Začínáme** klikněte na **Site Recovery** > **Připravit infrastrukturu** > **Cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)
3. V **cíl ochrany**, vyberte **tooAzure**a vyberte **Ano, s technologií Hyper-V**. Vyberte **ne** tooconfirm nepoužíváte VMM. Pak klikněte na **OK**.

    ![Zvolte cíle.](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí

Nastavení lokality hello technologie Hyper-V, nainstalujte hello zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure hello na hostitele Hyper-V a registraci hello lokality v trezoru hello.

1. V **Příprava infrastruktury**, klikněte na tlačítko **zdroj**. Klikněte na tlačítko tooadd nové lokality Hyper-V jako kontejner pro hostitele Hyper-V nebo clusterů, **serveru technologie Hyper-V +**.

    ![Nastavení zdroje](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)
2. V **web vytvořit Hyper-V**, zadejte název lokality hello. Pak klikněte na **OK**. Nyní, vyberte lokalitu hello jste vytvořili a klikněte na tlačítko **+ technologie Hyper-V Server** tooadd toohello serveru lokality.

    ![Nastavení zdroje](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. V **přidat Server** > **typ serveru**, zkontrolujte, zda **technologie Hyper-V server** se zobrazí.

    - Zajistěte, aby tento server hello technologie Hyper-V chcete tooadd odpovídá hello [požadavky](#on-premises-prerequisites), a je možné tooaccess hello zadaných adres URL.
    - Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery hello. Spusťte tento soubor tooinstall hello zprostředkovatele a hello agenta služeb zotavení na každém hostiteli technologie Hyper-V.

    ![Nastavení zdroje](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)


## <a name="install-hello-provider-and-agent"></a>Instalace hello zprostředkovatel a agent

1. Spusťte instalační soubor zprostředkovatele hello na každém hostiteli jste přidali lokality toohello technologie Hyper-V. Pokud instalujete na clusteru s podporou technologie Hyper-V, spusťte instalační program na každém uzlu clusteru. Instalace a registrace každého uzlu clusteru technologie Hyper-V zajistí, že jsou chráněné virtuální počítače, i v případě, že migraci mezi uzly.
2. V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
3. V **instalace**, přijmout nebo úpravě hello výchozí umístění instalace zprostředkovatele a klikněte na tlačítko **nainstalovat**.
4. V **nastavení trezoru**, klikněte na tlačítko **Procházet** tooselect hello trezoru klíčů soubor, který jste stáhli. Zadejte předplatné Azure Site Recovery hello, hello název trezoru, a patří hello technologie Hyper-V serveru toowhich hello technologie Hyper-V.

    ![Registrace serveru](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. V **nastavení proxy serveru**, zadejte, jak hello hello zprostředkovatel, který běží na hostitelích technologie Hyper-V připojí tooAzure Site Recovery přes internet.

    * Pokud chcete hello zprostředkovatele tooconnect přímo vyberte **připojit přímo bez proxy tooAzure Site Recovery**.
    * Pokud váš stávající proxy server vyžaduje ověření, nebo chcete použít pro připojení poskytovatele hello toouse vlastní proxy server, vyberte **připojit tooAzure Site Recovery pomocí proxy serveru**.
    * Pokud používáte proxy server:
        - Zadejte hello adresu, port a přihlašovací údaje
        - Ujistěte se, zda text hello adresy URL popisované v hello [požadavky](#prerequisites) jsou povoleny přes proxy server hello.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. Po dokončení instalace, klikněte na tlačítko **zaregistrovat** tooregister hello server v trezoru hello.

    ![Umístění instalace](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. Po dokončení registrace pomocí Azure Site Recovery je načíst metadata ze serveru hello technologie Hyper-V a hello server se zobrazí v **infrastruktura Site Recovery** > **hostitelů Hyper-V**.


## <a name="set-up-hello-target-environment"></a>Nastavení cílového prostředí hello

Zadejte hello účtu úložiště Azure pro replikaci a hello toowhich síť Azure virtuální počítače Azure připojí po převzetí služeb při selhání.

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**.
2. Vyberte hello předplatné a skupina prostředků hello, ve kterém chcete toocreate hello virtuální počítače Azure po převzetí služeb při selhání. Zvolte hello nasazení modelu, že chcete toouse v Azure (klasický nebo prostředek management) pro hello virtuálních počítačů.

3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

    - Pokud nemáte účet úložiště, klikněte na tlačítko **+ úložiště** toocreate vložené založené na správci prostředků účtu. Přečtěte si informace o [požadavky na úložiště](site-recovery-prereq.md#azure-requirements).
    - Pokud nemáte síť Azure, klikněte na tlačítko **+ síť** toocreate vložené sítě pomocí Správce prostředků.

    ![Úložiště](./media/site-recovery-vmware-to-azure/enable-rep3.png)




## <a name="configure-replication-settings"></a>Konfigurace nastavení replikace

1. Klikněte na tlačítko toocreate novou zásadu replikace, **připravit infrastrukturu** > **nastavení replikace** > **+ vytvořit a přidružit**.

    ![Síť](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady.
3. V **frekvence kopírování**, určete, jak často má tooreplicate rozdílová data po hello počáteční replikaci (každých 30 sekund, 5 nebo 15 minut).

    > [!NOTE]
    > 30 druhý frekvence nepodporuje při replikaci toopremium úložiště. omezení Hello je určen podle hello počet snímků za blob (100) nepodporuje storage úrovně premium. [Další informace](../storage/storage-premium-storage.md#snapshots-and-copy-blob).

4. V **uchování bodu obnovení**, zadejte v hodinách, jak dlouho hello intervalem pro uchovávání dat je pro každý bod obnovení. Virtuální počítače mohou být obnovena tooany bodu v rámci časového období.
5. V **frekvence snímkování konzistentní aplikace vzhledem**, zadejte, jak často (1 – 12 hodin) body obnovení obsahující snímky konzistentní s aplikacemi jsou vytvořeny.

    - Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače hello a snímky konzistentní s aplikací, která přebírá v okamžiku snímek dat aplikací hello uvnitř hello virtuálního počítače.
    - Snímky konzistentní s aplikacemi pomocí tooensure služby Stínová kopie svazku (VSS), které aplikace budou při pořízení snímku hello v konzistentním stavu.
    - Pokud povolíte snímky konzistentní s aplikacemi, ovlivní hello výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byl hello hodnoty, které nastavíte, menší než počet hello další body obnovení, které nakonfigurujete.

6. V **čas spuštění počáteční replikace**, zadejte, kdy toostart hello počáteční replikace. Hello replikace se provede přes pásma vašeho internetového připojení, můžete chtít tooschedule ho dobu mimo špičky. Pak klikněte na **OK**.

    ![Zásady replikace](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Když vytvoříte novou zásadu, je automaticky přidružená hello web Hyper-V. Budete moct přidružit k víc zásad replikace v lokalitě Hyper-V (a hello virtuálních počítačů v ní) **replikace** > název zásady > **přidružit web Hyper-V**.

## <a name="capacity-planning"></a>Plánování kapacity

Teď, když máte infrastruktury základní nastavení, vezměte v úvahu plánování kapacity a zjistit, jestli nepotřebujete další prostředky.

Site Recovery nabízí Plánovač toohelp kapacity přidělit správné prostředky hello pro výpočty, síť a úložiště. Můžete spustit hello planner v rychlém režimu pro odhady založené na průměrném počtu virtuálních počítačů, disků a úložiště, nebo v podrobném režimu se vlastní čísla na úrovni pracovního vytížení hello. Před zahájením potřebujete:

* Získat informace o prostředí replikace, včetně virtuální počítačů, disků na virtuální počítač a úložišti na disk.
* Odhad hello denní míry změn s ohledem pro replikovaná data. Můžete použít hello [Capacity Planner pro repliku technologie Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) toohelp to uděláte.

1. Klikněte na tlačítko **Stáhnout** toodownload hello nástroje a potom ho spusťte. [Přečíst článek hello](site-recovery-capacity-planner.md) doprovodný nástroj hello.
2. Až budete hotoví, vyberte **Ano** v **jste spustili hello Capacity Planner**?

   ![Plánování kapacity](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

Přečtěte si další informace o [řízení šířky pásma sítě](#network-bandwidth-considerations)



## <a name="enable-replication"></a>Povolení replikace

Než začnete, zajistěte, aby účtu uživatele Azure hello požadované [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci nové tooAzure virtuálního počítače.

Povolení replikace pro virtuální počítače takto:          

1. Klikněte na tlačítko **replikujte aplikaci** > **zdroj**. Poté, co jste nastavili replikaci hello poprvé, můžete kliknout na **+ replikovat** tooenable replikaci pro další počítače.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)
2. V **zdroj**, vyberte lokality hello technologie Hyper-V. Pak klikněte na **OK**.
3. V **cíl**, vyberte předplatné trezoru hello a hello převzetí služeb při selhání modelu chcete toouse v Azure (klasický nebo prostředek management), po převzetí služeb při selhání.
4. Vyberte účet úložiště hello, že chcete toouse. Pokud nemáte účet, který chcete toouse, můžete [vytvořit](#set-up-an-azure-storage-account). Pak klikněte na **OK**.
5. Vyberte hello Azure toowhich síť a podsíť virtuálních počítačů Azure se připojí, když vytváří převzetí služeb při selhání.

    - Vyberte tooapply hello nastavení tooall počítače sítě můžete povolit pro replikaci, **nakonfigurovat pro vybrané počítače**.
    - Vyberte **nakonfigurovat později** tooselect hello síť Azure pro konkrétní počítač.
    - Pokud nemáte síť chcete toouse, můžete [vytvořit](#set-up-an-azure-network). V odpovídajícím případě vyberte podsíť. Pak klikněte na **OK**.

   ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. V **virtuální počítače** > **vybrat virtuální počítače**, klikněte na tlačítko a vyberte každý počítač má tooreplicate. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication5-for-exclude-disk.png)

7. V **vlastnosti** > **konfigurovat vlastnosti**, vyberte hello operační systém pro virtuální počítače hello vybrané a hello disk operačního systému.
8. Ověřte, že hello název virtuálního počítače Azure (název cílové) v souladu s [požadavky na virtuální počítač Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
9. Ve výchozím nastavení jsou vybrány všechny disky hello hello virtuálních počítačů pro replikaci, je však možné vyjmout tooexclude disků je.
    - Můžete chtít tooexclude disky tooreduce replikace šířky pásma. Například chcete nemusí tooreplicate disků se dočasná data nebo data, která se má aktualizovat pokaždé, když počítače nebo aplikace se restartuje (například pagefile.sys nebo databázi tempdb systému Microsoft SQL Server). Hello disku můžete z replikace vyloučit unselecting hello disk.
    - Vyloučit můžete jenom základní disky. Disky operačního systému vyloučit nelze.
    - Doporučujeme, abyste nevylučovali dynamické disky. Site Recovery nemůže zjistit, jestli je virtuální pevný disk ve virtuálním počítači hosta základní nebo dynamický. Pokud nejsou všechny disky, závislé dynamický svazek vyloučené, hello chráněné dynamický disk se zobrazí jako poškozený disk při hello virtuálních počítačů při selhání a hello data na tomto disku nebudou přístupné.
        - Po povolení replikace už není možné přidávat nebo odebírat disky pro replikaci. Chcete-li tooadd nebo vyloučit disk, potřebujete toodisable ochranu pro hello virtuálního počítače a pak ji znovu povolte.
        - Disky, které ručně vytvoříte v Azure, nebude možné po navrácení služeb obnovit. Například pokud selhání tři disky a vytvořte dvě přímo ve virtuálním počítači Azure, pouze disky tři hello, které byly převzetí služeb při selhání se nepodařilo zpět z Azure tooHyper-V. Nesmí obsahovat disky ručně vytvořené v navrácení služeb po obnovení, nebo v zpětná replikace z tooAzure technologie Hyper-V.
        - Pokud vyloučíte disk, který je potřeba pro toooperate aplikaci, po převzetí služeb při selhání tooAzure musíte toocreate ručně v Azure, takže tento hello replikované aplikace, mohou spouštět. Alternativně může integrovat Azure automation do plánu obnovení disku hello toocreate během převzetí služeb při selhání hello počítače.

10. Klikněte na tlačítko **OK** toosave změny. Později můžete nastavit další vlastnosti.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication6-with-exclude-disk.png)

11. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, vyberte zásadu replikace hello chcete tooapply hello chráněné virtuální počítače. Pak klikněte na **OK**. Můžete změnit zásady replikace hello v **zásady replikace** > název zásady > **upravit nastavení**. Změny, které použijete, se použijí pro počítače, které už replikujete, a nové počítače.


   ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Můžete sledovat průběh hello **povolení ochrany** úlohy v **úlohy** > **úlohy Site Recovery**. Po hello **dokončení ochrany** úloha spustí počítač hello je připraven na převzetí služeb při selhání.

### <a name="view-and-manage-vm-properties"></a>Zobrazení a správa vlastností virtuálního počítače

Doporučujeme ověřit vlastnosti hello hello zdrojového počítače.

1. V **chráněné položky**, klikněte na tlačítko **replikované položky**a vyberte hello počítač.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)
2. V **vlastnosti**, můžete zobrazit replikace a informace o převzetí služeb při selhání pro hello virtuálních počítačů.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)
3. V **výpočty a síť** > **výpočetní vlastnosti**, můžete zadat název a cílovou velikost virtuálního počítače Azure hello. Upravte název toocomply hello s požadavky na Azure, pokud potřebujete. Můžete také zobrazit a upravit informace o hello Cílová síť, podsíť a IP adresu, která bude přiřazena toohello virtuálního počítače Azure. Vezměte na vědomí následující hello:

   * Můžete nastavit hello cílová IP adresa. Pokud adresu nezadáte, použije hello převzal počítač DHCP. Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, hello převzetí služeb při selhání se nezdaří. Dobrý den, které lze použít stejnou cílovou IP adresu pro testovací převzetí služeb při selhání, pokud není k dispozici v hello testovací převzetí služeb při selhání sítě hello adresa.
   * Hello počet síťových adaptérů závisí hello velikost, který jste zadali pro hello cílový virtuální počítač následujícím způsobem:

     * Pokud hello počet síťových adaptérů na zdrojovém počítači hello je menší než nebo rovna toohello počet adaptérů povoleno pro velikost cílového počítače hello pak bude mít cíl hello hello stejný počet adaptérů jako zdroj hello.
     * Pokud hello počet adaptérů pro hello zdrojový virtuální počítač překračuje počet hello povolený pro cílovou velikost hello pak maximální velikost cíle hello se použije.
     * Například pokud má zdrojový počítač dva síťové adaptéry a velikost hello cílového počítače podporuje čtyři, bude mít hello cílový počítač dva adaptéry. Pokud má hello zdrojový počítač dva adaptéry, ale hello podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač hello jenom jeden adaptér.     
     * Pokud hello virtuální počítač má několik síťových adaptérů připojí se všechny toohello stejné síti.
     * Pokud hello virtuální počítač více síťových adaptérů pak hello jeden uvedené v seznamu hello se stal hello *výchozí* síťový adaptér v hello virtuální počítač Azure.

     ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

4. V **disky**, uvidíte hello operačního systému a datové disky na hello virtuální počítač, který bude replikován.

#### <a name="managed-disks"></a>Managed Disks

V **výpočty a síť** > **výpočetní vlastnosti**, pokud chcete počítač tooyour tooattach spravovaných disků na tooAzure migrace můžete nastavit "Použití spravovaných disků" nastavení příliš "Ano" hello virtuálních počítačů. Spravované disky zjednodušuje Správa disku pro virtuální počítače Azure IaaS pomocí správy hello účty úložiště přidružené ke hello disky virtuálních počítačů. [Přečtěte si další informace o spravovaných discích](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Spravované disky jsou vytvořené a připojené toohello virtuální počítač jenom na tooAzure převzetí služeb při selhání. K povolení ochrany, data z místního počítače bude tooreplicate toostorage účty.
   Spravované disky se dají vytvořit jenom pro virtuální počítače nasazené pomocí modelu nasazení Resource manager hello.

  > [!NOTE]
  > Navrácení služeb po obnovení z Azure tooon místního prostředí Hyper-V se aktuálně nepodporuje pro počítače s spravované disky. Nastaví "Použití spravovaných disků" příliš "Ano" pouze v případě, že chcete toomigrate tooAzure tento počítač.

   - Při nastavení "Použití spravovaných disků" příliš "Ano", pouze skupiny dostupnosti ve skupině prostředků hello sadou "Použití spravovaných disků" příliš "Ano" by být k dispozici pro výběr. Je to proto, že virtuální počítače s spravované disky lze pouze součástí skupiny dostupnosti s "Použití spravovaných disků" vlastností nastavenou příliš "Ano". Zajistěte, aby vytváření skupiny dostupnosti s nastavenou vlastnost "Použití spravovaných disků" založené na discích záměrné toouse spravované na převzetí služeb při selhání. Podobně když nastavíte "Použití spravovaných disků" příliš "žádný", pouze skupiny dostupnosti ve skupině prostředků hello s "Použití spravovaných disků" vlastností nastavenou příliš "žádný" by být k dispozici pro výběr. [Přečtěte si další informace o spravovaných discích a skupinách dostupnosti](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Pokud účet úložiště hello používané pro replikaci byla zašifrovaná pomocí šifrování služby úložiště v libovolném bodě v čase, vytvoření spravovaných disků během převzetí služeb při selhání se nezdaří. Můžete buď nastavit "Použití spravovaných disků" příliš "žádný" a zkuste zopakovat převzetí služeb při selhání nebo zakažte ochranu pro virtuální počítač hello a chránit tooa účet úložiště, který nemá povolené v libovolném bodě v čase šifrování služby úložiště.
  > [Další informace o šifrování služby Storage a o spravovaných discích](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption)


## <a name="test-hello-deployment"></a>Testovací nasazení pro hello

tootest hello nasazení můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo plán obnovení, který obsahuje jeden nebo více virtuálních počítačů.

### <a name="before-you-start"></a>Než začnete

 - Pokud chcete, aby tooconnect tooAzure virtuálních počítačů pomocí protokolu RDP po převzetí služeb při selhání, další informace o [Příprava tooconnect](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - test toofully potřebujete toocopy služby Active Directory a DNS v testovacím prostředí. [Další informace](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

1. toofail přes jeden počítač, v **replikované položky**, klikněte na tlačítko hello virtuálního počítače > **+ testovací převzetí služeb při selhání** ikonu.
2. plánování toofail přes obnovení, **plány obnovení**, klikněte pravým tlačítkem na hello plán > **testovací převzetí služeb při selhání**. plán obnovení toocreate [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).
3. V **testovací převzetí služeb při selhání**, vyberte síť Azure toowhich hello virtuální počítače Azure připojí po převzetí služeb při selhání.
4. Klikněte na tlačítko **OK** toobegin hello převzetí služeb při selhání. Průběh můžete sledovat, když kliknete na virtuální počítač tooopen hello jeho vlastnosti, nebo na hello **testovací převzetí služeb při selhání** úlohy v název trezoru > **úlohy** > **úlohy Site Recovery**.
5. Po dokončení převzetí služeb při selhání hello, měli byste také mít možnost toosee hello repliky počítač Azure se zobrazí v hello portálu Azure > **virtuální počítače**. Měli byste si ověřit, že hello virtuálního počítače je hello odpovídající velikost, byl připojený toohello příslušnou síť, a zda je spuštěna.
6. Pokud jste připravili připojení po převzetí služeb při selhání, musí být schopný tooconnect toohello virtuálního počítače Azure.
7. Jakmile jste hotovi, klikněte na **vyčistit testovací převzetí služeb při selhání** v plánu obnovení hello. V **poznámky** zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání. Tato akce odstraní hello virtuálních počítačů, které byly vytvořeny během testovacího převzetí služeb při selhání.

Další informace najdete v hello [testovací převzetí služeb při selhání tooAzure](site-recovery-test-failover-to-azure.md) článku.



## <a name="monitor-hello-deployment"></a>Monitorování nasazení hello

Nastavení konfigurace hello monitorování, stav a stav pro vaše nasazení Site Recovery:

1. Klikněte na hello tooaccess název trezoru hello **Essentials** řídicího panelu. V tomto řídicím panelu můžete sledovat úlohy Site Recovery, stav replikace, plány obnovení, stav serveru a události.  

    ![Základy](./media/site-recovery-hyper-v-site-to-azure/essentials.png)
2. V hello **stavu** dlaždice můžete monitorovat servery lokality, které se jedná o problém a hello události vyvolané službou Site Recovery v hello posledních 24 hodin. Můžete přizpůsobit Essentials tooshow hello dlaždice a rozložení, které jsou velmi užitečné tooyou, včetně stavu hello jiných Site Recovery a trezory Backup.
3. Můžete spravovat a sledovat replikace v hello **replikované položky**, **plány obnovení**, a **úlohy Site Recovery** dlaždice. Můžete zobrazit další podrobnosti o úlohách další podrobnosti v **úlohy** > **úlohy Site Recovery**.

## <a name="command-line-provider-and-agent-installation"></a>Zprostředkovatel a agent instalace z příkazového řádku

Hello zprostředkovatele Azure Site Recovery a agenta můžete nainstalovat i pomocí hello následující příkazový řádek. Tato metoda může být použité tooinstall hello zprostředkovatele na serveru jádra pro Windows Server 2012 R2.

1. Hello zprostředkovatele instalace souboru a registraci klíče tooa složka pro stahování. Například C:\ASR.
2. Z příkazového řádku se zvýšenými oprávněními spusťte instalační program tyto příkazy tooextract hello zprostředkovatele:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Spusťte tento příkaz tooinstall hello součásti:

            C:\ASR> setupdr.exe /i
4. Pak spusťte tyto příkazy tooregister hello server v trezoru hello:

            CD C:\Program Files\Microsoft Azure Site Recovery Provider\
            C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file>

<br/>
Kde:

* **/ Credentials**: povinný parametr, který určuje hello umístění, ve které hello se nachází soubor registračního klíče  
* **/ FriendlyName**: povinný parametr pro hello název hostitele serveru hello technologie Hyper-V, který se zobrazí na portálu Azure Site Recovery hello.
* **/ proxyaddress**: volitelný parametr, který určuje adresu hello hello proxy serveru.
* **/ proxyport** : volitelný parametr, který určuje port hello hello proxy serveru.
* **/ proxyusername**: volitelný parametr, který určuje uživatelské jméno Proxy hello (pokud proxy server vyžaduje ověření).
* **/ proxypassword**: volitelný parametr, který určuje hello heslo pro ověřování pomocí serveru proxy hello (pokud proxy server vyžaduje ověření).


## <a name="network-bandwidth-considerations"></a>Aspekty šířky pásma sítě
Můžete použít hello [nástroje Plánovač kapacity technologie Hyper-V](site-recovery-capacity-planner.md) toocalculate hello šířku pásma potřebujete pro replikaci (počáteční replikace a pak rozdílové). velikost hello toocontrol šířky pásma využívané pro replikaci máte několik možností:

* **Omezení šířky pásma**: přenos Hyper-V, která se replikují tooAzure prochází konkrétního hostitele technologie Hyper-V. Můžete omezit šířku pásma na hostitelském serveru hello.
* **Optimalizace šířky pásma**: můžete ovlivnit hello pásma sítě používané pro replikaci pomocí několika klíčů registru.

### <a name="throttle-bandwidth"></a>Omezení šířky pásma
1. Otevřete hello Microsoft Azure Backup konzoly MMC modul snap-in na serveru hostitele technologie Hyper-V hello. Ve výchozím nastavení je k dispozici na ploše hello nebo v C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in hello klikněte na **změnit vlastnosti**.
3. Na hello **omezování** vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení**a nastavte hello limity pro pracovní a nepracovní dobu. Platné rozsahy jsou 512 kB/s too102 MB/s za sekundu.

    ![Omezení šířky pásma](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Můžete taky hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) rutiny tooset omezení. Tady je ukázkový skript:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** označuje, že není požadováno žádné omezování.

### <a name="influence-network-bandwidth"></a>Ovlivnění šířky pásma sítě
1. V registru hello přejděte příliš**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * tooinfluence hello šířky pásma provoz na replikaci disku, upravte hodnotu hello hello **UploadThreadsPerVM**, nebo vytvořte klíč hello, pokud neexistuje.
   * tooinfluence hello šířky pásma pro přenosy navrácení služeb po obnovení z Azure, upravte hodnotu hello **DownloadThreadsPerVM**.
2. Hello výchozí hodnota je 4. V síti "nadměrným zřízením" tyto klíče registru by mělo být změněno z hello výchozí hodnoty. maximální Hello je 32. Monitorování přenosů toooptimize hello hodnotu.

## <a name="next-steps"></a>Další kroky

Po dokončení počáteční replikace a otestujete hello nasazení, můžete podle potřeby hello vyvolat převzetí služeb při selhání. [Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak toorun je.
