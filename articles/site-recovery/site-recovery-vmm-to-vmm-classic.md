---
title: "Replikace virtuálních počítačů technologie Hyper-V v nástroji VMM do sekundární lokality (portál Azure classic) | Microsoft Docs"
description: "Tento článek popisuje, jak replikovat virtuální počítače Hyper-V v cloudech VMM do sekundární lokalita VMM s Azure Site Recovery."
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
ms.openlocfilehash: 6814f62f73bad272229205f4768dcce5947117d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Replikace virtuálních počítačů technologie Hyper-V v cloudech VMM do sekundární lokalita VMM
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Portál Classic](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Služba Azure Site Recovery přispívá ke strategii zachování plynulého chodu podniku a zotavení po havárii (BCDR) tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Počítače je možné replikovat do Azure nebo do sekundárního místního datového centra. Rychlý přehled najdete v článku [Co je Azure Site Recovery?](site-recovery-overview.md)

## <a name="overview"></a>Přehled
Tento článek popisuje, jak replikovat virtuální počítače Hyper-V na hostitelských serverech technologie Hyper-V, které jsou spravované v cloudech VMM do sekundární lokalita VMM pomocí Azure Site Recovery.

Článek obsahuje požadavků, ukazuje, jak nastavit trezor Site Recovery, nainstalujte zprostředkovatele Azure Site Recovery na zdroji a cílových serverů VMM, zaregistrujte server v trezoru, konfigurovat nastavení ochrany pro cloudy VMM a potom povolte ochranu pro virtuální počítače Hyper-V. Nakonec otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje podle očekávání.

Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="architecture"></a>Architektura
Následující obrázek ukazuje různé komunikační kanály a porty používané službou Azure Site Recovery pro orchestration a replikace

![Topologie e2e](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Než začnete
Ujistěte se, že máte zavedenou tyto požadavky:

| **Požadavky** | **Podrobnosti** |
| --- | --- |
| **Azure** |Potřebujete účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery |
| **VMM** |Musíte mít alespoň jeden server VMM.<br/><br/>VMM server by měl používat minimálně System Center 2012 SP1 s nejnovější kumulativní aktualizace.<br/><br/>Pokud chcete nastavení ochrany s jedním serverem VMM, je třeba alespoň dva cloudy nakonfigurovanému na serveru.<br/><br/>Pokud chcete nasadit ochranu s dvěma servery VMM, každý server musí mít alespoň jeden cloud nakonfigurované na primárním serveru VMM, které chcete chránit a k jednomu cloudu nakonfigurované na sekundárním serveru VMM, který chcete použít pro ochranu a obnovení<br/><br/>Všechny cloudy VMM musí mít profilovou sadu schopností Hyper-V.<br/><br/>Zdrojový cloud, který chcete chránit, musí obsahovat minimálně jednu skupinu hostitelů VMM. |
| **Hyper-V** |Budete potřebovat jeden nebo více servery hostitele technologie Hyper-V v primární a sekundární skupiny hostitelů VMM a jeden nebo více virtuálních počítačů na každém serveru hostitele technologie Hyper-V.<br/><br/>Na hostiteli a cílovým serverem, technologie Hyper-V musí běžet minimálně Windows Server 2012 s rolí Hyper-V a mají nainstalované nejnovější aktualizace.<br/><br/>Jakýkoli server Hyper-V obsahující virtuální počítače, které chcete chránit, musí být v cloudu VMM.<br/><br/>Pokud používáte technologii Hyper-V v clusteru, Všimněte si, že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte statické IP adresy na základě clusteru. Budete muset nakonfigurovat ručně. [Další informace](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) v najdete blogu Aidana Finna. |
| **Mapování sítě** |Můžete nakonfigurovat mapování sítě, abyste měli jistotu, že replikované virtuální počítače budou optimálně umístěny v sekundární hostitelské servery technologie Hyper-V po převzetí služeb při selhání a aby mohli připojit k příslušné sítě virtuálních počítačů. Pokud nenakonfigurujete mapování sítě, virtuální počítače replik se nepřipojí k žádné síti po převzetí služeb při selhání.<br/><br/>Pokud chcete nastavit mapování sítě během nasazení, ujistěte se, že virtuální počítače na serveru hostitele zdroje technologie Hyper-V jsou připojené k síti virtuálních počítačů nástroje VMM. Tuto síť musí být propojena na logickou síť, která souvisí s cloudem. < br /<br/>Cílový cloud na sekundárním serveru VMM, který používáte pro obnovení musí mít odpovídající síť virtuálních počítačů konfigurovanou a se pak musí být propojena na odpovídající logické sítě, který je přidružen cílový cloud. |
| **Mapování úložiště** |Ve výchozím nastavení při replikaci virtuálního počítače na zdrojovém serveru hostitele technologie Hyper-V na cílový server hostitele technologie Hyper-V, replikovaná data jsou uložena ve výchozím umístění, který je označen pro cílového hostitele technologie Hyper-V ve Správci technologie Hyper-V. Pro větší kontrolu nad se uloží replikovaná data můžete nakonfigurovat mapování úložiště<br/><br/> Ke konfiguraci mapování úložiště, musíte nastavit klasifikace úložiště na zdrojové a cílové servery VMM před zahájením nasazení. |

## <a name="step-1-create-a-site-recovery-vault"></a>Krok 1: Vytvoření trezoru Site Recovery
1. Přihlaste se k [portálu pro správu](https://portal.azure.com) ze serveru VMM, který chcete zaregistrovat.
2. Rozbalte položku **datové služby** > **služeb zotavení** a klikněte na tlačítko **trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. Jako **Název** zadejte popisný název pro identifikaci trezoru.
5. V **oblast** vyberte zeměpisnou oblast trezoru. Informace o tom, které oblasti jsou podporované, najdete v tématu s [podrobnostmi o cenách Azure Site Recovery](http://go.microsoft.com/fwlink/?LinkId=389880) v části s geografickou dostupností.
6. Klikněte na **Vytvořit trezor**.

    ![Vytvoření trezoru](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Ve stavovém řádku zkontrolujte, zda byl vytvořen v úložišti. Trezor bude uvedený jako **aktivní** na hlavní stránce Služeb zotavení.

## <a name="step-2-generate-a-vault-registration-key"></a>Krok 2: Vygenerování registračního klíče trezoru
Vygenerujte registrační klíč v trezoru. Po tom, co si stáhnete zprostředkovatele Azure Site Recovery a nainstalujete ho na server VMM, použijete tento klíč k registraci serveru VMM v trezoru.

1. Na stránce **Služby zotavení** kliknutím na trezor otevřete stránku Rychlý start. Stránku Rychlý Start je možné kdykoli otevřít pomocí ikony.

    ![Ikona Rychlý start](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)
2. V rozevíracím seznamu vyberte **mezi dvěma místními servery VMM**.
3. V **připravit servery VMM**, klikněte na tlačítko **vygenerovat soubor registračního klíče**. Soubor klíče se vygeneruje automaticky a je platný po dobu 5 dní od vygenerování. Pokud nejsou přístup k portálu Azure ze serveru VMM budete muset tento soubor zkopírovat na server.

    ![Registrační klíč](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Krok 3: Nainstalování zprostředkovatele Azure Site Recovery
1. Na **rychlý Start** stránky v **připravit servery VMM**, klikněte na tlačítko **stáhnout zprostředkovatele Microsoft Azure Site Recovery pro instalaci na serverech VMM** získat nejnovější verzi instalačního souboru zprostředkovatele.
2. Spusťte tento soubor na zdrojovém serveru VMM.

   > [!NOTE]
   > Pokud je server VMM nasazený v clusteru a instalujete zprostředkovatele poprvé, nainstalujte ho na aktivní uzel a dokončením instalace zaregistrujte server VMM v trezoru. Potom zprostředkovatele nainstalujte na ostatní uzly. Poznámka: Pokud zprostředkovatele upgradujete budete muset upgradovat na všech uzlech, protože jejich všech by měla běžet stejná verze zprostředkovatele.
   >
   >
3. Instalační program nemá několik **zkontrolujte předběžných požadavků** a požádá o oprávnění k zastavení služby VMM, chcete-li spustit instalační program zprostředkovatele. Služba VMM se po dokončení instalace automaticky restartuje. Pokud zprostředkovatele instalujete na clusteru VMM, budete vyzváni k zastavení role Cluster.
4. V nastavení služby **Microsoft Update** můžete vyjádřit výslovný souhlas se stahováním a instalací aktualizací. Když bude toto nastavení povoleno, aktualizace zprostředkovatele se budou instalovat v souladu s vašimi zásadami pro službu Microsoft Update.

    ![Aktualizace Microsoft Update](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)
5. Umístění instalace je nastavena  **<SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin**. Klikněte na tlačítko Instalovat zahájíte instalaci poskytovatele.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)
6. Po nainstalování zprostředkovatele kliknutím na **Zaregistrovat** zaregistrujte server v trezoru.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
7. V poli **Název trezoru** ověřte název trezoru, ve kterém bude server zaregistrovaný. Klikněte na *Další*.

    ![Registrace serveru](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)
8. V části **Připojení k internetu** určete, jak se zprostředkovatel, který běží na serveru VMM, připojí k internetu. Pokud chcete použít výchozí nastavení připojení k internetu nakonfigurované na serveru, vyberte **Připojit pomocí stávajícího nastavení proxy**.

    ![Nastavení internetu](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

   * Pokud chcete používat vlastní proxy server, měli byste ho nastavit před instalací zprostředkovatele. Při konfiguraci nastavení vlastního proxy serveru proběhne test, aby se zkontrolovalo připojení k proxy serveru.
   * Pokud používáte vlastní proxy server nebo váš výchozí proxy server vyžaduje ověření, musíte zadat podrobnosti o proxy serveru, včetně jeho adresy a portu.
   * Ze serveru VMM a hostitelů Hyper-V by měly být přístupné následující adresy URL
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Povolte IP adresy popsané v [rozsazích IP adres Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) a protokol HTTPS (443). Musíte také povolit rozsahy IP adres oblasti Azure, kterou budete chtít používat, a oblasti Západní USA.
   * Pokud používáte vlastní proxy server, vytvoří se automaticky účet VMM RunAs (DRAProxyAccount) pomocí zadaných přihlašovacích údajů proxy serveru. Proxy server nakonfigurujte tak, aby tento účet bylo možné úspěšně ověřit. Nastavení účtu VMM RunAs můžete upravovat v konzole VMM. Uděláte to tak, že otevřete pracovní prostor **Nastavení**, rozbalíte možnost **Zabezpečení**, kliknete na **Účty Spustit jako** a pak upravíte heslo pro DRAProxyAccount. Budete muset službu VMM restartovat, aby se toto nastavení projevilo.
9. V části **Registrační klíč** vyberte klíč, který jste si stáhli z Azure Site Recovery a zkopírovali na server VMM.
10. Nastavení šifrování se používá pouze při replikaci virtuálních počítačů Hyper-V v cloudech VMM do Azure. Pokud provádíte replikaci do sekundární lokality, tak se nepoužívá.
11. Do pole **Název serveru** zadejte popisný název, který bude identifikovat server VMM v trezoru. V konfiguraci clusteru zadejte název role clusteru VMM.
12. V části **Synchronizovat metadata cloudu** vyberte, zda chcete synchronizovat metadata pro všechny cloudy na serveru VMM v trezoru. Tuto akci stačí na každém serveru provést pouze jednou. Pokud nechcete provádět synchronizaci se všemi cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech cloudu v konzole VMM.
13. Dokončete proces kliknutím na **Další**. Po registraci načte Azure Site Recovery metadata ze serveru VMM. Server se zobrazí v **servery VMM** > **servery** v trezoru.

    ![Servery](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Instalace pomocí příkazového řádku
Zprostředkovatele Azure Site Recovery můžete také nainstalovat z příkazového řádku. Tato metoda slouží k instalaci zprostředkovatele na serveru jádra pro Windows Server 2012 R2.

1. Stáhněte si instalační soubor zprostředkovatele a registrační klíč do složky. Například C:\ASR.
2. Zastavte službu System Center Virtual Machine Manager
3. Vyextrahujte instalační program zprostředkovatele tak, že spustíte tyto příkazy z příkazového řádku s **správce** oprávnění:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Spuštěním instalace zprostředkovatele:

        C:\ASR> setupdr.exe /i
5. Zaregistrujte zprostředkovatele spuštěním:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Kde jsou parametry:

* **/ Credentials**: povinný parametr, který určuje umístění, ve kterém je umístěn soubor registračního klíče  
* **/FriendlyName**: Povinný parametr pro název serveru hostitele technologie Hyper-V, který se zobrazí na portálu Azure Site Recovery.
* **/ EncryptionEnabled**: volitelný parametr, který budete muset použít pouze v nástroji VMM pro scénáře Azure, pokud je třeba šifrování virtuálních počítačů v klidovém stavu uložených v Azure. Zkontrolujte, zda má název souboru, který zadáte **.pfx** rozšíření.
* **/proxyAddress**: Volitelný parametr, který určuje adresu proxy serveru.
* **/proxyport**: Volitelný parametr, který určuje port proxy serveru.
* **/ proxyusername**: volitelný parametr, který určuje uživatelské jméno Proxy (pokud proxy server vyžaduje ověření).
* **/ proxypassword**: volitelný parametr, který určuje heslo pro ověřování pomocí serveru proxy (pokud proxy server vyžaduje ověření).  

## <a name="step-4-configure-cloud-protection-settings"></a>Krok 4: Konfigurace cloudu nastavení ochrany
Po registraci jsou servery VMM, můžete nakonfigurovat nastavení ochrany cloudu. Pokud jste povolili možnost **synchronizovat data cloudu s trezorem** při instalaci zprostředkovatele, zobrazí se všechny cloudy na serveru VMM v **chráněné položky** v trezoru. Jestliže jste je určitý cloud můžete synchronizovat s Azure Site Recovery v **Obecné** stránky vlastností cloudu v konzole nástroje VMM.

![Publikovaný cloud](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Na stránce Rychlý Start klikněte na **Nastavení ochrany pro cloudy VMM**.
2. Na **Cloudech VMM** vyberte cloud, který chcete nakonfigurovat a přejděte k **konfigurace** kartě.
3. V **cíl**, vyberte **VMM**.
4. V **cílové umístění**, vyberte na místě serveru VMM, který spravuje cloudu, kterou chcete použít pro obnovení.
5. V **cílí na cloud**, vyberte cílový cloud, kterou chcete použít pro převzetí služeb při selhání virtuálních počítačů v cloudu zdroje. Poznámky:

   * Doporučujeme vám, že jste vybrali cílový cloud, který splňuje požadavky na obnovení pro virtuální počítače, který budete chránit.
   * Cloud lze přiřadit pouze jeden cloud pár – buď jako primární nebo cílový cloud.
6. V **frekvence kopírování**, určete, jak často se mají data synchronizovat mezi zdrojovým a cílovým umístěním 5he. Všimněte si, že toto nastavení platí jen pokud hostitele Hyper-V se systémem Windows Server 2012 R2. Pro další servery se používá výchozí nastavení pět minut.
7. V **další body obnovení**, zadejte, zda chcete vytvořit další body. Hodnota výchozí nula znamená, že je na hostitelském serveru repliky uložený jenom nejnovější bod obnovení pro primární virtuální počítač. Všimněte si, že povolení více bodů obnovení vyžaduje dodatečné úložiště pro snímky, které jsou uloženy v každém bodu obnovení. Ve výchozím nastavení vytvořeny body obnovení jsou každou hodinu, takže každý bod obnovení obsahuje data za jednu hodinu. Hodnota bodu obnovení, který přiřadíte pro virtuální počítač v konzole VMM by neměl být menší než hodnota, která přiřadíte v konzole Azure Site Recovery.
8. V **frekvence snímků konzistentní vzhledem k aplikacím**, určete, jak často se mají vytvářet snímky konzistentní s aplikacemi. Technologie Hyper-V používá dva typy snímků – standardní snímek, což je přírůstkový snímek celého virtuálního počítače, a snímek konzistentní vzhledem k aplikacím, který vytvoří snímek dat aplikací ve virtuálním počítači v daném okamžiku. Snímky konzistentní vzhledem k aplikacím využívají službu Stínová kopie svazku (VSS), aby bylo zajištěno, že aplikace budou při pořízení snímku v konzistentním stavu. Pokud povolíte snímky konzistentní vzhledem k aplikacím, bude to mít vliv na výkon aplikací běžících na zdrojových virtuálních počítačích. Zajistěte, aby byla hodnota, kterou nastavíte, menší než počet dalších bodů obnovení, které nakonfigurujete.

    ![Konfigurovat nastavení ochrany](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)
9. V **kompresi přenosu dat**, zadejte, zda by měl být komprimované replikovaná data, která se přenáší.
10. V **ověřování**, zadejte, jak ověření přenosů mezi primárními a obnovovacími hostitelské servery technologie Hyper-V. Pokud máte fungující Kerberos prostředí nakonfigurovat, vyberte HTTPS. Azure Site Recovery automaticky nakonfiguruje certifikáty pro ověřování pomocí protokolu HTTPS. Není nutná žádná ruční konfigurace. Pokud vyberete protokolu Kerberos, lístek protokolu Kerberos se použije pro vzájemné ověřování serverů hostitele. Ve výchozím nastavení bude otevřen port 8083 a 8084 (pro certifikáty) v bráně Windows Firewall na hostitelských serverech technologie Hyper-V. Všimněte si, že toto nastavení platí jen pro hostitelské servery technologie Hyper-V v systému Windows Server 2012 R2.
11. V **Port**, změňte číslo portu, na kterém hostitelských počítačích zdrojové a cílové naslouchat provoz replikace. Například je může upravit nastavení, pokud chcete použít (QoS) Quality of Service šířku pásma sítě, omezení šířky pásma pro přenosy replikace. Zkontrolujte, že port není používán jinou aplikaci a že je otevřen v nastavení brány firewall.
12. V **metodu replikace**, zadejte, jak počáteční replikaci dat ze zdrojového do cílového umístění se budou zpracovávat, před zahájením regulární replikace:

    * **Přes síť**– kopírování dat přes síť může být časově náročná a prostředky. Doporučujeme používat tuto možnost, pokud cloud obsahuje virtuální počítače s relativně malý virtuální pevné disky, a pokud primární lokalita je připojená k sekundární lokalitě přes připojení s celou šířku pásma. Můžete zadat, zda by měl kopie spustit okamžitě, nebo vyberte čas. Pokud používáte sítě replikace, doporučujeme, abyste naplánovali mimo špičku.
    * **Offline**– tato metoda určuje, že počáteční replikace se provede pomocí externího média. Je užitečné, pokud chcete, aby se zabránilo snížení výkonu sítě, nebo pro geograficky vzdálených umístěních. Chcete-li tuto metodu použijte, zadáte umístění exportu na zdrojový cloud a umístění importovat na cílový cloud. Když povolíte ochranu pro virtuální počítač, virtuální pevný disk se zkopírují do umístění zadaného exportu. Odeslat do cílové lokality a zkopírujte jej do umístění importu. Systém zkopíruje importované informace na virtuální počítače repliky.
13. Vyberte **odstranit virtuální počítač repliky** k určení, že virtuální počítač repliky měla by být odstraněna, pokud zastavíte ochranu virtuálního počítače tak, že vyberete **odstranit ochranu pro virtuální počítač** možnost na kartě virtuální počítače vlastnosti cloudu. Toto nastavení povoleno, při zakázání ochrany virtuálního počítače se odebere z Azure Site Recovery, odeberou se nastavení Site Recovery pro virtuální počítač v konzole VMM a replika odstraněn.

    ![Konfigurovat nastavení ochrany](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

Po uložení nastavení se vytvoří úloha, kterou je možné monitorovat na kartě **Úlohy**. Pro všechny hostitelské servery technologie Hyper-V ve zdrojovém cloudu VMM se nakonfiguruje replikace. Nastavení cloudu se dají změnit na **konfigurace** kartě. Pokud chcete upravit cílový umístění nebo cílový cloud musíte odebrat konfiguraci cloudu a potom cloud znovu nakonfigurovat.

### <a name="prepare-for-offline-initial-replication"></a>Příprava pro počáteční replikaci prováděnou offline
Budete muset provést následující akce, jež mají připravit na úvodní replikaci offline:

* Na zdrojovém serveru zadáte cestu umístění, ze kterého bude export dat probíhat. Přiřaďte úplné řízení pro systém souborů NTFS a oprávnění ke sdílení do služby VMM na cestu pro export. Na cílovém serveru zadáte cestu umístění, ze kterého dojde k importu. Přiřaďte stejné oprávnění pro tuto cestu importu.
* Pokud je import nebo export cesta sdílená, přiřaďte správce, Power Users, Print Operator nebo operátor serveru členství ve skupině pro účet služby VMM na vzdáleném počítači, na kterém sdílený se nachází.
* Používáte-li přidat hostitele, na cestám pro import a export všechny účty spustit jako, přiřaďte pro čtení a oprávnění k zápisu do účty spustit jako v nástroji VMM.
* Import a export sdílené složky nesmí nacházet na libovolném počítači se používá jako server hostitele technologie Hyper-V, protože konfigurace zpětné smyčky není podporována technologií Hyper-V.
* Ve službě Active Directory na každém Hyper-V serveru hostitele, který obsahuje virtuální počítače, který chcete chránit, povolit a nakonfigurovat omezené delegování důvěřovat vzdálených počítačích, na kterých cestám pro import a export nacházejí, následujícím způsobem:
  1. Na řadiči domény otevřete **Active Directory Users and Computers**.
  2. Ve stromu konzoly klikněte na tlačítko **DomainName** > **počítače**.
  3. Klikněte pravým tlačítkem na název serveru hostitele technologie Hyper-V > **vlastnosti**.
  4. Na **delegování** kartě klikněte na tlačítko T**koroze tomuto počítači pro delegování pouze určeným službám**.
  5. Klikněte na tlačítko **používající libovolný protokol pro ověřování**.
  6. Klikněte na tlačítko **přidat** > **uživatelé a počítače**.
  7. Zadejte název počítače, který je hostitelem cestu pro export > **OK**. Ze seznamu dostupných služeb, podržte stisknutou klávesu CTRL a klikněte na tlačítko **cifs** > **OK**. Opakujte pro název počítače, který je hostitelem cestu importu. Opakujte podle potřeby pro další hostitelské servery technologie Hyper-V.

## <a name="step-5-configure-network-mapping"></a>Krok 5: Konfigurace mapování sítě
1. Na obrazovce Rychlý start klikněte na **Mapovat sítě**.
2. Vyberte zdrojový server VMM, ze kterého chcete mapovat sítě a cílovém serveru VMM, ke kterému se mapovat sítě. Zobrazí se seznam zdrojové sítě a jejich přidružené cílové sítě. Pro sítě, které nejsou namapované aktuálně se zobrazí na prázdnou hodnotu.
3. Vyberte síť, v **sítě na zdroji** > **mapy**. Služba zjistí sítě virtuálních počítačů na cílovém serveru a zobrazí je. Klikněte na informační ikonu u zdrojové a cílové názvy sítě zobrazíte podsítě pro každou síť.

    ![Konfigurace mapování sítě](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)
4. V dialogovém okně vyberte jednu z sítě virtuálních počítačů cílovém serveru VMM.

    ![Výběr cílové sítě](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)
5. Když vyberete cílové sítě, zobrazí se počet chráněných cloudů, které používají zdrojové síti. Zobrazí se také k dispozici cílové sítě, které jsou spojené s cloudy sloužící k ochraně. Doporučujeme vám, že vyberete Cílová síť, která je k dispozici pro všechny cloudy, který používáte pro ochranu. Nebo můžete také přejít na serveru VMM a upravit vlastnosti cloudu pro přidání logické sítě odpovídající síti virtuálních počítačů, který chcete vybrat.
6. Kliknutím na značku zaškrtnutí dokončete proces mapování. Spustí úloha pro sledování postupu mapování. Můžete zobrazit na **úlohy** kartě.

## <a name="step-6-configure-storage-mapping"></a>Krok 6: Konfigurace mapování úložiště
Ve výchozím nastavení při replikaci virtuálního počítače na zdrojovém serveru hostitele technologie Hyper-V na cílový server hostitele technologie Hyper-V, replikovaná data jsou uložena ve výchozím umístění, který je označen pro cílového hostitele technologie Hyper-V ve Správci technologie Hyper-V. Pro větší kontrolu nad se uloží replikovaná data můžete nakonfigurovat úložiště mapování následujícím způsobem:

1. Zadejte klasifikace úložiště na zdrojovém i cílovém servery VMM. [Další informace](https://technet.microsoft.com/library/gg610685.aspx). Klasifikace musí být k dispozici pro hostitelské servery technologie Hyper-V v cloudech zdroje a cíle. Klasifikace nemusíte mít stejný typ úložiště. Například můžete namapovat klasifikace zdroj, který obsahuje sdílené složky protokolu SMB na cílové klasifikaci, který obsahuje sdílené svazky clusteru.
2. Po klasifikace jsou na místě můžete vytvořit mapování. To uděláte, na **rychlý Start** stránky > **mapování úložiště**.
3. Klikněte **úložiště** kartě > **mapování klasifikace úložiště**.
4. Na **mapování klasifikace úložiště** vyberte klasifikace na zdrojové a cílové servery VMM. Uložte nastavení.

    ![Výběr cílové sítě](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)

## <a name="step-7-enable-virtual-machine-protection"></a>Krok 7: Povolení ochrany virtuálního počítače
Po správném nakonfigurování serverů, cloudů a sítí můžete povolit ochranu pro virtuální počítače v cloudu.

1. Na **virtuální počítače** v cloudu, ve kterém je umístěn virtuální počítač, klikněte na tlačítko **povolení ochrany** > **přidat virtuální počítače**.
2. Ze seznamu virtuálních počítačů v cloudu vyberte ten, který chcete chránit.

    ![Povolení ochrany virtuálního počítače](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)
3. Sledovat průběh akce povolení ochrany ve **úlohy** kartě, včetně počáteční replikace. Po spuštění úlohy dokončit ochranu virtuálního počítače je připraven k převzetí služeb při selhání. Když je povolena ochrana a virtuální počítače jsou replikovány, uvidíte je v Azure.

    ![Úloha ochrany virtuálního počítače](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

> [!NOTE]
> Můžete také povolit ochranu pro virtuální počítače v konzole nástroje VMM. Klikněte na tlačítko **povolení ochrany** na panelu nástrojů v **Azure Site Recovery** ve vlastnostech virtuálního počítače.
>
>

### <a name="on-board-existing-virtual-machines"></a>Integrovaného existující virtuální počítače
Pokud máte existující virtuální počítače v nástroji VMM, které jsou replikace s replikou technologie Hyper-V musíte zařadit do provozu je pro ochranu Azure Site Recovery následujícím způsobem:

1. Ověřte, že máte primárních a sekundárních cloudech. Zajistěte, aby serveru technologie Hyper-V, který hostuje stávající virtuální počítač nacházel v primárním cloudu a že serveru technologie Hyper-V, který hostuje virtuální počítač repliky se nachází v sekundární cloudu. Ujistěte se, že jste nakonfigurovali nastavení ochrany pro cloudy. Nastavení by měl odpovídat aktuálně konfigurovaných pro repliku technologie Hyper-V. V opačném případě replikaci virtuálního počítače nemusí fungovat podle očekávání.
2. Potom povolte ochranu pro primární virtuální počítač. Azure Site Recovery a VMM zajišťují, že se zjistí stejné hostitele repliky a virtuální počítač, a bude znovu použít a opětovnému zavedení replikace nastavení během konfigurace cloudu pomocí Azure Site Recovery.

## <a name="test-your-deployment"></a>Otestujte nasazení
Chcete nasazení otestovat můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač, nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spustit testovací převzetí služeb pro plán.  Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti.

### <a name="create-a-recovery-plan"></a>Vytvoření plánu obnovení
1. Na **plány obnovení** , klikněte na **vytvořit plán obnovení**.
2. Zadejte název plánu obnovení a zdrojové a cílové servery VMM. Zdrojový server musí mít virtuální počítače, které jsou povolené pro převzetí služeb při selhání a obnovení. Vyberte **technologie Hyper-V** k zobrazení pouze cloudy, které jsou nakonfigurované pro replikaci technologie Hyper-V.

    ![Vytvoření plánu obnovení](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)
3. V **vybrat virtuální počítač**, vyberte replikační skupiny. Všechny virtuální počítače přidružené ke skupině replikace bude vybrána a přidat do plánu obnovení. Tyto virtuální počítače se přidají do výchozí skupiny plánu obnovení – Skupina 1. v případě potřeby můžete přidat další skupiny. Všimněte si, že po replikaci virtuálního počítače se spustí v souladu s pořadí skupiny plánu obnovení.

    ![Přidat virtuální počítače](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

Po vytvoření plánu obnovení se zobrazí v seznamu na **plány obnovení** kartě.

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
1. Na kartě **Plány obnovení** vyberte plán a klikněte na **Testovací převzetí služeb při selhání**.
2. Na **potvrzení převzetí služeb při selhání** vyberte **žádné**. Všimněte si, že se tato možnost povolena neúspěšný přes virtuální počítače repliky se nepřipojí k žádné síti. Tím otestujete, že virtuální počítač převezme podle očekávání, ale není testovací prostředí sítě replikace. Podívejte se na postup [spustit testovací převzetí služeb](site-recovery-failover.md) další podrobnosti o tom, jak používat různé možnosti sítě.
3. Testovací virtuální počítač bude vytvořen na stejném hostiteli jako hostitele, ve kterém se nachází virtuální počítač repliky. Přidá do stejné cloudu, ve kterém je umístěn virtuální počítač repliky.

### <a name="run-a-recovery-plan"></a>Spuštění plánu obnovení
Po replikaci virtuálního počítače repliky nemusí mít IP adresu, která je stejná jako IP adresa primárního virtuálního počítače. Virtuální počítače aktualizuje server DNS, který používají po spuštění. Můžete také přidat skript pro aktualizaci serveru DNS zajistit včasné aktualizace.

#### <a name="script-to-retrieve-the-ip-address"></a>Skript pro načtení IP adresu
Spusťte tento ukázkový skript pro načtení IP adresu.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-to-update-dns"></a>Skript pro aktualizaci DNS
Tento ukázkový skript k aktualizaci služby DNS, zadání adresy IP, který jste získali pomocí předchozí ukázkový skript spusťte.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Informace o ochraně osobních údajů pro obnovení lokality
Tato část obsahuje další ochrany osobních údajů pro službu Microsoft Azure Site Recovery (dále jen "služba"). Chcete-li zobrazit prohlášení o ochraně osobních údajů pro služby Microsoft Azure, najdete v článku [prohlášení o ochraně osobních údajů pro Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=324899)

**Funkce: registrace**

* **Co znamená**: zaregistruje server se službou tak, aby virtuální počítače můžou být chráněné
* **Informace shromážděné**: po registraci služby shromažďuje, procesy a odesílá informace o certifikátu správy ze serveru VMM, který je určen k poskytování zotavení po havárii pomocí názvu služby serveru VMM a název cloudy virtuálních počítačů na serveru VMM.
* **Použití informací**:

  * Certifikát pro správu – slouží k identifikaci a ověření zaregistrovaný server VMM pro přístup ke službě. Služba používá k zabezpečení token, který zaregistrovaný server VMM můžete získat přístup k části veřejného klíče certifikátu. Server musí používat tento token pro přístup k funkcím služby.
  * Název serveru VMM – název serveru VMM je potřeba identifikovat a komunikovat s příslušného serveru VMM, na kterém jsou umístěny cloudy.
  * Cloud názvy ze serveru VMM – název cloudu je potřeba při použití služby cloudu spárování nebo zrušení spárování funkce popsané níže. Pokud se rozhodnete spárujte vašeho cloudu z primární datové centrum s jiného cloudu v datovém centru obnovení, jsou uvedené názvy všechny cloudy z obnovení datového centra.
* **Volba**: tyto informace je nedílnou součást vámi vyžádaných proces registrace služby, protože pomáhá vám a službu identifikovat server VMM, pro které chcete zajistit ochranu Azure Site Recovery, a aby bylo možno zjistit správné zaregistrovaný server VMM. Pokud nechcete službě odesílat výše uvedené informace, nepoužívejte tuto službu. Pokud váš server zaregistrovat a později se chcete zrušení její registrace, můžete tak učinit odstraněním informace o serveru VMM z portálu služby (což je portál Azure).

**Funkce: Povolte ochranu Azure Site Recovery**

* **Co znamená**: Azure Site Recovery Provider nainstalovaný na serveru VMM je přenos pro komunikaci se službou. Zprostředkovatel je dynamická knihovna (DLL) hostované v procesu VMM. Po instalaci zprostředkovatele bude tato funkce "Obnovení Datacentra" získá povolena v konzole pro správu VMM. Všechny nové nebo existující virtuální počítače v cloudu můžete povolit vlastnost s názvem "Obnovení Datacentra" k ochraně virtuálního počítače. Jakmile je tato vlastnost nastavena, odešle zprostředkovatel název a ID virtuálního počítače ke službě. Virtuální ochrana povolena replikace technologie Windows Server 2012 nebo Windows Server 2012 R2 Hyper-V. Získá data virtuálního počítače replikují z jednoho hostitele technologie Hyper-V na jiný (obvykle umístěné v datovém centru různých "obnovení").
* **Informace shromážděné**: Služba Service shromažďuje, procesy a odesílá metadata pro virtuální počítač, který obsahuje název, ID, virtuální sítě a název cloudu, ke které patří.
* **Použití informací**: Služba Service používá tyto informace k naplnění informace o virtuálním počítači na portálu služby.
* **Volba**: Toto je základní součástí služby a nelze po zapnutí vypnout. Pokud nechcete, aby tato informace odesílané do služby, není povolte ochranu Azure Site Recovery pro všechny virtuální počítače. Všimněte si, že všechna data odesílaná zprostředkovatelem ke službě se neodesílají přes protokol HTTPS.

**Funkce: Plán obnovení**

* **Co znamená**: Tato funkce vám pomůže sestavit plán orchestration pro "obnovení" datového centra. Můžete definovat pořadí, ve kterém má být spuštěn virtuální počítače nebo skupiny virtuálních počítačů v lokalitě pro obnovení. Můžete také zadat všechny automatizované skripty být spustit nebo všechny ručně prováděné akce, které mají být provedeny v době obnovení pro každý virtuální počítač. Převzetí služeb při selhání (popsaná v další části) se obvykle aktivuje na úrovni plán obnovení pro koordinované obnovení.
* **Informace shromážděné**: Služba Service shromažďuje, procesy a odesílá metadata pro plán obnovení, včetně metadata virtuálního počítače a metadata všechny skripty pro automatizaci a poznámky k ruční akce.
* **Použití informací**: metadata popsané výše slouží k vytvoření plánu obnovení na portálu služby.
* **Volba**: Toto je základní součástí služby a nelze po zapnutí vypnout. Pokud nechcete, aby tato informace odesílané do služby, nemusíte vytvářet plány obnovení v rámci této služby.

**Funkce: Mapování sítě**

* **Co znamená**: Tato funkce umožňuje mapovat informace o síťové z primární datové centrum pro obnovení datového centra. Když virtuální počítače se obnoví na lokalitě pro obnovení, toto mapování funkce umožňuje navazování připojení k síti pro ně.
* **Informace shromážděné**: jako součást funkce mapování sítě, tato služba shromažďuje, procesy a odesílá metadata logické sítě pro každou lokalitu (primární i datacenter).
* **Použití informací**: Služba Service používá metadata k naplnění portálu služby kde můžete namapovat informace o síti.
* **Volba**: Toto je základní součástí služby a nelze po zapnutí vypnout. Pokud nechcete, aby tato informace odesílané do služby, nepoužívejte funkci mapování sítě.

**Funkce: Převzetí služeb při selhání - plánované, neplánované testu**

* **Co znamená**: Tato funkce pomáhá převzetí služeb při selhání virtuálního počítače z jednoho spravovat nástroj VMM datového centra do jiné spravovat nástroj VMM datového centra. Akce převzetí služeb při selhání je spuštěná uživatelem na jejich portálu služby. Možné důvody převzetí služeb při selhání zahrnují neplánované událostí (například v případě přírodní disaster0; plánované událostí (například datacenter Vyrovnávání zatížení); testovací převzetí služeb při selhání (například zkouška obnovení plánu).

Zprostředkovatel na serveru VMM získá oznámení události ze služby a provede akci převzetí služeb při selhání na hostiteli technologie Hyper-V prostřednictvím rozhraní VMM. Skutečné převzetí služeb při selhání virtuálního počítače z jednoho hostitele technologie Hyper-V na jiný (obvykle spuštěna v datovém centru různých "obnovení") se provádí replikace technologie Windows Server 2012 nebo Windows Server 2012 R2 Hyper-V. Po dokončení převzetí Provider nainstalovaný na serveru VMM "obnovení" datového centra odešle informace o úspěšnosti ke službě.

* **Informace shromážděné**: Služba Service používá tyto informace k naplnění stav informace o převzetí služeb při selhání akce na portálu služby.
* **Použití informací**: Služba Service používá výše uvedené informace o následujícím způsobem:

  * Certifikát pro správu – slouží k identifikaci a ověření zaregistrovaný server VMM pro přístup ke službě. Služba používá k zabezpečení token, který zaregistrovaný server VMM můžete získat přístup k části veřejného klíče certifikátu. Server musí používat tento token pro přístup k funkcím služby.
  * Název serveru VMM – název serveru VMM je potřeba identifikovat a komunikovat s příslušného serveru VMM, na kterém jsou umístěny cloudy.
  * Cloud názvy ze serveru VMM – název cloudu je potřeba při použití služby cloudu spárování nebo zrušení spárování funkce popsané níže. Pokud se rozhodnete spárujte vašeho cloudu z primární datové centrum s jiného cloudu v datovém centru obnovení, jsou uvedené názvy všechny cloudy z obnovení datového centra.
* **Volba**: Toto je základní součástí služby a nelze po zapnutí vypnout. Pokud nechcete, aby tato informace odesílané do služby, nepoužívejte tuto službu.

## <a name="next-steps"></a>Další kroky
Po spuštění převzetí služeb při selhání zkontrolujte prostředí funguje podle očekávání, [Další informace o](site-recovery-failover.md) různé typy převzetí služeb při selhání.
