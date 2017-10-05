---
title: "Ladění a analýze služby Hadoop s výpisů paměti haldy - Azure | Microsoft Docs"
description: "Automatické shromažďování výpisů paměti haldy pro služby Hadoop a umístěte do účtu úložiště objektů Blob v Azure pro ladění a analýzu."
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
ms.openlocfilehash: 6d1d4d47d279eb7a1f0bf1f587445683f0ace7a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a><span data-ttu-id="ce2ac-103">V úložišti objektů Blob k ladění a analýze služby Hadoop výpisy paměti shromažďování haldy</span><span class="sxs-lookup"><span data-stu-id="ce2ac-103">Collect heap dumps in Blob storage to debug and analyze Hadoop services</span></span>
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="ce2ac-104">Výpisy haldy obsahovat snímek paměti aplikace, včetně hodnot proměnných v době, kdy byla vytvořena výpis.</span><span class="sxs-lookup"><span data-stu-id="ce2ac-104">Heap dumps contain a snapshot of the application's memory, including the values of variables at the time the dump was created.</span></span> <span data-ttu-id="ce2ac-105">Takže se hodí pro diagnostiku problémů, ke kterým dochází za běhu.</span><span class="sxs-lookup"><span data-stu-id="ce2ac-105">So they are useful for diagnosing problems that occur at run-time.</span></span> <span data-ttu-id="ce2ac-106">Výpisy haldy můžete automaticky shromážděna pro služby Hadoop a umístěn uvnitř účtu Azure Blob storage uživatele v části HDInsightHeapDumps /.</span><span class="sxs-lookup"><span data-stu-id="ce2ac-106">Heap dumps can be automatically collected for Hadoop services and placed inside the Azure Blob storage account of a user under HDInsightHeapDumps/.</span></span>

<span data-ttu-id="ce2ac-107">Shromažďování výpisů paměti haldy pro různé služby musí být povolen pro služby v jednotlivých clusterech.</span><span class="sxs-lookup"><span data-stu-id="ce2ac-107">The collection of heap dumps for various services must be enabled for services on individual clusters.</span></span> <span data-ttu-id="ce2ac-108">Výchozí hodnota pro tuto funkci je potřeba vypnout pro cluster.</span><span class="sxs-lookup"><span data-stu-id="ce2ac-108">The default for this feature is to be off for a cluster.</span></span> <span data-ttu-id="ce2ac-109">Tyto výpisy haldy může být velký, proto se doporučuje pro monitorování účtu úložiště Blob, kde se uloží po kolekce byla povolena.</span><span class="sxs-lookup"><span data-stu-id="ce2ac-109">These heap dumps can be large, so it is advisable to monitor the Blob storage account where they are being saved once the collection has been enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ce2ac-110">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="ce2ac-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ce2ac-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ce2ac-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="ce2ac-112">Informace v tomto článku se vztahují pouze na HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="ce2ac-112">The information in this article only applies to Windows-based HDInsight.</span></span> <span data-ttu-id="ce2ac-113">Informace v HDInsight se systémem Linux najdete v tématu [výpisy paměti haldy povolit pro služby Hadoop v HDInsight se systémem Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span><span class="sxs-lookup"><span data-stu-id="ce2ac-113">For information on Linux-based HDInsight, see [Enable heap dumps for Hadoop services on Linux-based HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span></span>


## <a name="eligible-services-for-heap-dumps"></a><span data-ttu-id="ce2ac-114">Oprávněných služeb pro výpisy haldy</span><span class="sxs-lookup"><span data-stu-id="ce2ac-114">Eligible services for heap dumps</span></span>
<span data-ttu-id="ce2ac-115">Můžete povolit výpisů paměti haldy pro následující služby:</span><span class="sxs-lookup"><span data-stu-id="ce2ac-115">You can enable heap dumps for the following services:</span></span>

* <span data-ttu-id="ce2ac-116">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="ce2ac-116">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="ce2ac-117">**Hive** -hiveserver2, metaúložiště, derbyserver</span><span class="sxs-lookup"><span data-stu-id="ce2ac-117">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="ce2ac-118">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="ce2ac-118">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="ce2ac-119">**yarn** -resourcemanager, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="ce2ac-119">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="ce2ac-120">**hdfs** -datanode, secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="ce2ac-120">**hdfs** - datanode, secondarynamenode, namenode</span></span>

## <a name="configuration-elements-that-enable-heap-dumps"></a><span data-ttu-id="ce2ac-121">Konfigurační prvky, které umožňují výpisů paměti haldy</span><span class="sxs-lookup"><span data-stu-id="ce2ac-121">Configuration elements that enable heap dumps</span></span>
<span data-ttu-id="ce2ac-122">Chcete-li na výpisů paměti haldy pro službu, musíte nastavit odpovídající konfigurační prvky v části pro služby, který je určený **service_name**.</span><span class="sxs-lookup"><span data-stu-id="ce2ac-122">To turn on heap dumps for a service, you need to set the appropriate configuration elements in the section for that service, which is specified by **service_name**.</span></span>

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

<span data-ttu-id="ce2ac-123">Hodnota **service_name** mohou být některé z služeb tady: tempelton, hiveserver2, metaúložiště, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, nebo namenode.</span><span class="sxs-lookup"><span data-stu-id="ce2ac-123">The value of **service_name** can be any of the services listed here: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, or namenode.</span></span>

## <a name="enable-using-azure-powershell"></a><span data-ttu-id="ce2ac-124">Povolit pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ce2ac-124">Enable using Azure PowerShell</span></span>
<span data-ttu-id="ce2ac-125">Například pokud chcete zapnout haldy výpisy pomocí prostředí Azure PowerShell pro jobhistoryserver, můžete použít následující skript:</span><span class="sxs-lookup"><span data-stu-id="ce2ac-125">For example, to turn on heap dumps by using Azure PowerShell for jobhistoryserver, you can use the following script:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a><span data-ttu-id="ce2ac-126">Povolit pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="ce2ac-126">Enable using .NET SDK</span></span>
<span data-ttu-id="ce2ac-127">Například pokud chcete zapnout haldy výpisy pomocí .NET SDK služby Azure HDInsight pro jobhistoryserver, můžete použít následující kód:</span><span class="sxs-lookup"><span data-stu-id="ce2ac-127">For example, to turn on heap dumps by using the Azure HDInsight .NET SDK for jobhistoryserver, you can use the following code:</span></span>

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
