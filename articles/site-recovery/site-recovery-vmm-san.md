---
title: "aaaReplicate virtuálních počítačů Hyper-V v nástroji VMM se sítě SAN pomocí Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooreplicate technologie Hyper-V virtuální počítače mezi dvěma lokalitami s Azure Site Recovery pomocí replikace SAN."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: eb480459-04d0-4c57-96c6-9b0829e96d65
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: cee4ff519a8e9e7f29e17e923a9533fb339634b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-vms-in-vmm-clouds-tooa-secondary-site-with-azure-site-recovery-by-using-san"></a>Replikace virtuálních počítačů technologie Hyper-V ve VMM cloudy tooa sekundární lokalitu s Azure Site Recovery pomocí sítě SAN


Pomocí tohoto článku, pokud chcete, aby toodeploy [Azure Site Recovery](site-recovery-overview.md) toomanage replikaci virtuálních počítačů Hyper-V (spravované v cloudech System Center Virtual Machine Manager) tooa sekundární lokalita VMM, pomocí Azure Site Recovery na portálu classic hello. Tento scénář není k dispozici v hello nový portál Azure.



Odeslat všechny komentáře v hello konci tohoto článku. Získejte odpovědi tootechnical dotazy v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="why-replicate-with-san-and-site-recovery"></a>Proč replikovat pomocí sítě SAN a Site Recovery?

* Síť SAN představuje řešení podnikové úrovni, škálovatelné replikace tak, aby primární lokalitu obsahující technologie Hyper-V síti SAN můžete replikovat logické jednotky LUN tooa sekundární lokality pomocí sítě SAN. Úložiště spravované nástrojem VMM, a se službou Site Recovery orchestrovat replikaci a převzetí služeb při selhání.
* Site Recovery ve spolupráci se několik [partnery úložiště sítě SAN](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) tooprovide replikace napříč úložiště Fibre Channel a iSCSI.  
* Použijte existující síť SAN infrastruktury tooprotect důležité aplikace nasazené v clusterech technologie Hyper-V. Virtuální počítače, lze replikovat jako skupina tak, aby vícevrstvé aplikace mohou být přebrány konzistentně.
* Replikace sítě SAN zajišťuje konzistenci replikace v rámci různých vrstev aplikace s synchronizovaná replikace pro nízkou RTO a plánovaný bod obnovení a synchronního replikace pro vysokou flexibilitu (v závislosti na vaší možnosti pole úložiště).  
* Můžete spravovat úložiště sítě SAN v hello prostředků infrastruktury VMM a použít SMI-S v nástroji VMM toodiscover existující úložiště.  

Poznámky:
- Site-to-site s replikací pomocí sítě SAN, není k dispozici v hello portálu Azure. Je k dispozici na portálu classic hello. Nové trezory nelze vytvořit na portálu classic hello. Je možné udržovat existující trezorů.
- Replikace z tooAzure sítě SAN se nepodporuje.
- Nelze replikovat sdílené soubory Vhdx nebo logické jednotky (LUN), které jsou přímo připojené tooVMs přes iSCSI nebo Fibre Channel. Clustery hostů, lze replikovat.


## <a name="architecture"></a>Architektura

![Architektura sítě SAN](./media/site-recovery-vmm-san/architecture.png)

- **Azure**: nastavíte trezor Site Recovery v hello portálu Azure.
- **Úložiště SAN**: úložiště sítě SAN je spravován na hello prostředků infrastruktury. Přidejte zprostředkovatele úložiště hello, vytvoření klasifikací úložiště a nastavit fondy úložiště.
- **VMM nebo Hyper-V**: doporučujeme, aby server VMM v každé lokalitě. Nastavit privátní cloudy VMM a umístěte Clustery Hyper-V v tyto cloudy. Během nasazení hello zprostředkovatele Azure Site Recovery je nainstalován na každý server VMM a hello server je zaregistrován v trezoru hello. Hello zprostředkovatele komunikuje s hello Site Recovery service toomanage replikace, převzetí služeb při selhání a navrácení služeb po obnovení.
- **Replikace**: Po nastavení úložiště v nástroji VMM a konfigurovat replikaci ve službě Site Recovery, se replikují mezi hello primární a sekundární úložiště SAN. TooSite obnovení je odesílána žádná data replikace.
- **Převzetí služeb při selhání**: povolit převzetí služeb při selhání na portálu Site Recovery hello. Není nulové ztráty dat během převzetí služeb při selhání, protože je synchronní replikace.
- **Navrácení služeb po obnovení**: toofail zpět, povolíte zpětnou replikaci tootransfer rozdílové změny z primární lokality toohello hello sekundární lokalitě. Po dokončení zpětné replikace spustíte z sekundární tooprimary plánované převzetí služeb při selhání. Tato plánované převzetí služeb při selhání zastaví hello repliky virtuální počítače na sekundární lokalitě hello a spustí je v primární lokalitě hello.


