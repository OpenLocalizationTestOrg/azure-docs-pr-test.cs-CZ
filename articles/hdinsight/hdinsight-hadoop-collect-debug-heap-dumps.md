---
title: "aaaDebug a analyzovat služby Hadoop s výpisů paměti haldy - Azure | Microsoft Docs"
description: "Automaticky shromažďování výpisů paměti haldy pro služby Hadoop a umístěte uvnitř hello účtu úložiště objektů Blob v Azure pro ladění a analýzu."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e4ec4ebb-fd32-4668-8382-f956581485c4
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 70fbc2d6d97d35b0d7b1d9149673b02ae1878eb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-heap-dumps-in-blob-storage-toodebug-and-analyze-hadoop-services"></a><span data-ttu-id="de19c-103">Shromažďování výpisů paměti haldy v toodebug úložiště objektů Blob a analyzovat služby Hadoop</span><span class="sxs-lookup"><span data-stu-id="de19c-103">Collect heap dumps in Blob storage toodebug and analyze Hadoop services</span></span>
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="de19c-104">Výpisy haldy obsahovat snímek paměti hello aplikace, včetně hello hodnoty proměnných v hello čas vytvoření výpisu hello.</span><span class="sxs-lookup"><span data-stu-id="de19c-104">Heap dumps contain a snapshot of hello application's memory, including hello values of variables at hello time hello dump was created.</span></span> <span data-ttu-id="de19c-105">Takže se hodí pro diagnostiku problémů, ke kterým dochází za běhu.</span><span class="sxs-lookup"><span data-stu-id="de19c-105">So they are useful for diagnosing problems that occur at run-time.</span></span> <span data-ttu-id="de19c-106">Výpisy haldy můžete automaticky shromážděna pro služby Hadoop a umístěn uvnitř hello účtu Azure Blob storage uživatele v části HDInsightHeapDumps /.</span><span class="sxs-lookup"><span data-stu-id="de19c-106">Heap dumps can be automatically collected for Hadoop services and placed inside hello Azure Blob storage account of a user under HDInsightHeapDumps/.</span></span>

<span data-ttu-id="de19c-107">kolekce Hello výpisů paměti haldy pro různé služby musí být povolen pro služby v jednotlivých clusterech.</span><span class="sxs-lookup"><span data-stu-id="de19c-107">hello collection of heap dumps for various services must be enabled for services on individual clusters.</span></span> <span data-ttu-id="de19c-108">Výchozí hodnota Hello pro tuto funkci je toobe vypnout pro cluster.</span><span class="sxs-lookup"><span data-stu-id="de19c-108">hello default for this feature is toobe off for a cluster.</span></span> <span data-ttu-id="de19c-109">Tyto výpisy haldy může být velký, tak, aby byl účet úložiště Blob hello doporučuje toomonitor, kde se uloží po hello kolekce byla povolena.</span><span class="sxs-lookup"><span data-stu-id="de19c-109">These heap dumps can be large, so it is advisable toomonitor hello Blob storage account where they are being saved once hello collection has been enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="de19c-110">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="de19c-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="de19c-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="de19c-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="de19c-112">Hello informace v tomto článku se vztahuje pouze na základě tooWindows HDInsight.</span><span class="sxs-lookup"><span data-stu-id="de19c-112">hello information in this article only applies tooWindows-based HDInsight.</span></span> <span data-ttu-id="de19c-113">Informace v HDInsight se systémem Linux najdete v tématu [výpisy paměti haldy povolit pro služby Hadoop v HDInsight se systémem Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span><span class="sxs-lookup"><span data-stu-id="de19c-113">For information on Linux-based HDInsight, see [Enable heap dumps for Hadoop services on Linux-based HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span></span>


## <a name="eligible-services-for-heap-dumps"></a><span data-ttu-id="de19c-114">Oprávněných služeb pro výpisy haldy</span><span class="sxs-lookup"><span data-stu-id="de19c-114">Eligible services for heap dumps</span></span>
<span data-ttu-id="de19c-115">Můžete povolit výpisů paměti haldy pro hello následující služby:</span><span class="sxs-lookup"><span data-stu-id="de19c-115">You can enable heap dumps for hello following services:</span></span>

* <span data-ttu-id="de19c-116">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="de19c-116">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="de19c-117">**Hive** -hiveserver2, metaúložiště, derbyserver</span><span class="sxs-lookup"><span data-stu-id="de19c-117">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="de19c-118">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="de19c-118">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="de19c-119">**yarn** -resourcemanager, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="de19c-119">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="de19c-120">**hdfs** -datanode, secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="de19c-120">**hdfs** - datanode, secondarynamenode, namenode</span></span>

## <a name="configuration-elements-that-enable-heap-dumps"></a><span data-ttu-id="de19c-121">Konfigurační prvky, které umožňují výpisů paměti haldy</span><span class="sxs-lookup"><span data-stu-id="de19c-121">Configuration elements that enable heap dumps</span></span>
<span data-ttu-id="de19c-122">tooturn na výpisů paměti haldy pro službu, je nutné tooset hello příslušné konfigurace – elementy v části hello dané služby, který je určený **service_name**.</span><span class="sxs-lookup"><span data-stu-id="de19c-122">tooturn on heap dumps for a service, you need tooset hello appropriate configuration elements in hello section for that service, which is specified by **service_name**.</span></span>

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

<span data-ttu-id="de19c-123">Hello hodnotu **service_name** může být libovolná hello služeb tady: tempelton, hiveserver2, metaúložiště, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, nebo namenode.</span><span class="sxs-lookup"><span data-stu-id="de19c-123">hello value of **service_name** can be any of hello services listed here: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, or namenode.</span></span>

## <a name="enable-using-azure-powershell"></a><span data-ttu-id="de19c-124">Povolit pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="de19c-124">Enable using Azure PowerShell</span></span>
<span data-ttu-id="de19c-125">Například tooturn na výpisů paměti haldy pomocí prostředí Azure PowerShell pro jobhistoryserver, můžete použít hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="de19c-125">For example, tooturn on heap dumps by using Azure PowerShell for jobhistoryserver, you can use hello following script:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a><span data-ttu-id="de19c-126">Povolit pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="de19c-126">Enable using .NET SDK</span></span>
<span data-ttu-id="de19c-127">Například tooturn na výpisů paměti haldy pomocí hello .NET SDK služby Azure HDInsight pro jobhistoryserver, můžete použít následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="de19c-127">For example, tooturn on heap dumps by using hello Azure HDInsight .NET SDK for jobhistoryserver, you can use hello following code:</span></span>

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
