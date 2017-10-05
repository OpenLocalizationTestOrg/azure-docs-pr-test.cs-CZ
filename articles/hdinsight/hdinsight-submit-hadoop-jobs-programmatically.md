---
title: "Odesílání úloh Hadoop v HDInsight | Microsoft Docs"
description: "Zjistěte, jak k odesílání úloh Hadoop do Azure HDInsight Hadoop."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 50430b96-2329-4775-9713-19c5795b775f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 6829ff82afc7fcea9e027ad14ec7ed0c8015a5fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="submit-hadoop-jobs-in-hdinsight"></a><span data-ttu-id="91676-103">Odesílání úloh Hadoopu do HDInsight</span><span class="sxs-lookup"><span data-stu-id="91676-103">Submit Hadoop jobs in HDInsight</span></span>

<span data-ttu-id="91676-104">Můžete odeslat úloh Hadoop pomocí .NET SDK, Curl a prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="91676-104">You can submit Hadoop jobs using .NET SDK, Curl, and Azure PowerShell:</span></span>

- <span data-ttu-id="91676-105">Použití sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="91676-105">Use .NET SDK</span></span>

  - [<span data-ttu-id="91676-106">Vytvoření neinteraktivního ověřování aplikace .NET</span><span class="sxs-lookup"><span data-stu-id="91676-106">Create non-interactive authentication .NET applications</span></span>](hdinsight-create-non-interactive-authentication-dotnet-applications.md)
  - [<span data-ttu-id="91676-107">Spouštění dotazů Hive pomocí sady .NET SDK HDInsight</span><span class="sxs-lookup"><span data-stu-id="91676-107">Run Hive queries using HDInsight .NET SDK</span></span>](hdinsight-hadoop-use-hive-dotnet-sdk.md)
  - [<span data-ttu-id="91676-108">Spuštění úlohy Pig pomocí sady .NET SDK pro Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="91676-108">Run Pig jobs using the .NET SDK for Hadoop in HDInsight</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md)
  - [<span data-ttu-id="91676-109">Spuštění úloh Sqoop pro Hadoop v HDInsight pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="91676-109">Run Sqoop jobs using .NET SDK for Hadoop in HDInsight</span></span>](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)
  - [<span data-ttu-id="91676-110">Spuštění úloh MapReduce pomocí sady .NET SDK HDInsight</span><span class="sxs-lookup"><span data-stu-id="91676-110">Run MapReduce jobs using HDInsight .NET SDK</span></span>](hdinsight-hadoop-use-mapreduce-dotnet-sdk.md)

- <span data-ttu-id="91676-111">CURL</span><span class="sxs-lookup"><span data-stu-id="91676-111">CURL</span></span>

  - [<span data-ttu-id="91676-112">Spouštění dotazů Hive se systémem Hadoop v prostředí HDInsight pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="91676-112">Run Hive queries with Hadoop in HDInsight with Curl</span></span>](hdinsight-hadoop-use-hive-curl.md)
  - [<span data-ttu-id="91676-113">Spuštění úlohy Pig s Hadoop v HDInsight pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="91676-113">Run Pig jobs with Hadoop on HDInsight by using Curl</span></span>](hdinsight-hadoop-use-pig-curl.md)
  - [<span data-ttu-id="91676-114">Spuštění úloh Sqoop se systémem Hadoop v prostředí HDInsight pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="91676-114">Run Sqoop jobs with Hadoop in HDInsight with Curl</span></span>](hdinsight-hadoop-use-sqoop-curl.md)
  - [<span data-ttu-id="91676-115">Spuštění úloh MapReduce s Hadoop v HDInsight pomocí Curl</span><span class="sxs-lookup"><span data-stu-id="91676-115">Run MapReduce jobs with Hadoop on HDInsight using Curl</span></span>](hdinsight-hadoop-use-mapreduce-curl.md)

- <span data-ttu-id="91676-116">PowerShell</span><span class="sxs-lookup"><span data-stu-id="91676-116">PowerShell</span></span>

  - [<span data-ttu-id="91676-117">Spouštění dotazů Hive pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="91676-117">Run Hive queries using PowerShell</span></span>](hdinsight-hadoop-use-hive-powershell.md)
  - [<span data-ttu-id="91676-118">Spuštění úlohy Pig pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="91676-118">Run Pig jobs using PowerShell</span></span>](hdinsight-hadoop-use-pig-powershell.md)
  - [<span data-ttu-id="91676-119">Použití nástroje Sqoop se systémem Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="91676-119">Use Sqoop with Hadoop in HDInsight</span></span>](hdinsight-hadoop-use-sqoop-powershell.md)
  - [<span data-ttu-id="91676-120">Spuštění úloh MapReduce s Hadoop v HDInsight pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="91676-120">Run MapReduce jobs with Hadoop on HDInsight using PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md)

## <a name="see-also"></a><span data-ttu-id="91676-121">Viz také</span><span class="sxs-lookup"><span data-stu-id="91676-121">See also</span></span>

- [<span data-ttu-id="91676-122">Dokumentace Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="91676-122">Azure HDInsight Documentation</span></span>](https://docs.microsoft.com/azure/hdinsight/)