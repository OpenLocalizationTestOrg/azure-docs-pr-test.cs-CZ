---
title: "sekundární lokalitu tooa aaaReplicate virtuálních počítačů Hyper-V s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak hello tooreplicate virtuálních počítačů Hyper-V v nástroji VMM cloudy tooa sekundární VMM lokality pomocí portálu Azure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: b33a1922-aed6-4916-9209-0e257620fded
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: e79dbdeab74266e843e5146298dc5aadfe66b5df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site-using-hello-azure-portal"></a>Replikace virtuálních počítačů technologie Hyper-V ve VMM cloudy tooa sekundární lokalita VMM pomocí hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Portál Classic](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Tento článek popisuje, jak tooreplicate místní virtuální počítače Hyper-V spravované v cloudech System Center Virtual Machine Manager (VMM) pomocí sekundární lokality tooa [Azure Site Recovery](site-recovery-overview.md) v hello portálu Azure. Další informace o této [architekturu scénáře](site-recovery-components.md).

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites"></a>Požadavky

**Požadavek** | **Podrobnosti**
--- | ---
**Azure** | Potřebujete účet [Microsoft Azure](http://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery
**Místní VMM** | Doporučujeme, že abyste měli dva servery VMM, jeden v primární lokalitě hello a jeden v hello sekundární.<br/><br/> Můžete replikovat mezi cloudy na jednom serveru VMM.<br/><br/> Servery VMM by měl používat minimálně System Center 2012 SP1 s nejnovějšími aktualizacemi hello.<br/><br/> Servery VMM potřebovat přístup k Internetu.
**Cloudy VMM** | Každý server VMM musí mít na jeden nebo více cloudů a všechny cloudy musí mít nastaven profil hello kapacity technologie Hyper-V. <br/><br/>Cloudy musí obsahovat jeden nebo více skupin hostitelů VMM.<br/><br/> Pokud máte pouze jeden server VMM, musí aspoň dva cloudy, tooact jako primární a sekundární.
**Hyper-V** | Servery Hyper-V musí běžet minimálně Windows Server 2012 s rolí hello technologie Hyper-V, a mít hello nainstalované nejnovější aktualizace.<br/><br/> Server Hyper-V by měl obsahovat jeden nebo více virtuálních počítačů.<br/><br/>  Hostitelské servery Hyper-V by měl být umístěn v skupiny hostitelů v hello primárních a sekundárních cloudech VMM.<br/><br/> Pokud spustíte technologie Hyper-V v clusteru na Windows Server 2012 R2, nainstalujte [aktualizovat 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Pokud spustíte technologie Hyper-V v clusteru na Windows Server 2012, zprostředkovatele clusteru se nevytvoří automaticky, pokud máte statické IP adresy na základě clusteru. Ruční konfigurace zprostředkovatele clusteru hello. [Přečtěte si další informace](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).<br/><br/> Servery Hyper-V potřebují přístup k Internetu.
**Adresy URL** | Servery VMM a hostitelé Hyper-V by měl být schopný tooreach tyto adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

## <a name="deployment-steps"></a>Kroky nasazení

Zde je provést:

1. Ověřte požadavky.
2. Připravte hello VMM server a hostitelé technologie Hyper-V.
3. Vytvořte trezor služby Recovery Services. Trezor Hello obsahuje nastavení konfigurace a orchestruje replikaci.
4. Zadejte nastavení zdroje, cíl a replikace.
5. Nasazení služby Mobility hello na virtuálních počítačích chcete tooreplicate.
6. Příprava pro replikaci a zapnout replikaci pro virtuální počítače Hyper-V.
7. Spusťte toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.

## <a name="prepare-vmm-servers-and-hyper-v-hosts"></a>Příprava serverů VMM a hostitelé Hyper-V

tooprepare pro nasazení:

1. Zajistěte, aby hello VMM server a hostitelé technologie Hyper-V souladu s požadavky hello popsané výše a mohou dosáhnout hello požadované adresy URL.
2. Nastavení sítí VMM tak, aby můžete nakonfigurovat [mapování sítě](#network-mapping-overview).

    - Zajistěte, aby virtuální počítače na zdrojovém hello hostitelský server Hyper-V byla připojené tooa sítě virtuálních počítačů nástroje VMM. Tuto síť by měla být propojené tooa logické sítě, který je přidružený k hello cloudu.
    Ověřte, zda text hello sekundární cloudu, který používáte pro obnovení odpovídající síť virtuálních počítačů konfigurovanou. Tato síť virtuálních počítačů musí být propojené tooa logické sítě, který je spojen s hello sekundární cloudu.

3. Příprava [jednotné nasazení serveru](#single-vmm-server-deployment), pokud chcete, aby virtuální počítače tooreplicate mezi cloudy na hello stejnému serveru nástroje VMM.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na **Nové** > **Správy** > **Služby zotavení**.
3. V **název**, zadejte popisný název tooidentify hello trezoru. Máte-li více předplatných, vyberte jedno z nich.
4. [Vytvořte skupinu prostředků](../azure-resource-manager/resource-group-template-deploy-portal.md) nebo vyberte existující skupinu. Zadejte oblast Azure. Počítače jsou replikované toothis oblast. toocheck podporované oblasti, najdete v části geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/)
5. Pokud chcete, aby tooquickly přístup hello trezoru z hello řídicí panel, klikněte na tlačítko **Pin toodashboard** > **vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

Hello nový trezor se zobrazí na hello **řídicí panel**v **všechny prostředky**a na hello hlavní **trezory služeb zotavení** okno.


## <a name="choose-a-protection-goal"></a>Vyberte cíl ochrany

Vyberte, co chcete tooreplicate a místo, kam chcete tooreplicate k.

2. Klikněte na tlačítko **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.
3. Vyberte **toorecovery lokality**a vyberte **Ano, s technologií Hyper-V**.
4. Vyberte **Ano** tooindicate používáte hostitelů nástroje VMM toomanage hello technologie Hyper-V.
5. Vyberte **Ano** Pokud máte sekundární server VMM. Pokud nasazujete replikace mezi cloudy na jednom serveru VMM, klikněte na tlačítko **ne**. Pak klikněte na **OK**.

    ![Zvolte cíle.](./media/site-recovery-vmm-to-vmm/choose-goals.png)

## <a name="set-up-hello-source-environment"></a>Nastavit hello zdrojové prostředí

Nainstalujte hello zprostředkovatele Azure Site Recovery na serverech VMM a zjišťovat a zaregistrujte server v trezoru hello.

1. Klikněte na tlačítko **krok 1: Příprava infrastruktury** > **zdroj**.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-vmm/goals-source.png)
2. V **připravit zdroj**, klikněte na tlačítko **+ VMM** tooadd serveru VMM.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-vmm/set-source1.png)
3. V **přidat Server**, zkontrolujte, zda **serveru System Center VMM** se zobrazí v **typ serveru** a tímto serverem VMM hello splňuje hello [požadavky](#prerequisites).
4. Stáhněte si instalační soubor zprostředkovatele Azure Site Recovery hello.
5. Stáhněte si registrační klíč hello. Budete ho potřebovat, když spustíte instalaci. Hello klíč je platný pět dní od jeho vygenerování.

    ![Nastavení zdroje](./media/site-recovery-vmm-to-vmm/set-source3.png)
6. Nainstalujte hello zprostředkovatele Azure Site Recovery na serveru VMM hello. Nepotřebujete tooexplicitly nic instalovat na hostitelských serverech technologie Hyper-V.


### <a name="install-hello-azure-site-recovery-provider"></a>Nainstalujte zprostředkovatele Azure Site Recovery hello

1. Spusťte instalační soubor zprostředkovatele hello na každý server VMM. Pokud VMM je nasazen v clusteru, proveďte následující hello poprvé, co nainstalujete hello:
    -  Nainstalujte poskytovatele hello na aktivním uzlu a dokončit hello instalace tooregister hello VMM server v trezoru hello.
    - Potom nainstalujte hello zprostředkovatele na hello dalších uzlů. Uzly clusteru, měly být spuštěny hello stejnou verzi hello zprostředkovatele.
2. Instalační program spustí několik kontrol požadovaných součástí a požadavky služby VMM hello toostop oprávnění. Hello služby VMM se restartuje automaticky po dokončení instalace. Pokud instalujete na clusteru VMM, můžete se výzvami toostop hello Clusterové role.
3. V **Microsoft Update**, že se aktualizace zprostředkovatele nainstalovaly v souladu s vašimi zásadami Microsoft Update toospecify je volitelný.
4. V **instalace**, přijmout nebo upravte výchozí umístění instalace hello a klikněte na tlačítko **nainstalovat**.

    ![Umístění instalace](./media/site-recovery-vmm-to-vmm/provider-location.png)
5. Po dokončení instalace klikněte na tlačítko **zaregistrovat** tooregister hello server v trezoru hello.

    ![Umístění instalace](./media/site-recovery-vmm-to-vmm/provider-register.png)
6. V **název trezoru**, ověřte název hello hello úložiště, ve které hello bude server zaregistrovaný. Klikněte na *Další*.

    ![Registrace serveru](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)
7. V **připojení k Internetu**, určete, jak hello zprostředkovatel, který běží na serveru VMM hello připojuje tooAzure.

    ![Nastavení internetu](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

   - Můžete zadat tohoto zprostředkovatele hello by se měly připojit přímo toohello Internetu, nebo prostřednictvím proxy serveru.
   - Zadejte nastavení proxy serveru v případě potřeby.
   - Pokud používáte proxy server, účet VMM RunAs (DRAProxyAccount) se vytvoří automaticky pomocí hello zadané přihlašovací údaje pro proxy. Hello proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. Hello nastavení účtu spustit jako lze upravit v konzole VMM hello > **nastavení** > **zabezpečení** > **účty spustit jako**. Restartujte změny tooupdate služby VMM hello.
8. V **registrační klíč**, vyberte klíč hello stáhli z Azure Site Recovery a zkopírovali toohello serveru VMM.
9. nastavení šifrování Hello se používá pouze při replikaci virtuálních počítačů Hyper-V v tooAzure cloudy VMM. Pokud replikujete tooa sekundární lokality nepoužívá.
10. V **název serveru**, zadejte popisný název tooidentify hello VMM server v trezoru hello. V konfiguraci clusteru zadejte název role clusteru VMM hello.
11. V **synchronizovat metadata cloudu**vyberte, jestli chcete, aby toosynchronize metadata pro všechny cloudy na serveru VMM hello s úložištěm hello. Tuto akci stačí toohappen jednou na každém serveru. Pokud nechcete, aby toosynchronize všechny cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech hello cloudu v konzole VMM hello.
12. Klikněte na tlačítko **Další** toocomplete hello procesu. Po registraci načte Azure Site Recovery metadata ze serveru VMM hello. Hello server se zobrazí na hello **servery VMM** karty v hello **servery** stránky v trezoru hello.

    ![Server](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)
13. Po hello server je k dispozici v konzole Site Recovery hello v **zdroj** > **připravit zdroj** vyberte hello VMM server a vyberte hello cloudu, ve které hello technologie Hyper-V se hostitel nachází. Pak klikněte na **OK**.

Hello zprostředkovatele můžete také nainstalovat z příkazového řádku hello:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-hello-target-environment"></a>Nastavení cílového prostředí hello

Vyberte hello cílovém serveru VMM a cloud.

1. Klikněte na tlačítko **připravit infrastrukturu** > **cíl**a vyberte hello cílovém serveru VMM má toouse.
2. Zobrazí se cloudy na serveru hello, jež jsou synchronizovány se Site Recovery. Vyberte cílový cloud hello.

   ![cíl](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="set-up-replication-settings"></a>Nakonfigurování nastavení replikace

- Když vytvoříte zásadu replikace, všechny hostitele pomocí zásad hello musí mít hello stejný operační systém. Hello cloudu VMM může obsahovat hostitelů Hyper-V s různými verzemi systému Windows Server, ale v takovém případě musíte víc zásad replikace.
- Můžete provést hello počáteční replikaci v režimu offline. [Další informace](#prepare-for-initial-offline-replication)

1. Klikněte na tlačítko toocreate novou zásadu replikace **připravit infrastrukturu** > **nastavení replikace** > **+ vytvořit a přidružit**.

    ![Síť](./media/site-recovery-vmm-to-vmm/gs-replication.png)
2. V části **Vytvořit a přidružit zásady** zadejte název zásady. Hello zdrojové a cílové typ musí být **technologie Hyper-V**.
3. V **verze hostitele technologie Hyper-V**, vyberte, jaký operační systém běží na hostiteli hello.
4. V **typ ověřování** a **port ověřování**, zadejte, jak ověření přenosů mezi hello primární a servery hostitele obnovení technologie Hyper-V. Vyberte **certifikát** pouze tehdy, je pracovní prostředí protokolu Kerberos. Azure Site Recovery automaticky nakonfiguruje certifikáty pro ověřování pomocí protokolu HTTPS. Nepotřebujete toodo nic ručně. Ve výchozím nastavení port 8083 a 8084 (pro certifikáty) se otevřou v hello brány Windows Firewall na hostitelských serverech technologie Hyper-V hello. Pokud vyberete **Kerberos**, lístek protokolu Kerberos se použije pro vzájemné ověřování serverů hostitele hello. Všimněte si, že toto nastavení platí jen pro hostitelské servery technologie Hyper-V v systému Windows Server 2012 R2.
5. V **frekvence kopírování**, určete, jak často má tooreplicate rozdílová data po hello počáteční replikaci (každých 30 sekund, 5 nebo 15 minut).
6. V **uchování bodu obnovení**, zadejte v hodinách, jak dlouho bude hello intervalem pro uchovávání dat se pro každý bod obnovení. Chráněné počítače může být obnovena tooany bodu v rámci časového období.
7. V **frekvence snímkování konzistentní aplikace vzhledem**, zadejte, jak často (1 – 12 hodin) body obnovení obsahující snímky konzistentní s aplikacemi jsou vytvořeny. Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače hello a snímky konzistentní s aplikací, která přebírá v okamžiku snímek dat aplikací hello uvnitř hello virtuálního počítače. Snímky konzistentní s aplikacemi pomocí tooensure služby Stínová kopie svazku (VSS), které aplikace budou při pořízení snímku hello v konzistentním stavu. Pokud povolíte snímky konzistentní s aplikacemi, ovlivní hello výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byl hello hodnoty, které nastavíte, menší než počet hello další body obnovení, které nakonfigurujete.
8. V **kompresi přenosu dat**, zadejte, zda by měl být komprimované replikovaná data, která se přenáší.
9. Vyberte **odstranit repliku virtuálního počítače**, měla by být odstraněna toospecify, který hello virtuální počítač repliky, pokud zakažte ochranu pro hello zdrojového virtuálního počítače. Pokud povolíte toto nastavení při zakázání ochrany pro hello zdrojového virtuálního počítače se odebere z konzole Site Recovery hello, odeberou se z konzoly VMM hello nastavení Site Recovery pro hello VMM a hello repliky se odstraní.
10. V **počáteční metodu replikace**, pokud replikujete hello síti, určete, zda text hello počáteční replikace toostart, nebo ji můžete naplánovat. toosave šířku pásma sítě, můžete chtít tooschedule ho dobu mimo špičky. Pak klikněte na **OK**.

     ![Zásady replikace](./media/site-recovery-vmm-to-vmm/gs-replication2.png)
11. Když vytvoříte novou zásadu je přidružená automaticky hello cloudu VMM. V **zásady replikace**, klikněte na tlačítko **OK**. K této zásadě replikace můžete přidružit další Cloudy VMM (a hello virtuální počítače v nich) **replikace** > název zásady > **přidružit Cloud VMM**.

     ![Zásady replikace](./media/site-recovery-vmm-to-vmm/policy-associate.png)


### <a name="configure-network-mapping"></a>Konfigurace mapování sítě

- Další informace o [mapování sítě](#prepare-for-network-mapping) před zahájením.
- Ověřte, zda jsou virtuální počítače na serverech VMM připojené tooa síť virtuálních počítačů.


1. V **infrastruktura Site Recovery** > **mapování sítě** > **mapování sítí**, klikněte na tlačítko **+ mapování sítě**.

    ![Mapování sítě](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. V **přidání mapování sítě** vyberte hello zdrojové a cílové servery VMM. Hello sítě virtuálních počítačů spojené s servery VMM hello jsou načteny.
3. V **zdrojové síti**vyberte hello sítě chcete toouse ze seznamu hello sítě virtuálních počítačů spojené s primárním serverem VMM hello.
4. V **Cílová síť**vyberte hello sítě chcete toouse na sekundárním serveru VMM hello. Pak klikněte na **OK**.

    ![Mapování sítě](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Když se začne mapovat síť, dojde k tomuto:

* Všechny existující repliky virtuálních počítačů, které odpovídají zdrojové síti virtuálních počítačů toohello bude síť virtuálních počítačů připojených toohello cíl.
* Nové virtuální počítače, které jsou připojené toohello zdrojové síti virtuálních počítačů bude po replikaci připojené toohello cíl namapované síťové.
* Pokud upravíte existující mapování u nové sítě, virtuální počítače repliky připojí pomocí nového nastavení hello.
* Pokud hello Cílová síť více podsítí a jedna z těchto podsítí má hello nachází stejný název jako podsíť, na které hello zdrojového virtuálního počítače a potom virtuální počítač repliky hello budou po převzetí služeb při selhání připojené toothat cílové podsíti. Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač bude připojený toohello první podsíť v síti hello.

### <a name="configure-storage-mapping"></a>Konfigurace úložiště mapování.

[Mapování úložiště](#prepare-for-storage-mapping) není podporována v hello nový portál Azure. Ale můžete povolit, pomocí prostředí Powershell. [Další informace](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-7-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Krok 5: Plánování kapacity

Teď, když máte nastavenou základní infrastrukturu, začněte přemýšlet o plánování kapacity a zjistěte, jestli nepotřebujete další prostředky.

- Stažení a spuštění hello [Azure Site Recovery Capacity planner](site-recovery-capacity-planner.md), toogather informace o prostředí replikace, včetně virtuálních počítačů, disků na virtuální počítač a úložišti na disk.
- Poté, co jste shromažďují informace o replikaci v reálném čase, můžete upravit hello NetQos zásad toocontrol replikace šířky pásma pro virtuální počítače. Další informace o [omezení provozu replika technologie Hyper-V](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/), na blogu Thomase Maurer blogu. Další informace o hello [rutiny New-NetQosPolicy](https://technet.microsoft.com/library/hh967468.aspx.).

## <a name="enable-replication"></a>Povolení replikace

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**. Po povolení replikace pro hello poprvé, kliknutí **+ replikovat** v hello trezoru tooenable replikaci pro další počítače.

    ![Povolení replikace](./media/site-recovery-vmm-to-vmm/enable-replication1.png)
2. V **zdroj**, vyberte hello VMM server a hello cloudu, ve které hello hostitelů Hyper-V, které chcete tooreplicate nacházejí. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmm-to-vmm/enable-replication2.png)
3. V **cíl**, ověřte hello sekundární server VMM a cloud.
4. V **virtuální počítače**, vyberte virtuální počítače hello chcete tooprotect hello seznamu.

    ![Povolení ochrany virtuálního počítače](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

Můžete sledovat průběh hello **povolení ochrany** akce v **úlohy** > **úlohy Site Recovery**. Po hello **dokončení ochrany** dokončení úlohy, hello virtuální počítač je připraven na převzetí služeb při selhání.

Poznámky:

- Můžete také povolit ochranu pro virtuální počítače v konzole VMM hello. Klikněte na tlačítko **povolení ochrany** na panelu nástrojů hello ve vlastnostech virtuálního počítače hello > **Azure Site Recovery** kartě.
- Po povolení replikace, můžete zobrazit vlastnosti pro hello virtuálních počítačů v **replikované položky**. Na hello **Essentials** řídicího panelu, zobrazí se informace o zásadách replikace hello hello virtuálního počítače a jeho stav. Klikněte na tlačítko **vlastnosti** další podrobnosti.

### <a name="onboard-existing-virtual-machines"></a>Zařadit existující virtuální počítače
Pokud máte existující virtuální počítače v nástroji VMM, které jsou replikace s replikou technologie Hyper-V, můžete připojit je pro Azure Site Recovery replikaci následujícím způsobem:

1. Zkontrolujte, zda server hello technologie Hyper-V, který hostuje hello existující virtuální počítač se nachází v primárním cloudu hello a tento server technologie Hyper-V hello hostování virtuální počítač repliky hello je umístěný v cloudu sekundární hello.
2. Zajistěte, aby že zásady replikace je nakonfigurován pro hello primárního cloudu VMM.
3. Povolení replikace pro primární virtuální počítač hello. Azure Site Recovery a VMM zkontrolujte, zda hello stejného hostitele repliky a virtuální počítač je zjištěna a bude používat Azure Site Recovery a obnovit replikaci s využitím hello zadané nastavení.

## <a name="test-your-deployment"></a>Otestujte nasazení

tootest vaše nasazení, můžete spustit [testovací převzetí služeb při selhání](site-recovery-test-failover-vmm-to-vmm.md) pro jeden virtuální počítač, nebo [vytvoření plánu obnovení](site-recovery-create-recovery-plans.md) obsahující jeden nebo více virtuálních počítačů.



## <a name="prepare-for-offline-initial-replication"></a>Příprava pro počáteční replikaci prováděnou offline

Můžete provést offline replikaci pro kopírování dat počáteční hello. Můžete to následujícím způsobem připravit:

* Na zdrojovém serveru hello zadejte cestu umístění, ze které hello bude export dat probíhat. Přiřaďte úplné řízení pro oprávnění systému souborů NTFS a sdílení toohello služba VMM na cestu pro export hello. Na cílovém serveru hello určíte, že dojde k cestu umístění, ze kterého hello data importovat. Přiřaďte hello stejná oprávnění v této cestě importu.
* Pokud hello import nebo export cesta sdílená, přiřadit správce, Power Users, Print Operator nebo operátor serveru členství ve skupině pro účet služby VMM hello ve vzdáleném počítači hello na které hello sdílet se nachází.
* Pokud používáte všechny spustit jako účty tooadd hostitele, hello importovat a exportovat cesty, přiřadit pro čtení a zápis toohello účty spustit jako v nástroji VMM.
* Hello import a export sdílených složek nesmí nacházet na libovolném počítači se používá jako server hostitele technologie Hyper-V, protože konfigurace zpětné smyčky není podporována technologií Hyper-V.
* Ve službě Active Directory, na každém serveru hostitele technologie Hyper-V, který obsahuje virtuální počítače, které chcete tooprotect povolte a nakonfigurujte omezené delegování tootrust hello vzdálené počítačích, na které hello import a export cesty jsou umístěny, následujícím způsobem:
  1. V řadiči domény hello otevřete **Active Directory Users and Computers**.
  2. Ve stromu konzoly hello, klikněte na tlačítko **DomainName** > **počítače**.
  3. Název hostitelského serveru klikněte pravým tlačítkem na hello technologie Hyper-V > **vlastnosti**.
  4. Na hello **delegování** , klikněte na **důvěřovat tomuto počítači pro delegování pouze toospecified službám**.
  5. Klikněte na tlačítko **používající libovolný protokol pro ověřování**.
  6. Klikněte na tlačítko **přidat** > **uživatelé a počítače**.
  7. Název typu hello hello počítače, který je hostitelem cestu pro export hello > **OK**. Hello seznamu dostupných služeb, podržte klávesu CTRL hello a klikněte na **cifs** > **OK**. Opakujte pro hello název počítače hello cestu importu hello tohoto hostitele. Opakujte podle potřeby pro další hostitelské servery technologie Hyper-V.



## <a name="prepare-for-network-mapping"></a>Příprava mapování sítě
Mapování mapuje sítě mezi sítěmi virtuálních počítačů nástroje VMM na hello primární a sekundární servery VMM pro:

* Optimálně umístíte virtuální počítače repliky na sekundární hostitelů Hyper-V po převzetí služeb při selhání.
* Připojení sítě virtuálních počítačů tooappropriate virtuální počítače repliky po převzetí služeb při selhání.

Poznámky:
- Mapování sítě můžete nakonfigurovat sítě virtuálních počítačů na dvěma servery VMM nebo na jednom serveru VMM Pokud dvě lokality jsou spravovány nástrojem hello stejný server.
- Při mapování správně nakonfigurovaná a je povolená replikace, virtuální počítač na primárním umístění hello bude připojený tooa sítě a jeho repliky v cílové umístění hello se připojí tooits mapovat sítě.
- Pokud sítě byla nastavili správně v nástroji VMM, když při mapování sítě vyberte cílovou síť virtuálních počítačů, hello VMM zdroj cloudy, které používají hello zdrojové síti virtuálních počítačů se zobrazí, společně s hello dostupných cílových sítí virtuálních počítačů na hello cíl cloudy, které se používají pro ochrana.
- Pokud hello Cílová síť více podsítí a jedna z těchto podsítí má stejný název jako hello podsítě, na které hello zdrojový virtuální počítač nachází, pak hello hello bude virtuální počítač repliky po převzetí služeb při selhání připojené toothat cílové podsíti. Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač bude připojený toohello první podsíť v síti hello.


Tady je tooillustrate příklad tento mechanismus. Podívejme se na organizaci s dvěma umístěními v New Yorku a Chicagu.

| **Umístění** | **Server VMM** | **Sítě virtuálních počítačů** | **Mapovat na** |
| --- | --- | --- | --- |
| New York |VMM NewYork |VMNetwork1 NewYork |Mapovat tooVMNetwork1 Chicago |
| VMNetwork2 NewYork |Není mapováno | | |
| Chicago |VMM Chicago |VMNetwork1 Chicago |Mapovat tooVMNetwork1 NewYork |
| VMNetwork2 Chicago |Není mapováno | | |

Tento příklad:

* Když virtuální počítač repliky se vytvoří pro virtuální počítač, který je připojený tooVMNetwork1 NewYork, budou připojené tooVMNetwork1 Chicagu.
* Když virtuální počítač repliky se vytvoří pro VMNetwork2 NewYork nebo VMNetwork2 Chicagu, nebude připojený tooany sítě.

Zde je, jak jsou v naše ukázkové společnosti a hello logické sítě přidružené cloudy hello nastavit cloudech VMM.

### <a name="cloud-protection-settings"></a>Nastavení ochrany cloudu
| **Chráněném cloudu** | **Ochrana cloudu** | **Logické sítě (New Yorku)** |
| --- | --- | --- |
| GoldCloud1 |GoldCloud2 | |
| SilverCloud1 |SilverCloud2 | |
| GoldCloud2 |<p>Není k dispozici</p><p></p> |<p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p> |
| SilverCloud2 |<p>Není k dispozici</p><p></p> |<p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p> |

### <a name="logical-and-vm-network-settings"></a>Nastavení sítě logické a virtuální počítač
| **Umístění** | **Logické sítě** | **Přidružené sítě virtuálních počítačů** |
| --- | --- | --- |
| New York |LogicalNetwork1 NewYork |VMNetwork1 NewYork |
| Chicago |LogicalNetwork1 Chicago |VMNetwork1 Chicago |
| LogicalNetwork2Chicago |VMNetwork2 Chicago | |

### <a name="target-networks"></a>Cílové sítě
Na základě tohoto nastavení, když vyberete síť virtuálních počítačů cíl hello, hello následující tabulka znázorňuje hello možnosti, které budou k dispozici.

| **Výběr** | **Chráněném cloudu** | **Ochrana cloudu** | **Cílová síť k dispozici** |
| --- | --- | --- | --- |
| VMNetwork1 Chicago |SilverCloud1 |SilverCloud2 |Dostupné |
| GoldCloud1 |GoldCloud2 |Dostupné | |
| VMNetwork2 Chicago |SilverCloud1 |SilverCloud2 |Není k dispozici |
| GoldCloud1 |GoldCloud2 |Dostupné | |


### <a name="failback"></a>Navrácení služeb po obnovení
Co se stane, že v případě hello navrácení služeb po obnovení (zpětná replikace), toosee Předpokládejme, že je VMNetwork1 NewYork namapované tooVMNetwork1-Chicagu s hello následující nastavení.

| **Virtuální počítač** | **Připojené tooVM sítě** |
| --- | --- |
| VM1 |VMNetwork1 sítě |
| Virtuálního počítače 2 (repliky VM1) |VMNetwork1 Chicago |

S těmito nastaveními pojďme si shrnout, co se stane, že v několika možných scénářích.

| **Scénář** | **Výsledek** |
| --- | --- |
| Žádná změna v hello vlastnosti sítě virtuálních počítačů 2 po převzetí služeb při selhání. |Virtuální počítač 1 zůstává připojený toohello zdrojovou síť. |
| Vlastnosti sítě virtuálních počítačů 2 se změní po převzetí služeb při selhání a není připojen. |1 virtuální počítač není připojen. |
| Vlastnosti sítě virtuálních počítačů 2 se změní po převzetí služeb při selhání a je připojený tooVMNetwork2 Chicagu. |Pokud není namapované VMNetwork2 Chicagu, bude odpojen virtuálních počítačů 1. |
| Mapování sítě VMNetwork1 Chicagu se změní. |Virtuální počítač 1 bude nyní mapovat sítě připojené toohello tooVMNetwork1-Chicagu. |


## <a name="prepare-for-single-server-deployment"></a>Příprava pro nasazení jednoho serveru


Pokud máte pouze jeden server VMM, můžete replikovat virtuální počítače v hostitelích technologie Hyper-V v cloudu VMM hello příliš[Azure](site-recovery-vmm-to-azure.md) nebo tooa sekundární cloudu VMM. Protože replikovat mezi cloudy není bezproblémové doporučujeme první možnost hello. Pokud chcete tooreplicate mezi cloudy, můžete replikovat jeden samostatný server VMM nebo s jeden server VMM nasazený v roztažené clusteru se systémem Windows

### <a name="standalone-vmm-server"></a>Server samostatné VMM

V tomto scénáři nasazení hello jednoho serveru VMM jako virtuální počítač v primární lokalitě hello a replikovat tohoto virtuálního počítače tooa sekundárního webového serveru pomocí Site Recovery a replika technologie Hyper-V.

1. **Nastavit VMM na virtuálním počítači technologie Hyper-V**. Doporučujeme společné umístění instanci systému SQL Server hello používá VMM na hello stejného virtuálního počítače. Tím ušetříte čas, jako má toobe vytvořit pouze jeden virtuální počítač. Pokud chcete, aby toouse vzdálenou instanci systému SQL Server a dojde k výpadku, je třeba toorecover tuto instanci než bude možné obnovit VMM.
2. **Zkontrolujte má hello VMM server nakonfigurované alespoň dva cloudy**. Hello, které virtuální počítače mají tooreplicate a hello jiné cloudové bude sloužit jako hello sekundární umístění bude obsahovat jeden cloud. Hello cloudu, který obsahuje virtuální počítače hello chcete tooprotect, by měly splňovat [požadavky](#prerequisites).
3. Nastavte Site Recovery, jak je popsáno v tomto článku. Vytvoření a registrace serveru VMM hello v trezoru, nastavíte zásady replikace a povolení replikace. Hello zdrojové a cílové názvy VMM bude hello stejné. Zadejte že počáteční replikace probíhá přes síť hello.
4. Při nastavování mapování sítě namapujete hello síť virtuálních počítačů pro síť virtuálních počítačů toohello hello primárního cloudu pro cloud sekundární hello.
5. V konzole Správce technologie Hyper-V hello povolit replika technologie Hyper-V na hostiteli hello technologie Hyper-V, který obsahuje hello virtuálních počítačů nástroje VMM a zapnout replikaci na hello virtuálních počítačů. Zajistěte, aby že si nepřidáte tooclouds hello VMM virtuálního počítače, které jsou chráněné službou Site Recovery, tooensure nastavení repliky technologie Hyper-V nejsou přepsána Site Recovery.
6. Pokud vytvoříte plány obnovení pro převzetí služeb při selhání, které používáte hello stejnému serveru nástroje VMM zdroje a cíle.
7. V výpadku dokončení převzetí služeb při selhání a obnovit takto:

   1. Spusťte neplánované převzetí služeb při selhání v konzole Správce technologie Hyper-V hello v sekundární lokalitě hello toofail přes hello primárních virtuálních počítačů ve VMM toohello sekundární lokality.
   2. Ověřte, že hello virtuálních počítačů nástroje VMM je zapnutý a běží a v trezoru hello, spusťte toofail neplánované převzetí služeb při selhání přes hello virtuální počítače z primární toosecondary cloudů. Potvrdit hello převzetí služeb při selhání a vyberte alternativní bod obnovení v případě potřeby.
   3. Po hello neplánované převzetí služeb při selhání dokončení všechny prostředky jsou přístupné z primární lokality hello znovu.
   4. Pokud je k dispozici znovu v konzole Správce technologie Hyper-V hello v sekundární lokalitě hello hello primární lokality, povolte reverzní replikaci pro hello virtuálních počítačů nástroje VMM. Ze sekundární tooprimary spustí replikaci pro hello virtuálních počítačů.
   5. Spusťte plánované převzetí služeb při selhání v konzole Správce technologie Hyper-V hello v sekundární lokalitě hello toofail přes hello virtuálních počítačů ve VMM toohello primární lokality. Potvrzení převzetí služeb při selhání hello. Tak, aby hello virtuálních počítačů nástroje VMM je znovu replikace z primární toosecondary povolíte zpětnou replikaci.
   6. V trezoru služeb zotavení hello povolte reverzní replikaci pro hello zatížení virtuálních počítačů, je replikace z sekundární tooprimary toostart.
   7. V trezoru služeb zotavení hello spustit plánované převzetí služeb při selhání toofail back hello zatížení virtuálních počítačů toohello primární lokality. Potvrzení převzetí služeb při selhání toocomplete hello ho. Potom povolte zpětná replikace toostart replikaci hello zatížení z primárního toosecondary virtuálních počítačů.

### <a name="stretched-vmm-cluster"></a>Roztažené clusteru VMM

Místo nasazování samostatný server VMM jako virtuální počítač, který replikuje tooa sekundární lokality, můžete provést VMM, které jsou vysoce dostupné po nasazení jako virtuální počítač v clusteru s podporou převzetí služeb při selhání systému Windows. To poskytuje pružnost úloh a ochranu proti selhání hardwaru. toodeploy pomocí Site Recovery hello virtuálních počítačů nástroje VMM musí být nasazené v clusteru s podporou funkce stretch geograficky vzdálených lokalit. toodo toto:

1. Instalace VMM na virtuálním počítači v clusteru s podporou převzetí služeb při selhání systému Windows a vyberte hello možnost toorun hello serveru jako vysoce dostupný během instalace.
2. Hello instance systému SQL Server, který je používán VMM by měla být replikována se skupinami dostupnosti AlwaysOn serveru SQL, tak, aby bylo repliku hello databáze v sekundární lokalitě hello.
3. Postupujte podle pokynů hello v toocreate Tento článek k trezoru, zaregistrujte hello server a nastavení ochrany. Je nutné tooregister každý server VMM v hello cluster v hello trezor služeb zotavení. toodo, nainstalujete hello zprostředkovatele na aktivním uzlu a registrace serveru VMM hello. Potom nainstalujte hello zprostředkovatele na jiných uzlech.
4. Když dojde k výpadku, hello VMM server a jeho odpovídající databázi systému SQL Server jsou převzetí služeb při selhání a k němu přistupovat z hello sekundární lokality.



## <a name="prepare-for-storage-mapping"></a>Příprava na mapování úložiště


Nastavení úložiště mapování pomocí mapování klasifikace úložiště na zdroj a cíl VMM servery toodo hello následující:

  * **Určení cílového úložiště pro virtuální počítače repliky**– pevný disk zdrojový virtuální počítač bude replikovat toohello úložiště, který jste zadali (sdílené složce protokolu SMB nebo clusteru sdílené svazky (CSV)) v cílovém umístění hello.
  * **Umístění virtuálního počítače repliky**– mapování úložiště je použité toooptimally místní repliky virtuálních počítačů na hostitelských serverech technologie Hyper-V. Virtuální počítače repliky se umístí na hostitelích, kteří mohou přistupovat ke klasifikaci úložiště hello namapované.
  * **Žádné úložiště mapování**– Pokud nenakonfigurujete mapování úložiště, virtuální počítače budou replikované toohello výchozí umístění úložiště zadaná na hostitelském serveru Hyper-V hello přidružený virtuální počítač repliky hello.

Poznámky:
- Můžete nastavit mapování mezi dvěma cloudy VMM na jednom serveru.
- Klasifikace úložiště musí být umístěné v cloudech zdrojové a cílové skupiny hostitelů k dispozici toohello.
- Klasifikace nepotřebujete toohave hello stejný typ úložiště. Například můžete namapovat zdroj klasifikace, který obsahuje SMB sdílené složky tooa cíl klasifikace, která obsahuje sdílené svazky clusteru.

### <a name="example"></a>Příklad
Pokud klasifikace jsou správně nakonfigurována v nástroji VMM, když vyberete hello zdrojové a cílové serveru VMM během mapování úložiště, zobrazí se hello zdrojové a cílové klasifikace. Tady je příklad úložiště sdílených složek a klasifikace pro organizace s dvěma umístěními v New Yorku a Chicagu.

| **Umístění** | **Server VMM** | **Sdílené složky (zdroj)** | **Klasifikace (zdroj)** | **Mapovat na** | **Sdílené složky (cíl)** |
| --- | --- | --- | --- | --- | --- |
| New York |VMM_Source |SourceShare1 |ZLATÝ |GOLD_TARGET |TargetShare1 |
| SourceShare2 |STŘÍBRNÁ |SILVER_TARGET |TargetShare2 | | |
| SourceShare3 |BRONZOVÁ |BRONZE_TARGET |TargetShare3 | | |
| Chicago |VMM_Target | |GOLD_TARGET |Není mapováno | |
|  |SILVER_TARGET |Není mapováno | | | |
|  |BRONZE_TARGET |Není mapováno | | | |

Tento příklad:

* Když virtuální počítač repliky se vytvoří pro jakýkoli virtuální počítač na ZLATÝ úložiště (SourceShare1), bude replikované tooa GOLD_TARGET úložiště (TargetShare1).
* Když virtuální počítač repliky se vytvoří pro jakýkoli virtuální počítač na STŘÍBRNÝM úložiště (SourceShare2), bude úložiště replikované tooa SILVER_TARGET (TargetShare2) a tak dále.

### <a name="multiple-storage-locations"></a>Více umístění úložiště
Pokud cílový klasifikace hello přiřazený toomultiple sdílené složky protokolu SMB nebo sdílené svazky clusteru, umístění hello optimální úložiště bude automaticky vybrána, když hello virtuální počítač je chráněný. Pokud je k dispozici žádné úložiště vhodnou cílovou hello zadali klasifikaci, hello výchozí umístění úložiště na hostiteli hello technologie Hyper-V je použité tooplace hello repliky virtuálních pevných disků.

Následující tabulka Hello ukazují, jak klasifikaci úložiště a sdílenými svazky clusteru se nastavují v našem příkladu.

| **Umístění** | **Klasifikace** | **Přidruženého úložiště** |
| --- | --- | --- |
| New York |ZLATÝ |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> |
| STŘÍBRNÁ |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p> | |
| Chicago |GOLD_TARGET |<p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p> |
| SILVER_TARGET |<p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p> | |

Tato tabulka shrnuje hello chování, pokud povolíte ochranu pro virtuální počítače (VM1 - VM5) v tomto příkladu prostředí.

| **Virtuální počítač** | **Úložiště zdroje.** | **Zdroj klasifikace** | **Mapovat cílového úložiště** |
| --- | --- | --- | --- |
| VM1 |C:\ClusterStorage\SourceVolume1 |ZLATÝ |<p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Obě GOLD_TARGET</p> |
| VIRTUÁLNÍHO POČÍTAČE 2 |\\FileServer\SourceShare1 |ZLATÝ |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Obě GOLD_TARGET</p> |
| VM3 |C:\ClusterStorage\SourceVolume2 |STŘÍBRNÁ |<p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p> |
| VM4 |\FileServer\SourceShare2 |STŘÍBRNÁ |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Obě SILVER_TARGET</p> |
| VM5 |C:\ClusterStorage\SourceVolume3 |Není k dispozici |Žádné mapování, tak hello výchozí umístění úložiště hello technologie Hyper-V hostiteli se používá. |



### <a name="data-privacy-overview"></a>Přehled dat o ochraně osobních údajů

Tato tabulka shrnuje, jak jsou uložena data v tomto scénáři:

- - -
| Akce | **Podrobnosti** | **Shromážděná data** | **Použití** | **Požadované** |
| --- | --- | --- | --- | --- |
| **Registrace** | Zaregistrujte VMM server v trezoru služeb zotavení. Pokud chcete později toounregister serveru, můžete tak učinit odstraněním informací o serveru hello z hello portálu Azure. | Po registraci serveru VMM shromažďuje, procesy, Site Recovery a přenese metadata o hello serveru VMM a názvy hello cloudů VMM hello detekovaných službou Site Recovery. | Hello dat je použité tooidentify a komunikaci se serverem VMM pro příslušné hello a nakonfigurujte nastavení pro příslušné cloudy VMM. | Tato funkce se vyžaduje. Pokud nechcete, aby toosend tato informace tooSite obnovení byste neměli používat služba Site Recovery hello. |
| **Povolení replikace** | Hello zprostředkovatele Azure Site Recovery je nainstalován na serveru VMM hello a hello kanál pro komunikaci s hello služba Site Recovery. Hello zprostředkovatele je dynamická knihovna (DLL) hostované v procesu VMM hello. Po hello, který je nainstalován poskytovatel získá v konzole pro správu VMM hello povolena funkce "Obnovení Datacentra" hello. Nové a stávající virtuální počítače můžete povolit tuto funkci tooenable ochranu pro virtuální počítač. |S touto sadou vlastností hello zprostředkovatele odešle hello název a ID tooSite hello virtuálních počítačů pro obnovení.  Ve Windows Server 2012 nebo Windows Server 2012 R2 Hyper-V repliky je povolena replikace. získá data virtuálního počítače Hello replikují z jeden tooanother hostitele technologie Hyper-V (obvykle umístěné v datovém centru různých "obnovení"). |Obnovení lokality používá hello metadata toopopulate hello informace o virtuálních počítačů v hello portálu Azure. | Tato funkce je nedílnou součást vámi vyžádaných hello služby a nelze po zapnutí vypnout. Pokud nechcete, aby toosend tyto informace, nemáte povolit ochranu Site Recovery pro virtuální počítače. Všimněte si, že všechna data odeslaná hello zprostředkovatele tooSite obnovení je zaslaného prostřednictvím protokolu HTTPS. |
| **Plán obnovení** | Plány obnovení pomoct vám vytvořit plán orchestration pro hello obnovení datového centra. Můžete definovat hello pořadí, ve kterém má být spuštěn virtuální počítače nebo skupiny virtuálních počítačů v lokalitě obnovení hello. Můžete také zadat všechny automatizované skripty toobe spustit nebo toobe jakékoli ruční akce prováděné během hello obnovení pro každý virtuální počítač. Převzetí služeb při selhání se obvykle aktivuje úrovni plán obnovení hello koordinované obnovení. | Site Recovery shromažďuje, procesy a odesílá metadata pro plán obnovení hello, včetně metadata virtuálního počítače a metadata všechny skripty pro automatizaci a poznámky k ruční akce. |Hello metadata se plán obnovení hello použité toobuild v hello portálu Azure. |Tato funkce je nedílnou součást vámi vyžádaných hello služby a nelze po zapnutí vypnout. Pokud nechcete, aby toosend tooSite tato informace o obnovení, nevytvářejte plány obnovení. |
| **Mapování sítě** | Mapy sítě informace z hello primární datové centrum toohello obnovení datového centra. Když virtuální počítače se obnoví na hello obnovení lokality, pomáhá mapování sítě v navazování připojení k síti. |Site Recovery shromažďuje, procesy a odesílá metadata hello hello logických sítí pro každou lokalitu (primární i datacenter). |Hello metadata je použité toopopulate nastavení sítě, takže můžete namapovat informace o síti hello. | Tato funkce je nedílnou součást vámi vyžádaných hello služby a nelze po zapnutí vypnout. Pokud nechcete, aby toosend tooSite tato informace o obnovení, nepoužívejte mapování sítě. |
| **Převzetí služeb při selhání (plánované/neplánované/test)** | Převzetí služeb při selhání převezme virtuální počítače z tooanother center jeden datový spravovat nástroj VMM. Akce Hello převzetí služeb při selhání se aktivuje ručně v hello portálu Azure. |Hello zprostředkovatele na serveru VMM hello upozorněn hello události převzetí služeb při selhání pomocí Site Recovery a spouští akce převzetí služeb při selhání na hostiteli Hyper-V hello prostřednictvím rozhraní VMM. Skutečné převzetí služeb při selhání virtuálního počítače z jednoho tooanother hostitele technologie Hyper-V a řešit Windows Server 2012 nebo Windows Server 2012 R2 Hyper-V repliky. Zádi Site Recovery používá hello informace odesílané toopopulate hello stav informace o akci hello převzetí služeb při selhání v hello portálu Azure. | Tato funkce je nedílnou součást vámi vyžádaných hello služby a nelze po zapnutí vypnout. Pokud nechcete, aby toosend tooSite tato informace o obnovení, nepoužívejte převzetí služeb při selhání. |

## <a name="next-steps"></a>Další kroky

Po nasazení hello otestujete, další informace o dalších typů [převzetí služeb při selhání](site-recovery-failover.md)
