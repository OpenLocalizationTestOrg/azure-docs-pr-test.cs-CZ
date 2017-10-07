---
title: "aaaReplicate virtuálních počítačů technologie Hyper-V v nástroji VMM tooa sekundární lokality (portál Azure classic) | Microsoft Docs"
description: "Tento článek popisuje, jak tooreplicate virtuálních počítačů Hyper-V v nástroji VMM cloudů tooa sekundární lokalita VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: a63acba3-8fb3-4926-aa29-b1e64a765681
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: fe551e1f741579044f540ea0e50a6e36d48133ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site"></a>Replikace virtuálních počítačů technologie Hyper-V v nástroji VMM cloudy tooa sekundární lokalita VMM
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Portál Classic](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Hello služba Azure Site Recovery přispívá tooyour provozní kontinuitu a po havárii (BCDR) strategie Orchestrace replikace, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Počítače může být replikované tooAzure nebo tooa sekundárního místního datového centra. Rychlý přehled najdete v článku [Co je Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Přehled
Tento článek popisuje, jak tooreplicate technologie Hyper-V virtuální počítače v Hyper-V hostitelské servery, které jsou spravovány v nástroji VMM cloudy toosecondary VMM webu pomocí Azure Site Recovery.

Hello článek obsahuje požadavků, ukazuje, jak instalovat tooset do trezoru Site Recovery, hello zprostředkovatele Azure Site Recovery na zdrojové a cílové servery VMM, zaregistrovat hello servery v trezoru hello, nakonfigurovat nastavení ochrany pro cloudy VMM a potom povolit ochranu pro virtuální počítače Hyper-V. Nakonec testování převzetí služeb při selhání hello toomake se, že vše funguje podle očekávání.

POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Architektura
Následující obrázek Hello ukazuje hello různých komunikačních kanálů a porty používané službou Azure Site Recovery pro orchestration a replikace

![Topologie e2e](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Než začnete
Ujistěte se, že máte zavedenou tyto požadavky:

| **Požadavky** | **Podrobnosti** |
| --- | --- |
| **Azure** |Potřebujete účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery |
| **VMM** |Musíte mít alespoň jeden server VMM.<br/><br/>Hello VMM server by měl být spuštěn alespoň System Center 2012 SP1 s nejnovější kumulativní aktualizace hello.<br/><br/>Pokud chcete tooset ochrany s jedním serverem VMM služby, je nutné aspoň dva cloudy, které jsou nakonfigurované na serveru hello.<br/><br/>Pokud chcete ochranu toodeploy s dvěma servery VMM, každý server musí mít alespoň jeden cloud nakonfigurované na primárním serveru VMM hello chcete tooprotect a k jednomu cloudu nakonfigurované na sekundárním serveru VMM hello chcete toouse ochrany a obnovení<br/><br/>Všechny cloudy VMM musí mít hello technologie Hyper-V profilovou sadu schopností.<br/><br/>Hello zdrojový cloud, které chcete tooprotect musí obsahovat jeden nebo více skupin hostitelů VMM. |
| **Hyper-V** |Budete potřebovat nejméně jeden server hostitele technologie Hyper-V ve skupinách hostitelů VMM hello, primárního a sekundárního a jeden nebo více virtuálních počítačů na každém serveru hostitele technologie Hyper-V.<br/><br/>Hello hostitele a cílovým serverem, technologie Hyper-V musí běžet minimálně Windows Server 2012 s rolí hello technologie Hyper-V a mít hello nainstalované nejnovější aktualizace.<br/><br/>Jakýkoli server Hyper-V obsahující virtuální počítače, budete chtít tooprotect se musí nacházet v cloudu VMM.<br/><br/>Pokud používáte technologii Hyper-V v clusteru, Všimněte si, že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte statické IP adresy na základě clusteru. Zprostředkovatel clusteru hello tooconfigure musíte ručně. [Další informace](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) v najdete blogu Aidana Finna. |
| **Mapování sítě** |Můžete nakonfigurovat toomake mapování sítě se, že replikované virtuální počítače budou optimálně umístěny v sekundární hostitelské servery technologie Hyper-V po převzetí služeb při selhání a aby mohli připojit tooappropriate sítě virtuálních počítačů. Pokud nenakonfigurujete mapování sítě, virtuální počítače replik připojené tooany sítě nebude po převzetí služeb při selhání.<br/><br/>tooset si mapování sítě během nasazení, zajistěte, aby byla hello virtuální počítače na serveru hostitele hello zdroj technologie Hyper-V připojený tooa sítě virtuálních počítačů nástroje VMM. Tuto síť by měla být propojené tooa logické sítě, který je přidružen hello cloudu. < br /<br/>Cílový cloud Hello na hello sekundárního serveru VMM, který používáte pro obnovení musí mít odpovídající síť virtuálních počítačů konfigurovanou a pak musí být propojená tooa odpovídající logické sítě, který je přidružen hello cílový cloud. |
| **Mapování úložiště** |Ve výchozím nastavení při replikaci virtuálního počítače na zdroj technologie Hyper-V hostiteli serveru tooa cíl technologie Hyper-V hostitelském serveru, je replikovaná data uložená v hello výchozí umístění, uvedené hello cílového hostitele technologie Hyper-V ve Správci technologie Hyper-V. Pro větší kontrolu nad se uloží replikovaná data můžete nakonfigurovat mapování úložiště<br/><br/> tooconfigure úložiště mapování, potřebujete tooset až klasifikace úložiště na hello zdrojové a cílové servery VMM před zahájením nasazení. |

## <a name="step-1-create-a-site-recovery-vault"></a>Krok 1: Vytvoření trezoru Site Recovery
1. Přihlaste se toohello [portálu pro správu](https://portal.azure.com) ze serveru VMM hello chcete tooregister.
2. Rozbalte položku **datové služby** > **služeb zotavení** a klikněte na tlačítko **trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. V **název**, zadejte popisný název tooidentify hello trezoru.
5. V **oblast** hello vyberte zeměpisnou oblast trezoru hello. toocheck podporované oblasti, najdete v části geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](http://go.microsoft.com/fwlink/?LinkId=389880).
6. Klikněte na **Vytvořit trezor**.

    ![Vytvoření trezoru](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Vrácení se změnami stavu hello panelu trezoru této hello byl vytvořen. Hello trezor bude uvedený jako **Active** na hlavní stránce služeb zotavení hello.

## <a name="step-2-generate-a-vault-registration-key"></a>Krok 2: Vygenerování registračního klíče trezoru
Vygenerujte registrační klíč v trezoru hello. Po stažení hello zprostředkovatele Azure Site Recovery a nainstalujte ji na serveru VMM hello, použijete tento klíč tooregister hello VMM server v trezoru hello.

1. V hello **služeb zotavení** klikněte na tlačítko hello trezoru tooopen hello stránky rychlý Start. Rychlý Start můžete otevřít také kdykoli pomocí ikony hello.

    ![Ikona Rychlý start](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)
2. V rozevíracím seznamu hello vyberte **mezi dvěma místními servery VMM**.
3. V **připravit servery VMM**, klikněte na tlačítko **vygenerovat soubor registračního klíče**. soubor klíče Hello se vygeneruje automaticky a je platný po dobu 5 dní, po jeho vygenerování. Pokud nepoužíváte přístup ze serveru VMM hello k hello portálu Azure musíte toocopy tento souborový server toohello.

    ![Registrační klíč](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-hello-azure-site-recovery-provider"></a>Krok 3: Instalace hello zprostředkovatele Azure Site Recovery
1. Na hello **rychlý Start** stránky v **připravit servery VMM**, klikněte na tlačítko **stáhnout zprostředkovatele Microsoft Azure Site Recovery pro instalaci na serverech VMM** tooobtain hello nejnovější verze Instalační soubor zprostředkovatele hello.
2. Spusťte tento soubor na zdrojovém serveru VMM hello.

   > [!NOTE]
   > Pokud VMM je nasazen v clusteru a instalujete hello zprostředkovatele pro hello první instalaci na aktivním uzlu a dokončit hello instalace tooregister hello VMM server v trezoru hello. Nainstalujte hello zprostředkovatele na hello dalších uzlů. Všimněte si, že pokud upgradujete hello poskytovatel budete potřebovat tooupgrade na všech uzlech, protože všechny by měly být spuštěná hello tutéž verzi zprostředkovatele.
   >
   >
3. Hello instalační program nemá několik **zkontrolujte předběžných požadavků** a žádosti o oprávnění toostop hello VMM služby toobegin instalační program zprostředkovatele. Hello služby VMM se restartuje automaticky po dokončení instalace. Pokud instalujete na clusteru VMM, které budete výzva toostop hello Clusterové role.
4. V nastavení služby **Microsoft Update** můžete vyjádřit výslovný souhlas se stahováním a instalací aktualizací. Toto nastavení povoleno zprostředkovatele aktualizace se nainstalují podle zásad tooyour Microsoft Update.

    ![Aktualizace Microsoft Update](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)
5. umístění instalace Hello se nastaví příliš**<SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin**. Klikněte na hello instalace tlačítko toostart instalaci hello zprostředkovatele.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)
6. Po hello je nainstalován poskytovatel klikněte na tlačítko **zaregistrovat** tooregister hello server v trezoru hello.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
7. V **název trezoru**, ověřte název hello hello úložiště, ve které hello bude server zaregistrovaný. Klikněte na *Další*.

    ![Registrace serveru](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)
8. V **připojení k Internetu** zadat jak hello zprostředkovatel, který běží na hello VMM server připojuje toohello Internetu. Vyberte **připojit se s existujícím nastavením proxy serveru** toouse hello výchozí nastavení připojení k Internetu nakonfigurované na serveru hello.

    ![Nastavení internetu](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

   * Pokud chcete, aby toouse vlastní proxy server můžete musí ho nastavit před instalací hello zprostředkovatele. Při konfiguraci nastavení vlastního proxy serveru se test spustí toocheck hello proxy připojení.
   * Pokud používáte vlastní proxy server nebo váš výchozí proxy server vyžaduje ověření, je třeba tooenter hello podrobnosti o proxy serveru, včetně hello adresy a portu.
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
13. Klikněte na tlačítko **Další** toocomplete hello procesu. Po registraci načte Azure Site Recovery metadata ze serveru VMM hello. Hello server se zobrazí v **servery VMM** > **servery** v trezoru hello.

    ![Servery](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Instalace pomocí příkazového řádku
Hello zprostředkovatele Azure Site Recovery můžete také nainstalovat z příkazového řádku hello. Tato metoda může být použité tooinstall hello zprostředkovatele na serveru jádra pro Windows Server 2012 R2.

1. Hello zprostředkovatele instalace souboru a registraci klíče tooa složka pro stahování. Například C:\ASR.
2. Zastavit hello služba System Center Virtual Machine Manager
3. Vyextrahujte instalační program zprostředkovatele hello tak, že spustíte tyto příkazy z příkazového řádku s **správce** oprávnění:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Nainstalujte zprostředkovatele hello spuštěním:

        C:\ASR> setupdr.exe /i
5. Registrace zprostředkovatele hello spuštěním:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>     

Kde jsou hello parametry:

* **/ Credentials**: povinný parametr, který určuje hello umístění, ve které hello se nachází soubor registračního klíče  
* **/ FriendlyName**: povinný parametr pro hello název hostitele serveru hello technologie Hyper-V, který se zobrazí na portálu Azure Site Recovery hello.
* **/ EncryptionEnabled**: volitelný parametr, je nutné toouse pouze v hello VMM tooAzure scénář, pokud je třeba šifrování virtuálních počítačů v klidovém stavu uložených v Azure. Zkontrolujte, že hello název souboru hello zadáte má **.pfx** rozšíření.
* **/ proxyaddress**: volitelný parametr, který určuje adresu hello hello proxy serveru.
* **/ proxyport**: volitelný parametr, který určuje port hello hello proxy serveru.
* **/ proxyusername**: volitelný parametr, který určuje uživatelské jméno Proxy hello (pokud proxy server vyžaduje ověření).
* **/ proxypassword**: volitelný parametr, který určuje hello heslo pro ověřování pomocí serveru proxy hello (pokud proxy server vyžaduje ověření).  

## <a name="step-4-configure-cloud-protection-settings"></a>Krok 4: Konfigurace cloudu nastavení ochrany
Po registraci jsou servery VMM, můžete nakonfigurovat nastavení ochrany cloudu. Pokud jste povolili možnost hello **synchronizovat data cloudu s trezorem hello** při instalaci hello poskytovatele, zobrazí se všechny cloudy na serveru VMM hello v hello **chráněné položky** kartě v trezoru hello. Jestliže jste můžete synchronizovat určitý cloud s Azure Site Recovery v hello **Obecné** hello stránky vlastností cloudu v konzole VMM hello.

![Publikovaný cloud](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Na stránce hello rychlý Start, klikněte na **nastavení ochrany pro cloudy VMM**.
2. Na hello **Cloudech VMM** vyberte hello cloud má tooconfigure a přejděte toohello **konfigurace** kartě.
3. V **cíl**, vyberte **VMM**.
4. V **cílové umístění**, vyberte hello na místě serveru VMM, který spravuje hello cloudu chcete toouse pro obnovení.
5. V **cílí na cloud**, vyberte cílový cloud hello chcete toouse převzetí služeb při selhání virtuálních počítačů v cloudu zdroj hello. Poznámky:

   * Doporučujeme vám, že jste vybrali cílový cloud, který splňuje požadavky na obnovení pro hello virtuálních počítačů, které budete chránit.
   * Cloud lze přiřadit pouze jeden cloud pár tooa – buď jako primární nebo cílový cloud.
6. V **frekvence kopírování**, určete, jak často se mají data synchronizovat mezi zdrojovým a cílovým umístěním 5he. Všimněte si, že toto nastavení platí pouze při hello hostitele Hyper-V se systémem Windows Server 2012 R2. Pro další servery se používá výchozí nastavení pět minut.
7. V **další body obnovení**, určete, jestli má toocreate další body. Hodnota Hello výchozí nula znamená, že pouze hello nejnovější bod obnovení pro primární virtuální počítač je uložen na hostitelském serveru repliky. Všimněte si, že povolení více bodů obnovení vyžaduje dodatečné úložiště pro hello snímky, které jsou uloženy v každém bodu obnovení. Ve výchozím nastavení vytvořeny body obnovení jsou každou hodinu, takže každý bod obnovení obsahuje data za jednu hodinu. Hello hodnota bodu obnovení, který přiřadíte hello virtuálního počítače v konzole VMM hello by neměl být menší než hodnota hello, který přiřadíte v konzole Azure Site Recovery hello.
8. V **frekvence snímků konzistentní vzhledem k aplikacím**, určete, jak často toocreate snímky konzistentní s aplikací. Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače hello a snímky konzistentní s aplikací, která přebírá v okamžiku snímek dat aplikací hello uvnitř hello virtuálního počítače. Snímky konzistentní s aplikacemi pomocí tooensure služby Stínová kopie svazku (VSS), které aplikace budou při pořízení snímku hello v konzistentním stavu. Všimněte si, že pokud povolíte snímky konzistentní s aplikacemi, bude mít vliv hello výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byl hello hodnoty, které nastavíte, menší než počet hello další body obnovení, které nakonfigurujete.

    ![Konfigurovat nastavení ochrany](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)
9. V **kompresi přenosu dat**, zadejte, zda by měl být komprimované replikovaná data, která se přenáší.
10. V **ověřování**, zadejte, jak ověření přenosů mezi hello primární a servery hostitele obnovení technologie Hyper-V. Pokud máte fungující Kerberos prostředí nakonfigurovat, vyberte HTTPS. Azure Site Recovery automaticky nakonfiguruje certifikáty pro ověřování pomocí protokolu HTTPS. Není nutná žádná ruční konfigurace. Pokud vyberete protokolu Kerberos, lístek protokolu Kerberos se použije pro vzájemné ověřování serverů hostitele hello. Ve výchozím nastavení port 8083 a 8084 (pro certifikáty) se otevřou v hello brány Windows Firewall na hostitelských serverech technologie Hyper-V hello. Všimněte si, že toto nastavení platí jen pro hostitelské servery technologie Hyper-V v systému Windows Server 2012 R2.
11. V **Port**, úpravě hello číslo portu na hello zdroje a cíle naslouchání počítače hostitele pro přenosy replikace. Nastavení hello může například změnit, pokud chcete, aby tooapply Quality of Service (QoS) šířku pásma sítě omezení šířky pásma pro přenosy replikace. Zkontrolujte hello port není používán jinou aplikaci a že je otevřen v nastavení brány firewall hello.
12. V **metodu replikace**, zadejte, jak hello počáteční replikaci dat ze zdrojového umístění tootarget se budou zpracovávat, před zahájením regulární replikace:

    * **Přes síť**– kopírování dat přes síť hello může být časově náročná a prostředky. Doporučujeme používat tuto možnost, pokud hello cloud obsahuje virtuální počítače s relativně malý virtuální pevné disky, a pokud hello primární lokalita je sekundární lokality připojené toohello přes připojení s celou šířku pásma. Můžete uvést, že by měl hello kopie spustit okamžitě, nebo vyberte čas. Pokud používáte sítě replikace, doporučujeme, abyste naplánovali mimo špičku.
    * **Offline**– tato metoda určuje, že hello počáteční replikace se provede pomocí externího média. Je užitečné, pokud chcete, aby tooavoid snížení výkonu sítě, nebo pro geograficky vzdálených umístěních. toouse tuto metodu, zadejte umístění exportu hello na hello zdrojový cloud a umístění hello importovat na cílový cloud hello. Pokud povolíte ochranu pro virtuální počítač, hello virtuální pevný disk je zkopírovaný toohello zadané umístění exportu. Odeslat toohello cílové lokality a zkopírujte jej toohello import umístění. hello kopie systému Hello importovat informace toohello repliky virtuálních počítačů.
13. Vyberte **odstranit virtuální počítač repliky** toospecify, který hello virtuální počítač repliky, měl by být odstraněn, když zastavíte ochranu hello virtuálního počítače tak, že vyberete hello **odstranit ochranu pro virtuální počítač hello**  možnost hello virtuální počítače na kartě Vlastnosti cloudu hello. Toto nastavení povoleno, při zakázání ochrany hello virtuální počítač je odebrán z Azure Site Recovery, odeberou se v konzole VMM hello hello Site Recovery nastavení pro virtuální počítač hello a replika hello odstraněn.

    ![Konfigurovat nastavení ochrany](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

Po uložení nastavení hello úlohy se vytvoří a lze sledovat na hello **úlohy** kartě. Všechny servery hostitele technologie Hyper-V ve zdrojovém cloudu VMM hello budou nakonfigurované pro replikaci. Nastavení cloudu se dají změnit na hello **konfigurace** kartě. Pokud chcete toomodify hello cílové umístění nebo cílový cloud musíte odebrat konfiguraci cloudu hello a potom znovu nakonfigurovat hello cloudu.

### <a name="prepare-for-offline-initial-replication"></a>Příprava pro počáteční replikaci prováděnou offline
Hello toodo následující akce tooprepare pro počáteční replikaci v režimu offline, budete potřebovat:

* Na zdrojovém serveru hello zadáte cestu umístění, ze které hello bude export dat probíhat. Přiřaďte úplné řízení pro systém souborů NTFS a oprávnění ke sdílení toohello služba VMM na cestu pro export hello. Na cílovém serveru hello budete zadejte, že dojde k cestu umístění, ze kterého hello data importovat. Přiřaďte hello stejná oprávnění v této cestě importu.
* Pokud hello import nebo export cesta sdílená, přiřadit správce, Power Users, Print Operator nebo operátor serveru členství ve skupině pro účet služby VMM hello ve vzdáleném počítači hello na které hello sdílet se nachází.
* Pokud používáte všechny spustit jako účty tooadd hostitele, hello importovat a exportovat cesty, přiřadit pro čtení a zápis toohello účty spustit jako v nástroji VMM.
* Hello import a export sdílených složek nesmí nacházet na libovolném počítači se používá jako server hostitele technologie Hyper-V, protože konfigurace zpětné smyčky není podporována technologií Hyper-V.
* Ve službě Active Directory, na každém serveru hostitele technologie Hyper-V, který obsahuje virtuální počítače, které chcete tooprotect povolte a nakonfigurujte omezené delegování tootrust hello vzdálené počítačích, na které hello import a export cesty jsou umístěny, následujícím způsobem:
  1. V řadiči domény hello otevřete **Active Directory Users and Computers**.
  2. Ve stromu konzoly hello klikněte na tlačítko **DomainName** > **počítače**.
  3. Název hostitelského serveru klikněte pravým tlačítkem na hello technologie Hyper-V > **vlastnosti**.
  4. Na hello **delegování** kartě klikněte na tlačítko T**koroze tomuto počítači pro delegování pouze toospecified službám**.
  5. Klikněte na tlačítko **používající libovolný protokol pro ověřování**.
  6. Klikněte na tlačítko **přidat** > **uživatelé a počítače**.
  7. Název typu hello hello počítače, který je hostitelem cestu pro export hello > **OK**. Hello seznamu dostupných služeb, podržte klávesu CTRL hello a klikněte na **cifs** > **OK**. Opakujte pro hello název počítače hello cestu importu hello tohoto hostitele. Opakujte podle potřeby pro další hostitelské servery technologie Hyper-V.

## <a name="step-5-configure-network-mapping"></a>Krok 5: Konfigurace mapování sítě
1. Na stránce hello rychlý Start, klikněte na **mapovat sítě**.
2. Vyberte hello zdrojový server VMM ze kterého chcete toomap sítě a pak hello cíl VMM server toowhich hello, které budou mapována sítě. Zobrazí se seznam Hello zdrojové sítě a jejich přidružené cílové sítě. Pro sítě, které nejsou namapované aktuálně se zobrazí na prázdnou hodnotu.
3. Vyberte síť, v **sítě na zdroji** > **mapy**. Služba Hello zjistí hello sítě virtuálních počítačů na cílovém serveru hello a zobrazí je. Klikněte na tlačítko hello informace ikonu další toohello zdrojové a cílové názvy tooview hello podsítí pro každou síť.

    ![Konfigurace mapování sítě](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)
4. V dialogovém okně hello vyberte jednu z sítě virtuálních počítačů hello hello cílovém serveru VMM.

    ![Výběr cílové sítě](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)
5. Při výběru cílové sítě hello chráněných cloudů, které používají hello zdrojové síti jsou zobrazeny. Zobrazí se také k dispozici cílové sítě, které jsou spojené s cloudy hello sloužící k ochraně. Doporučujeme vám, že vyberete Cílová síť, která je k dispozici tooall hello cloudy, který používáte pro ochranu. Nebo můžete také přejít toohello serveru VMM a upravit hello cloudu vlastnosti tooadd hello logické sítě odpovídající toohello síť virtuálních počítačů, které chcete toochoose.
6. Klikněte na tlačítko proces mapování hello zaškrtnutí toocomplete hello. Úloha spustí tootrack hello mapování průběh. Můžete ji zobrazit v hello **úlohy** kartě.

## <a name="step-6-configure-storage-mapping"></a>Krok 6: Konfigurace mapování úložiště
Ve výchozím nastavení při replikaci virtuálního počítače na zdroj technologie Hyper-V hostiteli serveru tooa cíl technologie Hyper-V hostitelském serveru, je replikovaná data uložená v hello výchozí umístění, uvedené hello cílového hostitele technologie Hyper-V ve Správci technologie Hyper-V. Pro větší kontrolu nad se uloží replikovaná data můžete nakonfigurovat úložiště mapování následujícím způsobem:

1. Definovat klasifikace úložiště na obou hello zdrojové a cílové servery VMM. [Další informace](https://technet.microsoft.com/library/gg610685.aspx). Klasifikace musí být k dispozici toohello servery hostitele technologie Hyper-V v cloudech zdrojové a cílové. Klasifikace nepotřebujete toohave hello stejný typ úložiště. Například můžete namapovat zdroj klasifikace, který obsahuje SMB sdílené složky tooa cíl klasifikace, která obsahuje sdílené svazky clusteru.
2. Po klasifikace jsou na místě můžete vytvořit mapování. toodo to na hello **rychlý Start** stránky > **mapování úložiště**.
3. Klikněte na tlačítko hello **úložiště** kartě > **mapování klasifikace úložiště**.
4. Na hello **mapování klasifikace úložiště** vyberte klasifikace na hello zdrojové a cílové servery VMM. Uložte nastavení.

    ![Výběr cílové sítě](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)

## <a name="step-7-enable-virtual-machine-protection"></a>Krok 7: Povolení ochrany virtuálního počítače
Po serverů, cloudů a sítí jsou správně nakonfigurovaný, můžete povolit ochranu pro virtuální počítače v cloudu hello.

1. Na hello **virtuální počítače** v hello cloudu, ve které hello je umístěn virtuální počítač, klikněte na **povolení ochrany** > **přidat virtuální počítače**.
2. Hello seznam virtuálních počítačů v cloudu hello vyberte hello jeden chcete tooprotect.

    ![Povolení ochrany virtuálního počítače](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)
3. Sledovat průběh hello akce povolení ochrany v hello **úlohy** kartě, včetně počáteční replikace hello. Po úloze dokončení ochrany hello spustí hello virtuální počítač je připraven na převzetí služeb při selhání. Po povolení ochrany a virtuální počítače jsou replikovány, budete moct tooview je v Azure.

    ![Úloha ochrany virtuálního počítače](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

> [!NOTE]
> Můžete také povolit ochranu pro virtuální počítače v konzole VMM hello. Klikněte na tlačítko **povolení ochrany** na panelu nástrojů hello v hello **Azure Site Recovery** ve vlastnostech virtuálního počítače hello.
>
>

### <a name="on-board-existing-virtual-machines"></a>Integrovaného existující virtuální počítače
Pokud máte existující virtuální počítače v nástroji VMM, které jsou replikace s replikou technologie Hyper-V, budete potřebovat tooonboard je pro ochranu Azure Site Recovery následujícím způsobem:

1. Ověřte, že máte primárních a sekundárních cloudech. Zajistěte, aby server hello technologie Hyper-V, který hostuje hello existující virtuální počítač se nachází v primárním cloudu hello a tento server technologie Hyper-V hello hostování virtuální počítač repliky hello je umístěný v cloudu sekundární hello. Ujistěte se, že jste nakonfigurovali nastavení ochrany pro cloudy hello. nastavení Hello by měl odpovídat aktuálně konfigurovaných pro repliku technologie Hyper-V. V opačném případě replikaci virtuálního počítače nemusí fungovat podle očekávání.
2. Potom povolte ochranu pro primární virtuální počítač hello. Azure Site Recovery a VMM zajišťují, že hello stejného hostitele repliky a virtuální počítač je zjištěna, a bude znovu použít a opětovnému zavedení replikace hello nastavení během konfigurace cloudu pomocí Azure Site Recovery.

## <a name="test-your-deployment"></a>Otestujte nasazení
Plánování nasazení můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spustit testovací převzetí služeb pro hello tootest.  Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti.

### <a name="create-a-recovery-plan"></a>Vytvoření plánu obnovení
1. Na hello **plány obnovení** , klikněte na **vytvořit plán obnovení**.
2. Zadejte název pro plán obnovení hello a zdrojové a cílové servery VMM. Hello zdrojový server musí mít virtuální počítače, které jsou povolené pro převzetí služeb při selhání a obnovení. Vyberte **technologie Hyper-V** tooview pouze cloudy, které jsou nakonfigurované pro replikaci technologie Hyper-V.

    ![Vytvoření plánu obnovení](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)
3. V **vybrat virtuální počítač**, vyberte replikační skupiny. Všechny virtuální počítače spojené s hello replikační skupinu bude vybrána a přidat toohello plánu obnovení. Tyto virtuální počítače jsou přidány toohello výchozí skupiny plánu obnovení – skupina 1. v případě potřeby můžete přidat další skupiny. Všimněte si, že po replikaci virtuálního počítače se spustí v souladu s hello pořadí skupiny plánu obnovení hello.

    ![Přidat virtuální počítače](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

Po vytvoření plánu obnovení se zobrazí v seznamu hello na hello **plány obnovení** kartě.

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
1. Na hello **plány obnovení** vyberte hello plán a klikněte na tlačítko **testovací převzetí služeb při selhání**.
2. Na hello **potvrzení převzetí služeb při selhání** vyberte **žádné**. Všimněte si, že tato možnost povolená hello převzal virtuální počítače repliky nebude připojeného tooany sítě. Tím otestujete, který hello selže virtuálního počítače přes podle očekávání, ale není testovací prostředí sítě replikace. Podívejte se na to, jak příliš[spustit testovací převzetí služeb](site-recovery-failover.md) další podrobnosti o toouse různé možnosti sítě.
3. v hello stejné hostitele jako hello hostitele, na které hello existuje virtuální počítač repliky se vytvoří Hello testovacího virtuálního počítače. Přidá toohello stejném cloudu, ve které hello je umístěn virtuální počítač repliky.

### <a name="run-a-recovery-plan"></a>Spuštění plánu obnovení
Po replikaci hello repliky virtuální počítač nemusí mít IP adresu, která je hello stejné jako IP adresu hello hello primárního virtuálního počítače. Virtuální počítače zaktualizuje hello server DNS, který používají po spuštění. Můžete také přidat skript tooupdate hello DNS Server tooensure včasné aktualizace.

#### <a name="script-tooretrieve-hello-ip-address"></a>Skript tooretrieve hello IP adresu
Spusťte tento ukázkový skript tooretrieve hello IP adresu.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-tooupdate-dns"></a>Skript tooupdate DNS
Spustit tento ukázkový skript tooupdate DNS, zadání hello IP adresy, který jste získali pomocí hello předchozí ukázkový skript.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Informace o ochraně osobních údajů pro obnovení lokality
Tato část obsahuje informace o dalších ochrany osobních údajů pro hello služby Microsoft Azure Site Recovery (dále jen "služba"). tooview hello zásady ochrany osobních údajů pro služby Microsoft Azure, najdete v článku [prohlášení o ochraně osobních údajů pro Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=324899)

**Funkce: registrace**

* **Co znamená**: zaregistruje server se službou tak, aby virtuální počítače můžou být chráněné
* **Informace shromážděné**: po registraci hello služba shromažďuje, procesy a odesílá informace o certifikátu správy ze serveru VMM hello, který určil zotavení po havárii tooprovide pomocí názvu služby hello hello serveru VMM, a hello název cloudy virtuálních počítačů na serveru VMM.
* **Použití informací**:

  * Certifikát pro správu – používá se toohelp identifikovat a ověřovat hello zaregistrovat server VMM pro přístup k toohello služby. Hello služby používá hello část veřejného klíče toosecure certifikát hello token, který pouze hello zaregistrovat VMM server můžete získat přístup k. Hello server musí toouse této funkce služby toohello toogain tokenu přístupu.
  * Název serveru VMM hello – název serveru VMM hello je požadovaná tooidentify a komunikovat s hello odpovídající VMM server, na které hello cloudy jsou umístěny.
  * Cloud názvy ze serveru VMM hello – název cloudu hello je potřeba při použití cloudové služby hello spárování nebo zrušení spárování funkce popsané níže. Pokud se rozhodnete toopair vašeho cloudu z primární datové centrum s jiného cloudu v hello obnovení datového centra, jsou uvedené názvy hello všechny cloudy hello z hello obnovení datového centra.
* **Volba**: tyto informace je nedílnou součást vámi vyžádaných hello procesu registrace služby, protože vám pomáhá a hello služby tooidentify hello VMM serveru, pro které chcete ochranu Azure Site Recovery tooprovide, jakož i tooidentify hello správné zaregistrovaný server VMM. Pokud nechcete, aby toosend tato informace toohello služby, nepoužívejte tuto službu. Pokud váš server zaregistrovat a pak později chtít toounregister, můžete tak učinit odstraněním hello informace o serveru VMM z portálu hello (což je hello portál Azure).

**Funkce: Povolte ochranu Azure Site Recovery**

* **Co znamená**: hello Azure Site Recovery Provider nainstalovaný na serveru VMM hello je hello kanál pro komunikaci s hello služby. Hello zprostředkovatele je dynamická knihovna (DLL) hostované v procesu VMM hello. Po hello, který je nainstalován poskytovatel získá v konzole pro správu VMM hello povolena funkce "Obnovení Datacentra" hello. Všechny nové nebo existující virtuální počítače v cloudu můžete povolit vlastnost s názvem "Obnovení Datacentra" toohelp chránit hello virtuálního počítače. Jakmile je tato vlastnost nastavena, odešle hello zprostředkovatele hello název a ID hello virtuálního počítače toohello služby. Ochrana virtuálních Hello je povolena replikace technologie Windows Server 2012 nebo Windows Server 2012 R2 Hyper-V. získá data virtuálního počítače Hello replikují z jeden tooanother hostitele technologie Hyper-V (obvykle umístěné v datovém centru různých "obnovení").
* **Informace shromážděné**: hello Service shromažďuje, procesy a odesílá metadata pro hello virtuálního počítače, která zahrnuje hello název, ID, virtuální sítě a název hello hello cloudu, ke které patří.
* **Použití informací**: hello služby používá hello výše hello informace toopopulate-informace o virtuálním počítači na portálu služby.
* **Volba**: Toto je nedílnou součást vámi vyžádaných hello služby a nelze po zapnutí vypnout. Pokud nechcete, aby tato informace odesílané toohello služby, není povolte ochranu Azure Site Recovery pro všechny virtuální počítače. Všimněte si, že všechna data odeslaná hello zprostředkovatele toohello služby je zaslaného prostřednictvím protokolu HTTPS.

**Funkce: Plán obnovení**

* **Co znamená**: Tato funkce vám pomůže toobuild plán orchestration pro hello "obnovení" datového centra. Můžete definovat hello pořadí, ve které hello by měl být spuštěn virtuální počítače nebo skupiny virtuálních počítačů v lokalitě obnovení hello. Můžete také zadat všechny automatizované skripty toobe spustit nebo toobe jakékoli ruční akce prováděné během hello obnovení pro každý virtuální počítač. Převzetí služeb při selhání (popsaná v další části hello) se obvykle aktivuje v hello plán obnovení úroveň koordinované obnovení.
* **Informace shromážděné**: hello Service shromažďuje, procesy a odesílá metadata pro plán obnovení hello, včetně metadata virtuálního počítače a metadata všechny skripty pro automatizaci a poznámky k ruční akce.
* **Použití informací**: metadata hello popsané výše je plán obnovení hello použité toobuild na portálu služby.
* **Volba**: Toto je nedílnou součást vámi vyžádaných hello služby a nelze po zapnutí vypnout. Pokud nechcete, aby tato informace odesílané toohello služby, nemusíte vytvářet plány obnovení v rámci této služby.

**Funkce: Mapování sítě**

* **Co znamená**: Tato funkce vám umožní toomap informace o síti z hello primární datové centrum toohello obnovení datového centra. Když hello virtuální počítače se obnoví na hello obnovení lokality, toto mapování funkce umožňuje navazování připojení k síti pro ně.
* **Informace shromážděné**: jako součást funkce mapování sítě hello hello služby shromažďuje, procesy a odesílá metadata hello hello logických sítí pro každou lokalitu (primární i datacenter).
* **Použití informací**: hello služby používá hello metadata toopopulate portálu služby kde můžete namapovat informace o síti hello.
* **Volba**: Toto je nedílnou součást vámi vyžádaných hello služby a nelze po zapnutí vypnout. Pokud nechcete, aby tato informace odesílané toohello služby, nepoužívejte hello funkce mapování sítě.

**Funkce: Převzetí služeb při selhání - plánované, neplánované testu**

* **Co znamená**: Tato funkce vám pomůže převzetí služeb při selhání virtuálního počítače z jednoho data spravovaná VMM center tooanother spravovat nástroj VMM datového centra. Akce Hello převzetí služeb při selhání je spuštěná uživatelem hello na jejich portálu služby. Možné důvody převzetí služeb při selhání zahrnují neplánované událostí (například v případě přírodní disaster0; hello plánované událostí (například datacenter Vyrovnávání zatížení); testovací převzetí služeb při selhání (například zkouška obnovení plánu).

Hello zprostředkovatele na serveru VMM hello získá upozornění hello události z hello služby a provede akci převzetí služeb při selhání na hostiteli Hyper-V hello prostřednictvím rozhraní VMM. Skutečné převzetí služeb při selhání hello virtuálního počítače z jednoho tooanother hostitele technologie Hyper-V (obvykle spuštěna v datovém centru různých "obnovení") se zpracovává souborem hello replikační technologii Windows Server 2012 nebo Windows Server 2012 R2 Hyper-V. Po dokončení převzetí služeb při selhání hello hello Provider nainstalovaný na serveru VMM hello hello "obnovení" datového centra odešle hello úspěch informace toohello služby.

* **Informace shromážděné**: hello služby používá hello výše informace toopopulate hello stav hello převzetí služeb při selhání akce informací na portálu služby.
* **Použití informací**: hello služby používá hello výše informace o následujícím způsobem:

  * Certifikát pro správu – používá se toohelp identifikovat a ověřovat hello zaregistrovat server VMM pro přístup k toohello služby. Hello služby používá hello část veřejného klíče toosecure certifikát hello token, který pouze hello zaregistrovat VMM server můžete získat přístup k. Hello server musí toouse této funkce služby toohello toogain tokenu přístupu.
  * Název serveru VMM hello – název serveru VMM hello je požadovaná tooidentify a komunikovat s hello odpovídající VMM server, na které hello cloudy jsou umístěny.
  * Cloud názvy ze serveru VMM hello – název cloudu hello je potřeba při použití cloudové služby hello spárování nebo zrušení spárování funkce popsané níže. Pokud se rozhodnete toopair vašeho cloudu z primární datové centrum s jiného cloudu v hello obnovení datového centra, jsou uvedené názvy hello všechny cloudy hello z hello obnovení datového centra.
* **Volba**: Toto je nedílnou součást vámi vyžádaných hello služby a nelze po zapnutí vypnout. Pokud nechcete, aby tato informace odesílané toohello služby, nepoužívejte tuto službu.

## <a name="next-steps"></a>Další kroky
Po spuštění toocheck převzetí služeb při selhání testovací prostředí funguje podle očekávání, [Další informace o](site-recovery-failover.md) různé typy převzetí služeb při selhání.
