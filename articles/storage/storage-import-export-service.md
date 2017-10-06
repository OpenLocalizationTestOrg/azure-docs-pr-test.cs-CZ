---
title: "aaaUsing Azure Import/Export tootransfer data tooand z úložiště objektů blob | Microsoft Docs"
description: "Zjistěte, jak toocreate import a export úloh v hello portál Azure pro přenos dat tooand z úložiště objektů blob."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 668f53f2-f5a4-48b5-9369-88ec5ea05eb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: muralikk
ms.openlocfilehash: 84471d736d2433ffee9f49fbef91856d3dd56bb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-microsoft-azure-importexport-service-tootransfer-data-tooblob-storage"></a>Používání hello Microsoft Azure Import/Export služby tootransfer data tooblob úložiště
Hello služba Azure Import/Export můžete toosecurely přenos velkých objemů dat tooAzure úložiště objektů blob podle přenosů jednotky pevného disku tooan datového centra Azure. Můžete také použít tento služby tootransfer data z Azure blob storage toohard diskové jednotky a dodávat tooyour místního webu. Tato služba je vhodný v situacích, kdy chcete tootransfer několika terabajtů (TB) dat tooor z Azure, ale nahrávání nebo stahování přes síť hello je nemožné kvůli toolimited šířky pásma nebo vysokou sítě náklady.

Hello služba vyžaduje, aby pevné disky pro hello zabezpečení vašich dat šifrovaná nástrojem BitLocker. Služba Hello podporuje obě hello Classic a Azure Resource Manager účty úložiště (standardní a studená úroveň) existuje ve všech oblastech hello veřejný Azure. Je nutné dodat jednotky pevného disku tooone hello podporované umístění zadané později v tomto článku.

V tomto článku můžete další informace o hello Azure Import/Export služby a jak tooship jednotky pro kopírování tooand vaše data z úložiště objektů Blob Azure.

## <a name="when-should-i-use-hello-azure-importexport-service"></a>Kdy použít služba Azure Import/Export hello?
Zvažte použití služby Azure Import/Export při nahrávání nebo stahování dat přes síť hello je příliš pomalé nebo získání další šířku pásma sítě je cenovou konkurenceschopnost.

Tuto službu můžete použít ve scénářích, jako:

* Migrace dat v cloudu toohello: přesunout velké objemy dat tooAzure rychle a efektivně náklady.
* Distribuci obsahu: rychle odeslat data tooyour lokality zákazníka.
* Zálohování: Provádět zálohování toostore vaše místní data v úložišti objektů blob Azure.
* Obnovení dat: obnovení velké množství dat uložených v úložišti objektů blob a nastavit doručení tooyour místní umístění.

## <a name="prerequisites"></a>Požadavky
V této části jsme seznam hello požadavky požadované toouse této služby. Přečtěte si je pečlivě před přesouvání jednotky.

