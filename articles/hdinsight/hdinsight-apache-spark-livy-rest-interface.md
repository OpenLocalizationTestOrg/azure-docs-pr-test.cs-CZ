---
title: "cluster tooSpark aaaUse Livy Spark toosubmit úlohy v Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse Apache Spark REST API toosubmit Spark úlohy vzdáleně tooan cluster Azure HDInsight."
keywords: Apache spark rest api, livy spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2817b779-1594-486b-8759-489379ca907d
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: 417213b5f1db1522373188002fe05117faea5243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-spark-rest-api-toosubmit-remote-jobs-tooan-hdinsight-spark-cluster"></a>Použití Apache Spark REST API toosubmit vzdálené úlohy tooan clusteru HDInsight Spark

Zjistěte, jak toouse Livy hello Apache Spark REST API, které je použité toosubmit vzdálené úlohy tooan clusteru Azure HDInsight Spark. Podrobnou dokumentaci najdete v tématu [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Můžete použít Livy toorun interaktivní Spark nutný nebo odeslání dávky toobe úlohy spusťte na Spark. V tomto článku bude zmíněn pomocí Livy toosubmit dávkových úloh. Hello fragmenty kódu v tomto článku pomocí cURL toomake koncový bod REST API volání toohello Livy Spark.

**Požadavky:**

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

* [cURL](http://curl.haxx.se/). Tento článek používá cURL toodemonstrate, jakým způsobem volá toomake REST API proti clusteru služby HDInsight Spark.

## <a name="submit-a-livy-spark-batch-job"></a>Odeslání Livy Spark dávkovou úlohu
Před odesláním dávkovou úlohu, musíte nahrát hello jar aplikace v úložišti clusteru hello přidruženého k hello clusteru. Můžete použít [ **AzCopy**](../storage/common/storage-use-azcopy.md), nástroje příkazového řádku, toodo tak. Existují různé klienty tooupload data můžete použít. Můžete najít další informace o nich v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path tooapplication jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Příklady**:

* Pokud soubor jar hello v úložišti clusteru hello (WASB)
  
        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"
* Pokud chcete toopass hello jar filename a hello classname jako součást vstupní soubor (v tomto příkladu vstup.txt)
  
        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-livy-spark-batches-running-on-hello-cluster"></a>Informace o spuštěných v clusteru hello dávek Livy Spark
    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Příklady**:

* Pokud chcete tooretrieve všechny dávky Livy Spark hello běžící v clusteru s hello:
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
* Pokud chcete, aby tooretrieve konkrétní dávky s danou ID dávky
  
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="delete-a-livy-spark-batch-job"></a>Odstranit Livy Spark dávkovou úlohu
    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Příklad**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-spark-and-high-availability"></a>Livy Spark a vysoká dostupnost
Livy poskytuje vysokou dostupnost pro Spark úloh spuštěných v clusteru hello. Tady je několik příkladů.

* Pokud hello Livy služby přestane fungovat po odeslání úlohy vzdáleně tooa Spark cluster, hello úloha bude pokračovat toorun hello pozadí. Při zálohování Livy obnoví stav hello hello úlohy a sestavy zpátky.
* Poznámkové bloky Jupyter pro HDInsight jsou zapnuté pomocí Livy v back-end hello. Pokud poznámkový blok běží úlohy Spark a získá hello Livy službu restartovat, pokračuje hello Poznámkový blok buněk kód toorun hello. 

## <a name="show-me-an-example"></a>Zobrazit příklad
V této části jsme podívejte se na příklady toouse Livy Spark toosubmit dávkovou úlohu, sledovat průběh hello hello úlohy a poté jej odstraňte. Hello používáme v tomto příkladu je aplikace hello jeden vyvinuté v článku hello [vytvořit samostatná Scala aplikace a toorun na clusteru HDInsight Spark](hdinsight-apache-spark-create-standalone-application.md). Zde Hello postup předpokládá, že:

* Již jste zkopírovali přes hello aplikace jar toohello účtu úložiště přidruženého k hello clusteru.
* Máte CuRL nainstalovat hello počítače, které se pokoušíte tyto kroky.

Proveďte hello následující kroky:

1. Dejte nám nejdřív ověřte, zda Livy Spark je spuštěna v clusteru hello. Jsme to provést tím, že získáme seznam spuštěných dávky. Pokud používáte úlohu pomocí Livy pro hello poprvé, by měl vrátit výstup hello nula.
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    Měli byste obdržet výstup podobný toohello, v následující fragment kódu:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    Všimněte si, jak uvádí hello poslední řádek ve výstupu hello **celkem: 0**, která navrhuje žádné spuštěné dávky.

2. Dejte nám teď odešlete dávkovou úlohu. Následující fragment kódu Hello používá vstupní soubor (vstup.txt) toopass hello jar jméno a název třídy hello jako parametry. Pokud používáte tyto kroky ze počítači se systémem Windows, není použití souboru vstupní hello doporučenému přístupu.
   
        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"
   
    Hello parametry v souboru hello **vstup.txt** jsou definovány takto:
   
        { "file":"wasb:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
   
    Měli byste vidět výstup podobný toohello, v následující fragment kódu:
   
        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    Všimněte si, jak uvádí hello poslední řádek výstupu hello **stavu: spouštění**. Také uvádí, **id: 0**. Zde **0** je ID hello dávky.

3. Nyní můžete načíst hello stav této konkrétní batch pomocí ID hello dávky.
   
        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Měli byste vidět výstup podobný toohello, v následující fragment kódu:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    nyní zobrazuje výstup Hello **stavu: Úspěch**, která navrhuje této hello úlohy se úspěšně dokončila.

4. Pokud chcete, můžete nyní odstranit hello batch.
   
        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"
   
    Měli byste vidět výstup podobný toohello, v následující fragment kódu:
   
        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact
   
    Hello poslední řádek výstupu hello zobrazuje, že hello dávky byla úspěšně odstraněna. Odstranění úlohy, když je spuštěná, také ukončí úlohu hello. Pokud odstraníte úlohu, která byla dokončena úspěšně, nebo v opačném odstraní informace o úlohách hello úplně.

## <a name="using-livy-spark-on-hdinsight-35-clusters"></a>Pomocí Livy Spark v clusterech HDInsight 3.5

Cluster HDInsight 3.5, ve výchozím nastavení, zakáže použití místního souboru cesty tooaccess ukázkových datových souborů nebo souborů JAR. Doporučujeme vám toouse hello `wasb://` cesta místo JAR tooaccess nebo ukázková data souborů z clusteru hello. Pokud chcete toouse místní cestu, je nutné aktualizovat konfiguraci Ambari hello odpovídajícím způsobem. toodo tak:

1. Přejděte toohello Ambari portál pro hello cluster. je k dispozici v clusteru HDInsight na https:// Hello webové uživatelské rozhraní Ambari**CLUSTERNAME**. azurehdidnsight.net, kde CLUSTERNAME představuje název hello daného clusteru.

2. Z hello levé navigační, klikněte na tlačítko **Livy**a potom klikněte na **konfigurací**.

3. V části **livy výchozí** přidat název vlastnosti hello `livy.file.local-dir-whitelist` a nastavení jeho hodnoty příliš**"/"** Pokud chcete systém toofile tooallow úplný přístup. Pokud chcete tooallow přístup pouze tooa konkrétního adresáře, zadejte jako hodnotu hello hello cesta toothat adresáře.

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a>Odesílání úloh Livy pro cluster se v rámci virtuální sítě Azure

Pokud připojíte tooan clusteru HDInsight Spark z v rámci virtuální síť Azure, můžete přímo připojit tooLivy v clusteru hello. V takovém případě hello adresu URL pro koncový bod Livy `http://<IP address of hello headnode>:8998/batches`. Zde **8998** je hello port, na kterém běží Livy na headnode hello clusteru. Další informace o přístup ke službám na neveřejný portech najdete v tématu [porty používané služby Hadoop v HDInsight](hdinsight-hadoop-port-settings-for-services.md).

## <a name="troubleshooting"></a>Řešení potíží

Tady jsou některé problémy, které se mohou vyskytnout při používání Livy pro vzdálené úlohy odeslání tooSpark clustery.

### <a name="using-an-external-jar-from-hello-additional-storage-is-not-supported"></a>Použití externí jar z hello další úložiště není podporováno

**Problém:** Pokud vaše úlohy Livy Spark odkazuje externí jar z účtu hello další úložiště přidruženého k hello clusteru, úloha hello selže.

**Řešení:** Ujistěte se, že hello jar chcete toouse je k dispozici v hello výchozí úložiště přidruženého k hello clusteru HDInsight.





## <a name="next-step"></a>Další krok

* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)

