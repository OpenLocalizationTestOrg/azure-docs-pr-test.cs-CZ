---
title: "aaaReplicate technologie Hyper-V virtuální počítače v tooAzure cloudy VMM | Microsoft Docs"
description: "Tento článek popisuje, jak tooreplicate technologie Hyper-V virtuální počítače na hostitele Hyper-V umístěný v tooAzure cloudech System Center VMM."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 9d526a1f-0d8e-46ec-83eb-7ea762271db5
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: 136f68585534c17b615ecc4e82c0133bf813503f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure"></a>Replikace virtuálních počítačů technologie Hyper-V v tooAzure cloudy VMM
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Portál Classic](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Classic](site-recovery-deploy-with-powershell.md)
>
>

Hello služba Azure Site Recovery přispívá tooyour provozní kontinuitu a po havárii (BCDR) strategie Orchestrace replikace, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Počítače může být replikované tooAzure nebo tooa sekundárního místního datového centra. Rychlý přehled najdete v článku [Co je Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Přehled
Tento článek popisuje, jak toodeploy Site Recovery tooreplicate technologie Hyper-V virtuální počítače v Hyper-V hostitelské servery, které se nacházejí v tooAzure privátních cloudech VMM.

Hello článek obsahuje požadavky pro hello scénář a ukazuje, jak tooset do trezoru Site Recovery, získat hello Azure Site Recovery Provider nainstalovaný na zdrojovém serveru VMM hello, zaregistrujte hello server v trezoru hello, přidáte účet úložiště Azure, nainstalujte hello Agent služeb zotavení Azure na hostitelských serverech technologie Hyper-V, nakonfigurovat nastavení ochrany pro cloudy VMM, které budou použité tooall chráněné virtuální počítače a pak povolte ochranu pro tyto virtuální počítače. Nakonec testování převzetí služeb při selhání hello toomake se, že vše funguje podle očekávání.

POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Architektura
![Architektura](./media/site-recovery-vmm-to-azure-classic/topology.png)

* Hello zprostředkovatele Azure Site Recovery je nainstalován na hello VMM během nasazování Site Recovery a hello VMM server je zaregistrován v trezoru Site Recovery hello. komunikuje se službou Site Recovery orchestraci replikace toohandle Hello zprostředkovatele.
* agent služeb zotavení Azure Hello je nainstalován na hostitelských serverech technologie Hyper-V během nasazování Site Recovery. Zpracovává data replikace tooAzure úložiště.

## <a name="azure-prerequisites"></a>Požadavky Azure
Tady je seznam toho, co budete potřebovat v Azure.

| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **Účet Azure** |Budete potřebovat účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery |
| **Úložiště Azure** |Budete potřebovat data toostore replikovat účtu úložiště Azure. Replikovaná data jsou uložena v úložišti Azure a virtuální počítače Azure se spouštějí, když je třeba převzít služby při selhání. <br/><br/>Potřebujete [účet se standardním geograficky redundantním úložištěm](../storage/common/storage-redundancy.md#geo-redundant-storage). Hello účet musí být ve stejné oblasti jako služba Site Recovery hello hello a přidružen hello stejného předplatného. Upozorňujeme, že účty úložiště toopremium replikace není aktuálně podporována a by se neměla používat.<br/><br/>[Přečtěte si informace o](../storage/common/storage-introduction.md) úložišti Azure. |
| **Síť Azure** |Budete potřebovat virtuální síť Azure, virtuální počítače Azure připojí toowhen převzetí služeb při selhání. Hello virtuální síť Azure musí být v hello stejné oblasti jako trezor Site Recovery hello. |

## <a name="on-premises-prerequisites"></a>Místní požadavky
Tady je seznam toho, co budete potřebovat místně.

| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **VMM** |Budete potřebovat minimálně jeden server VMM nasazený jako fyzický nebo virtuální samostatný server nebo jako virtuální cluster. <br/><br/>Hello VMM server by měl běžet System Center 2012 R2 s nejnovější kumulativní aktualizace hello.<br/><br/>Budete potřebovat alespoň jeden cloud nakonfigurované na serveru VMM hello.<br/><br/>Hello zdrojový cloud, které chcete tooprotect musí obsahovat jeden nebo více skupin hostitelů VMM.<br/><br/>Další informace o nastavení cloudů najdete na blogu Keithema Mayera v příspěvku [Návod: vytvoření privátních cloudů pomocí produktu System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx). |
| **Hyper-V** |Budete potřebovat jeden nebo více hostitelských serverů Hyper-V nebo clusterů v cloudu VMM hello. Hello hostitelský server by měl mít a jeden nebo více virtuálních počítačů. <br/><br/>server Hello technologie Hyper-V musí být spuštěn alespoň **Windows Server 2012 R2** s rolí hello technologie Hyper-V nebo **Microsoft Hyper-V Server 2012 R2** a mít hello nainstalované nejnovější aktualizace.<br/><br/>Jakýkoli server Hyper-V obsahující virtuální počítače, budete chtít tooprotect se musí nacházet v cloudu VMM.<br/><br/>Pokud používáte technologii Hyper-V v clusteru, je důležité vědět, že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte clustery založené na statických IP adresách. Zprostředkovatel clusteru hello tooconfigure budete potřebovat ručně. [Další informace](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) v najdete blogu Aidana Finna. |
| **Chráněné počítače** | Virtuální počítače chcete tooprotect, by měly splňovat [požadavky pro Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). |

## <a name="network-mapping-prerequisites"></a>Požadavky na mapování sítě
Při ochraně virtuálních počítačů v síti Azure mapování mapuje sítě virtuálních počítačů na zdrojovém serveru VMM hello a cílové sítě Azure tooenable hello následující:

* Všechny počítače, které se Překlopí na hello stejné tooeach jiných, bez ohledu na plánu obnovení se mohou připojit.
* Pokud na hello cílové síti Azure nastavená brána sítě, virtuální počítače můžete připojit tooother místní virtuální počítače.
* Pokud nenakonfigurujete sítě mapování jenom virtuální počítače, které převzetí služeb při selhání v hello stejný plán obnovení bude možné tooconnect tooeach jiných po tooAzure převzetí služeb při selhání.

Pokud chcete, aby mapování sítě toodeploy budete potřebovat následující hello:

* Hello virtuálních počítačů má tooprotect na zdrojovém serveru VMM hello by měl být připojený tooa síť virtuálních počítačů. Tuto síť by měla být propojené tooa logické sítě, který je přidružený k hello cloudu.
* Po převzetí služeb při selhání můžete připojit síti Azure toowhich replikovat virtuální počítače. Tuto síť vyberete v době hello převzetí služeb při selhání. Hello síť musí být ve hello stejné oblasti jako vaše předplatné Azure Site Recovery.


Připravte sítě v nástroji VMM:

   * [Nastavení logických sítí](https://technet.microsoft.com/library/jj721568.aspx).
   * [Nastavení sítí virtuálních počítačů](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Krok 1: Vytvoření trezoru Site Recovery
1. Přihlaste se toohello [portálu pro správu](https://portal.azure.com) ze serveru VMM hello chcete tooregister.
2. Klikněte na **Data Services** > **Služby zotavení** > **Trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. V **název**, zadejte popisný název tooidentify hello trezoru.
5. V **oblast**, hello vyberte zeměpisnou oblast trezoru hello. toocheck podporované oblasti, najdete v části geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klikněte na **Vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Zkontrolujte, zda tooconfirm hello stav panelu, který hello trezor byl úspěšně vytvořen. Hello trezor bude uvedený jako **Active** na hlavní stránce služeb zotavení hello.

## <a name="step-2-generate-a-vault-registration-key"></a>Krok 2: Vygenerování registračního klíče trezoru
Vygenerujte registrační klíč v trezoru hello. Po stažení hello zprostředkovatele Azure Site Recovery a nainstalujte ji na serveru VMM hello, použijete tento klíč tooregister hello VMM server v trezoru hello.

1. V hello **služeb zotavení** klikněte na tlačítko hello trezoru tooopen hello stránky rychlý Start. Rychlý Start můžete otevřít také kdykoli pomocí ikony hello.

    ![Ikona Rychlý start](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)
2. V rozevíracím seznamu hello vyberte **mezi místními servery VMM a Microsoft Azure**.
3. V části **Připravit servery VMM** klikněte na **Vygenerovat soubor registračního klíče**. soubor klíče Hello se vygeneruje automaticky a je platný po dobu 5 dní, po jeho vygenerování. Pokud nejsou k hello portálu Azure ze serveru VMM hello budete potřebovat toocopy tento souborový server toohello.

    ![Registrační klíč](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-hello-azure-site-recovery-provider"></a>Krok 3: Instalace hello zprostředkovatele Azure Site Recovery
1. V **rychlý Start** > **připravit servery VMM**, klikněte na tlačítko **stáhnout zprostředkovatele Microsoft Azure Site Recovery pro instalaci na serverech VMM** tooobtain hello nejnovější verzi instalačního souboru zprostředkovatele hello.
2. Spusťte tento soubor na zdrojovém serveru VMM hello.

   > [!NOTE]
   > Pokud VMM je nasazen v clusteru a instalujete hello zprostředkovatele pro hello první instalaci na aktivním uzlu a dokončit hello instalace tooregister hello VMM server v trezoru hello. Nainstalujte hello zprostředkovatele na hello dalších uzlů. Všimněte si, že pokud upgradujete hello zprostředkovatele, budete potřebovat tooupgrade na všech uzlech, protože všechny by měly být spuštěná hello tutéž verzi zprostředkovatele.
   >
   >
3. Hello instalační program provádí kontrolu provádí a vyžádá oprávnění toostop hello VMM toobegin instalace produktu service Provider. Hello služby VMM se restartuje automaticky po dokončení instalace. Pokud instalujete na clusteru VMM, které budete výzva toostop hello Clusterové role.
4. V nastavení služby **Microsoft Update** můžete vyjádřit výslovný souhlas se stahováním a instalací aktualizací. Toto nastavení povoleno zprostředkovatele aktualizace se nainstalují podle zásad tooyour Microsoft Update.

    ![Aktualizace Microsoft Update](./media/site-recovery-vmm-to-azure-classic/updates.png)
5. umístění instalace pro zprostředkovatele je nastaven příliš hello Hello**<SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin**. Klikněte na **Nainstalovat**.

   ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)
6. Po hello je nainstalován poskytovatel klikněte na tlačítko **zaregistrovat** tooregister hello server v trezoru hello.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)
7. V **název trezoru**, ověřte název hello hello úložiště, ve které hello bude server zaregistrovaný. Klikněte na *Další*.

    ![Registrace serveru](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)
8. V **připojení k Internetu** zadat jak hello zprostředkovatel, který běží na hello VMM server připojuje toohello Internetu. Vyberte **připojit se s existujícím nastavením proxy serveru** toouse hello výchozí nastavení připojení k Internetu nakonfigurované na serveru hello.

    ![Nastavení internetu](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

   * Pokud chcete, aby toouse vlastní proxy server můžete musí ho nastavit před instalací hello zprostředkovatele. Při konfiguraci nastavení vlastního proxy serveru se test spustí toocheck hello proxy připojení.
   * Pokud používáte vlastní proxy server nebo váš výchozí proxy server vyžaduje ověření, budete potřebovat tooenter hello podrobnosti o proxy serveru, včetně hello adresy a portu.
   * Následující adresy URL musí být přístupný z hello serveru VMM a hostitelé Hyper-v hello
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Povolit hello IP adresy popsané v [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a protokol HTTPS (443). By měla mít toowhite seznam rozsahů IP hello oblast Azure, abyste naplánovali toouse a západní USA.
   * Pokud používáte vlastní proxy server, účet VMM RunAs (DRAProxyAccount) bude vytvořen automaticky pomocí hello zadané přihlašovací údaje pro proxy. Hello proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. nastavení účtu VMM RunAs Hello lze upravit v konzole VMM hello. toodo se otevřete hello **nastavení** pracovního prostoru, rozbalte položku **zabezpečení**, klikněte na tlačítko **účty spustit jako**a potom upravte hello heslo pro DRAProxyAccount. Služba VMM hello toorestart budete potřebovat, aby toto nastavení projevilo.
9. V **registrační klíč**, vyberte klíč hello stáhli z Azure Site Recovery a zkopírovali toohello serveru VMM.
10. nastavení šifrování Hello se používá pouze při replikaci virtuálních počítačů Hyper-V v tooAzure cloudy VMM. Pokud replikujete tooa sekundární lokality nepoužívá.
11. V **název serveru**, zadejte popisný název tooidentify hello VMM server v trezoru hello. V konfiguraci clusteru zadejte název role clusteru VMM hello.
12. V **synchronizovat metadata cloudu** vyberte, zda má toosynchronize metadata pro všechny cloudy na serveru VMM hello s úložištěm hello. Tuto akci stačí toohappen jednou na každém serveru. Pokud nechcete, aby toosynchronize všechny cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech hello cloudu v konzole VMM hello.
13. Klikněte na tlačítko **Další** toocomplete hello procesu. Po registraci načte Azure Site Recovery metadata ze serveru VMM hello. Hello server se zobrazí na hello **servery VMM** karty v hello **servery** stránky v trezoru hello.

    ![Poslední stránka](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Po registraci načte Azure Site Recovery metadata ze serveru VMM hello. Hello server se zobrazí na hello **servery VMM** karty v hello **servery** stránky v trezoru hello.

### <a name="command-line-installation"></a>Instalace pomocí příkazového řádku
Hello zprostředkovatele Azure Site Recovery můžete nainstalovat také pomocí hello následující příkazový řádek. Tato metoda může být použité tooinstall hello zprostředkovatele na serveru jádra pro Windows Server 2012 R2.

1. Hello zprostředkovatele instalace souboru a registraci klíče tooa složka pro stahování. Příklad: C:\ASR.
2. Zastavit službu System Center Virtual Machine Manager hello
3. Z příkazového řádku se zvýšenými oprávněními vyextrahujte instalační program zprostředkovatele hello pomocí těchto příkazů:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Nainstalujte zprostředkovatele hello následujícím způsobem:

        C:\ASR> setupdr.exe /i
5. Zaregistrujte hello zprostředkovatele následujícím způsobem:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>       

Kde parametry mají následující význam:

* **/ Credentials** : povinný parametr, který určuje hello umístění, ve které hello se nachází soubor registračního klíče  
* **/ FriendlyName** : povinný parametr pro hello název hostitele serveru hello technologie Hyper-V, který se zobrazí na portálu Azure Site Recovery hello.
* **/ EncryptionEnabled** : toospecify volitelný parametr, pokud chcete tooencryption vaše virtuální počítače v Azure (šifrování neaktivních dat). Název souboru Hello musí mít **.pfx** rozšíření.
* **/ proxyaddress** : volitelný parametr, který určuje adresu hello hello proxy serveru.
* **/ proxyport** : volitelný parametr, který určuje port hello hello proxy serveru.
* **/ proxyusername** : volitelný parametr, který určuje uživatelské jméno proxy hello.
* **/ proxypassword** : volitelný parametr, který určuje heslo pro proxy server hello.  

## <a name="step-4-create-an-azure-storage-account"></a>Krok 4: Vytvoření účtu úložiště Azure
1. Pokud nemáte účet úložiště Azure, klikněte na tlačítko **přidat účet úložiště Azure** toocreate účet.
2. Vytvořte účet s povolenou geografickou replikací. Musí v hello stejné oblasti jako hello služba Azure Site Recovery a být přidružen k hello stejného předplatného.

    ![Účet úložiště](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [!NOTE]
> [Migrace účtů úložiště](../azure-resource-manager/resource-group-move-resources.md) napříč prostředků skupiny v hello stejného předplatného, nebo ve předplatných není podporována pro účty úložiště používá pro nasazení Site Recovery.
>
>

## <a name="step-5-install-hello-azure-recovery-services-agent"></a>Krok 5: Instalace hello agenta služeb zotavení Azure
Nainstalujte agenta služeb zotavení Azure hello na každém serveru hostitele technologie Hyper-V v cloudu VMM hello.

1. Klikněte na tlačítko **rychlý Start** > **stáhnout lokality agenta služeb zotavení Azure a nainstalovat ho na hostitele** tooobtain hello nejnovější verzi instalačního souboru agenta hello.

    ![Instalace agenta Služeb zotavení Azure](./media/site-recovery-vmm-to-azure-classic/install-agent.png)
2. Spusťte instalační soubor hello na každém serveru hostitele technologie Hyper-V.
3. Na hello **Kontrola předpokladů** klikněte na stránce **Další**. Automaticky se nainstalují všechny chybějící požadované součásti.

    ![Požadavky agenta Služeb zotavení](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)
4. Na hello **nastavení instalace** určete, kam chcete tooinstall hello agenta a vyberte hello mezipaměti umístění, ve kterém se nainstalují metadata záloh. Pak klikněte na **Nainstalovat**.
5. Po dokončení instalace klikněte na **Zavřít** toocomplete hello průvodce.

    ![Registrace agenta MARS](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Instalace pomocí příkazového řádku
Hello agenta služeb zotavení Microsoft Azure můžete také nainstalovat z příkazového řádku hello použití tohoto příkazu:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Krok 6: Nakonfigurování nastavení ochrany cloudu
Po registraci serveru VMM hello můžete nakonfigurovat nastavení ochrany cloudu. Jste povolili možnost hello **synchronizovat data cloudu s trezorem hello** při instalaci hello poskytovatele, zobrazí se všechny cloudy na serveru VMM hello v hello <b>chráněné položky</b> kartě v trezoru hello.

![Publikovaný cloud](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Na stránce hello rychlý Start, klikněte na **nastavení ochrany pro cloudy VMM**.
2. Na hello **chráněné položky** klikněte na hello cloud chcete tooconfigure a přejděte toohello **konfigurace** kartě.
3. V seznamu **Cíl** vyberte **Azure**.
4. V **účet úložiště** vyberte účet úložiště Azure hello použít pro replikaci.
5. Nastavit **šifrovat uložená data** příliš**vypnout**. Toto nastavení určuje, že data měla šifrovat během replikace mezi hello místními servery a Azure.
6. V **frekvence kopírování** nechejte výchozí nastavení hello. Tato hodnota určuje, jak často se mají synchronizovat data mezi zdrojovým a cílovým umístěním.
7. V **zachovat body obnovení pro**, nechejte výchozí nastavení hello. Výchozí hodnotu nula je na hostitelském serveru repliky uložený jenom hello nejnovější bod obnovení pro primární virtuální počítač.
8. V **frekvence snímků konzistentní vzhledem k aplikacím**, nechejte výchozí nastavení hello. Tato hodnota určuje, jak často toocreate snímky. Snímky využívají službu Stínová kopie svazku (VSS) tooensure, které aplikace budou při pořízení snímku hello v konzistentním stavu.  Pokud nastavíte hodnotu, ujistěte se, že je menší než počet hello další body obnovení, které nakonfigurujete.
9. V **čas spuštění replikace**, zadejte zahájení počáteční replikaci dat tooAzure. Hello časové pásmo na serveru hostitele technologie Hyper-V hello se použije. Doporučujeme, abyste naplánovali hello počáteční replikaci mimo špičku.

    ![Nastavení replikace cloudu](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Po uložení nastavení hello úlohy se vytvoří a lze sledovat na hello **úlohy** kartě. Všechny servery hostitele technologie Hyper-V ve zdrojovém cloudu VMM hello budou nakonfigurované pro replikaci.

Po uložení, lze nastavení cloudu změnit na hello **konfigurace** kartě toomodify hello cílové umístění nebo cílový účet úložiště budete potřebovat konfigurace cloudu hello tooremove a potom znovu nakonfigurovat hello cloudu. Všimněte si, že pokud změníte změny hello účtu úložiště hello se použijí jenom pro virtuální počítače, které jsou povolené pro ochranu po hello účet úložiště se změnilo. Existující virtuální počítače se nemigrují toohello nový účet úložiště.

## <a name="step-7-configure-network-mapping"></a>Krok 7: Nakonfigurování mapování sítě
Před zahájením mapování sítě ověřte, zda virtuální počítače na zdrojovém serveru VMM hello jsou připojené tooa síť virtuálních počítačů. Dále pak vytvořte jednu nebo více virtuálních sítí Azure. Všimněte si, že více sítí virtuálních počítačů může být namapované tooa jednu síť Azure.

1. Na stránce hello rychlý Start, klikněte na **mapovat sítě**.
2. Na hello **sítě** ve **umístění zdroje**vyberte hello zdrojový server VMM. V části **Cílové umístění** vyberte Azure.
3. V **zdroj** se zobrazí seznam sítí virtuálních počítačů, které jsou přidruženy serveru VMM hello sítě. V **cíl** sítě hello Azure se zobrazí sítě přidružené k předplatnému hello.
4. Vyberte hello zdrojové síti virtuálních počítačů a klikněte na **mapy**.
5. Na hello **výběr cílové sítě** stránky, vyberte hello cílovou síť Azure, které chcete toouse.
6. Klikněte na tlačítko proces mapování hello zaškrtnutí toocomplete hello.

    ![Nastavení replikace cloudu](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Po uložení nastavení hello úloha spustí tootrack hello mapování průběh a je možné ji monitorovat na kartě úlohy hello. Všechny existující repliky virtuálních počítačů, které odpovídají zdrojové síti virtuálních počítačů toohello bude připojený toohello cílové sítě Azure. Nové virtuální počítače, které jsou připojené toohello zdrojové síti virtuálních počítačů bude po replikaci připojené toohello namapované síti Azure. Pokud upravíte existující mapování u nové sítě, virtuální počítače repliky připojí pomocí nového nastavení hello.

Všimněte si, že pokud hello Cílová síť více podsítí a jedna z těchto podsítí má hello stejný název jako podsíť, na které hello zdrojový virtuální počítač se nachází, pak bude virtuální počítač repliky hello připojené toothat cílové podsíti po převzetí služeb při selhání. Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač bude připojený toohello první podsíť v síti hello.

> [!NOTE]
> [Migrace sítí](../azure-resource-manager/resource-group-move-resources.md) napříč prostředků skupiny v hello stejného předplatného, nebo ve předplatných není podporována pro sítě používá pro nasazení Site Recovery.
>
>

## <a name="step-8-enable-protection-for-virtual-machines"></a>Krok 8: Povolení ochrany pro virtuální počítače
Po serverů, cloudů a sítí jsou správně nakonfigurovaný, můžete povolit ochranu pro virtuální počítače v cloudu hello. Vezměte na vědomí následující hello:

* Virtuální počítače, musí splňovat [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
* tooenable ochrany hello operační systém a vlastnosti disku operačního systému musí být nastavena pro virtuální počítač hello. Při vytváření virtuálního počítače v nástroji VMM pomocí šablony virtuálního počítače můžete nastavit vlastnost hello. Můžete také nastavit tyto vlastnosti pro existující virtuální počítače v hello **Obecné** a **konfigurace hardwaru** karty hello vlastnosti virtuálního počítače. Pokud tyto vlastnosti nenastavíte v nástroji VMM budete moct tooconfigure je na portálu Azure Site Recovery hello.

    ![Vytvoření virtuálního počítače](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Změna vlastností virtuálního počítače](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)

1. tooenable ochranu na hello **virtuální počítače** v hello cloudu, ve které hello je umístěn virtuální počítač, klikněte na **povolení ochrany** > **přidat virtuální počítače**.
2. Hello seznam virtuálních počítačů v cloudu hello vyberte hello jeden chcete tooprotect.

    ![Povolení ochrany virtuálního počítače](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    Sledovat průběh hello **povolení ochrany** akce v hello **úlohy** kartě, včetně počáteční replikace hello. Po hello **dokončení ochrany** úloha spustí hello virtuální počítač je připraven na převzetí služeb při selhání. Po povolení ochrany a virtuální počítače jsou replikovány, budete moct tooview je v Azure.

    ![Úloha ochrany virtuálního počítače](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

1. Ověřte vlastnosti virtuálního počítače hello a podle potřeby je upravte.

    ![Ověření virtuálních počítačů](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)
2. Na hello **konfigurace** hello vlastností virtuálního počítače následující vlastnosti sítě je možné upravit.

* **Počet síťových adaptérů na hello cílový virtuální počítač** -hello počet síťových adaptérů je závisí na velikosti hello, kterou zadáte pro hello cílového virtuálního počítače. Zkontrolujte [specifikace velikosti virtuálního počítače](../virtual-machines/linux/sizes.md) hello počet adaptérů podporovaných velikostí virtuálního počítače hello. Když změníte hello velikost virtuálního počítače a uložit nastavení hello, hello počet síťových adaptérů se změní hello při příštím otevření hello **konfigurace** stránky. Hello počet síťových adaptérů cílových virtuálních počítačů je hello minimální počet síťových adaptérů na zdrojový virtuální počítač a hello maximální počet síťových adaptérů nepodporuje velikost hello hello virtuálního počítače jste vybrali, následujícím způsobem:

  * Pokud hello počet síťových adaptérů na zdrojovém počítači hello je menší než nebo rovna toohello počet adaptérů povoleno pro velikost cílového počítače hello pak bude mít cíl hello hello stejný počet adaptérů jako zdroj hello.
  * Pokud hello počet adaptérů pro hello zdrojový virtuální počítač překračuje počet hello povolený pro cílovou velikost hello pak maximální velikost cíle hello se použije.
  * Například pokud má zdrojový počítač dva síťové adaptéry a velikost hello cílového počítače podporuje čtyři, bude mít hello cílový počítač dva adaptéry. Pokud má hello zdrojový počítač dva adaptéry, ale hello podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač hello jenom jeden adaptér.     
* **Síť hello cílového virtuálního počítače** -hello sítě toowhich hello virtuální počítač připojí toois dána mapováním sítě hello zdrojového virtuálního počítače. Pokud hello zdrojový virtuální počítač má více než jeden síťový adaptér a zdrojové sítě jsou namapované toodifferent sítě na cíli, pak musíte toochoose mezi jeden z cílových sítí hello.
* **Podsíť každého síťového adaptéru** – pro každý síťový adaptér můžete vybrat hello podsíť toowhich hello při selhání virtuálního počítače k připojení.
* **Cílová IP adresa** – Pokud hello síťový adaptér zdrojového virtuálního počítače je nakonfigurované toouse statických IP adres můžete zadat hello IP adresu pro hello cílového virtuálního počítače. Pomocí této funkce zachovat hello IP adresu zdrojového virtuálního počítače po převzetí služeb. Pokud žádné IP adresy se poskytuje pak libovolná dostupná IP adresa je uveden toohello síťový adaptér v době hello převzetí služeb při selhání. Pokud je zadaný hello cílová IP adresa, ale je již využíván jiným virtuálním počítačem spuštěným v Azure se nezdaří převzetí služeb při selhání.  

    ![Úprava vlastností sítě](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

> [!NOTE]
> Virtuální počítače s Linuxem se statickými IP adresami nejsou podporované.
>
>

## <a name="test-hello-deployment"></a>Testovací nasazení pro hello
Plánování nasazení můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spustit testovací převzetí služeb pro hello tootest.  

Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti. Poznámky:

* Pokud chcete, aby tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy po převzetí služeb při selhání hello, povolte připojení ke vzdálené ploše na virtuálním počítači hello před spuštěním hello testovací převzetí služeb při selhání.
* Po převzetí služeb při selhání použijte veřejný toohello tooconnect IP adresu virtuálního počítače Azure pomocí vzdálené plochy. Pokud chcete, toodo to, ujistěte se, že nemáte žádné zásady domény, které vám zabrání připojování tooa virtuálnímu počítači pomocí veřejné adresy.

> [!NOTE]
> tooget hello nejlepší výkon při selhání tooAzure, zkontrolujte, že jste nainstalovali hello agent Azure na hello virtuálních počítačů. Umožní rychlejší spouštění a pomáhá při řešení potíží. Stáhnout hello [agenta systému Linux](https://github.com/Azure/WALinuxAgent), nebo hello [agenta Windows](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

### <a name="create-a-recovery-plan"></a>Vytvoření plánu obnovení
1. Na hello **plány obnovení** přidejte nový plán. Zadejte název, **VMM** v **typ zdroje**a hello zdrojový server VMM **zdroj**. Hello cíl bude Azure.

    ![Vytvoření plánu obnovení](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)
2. V hello **vyberte virtuální počítače** stránky, plán obnovení toohello tooadd vyberte virtuální počítače. Tyto virtuální počítače jsou přidány toohello výchozí skupiny plánu obnovení – skupina 1. V jednom plánu obnovení bylo otestováno maximálně 100 virtuálních počítačů.

* Pokud chcete vlastnosti virtuálního počítače hello tooverify před jejich přidáním toohello plán, klikněte na tlačítko hello virtuálního počítače na stránce vlastností hello hello cloudu, na němž je umístěna. Vlastnosti virtuálního počítače hello můžete také nakonfigurovat v konzole VMM hello.
* Všechny hello virtuální počítače, které se zobrazují povolil pro ochranu. Hello seznam obsahuje jak virtuální počítače, které jsou povolené pro ochranu a počáteční replikace byla dokončena, a ty, které jsou povolené pro ochranu s počáteční replikace čeká na. Převzetí služeb při selhání v rámci plánu obnovení je možné pouze u virtuálních počítačů s dokončenou počáteční replikací.

    ![Vytvoření plánu obnovení](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Po jeho vytvoření plánu obnovení se zobrazí v hello **plány obnovení** kartě. Můžete také přidat [runbooky služby Azure automation](site-recovery-runbook-automation.md) toohello obnovení plán tooautomate akcí během převzetí služeb při selhání.

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
Existují dva způsoby toorun tooAzure testovací převzetí služeb při selhání.

* **Testovací převzetí služeb při selhání bez sítě Azure**– tento typ testovacího převzetí služeb při selhání ověří, že hello virtuální počítač se zobrazí správně v Azure. Hello virtuálního počítače nebudou připojené tooany síť Azure po převzetí služeb při selhání.
* **Testovací převzetí služeb při selhání se sítí Azure**– tento typ převzetí služeb při selhání kontroly, které hello celé prostředí replikace dodává až podle očekávání a který převzal hello virtuální počítače budou připojené toohello zadané cílové síti Azure. Podsíť, vybere testovací převzetí služeb při selhání podsíť hello hello testovacího virtuálního počítače budou být započítáno na základě podsítě hello hello repliky virtuálního počítače. Toto je replikace různých tooregular-li hello podsíť virtuálního počítače repliky založena na podsíti hello hello zdrojového virtuálního počítače.

Pokud chcete, aby toorun testovací převzetí služeb pro virtuální počítač, který je povolený pro ochranu tooAzure bez zadání cílové sítě Azure nepotřebujete tooprepare nic. toorun převzetí služeb při selhání s cílovou sítí Azure, budete potřebovat toocreate novou síť Azure, která bude izolovaná od produkční sítě Azure (výchozí chování při vytváření nových sítí v Azure). Podívejte se na to, jak příliš[spustit testovací převzetí služeb](site-recovery-failover.md) další podrobnosti.

Musíte také tooset až hello infrastruktury pro toowork hello replikovat virtuální počítač podle očekávání. Například virtuální počítač s řadičem domény a DNS může být replikované tooAzure pomocí Azure Site Recovery a lze vytvořit v testovací síti hello testovací převzetí služeb při selhání. Další podrobnosti najdete v tématu věnovaném [aspektům, které je třeba zvážit při testování převzetí služeb při selhání pro Active Directory](site-recovery-active-directory.md#test-failover-considerations).

toorun testovací převzetí služeb hello následující:

1. Na hello **plány obnovení** vyberte hello plán a klikněte na tlačítko **testovací převzetí služeb při selhání**.
2. Na hello **potvrzení převzetí služeb při selhání** vyberte **žádné** nebo konkrétní síť Azure.  Všimněte si, že pokud vyberete možnost žádné bude hello testovací převzetí služeb při selhání zkontrolujte, zda text hello virtuální počítač replikoval správně tooAzure ale neohlásí konfiguraci sítě replikace.

    ![Žádná síť](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)
3. Pokud je povolené šifrování dat pro hello cloud v **šifrovací klíč** hello vyberte certifikát, který byl vydán během instalace hello zprostředkovatele na serveru VMM hello, pokud je zapnutá hello možnost tooenable šifrování dat pro cloud .
4. Na hello **úlohy** můžete sledovat průběh převzetí služeb při selhání. Měli byste také mít testovací repliku virtuálního počítače může toosee hello v hello portálu Azure. Pokud jste nastavili tooaccess virtuální počítače z vaší místní sítě můžete spustit virtuální počítač toohello připojení vzdálené plochy.
5. Když hello převzetí služeb při selhání dosáhne hello **dokončit testování** fáze, klikněte na tlačítko **dokončení testovacího** toofinish ho. Podrobnostem toohello **úlohy** kartě tootrack převzetí služeb při selhání průběh a stav a tooperform všechny akce, které jsou potřeba.
6. Po převzetí služeb při selhání můžete zobrazit testovací repliku virtuálního počítače hello v hello portálu Azure. Pokud jste nastavili tooaccess virtuální počítače z vaší místní sítě můžete spustit virtuální počítač toohello připojení vzdálené plochy. Hello následující:

   1. Ověřte, že hello virtuální počítače úspěšně spustí.
   2. Pokud chcete, aby tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy po převzetí služeb při selhání hello, povolte připojení ke vzdálené ploše na virtuálním počítači hello před spuštěním hello testovací převzetí služeb při selhání. Budete také potřebovat tooadd koncový bod RDP na virtuální počítač hello. Můžete využít [sad Azure Automation Runbook](site-recovery-runbook-automation.md) toodo který.
   3. Po převzetí služeb při selhání Pokud použijete veřejnou IP adresu tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy, ujistěte se, že nemáte žádné zásady domény, které by vám bránily v připojení tooa virtuálnímu počítači pomocí veřejné adresy.
7. Po dokončení testování hello hello následující:

   * Klikněte na tlačítko **hello testovací převzetí služeb při selhání je dokončena**. Vyčištění hello testovací prostředí tooautomatically power vypnout a odstraňte hello testovací virtuální počítače.
   * Klikněte na tlačítko **poznámky** toorecord a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání.


## <a name="next-steps"></a>Další kroky
Další informace o [nastavení plánů obnovení](site-recovery-create-recovery-plans.md) a [převzetí služeb při selhání](site-recovery-failover.md).