### <a name="storage-account"></a>Účet úložiště
Musí mít stávající předplatné Azure a jeden nebo více účtů toouse hello importu/exportu služby úložiště. Každá úloha může být použité tootransfer tooor data z jenom jeden účet úložiště. Jinými slovy úlohu jeden importu a exportu nelze rozmístěny napříč více účtů úložiště. Informace o vytvoření nového účtu úložiště najdete v tématu [jak tooCreate účet úložiště](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>Typy objektů BLOB
Data toocopy služba Azure Import/Export můžete použít příliš**bloku** objektů BLOB nebo **stránky** objekty BLOB. Naopak můžete pouze exportovat **bloku** objekty BLOB, **stránky** objektů BLOB nebo **připojení** objekty BLOB ze služby Azure storage používání této služby.

### <a name="job"></a>Úloha
toobegin hello proces importu tooor export z úložiště objektů Blob, nejdřív vytvořit úlohu. Úloha může být úloha importu nebo úlohy exportu:

* Vytvořte úlohu importu, když chcete data tootransfer máte místní tooblobs ve vašem účtu úložiště Azure.
* Vytvoření úlohy exportu když chcete tootransfer dat aktuálně uložené jako objekty BLOB ve vaší jednotky toohard účet úložiště, které jsou dodané tooyou.s, když vytvoříte úlohu, můžete upozornit hello importu a exportu službu, že jste se distribuovat jeden nebo více pevných disků tooan Azure datové centrum.

* Pro úlohy importu bude přesouvání pevné disky obsahující data.
* Pro úlohy exportu bude přesouvání prázdný pevné disky.
* Můžete zaslat až too10 pevných disků na úlohu.

Můžete vytvořit importu nebo exportu úlohy pomocí hello portál Azure nebo hello [REST API služby Azure Storage importu a exportu](/rest/api/storageimportexport).

### <a name="waimportexport-tool"></a>Nástroj WAImportExport
Hello prvním krokem při vytváření **importovat** úlohy je tooprepare jednotky dodané pro import. tooprepare jednotky, je nutné připojit místní server tooa a spuštění hello WAImportExport nástroj na místním serveru hello. Tento nástroj WAImportExport usnadňuje kopírování jednotce toohello dat, šifrování dat hello na jednotce hello s nástrojem BitLocker a generování soubory deníku jednotky hello.

soubory deníku Hello ukládat základní informace o úloze a jednotky, jako jsou jednotky sériové číslo a název účtu úložiště. Tento soubor deníku není uložen na jednotce hello. Používá se při vytvoření úlohy importu. Podrobné informace o vytvoření úlohy zajišťuje později v tomto článku.

Nástroj WAImportExport Hello je pouze kompatibilní s operačním systémem Windows 64-bit. V tématu hello [operačního systému](#operating-system) části pro konkrétní verze operačního systému podporován.

Stáhněte si nejnovější verzi hello hello [WAImportExport nástroj](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExportV2.zip). Další podrobnosti o používání hello WAImportExport nástroj, najdete v části hello [hello pomocí nástroje WAImportExport](storage-import-export-tool-how-to.md).

>[!NOTE]
>**Předchozí verze:** můžete [stáhnout WAImportExpot V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip) verzi hello nástroje a odkazovat příliš[WAImportExpot V1 použití průvodce](storage-import-export-tool-how-to-v1.md). Verze WAImportExpot V1 nástroje hello poskytují podporu pro **Příprava disky, když je už předem zapisovat data toohello disku**. Také budete potřebovat nástroj toouse WAImportExpot V1 Pokud hello k dispozici pouze klíč je klíč SAS.

>

### <a name="hard-disk-drives"></a>Jednotky pevného disku
Pouze 2,5 SSD nebo 2,5" nebo 3.5" SATA II nebo interní HDD III jsou podporovány pro použití s hello služby importu a exportu. Úlohu jeden importu a exportu může mít maximálně 10 pevný disk nebo disky SSD a každé jednotlivé HDD/SSD může mít libovolnou velikost. Velký počet jednotek možné rozdělit do více úloh a neexistuje žádná omezení na hello počet úloh, které lze vytvořit. 

Pro úlohy importu se zpracují pouze hello první datový svazek na disku hello. musí být naformátovaná Hello datový svazek systému souborů NTFS.

> [!IMPORTANT]
> Tato služba nepodporuje externí jednotky pevného disku, které jsou předdefinované adaptérem USB. Navíc hello disku uvnitř hello malá a velká písmena externí pevný disk nelze použít; Neposílejte prosím externí pevné disky.
> 
> 

Níže je seznam externích USB adaptéry používají toocopy data toointernal pevné disky. Anker 68UPSATAA - 02BU Anker 68UPSHHDS BU Startech SATADOCK22UE Orico 6628SUS3-C-černá (6628 řada) Thermaltake BlacX odkládacího horká SATA externí pevné jednotky ukotvení stanice (USB 2.0 & eSATA)

### <a name="encryption"></a>Šifrování
Hello data na jednotce hello musí být šifrované pomocí nástroj BitLocker Drive Encryption. To chrání vaše data, i když je při přenosu.

Pro import úlohy existují dva způsoby tooperform hello šifrování. Hello první způsob je toospecify hello možnost, při použití souboru CSV datové sady při spuštění nástroje WAImportExport hello během přípravy na jednotku. Hello druhý způsob je tooenable šifrování nástrojem BitLocker na jednotce hello ručně a zadejte šifrovací klíč hello v hello driveset sdíleného svazku clusteru při spuštění WAImportExport nástroj příkazového řádku během přípravy na jednotku.

Pro úlohy exportu dat po zkopírovaný toohello jednotky, zašifruje hello služby hello jednotky pomocí Bitlockeru před přesouvání ji zpět tooyou. Hello šifrovací klíč bude poskytnuta tooyou prostřednictvím hello portálu Azure.  

### <a name="operating-system"></a>Operační systém
Můžete použít jednu z následujících 64bitové operační systémy tooprepare hello pevný disk pomocí hello WAImportExport nástroj před přesouvání hello jednotky tooAzure hello:

Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Všechny tyto operační systémy podporují nástroj BitLocker Drive Encryption.

### <a name="locations"></a>Umístění
Služba Azure Import/Export Hello podporuje tooand kopírování dat ze všech účtů úložiště veřejný Azure. Můžete zaslat tooone jednotky pevného disku z následujících umístění hello. Pokud je váš účet úložiště do veřejného umístění Azure, který není zde určený přesouvání alternativní umístění bude třeba zadat při vytváření pomocí úlohy hello hello portál Azure nebo hello importu a exportu REST API.

Podporované přenosů umístění:

* Východ USA
* Západní USA
* Východní USA 2
* Západní USA 2
* Střed USA
* Střed USA – sever
* Střed USA – jih
* Západní střed USA
* Severní Evropa
* Západní Evropa
* Východní Asie
* Jihovýchodní Asie
* Austrálie – východ
* Austrálie – jihovýchod
* Japonsko – západ
* Japonsko – východ
* Střed Indie
* Indie – jih
* Indie – západ
* Střední Kanada
* Východní Kanada
* Brazílie – jih
* Korea – střed
* USA (Gov) – Virginia
* USA (Gov) – Iowa
* US DoD – východ
* US DoD – střed
* Čína – východ
* Čína – sever
* Spojené království – jih

### <a name="shipping"></a>Přesouvání
**Přesouvání disků toohello datového centra:**

Při vytváření úlohu import nebo export, podporovaném adresu příjemce jednoho hello tooship umístění jednotky vám bude poskytnuta. Hello přesouvání zadaná adresa bude záviset na hello umístění účtu úložiště, ale nemusí být hello stejné jako vaše umístění účtu úložiště.

Můžete vytvořit prostředníci jako FedEx, DHL, UPS nebo tooship nám poštovní služby hello vaší jednotky toohello přesouvání adresu.

**Přesouvání jednotky z datového centra hello:**

Při vytváření úlohu import nebo export, musí zadejte zpáteční adresu pro Microsoft toouse při přenosů hello jednotky zpět po dokončení úlohy. Zkontrolujte prosím, že zadejte platný zpáteční adresu tooavoid zpoždění při zpracování.

Poskytovatel zvoleného můžete použít v pořadí tooforward lodě hello pevný disk. Poskytovatel Hello by měl mít odpovídající sledování v pořadí toomaintain předávací protokol. Je taky nutné zadat platný FedEx nebo DHL poskytovatel účet číslo toobe společnost Microsoft používá pro přesouvání hello jednotky zpět. FedEx účet je požadováno pro přesouvání jednotky zpět z hello USA a Evropě umístění. DHL účet je požadováno pro přesouvání jednotky zpět z Asii a Austrálie umístění. Můžete vytvořit [FedEx](http://www.fedex.com/us/oadr/) (pro USA a Evropě) nebo [DHL](http://www.dhl.com/) (Asii a Austrálie) poskytovatel účtu, pokud nemáte jeden. Pokud již máte účet číslo operátora, ověřte, zda je platný.

V přesouvání vlastních balíčků, je třeba postupovat podle hello podmínky v [podmínky služby Microsoft Azure](https://azure.microsoft.com/support/legal/services-terms/).

> [!IMPORTANT]
> Upozorňujeme, že hello fyzického média, který se může být nutné toocross mezinárodní hranice. Jste zodpovědní za zajištění, vaše fyzická média a data jsou importovat a exportovat v souladu s platnými zákony hello. Před přesouvání hello fyzická média, zkontrolujte u vaší tooverify poradci, média a data právními předpisy lze identifikovat uvidíte toohello datového centra. To vám pomůže tooensure nedosáhne Microsoft včas. Například všechny balíček, který bude křížové mezinárodní hranice musí toobe komerční faktury společně s hello balíčku (s výjimkou Pokud překračování hranic v rámci Evropské unie). Může vytiskněte vyplněný kopie hello komerční faktury z webu poskytovatel. Příklad komerční faktury jsou [DHL komerční faktury](http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) a [FedEx komerční faktury](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Ujistěte se, že Microsoft nebyl byly označeny jako hello exportu.
> 
> 

## <a name="how-does-hello-azure-importexport-service-work"></a>Jak funguje služba Azure Import/Export hello?
Přenos dat mezi místními servery a Azure blob storage pomocí služby Azure Import/Export hello vytvořením úlohy a přesouvání jednotky pevného disku tooan datového centra Azure. Každý pevný disk, dodávané se přidružit k jedné úloze. Každá úloha je přidružený k účtu jedno úložiště. Zkontrolujte hello [část předpoklady](#pre-requisites) pečlivě toolearn o hello specifikace této služby, například podporované typy objektů blob, typy disků, umístění a přesouvání.

V této části jsme popisují na vysoké úrovni hello kroky import a export úloh. Dále v hello [Quick Start článku](#quick-start), jsme poskytují toocreate podrobné pokyny importu a exportu úlohy.

### <a name="inside-an-import-job"></a>Uvnitř úlohy importu
Na vysoké úrovni úloha importu zahrnuje hello následující kroky:

* Určit toobe hello data importovat, a hello počet jednotek, které budete potřebovat.
* Identifikujte hello cílové umístění objektu blob pro vaše data v úložišti objektů Blob.
* Použijte hello WAImportExport nástroj toocopy vaše data tooone nebo více pevných disků a je šifrování pomocí nástroje BitLocker.
* Vytvoření úlohy importu v účtu úložiště cíl pomocí hello portál Azure nebo hello importu a exportu REST API. Pokud používáte hello portálu Azure, nahrajte soubory deníku jednotky hello.
* Zadejte zpáteční adresu hello a toobe číslo poskytovatel účet použitý pro přesouvání back tooyou hello jednotky.
* Jednotky pevného disku toohello hello lodě přesouvání adresa zadaná při vytvoření úlohy.
* Aktualizujte hello doručení číslo v podrobnostech úlohy importu hello sledování a odeslání úlohy importu hello.
* Disky jsou přijata a zpracovat hello datového centra Azure.
* Jednotky jsou dodávané pomocí poskytovatel účet toohello zpáteční adresu zadali v úloze importu hello.
  
    ![Obrázek toku 1:Import úlohy](./media/storage-import-export-service/importjob.png)

### <a name="inside-an-export-job"></a>Uvnitř úlohy exportu
Na vysoké úrovni zahrnuje úlohy exportu hello následující kroky:

* Určete hello data toobe exportovali a hello počet jednotek, které budete potřebovat.
* Identifikujte hello zdroje BLOB nebo kontejneru cesty vašich dat v úložišti objektů Blob.
* Vytvoření úlohy exportu ve vašem účtu úložiště zdroje pomocí hello portál Azure nebo hello importu a exportu REST API.
* Zadejte text hello zdroje BLOB nebo úloha exportu kontejneru cesty vaše data v hello.
* Zadejte hello návratové adresy a poskytovatel číslo účtu pro toobe používá pro přesouvání back tooyou hello jednotky.
* Jednotky pevného disku toohello hello lodě přesouvání adresa zadaná při vytvoření úlohy.
* Aktualizujte hello doručení číslo v podrobnostech úlohy exportu hello sledování a odeslání úlohy exportu hello.
* Hello jednotky jsou přijme a zpracuje v hello datového centra Azure.
* Hello jednotky jsou šifrované pomocí nástroje BitLocker; Hello klíče jsou k dispozici prostřednictvím hello portálu Azure.  
* Hello jednotky jsou dodávané pomocí poskytovatel účet toohello zpáteční adresu zadali v úloze importu hello.
  
    ![Obrázek toku 2:Export úlohy](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-and-drive-status"></a>Zobrazení stavu úlohy a jednotky
Můžete sledovat stav hello import nebo export úloh z hello portálu Azure. Klikněte na tlačítko hello **importu a exportu** kartě. Seznam úloh se zobrazí na stránce hello.

![Zobrazení stavu úlohy](./media/storage-import-export-service/jobstate.png)

Zobrazí se jeden z následujících stavy úlohy v závislosti na tom, kde je vaše jednotka v procesu hello hello.

| Stav úlohy | Popis |
|:--- |:--- |
| Vytváření | Po vytvoření úlohy, je její stav nastavit tooCreating. Během úlohy hello hello vytváření stavu, hello službu Import/Export předpokládá hello jednotky nebyly uvidíte toohello datového centra. Úlohy mohou zůstat v hello stav vytváření až tootwo týdnů, po které se automaticky odstraní službou hello. |
| Přesouvání | Po dodáte vašeho balíčku, by měl aktualizovat hello sledování informace v hello portálu Azure.  Tato úloha hello zapnout do "Přesouvání". Úloha Hello zůstane v hello stavu přesouvání až tootwo týdny. 
| Přijaté | Po přijetí všech jednotkách na hello datového centra bude stav úlohy hello nastaven toohello přijaté. |
| Přenos | Po zahájení zpracování alespoň jednu jednotku, bude stav úlohy hello nastaven toohello přenos. Hello jednotky stavy najdete v části níže podrobné informace. |
| Balení | Po dokončení zpracování všech jednotkách, hello úlohy budou umístěny ve stavu balení hello dokud hello jednotky jsou dodané back tooyou. |
| byla dokončena | Po všechny jednotky byly uvidíte back toohello zákazníka, pokud hello úlohy bez chyb, pak hello úlohy se nastaví toohello stav dokončeno. Úloha Hello bude automaticky odstraněna po 90 dnech v hello stav dokončeno. |
| uzavřený | Po všechny jednotky byly uvidíte back toohello zákazníka, pokud zde nejsou žádné chyby během zpracování hello hello úlohy, pak hello úlohy se nastaví toohello uzavřeném stavu. Úloha Hello bude automaticky odstraněna po 90 dnech v uzavřeném stavu hello. |

Následující tabulka Hello popisuje hello životní cyklus jednotlivé jednotky jako přechází prostřednictvím úlohu import nebo export. aktuální stav Hello každé jednotky, v rámci úlohy je nyní vidět hello portálu Azure.
Hello následující tabulka popisuje všechny stavy, které může předávat každé jednotky, v rámci úlohy.

| Stav disku | Popis |
|:--- |:--- |
| Zadaný | Pro úlohu importu z hello portál Azure, bude vytvořena úloha hello hello počátečního stavu pro jednotku při hello zadaná stavu. Pro úlohy exportu vzhledem k tomu, že není zadána žádná jednotka, když je vytvořen hello úlohy, stav počáteční jednotky hello je hello přijaté stavu. |
| Přijaté | jednotka Hello přechází toohello přijaté stavu, kdy hello importu/exportu služby operátor zpracovala hello jednotky, které byly přijaty z hello přesouvání společnosti úlohy importu. Pro úlohy exportu je stav počáteční jednotky hello hello přijaté stav. |
| NeverReceived | jednotka Hello přesune toohello NeverReceived stavu, při přijetí hello balíčku pro úlohu ale hello balíček neobsahuje hello jednotky. Jednotku také můžete přesunout do tohoto stavu, pokud to bylo dva týdny, protože služba hello přijala hello přenosů informace, ale hello balíček nebyl ještě byly přijaty hello datového centra. |
| Přenos | Jednotku přesune toohello přenos stavu, když služba hello zahájí tootransfer dat z jednotky tooWindows hello Azure Storage. |
| byla dokončena | Jednotku přesune toohello stav dokončeno, když má hello service úspěšně přenesla všechna data hello bez chyb.
| CompletedMoreInfo | Jednotku přesune toohello CompletedMoreInfo stav, když služba hello zjistila některé problémy při kopírování dat buď z nebo toohello jednotku. Hello informace může obsahovat chyby, upozornění a informativní zprávy o přepsání objektů BLOB.
| ShippedBack | jednotka Hello přesune toohello ShippedBack stav má zakoupení z hello data center zpět toohello zpáteční adresu. |

Tuto bitovou kopii z portálu Azure hello zobrazuje stav disku hello úlohu příklad:

![Zobrazení stavu jednotky](./media/storage-import-export-service/drivestate.png)

Hello následující tabulka popisuje stavy selhání jednotky hello a hello akcí pro každý stav.

| Stav disku | Událost | Řešení / další krok |
|:--- |:--- |:--- |
| NeverReceived | Jednotka, která je označena jako NeverReceived (protože nebyla přijata jako součást úlohy hello dodávky) dorazí v jiné dodávky. | Hello provozní tým přesune hello jednotky toohello přijaté stavu. |
| Není k dispozici | Jednotku, která není součástí všechny úlohy dorazí na hello datového centra v rámci jiné úlohy. | Hello jednotce budou označeny jako další jednotky a bude vrácen toohello zákazníka po dokončení úlohy hello přidružené k původní balíček hello. |

### <a name="time-tooprocess-job"></a>Čas tooprocess úlohy
Hello množství času potřebné tooprocess úlohu importu a exportu se liší v závislosti na různých faktorech, například dobu dodání, typ úlohy, typ a velikost hello kopírování dat a hello velikost disků hello zadat. Hello importu/exportu služby nemá SLA, ale po hello disky jsou přijetí služby hello usiluje toocomplete hello kopie za 7 dnů too10. Přesněji můžete průběh úlohy hello REST API tootrack hello. V operaci seznam úloh, která poskytuje údaje o průběhu kopírování hello je procenta dokončení parametr. Oslovení toous, pokud budete potřebovat odhad toocomplete úlohu čas kritické importu a exportu.

### <a name="pricing"></a>Ceny
**Jednotka poplatek za zpracování**

Je poplatek za zpracování jednotky pro každou jednotku zpracovat jako součást import nebo export úlohy. O hello najdete v podrobnostech hello [Azure Import/Export ceny](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Přesouvání náklady**

Jestliže doplníte tooAzure jednotky, platíte hello přesouvání poskytovatel přesouvání toohello náklady. Po návratu hello jednotky tooyou Microsoft hello přesouvání náklady je účtován toohello poskytovatel účet, který jste zadali v době hello vytvoření úlohy.

**Cena za transakci**

Neexistují žádné cena za transakci, při importu dat do úložiště objektů blob. Standardní výstupní poplatky Hello se dají použít při exportu dat z úložiště objektů blob. Další informace o cena za transakci, v [přenos dat se ceny.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Rychlý start
V této části jsme poskytují podrobné pokyny pro vytvoření importu a úlohy exportu. Zkontrolujte, zda splňujete všechny hello [předpoklady](#pre-requisites) než budete pokračovat dál.

> [!IMPORTANT]
> Hello služba podporuje jeden účet úložiště standard storage na import nebo export úlohy a nepodporuje prémiové účty úložiště. 
> 
> 
## <a name="create-an-import-job"></a>Vytvoření úlohy importu
Vytvořte tooyour toocopy data úlohy import úložiště Azure účet z pevných disků přesouvání jeden nebo více jednotek obsahujících data toohello zadaného datového centra. Hello úlohy importu přenese tak podrobnosti o jednotkách pevných disků, zkopírovat data toobe, cílový účet úložiště a přesouvání služba Azure Import/Export toohello informace. Vytvoření úlohy importu je proces třech krocích. Nejprve připravte jednotky pomocí nástroje WAImportExport hello. Druhý odeslání úlohy importu pomocí hello portálu Azure. Třetí dodávat toohello jednotky hello přesouvání adresa zadaná během vytváření a aktualizaci hello úlohu přesouvání informace ve vašem podrobnosti úlohy.   

### <a name="prepare-your-drives"></a>Příprava jednotky
Hello prvním krokem při importu dat pomocí služby Azure Import/Export hello je tooprepare jednotky pomocí nástroje WAImportExport hello. Postupujte podle kroků hello tooprepare jednotky.

1. Identifikujte toobe hello data importovat. To může být adresáře a samostatné soubory na místním serveru hello nebo sdílené síťové složce.  
2. Určení hello počet jednotek, které budete potřebovat v závislosti na celkovou velikost dat hello. Pořídit hello požadované číslo 2,5 SSD nebo 2,5" nebo 3,5" SATA II nebo III jednotky pevného disku.
3. Identifikujte hello cílový účet úložiště, kontejner, virtuálních adresářů a objekty BLOB.
4.  Určení hello adresáře nebo samostatné soubory, které budou zkopírovaný tooeach pevný disk.
5.  Vytvoření souborů CSV pro datovou sadu a driveset hello.
    
    **Soubor CSV datové sady**
    
    Dole je příklad příklad souboru CSV datové sady:
    
    ```
    BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
    "F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
    "F:\50M_original\","containername/",BlockBlob,rename,"None",None 
    ```
   
    V hello výše příklad bude 100M_1.csv.txt zkopírovaný toohello kořenovém hello kontejner s názvem "containername". Pokud containername"hello kontejneru název" neexistuje, bude vytvořen. Všechny soubory a složky v části 50M_original bude toocontainername rekurzivně zkopírovali. Struktura složek budou zachována.

    Další informace o [Příprava souboru CSV datovou sadu hello](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    
    **Mějte na paměti,**: ve výchozím nastavení, budou importována hello data jako objekty BLOB bloku. Hello BlobType hodnota pole tooimport data můžete použít jako objekty BLOB stránky. Například pokud importujete soubory virtuálního pevného disku, které se připojí jako disky na virtuální počítač Azure, je nutné je importovat jako objekty BLOB stránky.

    **Soubor Driveset CSV**

    Hodnota Hello hello driveset, je příznak soubor CSV, který obsahuje seznam hello písmena jednotek hello toowhich disky jsou namapované v pořadí pro hello nástroj toocorrectly vyberte hello seznam toobe disky připravené. 

    Dole je příklad hello driveset souboru CSV:
    
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    H,Format,SilentMode,Encrypt,
    ```

    V hello výše příkladu se předpokládá, že jsou připojené dva disky a základní svazků systému souborů NTFS s G:\ písmeno svazku a H:\ byly vytvořeny. Nástroj Hello formátu a šifrování hello disk, který je hostitelem H:\ a nebude formátu nebo šifrování hello disku, který je hostitelem svazku G:\.

    Další informace o [Příprava souboru CSV driveset hello](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file).

6.  Použití hello [WAImportExport nástroj](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) toocopy tooone vaše data nebo více pevných disků.
7.  Můžete zadat "Šifrovat" na pole šifrování v drivset CSV tooenable šifrování nástrojem BitLocker na jednotce pevného disku hello. Alternativně můžete také povolíte šifrování nástrojem BitLocker na jednotce pevného disku hello ručně a zadejte "AlreadyEncrypted" a zadejte klíč hello v hello driveset sdíleného svazku clusteru při spuštění nástroje hello.

8. Neměňte hello dat na jednotky pevného disku hello nebo hello deníku souboru po dokončení Příprava disku.

> [!IMPORTANT]
> Každý jednotku pevného disku, které připravíte způsobí souboru deníku. Při vytváření úlohy importu hello pomocí hello portálu Azure, musíte nahrát všechny soubory deníku hello hello jednotek, které jsou součástí této úlohy importu. Disky bez deníku, které soubory nebudou zpracována.
> 
>

Níže jsou příkazy hello a příklady pro přípravu hello pevný disk pomocí WAImportExport nástroje.

Nástroj WAImportExport PrepImport příkaz pro hello nejdříve zkopírovat relace toocopy adresáře nebo soubory s novou relací kopie:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**Import Příklad 1**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

V pořadí příliš**přidat další jednotky**, jeden vytvořit nový soubor driveset a spusťte příkaz hello jak je uvedeno níže. Pro následné kopírování relací toohello různých diskové jednotky než je zadáno v souboru .csv InitialDriveset zadejte nový soubor CSV driveset a zadat jako parametr toohello hodnotu "AdditionalDriveSet". Použití hello **stejný soubor deníku** název a zadejte **nové ID relace**. Formát souboru AdditionalDriveset CSV Hello je stejný jako formát InitialDriveSet.

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
```

**Import příklad 2**
```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
```

V pořadí tooadd další data toohello stejné driveset WAImportExport nástroj příkaz PrepImport lze volat pro následné relace toocopy další soubory nebo adresáře kopie: pro následné kopírování relací toohello stejné pevné disky zadané v CSV InitialDriveset souboru, zadejte hello **stejný soubor deníku** název a zadejte **nové ID relace**; je klíč účtu úložiště bez nutnosti tooprovide hello.

```
WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
```

**Import příklad 3**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Zobrazte další podrobnosti o použití nástroje WAImportExport hello v [Příprava pevné disky pro import](storage-import-export-tool-preparing-hard-drives-import.md).

Viz také toohello [ukázkový pracovní postup tooprepare pevné disky pro úlohy importu](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md) podrobnější pokyny krok za krokem.  

### <a name="create-hello-import-job"></a>Vytvoření úlohy importu hello
1. Jakmile připravíte jednotce, přejděte tooyour účet úložiště v hello portál Azure a zobrazit hello řídicího panelu. V části **rychlý přehled**, klikněte na tlačítko **vytvořit úlohu importu**. Projděte si kroky hello a vyberte tooindicate hello zaškrtávací políčko, že je vše připraveno vašeho disku a zda máte k dispozici souboru hello disku deníku.
2. V kroku 1 zadejte kontaktní informace pro hello osoba odpovědná za tato úloha importu a platný zpáteční adresu. Pokud chcete data podrobného protokolování toosave úlohy importu hello, zaškrtněte možnost hello příliš**uložit hello podrobného protokolování v kontejneru objektů blob Moje 'waimportexport'**.
3. V kroku 2 nahrajte hello jednotky deníku soubory, které jste získali během kroku přípravy jednotky hello. Budete potřebovat tooupload jeden soubor pro každou jednotku, který jste připravili.
   
   ![Vytvoření úlohy importu – krok 3](./media/storage-import-export-service/import-job-03.png)
4. V kroku 3 zadejte popisný název úlohy importu hello. Všimněte si, že hello název může obsahovat jenom malá písmena, číslice, pomlčky a podtržítka, musí začínat písmenem a nesmí obsahovat mezery. Použijete název hello zvolíte tootrack úlohách době, kdy jsou v průběhu a po jejich dokončení.
   
   V dalším kroku vyberte oblast dat center hello seznamu. Středová oblast dat Hello označí hello data center a adres toowhich, je nutné dodat vašeho balíčku. V tématu Nejčastější dotazy hello níže Další informace.
5. V kroku 4 vyberte vaše návratový poskytovatel hello seznamu a zadejte číslo svého operátora účtu. Po dokončení importu úlohu, bude společnost Microsoft používat tento účet tooship hello jednotky back tooyou.
   
   Pokud máte vaše číslo sledování, svého operátora doručení hello seznamu vyberte a zadejte číslo sledování.
   
   Pokud nemáte sledování Číslo ještě zvolte **po poslali Moje balíček informace o tomto přesouvání pro tuto úlohu importu zajistí**, dokončete proces importu hello.
6. Vrátí číslo svého sledování po byly součástí vašeho balíčku tooenter toohello **importu a exportu** stránky pro váš účet úložiště v hello portálu Azure, vyberte úlohu hello seznamu a zvolte **přesouvání informace**. Procházet hello průvodce a zadejte číslo sledování v kroku 2.
   
    Pokud hello číslo sledování není aktualizován v rámci 2 týdny vytváření hello úlohy, hello úloha vyprší.
   
    Pokud úloha hello je ve stavu vytvoření, přesouvání nebo přenos hello, můžete také aktualizovat číslo svého operátora účtu v kroku 2 Průvodce hello. Jakmile hello úloha je ve stavu hello balení, nelze aktualizovat číslo svého operátora účtu pro dané úlohy.
7. Průběh úlohy můžete sledovat na řídicím panelu portálu hello. V tématu každý stav úlohy v předchozí části hello znamená podle [zobrazení stavu vaše úlohy](#viewing-your-job-status).

## <a name="create-an-export-job"></a>Vytvoření úlohy exportu
Vytvořte toonotify hello úlohy exportu služby importu a exportu, že je budete přesouvání, jeden nebo více prázdný jednotky toohello datového centra softwaru tak, aby data se dá vyexportovat ze jednotky toohello účtu úložiště a jednotky hello pak dodaný tooyou.

### <a name="prepare-your-drives"></a>Příprava jednotky
Příprava jednotky pro úlohy exportu doporučujeme následující předběžné kontroly:

1. Zkontrolujte hello počet disků požadované příkazem PreviewExport nástroj WAImportExport hello. Další informace najdete v tématu [náhled jednotka využití pro úlohu exportovat](https://msdn.microsoft.com/library/azure/dn722414.aspx). Pomůže vám náhled využití disku pro objekty BLOB hello, které jste vybrali, na základě velikosti hello jednotek hello budete toouse.
2. Zkontrolujte, jestli je můžete pro čtení a zápis toohello pevný disk, který má být dodán pro úlohu export hello.

### <a name="create-hello-export-job"></a>Vytvoření úlohy exportu hello
1. toocreate úlohy exportu, přejděte tooyour účet úložiště v hello portálu Azure a zobrazit hello řídicího panelu. V části **rychlý přehled**, klikněte na tlačítko **vytvořit úlohu exportovat** a pokračovat v Průvodci hello.
2. V kroku 2 zadejte kontaktní informace pro hello osoba odpovědná za tuto úlohu exportu. Pokud chcete data toosave podrobného protokolování pro úlohu export hello, zaškrtněte možnost hello příliš**uložit hello podrobného protokolování v kontejneru objektů blob Moje 'waimportexport'**.
3. V kroku 3 zadejte které blob data chcete tooexport z vašeho úložiště účet tooyour prázdný jednotek nebo disků. Můžete zvolit tooexport všechna data objektů blob v účtu úložiště hello nebo můžete určit, které objekty BLOB nebo nastaví z tooexport objekty BLOB.
   
   toospecify tooexport objektů blob použít hello **rovno** selektor a zadejte hello relativní cestu toohello objektů blob, počínaje hello název kontejneru. Použití *$root* toospecify hello Kořenový kontejner.
   
   toospecify všechny objekty BLOB spouštění s předponou, použijte hello **začíná** selektor a zadejte předponu hello, počínaje lomítkem '/'. Předpona Hello může být předponou hello hello název kontejneru, hello kontejneru úplný název nebo název dokončení kontejneru hello následuje hello předpona názvu objektu blob hello.
   
   Hello následující tabulka uvádí příklady cesty platný objekt blob:
   
   | Selektor | Cesta k objektu BLOB | Popis |
   | --- | --- | --- |
   | Začíná |/ |Exportuje všech objektů BLOB v účtu úložiště hello |
   | Začíná |/$root / |Exportuje všech objektů BLOB v kontejneru kořenové hello |
   | Začíná |/Book |Exportuje všech objektů BLOB v kontejneru, který začíná předponu **adresáře** |
   | Začíná |/Music/ |Exportuje všech objektů BLOB v kontejneru **Hudba** |
   | Začíná |/ Hudba/láska |Exportuje všech objektů BLOB v kontejneru **Hudba** které začínají předponou **rádi** |
   | EQUAL příliš|$root/logo.bmp |Export objektu blob **logo.bmp** v kořenovém kontejneru hello |
   | EQUAL příliš|videos/Story.MP4 |Export objektu blob **story.mp4** v kontejneru **videa** |
   
   Je nutné zadat cesty hello objektů blob v platné formáty tooavoid chyb během zpracování, jak je vidět na tomto snímku obrazovky.
   
   ![Vytvoření úlohy exportu – krok 3](./media/storage-import-export-service/export-job-03.png)
4. V kroku 4 zadejte popisný název pro úlohu export hello. Hello název může obsahovat jenom malá písmena, číslice, pomlčky a podtržítka, musí začínat písmenem a nesmí obsahovat mezery.
   
   Středová oblast dat Hello označí hello data center toowhich je nutné dodat vašeho balíčku. V tématu Nejčastější dotazy hello níže Další informace.
5. V kroku 5 vyberte vaše návratový poskytovatel hello seznamu a zadejte číslo účtu poskytovatel. Microsoft použije tento účet tooship jednotky zpět tooyou po dokončení exportu úlohu.
   
   Pokud máte vaše číslo sledování, svého operátora doručení hello seznamu vyberte a zadejte číslo sledování.
   
   Pokud nemáte sledování Číslo ještě zvolte **po poslali Moje balíček informace o tomto přesouvání pro tuto úlohu export zajistí**, dokončete proces exportu hello.
6. Vrátí číslo svého sledování po byly součástí vašeho balíčku tooenter toohello **importu a exportu** stránky pro váš účet úložiště v hello portálu Azure, vyberte úlohu hello seznamu a zvolte **přesouvání informace**. Procházet hello průvodce a zadejte číslo sledování v kroku 2.
   
    Pokud hello číslo sledování není aktualizován v rámci 2 týdny vytváření hello úlohy, hello úloha vyprší.
   
    Pokud úloha hello je ve stavu vytvoření, přesouvání nebo přenos hello, můžete také aktualizovat číslo svého operátora účtu v kroku 2 Průvodce hello. Jakmile hello úloha je ve stavu hello balení, nelze aktualizovat číslo svého operátora účtu pro dané úlohy.
   
   > [!NOTE]
   > Pokud toobe blob hello exportovat se používá v době hello kopírování toohard disku, bude služba Azure Import/Export pořízení snímku hello objektů blob a zkopírujte hello snímku.
   > 
   > 
7. Průběh úlohy můžete sledovat na řídicím panelu hello v hello portálu Azure. V tématu každý stav úlohy znamená v předchozí části hello na "zobrazení stavu vaše úlohy".
8. Po obdržení hello jednotky s exportovaná data, můžete zobrazit a zkopírujte klíče nástroje BitLocker hello vygenerované službou hello jednotky. Účet úložiště tooyour hello portálu Azure přejděte a klikněte na kartě importu a exportu hello. Vyberte úlohu export hello seznamu a klikněte na tlačítko Zobrazit klíče hello. klíče Bitlockeru Hello vypadat takto:
   
   ![Zobrazit klíče nástroje BitLocker pro úlohu export](./media/storage-import-export-service/export-job-bitlocker-keys.png)

Přejděte prostřednictvím v části Nejčastější dotazy hello níže, jak vysvětluje hello nejčastější dotazy, které se zákazníci setkávají při používání této služby.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Můžete zkopírovat Azure File storage pomocí služby Azure Import/Export hello?**

Ne, hello služba Azure Import/Export podporuje pouze objekty BLOB bloku a objekty BLOB stránky. Všechny ostatní typy úložiště včetně úložiště Azure File, úložiště Table a Queue Storage nejsou podporovány.

**Služba Azure Import/Export hello je dostupná pro předplatná CSP?**

Služba Azure Import/Export podporuje odběry CSP.

**Můžete přeskočit krok přípravy hello jednotky pro úlohy importu nebo můžete připravit na jednotku bez kopírování?**

Všechny jednotky, které chcete pro import dat tooship musí připravený pomocí nástroje Azure WAImportExport hello. Je nutné použít hello WAImportExport nástroj toocopy data toohello jednotky.

**Je nutné tooperform všechny Příprava disku při vytváření úlohy exportu?**

Doporučuje se žádné, ale některé předběžné kontroly. Zkontrolujte hello počet disků požadované příkazem PreviewExport nástroj WAImportExport hello. Další informace najdete v tématu [náhled jednotka využití pro úlohu exportovat](https://msdn.microsoft.com/library/azure/dn722414.aspx). Pomůže vám náhled využití disku pro objekty BLOB hello, které jste vybrali, na základě velikosti hello jednotek hello budete toouse. Úloha exportu také zkontrolujte, zda může číst z a toohello zápisu pevný se disk, který má být dodán pro hello.

**Co se stane, když omylem odeslat pevný disk, který není v souladu s toohello podporovány požadavky?**

Hello datové centrum Azure vrátí hello jednotku, která není v souladu s požadavky tooyou toohello podporována. Pokud jenom některé jednotky hello v balíčku hello splnit požadavky na podporu hello, tyto jednotky se zpracují a hello jednotky, které nesplňují požadavky hello bude vrácen tooyou.

**Můžete zrušit má úloha?**

Úlohy můžete zrušit, pokud je jeho stav vytváření nebo přesouvání.

**Jak dlouho můžete zobrazit stav hello dokončené úlohy v hello portál Azure?**

Můžete zobrazit stav hello pro dokončené úlohy pro až too90 dnů. Dokončené úlohy budou odstraněny po 90 dnech.

**Pokud I má tooimport nebo exportovat víc než 10 jednotky, co mám dělat?**

Jeden import nebo export úloha může odkazovat jenom 10 jednotky v rámci jedné úlohy pro službu Import/Export hello. Pokud chcete tooship více než 10 disků, můžete vytvořit více úloh. Jednotky, které jsou přidruženy hello odeslání stejnou úlohu současně ve hello stejného balíčku.
Společnost Microsoft nabízí pokyny a pomoc při kapacity dat zahrnuje více disku úlohy importu. Obraťte se na bulkimport@microsoft.com Další informace

**Nepodporuje hello služby hello disky ve formátu před vrácením je?**

Ne. Všechny jednotky jsou šifrované pomocí Bitlockeru.

**Můžete zakoupit jednotky pro úlohy importu a exportu od společnosti Microsoft?**

Ne. Budete potřebovat tooship vlastní jednotky pro obě import a export úloh.

** Jak může přistupovat k datům, importovaných pomocí této služby **

Hello data v rámci účtu úložiště Azure je přístupná prostřednictvím hello portálu Azure nebo pomocí samostatný nástroj nazývá Průzkumník úložišť. https://docs.microsoft.com/en-us/Azure/vs-Azure-Tools-Storage-Manage-with-Storage-Explorer 

**Po dokončení úlohy importu hello, co bude Moje data vypadat v účtu úložiště hello? Moje hierarchie adresářů se zachovají?**

Při přípravě na pevný disk pro úlohy importu, je zadána hello cílového pole DstBlobPathOrPrefix hello v datové sadě sdíleného svazku clusteru. Toto je hello cílový kontejner v toowhich účet úložiště hello, který se zkopíruje data z pevného disku hello. V tomto kontejneru cílové virtuální adresáře jsou vytvořeny pro složky z pevného disku hello a objekty BLOB jsou vytvořené pro soubory. 

**Pokud hello jednotka obsahuje soubory, které již existují v svůj účet úložiště, bude služba hello přepsat existující objekty BLOB v svůj účet úložiště?**

Při přípravě hello jednotku, můžete určit, zda hello cílové soubory by měl být přepsány nebo ignorovat pomocí hello pole v souboru CSV datovou sadu s názvem dispozice: < přejmenovat | přepsat ne | přepsat >. Ve výchozím nastavení hello služby bude přejmenovat hello nové soubory a nikoli přepsat existující objekty BLOB.

**Nástroj WAImportExport hello je kompatibilní s 32bitové operační systémy?**
Ne. Nástroj WAImportExport Hello je pouze kompatibilní s operačními systémy Windows 64-bit. Naleznete v části toohello operačních systémů v hello [předpoklady](#pre-requisites) pro úplný seznam podporovaných verzí operačního systému.

**By měla obsahovat jakoukoli jinou hodnotu než hello pevný disk v mé balíčku?**

Prosím dodávat pouze pevné disky. Nezahrnujte věci, jako je napájecích kabelů napájení nebo kabely USB.

**Je nutné provést tooship Moje jednotky pomocí FedEx nebo DHL?**

Můžete zaslat jednotky toohello datového centra pomocí žádné známé poskytovatel jako FedEx DHL, UPS nebo nám poštovní služby. Pro přenosů hello jednotky back tooyou z hello datového centra, je však nutné zadat číslo účtu FedEx hello USA a EU nebo číslo účtu DHL hello Asii a oblastí, Austrálie.

**Existují nějaká omezení s přesouvání mezinárodní úrovni svou jednotku?**

Upozorňujeme, že hello fyzického média, který se může být nutné toocross mezinárodní hranice. Jste zodpovědní za zajištění, vaše fyzická média a data jsou importovat a exportovat v souladu s platnými zákony hello. Před přesouvání hello fyzická média, zkontrolujte u vaší tooverify poradci, média a data právními předpisy lze identifikovat uvidíte toohello datového centra. To vám pomůže tooensure nedosáhne Microsoft včas.

**Při vytváření úlohy, hello přesouvání adresa je umístění, které se liší od umístění účtu úložiště. Co bych měl/a dělat?**

Některé umístění účtu úložiště, jsou namapované tooalternate přesouvání umístění. Dříve k dispozici přesouvání umístění může být také dočasně namapované tooalternate umístění. Vždy zkontrolujte hello přesouvání adresa zadaná při vytváření úlohy před přesouvání jednotky.

**Při přesouvání svou jednotku, poskytovatel hello požádá o hello data center kontaktní adresa a telefonní číslo. Co by měl poskytovat?**

Hello řadiče domény a telefonní číslo adresy se poskytuje jako součást úlohy vytvoření.

**Můžete použít hello Azure Import/Export služby toocopy PST poštovních schránek a tooO365 dat služby SharePoint?**

Naleznete příliš[Import PST souborů nebo tooOffice dat služby SharePoint 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Můžete použít toocopy služba Azure Import/Export hello Moje zálohování offline toohello služby zálohování Azure?**

Naleznete příliš[pracovní postup Offline zálohování v Azure Backup](../backup/backup-azure-backup-import-export.md).

**Co je hello maximální počet HDD pro v jedné dodávky?**

Může být libovolný počet pevných disků v jeden dodávky a pokud hello disky patří toomultiple úlohy se doporučuje příliš a) disky hello označený verzí hello odpovídající název úlohy. b) úlohy aktualizace hello s číslem sledování na konci -1,-2 atd.
  
**Co je hello maximální objekt Blob bloku a velikost objektu Blob stránky podporovány disku importu a exportu?**

Objekt Blob bloku maximální velikost je přibližně 4.768TB nebo 5 000 000 MB.
Objekt Blob stránky maximální velikost je 1TB.

**Podporuje disku importu a exportu šifrování AES 256?**

Služba Azure Import/Export ve výchozím nastavení zašifruje pomocí nástroje bitlocker šifrování AES 128, ale může to být zvýšená tooAES 256 ručně šifrování nástrojem bitlocker před data budou zkopírována. 

Pokud používáte [WAImportExpot V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), zde je ukázka příkazu
```
WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
```
Pokud používáte [WAImportExport nástroj](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) zadejte "AlreadyEncrypted" a zadejte klíč hello v hello driveset sdíleného svazku clusteru.
```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
```
## <a name="next-steps"></a>Další kroky

* [Nastavení nástroje WAImportExport hello](storage-import-export-tool-how-to.md)
* [Přenos dat pomocí hello příkazového řádku azcopy](storage-use-azcopy.md)
* [Ukázka Azure Import, Export REST API](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

