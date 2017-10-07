---
title: "clusterů aaaKernels pro poznámkový blok Jupyter na Spark v Azure HDInsight | Microsoft Docs"
description: "Další informace o hello jádra PySpark, PySpark3 a Spark pro poznámkový blok Jupyter s clustery Spark v Azure HDInsight k dispozici."
keywords: "Poznámkový blok jupyter na spark, jupyter spark"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0719e503-ee6d-41ac-b37e-3d77db8b121b
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: nitinme
ms.openlocfilehash: 560c944fe850c5753ac9fa90550b804f0c47d14c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="kernels-for-jupyter-notebook-on-spark-clusters-in-azure-hdinsight"></a>Jádra pro poznámkový blok Jupyter na clustery Spark v Azure HDInsight 

Clustery HDInsight Spark poskytují jádra, která můžete pomocí poznámkového bloku Jupyter hello na Spark pro testování vašich aplikací. Jádro je program, který spouští a interpretuje vašeho kódu. jsou tři jádra Hello:

- **PySpark** – pro aplikace napsané v Python2
- **PySpark3** – pro aplikace napsané v Python3
- **Spark** – pro aplikace napsané v jazyce Scala

V tomto článku se dozvíte, jak toouse tyto jádra a výhod hello jejich používání.

## <a name="prerequisites"></a>Požadavky

* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>Vytvoření poznámkového bloku Jupyter v Spark HDInsight

