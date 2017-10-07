---
title: aaaUse MapReduce a Curl s Hadoop v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak tooremotely spuštění úloh MapReduce s Hadoop v HDInsight pomocí Curl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 16920205bacf9699f88090568099e0508a172b3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>Spuštění úloh MapReduce s Hadoop v HDInsight pomocí REST

Zjistěte, jak toouse hello WebHCat REST API toorun MapReduce úlohy systému Hadoop v clusteru HDInsight. Curl je použité toodemonstrate, jak můžete pracovat s HDInsight pomocí nezpracovaná úlohy MapReduce toorun požadavky HTTP.

> [!NOTE]
> Pokud jste již obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové tooHDInsight, přečtěte si téma hello [co potřebujete tooknow o systémem Linux Hadoop v HDInsight](hdinsight-hadoop-linux-information.md) dokumentu.


## <a id="prereq"></a>Požadavky

* Na clusteru HDInsight Hadoop
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Spuštění úloh MapReduce pomocí Curl

> [!NOTE]
> Pokud používáte Curl nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti hello tím, že poskytuje hello HDInsight clusteru správce uživatelské jméno a heslo. Hello název clusteru musí používat jako součást hello identifikátor URI, který je použité toosend hello požadavky toohello server.
>
> Hello příkazy v této části, nahraďte **uživatelské jméno** s hello uživatele tooauthenticate toohello clusteru, a **heslo** s hello heslo pro uživatelský účet hello. Nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru.
>
> Hello REST API zabezpečené pomocí [ověřování přístupu k základní](http://en.wikipedia.org/wiki/Basic_access_authentication). Vždy doporučujeme provádět požadavky pomocí protokolu HTTPS tooensure vaše přihlašovací údaje jsou odeslány bezpečně toohello serveru.


1. Z příkazového řádku použijte následující příkaz tooverify, že se můžete připojit tooyour HDInsight cluster hello:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Mělo by se zobrazit odpověď podobná toohello následující JSON:

        {"status":"ok","version":"v1"}

    Hello parametry použité v tomto příkazu jsou následující:

   * **-u**: Určuje hello uživatelské jméno a heslo použít tooauthenticate hello požadavku
   * **-G**: označuje, že tato operace je požadavek GET.

     Dobrý den zahájení hello identifikátor URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello stejné pro všechny požadavky.

2. toosubmit úlohu MapReduce hello použijte následující příkaz:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    Hello konec hello identifikátor URI (/ mapreduce/jar) informuje WebHCat, že tento požadavek spustí úlohu MapReduce z třídy v souboru jar. Hello parametry použité v tomto příkazu jsou následující:

   * **-d**: `-G` se nepoužívá, takže hello požadavek výchozí metodu POST toohello. `-d`Určuje hello hodnot dat, které jsou odesílány hello požadavku.
    * **User.Name**: hello uživateli, který spouští příkaz hello
    * **JAR**: spustili hello umístění soubor jar hello, který obsahuje toobe – třída
    * **Třída**: hello třídu, která obsahuje logiku MapReduce hello
    * **arg**: toobe argumenty hello předán toohello úlohu MapReduce. V takovém případě hello vstupní textového souboru a hello adresář, který se používají pro výstup hello

     Tento příkaz by měla vrátit ID úlohy, které můžou být použité toocheck hello stav úlohy hello:

       {"id": "job_1415651640909_0026"}

3. Stav hello toocheck hello úlohy, hello použijte následující příkaz:

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Nahraďte hello **JOBID** s hodnotou hello vráceném v předchozím kroku hello. Například pokud hello se vrátit hodnota byla `{"id":"job_1415651640909_0026"}`, pak hello JOBID by `job_1415651640909_0026`.

    Pokud se dokončí úloha hello, vrácená hello stavu `SUCCEEDED`.

   > [!NOTE]
   > Tento požadavek Curl vrátí dokumentu JSON s informacemi o úloze hello. Se používá Jq tooretrieve pouze hello hodnotu stavu.

4. Pokud stav hello hello úlohy se změnil příliš`SUCCEEDED`, výsledky hello hello úlohy můžete načíst z úložiště objektů Blob Azure. Hello `statusdir` parametr, který je předán s hello dotazu obsahuje umístění hello hello výstupního souboru. V tomto příkladu je umístění hello `/example/curl`. Tato adresa ukládá hello výstup hello úlohy v hello clustery výchozí úložiště na `/example/curl`.

Můžete zobrazit seznam a stažení těchto souborů pomocí hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Další informace o práci s objekty BLOB z hello rozhraní příkazového řádku Azure najdete v tématu hello [hello pomocí Azure CLI 2.0 s Azure Storage](../storage/common/storage-azure-cli.md#create-and-manage-blobs) dokumentu.

## <a id="nextsteps"></a>Další kroky

Obecné informace o úloh MapReduce v HDInsight:

* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)

Další informace o rozhraní REST hello, který se používá v tomto článku najdete v tématu hello [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).
