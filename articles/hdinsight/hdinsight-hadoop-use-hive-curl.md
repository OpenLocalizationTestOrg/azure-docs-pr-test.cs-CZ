---
title: aaaUse Hadoop Hive s Curl v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak odeslat tooremotely Pig úlohy tooHDInsight pomocí Curl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: e725829ad2adcf3540f44375e3e87b7cdaebd15e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>Spouštění dotazů Hive se systémem Hadoop v HDInsight pomocí REST

[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Zjistěte, jak toouse hello WebHCat REST API toorun Hive dotazy s Hadoop v clusteru Azure HDInsight.

[Curl](http://curl.haxx.se/) je použité toodemonstrate, jak můžete pracovat s HDInsight pomocí nezpracované požadavky HTTP. Hello [jq](http://stedolan.github.io/jq/) nástroj je použité tooprocess hello JSON data vrácená z požadavky REST.

> [!NOTE]
> Pokud jste obeznámeni s pomocí serverů se systémem Linux Hadoop, ale jsou nové tooHDInsight, přečtěte si téma hello [co potřebujete tooknow o Hadoop na HDInsight se systémem Linux](hdinsight-hadoop-linux-information.md) dokumentu.

## <a id="curl"></a>Spouštění dotazů Hive

> [!NOTE]
> Pokud používáte cURL nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti hello zadáním hello uživatelské jméno a heslo pro správce clusteru HDInsight hello.
>
> Hello příkazy v této části, nahraďte **uživatelské jméno** s hello uživatele tooauthenticate toohello clusteru a nahraďte **heslo** s hello heslo pro uživatelský účet hello. Nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru.
>
> Hello rozhraní API REST je zabezpečeno pomocí [základní ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication). toohelp Ujistěte se, že vaše přihlašovací údaje jsou bezpečně odesílá serveru toohello, vždy provádět požadavky pomocí Secure HTTP (HTTPS).

1. Z příkazového řádku použijte následující příkaz tooverify, že se můžete připojit tooyour HDInsight cluster hello:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Zobrazí se podobné toohello odpovědi následující text:

        {"status":"ok","version":"v1"}

    Hello parametry použité v tomto příkazu jsou následující:

   * **-u** -hello uživatelské jméno a heslo použít tooauthenticate hello požadavku.
   * **-G** – označuje, že tento požadavek je operaci GET.

     Dobrý den, od adresy URL hello, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, hello stejné pro všechny požadavky. Cesta Hello **/status**, signalizuje tento požadavek hello tooreturn stav WebHCat (také známé jako Templeton) pro hello server. Můžete také vyžadovat hello verzi Hive pomocí hello následující příkaz:

    ```bash
    curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

     Tento požadavek vrátí odpověď podobnou toohello následující text:

       {"module": "hive", "verze": "0.13.0.2.1.6.0-2103"}

2. Hello použijte následující tabulku s názvem toocreate **log4jLogs**:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Hello následující parametry použité s touto žádostí:

   * **-d** – od `-G` se nepoužívá, žádost hello výchozí toohello metodu POST. `-d`Určuje hello hodnot dat, které jsou odesílány hello požadavku.

     * **User.Name** -hello uživatele, který spouští příkaz hello.
     * **spuštění** -hello tooexecute příkazy HiveQL.
     * **statusdir** -hello adresář, který hello stavu pro tuto úlohu je zapsán do.

     Tyto příkazy provádět hello následující akce:
   * **VYŘAĎTE tabulku** -hello tabulka již existuje, je odstraněn.
   * **Vytvoření externí tabulky** -vytvoří novou tabulku "externí" v Hive. Externí tabulky uložit jenom hello Definice tabulky Hive. Hello data je ponechán v hello původního umístění.

     > [!NOTE]
     > Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje. Například automatizované data nahrát procesu nebo jiná operace MapReduce.
     >
     > Vyřazení externí tabulku nemá **není** odstranit hello data, jenom hello definici tabulky.

   * **Řádek formátu** – jak hello datového naformátován. Hello pole v každém protokolu jsou oddělené mezerou.
   * **ULOŽENÉ umístění textový soubor AS** – tam, kde jsou uložena hello data (adresář hello příklad nebo data) a která je uložena jako text.
   * **Vyberte** -počet všech řádků vybere kde sloupec **t4** obsahuje hodnotu hello **[Chyba]**. Tento příkaz vrátí hodnotu **3** jsou tři řádky, které obsahují tuto hodnotu.

     > [!NOTE]
     > Všimněte si, že hello mezery mezi příkazy HiveQL jsou nahrazovány hello `+` znak při použití s Curl. Hodnoty v uvozovkách, které obsahují mezery, jako je například hello oddělovač, by neměl být nahrazen `+`.

   * **INPUT__FILE__NAME jako '% 25.log'** – tento příkaz omezení hello vyhledávání tooonly používané soubory končící na. log.

     > [!NOTE]
     > Hello `%25` je hello kódovaná adresou URL formuláře %, je skutečný stav hello `like '%.log'`. Hello % má adresa URL toobe kódování, jako je považován za zvláštní znak v adresách URL.

     Tento příkaz by měl vrátit ID úlohy, které můžou být použité toocheck hello stav úlohy hello.

       {"id": "job_1415651640909_0026"}

3. Stav hello toocheck hello úlohy, hello použijte následující příkaz:

    ```bash
    curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Nahraďte **JOBID** s hodnotou hello vráceném v předchozím kroku hello. Například pokud hello se vrátit hodnota byla `{"id":"job_1415651640909_0026"}`, pak **JOBID** by `job_1415651640909_0026`.

    Pokud hello úloha dokončí, je stav hello **úspěšné**.

   > [!NOTE]
   > Tento požadavek Curl vrátí dokument JavaScript Object Notation (JSON) s informace o úloze hello. Se používá Jq tooretrieve pouze hello hodnotu stavu.

4. Jakmile se stav hello hello úlohy se změnil příliš**úspěšné**, výsledky hello hello úlohy můžete načíst z úložiště objektů Blob Azure. Hello `statusdir` parametr předaný s hello dotazu obsahuje hello umístění výstupního souboru hello; v tomto případě **/příklad/curl**. Tato adresa ukládá hello výstup hello **příklad/curl** adresáře v hello clusterů výchozí úložiště.

    Můžete zobrazit seznam a stažení těchto souborů pomocí hello [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). Další informace o používání hello rozhraní příkazového řádku Azure s Azure Storage najdete v tématu hello [použití Azure CLI 2.0 s Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli#create-and-manage-blobs) dokumentu.

5. Hello použijte následující příkazy toocreate novou "interní" tabulku s názvem **errorLogs**:

    ```bash
    curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Tyto příkazy provádět hello následující akce:

   * **Vytvoření tabulka není v případě existuje** -vytvoří tabulku, pokud ještě neexistuje. Tento příkaz vytvoří interní tabulku, která je uložena v datovém skladu hello Hive a je zcela spravuje Hive.

     > [!NOTE]
     > Na rozdíl od externích tabulek se odstranit interní tabulku odstraní hello základní data.

   * **ULOŽENÉ ORC AS** – ukládá hello data ve formátu optimalizované řádek sloupcovém (ORC). ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.
   * **PŘEPSAT INSERT... Vyberte** -vybere řádky z hello **log4jLogs** tabulku, která obsahují **[Chyba]**, pak vloží hello data do hello **errorLogs** tabulky.
   * **Vyberte** -vybere všechny řádky z hello nové **errorLogs** tabulky.

6. Pomocí ID úlohy hello vrátil stav hello toocheck hello úlohy. Jakmile ho proběhla úspěšně, použijte hello rozhraní příkazového řádku Azure, jak je popsáno výše toodownload a zobrazení výsledků hello. výstup Hello by měla obsahovat tři řádky, které obsahují **[Chyba]**.

## <a id="nextsteps"></a>Další kroky

Obecné informace o Hive s HDInsight:

* [Použijte Hive s Hadoop v HDInsight](hdinsight-use-hive.md)

Informace o jiných způsobech můžete pracovat s Hadoop v HDInsight:

* [Použijte Pig s Hadoop v HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s Hadoop v HDInsight](hdinsight-use-mapreduce.md)

Pokud používáte Tez s Hive, zobrazit hello následující dokumenty pro ladění informace:

* [Použití hello zobrazení Ambari Tez na HDInsight se systémem Linux](hdinsight-debug-ambari-tez-view.md)

Další informace o hello REST API, které jsou v tomto dokumentu najdete v tématu hello [WebHCat odkaz](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) dokumentu.

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


