---
title: "Replikace virtuálních počítačů technologie Hyper-V do Azure | Microsoft Docs"
description: "Popisuje, jak k orchestraci replikace, převzetí služeb při selhání a obnovení místní virtuální počítače Hyper-V do Azure"
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
ms.openlocfilehash: 8a2ea92759a777b1178fbfe8084a97eec931f709
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Replikace virtuálních počítačů s Hyper-V (bez VMM) do Azure pomocí Azure Site Recovery a webu Azure Portal

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Azure Classic](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

Tento článek popisuje, jak replikovat místní technologie Hyper-V virtuální počítače Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) na portálu Azure. Ve virtuálních počítačích této nasazení technologie Hyper-V nejsou spravovány nástrojem System Center Virtual Machine Manager (VMM).

Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Pokud chcete migrovat počítače do Azure (bez navrácení služeb po obnovení), najdete další informace v [tomto článku](site-recovery-migrate-to-azure.md).



## <a name="deployment-steps"></a>Kroky nasazení

Podle informací uvedených v tomto článku dokončete tento postup nasazení:

1. [Přečtěte si další informace](site-recovery-components.md#hyper-v-to-azure) o architektuře pro toto nasazení. Dále si [můžete přečíst](site-recovery-hyper-v-azure-architecture.md), jak ve službě Site Recovery funguje replikace Hyper-V.
2. Ověřte požadavky a omezení.
3. Nastavte síť Azure a účty úložiště.
4. Příprava hostitele Hyper-V.
5. Vytvořte trezor služby Recovery Services. Trezor obsahuje konfigurační nastavení a orchestruje replikaci.
6. Zadejte nastavení zdroje. Vytvořit web technologie Hyper-V, který obsahuje hostitelů Hyper-V a zaregistrujte lokality v trezoru. Nainstalujte zprostředkovatele Azure Site Recovery a agenta služeb zotavení Microsoft na hostitelích technologie Hyper-V.
7. Nastavte cíl a replikaci.
8. Povolte replikaci pro virtuální počítače.
9. Otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje, jak má.



## <a name="prerequisites"></a>Požadavky


**Požadavek** | **Podrobnosti** |
--- | --- |
**Azure** | Přečtěte si o [požadavcích na Azure](site-recovery-prereq.md#azure-requirements).
**Místní servery** | [Další informace](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager) o požadavcích pro hostitele Hyper-V na místě.
**Místní virtuální počítače Hyper-V** | Na virtuálních počítačích, které chcete replikovat, musí běžet [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) a počítače musí splňovat [požadavky na Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Adresy URL Azure** | Hostitelé Hyper-V potřebují přístup k tyto adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte zavedená pravidla brány firewall založená na IP adrese, zkontrolujte, že tato pravidla umožňují komunikaci s Azure.<br/></br> Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).



## <a name="prepare-for-deployment"></a>Příprava nasazení

Při přípravě nasazení musíte:

1. [Nastavit síť Azure](#set-up-an-azure-network) ve kterém virtuální počítače Azure budou umístěné když jste vytvořili po převzetí služeb při selhání.
2. [Nastavit účet úložiště Azure](#set-up-an-azure-storage-account) pro replikovaná data.
3. [Příprava hostitele Hyper-V](#prepare-the-hyper-v-hosts) zajistit získají přístup k požadované adresy URL.

### <a name="set-up-an-azure-network"></a>Nastavení sítě Azure

Nastavte síť Azure. To budete potřebovat, aby virtuální počítače Azure vytvořené po převzetí služeb při selhání jsou připojené k síti.

* Síť by měla být ve stejném umístění jako trezor služby Recovery Services.
* V závislosti na modelu prostředků, který chcete použít pro převzal virtuálních počítačích Azure, které nastavíte síť Azure v [režimu Resource Manager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md), nebo [klasickém režimu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* Doporučujeme nastavit síť ještě před tím, než začnete. Pokud to neuděláte, budete to muset udělat při nasazení služby Site Recovery.
- Účty úložiště používané službou Site Recovery nemůže být [přesunout](../azure-resource-manager/resource-group-move-resources.md) v rámci stejné, nebo v různých, předplatných.


### <a name="set-up-an-azure-storage-account"></a>Nastavení účtu úložiště Azure

- Je nutné standard nebo premium účtu úložiště Azure k ukládání dat replikovaných do Azure. [Storage úrovně premium](../storage/storage-premium-storage.md) se obvykle používá u virtuálních počítačů, které je třeba neustále vysoké vstupně-výstupní výkon a nízká latence pro zatížení s intenzivním vstupně-výstupní operace hostitele.
- Pokud pro ukládání replikovaných dat chcete používat účet Storage úrovně Premium, musíte mít také účet Storage úrovně Standard pro ukládání protokolů replikace, do kterých se zaznamenávají průběžné změny místních dat.
- V závislosti na modelu prostředků, který chcete použít pro převzal virtuálních počítačích Azure, které nastavíte účet v [režimu Resource Manager](../storage/storage-create-storage-account.md), nebo [klasickém režimu](../storage/storage-create-storage-account-classic-portal.md).
- Doporučujeme, abyste před zahájením nastavení účtu úložiště. Pokud to neuděláte, budete muset udělat při nasazování Site Recovery. Tyto účty musí být ve stejné oblasti jako trezor služeb zotavení.
- Nelze přesunout úložiště účtů používaných při obnovení lokality, mezi skupinami prostředků v rámci stejného předplatného, nebo v různých předplatných.


### <a name="prepare-the-hyper-v-hosts"></a>Příprava hostitele Hyper-V

* Ujistěte se, že splňují hostitelů Hyper-V [požadavky](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager).
- Ujistěte se, že hostitelé mají přístup k požadované adresy URL.


## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Klikněte na **Nový** > **Monitorování a správa** > **Backup a Site Recovery (OMS)**.  

    ![Nový trezor](./media/site-recovery-hyper-v-site-to-azure/new-vault.png)

3. Do pole **Název** zadejte popisný název pro identifikaci trezoru. Máte-li více předplatných, vyberte jedno z nich.

4. [Vytvořit novou skupinu prostředků](../azure-resource-manager/resource-group-template-deploy-portal.md) nebo vyberte existující šablonu a zadejte oblast Azure. Počítače budou replikovány do této oblasti. Pokud chcete zkontrolovat oblasti jsou podporované, najdete v části geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).

5. Pokud chcete rychle se dostat k trezoru z řídicího panelu, klikněte na tlačítko **připnout na řídicí panel**a potom klikněte na **vytvořit**.

    ![Nový trezor](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Nový trezor se zobrazí v **řídicí panel** > **všechny prostředky** seznamu a v hlavním **trezory služeb zotavení** okno.



## <a name="select-the-protection-goal"></a>Výběr cíle ochrany

Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.

1. V **trezory služeb zotavení**, vyberte trezor.
2. V části **Začínáme** klikněte na **Site Recovery** > **Připravit infrastrukturu** > **Cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)
3. V **cíl ochrany**, vyberte **do Azure**a vyberte **Ano, s technologií Hyper-V**. Vyberte **ne** potvrďte, že nepoužíváte VMM. Pak klikněte na **OK**.

    ![Zvolte cíle.](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nastavte web Hyper-V, nainstalujte zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure na hostitele Hyper-V a zaregistrujte lokality v trezoru.

1. V **Příprava infrastruktury**, klikněte na tlačítko **zdroj**. Chcete-li přidat nový web technologie Hyper-V jako kontejner pro hostitele Hyper-V nebo clusterů, klikněte na tlačítko **serveru technologie Hyper-V +**.

    ![Nastavení zdroje](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)
2. V **web vytvořit Hyper-V**, zadejte název pro tuto lokalitu. Pak klikněte na **OK**. Nyní, vyberte lokalitu, jste vytvořili a klikněte na tlačítko **+ technologie Hyper-V Server** přidání serveru do lokality.

    ![Nastavení zdroje](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. V **přidat Server** > **typ serveru**, zkontrolujte, zda **technologie Hyper-V server** se zobrazí.

    - Ujistěte se, že server Hyper-V, které chcete přidat v souladu se [požadavky](#on-premises-prerequisites)a bude schopen získat přístup k zadané adresy URL.
    - Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery. Je-li spustit tento soubor k instalaci zprostředkovatele a agenta služeb zotavení na každém hostiteli technologie Hyper-V.

    ![Nastavení zdroje](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)


## <a name="install-the-provider-and-agent"></a>Nainstaluje zprostředkovatele a agenta

1. Spusťte instalační soubor na každém hostiteli, které jste přidali do lokality Hyper-V zprostředkovatele. Pokud instalujete na clusteru s podporou technologie Hyper-V, spusťte instalační program na každém uzlu clusteru. Instalace a registrace každého uzlu clusteru technologie Hyper-V zajistí, že jsou chráněné virtuální počítače, i v případě, že migraci mezi uzly.
2. V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
3. V okně **Instalace** přijměte nebo změňte výchozí umístění instalace zprostředkovatele a klikněte na **Nainstalovat**.
4. V **nastavení trezoru**, klikněte na tlačítko **Procházet** a vyberte soubor klíče trezoru, který jste si stáhli. Zadejte předplatné Azure Site Recovery, název trezoru a Web Hyper-V, do které patří server technologie Hyper-V.

    ![Registrace serveru](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. V **nastavení proxy serveru**, určete, jak se zprostředkovatel, který běží na hostitelích technologie Hyper-V připojuje k Azure Site Recovery přes internet.

    * Pokud chcete, aby se zprostředkovatel připojil přímo, vyberte **Připojit k Azure Site Recovery přímo bez proxy serveru**.
    * Pokud váš stávající proxy server vyžaduje ověření, nebo pokud chcete používat vlastní proxy server pro připojení poskytovatele, vyberte **připojit k Azure Site Recovery pomocí proxy serveru**.
    * Pokud používáte proxy server:
        - Zadejte adresu, port a přihlašovací údaje
        - Zajistěte, aby k adresám URL popsaným v [požadavky](#prerequisites) jsou povoleny prostřednictvím proxy serveru.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. Po dokončení instalace, klikněte na tlačítko **zaregistrovat** zaregistrujte server v trezoru.

    ![Umístění instalace](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. Po dokončení registrace pomocí Azure Site Recovery je načíst metadata ze serveru technologie Hyper-V a server se zobrazí v **infrastruktura Site Recovery** > **hostitelů Hyper-V**.


## <a name="set-up-the-target-environment"></a>Nastavení cílového prostředí

Zadejte účet úložiště Azure pro replikaci a síť Azure, ke které se virtuální počítače Azure připojí po převzetí služeb při selhání.

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**.
2. Vyberte předplatné a skupině prostředků, ve kterém chcete vytvořit virtuální počítače Azure po převzetí služeb při selhání. Vyberte model nasazení, který chcete použít pro virtuální počítače v Azure (klasický nebo prostředek management).

3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

    - Pokud nemáte účet úložiště, klikněte na tlačítko **+ úložiště** vytvořit vložený založené na správci prostředků účtu. Přečtěte si informace o [požadavky na úložiště](site-recovery-prereq.md#azure-requirements).
    - Pokud nemáte síť Azure, klikněte na tlačítko **+ síť** vytvořit vložený sítě pomocí Správce prostředků.

    ![Úložiště](./media/site-recovery-vmware-to-azure/enable-rep3.png)




## <a name="configure-replication-settings"></a>Konfigurace nastavení replikace

1. Pokud chcete vytvořit novou zásadu replikace, klikněte na **Připravit infrastrukturu** > **Nastavení replikace** > **+Vytvořit a přidružit**.

    ![Síť](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady.
3. V části **Frekvence kopírování** určete, jak často chcete replikovat rozdílová data po počáteční replikaci (každých 30 sekund, 5 minut nebo 15 minut).

    > [!NOTE]
    > Frekvence každých 30 sekund se nepodporuje při replikaci do Storage úrovně Premium. Omezení je určeno počtem snímků na jeden objekt blob (100), který Storage úrovně Premium podporuje. [Další informace](../storage/storage-premium-storage.md#snapshots-and-copy-blob).

4. V **uchování bodu obnovení**, zadejte v hodinách, jak dlouho je okno uchování pro každý bod obnovení. Virtuální počítače můžete obnovit do libovolného bodu v rámci časového období.
5. V **frekvence snímkování konzistentní aplikace vzhledem**, zadejte, jak často (1 – 12 hodin) body obnovení obsahující snímky konzistentní s aplikacemi jsou vytvořeny.

    - Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače, a snímek konzistentní vzhledem k aplikacím, který vytvoří snímek dat aplikací ve virtuálním počítači v daném okamžiku.
    - Snímky konzistentní vzhledem k aplikacím využívají službu Stínová kopie svazku (VSS), aby bylo zajištěno, že aplikace budou při pořízení snímku v konzistentním stavu.
    - Pokud povolíte snímky konzistentní s aplikacemi, bude mít vliv výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byla hodnota, kterou nastavíte, menší než počet dalších bodů obnovení, které nakonfigurujete.

6. V **čas spuštění počáteční replikace**, určete, kdy má spustit počáteční replikaci. Při replikaci se využívá šířka pásma vašeho internetového připojení, může být proto vhodné naplánovat ji na dobu mimo špičky. Pak klikněte na **OK**.

    ![Zásady replikace](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Když vytvoříte novou zásadu, je automaticky spojen s web Hyper-V. Je možné přidružit web Hyper-V (a virtuální počítače v něm) víc zásad replikace v **replikace** > název zásady > **přidružit web Hyper-V**.

## <a name="capacity-planning"></a>Plánování kapacity

Teď, když máte infrastruktury základní nastavení, vezměte v úvahu plánování kapacity a zjistit, jestli nepotřebujete další prostředky.

Site Recovery nabízí Plánovač kapacity, který vám pomůže přidělit správné prostředky pro výpočty, síť a úložiště. Můžete spustit Plánovač v rychlém režimu pro odhady založené na průměrném počtu virtuálních počítačů, disků a úložiště, nebo v podrobném režimu se vlastní čísla na úrovni pracovního vytížení. Před zahájením potřebujete:

* Získat informace o prostředí replikace, včetně virtuální počítačů, disků na virtuální počítač a úložišti na disk.
* Odhad denní míry změn s ohledem pro replikovaná data. Můžete použít [Capacity Planner pro repliku technologie Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) k tomuto účelu.

1. Klikněte na tlačítko **Stáhnout** ke stažení nástroje a potom ho spusťte. [Přečtěte si doprovodný článek](site-recovery-capacity-planner.md) k nástroji.
2. Po dokončení vyberte pro otázku **Have you run the Capacity Planner** (Spustili jste plánovač kapacity) odpověď **Yes** (Ano).

   ![Plánování kapacity](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

Přečtěte si další informace o [řízení šířky pásma sítě](#network-bandwidth-considerations)



## <a name="enable-replication"></a>Povolení replikace

Než začnete, zkontrolujte, že váš uživatelský účet Azure má požadovaná [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) k replikaci nového virtuálního počítače do Azure.

Povolení replikace pro virtuální počítače takto:          

1. Klikněte na tlačítko **replikujte aplikaci** > **zdroj**. Poté, co jste nastavili replikaci poprvé, můžete kliknout na **+ replikovat** k povolení replikace pro další počítače.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)
2. V **zdroj**, vyberte lokalitu, technologie Hyper-V. Pak klikněte na **OK**.
3. V **cíl**, vyberte předplatné trezoru a modelu převzetí služeb při selhání můžete používat v Azure (klasický nebo prostředek management), po převzetí služeb při selhání.
4. Vyberte účet úložiště, který chcete použít. Pokud nemáte účet, který chcete použít, můžete [vytvořit](#set-up-an-azure-storage-account). Pak klikněte na **OK**.
5. Vyberte síť Azure a podsíť, do kterého virtuální počítače Azure připojí, když vytváří převzetí služeb při selhání.

    - Chcete-li použít nastavení sítě pro všechny počítače povolíte pro replikaci, vyberte **nakonfigurovat pro vybrané počítače**.
    - Vyberte **Nakonfigurovat později** a vyberte síť Azure pro konkrétní počítač.
    - Pokud nemáte síť, kterou chcete použít, můžete [vytvořit](#set-up-an-azure-network). V odpovídajícím případě vyberte podsíť. Pak klikněte na **OK**.

   ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. V nastavení **Virtuální počítače** > **Výběr virtuálních počítačů** klikněte a vyberte každý počítač, který chcete replikovat. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication5-for-exclude-disk.png)

7. V nastavení **Vlastnosti** > **Konfigurace vlastností** vyberte operační systém pro vybrané virtuální počítače a disk operačního systému.
8. Ověřte, že název virtuálního počítače Azure (název cíle) splňuje [požadavky na virtuální počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
9. Ve výchozím nastavení jsou pro replikaci vybrány všechny disky virtuálního počítače, ale zrušením zaškrtnutí můžete požadované disky z replikace vyloučit.
    - Disky můžete vyloučit třeba proto, abyste zmenšili šířku pásma replikace. Možná nebudete chtít replikovat disky s dočasnými daty nebo daty, která se obnovují při každém restartování počítače nebo aplikace (jako je například soubor pagefile.sys nebo databáze tempdb Microsoft SQL Serveru). Disk můžete z replikace vyloučit zrušením výběru disku.
    - Vyloučit můžete jenom základní disky. Disky operačního systému vyloučit nelze.
    - Doporučujeme, abyste nevylučovali dynamické disky. Site Recovery nemůže zjistit, jestli je virtuální pevný disk ve virtuálním počítači hosta základní nebo dynamický. Pokud nejsou vyloučeny všechny disky se závislými dynamickými svazky, chráněné dynamické disky se budou při převzetí služeb při selhání na virtuálním počítači zobrazovat jako chybné a data na takovém disku budou nepřístupná.
        - Po povolení replikace už není možné přidávat nebo odebírat disky pro replikaci. Pokud chcete přidat nebo vyloučit disk, budete muset zakázat ochranu virtuálního počítače a potom ji znovu povolit.
        - Disky, které ručně vytvoříte v Azure, nebude možné po navrácení služeb obnovit. Například pokud provedete převzetí služeb při selhání u tří disků a dva disky vytvoříte přímo ve virtuálním počítači Azure, po obnovení z Azure do Hyper-V se navrátí pouze tři disky, u nichž se provedlo převzetí služeb při selhání. Ručně vytvořené disky není možné zahrnout do navrácení služeb po obnovení ani do zpětné replikace z Hyper-V do Azure.
        - Pokud vyloučíte disk, který je nezbytný pro provoz aplikace, po převzetí služeb při selhání do Azure jej budete muset v Azure znovu ručně vytvořit, aby se replikovaná aplikace mohla spustit. Alternativně můžete do plánu obnovení integrovat službu Azure Automation, která disk vytvoří během převzetí služeb při selhání počítače.

10. Kliknutím na **OK** uložte změny. Později můžete nastavit další vlastnosti.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication6-with-exclude-disk.png)

11. V **Nastavení replikace** > **Konfigurace nastavení replikace** vyberte zásadu replikace, kterou chcete použít pro chráněné virtuální počítače. Pak klikněte na **OK**. Můžete změnit zásady replikace v **zásady replikace** > název zásady > **upravit nastavení**. Změny, které použijete, se použijí pro počítače, které už replikujete, a nové počítače.


   ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

Průběh úlohy **Zapnout ochrany** můžete sledovat v části **Úlohy** > **Úlohy Site Recovery**. Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.

### <a name="view-and-manage-vm-properties"></a>Zobrazení a správa vlastností virtuálního počítače

Doporučujeme ověřit vlastnosti zdrojového počítače.

1. V **chráněné položky**, klikněte na tlačítko **replikované položky**a vyberte počítač.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)
2. V části **Vlastnosti** můžete zobrazit informace o replikaci a převzetí služeb při selhání pro virtuální počítač.

    ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)
3. V části **Výpočty a síť** > **Výpočetní vlastnosti** můžete zadat název a cílovou velikost virtuálního počítače Azure. Název pro dosažení souladu s požadavky na Azure, pokud potřebujete změňte. Můžete také zobrazit a upravit informace o cílové síti, podsíti a IP adrese, která bude přiřazená k virtuálnímu počítači Azure. Je třeba počítat s následujícím:

   * Můžete nastavit cílovou IP adresu. Pokud adresu nezadáte, bude počítač, který převezme služby při selhání, používat DHCP. Pokud nastavíte adresu, která není k dispozici pro převzetí služeb při selhání, převzetí služeb při selhání se nezdaří. Stejnou cílovou IP adresu je možné použít pro testovací převzetí služeb při selhání, pokud je adresa k dispozici v testovací síti převzetí služeb při selhání.
   * Počet síťových adaptérů závisí na velikosti, kterou zadáte pro cílový virtuální počítač, a to následujícím způsobem:

     * Pokud je počet síťových adaptérů na zdrojovém počítači menší nebo roven počtu adaptérů, které jsou povolené pro velikost cílového počítače, pak bude mít cíl stejný počet adaptérů jako zdroj.
     * Pokud počet adaptérů pro zdrojový virtuální počítač překračuje počet povolený pro cílovou velikost, použije se maximální velikost cíle.
     * Pokud má například zdrojový počítač dva síťové adaptéry a velikost cílového počítače podporuje čtyři, bude mít cílový počítač dva adaptéry. Pokud má zdrojový počítač dva adaptéry, ale podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač pouze jeden adaptér.     
     * Pokud má virtuální počítač více síťových adaptérů, připojí se všechny ke stejné síti.
     * Pokud virtuální počítač má několik síťových adaptérů se pak stane první z nich uvedené v seznamu *výchozí* síťový adaptér ve virtuálním počítači Azure.

     ![Povolení replikace](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

4. V **disky**, uvidíte operačního systému a datové disky na virtuální počítač, který bude replikován.

#### <a name="managed-disks"></a>Managed Disks

V části **Výpočty a síť** > **Výpočetní vlastnosti** můžete pro virtuální počítač nastavit Použít spravované disky na hodnotu Ano, pokud chcete spravované disky připojit k počítači při migraci do Azure. Spravované disky zjednodušují správu disků virtuálních počítačů Azure IaaS tím, že spravují účty úložiště přidružené k diskům virtuálních počítačů. [Přečtěte si další informace o spravovaných discích](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Spravované disky se vytvoří a připojí k virtuálnímu počítači jenom v případě, že při selhání služby převezme Azure. Když zapnete ochranu, data z místních počítačů se budou i nadále replikovat do účtů úložiště.
   Spravované disky je možné vytvořit pouze pro virtuální počítače nasazené pomocí modelu nasazení Resource Manageru.

  > [!NOTE]
  > U počítačů se spravovanými disky se momentálně nepodporuje navrácení služeb po obnovení z Azure do místního prostředí Hyper-V. Nastavte vlastnost Použít spravované disky na Ano jenom v případě, že máte v plánu tento počítač migrovat do Azure.

   - Když nastavíte vlastnost Použít spravované disky na Ano, budete moci vybrat jenom skupiny dostupnosti ve skupině prostředků, která má vlastnost Použít spravované disky nastavenou na Ano. Důvodem je to, že virtuální počítače se spravovanými disky můžou být součástí jenom skupin dostupnosti, které mají vlastnost Použít spravované disky nastavenou na Ano. Při vytváření skupin dostupnosti nezapomeňte vlastnost Použít spravované disky nastavit podle toho, jak chcete se spravovanými disky nakládat při převzetí služeb při selhání. Stejně tak když nastavíte Použít spravované disky na Ne, budete moci vybrat jenom skupiny dostupnosti ve skupině prostředků, která má vlastnost Použít spravované disky nastavenou na Ne. [Přečtěte si další informace o spravovaných discích a skupinách dostupnosti](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Pokud byl někdy účet úložiště používaný pro replikaci zašifrován pomocí šifrování služby Storage, vytvoření spravovaných disků se během převzetí služeb při selhání nezdaří. Buď můžete vlastnost Použít spravované disky nastavit na Ne a pokusit se o převzetí služeb při selhání, nebo můžete zakázat ochranu virtuálního počítače a chránit ho pomocí účtu úložiště, který nikdy neměl povoleno šifrování služby Storage.
  > [Další informace o šifrování služby Storage a o spravovaných discích](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption)


## <a name="test-the-deployment"></a>Otestování nasazení

Pokud budete chtít otestovat nasazení, můžete spustit test převzetí služeb při selhání pro jediný virtuální počítač nebo plán obnovení, který obsahuje jeden nebo více virtuálních počítačů.

### <a name="before-you-start"></a>Než začnete

 - Pokud se chcete po převzetí služeb při selhání připojit k virtuálním počítačům Azure pomocí protokolu RDP, přečtěte si informace o [přípravě připojení](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Abyste mohli převzetí služeb při selhání plně otestovat, musíte zkopírovat Active Directory a DNS do testovacího prostředí. [Další informace](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

1. K převzetí služeb při selhání jednom počítači, v **replikované položky**, klikněte na virtuální počítač > **+ testovací převzetí služeb při selhání** ikonu.
2. Pokud chcete pro převzetí služeb při selhání použít plán obnovení, klikněte v části **Plány obnovení** pravým tlačítkem myši na plán > **Otestovat převzetí služeb při selhání**. Pokud chcete vytvořit plán obnovení, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).
3. V **testovací převzetí služeb při selhání**, vyberte síť Azure, ke které virtuální počítače Azure připojí po převzetí služeb při selhání.
4. Kliknutím na **OK** zahajte převzetí služeb při selhání. Průběh můžete sledovat kliknutím na virtuálním počítači otevřete jeho vlastnosti, nebo na **testovací převzetí služeb při selhání** úlohy v název trezoru > **úlohy** > **úlohy Site Recovery**.
5. Po dokončení převzetí služeb při selhání by se vám také měl zobrazit počítač Azure repliky na portálu Azure Portal > **Virtuální počítače**. Měli byste zajistit, aby měl virtuální počítač odpovídající velikost, byl připojený k odpovídající síti a aby běžel.
6. Pokud jste připravili připojení po převzetí služeb při selhání, měli byste být schopni se k virtuálnímu počítači Azure připojit.
7. Až budete hotovi, klikněte v plánu obnovení na **Cleanup test failover** (Vyčistit po testu převzetí při selhání). V části **Poznámky** si zaznamenejte a uložte jakékoli připomínky související s testovacím převzetím služeb při selhání. Tím odstraníte virtuální počítače, které se vytvořily během testu.

Další podrobnosti najdete v článku [Testování převzetí služeb při selhání pomocí Azure](site-recovery-test-failover-to-azure.md).



## <a name="monitor-the-deployment"></a>Monitorování nasazení

Monitorujte nastavení konfigurace, stavu a stavu pro nasazení Site Recovery:

1. Klikněte na název trezoru. Tím se dostanete na řídicí panel **Základy**. V tomto řídicím panelu můžete sledovat úlohy Site Recovery, stav replikace, plány obnovení, stav serveru a události.  

    ![Základy](./media/site-recovery-hyper-v-site-to-azure/essentials.png)
2. V **stavu** dlaždice můžete monitorovat servery lokality, které dochází k potížím a události vyvolané službou Site Recovery za posledních 24 hodin. Řídicí panel Základy si můžete přizpůsobit, aby se na něm zobrazovaly dlaždice a rozložení, které jsou pro vás nejužitečnější, včetně stavu dalších trezorů Site Recovery a Backup.
3. Replikaci můžete spravovat a monitorovat na dlaždicích **Replikované položky**, **Plány obnovení** a **Úlohy Site Recovery**. Můžete zobrazit další podrobnosti o úlohách další podrobnosti v **úlohy** > **úlohy Site Recovery**.

## <a name="command-line-provider-and-agent-installation"></a>Zprostředkovatel a agent instalace z příkazového řádku

Zprostředkovatele Azure Site Recovery a agenta můžete nainstalovat i pomocí následující příkazový řádek. Tuto metodu je možné použít k instalaci zprostředkovatele na jádro serveru pro Windows Server 2012 R2.

1. Stáhněte si instalační soubor zprostředkovatele a registrační klíč do složky. Například C:\ASR.
2. Spuštěním těchto příkazů na příkazovém řádku se zvýšenými oprávněními vyextrahujte instalační program zprostředkovatele:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Spuštěním tohoto příkazu nainstalujte součásti:

            C:\ASR> setupdr.exe /i
4. Potom spuštěním těchto příkazů zaregistrujte server v trezoru:

            CD C:\Program Files\Microsoft Azure Site Recovery Provider\
            C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file>

<br/>
Kde:

* **/ Credentials**: povinný parametr, který určuje umístění, ve kterém je umístěn soubor registračního klíče  
* **/FriendlyName**: Povinný parametr pro název serveru hostitele technologie Hyper-V, který se zobrazí na portálu Azure Site Recovery.
* **/proxyAddress**: Volitelný parametr, který určuje adresu proxy serveru.
* **/proxyport** je volitelný parametr, který určuje port proxy serveru.
* **/ proxyusername**: volitelný parametr, který určuje uživatelské jméno Proxy (pokud proxy server vyžaduje ověření).
* **/ proxypassword**: volitelný parametr, který určuje heslo pro ověřování pomocí serveru proxy (pokud proxy server vyžaduje ověření).


## <a name="network-bandwidth-considerations"></a>Aspekty šířky pásma sítě
Můžete použít [nástroje Plánovač kapacity technologie Hyper-V](site-recovery-capacity-planner.md) můžete vypočítat šířku pásma potřebujete pro replikaci (počáteční replikace a pak rozdílové). K řízení velikosti šířky pásma využívané pro replikaci máte tyto možnosti:

* **Omezení šířky pásma**: přenos Hyper-V, která se replikují do Azure prochází konkrétního hostitele technologie Hyper-V. Šířku pásma na hostitelském serveru můžete omezit.
* **Optimalizace šířky pásma:** Šířku pásma používanou pro replikaci můžete ovlivnit pomocí několika klíčů registru.

### <a name="throttle-bandwidth"></a>Omezení šířky pásma
1. Otevřete modul snap-in Microsoft Azure Backup konzoly MMC na hostitelském serveru Hyper-V. Ve výchozím nastavení je na ploše nebo v umístění C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in klikněte na **Změnit vlastnosti**.
3. Na kartě **Omezování** vyberte **Povolit omezování šířky pásma internetu u operací zálohování** a nastavte limity pro pracovní a nepracovní dobu. Platné rozsahy jsou 512 kB/s až 102 MB/s.

    ![Omezení šířky pásma](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

Pro nastavení omezování můžete také použít rutinu [Set OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx). Tady je ukázkový skript:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** označuje, že není požadováno žádné omezování.

### <a name="influence-network-bandwidth"></a>Ovlivnění šířky pásma sítě
1. V registru přejděte na **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * K ovlivnění šířky pásma provoz na replikaci disku, změňte hodnotu **UploadThreadsPerVM**, nebo vytvořte klíč, pokud neexistuje.
   * K ovlivnění šířky pásma pro přenosy navrácení služeb po obnovení z Azure, změňte hodnotu **DownloadThreadsPerVM**.
2. Výchozí hodnota je 4. V síti s „nadměrným zřízením“ je třeba tyto klíče registru změnit z výchozích hodnot. Maximum je 32. Monitorováním provozu hodnotu optimalizujte.

## <a name="next-steps"></a>Další kroky

Po dokončení počáteční replikace a po otestování nasazení můžete podle potřeby vyvolat převzetí služeb při selhání. Přečtěte si [další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a o tom, jak je spustit.
