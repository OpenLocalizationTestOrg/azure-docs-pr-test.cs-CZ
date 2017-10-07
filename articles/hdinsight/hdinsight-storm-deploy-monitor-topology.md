---
title: "aaaDeploy a správa topologií Apache Storm v HDInsight | Microsoft Docs"
description: "Zjistěte, jak toodeploy, monitorování a správa topologií Apache Storm pomocí hello řídicí panel Storm v HDInsight. Pomocí nástroje Hadoop pro sadu Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5e542072-f014-42aa-82d6-2694a76df520
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 05f05fe8dd519fe99fb771d36bfc3d28168ca85f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Nasazení a správa topologií Apache Storm v HDInsight se systémem Windows

Hello řídicí panel Storm můžete tooeasily nasazení a spuštění clusteru HDInsight tooyour topologií Apache Storm pomocí webového prohlížeče. Můžete také použít toomonitor hello řídicí panel a správě spuštěných topologií. Pokud používáte Visual Studio, hello nástroje HDInsight pro Visual Studio poskytují podobné funkce v sadě Visual Studio.

Hello řídicí panel Storm a hello Storm funkce hello nástroje HDInsight využívají hello Storm REST API, které lze použít toocreate vlastní řešení pro monitorování a správu.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují Storm v clusteru HDInsight se systémem Windows jako hello operační systém. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Informace o nasazení a správa topologií Storm pomocí clusteru služby HDInsight, který používá Linux najdete v tématu [nasazení a správa topologií Apache Storm v HDInsight se systémem Linux](hdinsight-storm-deploy-monitor-topology-linux.md)

## <a name="prerequisites"></a>Požadavky

* **Apache Storm v HDInsight** -najdete v části [začít pracovat s Apache Storm v HDInsight](hdinsight-apache-storm-tutorial-get-started.md) pokyny týkající se vytvoření clusteru.

* Pro hello **řídicí panel Storm**: moderní webový prohlížeč, který podporuje HTML5.

* Pro **Visual Studio** -Azure SDK 2.5.1 nebo novější a hello nástroje HDInsight pro Visual Studio. V tématu [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) tooinstall a nakonfigurujte hello nástroje HDInsight pro Visual Studio.

    Jedna z následujících verzí sady Visual Studio hello:

  * Visual Studio 2012 s aktualizací 4

  * Visual Studio 2013 s aktualizací 4 nebo Visual Studio 2013 Community

  * Visual Studio 2015 (všechny edice)

  * Visual Studio 2017 (všechny edice)

## <a name="storm-dashboard"></a>Řídicí panel Storm

Hello řídicí panel Storm je webová stránka, k dispozici v clusteru Storm. Adresa URL Hello je **https://&lt;clustername >.azurehdinsight.net/**, kde **clustername** je název hello Storm v clusteru HDInsight.

Hello horní části hello řídicí panel Storm, vyberte **odeslání topologie**. Postupujte podle pokynů hello na toorun stránku hello ukázková topologie nebo tooupload a spusťte topologie, kterou jste vytvořili.

![Hello odeslání stránky topologie][storm-dashboard-submit]

### <a name="storm-ui"></a>Storm uživatelského rozhraní

Hello řídicí panel Storm, vyberte hello **uživatelské rozhraní Storm** odkaz. Zobrazí informace o clusteru hello v přidání tooany spuštěnými topologiemi.

![uživatelské rozhraní storm Hello][storm-dashboard-ui]

> [!NOTE]
> U některých verzí Internet Explorer může se stát, že hello uživatelské rozhraní Storm neaktualizuje po jste ji nejprve navštívili. Například se nemusí zobrazit nové topologie hello odeslání, nebo se může zobrazit topologii jako aktivní, když je dříve deaktivováno. Společnost Microsoft si je vědoma tento problém a pracuje na řešení.

#### <a name="main-page"></a>Hlavní stránka

