---
title: "Příklady aaaStorm starter na Apache Storm v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toodo analýzy velkých objemů dat a zpracování dat v reálném čase pomocí Apache Storm a hello počáteční příklady storm v HDInsight."
keywords: "Storm Starter, příklad Apache Storm"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: d710dcac-35d1-4c27-a8d6-acaf8146b485
ms.service: hdinsight
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: bb6d6549e67ca5b557f0692f98c89692a87267b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#<a name="get-started-with-apache-storm-on-hdinsight-using-hello-storm-starter-examples"></a>Začínáme s Apache Storm v HDInsight pomocí hello počáteční příklady storm

Zjistěte, jak toouse Apache Storm v HDInsight pomocí hello počáteční příklady storm.

Apache Storm je škálovatelný výpočetní systém v reálném čase odolný proti chybám, distribuovaný určený pro zpracování datových proudů. Pomocí Storm v Azure HDInsight můžete vytvořit cloudový cluster Storm, který bude provádět analýzy velkých objemů dat v reálném čase.

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Znalost SSH a SCP**. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-a-storm-cluster"></a>Vytvoření clusteru Storm

Použijte následující postup toocreate Storm v clusteru HDInsight hello:

1. Z hello [portál Azure](https://portal.azure.com), vyberte **+ nový**, **Intelligence + analýzy**a potom vyberte **HDInsight**.

    ![Vytvoření clusteru HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. Z hello **Základy** okno, zadejte hello následující informace:

    * **Název clusteru**: název hello hello clusteru HDInsight.
    * **Předplatné**: Vyberte předplatné toouse hello.
    * **Uživatelské jméno přihlášení clusteru** a **clusteru přihlašovacího hesla**: hello přihlášení při přístupu k hello clusteru přes protokol HTTPS. Tyto přihlašovací údaje služby tooaccess například hello webové uživatelské rozhraní Ambari nebo REST API používáte.
    * **Secure Shell (SSH) username**: hello přihlášení, které se používá při získávání hello clusteru prostřednictvím SSH. Ve výchozím nastavení je hello hello heslo stejné jako heslo pro přihlášení hello clusteru.
    * **Skupina prostředků**: hello prostředků skupiny toocreate hello clusteru v.
    * **Umístění**: hello oblast Azure toocreate hello clusteru v.

    ![Výběr předplatného](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. Vyberte **clusteru typ**, a pak sadu hello následující hodnoty na hello **konfigurace clusteru** okno:

    * **Typ clusteru:** Storm

    * **Operační systém:** Linux

    * **Verze:** Storm 1.1.0 (HDI 3.6)

    * **Úroveň clusteru:** Standard

    Nakonec použijte hello **vyberte** tlačítko toosave nastavení.

    ![Výběr typu clusteru](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. Po výběru typu hello clusteru, použijte hello __vyberte__ tooset hello clusteru typ tlačítka. Pak pomocí hello __Další__ tlačítko toofinish základní konfigurace.

5. Z hello **úložiště** okně vyberte nebo vytvořte účet úložiště. Hello kroky v tomto dokumentu ponechte hello další pole v tomto okně na hello výchozí hodnoty. Použití hello __Další__ tlačítko toosave úložiště konfigurace.

    ![Nastavit hello nastavení účtu úložiště pro HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. Z hello **Souhrn** okno, zkontrolujte konfiguraci hello pro hello cluster. Použití hello __upravit__ odkazy toochange všechna nastavení, která jsou nesprávné. Nakonec použijte the__Create__ tlačítko toocreate hello clusteru.

    ![Souhrn konfigurace clusteru](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)

    > [!NOTE]
    > To může trvat až too20 minut toocreate hello clusteru.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>Spuštění ukázky topologie Storm Starter v HDInsight

1. Připojte toohello clusteru HDInsight pomocí protokolu SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Pokud jste použili toosecure hesla účtu uživatele SSH, jste výzvami tooenter ho. Pokud jste použili veřejný klíč, můžete potřebovat použít hello `-i` parametr toospecify hello odpovídající soukromý klíč. Například, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Použijte následující příkaz toostart ukázkové topologie hello:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount

    > [!NOTE]
    > V dřívějších verzích systému HDInsight, název třídy hello hello topologie je `storm.starter.WordCountTopology` místo `org.apache.storm.starter.WordCountTopology`.

    Tento příkaz spustí hello ukázkovou topologii WordCount v clusteru hello s popisným názvem "WordCount". Generuje náhodně věty a počet hello výskytem jednotlivých slov v hello věty.

    > [!NOTE]
    > Při odesílání vlastní topologie toohello cluster, je nutné nejprve zkopírovat soubor jar hello obsahující hello cluster před použitím hello `storm` příkaz. Použití hello `scp` příkaz toocopy hello souboru. Například `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`.
    >
    > Příklad WordCount Hello a další příklady storm starter jsou již zahrnuty v clusteru na `/usr/hdp/current/storm-client/contrib/storm-starter/`.

Pokud vás zajímá v zobrazení zdroje hello hello příklady storm-starter, můžete najít hello kód na [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter). Tento odkaz je pro Storm 1.1.x, který je součástí služby HDInsight 3.6. U ostatních verzí Storm použít hello __větve__ tlačítko hello horní části stránky tooselect hello jinou verzi Storm.

## <a name="monitor-hello-topology"></a>Monitorování hello topologie

Hello uživatelské rozhraní Storm poskytuje webové rozhraní pro práci se spuštěnými topologiemi a je součástí clusteru HDInsight.

Použijte následující postup toomonitor hello topologie pomocí hello uživatelské rozhraní Storm hello:

1. hello toodisplay uživatelské rozhraní Storm, otevřete toohttps://CLUSTERNAME.azurehdinsight.net/stormui webového prohlížeče. Nahraďte **CLUSTERNAME** s hello názvem vašeho clusteru.

    > [!NOTE]
    > Pokud se zobrazí dotaz tooprovide uživatelské jméno a heslo, zadejte Správce clusteru hello (správce) a heslo použité při vytváření clusteru hello.

2. V části **souhrn topologie**, vyberte hello **wordcount** položku v hello **název** sloupce. Zobrazí se informace o topologii hello.

    ![Řídicí panel Storm s informacemi o topologii Storm Starter WordCount.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    Tato stránka obsahuje hello následující informace:

    * **Statistiky topologie** – základní informace o hello výkonu topologie uspořádané do časových oken.

        > [!NOTE]
        > Výběr určité časové okno změny hello časové okno informací zobrazených v dalších částech stránky hello.

    * **Spouts** – základní informace o funkcích spouts, včetně hello poslední chyby vrácené každou funkcí spout.

    * **Funkce Bolts** – základní informace o funkcích bolts.

    * **Topologie konfigurace** -podrobné informace o konfiguraci topologie hello.

    Tato stránka také obsahuje akce, které můžete provést na topologii hello:

    * **Aktivovat** – obnoví zpracování deaktivované topologie.

    * **Deaktivovat** – pozastaví spuštěné topologie.

    * **Znovu vyvážit** – upraví paralelismus hello hello topologie. Po změně hello počet uzlů v clusteru hello, znovu vyvážit spuštěné topologie. Vyrovnává upraví paralelismus toocompensate pro hello zvýšení nebo snížení počtu uzlů v clusteru hello. Další informace najdete v tématu [pochopení paralelismu topologie Storm hello](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

    * **Příkaz kill** – ukončí topologii Storm po hello zadaný časový limit.

3. Z této stránky, vyberte položku z hello **Spouts** nebo **Bolts** části. Zobrazí se informace o vybrané součásti hello.

    ![Řídicí panel Storm s informacemi o vybraných součástech.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)

    Tato stránka zobrazuje hello následující informace:

    * **Statistiky funkcí spout/Bolt** – základní informace o hello výkonu komponenty uspořádané do časových oken.

        > [!NOTE]
        > Výběr určité časové okno změny hello časové okno informací zobrazených v dalších částech stránky hello.

    * **Statistiky vstupu** (pouze funkce bolt) – informace o komponentech, které vytváří dat využívaná funkcí hello bolt.

    * **Statististiky výstupu** – informace o datech vysílaných touto funkcí bolt.

    * **Prováděcí moduly** – informace o instancích této komponenty.

    * **Chyby** – chyby vytvořené touto komponentou.

4. Při zobrazení podrobností hello spout nebo bolt, vyberte položku z hello **Port** sloupec v hello **vykonavatelů** části tooview podrobnosti pro konkrétní instanci komponenty hello.

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    V tomto příkladu hello word **sedm** došlo 1 493 957krát. Je to počet kolikrát hello word došlo od spuštění této topologii.

## <a name="stop-hello-topology"></a>Zastavení topologie hello

Vrátí toohello **souhrn topologie** pro hello topologii počtu slov a pak vyberte hello **Kill** tlačítko z hello **topologie akce** části. Po zobrazení výzvy zadejte 10 sekund toowait hello před zastavením topologie hello. Po hello časového limitu hello topologie již nadále nebude zobrazovat při návštěvě hello **uživatelské rozhraní Storm** část řídicího panelu hello.

## <a name="delete-hello-cluster"></a>Odstranění clusteru hello

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Pokud narazíte na problém s vytvářením clusteru HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a id="next"></a>Další kroky

V tomto kurzu Apache Storm jste se dozvěděli hello základy práce se Storm v HDInsight. Dále se naučíte, jak příliš[založené na jazyce Java vyvíjet topologie pomocí nástroje Maven](hdinsight-storm-develop-java-topology.md).

Pokud jste již obeznámeni s vývojem topologií založených na jazyce Java a chcete toodeploy existující tooHDInsight topologii, přečtěte si téma [nasazení a správa topologií Apache Storm v HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).

Pokud jste vývojář .NET, můžete s použitím sady Visual Studio vytvořit topologie C# nebo hybridní topologie C#/Java. Další informace najdete v tématu [Vývoj topologií C# pro Apache Storm ve službě HDInsight pomocí nástrojů Hadoop pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Topologie, které lze použít s Storm v HDInsight, například zobrazit hello následující příklady:

* [Příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[preview-portal]: https://portal.azure.com/
