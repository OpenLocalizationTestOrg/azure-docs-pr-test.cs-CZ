---
title: "Akce skriptu tooinstall aaaUse Solr na HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall Solr na systémem Linux HDInsight Hadoop clustery pomocí akcí skriptů."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cc93ed5c-a358-456a-91a4-f179185c0e98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: 4c179032b95ae187f1830d8927f8796372fa8ebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Na nainstalovat a používat Solr clusterů systému HDInsight Hadoop

Zjistěte, jak tooinstall Solr v Azure HDInsight pomocí akce skriptu. Solr je platforma pro efektivní vyhledávání a poskytuje možnosti vyhledávání na úrovni podniku na data spravovaná přes Hadoop.

> [!IMPORTANT]
    > Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!IMPORTANT]
> v tomto dokumentu Hello ukázkový skript nainstaluje Solr 4.9 s konkrétní konfigurací. Pokud chcete tooconfigure hello Solr clusteru pomocí různých kolekcí, horizontálních oddílů, schémata, repliky, atd., je třeba upravit hello skriptu a Solr binární soubory.

## <a name="whatis"></a>Co je Solr

[Apache Solr](http://lucene.apache.org/solr/features.html) je platforma enterprise vyhledávání, která umožňuje efektivní fulltextové vyhledávání na data. Při Hadoop umožňuje ukládání a správě obrovské množství dat, Apache Solr poskytuje možnosti vyhledávání hello tooquickly načtení hello data.

> [!WARNING]
> Microsoft podporuje součásti, které jsou součástí clusteru HDInsight hello.
>
> Vlastní komponenty, například Solr, přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello. Podporu společnosti Microsoft nemusí být možné tooresolve problémy s vlastními komponentami. O pomoc může být nutné tooengage hello s otevřeným zdrojem komunit. Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).

## <a name="what-hello-script-does"></a>Jaké skript hello

Tento skript vytvoří hello následující clusteru HDInsight toohello změny:

