---
title: "Prozkoumejte data v tabulkách Hive pomocí dotazů Hive | Microsoft Docs"
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
ms.openlocfilehash: 67a33a9abc3d3dcdd2fc7205e11feff97e3582a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a><span data-ttu-id="6cce8-103">Zkoumání dat v tabulkách Hivu pomocí dotazů Hivu</span><span class="sxs-lookup"><span data-stu-id="6cce8-103">Explore data in Hive tables with Hive queries</span></span>
<span data-ttu-id="6cce8-104">Tento dokument obsahuje ukázkové skripty Hive, které se používají k zkoumat data v tabulkách Hive v clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="6cce8-104">This document provides sample Hive scripts that are used to explore data in Hive tables in an HDInsight Hadoop cluster.</span></span>

<span data-ttu-id="6cce8-105">Následující **nabídky** odkazy na témata, které popisují, jak používat nástroje a prozkoumejte data z různých prostředích úložiště.</span><span class="sxs-lookup"><span data-stu-id="6cce8-105">The following **menu** links to topics that describe how to use tools to explore data from various storage environments.</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="6cce8-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6cce8-106">Prerequisites</span></span>
<span data-ttu-id="6cce8-107">Tento článek předpokládá, že máte:</span><span class="sxs-lookup"><span data-stu-id="6cce8-107">This article assumes that you have:</span></span>

* <span data-ttu-id="6cce8-108">Vytvořit účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6cce8-108">Created an Azure storage account.</span></span> <span data-ttu-id="6cce8-109">Pokud budete potřebovat pokyny, najdete v části [vytvoření účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="6cce8-109">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="6cce8-110">Zřizuje přizpůsobené clusteru Hadoop se službou HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6cce8-110">Provisioned a customized Hadoop cluster with the HDInsight service.</span></span> <span data-ttu-id="6cce8-111">Pokud budete potřebovat pokyny, najdete v části [přizpůsobit Azure HDInsight Hadoop clusterů pro pokročilé analýzy](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="6cce8-111">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="6cce8-112">Data byla uložena do tabulek Hive v Azure HDInsight Hadoop clusterů.</span><span class="sxs-lookup"><span data-stu-id="6cce8-112">The data has been uploaded to Hive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="6cce8-113">Pokud ne, postupujte podle pokynů v [vytvoření a načtení dat do tabulek Hive](machine-learning-data-science-move-hive-tables.md) nejprve nahrát data do tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="6cce8-113">If it has not, follow the instructions in [Create and load data to Hive tables](machine-learning-data-science-move-hive-tables.md) to upload data to Hive tables first.</span></span>
* <span data-ttu-id="6cce8-114">Povolit pro vzdálený přístup ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="6cce8-114">Enabled remote access to the cluster.</span></span> <span data-ttu-id="6cce8-115">Pokud budete potřebovat pokyny, najdete v části [přístup hlavního uzlu Hadoop clusteru](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="6cce8-115">If you need instructions, see [Access the Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>
* <span data-ttu-id="6cce8-116">Pokud budete potřebovat pokyny k odesílání dotazů Hive, najdete v části [postup odesílání dotazů Hive](machine-learning-data-science-move-hive-tables.md#submit)</span><span class="sxs-lookup"><span data-stu-id="6cce8-116">If you need instructions on how to submit Hive queries, see [How to Submit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit)</span></span>

## <a name="example-hive-query-scripts-for-data-exploration"></a><span data-ttu-id="6cce8-117">Příklad Hive dotaz skripty pro zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="6cce8-117">Example Hive query scripts for data exploration</span></span>
1. <span data-ttu-id="6cce8-118">Získat počet připomínky na oddíly`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span><span class="sxs-lookup"><span data-stu-id="6cce8-118">Get the count of observations per partition  `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span></span>
2. <span data-ttu-id="6cce8-119">Získat počet připomínky za den`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span><span class="sxs-lookup"><span data-stu-id="6cce8-119">Get the count of observations per day  `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span></span>
3. <span data-ttu-id="6cce8-120">Získat úrovně ve sloupci kategorií</span><span class="sxs-lookup"><span data-stu-id="6cce8-120">Get the levels in a categorical column</span></span>  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. <span data-ttu-id="6cce8-121">Získat počet úrovní v kombinaci dvou kategorií sloupců`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span><span class="sxs-lookup"><span data-stu-id="6cce8-121">Get the number of levels in combination of two categorical columns  `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span></span>
5. <span data-ttu-id="6cce8-122">Získat distribuce pro číselné sloupce</span><span class="sxs-lookup"><span data-stu-id="6cce8-122">Get the distribution for numerical columns</span></span>  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. <span data-ttu-id="6cce8-123">Extrahování záznamů z spojení dvou tabulek</span><span class="sxs-lookup"><span data-stu-id="6cce8-123">Extract records from joining two tables</span></span>
   
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

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a><span data-ttu-id="6cce8-124">Další dotaz skripty pro scénáře datových taxíkem cesty</span><span class="sxs-lookup"><span data-stu-id="6cce8-124">Additional query scripts for taxi trip data scenarios</span></span>
<span data-ttu-id="6cce8-125">Příklady dotazů, které jsou specifické pro [NYC taxíkem cestě Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scénáře jsou také uvedeny v [úložiště GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="6cce8-125">Examples of queries that are specific to [NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="6cce8-126">Tyto dotazy už máte zadané schéma data a jsou připravené k odeslání do spustit.</span><span class="sxs-lookup"><span data-stu-id="6cce8-126">These queries already have data schema specified and are ready to be submitted to run.</span></span>

