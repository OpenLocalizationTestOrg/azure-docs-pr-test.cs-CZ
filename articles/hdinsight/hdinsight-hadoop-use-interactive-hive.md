---
title: "Použít interaktivní Hive v HDInsight - Azure | Microsoft Docs"
description: "Další informace o použití interaktivní (Hive na LLAP) Hive v HDInsight."
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: e7874b55fc72f14d8e2c801872359e823cb2ba34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-interactive-hive-in-hdinsight-preview"></a>Použít interaktivní Hive v HDInsight (Preview)
Interaktivní (také známa jako Hive [Live dlouhé a proces](https://cwiki.apache.org/confluence/display/Hive/LLAP)) je nový HDInsight [clusteru typu](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Umožňuje interaktivní Hive v ukládání do mezipaměti, který usnadňuje dotazů Hive mnohem víc interaktivní a rychlejší. Tato nová funkce umožňuje HDInsight jeden na světě většina původce, flexibilní a otevřete řešení pro velká Data do cloudu s mezipaměti v paměti (pomocí Hive a Spark) a pokročilou analýzu prostřednictvím těsná integrace s R služby. 

Interaktivní Hive clusteru se liší od clusteru Hadoop. Obsahuje jenom službu Hive. 

> [!NOTE]
> MapReduce, Pig, Sqoop, Oozie a dalším službám se odeberou z tohoto typu clusteru brzy k dispozici.
> Službu Hive v clusteru interaktivní Hive přístupný jenom prostřednictvím zobrazení Ambari Hive, Beeline a Hive ODBC. Není přístupná prostřednictvím konzoly Hive, Templeton, rozhraní příkazového řádku Azure a prostředí Azure PowerShell. 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a>Vytvoření clusteru interaktivní Hive
Interaktivní Hive clusteru je podporována pouze na clusterech se systémem Linux. Informace o vytváření clusterů HDInsight naleznete v tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="execute-hive-from-interactive-hive"></a>Spuštění Hive z interaktivní Hive
Existují různé možnosti, jak můžete provádět dotazy Hive:

* Spuštění Hive pomocí zobrazení Ambari Hive
  
    Informace o používání zobrazení Hive naleznete v tématu [použít zobrazení Hive se systémem Hadoop v HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).
* Spuštění Hive pomocí Beeline
  
    Informace o používání Beeline v HDInsight naleznete v tématu [používání Hive s Hadoop v HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md).
  
    Používáte Beeline z headnode nebo prázdný hraniční uzel.  Doporučuje se použít Beeline z prázdné hraniční uzel.  Informace o vytváření clusteru HDInsight se prázdný edgenode najdete v tématu [použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md).
* Spuštění Hive pomocí ovladače ODBC Hive
  
    Informace o používání Hive ODBC naleznete v tématu [připojit Excel k systému Hadoop pomocí ovladače Microsoft Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md).

**Vyhledání JDBC připojovací řetězec:**

1. Přihlaste se pomocí následující adresy URL Ambari: https://<ClusterName>. AzureHDInsight.net.
2. Klikněte na tlačítko **Hive** v levé nabídce.
3. Klikněte na ikonu zvýrazněná zkopírujte adresu URL:
   
   ![HDInsight Hadoop Hive interaktivní LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Viz také
* [Vytvořit clustery se systémem Linux Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Naučte se vytvářet clustery interaktivní Hive v HDInsight.
* [Použijte Hive s Hadoop v HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md): Naučte se používat Beeline k odesílání dotazů Hive.
* [Připojení aplikace Excel k systému Hadoop pomocí ovladače Microsoft Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md): Zjistěte, jak připojit Excel k Hive.

