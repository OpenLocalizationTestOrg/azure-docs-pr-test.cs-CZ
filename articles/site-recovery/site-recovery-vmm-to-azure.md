---
title: "aaaReplicate virtuálních počítačů Hyper-V v nástroji VMM cloudů tooAzure | Microsoft Docs"
description: "Orchestraci replikace, převzetí služeb při selhání a obnovení virtuálních počítačů technologie Hyper-V spravované v cloudech tooAzure System Center VMM"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: 84182fe4b63862ac7643208a22f236a7515a25a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-site-recovery-in-hello-azure-portal"></a>Replikace virtuálních počítačů technologie Hyper-V v tooAzure cloudů VMM pomocí Site Recovery v hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Azure Classic](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell – Classic](site-recovery-deploy-with-powershell.md)


Tento článek popisuje, jak tooreplicate místní virtuální počítače Hyper-V spravované v cloudech tooAzure System Center VMM, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Pokud chcete, aby počítače tooAzure toomigrate (bez navrácení služeb po obnovení), přečtěte si informace v [v tomto článku](site-recovery-migrate-to-azure.md).


## <a name="deployment-steps"></a>Kroky nasazení

Postupujte podle článku toocomplete hello tyto kroky nasazení:


1. [Další informace](site-recovery-components.md) o architektuře hello pro toto nasazení. Dále si [můžete přečíst](site-recovery-hyper-v-azure-architecture.md), jak ve službě Site Recovery funguje replikace Hyper-V.
2. Ověřte požadavky a omezení.
3. Nastavte síť Azure a účty úložiště.
4. Připravte server hello místní VMM a hostitelé Hyper-V.
5. Vytvořte trezor služby Recovery Services. Trezor Hello obsahuje nastavení konfigurace a orchestruje replikaci.
6. Zadejte nastavení zdroje. Registraci serveru VMM hello v trezoru hello. Nainstalujte zprostředkovatele Azure Site Recovery hello hello serveru instalace hello služeb zotavení Microsoft agenta VMM na hostitelích Hyper-V.
7. Nastavte cíl a replikaci.
8. Povolení replikace pro virtuální počítače hello.
9. Spusťte toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.



## <a name="prerequisites"></a>Požadavky


**Požadavek na podporu** | **Podrobnosti**
--- | ---
**Azure** | Přečtěte si o [požadavcích na Azure](site-recovery-prereq.md#azure-requirements).
**Místní servery** | [Další informace](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-azure) o požadavcích pro server VMM místní hello a hostitelů Hyper-V.
**Místní virtuální počítače Hyper-V** | Virtuální počítače chcete tooreplicate by měla být spuštěná [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)a v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Adresy URL Azure** | Hello VMM server potřebuje přístup k toothese adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.<br/></br> Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).


## <a name="prepare-for-deployment"></a>Příprava nasazení
tooprepare pro nasazení, budete muset:

