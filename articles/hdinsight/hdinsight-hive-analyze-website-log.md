---
title: "Použijte Hive s Hadoop pro analýzu protokolu Web - Azure HDInsight | Microsoft Docs"
description: "Další informace o použití Hive s HDInsight k analýze webových protokolů. Budete používat soubor protokolu jako vstup do tabulky HDInsight a použít HiveQL k dotazování data."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6fb7b5c2-8df4-40b1-a9e2-6815080004f9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: e1cdb786bb6049980aafc0213abf53013e342618
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-hive-with-windows-based-hdinsight-to-analyze-logs-from-websites"></a><span data-ttu-id="cb6ae-104">Použijte Hive s HDInsight se systémem Windows k analýze protokolů z webů</span><span class="sxs-lookup"><span data-stu-id="cb6ae-104">Use Hive with Windows-based HDInsight to analyze logs from websites</span></span>
<span data-ttu-id="cb6ae-105">Zjistěte, jak použít HiveQL v prostředí HDInsight k analýze protokolů z webu.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-105">Learn how to use HiveQL with HDInsight to analyze logs from a website.</span></span> <span data-ttu-id="cb6ae-106">Analýza protokolu webu můžete použít, segmentovat cílovou skupinu podle podobné aktivity, kategorizace návštěvníky podle demografické údaje a zjistit, obsah se zobrazení, webů, které pocházejí z a tak dále.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-106">Website log analysis can be used to segment your audience based on similar activities, categorize site visitors by demographics, and to find out the content they view, the websites they come from, and so on.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cb6ae-107">Kroky v tomto dokumentu pracovat pouze s clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-107">The steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="cb6ae-108">HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-108">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="cb6ae-109">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="cb6ae-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="cb6ae-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="cb6ae-111">V této ukázce použijete clusteru služby HDInsight k analýze webu soubory protokolu pro získání přehledu o četnosti návštěv k webu z externích webových stránek za den.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-111">In this sample, you will use an HDInsight cluster to analyze website log files to get insight into the frequency of visits to the website from external websites in a day.</span></span> <span data-ttu-id="cb6ae-112">Pokud vytvoříte souhrn chyb na webových stránkách, kterými se uživatelé setkávají.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-112">You'll also generate a summary of website errors that the users experience.</span></span> <span data-ttu-id="cb6ae-113">Se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="cb6ae-113">You will learn how to:</span></span>

* <span data-ttu-id="cb6ae-114">Připojte k Azure Blob storage, který obsahuje soubory protokolu webové stránky.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-114">Connect to a Azure Blob storage, which contains website log files.</span></span>
* <span data-ttu-id="cb6ae-115">Vytváření tabulek HIVE k dotazování tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-115">Create HIVE tables to query those logs.</span></span>
* <span data-ttu-id="cb6ae-116">Vytváření dotazů HIVE analyzovat data.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-116">Create HIVE queries to analyze the data.</span></span>
* <span data-ttu-id="cb6ae-117">V aplikaci Microsoft Excel pro připojení k HDInsight (pomocí připojení k databázi otevřít (ODBC) pro načtení analyzovaná data.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-117">Use Microsoft Excel to connect to HDInsight (by using open database connectivity (ODBC) to retrieve the analyzed data.</span></span>

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

## <a name="prerequisites"></a><span data-ttu-id="cb6ae-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cb6ae-119">Prerequisites</span></span>
* <span data-ttu-id="cb6ae-120">Musí být zřízená clusteru Hadoop v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-120">You must have provisioned a Hadoop cluster on Azure HDInsight.</span></span> <span data-ttu-id="cb6ae-121">Pokyny najdete v tématu [zřizování clusterů HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="cb6ae-121">For instructions, see [Provision HDInsight Clusters][hdinsight-provision].</span></span>
* <span data-ttu-id="cb6ae-122">Musíte mít aplikaci Microsoft Excel 2013 nebo nainstalovat aplikaci Excel 2010.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-122">You must have Microsoft Excel 2013 or Excel 2010 installed.</span></span>
* <span data-ttu-id="cb6ae-123">Musíte mít [ovladače ODBC Microsoft Hive](http://www.microsoft.com/download/details.aspx?id=40886) k importu dat z podregistru do aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-123">You must have [Microsoft Hive ODBC Driver](http://www.microsoft.com/download/details.aspx?id=40886) to import data from Hive into Excel.</span></span>

## <a name="to-run-the-sample"></a><span data-ttu-id="cb6ae-124">Ke spuštění ukázky</span><span class="sxs-lookup"><span data-stu-id="cb6ae-124">To run the sample</span></span>
1. <span data-ttu-id="cb6ae-125">Z [portálu Azure](https://portal.azure.com/), z úvodního panelu (Pokud je připnutý clusteru existuje), klikněte na dlaždici clusteru, na kterém chcete spustit ukázku.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-125">From the [Azure Portal](https://portal.azure.com/), from the Startboard (if you pinned the cluster there), click the cluster tile on which you want to run the sample.</span></span>
2. <span data-ttu-id="cb6ae-126">V okně clusteru pod **rychlé odkazy**, klikněte na tlačítko **řídicí panel clusteru**a potom z **řídicí panel clusteru** okně klikněte na tlačítko **řídicí panel clusteru HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-126">From the cluster blade, under **Quick Links**, click **Cluster Dashboard**, and then from the **Cluster Dashboard** blade, click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="cb6ae-127">Alternativně můžete přímo otevře řídicí panel pomocí následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="cb6ae-127">Alternatively, you can directly open the dashboard by using the following URL:</span></span>

         https://<clustername>.azurehdinsight.net

    <span data-ttu-id="cb6ae-128">Po zobrazení výzvy ověřování pomocí správce uživatelské jméno a heslo, které jste použili při zřizování clusteru.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-128">When prompted, authenticate by using the administrator user name and password you used when provisioning the cluster.</span></span>
3. <span data-ttu-id="cb6ae-129">Z webové stránky, které se otevře, klikněte na tlačítko **postupem Začínáme Galerie** kartě a potom v části **řešení s ukázkovými daty** kategorie, klikněte na tlačítko **analýza webového protokolu** ukázka.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-129">From the web page that opens, click the **Getting Started Gallery** tab, and then under the **Solutions with Sample Data** category, click the **Website Log Analysis** sample.</span></span>
4. <span data-ttu-id="cb6ae-130">Postupujte podle pokynů na webové stránce ukončíte vzorku.</span><span class="sxs-lookup"><span data-stu-id="cb6ae-130">Follow the instructions provided on the web page to finish the sample.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb6ae-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb6ae-131">Next steps</span></span>
<span data-ttu-id="cb6ae-132">Zkuste následující ukázka: [analýza dat snímače používání Hive s HDInsight](hdinsight-hive-analyze-sensor-data.md).</span><span class="sxs-lookup"><span data-stu-id="cb6ae-132">Try the following sample: [Analyzing sensor data using Hive with HDInsight](hdinsight-hive-analyze-sensor-data.md).</span></span>

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
