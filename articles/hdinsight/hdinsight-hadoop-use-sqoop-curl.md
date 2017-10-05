---
title: "Použití nástroje Hadoop Sqoop se Curl v HDInsight - Azure | Microsoft Docs"
description: "Naučte se vzdáleně odesílání úloh Sqoop do HDInsight pomocí Curl."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 39798321-78ca-428c-bcfe-322e49af4059
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 0975aedf58c6e110726dd3308eae5f9ad3907cc7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Spuštění úloh Sqoop se systémem Hadoop v prostředí HDInsight pomocí Curl
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Další informace o použití Curl ke spuštění úloh Sqoop na cluster Hadoop v HDInsight.

Curl se používá k ukazují, jak můžete pracovat s HDInsight pomocí nezpracované požadavky HTTP na spouštět, monitorovat a načíst výsledky úlohy Sqoop. To funguje tak, že pomocí WebHCat REST API (dříve označované jako Templeton) poskytované clusteru HDInsight.

> [!NOTE]
> Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové do HDInsight, projděte si téma [informace o používání HDInsight v Linuxu](hdinsight-hadoop-linux-information.md).
> 
> 

## <a name="prerequisites"></a>Požadavky
Pokud chcete provést kroky v tomto článku, budete potřebovat následující:

* Hadoop v clusteru HDInsight (Linux nebo na základě Windows)
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a name="submit-sqoop-jobs-by-using-curl"></a>Odesílání úloh Sqoop pomocí Curl
> [!NOTE]
> Pokud používáte Curl nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti zadáním uživatelského jména a hesla pro správce clusteru HDInsight. Název clusteru také musíte použít jako součást identifikátoru URI (Uniform Resource Identifier) sloužícímu k odesílání požadavků na server.
> 
> Pro příkazy v této části nahraďte **UŽIVATELSKÉ JMÉNO** uživatelem pro ověření do clusteru a nahraďte **HESLO** heslem pro uživatelský účet. Nahraďte **CLUSTERNAME** názvem vašeho clusteru.
> 
> Rozhraní API REST je zabezpečeno pomocí [základního ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication). Vždy doporučujeme provádět požadavky pomocí protokolu HTTPS (Secure HTTP) a pomoci tak zajistit, že přihlašovací údaje budou na server odeslány bezpečně.
> 
> 

1. Z příkazového řádku použijte následující příkaz k ověření, zda se můžete připojit ke clusteru HDInsight:
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    Měla by se zobrazit odpověď podobná následujícímu:
   
        {"status":"ok","version":"v1"}
   
    Parametry použité v tomto příkazu jsou následující:
   
   * **-u** – uživatelské jméno a heslo použité pro ověření žádosti.
   * **-G** – označuje, že se jedná o požadavek GET.
     
     Na začátek adresu URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, budou stejné pro všechny požadavky. Cesta **/status**, označuje, že požadavek je vrátit stav WebHCat (také známé jako Templeton) pro server. 
2. Použijte následující postupy se odeslat úlohu sqoop:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Parametry použité v tomto příkazu jsou následující:

    * **-d** – od `-G` se nepoužívá, použije se výchozí hodnota žádost na metodu POST. `-d`Určuje datových hodnot, které se odesílají s požadavkem.

        * **User.Name** -uživatel, který spouští příkaz.

        * **příkaz** -Sqoop příkaz k provedení.

        * **statusdir** -adresáře, který budou zapisovat do stavu pro tuto úlohu.

    Tento příkaz by měl vrátit ID úlohy, který slouží ke kontrole stavu úlohy.

        {"id":"job_1415651640909_0026"}

1. Chcete-li zkontrolovat stav úlohy, použijte následující příkaz. Nahraďte **JOBID** se hodnota vrácená v předchozím kroku. Například, pokud byl návratovou hodnotu `{"id":"job_1415651640909_0026"}`, pak **JOBID** by `job_1415651640909_0026`.
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    Pokud se úloha dokončí, bude stav **úspěšné**.
   
   > [!NOTE]
   > Tento požadavek Curl vrátí dokument JavaScript Object Notation (JSON) s informacemi o úloze; jq se používá k načtení jenom hodnotu stavu.
   > 
   > 
2. Jakmile se stav úlohy se změnila na **úspěšné**, můžete načíst výsledky úlohy z Azure Blob storage. `statusdir` Parametr předaný s dotaz obsahuje umístění výstupního souboru; v tomto případě **wasb: / / / Příklad/curl**. Tato adresa ukládá výstup úlohy v **příklad/curl** adresář na výchozí kontejner úložiště používané clusteru HDInsight.
   
    Můžete zobrazit seznam a stáhnout tyto soubory pomocí [rozhraní příkazového řádku Azure](../cli-install-nodejs.md). Například pro zobrazení seznamu souborů v **příklad/curl**, použijte následující příkaz:
   
        azure storage blob list <container-name> example/curl
   
    Ke stažení souboru, použijte tento příkaz:
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > Buď musíte zadat název účtu úložiště, který obsahuje objekt blob pomocí `-a` a `-k` parametry, nebo sady **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_přístup\_klíč** proměnné prostředí. Najdete v části < href = "hdinsight data.md nahrávání" target = "_blank" Další informace.
   > 
   > 

## <a name="limitations"></a>Omezení
* Hromadné export - s Linuxovým systémem HDInsight, Sqoop konektor umožňuje exportovat data do systému Microsoft SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.
* Dávkování - s HDInsight se systémem Linux, při použití `-batch` přepnout při vložení, Sqoop provede několik vloží místo dávkování operace insert.

## <a name="summary"></a>Souhrn
Jak je ukázáno v tomto dokumentu, můžete spouštět, monitorovat a zobrazit výsledky Sqoop úloh na clusteru HDInsight nezpracovaná požadavek HTTP.

Další informace o rozhraní REST používané v tomto článku najdete v tématu <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API průvodce</a>.

## <a name="next-steps"></a>Další kroky
Obecné informace o Hive s HDInsight:

* [Použití nástroje Sqoop se systémem Hadoop v HDInsight](hdinsight-use-sqoop.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)
* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


