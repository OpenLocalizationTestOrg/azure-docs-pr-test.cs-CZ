---
title: "aaaDeploy a správa topologií Apache Storm v HDInsight se systémem Linux | Microsoft Docs"
description: "Zjistěte, jak toodeploy, monitorování a správa topologií Apache Storm pomocí hello řídicí panel Storm v HDInsight se systémem Linux. Pomocí nástroje Hadoop pro sadu Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 35086e62-d6d8-4ccf-8cae-00073464a1e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 3a1edb773089cc596fea423710aa88cf83c7b841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a>Nasazení a správa topologií Apache Storm v HDInsight

V tomto dokumentu zjistěte hello základy správy a monitorování topologií Storm spuštěných na Storm v HDInsight clustery.

> [!IMPORTANT]
> Hello kroky v tomto článku vyžadují Storm se systémem Linux v clusteru HDInsight. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). 
>
> Informace o nasazení a monitorování topologií v HDInsight se systémem Windows, najdete v části [nasazení a správa topologií Apache Storm v HDInsight se systémem Windows](hdinsight-storm-deploy-monitor-topology.md)


## <a name="prerequisites"></a>Požadavky

* **Storm v clusteru HDInsight se systémem Linux**: najdete v části [začít pracovat s Apache Storm v HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) pokyny týkající se vytvoření clusteru

