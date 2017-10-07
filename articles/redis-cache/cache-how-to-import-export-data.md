---
title: "aaaImport a exportu dat ve službě Azure Redis Cache | Microsoft Docs"
description: "Zjistěte, jak tooimport a export tooand dat z úložiště objektů blob s vaší instancí Azure Redis Cache premium"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: sdanie
ms.openlocfilehash: f17464b207f1c652952f4da63ca147473fee2759
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="import-and-export-data-in-azure-redis-cache"></a>Import a Export dat ve službě Azure Redis Cache
Import a Export je Azure Redis Cache operace správy dat, která vám umožní tooimport data do Azure Redis Cache nebo exportu dat z Azure Redis Cache pomocí import a Export snímku databáze Redis Cache (RDB) z mezipaměti premium tooa objektu blob v Azure Účet úložiště. 

- **Export** -exportujete vaší Azure Redis Cache RDB snímky tooa objekt Blob stránky.
- **Import** -vaší RDB mezipaměti Redis snímky můžete importovat z objekt Blob stránky nebo objekt Blob bloku.

Import a Export vám umožní toomigrate mezi různými instancemi Azure Redis Cache nebo naplňte hello mezipaměť s daty před použitím.

Tento článek obsahuje Průvodce pro import a export dat pomocí Azure Redis Cache a poskytuje hello odpovědi toocommonly dotazy.

