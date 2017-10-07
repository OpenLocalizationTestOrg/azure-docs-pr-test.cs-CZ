---
title: "aaaCopy aktivity vyladění průvodce a výkonu | Microsoft Docs"
description: "Další informace o klíčových faktorů, které ovlivňují výkon hello přesun dat v Azure Data Factory při použití aktivity kopírování."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 4b9a6a4f-8cf5-4e0a-a06f-8133a2b7bc58
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jingwang
ms.openlocfilehash: b0fb5a76c34752d07e8ddfffbb799a05fb5d6be6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-activity-performance-and-tuning-guide"></a>Zkopírujte aktivity výkonu a vyladění Průvodce
Aktivita kopírování Azure Data Factory nabízí prvotřídní dat zabezpečeným, spolehlivým a vysoce výkonné načítání řešení. Ji budete toocopy desítkami terabajtů dat každý den s bohatou různých cloudové a místní úložiště dat. Výkon při načítání dat svěží fast je klíče tooensure, můžete se zaměřit na problém "velkých objemů dat" základní hello: vytváření řešení pro pokročilou analýzu a získávání hlubšímu porozumění z všechno, co data.

Azure poskytuje sadu podnikové úrovni řešení pro úložiště a datového skladu dat a aktivity kopírování nabízí vysoce optimalizovaného data načítání prostředí, které je snadno tooconfigure a nastavení. Jenom jedna kopie aktivity můžete dosáhnout:

* Načítání dat do **Azure SQL Data Warehouse** v **1,2 GB/s**. Návod s případu použití najdete v tématu [načíst 1 TB do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut](data-factory-load-sql-data-warehouse.md).
* Načítání dat do **úložiště objektů Azure Blob** v **1.0 GB/s**
* Načítání dat do **Azure Data Lake Store** v **1.0 GB/s**

Tento článek popisuje:

