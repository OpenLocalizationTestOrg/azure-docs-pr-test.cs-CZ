---
title: aaaUse Hadoop Pig s REST v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak toouse REST toorun Pig Latin úloh na Hadoop cluster v Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ed5e10d1-4f47-459c-a0d6-7ff967b468c4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 760139e3caad9103d8c9d34e7f548d476014b5ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-rest"></a>Spuštění úlohy Pig s Hadoop v HDInsight pomocí REST

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Zjistěte, jak toorun Pig Latin úlohy tak, že cluster Azure HDInsight tooan požadavky REST. Curl je použité toodemonstrate, jak mohou komunikovat s HDInsight pomocí hello WebHCat REST API.

> [!NOTE]
> Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové tooHDInsight, přečtěte si téma [systémem Linux HDInsight tipy](hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Požadavky

* Clusteru Azure HDInsight (Hadoop v HDInsight) (systémem Linux nebo systému Windows)

  > [!IMPORTANT]
  > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [Curl](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Spuštění úlohy Pig pomocí Curl

> [!NOTE]
> Hello rozhraní API REST je zabezpečeno pomocí [ověřování přístupu k základní](http://en.wikipedia.org/wiki/Basic_access_authentication). Vždy proveďte požadavky pomocí přihlašovacích údajů jsou odeslány bezpečně toohello server tooensure Secure HTTP (HTTPS).
>
> Když používáte hello příkazy v této části, vyměňte `USERNAME` s hello uživatele tooauthenticate toohello clusteru a nahraďte `PASSWORD` s hello heslo pro uživatelský účet hello. Nahraďte `CLUSTERNAME` s hello názvem vašeho clusteru.
>


1. Z příkazového řádku použijte následující příkaz tooverify, že se můžete připojit tooyour HDInsight cluster hello:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Měli byste obdržet hello následující odpověď JSON:

        {"status":"ok","version":"v1"}

    Hello parametry použité v tomto příkazu jsou následující:

    * **-u**: hello uživatelské jméno a heslo použít tooauthenticate hello požadavku
    * **-G**: označuje, že tento požadavek je požadavek GET.

     Dobrý den, od adresy URL hello, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello stejné pro všechny požadavky. Cesta Hello **/status**, signalizuje tento požadavek hello tooreturn hello stav WebHCat (také známé jako Templeton) pro hello server.

2. Použijte následující kód toosubmit clusteru toohello úlohy Pig Latin hello:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'/example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="/example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig
    ```

    Hello parametry použité v tomto příkazu jsou následující:

    * **-d**: protože `-G` se nepoužívá, žádost hello výchozí toohello metodu POST. `-d`Určuje hello hodnot dat, které jsou odesílány hello požadavku.

    * **User.Name**: hello uživateli, který spouští příkaz hello
    * **spuštění**: hello Pig Latin příkazy tooexecute
    * **statusdir**: hello adresář, který hello stavu pro tuto úlohu je zapsán do

    > [!NOTE]
    > Všimněte si, že hello prostory v příkazech Pig Latin jsou nahrazovány hello `+` znak při použití s Curl.

    Tento příkaz by měla vrátit ID úlohy, které můžou být použité toocheck hello stav hello úlohy, například:

        {"id":"job_1415651640909_0026"}

3. Stav hello toocheck hello úlohy, hello použijte následující příkaz

     ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

     Nahraďte `JOBID` s hodnotou hello vráceném v předchozím kroku hello. Například pokud hello se vrátit hodnota byla `{"id":"job_1415651640909_0026"}`, pak `JOBID` je `job_1415651640909_0026`.

    Pokud hello úloha dokončí, je stav hello **úspěšné**.

    > [!NOTE]
    > Tento požadavek Curl vrací JavaScript Object Notation (JSON) dokumentu s informacemi o hello úlohy a jq je použité tooretrieve hello pouze hodnotu stavu.

## <a id="results"></a>Zobrazení výsledků

Pokud stav hello hello úlohy se změnil příliš**úspěšné**, můžete načíst výsledky hello hello úlohy. Hello `statusdir` parametr předaný s hello dotazu obsahuje hello umístění výstupního souboru hello; v tomto případě `/example/pigcurl`.

HDInsight, můžete použít jako úložiště dat výchozí hello Azure Storage nebo Azure Data Lake Store. Existují různé způsoby tooget v hello dat podle toho, který používáte. Další informace najdete v tématu hello úložiště v tématu hello [HDInsight se systémem Linux informace](hdinsight-hadoop-linux-information.md#hdfs-azure-storage-and-data-lake-store) dokumentu.

## <a id="summary"></a>Shrnutí

Jak je ukázáno v tomto dokumentu, můžete použít nezpracovaná toorun požadavku HTTP, monitorování a zobrazení výsledků hello Pig úloh na clusteru HDInsight.

Další informace o rozhraní REST hello používané v tomto článku najdete v tématu hello [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

## <a id="nextsteps"></a>Další kroky

Obecné informace o Pig v HDInsight:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)