> [!IMPORTANT]
> Import a Export je ve verzi preview a je dostupná jenom pro [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti.
>
>

## <a name="import"></a>Import
Import lze použít toobring Redis kompatibilní RDB soubory z jakéhokoli Redis serveru se systémem v libovolném cloudu nebo prostředí, včetně Redis systémem Linux, Windows nebo kteréhokoli poskytovatele cloudových služeb jako Amazon Web Services a dalších. Import dat se snadný způsob toocreate mezipaměti s předem vyplněná daty. Během procesu importu hello Azure Redis Cache načte hello RDB soubory z úložiště Azure do paměti a poté vloží hello klíčů do mezipaměti hello.

> [!NOTE]
> Před zahájením operace importu hello, zkontrolujte, že Redis databáze (RDB) soubor nebo soubory jsou odeslány do stránky, nebo blok objektů BLOB v úložišti Azure, v hello stejné oblasti a předplatné jako instance služby Azure Redis Cache. Další informace najdete v tématu [Začínáme s Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Pokud jste exportovali souboru RDB pomocí hello [Azure Redis Cache exportovat](#export) funkce RDB soubor je již uložen do objektu BLOB stránky a je připravený pro import.
>
>

1. tooimport jeden nebo více exportovat mezipaměti objektů BLOB, [procházet mezipaměti tooyour](cache-configure.md#configure-redis-cache-settings) v hello portál Azure a klikněte na tlačítko **importovat data** z hello **prostředků nabídky**.

    ![Import dat][cache-import-data]
2. Klikněte na tlačítko **zvolte Blob(s)** a vyberte účet úložiště hello, který obsahuje hello data tooimport.

    ![Zvolte účet úložiště][cache-import-choose-storage-account]
3. Klikněte na tlačítko hello kontejner, který obsahuje hello data tooimport.

    ![Vyberte kontejner][cache-import-choose-container]
4. Vyberte jeden nebo více objektů BLOB tooimport kliknutím hello oblasti toohello nalevo od hello název objektu blob a potom klikněte na **vyberte**.

    ![Vyberte objekty BLOB][cache-import-choose-blobs]
5. Klikněte na tlačítko **importovat** toobegin hello importu.

   > [!IMPORTANT]
   > mezipaměť Hello není během procesu importu hello přístup klienti mezipaměti, a všechna existující data v mezipaměti hello se odstraní.
   >
   >

    ![Import][cache-import-blobs]

    Můžete sledovat průběh operace importu hello hello následující hello oznámení z hello portál Azure nebo zobrazením hello události v hello [protokol auditování](../azure-resource-manager/resource-group-audit.md).

    ![Probíhá import][cache-import-data-import-complete]

## <a name="export"></a>Export
Export umožňuje tooexport hello data uložená v Azure Redis Cache tooRedis kompatibilní RDB soubory. Můžete použít tuto funkci toomove data tooanother instanci Azure Redis Cache či tooanother serveru Redis. Během procesu exportu hello dočasný soubor vytvořen na hello virtuálních počítačů, hostitelů hello instanci serveru Azure Redis Cache a soubor hello je nahrané toohello určený účet úložiště. Po dokončení operace exportu hello se stavem úspěch nebo selhání hello dočasný soubor bude odstraněn.

1. tooexport hello aktuální obsah hello mezipaměti toostorage, [procházet mezipaměti tooyour](cache-configure.md#configure-redis-cache-settings) v hello portál Azure a klikněte na tlačítko **Export dat** z hello **prostředků nabídky**.

    ![Vyberte kontejner úložiště][cache-export-data-choose-storage-container]
2. Klikněte na tlačítko **vyberte kontejner úložiště** a vyberte účet úložiště hello potřeby. účet úložiště Hello musí být v hello stejném předplatném, oblasti jako vaše mezipaměť.

   > [!IMPORTANT]
   > Export funguje s objekty BLOB stránky, které podporují classic i Resource Manager účty úložiště, ale nepodporuje [účty úložiště Blob](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts) v tuto chvíli.
   >
   >

    ![Účet úložiště][cache-export-data-choose-account]
3. Zvolte hello potřeby kontejner objektů blob a klikněte na tlačítko **vyberte**. toouse nový kontejner, klikněte na tlačítko **přidat kontejner** tooadd první a pak vyberte ze seznamu hello.

    ![Vyberte kontejner úložiště][cache-export-data-container]
4. Zadejte **předponu názvu objektu Blob** a klikněte na tlačítko **exportovat** procesu exportu toostart hello. Předpona názvu objektu blob Hello je použité tooprefix hello názvy soubory generované této operace exportu.

    ![Export][cache-export-data]

    Můžete sledovat průběh operace exportu hello hello následující hello oznámení z hello portál Azure nebo zobrazením hello události v hello [protokol auditování](../azure-resource-manager/resource-group-audit.md).

    ![Exportovat data dokončení][cache-export-data-export-complete]

    Během procesu exportu hello zůstávají dostupné k použití mezipaměti.

## <a name="importexport-faq"></a>Nejčastější dotazy k importu a exportu
Tento oddíl obsahuje nejčastější dotazy o funkci hello importu a exportu.

* [Jaké cenové úrovně můžete použít k importu a exportu?](#what-pricing-tiers-can-use-importexport)
* [Můžete importovat data z jakéhokoli serveru Redis?](#can-i-import-data-from-any-redis-server)
* [Jaké verze RDB můžete importovat?](#what-rdb-versions-can-i-import)
* [Je k dispozici Moje mezipaměti během operace importu a exportu?](#is-my-cache-available-during-an-importexport-operation)
* [Můžete použít s clusteru Redis importu a exportu?](#can-i-use-importexport-with-redis-cluster)
* [Jak funguje importu a exportu s vlastní databáze nastavení?](#how-does-importexport-work-with-a-custom-databases-setting)
* [Jak se liší od trvalosti Redis importu a exportu?](#how-is-importexport-different-from-redis-persistence)
* [Můžete automatizovat Import a Export pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné správy klientů?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [Zobrazila vypršení časového limitu během Moje operace importu a exportu. Co to znamená?](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [Chyba se zobrazí chybové při exportu Moje data tooAzure úložiště objektů Blob. Co se přihodilo?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a>Jaké cenové úrovně můžete použít k importu a exportu?
Import a Export je k dispozici pouze v cenová úroveň premium hello.

### <a name="can-i-import-data-from-any-redis-server"></a>Můžete importovat data z jakéhokoli serveru Redis?
Ano, kromě tooimporting data exportovaná z instance služby Azure Redis Cache, můžete importovat soubory RDB z jakéhokoli Redis serveru se systémem v libovolném cloudu nebo prostředí, například Linux, Windows nebo poskytovatelů cloudových například Amazon Web Services. toodo to nahrávání hello RDB soubor ze serveru Redis hello potřeby do stránky nebo blok objektů blob do účtu úložiště Azure a pak importovat ho do instance služby Azure Redis Cache premium. Můžete například chcete tooexport hello data z mezipaměti produkční a importujte ho do mezipaměti, používá jako součást pracovní prostředí pro testování nebo pro migraci.

> [!IMPORTANT]
> toosuccessfully importovat data exportovaná z Redis servery než Azure Redis Cache, při použití objektů blob stránky, velikost objektu blob stránky hello musí být zarovnána na hranici 512 bajtů. Pro ukázkový kód tooperform potřebné odsazení bajtu najdete v tématu [ukázkové stránky blogu nahrávání](https://github.com/JimRoberts-MS/SamplePageBlobUpload).
> 
> 

### <a name="what-rdb-versions-can-i-import"></a>Jaké verze RDB můžete importovat?

Azure Redis Cache podporuje RDB import až prostřednictvím RDB verze 7.

### <a name="is-my-cache-available-during-an-importexport-operation"></a>Je k dispozici Moje mezipaměti během operace importu a exportu?
* **Export** – mezipamětí zůstanou k dispozici a můžete pokračovat toouse mezipaměti během operace exportu.
* **Import** – mezipaměti k dispozici, když se spustí operace importu a bude k dispozici pro použití při dokončení operace importu hello.

### <a name="can-i-use-importexport-with-redis-cluster"></a>Můžete použít s clusteru Redis importu a exportu?
Ano, a můžete můžete importu a exportu mezi Clusterové mezipaměti a mezipaměti vypojené z clusteru. Od clusteru Redis [jen podporuje databáze 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), není-li importovat všechna data v databázích než 0. Při importu dat Clusterové mezipaměti, hello klíče se předistribuují mezi horizontálních oddílů hello hello clusteru.

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Jak funguje importu a exportu s vlastní databáze nastavení?
Některé cenové úrovně mají různé [databáze omezení](cache-configure.md#databases), takže existují některé aspekty při importu, pokud jste nakonfigurovali vlastní hodnotu hello `databases` nastavení během vytváření mezipaměti.

* Při importu tooa cenová úroveň s nižší `databases` limit než hello vrstvy, ze kterého jste exportovali:
  * Pokud používáte výchozí číslo hello `databases`, což je 16 pro všechny cenové úrovně, dojde ke ztrátě žádná data.
  * Pokud používáte vlastní počet `databases` že patří do hello limity pro hello vrstvy toowhich importujete, všechna data budou zachována.
  * Pokud exportovaná data obsažená data v databázi, která překračuje omezení hello hello nové vrstvy, není importován hello data z těchto vyšší databází.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Jak se liší od trvalosti Redis importu a exportu?
Trvalost Azure Redis Cache vám umožní toopersist data uložená v Redis tooAzure úložiště. Pokud je nakonfigurovaná trvalost, trvá Azure Redis Cache snímek hello mezipaměti Redis v toodisk Redis binární formát na základě konfigurovat četnosti zálohování. Pokud dojde k závažné události, zakazující hello primární a mezipaměti repliky, je obnoven data do mezipaměti hello automaticky pomocí hello poslední snímek. Další informace najdete v tématu [jak tooconfigure trvalosti dat pro mezipaměť Azure Redis Cache Premium](cache-how-to-premium-persistence.md).

Import / Export můžete toobring data do nebo export z Azure Redis Cache. Není konfigurace zálohování a obnovení pomocí trvalosti Redis.

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Můžete automatizovat Import a Export pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiné správy klientů?
Ano, pro prostředí PowerShell pokyny naleznete v části [tooimport mezipaměti Redis](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) a [tooexport mezipaměti Redis](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Zobrazila vypršení časového limitu během Moje operace importu a exportu. Co to znamená?
Pokud zůstanou na hello **importovat data** nebo **exportovat data** okno delší než 15 minut před zahájením operace hello, obdržíte chybu s chybové zprávy podobné toohello následující ukázka:

    hello request tooimport data into cache 'contoso55' failed with status 'error' and error 'One of hello SAS URIs provided could not be used for hello following reason: hello SAS token end time (se) must be at least 1 hour from now and hello start time (st), if given, must be at least 15 minutes in hello past.

tooresolve, zahájení importu hello nebo operace exportu předtím, než dojde k uplynutí 15 minut.

### <a name="i-got-an-error-when-exporting-my-data-tooazure-blob-storage-what-happened"></a>Chyba se zobrazí chybové při exportu Moje data tooAzure úložiště objektů Blob. Co se přihodilo?
Export pracuje pouze s RDB soubory uložené jako objekty BLOB stránky. Jiné typy objektů blob nejsou aktuálně podporovány, včetně účty úložiště blob s horká a studená vrstvami. Další informace najdete v tématu [účty úložiště Blob](../storage/blobs/storage-blob-storage-tiers.md#blob-storage-accounts).

## <a name="next-steps"></a>Další kroky
Zjistěte, jak toouse více premium mezipaměti funkce.

* [Úvod toohello Azure Redis Cache na úrovni Premium](cache-premium-tier-intro.md)    

<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png