## <a name="before-you-start"></a>Než začnete


**Požadavky** | **Podrobnosti**
--- | ---
**Azure** |Potřebujete účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). [Další informace](https://azure.microsoft.com/pricing/details/site-recovery/) o cenách za Site Recovery Vytvoření tooconfigure trezoru Azure Site Recovery a správu replikace a převzetí služeb při selhání.
**VMM** | Můžete použít jeden server VMM a replikovat mezi cloudy jiné, ale doporučujeme jeden VMM v primární lokalitě hello a po jednom v sekundární lokalitě hello. VMM se dá nasadit jako fyzický nebo virtuální samostatný server nebo jako cluster. <br/><br/>Hello VMM server by měl běžet System Center 2012 R2 nebo novějším s nejnovější kumulativní aktualizace hello.<br/><br/> Je nutné nakonfigurovat alespoň jeden cloud na primárním serveru VMM hello chcete tooprotect a k jednomu cloudu nakonfigurované na sekundárním serveru VMM hello chcete toouse převzetí služeb při selhání.<br/><br/> Hello zdrojový cloud musí obsahovat jeden nebo více skupin hostitelů VMM.<br/><br/> Sada profilů hello kapacity technologie Hyper-V musí mít všechny cloudy VMM.<br/><br/> Další informace o nastavení cloudů VMM najdete v tématu [nasazení privátního cloudu virtuálních počítačů](https://technet.microsoft.com/en-us/system-center-docs/vmm/scenario/cloud-overview).
**Hyper-V** | Budete potřebovat minimálně jeden cluster technologie Hyper-V v primárních a sekundárních cloudech VMM.<br/><br/> Hello zdroj technologie Hyper-V clusteru musí obsahovat jeden nebo více virtuálních počítačů.<br/><br/> skupiny hostitelů VMM Hello v primárních a sekundárních lokalit hello musí obsahovat alespoň jeden z hello Clustery Hyper-V.<br/><br/>Hello hostitele a cílové servery Hyper-V musí být spuštěn systém Windows Server 2012 nebo novější s hello technologie Hyper-V role a hello nejnovější aktualizace nainstalované.<br/><br/> Pokud používáte technologii Hyper-V v clusteru a máte statické IP adresy na základě clusteru, není automaticky vytvořit zprostředkovatele clusteru. Je nutné ho nakonfigurovat ručně. Další informace najdete v tématu [Příprava hostitelské clustery pro repliku technologie Hyper-V](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters).
**Úložiště SAN** | Můžete replikovat virtuální počítače hostovaný clusterovaný s úložiště iSCSI nebo kanál, nebo pomocí sdílených virtuálních pevných disků (vhdx).<br/><br/> Budete potřebovat dvě pole SAN: jeden v primární lokalitě hello a jeden v hello sekundární lokality.<br/><br/> Síťové infrastruktury měli nastavit mezi poli hello. Partnerský vztah a replikace by měl být nakonfigurovaný. Replikace licencí měli nastavit v souladu s požadavky na pole úložiště hello.<br/><br/>Nastavení sítě mezi servery hostitele hello technologie Hyper-V a pole úložiště hello tak, aby hostitele může komunikovat s logických jednotek úložiště pomocí standardu iSCSI nebo Fibre Channel.<br/><br/> Zkontrolujte [podporovaná pole úložišť](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx).<br/><br/> Poskytovatelé SMI-S od výrobců pole úložiště by měly být nainstalovány a hello pole SAN se mají spravovat nástrojem hello zprostředkovatele. Nastavte pokyny podle toomanufacturer hello zprostředkovatele.<br/><br/>Ujistěte se, poskytovatel SMI-S tohoto pole hello je na serveru a že hello VMM server má přístup k síti hello s IP adresu nebo plně kvalifikovaný název domény.<br/><br/> Každé pole SAN musí mít jeden nebo více fondů úložiště k dispozici.<br/><br/> Hello primárním serverem VMM by měla spravovat hello primární pole a sekundární server VMM hello by měla spravovat hello sekundární pole.
**Mapování sítě** | Nastavte mapování sítě, aby se po převzetí služeb při selhání optimálně umístit replikované virtuální počítače na sekundární hostitelské servery technologie Hyper-V a, aby byly připojené tooappropriate sítě virtuálních počítačů. Pokud nenakonfigurujete mapování sítě, virtuální počítače replik připojené tooany sítě nebude po převzetí služeb při selhání.<br/><br/> Ujistěte se, že VMM sítě jsou správně nakonfigurovaná tak, aby můžete nastavit mapování sítě během nasazování Site Recovery. V nástroji VMM by měla být hello virtuální počítače na hostitele Hyper-V zdroj hello připojené tooa sítě virtuálních počítačů nástroje VMM. Tuto síť by měla být propojené tooa logické sítě, který je přidružený k hello cloudu.<br/><br/> Hello cílový cloud by měl mít odpovídající síť virtuálních počítačů, a pak musí být propojená tooa odpovídající logickou síť, která souvisí s hello cílový cloud.<br/><br/>.

## <a name="step-1-prepare-hello-vmm-infrastructure"></a>Krok 1: Příprava infrastruktury VMM hello
tooprepare infrastruktury VMM, budete muset:

1. Ověřte cloudech VMM.
2. Integrovat a klasifikaci úložiště sítě SAN v nástroji VMM.
3. Vytvoření logické jednotky a přidělovat úložiště.
4. Vytvořte replikační skupiny.
5. Nastavení sítí virtuálních počítačů.

### <a name="verify-vmm-clouds"></a>Ověřte cloudech VMM

Ujistěte se, že vaše cloudech VMM je správně nastavena před zahájením nasazení Site Recovery.

### <a name="integrate-and-classify-san-storage-in-hello-vmm-fabric"></a>Integrovat a klasifikaci úložiště sítě SAN v hello prostředků infrastruktury VMM

1. V konzole VMM hello přejděte příliš**Fabric** > **úložiště** > **přidat prostředky** > **zařízení úložiště**.
2. V hello **přidat zařízení úložiště** průvodci vyberte **vyberte typ poskytovatele úložiště** a vyberte **zařízení sítě SAN a NAS zjištěná a spravovaný pomocí poskytovatele SMI-S**.

    ![Typ poskytovatele](./media/site-recovery-vmm-san/provider-type.png)

3. Na hello **zadat protokol a adresu poskytovatele úložiště SMI-S hello** vyberte **SMI-S CIMXML** a zadejte nastavení hello připojování toohello zprostředkovatele.
4. V **zprostředkovatele IP adresu nebo plně kvalifikovaný název domény** a **TCP/IP port**, zadejte hello nastavení pro připojování toohello zprostředkovatele. Připojení protokolem SSL můžete použít pouze pro CIMXML SMI-S.

    ![Poskytovatel připojení](./media/site-recovery-vmm-san/connect-settings.png)
5. V **účet Spustit jako**, zadejte účet VMM spustit jako, který může získat přístup k poskytovateli hello, nebo vytvořit účet.
6. Na hello **shromáždit informace** stránky, VMM automaticky se pokusí toodiscover a import hello informace o úložném zařízení. tooretry zjišťování, klikněte na tlačítko **vyhledat poskytovatele**. Pokud proces zjišťování hello úspěšný, zjištěné hello polí úložiště, fondy úložiště, výrobce, model a kapacity jsou uvedeny na stránce hello.

    ![Zjišťování úložiště](./media/site-recovery-vmm-san/discover.png)
7. V **vyberte tooplace fondy úložiště pod správou a přiřaďte klasifikaci**, vyberte hello fondy úložiště, VMM bude spravovat a přiřadit jim klasifikaci. Informace o logických jednotkách importovány z fondů úložiště hello. Vytvoření logické jednotky LUN, na základě na hello aplikace musíte tooprotect, jejich požadavky na kapacitu a vaše požadavky pro toho, co je tooreplicate společně.

    ![Klasifikace úložiště](./media/site-recovery-vmm-san/classify.png)

### <a name="create-luns-and-allocate-storage"></a>Vytvoření logické jednotky a přidělit úložiště

Vytvoření logické jednotky LUN, na základě na hello aplikace musíte tooprotect, požadavky na kapacitu a vaše požadavky pro toho, co je tooreplicate společně.

1. Po hello úložiště se zobrazí v hello prostředků infrastruktury VMM, můžete [zřízení logické jednotky LUN](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-storage-host-groups#create-a-lun-in-vmm).

     > [!NOTE]
     > Nepřidáte virtuální pevné disky pro virtuální počítač, který jsou povolené pro replikaci tooLUNs hello. Nejsou-li tyto logické jednotky do replikační skupiny Site Recovery, nebudou zjištěna službou Site Recovery.
     >

2. Přidělte kapacitu úložiště, toohello clusteru hostitele Hyper-V tak, aby VMM můžete nasadit úložiště toohello zřídit data virtuálního počítače:

   * Před přidělování toohello clusteru úložiště, musíte tooallocate se skupiny hostitelů VMM toohello, na které hello nachází cluster. Další informace najdete v tématu [jak tooa logické jednotky úložiště tooallocate skupiny v nástroji VMM hostitelů](https://technet.microsoft.com/library/gg610686.aspx) a [jak fondy úložiště tooallocate tooa skupiny hostitelů v nástroji VMM](https://technet.microsoft.com/library/gg610635.aspx).
   * Přidělit clusteru toohello kapacity úložiště, jak je popsáno v [jak clusteru tooconfigure úložiště na hostiteli technologie Hyper-V v nástroji VMM](https://technet.microsoft.com/library/gg610692.aspx).

    ![Typ poskytovatele](./media/site-recovery-vmm-san/provider-type.png)
3. V **zadat protokol a adresu poskytovatele úložiště SMI-S hello**, vyberte **SMI-S CIMXML**. Zadejte hello nastavení pro připojení toohello zprostředkovatele. Pouze pro CIMXML SMI-S můžete použít připojení protokolem SSL.

    ![Poskytovatel připojení](./media/site-recovery-vmm-san/connect-settings.png)
4. V **účet Spustit jako**, zadejte účet VMM spustit jako, který může získat přístup k poskytovateli hello, nebo vytvořit účet.
5. V **shromáždit informace**, VMM automaticky se pokusí toodiscover a import hello informace o úložném zařízení. Pokud potřebujete tooretry, klikněte na tlačítko **vyhledat poskytovatele**. Pokud je proces zjišťování hello úspěšná, hello pole úložiště, fondy úložiště, výrobce, model a kapacity jsou uvedeny na stránce hello.

    ![Zjišťování úložiště](./media/site-recovery-vmm-san/discover.png)
7. V **vyberte tooplace fondy úložiště pod správou a přiřaďte klasifikaci**, vyberte hello fondy úložiště, bude VMM spravovat a přiřadit jim klasifikaci. Informace o logických jednotkách importovány z fondů úložiště hello.

    ![Klasifikace úložiště](./media/site-recovery-vmm-san/classify.png)


### <a name="create-replication-groups"></a>Vytvořte replikační skupiny

Vytvořte skupinu replikace, která zahrnuje všechny hello logické jednotky, které bude nutné tooreplicate společně.

1. V konzole VMM hello otevřete hello **replikační skupiny** hello vlastnosti pole úložiště a pak klikněte na kartu **nový**.
2. Vytvořte hello replikační skupiny.

    ![Skupiny replikace sítě SAN](./media/site-recovery-vmm-san/rep-group.png)

### <a name="set-up-networks"></a>Nastavení sítě

Pokud chcete tooconfigure mapování sítě, hello následující:

1. V tématu mapování sítě Site Recovery.
2. Připravte sítě virtuálních počítačů v nástroji VMM:

   * [Nastavení logických sítí](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-logical-networks).
   * [Nastavení sítí virtuálních počítačů](https://technet.microsoft.com/en-us/system-center-docs/vmm/manage/manage-network-vm-networks).


## <a name="step-2-create-a-vault"></a>Krok 2: Vytvoření trezoru

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) ze serveru VMM hello chcete tooregister v trezoru hello.
2. Rozbalte položku **datové služby** > **služeb zotavení**a potom klikněte na **trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. V **název**, zadejte popisný název tooidentify hello trezoru.
5. V **oblast**, hello vyberte zeměpisnou oblast trezoru hello. toocheck podporované oblasti, najdete v části [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klikněte na **Vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-vmm-san/create-vault.png)

Zkontrolujte, zda tooconfirm hello stav panelu, který hello trezor byl úspěšně vytvořen. Hello trezor bude uvedený jako **Active** na hello hlavní **služeb zotavení** stránky.

### <a name="register-hello-vmm-servers"></a>Zaregistrujte server VMM hello

1. Otevřete hello **rychlý Start** stránky z hello **služeb zotavení** stránky. Rychlý Start můžete otevřít také kdykoli výběrem ikony hello.

    ![Ikona Rychlý start](./media/site-recovery-vmm-san/quick-start-icon.png)
2. V rozevíracím seznamu hello, vyberte **mezi Hyper-V místní lokality pomocí replikace pole**.

    ![Registrační klíč](./media/site-recovery-vmm-san/select-san.png)
3. V **připravit servery VMM**, stáhněte si nejnovější verzi instalační soubor zprostředkovatele Azure Site Recovery hello hello.
4. Spusťte tento soubor na zdrojovém serveru VMM hello. Pokud VMM je nasazen v clusteru a instalujete hello zprostředkovatele pro hello poprvé, nainstalujte hello zprostředkovatele na aktivním uzlu a dokončit hello instalace tooregister hello VMM server v trezoru hello. Nainstalujte hello zprostředkovatele na hello dalších uzlů. Pokud upgradujete hello zprostředkovatele, budete potřebovat tooupgrade na všech uzlech, aby se mají hello tutéž verzi zprostředkovatele.
5. Instalační program Hello zkontroluje požadavky a požadavky oprávnění toostop hello instalace zprostředkovatele toobegin služby VMM. Služba Hello se restartuje automaticky po dokončení instalace. Na clusteru VMM budete výzvami toostop hello Clusterové role.
6. V **Microsoft Update**, můžete vyjádřit výslovný souhlas pro aktualizace a zprostředkovatele se budou instalovat aktualizace podle zásad tooyour Microsoft Update.

    ![Aktualizace Microsoft Update](./media/site-recovery-vmm-san/ms-update.png)

7. Ve výchozím umístění instalace hello hello zprostředkovatele je <SystemDrive>\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin. Klikněte na tlačítko **nainstalovat** toobegin.

    ![Umístění instalace](./media/site-recovery-vmm-san/install-location.png)

8. Po instalaci hello zprostředkovatele, klikněte na tlačítko **zaregistrovat** tooregister hello serveru VMM v trezoru hello.

    ![Instalace byla dokončena](./media/site-recovery-vmm-san/install-complete.png)

9. V **připojení k Internetu**, určete, jak hello zprostředkovatel připojuje toohello Internetu. Vyberte **použít výchozí nastavení proxy serveru systému** Pokud chcete nastavení připojení k toouse hello výchozí Internetu na serveru hello.

    ![Nastavení internetu](./media/site-recovery-vmm-san/proxy-details.png)

   * Pokud chcete toouse vlastní proxy server, ho nastavte před instalací hello zprostředkovatele. Při konfiguraci nastavení vlastního proxy serveru, testovací běhy toocheck hello proxy připojení.
   * Pokud používáte vlastní proxy server, nebo pokud váš výchozí proxy server vyžaduje ověření, měli byste zadat podrobnosti o proxy serveru hello, včetně hello adresu a port.
   * Hello požadované že adresy URL by měla být přístupný ze serveru VMM hello.
   * Pokud používáte vlastní proxy server, účet VMM spustit jako (DRAProxyAccount) je automaticky vytvořená pomocí hello zadané přihlašovací údaje pro proxy. Hello proxy server nakonfigurujte tak, aby tento účet může ověřit. Můžete upravit hello spustit jako účet nastavení v konzole VMM hello (**nastavení** > **zabezpečení** > **účty spustit jako**  >  **DRAProxyAccount**). Je nutné restartovat službu VMM hello pro hello změnit tootake efekt.
10. V **registrační klíč**, vyberte hello klíč, který jste stáhli ze serveru VMM portálu a zkopírovaný toohello hello.
11. V **název trezoru**, ověřte název hello hello úložiště, ve které hello bude server zaregistrovaný.

    ![Registrace serveru](./media/site-recovery-vmm-san/vault-creds.png)
12. nastavení šifrování Hello se používá pouze pro replikaci tooAzure VMM. Můžete ji ignorovat.

    ![Registrace serveru](./media/site-recovery-vmm-san/encrypt.png)
13. V **název serveru**, zadejte popisný název tooidentify hello VMM server v trezoru hello. V konfiguraci clusteru zadejte název role clusteru VMM hello.
14. V **synchronizovat metadata cloudu počáteční**vyberte, jestli chcete, aby toosynchronize metadata pro všechny cloudy na serveru VMM hello. Tuto akci stačí toohappen jednou na každém serveru. Pokud nechcete, aby toosynchronize všechny cloudy, můžete toto políčko nechat nezaškrtnuté a synchronizovat každý cloud jednotlivě ve vlastnostech hello cloudu v konzole VMM hello.

    ![Registrace serveru](./media/site-recovery-vmm-san/friendly-name.png)

15. Klikněte na tlačítko **Další** toocomplete hello procesu. Po registraci načte Azure Site Recovery metadata ze serveru VMM hello. Hello server se zobrazí v **servery** > **servery VMM** v trezoru hello.

### <a name="command-line-installation"></a>Instalace z příkazového řádku

Hello zprostředkovatele Azure Site Recovery můžete nainstalovat i pomocí hello následující příkazový řádek. Tato metoda může být použité tooinstall hello zprostředkovatele na jádro serveru pro Windows Server 2012 R2.

1. Hello zprostředkovatele instalace souboru a registraci klíče tooa složka pro stahování. Příklad: C:\ASR.
2. Zastavte službu VMM hello.
3. Extrahujte instalační program zprostředkovatele hello. Jako správce spusťte tyto příkazy:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
4. Nainstalujte hello zprostředkovatele:

        C:\ASR> setupdr.exe /i
5. Zaregistrujte hello zprostředkovatele:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of hello server> /Credentials <path of hello credentials file> /EncryptionEnabled <full file name toosave hello encryption certificate>         

Parametry:

* **/ Credentials**: povinný parametr pro hello umístění, ve které hello se nachází soubor registračního klíče.  
* **/ FriendlyName**: povinný parametr pro hello název hostitele serveru hello technologie Hyper-V, který se zobrazí na portálu Azure Site Recovery hello.
* **/ EncryptionEnabled**: volitelný parametr použít pouze v případě replikace z VMM tooAzure.
* **/ proxyaddress**: volitelný parametr, který určuje adresu hello hello proxy serveru.
* **/ proxyport**: volitelný parametr, který určuje port hello hello proxy serveru.
* **/ proxyusername**: volitelný parametr, který určuje uživatelské jméno proxy hello (Pokud hello proxy server vyžaduje ověření).
* **/ proxypassword**: volitelný parametr, který určuje hello heslo pro ověřování pomocí serveru proxy hello (Pokud hello proxy server vyžaduje ověření).

## <a name="step-3-map-storage-arrays-and-pools"></a>Krok 3: Mapování polí úložiště a fondy

Mapování toospecify primární a sekundární pole, které fond úložiště sekundární přijímá data replikace z primární fondu hello. Mapování úložiště před konfigurací replikace, protože informace o mapování hello se používá, pokud povolíte ochranu pro replikační skupiny.

Než začnete, zkontrolujte, že se objeví cloudech VMM v trezoru hello. Cloudy jsou zjištěna při synchronizaci veškerých cloudů v průběhu instalace zprostředkovatele nebo při synchronizaci konkrétní cloudu v konzole VMM hello.

1. Klikněte na tlačítko **prostředky** > **úložiště serveru** > **mapy zdrojové a cílové pole**.
    ![Registrace serveru](./media/site-recovery-vmm-san/storage-map.png)

2. Vyberte pole úložiště hello v primární lokalitě hello a jejich namapování toostorage pole na sekundární lokalitě hello. V **fondy úložiště**, zvolte zdroj a cíl toomap fondu úložiště.

    ![Registrace serveru](./media/site-recovery-vmm-san/storage-map-pool.png)

## <a name="step-4-configure-replication-settings"></a>Krok 4: Konfigurace nastavení replikace

Po registraci servery VMM se nakonfigurujte nastavení ochrany cloudu.

1. Na hello **rychlý Start** klikněte na tlačítko **nastavení ochrany pro cloudy VMM**.
2. Na hello **chráněné položky** vyberte hello cloud **konfigurace**.
3. V **cíl**, vyberte **VMM**.
4. V **cílové umístění**vyberte hello VMM serveru, který spravuje hello cloudu chcete toouse pro obnovení.
5. V **cílí na cloud**, vyberte cílový cloud hello chcete toouse převzetí služeb při selhání virtuálního počítače.
   * Doporučujeme vám, že vybíráte cílový cloud, který splňuje požadavky na obnovení pro hello virtuálních počítačů, které chráníte.
   * Cloud lze přiřadit pouze jeden cloud pár tooa – jako primární nebo cílový cloud.
6. Site Recovery ověřuje, že cloudy mají přístup k úložišti tooSAN a že hello úložiště, které jsou namapované pole.
7. Pokud je ověření úspěšné, v **typ replikace**, vyberte **SAN**.

Po uložení nastavení hello se vytvoří úloha, která lze sledovat na hello **úlohy** kartě. Nastavení můžete změnit na hello **konfigurace** kartě. Pokud chcete toomodify hello cílové umístění nebo cílový cloud, musíte odebrat konfiguraci cloudu hello a potom znovu nakonfigurovat hello cloudu.

## <a name="step-5-enable-network-mapping"></a>Krok 5: Povolení mapování sítě

1. Na hello **rychlý Start** klikněte na tlačítko **mapovat sítě**.
2. Vyberte hello zdrojový server VMM a pak vyberte hello cíl VMM server toowhich hello, které budou mapována sítě. Zobrazí se seznam Hello zdrojové sítě a jejich přidružené cílové sítě. Pro sítě, které nejsou namapované je zobrazen na prázdnou hodnotu. Klikněte na tlačítko hello informace ikonu další toohello zdrojové a cílové názvy tooview hello podsítí pro každou síť.
3. Vyberte síť, v **sítě na zdroji**a potom klikněte na **mapy**. Služba Hello zjistí hello sítě virtuálních počítačů na cílovém serveru hello a zobrazí je.

    ![Architektura sítě SAN](./media/site-recovery-vmm-san/network-map1.png)
4. Vyberte jednu z sítě virtuálních počítačů hello hello cílovém serveru VMM.

    ![Architektura sítě SAN](./media/site-recovery-vmm-san/network-map2.png)

5. Při výběru cílové sítě hello chráněných cloudů, které používají hello zdrojové síti jsou zobrazeny. Zobrazí se také k dispozici cílové sítě. Doporučujeme vám, že vyberete Cílová síť, která je k dispozici tooall hello cloudy, kterou používáte pro replikaci.
6. Klikněte na tlačítko proces mapování hello zaškrtnutí toocomplete hello. Spustí úloha, která sleduje průběh. Můžete ji zobrazit v hello **úlohy** kartě.

## <a name="step-6-enable-replication-for-replication-groups"></a>Krok 6: Povolení replikace pro replikační skupiny

Než můžete povolit ochranu pro virtuální počítače, je třeba tooenable replikace pro replikační skupiny úložiště.

1. Na hello **vlastnosti** stránku hello primárního cloudu na portálu Site Recovery hello, otevřete hello **virtuální počítače** a klikněte na **přidat skupinu replikace**.
2. Vyberte jednu nebo více skupin replikace VMM, které jsou přidruženy hello cloudu, ověřte hello zdrojové a cílové pole a zadejte četnost replikace hello.

Site Recovery, VMM a zprostředkovatele SMI-S hello zřídit hello cílové lokality úložiště logických jednotek a povolit replikaci úložiště. Pokud už je replikována hello replikační skupiny, Site Recovery opětovně používá existující relaci replikace hello a aktualizace hello informace.

## <a name="step-7-enable-protection-for-virtual-machines"></a>Krok 7: Povolení ochrany pro virtuální počítače


Když je skupinou úložišť pro replikaci, povolte ochranu pro virtuální počítače v konzole VMM hello s jedním z následujících metod hello:

* **Nový virtuální počítač**: při vytváření virtuálního počítače, replikaci povolíte a přiřaďte hello virtuálního počítače k replikační skupině hello.
  Tato možnost používá VMM inteligentní umístění toooptimally místní hello virtuálních počítačů úložiště na jednotky LUN hello hello replikační skupiny. Site Recovery orchestruje hello vytvoření stínové virtuálního počítače na sekundární lokalitě hello a přiděluje kapacitu tak, že replikované virtuální počítače lze spustit po převzetí služeb při selhání.
* **Existující virtuální počítač**: Pokud virtuální počítač je už nasazená v nástroji VMM, můžete povolit replikaci a provádět k replikační skupině úložiště migrace tooa. Po dokončení, VMM a Site Recovery zjistit hello nového virtuálního počítače a zahájení správy ve službě Site Recovery. Stín virtuálního počítače je vytvořen v sekundární lokalitě hello a kapacita přidělí tak hello repliky virtuálních počítačů lze spustit po převzetí služeb při selhání.

![Povolení ochrany](./media/site-recovery-vmm-san/enable-protect.png)

Když virtuální počítače jsou povolená pro replikaci, zobrazí se v konzole Site Recovery hello. Můžete zobrazit vlastnosti virtuálního počítače, sledovat stav a sledovat skupiny replikace převzetí služeb při selhání, které obsahují víc virtuálních počítačů. V replikaci sítě SAN všechny virtuální počítače přidružené skupině replikace musí převzetí služeb při selhání společně. Je to proto, že nastane převzetí služeb při selhání ve vrstvě úložiště hello dříve. Je důležité toogroup vaší replikace skupiny správně a společně umístit jenom přidružené virtuální počítače.

> [!NOTE]
> Můžete po povolení replikace pro virtuální počítač, nemusíte přidávat jeho tooLUNs virtuální pevné disky, pokud se nacházejí v replikační skupině Site Recovery. Virtuální pevné disky se zjistit pomocí Site Recovery pouze v případě, že se nacházejí v replikační skupině Site Recovery.
>
>

Můžete sledovat průběh, včetně počáteční replikace hello na hello **úlohy** kartě. Po spuštění úloze dokončení ochrany hello hello virtuální počítač je připraven na převzetí služeb při selhání.

![Úloha ochrany virtuálního počítače](./media/site-recovery-vmm-san/job-props.png)

## <a name="step-8-test-hello-deployment"></a>Krok 8: Testování nasazení hello

Otestujte vaše nasazení toomake jistotu, že virtuální počítače fungovat přes podle očekávání. toodo, vytvoření plánu obnovení a spustit testovací převzetí služeb.

1. Na hello **plány obnovení** , klikněte na **vytvořit plán obnovení**.
2. Zadejte název pro plán obnovení hello a vyberte zdrojové a cílové servery VMM. Hello zdrojový server musí mít virtuální počítače, které jsou povolené pro převzetí služeb při selhání a obnovení. Vyberte **SAN** tooview pouze cloudy, které jsou nakonfigurované pro replikaci sítě SAN.

    ![Vytvoření plánu obnovení](./media/site-recovery-vmm-san/r-plan.png)

3. V **vyberte virtuální počítače**, vyberte replikační skupiny. Všechny virtuální počítače přidružené skupině hello se přidají toohello plánu obnovení. Tyto virtuální počítače se přidají výchozí skupiny plánu obnovení toohello (skupina 1). V případě potřeby můžete přidat další skupiny. Virtuální počítače po replikaci, jsou číslované podle pořadí toohello skupiny plánu obnovení hello.

    ![Vyberte virtuální počítače](./media/site-recovery-vmm-san/r-plan-vm.png)
4. Po vytvoření plánu obnovení hello se zobrazí v seznamu hello na hello **plány obnovení** kartě. Vyberte plán hello a zvolte **testovací převzetí služeb při selhání**.
5. Na hello **potvrzení převzetí služeb při selhání** vyberte **žádné**. Když povolíte tuto možnost nebudou hello převzal replikované virtuální počítače připojené tooany sítě. Tím otestujete této hello, které virtuální počítače převzetí služeb při selhání podle očekávání, ale ji nepodporuje testování hello síťového prostředí. Další informace o jiných možností sítě najdete v tématu [Site Recovery převzetí služeb při selhání](site-recovery-failover.md).

    ![Vyberte testovací síti](./media/site-recovery-vmm-san/test-fail1.png)

6. Hello testovací virtuální počítač se vytvoří na hello stejné hostitele jako hello hostitele, na které hello repliky virtuálních počítačů existuje. Není přidáno toohello cloudu, ve které hello repliku virtuálního počítače se nachází.
2. Po dokončení replikace hello repliku virtuálního počítače bude mít jinou IP adresu než hello primárního virtuálního počítače. Pokud vystavujete adresy ze serveru DHCP, se neaktualizují automaticky. Pokud nepoužíváte protokol DHCP a chcete hello stejné adresy, je nutné toorun několik skriptů.
3. Spusťte tento skript tooretrieve hello IP adresa:

       $vm = Get-SCVirtualMachine -Name <VM_NAME>
       $na = $vm[0].VirtualNetworkAdapters>
       $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
       $ip.address  

4. Spusťte tento ukázkový skript tooupdate DNS. Zadejte IP adresu hello, který jste získali.

       [string]$Zone,
       [string]$name,
       [string]$IP
       )
       $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
       $newrecord = $record.clone()
       $newrecord.RecordData[0].IPv4Address  =  $IP
       Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


## <a name="next-steps"></a>Další kroky

Po spuštění toocheck testovací převzetí služeb při selhání, který prostředí pracuje dle očekávání, najdete v části [Site Recovery převzetí služeb při selhání](site-recovery-failover.md) toolearn o různých typech převzetí služeb při selhání.
