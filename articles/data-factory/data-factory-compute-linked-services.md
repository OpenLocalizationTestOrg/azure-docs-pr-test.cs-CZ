---
title: "prostředí aaaCompute podporovaných službou Azure Data Factory | Microsoft Docs"
description: "Další informace o výpočetní prostředí, které můžete použít v Azure Data Factory kanály (například Azure HDInsight) tootransform nebo proces data."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6877a7e8-1a58-4cfb-bbd3-252ac72e4145
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/25/2017
ms.author: shlo
ms.openlocfilehash: aba7d7de695bc1c7d475f1e741ee3b3e884151c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Výpočetní prostředí podporovaných službou Azure Data Factory
Tento článek vysvětluje různé výpočetní prostředí, můžete použít tooprocess nebo transformovat data. Obsahuje také podrobnosti o různých konfiguracích (na vyžádání oproti přineste si vlastní) podporovaných službou Data Factory při konfiguraci propojených služeb propojení těchto výpočetní prostředí tooan pro vytváření dat Azure.

Hello následující tabulka obsahuje seznam výpočetních prostředích nepodporuje objekt pro vytváření dat a hello aktivity, které můžou běžet na ně. 

| Výpočetní prostředí | activities |
| --- | --- |
| [Cluster HDInsight na vyžádání](#azure-hdinsight-on-demand-linked-service) nebo [vlastní cluster HDInsight](#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [streamování Hadoop](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](#azure-batch-linked-service) |[DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) |[Aktivity Machine Learning: Dávkové spouštění a Aktualizace prostředku](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) |[U-SQL Data Lake Analytics](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-linked-service), [systému SQL Server](#sql-server-linked-service) |[Uložená procedura](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>Podporované verze HDInsight v Azure Data Factory
Azure HDInsight podporuje více verzích clusterů Hadoop, které lze nasadit kdykoli. Každou verzi volbu vytvoří na konkrétní verzi rozdělení hello Hortonworks Data Platform (HDP) a sadu součástí, které jsou obsažené v příslušné distribuci. Microsoft udržuje aktualizace hello seznam podporovaných verzích HDInsight tooprovide nejnovější ekosystém komponent systému Hadoop a opravy. Hello HDInsight 3.2 je zastaralý na 1. dubna 2017. Podrobné informace najdete v tématu [podporované HDInsight verze](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

To ovlivní existující datové továrny Azure, které mají aktivity spuštěným pro clustery HDInsight 3.2. Doporučujeme, abyste uživatelům toofollow hello pokyny v následující části tooupdate hello hello ovlivněné datové továrny:

### <a name="for-linked-services-pointing-tooyour-own-hdinsight-clusters"></a>Pro propojené služby odkazující tooyour vlastní clustery HDInsight
* **Propojené služby HDInsight odkazující tooyour vlastní HDInsight 3.2 nebo pod clustery:**

  Azure Data Factory podporuje odesílá tooyour úlohy z HDI 3.1 příliš clusterů HDInsight vlastní[hello nejnovější podporované HDInsight verze](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions). Však již vytvořením clusteru HDInsight 3.2 po 1. dubna 2017 podle zásady vyřazení hello zdokumentována [podporované HDInsight verze](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).  

  **Doporučení:** 
  * Proveďte testy tooensure hello kompatibility hello aktivity, které odkazují této propojené služby příliš[hello nejnovější podporované HDInsight verze](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) s informacemi o zdokumentovány v [komponent systému Hadoop k dispozici různé verze HDInsight](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions) a [poznámky související s HDInsight verze verzi Hortonworks](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions).
  * Upgrade clusteru HDInsight 3.2 příliš[hello nejnovější podporované HDInsight verze](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) tooget hello nejnovější součástí ekosystému Hadoop a opravy. 

* **Propojené služby HDInsight odkazující tooyour vlastní HDInsight 3.3 nebo vyšší clustery:**

  Azure Data Factory podporuje odesílá tooyour úlohy z HDI 3.1 příliš clusterů HDInsight vlastní[hello nejnovější podporované HDInsight verze](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions). 
  
  **Doporučení:** 
  * Není vyžadována z hlediska Data Factory žádná akce. Pokud jste na nižší verzi HDInsight, přesto doporučujeme však upgrade příliš[hello nejnovější podporované HDInsight verze](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) tooget hello nejnovější součástí ekosystému Hadoop a opravy.

### <a name="for-hdinsight-on-demand-linked-services"></a>Pro HDInsight na vyžádání propojených služeb
* **Verze 3.2 nebo níže je uveden v definici JSON propojené služby HDInsight na vyžádání:**
  
  Azure Data Factory bude podporovat vytváření clusterů HDInsight na vyžádání verze 3.3 nebo víc z **15/05/2017** a vyšší. A hello konec podpory pro existující 3.2 HDInsight na vyžádání propojené služby je rozšířeno příliš**07, 15 nebo 2017**.  

  **Doporučení:** 
  * Proveďte testy tooensure hello kompatibility hello aktivity, které odkazují této propojené služby příliš [hello nejnovější podporované HDInsight verze](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) s informacemi o zdokumentovány v [komponent systému Hadoop k dispozici různé verze HDInsight](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions) a [poznámky související s HDInsight verze verzi Hortonworks](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions).
  * Před **07, 15 nebo 2017**, aktualizujte vlastnost hello verze v definici JSON propojené služby na vyžádání HDI příliš[hello nejnovější podporované HDInsight verze](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) tooget hello nejnovější ekosystém komponent systému Hadoop a opravy. Podrobné definici JSON, najdete v části toohello [propojená služba Azure HDInsight na vyžádání ukázka](#azure-hdinsight-on-demand-linked-service). 

* **Verze nebyly zadány v propojené služby HDInsight na vyžádání:**
  
  Azure Data Factory bude podporovat vytváření clusterů HDInsight na vyžádání verze 3.3 nebo víc z **15/05/2017** a vyšší. A hello konec podpory tooexisting na vyžádání HDInsight 3.2 propojené služby je rozšířeno příliš**07, 15 nebo 2017**. 

  Před **07, 15 nebo 2017**, pokud je ponecháno prázdné, hello výchozí hodnoty pro verzi a jsou osType vlastnosti: 

  | Vlastnost | Výchozí hodnota | Požaduje se |
  | --- | --- | --- |
  Verze   | HDI 3.1 clusteru se systémem Windows a 3.2 HDI pro cluster s Linuxem.| Ne
  osType | Hello výchozí hodnota je Windows | Ne

  Po **07, 15 nebo 2017**, pokud je ponecháno prázdné, hello výchozí hodnoty pro verzi a jsou osType vlastnosti:

  | Vlastnost | Výchozí hodnota | Požaduje se |
  | --- | --- | --- |
  Verze   | 3.3 HDI pro clusteru se systémem Windows a 3.5 pro cluster s Linuxem.    | Ne
  osType | Výchozí hodnota Hello je Linux   | Ne

  **Doporučení:** 
  * Před **07, 15 nebo 2017**, provádět testy tooensure hello kompatibility hello aktivity, které odkazují této propojené služby příliš[hello nejnovější podporované HDInsight verze](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) s informacemi o zdokumentovány v [Hadoop součásti, které jsou k dispozici s různými verzemi HDInsight](../hdinsight/hdinsight-component-versioning.md#hadoop-components-available-with-different-hdinsight-versions) a [poznámky související s HDInsight verze verzi Hortonworks](../hdinsight/hdinsight-component-versioning.md#hortonworks-release-notes-associated-with-hdinsight-versions).  
  * Po **07, 15 nebo 2017**, ujistěte se, pokud chcete výchozí nastavení toooverride hello explicitně zadáte hodnoty osType a verze. 

>[!Note]
>Azure Data Factory aktuálně nepodporuje clusterů HDInsight pomocí Azure Data Lake Store jako primární úložiště. Používání Azure Storage jako primární úložiště pro clustery služby HDInsight. 
>  
>  

## <a name="on-demand-compute-environment"></a>Na vyžádání výpočetním prostředí
V tomto typu konfigurace je výpočetní prostředí hello plně spravované službou Azure Data Factory hello. Je automaticky vytvořen hello služba Data Factory předtím, než je úloha data odeslaná tooprocess a odebrat po dokončení úlohy hello. Můžete vytvořit propojené služby pro hello na vyžádání výpočetním prostředí, konfiguraci a řídit granulární nastavení pro provádění úlohy, správu clusteru a zavádění akce.

> [!NOTE]
> konfigurace na vyžádání Hello v současné době podporuje pouze pro clustery služby Azure HDInsight.
> 
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Propojená služba Azure HDInsight na vyžádání
Hello služby Azure Data Factory můžete automaticky vytvoří systému Windows nebo Linux na vyžádání HDInsight tooprocess data clusteru. Hello clusteru je vytvořen ve stejné oblasti jako účet úložiště hello (vlastnost linkedServiceName v hello JSON) přidruženého k hello clusteru hello. účet úložiště Hello musí být účet úložiště Azure pro obecné účely úrovně standard. 

Všimněte si následujících hello **důležité** body o HDInsight na vyžádání propojené služby:

* Nevidíte hello na vyžádání vytvořit ve vašem předplatném Azure clusteru HDInsight. Hello služby Azure Data Factory slouží ke správě clusteru HDInsight na vyžádání hello vaším jménem.
* Hello protokoly pro úlohy, které se spouštějí na HDInsight na vyžádání se cluster zkopírovat toohello úložiště účet přidružený ke clusteru HDInsight hello. Tyto protokoly můžete přistupovat z hello portál Azure hello **podrobnosti o spuštění aktivit** okno. V tématu [monitorování a Správa kanálů](data-factory-monitor-manage-pipelines.md) článku.
* Budou se účtovat pouze pro hello čas, kdy clusteru HDInsight hello nahoru a spuštěné úlohy.

> [!IMPORTANT]
> Obvykle trvá **20 minut** nebo clusteru více tooprovision Azure HDInsight na vyžádání.
> 
> 

### <a name="example"></a>Příklad
Hello následující JSON definuje základě Linux na vyžádání propojené služby HDInsight. Hello služba Data Factory automaticky vytvoří **systémem Linux** clusteru HDInsight se při zpracování dat řezu. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

nastavit toouse cluster HDInsight se systémem Windows **osType** příliš**windows** nebo nepoužívejte hello vlastnost jako hello výchozí hodnota je: systému windows.  

> [!IMPORTANT]
> Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (**linkedServiceName**). HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello. Toto chování je záměrné. S na vyžádání propojené služby HDInsight, HDInsight cluster vytvoří pokaždé, když řez musí toobe zpracování, pokud neexistuje aktivní cluster (**timeToLive**) a po dokončení zpracování hello, se odstraní. 
> 
> Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů. Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady. vzor podle Hello názvy těchto kontejnerů: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.
> 
> 

### <a name="properties"></a>Vlastnosti
| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavená příliš**HDInsightOnDemand**. |Ano |
| Parametr ClusterSize |Počet uzlů pracovního procesu nebo data v clusteru hello. Vytvoření clusteru HDInsight Hello s 2 hlavních uzlech společně s hello počet uzlů pracovního procesu, který jste zadali pro tuto vlastnost. Hello uzly jsou velikosti Standard_D3, který má 4 jádra, 4 pracovní uzly clusteru trvá 24 jader (4\*4 = 16 jader pro uzly pracovního procesu, plus 2\*4 = 8 jader pro head uzly). V tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) podrobnosti o hello Standard_D3 vrstvy. |Ano |
| TimeToLive |Hello povolená doba nečinnosti hello clusteru HDInsight na vyžádání. Určuje, jak dlouho clusteru HDInsight na vyžádání hello zůstane aktivní po dokončení činnosti spustit, pokud v clusteru hello nejsou žádné aktivní úlohy.<br/><br/>Pokud aktivita spuštění trvá 6 minut a timetolive je třeba nastavit too5 minut, hello clusteru zůstane aktivní po dobu 5 minut po hello 6 minut zpracování hello aktivity při spuštění. Pokud jiné aktivity při spuštění je proveden s hello 6 minut okna, je zpracován hello stejného clusteru.<br/><br/>Vytvoření clusteru HDInsight na vyžádání je náročná operace (může trvat), takže toto nastavení použijte jako potřebné tooimprove výkonu objektu pro vytváření dat pomocí opakovaného použití clusteru HDInsight na vyžádání.<br/><br/>Pokud jste nastavili too0 hodnota timetolive, odstranění clusteru hello ihned po dokončení aktivity hello spustit. Vzhledem k tomu, pokud jste nastavili na vysokou hodnotu, může zůstat hello clusteru nečinnosti zbytečně výsledkem vysoké náklady. Proto je důležité nastavit hello odpovídající hodnotu na základě potřeb.<br/><br/>Pokud je hodnota vlastnosti timetolive hello správně nastavena, můžete sdílet více kanálů hello instanci clusteru HDInsight na vyžádání hello.  |Ano |
| Verze |Verze clusteru HDInsight hello. Hello výchozí hodnota je pro cluster systému Windows 3.1 a 3.2 pro cluster s Linuxem. |Ne |
| linkedServiceName | Propojená služba toobe používá hello cluster na vyžádání pro ukládání a zpracování dat Azure Storage. Hello se HDInsight cluster vytvoří v hello stejné oblasti jako účet úložiště Azure.<p>V současné době nelze vytvořit cluster HDInsight na vyžádání, která používá Azure Data Lake Store jako hello úložiště. Pokud chcete toostore hello Výsledná data z prostředí HDInsight zpracování v Azure Data Lake Store, pomocí aktivity kopírování toocopy hello dat z Azure Blob Storage toohello hello Azure Data Lake Store. </p>  | Ano |
| additionalLinkedServiceNames |Určuje, že další účty úložiště pro hello HDInsight propojená služba tak, aby služba Data Factory hello můžete zaregistrovat vaším jménem. Tyto účty úložiště musí být v hello stejné oblasti jako cluster HDInsight hello, který je vytvořen v hello stejné oblasti jako účet úložiště hello určeného linkedServiceName. |Ne |
| osType |Typ operačního systému. Povolené hodnoty jsou: systému Windows (výchozí) a Linux |Ne |
| hcatalogLinkedServiceName |Název Hello SQL Azure, propojené služby databázi HCatalog toohello bodu. cluster HDInsight na vyžádání Hello je vytvořená pomocí hello Azure SQL database jako hello metaúložiště. |Ne |

#### <a name="additionallinkedservicenames-json-example"></a>Příklad additionalLinkedServiceNames JSON

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Rozšířené vlastnosti
Můžete také zadat hello následující vlastnosti pro konfiguraci podrobné hello hello clusteru HDInsight na vyžádání.

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| coreConfiguration |Určuje parametry konfigurace základní hello (jako základní site.xml) pro toobe clusteru HDInsight hello vytvořili. |Ne |
| hBaseConfiguration |Určuje parametry konfigurace HBase (hbase-site.xml) hello pro hello HDInsight cluster. |Ne |
| hdfsConfiguration |Určuje parametry konfigurace HDFS (hdfs-site.xml) hello pro hello HDInsight cluster. |Ne |
| hiveConfiguration |Určuje parametry konfigurace hive (hive-site.xml) hello pro hello HDInsight cluster. |Ne |
| mapReduceConfiguration |Určuje parametry konfigurace MapReduce (mapred-site.xml) hello pro hello HDInsight cluster. |Ne |
| oozieConfiguration |Určuje parametry konfigurace Oozie (oozie-site.xml) hello pro hello HDInsight cluster. |Ne |
| stormConfiguration |Určuje parametry konfigurace Storm (storm-site.xml) hello pro hello HDInsight cluster. |Ne |
| yarnConfiguration |Určuje parametry konfigurace Yarn (yarn-site.xml) hello pro hello HDInsight cluster. |Ne |

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Příklad – konfigurace clusteru HDInsight na vyžádání pomocí rozšířených vlastností

```json
{
  "name": " HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "clusterSize": 16,
      "timeToLive": "01:30:00",
      "linkedServiceName": "adfods1",
      "coreConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "hiveConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "mapReduceConfiguration": {
        "mapreduce.reduce.java.opts": "-Xmx4000m",
        "mapreduce.map.java.opts": "-Xmx4000m",
        "mapreduce.map.memory.mb": "5000",
        "mapreduce.reduce.memory.mb": "5000",
        "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
      },
      "yarnConfiguration": {
        "yarn.app.mapreduce.am.resource.mb": "5000",
        "mapreduce.map.memory.mb": "5000"
      },
      "additionalLinkedServiceNames": [
        "datafeeds",
        "adobedatafeed"
      ]
    }
  }
}
```

### <a name="node-sizes"></a>Velikost uzlu
Zadávat lze velikosti hello head, data a zookeeper uzlů pomocí hello následující vlastnosti: 

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| headNodeSize |Určuje velikost hello hello hlavního uzlu. Hello výchozí hodnota je: Standard_D3. V tématu hello **určení velikosti uzlu** podrobnosti. |Ne |
| dataNodeSize |Určuje velikost hello hello datový uzel. Hello výchozí hodnota je: Standard_D3. |Ne |
| zookeeperNodeSize |Určuje velikost hello hello správce Zoo uzlu. Hello výchozí hodnota je: Standard_D3. |Ne |

#### <a name="specifying-node-sizes"></a>Určení velikosti uzlu
V tématu hello [velikosti virtuálních počítačů](../virtual-machines/linux/sizes.md) článku řetězcové hodnoty pro vlastnosti hello uvedeno v předchozí části hello potřebujete toospecify. Hello hodnoty se musí tooconform toohello **rutiny & rozhraní API** popsána v článku hello. Jak vidíte v článku hello, má hello datový uzel velikosti velké (výchozí) 7 GB paměti, která nemusí být dostatečně vhodné pro váš scénář. 

Pokud chcete toocreate D4 velikost head uzlů a uzlů pracovního procesu, zadejte **Standard_D4** hello hodnotu vlastnosti headNodeSize a dataNodeSize. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Pokud zadáte chybnou hodnotu pro tyto vlastnosti, může se zobrazit následující hello **Chyba:** toocreate clusteru se nezdařilo. Výjimka: Operace vytváření nemůže toocomplete hello clusteru. Operace se nezdařila, kód chyby je 400. Zanechaný stav clusteru: Chyba. Zpráva: 'PreClusterCreationValidationFailure'. Jakmile se zobrazí tato chyba, ujistěte se, že používáte hello **RUTINY & rozhraní API** názvem z tabulky hello v hello [velikosti virtuálních počítačů](../virtual-machines/linux/sizes.md) článku.  

## <a name="bring-your-own-compute-environment"></a>Výpočetní prostředí
V tomto typu konfigurace uživatelé mohou registrovat stávající výpočetní prostředí jako propojené služby ve službě Data Factory. výpočetní prostředí Hello spravuje hello uživatele a hello služba Data Factory používá, je tooexecute hello aktivity.

Tento typ konfigurace se podporuje pro hello výpočetním prostředí:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Databáze Azure SQL, Azure SQL DW, systému SQL Server

## <a name="azure-hdinsight-linked-service"></a>Propojená služba Azure HDInsight
Tooregister Azure HDInsight propojené služby můžete vytvořit vlastní cluster HDInsight s Data Factory.

### <a name="example"></a>Příklad

```json
{
  "name": "HDInsightLinkedService",
  "properties": {
    "type": "HDInsight",
    "typeProperties": {
      "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
      "userName": "admin",
      "password": "<password>",
      "linkedServiceName": "MyHDInsightStoragelinkedService"
    }
  }
}
```

### <a name="properties"></a>Vlastnosti
| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavená příliš**HDInsight**. |Ano |
| clusterUri |identifikátor URI clusteru HDInsight hello Hello. |Ano |
| uživatelské jméno |Zadejte název hello toobe uživatele hello používá tooconnect tooan stávajícího clusteru HDInsight. |Ano |
| heslo |Zadejte heslo pro uživatelský účet hello. |Ano |
| linkedServiceName | Název hello Azure Storage, propojené služby, která odkazuje úložiště objektů blob Azure toohello používá hello clusteru HDInsight. <p>V současné době nelze zadat, že Azure Data Lake Store propojené služby pro tuto vlastnost. Pokud hello HDInsight cluster má přístup toohello Data Lake Store, můžete může přistupovat k datům v Azure Data Lake Store hello z Hive nebo Pig skriptů. </p>  |Ano |

## <a name="azure-batch-linked-service"></a>Propojená služba Azure Batch
Můžete vytvořit Azure Batch propojené služby tooregister fondu služby Batch objektu pro vytváření dat tooa virtuální počítače (VM). Můžete spustit pomocí Azure Batch nebo Azure HDInsight vlastní aktivity .NET.

Najdete v následujících tématech, pokud jsou nová služba Batch tooAzure:

* [Základy Azure Batch](../batch/batch-technical-overview.md) přehled hello služby Azure Batch.
* [Nové AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) rutiny toocreate účtu Azure Batch (nebo) [portál Azure](../batch/batch-account-create-portal.md) účtu Azure Batch hello toocreate pomocí portálu Azure. V tématu [pomocí prostředí PowerShell toomanage účtu Azure Batch](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) téma pro podrobné pokyny k použití rutiny hello.
* [Nový-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) rutiny toocreate fondu Azure Batch.

### <a name="example"></a>Příklad

```json
{
  "name": "AzureBatchLinkedService",
  "properties": {
    "type": "AzureBatch",
    "typeProperties": {
      "accountName": "<Azure Batch account name>",
      "accessKey": "<Azure Batch account key>",
      "poolName": "<Azure Batch pool name>",
      "linkedServiceName": "<Specify associated storage linked service reference here>"
    }
  }
}
```

Připojit "**.\< Název oblasti\>**"toohello název vašeho účtu batch pro hello **accountName** vlastnost. Příklad:

```json
"accountName": "mybatchaccount.eastus"
```

Další možností je tooprovide hello batchUri koncový bod, jak je znázorněno v hello následující ukázka:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Vlastnosti
| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| type |vlastnost typu Hello musí být nastavená příliš**AzureBatch**. |Ano |
| název účtu |Název hello účtu Azure Batch. |Ano |
| accessKey |Přístupový klíč pro účet Azure Batch hello. |Ano |
| poolName |Název fondu hello virtuálních počítačů. |Ano |
| linkedServiceName |Název hello propojenou službu úložiště Azure přidruženého k této službě Azure Batch propojený. Tato propojená služba se používá pro pracovní soubory požadované toorun hello aktivity a ukládání hello protokoly provádění aktivity. |Ano |

## <a name="azure-machine-learning-linked-service"></a>Propojená služba Azure Machine Learning
Můžete vytvořit tooregister Azure Machine Learning propojené služby Machine Learning dávkového vyhodnocování koncový bod tooa data factory.

### <a name="example"></a>Příklad

```json
{
  "name": "AzureMLLinkedService",
  "properties": {
    "type": "AzureML",
    "typeProperties": {
      "mlEndpoint": "https://[batch scoring endpoint]/jobs",
      "apiKey": "<apikey>"
    }
  }
}
```

### <a name="properties"></a>Vlastnosti
| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| Typ |vlastnost typu Hello by měla být nastavena na: **AzureML**. |Ano |
| mlEndpoint |Hello dávkového vyhodnocování adresy URL. |Ano |
| apiKey |Hello publikovat rozhraní API modelu pracovního prostoru. |Ano |

## <a name="azure-data-lake-analytics-linked-service"></a>Propojená služba Azure Data Lake Analytics
Vytvoříte **Azure Data Lake Analytics** propojené služby toolink služby Azure Data Lake Analytics výpočetní služby tooan Azure data factory. Hello Data Lake Analytics U-SQL aktivitu v kanálu hello odkazuje toothis propojené služby. 

Hello následující tabulka obsahuje popis hello obecné vlastnosti používané ve hello definici JSON. Dále můžete mezi instanční objekt a ověření přihlašovacích údajů uživatele.

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| **Typ** |vlastnost typu Hello by měla být nastavena na: **AzureDataLakeAnalytics**. |Ano |
| **název účtu** |Název účtu Azure Data Lake Analytics. |Ano |
| **dataLakeAnalyticsUri** |Identifikátor URI služby Azure Data Lake Analytics. |Ne |
| **ID předplatného** |Id předplatného Azure |Ne (když není určeno předplatné hello se používá pro vytváření dat). |
| **Název skupiny prostředků** |Název skupiny prostředků Azure. |Ne (když není určeno skupiny prostředků hello se používá pro vytváření dat). |

### <a name="service-principal-authentication-recommended"></a>Objekt zabezpečení ověřování služby (doporučeno)
objekt zabezpečení ověřování služby toouse registrace entity aplikace v Azure Active Directory (Azure AD) a udělte ho hello přístup tooData Lake Store. Podrobné pokyny najdete v tématu [Service-to-service ověřování](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Poznamenejte si hello následující hodnoty, které používáte toodefine hello propojené služby:
* ID aplikace
* Klíč aplikace 
* ID tenanta

Objekt zabezpečení ověřování služby použijte zadáním hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| **servicePrincipalId** | Zadejte ID aplikace hello klienta. | Ano |
| **servicePrincipalKey** | Zadejte klíč aplikace hello. | Ano |
| **klienta** | Zadejte informace klienta hello (název nebo klienta domény ID) v rámci které se nachází aplikace. Můžete jej načíst po výběru ukázáním hello myši v pravém horním rohu hello hello portálu Azure. | Ano |

**Příkladu: Ověření objektu službu**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Ověření pověření uživatele
Alternativně můžete použít ověřování pověření uživatele pro Data Lake Analytics zadáním hello následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
|:--- |:--- |:--- |
| **autorizace** | Klikněte na tlačítko hello **Autorizovat** tlačítka na hello editoru služby Data Factory a zadejte svoje přihlašovací údaje, který přiřazuje hello automaticky vygenerovanou autorizace URL toothis vlastnost. | Ano |
| **ID relace** | ID relace OAuth z autorizační relace, hello OAuth. Každé ID relace je jedinečné a může být použit pouze jednou. Toto nastavení se automaticky generuje při použití hello editoru služby Data Factory. | Ano |

**Příklad: Ověření pověření uživatele**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Vypršení platnosti tokenu
Hello autorizační kód vygenerovaného s využitím hello **Autorizovat** tlačítko vyprší po určité době. Viz následující tabulka pro hello vypršení platnosti časy pro různé typy uživatelských účtů hello. Zobrazí následující chybová zpráva hello při hello ověřování **platnost tokenu vyprší**: přihlašovací údaje chyby operace: invalid_grant - AADSTS70002: Chyba při ověřování přihlašovacích údajů. AADSTS70008: hello údaje udělení přístupu vypršela platnost nebo odvolat. ID trasování: ID korelace d18629e8-af88-43c5-88e3-d8419eb1fca1: časové razítko fac30a0c-6be6-4e02-8d69-a776d2ffefd7: 2015-12-15 21:09:31Z

| Typ uživatele | Platnost vyprší po |
|:--- |:--- |
| Uživatelské účty, které nejsou spravované přes Azure Active Directory (@hotmail.com, @live.comatd.) |12 hodin |
| Účty uživatelů spravované pomocí Azure Active Directory (AAD) |14 dnů po poslední řez hello spustit. <br/><br/>90 dnů, pokud řezu založeného na základě OAuth propojené služby používá alespoň jednou za 14 dní. |

tooavoid nebo vyřešit tuto chybu, opětovné pověření pomocí hello **Authorize** když hello **platnost tokenu vyprší** a znovu nasaďte hello propojené služby. Můžete také vygenerovat hodnoty pro **sessionId** a **autorizace** vlastnosti programově pomocí kódu následujícím způsobem:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

V tématu [azuredatalakestorelinkedservice třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), a [AuthorizationSessionGetResponse třída](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témata podrobnosti o třídách Data Factory hello používá v kódu hello. Přidat odkaz na: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll pro hello WindowsFormsWebAuthenticationDialog třídy. 

## <a name="azure-sql-linked-service"></a>Propojená služba Azure SQL
Vytvoření služby Azure SQL propojené a jeho použití s hello [aktivity uložené procedury](data-factory-stored-proc-activity.md) tooinvoke uložené procedury z objektu pro vytváření dat kanál. V tématu [konektor služby Azure SQL](data-factory-azure-sql-connector.md#linked-service-properties) článku podrobnosti o této propojené službě.

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL datového skladu propojené služby
Vytvoření služby Azure SQL Data Warehouse propojené a jeho použití s hello [aktivity uložené procedury](data-factory-stored-proc-activity.md) tooinvoke uložené procedury z objektu pro vytváření dat kanál. V tématu [konektor služby Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) článku podrobnosti o této propojené službě.

## <a name="sql-server-linked-service"></a>Propojená služba systému SQL Server
Vytvoření služby SQL serveru propojená a jeho použití s hello [aktivity uložené procedury](data-factory-stored-proc-activity.md) tooinvoke uložené procedury z objektu pro vytváření dat kanál. V tématu [systému SQL Server konektoru](data-factory-sqlserver-connector.md#linked-service-properties) článku podrobnosti o této propojené službě.

