---
title: "virtuální počítače Hyper-V tooAzure aaaReplicate portálu classic hello | Microsoft Docs"
description: "Tento článek popisuje, jak tooreplicate technologie Hyper-V virtuální počítače tooAzure, když počítače nejsou spravované v cloudech VMM."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 3f4c4483-e3dd-495a-bd02-c16e9e28c88d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/21/2017
ms.author: raynew
ms.openlocfilehash: 12d08d950a79e956436cb03ffc87ab40e86c589e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-without-vmm-with-azure-site-recovery"></a>Replikaci mezi místními technologie Hyper-V virtuální počítače a Azure (bez VMM) s Azure Site Recovery
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Portál Classic](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

Tento článek popisuje, jak tooreplicate místní tooAzure virtuální počítače Hyper-V, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure. V tomto scénáři nejsou spravované servery Hyper-V v cloudech VMM.

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="site-recovery-in-hello-azure-portal"></a>Site Recovery na portálu Azure hello

Azure má dva různé [modely nasazení](../resource-manager-deployment-model.md) pro vytváření prostředků a práci s nimi: Azure Resource Manager a Classic. Azure má také dva portály – portál Azure classic hello a hello portálu Azure.

Tento článek popisuje, jak toodeploy portálu classic hello. portál classic Hello lze použít toomaintain existující trezorů. Nelze vytvořit nové trezory pomocí portálu classic hello.

## <a name="site-recovery-in-your-business"></a>Site Recovery v podnikání

Organizace potřebují strategii BCDR, která určuje, jak aplikace a data zůstanou spuštěné a dostupné během plánovaných a neplánovaných výpadků a co nejdříve obnovit toonormal pracovní podmínky. Tady je výčet, co může Site Recovery poskytnout:

* Ochrana mimo pracoviště pro firemní aplikace běžící na virtuálních počítačích Hyper-V
* Jedno umístění tooset, správu a sledování replikace, převzetí služeb při selhání a obnovení.
* TooAzure jednoduché převzetí služeb při selhání a navrácení služeb po obnovení (restore) z hostitelské servery Azure tooHyper-V ve vaší místní lokalitě.
* Plány obnovy, které obsahují několik virtuálních počítačů, aby u vrstvených úloh aplikací docházelo ke společnému převzetí služeb při selhání

## <a name="azure-prerequisites"></a>Požadavky Azure
* Potřebujete účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).
* Je třeba data toostore replikovat účtu úložiště Azure. Hello účet potřebuje povolenou geografickou replikací. By mělo být ve stejné oblasti jako trezor Azure Site Recovery hello hello a přidružen hello stejného předplatného. [Další informace o Azure storage](../storage/common/storage-introduction.md). Všimněte si, že nepodporujeme přesunutí účty úložiště vytvořené pomocí hello [nový portál Azure](../storage/common/storage-create-storage-account.md) mezi skupinami prostředků.
* Budete potřebovat virtuální síť Azure, aby virtuální počítače Azure bude sítě připojené tooa při selhání z primární lokality.

