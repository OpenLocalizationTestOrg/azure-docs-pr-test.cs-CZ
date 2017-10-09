---
title: aaaUse Hadoop Pig v HDInsight | Microsoft Docs
description: "Zjistěte, jak toouse vepřových s Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: acfeb52b-4b81-4a7d-af77-3e9908407404
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 90850f2c742b8954c66ce277127e01b14fc3906f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a>Použijte Pig s Hadoop v HDInsight

Zjistěte, jak toouse [Apache Pig](http://pig.apache.org/) s HDInsight...

Pig je platforma pro vytváření programů pro Hadoop pomocí procedurální jazyka známé jako *Pig Latin*. Pig je alternativní tooJava pro vytvoření *MapReduce* řešení a je součástí Azure HDInsight. Použijte následující tabulku toodiscover hello různé způsoby, že lze použít Pig s HDInsight hello:

| **Použít** Pokud chcete... | .. .an **interaktivní** prostředí | ... **batch** zpracování | .. při to **clusteru operačního systému** | .. .from to **klientský operační systém** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X nebo systému Windows |
| [REST API](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux nebo Windows |Linux, Unix, Mac OS X nebo systému Windows |
| [Sada .NET SDK pro Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux nebo Windows |Windows (prozatím) |
| [Prostředí Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux nebo Windows |Windows |
| [Vzdálená plocha](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 a 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="why"></a>Proč používat Pig

Jeden z hello výzev zpracování dat pomocí MapReduce v Hadoop je implementace logika zpracování pomocí pouze mapu a snižte funkce. Pro komplexní zpracování, je často mají toobreak zpracování do více operací MapReduce, které jsou zřetězené společně tooachieve hello požadovaného výsledku.

Pig vám umožní toodefine zpracování jako řadu transformace, které hello dat prochází tooproduce hello potřeby výstup.

jazyka Pig Latin Hello umožňuje vám toodescribe hello tok dat z nezpracovaná vstupu prostřednictvím jeden nebo více transformace, výstup hello potřeby tooproduce. Pig Latin programy postupujte podle tohoto vzoru Obecné:

* **Zatížení**: čtení dat toobe s nimi manipulovat, hello systému souborů

* **Transformace**: pracovat s daty hello

* **Výpis stavu nebo ukládat**: výstupní data toohello obrazovky nebo uloží jej pro zpracování

### <a name="user-defined-functions"></a>Uživatelem definované funkce

Pig Latin také podporuje uživatelsky definované funkce (UDF), což vám umožní tooinvoke externí součásti, které implementují logiku, která je obtížné toomodel v Pig Latin.

Další informace o Pig Latin najdete v tématu [Pig Latin odkazu ruční 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) a [Pig Latin odkaz ruční 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Příklad použití UDF s Pig naleznete v části hello následující dokumenty:

* [Pig v HDInsight pomocí DataFu](hdinsight-hadoop-use-pig-datafu-udf.md) -DataFu je kolekce užitečné UDF udržované Apache
* [Používat jazyk Python s Pig a Hive v HDInsight](hdinsight-python.md)
* [Použití jazyka C# s Hive a Pig v HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a id="data"></a>Příklad dat

HDInsight poskytuje různé příklad datových sad, které jsou uloženy v hello `/example/data` a `/HdiSamples` adresáře. Tyto adresáře jsou hello výchozí úložiště pro cluster. Příklad Pig Hello v tomto dokumentu používá hello *log4j* souboru z `/example/data/sample.log`.

V souboru hello každý protokol se skládá z řádku pole, která obsahuje `[LOG LEVEL]` pole typu a hello závažnost hello tooshow například:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

V předchozím příkladu hello je hello úroveň protokolu chyba.

> [!NOTE]
> Můžete také vygenerovat soubor log4j pomocí hello [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) protokolování nástroj a pak nahrajte tento soubor tooyour objekt blob. V tématu [nahrát Data tooHDInsight](hdinsight-upload-data.md) pokyny. Další informace o použití objektů BLOB v úložišti Azure s HDInsight naleznete v tématu [použití Azure Blob Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md).

## <a id="job"></a>Příklad úloh

Hello následující úlohy Pig Latin načte hello `sample.log` soubor z hello výchozí úložiště pro váš cluster HDInsight. Potom provede řady transformací, jejichž výsledkem počet jak mnohokrát každý vstupní data úrovně hello k chybě v protokolu. výsledky Hello se zapisují tooSTDOUT.

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Hello následující obrázek zobrazuje souhrn co každý transformace nemá toohello data.

![Grafické znázornění transformací hello][image-hdi-pig-data-transformation]

## <a id="run"></a>Spustit úlohu Pig Latin hello

HDInsight můžete spustit úlohy Pig Latin pomocí různých metod. Pomocí hello následující toodecide tabulku, která metoda je pro vás nejvhodnější a potom postupujte podle hello odkaz návod.

| **Použít** Pokud chcete... | .. .an **interaktivní** prostředí | ... **batch** zpracování | .. při to **clusteru operačního systému** | .. .from to **klienta** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X nebo systému Windows |
| [Curl](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux nebo Windows |Linux, Unix, Mac OS X nebo systému Windows |
| [Sada .NET SDK pro Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux nebo Windows |Windows (prozatím) |
| [Prostředí Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux nebo Windows |Windows |
| [Vzdálená plocha](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 a 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="pig-and-sql-server-integration-services"></a>Pig a SQL Server Integration Services

Pomocí integrace služby SSIS (SQL Server) toorun úlohu Pig. Hello Azure Feature Pack pro službu SSIS poskytuje hello následující součásti, které pracují se úlohy Pig v HDInsight.

* [Úloha Azure HDInsight Pig][pigtask]

* [Správce připojení k Azure předplatného][connectionmanager]

Další informace o hello Azure Feature Pack pro služby SSIS [sem][ssispack].

## <a id="nextsteps"></a>Další kroky
Teď, když jste se naučili, jak toouse Pig s HDInsight, hello použijte následující odkazy tooexplore jiné způsoby toowork s Azure HDInsight.

* [Nahrání dat tooHDInsight][hdinsight-upload-data]
* [Použití Hivu se službou HDInsight][hdinsight-use-hive]
* [Použití nástroje Sqoop s HDInsight](hdinsight-use-sqoop.md)
* [Použijte Oozie s HDInsight](hdinsight-use-oozie.md)
* [Použijte úlohy MapReduce s HDInsight][hdinsight-use-mapreduce]

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx


[hdinsight-upload-data]: hdinsight-upload-data.md

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
