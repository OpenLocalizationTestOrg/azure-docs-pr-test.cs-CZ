---
title: "aaaHue s Hadoop v clusterech se systémem HDInsight Linux - Azure | Microsoft Docs"
description: "Zjistěte, jak Hue tooinstall v HDInsight clustery a používání tunelového propojení tooroute hello požadavky tooHue. Používání úložiště toobrowse Hue a spusťte Hive nebo Pig."
keywords: HUE hadoop
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 9e57fcca-e26c-479d-a745-7b80a9290447
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: f086cbad2a90cc6903fbfccbf4a6be44f8999d07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Na nainstalovat a používat Hue clusterů systému HDInsight Hadoop

Zjistěte, jak Hue tooinstall v HDInsight clustery a používání tunelového propojení tooroute hello požadavky tooHue.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="what-is-hue"></a>Co je Hue?
HUE je že sada webových aplikací používá toointeract s clusterem Hadoop. Můžete použít Hue toobrowse hello úložiště přidružený k clusteru Hadoop (WASB, v případě hello clusterů HDInsight), spouštět úlohy Hive a Pig skripty a tak dále. Hello následující součásti jsou k dispozici s instalací Hue na clusteru HDInsight Hadoop.

* Editor Hive včelí vosk
* Pig
* Správce Metaúložiště.
* Oozie
* FileBrowser (který komunikuje tooWASB výchozí kontejner)
* Úloha prohlížeče

> [!WARNING]
> Součásti, které jsou součástí clusteru HDInsight hello jsou plně podporované a Microsoft Support bude pomoci tooisolate a vyřešit problémy související toothese součásti.
>
> Vlastní komponenty přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello. To může způsobit řešení problému hello nebo žádat, že jste tooengage dostupné kanály pro hello otevřít zdroj technologie, kterých se nachází hluboké znalosti pro tuto technologii. Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).
>
>

## <a name="install-hue-using-script-actions"></a>Instalace aplikace Hue pomocí akcí skriptů

Hello skriptu tooinstall Hue na clusteru HDInsight se systémem Linux je k dispozici na https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. Tento skript tooinstall Hue v clusterech s Azure Storage objekty BLOB (WASB) nebo Azure Data Lake Store můžete použít jako výchozí úložiště.

Tato část obsahuje pokyny, jak toouse hello skriptu při zřizování clusteru hello pomocí hello portálu Azure.

> [!NOTE]
> Azure PowerShell, hello rozhraní příkazového řádku Azure, hello HDInsight .NET SDK nebo šablon Azure Resource Manageru může být také použít tooapply akcí skriptů. Můžete také použít skript akce tooalready spuštěné clustery. Další informace najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Spuštění zřizování clusteru pomocí kroků hello v [zřizování clusterů HDInsight v Linuxu](hdinsight-hadoop-provision-linux-clusters.md), ale se nedokončí zřizování.

   > [!NOTE]
   > Hue tooinstall v HDInsight clustery, hello headnode doporučená velikost je alespoň A4 (8 jader, 14 GB paměti).
   >
   >
2. Na hello **volitelné konfiguraci** vyberte **akcí skriptů**a zadejte hello informace, jak je uvedeno níže:

    ![Zadejte parametry akce skriptu pro Hue](./media/hdinsight-hadoop-hue-linux/hue-script-action.png "poskytnout skripty parametry akcí pro Hue")

   * **NÁZEV**: Zadejte popisný název akce skriptu hello.
   * **Identifikátor URI skriptu**: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
   * **HEAD**: zaškrtnete tuto možnost,
   * **PRACOVNÍ**: nechat prázdné.
   * **ZOOKEEPER**: nechat prázdné.
   * **Parametry**: nechat prázdné.
