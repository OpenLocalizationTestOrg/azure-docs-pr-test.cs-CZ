---
title: "aaaInstall a použití Giraph na Hadoop clusterů v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toocustomize HDInsight, cluster s Giraph a toouse Giraph."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 77a1d0e0-55de-4e61-98a0-060914fb7ca0
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: bd473faca9d3c87c29d7566a18fc94211c50f059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a>Nainstalovat a používat Giraph v clusterech HDInsight se systémem Windows

Zjistěte, jak toocustomize Windows na základě clusteru HDInsight s Giraph pomocí akce skriptu a jak toouse Giraph tooprocess rozsáhlé grafy. Informace o používání Giraph s clusteru se systémem Linux najdete v tématu [Giraph nainstalovat na clusterů systému HDInsight Hadoop (Linux)](hdinsight-hadoop-giraph-install-linux.md).

> [!IMPORTANT]
> Hello kroky v této dokumentu funguje jenom s clustery HDInsight se systémem Windows. HDInsight je k dispozici pouze v systému Windows verze nižší než HDInsight 3.4. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Informace o tom najdete v části tooinstall Giraph na cluster HDInsight se systémem Linux [Giraph nainstalovat na clusterů systému HDInsight Hadoop (Linux)](hdinsight-hadoop-giraph-install-linux.md).


Giraph můžete nainstalovat na libovolný typ clusteru (Hadoop, Spark, HBase, Storm) v Azure HDInsight pomocí *akce skriptu*. Tooinstall skriptu ukázka Giraph v clusteru HDInsight je k dispozici z objektu blob úložiště Azure jen pro čtení v [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1). Ukázkový skript Hello pracuje pouze s verze clusteru HDInsight verze 3.1. Další informace o verzích clusterů HDInsight, naleznete v části [verze clusteru HDInsight](hdinsight-component-versioning.md).

**Související články**