* Nainstaluje Solr 4.9 do`/usr/hdp/current/solr`
* Vytvoří uživatele, **solrusr**, což je použité toorun hello Solr služby
* Nastaví **solruser** jako vlastník hello`/usr/hdp/current/solr`
* Přidá [Upstart](http://upstart.ubuntu.com/) konfigurace, který se automaticky spouští Solr.

## <a name="install"></a>Nainstalujte Solr pomocí akcí skriptů

Tooinstall skriptu ukázka Solr v clusteru HDInsight je k dispozici na hello následující umístění:

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

toocreate cluster, který má nainstalovaný Solr hello použijte kroky v hello [Tvorba clusterů HDInsight](hdinsight-hadoop-create-linux-clusters-portal.md) dokumentu. Během procesu vytváření hello použijte následující postup tooinstall Solr hello:

1. Z hello __clusteru Souhrn__ okno, select__Advanced settings__, pak __skript akce__. Použijte následující informace toopopulate hello formuláře hello:

   * **NÁZEV**: Zadejte popisný název akce skriptu hello.
   * **Identifikátor URI skriptu**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh
   * **HEAD**: zaškrtnete tuto možnost,
   * **PRACOVNÍ**: zaškrtnete tuto možnost,
   * **ZOOKEEPER**: Zkontrolujte tuto možnost tooinstall na uzlu Zookeeper hello
   * **Parametry**: to pole ponechat prázdné

2. Na konci hello hello **skript akce** okno, použijte hello **vyberte** tlačítko toosave hello konfigurace. Nakonec použijte hello **Další** tlačítko tooreturn toohello __souhrn clusteru__

3. Z hello __clusteru Souhrn__ vyberte __vytvořit__ toocreate hello clusteru.

## <a name="usesolr"></a>Jak používat Solr v prostředí HDInsight

> [!IMPORTANT]
> Hello kroky v této části ukazují základní funkce Solr. Další informace o použití Solr, najdete v části hello [Apache Solr lokality](http://lucene.apache.org/solr/).

### <a name="index-data"></a>Data indexu

Použijte následující kroky tooadd příklad dat tooSolr hello a pak zadat dotaz:

1. Připojte toohello clusteru HDInsight pomocí protokolu SSH:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

     > [!IMPORTANT]
     > Kroky později v tomto dokumentu používají SSL tunel tooconnect toohello Solr webového uživatelského rozhraní. toouse tyto kroky, je potřeba vytvořit protokolem SSL tunelování a pak proveďte konfiguraci vašeho prohlížeče toouse ho.
     >
     > Další informace najdete v tématu hello [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.

2. Použijte následující příkazy toohave Solr index ukázková data hello:

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    Hello následující výstup se vrátí toohello konzoly:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Hello `post.jar` nástroj přidá hello **solr.xml** a **monitor.xml** dokumenty toohello index.
  
3. Použijte následující příkaz tooquery hello Solr REST API hello:

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    Tento příkaz vyhledá **collection1** pro všechny dokumenty odpovídající  **\*:\***  (kódovaná jako \*% 3A\* v řetězci dotazu hello). Hello následující dokumentu JSON je příklad hello odpovědi:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, hello Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication tooother Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over hello e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }

### <a name="using-hello-solr-dashboard"></a>Pomocí řídicího panelu Solr hello

řídicí panel Solr Hello je webové uživatelské rozhraní, které vám umožní toowork s Solr prostřednictvím webového prohlížeče. řídicí panel Solr Hello není zveřejněné přímo na hello Internetu z clusteru HDInsight. Můžete použít tooaccess tunelového propojení SSH ho. Další informace o používání tunelového propojení SSH naleznete v části hello [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.

Po vytvoření tunelu SSH použijte následující postup toouse hello Solr řídicí panel hello:

1. Určete hello název hostitele pro primární headnode hello:

   1. Použití SSH tooconnect toohello hlavního uzlu clusteru. Například, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

       Další informace o používání SSH najdete v tématu hello [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

   2. Použijte následující příkaz tooget hello plně kvalifikovaný název hostitele hello:

        ```bash
        hostname -f
        ```

        Tento příkaz vrátí hodnotu podobné toohello následující název hostitele:

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        Hello hodnota vrácená, uložte, protože se později používá.

2. V prohlížeči připojit příliš**http://HOSTNAME:8983/solr / #/**, kde **HOSTNAME** je název hello určit v předchozích krocích hello.

    žádost o Hello směrován přes hello SSH tunel toohello Solr webového uživatelského rozhraní v clusteru. Podobně jako toohello následující bitové kopie se zobrazí stránka Hello:

    ![Obrázek Solr řídicí panel](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. Použijte v levém podokně hello hello **základní selektor** rozevíracího seznamu tooselect **collection1**. Několik položek by je se níže zobrazí **collection1**.

4. Z níže uvedené položky hello **collection1**, vyberte **dotazu**. Použijte následující hodnoty toopopulate hello vyhledávání stránku hello:

   * V hello **q** textové pole, zadejte  **\*:**\*. Tento dotaz vrací všechny hello dokumenty, které jsou uloženy v Solr. Pokud chcete toosearch konkrétní řetězec v hello dokumenty, můžete zadat sem tento řetězec.
   * V hello **wt** textového pole, vyberte hello výstupní formát. Výchozí hodnota je **json**.

     Nakonec vyberte hello **spuštění dotazu** tlačítko dole hello pate vyhledávání hello.

     ![Pomocí akce skriptu toocustomize clusteru](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     výstup Hello vrátí hello dva dokumenty, zda jste přidali toohello indexu dříve. Hello výstup je podobné toohello následující dokumentu JSON:

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, hello Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication tooother Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over hello e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }

### <a name="starting-and-stopping-solr"></a>Spuštění a zastavení Solr

Použijte následující příkazy toomanually zastavit a spustit Solr hello:

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a>Zálohování indexovaná data

Použijte následující postup tooback Solr data toohello výchozí úložiště pro váš cluster hello:

1. Připojte toohello clusteru pomocí protokolu SSH, potom použijte následující příkaz tooget hello název hostitele hlavního uzlu hello hello:

    ```bash
    hostname -f
    ```

2. Použijte následující příkaz toocreate snímek dat hello indexované hello. Nahraďte **HOSTNAME** s názvem hello vrácená z předchozí příkaz hello:

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    odpověď Hello je podobné toohello následující XML:

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. Změňte adresáře příliš`/usr/hdp/current/solr/example/solr`. Není podadresáři sem pro každou kolekci. Každý adresář kolekce obsahuje `data` adresář, který obsahuje snímek hello hello kolekce.

4. toocreate komprimovaný archiv hello snímku složky, hello použijte následující příkaz:

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    Nahraďte hello `snapshot.20150806185338855` hodnoty s názvem hello hello snímku pro vaše kolekce.

    Tento příkaz vytvoří archiv s názvem **snapshot.20150806185338855.tgz**, který obsahuje obsah hello hello **snapshot.20150806185338855** adresáře.

5. Pak můžete uložit hello archivu toohello clusteru primárního úložiště pomocí hello následující příkaz:

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

Další informace o práci s Solr zálohování a obnovení najdete v tématu [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).

## <a name="next-steps"></a>Další kroky

* [Nainstalujte Giraph clustery HDInsight](hdinsight-hadoop-giraph-install-linux.md). Použijte vlastní nastavení tooinstall clusteru, které Giraph na HDInsight Hadoop clusterů. Giraph můžete graf tooperform zpracování pomocí Hadoop a lze použít s Azure HDInsight.

* [Instalace aplikace Hue v clusterech HDInsight](hdinsight-hadoop-hue-linux.md). Použití clusteru přizpůsobení tooinstall Hue v clusterech HDInsight Hadoop. HUE je že sada webových aplikací používá toointeract s clusterem Hadoop.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
