---
title: "aaaIntroduction tooData integrační služba data Factory | Microsoft Docs"
description: "Dozvíte se, že Data Factory je cloudová služba pro integraci dat, která orchestruje a automatizuje přesouvání a transformaci dat."
keywords: integrace dat, integrace dat v cloudu, co je azure data factory
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: cec68cb5-ca0d-473b-8ae8-35de949a009e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 4cc30515315efc938951057743ff8eb3701214ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-data-factory"></a>Úvod tooAzure objekt pro vytváření dat 
## <a name="what-is-azure-data-factory"></a>Co je služba Azure Data Factory?
V hello, world velkých objemů dat jak je stávající data využity v obchodní? Je možné tooenrich data v cloudu hello vygenerovaného s využitím referenční data z místního zdroje dat nebo jiných různorodých zdrojů dat? Například společnost herní shromažďuje mnoho protokoly vyprodukované hry v cloudu hello. Tyto protokoly toogain insights toocustomer předvolby, využití chování a demografie chce tooanalyze až prodává a mezi prodává příležitostí tooidentify atd vývoj nových přesvědčivé růst podniku toodrive funkce a zajistit lepší prostředí toocustomers. 

tooanalyze tyto protokoly hello společnosti musí toouse hello referenční data jsou to například informace zákazníka, herní informace, marketingové kampaně informace, které jsou v úložišti místní data. Proto hello společnost chce tooingest dat protokolu z úložiště dat hello cloudu a referenčních dat z úložiště dat místní hello. Poté proces hello dat pomocí Hadoop v hello cloudu (Azure HDInsight) a publikovat hello výsledek úložiště dat do datového skladu cloudu, například Azure SQL Data Warehouse nebo místním datům, jako je SQL Server. Je chce tento pracovní postup toorun týdně jednou. 

Co je potřeba je platforma, která umožňuje hello společnosti toocreate pracovní postup, který můžete ingestovat data z místní i cloudové úložiště dat a transformace nebo proces data pomocí stávající výpočetní služby, jako je například Hadoop a publikovat hello výsledky tooan na místě nebo cloudové úložiště dat pro tooconsume BI aplikace. 

![Přehled služby Data Factory](media/data-factory-introduction/what-is-azure-data-factory.png) 

Azure Data Factory je hello platforma pro tento druh scénářů. Je **služba pro integraci dat založené na cloudu, který vám umožní toocreate řízené daty pracovních v cloudu hello pro orchestruje a automatizuje přesouvání dat a transformaci dat**. Pomocí Azure Data Factory, můžete vytvořit a naplánovat řízené daty pracovní postupy (nazývané kanály) můžete načítání dat z úložiště dat různorodých, proces/transformace hello dat pomocí výpočetních služeb například Azure HDInsight Hadoop, Spark, Azure Data Lake Analýza a Azure Machine Learning a publikovat výstupní data toodata ukládá jako je například Azure SQL Data Warehouse pro tooconsume aplikace business intelligence (BI).  

Spíše než o tradiční platformu extrakce, transformace a načítání (ETL) se jedná o platformu extrakce a načítání (EL) a následné transformace a načtení (TL). Hello transformace, které se provádí jsou tootransform nebo zpracování dat pomocí výpočetních služeb, nikoli tooperform transformace jako hello ty, které jsou pro přidání odvozených sloupců, počítání počet řádků, řazení dat atd. 

V současné době v Azure Data Factory, je hello data, která je využívat a vytvářených pracovních **rozříznut čas data** (každou hodinu, denně, týdně atd.). Kanál například může číst vstupní data, zpracovat je a vyprodukovat výstupní data jednou denně. Pracovní postup můžete také spustit jenom jednou.  
  

## <a name="how-does-it-work"></a>Jak to funguje? 
Hello kanály (datové pracovních postupů) v Azure Data Factory provádět obvykle hello následující tři kroky:

![Tři fáze služby Azure Data Factory](media/data-factory-introduction/three-information-production-stages.png)

### <a name="connect-and-collect"></a>Připojení a shromažďování
Podniky mají data různých typů, která se nachází v různorodých zdrojích. Hello prvním krokem při vytvoření informačního produkční systému je tooconnect tooall hello požadované zdroje dat a zpracování, např. služby SaaS, souborů sdílených složek, FTP, webové služby a přesunutí hello data podle potřeby tooa centralizované umístění pro následné zpracování.

Bez služby Data Factory musí podniky sestavení součásti přesun vlastních dat nebo zápis vlastních služeb toointegrate tyto zdroje dat a zpracování. Je nákladné a pevné toointegrate a udržovat takového systému a často neobsahuje hello podnikové úrovni monitorování a výstrah a hello ovládacích prvků, které nabízejí plně spravované služby.

