---
title: "aaaUse Hive se systémem Hadoop pro analýzu protokolu Web - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse Hive s HDInsight tooanalyze webu protokoly. Můžete použít soubor protokolu jako vstup do tabulky HDInsight a použít HiveQL tooquery hello data."
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
ms.openlocfilehash: 9cbce3cc8cf8bc3ad104dc4ca6a5628802c8fe89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hive-with-windows-based-hdinsight-tooanalyze-logs-from-websites"></a><span data-ttu-id="e3a81-104">Použijte Hive s HDInsight se systémem Windows tooanalyze protokoly z webů</span><span class="sxs-lookup"><span data-stu-id="e3a81-104">Use Hive with Windows-based HDInsight tooanalyze logs from websites</span></span>
<span data-ttu-id="e3a81-105">Zjistěte, jak toouse HiveQL s HDInsight tooanalyze protokoly z webu.</span><span class="sxs-lookup"><span data-stu-id="e3a81-105">Learn how toouse HiveQL with HDInsight tooanalyze logs from a website.</span></span> <span data-ttu-id="e3a81-106">Analýza protokolu webu můžete být použité toosegment cílovou skupinu podle podobné aktivity, kategorizace návštěvníky demografické údaje a toofind hello obsah, který uživatel vidí, hello webů, které pocházejí z a tak dále.</span><span class="sxs-lookup"><span data-stu-id="e3a81-106">Website log analysis can be used toosegment your audience based on similar activities, categorize site visitors by demographics, and toofind out hello content they view, hello websites they come from, and so on.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e3a81-107">Hello kroky v této dokumentu funguje jenom s clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="e3a81-107">hello steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="e3a81-108">HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="e3a81-108">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="e3a81-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e3a81-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e3a81-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e3a81-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="e3a81-111">V této ukázce použijete HDInsight clusteru tooanalyze webu protokolu soubory tooget lépe pochopit četnost hello webu toohello návštěv z externích webových stránek za den.</span><span class="sxs-lookup"><span data-stu-id="e3a81-111">In this sample, you will use an HDInsight cluster tooanalyze website log files tooget insight into hello frequency of visits toohello website from external websites in a day.</span></span> <span data-ttu-id="e3a81-112">Pokud vytvoříte souhrn chyb na webových stránkách, které uživatelé hello zaznamenat.</span><span class="sxs-lookup"><span data-stu-id="e3a81-112">You'll also generate a summary of website errors that hello users experience.</span></span> <span data-ttu-id="e3a81-113">Se dozvíte, jak:</span><span class="sxs-lookup"><span data-stu-id="e3a81-113">You will learn how to:</span></span>

