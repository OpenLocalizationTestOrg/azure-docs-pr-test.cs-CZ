---
title: "scénáře aaaData obsahující Data Lake Store | Microsoft Docs"
description: "Pochopení hello různé scénáře a nástroje pomocí data, která může požity, zpracování, stáhnout a vizualizována v Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 37409a71-a563-4bb7-bc46-2cbd426a2ece
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: caaa3979b8a2532089778c3e3db3c711714d3c42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Požadavky na velkých objemů dat pomocí Azure Data Lake Store
Existují čtyři fáze klíče v zpracování velkých objemů dat:

* Příjem velkých objemů dat do úložiště dat v reálném čase nebo v dávkách
* Zpracování dat hello
* Stahování dat hello
* Vizualizace dat hello

V tomto článku se podíváme na tyto fáze s ohledem tooAzure Data Lake Store toounderstand hello možnosti a nástroje k dispozici toomeet vašim potřebám velkých objemů dat.

## <a name="ingest-data-into-data-lake-store"></a>Ingestovat data do Data Lake Store
V této části jsou zdůrazněné hello různých zdrojů dat a hello různými způsoby, ve kterém můžete konzumaci dat do účtu Data Lake Store.

![Ingestovat data do Data Lake Store](./media/data-lake-store-data-scenarios/ingest-data.png "Ingestovat data do Data Lake Store")

### <a name="ad-hoc-data"></a>Ad hoc dat
To představuje menší sady dat, které se používá pro vytváření prototypů velkých objemů dat aplikace. Příjem ad hoc dat v závislosti na hello zdroj dat hello různými způsoby.

