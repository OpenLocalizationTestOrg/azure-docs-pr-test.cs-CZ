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
# <a name="collect-heap-dumps-in-blob-storage-toodebug-and-analyze-hadoop-services"></a>Shromažďování výpisů paměti haldy v toodebug úložiště objektů Blob a analyzovat služby Hadoop
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Výpisy haldy obsahovat snímek paměti hello aplikace, včetně hello hodnoty proměnných v hello čas vytvoření výpisu hello. Takže se hodí pro diagnostiku problémů, ke kterým dochází za běhu. Výpisy haldy můžete automaticky shromážděna pro služby Hadoop a umístěn uvnitř hello účtu Azure Blob storage uživatele v části HDInsightHeapDumps /.

kolekce Hello výpisů paměti haldy pro různé služby musí být povolen pro služby v jednotlivých clusterech. Výchozí hodnota Hello pro tuto funkci je toobe vypnout pro cluster. Tyto výpisy haldy může být velký, tak, aby byl účet úložiště Blob hello doporučuje toomonitor, kde se uloží po hello kolekce byla povolena.

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Hello informace v tomto článku se vztahuje pouze na základě tooWindows HDInsight. Informace v HDInsight se systémem Linux najdete v tématu [výpisy paměti haldy povolit pro služby Hadoop v HDInsight se systémem Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)


## <a name="eligible-services-for-heap-dumps"></a>Oprávněných služeb pro výpisy haldy
Můžete povolit výpisů paměti haldy pro hello následující služby:

* **hcatalog** -tempelton
* **Hive** -hiveserver2, metaúložiště, derbyserver
* **mapreduce** -jobhistoryserver
* **yarn** -resourcemanager, nodemanager, timelineserver
* **hdfs** -datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Konfigurační prvky, které umožňují výpisů paměti haldy
tooturn na výpisů paměti haldy pro službu, je nutné tooset hello příslušné konfigurace – elementy v části hello dané služby, který je určený **service_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Hello hodnotu **service_name** může být libovolná hello služeb tady: tempelton, hiveserver2, metaúložiště, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, nebo namenode.

## <a name="enable-using-azure-powershell"></a>Povolit pomocí Azure PowerShell
Například tooturn na výpisů paměti haldy pomocí prostředí Azure PowerShell pro jobhistoryserver, můžete použít hello následující skript:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Povolit pomocí sady .NET SDK
Například tooturn na výpisů paměti haldy pomocí hello .NET SDK služby Azure HDInsight pro jobhistoryserver, můžete použít následující kód hello:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