3. Na konci hello hello **akcí skriptů**, použijte hello **vyberte** tlačítko toosave hello konfigurace. Nakonec použijte hello **vyberte** tlačítko dole hello hello **volitelné konfiguraci** okno toosave hello volitelné konfiguraci informace.
4. Pokračovat zřizování hello clusteru, jak je popsáno v [zřizování clusterů HDInsight v Linuxu](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="use-hue-with-hdinsight-clusters"></a>Použití Hue s clustery HDInsight

Tunelové připojení SSH je hello pouze tooaccess způsob Hue na hello clusteru, jakmile je spuštěna. Tunelové propojení pomocí protokolu SSH umožňuje toogo hello provoz přímo toohello headnode Dobrý den clusteru, kde je spuštěna Hue. Po dokončení zřizování hello clusteru použijte následující postup toouse Hue v clusteru HDInsight Linux hello.

> [!NOTE]
> Doporučujeme používat Firefox webové prohlížeče toofollow hello podle následujících pokynů.
>
>

1. Hello informace v [používání tunelového propojení SSH tooaccess Ambari webové uživatelské rozhraní, ResourceManager, JobHistory, NameNode, Oozie a dalších webového uživatelského rozhraní na](hdinsight-linux-ambari-ssh-tunnel.md) toocreate SSH tunelování z clusteru HDInsight toohello systému klienta a pak proveďte konfiguraci váš webový prohlížeč toouse hello tunelové propojení SSH jako proxy server.

2. Po vytvoření tunelu SSH a nakonfigurovaný provozu tooproxy prohlížeče jeho prostřednictvím, musíte vyhledat název hostitele hello hello primární hlavního uzlu. To provedete připojením toohello clusteru pomocí protokolu SSH na port 22. Například `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` kde **uživatelské jméno** se svým uživatelským jménem SSH a **CLUSTERNAME** je hello názvem vašeho clusteru.

    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Po připojení, použijte následující příkaz tooobtain hello plně kvalifikovaný název domény primární headnode hello hello:

        hostname -f

    Tato možnost vrátí název podobné toohello následující:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

    Toto je název hostitele hello hello primární headnode, kde se nachází webu Hue hello.
4. Používáte portál Hue hello prohlížeče tooopen hello na http://HOSTNAME:8888. Nahraďte název hostitele hello název, který jste získali v předchozím kroku hello.

   > [!NOTE]
   > Při přihlášení pro hello poprvé, budete výzvami toocreate účtu toolog toohello Hue portálu. Hello přihlašovacích údajů, který zde určíte bude omezený toohello portál a nejsou související toohello správce nebo pověření uživatele SSH, které jste zadali při zřizování clusteru hello.
   >
   >

    ![Portál Hue toohello přihlášení](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-login.png "zadejte přihlašovací údaje pro portál Hue")

### <a name="run-a-hive-query"></a>Spuštění dotazu Hive
1. Z portálu hello Hue, klikněte na tlačítko **dotazu editory**a potom klikněte na **Hive** tooopen hello Hive editor.

    ![Použijte Hive](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-use-hive.png "používání Hive")
2. Na hello **pomůže** v části **databáze**, měli byste vidět **hivesampletable**. Toto je ukázkový tabulku, která je součástí všech clusterů systému Hadoop v HDInsight. V pravém podokně hello zadejte ukázkový dotaz a zobrazí výstup hello na hello **výsledky** klikněte v podokně hello níže, jak je znázorněno v snímek obrazovky hello.

    ![Spuštění dotazu Hive](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-hive-query.png "spuštění Hive dotaz")

    Můžete taky hello **grafu** kartě toosee vizuální znázornění výsledků hello.

### <a name="browse-hello-cluster-storage"></a>Procházet hello úložiště clusteru
1. Z portálu hello Hue, klikněte na tlačítko **prohlížeč souborů** v pravém horním rohu hello hello řádku nabídek.
2. Ve výchozím nastavení otevře prohlížeč souborů hello v hello **/uživatel/myuser** adresáře. Klikněte na tlačítko hello lomítkem bezprostředně před adresář uživatelského hello hello cesta toogo toohello kořenové hello kontejneru úložiště Azure přidruženého k hello clusteru.

    ![Prohlížeč souborů použijte](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-file-browser.png "použijte prohlížeč souborů")
3. Klikněte pravým tlačítkem myši na soubor nebo složku toosee hello dostupné operace. Použití hello **nahrát** tlačítka na hello pravém rohu tooupload soubory toohello aktuální adresář. Použití hello **nový** tlačítko toocreate nové soubory nebo adresáře.

> [!NOTE]
> Prohlížeč souborů Hue Hello lze zobrazit pouze obsah hello hello výchozího kontejneru přidruženého k hello clusteru HDInsight. Žádné další úložiště účtů nebo kontejnery, které vám mohou být přidruženy s clusterem hello nebudou přístupné pomocí prohlížeče souboru hello. Ale hello další kontejnery přidruženého k hello clusteru bude vždy dostupné pro úlohy Hive hello. Například pokud zadáte příkaz hello `dfs -ls wasb://newcontainer@mystore.blob.core.windows.net` v editoru Hive hello, se zobrazí obsah hello také další kontejnerů. V tomto příkazu **newcontainer** není hello výchozí kontejner spojené s clusterem.
>
>

## <a name="important-considerations"></a>Důležité informace
1. Hue tooinstall skriptu použít Hello ho nainstaluje jenom na primární headnode hello hello clusteru.

2. Během instalace jsou pro aktualizaci konfigurace hello restartovat více služby Hadoop (HDFS, YARN, MR2, Oozie). Po dokončení instalace Hue hello skript může trvat nějakou dobu, než ostatní toostart služby Hadoop. Může to mít vliv na Hue výkonu původně. Po spuštění všech služeb, bude Hue plně funkční.
3. HUE nerozumí úlohách Tez, což je hello aktuální výchozí nastavení pro Hive. Pokud chcete toouse MapReduce jako hello modul pro spuštění Hive, aktualizujte hello skriptu toouse hello následující příkaz ve skriptu:

        set hive.execution.engine=mr;

4. U clusterů systému Linux můžete mít scénář, kde vaše služby spuštěné na primární headnode hello při hello Resource Manager může běžet na sekundární hello. Takové situaci může způsobit chyby (viz následující obrázek) při použití Hue tooview podrobnosti o úlohách SPUŠTĚNÁ na clusteru hello. Po dokončení úlohy hello však můžete zobrazit podrobnosti úlohy hello.

   ![Chyba portál Hue](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-error.png "chyba portál Hue")

   To je kvůli tooa známý problém. Jako alternativní řešení upravte Ambari, tak, aby hello active Resource Manager je spuštěna také na primární headnode hello.
5. HUE rozumí WebHDFS i clusterů HDInsight pomocí Azure Storage pomocí `wasb://`. Ano pomocí akce skriptu použít vlastní skript hello nainstaluje WebWasb, který je kompatibilní s webhdfs, které služba pro rozhovoru tooWASB. Ano, i když portál Hue hello uvádí HDFS místech (například když přesunutím ukazatele myši nad hello **prohlížeč souborů**), by měl být interpretován jako WASB.

## <a name="next-steps"></a>Další kroky
* [Nainstalujte Giraph clustery HDInsight](hdinsight-hadoop-giraph-install-linux.md). Použijte vlastní nastavení tooinstall clusteru, které Giraph na HDInsight Hadoop clusterů. Giraph vám umožní zpracování pomocí Hadoop tooperform grafů a dá použít s Azure HDInsight.
* [Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install-linux.md). Použijte vlastní nastavení tooinstall clusteru, které Solr na HDInsight Hadoop clusterů. Solr vám umožní operace výkonné hledání tooperform na uložená data.
* [Nainstalujte jazyk R v clusterech HDInsight](hdinsight-hadoop-r-scripts-linux.md). Použití clusteru přizpůsobení tooinstall R v clusterech HDInsight Hadoop. R je open-source jazyk a prostředí pro statistické výpočty. Poskytuje stovky statistické funkce předdefinované a vlastní programovací jazyk, který kombinuje aspektů funkčnosti a objektově orientované programování. Také poskytuje rozsáhlé možnosti grafického rozhraní.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
