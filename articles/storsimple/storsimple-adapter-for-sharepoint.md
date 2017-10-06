---
title: "aaaInstall adaptér StorSimple pro službu SharePoint | Microsoft Docs"
description: "Popisuje, jak tooinstall a nakonfigurovat nebo odebrat hello adaptér StorSimple pro službu SharePoint ve farmě serverů SharePoint."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 36c20b75-f2e5-4184-a6b5-9c5e618f79b2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/06/2017
ms.author: v-sharos
ms.openlocfilehash: 9a7347232fb80156d93212e6382cdd4fab98a2d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-hello-storsimple-adapter-for-sharepoint"></a>Instalace a konfigurace hello adaptér StorSimple pro službu SharePoint
## <a name="overview"></a>Přehled
Hello adaptér StorSimple pro službu SharePoint je komponenta, která vám umožňuje zadat flexibilní úložiště Microsoft Azure StorSimple a data protection tooSharePoint serverové farmy. Můžete použít hello adaptér toomove binární velkého objektu (binární rozsáhlý OBJEKT) obsah z hello systému SQL Server databáze obsahu toohello Microsoft Azure StorSimple hybridní cloudové úložiště zařízení.

Hello adaptér StorSimple pro službu SharePoint funguje jako zprostředkovatel vzdáleného úložiště objektů BLOB (RBS) a používá hello toostore funkce SQL Server vzdáleného úložiště objektů BLOB nestrukturovaných obsahu služby SharePoint (ve formě hello objektů BLOB) na souborový server, který je zálohovaný díky zařízení StorSimple.

> [!NOTE]
> Hello adaptér StorSimple pro službu SharePoint podporuje SharePoint Server 2010 vzdálené objektu BLOB úložiště (RBS). SharePoint Server 2010 externí objekt BLOB úložiště (EBS) nejsou podporovány.


* toodownload hello adaptér StorSimple pro službu SharePoint, přejděte příliš[StorSimple adaptéru pro službu SharePoint] [ 1] v hello Microsoft Download Center.
* Informace o plánování pro strukturu rozpisu zdrojů a RBS omezení, přejděte příliš[rozhodování toouse RBS v SharePoint 2013] [ 2] nebo [plánování RBS (SharePoint Server 2010)] [3].