Hello hlavní stránce hello uživatelské rozhraní Storm poskytuje hello následující informace:

* **Souhrn clusteru**: základní informace o clusteru Storm hello.

* **Souhrn topologie**: seznam spuštěných topologií. Hello odkazy v této části tooview použijte další informace o konkrétní topologie.

* **Souhrn nadřízeného**: informace o hello Storm nadřízeného.

* **Konfigurace nimbus**: Nimbus konfiguraci pro hello cluster.

#### <a name="topology-summary"></a>Souhrn topologie

Výběr odkaz z hello **souhrn topologie** části zobrazí následující informace o topologii hello hello:

* **Souhrn topologie**: základní informace o topologii hello.

* **Topologie akce**: akce správy, které můžete provést pro hello topologie.

  * **Aktivovat**: obnoví zpracování deaktivované topologie.

  * **Deaktivovat**: Pozastaví spuštěné topologie.

  * **Znovu vyvážit**: upraví paralelismus hello hello topologie. Po změně hello počet uzlů v clusteru hello, znovu vyvážit spuštěné topologie. To umožňuje hello topologie tooadjust paralelismus toocompensate pro hello zvětšit nebo zmenšit počet uzlů v clusteru hello.

      Další informace najdete v tématu [pochopení paralelismu topologie Storm (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) hello](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

  * **Příkaz kill**: ukončí topologii Storm po hello zadaný časový limit.

* **Statistiky topologie**: Statistika hello topologie. Pomocí odkazů hello v hello **okno** sloupec tooset hello časový rámec hello zbývající položky na stránce hello.

* **Spouts**: hello funkcích spouts používané hello topologie. Použijte další informace o konkrétních funkcích spouts hello odkazy v této části tooview.

* **Bolts**: hello funkce bolts používané hello topologie. Použijte hello odkazy v této části tooview Další informace o konkrétních funkcích bolts.

* **Topologie konfigurace**: hello konfiguraci topologie hello vybrané.

#### <a name="spout-and-bolt-summary"></a>Souhrn funkcí Bolt a spout

Výběr spout z hello **Spouts** nebo **Bolts** části zobrazí následující informace o položce vybrané hello hello:

* **Souhrn součást**: základní informace o funkcích hello spout nebo bolt.

* **Statistiky funkcí spout/Bolt**: Statistika hello spout nebo funkce bolt. Pomocí odkazů hello v hello **okno** sloupec tooset hello časový rámec hello zbývající položky na stránce hello.

* **Statistiky vstupu** (pouze funkce bolt): informace o hello vstupní datové proudy spotřebovávají hello bolt.

* **Statististiky výstupu**: informace o datových proudů hello vygenerované tímto objektem spout nebo funkce bolt.

* **Vykonavatelů**: informace o instancích hello hello spout nebo bolt. Vyberte hello **Port** položka pro konkrétní vykonavatele tooview vytvořeného protokolu diagnostické informace pro tuto instanci.

* **Chyby**: všechny informace o chybě pro tento spout nebo funkce bolt.

## <a name="hdinsight-tools-for-visual-studio"></a>Nástroje HDInsight pro Visual Studio

Nástroje HDInsight Hello lze použít toosubmit jazyka C# nebo hybridní topologie tooyour cluster Storm. Hello následující kroky pomocí ukázkové aplikace. Informace o vytváření vlastního topologie pomocí nástrojů HDInsight hello najdete v tématu [vývoj topologií C# pomocí hello nástroje HDInsight pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Použijte následující postup toodeploy ukázka tooyour Storm v clusteru HDInsight hello pak zobrazovat a spravovat topologie hello.

1. Pokud jste ještě nenainstalovali hello nejnovější verzi hello nástroje HDInsight pro Visual Studio, najdete v části [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Otevřete Visual Studio, vyberte **soubor** > **nový** > **projektu**.

3. V hello **nový projekt** dialogové okno, rozbalte seznam **nainstalovaná** > **šablony**a potom vyberte **HDInsight**. Hello seznam šablon, vyberte **Storm ukázka**. V dolní části hello hello dialogového okna zadejte název aplikace hello.

    ![Bitové kopie](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

4. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **odeslání tooStorm v HDInsight**.

   > [!NOTE]
   > Pokud se zobrazí výzva, zadejte hello přihlašovací údaje pro vaše předplatné Azure. Pokud máte více než jedno předplatné, přihlaste se toohello, která obsahuje váš cluster Storm v HDInsight.

5. Vyberte váš cluster Storm v HDInsight z hello **Storm Cluster** rozevíracího seznamu a potom vyberte **odeslání**. Můžete sledovat, jestli je pomocí hello úspěšné odeslání hello **výstup** okno.

6. Když hello topologie byl úspěšně odeslán, hello **topologie Storm** pro hello clusteru by se měla objevit. Vyberte topologii hello hello seznamu tooview informace o hello spuštěná topologie.

    ![monitorování v sadě Visual studio](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > Můžete také zobrazit **topologie Storm** z **Průzkumníka serveru** rozšířením **Azure** > **HDInsight**a potom kliknete pravým tlačítkem cluster Storm v HDInsight a výběr **topologie Storm zobrazení**.

    Vyberte hello tvar pro hello spouts nebo bolts tooview informace o těchto součástí. Otevře se nové okno pro každé vybrané položky.

   > [!NOTE]
   > Název Hello hello topologie je název třídy hello topologie hello (v tomto případě `HelloWord`,) s časovým razítkem připojí.

7. Z hello **souhrn topologie** zobrazit, vyberte možnost **Kill** toostop hello topologie.

   > [!NOTE]
   > Topologie Storm pokračovat, spuštění, dokud jsou zastaveny nebo odstranění clusteru hello.


## <a name="rest-api"></a>REST API

Hello uživatelské rozhraní Storm je postavený na hello rozhraní REST API, takže můžete provádět podobné správy a monitorování funkce pomocí hello REST API. Můžete vytvořit vlastní nástroje toocreate hello REST API pro správu a monitorování topologie Storm.

Další informace najdete v tématu [Storm uživatelského rozhraní REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). Hello tyto informace o konkrétní toousing hello REST API s Apache Storm v HDInsight.

### <a name="base-uri"></a>Základní identifikátor URI

Hello je základní identifikátor URI pro hello rozhraní API REST v clusterech HDInsight **https://&lt;clustername >.azurehdinsight.net/stormui/api/v1/**, kde **clustername** je název hello Storm na HDInsight cluster.

### <a name="authentication"></a>Authentication

Toohello požadavky musí používat rozhraní REST API **základní ověřování**, takže můžete použít název správce clusteru HDInsight hello a heslo.

> [!NOTE]
> Protože základní ověřování je odeslána pomocí nešifrovaného textu, měli byste **vždy** používat s clusterem s hello toosecure komunikaci prostřednictvím protokolu HTTPS.

### <a name="return-values"></a>Návratové hodnoty

Informace, které se vrátí z hello REST API, může být pouze z použitelné v rámci clusteru hello nebo virtuální počítače na hello stejné virtuální síti Azure jako hello cluster. Například hello plně kvalifikovaný název domény (FQDN) pro servery Zookeeper vrátit nejsou být dostupný z Internetu hello.

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili, jak toodeploy a sledování topologie pomocí hello řídicí panel Storm, se naučíte, jak:

* [Vývoj topologie C# pomocí hello nástroje HDInsight pro Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [Vyvíjet topologie založené na jazyce Java pomocí nástroje Maven](hdinsight-storm-develop-java-topology.md)

Seznam Další příklad topologií najdete v tématu [příklad topologií pro Storm v HDInsight](hdinsight-storm-example-topology.md).

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
