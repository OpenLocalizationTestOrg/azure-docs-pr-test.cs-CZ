---
title: aaaUse Hadoop Sqoop s Curl v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak odeslat tooremotely tooHDInsight Sqoop úlohy pomocí Curl."
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
ms.openlocfilehash: d9c09a6704ab6c5f48be50ed6d6314ec406df8ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Spuštění úloh Sqoop se systémem Hadoop v prostředí HDInsight pomocí Curl
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Zjistěte, jak clusteru toouse Curl toorun Sqoop úloh na Hadoop v HDInsight.

Curl je použité toodemonstrate, jak můžete pracovat s HDInsight pomocí nezpracovaná toorun požadavky HTTP, monitorování a načíst výsledky hello Sqoop úloh. Tento postup funguje s použitím hello WebHCat rozhraní API REST (dříve označované jako Templeton) poskytované clusteru HDInsight.

> [!NOTE]
> Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové tooHDInsight, přečtěte si téma [informace o používání HDInsight v Linuxu](hdinsight-hadoop-linux-information.md).
> 
> 

## <a name="prerequisites"></a>Požadavky
toocomplete hello kroky v tomto článku, budete potřebovat následující hello:

* Hadoop v clusteru HDInsight (Linux nebo na základě Windows)
* [Curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a name="submit-sqoop-jobs-by-using-curl"></a>Odesílání úloh Sqoop pomocí Curl
> [!NOTE]
> Pokud používáte Curl nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti hello zadáním hello uživatelské jméno a heslo pro správce clusteru HDInsight hello. Název clusteru hello musíte použít také jako součást hello identifikátor URI (Uniform Resource) použít toosend hello požadavky toohello serveru.
> 
> Hello příkazy v této části, nahraďte **uživatelské jméno** s hello uživatele tooauthenticate toohello clusteru a nahraďte **heslo** s hello heslo pro uživatelský účet hello. Nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru.
> 
> Hello rozhraní API REST je zabezpečeno pomocí [základní ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication). Vždy doporučujeme provádět požadavky pomocí HTTPS (Secure HTTP) toohelp Ujistěte se, že vaše přihlašovací údaje jsou odeslány bezpečně toohello serveru.
> 
> 

1. Z příkazového řádku použijte následující příkaz tooverify, že se můžete připojit tooyour HDInsight cluster hello:
   
        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
   
    Měli byste obdržet a odpovědi podobné toohello následující:
   
        {"status":"ok","version":"v1"}
   
    Hello parametry použité v tomto příkazu jsou následující:
   
   * **-u** -hello uživatelské jméno a heslo použít tooauthenticate hello požadavku.
   * **-G** – označuje, že se jedná o požadavek GET.
     
     Dobrý den, od adresy URL hello, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, bude hello stejné pro všechny požadavky. Cesta Hello **/status**, signalizuje tento požadavek hello tooreturn stav WebHCat (také známé jako Templeton) pro hello server. 
2. Použijte následující toosubmit úlohu sqoop hello:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Hello parametry použité v tomto příkazu jsou následující:

    * **-d** – od `-G` se nepoužívá, žádost hello výchozí toohello metodu POST. `-d`Určuje hello hodnot dat, které jsou odesílány hello požadavku.

        * **User.Name** -hello uživatele, který spouští příkaz hello.

        * **příkaz** -hello tooexecute příkaz Sqoop.

        * **statusdir** -hello adresář, který hello stavu pro tuto úlohu se zapíšou do.

    Tento příkaz by měl vrátit ID úlohy, které můžou být použité toocheck hello stav úlohy hello.

        {"id":"job_1415651640909_0026"}

1. Stav hello toocheck hello úlohy, hello použijte následující příkaz. Nahraďte **JOBID** s hodnotou hello vráceném v předchozím kroku hello. Například pokud hello se vrátit hodnota byla `{"id":"job_1415651640909_0026"}`, pak **JOBID** by `job_1415651640909_0026`.
   
        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
   
    Pokud hello úloha dokončí, bude mít stav hello **úspěšné**.
   
   > [!NOTE]
   > Tento požadavek Curl vrátí dokument JavaScript Object Notation (JSON) s informacemi o úloze hello; se používá jq tooretrieve pouze hello hodnotu stavu.
   > 
   > 
2. Jakmile se stav hello hello úlohy se změnil příliš**úspěšné**, výsledky hello hello úlohy můžete načíst z úložiště objektů Blob Azure. Hello `statusdir` parametr předaný s hello dotazu obsahuje hello umístění výstupního souboru hello; v tomto případě **wasb: / / / Příklad/curl**. Tato adresa ukládá hello výstup hello úlohy v hello **příklad/curl** v hello výchozí kontejner úložiště používané clusteru HDInsight.
   
    Můžete zobrazit seznam a stažení těchto souborů pomocí hello [rozhraní příkazového řádku Azure](../cli-install-nodejs.md). Například soubory toolist **příklad/curl**, použijte následující příkaz hello:
   
        azure storage blob list <container-name> example/curl
   
    toodownload soubor, použijte následující hello:
   
        azure storage blob download <container-name> <blob-name> <destination-file>
   
   > [!NOTE]
   > Buď musíte zadat název účtu úložiště hello, která obsahuje objekt blob hello pomocí hello `-a` a `-k` parametry, nebo sadu hello **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_přístup\_klíč** proměnné prostředí. Najdete v části < href = "hdinsight data.md nahrávání" target = "_blank" Další informace.
   > 
   > 

## <a name="limitations"></a>Omezení
* Hromadně export - s Linuxovým systémem HDInsight, hello Sqoop konektor používaný tooexport data tooMicrosoft systému SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.
* Dávkování - s HDInsight se systémem Linux, při použití hello `-batch` přepnout při vložení, Sqoop provede několik vloží místo dávkování operace insert hello.

## <a name="summary"></a>Souhrn
Jak je ukázáno v tomto dokumentu, můžete použít nezpracovaná toorun požadavku HTTP, monitorování a zobrazení výsledků hello Sqoop úloh na clusteru HDInsight.

Další informace o rozhraní REST hello používané v tomto článku najdete v tématu hello <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST API průvodce</a>.

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