Hello zbytek tento přehled stručně popisuje role hello hello adaptér StorSimple pro službu SharePoint a hello SharePoint kapacity a výkonu omezení, byste měli vědět, než nainstalujete a nakonfigurujete hello adaptér. Poté, co si tyto informace, přejděte příliš[StorSimple adaptéru pro instalaci služby SharePoint](#storsimple-adapter-for-sharepoint-installation) toobegin nastavení adaptéru hello.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>Adaptér StorSimple pro výhody služby SharePoint
Na webu služby SharePoint obsah se ukládá jako nestrukturovaných dat objektů BLOB v jedné nebo více databází obsahu. Ve výchozím nastavení jsou tyto databáze hostované na počítačích se systémem SQL Server a jsou umístěny ve farmě serverů SharePoint hello. Objekty BLOB můžete rychle zvětšují svou velikost, spotřebovává velké množství místní úložiště. Z tohoto důvodu můžete chtít toofind řešení jiného, levnější úložiště. SQL Server poskytuje technologie, která volá vzdálenou objektu Blob úložiště (RBS), umožňuje uložit obsah objektu BLOB v systému souborů hello, mimo databázi systému SQL Server hello. S rozpisu zdrojů objekty BLOB mohou být v systému souborů hello hello počítače se systémem SQL Server, nebo mohou být uloženy v systému souborů hello na jiném počítači serveru.

RBS vyžaduje, že používáte poskytovatele RBS, jako je například hello adaptér StorSimple pro službu SharePoint, tooenable RBS ve službě SharePoint. Hello adaptér StorSimple pro službu SharePoint pracuje s rozpisu zdrojů umožňuje přesunout objekty BLOB tooa serveru zálohovat systém Microsoft Azure StorSimple hello. Microsoft Azure StorSimple uloží data objektu BLOB hello místně nebo v cloudu hello na základě využití. Objekty BLOB, které jsou velmi aktivní (obvykle označují tooas vrstvě 1 nebo aktivní data) jsou umístěny místně. Archivace dat a míň aktivní dat nacházet v cloudu hello. Jakmile povolíte RBS na databázi obsahu, žádný nový obsah objektu BLOB, které jsou vytvořeny ve službě SharePoint jsou uloženy v zařízení StorSimple hello a není v databázi obsahu hello.

implementace Microsoft Azure StorSimple Hello RBS poskytuje hello následující výhody:

* Přesunutí objektu BLOB obsahu tooa samostatný server můžete snížit zatížení hello dotazu na SQL serveru, což může zlepšit odezvu systému SQL Server. 
* Azure StorSimple používá velikost dat tooreduce odstraňování duplicitních dat a komprese.
* Azure StorSimple poskytuje ochranu dat ve formě hello místních a cloudových snímků. Navíc pokud jste na zařízení StorSimple hello hello databáze, můžete zálohovat databázi obsahu hello a objekty BLOB společně v zhroutí konzistentní způsob. (Přesunutí hello databázi obsahu toohello zařízení je podporována pouze pro zařízení řady StorSimple 8000 hello. Tato funkce není podporována pro řadu hello 5000 a 7000.)
* Azure StorSimple zahrnuje funkce obnovení po havárii, včetně rychlé obnovení dat, převzetí služeb při selhání a obnovení souborů a svazek (včetně zkušební obnovení).
* Software obnovení dat, například Kroll Ontrack PowerControls, můžete použít s StorSimple snímky BLOB dat tooperform obnovení na úrovni položek obsahu služby SharePoint. (Tento software obnovení dat je samostatného nákupu.)
* Hello adaptér StorSimple pro službu SharePoint zapojení do portálu hello Centrální správa SharePoint, umožní vám toomanage celé řešení služby SharePoint z centrálního umístění.

Přesunutí systém souborů obsahu toohello objektů BLOB můžete zadat další úsporu nákladů a výhody. Například pomocí RBS můžete snížit hello potřebu levnější úložiště vrstvy 1 a protože zaplňování hello databáze obsahu, RBS můžete snížit hello počet databází ve farmě serverů SharePoint hello vyžaduje. Dalších faktorech, například omezení velikosti databáze a hello množství obsahu, bez RBS, však může ovlivnit také požadavky na úložiště. Další informace o hello náklady a výhody použití RBS najdete v tématu [plánování RBS (SharePoint Foundation 2010)] [ 4] a [rozhodování toouse RBS v SharePoint 2013] [ 5].

### <a name="capacity-and-performance-limits"></a>Omezení kapacitu a výkon
Před uvažujete o používání RBS ve vašem řešení služby SharePoint, byste měli být vědomi hello testování výkonu a limity kapacity serveru SharePoint Server 2010 a SharePoint Server 2013 a jak tyto limity vztahují tooacceptable výkonu. Další informace najdete v tématu [softwaru hranice a omezení pro SharePoint 2013](https://technet.microsoft.com/library/cc262787.aspx).

Před konfigurací RBS, projděte si následující hello:

* Ujistěte se, že hello celková velikost obsahu (hello velikost databáze obsahu) a všechny související objekty BLOB, externalized hello velikost nepřekračuje maximální velikost RBS hello je podporované službou SharePoint hello. Tento limit je 200 GB. 
  
    **databázi obsahu toomeasure a velikost objektu BLOB**
  
  1. Spusťte tento dotaz na hello WFE centrální správy. Spustit hello prostředí pro správu služby SharePoint a poté zadejte následující příkaz prostředí Windows PowerShell tooget hello velikost databází obsahu hello hello:
     
     `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`
     
      Tento krok získá hello velikost databáze obsahu hello na disku hello.
  2. Spusťte jeden z hello následující dotazy SQL v SQL Management Studio na pole server hello SQL v jednotlivých databázích obsahu a přidat hello výsledek toohello číslo získanou v kroku 1.
     
     U databází obsahu SharePoint 2013 zadejte:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`
     
     U databází obsahu SharePoint 2010 zadejte:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`
     
     Tento krok získá velikost hello hello objekty BLOB, které mají byla externalized.
* Doporučujeme, abyste ukládání všech objektů BLOB a databáze obsahu místně na zařízení StorSimple hello. zařízení StorSimple Hello se dvěma uzly clusteru s podporou pro vysokou dostupnost. Umístění databáze obsahu hello a objekty BLOB na zařízení StorSimple hello poskytuje vysokou dostupnost.
  
    Použijte tradiční SQL Server migrace osvědčené postupy toomove hello databázi obsahu toohello zařízení StorSimple. Přesunutí databáze hello pouze po veškerý obsah objektu BLOB z databáze hello přesunutý toohello sdílenou složku přes RBS. Pokud se rozhodnete zařízení StorSimple toomove hello databázi obsahu toohello, doporučujeme nakonfigurovat hello databázi obsahu úložiště na zařízení hello jako primární svazek.
* V Microsoft Azure StorSimple, pokud používáte vrstvené svazky neexistuje žádný způsob, tooguarantee, který obsah místně uložených na zařízení StorSimple hello nebudete mít vrstvené tooMicrosoft cloudového úložiště Azure. Proto doporučujeme používat svazky zařízení StorSimple místně vázaný ve spojení s RBS služby SharePoint. Tím bude zajištěno, že zůstane místně na zařízení StorSimple hello veškerý obsah objektu BLOB a není přesunutý tooMicrosoft Azure.
* Pokud na zařízení StorSimple hello neukládáte hello databáze obsahu, použijte tradiční SQL Server vysokou dostupnost osvědčené postupy podporující RBS. Clustering SQL Server podporuje RBS, při systému SQL Server zrcadlení neexistuje. 

> [!WARNING]
> Pokud jste nepovolili RBS, nedoporučujeme přesunutí zařízení StorSimple toohello hello databázi obsahu. Toto je netestované konfigurace.

## <a name="storsimple-adapter-for-sharepoint-installation"></a>Adaptér StorSimple pro instalaci služby SharePoint
Před instalací hello adaptér StorSimple pro službu SharePoint, musíte nakonfigurovat nastavení zařízení StorSimple hello a ujistěte se, že serverové farmy služby SharePoint hello a vytváření instancí systému SQL Server splňují všechny požadavky. Tento kurz popisuje požadavky na konfiguraci, také s postupy pro instalaci a upgrade hello adaptér StorSimple pro službu SharePoint.

## <a name="configure-prerequisites"></a>Konfigurace požadavků
Před instalací hello adaptér StorSimple pro službu SharePoint, ujistěte se, že zařízení StorSimple hello serverové farmy služby SharePoint a vytváření instancí systému SQL Server splňují hello následující požadavky.

### <a name="system-requirements"></a>Požadavky na systém
Hello adaptér StorSimple pro službu SharePoint funguje s hello následující hardware a software:

* Podporovaný operační systém – Windows Server 2008 R2 SP1, Windows Server 2012 nebo Windows Server 2012 R2
* Podporované verze služby SharePoint – SharePoint Server 2010 nebo SharePoint Server 2013
* Podporované verze systému SQL Server – SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition nebo SQL Server 2012 Enterprise Edition
* Podporovaná zařízení StorSimple – řady StorSimple 8000, řady 7000 zařízení StorSimple nebo 5000 zařízení StorSimple řady.

### <a name="storsimple-device-configuration-prerequisites"></a>Předpoklady konfigurace zařízení StorSimple
zařízení StorSimple Hello se zařízení blokovat a jako takový vyžaduje souborový server, na kterém se dají hostovat hello data. Doporučujeme používat samostatný server, nikoli existující server z farmy služby SharePoint hello. Tento souborový server musí být na hello stejné místní síti (LAN) jako počítače serveru SQL Server hello databází obsahu hello tohoto hostitele.

> [!TIP]
> * Pokud provedete konfiguraci farmy služby SharePoint pro zajištění vysoké dostupnosti, měli byste také nasadit hello souborový server pro vysokou dostupnost.
> * Pokud na zařízení StorSimple hello neukládáte hello databáze obsahu, použijte tradiční vysokou dostupnost osvědčené postupy, které podporují RBS. Clustering SQL Server podporuje RBS, při systému SQL Server zrcadlení neexistuje. 


Ujistěte se, že zařízení StorSimple je správně nakonfigurován a že toosupport odpovídajících svazků nasazení služby SharePoint jsou nakonfigurované a dostupné z počítače serveru SQL Server. Přejděte příliš[nasazení místního zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md) Pokud ještě nebyla nasazená a nakonfigurovaná zařízení StorSimple. Všimněte si hello IP adresu zařízení StorSimple hello; je nutné ho při StorSimple adaptéru pro instalaci služby SharePoint.

Kromě toho ověřte, že tento toobe svazku hello používá pro objekt BLOB externalization splňuje hello následující požadavky:

* Hello svazek musí být naformátovaná velikost alokační jednotky 64 KB.
* Web front end (WFE) a aplikační servery musí být schopný tooaccess hello svazek prostřednictvím cestě Universal Naming Convention (UNC).
* farma serverů SharePoint Hello musí být nakonfigurované toowrite toohello svazek.

> [!NOTE]
> Po instalaci a konfiguraci adaptéru hello, všech objektů BLOB externalization musí projít zařízení StorSimple hello (hello zařízení bude k dispozici hello svazky tooSQL serveru a spravovat vrstvy úložiště hello). Pro objekt BLOB externalization nelze použít jiné cíle.


Pokud plánu toouse StorSimple Snapshot Manager tootake snímky hello objektů BLOB a databázových dat, se že tooinstall StorSimple Snapshot Manager na serveru databáze hello tak, aby mohl používat hello SQL Writer Service tooimplement hello Stínová kopie svazku systému Windows Service (VSS).

> [!IMPORTANT]
> Snapshot Manager zařízení StorSimple nepodporuje hello zapisovač SharePoint VSS a snímky konzistentní s aplikacemi dat služby SharePoint nelze provést. V případě služby SharePoint poskytuje StorSimple Snapshot Manager pouze konzistentní při selhání zálohování.


## <a name="sharepoint-farm-configuration-prerequisites"></a>Předpoklady konfigurace farmy služby SharePoint
Ujistěte se, že server farmy služby SharePoint je správně nakonfigurována, následujícím způsobem:

* Ověřte, zda server farmy služby SharePoint je v pořádku a zkontrolujte následující hello:
* Všechny služby SharePoint WFE a aplikační servery zaregistrované ve farmě hello běží a můžete otestovat pomocí příkazu ping ze serveru hello, na který budete instalovat hello adaptér StorSimple pro službu SharePoint.
* Hello služba Časovač služby SharePoint (SPTimerV3 nebo SPTimerV4) běží na každém serveru WFE a aplikačního serveru.
* Hello služba Časovač služby SharePoint i fond aplikací služby IIS hello pod nimiž hello Centrální správa SharePoint Web spuštěn mít oprávnění správce.
* Ujistěte se, že Internet Explorer rozšířeného bezpečnostní kontext (IE ESC) je zakázán. Postupujte podle těchto kroků toodisable IE ESC:
  
  1. Zavřete všechny instance Internet Exploreru.
  2. Spusťte hello správce serveru.
  3. V levém podokně hello, klikněte na **místní Server**.
  4. Na hello pravým podokně vedle příliš**konfigurace rozšířeného zabezpečení aplikace Internet Explorer**, klikněte na tlačítko **na**.
  5. V části **správci**, klikněte na tlačítko **vypnout**.
  6. Klikněte na **OK**.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Vzdálené úložiště objektů BLOB (RBS) požadavky
Ujistěte se, že používáte podporovanou verzi systému SQL Server. Pouze hello jsou následující verze podporované a může toouse RBS:

* SQL Server 2008 Enterprise Edition
* SQL Server 2008 R2 Enterprise Edition
* SQL Server 2012 Enterprise Edition

Objekty BLOB může na externalized, pouze tyto svazky, které hello zařízení StorSimple uvede tooSQL serveru. Jsou podporovány žádné jiné cíle pro externalization objektů BLOB.

Po dokončení všech kroků konfigurace požadovaných přejděte příliš[hello instalaci adaptéru StorSimple pro službu SharePoint](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-hello-storsimple-adapter-for-sharepoint"></a>Nainstalujte hello adaptér StorSimple pro službu SharePoint
Pomocí následujících kroků tooinstall hello adaptér StorSimple pro službu SharePoint hello. Pokud provádíte přeinstalaci programu hello softwaru, přečtěte si téma [upgradovat nebo přeinstalovat hello adaptér StorSimple pro službu SharePoint](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). Hello čas potřebný k instalaci hello závisí na hello celkový počet databází služby SharePoint v serverové farmě služby SharePoint.

[!INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]

## <a name="configure-rbs"></a>Konfigurace RBS
Po instalaci hello adaptér StorSimple pro službu SharePoint, nakonfigurujte RBS, jak je popsáno v hello následující postup.

> [!TIP]
> Hello adaptér StorSimple pro službu SharePoint zapojení do stránku hello Centrální správa SharePoint, což RBS toobe povolit nebo zakázat v jednotlivých databázích obsahu ve farmě služby SharePoint hello. Povolení nebo zakázání RBS na databázi obsahu hello však způsobí resetování služby IIS, který v závislosti na konfiguraci farmy může přerušit na okamžik hello dostupnost hello SharePoint web front end (WFE). (Faktorů, jako je například hello použít nástroj pro vyrovnávání zatížení front-end, hello aktuální zatížení serveru a tak dále, můžete omezit nebo odstranit tento přerušení.) tooprotect uživatele z narušení služeb, doporučujeme povolit nebo zakázat RBS pouze během plánované údržby.


[!INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]

## <a name="configure-garbage-collection"></a>Konfigurace uvolňování paměti
Pokud objekty jsou odstraněny z webu služby SharePoint, nejsou odstraněny automaticky z hello RBS svazku úložiště. Místo toho asynchronní, osamocených objektů BLOB program údržby pozadí odstraní z úložiště souborů hello. Správci systému můžou naplánovat tento proces toorun pravidelně nebo můžou spustit se vždy, když je to nezbytné.

Tento program údržby (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) se automaticky nainstaluje na všechny servery SharePoint WFE a aplikační servery, pokud povolíte RBS. v následujícím umístění hello je nainstalován Hello program: *spouštěcí jednotka*: 10.50\Maintainer\ \Program Files\Microsoft SQL vzdáleného úložiště objektů Blob

Informace o konfiguraci a použití programu hello údržby najdete v tématu [udržovat RBS v SharePoint Server 2013][8].

> [!IMPORTANT]
> program funkce maintainer RBS Hello je náročný na prostředky. Je vhodné naplánovat ji toorun pouze během období mírnou aktivitu na hello farmy služby SharePoint.


### <a name="delete-orphaned-blobs-immediately"></a>Okamžitě Odstranit osamocené objekty BLOB
Pokud potřebujete objekty BLOB toodelete osamocené okamžitě, můžete použít hello pokynů. Všimněte si, že tyto pokyny jsou příklad, jak to lze provést v prostředí SharePoint 2013 s hello následující součásti:

* Název databáze obsahu Hello je WSS_Content.
* Název systému SQL Server Hello je SHRPT13 SQL12\SHRPT13.
* Název webové aplikace Hello je SharePoint – 80.

[!INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]

## <a name="upgrade-or-reinstall-hello-storsimple-adapter-for-sharepoint"></a>Upgradovat nebo přeinstalovat hello adaptér StorSimple pro službu SharePoint
Použijte následující postup tooupgrade SharePoint server hello a znovu nainstalovat adaptér StorSimple pro upgrade služby SharePoint nebo toosimply nebo přeinstalovat hello adaptér v existující serverové farmy služby SharePoint.

> [!IMPORTANT]
> Zkontrolujte následující informace, než se pokusíte tooupgrade hello SharePoint softwaru nebo upgradu nebo přeinstalovat hello adaptér StorSimple pro službu SharePoint:
> 
> * Všechny soubory, které byly dříve přesunout úložiště tooexternal prostřednictvím RBS nebudou k dispozici, dokud nebude dokončeno hello přeinstalování a hello RBS funkce je znovu povolena. uživatel toolimit vliv, proveďte všechny upgrade nebo přeinstalaci během plánované údržby.
> * Hello čas potřebný k hello upgrade nebo přeinstalování se může lišit v závislosti na hello celkový počet databází služby SharePoint ve farmě serverů SharePoint hello.
> * Po dokončení upgradu nebo přeinstalování hello potřebujete tooenable RBS pro databází obsahu hello. V tématu [konfigurace RBS](#configure-rbs) Další informace.
> * Pokud konfigurujete RBS pro farmu služby SharePoint, který má velký počet databází (větší než 200), hello **Centrální správa SharePoint** stránky může vypršení časového limitu. Pokud k tomu dojde, aktualizujte stránku hello. Proces konfigurace hello to nemá vliv.


[!INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]

## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple adaptér pro odebrání služby SharePoint
Hello následující postupy popisují, jak toomove objekty BLOB hello zálohování databází obsahu toohello systému SQL Server a pak ji odinstalujte hello adaptér StorSimple pro službu SharePoint. 

> [!IMPORTANT]
> Máte toomove hello objekty BLOB back toohello obsahu databáze, před odinstalací softwaru adaptér hello.


### <a name="before-you-begin"></a>Než začnete
Shromážděte následující informace, před přesunutím hello data zálohovat databáze obsahu toohello systému SQL Server a spusťte proces odebrání adaptér hello hello:

* Hello názvy všech databází hello, pro které je povoleno RBS
* cesta UNC Hello hello nakonfigurované úložišti objektů BLOB

### <a name="move-hello-blobs-back-toohello-content-databases"></a>Přesunutí databází obsahu back toohello hello objektů BLOB
Před odinstalací hello adaptér StorSimple pro software, SharePoint, je nutné migrovat všechny objekty BLOB, které byly obsahu databáze systému SQL Server externalized back toohello hello. Když zkusíte toouninstall hello adaptér StorSimple pro službu SharePoint, před přesunutím všechny hello objekty BLOB back toohello obsahu databáze, zobrazí se následující zpráva upozornění hello.

![Zpráva upozornění](./media/storsimple-adapter-for-sharepoint/sasp1.png)

#### <a name="toomove-hello-blobs-back-toohello-content-databases"></a>toomove hello objekty BLOB back toohello obsahu databáze
1. Stáhněte si každý z hello externalized objektů.
2. Otevřete hello **Centrální správa SharePoint** stránky a procházet příliš**nastavení systému**.
3. V části **Azure StorSimple**, klikněte na tlačítko **nakonfigurovat adaptér StorSimple**.
4. Na hello **nakonfigurovat adaptér StorSimple** klikněte na tlačítko hello **zakázat** tlačítko pod každý hello databází obsahu, které chcete tooremove z externího úložiště objektů BLOB. 
5. Odstraňte hello objekty ze služby SharePoint a potom je odešlete znovu.

Alternativně můžete použít hello Microsoft` RBS Migrate()` rutiny prostředí PowerShell, které jsou součástí služby SharePoint. Další informace najdete v tématu [migraci obsahu do nebo z RBS](https://technet.microsoft.com/library/ff628255.aspx).

Po přesunutí databáze obsahu back toohello hello objekty BLOB, přejděte toohello další krok: [odinstalace adaptéru hello](#uninstall-the-adapter).

### <a name="uninstall-hello-adapter"></a>Odinstalace adaptéru hello
Po přesunutí obsahu databáze hello objekty BLOB back toohello systému SQL Server, použijte jednu z hello následující možnosti toouninstall hello adaptér StorSimple pro službu SharePoint.

#### <a name="toouse-hello-installation-program-toouninstall-hello-adapter"></a>toouse hello instalační program toouninstall hello adaptéru
1. Použijte účet s toolog oprávnění správce na toohello webovém serveru front-end (WFE).
2. Dvakrát klikněte na hello adaptér StorSimple pro instalační program služby SharePoint. Spustí se Průvodce instalací Hello.
   
    ![Průvodce instalací](./media/storsimple-adapter-for-sharepoint/sasp2.png)
3. Klikněte na **Další**. Zobrazí se následující stránka Hello.
   
    ![Průvodce instalací odebrat stránka](./media/storsimple-adapter-for-sharepoint/sasp3.png)
4. Klikněte na tlačítko **odebrat** procesu odebrání tooselect hello. Zobrazí se následující stránka Hello.
   
    ![Instalační program potvrzovací stránku průvodce](./media/storsimple-adapter-for-sharepoint/sasp4.png)
5. Klikněte na tlačítko **odebrat** tooconfirm hello odebrání. Zobrazí se následující stránka s průběhem Hello.
   
    ![Stránka s průběhem instalace Průvodce](./media/storsimple-adapter-for-sharepoint/sasp5.png)
6. Po dokončení odebrání hello se zobrazí stránku hello. Klikněte na tlačítko **Dokončit** tooclose hello Průvodce instalací.

#### <a name="toouse-hello-control-panel-toouninstall-hello-adapter"></a>toouse hello ovládací panely toouninstall hello adaptéru
1. Otevřete hello ovládacích panelech a pak klikněte na tlačítko **programy a funkce**.
2. Vyberte **StorSimple adaptéru pro službu SharePoint**a potom klikněte na **odinstalovat**.

## <a name="next-steps"></a>Další kroky
[Další informace o zařízení StorSimple](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/en-us/library/ff943565.aspx
