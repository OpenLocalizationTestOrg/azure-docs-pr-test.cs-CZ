---
title: aaaUse akce skriptu tooinstall Solr na clusteru Hadoop - Azure | Microsoft Docs
description: "Zjistěte, jak toocustomize HDInsight, cluster s Solr pomocí akce skriptu."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b1e6f338-8ac1-4b38-bbb5-2f7388b9de3b
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 022ba56b7499390a91bfe869e5069893e56b6503
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-windows-based-hdinsight-clusters"></a>Nainstalovat a používat Solr v clusterech HDInsight se systémem Windows

Zjistěte, jak toocustomize HDInsight se systémem Windows cluster s Solr pomocí akce skriptu a jak toouse Solr toosearch data.

> [!IMPORTANT]
> Hello kroky v této dokumentu funguje jenom s clustery HDInsight se systémem Windows. HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Informace o používání Solr s clusteru se systémem Linux najdete v tématu [instalací a použitím Solr na clusterů systému HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md).


Solr můžete nainstalovat na libovolný typ clusteru (Hadoop, Spark, HBase, Storm) v Azure HDInsight pomocí *akce skriptu*. Tooinstall skriptu ukázka Solr v clusteru HDInsight je k dispozici z objektu blob úložiště Azure jen pro čtení v [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

Ukázkový skript Hello pracuje pouze s verze clusteru HDInsight verze 3.1. Další informace o verzích clusterů HDInsight, naleznete v části [verze clusteru HDInsight](hdinsight-component-versioning.md).

Ukázkový skript Hello použitým v tomto tématu vytvoří cluster se systémem Windows Solr s konkrétní konfigurací. Pokud chcete tooconfigure hello Solr clusteru pomocí různých kolekcí, horizontálních oddílů, schémata, repliky, atd., musíte upravit hello skript a binární soubory Solr odpovídajícím způsobem.

**Související články**

* [Na nainstalovat a používat Solr clusterů systému HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md)
* [Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.
* [Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu.
* [Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-solr"></a>Co je Solr?
<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> je platforma enterprise vyhledávání, která umožňuje efektivní fulltextové vyhledávání na data. Při Hadoop umožňuje ukládání a správě obrovské množství dat, Apache Solr poskytuje možnosti vyhledávání hello tooquickly načtení hello data.

## <a name="install-solr-using-portal"></a>Nainstalujte Solr pomocí portálu
1. Zahájení vytváření clusteru pomocí hello **vytvořit vlastní** možnost, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-provision-clusters.md).
2. Na hello **akcí skriptů** stránku hello průvodce, klikněte na tlačítko **přidat akce skriptu** tooprovide podrobností o hello akce skriptu, jak je uvedeno níže:

    ![Pomocí akce skriptu toocustomize cluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "použití akce skriptu toocustomize clusteru")

    <table border='1'>
        <tr><th>Vlastnost</th><th>Hodnota</th></tr>
        <tr><td>Name (Název)</td>
            <td>Zadejte název akce skriptu hello. Například <b>nainstalovat Solr</b>.</td></tr>
        <tr><td>Identifikátor URI skriptu</td>
            <td>Zadejte hello identifikátor URI (Uniform Resource) toohello skript, který je vyvolaná toocustomize hello clusteru. Například <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Typ uzlu</td>
            <td>Zadejte hello uzly, na kterých běží hello přizpůsobení skriptu. Můžete zvolit <b>všechny uzly</b>, <b>hlavní pouze uzly</b>, nebo <b>pouze uzly pracovního procesu</b>.
        <tr><td>Parametry</td>
            <td>Zadejte parametry hello, pokud to vyžaduje hello skriptu. Hello skriptu tooinstall Solr nevyžaduje žádné parametry, takže to můžete nechat prázdné.</td></tr>
    </table>

    Více součástí, které můžete přidat více než jeden tooinstall akce skriptu v clusteru hello. Po přidání hello skripty, klikněte na tlačítko toostart zaškrtnutí hello vytváření clusteru hello.

## <a name="use-solr"></a>Použití Solr
Musí začínat indexování Solr s některých datových souborů. Potom můžete Solr toorun vyhledávací dotazy na data hello indexované. Proveďte následující kroky toouse Solr v clusteru služby HDInsight hello:

1. **Použití tooremote protokol RDP (Remote Desktop) do clusteru HDInsight hello s Solr nainstalován**. Z hello portálu Azure povolení vzdálené plochy pro hello clusteru, který jste vytvořili pomocí Solr hello nainstalovaná a pak vzdáleného do clusteru. Pokyny najdete v tématu [připojit pomocí protokolu RDP clustery tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **Index Solr tím, že nahrajete datové soubory**. Pokud jste indexu Solr, vložíte dokumenty v něm, které můžete potřebovat toosearch na. tooindex Solr, pomocí protokolu RDP tooremote do clusteru hello přejděte toohello plochy, otevřete hello Hadoop příkazového řádku a přejděte příliš**C:\apps\dist\solr-4.7.2\example\exampledocs**. Spusťte následující příkaz hello:

        java -jar post.jar solr.xml monitor.xml

    Zobrazí se následující výstup na konzolu hello hello:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Nástroj post.jar Hello indexuje Solr s dva ukázkové dokumenty **solr.xml** a **monitor.xml**. Nástroj post.jar Hello a hello ukázkové dokumenty jsou dostupné s Solr instalace.
3. **Použití hello Solr řídicí panel toosearch v rámci hello indexované dokumenty**. V hello RDP relace toohello HDInsight clusteru, otevřete Internet Explorer a spusťte hello Solr řídicí panel v **http://headnodehost:8983/solr / #/**. Z levého podokna hello z hello **základní selektor** rozevíracího seznamu, vyberte **collection1**a v rámci, klikněte na tlačítko **dotazu**. Jako příklad, tooselect a vrátí všechny dokumenty hello v Solr, zadejte hello následující hodnoty:

   * V hello **q** textové pole, zadejte  **\*:**\*. Tato možnost vrátí všechny hello dokumenty, které jsou uloženy ve Solr. Pokud chcete toosearch konkrétní řetězec v hello dokumenty, můžete zadat sem tento řetězec.
   * V hello **wt** textového pole, vyberte hello výstupní formát. Výchozí hodnota je **json**. Klikněte na tlačítko **spuštění dotazu**.

     ![Pomocí akce skriptu toocustomize cluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "spuštění dotazu na řídicím panelu Solr")

     výstup Hello vrátí hello dva dokumenty, které jsme použili pro indexování Solr. výstup Hello se podobá následující hello:

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
4. **Doporučená: Zálohování hello indexované data z Solr tooAzure úložiště objektů Blob, které jsou spojené s clusterem HDInsight hello**. Jako osvědčených postupů měli zálohovat data hello indexované z uzlů clusteru Solr hello do úložiště objektů Blob v Azure. Proveďte následující kroky toodo tak hello:

   1. Z relace RDP hello otevřete Internet Explorer a bodu toohello následující adresu URL:

           http://localhost:8983/solr/replication?command=backup

       Zobrazí takováto odpověď:

           <?xml version="1.0" encoding="UTF-8"?>
           <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
             <str name="status">OK</str>
           </response>
   2. V relaci vzdálené hello přejděte příliš {SOLR_HOME}\{kolekce} \data. Pro cluster hello vytvořené prostřednictvím hello ukázkový skript, měl by být **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. V tomto umístění, měli byste vidět do složky snímku vytvořen s názvem podobné příliš**snímku.* časové razítko***.
   3. Složka snímku hello ZIP a nahrajte ho tooAzure úložiště objektů Blob. Z příkazového řádku hello Hadoop přejděte toohello umístění složky snímků hello pomocí hello následující příkaz:

             hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

       Tento příkaz kopie hello příliš/příklad nebo data snímku/v kontejneru hello v rámci hello výchozí úložiště účtu přidruženého k hello clusteru.

## <a name="install-solr-using-aure-powershell"></a>Nainstalujte Solr pomocí prostředí Azure PowerShell
V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Ukázka Hello ukazuje, jak tooinstall Spark pomocí Azure PowerShell. Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>Nainstalujte Solr pomocí sady .NET SDK
V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Ukázka Hello ukazuje, jak tooinstall Spark pomocí hello .NET SDK. Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="see-also"></a>Viz také
* [Na nainstalovat a používat Solr clusterů systému HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md)
* [Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.
* [Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu.
* [Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md).
* [Nainstalovat a používat Spark v clusterech HDInsight][hdinsight-install-spark]: akce skriptu vzorku o instalaci Spark.
* [Nainstalujte jazyk R v clusterech HDInsight][hdinsight-install-r]: akce skriptu vzorku o instalaci R.
* [Nainstalujte Giraph clustery HDInsight](hdinsight-hadoop-giraph-install.md): akce skriptu vzorku o instalaci Giraph.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