1. [Nastavit síť Azure](#set-up-an-azure-network), ve které budou virtuální počítače umístěné po převzetí služeb při selhání.
2. [Nastavit účet úložiště Azure](#set-up-an-azure-storage-account) pro replikovaná data.
3. [Příprava serveru VMM hello](#prepare-the-vmm-server) pro nasazení Site Recovery.
4. Připravit se na mapování sítě. Nastavte sítě tak, abyste mohli nakonfigurovat mapování sítě během nasazování Site Recovery.

### <a name="set-up-an-azure-network"></a>Nastavení sítě Azure
Budete potřebovat síti Azure toowhich virtuální počítače Azure vytvořené po převzetí služeb při selhání připojí.

* Hello síť musí být ve hello stejné oblasti jako hello trezoru služeb zotavení.
* V závislosti na modelu prostředků hello chcete toouse pro převzetí služeb virtuálních počítačích Azure při selhání, nastavíte hello síť Azure [režimu Resource Manager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) nebo [klasickém režimu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* Doporučujeme nastavit síť ještě před tím, než začnete. Pokud to neuděláte, musíte toodo ji během nasazování Site Recovery.
Azure sítě používané službou Site Recovery nemůže být [přesunout](../azure-resource-manager/resource-group-move-resources.md) v rámci hello stejné, nebo v různých, předplatných.

### <a name="set-up-an-azure-storage-account"></a>Nastavení účtu úložiště Azure
* Je nutné účtu úložiště Azure, že toohold data replikují tooAzure standard nebo premium. [Storage úrovně premium](../storage/common/storage-premium-storage.md) se používá pro virtuální počítače, které je třeba konzistentně vysoký výkon vstupně-výstupní operace a s nízkou latencí toohost vstupně-výstupní operace náročné úlohy. Pokud chcete toouse toostore účtu premium replikovaná data, musíte taky standardní úložiště účet toostore protokoly replikace, zachycení probíhající změny tooon místní data. Hello účet musí být v hello stejné oblasti jako hello trezoru služeb zotavení.
* V závislosti na modelu prostředků hello chcete toouse pro převzetí služeb virtuálních počítačích Azure při selhání, nastavíte účet v [režimu Resource Manager](../storage/common/storage-create-storage-account.md) nebo [klasickém režimu](../storage/common/storage-create-storage-account.md).
* Doporučujeme nastavit účet ještě před tím, než začnete. Pokud to neuděláte, musíte toodo ji během nasazování Site Recovery.
- Všimněte si, že účty úložiště používané službou Site Recovery nemůže být [přesunout](../azure-resource-manager/resource-group-move-resources.md) v rámci hello stejné, nebo v různých, předplatných.

### <a name="prepare-hello-vmm-server"></a>Příprava serveru VMM hello
* Ujistěte se, že hello server VMM splňuje hello [požadavky](#prerequisites).
* Během nasazování Site Recovery můžete zadat, že všechny cloudy na serveru VMM, by měly být dostupné v hello portálu Azure. Pokud chcete pouze konkrétní cloudy tooappear hello portálu, můžete povolit toto nastavení v hello cloudu v konzole pro správu VMM hello.

### <a name="prepare-for-network-mapping"></a>Příprava mapování sítě
Během nasazování Site Recovery musíte nastavit mapování sítě. Mapování sítě zajišťuje mapování mezi zdrojovými sítě virtuálních počítačů nástroje VMM a cílovými sítěmi Azure tooenable hello následující:

* Počítače, které se Překlopí na hello stejné sítě můžete připojit tooeach jiné, i když nejsou při selhání ve stejné hello způsobem nebo ve hello stejného plánu obnovení.
* Pokud na hello cílové síti Azure nastavená brána sítě, virtuální počítače Azure připojovat tooon lokální virtuální počítače.
* tooset si mapování sítě, zde je co potřebujete:

  * Zajistěte, aby virtuální počítače na zdrojovém hello hostitelský server Hyper-V byla připojené tooa sítě virtuálních počítačů nástroje VMM. Tuto síť by měla být propojené tooa logické sítě, který je přidružený k hello cloudu.
  * Síť Azure, jak je popsána [výše](#set-up-an-azure-network)

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na **Nový** > **Monitorování a správa** > **Backup a Site Recovery (OMS)**.

    ![Nový trezor](./media/site-recovery-vmm-to-azure/new-vault3.png)
3. V **název**, zadejte popisný název tooidentify hello trezoru. Máte-li více předplatných, vyberte jedno z nich.
4. [Vytvořte skupinu prostředků](../azure-resource-manager/resource-group-template-deploy-portal.md) nebo vyberte existující skupinu. Zadejte oblast Azure. Počítače budou replikované toothis oblast. toocheck podporované oblasti, najdete v části geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/)
5. Pokud chcete, aby tooquickly přístup hello trezoru z hello řídicí panel, klikněte na tlačítko **Pin toodashboard** > **vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-vmm-to-azure/new-vault.png)

Hello nový trezor se zobrazí na hello **řídicí panel** > **všechny prostředky**a na hello hlavní **trezory služeb zotavení** okno.


## <a name="select-hello-protection-goal"></a>Vyberte cíl ochrany hello

Vyberte, co chcete tooreplicate, a místo, kam chcete tooreplicate k.

1. V **trezory služeb zotavení**vyberte trezor hello.
2. V části **Začínáme** klikněte na **Site Recovery** > **Připravit infrastrukturu** > **Cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-vmm-to-azure/choose-goals.png)
3. V **cíl ochrany** vyberte **tooAzure**a vyberte **Ano, s technologií Hyper-V**. Vyberte **Ano** tooconfirm používáte hostitelů nástroje VMM toomanage technologie Hyper-V a hello obnovení lokality. Pak klikněte na **OK**.

## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí

Nainstalujte hello zprostředkovatele Azure Site Recovery na serveru VMM hello a zaregistrujte hello server v trezoru hello. Nainstalujte agenta služeb zotavení Azure hello na hostitele Hyper-V.

1. Klikněte na **Připravit infrastrukturu** > **Zdroj**.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source1.png)

2. V **připravit zdroj**, klikněte na tlačítko **+ VMM** tooadd serveru VMM.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source2.png)

3. V **přidat Server**, zkontrolujte, zda **serveru System Center VMM** se zobrazí v **typ serveru** a tímto serverem VMM hello splňuje hello [požadavky a adresy URL požadavky na](#prerequisites).
4. Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery hello.
5. Stáhněte si registrační klíč hello. Budete ho potřebovat, když spustíte instalaci. Hello klíč je platný pět dní od jeho vygenerování.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-azure/set-source3.png)


## <a name="install-hello-provider-on-hello-vmm-server"></a>Nainstalujte hello zprostředkovatele na serveru VMM hello

1. Spusťte instalační soubor zprostředkovatele hello na serveru VMM hello.
2. V rámci **Microsoft Update** můžete vyjádřit výslovný souhlas s aktualizacemi, aby se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update.
3. V **instalace**, přijmout nebo úpravě hello výchozí umístění instalace zprostředkovatele a klikněte na tlačítko **nainstalovat**.

    ![Umístění instalace](./media/site-recovery-vmm-to-azure/provider2.png)
4. Po dokončení instalace klikněte na tlačítko **zaregistrovat** tooregister hello serveru VMM v trezoru hello.
5. V hello **nastavení trezoru** klikněte na tlačítko **Procházet** tooselect hello soubor klíče trezoru. Zadejte předplatné Azure Site Recovery hello a název trezoru hello.

    ![Registrace serveru](./media/site-recovery-vmm-to-azure/provider10.PNG)
6. V **připojení k Internetu**, zadejte, jak hello hello zprostředkovatel, který běží na serveru VMM hello připojí tooSite obnovení přes internet.

   * Pokud chcete hello zprostředkovatele tooconnect přímo, vyberte **připojit přímo bez proxy tooAzure Site Recovery**.
   * Pokud váš stávající proxy server vyžaduje ověření, nebo chcete toouse vlastní proxy server, vyberte **připojit tooAzure Site Recovery pomocí proxy serveru**.
   * Pokud používáte vlastní proxy server, zadejte hello adresu, port a přihlašovací údaje.
   * Pokud používáte proxy server, které by už mít povolené hello adresy URL popisované v [požadavky](#on-premises-prerequisites).
   * Pokud používáte vlastní proxy server, účet VMM RunAs (DRAProxyAccount) se vytvoří automaticky pomocí hello zadané přihlašovací údaje pro proxy. Hello proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. nastavení účtu VMM RunAs Hello lze upravit v konzole VMM hello. V **nastavení**, rozbalte položku **zabezpečení** > **účty spustit jako**a potom upravte hello heslo pro DRAProxyAccount. Služba VMM hello toorestart budete potřebovat, aby toto nastavení projevilo.

     ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)
7. Přijměte nebo změňte umístění certifikátu SSL, který je automaticky generován pro šifrování dat hello. Tento certifikát se používá, pokud povolíte šifrování dat pro cloud chráněný platformou Azure na portálu Azure Site Recovery hello. Uchovávejte tento certifikát v bezpečí. Když spustíte převzetí služeb při selhání tooAzure budete je potřebovat toodecrypt, pokud je povolené šifrování dat.

    > [!NOTE]
    > Doporučujeme toouse hello šifrování funkce poskytované službou Azure pro šifrování dat v klidovém stavu, místo použití možnost šifrování dat hello poskytované Azure Site Recovery. Hello šifrování funkce poskytované službou Azure lze zapnout pro úložiště > účet a pomáhá dosáhnout lepší výkon, protože hello šifrování a dešifrování se zpracovává souborem úložiště Azure.
    > [Další informace o šifrování služby Storage z Azure](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
8. V **název serveru**, zadejte popisný název tooidentify hello VMM server v trezoru hello. V konfiguraci clusteru zadejte název role clusteru VMM hello.
9. Povolit **synchronizovat metadata cloudu**, pokud chcete toosynchronize metadata pro všechny cloudy na serveru VMM hello s úložištěm hello. Tuto akci stačí toohappen jednou na každém serveru. Pokud nechcete, aby toosynchronize všechny cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech hello cloudu v konzole VMM hello. Klikněte na tlačítko **zaregistrovat** toocomplete hello procesu.

    ![Registrace serveru](./media/site-recovery-vmm-to-azure/provider16.PNG)
10. Spustí se registrace. Po dokončení registrace hello serveru zobrazuje v **infrastruktura Site Recovery** > **servery VMM**.


## <a name="install-hello-azure-recovery-services-agent-on-hyper-v-hosts"></a>Nainstalujte agenta služeb zotavení Azure hello na hostitele Hyper-V

1. Poté, co jste nastavili hello poskytovatele, musíte pro agenta služeb zotavení Azure hello toodownload hello instalační soubor. Spusťte instalační program na každém serveru technologie Hyper-V v cloudu VMM hello.

    ![Lokality Hyper-V](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)
2. Na stránce **Kontrola předpokladů** klikněte na **Další**. Automaticky se nainstalují všechny chybějící požadované součásti.

    ![Požadavky agenta Služeb zotavení](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)
3. V **nastavení instalace**přijměte nebo změňte umístění instalace hello a umístění mezipaměti hello. Hello mezipaměti můžete nakonfigurovat na jednotce, která má minimálně 5 GB dostupného úložiště, ale doporučujeme mezipaměti jednotku s 600 GB nebo více volného místa. Pak klikněte na **Nainstalovat**.
4. Po dokončení instalace klikněte na tlačítko **Zavřít** toofinish.

    ![Registrace agenta MARS](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

### <a name="command-line-installation"></a>Instalace pomocí příkazového řádku
Hello agenta služeb zotavení Microsoft Azure si můžete nainstalovat z příkazového řádku pomocí hello následující příkaz:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-toosite-recovery-from-hyper-v-hosts"></a>Nastavení Internetu proxy přístup tooSite obnovení z hostitelů Hyper-V

agent služeb zotavení Hello spuštěné v hostitelích technologie Hyper-V potřebuje tooAzure přístup k Internetu pro replikaci virtuálního počítače. Pokud získáváte přístup k hello Internetu prostřednictvím proxy serveru, nastavte ho takto:

1. Otevřete hello Microsoft Azure Backup konzoly MMC modul snap-in na hostiteli Hyper-V hello. Ve výchozím nastavení je k dispozici na ploše hello nebo v C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in hello, klikněte na **změnit vlastnosti**.
3. Na hello **konfiguraci proxy serveru** zadejte informace o proxy serveru.

    ![Registrace agenta MARS](./media/site-recovery-vmm-to-azure/mars-proxy.png)
4. Zkontrolujte, že agent hello přístup hello adresy URL popisované v hello [požadavky](#on-premises-prerequisites).

## <a name="set-up-hello-target-environment"></a>Nastavení cílového prostředí hello
Zadejte toobe účet úložiště Azure hello používá pro replikaci a hello toowhich síť Azure virtuální počítače Azure připojí po převzetí služeb při selhání.

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**, vyberte předplatné hello a hello skupinu prostředků, kam chcete hello toocreate při selhání virtuálního počítače. Vyberte model nasazení hello, který chcete toouse v Azure (klasický nebo prostředek management) pro hello při selhání virtuálního počítače.

    ![Úložiště](./media/site-recovery-vmm-to-azure/enablerep3.png)

2. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

    ![Úložiště](./media/site-recovery-vmm-to-azure/compatible-storage.png)

3. Pokud jste ještě nevytvořili účet úložiště, a chcete toocreate jeden pomocí Resource Manager, klikněte na tlačítko **+ účet úložiště** toodo přímo tady.  Na hello **vytvořit účet úložiště** okně zadejte název účtu, typ, předplatné a umístění. Hello účet by měl být v hello stejné umístění jako hello trezoru služeb zotavení.

   ![Úložiště](./media/site-recovery-vmm-to-azure/gs-createstorage.png)


   * Pokud chcete účet úložiště pomocí klasického modelu hello toocreate, udělat na hello portálu Azure. [Další informace](../storage/common/storage-create-storage-account.md)
   * Pokud pro replikovaná data používáte účet úložiště premium, nastavte na účet další standardní úložiště toostore protokoly replikace, které zaznamenání dat tooon místní probíhající změny.
5. Pokud jste ještě nevytvořili síť Azure a chcete toocreate jeden pomocí Resource Manager, klikněte na tlačítko **+ síť** toodo přímo tady. Na hello **vytvořit virtuální síť** okně zadejte název sítě, rozsah adres, podrobnosti o podsíti, předplatné a umístění. Hello síť musí být ve hello stejné umístění jako hello trezoru služeb zotavení.

   ![Síť](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

   Pokud chcete toocreate síť pomocí klasického modelu hello, udělat na hello portálu Azure. [Další informace](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Konfigurace mapování sítě

* [Projděte si](#prepare-for-network-mapping) rychlý přehled toho, co mapování sítě dělá.
* Ověřte, že jsou virtuální počítače na serveru VMM hello připojené tooa síť virtuálních počítačů a že jste vytvořili aspoň jednu virtuální síť Azure. Více sítí virtuálních počítačů může být namapované tooa jednu síť Azure.

Nakonfigurujte mapování následujícím způsobem:

1. V **infrastruktura Site Recovery** > **mapování sítí** > **mapování sítě**, klikněte na tlačítko hello **+ mapování sítě**  ikonu.

    ![Mapování sítě](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. V **přidání mapování sítě**, vyberte hello zdrojový server VMM, a **Azure** jako cíl hello.
3. Ověřte předplatné hello a hello model nasazení po převzetí služeb při selhání.
4. V **zdrojové síti**vyberte hello zdrojovou místní síť virtuálních počítačů má toomap hello seznamu přidružené k serveru VMM hello.
5. V **Cílová síť**, vyberte hello síť Azure, ve které repliky virtuálních počítačů Azure budou umístěné při vytváří. Pak klikněte na **OK**.

    ![Mapování sítě](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Když se začne mapovat síť, dojde k tomuto:

* Existující virtuální počítače ve zdrojové síti virtuálních počítačů hello jsou připojené toohello cílové síti, když začne mapování. Nová síť virtuálních počítačů připojených toohello zdrojové virtuální počítače jsou připojené toohello namapované síti Azure, když dojde k replikaci.
* Pokud upravíte existující mapování sítě, virtuální počítače repliky se připojují pomocí hello nové nastavení.
* Pokud má hello Cílová síť více podsítí a jedna z těchto podsítí má hello stejný název jako podsíť, na které hello zdrojový virtuální počítač nachází, pak virtuální počítač repliky hello připojí po převzetí služeb při selhání toothat cílové podsíti.
* Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač připojí toohello první podsíť v síti hello.

## <a name="configure-replication-settings"></a>Konfigurace nastavení replikace
1. Klikněte na tlačítko toocreate novou zásadu replikace, **připravit infrastrukturu** > **nastavení replikace** > **+ vytvořit a přidružit**.

    ![Síť](./media/site-recovery-vmm-to-azure/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady.
3. V **frekvence kopírování**, určete, jak často má tooreplicate rozdílová data po hello počáteční replikaci (každých 30 sekund, 5 nebo 15 minut).

    > [!NOTE]
    >  30 druhý frekvence nepodporuje při replikaci toopremium úložiště. omezení Hello je určen podle hello počet snímků za blob (100) nepodporuje storage úrovně premium. [Další informace](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. V **uchování bodu obnovení**, zadejte v hodinách, jak dlouho bude hello intervalem pro uchovávání dat se pro každý bod obnovení. Chráněné počítače může být obnovena tooany bodu v rámci časového období.
5. V nastavení **Frekvence snímků konzistentní vzhledem k aplikacím** určete, jak často (1–12 hodin) se mají vytvářet body obnovení obsahující snímky konzistentní vzhledem k aplikacím. Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače hello a snímky konzistentní s aplikací, která přebírá v okamžiku snímek dat aplikací hello uvnitř hello virtuálního počítače. Snímky konzistentní s aplikacemi pomocí tooensure služby Stínová kopie svazku (VSS), které aplikace budou při pořízení snímku hello v konzistentním stavu. Všimněte si, že pokud povolíte snímky konzistentní s aplikacemi, bude mít vliv hello výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byl hello hodnoty, které nastavíte, menší než počet hello další body obnovení, které nakonfigurujete.
6. V **čas spuštění počáteční replikace**, indikovat, kdy toostart hello počáteční replikace. Hello replikace se provede přes pásma vašeho internetového připojení, můžete chtít tooschedule ho dobu mimo špičky.
7. V **šifrovat data uložená v Azure**, určit, zda tooencrypt neaktivní data v úložišti Azure. Pak klikněte na **OK**.

    ![Zásady replikace](./media/site-recovery-vmm-to-azure/gs-replication2.png)
8. Když vytvoříte novou zásadu je přidružená automaticky hello cloudu VMM. Klikněte na **OK**. K této zásadě replikace můžete přidružit další cloudy VMM (a hello virtuální počítače v nich) **replikace** > název zásady > **přidružit Cloud VMM**.

    ![Zásady replikace](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="capacity-planning"></a>Plánování kapacity

Teď, když máte nastavenou základní infrastrukturu, začněte přemýšlet o plánování kapacity a zjistěte, jestli nepotřebujete další prostředky.

Site Recovery nabízí Plánovač toohelp kapacity přidělit hello správné prostředky pro vaše zdrojové prostředí, součásti Site Recovery, sítě a úložiště. Hello planner můžete spustit v rychlém režimu pro odhady založené na průměrném počtu virtuálních počítačů, disků a úložiště, nebo v podrobném režimu, ve kterém můžete zadávat hodnoty na úrovni zpracovávaných úloh hello. Než začnete, potřebujete:

* Získat informace o prostředí replikace, včetně virtuální počítačů, disků na virtuální počítač a úložišti na disk.
* Odhad hello denní míry změn s ohledem budete mít pro replikovaná data. Můžete použít hello [Capacity planner pro repliku technologie Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) toohelp to uděláte.

1. Klikněte na tlačítko **Stáhnout**, toodownload hello nástroje a potom ho spusťte. [Přečíst článek hello](site-recovery-capacity-planner.md) doprovodný nástroj hello.
2. Až budete hotoví, vyberte **Ano** v **jste spustili hello Capacity Planner**?

   ![Plánování kapacity](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

   Přečtěte si další informace o [řízení šířky pásma sítě](#network-bandwidth-considerations)




## <a name="enable-replication"></a>Povolení replikace

Než začnete, zajistěte, aby účtu uživatele Azure hello požadované [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci nové tooAzure virtuálního počítače.

Teď následujícím způsobem povolte replikaci:

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**. Po povolení replikace pro hello poprvé, klikněte na tlačítko **+ replikovat** v hello trezoru tooenable replikaci pro další počítače.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication1.png)
2. V hello **zdroj** okně, vyberte hello serveru VMM a hello cloud v které hello technologie Hyper-V jsou umístěni hostitelé. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication-source.png)
3. V **cíl**, vyberte hello předplatné, model nasazení post-převzetí služeb při selhání, a hello účet úložiště, který používáte pro replikovaná data.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication-target.png)
4. Vyberte účet úložiště hello, že chcete toouse. Pokud chcete, aby toouse jiný účet úložiště než ty, které máte, můžete [vytvořit](#set-up-an-azure-storage-account). Pokud pro replikovaná data používáte účet úložiště premium, je třeba tooselect protokoly další standardní úložiště účet toostore replikace, které zaznamenat data.toocreate probíhající změny tooon místní účet úložiště pomocí modelu Resource Manager hello Klikněte na tlačítko **vytvořit nový**. Pokud chcete účet úložiště pomocí klasického modelu hello toocreate, udělat [v hello portál Azure](../storage/common/storage-create-storage-account.md). Pak klikněte na **OK**.
5. Vyberte hello Azure toowhich síť a podsíť virtuálních počítačů Azure se připojí, když jste vytvořili po převzetí služeb při selhání. Vyberte **nakonfigurovat pro vybrané počítače**, tooapply hello nastavení tooall počítače sítě je vybrat pro ochranu. Vyberte **nakonfigurovat později**, tooselect hello síť Azure pro konkrétní počítač. Pokud chcete, aby toouse jinou síť než ten, který máte, můžete [vytvořit](#set-up-an-azure-network). hello toocreate sítě pomocí modelu Resource Manager, klikněte na **vytvořit nový**. Pokud chcete toocreate síť pomocí klasického modelu hello, udělat [v hello portál Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). V odpovídajícím případě vyberte podsíť. Pak klikněte na **OK**.
6. V **virtuální počítače** > **vybrat virtuální počítače**, klikněte na tlačítko a vyberte každý počítač má tooreplicate. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication5.png)

7. V **vlastnosti** > **konfigurovat vlastnosti**, vyberte hello operační systém pro virtuální počítače hello vybrané a hello disk operačního systému.

    - Ověřte, že hello název virtuálního počítače Azure (název cílové) v souladu s [požadavky na virtuální počítač Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).   
    - Ve výchozím nastavení jsou vybrány všechny disky hello hello virtuálních počítačů pro replikaci, je však možné vyjmout tooexclude disků je.

        - Můžete chtít tooexclude disky tooreduce replikace šířky pásma. Například chcete nemusí tooreplicate disků se dočasná data nebo data, která se má aktualizovat pokaždé, když počítače nebo aplikace se restartuje (například pagefile.sys nebo databázi tempdb systému Microsoft SQL Server). Hello disku můžete z replikace vyloučit unselecting hello disk.
        - Vyloučit můžete jenom základní disky. Disky operačního systému vyloučit nelze.
        - Doporučujeme, abyste nevylučovali dynamické disky. Site Recovery nemůže zjistit, jestli je virtuální pevný disk ve virtuálním počítači hosta základní nebo dynamický. Pokud nejsou všechny disky, závislé dynamický svazek vyloučené, hello chráněné dynamický disk se zobrazí jako poškozený disk při hello virtuálních počítačů při selhání a hello data na tomto disku nebudou přístupné.
        - Po povolení replikace už není možné přidávat nebo odebírat disky pro replikaci. Chcete-li tooadd nebo vyloučit disk, potřebujete toodisable ochranu pro hello virtuálního počítače a pak ji znovu povolte.
        - Disky, které ručně vytvoříte v Azure, nebude možné po navrácení služeb obnovit. Například pokud selhání tři disky a vytvořte dvě přímo ve virtuálním počítači Azure, pouze disky tři hello, které byly převzetí služeb při selhání se nepodařilo zpět z Azure tooHyper-V. Nesmí obsahovat disky ručně vytvořené v navrácení služeb po obnovení, nebo v zpětná replikace z tooAzure technologie Hyper-V.
        - Pokud vyloučíte disk, který je potřeba pro toooperate aplikaci, po převzetí služeb při selhání tooAzure musíte toocreate ručně v Azure, takže tento hello replikované aplikace, mohou spouštět. Alternativně může integrovat Azure automation do plánu obnovení disku hello toocreate během převzetí služeb při selhání hello počítače.

    Klikněte na tlačítko **OK** toosave změny. Později můžete nastavit další vlastnosti.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

8. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, vyberte zásadu replikace hello chcete tooapply hello chráněné virtuální počítače. Pak klikněte na **OK**. Můžete změnit zásady replikace hello v **zásady replikace** > název zásady > **upravit nastavení**. Změny, které použijete, se použijí pro počítače, které už replikujete, a nové počítače.

   ![Povolení replikace](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Můžete sledovat průběh hello **povolení ochrany** úlohy v **úlohy** > **úlohy Site Recovery**. Po hello **dokončení ochrany** úloha spustí, počítač hello je připraven na převzetí služeb při selhání.

### <a name="view-and-manage-vm-properties"></a>Zobrazení a správa vlastností virtuálního počítače

Doporučujeme ověřit vlastnosti hello hello zdrojového počítače. Mějte na paměti, že název virtuálního počítače Azure hello musí být v souladu s [požadavky na virtuální počítač Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. V **chráněné položky**, klikněte na tlačítko **replikované položky**, a vyberte hello počítač toosee její podrobnosti.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/vm-essentials.png)
2. V **vlastnosti**, můžete zobrazit replikace a informace o převzetí služeb při selhání pro hello virtuálních počítačů.

    ![Povolení replikace](./media/site-recovery-vmm-to-azure/test-failover2.png)
3. V **výpočty a síť** > **výpočetní vlastnosti**, můžete zadat název a cílovou velikost virtuálního počítače Azure hello. Upravit název toocomply hello s [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) Pokud potřebujete. Můžete také zobrazit a upravit informace o hello Cílová síť, podsíť a hello IP adresu, která je přiřazena toohello virtuálního počítače Azure.
Poznámky:

   * Můžete nastavit hello cílová IP adresa. Pokud adresu nezadáte, použije hello převzal počítač DHCP. Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, hello převzetí služeb při selhání se nezdaří. Dobrý den, které lze použít stejnou cílovou IP adresu pro testovací převzetí služeb při selhání, pokud není k dispozici v hello testovací převzetí služeb při selhání sítě hello adresa.
   * Hello počet síťových adaptérů závisí hello velikost, který jste zadali pro hello cílový virtuální počítač následujícím způsobem:

     * Pokud hello počet síťových adaptérů na zdrojovém počítači hello je menší než nebo rovna toohello počet adaptérů povoleno pro velikost cílového počítače hello pak bude mít cíl hello hello stejný počet adaptérů jako zdroj hello.
     * Hello počet adaptérů pro hello zdrojového virtuálního počítače, překročí-li hello počet povolených pro hello cílovou velikost, maximální velikost cíle hello je použita.
     * Například pokud má zdrojový počítač dva síťové adaptéry a velikost hello cílového počítače podporuje čtyři, bude mít hello cílový počítač dva adaptéry. Pokud má hello zdrojový počítač dva adaptéry, ale hello podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač hello jenom jeden adaptér.     
     * Pokud hello virtuální počítač má několik síťových adaptérů, připojí se všechny toohello stejné síti.

     ![Povolení replikace](./media/site-recovery-vmm-to-azure/test-failover4.png)

4. V **disky** uvidíte hello operačního systému a datové disky na virtuální počítač, který bude replikován hello.

#### <a name="managed-disks"></a>Managed Disks

V **výpočty a síť** > **výpočetní vlastnosti**, pokud chcete počítač tooyour tooattach spravovaných disků na tooAzure migrace můžete nastavit "Použití spravovaných disků" nastavení příliš "Ano" hello virtuálních počítačů. Spravované disky zjednodušuje Správa disku pro virtuální počítače Azure IaaS pomocí správy hello účty úložiště přidružené ke hello disky virtuálních počítačů. [Přečtěte si další informace o spravovaných discích](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Spravované disky jsou vytvořené a připojené toohello virtuální počítač jenom na tooAzure převzetí služeb při selhání. K povolení ochrany, data z místního počítače bude tooreplicate toostorage účty.
   Spravované disky se dají vytvořit jenom pro virtuální počítače nasazené pomocí modelu nasazení Resource manager hello.  

  > [!NOTE]
  > Navrácení služeb po obnovení z Azure tooon místního prostředí Hyper-V se aktuálně nepodporuje pro počítače s spravované disky. Nastaví "Použití spravovaných disků" příliš "Ano" pouze v případě, že chcete toomigrate tento počítač do Azure.

   - Při nastavení "Použití spravovaných disků" příliš "Ano", pouze skupiny dostupnosti ve skupině prostředků hello sadou "Použití spravovaných disků" příliš "Ano" by být k dispozici pro výběr. Je to proto, že virtuální počítače s spravované disky lze pouze součástí skupiny dostupnosti s "Použití spravovaných disků" vlastností nastavenou příliš "Ano". Zajistěte, aby vytváření skupiny dostupnosti s nastavenou vlastnost "Použití spravovaných disků" založené na discích záměrné toouse spravované na převzetí služeb při selhání.  Podobně když nastavíte "Použití spravovaných disků" příliš "žádný", pouze skupiny dostupnosti ve skupině prostředků hello s "Použití spravovaných disků" vlastností nastavenou příliš "žádný" by být k dispozici pro výběr. [Přečtěte si další informace o spravovaných discích a skupinách dostupnosti](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Pokud účet úložiště hello používané pro replikaci byla zašifrovaná pomocí šifrování služby úložiště v libovolném bodě v čase, vytvoření spravovaných disků během převzetí služeb při selhání se nezdaří. Můžete buď nastavit "Použití spravovaných disků" příliš "žádný" a zkuste zopakovat převzetí služeb při selhání nebo zakažte ochranu pro virtuální počítač hello a chránit tooa účet úložiště, který nemá povolené v libovolném bodě v čase šifrování služby úložiště.
  > [Další informace o šifrování služby Storage a o spravovaných discích](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption)


## <a name="test-hello-deployment"></a>Testovací nasazení pro hello

tootest hello nasazení můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo plán obnovení, který obsahuje jeden nebo více virtuálních počítačů.

### <a name="before-you-start"></a>Než začnete

 - Pokud chcete, aby tooconnect tooAzure virtuálních počítačů pomocí protokolu RDP po převzetí služeb při selhání, další informace o [Příprava tooconnect](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - test toofully potřebujete toocopy služby Active Directory a DNS v testovacím prostředí. [Další informace](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

1. toofail přes jeden virtuální počítač, v **replikované položky**, klikněte na tlačítko hello virtuálního počítače > **+ testovací převzetí služeb při selhání**.
2. plánování toofail přes obnovení, **plány obnovení**, klikněte pravým tlačítkem na hello plán > **testovací převzetí služeb při selhání**. plán obnovení toocreate [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).
3. V **testovací převzetí služeb při selhání**, vyberte možnost připojit hello toowhich síť Azure virtuální počítače Azure po převzetí služeb při selhání.
4. Klikněte na tlačítko **OK** toobegin hello převzetí služeb při selhání. Průběh můžete sledovat, když kliknete na virtuální počítač tooopen hello jeho vlastnosti, nebo na hello **testovací převzetí služeb při selhání** úlohy v **úlohy Site Recovery**.
5. Po dokončení převzetí služeb při selhání hello, měli byste také mít možnost toosee hello repliky počítač Azure se zobrazí v hello portálu Azure > **virtuální počítače**. Měli byste si ověřit, že hello virtuálního počítače je hello odpovídající velikost, zda byl připojen k příslušné síti toohello a je spuštěna.
6. Pokud jste připravili připojení po převzetí služeb při selhání, musí být schopný tooconnect toohello virtuálního počítače Azure.
7. Jakmile jste hotovi, klikněte na **vyčistit testovací převzetí služeb při selhání** v plánu obnovení hello. V **poznámky** zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání. Tato akce odstraní hello virtuálních počítačů, které byly vytvořeny během testovacího převzetí služeb při selhání.

Další informace najdete v hello [testovací převzetí služeb při selhání tooAzure](site-recovery-test-failover-to-azure.md) článku.

## <a name="monitor-hello-deployment"></a>Monitorování nasazení hello

Zde je, jak můžete monitorovat nastavení konfigurace, stav a stav hello nasazení Site Recovery:

1. Klikněte na hello tooaccess název trezoru hello **Essentials** řídicího panelu. V tomto řídicím panelu uvidíte úlohy Site Recovery, stav replikace, plány obnovení, stav serveru a události.  Můžete přizpůsobit **Essentials** tooshow hello dlaždice a rozložení, které jsou velmi užitečné tooyou, včetně hello stavu dalších trezorů Site Recovery a Backup.

    ![Základy](./media/site-recovery-vmm-to-azure/essentials.png)
2. V **stavu**, můžete sledovat problémy na místní servery (VMM nebo konfigurační servery) a hello události vyvolané službou Site Recovery v hello posledních 24 hodin.
3. V hello **replikované položky**, **plány obnovení**, a **úlohy Site Recovery** dlaždice můžete spravovat a sledovat replikace. Podrobnosti o úlohách si můžete zobrazit v části **Úlohy** > **Úlohy Site Recovery**.

## <a name="command-line-installation-for-hello-azure-site-recovery-provider"></a>Instalace z příkazového řádku pro hello zprostředkovatele Azure Site Recovery

Hello zprostředkovatele Azure Site Recovery můžete nainstalovat z příkazového řádku hello. Tato metoda může být použité tooinstall hello zprostředkovatele na jádro serveru pro Windows Server 2012 R2.

1. Hello zprostředkovatele instalace souboru a registraci klíče tooa složka pro stahování. Příklad: C:\ASR.
2. Z příkazového řádku se zvýšenými oprávněními spusťte instalační program tyto příkazy tooextract hello zprostředkovatele:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Spusťte tento příkaz tooinstall hello součásti:

            C:\ASR> setupdr.exe /i
4. Spusťte tyto příkazy, tooregister hello server v trezoru hello:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>       

Kde:

* **/ Credentials**: povinný parametr, který určuje, kde je umístěn soubor registračního klíče hello.  
* **/ FriendlyName**: povinný parametr pro hello název hostitele serveru hello technologie Hyper-V, který se zobrazí na portálu Azure Site Recovery hello.
* * **/ EncryptionEnabled**: volitelný parametr, pokud replikujete virtuální počítače Hyper-V v nástroji VMM cloudů tooAzure. Zadejte, pokud chcete, aby tooencrypt virtuální počítače v Azure (šifrování neaktivních dat). Ujistěte se, že hello má název souboru hello **.pfx** rozšíření. Šifrování je ve výchozím nastavení vypnuté.

    > [!NOTE]
    > Doporučujeme toouse hello šifrování funkce poskytované službou Azure pro šifrování dat v klidovém stavu, místo použití hello šifrování (možnost EncryptionEnabled) poskytované Azure Site Recovery. Hello šifrování funkce poskytované službou Azure lze zapnout pro účet úložiště a pomáhá dosáhnout lepšího výkonu jako hello šifrování a dešifrování se provádí v Azure  
    > Azure.
    > [Další informace o šifrování služby Storage v Azure](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
* **/ proxyaddress**: volitelný parametr, který určuje adresu hello hello proxy serveru.
* **/ proxyport**: volitelný parametr, který určuje port hello hello proxy serveru.
* **/ proxyusername**: volitelný parametr, který určuje uživatelské jméno proxy hello (pokud proxy server vyžaduje ověření).
* **/ proxypassword**: volitelný parametr, který určuje heslo tooauthenticate hello proxy serveru (Pokud hello proxy server vyžaduje ověření).


### <a name="network-bandwidth-considerations"></a>Aspekty šířky pásma sítě
Můžete použít hello kapacity planner nástroj toocalculate hello šířky pásma, které potřebujete pro replikaci (počáteční replikace a pak rozdílové). toocontrol hello množství využití šířky pásma pro replikaci, máte několik možností:

* **Omezení šířky pásma**: provozu technologie Hyper-V, který replikuje sekundární lokality tooa prochází konkrétního hostitele technologie Hyper-V. Můžete omezit šířku pásma na hostitelském serveru hello.
* **Optimalizace šířky pásma**: můžete ovlivnit hello pásma sítě používané pro replikaci pomocí několika klíčů registru.

#### <a name="throttle-bandwidth"></a>Omezení šířky pásma
1. Otevřete hello Microsoft Azure Backup konzoly MMC modul snap-in na serveru hostitele technologie Hyper-V hello. Ve výchozím nastavení je k dispozici na ploše hello nebo v C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin zástupce služby Microsoft Azure Backup.
2. V modulu snap-in hello, klikněte na **změnit vlastnosti**.
3. Na hello **omezování** vyberte **povolit využití šířky pásma Internetu u operací zálohování omezení**a nastavte hello limity pro pracovní a nepracovní dobu. Platné rozsahy jsou 512 kB/s too102 MB/s za sekundu.

    ![Omezení šířky pásma](./media/site-recovery-vmm-to-azure/throttle2.png)

Můžete taky hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) rutiny tooset omezení. Tady je ukázkový skript:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** označuje, že není požadováno žádné omezování.

#### <a name="influence-network-bandwidth"></a>Ovlivnění šířky pásma sítě
Hello **UploadThreadsPerVM** ovládacích prvků hodnota registru hello počet vláken, které se používají pro přenos dat (počáteční nebo rozdílové replikace) disku. Vyšší hodnota zvětšuje hello šířku pásma sítě používané pro replikaci. Hello **DownloadThreadsPerVM** hodnotu registru určuje hello počet vláken používaných pro přenos dat během navrácení služeb po obnovení.

1. V registru hello přejděte příliš**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

   * Změnit hodnotu hello **UploadThreadsPerVM** (nebo vytvořte klíč hello, pokud neexistuje) toocontrol vlákna používaná pro replikaci disku.
   * Změnit hodnotu hello **DownloadThreadsPerVM** (nebo vytvořte klíč hello, pokud neexistuje) toocontrol vlákna používaná pro navrácení služeb po obnovení provoz z Azure.
2. Hello výchozí hodnota je 4. V síti "nadměrným zřízením" tyto klíče registru by mělo být změněno z hello výchozí hodnoty. maximální Hello je 32. Monitorování přenosů toooptimize hello hodnotu.

## <a name="next-steps"></a>Další kroky

Po dokončení počáteční replikace a otestujete hello nasazení, můžete podle potřeby hello vyvolat převzetí služeb při selhání. [Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak toorun je.
