---
title: 'Transformaci dat: Proces & transformace dat | Microsoft Docs'
description: "Zjistěte, jak tootransform dat nebo zpracování dat v Azure Data Factory pomocí Hadoop, Machine Learning nebo Azure Data Lake Analytics."
keywords: "transformace dat, zpracování dat, transformovat data aktivity transformace"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 39786731-1e4b-40a4-81b7-d06e127427aa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 917d617259699b0e71de3a0e0c17463d00f2e0a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-in-azure-data-factory"></a>Transformace dat v Azure Data Factory
> [!div class="op_single_selector"]
> * [Hive](data-factory-hive-activity.md)  
> * [Pig](data-factory-pig-activity.md)  
> * [MapReduce](data-factory-map-reduce.md)  
> * [Streamování Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
> * [Uložená procedura](data-factory-stored-proc-activity.md)
> * [U-SQL Data Lake Analytics](data-factory-usql-activity.md)
> * [Vlastní rozhraní .NET](data-factory-use-custom-activities.md)

## <a name="overview"></a>Přehled
Tento článek vysvětluje aktivit transformace dat v Azure Data Factory můžete použít tootransform a zpracuje nezpracovaná data do předpovědi a statistiky. Transformace aktivity spustí výpočetní prostředí jako je cluster Azure HDInsight nebo Azure Batch. Podrobné informace o každé aktivity transformace poskytne tooarticles odkazy.

Objekt pro vytváření dat podporuje následující aktivit transformace dat, které mohou být přidány příliš hello[kanály](data-factory-create-pipelines.md) buď jednotlivě nebo zřetězené s jinou aktivitou.

> [!NOTE]
> Návod s podrobné pokyny najdete v tématu [vytvoření kanálu s transformace Hive](data-factory-build-your-first-pipeline.md) článku.  
> 
> 

## <a name="hdinsight-hive-activity"></a>Aktivitu HDInsight Hive
Hello aktivitu HDInsight Hive v kanálu pro vytváření dat provede dotazů Hive sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání. V tématu [Hive aktivity](data-factory-hive-activity.md) článku podrobnosti o této aktivitě. 

## <a name="hdinsight-pig-activity"></a>Aktivita HDInsight Pig
Hello HDInsight Pig aktivity v kanálu pro vytváření dat provede Pig dotazy na vlastní nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání. V tématu [Pig aktivity](data-factory-pig-activity.md) článku podrobnosti o této aktivitě. 

## <a name="hdinsight-mapreduce-activity"></a>Činnost MapReduce s HDInsight
Hello činnost HDInsight MapReduce v objektu pro vytváření dat kanál provede MapReduce programy sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání. V tématu [činnost MapReduce](data-factory-map-reduce.md) článku podrobnosti o této aktivitě.

## <a name="hdinsight-streaming-activity"></a>HDInsight streamované aktivitě
Hello HDInsight streamované aktivitě v objektu pro vytváření dat kanál provede streamování Hadoop programy sami nebo clusteru HDInsight se systémem Windows nebo Linux na vyžádání. V tématu [HDInsight streamované aktivitě](data-factory-hadoop-streaming-activity.md) podrobnosti o této aktivitě.

## <a name="hdinsight-spark-activity"></a>Aktivita Spark služby HDInsight
Hello aktivity HDInsight Spark v objektu pro vytváření dat kanál provede na clusteru HDInsight Spark programy. Podrobnosti najdete v tématu [vyvolání Spark programy z Azure Data Factory](data-factory-spark.md). 

## <a name="machine-learning-activities"></a>Machine Learning aktivity
Azure Data Factory umožňuje tooeasily můžete vytvořit kanály, které používají publikované Azure Machine Learning webová služba pro prediktivní analýzy. Pomocí hello [aktivita provedení dávky](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) v kanál služby Azure Data Factory můžete vyvolat Machine Learning webové služby toomake předpovědi hello data v dávce.

V průběhu času hello prediktivní modely v hello Machine Learning vyhodnocování experimenty potřebovat toobe retrained pomocí nové vstupní datové sady. Po dokončení práce s retraining, budete chtít tooupdate hello vyhodnocování webové služby s hello retrained model Machine Learning. Můžete použít hello [aktivita prostředku aktualizace](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) tooupdate hello webové služby s nově trained model hello.  

V tématu [použití Machine Learning aktivity](data-factory-azure-ml-batch-execution-activity.md) podrobnosti o tyto aktivity Machine Learning. 

## <a name="stored-procedure-activity"></a>Aktivita uložené procedury
Aktivita hello uloženou proceduru SQL Server můžete použít v objektu pro vytváření dat kanál tooinvoke uložené procedury v jednom z hello následující úložišť dat: databáze SQL Azure, Azure SQL Data Warehouse, databáze SQL serveru ve vašem podniku nebo virtuální počítač Azure. V tématu [aktivity uložené procedury](data-factory-stored-proc-activity.md) článku.  

## <a name="data-lake-analytics-u-sql-activity"></a>Aktivita Data Lake Analytics U-SQL
Data Lake Analytics U-SQL aktivity spouští skript U-SQL na clusteru služby Azure Data Lake Analytics. V tématu [Data Analytics U-SQL aktivity](data-factory-usql-activity.md) článku. 

## <a name="net-custom-activity"></a>Vlastní aktivita .NET
Pokud potřebujete tootransform data způsobem, který není podporován službou Data Factory, můžete vytvořit vlastní aktivity s vlastní logikou zpracování dat a používat hello aktivity v kanálu hello. Můžete nakonfigurovat hello vlastní .NET aktivity toorun pomocí služby Azure Batch nebo clusteru Azure HDInsight. V tématu [použít vlastní aktivity](data-factory-use-custom-activities.md) článku. 

Můžete vytvořit vlastní aktivity skriptů toorun R na clusteru HDInsight s R nainstalované. Viz [Spuštění skriptu jazyka R pomocí služby Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Výpočetní prostředí
Vytvoření propojené služby pro hello výpočetní prostředí a potom pomocí hello propojené služby, při definování aktivitou transformace. Existují dva typy výpočetní prostředí podporovaných službou Data Factory. 

1. **Na vyžádání**: V tomto případě je plně spravovaná výpočetní prostředí hello službou Data Factory. Je automaticky vytvořen hello služba Data Factory předtím, než je úloha data odeslaná tooprocess a odebrat po dokončení úlohy hello. Můžete konfigurovat a řídit granulární nastavení hello na vyžádání výpočetním prostředí pro spuštění úlohy, správu clusteru a zavádění akce. 
2. **Přineste si vlastní**: V tomto případě můžete zaregistrovat vlastní výpočetní prostředí (například cluster HDInsight) jako propojené služby ve službě Data Factory. výpočetní prostředí Hello je spravovaná vámi a hello služba Data Factory používá, je tooexecute hello aktivity. 

V tématu [propojené výpočetní služby](data-factory-compute-linked-services.md) toolearn článek o výpočetních služeb podporovaných službou Data Factory. 

## <a name="summary"></a>Souhrn
Azure Data Factory podporuje hello následující aktivit transformace dat a hello výpočetních prostředích hello aktivity. aktivity transformace Hello může být přidané toopipelines buď jednotlivě nebo zřetězené s jinou aktivitou.

| Aktivita transformace dat | Výpočetní prostředí |
|:--- |:--- |
| [Hive](data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Streamování Hadoop](data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Aktivity Machine Learning: Dávkové spouštění a Aktualizace prostředku](data-factory-azure-ml-batch-execution-activity.md) |Virtuální počítač Azure |
| [Uložená procedura](data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Data Warehouse nebo SQL Server |
| [U-SQL Data Lake Analytics](data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [DotNet](data-factory-use-custom-activities.md) |HDInsight [Hadoop] nebo Azure Batch |

