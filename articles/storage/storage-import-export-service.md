---
title: "Používání Azure Import/Export pro přenos dat do a z úložiště objektů blob | Microsoft Docs"
description: "Naučte se vytvořit import a export úloh na portálu Azure pro přenos dat do a z úložiště objektů blob."
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
ms.openlocfilehash: 9dc50a101384bb40ad3a878245b80dcb31a7c08e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-blob-storage"></a>Používat službu Microsoft Azure Import/Export k přenosu dat do úložiště objektů blob
Služba Azure Import/Export umožňuje bezpečně přenést velké objemy dat do Azure blob storage pomocí přenosů jednotky pevného disku pro datové centrum Azure. Tuto službu můžete použít také k přenosu dat z Azure blob storage na jednotky pevného disku a dodávat k místní lokalitě. Tato služba je vhodný v situacích, kdy chcete přenést několika terabajtů (TB) dat do nebo z Azure, ale je nemožné z důvodu omezenou šířkou pásma nebo vysokou sítě náklady na nahrávání nebo stahování přes síť.

Služba vyžaduje, aby pevné disky pro zabezpečení vašich dat šifrovaná nástrojem BitLocker. Služba podporuje jak Classic a Azure Resource Manager účty úložiště (standardní a studená úroveň) existuje ve všech oblastech veřejný Azure. Je nutné dodat jednotky pevného disku na jeden z podporovaných umístění zadané později v tomto článku.

V tomto článku můžete další informace o službě Azure Import/Export a jak dodat jednotky pro kopírování dat z Azure Blob storage a do.

## <a name="when-should-i-use-the-azure-importexport-service"></a>Kdy by se měla použít služba Azure Import/Export?
Zvažte použití služby Azure Import/Export při nahrávání nebo stahování dat přes síť je příliš pomalé nebo získání další šířku pásma sítě je cenovou konkurenceschopnost.

Tuto službu můžete použít ve scénářích, jako:

* Migrace dat do cloudu: rychle přesouvat velké objemy dat do Azure a nákladově efektivní.
* Distribuci obsahu: rychle posílat data do lokalit zákazníka.
* Zálohování: Trvat záloh vaše místní data ukládat do úložiště objektů blob v Azure.
* Obnovení dat: obnovení velké množství dat uložených v úložišti objektů blob a nastavit doručení vaše místní umístění.

## <a name="prerequisites"></a>Požadavky
V této části jsme seznam požadovaných součástí pro tuto službu využívat. Přečtěte si je pečlivě před přesouvání jednotky.

