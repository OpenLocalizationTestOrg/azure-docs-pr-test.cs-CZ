---
title: aaaUse R v clusterech HDInsight toocustomize - Azure | Microsoft Docs
description: "Zjistěte, jak pomocí tooinstall R skript akce a použití R na clustery HDInsight."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: be851270-afa5-4af0-a69e-2d343a4deeb7
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: bf5adf2e18dc43a743b29fd1567fad731b9c3ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Instalace a použití R na clusterech HDInsight Hadoop

Zjistěte, jak toocustomize Windows na základě clusteru HDInsight pomocí pomocí akce skriptu R a jak toouse R v HDInsight clustery. Hello [HDInsight nabídky](https://azure.microsoft.com/pricing/details/hdinsight/) zahrnuje R Server v rámci clusteru HDInsight. To umožňuje skripty R výpočtů toorun distribuované toouse MapReduce a Spark. Další informace naleznete v tématu [Začínáme používat R Server v HDInsight](hdinsight-hadoop-r-server-get-started.md). Informace o používání R s clusteru se systémem Linux najdete v tématu [instalací a použitím R na clusterů systému HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md).

R můžete nainstalovat na libovolný typ clusteru (Hadoop, Spark, HBase, Storm) v Azure HDInsight pomocí *akce skriptu*. Tooinstall skriptu ukázka R na clusteru služby HDInsight je k dispozici z objektu blob úložiště Azure jen pro čtení v [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

**Související články**

* [Na nainstalovat a používat R clusterů systému HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md)
* [Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Obecné informace o vytváření clusterů HDInsight
* [Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu
* [Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Co je R?
Hello <a href="http://www.r-project.org/" target="_blank">R projekt pro statistické výpočty</a> je open source jazyk a prostředí pro statistické výpočty. R poskytuje stovky sestavení v statistických funkcí a vlastní programovací jazyk, který kombinuje aspektů funkčnosti a objektově orientované programování. Také poskytuje rozsáhlé možnosti grafického rozhraní. R je hello upřednostňované programovací prostředí pro nejvíce professional statistikami a vědců v celé řadě polí.

R je kompatibilní s Azure Blob Storage (WASB) tak, aby data, která je uložená existuje lze zpracovat využití R v HDInsight.  

## <a name="install-r"></a>Nainstalujte jazyk R
A [ukázkový skript](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) tooinstall R na clusteru HDInsight má k dispozici jen pro čtení objektů blob v Azure Storage. Tato část obsahuje pokyny, jak toouse hello ukázkový skript při vytváření clusteru hello pomocí hello portálu Azure.

> [!NOTE]
> Ukázkový skript Hello byla zavedena v systému verze clusteru HDInsight verze 3.1. Další informace o verzích clusterů HDInsight, naleznete v části [verze clusteru HDInsight](hdinsight-component-versioning.md).
>
>

1. Při vytváření clusteru služby HDInsight z hello portálu, klikněte na tlačítko **volitelné konfiguraci**a potom klikněte na **akcí skriptů**.
2. Na hello **akcí skriptů** zadejte hello následující hodnoty:

    ![Pomocí akce skriptu toocustomize cluster](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "použití akce skriptu toocustomize clusteru")

    <table border='1'>
        <tr><th>Vlastnost</th><th>Hodnota</th></tr>
        <tr><td>Name (Název)</td>
            <td>Zadejte název akce skriptu hello, například <b>nainstalovat R</b>.</td></tr>
        <tr><td>Identifikátor URI skriptu</td>
            <td>Zadejte hello URI toohello skript, který je vyvolaná toocustomize hello cluster, například <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>Typ uzlu</td>
            <td>Zadejte hello uzly, na kterých běží hello přizpůsobení skriptu. Můžete zvolit <b>všechny uzly</b>, <b>hlavní pouze uzly</b>, nebo <b>uzlů pracovního procesu</b> pouze.
        <tr><td>Parametry</td>
            <td>Zadejte parametry hello, pokud to vyžaduje hello skriptu. Hello tooinstall skriptu R však nevyžaduje žádné parametry, takže to můžete nechat prázdné.</td></tr>
    </table>

    Více součástí, které můžete přidat více než jeden tooinstall akce skriptu v clusteru hello. Po přidání hello skripty, klikněte na tlačítko toostart zaškrtnutí hello crating hello clusteru.

Můžete taky tooinstall hello skriptu R v HDInsight pomocí prostředí Azure PowerShell nebo hello HDInsight .NET SDK. Pokyny pro tyto postupy jsou uvedeny dále v tomto článku.

## <a name="run-r-scripts"></a>Spusťte skripty R
Tato část popisuje, jak clusteru toorun R skript na hello Hadoop v prostředí HDInsight.

1. **Vytvoření clusteru toohello připojení vzdálené plochy**: Z hello portál, povolení vzdálené plochy pro hello clusteru, které jste vytvořili pomocí R nainstalované a potom se připojte toohello clusteru. Pokyny najdete v tématu [připojit pomocí protokolu RDP clustery tooHDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **Otevřete hello R konzoly**: instalace hello R vloží konzoly toohello R odkaz na ploše hello hello hlavního uzlu. Klikněte na jeho tooopen hello R konzoly.
3. **Spusťte skript hello R**: hello R skript můžete spustit přímo z konzoly hello R vložením, ho vyberete a stisknutím klávesy ENTER. Zde je jednoduchý příklad skript, který generuje hello čísla 1 too100 a potom je vynásobí 2.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

Hello první dva řádky volání hello RHadoop knihovny, které jsou nainstalované s R. hello poslední řádek výtisků hello výsledky toohello konzoly. výstup Hello by měl vypadat takto:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>Nainstalujte jazyk R pomocí prostředí Azure PowerShell
V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Ukázka Hello ukazuje, jak tooinstall Spark pomocí Azure PowerShell. Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

## <a name="install-r-using-net-sdk"></a>Nainstalujte jazyk R pomocí sady .NET SDK
V tématu [HDInsight přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Ukázka Hello ukazuje, jak tooinstall Spark pomocí hello .NET SDK. Je třeba toocustomize hello skriptu toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).

## <a name="see-also"></a>Viz také
* [Na nainstalovat a používat R clusterů systému HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md)
* [Vytvoření clusterů systému Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Obecné informace o vytváření clusterů HDInsight
* [Přizpůsobení clusteru HDInsight pomocí akce skriptu][hdinsight-cluster-customize]: Obecné informace o přizpůsobení clusterů HDInsight pomocí akce skriptu
* [Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions.md)
* [Nainstalovat a používat Spark v clusterech HDInsight][hdinsight-install-spark]: akce skriptu vzorku o instalaci Spark
* [Nainstalujte Giraph clustery HDInsight](hdinsight-hadoop-giraph-install.md): akce skriptu vzorku o instalaci Giraph
* [Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install-linux.md): akce skriptu vzorku o instalaci Solr.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
