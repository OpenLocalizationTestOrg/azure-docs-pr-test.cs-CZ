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
# <a name="explore-data-in-hive-tables-with-hive-queries"></a><span data-ttu-id="10c0d-103">Zkoumání dat v tabulkách Hivu pomocí dotazů Hivu</span><span class="sxs-lookup"><span data-stu-id="10c0d-103">Explore data in Hive tables with Hive queries</span></span>
<span data-ttu-id="10c0d-104">Tento dokument obsahuje ukázkové skripty Hive, které jsou používané tooexplore data v tabulkách Hive v clusteru služby HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="10c0d-104">This document provides sample Hive scripts that are used tooexplore data in Hive tables in an HDInsight Hadoop cluster.</span></span>

<span data-ttu-id="10c0d-105">Následující Hello **nabídky** odkazy tootopics, které popisují, jak toouse nástroje tooexplore data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="10c0d-105">hello following **menu** links tootopics that describe how toouse tools tooexplore data from various storage environments.</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="10c0d-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="10c0d-106">Prerequisites</span></span>
<span data-ttu-id="10c0d-107">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="10c0d-107">This article assumes that you have:</span></span>

* <span data-ttu-id="10c0d-108">Vytvořit účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="10c0d-108">Created an Azure storage account.</span></span> <span data-ttu-id="10c0d-109">Pokud budete potřebovat pokyny, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="10c0d-109">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="10c0d-110">Zřizuje přizpůsobené clusteru Hadoop s hello služba HDInsight.</span><span class="sxs-lookup"><span data-stu-id="10c0d-110">Provisioned a customized Hadoop cluster with hello HDInsight service.</span></span> <span data-ttu-id="10c0d-111">Pokud budete potřebovat pokyny, najdete v části [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilé analýzy](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="10c0d-111">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="10c0d-112">Hello data nebyla nahrané tooHive tabulek v Azure HDInsight Hadoop clusterů.</span><span class="sxs-lookup"><span data-stu-id="10c0d-112">hello data has been uploaded tooHive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="10c0d-113">Pokud ne, postupujte podle pokynů hello v [vytvořit a zatížení tabulky dat tooHive](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive první tabulky.</span><span class="sxs-lookup"><span data-stu-id="10c0d-113">If it has not, follow hello instructions in [Create and load data tooHive tables](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive tables first.</span></span>
* <span data-ttu-id="10c0d-114">Povolit clusteru toohello vzdáleného přístupu.</span><span class="sxs-lookup"><span data-stu-id="10c0d-114">Enabled remote access toohello cluster.</span></span> <span data-ttu-id="10c0d-115">Pokud budete potřebovat pokyny, najdete v části [přístup hello uzlu clusteru Hadoop Head](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="10c0d-115">If you need instructions, see [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>
* <span data-ttu-id="10c0d-116">Pokud budete potřebovat pokyny najdete v části dotazů Hive toosubmit, [jak tooSubmit dotazů Hive](machine-learning-data-science-move-hive-tables.md#submit)</span><span class="sxs-lookup"><span data-stu-id="10c0d-116">If you need instructions on how toosubmit Hive queries, see [How tooSubmit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit)</span></span>

## <a name="example-hive-query-scripts-for-data-exploration"></a><span data-ttu-id="10c0d-117">Příklad Hive dotaz skripty pro zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="10c0d-117">Example Hive query scripts for data exploration</span></span>
1. <span data-ttu-id="10c0d-118">Získat hello počet připomínky na oddíly`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span><span class="sxs-lookup"><span data-stu-id="10c0d-118">Get hello count of observations per partition  `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span></span>
2. <span data-ttu-id="10c0d-119">Získat hello počet připomínky za den`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span><span class="sxs-lookup"><span data-stu-id="10c0d-119">Get hello count of observations per day  `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span></span>
3. <span data-ttu-id="10c0d-120">Získat hello úrovně ve sloupci kategorií</span><span class="sxs-lookup"><span data-stu-id="10c0d-120">Get hello levels in a categorical column</span></span>  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. <span data-ttu-id="10c0d-121">Získat hello počet úrovní v kombinaci dvou kategorií sloupců`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span><span class="sxs-lookup"><span data-stu-id="10c0d-121">Get hello number of levels in combination of two categorical columns  `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span></span>
5. <span data-ttu-id="10c0d-122">Získat hello distribuce pro číselné sloupce</span><span class="sxs-lookup"><span data-stu-id="10c0d-122">Get hello distribution for numerical columns</span></span>  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. <span data-ttu-id="10c0d-123">Extrahování záznamů z spojení dvou tabulek</span><span class="sxs-lookup"><span data-stu-id="10c0d-123">Extract records from joining two tables</span></span>
   
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

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a><span data-ttu-id="10c0d-124">Další dotaz skripty pro scénáře datových taxíkem cesty</span><span class="sxs-lookup"><span data-stu-id="10c0d-124">Additional query scripts for taxi trip data scenarios</span></span>
<span data-ttu-id="10c0d-125">Příklady dotazů, které jsou specifické příliš[NYC taxíkem cestě Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scénáře jsou také uvedeny v [úložiště GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="10c0d-125">Examples of queries that are specific too[NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="10c0d-126">Tyto dotazy už máte zadané schéma data a jsou připravené toobe odeslána toorun.</span><span class="sxs-lookup"><span data-stu-id="10c0d-126">These queries already have data schema specified and are ready toobe submitted toorun.</span></span>