### <a name="storage-account"></a>Účet úložiště
Musí mít stávající předplatné Azure a jeden nebo více účtů úložiště používat službu Import/Export. Každá úloha může použít k přenosu dat do nebo z jenom jeden účet úložiště. Jinými slovy úlohu jeden importu a exportu nelze rozmístěny napříč více účtů úložiště. Informace o vytvoření nového účtu úložiště najdete v tématu [postup vytvoření účtu úložiště](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>Typy objektů BLOB
Služba Azure Import/Export můžete použít ke zkopírování dat do **bloku** objektů BLOB nebo **stránky** objekty BLOB. Naopak můžete pouze exportovat **bloku** objekty BLOB, **stránky** objektů BLOB nebo **připojení** objekty BLOB ze služby Azure storage používání této služby.

### <a name="job"></a>Úloha
Chcete-li zahájit proces pro import nebo export z úložiště objektů Blob, nejdřív vytvořit úlohu. Úloha může být úloha importu nebo úlohy exportu:

* Pokud chcete k přenosu dat máte místně na objekty BLOB v účtu úložiště Azure, vytvořte úlohy importu.
* Vytvoření úlohy exportu když chcete k přenosu dat, které jsou aktuálně uloženy jako objekty BLOB ve vašem účtu úložiště pro pevné disky, které jsou součástí na něž při vytvoření úlohy, můžete upozornit službu Import/Export, které bude distribuovat jeden nebo více pevných disků pro datové centrum Azure.

* Pro úlohy importu bude přesouvání pevné disky obsahující data.
* Pro úlohy exportu bude přesouvání prázdný pevné disky.
* Můžete zaslat až 10 pevných disků na úlohu.

Můžete vytvořit importu nebo exportu úlohy pomocí portálu Azure nebo [REST API služby Azure Storage importu a exportu](/rest/api/storageimportexport).

### <a name="waimportexport-tool"></a>Nástroj WAImportExport
Prvním krokem při vytváření **importovat** úloha je k přípravě vaše jednotky, které bude dodáno pro import. Příprava jednotky, je třeba připojit k místní server a spustit nástroj WAImportExport na místním serveru. Tento nástroj WAImportExport usnadňuje kopírování dat na jednotku, šifrování dat na jednotce s nástrojem BitLocker a generování souborů deníku jednotky.

Soubory deníku ukládat základní informace o úloze a jednotky, jako jsou jednotky sériové číslo a název účtu úložiště. Tento soubor deníku není uložená na disku. Používá se při vytvoření úlohy importu. Podrobné informace o vytvoření úlohy zajišťuje později v tomto článku.

Nástroj WAImportExport je jenom kompatibilní s operačním systémem Windows 64-bit. Najdete v článku [operačního systému](#operating-system) části pro konkrétní verze operačního systému podporován.

Stáhněte si nejnovější verzi [WAImportExport nástroj](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExportV2.zip). Další podrobnosti o použití nástroje WAImportExport najdete v tématu [pomocí nástroje WAImportExport](storage-import-export-tool-how-to.md).

>[!NOTE]
>**Předchozí verze:** můžete [stáhnout WAImportExpot V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip) verze nástroje a odkazovat na [WAImportExpot V1 použití průvodce](storage-import-export-tool-how-to-v1.md). Verze WAImportExpot V1 nástroje poskytují podporu pro **Příprava disky, když je už předem zapisovat data na disk**. Také budete muset použít nástroj WAImportExpot V1, pokud je k dispozici pouze klíč SAS klíč.

>

### <a name="hard-disk-drives"></a>Jednotky pevného disku
Pouze 2,5 SSD nebo 2,5" nebo 3.5" SATA II nebo interní HDD III jsou podporovány pro použití se službou importu a exportu. Úlohu jeden importu a exportu může mít maximálně 10 pevný disk nebo disky SSD a každé jednotlivé HDD/SSD může mít libovolnou velikost. Velký počet jednotek možné rozdělit do více úloh a neexistuje žádná omezení na počet úloh, které lze vytvořit. 

Pro úlohy importu se zpracují pouze první datový svazek na disku. Datový svazek musí být formátován pomocí systému souborů NTFS.

> [!IMPORTANT]
> Tato služba nepodporuje externí jednotky pevného disku, které jsou předdefinované adaptérem USB. Navíc nelze použít uvnitř malá a velká písmena externí pevný disk na disk; Neposílejte prosím externí pevné disky.
> 
> 

Níže je seznam externích adaptéry USB použít ke zkopírování dat do interní pevné disky. Anker 68UPSATAA - 02BU Anker 68UPSHHDS BU Startech SATADOCK22UE Orico 6628SUS3-C-černá (6628 řada) Thermaltake BlacX odkládacího horká SATA externí pevné jednotky ukotvení stanice (USB 2.0 & eSATA)

### <a name="encryption"></a>Šifrování
Data na disku musí být šifrované pomocí nástroj BitLocker Drive Encryption. To chrání vaše data, i když je při přenosu.

Pro import úlohy existují dva způsoby, jak provést šifrování. První způsob je zadejte možnost při použití souboru CSV datové sady při spuštění nástroje WAImportExport během přípravy na jednotku. Druhý způsob je povolíte šifrování nástrojem BitLocker na jednotce ručně a zadejte šifrovací klíč v driveset sdíleného svazku clusteru při spuštění WAImportExport nástroj příkazového řádku během přípravy na jednotku.

Pro úlohy exportu po zkopírování dat na discích, bude služba šifrování jednotky pomocí nástroje BitLocker před přesouvání zpět do. Šifrovací klíč vám bude poskytnuta prostřednictvím portálu Azure.  

### <a name="operating-system"></a>Operační systém
Příprava pevný disk pomocí nástroje WAImportExport před přesouvání jednotky do Azure můžete použít jednu z následujících 64bitových operačních systémů:

Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise, Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Všechny tyto operační systémy podporují nástroj BitLocker Drive Encryption.

### <a name="locations"></a>Umístění
Služba Azure Import/Export podporuje kopírování dat do a ze všech účtů úložiště veřejný Azure. Můžete zaslat jednotky pevného disku na jeden z následujících umístění. Pokud váš účet úložiště je do veřejného umístění Azure, který není zde určený přesouvání alternativní umístění bude třeba zadat při vytváření úlohy pomocí portálu Azure nebo REST API pro Import nebo Export.

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
**Přesouvání jednotky k datovému centru:**

Při vytváření úlohu import nebo export, bude třeba zadat adresu příjemce jednoho z podporovaných umístění pro odeslání jednotky. Zadaná adresa přesouvání bude záviset na umístění účtu úložiště, ale nemusí být stejný jako vaše umístění účtu úložiště.

Prostředníci jako FedEx DHL, UPS nebo poštovní služba nám můžete použít pro odeslání na adresu přesouvání jednotky.

**Přesouvání jednotky v datovém centru:**

Při vytváření úlohu import nebo export, musí se zadat zpáteční adresu pro společnost Microsoft k použití při přesouvání jednotky zpět po dokončení úlohy. Zkontrolujte prosím, že zadáte platná zpáteční adresa. aby se zabránilo zpoždění při zpracování.

Chcete-li předat dodávat pevný disk můžete rozumí provozovatel podle svého výběru. Zařadit by měl mít odpovídající sledování, aby byla zachována o jeho postupném předávání. Je taky nutné zadat platný FedEx nebo DHL poskytovatel účet číslo, které má být společnost Microsoft používá pro přesouvání jednotky zpět. FedEx účet je požadováno pro přesouvání jednotky zpět z USA a Evropě umístění. DHL účet je požadováno pro přesouvání jednotky zpět z Asii a Austrálie umístění. Můžete vytvořit [FedEx](http://www.fedex.com/us/oadr/) (pro USA a Evropě) nebo [DHL](http://www.dhl.com/) (Asii a Austrálie) poskytovatel účtu, pokud nemáte jeden. Pokud již máte účet číslo operátora, ověřte, zda je platný.

V přesouvání vlastních balíčků, je třeba postupovat podle podmínek na [podmínky služby Microsoft Azure](https://azure.microsoft.com/support/legal/services-terms/).

> [!IMPORTANT]
> Upozorňujeme, že fyzická média, která jsou přesouvání muset křížové mezinárodní hranice. Jste zodpovědní za zajištění, vaše fyzická média a data jsou importovat a exportovat v souladu s platné zákony. Před jejich odesláním fyzických médií, zkontrolujte s vaší poradci ověřit, jestli se médiu a dat můžete souladu s právem odeslaná do identifikovaného datového centra. To vám pomůže zajistit, aby obdržel Microsoft včas. Například všechny balíček, který bude křížové mezinárodní hranice musí obchodní faktury, chcete-li být dodávány spolu s balíčkem (s výjimkou Pokud překračování hranic v rámci Evropské unie). Může vytiskněte vyplněný kopie komerční faktury z webu poskytovatel. Příklad komerční faktury jsou [DHL komerční faktury](http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) a [FedEx komerční faktury](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Ujistěte se, že Microsoft nebyl byly označeny jako exportu.
> 
> 

## <a name="how-does-the-azure-importexport-service-work"></a>Jak funguje služba Azure Import/Export?
Přenos dat mezi místními servery a Azure blob storage pomocí služby Azure Import/Export vytvořením úlohy a přesouvání jednotky pevného disku pro datové centrum Azure. Každý pevný disk, dodávané se přidružit k jedné úloze. Každá úloha je přidružený k účtu jedno úložiště. Zkontrolujte [část předpoklady](#pre-requisites) pečlivě a zjistěte, jaké jsou specifikace této služby, například podporované typy objektů blob, typy disků, umístění a přesouvání.

V této části jsme popisují na vysoké úrovni kroky import a export úloh. Dále v [Quick Start článku](#quick-start), jsme poskytují podrobné pokyny k vytvoření importu a exportu úlohy.

### <a name="inside-an-import-job"></a>Uvnitř úlohy importu
Na vysoké úrovni úloha importu zahrnuje následující kroky:

* Určete data, která mají být importována a počet jednotek, které budete potřebovat.
* Určete cílové umístění objektu blob pro vaše data v úložišti objektů Blob.
* Pomocí nástroje WAImportExport si zkopírujte svá data na jeden nebo více pevných disků a je šifrování pomocí nástroje BitLocker.
* Vytvoření úlohy importu v účtu úložiště cíl pomocí portálu Azure nebo REST API pro Import nebo Export. Pokud používáte portál Azure, nahrajte soubory deníku jednotky.
* Zadejte zpáteční adresu a číslo účtu poskytovatel má být použit pro přesouvání jednotky pro vás.
* Dodávat pevných disků o přenosů adresa zadaná při vytvoření úlohy.
* Aktualizujte doručení sledování Číslo v podrobnostech úlohy importu a odeslání úlohy importu.
* Disky jsou přijata a zpracovat datového centra Azure.
* Jednotky jsou dodávané pomocí účtu poskytovatel návratové adresy uvedené v úloze importu.
  
    ![Obrázek toku 1:Import úlohy](./media/storage-import-export-service/importjob.png)

### <a name="inside-an-export-job"></a>Uvnitř úlohy exportu
Na vysoké úrovni úlohy exportu zahrnuje následující kroky:

* Určete data, která mají být exportovány a počet jednotek, které budete potřebovat.
* Určete zdroj objektů BLOB nebo kontejneru cesty vašich dat v úložišti objektů Blob.
* Vytvoření úlohy exportu ve vašem účtu úložiště zdroje pomocí portálu Azure nebo REST API pro Import nebo Export.
* Zadejte zdroj objektů BLOB nebo kontejneru cest vaše data v úloha exportu.
* Zadejte zpáteční adresu a číslo účtu poskytovatel má být použit pro přesouvání jednotky pro vás.
* Dodávat pevných disků o přenosů adresa zadaná při vytvoření úlohy.
* Aktualizujte doručení číslo v podrobnostech úlohy exportu sledování a odeslání úlohy exportu.
* Jednotky jsou přijme a zpracuje v datového centra Azure.
* Jednotky jsou šifrované pomocí nástroje BitLocker; klíče jsou k dispozici prostřednictvím portálu Azure.  
* Jednotky jsou dodávané pomocí účtu poskytovatel návratové adresy uvedené v úloze importu.
  
    ![Obrázek toku 2:Export úlohy](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-and-drive-status"></a>Zobrazení stavu úlohy a jednotky
Můžete sledovat stav import nebo export úloh z portálu Azure. Klikněte **importu a exportu** kartě. Na stránce se zobrazí seznam vašich úloh.

![Zobrazení stavu úlohy](./media/storage-import-export-service/jobstate.png)

Zobrazí se jeden z následujících stavů úlohy v závislosti na tom, kde je vaše jednotka v procesu.

| Stav úlohy | Popis |
|:--- |:--- |
| Vytváření | Po vytvoření úlohy, je její stav nastavit na vytváření. Když úloha je ve stavu vytvoření, službu Import/Export předpokládá, že jednotky nebyly byla odeslaná do datového centra. Úlohy mohou zůstat ve stavu vytvoření až dvou týdnů, po které se automaticky odstraní službou. |
| Přesouvání | Po dodáte vašeho balíčku, by měl aktualizovat informace o sledování na portálu Azure.  Tato úloha zapnout do "Přesouvání". Úloha zůstane ve stavu přesouvání dobu až dvou týdnů. 
| Přijaté | Po přijetí všech jednotkách v datovém centru, nastaví se na přijaté stav úlohy. |
| Přenos | Alespoň jedna jednotka zahájení zpracování, bude stav úlohy na přenos nastavovat. Najdete v části stavy jednotky pod podrobné informace. |
| Balení | Po dokončení zpracování všech jednotkách, úlohy budou umístěny ve stavu balení dokud jednotky jsou sice vám. |
| byla dokončena | Po všechny jednotky byly dodány zpět na zákazníka, pokud úloha byla dokončena bez chyb, bude úloha nastavit stav dokončeno. Úloha se automaticky odstraní po 90 dnech ve stavu dokončeno. |
| uzavřený | Po všechny jednotky byly dodány zpět na zákazníka, pokud zde nejsou žádné chyby během zpracování úlohy, bude úloha nastavit na zavřeném stavu. Úlohy budou automaticky odstraněny po 90 dnech v uzavřeném stavu. |

Následující tabulka popisuje životní cyklus jednotlivé jednotky jako přechází prostřednictvím úlohu import nebo export. Aktuální stav každé jednotky, v rámci úlohy je nyní viditelné z portálu Azure.
Následující tabulka popisuje všechny stavy, které může předávat každé jednotky, v rámci úlohy.

| Stav disku | Popis |
|:--- |:--- |
| Zadaný | Pro úlohu importu při vytvoření úlohy z portálu Azure počáteční stav pro jednotku je zadaná stavu. Pro úlohy exportu vzhledem k tomu, že není zadána žádná jednotka při vytvoření úlohy, stav počáteční jednotky je stav přijaté. |
| Přijaté | Jednotka přechody stavu přijaté při importu a exportu služby operátor má zpracování jednotek, které byly přijaty z společnosti přesouvání úlohy importu. Stav počáteční jednotky pro úlohy exportu, je stav přijaté. |
| NeverReceived | Jednotka se přesune do stavu NeverReceived při přijetí balíčku pro úlohu, ale balíček neobsahuje jednotku. Jednotku také můžete přesunout do tohoto stavu, pokud to bylo dva týdny, protože služba přijala přesouvání informace, ale balíček nebyl ještě přijaty v datovém centru. |
| Přenos | Na jednotku se přesune do stavu přenos zahájení službu k přenosu dat z jednotky do služby Windows Azure Storage. |
| byla dokončena | Jednotku přesune do stav dokončeno, když služba má úspěšně přenesla všechna data bez chyb.
| CompletedMoreInfo | Jednotku přesune do stavu CompletedMoreInfo, když služba zjistila některé problémy při kopírování dat z nebo na jednotku. Informace může obsahovat chyby, upozornění a informativní zprávy o přepsání objektů BLOB.
| ShippedBack | Jednotka přesune do stavu ShippedBack má byla zakoupení z center zálohování dat na návratovou adresu. |

Tuto bitovou kopii z portálu Azure zobrazuje stav disku úlohu příklad:

![Zobrazení stavu jednotky](./media/storage-import-export-service/drivestate.png)

Následující tabulka popisuje stavy selhání jednotky a akcí provedených pro každý stav.

| Stav disku | Událost | Řešení / další krok |
|:--- |:--- |:--- |
| NeverReceived | Jednotka, která je označena jako NeverReceived (protože nebyla přijata jako součást úlohy dodávky) dorazí v jiné dodávky. | Provozní tým přesune do stavu přijaté jednotku. |
| Není k dispozici | Jednotku, která není součástí všechny úlohy dorazí v datovém centru jako součást jiná úloha. | Jednotka budou označeny jako další jednotky a obnoví se zákazník při dokončení úlohy spojené s původní balíčku. |

### <a name="time-to-process-job"></a>Čas do procesu úlohy
Množství času úlohu importu a exportu se liší v závislosti na různých faktorech, například přesouvání čas zpracování úlohy typu, typ a velikost dat kopírovány a velikosti disků zadat. Službu Import/Export nemá SLA, ale po disky jsou přijaty službu snaží dokončení kopírování v 7 až 10 dní. Přesněji sledovat průběh úlohy můžete použít rozhraní REST API. V seznamu úloh operaci, která poskytuje údaje o průběhu kopie není parametr procenta dokončení. Pokud potřebujete odhad k dokončení úlohy importu a exportu kritické čas oslovení do us

### <a name="pricing"></a>Ceny
**Jednotka poplatek za zpracování**

Je poplatek za zpracování jednotky pro každou jednotku zpracovat jako součást import nebo export úlohy. Zobrazit podrobnosti na [Azure Import/Export ceny](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Přesouvání náklady**

Jestliže doplníte jednotky do Azure, platíte náklady na přesouvání poskytovatel přesouvání. Když se Microsoft vrátí jednotky pro vás, náklady na přesouvání je zodpovědné za poskytovatel účet, který jste zadali při vytvoření úlohy.

**Cena za transakci**

Neexistují žádné cena za transakci, při importu dat do úložiště objektů blob. Standardní výstupní poplatky se dají použít při exportu dat z úložiště objektů blob. Další informace o cena za transakci, v [přenos dat se ceny.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Rychlý start
V této části jsme poskytují podrobné pokyny pro vytvoření importu a úlohy exportu. Zkontrolujte, zda splňujete všechny [předpoklady](#pre-requisites) než budete pokračovat dál.

> [!IMPORTANT]
> Služba podporuje jeden účet úložiště standard storage na import nebo export úlohy a nepodporuje prémiové účty úložiště. 
> 
> 
## <a name="create-an-import-job"></a>Vytvoření úlohy importu
Vytvořte úlohu importu ke zkopírování dat do účtu úložiště Azure z pevných disků odesíláním jeden nebo více jednotek obsahujících data do zadaného datového centra. Úloha importu přenese tak podrobnosti o jednotky pevného disku, data k zkopírují cíle účtu úložiště a informace o přesouvání ke službě Azure Import/Export. Vytvoření úlohy importu je proces třech krocích. Nejprve připravte jednotky pomocí nástroje WAImportExport. Druhý odeslání úlohy importu pomocí portálu Azure. Potom se dodávají jednotky na adresu přenosů zadané během vytváření úlohy a aktualizovat informace o přesouvání ve vašem podrobnosti úlohy.   

### <a name="prepare-your-drives"></a>Příprava jednotky
Prvním krokem při importu dat pomocí služby Azure Import/Export je příprava jednotky pomocí nástroje WAImportExport. Postupujte podle následujících kroků a příprava jednotky.

1. Identifikaci dat určených k importu. To může být adresářů a samostatné soubory na místním serveru nebo sdílené síťové složce.  
2. Určete počet jednotek, které budete potřebovat v závislosti na celkovou velikost data. Pořídit požadovaný počet 2,5 SSD nebo 2,5" nebo 3.5" SATA II nebo III jednotky pevného disku.
3. Identifikujte cílový účet úložiště, kontejner, virtuálních adresářů a objekty BLOB.
4.  Určí, adresáře nebo samostatné soubory, které se zkopírují na každý pevný disk.
5.  Vytvoření souborů CSV pro datovou sadu a driveset.
    
    **Soubor CSV datové sady**
    
    Dole je příklad příklad souboru CSV datové sady:
    
    ```
    BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
    "F:\50M_original\100M_1.csv.txt","containername/100M_1.csv.txt",BlockBlob,rename,"None",None
    "F:\50M_original\","containername/",BlockBlob,rename,"None",None 
    ```
   
    V předchozím příkladu 100M_1.csv.txt se zkopírují do kořenového adresáře kontejner s názvem "containername". Pokud název kontejneru "containername" neexistuje, bude vytvořen. Všechny soubory a složky v části 50M_original bude zkopírován do containername rekurzivně. Struktura složek budou zachována.

    Další informace o [soubor CSV datovou sadu se připravuje](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    
    **Mějte na paměti,**: ve výchozím nastavení, bude importována data jako objekty BLOB bloku. Hodnota pole BlobType můžete použít k importu dat jako objekty BLOB stránky. Například pokud importujete soubory virtuálního pevného disku, které se připojí jako disky na virtuální počítač Azure, je nutné je importovat jako objekty BLOB stránky.

    **Soubor Driveset CSV**

    Hodnota příznaku driveset je soubor CSV, který obsahuje seznam disků, na které jsou namapované písmena jednotek, aby nástroj, který správně vyberte seznam disků, abyste byli připraveni. 

    Dole je příklad driveset souboru CSV:
    
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    H,Format,SilentMode,Encrypt,
    ```

    Ve výše uvedeném příkladu se předpokládá, že jsou připojené dva disky a základní svazků systému souborů NTFS s G:\ písmeno svazku a H:\ byly vytvořeny. Nástroj formátu a šifrování disku, který je hostitelem H:\ a nebude formátu nebo šifrování disku, který je hostitelem svazku G:\.

    Další informace o [Příprava souboru CSV driveset](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file).

6.  Použití [WAImportExport nástroj](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) ke kopírování dat na jeden nebo více pevných disků.
7.  Můžete zadat "Šifrovat" na pole šifrování v drivset CSV povolíte šifrování nástrojem BitLocker na jednotce pevného disku. Alternativně můžete také povolíte šifrování nástrojem BitLocker na jednotce pevného disku ručně a zadejte "AlreadyEncrypted" a zadejte klíče v driveset sdíleného svazku clusteru při spuštění nástroje.

8. Neprovádějte žádné změny dat na jednotky pevného disku nebo souboru deníku po dokončení Příprava disku.

> [!IMPORTANT]
> Každý jednotku pevného disku, které připravíte způsobí souboru deníku. Při vytváření úlohy importu pomocí portálu Azure, musíte nahrát všechny soubory deníku jednotek, které jsou součástí této úlohy importu. Disky bez deníku, které soubory nebudou zpracována.
> 
>

Níže jsou příklady pro přípravu na pevný disk pomocí WAImportExport nástroje a příkazy.

Příkaz PrepImport WAImportExport nástroj pro první relaci kopírování ke kopírování adresářů a souborů s novou relací kopie:

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
```

**Import Příklad 1**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Za účelem **přidat další jednotky**, jeden vytvořit nový soubor driveset a spusťte příkaz jak je uvedeno níže. Pro následné kopírování relace jiné diskových jednotek než je zadáno v souboru .csv InitialDriveset zadejte nový soubor CSV driveset a zadejte jako hodnotu parametru "AdditionalDriveSet". Použití **stejný soubor deníku** název a zadejte **nové ID relace**. Formát souboru AdditionalDriveset CSV je stejný jako formát InitialDriveSet.

```
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
```

**Import příklad 2**
```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
```

Chcete-li přidat další data do stejné driveset, WAImportExport nástroj příkaz PrepImport lze volat pro následné kopírování relací pro kopírování další soubory nebo adresáře: pro následné kopírování relace na stejné jednotky pevného disku zadaná v souboru .csv InitialDriveset, zadejte **stejný soubor deníku** název a zadejte **nové ID relace**; je potřeba zadat klíč účtu úložiště.

```
WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
```

**Import příklad 3**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Zobrazte další podrobnosti o použití nástroje WAImportExport v [Příprava pevné disky pro import](storage-import-export-tool-preparing-hard-drives-import.md).

Navíc vztahují [ukázkový pracovní postup přípravy pevné disky na úlohu importu](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md) podrobnější pokyny krok za krokem.  

### <a name="create-the-import-job"></a>Vytvoření úlohy importu
1. Jakmile připravíte jednotce, přejděte na svůj účet úložiště na portálu Azure a zobrazení řídicího panelu. V části **rychlý přehled**, klikněte na tlačítko **vytvořit úlohu importu**. Projděte si kroky a zaškrtněte políčko znamená, že je vše připraveno jednotce a, abyste měli deníku souboru disku, které jsou k dispozici.
2. V kroku 1 zadejte kontaktní informace pro osoba odpovědná za tato úloha importu a platný zpáteční adresu. Pokud chcete uložit data podrobného protokolování úlohy importu, zaškrtněte možnost pro **uložit podrobného protokolování v kontejneru objektů blob Moje 'waimportexport'**.
3. V kroku 2 odešlete soubory deníku jednotky, které jste získali během přípravy kroku jednotky. Musíte nahrát jeden soubor pro každou jednotku, který jste připravili.
   
   ![Vytvoření úlohy importu – krok 3](./media/storage-import-export-service/import-job-03.png)
4. V kroku 3 zadejte popisný název úlohy importu. Všimněte si, že, které zadáte název může obsahovat jenom malá písmena, číslice, pomlčky a podtržítka, musí začínat písmenem a nesmí obsahovat mezery. Použijete název, který jste se rozhodli sledovat vaše úlohy v době, kdy jsou v průběhu a po jejich dokončení.
   
   V dalším kroku vyberte oblast center dat ze seznamu. Oblasti dat center označí datového centra a adresu, na kterou je nutné dodat vašeho balíčku. Najdete v části Nejčastější dotazy níže Další informace.
5. V kroku 4 vyberte vaše návratový poskytovatel ze seznamu a zadejte číslo svého operátora účtu. Microsoft použije tento účet pro odeslání jednotky vám po dokončení importu úlohu.
   
   Pokud máte vaše číslo sledování, svého operátora doručení vyberte ze seznamu a zadejte číslo sledování.
   
   Pokud nemáte sledování Číslo ještě zvolte **po poslali Moje balíček informace o tomto přesouvání pro tuto úlohu importu zajistí**, dokončete proces importu.
6. Chcete-li po byly součástí vašeho balíčku, zadejte číslo sledování, vraťte se k **importu a exportu** stránky pro váš účet úložiště na portálu Azure, vyberte úlohu v seznamu a zvolte **informace o přesouvání**. Procházet průvodce a zadejte číslo sledování v kroku 2.
   
    Pokud toto číslo není aktualizován v rámci 2 týdny vytvoření úlohy, tato úloha vyprší.
   
    Pokud úloha je ve stavu vytvoření, přesouvání nebo přenos, je také aktualizovat číslo svého operátora účtu v kroku 2 průvodce. Jakmile úloha je ve stavu balení, nelze aktualizovat číslo svého operátora účtu pro dané úlohy.
7. Průběh úlohy můžete sledovat na řídicím panelu portálu. V tématu každý stav úlohy v předchozí části znamená podle [zobrazení stavu vaše úlohy](#viewing-your-job-status).

## <a name="create-an-export-job"></a>Vytvoření úlohy exportu
Vytvoření úlohy exportu službu Import/Export oznámit, že je budete distribuovat jedné nebo několika prázdný jednotek do datového centra, aby data můžete exportovat z vašeho účtu úložiště do jednotky a jednotky a dodání.

### <a name="prepare-your-drives"></a>Příprava jednotky
Příprava jednotky pro úlohy exportu doporučujeme následující předběžné kontroly:

1. Zkontrolujte počet disků požadované pomocí nástroje WAImportExport PreviewExport příkazu. Další informace najdete v tématu [náhled jednotka využití pro úlohu exportovat](https://msdn.microsoft.com/library/azure/dn722414.aspx). Umožňuje zobrazit náhled využití disku pro objekty BLOB, které jste vybrali, na základě velikosti jednotky, které chcete použít.
2. Zkontrolujte, jestli je můžete pro čtení a zápis na pevný disk, který má být dodán pro úlohu export.

### <a name="create-the-export-job"></a>Vytvoření úlohy exportu
1. Pokud chcete vytvořit úlohy exportu, přejděte na svůj účet úložiště na portálu Azure a zobrazení řídicího panelu. V části **rychlý přehled**, klikněte na tlačítko **vytvořit úlohu exportovat** a pokračujte podle pokynů průvodce.
2. V kroku 2 zadejte kontaktní informace pro osoba odpovědná za tuto úlohu exportu. Pokud chcete uložit data podrobného protokolování pro úlohu export, zaškrtněte možnost pro **uložit podrobného protokolování v kontejneru objektů blob Moje 'waimportexport'**.
3. V kroku 3 určete, jaká data objektů blob, které chcete exportovat z vašeho účtu úložiště do prázdné jednotku nebo jednotky. Můžete exportovat všechny data objektů blob v účtu úložiště, nebo můžete určit, které objekty BLOB nebo nastaví objektů blob pro export.
   
   Pokud chcete zadat objekt blob pro export, použijte **rovno** selektor a zadejte relativní cestu do objektu blob, počínaje název kontejneru. Použití *$root* k určení Kořenový kontejner.
   
   K určení všech objektů BLOB od s předponou, použijte **začíná** selektor a zadejte předponu, počínaje lomítkem '/'. Předpona, která může být předponu název kontejneru, název dokončení kontejneru nebo název dokončení kontejneru, za nímž následuje předponu názvu objektu blob.
   
   Následující tabulka uvádí příklady cesty platný objekt blob:
   
   | Selektor | Cesta k objektu BLOB | Popis |
   | --- | --- | --- |
   | Začíná |/ |Exportuje všech objektů BLOB v účtu úložiště |
   | Začíná |/$root / |Exportuje všech objektů BLOB v kontejneru kořenové |
   | Začíná |/Book |Exportuje všech objektů BLOB v kontejneru, který začíná předponu **adresáře** |
   | Začíná |/Music/ |Exportuje všech objektů BLOB v kontejneru **Hudba** |
   | Začíná |/ Hudba/láska |Exportuje všech objektů BLOB v kontejneru **Hudba** které začínají předponou **rádi** |
   | Rovno |$root/logo.bmp |Export objektu blob **logo.bmp** v kořenovém kontejneru |
   | Rovno |videos/Story.MP4 |Export objektu blob **story.mp4** v kontejneru **videa** |
   
   Cesty objektů blob v platné formáty, aby nedocházelo k chybám při zpracování, je nutné zadat, jak je vidět na tomto snímku obrazovky.
   
   ![Vytvoření úlohy exportu – krok 3](./media/storage-import-export-service/export-job-03.png)
4. V kroku 4 zadejte popisný název pro úlohu export. Název, který zadáte, může obsahovat jenom malá písmena, číslice, pomlčky a podtržítka, musí začínat písmenem a nesmí obsahovat mezery.
   
   Oblasti dat center označí datovém centru, ke které je nutné dodat vašeho balíčku. Najdete v části Nejčastější dotazy níže Další informace.
5. V kroku 5 vyberte vaše návratový poskytovatel ze seznamu a zadejte číslo svého operátora účtu. Microsoft použije tento účet pro odeslání jednotky vám po dokončení exportu úlohu.
   
   Pokud máte vaše číslo sledování, svého operátora doručení vyberte ze seznamu a zadejte číslo sledování.
   
   Pokud nemáte sledování Číslo ještě zvolte **po poslali Moje balíček informace o tomto přesouvání pro tuto úlohu export zajistí**, dokončete proces exportu.
6. Chcete-li po byly součástí vašeho balíčku, zadejte číslo sledování, vraťte se k **importu a exportu** stránky pro váš účet úložiště na portálu Azure, vyberte úlohu v seznamu a zvolte **informace o přesouvání**. Procházet průvodce a zadejte číslo sledování v kroku 2.
   
    Pokud toto číslo není aktualizován v rámci 2 týdny vytvoření úlohy, tato úloha vyprší.
   
    Pokud úloha je ve stavu vytvoření, přesouvání nebo přenos, je také aktualizovat číslo svého operátora účtu v kroku 2 průvodce. Jakmile úloha je ve stavu balení, nelze aktualizovat číslo svého operátora účtu pro dané úlohy.
   
   > [!NOTE]
   > Pokud objekt blob export se používá v době kopírování na pevném disku, bude služba Azure Import/Export pořízení snímku objektu blob a zkopírujte snímku.
   > 
   > 
7. Průběh úlohy můžete sledovat na řídicím panelu portálu Azure. Najdete v každém stavu úlohy znamená v předchozí části na "zobrazení stavu vaše úlohy".
8. Jakmile se zobrazí na discích s exportovaná data, můžete zobrazit a zkopírujte tyto klíče nástroje BitLocker vygenerované službou pro jednotce. Přejděte na svůj účet úložiště na portálu Azure a klikněte na kartu importu a exportu. Vyberte úlohu export ze seznamu a klikněte na tlačítko Zobrazit klíče. Klíče Bitlockeru vypadat takto:
   
   ![Zobrazit klíče nástroje BitLocker pro úlohu export](./media/storage-import-export-service/export-job-bitlocker-keys.png)

Přejděte prostřednictvím níže v části Nejčastější dotazy, jak vysvětluje nejběžnější otázky, které zákazníci setkávají při používání této služby.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Můžete zkopírovat pomocí služby Azure Import/Export úložiště Azure File?**

Ne, služba Azure Import/Export podporuje pouze objekty BLOB bloku a objekty BLOB stránky. Všechny ostatní typy úložiště včetně úložiště Azure File, úložiště Table a Queue Storage nejsou podporovány.

**Je dostupná pro předplatná CSP služba Azure Import/Export?**

Služba Azure Import/Export podporuje odběry CSP.

**Můžete přeskočit krok přípravy jednotky pro úlohy importu nebo můžete připravit na jednotku bez kopírování?**

Všechny jednotky, který chcete použít pro odeslání pro import dat musí být připraveny pomocí nástroje Azure WAImportExport. Musíte použít nástroj WAImportExport ke zkopírování dat na disk.

**Je potřeba provést jakékoli přípravu disku při vytváření úlohy exportu?**

Doporučuje se žádné, ale některé předběžné kontroly. Zkontrolujte počet disků požadované pomocí nástroje WAImportExport PreviewExport příkazu. Další informace najdete v tématu [náhled jednotka využití pro úlohu exportovat](https://msdn.microsoft.com/library/azure/dn722414.aspx). Umožňuje zobrazit náhled využití disku pro objekty BLOB, které jste vybrali, na základě velikosti jednotky, které chcete použít. Také zkontrolujte, zda lze číst z a zapisovat na pevný disk, který má být dodán pro úlohu export.

**Co se stane, když I omylem poslat pevný disk, který není v souladu s požadavky na podporované?**

Datového centra Azure vrátí na jednotku, která není v souladu s požadavky na podporované pro vás. Pokud jenom některé jednotky v balíčku splnit požadavky na podporu, tyto jednotky se zpracují a jednotky, které nesplňují požadavky na bude vrácen vám.

**Můžete zrušit má úloha?**

Úlohy můžete zrušit, pokud je jeho stav vytváření nebo přesouvání.

**Jak dlouho na portálu Azure můžete zobrazit stav dokončené úlohy?**

Můžete zobrazit stav pro dokončené úlohy po dobu 90 dnů. Dokončené úlohy budou odstraněny po 90 dnech.

**Pokud chcete importovat nebo exportovat víc než 10 jednotky, co mám dělat?**

Jeden import nebo export úloha může odkazovat jenom 10 jednotky v rámci jedné úlohy pro službu Import/Export. Pokud chcete pro odeslání více než 10 disků, můžete vytvořit více úloh. Jednotky, které jsou přidruženy k této úloze stejné musí dodávané společně ve stejném balíčku.
Společnost Microsoft nabízí pokyny a pomoc při kapacity dat zahrnuje více disku úlohy importu. Obraťte se na bulkimport@microsoft.com Další informace

**Službu formátu jednotky před vrácením je?**

Ne. Všechny jednotky jsou šifrované pomocí Bitlockeru.

**Můžete zakoupit jednotky pro úlohy importu a exportu od společnosti Microsoft?**

Ne. Musíte dodávat vlastní jednotky pro obě import a export úloh.

** Jak může přistupovat k datům, importovaných pomocí této služby **

Data v rámci účtu úložiště Azure se dají zpřístupnit přes portál Azure nebo pomocí samostatný nástroj nazývá Průzkumník úložišť. https://docs.microsoft.com/en-us/Azure/vs-Azure-Tools-Storage-Manage-with-Storage-Explorer 

**Po dokončení úlohy importu, co bude Moje data vypadat v účtu storage? Moje hierarchie adresářů se zachovají?**

Při přípravě na pevný disk pro úlohy importu, je zadána cílového pole DstBlobPathOrPrefix v datové sadě sdíleného svazku clusteru. Toto je cílový kontejner v účtu úložiště, ke kterému se zkopíruje data z pevného disku. V tomto kontejneru cílové virtuální adresáře jsou vytvořeny pro složky z pevného disku a objekty BLOB jsou vytvořené pro soubory. 

**Pokud má jednotka souborů, které již existují v svůj účet úložiště, bude služba přepsat existující objekty BLOB v svůj účet úložiště?**

Při přípravě na jednotku, můžete určit, zda by dojít k přepsání souborů cílové nebo ignorováno pomocí pole v souboru CSV datovou sadu názvem dispozice: < přejmenovat | přepsat ne | přepsat >. Ve výchozím nastavení služba bude přejmenovat nové soubory a nikoli přepsat existující objekty BLOB.

**Je nástroj WAImportExport kompatibilní s 32bitové operační systémy?**
Ne. Nástroj WAImportExport je jenom kompatibilní s operačními systémy Windows 64-bit. Naleznete v části operační systémy v [předpoklady](#pre-requisites) pro úplný seznam podporovaných verzí operačního systému.

**By měla obsahovat jakoukoli jinou hodnotu než jednotku pevného disku v mé balíčku?**

Prosím dodávat pouze pevné disky. Nezahrnujte věci, jako je napájecích kabelů napájení nebo kabely USB.

**Je nutné dodávat Moje jednotky pomocí FedEx nebo DHL?**

Můžete zaslat jednotky do datového centra pomocí žádné známé poskytovatel jako FedEx DHL, UPS nebo nám poštovní služby. Pro přesouvání jednotky vám v datovém centru, je však nutné zadat číslo účtu FedEx v USA a EU nebo DHL číslo účtu v Asii a Austrálie oblastech.

**Existují nějaká omezení s přesouvání mezinárodní úrovni svou jednotku?**

Upozorňujeme, že fyzická média, která jsou přesouvání muset křížové mezinárodní hranice. Jste zodpovědní za zajištění, vaše fyzická média a data jsou importovat a exportovat v souladu s platné zákony. Před jejich odesláním fyzických médií, zkontrolujte s vaší poradci ověřit, jestli se médiu a dat můžete souladu s právem odeslaná do identifikovaného datového centra. To vám pomůže zajistit, aby obdržel Microsoft včas.

**Při vytváření úlohy, adresy příjemce je umístění, které se liší od umístění účtu úložiště. Co bych měl/a dělat?**

Některé umístění účtu úložiště jsou namapované na alternativní přesouvání umístění. Dříve dostupná přenosů umístění můžete také dočasně mapovat do alternativního umístění. Vždy zkontrolujte přenosů adresa zadaná při vytváření úlohy před přesouvání jednotky.

**Při přesouvání svou jednotku, poskytovatel požádá o data center adresa a telefonní číslo kontaktu. Co by měl poskytovat?**

Řadič domény a telefonní číslo adresy se poskytuje jako součást úlohy vytvoření.

**Můžete použít službu Azure Import/Export pro kopírování PST poštovní schránky a dat služby SharePoint k O365?**

Naleznete [Import PST souborů nebo dat služby SharePoint do služeb Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Můžete použít službu Azure Import/Export pro kopírování Moje zálohování offline ke službě Azure Backup?**

Naleznete [pracovní postup Offline zálohování v Azure Backup](../backup/backup-azure-backup-import-export.md).

**Jaký je maximální počet HDD pro v jedné dodávky?**

Může být libovolný počet pevných disků v jedné dodávky a pokud disky patří do více úloh se doporučuje) disky s názvem bez přípony s odpovídající název úlohy. b) aktualizovat úlohy s číslem sledování na konci -1,-2 atd.
  
**Jaký je maximální objekt Blob bloku a velikost objektu Blob stránky podporovány disku importu a exportu?**

Objekt Blob bloku maximální velikost je přibližně 4.768TB nebo 5 000 000 MB.
Objekt Blob stránky maximální velikost je 1TB.

**Podporuje disku importu a exportu šifrování AES 256?**

Služba Azure Import/Export ve výchozím nastavení zašifruje pomocí nástroje bitlocker šifrování AES 128, ale to je možné zvýšit na AES 256 ručně šifrování nástrojem bitlocker před data budou zkopírována. 

Pokud používáte [WAImportExpot V1](http://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), zde je ukázka příkazu
```
WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
```
Pokud používáte [WAImportExport nástroj](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) zadejte "AlreadyEncrypted" a zadejte klíče v driveset sdíleného svazku clusteru.
```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
```
## <a name="next-steps"></a>Další kroky

* [Nastavení nástroje WAImportExport](storage-import-export-tool-how-to.md)
* [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
* [Ukázka Azure Import, Export REST API](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