* (Volitelné) **Znalost SSH a SCP**: Další informace najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* (Volitelné) **Visual Studio**: Azure SDK 2.5.1 nebo novější a hello nástrojů Data Lake pro Visual Studio. Další informace najdete v tématu [Začínáme pomocí nástrojů Data Lake pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

    Jedna z následujících verzí sady Visual Studio hello:

  * Visual Studio 2012 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=39305)

  * Visual Studio 2013 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=44921) nebo [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)
  * [Visual Studio 2015](https://www.visualstudio.com/downloads/)

  * Visual Studio 2015 (všechny edice)

  * Visual Studio 2017 (všechny edice). Nástroje data Lake pro Visual Studio 2017 se instalují jako součást hello pracovního vytížení Azure.

## <a name="submit-a-topology-visual-studio"></a>Odeslání topologii: Visual Studio

Nástroje HDInsight Hello lze použít toosubmit jazyka C# nebo hybridní topologie tooyour cluster Storm. Hello následující kroky pomocí ukázkové aplikace. Informace o vytváření vlastního topologie pomocí nástrojů HDInsight hello najdete v tématu [vývoj topologií C# pomocí hello nástroje HDInsight pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

1. Pokud jste ještě nenainstalovali hello nejnovější verzi nástroje hello Data Lake pro Visual Studio, najdete v části [Začínáme pomocí nástrojů Data Lake pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

    > [!NOTE]
    > Hello nástrojů Data Lake pro Visual Studio se dřív označovaly jako hello nástroje HDInsight pro Visual Studio.
    >
    > Nástroje data Lake pro Visual Studio jsou součástí hello __pracovního vytížení Azure__ pro Visual Studio 2017.

2. Otevřete Visual Studio, vyberte **soubor** > **nový** > **projektu**.

3. V hello **nový projekt** dialogové okno, rozbalte seznam **nainstalovaná** > **šablony**a potom vyberte **HDInsight**. Hello seznam šablon, vyberte **Storm ukázka**. V dolní části hello hello dialogového okna zadejte název aplikace hello.

    ![Bitové kopie](./media/hdinsight-storm-deploy-monitor-topology-linux/sample.png)

4. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **odeslání tooStorm v HDInsight**.

   > [!NOTE]
   > Pokud se zobrazí výzva, zadejte hello přihlašovací údaje pro vaše předplatné Azure. Pokud máte více než jedno předplatné, přihlaste se toohello, která obsahuje váš cluster Storm v HDInsight.

5. Vyberte váš cluster Storm v HDInsight z hello **Storm Cluster** rozevíracího seznamu a potom vyberte **odeslání**. Můžete sledovat, jestli je pomocí hello úspěšné odeslání hello **výstup** okno.

## <a name="submit-a-topology-ssh-and-hello-storm-command"></a>Odeslání topologii: SSH a hello Storm příkaz

1. Použití clusteru HDInsight toohello tooconnect SSH. Nahraďte **uživatelské jméno** hello název vaší přihlašování přes SSH. Nahraďte **CLUSTERNAME** názvem clusteru HDInsight:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Další informace o používání SSH tooconnect tooyour HDInsight clusteru najdete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Použijte následující příkaz toostart ukázkové topologie hello:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    Tento příkaz spustí hello ukázkovou topologii WordCount v clusteru hello. Tato topologie náhodně generovat věty a počet hello výskytem jednotlivých slov v hello věty.

   > [!NOTE]
   > Při odesílání topologie toohello clusteru, je nutné nejprve zkopírovat soubor jar hello obsahující hello cluster před použitím hello `storm` příkaz. toocopy hello souboru toohello clusteru, můžete použít hello `scp` příkaz. Například `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`.
   >
   > Příklad WordCount Hello a další příklady starter storm jsou již zahrnuty v clusteru na `/usr/hdp/current/storm-client/contrib/storm-starter/`.

## <a name="submit-a-topology-programmatically"></a>Odeslání topologii: prostřednictvím kódu programu

Topologie tooStorm v HDInsight programově nasadíte komunikaci s hello Nimbus služby hostované v clusteru. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-Storm-Topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) poskytuje příklad aplikaci Java, která ukazuje, jak toodeploy a spusťte topologii prostřednictvím hello Nimbus služby.

## <a name="monitor-and-manage-visual-studio"></a>Sledování a správě: Visual Studio

Když topologii byla úspěšně odeslána pomocí sady Visual Studio, hello **topologie Storm** hello clusteru se zobrazí v zobrazení. Vyberte topologii hello hello seznamu tooview informace o hello spuštěná topologie.

![monitorování v sadě Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

> [!NOTE]
> Můžete také zobrazit **topologie Storm** z **Průzkumníka serveru** rozšířením **Azure** > **HDInsight**a potom kliknete pravým tlačítkem cluster Storm v HDInsight a výběr **topologie Storm zobrazení**.

Vyberte hello tvar pro hello spouts nebo bolts tooview informace o těchto součástí. Otevře se nové okno pro každé vybrané položky.

### <a name="deactivate-and-reactivate"></a>Deaktivovat a znovu aktivovat

Deaktivace topologii pozastaví ho, dokud nebude ukončen nebo znovu aktivovat. tooperform těchto operací použít hello __deaktivovat__ a __znovu aktivovat__ tlačítka hello horní části hello __souhrn topologie__.

### <a name="rebalance"></a>Znovu vyvážit

Vyrovnává topologii umožňuje hello systému toorevise hello paralelismu topologie hello. Například pokud jste změnili hello clusteru tooadd další poznámky, vyrovnává umožňuje topologii toosee hello nové uzly.

toorebalance topologii, použijte hello __znovu vyvážit__ tlačítko hello horní části hello __souhrn topologie__.

> [!WARNING]
> Vyrovnává topologii nejprve deaktivuje hello topologie, pak znovu distribuuje pracovníci rovnoměrně mezi hello clusteru a potom nakonec vrátí hello topologie toohello stavu, ve kterém se nacházel před vyrovnává došlo k chybě. Takže pokud hello topologie byl aktivní, se zase aktivní. Pokud se ji deaktivovala, zůstane deaktivované.

### <a name="kill-a-topology"></a>Příkaz kill topologii

Topologie Storm pokračovat, spuštění, dokud jsou zastaveny nebo odstranění clusteru hello. toostop topologii, použijte hello __Kill__ tlačítko hello horní části hello __souhrn topologie__.

## <a name="monitor-and-manage-ssh-and-hello-storm-command"></a>Sledování a správě: SSH a hello Storm příkaz

Hello `storm` nástroje vám umožní toowork se spuštěnými topologiemi z příkazového řádku hello. Použití `storm -h` pro úplný seznam příkazů.

### <a name="list-topologies"></a>Seznam topologie

Použijte následující příkaz toolist hello všechny spuštěné topologie:

    storm list

Tento příkaz vrátí informace podobné toohello následující text:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Deaktivovat a znovu aktivovat

Deaktivace topologii pozastaví ho, dokud nebude ukončen nebo znovu aktivovat. Použijte následující příkaz toodeactivate hello a opětovná aktivace:

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Příkaz kill spuštěné topologie

Storm topologie, jednou spustit, pokračovat spuštění, dokud se zastavila. toostop topologii, hello použijte následující příkaz:

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a>Znovu vyvážit

Vyrovnává topologii umožňuje hello systému toorevise hello paralelismu topologie hello. Například pokud jste změnili hello clusteru tooadd další poznámky, vyrovnává umožňuje topologii toosee hello nové uzly.

> [!WARNING]
> Vyrovnává topologii nejprve deaktivuje hello topologie, pak znovu distribuuje pracovníci rovnoměrně mezi hello clusteru a potom nakonec vrátí hello topologie toohello stavu, ve kterém se nacházel před vyrovnává došlo k chybě. Takže pokud hello topologie byl aktivní, se zase aktivní. Pokud se ji deaktivovala, zůstane deaktivované.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a>Sledování a správě: Storm uživatelského rozhraní

Hello uživatelské rozhraní Storm poskytuje webové rozhraní pro práci se spuštěnými topologiemi a je součástí clusteru HDInsight. hello tooview uživatelské rozhraní Storm, použití webové prohlížeče tooopen **https://CLUSTERNAME.azurehdinsight.net/stormui**, kde **CLUSTERNAME** je hello názvem vašeho clusteru.

> [!NOTE]
> Pokud se zobrazí dotaz tooprovide uživatelské jméno a heslo, zadejte Správce clusteru hello (správce) a heslo použité při vytváření clusteru hello.

### <a name="main-page"></a>Hlavní stránka

Hello hlavní stránce hello uživatelské rozhraní Storm poskytuje hello následující informace:

* **Souhrn clusteru**: základní informace o clusteru Storm hello.
* **Souhrn topologie**: seznam spuštěných topologií. Hello odkazy v této části tooview použijte další informace o konkrétní topologie.
* **Souhrn nadřízeného**: informace o hello Storm nadřízeného.
* **Konfigurace nimbus**: Nimbus konfiguraci pro hello cluster.

### <a name="topology-summary"></a>Souhrn topologie

Výběr odkaz z hello **souhrn topologie** části zobrazí následující informace o topologii hello hello:

* **Souhrn topologie**: základní informace o topologii hello.
* **Topologie akce**: akce správy, které můžete provést pro hello topologie.

  * **Aktivovat**: obnoví zpracování deaktivované topologie.
  * **Deaktivovat**: Pozastaví spuštěné topologie.
  * **Znovu vyvážit**: upraví paralelismus hello hello topologie. Po změně hello počet uzlů v clusteru hello, znovu vyvážit spuštěné topologie. Tato operace povoluje hello topologie tooadjust paralelismus toocompensate pro hello zvětšit nebo zmenšit počet uzlů v clusteru hello.

    Další informace najdete v tématu <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">pochopení paralelismu topologie Storm hello</a>.
  * **Příkaz kill**: ukončí topologii Storm po hello zadaný časový limit.
* **Statistiky topologie**: Statistika hello topologie. tooset hello časový rámec hello zbývající položky na stránce hello, použijte odkazy hello hello **okno** sloupce.
* **Spouts**: hello funkcích spouts používané hello topologie. Použijte další informace o konkrétních funkcích spouts hello odkazy v této části tooview.
* **Bolts**: hello funkce bolts používané hello topologie. Použijte hello odkazy v této části tooview Další informace o konkrétních funkcích bolts.
* **Topologie konfigurace**: hello konfiguraci topologie hello vybrané.

### <a name="spout-and-bolt-summary"></a>Souhrn funkcí Bolt a spout

Výběr spout z hello **Spouts** nebo **Bolts** části zobrazí následující informace o položce vybrané hello hello:

* **Souhrn součást**: základní informace o funkcích hello spout nebo bolt.
* **Statistiky funkcí spout/Bolt**: Statistika hello spout nebo funkce bolt. tooset hello časový rámec hello zbývající položky na stránce hello, použijte odkazy hello hello **okno** sloupce.
* **Statistiky vstupu** (pouze funkce bolt): informace o hello vstupní datové proudy spotřebovávají hello bolt.
* **Statististiky výstupu**: informace o datových proudů hello vygenerované tímto objektem spout nebo funkce bolt.
* **Vykonavatelů**: informace o instancích hello hello spout nebo bolt. Vyberte hello **Port** položka pro konkrétní vykonavatele tooview vytvořeného protokolu diagnostické informace pro tuto instanci.
* **Chyby**: všechny informace o chybě pro tento spout nebo funkce bolt.

## <a name="monitor-and-manage-rest-api"></a>Sledování a správě: REST API

Hello uživatelské rozhraní Storm je postavený na hello rozhraní REST API, takže můžete provádět podobné správy a monitorování funkce pomocí hello REST API. Můžete vytvořit vlastní nástroje toocreate hello REST API pro správu a monitorování topologie Storm.

Další informace najdete v tématu [Storm uživatelského rozhraní REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). Hello tyto informace o konkrétní toousing hello REST API s Apache Storm v HDInsight.

> [!IMPORTANT]
> Hello Storm REST API není veřejně dostupné přes hello Internetu a musí být přístup pomocí SSH tunel toohello hlavního uzlu clusteru HDInsight. Informace o vytváření a používání tunelového propojení SSH naleznete v tématu [používání tunelového propojení SSH tooaccess Ambari webové uživatelské rozhraní, ResourceManager, JobHistory, NameNode, Oozie a jiné webové uživatelská](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Základní identifikátor URI

Hello základní identifikátor URI pro hello rozhraní API REST v clusterech HDInsight se systémem Linux je k dispozici hello hlavního uzlu v **https://HEADNODEFQDN:8744/api/v1/**. název domény Hello hello hlavního uzlu se vygeneruje při vytváření clusteru a nejsou statické.

Hello plně kvalifikovaný název domény (FQDN) pro hello hlavního uzlu clusteru můžete najít v několika různými způsoby:

* **Z relace SSH**: použijte příkaz hello `headnode -f` z clusteru toohello relace SSH.
* **Z webové Ambari**: vyberte **služby** hello horní části stránky hello, pak vyberte **Storm**. Z hello **Souhrn** vyberte **serveru uživatelského rozhraní Storm**. Hello plně kvalifikovaný název domény hello uzlu se systémem hello uživatelské rozhraní Storm a rozhraní API REST je v horní části hello hello stránky.
* **Z rozhraní Ambari REST API**: použijte příkaz hello `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` tooretrieve informace o hello uzlu, který hello uživatelské rozhraní Storm a REST API, které jsou spuštěny na. Nahraďte **heslo** s hello heslo správce pro hello cluster. Nahraďte **CLUSTERNAME** s názvem clusteru hello. V odpovědi hello obsahuje položku "název_hostitele" hello hello plně kvalifikovaný název domény uzlu hello.

### <a name="authentication"></a>Authentication

Toohello požadavky musí používat rozhraní REST API **základní ověřování**, takže můžete použít název správce clusteru HDInsight hello a heslo.

> [!NOTE]
> Protože základní ověřování je odeslána pomocí nešifrovaného textu, měli byste **vždy** používat s clusterem s hello toosecure komunikaci prostřednictvím protokolu HTTPS.

### <a name="return-values"></a>Návratové hodnoty

Informace, které se vrátí z hello REST API, může být pouze z použitelné v rámci clusteru hello nebo virtuální počítače na hello stejné virtuální síti Azure jako hello cluster. Například hello plně kvalifikovaný název domény (FQDN) pro servery Zookeeper vrátit není být dostupný z Internetu hello.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak toodeploy a sledování topologie pomocí hello řídicí panel Storm, se naučíte, jak příliš[založené na jazyce Java vyvíjet topologie pomocí nástroje Maven](hdinsight-storm-develop-java-topology.md).

Seznam Další příklad topologií najdete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).