* [Výkon referenční čísla](#performance-reference) pro podporované zdroj a jímka toohelp úložiště dat máte v plánu projektu;
* Funkce, které může zvýšit hello kopie propustnost v různých scénářích, včetně [jednotky přesun dat v cloudu](#cloud-data-movement-units), [paralelní kopie](#parallel-copy), a [připravený kopie](#staged-copy);
* [Ladění pokyny výkonu](#performance-tuning-steps) na tom, jak zkopírovat tootune hello výkon a hello klíčové faktory, které může mít vliv na výkon.

> [!NOTE]
> Pokud nejste obeznámeni s aktivitou kopírování obecně, přečtěte si téma [přesun dat pomocí aktivity kopírování](data-factory-data-movement-activities.md) před přečtení tohoto článku.
>

## <a name="performance-reference"></a>Referenční dokumentace výkonu

Jako odkaz níže uvedená tabulka ukazuje hello kopie propustnost číslo v MB/s pro zadaný zdrojový a podřízený páry založené na interní testování hello. Pro porovnání, také ukazuje, jak budou různí nastavení [jednotky přesun dat v cloudu](#cloud-data-movement-units) nebo [Brána pro správu dat škálovatelnost](data-factory-data-management-gateway-high-availability-scalability.md) (více uzlů brány) může pomoct na výkon kopírování.

![Matice výkonu](./media/data-factory-copy-activity-performance/CopyPerfRef.png)


**Toonote body:**
* Propustnost se vypočítává pomocí hello následující vzorec: [velikost dat číst ze zdroje] / [spustit doba trvání aktivity kopírování].
* Hello výkonu referenční čísla v tabulce hello se měří pomocí [TPC-H](http://www.tpc.org/tpch/) datové sady v jedna kopie aktivity při spuštění.
* V úložištích dat Azure, jsou v hello hello zdroj a jímka stejné oblasti Azure.
* Pro hybridní kopírování mezi místními a cloudovými úložiště dat, každý uzel brány byla spuštěna na počítači, který byl odděleně od hello místní datové úložiště se pod specifikaci. Při spuštění jediné aktivity v brány, operace kopírování hello spotřebováno pouze malou část hello testovací počítač CPU, paměť či šířku pásma sítě. Další informace z [aspekt Brána pro správu dat](#considerations-for-data-management-gateway).
    <table>
    <tr>
        <td>Procesor</td>
        <td>32 jader 2,20 GHz Intel Xeon E5-2660 v2</td>
    </tr>
    <tr>
        <td>Memory (Paměť)</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Síť</td>
        <td>Internetové rozhraní: 10 GB/s; rozhraní sítě intranet: 40 GB/s</td>
    </tr>
    </table>


> [!TIP]
> Vyšší propustnost můžete dosáhnout využitím jednotky další přesun dat (DMUs) než hello výchozí maximální DMUs, což je 32 pro spuštění aktivity kopírování cloudu do cloudu. Například s 100 DMUs, můžete dosáhnout kopírování dat z objektu Blob Azure do Azure Data Lake Store v **1.0GBps**. V tématu hello [jednotky přesun dat v cloudu](#cloud-data-movement-units) část Podrobnosti o této funkci a hello Podporované scénáře. Obraťte se na [podporu Azure](https://azure.microsoft.com/support/) toorequest další DMUs.

## <a name="parallel-copy"></a>Paralelní kopie
Můžete si přečíst zdroje dat z hello nebo zapisovat data toohello cílové **paralelně v rámci aktivity kopírování spustit**. Tato funkce vylepšuje hello propustnosti operace kopírování a snižuje hello doba trvání toomove data.

Toto nastavení se liší od hello **souběžnosti** vlastnost v definici aktivity hello. Hello **souběžnosti** vlastnost určuje počet hello **spouští souběžné aktivity kopírování** tooprocess data z jiné aktivity windows (1 AM too2 AM, se 2 AM too3, se 3 AM too4, a tak dále). Tato možnost je užitečná při provádění historických zatížení. Funkce paralelní kopie Hello platí tooa **jednotné aktivity při spuštění**.

Podívejme se na vzorový scénář. V následujícím příkladu hello třeba více řezů z posledních hello toobe zpracovat. Objekt pro vytváření dat běží instance aktivity kopírování (aktivita spustit) pro každý řez:

* datový řez Hello z první okno aktivity hello (1 AM too2 mě) == > aktivita běžet 1
* datový řez Hello z okna aktivita druhý hello (2 AM too3 mě) == > aktivita běžet 2
* datový řez Hello z okna aktivita druhý hello (3 AM too4 mě) == > aktivita běžet 3

A tak dále.

V tomto příkladu, když hello **souběžnosti** too2, je nastavena hodnota **aktivita běžet 1** a **aktivita běžet 2** kopírování dat z okna dvě aktivity **současně**  výkonu přesun dat tooimprove. Ale pokud více souborů jsou spojené s aktivity při spuštění 1, služba pro přesun dat hello zkopíruje soubory z hello zdroj toohello cílový jeden soubor současně.

### <a name="cloud-data-movement-units"></a>Jednotky přesun dat cloudu
A **jednotky přesun dat cloudu (DMU)** je míra, která reprezentuje hello výkon (kombinaci procesoru, paměti a přidělení prostředků sítě) v objektu pro vytváření dat na jednu jednotku. DMU se dají používat v operace kopírování cloudu do cloudu, ale není v hybridní kopírování.

Ve výchozím nastavení používá pro vytváření dat jeden cloud DMU tooperform jediné kopie aktivity spustit. toooverride toto výchozí nastavení, zadejte hodnotu pro hello **cloudDataMovementUnits** vlastnost následujícím způsobem. Informace o hello úroveň výkonnější se mohou objevit, když konfigurujete další jednotky pro konkrétní kopie zdroj a jímka najdete v tématu hello [referenční dokumentace výkonu](#performance-reference).

```json
"activities":[  
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "cloudDataMovementUnits": 32
        }
    }
]
```
Hello **povolené hodnoty** pro hello **cloudDataMovementUnits** vlastnost jsou 1 (výchozí), 2, 4, 8, 16, 32. Hello **skutečný počet cloudu DMUs** , operace kopírování hello používá v době běhu je rovna tooor nižší než hello nakonfigurovaná hodnota, v závislosti na vaší vzorek dat.

> [!NOTE]
> Pokud potřebujete další cloudu DMUs pro vyšší propustnost, obraťte se na [podporu Azure](https://azure.microsoft.com/support/). Nastavení 8 a vyšší aktuálně funguje pouze tehdy, když jste **zkopírovat soubory z objektu Blob úložiště, Data Lake Store nebo Amazon S3 nebo cloudem FTP nebo cloudem SFTP tooBlob úložiště nebo Data Lake Store nebo Azure SQL Database**.
>

### <a name="parallelcopies"></a>parallelCopies
Můžete použít hello **parallelCopies** paralelismus hello tooindicate vlastnosti, které chcete toouse aktivity kopírování. Tato vlastnost si můžete představit jako hello maximální počet vláken v rámci aktivitu kopírování, který může číst ze zdroje nebo zápisu úložiště dat podřízený tooyour paralelně.

Pro každou aktivitu kopírování, spuštění objekt pro vytváření dat určuje hello počet paralelní zkopíruje toouse toocopy data z úložiště dat hello zdrojového a cílového úložiště dat toohello. Hello výchozí počet paralelní kopie, které používá závisí na typu hello zdroj a jímka, který používáte.  

| Zdroj a jímka | Výchozí paralelní kopie počet určit službou |
| --- | --- |
| Kopírovat data mezi souborové úložiště (úložiště objektů Blob; Data Lake Store; Amazon S3; systému souborů na místě. místní službě HDFS) |Mezi 1 a 32. Závisí na velikosti hello hello souborů a hello počet jednotek přesun dat cloudu (DMUs) používané toocopy data mezi dvěma úložišti dat cloud nebo hello konfiguraci fyzických hello počítači brány pro hybridní kopírování (tooor data toocopy součásti z místního úložiště dat ). |
| Kopírování dat z **tooAzure Table storage úložiště všechny zdroje dat** |4 |
| Všechny ostatní zdroj a jímka páry |1 |

Obvykle hello výchozí chování měl dát hello nejlepší propustnost. Ale toocontrol hello načíst na počítačích tohoto hostitele vaší tootune kopie výkon nebo úložiště dat, můžete zvolit toooverride hello výchozí hodnotu a zadejte hodnotu pro hello **parallelCopies** vlastnost. Hello hodnota musí být mezi 1 a 32 (obě včetně). V době běhu pro zajištění nejlepšího výkonu hello aktivity kopírování používá hodnotu, která je menší než nebo rovna toohello hodnotu, která nastavíte.

```json
"activities":[  
    {
        "name": "Sample copy activity",
        "description": "",
        "type": "Copy",
        "inputs": [{ "name": "InputDataset" }],
        "outputs": [{ "name": "OutputDataset" }],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 8
        }
    }
]
```
Toonote body:

* Při kopírování dat mezi úložišti na základě souborů hello **parallelCopies** určení stupně paralelního zpracování hello na úrovni souborů hello. Hello rozdělování v rámci jednoho souboru by se stalo pod automaticky a transparentně a je navržena toouse hello velikost na nejlepší vhodný bloku pro zadaná zdrojová úložiště dat typů tooload data v paralelní a ortogonální tooparallelCopies. Hello skutečný počet kopií paralelní hello data přesun služba používá pro operace kopírování hello v době běhu je pouze hello počet souborů, které máte. Pokud je kopie chování hello **mergeFile**, aktivity kopírování nemohou využívat paralelismus úrovni souborů.
* Pokud zadáte hodnotu hello **parallelCopies** vlastnost, zvažte zvýšení zatížení hello na zdroj a jímka úložiště dat a toogateway, pokud je hybridní kopírování. To se stane, zejména pokud máte více aktivit nebo souběžných spustí hello stejné aktivity, které spouštění hello stejné úložiště dat. Pokud si všimnete, že úložiště dat hello nebo brány je zahlcen hello zatížení, snížit hello **parallelCopies** hodnotu toorelieve hello zatížení.
* Při kopírování dat z úložiště, které nejsou toostores na základě souborů, které jsou na základě souborů, služba pro přesun dat hello ignoruje hello **parallelCopies** vlastnost. I když je zadán paralelismus, není použita v tomto případě.

> [!NOTE]
> Je nutné použít hello toouse 1.11 nebo novější verze brány pro správu dat **parallelCopies** funkce při hybridní kopírování.
>
>

toobetter použít tyto dvě vlastnosti a tooenhance vaše propustnost přesun dat najdete v tématu hello [ukázkové případy použití](#case-study-use-parallel-copy). Nepotřebujete tooconfigure **parallelCopies** tootake výhod hello výchozí chování. Pokud nakonfigurujete a **parallelCopies** je příliš malá, více cloudu DMUs nemusí budou plně využívat.  

### <a name="billing-impact"></a>Dopad fakturace
Má **důležité** tooremember, který vám účtovat podle hello celkový čas operace kopírování hello. Pokud úlohu kopírování použít tootake jednu hodinu s jednotkou k jednomu cloudu a teď bude trvat 15 minut s čtyři jednotky cloudu, hello celkové faktury zůstane téměř hello stejné. Například můžete použít čtyři jednotky cloudu. první jednotky cloudu Hello stráví 10 minut, hello druhý, 10 minut, hello třetí jeden, 5 minut a hello čtvrtý jeden, 5 minut, všechny v jedné kopie aktivity při spuštění. Budou se vám účtovat dobu hello celkový kopírování (přesun dat), což je 10 + 10 + 5 + 5 = 30 minut. Pomocí **parallelCopies** nemá vliv na fakturace.

## <a name="staged-copy"></a>Kopírování dvoufázové instalace
Při kopírování dat z úložiště zdroje dat úložiště tooa jímku dat, můžete zvolit úložiště objektů Blob toouse jako dočasné pracovní úložiště. Pracovní je zvlášť užitečné v následujících případech hello:

1. **Chcete-li tooingest data z různých úložišť dat do SQL Data Warehouse pomocí PolyBase**. SQL Data Warehouse PolyBase používá jako mechanismus vysokou propustností tooload velké množství dat do SQL Data Warehouse. Ale hello zdroje dat musí být v úložišti objektů Blob a musí splňovat další kritéria. Při načítání dat z úložiště dat než úložiště objektů Blob, můžete aktivovat data kopírování prostřednictvím dočasné pracovní úložiště objektů Blob. V takovém případě objekt pro vytváření dat provádí tooensure transformace dat hello požadované splňuje požadavky hello PolyBase. Poté použije PolyBase tooload dat do SQL Data Warehouse. Další podrobnosti najdete v tématu [použijte PolyBase tooload data do Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse). Návod s případu použití najdete v tématu [načíst 1 TB do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut](data-factory-load-sql-data-warehouse.md).
2. **Někdy trvá chvíli tooperform přesun dat hybridní (tedy toocopy mezi úložišti dat místní a cloudové úložiště dat), přes pomalé síťové připojení**. tooimprove výkonu lze komprimovat hello data místně tak, aby trvá méně času toomove data toohello pracovní datové úložiště v cloudu hello. Potom můžete dekomprimaci dat hello v hello pracovní úložiště před načtením do cílového úložiště dat hello.
3. **Nechcete, aby porty tooopen jiné než port 80 a portu 443 v bráně firewall kvůli podnikové zásady IT**. Například při kopírování dat z Azure SQL Database jímky místní úložiště dat tooan nebo jímky Azure SQL Data Warehouse, je nutné tooactivate odchozí komunikaci TCP na portu 1433 pro brány firewall systému Windows hello i váš podnikový firewall. V tomto scénáři Využijte výhod hello toofirst kopírování dat tooa objektu Blob úložiště pracovní instance brány pomocí protokolu HTTP nebo HTTPS na portu 443. Potom načtení hello dat do SQL Database nebo SQL Data Warehouse z pracovní databáze úložiště objektů Blob. V tomto toku nepotřebujete tooenable port 1433.

### <a name="how-staged-copy-works"></a>Kopírování funguje jak dvoufázové instalace
Když aktivujete hello pracovní funkce, první hello data budou zkopírována z hello zdroje dat úložiště toohello pracovní úložiště dat (funkce přineste si vlastní). V dalším kroku hello data budou zkopírována z hello pracovní dat úložiště toohello jímku dat úložiště. Objekt pro vytváření dat automaticky spravuje hello dvoufázová toku za vás. Objekt pro vytváření dat také vyčistí dočasná data z hello pracovní úložiště po dokončení přesunu dat hello.

V kopii scénář cloudu hello (zdroj a jímka data, která jsou úložiště v cloudu hello) se nepoužije brány. operace kopírování hello provádí Hello služba Data Factory.

![Dvoufázové instalace kopírování: scénář cloudu](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

Ve scénáři kopie hybridní hello (je místní zdroj a jímka je v cloudu hello), hello gateway se přesune tooa pracovní úložiště dat úložiště dat ze zdrojových dat hello. Služba data Factory přesouvá data z hello dat pracovních úložiště toohello podřízený data. Kopírování dat z tooan úložiště dat cloudu místní úložiště dat prostřednictvím přípravy je podporováno také s tokem hello vrátit zpět.

![Dvoufázové instalace kopírování: hybridní scénář](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Když aktivujete přesun dat s použitím pracovní úložiště, můžete určete, jestli má toobe data hello komprimované před přesun dat ze zdroje hello data ukládat tooan provizorní nebo přípravu úložiště dat a pak dekomprimovat před přesouvání dat od jako dočasné nebo pracovní datové úložiště toohello jímku dat úložiště.

V současné době nelze kopírovat data mezi dvěma místní úložišti dat pomocí pracovní úložiště. Očekáváme, že tato možnost toobe k dispozici brzy k dispozici.

### <a name="configuration"></a>Konfigurace
Konfigurace hello **enableStaging** nastavení v aktivitě kopírování toospecify, jestli se mají toobe data hello dvoufázové instalace v úložišti objektů Blob před načtením do cílového úložiště dat. Když nastavíte **enableStaging** tooTRUE, zadejte další vlastnosti hello uvedené v další tabulce hello. Pokud nemáte, musíte taky toocreate Azure Storage nebo úložiště sdíleného přístupu podpis propojené služby pro přípravu.

| Vlastnost | Popis | Výchozí hodnota | Požaduje se |
| --- | --- | --- | --- |
| **enableStaging** |Zadejte, zda chcete, aby toocopy data prostřednictvím jako dočasné pracovní úložiště. |False |Ne |
| **linkedServiceName** |Zadejte název hello [azurestorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) nebo [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) propojené služby, která odkazuje toohello instanci úložiště, který používáte jako dočasné pracovní úložiště. <br/><br/> Úložiště nelze použít s sdílený přístupový podpis tooload dat do SQL Data Warehouse pomocí PolyBase. Můžete ji v jiných scénářích. |Není k dispozici |Ano, když **enableStaging** nastavena tooTRUE |
| **Cesta** |Zadejte cestu úložiště objektů Blob hello má data toocontain hello dvoufázové instalace. Pokud nezadáte cestu, služba hello vytvoří kontejner dočasná data toostore. <br/><br/> Zadejte cestu, pouze v případě, že používáte úložiště s sdílený přístupový podpis, nebo vyžadujete toobe dočasná data v určitém umístění. |Není k dispozici |Ne |
| **enableCompression** |Určuje, zda by měl předtím, než je cílový zkopírovaný toohello komprimována data. Toto nastavení omezuje hello objem přenášených dat. |False |Ne |

Zde je ukázka definice aktivity kopírování hello vlastnostmi, které jsou popsané v předcházející tabulce hello:

```json
"activities":[  
{
    "name": "Sample copy activity",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDBOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlSink"
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob",
            "path": "stagingcontainer/path",
            "enableCompression": true
        }
    }
}
]
```

### <a name="billing-impact"></a>Dopad fakturace
Budou se vám účtovat na základě dva kroky: kopírování doba trvání a zkopírujte typu.

* Při použití pracovní během kopírování cloudu (kopírování dat z úložiště dat, které cloudové data store tooanother cloudu), vám budou účtovat hello [Celková doba trvání kopie kroky 1 a 2] x [cloudové kopírování jednotkové ceny].
* Při použití pracovní během hybridní kopírování (kopírování dat z úložiště místní data store tooa cloudu data), budeme vám účtovat [kopírování hybridní doba trvání] x [cena za jednotku hybridního kopie] + [cloudu kopie trvání] x [cloudové kopírování jednotkové ceny].

## <a name="performance-tuning-steps"></a>Postup ladění výkonu
Doporučujeme provést tyto kroky tootune hello výkonu služby Data Factory s aktivitou kopírování:

1. **Stanovení základní úrovně**. V průběhu fáze vývoje hello Otestujte svůj kanál pomocí aktivity kopírování proti data reprezentativní ukázka. Můžete použít hello Data Factory [řezů modelu](data-factory-scheduling-and-execution.md) toolimit hello množství dat, můžete pracovat.

   Shromažďovat dobu provádění a výkonové charakteristiky pomocí hello **monitorování a správu aplikací**. Zvolte **monitorování a správa** na domovské stránce objektu pro vytváření dat. V zobrazení stromu hello, zvolte hello **výstupní datovou sadu**. V hello **aktivity Windows** vyberte hello kopie aktivity při spuštění. **Aktivity Windows** uvádí dobu trvání aktivity kopírování hello a hello velikost hello dat, která se zkopírují. Hello propustnost je uvedena v **aktivity okno Průzkumníka**. najdete v části toolearn Další informace o aplikaci hello [monitorování a Správa kanálů služby Azure Data Factory pomocí hello monitorování a správu aplikací](data-factory-monitor-manage-app.md).

   ![Podrobnosti o spuštění aktivit](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

   Dále v článku hello, můžete porovnat výkon hello a konfiguraci váš scénář tooCopy aktivity na [referenční dokumentace výkonu](#performance-reference) z našich testů.
2. **Diagnostika a optimalizace výkonu**. Pokud zjistíte výkonu hello nevyhovuje vašim požadavkům, je nutné kritické body tooidentify. Potom optimalizovat výkon tooremove nebo snižte hello účinku kritická místa. Úplný popis diagnostiku výkonu, je nad rámec hello rámec tohoto článku, ale tady jsou některé společné aspekty:

   * Funkce výkonu:
     * [Paralelní kopie](#parallel-copy)
     * [Jednotky přesun dat cloudu](#cloud-data-movement-units)
     * [Kopírování dvoufázové instalace](#staged-copy)
     * [Škálovatelnost Brána pro správu dat](data-factory-data-management-gateway-high-availability-scalability.md)
   * [Brána správy dat](#considerations-for-data-management-gateway)
   * [Zdroj](#considerations-for-the-source)
   * [Podřízený](#considerations-for-the-sink)
   * [Serializace a deserializace](#considerations-for-serialization-and-deserialization)
   * [Komprese](#considerations-for-compression)
   * [Mapování sloupce](#considerations-for-column-mapping)
   * [Další důležité informace](#other-considerations)
3. **Rozbalte položku konfigurace hello tooyour celé datové sady**. Až budete spokojeni s výsledky spuštění hello a výkonu, můžete rozbalit hello Definice a toocover aktivní období kanálu celé datové sady.

## <a name="considerations-for-data-management-gateway"></a>Důležité informace týkající se Brána pro správu dat
**Instalační program brány**: doporučujeme použít vyhrazený počítač toohost Brána pro správu dat. V tématu [předpoklady pro použití brány pro správu dat](data-factory-data-management-gateway.md#considerations-for-using-gateway).  

**Monitorování brány a nahoru nebo škálování**: jeden logické brány se jeden nebo více uzlů brány může obsluhovat několik aktivity kopírování běží na hello stejný čas současně. Skoro v reálném čase snímek využití prostředků (procesor, paměť, network(in/out) atd.) můžete zobrazit na počítači brány i hello počet souběžných úloh spuštěných versus limit v hello portál Azure, najdete v části [monitorování brány portálu hello](data-factory-data-management-gateway.md#monitor-gateway-in-the-portal). Pokud nemáte velkou požadavky na přesun dat hybridní buď velký počet souběžných kopie aktivity spustí nebo s velkým množstvím dat toocopy, zvažte příliš[vertikální navýšení kapacity nebo škálovat. brána](data-factory-data-management-gateway-high-availability-scalability.md#scale-considerations) tak jako toobetter využívat prostředku nebo tooprovision zkopírujte tooempower více prostředků. 

## <a name="considerations-for-hello-source"></a>Důležité informace týkající se zdrojové hello
### <a name="general"></a>Obecné
Ujistěte se, že tento hello základní úložiště dat není zahlcen jiné úlohy, které jsou spuštěné na nebo před ním.

Pro úložiště dat společnosti Microsoft, najdete v části [sledování a ladění témata](#performance-reference) konkrétní toodata úložiště a pomáhá pochopit ukládání výkonové charakteristiky, minimalizovat dobu odezvy a maximalizovat propustnost dat, které jsou.

Při kopírování dat z objektu Blob úložiště tooSQL datového skladu, zvažte použití **PolyBase** tooboost výkonu. V tématu [použijte PolyBase tooload data do Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) podrobnosti. Návod s případu použití najdete v tématu [načíst 1 TB do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Úložiště dat na základě souborů
*(Včetně úložiště objektů Blob, Data Lake Store, Amazon S3, systémy souborů na místě a místní HDFS)*

* **Průměrná velikost souboru a počet souborů**: aktivita kopírování přenosy dat jeden soubor současně. S hello stejné množství dat toobe přesunout hello celkovou propustnost je nižší, pokud hello dat obsahuje mnoho malých souborů než několik velkých souborů z důvodu toohello fáze zavedení pro každý soubor. Pokud je to možné, kombinovat proto malé soubory do větší soubory toogain vyšší propustnost.
* **Soubor formátu a komprese**: Další způsoby tooimprove výkonu, najdete v části hello [důležité informace týkající se serializace a deserializace](#considerations-for-serialization-and-deserialization) a [důležité informace týkající se komprese](#considerations-for-compression) oddíly.
* Pro hello **místní systém souborů** scénář, ve kterém **Brána pro správu dat** je potřeba, najdete v části hello [důležité informace týkající se Brána pro správu dat](#considerations-for-data-management-gateway) části.

### <a name="relational-data-stores"></a>Relační datové úložiště
*(Zahrnuje SQL Database; SQL Data Warehouse; Amazon Redshift; Databáze systému SQL Server; a Oracle, MySQL, DB2, Teradata, Sybase a PostgreSQL databáze atd.)*

* **Vzorek dat**: schéma tabulky ovlivňuje kopie propustnost. Velikost velké řádku získáte lepší výkon než velikost řádku malé, toocopy hello stejné množství dat. Hello důvodem je, že databáze hello můžete načíst efektivněji méně dávek dat, která obsahují menší počet řádků.
* **Dotazu nebo uložené proceduře**: optimalizace hello logiku hello dotazu nebo uložené proceduře zadáte hello aktivity kopírování zdrojová toofetch data, efektivněji.
* Pro **místní relační databáze**, jako je SQL Server a Oracle, které vyžadují použití hello **Brána pro správu dat**, najdete v části hello [důležité informace týkající se Brána pro správu dat](#considerations-on-data-management-gateway) části.

## <a name="considerations-for-hello-sink"></a>Důležité informace pro hello sink
### <a name="general"></a>Obecné
Ujistěte se, že tento hello základní úložiště dat není zahlcen jiné úlohy, které jsou spuštěné na nebo před ním.

Úložiště dat společnosti Microsoft, najdete v části příliš[sledování a ladění témata](#performance-reference) konkrétní toodata úložiště, které jsou. Tato témata vám může pomoct pochopit ukazatele výkonu úložiště, a jak času odezvy toominimize a maximalizovat propustnost.

Pokud chcete kopírovat data z **úložiště objektů Blob** příliš**SQL Data Warehouse**, zvažte použití **PolyBase** tooboost výkonu. V tématu [použijte PolyBase tooload data do Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) podrobnosti. Návod s případu použití najdete v tématu [načíst 1 TB do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut](data-factory-load-sql-data-warehouse.md).

### <a name="file-based-data-stores"></a>Úložiště dat na základě souborů
*(Včetně úložiště objektů Blob, Data Lake Store, Amazon S3, systémy souborů na místě a místní HDFS)*

* **Zkopírujte chování**: Při kopírování dat z jiné na základě souborů datové úložiště, aktivity kopírování má tři možnosti prostřednictvím hello **copyBehavior** vlastnost. Zachovává hierarchie, vyrovná hierarchie nebo sloučí soubory. Buď se zachováním nebo vyrovnání hierarchie má žádné nebo téměř žádné nároky na výkon, ale sloučení souborů způsobí, že tooincrease režijní výkon.
* **Soubor formátu a komprese**: viz hello [důležité informace týkající se serializace a deserializace](#considerations-for-serialization-and-deserialization) a [důležité informace týkající se komprese](#considerations-for-compression) oddíly pro další způsoby tooimprove výkonu .
* **Úložiště objektů blob**: v současné době podporuje úložiště objektů Blob blokovat jenom objekty BLOB pro přenos dat optimalizované a propustnosti.
* Pro **systémy souborů místní** scénáře, které vyžadují použití hello **Brána pro správu dat**, najdete v části hello [důležité informace týkající se Brána pro správu dat](#considerations-for-data-management-gateway) části.

### <a name="relational-data-stores"></a>Relační datové úložiště
*(Zahrnuje SQL Database, SQL datového skladu, databáze systému SQL Server a Oracle – databáze)*

* **Zkopírujte chování**: v závislosti na vlastnosti hello jste nastavili pro **sqlSink**, aktivity kopírování zapíše data toohello cílové databáze různými způsoby.
  * Ve výchozím nastavení hello hello data přesun služby používá hello hromadné kopírování API tooinsert dat v připojení režimu, který poskytuje nejlepší výkon.
  * Pokud nakonfigurujete uložené procedury Sink hello, platí hello databáze hello dat jeden řádek v době místo jako hromadného načtení. Výkon výrazně zahodí. Pokud sadu dat velká, v případě potřeby, zvažte toousing hello **sqlWriterCleanupScript** vlastnost.
  * Pokud nakonfigurujete hello **sqlWriterCleanupScript** spouštět vlastnosti pro každou aktivitu kopírování, aktivační události služby hello hello skriptu a pak použijete hello hromadné kopírování API tooinsert hello data. Například hello celou tabulku toooverwrite s hello nejnovější data, můžete skript toofirst odstranit všechny záznamy před hromadného načtení hello nová data ze zdroje hello.
* **Velikost dat vzor a batch**:
  * Schéma tabulky ovlivňuje kopie propustnost. toocopy hello stejné množství dat, velikost velké řádku umožňuje lepší výkon než velikost malých řádku, protože databáze hello můžete potvrdit efektivněji méně dávky data.
  * Aktivita kopírování vkládá data v řadě dávek. Můžete nastavit hello počet řádků v dávce pomocí hello **writeBatchSize** vlastnost. Pokud data obsahují malé řádky, můžete nastavit hello **writeBatchSize** vlastnost s vyšší toobenefit hodnotu z nižší režijní náklady na dávky a vyšší propustnost. Pokud velikost řádku hello data velká, dávejte pozor, když zvýšíte **writeBatchSize**. Vysoká hodnota můžou způsobit selhání kopírování tooa způsobené přetížení hello databáze.
* Pro **místní relační databáze** , jako třeba SQL Server a Oracle, které vyžadují použití hello **Brána pro správu dat**, najdete v části hello [důležité informace týkající se Brána pro správu dat](#considerations-for-data-management-gateway) části.

### <a name="nosql-stores"></a>NoSQL úložiště
*(Zahrnuje Table storage a Azure Cosmos DB)*

* Pro **tabulky úložiště**:
  * **Oddíl**: zápis dat toointerleaved oddíly výrazně snižuje výkon. Zdrojová data seřadit klíč oddílu tak, aby hello vloží se data efektivně do jednoho oddílu po druhém, nebo upravte hello logiku toowrite hello data tooa jeden oddíl.
* Pro **Azure Cosmos DB**:
  * **Velikost dávky**: hello **writeBatchSize** vlastnost nastaví hello počet požadavků na paralelní toohello Azure Cosmos DB služby toocreate dokumenty. Lepšího výkonu můžete očekávat, když zvýšíte **writeBatchSize** další paralelní žádosti jsou odesílány tooAzure Cosmos DB. Omezení při psaní tooAzure Cosmos DB však sledovat (hello chybová zpráva je "Požadavků je velká"). Různé faktory může způsobit, že omezení velikosti dokumentu, včetně hello počet podmínky v dokumentech hello a hello zásady indexování cílovou kolekci. vyšší propustnost kopie tooachieve, zvažte použití lepší kolekci, například S3.

## <a name="considerations-for-serialization-and-deserialization"></a>Důležité informace týkající se serializace a deserializace
Serializace a deserializace může dojít, když vaše vstupní datové sady nebo výstupní datové sady není soubor. V tématu [podporované formáty souborů a komprese](data-factory-supported-file-and-compression-formats.md) s informace o podporovaných formátech souborů pomocí aktivity kopírování.

**Zkopírujte chování**:

* Kopírování souborů mezi úložišti dat na základě souborů:
  * Když hello vstupní a výstupní datové sady, které mají stejné nebo žádné nastavení formátu souboru, služba pro přesun dat hello provede binární kopie bez jakýchkoli serializaci nebo deserializaci. Zobrazí vyšší propustnost porovnání toohello scénáři, ve které hello se liší od sebe navzájem zdroj a jímka nastavení formátu souboru.
  * Pokud vstupní a výstupní datové sady obě jsou ve formátu textu a pouze kódování hello typ je jiné, služba pro přesun dat hello pouze nepodporuje kódování převod. Neprovádí žádné serializace a deserializace, což způsobí, že některé výkonu režie porovná tooa binární kopie.
  * Pokud vstupní a výstupní datové sady oba mají jiné formáty souborů nebo různé konfigurace, jako oddělovače, hello služba pro přesun dat deserializuje toostream zdroje dat, transformace a pak ho serializaci do formátu výstup hello, kterou jste uvedli. Výsledkem operace režie mnohem větších výkon ve srovnání tooother scénáře.
* Při kopírování souborů z úložiště dat, který není na základě souborů (například z tooa souborové úložiště relační úložiště) hello serializaci nebo deserializaci krok je povinný. Tento krok vede režijní náklady na významně zvýšit výkon.

**Formát souboru**: formát souboru hello zvolíte může ovlivnit výkon kopírování. Například je Avro kompaktní binární formát, který ukládá metadata s daty. Rozsáhlá podpora má v hello ekosystém Hadoop pro zpracování a dotazování. Je však nákladnější k serializaci a deserializaci, výsledkem nižší propustnost kopie porovnání tootext formát Avro. Zkontrolujte výběr formátu souboru v rámci hello komplexní zpracování toku. Spustit s co formuláře hello data se ukládají do, zdrojové úložiště dat nebo toobe extrahovat z externích systémů; Hello nejlepší formát pro úložiště, analytického zpracování a dotazování; a jaký formát hello data mají být exportovány do datových tržišť nástrojů pro vytváření sestav a vizualizace nástroje. Někdy formát souboru, který je zhoršené pro čtení a zápisu, výkon může být vhodné použít při zvažování hello celkové analytical procesu.

## <a name="considerations-for-compression"></a>Důležité informace pro kompresi
Pokud vstupní nebo výstupní datové sady je soubor, můžete nastavit aktivitu kopírování tooperform kompresi nebo dekompresi jako zapíše data toohello cílový. Když zvolíte komprese, můžete udělat kompromis mezi vstupně výstupní (I/O) a procesoru. Komprese dat hello náklady navíc v výpočetní prostředky. Ale na oplátku omezuje sítě vstupně-výstupních operací a úložiště. V závislosti na vašich dat se může zobrazit nárůst v celkovou propustnost kopírování.

**Kodeků**: aktivita kopírování podporuje typy kompresi Deflate, bzip2 a gzip. Azure HDInsight můžete využívat všechny tři typy zpracování. Každý komprese kodek obsahuje výhody. Například bzip2 má hello nejnižší kopie propustnost, ale můžete získat hello Hive dotazy vracely co nejlepší s bzip2, protože je možné rozdělit pro zpracování. GZIP je hello nejvíce vyrovnáváním možnost a používá se nejčastěji používá hello. Zvolte hello kodeků, který nejlépe vyhovuje danému typu klient server.

**Úroveň**: můžete vybrat z dvě možnosti pro každý komprese kodeků: nejrychlejších komprimované a optimálně komprimované. Hello nejrychlejších komprimované možnost komprimuje hello data co nejrychleji, i když hello výsledný soubor není komprimována optimálně. Hello optimálně komprimované možnost stráví více času na komprese a výsledkem minimální množství dat. Obě možnosti toosee, který poskytuje lepší celkový výkon ve vašem případě můžete otestovat.

**Zabývat**: toocopy velké množství dat mezi místnímu úložišti a cloudu hello, zvažte použití úložiště objektů blob v mezičase s kompresí. Použití dočasné úložiště je užitečné, když hello šířky pásma sítě a služeb Azure je hello omezující faktor, který chcete hello vstupní datové sady a výstupní data nastavit obě toobe nekomprimované formuláře. Jedna kopie aktivity přesněji řečeno, lze rozdělit na dvě kopie aktivity. Hello první kopie aktivity se zkopíruje z hello zdroj tooan provizorní nebo pracovní objekt blob v komprimované formě. Druhá aktivita kopírování Hello zkopíruje hello komprimována data z pracovní databáze a pak dekomprimuje při zapíše toohello podřízený.

## <a name="considerations-for-column-mapping"></a>Důležité informace týkající se mapování sloupců
Můžete nastavit hello **columnMappings** vlastnost v aktivitě kopírování toomap všechny nebo jen některé hello vstupní sloupce toohello výstupní sloupce. Poté, co služba pro přesun dat hello načte hello data ze zdroje hello, musí tooperform mapování sloupců na hello data před zapíše jímku toohello dat hello. Další zpracování snižuje kopie propustnost.

Pokud zdrojového úložiště dat dotazovatelné, například pokud je relační úložiště, jako je SQL Database nebo SQL Server, nebo pokud je úložiště typu NoSQL jako Table storage nebo Azure Cosmos DB, zvažte vkládání hello sloupec filtrování a změna logiku toohello **dotazu**  vlastnost místo použití mapování sloupců. Tímto způsobem hello projekce provádí služba pro přesun dat hello přečte úložiště dat z hello zdroje dat, kde je mnohem efektivnější.

## <a name="other-considerations"></a>Další důležité informace
Pokud hello velikost dat, které chcete toocopy je velká, můžete nastavit obchodní logiky toofurther oddílu hello data pomocí hello řezů mechanismus v datové továrně. Poté naplánujte aktivity kopírování toorun častěji spouštět tooreduce hello velikost dat pro každou aktivitu kopírování.

Být opatrní při hello počet datových sad a kopie aktivity nutnosti Data Factory tooconnector toohello stejné úložiště dat v hello stejný čas. Mnoho souběžných kopírování úlohy může omezení úložiště dat a vést toodegraded výkonu, zkopírujte interní opakování úlohy a v některých případech selhání spuštění.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-tooblob-storage"></a>Vzorový scénář: kopírování z místního úložiště tooBlob systému SQL Server
**Scénář**: kanál vychází toocopy data z místního úložiště tooBlob systému SQL Server ve formátu CSV. toomake hello úlohu kopírování rychleji, soubory CSV hello by měl komprimované do bzip2 formátu.

**Test a analýzy**: hello propustnost aktivity kopírování je menší než 2 MB/s, což je mnohem nižší než srovnávacího testu výkonu hello.

**Analýza výkonu a ladění**: tootroubleshoot hello týkající se výkonu, podíváme se na způsobu zpracování a přesunout hello data.

1. **Čtení dat**: Brána otevře připojení tooSQL serveru a odešle hello dotazu. SQL Server odpoví odesláním hello datový proud tooGateway prostřednictvím intranetu hello.
2. **Serializuje a komprese dat**: brány serializuje hello datový proud tooCSV formátu, a zkomprimuje hello datový proud bzip2 tooa.
3. **Zápis dat**: brány odešle hello bzip2 datového proudu tooBlob úložiště prostřednictvím hello Internetu.

Jak vidíte, hello dat se zpracovat a přesunout způsobem streamování sekvenční: SQL Server > LAN > brány > WAN > úložiště objektů Blob. **Hello celkový výkon jsou závislé na minimální propustnost hello napříč hello kanálu**.

![Tok dat](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Přetížení hello může způsobit jeden nebo více hello následující faktory:

* **Zdroj**: samotný SQL Server má nízkou propustnost kvůli velkým zatížením.
* **Brána pro správu dat**:
  * **LAN**: brány se nachází daleko od hello počítači serveru SQL Server a má připojení s malou šířkou pásma.
  * **Brána**: brány bylo dosaženo jeho zatížení hello tooperform omezení následující operace:
    * **Serializace**: serializaci hello datový proud tooCSV formátu má pomalé propustnost.
    * **Komprese**: jste zvolili pomalé kompresní kodek (například bzip2, což je 2,8 MB/s s i7 jádra).
  * **WAN**: hello šířky pásma mezi podnikovou sítí hello a služeb Azure je nízká (například T1 = 1,544 kbps; T2 = 6,312 kb/s).
* **Jímky**: úložiště objektů Blob má nízkou propustnost. (Tento scénář nepravděpodobné, protože jeho SLA zaručuje minimálně 60 MB/s.)

V takovém případě může komprese dat bzip2 zpomalení hello celého kanálu. Přepínání kodek pro kompresi gzip tooa může usnadňují tyto potíže.

## <a name="sample-scenarios-use-parallel-copy"></a>Ukázkové scénáře: použití paralelních kopie
**Scénář I:** kopírování 1 000 1 MB souborů z úložiště tooBlob hello místní soubor systému.

**Analýzy a optimalizace výkonu**: pro příklad, pokud jste nainstalovali bránu na počítači čtyřjádrový, objekt pro vytváření dat používá 16 toomove paralelní zkopíruje soubory z úložiště tooBlob systému souboru hello současně. Paralelní spuštění tohoto by měl mít za následek vysoké propustnosti. Také lze explicitně zadat počet kopií paralelní hello. Při kopírování mnoho malých souborů paralelní kopie výrazně pomoct propustnost pomocí prostředků efektivněji.

![Scénář 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Scénář II**: Zkopírujte 20 objekty BLOB 500 MB z objektu Blob úložiště tooData Lake Analytics úložiště a pak optimalizaci výkonu.

**Analýzy a optimalizace výkonu**: V tomto scénáři pro vytváření dat zkopíruje hello data z objektu Blob úložiště tooData Lake Store pomocí jedné kopie (**parallelCopies** nastavit too1) a data v cloudu jedním přesun jednotek. Hello propustnost zjistíte bude zavřít toothat popsané v hello [výkonu referenčním oddílu](#performance-reference).   

![Scénář 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Scénář III**: velikost jednotlivých souborů je větší než desítek MB a celkového objemu velký.

**Analýzy a výkonu měnící**: zvýšení **parallelCopies** nevede k lepší výkon kopírování kvůli omezení prostředků hello DMU jeden cloud. Místo toho musíte zadat další cloudu DMUs tooget přesun tooperform dat hello další prostředky. Nezadávejte hodnotu pro hello **parallelCopies** vlastnost. Objekt pro vytváření dat zpracovává hello paralelismus za vás. V tomto případě, pokud jste nastavili **cloudDataMovementUnits** too4, propustnost o čtyřikrát dojde.

![Scénář 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Referenční informace
Zde jsou sledování výkonu a optimalizace odkazy pro některé podporované hello datová úložiště:

* Úložiště Azure (včetně úložiště objektů Blob a Table storage): [cíle škálovatelnosti Azure Storage](../storage/common/storage-scalability-targets.md) a [kontrolní seznam výkonu a škálovatelnosti Azure Storage](../storage/common/storage-performance-checklist.md)
* Azure SQL Database: Můžete [sledovat výkon hello](../sql-database/sql-database-single-database-monitor.md) a zkontrolujte hello databáze transakce (DTU) jednotky procento
* Azure SQL Data Warehouse: Jeho schopnosti se měří v jednotky datového skladu (Dwu); v tématu [spravovat výpočetní výkon v Azure SQL Data Warehouse (přehled)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
* Azure Cosmos DB: [úrovně výkonu v Azure Cosmos DB](../documentdb/documentdb-performance-levels.md)
* Místní SQL Server: [monitorování a optimalizace výkonu](https://msdn.microsoft.com/library/ms189081.aspx)
* Místní souborový server: [optimalizace výkonu pro souborové servery](https://msdn.microsoft.com/library/dn567661.aspx)
