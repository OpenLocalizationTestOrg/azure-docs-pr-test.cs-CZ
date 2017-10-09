---
title: "aaaInstall Presto v Azure HDInsight Linux clusterů | Microsoft Docs"
description: "Zjistěte, jak se tooinstall Presto a Airpal na systémem Linux HDInsight Hadoop clusterů, pomocí akcí skriptů."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: nitinme
ms.openlocfilehash: 5d54d0efc3e5fdc6f5a8d3a94ad2f61d16df24c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a>Na nainstalovat a používat Presto clusterů systému HDInsight Hadoop

V tomto tématu se dozvíte, jak tooinstall Presto na HDInsight Hadoop clusterů pomocí akce skriptu. Také zjistíte, jak tooinstall Airpal na stávající cluster Presto HDInsight.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují **clusteru HDInsight 3.5 Hadoop** Linux, který používá. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [HDInsight verze](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Co je Presto?
[Presto](https://prestodb.io/overview.html) je rychlé distribuované modul dotazů SQL pro velká data. Presto je vhodná pro interaktivní dotazování petabajty dat. Další informace o součásti hello Presto a jak pracují společně, najdete v části [Presto koncepty](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]
> Součásti, které jsou součástí clusteru HDInsight hello jsou plně podporované a Microsoft Support bude pomoci tooisolate a vyřešit problémy související toothese součásti.
> 
> Vlastní komponenty, například Presto přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello. To může způsobit řešení problému hello nebo žádat, že jste tooengage dostupné kanály pro hello otevřít zdroj technologie, kterých se nachází hluboké znalosti pro tuto technologii. Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).
> 
> 


## <a name="install-presto-using-script-action"></a>Nainstalujte Presto pomocí akce skriptu

Tato část obsahuje pokyny jak hello toouse hello ukázkový skript při vytváření nového clusteru pomocí portálu Azure. 

1. Spuštění zřizování clusteru pomocí kroků hello v [clustery HDInsight se systémem Linux](hdinsight-hadoop-create-linux-clusters-portal.md). Zajistěte, aby vytvoříte hello clusteru pomocí hello **vlastní** postup vytvoření clusteru. Je nutné zajistit, že splňuje následující požadavky hello hello clusteru, který vytvoříte.

    a. Musí to být clusteru Hadoop v prostředí HDInsight verze 3.5.

    b. Azure Storage musí používat jako úložiště dat hello. Použití Presto v clusteru, který používá Azure Data Lake Store jako možnost úložiště hello není dosud podporováno. 

    ![Vytvoření clusteru HDInsight pomocí vlastních možností](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. Na hello **upřesňující nastavení** vyberte **akcí skriptů**a zadejte níže uvedené informace hello:
   
   * **NÁZEV**: Zadejte popisný název akce skriptu hello.
   * **Identifikátor URI skriptu Bash:**`https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`
   * **HEAD**: zaškrtnete tuto možnost,
   * **PRACOVNÍ**: zaškrtnete tuto možnost,
   * **ZOOKEEPER**: zaškrtnutí tohoto políčka zrušte
   * **Parametry**: to pole ponechat prázdné


3. Na konci hello hello **akcí skriptů** okně klikněte na tlačítko hello **vyberte** tlačítko toosave hello konfigurace. Nakonec klikněte na hello **vyberte** tlačítko dole hello hello **Upřesnit nastavení** informace o konfiguraci okna toosave hello.

4. Pokračovat zřizování hello clusteru, jak je popsáno v [clustery HDInsight se systémem Linux](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]
    > Azure PowerShell, hello rozhraní příkazového řádku Azure, hello HDInsight .NET SDK nebo šablon Azure Resource Manageru může být také použít tooapply akcí skriptů. Můžete také použít skript akce tooalready spuštěné clustery. Další informace najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Použijte Presto s HDInsight

Proveďte následující kroky toouse Presto v clusteru služby HDInsight, po instalaci pomocí výše popsané kroky hello hello.

1. Připojte toohello clusteru HDInsight pomocí protokolu SSH:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
     

2. Spusťte prostředí Presto hello pomocí hello následující příkaz.
   
        presto --schema default

3. Spuštění dotazu v tabulce ukázka **hivesampletable**, která je k dispozici na všech clusterech HDInsight ve výchozím nastavení.
   
        select count (*) from hivesampletable;
   
    Ve výchozím nastavení [Hive](https://prestodb.io/docs/current/connector/hive.html) a [TPCH](https://prestodb.io/docs/current/connector/tpch.html) konektory pro Presto jsou již nakonfigurována. Hive konektor je nakonfigurované toouse hello výchozí nainstalován Hive instalace, takže všechny tabulky hello z podregistru budou automaticky viditelné v Presto.

    Podrobný popis na použití Presto naleznete v tématu [Presto dokumentaci](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Pomocí Airpal Presto

[Airpal](https://github.com/airbnb/airpal#airpal) je dotazu open-source webové rozhraní pro Presto. Další informace o Airpal najdete v tématu [Airpal dokumentaci](https://github.com/airbnb/airpal#airpal).

V této části se podíváme na hello kroky příliš**nainstalujte Airpal hello edgenode** clusteru HDInsight Hadoop, kde je již Presto nainstalována. Tím se zajistí, že tento hello Airpal webové rozhraní je k dispozici prostřednictvím hello Internetu.

1. Pomocí protokolu SSH, připojte toohello headnode hello clusteru HDInsight, která má nainstalovanou Presto:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Jakmile se připojíte, spusťte následující příkaz hello.

        sudo slider registry  --name presto1 --getexp presto 
   
    Měli byste vidět výstup jako hello následující:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. Z výstupu hello, poznamenejte si hodnotu hello pro hello **hodnotu** vlastnost. Je nutné při instalaci Airpal na edgenode hello clusteru. Z výstupu hello výše hello hodnotu, která je nutné je **10.0.0.12:9090**.

4. Použít šablonu hello  **[sem](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)**  toocreate HDInsight clusteru edgenode a zadejte hodnoty hello, jak ukazuje následující snímek obrazovky hello.

    ![Instalace HDInsight Airpal na Presto clusteru](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Klikněte na **Koupit**.

6. Jakmile jsou změny hello použité toohello konfiguraci clusteru, můžete pomocí následujících kroků hello přístup hello Airpal webové rozhraní.

    a. V okně hello clusteru, klikněte na tlačítko **aplikace**.

    ![Spuštění HDInsight Airpal na Presto clusteru](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    b. Z hello **nainstalované aplikace** okně klikněte na tlačítko **portál** proti airpal.

    ![Spuštění HDInsight Airpal na Presto clusteru](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    c. Po zobrazení výzvy zadejte přihlašovací údaje správce hello, které jste zadali při vytváření clusteru HDInsight Hadoop hello.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>Přizpůsobení Presto instalace na clusteru HDInsight

Po instalaci Presto na clusteru HDInsight Hadoop, můžete přizpůsobit hello instalace toomake změny např. nastavení paměti aktualizace, změnit konektorů, atd. Proveďte následující kroky toodo tak hello.

1. Pomocí protokolu SSH, připojte toohello headnode hello clusteru HDInsight, která má nainstalovanou Presto:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Provedené změny konfigurace v souboru hello `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Další informace o konfiguraci Presto, najdete v části [Presto konfigurace pro clustery založené na YARN](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).

3. Zastavení a ukončení hello aktuální spuštěnou instanci Presto.

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. Spusťte novou instanci třídy Presto s hello přizpůsobení.

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. Počkejte hello nové instance toobe připravené a poznamenejte si adresu presto coordinator.


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Generovat srovnávacího testu data pro clustery služby HDInsight se systémem Presto

TPC DS je hello oborový standard pro měření výkonu hello mnoho rozhodnutí podpora systémů, včetně systémy velkých objemů dat. Můžete použít Presto na HDInsight clustery toogenerate data a vyhodnotit, jak se porovnává s vlastními daty srovnávacího testu HDInsight. Další informace najdete v tématu [zde](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="see-also"></a>Viz také
* [Nainstalovat a používat Hue v clusterech HDInsight](hdinsight-hadoop-hue-linux.md). HUE je webové uživatelské rozhraní, které umožňuje snadno toocreate, spuštění a uložte úlohy Pig a Hive, stejně jako procházet hello výchozí úložiště pro váš cluster HDInsight.

* [Nainstalujte Giraph clustery HDInsight](hdinsight-hadoop-giraph-install-linux.md). Použijte vlastní nastavení tooinstall clusteru, které Giraph na HDInsight Hadoop clusterů. Giraph můžete graf tooperform zpracování pomocí Hadoop a lze použít s Azure HDInsight.

* [Nainstalujte Solr clustery HDInsight](hdinsight-hadoop-solr-install-linux.md). Použijte vlastní nastavení tooinstall clusteru, které Solr na HDInsight Hadoop clusterů. Solr vám umožní operace výkonné hledání tooperform na uložená data.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