* [Nainstalujte Giraph clusterů systému HDInsight Hadoop (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.
* [Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu.
* [Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-giraph"></a>Co je Giraph?
<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> vám umožní tooperform grafu zpracování pomocí Hadoop a lze použít s Azure HDInsight. Grafy modelování vztahů mezi objekty, například hello připojení mezi směrovači v velkým síťovým jako hello Internetu, nebo vztahy mezi uživatelé v sociálních sítích (někdy označují tooas sociálních grafu). Graf zpracování vám umožní tooreason o hello vztahy mezi objekty v grafu, jako například:

* Identifikace potenciální přátel na základě vaší aktuální relací.
* Identifikace hello nejkratší trasy mezi dvěma počítači v síti.
* Výpočet pořadí stránku hello webové stránky.

## <a name="install-giraph-using-portal"></a>Nainstalujte Giraph pomocí portálu
1. Zahájení vytváření clusteru pomocí hello **vytvořit vlastní** možnost, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-provision-clusters.md).
2. Na hello **akcí skriptů** stránku hello průvodce, klikněte na tlačítko **přidat akce skriptu** tooprovide podrobností o hello akce skriptu, jak je uvedeno níže:

    ![Pomocí akce skriptu toocustomize cluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "použití akce skriptu toocustomize clusteru")

    <table border='1'>
        <tr><th>Vlastnost</th><th>Hodnota</th></tr>
        <tr><td>Name (Název)</td>
            <td>Zadejte název akce skriptu hello. Například <b>nainstalovat Giraph</b>.</td></tr>
        <tr><td>Identifikátor URI skriptu</td>
            <td>Zadejte hello identifikátor URI (Uniform Resource) toohello skript, který je vyvolaná toocustomize hello clusteru. Například <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>Typ uzlu</td>
            <td>Zadejte hello uzly, na kterých běží hello přizpůsobení skriptu. Můžete zvolit <b>všechny uzly</b>, <b>hlavní pouze uzly</b>, nebo <b>pouze uzly pracovního procesu</b>.
        <tr><td>Parametry</td>
            <td>Zadejte parametry hello, pokud to vyžaduje hello skriptu. Hello skriptu tooinstall Giraph nevyžaduje žádné parametry, takže to můžete nechat prázdné.</td></tr>
    </table>

    Více součástí, které můžete přidat více než jeden tooinstall akce skriptu v clusteru hello. Po přidání hello skripty, klikněte na tlačítko toostart zaškrtnutí hello vytváření clusteru hello.

## <a name="use-giraph"></a>Použití Giraph
Používáme hello SimpleShortestPathsComputation příklad toodemonstrate hello basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementace pro hledání hello nejkratší cesta mezi objekty v grafu. Pomocí následujících kroků tooupload hello ukázkových dat a hello ukázka jar, spustit úlohu pomocí hello SimpleShortestPathsComputation příklad a zobrazení výsledků hello hello.

1. Nahrajte ukázková data souboru tooAzure úložiště objektů Blob. Na místní pracovní stanici, vytvořte nový soubor s názvem **tiny_graph.txt**. Měl by obsahovat hello následující řádky:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Nahrajte hello tiny_graph.txt souboru toohello primárního úložiště pro váš cluster HDInsight. Pokyny najdete v části tooupload data [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).

    Tato data popisuje vztah mezi objekty v řízené grafu, ve formátu hello [zdroj\_id, zdroj\_hodnotu [[cíle\_id], [edge\_hodnotu],...]]. Každý řádek představuje vztah mezi **zdroj\_id** objektů a jeden nebo více **cíle\_id** objekty. Hello **edge\_hodnotu** (nebo váhy) lze považovat za hello sílu nebo distance hello připojení mezi **source_id** a **cíle\_id**.

    Vykreslovat limitu, a pomocí hodnoty hello (nebo váhy) jako hello vzdálenost mezi objekty, hello výše dat může vypadat například takto:

    ![tiny_graph.txt vykreslovat jako kroužky řádků různých vzdálenost mezi](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. Spusťte příklad SimpleShortestPathsComputation hello. Použijte následující příklad hello toorun rutiny Azure PowerShell pomocí souboru tiny_graph.txt hello jako vstup hello.

    > [!IMPORTANT]
    > Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali. kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.
    >
    > Postupujte podle kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello nejnovější verzi prostředí Azure PowerShell. Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) Další informace.

    ```powershell
    $clusterName = "clustername"
    # Giraph examples jar
    $jarFile = "wasb:///example/jars/giraph-examples.jar"
    # Arguments for this job
    $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                    "-ca", "mapred.job.tracker=headnodehost:9010",
                    "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                    "-vip", "wasb:///example/data/tiny_graph.txt",
                    "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                    "-op",  "wasb:///example/output/shortestpaths",
                    "-w", "2"
    # Create hello definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
        -JarFile $jarFile
        -ClassName "org.apache.giraph.GiraphRunner"
        -Arguments $jobArguments

    # Run hello job, write output toohello Azure PowerShell window
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for hello job toocomplete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
    Write-Host "Display hello standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput
    ```

    V hello výše příklad, nahraďte **clustername** s názvem hello clusteru HDInsight s Giraph nainstalována.
3. Zobrazení výsledků hello. Po dokončení úlohy hello hello výsledky se budou ukládat do dvou výstupní soubory v hello **wasb: / / / Příklad nebo na více systémů nebo shotestpaths** složky. soubory Hello se nazývají **část m-00001** a **část m-00002**. Proveďte následující kroky toodownload a zobrazení hello výstup hello:

    ```powershell
    $subscriptionName = "<SubscriptionName>"       # Azure subscription name
    $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
    $containerName = "<ContainerName>"             # Blob storage container name

    # Select hello current subscription
    Select-AzureSubscription $subscriptionName

    # Create hello Storage account context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Download hello job output toohello workstation
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force
    ```

    Tím se vytvoří hello **příklad nebo výstupní/shortestpaths** struktury adresářů v aktuálním adresáři hello na pracovní stanici a stažení hello dva výstupní soubory toothat umístění.

    Použití hello **Cat** rutiny toodisplay hello obsah hello souborů:

        Cat example/output/shortestpaths/part*

    výstup Hello by měla vypadat podobně jako toohello následující:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    Příklad SimpleShortestPathComputation Hello je naprogramováno toostart s objektem ID 1 a najít hello nejkratší cesta tooother objekty. Proto byste si měli přečíst výstup hello jako `destination_id distance`, kde je vzdálenost hello hodnota (nebo váhy) hello okrajů kterou urazit mezi objektem ID 1 a ID hello cíl.

    Tato vizualizace, můžete ověřit výsledky hello pomocí přechodu cesty nejkratší hello mezi ID 1 a všechny ostatní objekty. Všimněte si, že hello nejkratší cesta ID 1 až ID 4 je 5. Toto je celkový počet vzdálenost hello mezi <span style="color:orange">ID 1 a 3</span>a potom <span style="color:red">ID 3 a 4</span>.

    ![Kreslení objektů jako kroužky s nejkratší cest mezi](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a>Nainstalujte Giraph pomocí prostředí Azure PowerShell
V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Ukázka Hello ukazuje, jak tooinstall Spark pomocí Azure PowerShell. Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="install-giraph-using-net-sdk"></a>Nainstalujte Giraph pomocí sady .NET SDK
V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Ukázka Hello ukazuje, jak tooinstall Spark pomocí hello .NET SDK. Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="see-also"></a>Viz také
* [Nainstalujte Giraph clusterů systému HDInsight Hadoop (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-provision-clusters.md): Obecné informace o vytváření clusterů HDInsight.
* [Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu.
* [Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md).
* [Nainstalovat a používat Spark v clusterech HDInsight][hdinsight-install-spark]: akce skriptu vzorku o instalaci Spark.
* [Nainstalujte jazyk R v clusterech HDInsight][hdinsight-install-r]: akce skriptu vzorku o instalaci R.
* [Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install.md): akce skriptu vzorku o instalaci Solr.

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
