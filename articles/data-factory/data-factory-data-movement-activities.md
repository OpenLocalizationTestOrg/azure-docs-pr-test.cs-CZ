---
title: "aaaMove dat pomocí aktivity kopírování | Microsoft Docs"
description: "Další informace o přesun dat v kanálů služby Data Factory: migrace dat mezi cloudové úložiště a mezi úložišti místního a cloudového úložiště. Pomocí aktivity kopírování."
keywords: "kopírování dat, přesunu dat, data migrace, přenos dat"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 67543a20-b7d5-4d19-8b5e-af4c1fd7bc75
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: 29b74154b9006795ead3b0ee9638a3dbf2c5d831
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-by-using-copy-activity"></a>Přesun dat pomocí aktivity kopírování
## <a name="overview"></a>Přehled
V Azure Data Factory, můžete data toocopy aktivity kopírování mezi místními a cloudovými datová úložiště. Po zkopírování dat hello může být další transformovat a analyzovat. Aktivita kopírování toopublish transformaci a výsledky analýzy můžete použít také pro business intelligence (BI) a využití aplikace.

![Role aktivity kopírování](media/data-factory-data-movement-activities/copy-activity.png)

Aktivita kopírování používá technologii zabezpečený, spolehlivou a škálovatelnou, a [globálně dostupnou službu,](#global). Tento článek poskytuje podrobné informace o přesouvání dat ve službě Data Factory a aktivita kopírování.

Nejprve si ukážeme, jak dojde k migraci dat mezi dvě úložiště dat cloudu a mezi místnímu úložišti dat a úložiště dat cloudu.

> [!NOTE]
> Obecně platí, najdete v části toolearn o aktivitách [Principy kanály a aktivity](data-factory-create-pipelines.md).
>
>

### <a name="copy-data-between-two-cloud-data-stores"></a>Kopírovat data mezi dvěma cloudové úložiště dat
Pokud jsou zdroj a jímka datová úložiště v cloudu hello, aktivity kopírování projde hello následujících fázích toocopy data z hello zdroj toohello jímky. Hello služba, která pohání aktivity kopírování:

1. Přečte data ze zdrojového úložiště dat hello.
2. Provede serializaci nebo deserializaci, kompresi nebo dekompresi, mapování sloupců a typ – převod. Dělá tyto operace na základě hello konfigurací hello vstupní datové sady, výstupní datovou sadu a kopie aktivity.
3. Zapíše data toohello cílového úložiště dat.

Služba Hello automaticky zvolí přesun dat hello optimální oblast tooperform hello. Tato oblast je obvykle hello jeden nejbližší toohello podřízený datové úložiště.

![Cloud cloudové kopírování](./media/data-factory-data-movement-activities/cloud-to-cloud.png)

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Kopírování dat mezi úložišti dat místní a cloudové úložiště dat
toosecurely přesun dat mezi úložišti dat místní a cloudové úložiště dat, nainstalujte Brána pro správu dat na místním počítači. Brána pro správu dat je agenta, který umožňuje přesun dat hybridní a zpracování. Můžete ho nainstalovat na stejný počítač jako úložiště dat hello samotné hello nebo na samostatný počítač, který má přístup k datům toohello uložit.

V tomto scénáři Brána pro správu dat provede serializaci nebo deserializaci hello, kompresi nebo dekompresi, mapování sloupců a typ – převod. Data nejsou procházet skrz hello služby Azure Data Factory. Místo toho brána pro správu dat přímo zapíše hello data toohello cílové úložiště.

![Kopírování na místní--cloud](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

V tématu [přesun dat mezi místní a cloudové úložiště dat](data-factory-move-data-between-onprem-and-cloud.md) úvod a návod. V tématu [Brána pro správu dat](data-factory-data-management-gateway.md) podrobné informace o tohoto agenta.

Také můžete přesunout data z / úložiště toosupported dat, které jsou hostované na virtuální počítače Azure IaaS (VM) s použitím brány pro správu dat. V takovém případě můžete nainstalovat brána pro správu dat na hello stejného virtuálního počítače jako úložiště dat hello sám sebe nebo na samostatný virtuální počítač, který má přístup k datům toohello uložit.

## <a name="supported-data-stores-and-formats"></a>Podporované datové úložiště a formáty
Aktivita kopírování v datové továrně zkopíruje data z úložiště zdroje dat úložiště tooa podřízený data. Objekt pro vytváření dat podporuje hello následující datová úložiště. Data z jakéhokoli zdroje může být napsán tooany jímky. Klikněte na tlačítko data store toolearn jak toocopy tooand data z tohoto úložiště.

> [!NOTE] 
> Pokud potřebujete toomove data do nebo z úložiště dat, která nepodporuje aktivity kopírování, použijte **vlastní aktivity** v objektu pro vytváření dat s vlastní logikou pro kopírování nebo přesunutí data. Podrobnosti o vytvoření a používání vlastní aktivity najdete v tématu [Použití vlastních aktivit v kanálu služby Azure Data Factory](data-factory-use-custom-activities.md).

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Úložiště dat, s * může být místní nebo v Azure IaaS a vyžadují tooinstall [Brána pro správu dat](data-factory-data-management-gateway.md) na počítači v místní nebo Azure IaaS.

### <a name="supported-file-formats"></a>Podporované formáty souborů
Můžete použít aktivitu kopírování příliš**zkopírujte soubory jako-je** mezi dvě souborové úložiště dat, můžete přeskočit hello [formátovat oddíl](data-factory-create-datasets.md) v hello vstupní i výstupní datovou sadu definice. Hello data se zkopírují efektivně bez serializaci nebo deserializaci.

Aktivita kopírování také čte z a zapíše toofiles v zadané formáty: **Text JSON, Avro, ORC a Parquet**a kompresní kodek **GZip, Deflate, BZip2 a ZipDeflate** jsou podporovány. V tématu [podporované formáty souborů a komprese](data-factory-supported-file-and-compression-formats.md) s podrobnostmi.

Například můžete provést následující hello zkopírujte aktivity:

* Kopírování dat na místním serveru SQL a zápisu ve formátu ORC tooAzure Data Lake Store.
* Zkopírujte soubory ve formátu textu (CSV) z místní systém souborů a zápis tooAzure objektů Blob ve formátu Avro.
* Kopírování komprimované soubory ze systému souborů na místě a dekomprese poté zobrazovat tooAzure Data Lake Store.
* Kopírování dat z Azure Blob ve formátu GZip komprimované textu (CSV) a zápis tooAzure databáze SQL.

## <a name="global"></a>Přesun globálně dostupnou dat
Azure Data Factory je k dispozici pouze v oblastech severní Evropa, západní USA a východní USA hello. Však je k dispozici globálně v hello hello služby, která pohání aktivity kopírování oblasti a zeměpisných oblastí. globálně dostupnou topologie Hello zajišťuje přesun efektivní dat, která obvykle zabraňuje směrování mezi oblastmi. V tématu [služby podle oblasti](https://azure.microsoft.com/regions/#services) dostupnost služby Data Factory a přesun dat v oblasti.

### <a name="copy-data-between-cloud-data-stores"></a>Kopírování dat mezi úložišti dat cloudu
Pokud jsou zdroj a jímka datová úložiště v cloudu hello, objekt pro vytváření dat používá nasazení služby v oblasti hello, který je nejblíže podřízený toohello v hello stejná geography toomove hello data. Viz následující tabulka pro mapování toohello:

| Geography hello cílové datová úložiště | Oblast hello cílového úložiště dat | Oblast pro přesun dat |
|:--- |:--- |:--- |
| Spojené státy | Východ USA | Východ USA |
| &nbsp; | Východní USA 2 | Východní USA 2 |
| &nbsp; | Střed USA | Střed USA |
| &nbsp; | Střed USA – sever | Střed USA – sever |
| &nbsp; | Střed USA – jih | Střed USA – jih |
| &nbsp; | Západní střed USA | Západní střed USA |
| &nbsp; | Západní USA | Západní USA |
| &nbsp; | Západní USA 2 | Západní USA |
| Kanada | Východní Kanada | Střední Kanada |
| &nbsp; | Střední Kanada | Střední Kanada |
| Brazílie | Brazílie – jih | Brazílie – jih |
| Evropa | Severní Evropa | Severní Evropa |
| &nbsp; | Západní Evropa | Západní Evropa |
| Spojené království | Spojené království – západ | Spojené království – jih |
| &nbsp; | Spojené království – jih | Spojené království – jih |
| Asie a Tichomoří | Jihovýchodní Asie | Jihovýchodní Asie |
| &nbsp; | Východní Asie | Jihovýchodní Asie |
| Austrálie | Austrálie – východ | Austrálie – východ |
| &nbsp; | Austrálie – jihovýchod | Austrálie – jihovýchod |
| Japonsko | Japonsko – východ | Japonsko – východ |
| &nbsp; | Japonsko – západ | Japonsko – východ |
| Indie | Střed Indie | Střed Indie |
| &nbsp; | Indie – západ | Střed Indie |
| &nbsp; | Indie – jih | Střed Indie |

Alternativně můžete explicitně určit hello oblast toobe služby Data Factory používá tooperform hello kopie zadáním `executionLocation` vlastnost v aktivitě kopírování `typeProperties`. Podporované hodnoty této vlastnosti jsou nahoře uvedené v **používá oblast pro přesun dat** sloupce. Všimněte si, že vaše data procházejí v této oblasti průběhu hello přenosová během kopírování. Například toocopy mezi Azure ukládá v Koreji, můžete zadat `"executionLocation": "Japan East"` tooroute prostřednictvím Japonsko oblast (najdete v části [ukázkové JSON](#by-using-json-scripts) jako odkaz).

> [!NOTE]
> Pokud hello oblast hello cílového úložiště dat není v předchozím seznamu nebo nezjistitelný, ve výchozím nastavení aktivity kopírování selže místo alternativní oblast, pokud `executionLocation` je zadán. Rozbalí se seznam Hello podporované oblasti v čase.
>

### <a name="copy-data-between-an-on-premises-data-store-and-a-cloud-data-store"></a>Kopírování dat mezi úložišti dat místní a cloudové úložiště dat
Při kopírování dat mezi místními (nebo Azure virtuální počítače nebo IaaS) a cloudových úložišť [Brána pro správu dat](data-factory-data-management-gateway.md) provádí přesun dat na místní počítač nebo virtuální počítač. Hello není toku dat prostřednictvím služby hello v cloudu hello, pokud nechcete použít hello [připravený kopie](data-factory-copy-activity-performance.md#staged-copy) schopností. V takovém případě toky dat prostřednictvím hello před zápisem do úložiště dat podřízený hello pracovní úložiště objektů Blob v Azure.

## <a name="create-a-pipeline-with-copy-activity"></a>Vytvoření kanálu s aktivitou kopírování
Vytvoření kanálu s aktivitou kopírování v několika způsoby:

### <a name="by-using-hello-copy-wizard"></a>Pomocí Průvodce kopírováním hello
Průvodce kopírováním služby Data Factory Hello vám pomůže toocreate kanál s aktivitou kopírování. Tento kanál vám umožní toocopy data z podporovaných zdrojů toodestinations *bez nutnosti psaní JSON* definice pro propojené služby, datové sady a kanály. V tématu [Průvodce kopírováním služby Data Factory](data-factory-copy-wizard.md) podrobnosti o hello průvodce.  

### <a name="by-using-json-scripts"></a>Pomocí skriptů JSON
Můžete použít Editor služby Data Factory v hello portálu Azure, Visual Studio nebo Azure PowerShell toocreate definici JSON pro kanál (pomocí aktivity kopírování). Pak můžete nasadit toocreate hello kanál v datové továrně. V tématu [kurz: použijte aktivitu kopírování v kanál služby Azure Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kurz s podrobnými pokyny.    

Jsou dostupné pro všechny typy aktivit JSON vlastnosti (například název, popis, vstupní a výstupní tabulky a zásady). Vlastnosti, které jsou k dispozici v hello `typeProperties` části hello aktivity se liší podle každý typ aktivity.

Pro aktivitu kopírování, hello `typeProperties` části se liší v závislosti na typech hello zdrojů a jímky. Klikněte na tlačítko zdroj/jímka v hello [podporované zdroje a jímky](#supported-data-stores-and-formats) toolearn část o typu vlastnosti, které aktivity kopírování podporuje pro toto datové úložiště.

Zde je ukázka definice JSON:

```json
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from Azure blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputBlobTable"
          }
        ],
        "outputs": [
          {
            "name": "OutputSQLTable"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
          },
          "executionLocation": "Japan East"          
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
}
```
Hello plán, který je definován v hello výstupní datovou sadu Určuje spuštění aktivity hello (například: **denní**, frekvence jako **den**a intervalu jako **1**). Hello aktivity kopíruje data ze vstupní datové sady (**zdroj**) tooan výstupní datovou sadu (**podřízený**).

Můžete zadat více než jeden vstupní datové sady tooCopy aktivity. Před spuštěním hello aktivity jsou použité tooverify hello závislosti. Jenom hello data z první datovou sadu hello je však zkopírovaný toohello cílové datové sadě. Další informace najdete v tématu [plánování a provádění](data-factory-scheduling-and-execution.md).  

## <a name="performance-and-tuning"></a>Výkon a ladění
V tématu hello [výkonu kopie aktivity a vyladění průvodce](data-factory-copy-activity-performance.md), který popisuje klíčové faktory ovlivňující výkon hello přesun dat (aktivita kopírování) v Azure Data Factory. Také uvádí hello zjištěnými výkon při interním testování a popisuje různé způsoby toooptimize hello výkonu kopie aktivity.

## <a name="fault-tolerance"></a>Odolnost proti chybám
Ve výchozím nastavení, bude aktivity kopírování zastavíte kopírování dat a vraťte se selhání když dojde k nekompatibilní data mezi zdroj a jímka; explicitně nakonfigurovat tooskip a protokolu hello nekompatibilní řádky a pouze kopie těchto kopírování hello toomake kompatibilní dat bylo úspěšně dokončeno. V tématu hello [aktivity kopírování odolnost proti chybám](data-factory-copy-activity-fault-tolerance.md) na další podrobnosti.

## <a name="security-considerations"></a>Aspekty zabezpečení
V tématu hello [aspekty zabezpečení](data-factory-data-movement-security-considerations.md), který popisuje, infrastruktura zabezpečení pomocí služby pro přesun dat v Azure Data Factory toosecure dat.

## <a name="scheduling-and-sequential-copy"></a>Plánování a sekvenčních kopie
V tématu [plánování a provádění](data-factory-scheduling-and-execution.md) podrobné informace o plánování a provádění fungování ve službě Data Factory. Ho je možné toorun více operací kopírování jedna po druhé způsobem sekvenční/řazení. V tématu hello [zkopírujte postupně](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) části.

## <a name="type-conversions"></a>Převody typu
Různé datové úložiště mají jiný typ nativní systémy. Aktivita kopírování provádí automatické typ převody z typů toosink typy zdroje s hello dvoustupňový přístup následující:

1. Převod typu .NET tooa typy nativní zdroje.
2. Převeďte z typu nativní podřízený tooa typ rozhraní .NET.

Hello mapování z typu nativní typ systému tooa .NET pro úložiště dat, které je v článku úložiště příslušných dat hello. (Klikněte na konkrétním propojení hello v hello [podporovanými úložišti dat](#supported-data-stores) tabulky). Při vytváření tabulky, můžete použít tyto typy odpovídající toodetermine mapování, tak, aby aktivita kopírování provádí převody správné hello.

## <a name="next-steps"></a>Další kroky
* toolearn o hello více aktivitě kopírování najdete v části [kopírování dat z tooAzure úložiště objektů Blob v Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* toolearn o přesun dat z místní data store tooa cloudu dat úložiště, najdete v části [přesun dat z úložiště dat toocloud místní](data-factory-move-data-between-onprem-and-cloud.md).
