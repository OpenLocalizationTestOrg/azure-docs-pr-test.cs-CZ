---
title: "Analýza dat snímačů pomocí Hive a Hadoop - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak analyzovat data snímačů pomocí konzole dotaz Hive s HDInsight (Hadoop) a potom vizualizaci dat v aplikaci Microsoft Excel s PowerView."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a8ac160c-1cef-45d9-bf36-7beb5a439105
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 3abb71c12b4769bebd808276f8bdd832aad22d7a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a><span data-ttu-id="3cba9-103">Analýza dat snímačů v konzole dotaz Hive na Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="3cba9-103">Analyze sensor data using the Hive Query Console on Hadoop in HDInsight</span></span>

<span data-ttu-id="3cba9-104">Zjistěte, jak analyzovat data snímačů pomocí konzole dotaz Hive s HDInsight (Hadoop) a potom vizualizovat data v aplikaci Microsoft Excel pomocí Power View.</span><span class="sxs-lookup"><span data-stu-id="3cba9-104">Learn how to analyze sensor data by using the Hive Query Console with HDInsight (Hadoop), then visualize the data in Microsoft Excel by using Power View.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3cba9-105">Kroky v tomto dokumentu pracovat pouze s clustery HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="3cba9-105">The steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="3cba9-106">HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="3cba9-106">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="3cba9-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="3cba9-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3cba9-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3cba9-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


<span data-ttu-id="3cba9-109">V této ukázce použijete Hive ke zpracování historických dat a identifikovat problémy s vytápění a klimatizace systémy.</span><span class="sxs-lookup"><span data-stu-id="3cba9-109">In this sample, you use Hive to process historical data and identify problems with heating and air conditioning systems.</span></span> <span data-ttu-id="3cba9-110">Konkrétně určit systémy, nejsou schopna spolehlivě udržovat nastavenou teplotu provedením následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="3cba9-110">Specifically, you identify systems are not able to reliably maintain a set temperature by performing the following tasks:</span></span>

* <span data-ttu-id="3cba9-111">Vytváření tabulek HIVE k dotazování na data uložená v souborech s oddělovači (CSV).</span><span class="sxs-lookup"><span data-stu-id="3cba9-111">Create HIVE tables to query data stored in comma-separated value (CSV) files.</span></span>
* <span data-ttu-id="3cba9-112">Vytváření dotazů HIVE analyzovat data.</span><span class="sxs-lookup"><span data-stu-id="3cba9-112">Create HIVE queries to analyze the data.</span></span>
* <span data-ttu-id="3cba9-113">Pokud chcete načíst analyzovaná data, aplikaci Microsoft Excel pro připojení k HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3cba9-113">To retrieve the analyzed data, use Microsoft Excel to connect to HDInsight.</span></span>
* <span data-ttu-id="3cba9-114">K vizualizaci dat, použijte Power View.</span><span class="sxs-lookup"><span data-stu-id="3cba9-114">To visualize the data, use Power View.</span></span>

![Diagram architektury řešení](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="3cba9-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3cba9-116">Prerequisites</span></span>

* <span data-ttu-id="3cba9-117">Cluster služby HDInsight (Hadoop): najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md) informace o vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="3cba9-117">An HDInsight (Hadoop) cluster: See [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) for information about creating a cluster.</span></span>
* <span data-ttu-id="3cba9-118">Microsoft Excel 2013</span><span class="sxs-lookup"><span data-stu-id="3cba9-118">Microsoft Excel 2013</span></span>

  > [!NOTE]
  > <span data-ttu-id="3cba9-119">Aplikace Microsoft Excel se používá pro vizualizaci dat s [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).</span><span class="sxs-lookup"><span data-stu-id="3cba9-119">Microsoft Excel is used for data visualization with [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).</span></span>

* [<span data-ttu-id="3cba9-120">Ovladač ODBC Microsoft Hive</span><span class="sxs-lookup"><span data-stu-id="3cba9-120">Microsoft Hive ODBC Driver</span></span>](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="to-run-the-sample"></a><span data-ttu-id="3cba9-121">Ke spuštění ukázky</span><span class="sxs-lookup"><span data-stu-id="3cba9-121">To run the sample</span></span>

1. <span data-ttu-id="3cba9-122">Z webového prohlížeče přejděte na následující adresu URL:</span><span class="sxs-lookup"><span data-stu-id="3cba9-122">From your web browser, navigate to the following URL:</span></span> 

         https://<clustername>.azurehdinsight.net

    <span data-ttu-id="3cba9-123">Parametr `<clustername>` nahraďte názvem vašeho clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3cba9-123">Replace `<clustername>` with the name of your HDInsight cluster.</span></span>

    <span data-ttu-id="3cba9-124">Po zobrazení výzvy ověřování pomocí správce uživatelské jméno a heslo, které jste použili při zřizování tento cluster.</span><span class="sxs-lookup"><span data-stu-id="3cba9-124">When prompted, authenticate by using the administrator user name and password you used when provisioning this cluster.</span></span>

2. <span data-ttu-id="3cba9-125">Z webové stránky, které se otevře, klikněte na tlačítko **postupem Začínáme Galerie** kartě a potom v části **řešení s ukázkovými daty** kategorie, klikněte na tlačítko **analýza dat snímačů** ukázka.</span><span class="sxs-lookup"><span data-stu-id="3cba9-125">From the web page that opens, click the **Getting Started Gallery** tab, and then under the **Solutions with Sample Data** category, click the **Sensor Data Analysis** sample.</span></span>

    ![Získávání Začínáme Galerie image](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. <span data-ttu-id="3cba9-127">Postupujte podle pokynů na webové stránce ukončíte vzorku.</span><span class="sxs-lookup"><span data-stu-id="3cba9-127">Follow the instructions provided on the web page to finish the sample.</span></span>
