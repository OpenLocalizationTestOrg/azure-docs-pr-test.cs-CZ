---
title: "aaaInstall Jupyter místně & připojení clusteru Azure HDInsight Spark tooan | Microsoft Docs"
description: "Zjistěte, jak poznámkového bloku Jupyter tooinstall místně na vašem počítači a připojte ho tooan cluster Apache Spark v Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48593bdf-4122-4f2e-a8ec-fdc009e47c16
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 95c052110b84b677fd23048597af9511365cacfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-tooapache-spark-on-hdinsight"></a>Do počítače nainstalovat Poznámkový blok Jupyter a připojte tooApache Spark v HDInsight

V tomto článku zjistíte, jak poznámkového bloku Jupyter tooinstall, s hello vlastní PySpark (pro jazyk Python) a Spark (pro Scala) jader s Spark magic a připojit cluster HDInsight tooan hello poznámkového bloku. Může být několik důvodů tooinstall Jupyter v místním počítači a může být také některé běžné problémy. Další informace v této části hello [Proč v mém počítači nainstalujte Jupyter](#why-should-i-install-jupyter-on-my-computer) na konci hello tohoto článku.

Účastnící se instalace Jupyter a hello Spark magic v počítači jsou tři klíčové kroky.

* Nainstalujte poznámkového bloku Jupyter
* Instalace s hello Spark magic hello PySpark a Spark jádra
* Konfigurace Spark magic tooaccess Spark cluster v HDInsight

Další informace o hello vlastní jádra a magic Spark hello k dispozici pro poznámkové bloky Jupyter s clusterem HDInsight najdete v tématu [jádra dostupná pro poznámkové bloky Jupyter s Apache Spark Linux clusterů v HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Požadavky
Zde uvedené požadavky Hello nejsou pro instalaci Jupyter. Toto jsou pro připojování hello Jupyter poznámkového bloku tooan HDInsight cluster po instalaci hello Poznámkový blok.

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Do počítače nainstalovat poznámkového bloku Jupyter

Python je třeba nainstalovat před instalací poznámkové bloky Jupyter. Python a Jupyter jsou k dispozici jako součást hello [Anaconda distribuční](https://www.continuum.io/downloads). Při instalaci Anaconda instalujete distribuční jazyka Python. Po instalaci Anaconda přidáte hello Jupyter instalace spuštěním příslušnými příkazy.

1. Stáhnout hello [instalační program Anaconda](https://www.continuum.io/downloads) pro vaše platformy a instalační program spusťte hello. Při spuštění Průvodce instalací hello Ujistěte se, že jste vybrali hello možnost tooadd Anaconda tooyour proměnné PATH.
2. Spusťte následující příkaz tooinstall Jupyter hello.

        conda install jupyter

    Další informace o instalaci Jupyter najdete v tématu [Jupyter instalaci pomocí Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-hello-kernels-and-spark-magic"></a>Instalace jádra hello a Spark magic

Pokyny, jak tooinstall hello Spark magic, hello PySpark a Spark jádra podle pokynů instalace hello v hello [sparkmagic dokumentace](https://github.com/jupyter-incubator/sparkmagic#installation) na Githubu. Hello prvním krokem při hello Spark magic dokumentace žádostí tooinstall Spark magic. Nahraďte hello následující příkazy, v závislosti na verzi hello hello clusteru HDInsight, které se připojí k této prvním krokem při hello odkaz. Potom postupujte podle hello zbývající kroky v dokumentaci magic hello Spark. Pokud chcete tooinstall hello různých jádra, je nutné provést v hello Spark magic instalační pokyny části kroku 3.

* Pro clustery v3.4 nainstalujte sparkmagic 0.2.3 spuštěním`pip install sparkmagic==0.2.3`

* Pro clustery v3.5 a v3.6 nainstalujte sparkmagic 0.11.2 spuštěním`pip install sparkmagic==0.11.2`

## <a name="configure-spark-magic-tooconnect-toohdinsight-spark-cluster"></a>Konfigurace clusteru Spark tooHDInsight magic tooconnect Spark

V této části nakonfigurujete magic hello Spark, který jste nainstalovali starší cluster Apache Spark tooan tooconnect, který musí již jste vytvořili v Azure HDInsight.

1. informace o konfiguraci Jupyter Hello je obvykle uložen ve hello uživatelé domovský adresář. toolocate domovského adresáře na jakékoli platformě operačního systému, typ hello následující příkazy.

    Spusťte prostředí Python hello. V okně příkazového řádku zadejte následující hello:

        python

    Na hello prostředí Python zadejte následující příkaz toofind na domovský adresář hello hello.

        import os
        print(os.path.expanduser('~'))

2. Přejděte toohello domovský adresář a vytvořte složku s názvem **.sparkmagic** Pokud ještě neexistuje.
3. Ve složce hello, vytvořte soubor s názvem **config.json** a přidejte následující fragment kódu JSON je uvnitř hello.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. SUBSTITUTE **{USERNAME}**, **{CLUSTERDNSNAME}**, a **{BASE64ENCODEDPASSWORD}** s příslušnými hodnotami. Můžete použít několik nástrojů v oblíbených programovací jazyk nebo online toogenerate s kódováním base64 zakódované heslo pro váš vlastní heslo.

5. Konfigurace hello správné nastavení prezenčního signálu v `config.json`. Měli byste přidat tato nastavení na stejné úrovni jako hello hello `kernel_python_credentials` a `kernel_scala_credentials` fragmenty vaší přidaných výše. Příklad na tom, jak a kde tooadd hello nastavení prezenčního signálu najdete [ukázka config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

    * Pro `sparkmagic 0.2.3` (clusterů v3.4), zahrnují:

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * Pro `sparkmagic 0.11.2` (v3.5 clustery a v3.6), zahrnují:

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    >Prezenční signály odešlou tooensure nejsou úniku relací. Pokud počítač přejde toosleep nebo je vypnutý, není odeslán hello prezenčního signálu, výsledkem hello relace se vyčistit. Pro clustery v3.4, pokud chcete toodisable toto chování můžete nastavit hello Livy konfigurace `livy.server.interactive.heartbeat.timeout` příliš`0` z hello uživatelského rozhraní Ambari. Pro clustery v3.5 Pokud není nastavena konfigurace hello 3.5 výše, hello relace nebudou odstraněna.

6. Spusťte Jupyter. Použijte následující příkaz z příkazového řádku hello hello.

        jupyter notebook

7. Ověřte, zda se můžete připojit toohello clusteru pomocí poznámkového bloku Jupyter hello a, které můžete použít k dispozici magic Spark hello s hello jádra. Proveďte následující kroky hello.

    a. Vytvořte nový poznámkový blok. V pravém rohu hello, klikněte na **nový**. Měli byste vidět hello výchozí jádra **Python2** a hello dvě nové jádra, které nainstalujete, **PySpark** a **Spark**. Klikněte na tlačítko **PySpark**.

    ![Jádra v Poznámkový blok Jupyter](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "jádra v poznámkového bloku Jupyter")

    b. Spusťte hello následující fragment kódu.

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    Pokud se můžete úspěšně načíst výstup hello, je testován clusteru HDInsight toohello připojení.

    >[!TIP]
    >Pokud chcete tooupdate hello poznámkového bloku konfigurace tooconnect tooa jiného clusteru, aktualizujte hello config.json hello nové sady hodnot, jak je znázorněno v kroku 3 výše.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Proč nainstalovat Jupyter v mém počítači?
Může být číslo z důvodů, proč má tooinstall Jupyter v počítači a připojte jej clusteru tooa Spark v HDInsight.

* I když poznámkové bloky Jupyter jsou již k dispozici na hello cluster Spark v Azure HDInsight, instalaci do vašeho počítače Jupyter poskytuje hello možnost toocreate poznámkové bloky místně, aplikaci otestovat s spuštěného clusteru a potom odeslat hello cluster toohello poznámkových bloků. tooupload hello poznámkových bloků toohello clusteru, můžete buď je načíst pomocí poznámkového bloku Jupyter hello, který běží nebo hello clusteru, nebo je uložíte toohello /HdiNotebooks složek v účtu úložiště hello přidruženého k hello clusteru. Další informace o ukládání poznámkových bloků v hello clusteru najdete v tématu [úložiště poznámkové bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Díky poznámkových bloků hello k dispozici místně, můžete připojit clustery Spark toodifferent podle požadavků vaší aplikace.
* Můžete použít Githubu tooimplement systému správy zdrojů a mají Správa verzí pro poznámkové bloky hello. Můžete mít také možnost spolupráce při prostředí, kde více mohou uživatelé pracovat s hello stejné poznámkového bloku.
* Můžete pracovat s poznámkových bloků místně bez nutnosti i cluster s podporou. Potřebujete jenom tootest clusteru poznámkové bloky proti, není toomanually spravovat poznámkové bloky nebo vývojovém prostředí.
* Ho může být snazší tooconfigure vlastní místní vývojové prostředí než je tooconfigure hello Jupyter instalace v clusteru hello.  Můžete využít výhod všech hello softwaru, které jste nainstalovali místně bez konfigurace minimálně jeden vzdálený cluster.

> [!WARNING]
> S Jupyter nainstalovaná v místním počítači, můžete spustit více uživatelů hello stejné poznámkového bloku na clusteru stejné Spark v hello hello stejnou dobu. V takovém případě se vytvoří více Livy relací. Pokud narazíte na problém a chcete, aby mohla být složité úlohy tootrack, které Livy relace patří toowhich uživatele toodebug.
>
>

## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase](hdinsight-apache-spark-eventhub-streaming.md)
* [Analýza protokolu webu pomocí Sparku v HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Vytvoření a spouštění aplikací
* [Vytvoření samostatné aplikace pomocí Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Nástroje a rozšíření
* [Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