## <a name="hyper-v-prerequisites"></a>Požadavky technologie Hyper-V
* V hello zdrojové místní lokality budete potřebovat jeden nebo více serverů se systémem **Windows Server 2012 R2** s nainstalovanou rolí hello technologie Hyper-V nebo **Microsoft Hyper-V Server 2012 R2**. Tento server proveďte následující kroky:
* Obsahovat jeden nebo více virtuálních počítačů.
* Být připojené toohello Internetu, buď přímo nebo prostřednictvím proxy serveru.
* Používat hello opravy popsané v článku KB [2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977").

## <a name="virtual-machine-prerequisites"></a>Požadavky virtuálního počítače
Virtuální počítače, které chcete tooprotect musí být v souladu s [požadavky na virtuální počítač Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="provider-and-agent-prerequisites"></a>Zprostředkovatel a agent požadavky
Jako součást nasazení Azure Site Recovery budete instalovat hello zprostředkovatele Azure Site Recovery a hello agenta služeb zotavení Azure na každém serveru technologie Hyper-V. Poznámky:

* Doporučujeme vždy spustit hello nejnovější verze hello zprostředkovatel a agent. Tyto jsou k dispozici na portálu Site Recovery hello.
* Všechny servery Hyper-V v trezoru by měl mít hello stejné verze hello zprostředkovatel a agent.
* Hello zprostředkovatel, který běží na serveru hello připojí tooSite obnovení přes hello Internetu. Musíte bez serveru proxy, s nastavením proxy serveru hello aktuálně nakonfigurované na serveru Hyper-V hello nebo s nastavením vlastní proxy server, které můžete konfigurovat během instalace zprostředkovatele. Toomake se, že tento server proxy hello chcete toouse přístup tyto hello adresy URL pro připojení tooAzure, budete potřebovat:

  * *.accesscontrol.windows.net
  * *.backup.windowsazure.com
  * *.hypervrecoverymanager.windowsazure.com
  * *.store.core.windows.net      
  * *.blob.core.windows.net
  - https://www.msftncsi.com/ncsi.txt
  - time.windows.com
  - time.nist.gov
* Kromě toho povolit hello IP adresy popsané v [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653) a protokol HTTPS (443). Máte tooallow hello rozsahů IP hello oblast Azure, abyste naplánovali toouse a západní USA.

Tento obrázek zobrazuje hello různých komunikačních kanálů a porty používané službou Site Recovery pro orchestration a replikace

![B2A topologie](./media/site-recovery-hyper-v-site-to-azure-classic/b2a-topology.png)

## <a name="step-1-create-a-vault"></a>Krok 1: Vytvoření trezoru
1. Přihlaste se toohello [portálu pro správu](https://portal.azure.com).
2. Rozbalte položku **datové služby** > **služeb zotavení** a klikněte na tlačítko **trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. V **název**, zadejte popisný název tooidentify hello trezoru.
5. V **oblast**, hello vyberte zeměpisnou oblast trezoru hello. toocheck podporované oblasti, najdete v části geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klikněte na **Vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-hyper-v-site-to-azure-classic/vault.png)

Zkontrolujte, zda tooconfirm hello stav panelu, který hello trezor byl úspěšně vytvořen. Hello trezor bude uvedený jako **Active** na hlavní stránce služeb zotavení hello.

## <a name="step-2-create-a-hyper-v-site"></a>Krok 2: Vytvoření web Hyper-V
1. Na stránce služeb zotavení hello klikněte na tlačítko hello trezoru tooopen hello stránky rychlý Start. Rychlý Start můžete otevřít také kdykoli pomocí ikony hello.

    ![Rychlý start](./media/site-recovery-hyper-v-site-to-azure-classic/quick-start-icon.png)
2. V rozevíracím seznamu hello vyberte **mezi místními servery technologie Hyper-V a Azure**.

    ![Scénář web Hyper-V](./media/site-recovery-hyper-v-site-to-azure-classic/select-scenario.png)
3. V **vytvořit web Hyper-V** klikněte na tlačítko **web vytvořit Hyper-V**. Zadejte název lokality a uložte.

    ![Web Hyper-V](./media/site-recovery-hyper-v-site-to-azure-classic/create-site.png)

## <a name="step-3-install-hello-provider-and-agent"></a>Krok 3: Instalace hello zprostředkovatel a agent
Nainstalujte hello zprostředkovatel a agent na každém serveru technologie Hyper-V, který chcete tooprotect virtuální počítače.

Pokud instalujete na clusteru s podporou technologie Hyper-V, provede kroky 5-11 na každém uzlu v clusteru převzetí služeb při selhání hello. Po všechny uzly jsou zaregistrované a povolit ochranu, budou virtuální počítače chráněné i v případě, že migraci mezi uzly v clusteru hello.

1. V **Příprava technologie Hyper-V serverů**, klikněte na tlačítko **stáhněte si registrační klíč** souboru.
2. Na hello **stáhnout registrační klíč** klikněte na tlačítko **Stáhnout** další toohello lokality. Stáhněte si klíče tooa hello bezpečné se místo, kam můžete snadno přistupovat serverem hello technologie Hyper-V. Hello klíč je platný po dobu 5 dní, po jeho vygenerování.

    ![Registrační klíč](./media/site-recovery-hyper-v-site-to-azure-classic/download-key.png)
3. Klikněte na tlačítko **stažení hello zprostředkovatele** tooobtain hello nejnovější verzi.
4. Spusťte soubor hello na každém serveru technologie Hyper-V chcete tooregister v trezoru hello. soubor Hello nainstaluje dvě součásti:
   * **Zprostředkovatele Azure Site Recovery**– zpracovává komunikace a orchestraci mezi serverem hello technologie Hyper-V a portálu Azure Site Recovery hello.
   * **Azure agenta služeb zotavení**– zpracovává přenos dat mezi virtuálními počítači běžícími na server hello zdroj technologie Hyper-V a úložiště Azure.
5. V nastavení služby **Microsoft Update** můžete vyjádřit výslovný souhlas se stahováním a instalací aktualizací. Toto nastavení povoleno, budou nainstalovány aktualizace zprostředkovatel a Agent podle zásad tooyour Microsoft Update.

    ![Aktualizace Microsoft Update](./media/site-recovery-hyper-v-site-to-azure-classic/provider1.png)
6. V **instalace** určíte, kam chcete tooinstall hello zprostředkovatele a hello agenta na server Hyper-V.

    ![Umístění instalace](./media/site-recovery-hyper-v-site-to-azure-classic/provider2.png)
7. Po dokončení instalace pokračovat instalace tooregister hello server v trezoru hello.
8. Na hello **nastavení trezoru** klikněte na tlačítko **Procházet** soubor klíče tooselect hello. Zadejte předplatné Azure Site Recovery hello, hello název trezoru, a patří hello technologie Hyper-V serveru toowhich hello technologie Hyper-V.

    ![Registrace serveru](./media/site-recovery-hyper-v-site-to-azure-classic/provider8.PNG)
9. Na hello **připojení k Internetu** určit způsob hello poskytovatele připojení tooAzure Site Recovery. Vyberte **použít výchozí nastavení proxy serveru systému** toouse hello výchozí nastavení připojení k Internetu nakonfigurované na serveru hello. Pokud nezadáte nastavení budou použita výchozí hodnota hello.

   ![Nastavení internetu](./media/site-recovery-hyper-v-site-to-azure-classic/provider7.PNG)
10. Spustí se registrace tooregister hello server v trezoru hello.

    ![Registrace serveru](./media/site-recovery-hyper-v-site-to-azure-classic/provider15.PNG)
11. Po dokončení registrace metadata z hello technologie Hyper-V server načte Azure Site Recovery a hello server se zobrazí na hello **lokalit Hyper-V** karty v hello **servery** stránky v trezoru hello.

### <a name="install-hello-provider-from-hello-command-line"></a>Instalace z příkazového řádku hello hello zprostředkovatele
Jako alternativu můžete nainstalovat z příkazového řádku hello hello zprostředkovatele Azure Site Recovery. Tato metoda by měla použít, pokud chcete, aby tooinstall hello zprostředkovatele v počítači se systémem Windows Server Core 2012 R2. Spusťte z příkazového řádku hello následujícím způsobem:

1. Hello zprostředkovatele instalace souboru a registraci klíče tooa složka pro stahování. Například C:\ASR.
2. Spusťte příkazový řádek jako správce a zadejte:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Pak spuštěním nainstalujte hello zprostředkovatele:

        C:\ASR> setupdr.exe /i
4. Spusťte následující toocomplete registrace hello:

        CD C:\Program Files\Microsoft Azure Site Recovery Provider
        C:\Program Files\Microsoft Azure Site Recovery Provider\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>         

Kde parametry patří:

* **/ Credentials**: Zadejte umístění hello hello registrační klíč jste si stáhli.
* **/ FriendlyName**: Zadejte název tooidentify hello technologie Hyper-V hostitelském serveru. Tento název se zobrazí na portálu hello
* **/ EncryptionEnabled**: volitelné. Určete, jestli má tooencrypt repliky virtuálního počítače v Azure (šifrování neaktivních dat).
* **/ proxyaddress**; **/proxyport**; **/proxyusername**; **/proxypassword**: volitelné. Zadejte parametry serveru proxy, pokud chcete toouse vlastní proxy server nebo váš stávající proxy server vyžaduje ověření.

## <a name="step-4-create-an-azure-storage-account"></a>Krok 4: Vytvoření účtu úložiště Azure
* V **Příprava prostředků** vyberte **vytvořit účet úložiště** toocreate účet úložiště Azure, pokud nemáte. Hello účet by měl mít povolenou geografickou replikací. Je nutné ve stejné oblasti jako trezor Azure Site Recovery hello hello a přidružen hello stejného předplatného.

    ![Vytvořit účet úložiště](./media/site-recovery-hyper-v-site-to-azure-classic/create-resources.png)

> [!NOTE]
> 1. Nepodporujeme hello přesun účty úložiště vytvořené pomocí hello [nový portál Azure](../storage/common/storage-create-storage-account.md) mezi skupinami prostředků.
> 2. [Migrace účtů úložiště](../azure-resource-manager/resource-group-move-resources.md) napříč prostředků skupiny v hello stejného předplatného, nebo ve předplatných není podporována pro účty úložiště používá pro nasazení Site Recovery.
>

## <a name="step-5-create-and-configure-protection-groups"></a>Krok 5: Vytvoření a konfigurace skupin ochrany
Skupiny ochrany jsou logickými seskupeními virtuálních počítačů, které chcete pomocí tooprotect hello stejné nastavení ochrany. Použijte skupiny ochrany tooa nastavení ochrany, a tato nastavení jsou použité tooall virtuální počítače přidat toohello skupiny.

1. V **vytvořit a nakonfigurovat skupiny ochrany** klikněte na tlačítko **vytvořte skupinu ochrany**. Pokud všechny požadované součásti nejsou v místní zpráva se objeví, a můžete kliknout na **zobrazit podrobnosti** Další informace.
2. V hello **skupin ochrany** přidejte skupinu ochrany. Zadejte název, hello zdrojové technologie Hyper-V lokalitě, cílový hello **Azure**, vaše jméno předplatné Azure Site Recovery a hello účtu úložiště Azure.

    ![Skupiny ochrany](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group.png)
3. V **nastavení replikace** sadu hello **frekvence kopírování** toospecify jak často by měl hello rozdílová data synchronizovat mezi hello zdrojem a cílem. Můžete nastavit too30 sekund, 5 minut nebo 15 minut.
4. V **zachovat body obnovení** zadejte počet hodin historie obnovení by měly být uložené.
5. V **frekvence snímků konzistentní vzhledem k aplikacím** můžete určit, zda tootake snímky, které používají tooensure služby Stínová kopie svazku (VSS), které aplikace budou při pořízení snímku hello v konzistentním stavu. Ve výchozím nastavení se tyto nezavedou. Ujistěte se, že tato hodnota se nastavuje tooless než hello počet dalších bodů obnovení, které nakonfigurujete. Je podporováno pouze pokud hello virtuální počítač běží operační systém Windows.
6. V **čas spuštění počáteční replikace** zadejte při počáteční replikace virtuálních počítačů ve skupině ochrany hello by měly být odeslány tooAzure.

    ![Skupiny ochrany](./media/site-recovery-hyper-v-site-to-azure-classic/protection-group2.png)

## <a name="step-6-enable-virtual-machine-protection"></a>Krok 6: Povolení ochrany virtuálního počítače
Přidáte virtuální počítače tooenable ochranu skupiny ochrany tooa pro ně.

> [!NOTE]
> Ochrana virtuálních počítačů s Linuxem a statickou IP adresou není podporována.
>
>

1. Na hello **počítače** hello skupiny ochrany, klikněte na tlačítko ** Přidat virtuální počítače tooprotection skupiny ochrany tooenable **.
2. Na hello **povolit ochranu virtuálního počítače** stránce vyberte hello virtuálních počítačů má tooprotect.

    ![Povolení ochrany virtuálního počítače](./media/site-recovery-hyper-v-site-to-azure-classic/add-vm.png)

    spustí úlohy Hello povolení ochrany. Průběh můžete sledovat na hello **úlohy** kartě. Po úloze dokončení ochrany hello spustí hello virtuální počítač je připraven na převzetí služeb při selhání.
3. Po ochrany je nastavení můžete provádět následující akce:

   * Zobrazení virtuálních počítačů v **chráněné položky** > **skupin ochrany** > *protectiongroup_name*  >  **Virtuální počítače** podrobnostem toomachine podrobnosti v hello **vlastnosti** kartě...
   * Konfigurovat vlastnosti hello převzetí služeb při selhání pro virtuální počítače v **chráněné položky** > **skupin ochrany** > *protectiongroup_name*  >  **Virtuální počítače** *virtual_machine_name* > **konfigurace**. Můžete nakonfigurovat:

     * **Název**: název hello hello virtuálního počítače v Azure.
     * **Velikost**: hello cílovou velikost hello virtuálního počítače, který převezme.

       ![Konfigurovat vlastnosti virtuálního počítače](./media/site-recovery-hyper-v-site-to-azure-classic/vm-properties.png)
   * Nakonfigurujte další virtuální počítač v *chráněné položky** > **skupin ochrany** > *protectiongroup_name* > **virtuální počítače** *virtual_machine_name* > **konfigurace**, včetně:

     * **Síťové adaptéry**: hello počet síťových adaptérů je závisí na velikosti hello, kterou zadáte pro hello cílového virtuálního počítače. Zkontrolujte [specifikace velikosti virtuálního počítače](../virtual-machines/linux/sizes.md) pro hello počet síťových adaptérů nepodporuje hello velikost virtuálního počítače.

       Když změníte hello velikost virtuálního počítače a uložit nastavení hello, hello počet síťových adaptérů se změní při otevření **konfigurace** stránku hello příště. Hello počet síťových adaptérů cílových virtuálních počítačů je minimální hello počet síťových adaptérů na zdrojovém virtuálním počítači a maximální počet síťových adaptérů nepodporuje hello velikost virtuálního počítače hello vybrali. Jsou vysvětleny níže:

       * Pokud hello počet síťových adaptérů na zdrojovém počítači hello je menší než nebo rovna toohello počet adaptérů povoleno pro velikost cílového počítače hello pak bude mít cíl hello hello stejný počet adaptérů jako zdroj hello.
       * Pokud hello počet adaptérů pro hello zdrojový virtuální počítač překračuje počet hello povolený pro cílovou velikost hello pak maximální velikost cíle hello se použije.
       * Například pokud má zdrojový počítač dva síťové adaptéry a velikost hello cílového počítače podporuje čtyři, bude mít hello cílový počítač dva adaptéry. Pokud má hello zdrojový počítač dva adaptéry, ale hello podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač hello jenom jeden adaptér.

     * **Síť Azure**: Zadejte hello sítě toowhich hello virtuální počítač by měl převzetí služeb při selhání. Pokud hello virtuální počítač má několik síťových adaptérů všechny adaptéry měli připojené toohello stejné síti Azure.
     * **Podsíť** pro každý síťový adaptér na virtuálním počítači hello by se měly připojit vyberte hello podsítě v hello síť Azure toowhich hello počítače po převzetí služeb při selhání.
     * **Cílová IP adresa**: Pokud je nakonfigurované toouse statické hello síťový adaptér zdrojového virtuálního počítače IP adres, potom můžete zadat hello IP adresu pro tooensure hello cílový virtuální počítač, který hello počítač má hello stejnou IP adresu po převzetí služeb při selhání .  Pokud nezadáte IP adresu budou všechny dostupnou adresu přiřadit hello během převzetí služeb při selhání. Pokud zadáte adresu, která se používá, převzetí služeb při selhání se nezdaří.

     > [!NOTE]
     > [Migrace sítí](../azure-resource-manager/resource-group-move-resources.md) napříč prostředků skupiny v hello stejného předplatného, nebo ve předplatných není podporována pro sítě používá pro nasazení Site Recovery.
     >

     ![Konfigurovat vlastnosti virtuálního počítače](./media/site-recovery-hyper-v-site-to-azure-classic/multiple-nic.png)




## <a name="step-7-create-a-recovery-plan"></a>Krok 7: Vytvoření plánu obnovení
V pořadí tootest hello nasazení můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo plán obnovení, který obsahuje jeden nebo více virtuálních počítačů. [Další informace](site-recovery-create-recovery-plans.md) o vytvoření plánu obnovení.

## <a name="step-8-test-hello-deployment"></a>Krok 8: Testování nasazení hello
Existují dva způsoby toorun tooAzure testovací převzetí služeb při selhání.

* **Testovací převzetí služeb při selhání bez sítě Azure**– tento typ testovacího převzetí služeb při selhání ověří, že hello virtuální počítač se zobrazí správně v Azure. Hello virtuálního počítače nebudou připojené tooany síť Azure po převzetí služeb při selhání.
* **Testovací převzetí služeb při selhání se sítí Azure**– tento typ převzetí služeb při selhání kontroly, které hello celé prostředí replikace dodává až podle očekávání a který převzal hello virtuální počítače se připojuje toohello zadané cílové síti Azure. Všimněte si, že pro testovací převzetí služeb při selhání podsíť hello hello testovacího virtuálního počítače bude být započítáno na základě podsítě hello hello repliky virtuálního počítače. Toto je replikace různých tooregular-li hello podsíť virtuálního počítače repliky založena na podsíti hello hello zdrojového virtuálního počítače.

Pokud chcete, aby toorun testovací převzetí služeb bez zadání síť Azure nepotřebujete tooprepare nic.

toorun převzetí služeb při selhání s cílovou sítí Azure, budete potřebovat toocreate novou síť Azure, která bude izolovaná od produkční sítě Azure (výchozí chování při vytváření nových sítí v Azure). Čtení [spustit testovací převzetí služeb](site-recovery-failover.md) další podrobnosti.

toofully otestujte nasazení replikace a sítě tooset až hello infrastruktury budete potřebovat, takže tento hello replikované virtuální počítač toowork podle očekávání. Jeden způsob provádění této tootooset virtuální počítač jako řadič domény s DNS a jej replikovat pomocí Site Recovery toocreate v hello testovací sítě tak, že spustíte testovací převzetí služeb tooAzure.  [Další informace](site-recovery-active-directory.md#test-failover-considerations) aspekty testovací převzetí služeb při selhání pro Active Directory.

Hello testovací převzetí služeb při selhání spusťte následujícím způsobem:

> [!NOTE]
> tooget hello nejlepší výkon při tooAzure převzetí služeb při selhání Ujistěte se, jestli máte nainstalovanou hello Azure Agent hello chráněný počítač. Tato funkce umožňuje rychlejší spouštění a také pomáhá s diagnostikou v případě problémů. Agenta pro Linux najdete [tady](https://github.com/Azure/WALinuxAgent) a agenta pro Windows [tady](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

1. Na hello **plány obnovení** vyberte hello plán a klikněte na tlačítko **testovací převzetí služeb při selhání**.
2. Na hello **potvrzení převzetí služeb při selhání** vyberte **žádné** nebo konkrétní síť Azure.  Všimněte si, že pokud vyberete **žádné** hello testovací převzetí služeb při selhání zkontroluje, že hello virtuální počítač replikoval správně tooAzure ale neohlásí konfiguraci sítě replikace.

    ![Testovací převzetí služeb při selhání](./media/site-recovery-hyper-v-site-to-azure-classic/test-nonetwork.png)
3. Na hello **úlohy** můžete sledovat průběh převzetí služeb při selhání. Měli byste také mít testovací repliku virtuálního počítače může toosee hello v hello portálu Azure. Pokud jste nastavili tooaccess virtuální počítače z vaší místní sítě můžete spustit virtuální počítač toohello připojení vzdálené plochy.
4. Když hello převzetí služeb při selhání dosáhne hello **dokončit testování** fáze, klikněte na tlačítko **dokončit Test** toofinish až hello testovací převzetí služeb při selhání. Podrobnostem toohello **úlohy** kartě tootrack převzetí služeb při selhání průběh a stav a tooperform všechny akce, které jsou potřeba.
5. Po převzetí služeb při selhání budete testovací repliku virtuálního počítače může toosee hello v hello portálu Azure. Pokud jste nastavili tooaccess virtuální počítače z vaší místní sítě můžete spustit virtuální počítač toohello připojení vzdálené plochy.

   1. Ověřte, že hello virtuální počítače úspěšně spustí.
   2. Pokud chcete, aby tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy po převzetí služeb při selhání hello, povolte připojení ke vzdálené ploše na virtuálním počítači hello před spuštěním hello testovací převzetí služeb při selhání. Budete také potřebovat tooadd koncový bod RDP na virtuální počítač hello. Můžete využít [služby Azure automation runbook](site-recovery-runbook-automation.md) toodo který.
   3. Po převzetí služeb při selhání, pokud používáte veřejnou IP adresu tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy, zajistěte nemáte žádné zásady domény, které by vám bránily v připojení tooa virtuálnímu počítači pomocí veřejné adresy.
6. Po dokončení testování hello hello následující:

   * Klikněte na tlačítko **hello testovací převzetí služeb při selhání je dokončena**. Vyčištění hello testovací prostředí tooautomatically power vypnout a odstraňte hello testovací virtuální počítače.
   * Klikněte na tlačítko **poznámky** toorecord a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání.
7. Když hello převzetí služeb při selhání dosáhne hello **dokončit testování** fáze dokončit ověření hello následujícím způsobem:
   1. Zobrazení hello virtuální počítač repliky v hello portálu Azure. Ověřte, zda že text hello virtuálního počítače úspěšně spustí.
   2. Pokud jste nastavili tooaccess virtuální počítače z vaší místní sítě můžete spustit virtuální počítač toohello připojení vzdálené plochy.
   3. Klikněte na tlačítko **dokončení hello test** toofinish ho.
   4. Klikněte na tlačítko **poznámky** toorecord a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání.
   5. Klikněte na tlačítko **hello testovací převzetí služeb při selhání je dokončena**. Vyčištění hello testovací prostředí tooautomatically power vypnout a odstraňte hello testovacího virtuálního počítače.

## <a name="next-steps"></a>Další kroky
Po nastavení a zprovoznění nasazení si můžete přečíst [další informace](site-recovery-failover.md) o převzetí služeb při selhání.
