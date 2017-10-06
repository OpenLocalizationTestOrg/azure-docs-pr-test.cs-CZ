---
title: "aaaExplore data v tabulkách Hive pomocí dotazů Hive | Microsoft Docs"
description: "Prozkoumejte data v tabulkách Hive pomocí dotazů Hive."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0d46cea5-2b4c-4384-9bfa-fa20f6f75148
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 2ede3d41682aa08ced19284f7a83ec95e0c2a93a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Zkoumání dat v tabulkách Hivu pomocí dotazů Hivu
Tento dokument obsahuje ukázkové skripty Hive, které jsou používané tooexplore data v tabulkách Hive v clusteru služby HDInsight Hadoop.

Následující Hello **nabídky** odkazy tootopics, které popisují, jak toouse nástroje tooexplore data z různých prostředích úložiště.

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že máte:

* Vytvořit účet úložiště Azure. Pokud budete potřebovat pokyny, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Zřizuje přizpůsobené clusteru Hadoop s hello služba HDInsight. Pokud budete potřebovat pokyny, najdete v části [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilé analýzy](machine-learning-data-science-customize-hadoop-cluster.md).
* Hello data nebyla nahrané tooHive tabulek v Azure HDInsight Hadoop clusterů. Pokud ne, postupujte podle pokynů hello v [vytvořit a zatížení tabulky dat tooHive](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive první tabulky.
* Povolit clusteru toohello vzdáleného přístupu. Pokud budete potřebovat pokyny, najdete v části [přístup hello uzlu clusteru Hadoop Head](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* Pokud budete potřebovat pokyny najdete v části dotazů Hive toosubmit, [jak tooSubmit dotazů Hive](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Příklad Hive dotaz skripty pro zkoumání dat
1. Získat hello počet připomínky na oddíly`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Získat hello počet připomínky za den`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. Získat hello úrovně ve sloupci kategorií  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. Získat hello počet úrovní v kombinaci dvou kategorií sloupců`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. Získat hello distribuce pro číselné sloupce  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. Extrahování záznamů z spojení dvou tabulek
   
        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Další dotaz skripty pro scénáře datových taxíkem cesty
Příklady dotazů, které jsou specifické příliš[NYC taxíkem cestě Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scénáře jsou také uvedeny v [úložiště GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Tyto dotazy už máte zadané schéma data a jsou připravené toobe odeslána toorun.

