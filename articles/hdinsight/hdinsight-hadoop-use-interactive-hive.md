---
title: "aaaUse interaktivní Hive v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse interaktivní (Hive na LLAP) Hive v HDInsight."
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
ms.openlocfilehash: 9e751a08091d18bc1b3d070468feef87f6828c61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-interactive-hive-in-hdinsight-preview"></a>Použít interaktivní Hive v HDInsight (Preview)
Interaktivní (také známa jako Hive [Live dlouhé a proces](https://cwiki.apache.org/confluence/display/Hive/LLAP)) je nový HDInsight [clusteru typu](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Umožňuje interaktivní Hive v ukládání do mezipaměti, který usnadňuje dotazů Hive mnohem víc interaktivní a rychlejší. Tato nová funkce umožňuje HDInsight jeden hello světě většina původce, flexibilní a otevřete řešení pro velká Data v cloudu hello s mezipaměti v paměti (pomocí Hive a Spark) a pokročilou analýzu prostřednictvím těsná integrace s R služby. 

Hello interaktivní Hive clusteru se liší od clusteru Hadoop hello. Obsahuje jenom hello podregistru služby. 

> [!NOTE]
> MapReduce, Pig, Sqoop, Oozie a dalším službám se odeberou z tohoto typu clusteru brzy k dispozici.
> Hello služby Hive v clusteru interaktivní Hive hello přístupný jenom prostřednictvím hello zobrazení Ambari Hive, Beeline a Hive ODBC. Není přístupná prostřednictvím konzoly Hive, Templeton, rozhraní příkazového řádku Azure a prostředí Azure PowerShell. 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a>Vytvoření clusteru interaktivní Hive
Interaktivní Hive clusteru je podporována pouze na clusterech se systémem Linux. Informace o vytváření clusterů HDInsight naleznete v tématu [vytvořit systémem Linux Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="execute-hive-from-interactive-hive"></a>Spuštění Hive z interaktivní Hive
Existují různé možnosti, jak můžete provádět dotazy Hive:

* Spuštění Hive pomocí hello zobrazení Ambari Hive
  
    Hello informace o používání hello zobrazení Hive naleznete v tématu [použití hello zobrazení Hive se systémem Hadoop v HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).
* Spuštění Hive pomocí Beeline
  
    Hello informace o používání Beeline v HDInsight naleznete v tématu [používání Hive s Hadoop v HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md).
  
    Používáte Beeline z hello headnode nebo prázdný hraniční uzel.  Doporučuje se použít Beeline z prázdné hraniční uzel.  Informace o vytváření clusteru HDInsight se prázdný edgenode najdete v tématu [použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md).
* Spuštění Hive pomocí ovladače ODBC Hive
  
    Hello informace o používání Hive ODBC naleznete v tématu [tooHadoop připojení aplikace Excel pomocí ovladače Microsoft Hive ODBC hello](hdinsight-connect-excel-hive-odbc-driver.md).

**toofind hello JDBC připojovací řetězec:**

1. Přihlaste se pomocí hello následující adresu URL tooAmbari: https://<ClusterName>. AzureHDInsight.net.
2. Klikněte na tlačítko **Hive** hello levé nabídce.
3. Klikněte na tlačítko hello zvýrazněná ikonu toocopy hello URL:
   
   ![HDInsight Hadoop Hive interaktivní LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Viz také
* [Vytvořit clustery se systémem Linux Hadoop v HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Zjistěte, jak clusterů toocreate interaktivní Hive v HDInsight.
* [Použijte Hive s Hadoop v HDInsight s Beeline](hdinsight-hadoop-use-hive-beeline.md): Zjistěte, jak dotazů Hive toosubmit Beeline toouse.
* [Připojení aplikace Excel tooHadoop pomocí ovladače Microsoft Hive ODBC hello](hdinsight-connect-excel-hive-odbc-driver.md): Zjistěte, jak tooHive tooconnect aplikace Excel.

