---
title: "aaaUse zobrazení Ambari Tez s HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Ambari Tez zobrazit úlohy Tez toodebug v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 9c39ea56-670b-4699-aba0-0f64c261e411
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 5d61bd0403c98284c86982073af91468ae62ac60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-ambari-views-toodebug-tez-jobs-on-hdinsight"></a>Pomocí zobrazení Ambari toodebug Tez úloh v HDInsight

Hello webové uživatelské rozhraní Ambari pro HDInsight obsahuje Tez zobrazení, které lze použít toounderstand a ladění úlohy, které používají Tez. zobrazení Tez Hello umožňuje toovisualize hello úlohy graf připojených položek, přejdete na každou položku a načíst informace o protokolování a statistiky.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Požadavky

* Cluster HDInsight se systémem Linux. Pokyny týkající se vytvoření clusteru, najdete v části [začněte používat HDInsight se systémem Linux](hdinsight-hadoop-linux-tutorial-get-started.md).
* Moderní webový prohlížeč, který podporuje HTML5.

## <a name="understanding-tez"></a>Principy Tez

Tez je rozšiřitelná architektura pro zpracování dat v Hadoop, která poskytuje vyšší rychlosti než tradiční zpracování prostředí MapReduce. Clustery HDInsight se systémem Linux je hello výchozí modul pro Hive.

Tez vytvoří směrované Acyklické grafu (DAG) popisující hello pořadí akce požadované úlohami. Jednotlivé akce se nazývají vrcholy a spouštět úsek hello celkové úlohy. skutečné provádění Hello popsaného Vrchol pracovní hello je volána úloha a mohou být distribuovány mezi několika uzly v clusteru hello.

### <a name="understanding-hello-tez-view"></a>Principy hello Tez zobrazení

Hello Tez zobrazení poskytuje historické informace a informace na procesy, které jsou spuštěné. Tyto informace ukazuje, jak je úlohu rozdělené mezi clustery. Také zobrazuje čítače používané úlohy a vrcholy a informace o chybě související s toohello úlohy. Vám může nabídnout užitečné informace v hello následující scénáře:

* Monitorování dlouho běžící procesy, zobrazení hello průběh mapy a snížit úlohy.
* Analýza historické údaje o úspěšném nebo neúspěšném procesy toolearn, jak lze zlepšit zpracování nebo proč se nezdařilo.

## <a name="generate-a-dag"></a>Generovat DAG

Hello Tez zobrazení obsahuje data pouze, pokud úlohu, která používá hello modulu Tez je aktuálně spuštěna, nebo má již dříve spustil. Jednoduché dotazů Hive, bez použití Tez lze vyřešit. Složitější dotazy tento filtrování proveďte seskupení, řazení, spojení, atd. pomocí modulu Tez hello.

Použijte následující postup toorun dotaz Hive, který používá Tez hello:

1. Ve webovém prohlížeči přejděte toohttps://CLUSTERNAME.azurehdinsight.net, kde **CLUSTERNAME** je hello název clusteru HDInsight.

2. Z nabídky hello hello horní části stránky hello vyberte hello **zobrazení** ikonu. Tato ikona vypadá jako řadu kvadratických hodnot. V rozevírací nabídce hello, který se zobrazí, vyberte **Hive zobrazení**.

    ![Výběr zobrazení Hive](./media/hdinsight-debug-ambari-tez-view/selecthive.png)

3. Když hello zobrazení Hive načte, vložte následující hello dotaz do hello Editor dotazů a pak klikněte na **provést**.

        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;

    Po dokončení úlohy hello byste měli vidět výstup hello zobrazí v hello **výsledky zpracování dotazu** části. výsledky Hello by měl vypadat podobně jako toohello následující text:

        market  state       country
        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica

4. Vyberte hello **protokolu** kartě. Zobrazí informace o podobné toohello následující text:

        INFO : Session is already open
        INFO :

        INFO : Status: Running (Executing on YARN cluster with App id application_1454546500517_0063)

    Uložit hello **id aplikace** hodnota, protože tato hodnota se používá v další části hello.

## <a name="use-hello-tez-view"></a>Použití hello Tez zobrazení

1. Z nabídky hello hello horní části stránky hello vyberte hello **zobrazení** ikonu. V rozevírací nabídce hello, který se zobrazí, vyberte **Tez zobrazení**.

    ![Výběr zobrazení Tez](./media/hdinsight-debug-ambari-tez-view/selecttez.png)

2. Při načtení hello Tez zobrazení uvidíte, že seznam dotazů hive, které jsou aktuálně spuštěné, nebo byla spuštěna v clusteru hello.

    ![Všechny DAG](./media/hdinsight-debug-ambari-tez-view/tez-view-home.png)

3. Pokud máte pouze jednu položku, je pro hello dotaz, který jste spustili v předchozí části hello. Pokud máte více položek, může hledat pomocí hello polí v hello horní části stránky hello.

4. Vyberte hello **ID dotazu** pro dotaz Hive. Zobrazí se informace o dotazu hello.

    ![Podrobnosti o DAG](./media/hdinsight-debug-ambari-tez-view/query-details.png)

5. Hello karty na této stránce povolit tooview hello následující informace:

    * **Dotaz na podrobnosti**: Podrobnosti o dotaz Hive hello.
    * **Časová osa**: informace o tom, jak dlouho trvalo každé fáze zpracování.
    * **Konfigurace**: Konfigurace hello použitá pro tento dotaz.

    Z __dotazu podrobnosti__ můžete hello odkazy toofind informací o hello __aplikace__ nebo hello __DAG__ pro tento dotaz.
    
    * Hello __aplikace__ odkaz zobrazí informace o hello YARN aplikace pro tento dotaz. Odsud můžete přistupovat hello YARN aplikační protokoly.
    * Hello __DAG__ odkaz zobrazí informace o hello necyklicky pro tento dotaz. Odtud můžete zobrazit grafické reprezentace hello DAG. Můžete také získat informace o hello vrcholy v rámci hello DAG.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak zobrazit toouse hello Tez, další informace o [pomocí Hive v HDInsight](hdinsight-use-hive.md).

Další podrobné technické informace o Tez naleznete v tématu hello [Tez stránku v Hortonworks](http://hortonworks.com/hadoop/tez/).

Další informace o používání Ambari s HDInsight najdete v tématu [clusterů HDInsight spravovat pomocí hello webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md)
