---
title: "aaaGet začít s příklad HBase v HDInsight - Azure | Microsoft Docs"
description: "Postupujte podle tohoto toostart příklad Apache HBase pomocí hadoop v HDInsight. Vytvářejte tabulky z hello prostředí HBase a dotazujte je pomocí Hive."
keywords: "příkaz hbase,příklad hbase"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 4d6a2658-6b19-4268-95ee-822890f5a33a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 43419780142b320b16180a2b1f25020dee2f7a11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a>Začínáme s příkladem Apache HBase ve službě HDInsight

Zjistěte, jak toocreate cluster HBase v HDInsight, vytvářet tabulky HBase a dotazovat tabulky pomocí Hive. Obecné informace o HBase najdete v tématu [Přehled HBase ve službě HDInsight][hdinsight-hbase-overview].

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Požadavky
Než začnete při tomto příkladu HBase, musíte mít hello následující položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Secure Shell (SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
* [curl](http://curl.haxx.se/download.html).

## <a name="create-hbase-cluster"></a>Vytvoření clusteru HBase
Hello následující postup používá toocreate šablony Azure Resource Manager verze 3.4 systémem Linux HBase clusteru a hello závislé výchozí účet úložiště Azure. toounderstand hello parametrů použitých v postupu hello a ostatní metody tvorby clusteru najdete v části [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klikněte na tlačítko hello následující šablony hello tooopen bitové kopie v hello portálu Azure. Šablona Hello je umístěna v kontejneru veřejného objektu blob. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Z hello **vlastní nasazení** okno, zadejte hello následující hodnoty:
   
   * **Předplatné**: Vyberte předplatné Azure, který je použité toocreate hello clusteru.
   * **Skupina prostředků:** Vytvořte skupinu správy prostředků Azure nebo použijte již existující.
   * **Umístění**: Zadejte hello umístění skupiny prostředků hello. 
   * **Název clusteru**: Zadejte název pro hello HBase cluster.
   * **Přihlašovací jméno a heslo clusteru**: hello výchozí přihlašovací jméno je **správce**.
   * **SSH uživatelské jméno a heslo**: výchozí uživatelské jméno hello **sshuser**.  Můžete ho změnit.
     
     Další parametry jsou volitelné.  
     
     Každý cluster obsahuje závislost účtu Azure Storage. Po odstranění clusteru s podporou hello data zachovají hello účet úložiště. Hello clusteru výchozí název účtu úložiště je název clusteru hello s "úložiště" připojí. Je pevně kódovaný v části proměnných šablony hello.
3. Vyberte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**a potom klikněte na **nákupu**. Trvá přibližně 20 minut toocreate cluster.

> [!NOTE]
> Po odstranění clusteru služby HBase můžete vytvořit jiný cluster HBase pomocí hello stejného výchozího kontejneru blob. Hello nový cluster převezme tabulky HBase hello, kterou jste vytvořili v původním clusteru hello. tooavoid nekonzistence, doporučujeme zakázat hello tabulek HBase, před odstraněním clusteru hello.
> 
> 

## <a name="create-tables-and-insert-data"></a>Vytváření tabulek a vkládání dat
Pomocí SSH tooconnect tooHBase clusterů a potom pomocí tabulky HBase toocreate prostředí HBase, vkládání dat a dotaz na data. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

Pro většinu osob se data zobrazí v tabulkovém formátu hello:

![Tabulková data HDInsight HBase][img-hbase-sample-data-tabular]

V HBase (implementaci BigTable), hello stejné data vypadá jako:

![Velké objemy tabulkových dat HDInsight HBase][img-hbase-sample-data-bigtable]


**toouse hello prostředí HBase**

1. Ze SSH spusťte následující příkaz HBase hello:
   
    ```bash
    hbase shell
    ```

2. Vytvořte HBase se skupinami o dvou sloupcích:

    ```hbaseshell   
    create 'Contacts', 'Personal', 'Office'
    list
    ```
3. Vložte některá data:
    
    ```hbaseshell   
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    scan 'Contacts'
    ```
   
    ![Prostředí HDInsight Hadoop HBase][img-hbase-shell]
4. Získání jednoho řádku
   
    ```hbaseshell
    get 'Contacts', '1000'
    ```
   
    Zobrazí se hello stejné výsledky jako pomocí příkazu hello vyhledávání, protože existuje pouze jeden řádek.
   
    Další informace o schématu tabulky HBase hello najdete v tématu [Úvod tooHBase návrhu schématu][hbase-schema]. Další příkazy HBase najdete v tématu [Referenční příručka Apache HBase][hbase-quick-start].
5. Ukončete prostředí hello
   
    ```hbaseshell
    exit
    ```

**toobulk načítání dat do tabulky kontaktů HBase hello**

HBase obsahuje několik metod načítání dat do tabulek.  Další informace naleznete v tématu [Hromadné načítání](http://hbase.apache.org/book.html#arch.bulk.load).

Ukázkový datový soubor najdete ve veřejném kontejneru objektů blob: *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  obsah Hello hello datového souboru je:

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

Volitelně můžete vytvořte textový soubor a nahrajte hello soubor tooyour úložiště vlastní účet. Hello pokyny najdete v tématu [nahrávání dat pro úlohy Hadoop do HDInsight][hdinsight-upload-data].

> [!NOTE]
> Tento postup používá tabulku kontaktů HBase hello, které jste vytvořili v posledním postupu hello.
> 

1. Ze SSH spusťte následující příkaz tootransform hello dat souboru tooStoreFiles a uložit do relativní cesty určené položkou Dimporttsv.bulk.output hello.  Pokud jste v prostředí HBase, použijte hello ukončení příkazu tooexit.

    ```bash   
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. Spusťte následující příkaz tooupload hello data z tabulky HBase toohello/storedatafileoutput hello:
   
    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. Můžete otevřít hello prostředí HBase a použít hello kontroly příkaz toolist hello obsahu tabulky.

## <a name="use-hive-tooquery-hbase"></a>Použijte Hive tooquery HBase

Data v tabulkách HBase můžete dotazovat pomocí Hive. V této části vytvoříte tabulku Hive, mapuje toohello tabulky HBase a použije tooquery hello data v tabulce HBase.

1. Otevřete **PuTTY**a připojte toohello cluster.  Hello pokyny naleznete v předchozím postupu hello.
2. Z relace SSH hello použijte následující příkaz toostart Beeline hello:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    Další informace o Beeline najdete v tématu [Použití Hivu s Hadoopem ve službě HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md).
       
3. Spusťte následující skript toocreate HiveQL hello tabulku Hive, která mapuje toohello tabulky HBase. Ujistěte se, že jste vytvořili hello ukázkové tabulky odkazované dříve v tomto kurzu pomocí prostředí HBase hello před spuštěním tohoto prohlášení.

    ```hiveql   
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

4. Spusťte následující HiveQL skriptu tooquery hello data v tabulce HBase hello hello:

    ```hiveql   
    SELECT count(rowkey) FROM hbasecontacts;
    ```

## <a name="use-hbase-rest-apis-using-curl"></a>Použití rozhraní REST API HBase pomocí Curl

Hello rozhraní API REST je zabezpečeno pomocí [základní ověřování](http://en.wikipedia.org/wiki/Basic_access_authentication). Požadavky se vždy provádět pomocí HTTPS (Secure HTTP) toohelp Ujistěte se, že vaše přihlašovací údaje jsou odeslány bezpečně toohello serveru.

2. Použijte následující příkaz toolist hello existujících tabulek HBase hello:

    ```bash
    curl -u <UserName>:<Password> \
    -G https://<ClusterName>.azurehdinsight.net/hbaserest/
    ```

3. Použijte následující příkaz toocreate novou tabulku HBase se dvěma sloupci rodiny hello:

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    schéma Hello je k dispozici ve formátu JSon hello.
4. Použijte následující příkaz tooinsert hello některá data:

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```
   
    Je nutné base64 kódování hello hodnoty zadané v přepínači -d hello. V příkladu hello:
   
   * MTAwMA==: 1000
   * UGVyc29uYWw6TmFtZQ==: Personal:Name
   * Sm9obiBEb2xl: John Dole
     
     [klíč řádku false](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) vám umožní tooinsert více hodnot (dávkové).
5. Použijte následující příkaz tooget řádek hello:
   
    ```bash 
    curl -u <UserName>:<Password> \
    -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

Další informace o HBase Rest naleznete v tématu [Referenční příručka Apache HBase](https://hbase.apache.org/book.html#_rest).

> [!NOTE]
> Thrift není podporovaný HBase v HDInsight.
>
> Pokud používáte Curl nebo jinou komunikaci REST s WebHCat, je třeba ověřit žádosti hello zadáním hello uživatelské jméno a heslo pro správce clusteru HDInsight hello. Název clusteru hello musíte také použít jako součást hello identifikátor URI (Uniform Resource) použít toosend hello požadavky toohello serveru:
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    Mělo by se zobrazit odpověď podobná toohello následující odpověď:
>   
>        {"status":"ok","version":"v1"}
   


## <a name="check-cluster-status"></a>Kontrola stavu clusteru
HBase v HDInsight se dodává s webovým uživatelským rozhraním pro sledování clusterů. Pomocí hello webového uživatelského rozhraní, můžete požádat statistické údaje nebo informace o oblastech.

**hello tooaccess HBase hlavního uživatelského rozhraní**

1. Přihlaste se k hello hello webové uživatelské rozhraní Ambari na https://&lt;Clustername >. azurehdinsight.net.
2. Klikněte na tlačítko **HBase** hello levé nabídce.
3. Klikněte na tlačítko **rychlé odkazy** hello horní části stránky hello, bod toohello active Zookeeper uzel propojení a pak klikněte na tlačítko **HBase hlavního uživatelského rozhraní**.  Hello uživatelského rozhraní je otevřen v jiné kartě prohlížeče:

  ![Hlavní uživatelské rozhraní HDInsight HBase](./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

  Hello HBase hlavního uživatelského rozhraní obsahuje hello následující části:

  - oblastní servery
  - zálohování hlavních serverů
  - tabulky
  - úlohy
  - atributy softwaru

## <a name="delete-hello-cluster"></a>Odstranění clusteru hello
tooavoid nekonzistence, doporučujeme zakázat hello tabulek HBase, před odstraněním clusteru hello.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli, jak hello toocreate HBase cluster a jak toocreate tabulek a zobrazení hello data v těchto tabulkách z prostředí HBase. Také jste zjistili, jak toouse podregistru dotazování na data v tabulkách HBase a jak toouse hello REST API HBase C# toocreate tabulky HBase a načtení dat z tabulky hello.

toolearn více, najdete v části:

* [Přehled HBase ve službě HDInsight][hdinsight-hbase-overview]: HBase je NoSQL open source databáze Apache postavená na systému Hadoop, která poskytuje náhodný přístup a silnou konzistenci pro velké objemy nestrukturovaných a částečně strukturovaných dat.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