| Zdroj dat | Ingestování pomocí |
| --- | --- |
| Místní počítač |<ul> <li>[Azure Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure CLI a platformy 2.0](data-lake-store-get-started-cli-2.0.md)</li> <li>[Pomocí nástrojů Data Lake pro Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md) </li></ul> |
| Objekt Blob úložiště Azure |<ul> <li>[Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)</li> <li>[Nástroj AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp běžící v clusteru HDInsight](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Datové proudy
Reprezentuje data, která může být generována různých zdrojů, jako je například aplikace, zařízení, senzorů, atd. Tato data můžete konzumaci do Data Lake Store pomocí různých nástrojů. Tyto nástroje se obvykle zachycení a zpracování dat hello u jednotlivých událostí událostí v reálném čase a zapište si hello událostí v dávkách do Data Lake Store tak, že můžete mít další zpracovány.

Toto jsou nástroje, které můžete použít:

* [Azure Stream Analytics](../stream-analytics/stream-analytics-data-lake-output.md) -události požity do centra událostí lze zapisovat tooAzure Data Lake pomocí Azure Data Lake Store výstup.
* [Azure HDInsight Storm](../hdinsight/hdinsight-storm-write-data-lake-store.md) – data můžete psát přímo tooData Lake Store z hello Storm cluster.
* [EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) – může přijímat události ze služby Event Hubs a zapište si ji tooData Lake Store pomocí hello [Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relačních dat
Zdrojem může také data z relační databáze. Období čas relačních databází shromažďovat obrovské objemy dat, které můžou poskytovat klíče statistiky, pokud je zpracování velkých objemů dat kanálu. Taková data můžete použít následující nástroje toomove hello do Data Lake Store.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Data protokolu webového serveru (nahrávání pomocí vlastních aplikací)
Tento typ datové sady je konkrétně označuje, protože analýzy dat protokolu webového serveru se běžně používá pro velké objemy dat aplikace a vyžaduje, že velké objemy toobe soubory protokolu načtený toohello Data Lake Store. Některé z následujících nástrojů toowrite hello můžete použít vlastní skripty nebo aplikace tooupload taková data.

* [Azure CLI a platformy 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [.NET SDK služby Azure Data Lake Store](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

Pro nahrávání dat protokolu webového serveru a také pro odesílání dalších typů dat (např. sociálních chráněny data), je správné přístupu toowrite vlastní vlastních skriptů nebo aplikací, protože nabízejí flexibilitu tooinclude hello data odesílání součásti v rámci větší velkých objemů dat aplikace. V některých případech může trvat tento kód formuláře hello skriptu nebo nástroj příkazového řádku jednoduché. V jiných případech může být hello kód používané toointegrate velkých objemů dat zpracování na obchodní aplikace nebo řešení.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Data přidružená k Azure HDInsight clustery
Většina typů clusteru HDInsight (Hadoop, HBase, Storm) podporují jako úložiště dat úložiště Data Lake Store. Clustery HDInsight přístup k datům z Azure úložiště objektů BLOB (WASB). Pro lepší výkon můžete zkopírovat z WASB hello dat do účtu Data Lake Store přidruženého k hello clusteru. Můžete použít následující nástroje toocopy hello data hello.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy služby](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Data uložená v místní nebo IaaS Hadoop clusterů
Velké objemy dat mohou být uloženy ve stávajících clusterů Hadoop, místně do počítače, které používají HDFS. clustery Hadoop Hello může být v místním nasazení nebo může být v rámci clusteru služby IaaS v Azure. Může dojít k toocopy požadavky těchto dat tooAzure Data Lake Store přístup, jednorázové nebo opakované způsobem. Existují různé možnosti, které můžete použít tooachieve to. Níže uvádíme seznam alternativních a hello související kompromis.

| Přístup | Podrobnosti | Výhody | Požadavky |
| --- | --- | --- | --- |
| Pomocí Azure Data Factory (ADF) toocopy data přímo z Hadoop clusterů tooAzure Data Lake Store |[ADF podporuje HDFS jako zdroj dat](../data-factory/data-factory-hdfs-connector.md) |ADF poskytuje podporu se na pole pro HDFS a první klient server správy a monitorování |Vyžaduje Brána pro správu dat toobe nasadit místně nebo v clusteru IaaS hello |
| Exportujte data z Hadoop jako soubory. Pak zkopírujte hello soubory tooAzure Data Lake Store pomocí vhodný mechanismus. |Můžete zkopírovat soubory tooAzure Data Lake Store pomocí: <ul><li>[Prostředí Azure PowerShell pro systém Windows operačního systému](data-lake-store-get-started-powershell.md)</li><li>[Azure CLI a platformy 2.0 pro Windows OS](data-lake-store-get-started-cli-2.0.md)</li><li>Vlastní aplikaci pomocí jakékoli Data Lake Store SDK</li></ul> |Rychlé tooget spustit. Můžete provést vlastní nahrávání |Vícekrokový proces, který zahrnuje několik technologie. Správa a sledování se zvýší toobe výzvu přes čas zadaný hello přizpůsobit povaha hello nástroje |
| Použití Distcp toocopy data z Hadoop tooAzure úložiště. Potom zkopírujte data z Azure Storage tooData Lake Store pomocí vhodný mechanismus. |Může kopírovat data z Azure Storage tooData Lake Store pomocí: <ul><li>[Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)</li><li>[Nástroj AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp systémem clustery HDInsight](data-lake-store-copy-data-wasb-distcp.md)</li></ul> |Můžete použít nástroje open source. |Vícekrokový proces, který zahrnuje několik technologie |

### <a name="really-large-datasets"></a>Opravdu rozsáhlých datových sad
Nahrát datové sady, které rozsahu v několika terabajtů, pomocí výše popsané metody hello může někdy být pomalé a nákladná. V takových případech můžete hello níže uvedené možnosti.

* **Pomocí služby Azure ExpressRoute**. Azure ExpressRoute vám umožňuje vytvářet privátní připojení mezi datovými centry Azure a infrastruktury místně. To poskytuje spolehlivé možnost pro přenos velkých objemů dat. Další informace najdete v tématu [dokumentace Azure ExpressRoute](../expressroute/expressroute-introduction.md).
* **"Do režimu offline" nahrávání dat**. Pokud pomocí Azure ExpressRoute není možné je z jakéhokoli důvodu, můžete použít [služba Azure Import/Export](../storage/common/storage-import-export-service.md) tooship pevných disků s vaše data tooan datového centra Azure. Vaše data jsou první nahrané tooAzure objektů BLOB Storage. Pak můžete použít [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) nebo [AdlCopy nástroj](data-lake-store-copy-data-azure-storage-blob.md) toocopy dat z Azure úložiště objektů BLOB tooData Lake Store.

  > [!NOTE]
  > Při importu a exportu služby pomocí hello, velikost souboru hello na hello center disky dodávají spolu tooAzure dat nesmí být větší než 195 GB.
  >
  >

## <a name="process-data-stored-in-data-lake-store"></a>Zpracování dat uložených v Data Lake Store
Jakmile hello dat je k dispozici v Data Lake Store můžete spustit analýzu že dat pomocí hello podporuje velké objemy dat aplikací. V současné době můžete použít Azure HDInsight a Azure Data Lake Analytics úlohy analýzy datového toorun na hello data uložená v Data Lake Store.

![Analýza dat v Data Lake Store](./media/data-lake-store-data-scenarios/analyze-data.png "analýza dat v Data Lake Store")

Můžete si prohlédnout hello následující příklady.

* [Vytvoření clusteru HDInsight s Data Lake Store jako úložiště](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="download-data-from-data-lake-store"></a>Stahování dat z Data Lake Store
Může také chcete toodownload nebo přesun dat z Azure Data Lake Store pro scénáře, jako například:

* Přesun dat tooother úložiště toointerface s vaší stávající zpracování dat kanály. Například můžete chtít toomove dat z Data Lake Store tooAzure SQL Database nebo místní systém SQL Server.
* Stáhněte si data tooyour místního počítače pro zpracování v prostředí IDE při vytváření prototypů aplikace.

![Výstupních dat z Data Lake Store](./media/data-lake-store-data-scenarios/egress-data.png "výstupních dat z Data Lake Store")

V takových případech můžete použít některý z hello následující možnosti:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Můžete taky hello následující metody toowrite svoje vlastní data toodownload aplikace nebo skriptu z Data Lake Store.

* [Azure CLI a platformy 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [.NET SDK služby Azure Data Lake Store](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Vizualizace dat v Data Lake Store
Můžete použít kombinaci služby toocreate vizuální znázornění dat uložených v Data Lake Store.

![Vizualizace dat v Data Lake Store](./media/data-lake-store-data-scenarios/visualize-data.png "vizualizaci dat v Data Lake Store")

* Můžete spustit pomocí [Azure Data Factory toomove dat z Data Lake Store tooAzure SQL Data Warehouse](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats)
* Potom můžete [Power BI integrovat Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) toocreate vizuální znázornění dat hello.