Pomocí služby Data Factory můžete použít hello aktivitu kopírování v kanálu dat toomove dat z místní a cloudové zdrojového úložiště tooa centralizace datového úložiště dat v cloudu hello k další analýze. Například můžete shromažďovat data v Azure Data Lake Store a transformace hello data později pomocí výpočetní služby Azure Data Lake Analytics. Nebo můžete data shromažďovat v Azure Blob Storage a později je transformovat pomocí clusteru Azure HDInsight Hadoop.

### <a name="transform-and-enrich"></a>Transformace a rozšíření
Jakmile se data nachází v úložišti dat centralizovaný v cloudu hello, budete chtít hello shromažďovaných dat toobe zpracovat nebo transformovat pomocí výpočetní služby jako HDInsight Hadoop, Spark, Data Lake Analytics a Machine Learning. Chcete, aby tooreliably produktu transformovat data na provozní prostředí plán údržby a řízené toofeed důvěryhodné daty. 

### <a name="publish"></a>Publikování 
Poskytování Transformovaná data z cloudu hello tooon místní zdroje, jako je SQL Server, nebo jej zachovat ve vašem cloudu zdrojů úložiště pro používání business intelligence (BI) a nástroje pro analýzu a další aplikace.

## <a name="key-components"></a>Klíčové komponenty
Předplatné Azure může obsahovat jednu nebo více instancí služby Azure Data Factory (neboli datových továren). Azure Data Factory se skládá ze čtyř klíčové komponenty, které vzájemně spolupracují tooprovide hello platformy, na které můžete vytvořit datové pracovních s kroky toomove a transformace dat. 

### <a name="pipeline"></a>Kanál
Datová továrna může mít jeden nebo víc kanálů. Kanál je skupina aktivit. Hello aktivity v kanálu společně, provádějí úlohy. Kanál může například obsahovat skupinu aktivit, která ingestuje dat z objektu blob Azure a pak spuštění dotazu Hive v clusteru HDInsight toopartition hello data. Výhodou Hello je, že tento kanál hello vám umožní aktivity hello toomanage jako sada místo každé z nich jednotlivě. Například můžete nasadit a naplánovat hello kanálu, místo hello aktivity nezávisle. 

### <a name="activity"></a>Aktivita
Kanál může mít jednu nebo víc aktivit. Aktivity definují akce tooperform hello na vaše data. Například může použít data toocopy aktivity kopírování z úložiště dat tooanother jednoho datového úložiště. Podobně můžete použít aktivitu Hive, který běží na clusteru tootransform Azure HDInsight dotazů Hive nebo analyzovat data. Data Factory podporuje dva typy aktivit: aktivity přesunu dat a transformace dat.

### <a name="data-movement-activities"></a>Aktivity přesunu dat
Aktivita kopírování v datové továrně zkopíruje data z úložiště zdroje dat úložiště tooa podřízený data. Objekt pro vytváření dat podporuje hello následující datová úložiště. Data z jakéhokoli zdroje může být napsán tooany jímky. Klikněte na tlačítko data store toolearn jak toocopy tooand data z tohoto úložiště.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Podrobnosti najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).

### <a name="data-transformation-activities"></a>Aktivity transformace dat
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Podrobnosti najdete v článku [Aktivity transformace dat](data-factory-data-transformation-activities.md).

### <a name="custom-net-activities"></a>Vlastní aktivity .NET
Pokud potřebujete toomove dat do/z jiného úložiště dat není podporují, nebo transformace dat pomocí vlastní logiky, vytvoření kopie aktivity **vlastní aktivity .NET**. Podrobnosti o vytvoření a používání vlastní aktivity najdete v tématu [Použití vlastních aktivit v kanálu služby Azure Data Factory](data-factory-use-custom-activities.md).

### <a name="datasets"></a>Datové sady
Aktivita přijímá jako vstup nula nebo více datových sad a produkuje jednu nebo více datových sad jako výstup. Datové sady představují datové struktury v rámci úložišť dat hello, které jednoduše bodu nebo referenční data hello chcete toouse vašich aktivit jako vstupní nebo výstupní. Například datové sadě služby Azure Blob určuje hello kontejner objektů blob a složky v hello Azure Blob Storage, ze které hello kanálu by měl číst hello data. Nebo datové sadě služby Azure SQL tabulka určuje hello tabulky toowhich hello výstupní data jsou zapsána pomocí aktivity hello. 

