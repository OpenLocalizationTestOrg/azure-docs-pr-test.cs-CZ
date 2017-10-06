---
title: "aaaIntegrating Data Lake Store s jinými službami Azure | Microsoft Docs"
description: "Pochopit, jak Data Lake Store se integruje s jinými službami Azure"
documentationcenter: 
services: data-lake-store
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 48a5d1f4-3850-4c22-bbc4-6d1d394fba8a
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 839689f04623f8225884e7aa9c0b533a09323e80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-data-lake-store-with-other-azure-services"></a>Integrace Data Lake Store s ostatními službami Azure
Azure Data Lake Store můžete použít ve spojení s další služby Azure tooenable širší rozsah scénáře. Hello následující článek obsahuje seznam hello služby, které se dá integrovat Data Lake Store.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Použití Data Lake Store s Azure HDInsight
Můžete zřídit [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) clusteru, který používá jako hello HDFS kompatibilní úložiště Data Lake Store. Pro tuto verzi pro clustery Hadoop a Storm v systému Windows a Linux, můžete Data Lake Store pouze jako další úložiště. Tyto clustery dál používat Azure Storage (WASB) jako výchozí úložiště hello. Clustery HBase v systému Windows a Linux, ale můžete Data Lake Store jako hello výchozí úložiště nebo další úložiště.

Pokyny jak clusteru tooprovision HDInsight s Data Lake Store najdete v části:

* [Zřízení clusteru HDInsight s Data Lake Store pomocí portálu Azure](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Zřízení clusteru HDInsight s Data Lake Store jako výchozí úložiště pomocí prostředí Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Zřízení clusteru HDInsight s Data Lake Store jako další úložiště pomocí prostředí Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)

## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Použití Data Lake Store s Azure Data Lake Analytics
[Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) vám umožní toowork s velkým objemem dat v cloudovém měřítku. Dynamicky Zřizuje prostředky a umožňuje provádět analýzy terabajtů nebo dokonce exabajtů dat., které můžou být uložené v počtu podporovaných zdrojů dat, jeden z nich se Data Lake Store. Data Lake Analytics je speciálně optimalizovaný toowork s Azure Data Lake Store – poskytuje hello nejvyšší úroveň výkonu, propustnosti a paralelizace pro úlohy big data.

Pokyny najdete v části toouse Data Lake Analytics s Data Lake Store, [Začínáme s Data Lake Analytics pomocí Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="use-data-lake-store-with-azure-data-factory"></a>Použití Data Lake Store pomocí Azure Data Factory.
Můžete použít [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) tooingest data z tabulky Azure, Azure SQL Database, datový sklad SQL Azure, objektů BLOB služby Azure Storage a místní databáze. S občanstvím první třídy v hello ekosystému Azure, Azure Data Factory lze použít tooorchestrate hello přijímání dat z těchto tooAzure zdroje Data Lake Store.

Pokyny, jak toouse Azure Data Factory s Data Lake Store najdete v části [přesunout tooand dat z Data Lake Store pomocí služby Data Factory](../data-factory/data-factory-azure-datalake-connector.md).

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Ke zkopírování dat z úložiště objektů BLOB Azure do Data Lake Store
Azure Data Lake Store poskytuje nástroj příkazového řádku, AdlCopy, který umožňuje toocopy dat z Azure Blob Storage do účtu Data Lake Store. Další informace najdete v tématu [kopírování dat z Azure úložiště objektů BLOB tooData Lake Store](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Kopírovat data mezi Azure SQL Database a Data Lake Store
Můžete použít Apache Sqoop tooimport a export dat mezi Azure SQL Database a Data Lake Store. Další informace najdete v tématu [kopírovat data mezi Data Lake Store a Azure SQL database pomocí Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

## <a name="use-data-lake-store-with-stream-analytics"></a>Použití Data Lake Store pomocí služby Stream Analytics
Data Lake Store můžete použít, protože jeden z hello odesílá výstupní data toostore streamování pomocí služby Azure Stream Analytics. Další informace najdete v tématu [Streamovat data z Azure Storage Blob do Data Lake Store pomocí Azure Stream Analytics](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Použití Data Lake Store s Power BI
Můžete pomocí Power BI tooimport dat z tooanalyze účet Data Lake Store a vizualizovat hello data. Další informace najdete v tématu [v Data Lake Store pomocí Power BI analyzovat data](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Použití Data Lake Store pomocí katalogu Data Catalog
Můžete zaregistrovat dat z Data Lake Store do hello Azure Data Catalog toomake hello data zjistitelný v rámci organizace hello. Další informace najdete v části [zaregistrovat v Azure Data Catalog dat z Data Lake Store](data-lake-store-with-data-catalog.md).

## <a name="use-data-lake-store-with-sql-server-integration-services-ssis"></a>Použití Data Lake Store s SQL Server Integration Services (služby SSIS)
Správce připojení hello Azure Data Lake Store můžete použít v SSIS tooconnect balíčku služby SSIS s Azure Data Lake Store. Další informace najdete v tématu [použití Data Lake Store s SSIS](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager).

## <a name="use-data-lake-store-with-sql-data-warehouse"></a>Použití Data Lake Store s SQL Data Warehouse
Můžete použít PolyBase tooload dat z Azure Data Lake Store do SQL Data Warehouse. Další informace najdete v části [použití Data Lake Store s SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="see-also"></a>Viz také
* [Přehled Azure Data Lake Store](data-lake-store-overview.md)
* [Začínáme s Data Lake Store pomocí portálu](data-lake-store-get-started-portal.md)
* [Začínáme s Data Lake Store pomocí prostředí PowerShell](data-lake-store-get-started-powershell.md)  