* <span data-ttu-id="e3a81-114">Připojte tooa úložiště objektů Azure Blob, který obsahuje soubory protokolu webové stránky.</span><span class="sxs-lookup"><span data-stu-id="e3a81-114">Connect tooa Azure Blob storage, which contains website log files.</span></span>
* <span data-ttu-id="e3a81-115">Vytvoření HIVE tabulky tooquery tyto protokoly.</span><span class="sxs-lookup"><span data-stu-id="e3a81-115">Create HIVE tables tooquery those logs.</span></span>
* <span data-ttu-id="e3a81-116">Tooanalyze hello data vytvořte dotazy HIVE.</span><span class="sxs-lookup"><span data-stu-id="e3a81-116">Create HIVE queries tooanalyze hello data.</span></span>
* <span data-ttu-id="e3a81-117">Pomocí aplikace Microsoft Excel tooconnect tooHDInsight (pomocí otevřenou databázi připojení (ODBC) tooretrieve hello analyzovat data.</span><span class="sxs-lookup"><span data-stu-id="e3a81-117">Use Microsoft Excel tooconnect tooHDInsight (by using open database connectivity (ODBC) tooretrieve hello analyzed data.</span></span>

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

## <a name="prerequisites"></a><span data-ttu-id="e3a81-119">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e3a81-119">Prerequisites</span></span>
* <span data-ttu-id="e3a81-120">Musí být zřízená clusteru Hadoop v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e3a81-120">You must have provisioned a Hadoop cluster on Azure HDInsight.</span></span> <span data-ttu-id="e3a81-121">Pokyny najdete v tématu [zřizování clusterů HDInsight][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="e3a81-121">For instructions, see [Provision HDInsight Clusters][hdinsight-provision].</span></span>
* <span data-ttu-id="e3a81-122">Musíte mít aplikaci Microsoft Excel 2013 nebo nainstalovat aplikaci Excel 2010.</span><span class="sxs-lookup"><span data-stu-id="e3a81-122">You must have Microsoft Excel 2013 or Excel 2010 installed.</span></span>
* <span data-ttu-id="e3a81-123">Musíte mít [ovladače ODBC Microsoft Hive](http://www.microsoft.com/download/details.aspx?id=40886) tooimport data z podregistru do aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="e3a81-123">You must have [Microsoft Hive ODBC Driver](http://www.microsoft.com/download/details.aspx?id=40886) tooimport data from Hive into Excel.</span></span>

## <a name="toorun-hello-sample"></a><span data-ttu-id="e3a81-124">Ukázka toorun hello</span><span class="sxs-lookup"><span data-stu-id="e3a81-124">toorun hello sample</span></span>
1. <span data-ttu-id="e3a81-125">Z hello [portálu Azure](https://portal.azure.com/), z hello úvodní panel (Pokud je připnutý hello clusteru existuje), klikněte na dlaždici clusteru hello, na kterém chcete toorun hello ukázka.</span><span class="sxs-lookup"><span data-stu-id="e3a81-125">From hello [Azure Portal](https://portal.azure.com/), from hello Startboard (if you pinned hello cluster there), click hello cluster tile on which you want toorun hello sample.</span></span>
2. <span data-ttu-id="e3a81-126">Z hello clusteru okno, v části **rychlé odkazy**, klikněte na tlačítko **řídicí panel clusteru**a potom z hello **řídicí panel clusteru** okně klikněte na tlačítko **clusteru HDInsight Řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="e3a81-126">From hello cluster blade, under **Quick Links**, click **Cluster Dashboard**, and then from hello **Cluster Dashboard** blade, click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="e3a81-127">Alternativně můžete přímo otevřít řídicí panel hello pomocí hello následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="e3a81-127">Alternatively, you can directly open hello dashboard by using hello following URL:</span></span>

         https://<clustername>.azurehdinsight.net

    <span data-ttu-id="e3a81-128">Po zobrazení výzvy ověřování pomocí hello správce uživatelského jména a hesla, který jste použili při zřizování clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="e3a81-128">When prompted, authenticate by using hello administrator user name and password you used when provisioning hello cluster.</span></span>
3. <span data-ttu-id="e3a81-129">Z hello webové stránky, které se otevře, klikněte na tlačítko hello **postupem Začínáme Galerie** kartě a potom v části hello **řešení s ukázkovými daty** kategorii, klikněte na tlačítko hello **analýza protokolu webu** Ukázka.</span><span class="sxs-lookup"><span data-stu-id="e3a81-129">From hello web page that opens, click hello **Getting Started Gallery** tab, and then under hello **Solutions with Sample Data** category, click hello **Website Log Analysis** sample.</span></span>
4. <span data-ttu-id="e3a81-130">Postupujte podle pokynů hello vzorkem hello toofinish webovou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="e3a81-130">Follow hello instructions provided on hello web page toofinish hello sample.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3a81-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3a81-131">Next steps</span></span>
<span data-ttu-id="e3a81-132">Zkuste hello následující ukázka: [analýza dat snímače používání Hive s HDInsight](hdinsight-hive-analyze-sensor-data.md).</span><span class="sxs-lookup"><span data-stu-id="e3a81-132">Try hello following sample: [Analyzing sensor data using Hive with HDInsight](hdinsight-hive-analyze-sensor-data.md).</span></span>

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
