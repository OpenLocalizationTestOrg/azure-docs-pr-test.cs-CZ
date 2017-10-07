---
title: "aaaMachine učení příklad s MLlib Spark v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Spark MLlib toocreate aplikaci learning počítače, která analyzuje klasifikací prostřednictvím logistic regression datové sady."
keywords: "Spark strojového učení, spark machine learning příklad"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c0fd4baa-946d-4e03-ad2c-a03491bd90c8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 5c3b83482de5d8fba224398aaafe07fa67ec1fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-spark-mllib-toobuild-a-machine-learning-application-and-analyze-a-dataset"></a>Použít Spark MLlib toobuild machine learning aplikací a analýza datové sady

Zjistěte, jak toouse Spark **MLlib** toocreate a strojového učení toodo jednoduché Prediktivní analýza aplikace na otevřete datovou sadu. Z Spark předdefinované strojového učení knihovny, tento příklad používá *klasifikace* prostřednictvím logistic regression. 

> [!TIP]
> V tomto příkladu je také k dispozici jako poznámkový blok Jupyter v clusteru Spark (Linux), který vytvoříte v HDInsight. Hello poznámkového bloku prostředí vám umožní spustit hello Python fragmenty z Poznámkový blok hello sám sebe. kurz hello toofollow z v rámci Poznámkový blok, vytvoření Spark clusteru a spouštět poznámkového bloku Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`). Potom spusťte Poznámkový blok hello **Spark Machine Learning - prediktivní analýzy dat kontroly potravin pomocí MLlib.ipynb** pod hello **Python** složky.
>
>

MLlib je základní knihovna Spark, která poskytuje řadu nástrojů, které jsou užitečné pro úkoly strojového učení, včetně nástroje, které jsou vhodné pro:

* klasifikace
* Regrese
* Clustering
* Téma modelování
* Hodnota singulární rozložením (SVD) a analýzu hlavní součásti (PCA)
* Předpoklad testování a výpočet ukázka statistiky

## <a name="what-are-classification-and-logistic-regression"></a>Co jsou klasifikace a logistic regression?
*Klasifikace*, oblíbených strojového učení úloh, je hello proces řazení do kategorií vstupní data. Je hello úlohy z toofigure algoritmus klasifikace jak tooassign "popisků" tooinput data, která zadáte. Například může úvahách o algoritmu strojového učení, který přijímá uložené informace jako vstup a vydělí hello stock do dvou kategorií: které by měl prodeje a populací, které byste měli mít.

Logistic regression je hello algoritmus, který používáte pro klasifikaci. Spark logistic regression rozhraní API je užitečné pro *binární klasifikace*, nebo do jedné ze dvou skupin klasifikace vstupní data. Další informace o logistic regresí najdete v tématu [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

V souhrnu, vytváří hello proces logistic regression *logistic funkce* , může být použité toopredict hello pravděpodobnost, že vstupní vektoru patří jednu skupinu nebo hello jiné.  

## <a name="predictive-analysis-example-on-food-inspection-data"></a>Příklad prediktivní analýzy dat kontroly potravin
V tomto příkladu použijete Spark tooperform některé prediktivní analýzy dat kontroly potravin (**Food_Inspections1.csv**), byl získán v rámci hello [města Chicagu datovém portálu](https://data.cityofchicago.org/). Tato datová sada obsahuje informace o kontroly potravin zařízení, které byly provedeny v Chicagu, včetně informací o každé zařízení, porušení hello nalezen (pokud existuje) a hello výsledky kontroly hello. datový soubor CSV Hello je již k dispozici v účtu úložiště hello přidruženého k hello clusteru **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

V následujících kroků hello vývoji modelu toosee trvá toopass nebo selhání kontroly potravin.

## <a name="start-building-a-spark-mmlib-machine-learning-app"></a>Začněte vytvářet Spark MMLib strojového učení aplikace
1. Z hello [portál Azure](https://portal.azure.com/), z úvodního panelu hello klikněte hello dlaždici pro váš cluster Spark (Pokud je připnutý toohello úvodní panel). Můžete také přejít tooyour clusteru pod **Procházet vše** > **clustery HDInsight**.   
1. Z okna clusteru Spark hello, klikněte na tlačítko **řídicí panel clusteru**a potom klikněte na **Poznámkový blok Jupyter**. Pokud se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.

   > [!NOTE]
   > Může také nedostanete hello Poznámkový blok Jupyter pro váš cluster pomocí hello otevření následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** s hello název clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
1. Vytvořte poznámkový blok. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**.

    ![Vytvoření poznámkového bloku Jupyter](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "vytvoření nového poznámkového bloku Jupyter")
1. Nový poznámkový blok se vytvoří a otevřít s hello názvem Untitled.pynb. Klikněte na název hello poznámkového bloku v horní části hello a zadejte popisný název.

    ![Zadejte název pro hello notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "zadejte název pro hello Poznámkový blok")
1. Vzhledem k tomu, že jste vytvořili pomocí jádra PySpark hello Poznámkový blok, není nutné toocreate tvořit kontexty explicitně. kontexty Spark a Hive Hello jsou pro vás automaticky vytvoří při spuštění první buňky kódu hello. Můžete začít vytvářet svůj počítač učení aplikace importováním hello typů nezbytných pro tento scénář. toodo Ano, umístěte kurzor hello hello buňky a stiskněte klávesu **SHIFT + ENTER**.

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Vytvoření vstupní dataframe
Můžeme použít `sqlContext` tooperform transformací strukturovaná data. první úlohou Hello je tooload hello ukázková data ((**Food_Inspections1.csv**)) do Spark SQL *dataframe*.

1. Protože hello nezpracovaná data jsou ve formátu CSV, potřebujeme toouse hello Spark kontextu toopull každý jednotlivý řádek hello souboru do paměti jako nestrukturovaných text; potom použít jazyka Python sdíleného svazku clusteru knihovnu tooparse každý řádek zvlášť.

        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value

        inspections = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)
1. Jako RDD máme teď hello souboru CSV.  schéma hello toounderstand hello dat, můžeme ze hello RDD načíst jeden řádek.

        inspections.take(1)

    Měli byste vidět výstup jako hello následující:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of hello plumbing section of hello Municipal Code of Chicago and Rules and Regulation of hello Board of Health. OBSEVERD hello 3 COMPARTMENT SINK BACKING UP INTO hello 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding tooprotect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED tooREPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN hello REAR CHILDREN AREA,IN hello KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED tooREPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY hello 15MOS AREA. NEED tooBE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY hello EXPOSED HAND SINK IN hello KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: hello floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED tooELEVATE ALL FOOD ITEMS 6INCH OFF hello FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]
1. Hello předchozí výstup nám poskytuje základní přehled o hello schéma hello vstupní soubor. Obsahuje název hello každé zařízení, hello typ zařízení, hello adresu, data hello hello kontrol a hello umístění, mimo jiné. Umožňuje vybrat několik sloupců, které jsou užitečné pro naše prediktivní analýzy a výsledky hello skupiny jako dataframe, které jsme potom použít toocreate dočasné tabulky.

        schema = StructType([
        StructField("id", IntegerType(), False),
        StructField("name", StringType(), False),
        StructField("results", StringType(), False),
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')
1. Nyní je k dispozici *dataframe*, `df` na kterém jsme můžete provádět analýzy. Máme také volání dočasné tabulky **CountResults**. Jsme zahrnuli čtyři sloupce zájem o hello dataframe: **id**, **název**, **výsledky**, a **porušení**.

    Pojďme malé ukázkové hello dat:

        df.show(5)

    Měli byste vidět výstup jako hello následující:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES too...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-hello-data"></a>Pochopení hello dat
1. Začněme tooget představu o co obsahuje naší datové sadě. Co jsou například hello různé hodnoty v hello **výsledky** sloupec?

        df.select('results').distinct().show()

    Měli byste vidět výstup jako hello následující:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
1. Rychlé vizualizace pomůžete nám důvod informace o distribuci hello tyto výstupy. Jsme již máte data, hello v dočasné tabulce **CountResults**. Můžete spustit následující dotaz SQL na hello tabulky tooget hello lépe porozumět rozdělení hello výsledky.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    Hello `%%sql` magic následuje `-o countResultsdf` zajistí, že výstup hello hello dotazu je trvalé místně na serveru Jupyter hello (obvykle hello headnode hello clusteru). výstup Hello je uchován jako [Pandas](http://pandas.pydata.org/) dataframe s hello zadaný název **countResultsdf**.

    Měli byste vidět výstup jako hello následující:

    ![Výstup dotazu SQL](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "výstupu dotazu SQL")

    Další informace o hello `%%sql` magic a dalších Magic, které jsou k dispozici s jádrem pyspark hello, najdete v části [jádra dostupná v poznámkových blocích Jupyter s clustery Spark HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
1. Můžete také Matplotlib, knihovny použita tooconstruct vizualizaci dat, toocreate vykreslení. Protože hello výkresu musí být vytvořeny z místně hello trvalé **countResultsdf** dataframe, fragment kódu hello musí začínat řetězcem hello `%%local` magic. To zajistí hello kód je místně na serveru Jupyter hello.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Měli byste vidět výstup jako hello následující:

    ![Výstup Spark strojového učení aplikace - výsečového grafu s pěti výsledky kontroly odlišné](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark strojového učení výstup výsledků")
1. Můžete zjistit, že jsou 5 odlišné výsledky, které může mít kontrola:

   * Obchodní není umístěný.
   * Selhání
   * Předat
   * PSS s podmínky
   * Mimo firmy

     Dejte nám vyvinout model, který lze snadno uhodnout hello výsledek kontroly potravin, porušení dané hello. Vzhledem k tomu, že logistic regression je metoda binární klasifikace, má smysl toogroup naše data do dvou kategorií: **nezdaří** a **předat**. "Předat s podmínky" je stále Pass, takže když jsme trénování modelu hello, jsme zvažte hello dva výsledků ekvivalent. Data s hello jiných výsledků ("Obchodní není umístěný" nebo "mimo firemní") nejsou užitečné proto jsme odebrat z našich trénovací sady. Toto musí být v pořádku, vzhledem k tomu, že tyto dvě kategorie tvoří velmi malá část výsledků hello přesto.
1. Dejte nám pokračujte a převést naše stávající dataframe (`df`) do nového dataframe, kde je každý kontroly reprezentován jako pár porušení popisek. V našem případě štítek z `0.0` představuje selhání, popisek `1.0` představuje úspěšné a popisek `-1.0` představuje některé výsledky kromě těchto dvou. Jsme odfiltrovat jiných výsledků při výpočtu hello nové datové rámce.

        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')

    toosee jaké hello označené data, která vypadá, můžeme načíst jeden řádek.

        labeledData.take(1)

    Měli byste vidět výstup jako hello následující:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of hello food establishment and all parts of hello property used in connection with hello operation of hello establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF hello FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: hello flow of air discharged from kitchen fans shall always be through a duct tooa point above hello roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT tooDINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT tooOFFICE.")]

## <a name="create-a-logistic-regression-model-from-hello-input-dataframe"></a>Vytvoření modelu logistic regression ze vstupní dataframe hello
Naše poslední je označené data do formátu, který lze analyzovat pomocí logistic regression hello tooconvert. Hello vstupní tooa logistic regression algoritmus musí být sadu *popisek funkce vector páry*, kde je hello "funkce vector" vektor čísel představující hello vstupní bod. Ano potřebujeme tooconvert porušení"hello" sloupec, který je částečně strukturovaných a obsahuje mnoho komentáře ve volné, tooan pole reálná čísla, které by mohl snadno porozumíte na počítač.

Jeden standardní machine learning přístup pro zpracování přirozeného jazyka je tooassign všech jedinečných slov "index" a pak předejte vektoru toohello algoritmus strojového učení tak, aby každý index hodnota obsahuje hello relativní frekvenci dané slovo v textu hello řetězec.

MLlib poskytuje snadný způsob tooperform tuto operaci. Nejprve "tokenizaci" každé porušení řetězec tooget hello jednotlivých slov v každé řetězci. Poté použijte `HashingTF` tooconvert každou sadu tokeny do funkce vektor, který lze potom předaný toohello logistic regression algoritmus tooconstruct modelu. Můžeme provést všechny tyto kroky v pořadí pomocí "kanál".

    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

    model = pipeline.fit(labeledData)

## <a name="evaluate-hello-model-on-a-separate-test-dataset"></a>Vyhodnocení modelu hello na samostatné testovací datové sady
Můžeme použít model hello jsme vytvořili předtím příliš*předpovědi* jaké hello výsledky kontrol nové budou, podle hello porušení zásad, která měla dodržen. Jsme natrénovali tento model pro datovou sadu hello **Food_Inspections1.csv**. Dejte nám použít druhý datovou sadu, **Food_Inspections2.csv**, příliš*vyhodnotit* hello sílu tento model na nová data. Druhé sadě dat (**Food_Inspections2.csv**) by už měla být v hello výchozí kontejner úložiště přidruženého k hello clusteru.

1. Hello následující fragment vytváří nové dataframe, **predictionsDf** obsahující hello předpovědi generované hello modelu. fragment kódu Hello také vytvoří dočasné tabulky nazývané **předpovědi** podle hello dataframe.

        testData = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns

    Měli byste vidět výstup jako hello následující:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']
1. Podívejte se na jednu z hello předpovědi. Spusťte tento fragment kódu:

        predictionsDf.take(1)

   Není předpovědi hello první položku v hello testovací datové sady.
1. Hello `model.transform()` metoda platí hello stejnou transformaci tooany nová data s hello stejné schéma a přicházejí na předpověď jak tooclassify hello data. Některé jednoduché statistiky tooget můžeme představu o jak přesný byly naše předpovědi udělat:

        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                              (prediction = 1 AND (results = 'Pass' OR
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()

        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    výstup Hello vypadá hello následující:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate

    Pomocí Spark logistic regression nám poskytuje model přesné hello vztahu mezi popisy porušení v angličtině a zda se daný obchodní úspěch nebo selhání kontroly potravin.

## <a name="create-a-visual-representation-of-hello-prediction"></a>Vytvoření vizuální reprezentace předpovědi hello
Nyní jsme můžete vytvořit toohelp konečné vizualizace, které nám důvodu o hello výsledky tento test.

1. Začneme extrahováním hello různých předpovědi a výsledky z hello **předpovědi** dočasné tabulky vytvořili dříve. Hello následující dotazy oddělit výstup hello jako *true_positive*, *false_positive*, *true_negative*, a *false_negative*. V dotazech hello níže, jsme vypnout vizualizaci pomocí `-q` a také uložit výstup hello (pomocí `-o`) jako dataframes, který lze potom použít s hello `%%local` magic.

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
1. Nakonec použijte následující fragment kódu toogenerate hello výkresu pomocí hello **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Měli byste vidět hello následující výstup:

    ![Spark strojového učení výstupu aplikace - výsečového grafu procenta kontroly potravin se nezdařilo. ] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark strojového učení výstup výsledků")

    V tomto grafu odkazuje "pozitivní" výsledek kontroly potravin toohello se nezdařilo, zatímco záporný výsledek odkazuje tooa předán kontroly.

## <a name="shut-down-hello-notebook"></a>Vypnout hello Poznámkový blok
Po dokončení spuštění aplikace hello byste měli vypínat hello poznámkového bloku toorelease hello prostředky. toodo Ano, z hello **soubor** nabídce hello Poznámkový blok, klikněte na tlačítko **zavřít a zastavit**. To ukončí a zavře hello poznámkového bloku.

## <a name="seealso"></a>Viz také
* [Přehled: Apache Spark v Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scénáře
* [Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI](hdinsight-apache-spark-use-bi-tools.md)
* [Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
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
* [Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Správa prostředků
* [Správa prostředků hello cluster Apache Spark v Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight](hdinsight-apache-spark-job-debugging.md)