1. Z hello [portál Azure](https://portal.azure.com/), otevřete váš cluster.  V tématu [seznamu a zobrazit clustery](hdinsight-administer-use-portal-linux.md#list-and-show-clusters) pokyny hello. Hello clusteru se otevře v novém okně portálu.

2. Z hello **rychlé odkazy** klikněte na tlačítko **clusteru řídicí panely** tooopen hello **clusteru řídicí panely** okno.  Pokud nevidíte **rychlé odkazy**, klikněte na tlačítko **přehled** hello levé nabídce v okně hello.

    ![Poznámkový blok Jupyter na Spark](./media/hdinsight-apache-spark-jupyter-notebook-kernels/hdinsight-jupyter-notebook-on-spark.png "Poznámkový blok Jupyter na Spark") 

3. Klikněte na tlačítko **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.
   
   > [!NOTE]
   > Může také dosáhnout hello Poznámkový blok Jupyter v clusteru Spark pomocí hello otevření následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** s hello název clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 

3. Klikněte na tlačítko **nový**a pak klikněte buď **Pyspark**, **PySpark3**, nebo **Spark** toocreate Poznámkový blok. Používat hello jádra Spark Scala aplikací jádra PySpark pro Python2 aplikace a PySpark3 jádra pro Python3 aplikace.
   
    ![Jádra pro poznámkový blok Jupyter na Spark](./media/hdinsight-apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "jádra pro poznámkový blok Jupyter na Spark") 

4. Poznámkový blok se otevře s hello jádra, který jste vybrali.

## <a name="benefits-of-using-hello-kernels"></a>Výhody používání jádra hello

Zde naleznete několik výhod nového jádra hello pomocí poznámkového bloku Jupyter na clustery Spark HDInsight.

- **Předvolby kontexty**. S **PySpark**, **PySpark3**, nebo hello **Spark** jádra, není nutné tooset kontexty Spark nebo Hive hello explicitně před zahájením práce s vašimi aplikacemi. Toto jsou k dispozici ve výchozím nastavení. Tyto kontexty jsou:
   
   * **sc** – pro kontext Spark
   * **sqlContext** – pro kontext Hive

    Ano nemáte toorun příkazy, jako je třeba hello tooset hello kontexty následující:

        sc = SparkContext('yarn-client') sqlContext = HiveContext(sc)

    Místo toho můžete přímo použít hello přednastavení kontexty ve vaší aplikaci.

- **Buňky Magic**. Hello jádra PySpark poskytuje některé předdefinované "Magic", které jsou speciální příkazy, které můžete volat s `%%` (například `%%MAGIC` <args>). příkaz magic Hello musí být první slovo hello v buňce kódu a povolit pro více řádků obsahu. Hello magic word by měl být hello první slovo v buňce hello. Přidání nic před hello magic, i komentáře, výsledkem bude chyba.     Další informace o Magic, které najdete v tématu [zde](http://ipython.readthedocs.org/en/stable/interactive/magics.html).
   
    Hello následující tabulka uvádí hello různých Magic, které jsou k dispozici prostřednictvím hello jádra.

   | Magic | Příklad | Popis |
   | --- | --- | --- |
   | Pomoc |`%%help` |Vytvoří tabulku všechny dostupné Magic hello s příklad a popis |
   | Informace o |`%%info` |Informace o relaci výstupy pro hello aktuální koncový bod Livy |
   | Konfigurace |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Nakonfiguruje hello parametry pro vytvoření relace. Hello příznak force (-f) je povinná, pokud relaci již byla vytvořena, což zajistí, že hello relace je vyřadit a vytvořit znovu. Podívejte se na [/sessions POST na Livy text žádosti](https://github.com/cloudera/livy#request-body) pro seznam platných parametrů. Parametry musí být předán jako řetězec formátu JSON a musí být na další řádek hello po hello magic, jak je znázorněno v příkladu sloupec hello. |
   | SQL |`%%sql -o <variable name>`<br> `SHOW TABLES` |Provede dotaz Hive proti hello sqlContext. Pokud hello `-o` parametr se předává, hello výsledek dotazu hello je uchován v hello %% lokální kontext Python jako [Pandas](http://pandas.pydata.org/) dataframe. |
   | místní |`%%local`<br>`a=1` |Všechny hello kód v další řádek se spustí místně. Kód musí být platný kód Python2 i bez ohledu na hello jádra, který používáte. Ano, i v případě, že jste vybrali **PySpark3** nebo **Spark** jádra při vytváření hello Poznámkový blok, pokud používáte hello `%%local` magic v buňce, dané buňky musí mít pouze platný kód Python2... |
   | Protokoly |`%%logs` |Výstupy hello protokoly pro aktuální relaci Livy hello. |
   | Odstranit |`%%delete -f -s <session number>` |Odstraní konkrétní relace hello aktuální Livy koncového bodu. Všimněte si, že nelze odstranit hello relace iniciovaného pro hello jádra sám sebe. |
   | Čištění |`%%cleanup -f` |Odstraní všechny hello relace pro hello aktuální Livy koncový bod, včetně relace tento poznámkový blok. platnost Hello příznak -f je povinný. |

   > [!NOTE]
   > Kromě toho toohello Magic, které jsou přidávány hello jádra PySpark, můžete použít také hello [předdefinované Magic IPython](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), včetně `%%sh`. Můžete použít hello `%%sh` kouzelná toorun skripty a blok kódu na headnode hello clusteru.
   >
   >
2. **Automaticky vizualizace**. Hello **Pyspark** jádra automaticky vizualizuje výstup hello dotazů Hive a SQL. Můžete zvolit několika různých typů vizualizace včetně tabulky, kruhový, řádku, oblasti, panelu.

## <a name="parameters-supported-with-hello-sql-magic"></a>Parametry podporovány s hello %% sql magic
Hello `%%sql` magic podporuje různé parametry, které můžete použít toocontrol hello druh výstup, která se zobrazí při spuštění dotazů. Hello následující tabulka uvádí výstup hello.

| Parametr | Příklad | Popis |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |Použijte tento parametr toopersist hello výsledek dotazu hello v hello %% lokální kontext Python, jako [Pandas](http://pandas.pydata.org/) dataframe. Hello název proměnné dataframe hello je hello název proměnné, které určíte. |
| -q |`-q` |Pomocí této tooturn vypnout vizualizace pro hello buňky. Pokud nechcete, aby tooauto-vizualizovat hello obsah buňky a právě chcete toocapture jej jako dataframe, potom použijte `-q -o <VARIABLE>`. Pokud chcete tooturn vypnout vizualizace bez zaznamenávání hello výsledky (například pro spuštění příkazu jazyka SQL, jako je třeba `CREATE TABLE` příkaz), použijte `-q` bez zadání `-o` argument. |
| -m |`-m <METHOD>` |Kde **metoda** je buď **trvat** nebo **ukázka** (výchozí hodnota je **trvat**). Pokud je metoda hello **trvat**, hello jádra vybere elementy shora hello hello výsledek datových sad určeného MAXROWS (popsané dál v této tabulce). Pokud je metoda hello **ukázka**, hello jádra náhodně ukázky elementy hello datových sad podle příliš`-r` parametr popsána dále v této tabulce. |
| -r |`-r <FRACTION>` |Zde **ZLOMEK** je číslo s plovoucí desetinnou čárkou mezi 0,0 a 1,0. Pokud je metoda hello ukázky pro dotaz SQL hello `sample`, pak hello jádra náhodně ukázky hello zadaný podíl hello elementy hello výsledku nastavení za vás. Například pokud spustíte dotaz SQL s argumenty hello `-m sample -r 0.01`, pak se náhodně vzorkovat 1 % hello výsledek řádků. |
| -n |`-n <MAXROWS>` |**MAXROWS** celočíselná hodnota. Hello jádra omezuje hello počet řádků, výstup příliš**MAXROWS**. Pokud **MAXROWS** záporné číslo, jako je **-1**, pak hello počet řádků v sadě výsledků hello není omezen. |

**Příklad:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

příkaz Hello výše hello následující:

* Vybere všechny záznamy z **hivesampletable**.
* Vzhledem k tomu, že používáme - q, vypne automatické vizualizace.
* Vzhledem k tomu, že používáme `-m sample -r 0.1 -n 500` ho náhodně ukázky 10 % hello řádků v hello hivesampletable a omezení hello velikost hello výsledek sadu too500 řádků.
* Nakonec protože jsme použili `-o query2` navíc šetří hello výstup do dataframe, nazývá **dotaz2**.

## <a name="considerations-while-using-hello-new-kernels"></a>Aspekty při použití nové jádra hello

Podle toho, která jádra, které používáte, ponechat poznámkových bloků hello systémem spotřebovává prostředky clusteru hello.  S tyto jádra protože jsou přednastavení kontexty hello, jednoduše ukončení poznámkových bloků hello není hello kontextu ukončit a proto hello prostředky clusteru pokračovat toobe používá. Doporučeným postupem je toouse hello **zavřít a zastavit** možnost hello poznámkového bloku **souboru** nabídky, když jste dokončili pomocí hello Poznámkový blok, který ukončí kontext hello a pak ukončí hello poznámkového bloku.     

## <a name="show-me-some-examples"></a>Ukázat některé příklady

Otevřete Poznámkový blok Jupyter, uvidíte dvě složky k dispozici na kořenové úrovni hello.

* Hello **PySpark** složka obsahuje ukázkové poznámkových bloků této hello použití nové **Python** jádra.
* Hello **Scala** složka obsahuje ukázkové poznámkových bloků této hello použití nové **Spark** jádra.

Můžete otevřít hello **00 - [přečtěte si NEJPRVE] funkce jádra Magic Spark** poznámkového bloku z hello **PySpark** nebo **Spark** složky toolearn o hello různých Magic, které jsou k dispozici. Můžete také použít jak hello k dispozici v části toolearn složky hello dva další poznámkových bloků ukázka tooachieve různé scénáře použití poznámkové bloky Jupyter s clustery HDInsight Spark.

## <a name="where-are-hello-notebooks-stored"></a>Kde jsou uložené poznámkových bloků hello?

Poznámkové bloky Jupyter ukládají toohello účtu úložiště přidruženého k hello clusteru pod hello **/HdiNotebooks** složky.  Poznámkové bloky, textové soubory a složky, které vytvoříte z v rámci Jupyter jsou přístupné z účtu úložiště hello.  Například, pokud používáte Jupyter toocreate složku **Moje_složka** a Poznámkový blok **myfolder/mynotebook.ipynb**, dostanete tento poznámkový blok v `/HdiNotebooks/myfolder/mynotebook.ipynb` v rámci účtu úložiště hello.  Hello zpětné je také nastavena hodnota true, to znamená, pokud nahrát Poznámkový blok přímo tooyour úložiště účet v `/HdiNotebooks/mynotebook1.ipynb`, hello Poznámkový blok je také zobrazit z Jupyter.  Poznámkové bloky zůstat v účtu úložiště hello i po odstranění clusteru hello.

způsob Hello poznámkových bloků se uloží toohello účet úložiště je kompatibilní s HDFS. Pokud tedy můžete SSH do clusteru hello, které můžete použít soubor příkazy pro správu, jak je znázorněno v následujícím fragmentu kódu hello:

    hdfs dfs -ls /HdiNotebooks                               # List everything at hello root directory – everything in this directory is visible tooJupyter from hello home page
    hdfs dfs –copyToLocal /HdiNotebooks                    # Download hello contents of hello HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb toohello root folder so it’s visible from Jupyter


V případě, že nedochází k potížím přístupu k účtu úložiště hello hello clusteru, poznámkových bloků hello jsou také uloženy na hello headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Podporovaný prohlížeč

Poznámkové bloky Jupyter na clustery Spark HDInsight jsou podporovány pouze na Google Chrome.

## <a name="feedback"></a>Váš názor
nové jádra Hello jsou v vyvíjející se fáze a bude pro dospělé v čase. To může znamenat, že rozhraní API může změnit, protože tyto jádra pro dospělé. Uvítáme jakékoli zpětnou vazbu, která máte při použití těchto nových jádra. To je užitečné v shaping hello finální verzi nástroje tyto jádra. Můžete ponechat vaše komentáře nebo názory pod hello **komentáře** oddíl hello dolní části tohoto článku.

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
* [Použití externích balíčků s poznámkovými bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