### <a name="linked-services"></a>Propojené služby
Propojené služby jsou mnohem jako připojovací řetězce, které definují hello připojení informace potřebné pro vytváření dat tooconnect tooexternal prostředky. Považujte ho tímto způsobem – propojené služby definuje zdroj dat toohello hello připojení a datové sady představuje hello strukturu dat hello. Například služby Azure Storage, propojené Určuje účet úložiště Azure toohello tooconnect řetězec připojení. A datové sadě služby Azure Blob určuje hello kontejner objektů blob a hello složku, která obsahuje hello data.   

Propojené služby slouží ve službě Data Factory ke dvěma účelům:

* toorepresent **úložiště dat** včetně, ale bez omezení, místní SQL Server, databáze Oracle, sdílené složky nebo účet Azure Blob Storage. V tématu hello [aktivity přesunu dat](#data-movement-activities) části Seznam podporované datové úložiště.
* toorepresent **výpočetní prostředky** , může být hostitelem hello provádění aktivity. Například hello aktivita HDInsightHive běží na clusteru HDInsight Hadoop. Seznam podporovaných výpočetních prostředí najdete v oddílu [Aktivity transformace dat](#data-transformation-activities).

### <a name="relationship-between-data-factory-entities"></a>Vztah mezi entitami služby Data Factory
![Diagram: Data Factory, služba pro integraci dat v cloudu – Klíčové koncepty](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Obrázek 2** Vztahy mezi datovou sadou, aktivitou, kanálem a propojenou službou

## <a name="supported-regions"></a>Podporované oblasti
V současné době můžete vytvořit datové továrny hello **západní USA**, **východní USA**, a **Severní Evropa** oblasti. Ale objekt pro vytváření dat může přistupovat k úložištím dat a výpočetním službám v jiných oblastech Azure toomove dat mezi úložišti dat nebo zpracování dat pomocí výpočetních služeb.

Samotná služba Azure Data Factory žádná data neuchovává. Umožňuje vám vytvořit datové pracovních tooorchestrate přesouvání dat mezi [podporovanými úložišti dat](#data-movement-activities) a zpracování dat pomocí [výpočetní služby](#data-transformation-activities) v jiných oblastech nebo v místní prostředí. Můžete taky příliš[sledování a správa pracovních](data-factory-monitor-manage-pipelines.md) pomocí prostřednictvím kódu programu a uživatelského prostředí.

I když objekt pro vytváření dat je k dispozici v pouze **západní USA**, **východní USA**, a **Severní Evropa** oblastí hello služba pohánějící přesouvání dat hello ve službě Data Factory je k dispozici [globálně](data-factory-data-movement-activities.md#global) v několika oblastech. Pokud úložiště dat je za bránou firewall, pak [Brána pro správu dat](data-factory-move-data-between-onprem-and-cloud.md) místo toho nainstaluje v datech hello přesune místní prostředí.

Předpokládejme například, že vaše výpočetní prostředí, jako je cluster Azure HDInsight nebo Azure Machine Learning, běží v oblasti Západní Evropa. Můžete vytvořit a použít instanci Azure Data Factory v oblasti Severní Evropa a použít ho tooschedule úlohy na výpočetních prostředích v oblasti západní Evropa. Trvá několik milisekund pro vytváření dat tootrigger hello úlohu na výpočetním prostředí, ale hello dobu pro spuštění hello úlohy na výpočetním prostředí se nemění.

## <a name="get-started-with-creating-a-pipeline"></a>Začínáme s vytvořením kanálu
V Azure Data Factory můžete použít jednu z těchto nástrojů nebo rozhraní API toocreate datových kanálů: 

- portál Azure
- Visual Studio
- PowerShell
- .NET API
- REST API
- Šablona Azure Resource Manageru 

toolearn jak kanálů toobuild datové továrny s daty, postupujte podle podrobných pokynů v hello následující kurzy:

| Kurz | Popis |
| --- | --- |
| [Přesun dat mezi dvěma cloudovými úložišti dat](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |V tomto kurzu vytvoříte objekt pro vytváření dat se zřetězením příkazů, **přesouvá data** z databáze tooSQL úložiště objektů Blob. |
| [Transformace dat pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md) |V tomto kurzu sestavíte svůj první objekt pro vytváření dat Azure s datovým kanálem, který **zpracovává data** pomocí skriptu Hive v clusteru Azure HDInsight (Hadoop). |
| [Přesun dat mezi místním úložištěm dat a cloudovým úložištěm dat pomocí brány správy dat](data-factory-move-data-between-onprem-and-cloud.md) |V tomto kurzu vytvoříte objekt pro vytváření dat se zřetězením příkazů, **přesouvá data** z **místní** tooan databáze systému SQL Server objektů blob v Azure. Jako součást hello návod nainstalujte a nakonfigurujte hello Brána pro správu dat na počítači. |
