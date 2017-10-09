---
title: "aaaUse nástrojů Azure pro IntelliJ s Hortonworks karanténě | Microsoft Docs"
description: "Zjistěte, jak toouse HDInsight nástroje v Azure nástrojů pro IntelliJ s Hortonworks karanténě."
keywords: "nástroje hadoop hive dotaz, intellij, hortonworks karanténě, azure nástrojů pro intellij"
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: b587cc9b-a41a-49ac-998f-b54d6c0bdfe0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 2bf97068a9cec99fcc7b4ff9469b91d8cbe2a8d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě

Zjistěte, jak toouse nástroje HDInsight pro IntelliJ toodevelop Apache Scala aplikace a pak testování hello aplikace na [Hortonworks karanténě](http://hortonworks.com/products/sandbox/) běží na pracovní stanici. 

[IntelliJ IDEA](https://www.jetbrains.com/idea/) je Java integrované vývojové prostředí (IDE) pro vývoj počítačového softwaru. Po vyvinuté a testování vaší aplikace na Hortonworks karanténě, můžete je přesunout příliš[Azure HDInsight](hdinsight-hadoop-introduction.md).

## <a name="prerequisites"></a>Požadavky

Než začnete tento kurz, musíte mít:

- Hortonworks Data Platform (HDP) 2.4 na Hortonworks karanténě spuštěné v místním prostředí. tooconfigure HDP, najdete v části [Začínáme v ekosystému Hadoop hello s Hadoop izolovaném prostoru na virtuálním počítači](hdinsight-hadoop-emulator-get-started.md). 
    >[!NOTE]
    >Nástroje HDInsight pro IntelliJ byl testován pouze s HDP 2.4. Rozbalte tooget HDP 2.4 **Hortonworks karanténě archivu** na hello [Hortonworks karanténě stáhne lokality](http://hortonworks.com/downloads/#sandbox).

- [Java Developer Kit (JDK) verze 1,8 nebo novější](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). Je požadován JDK hello Azure Toolkit pro IntelliJ.

- [Edice community IntelliJ IDEA](https://www.jetbrains.com/idea/download) s hello [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) modulu plug-in a hello [nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij.md) modulu plug-in. Nástroje HDInsight pro IntelliJ je k dispozici jako součást hello nástrojů Azure pro IntelliJ. 

  tooinstall hello moduly plug-in, hello následující:

  1. Otevřete IntelliJ IDEA.
  2. Na hello **úvodní** obrazovku, vyberte **konfigurace**a potom vyberte **modulů plug-in**.
  3. Vyberte **JetBrains instalace modulu plug-in** v levém dolním rohu hello.
  4. Použít hello hledání funkce toosearch pro **Scala**a potom vyberte **nainstalovat**.
  5. Vyberte **restartujte IntelliJ IDEA** toocomplete hello instalace.
  6. Opakováním kroků 4 a 5 tooinstall hello **nástrojů Azure pro IntelliJ**. Další informace najdete v tématu [hello instalace nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="create-a-spark-scala-application"></a>Vytvoření aplikací Spark Scala

V této části vytvoříte projekt Scala ukázka pomocí IntelliJ IDEA. V další části hello propojit IntelliJ IDEA toohello Hortonworks karanténě (emulátoru) před odesláním hello projektu.

1. Otevřete IntelliJ IDEA z pracovní stanice. V hello **nový projekt** dialogové okno pole, hello následující:

   a. Vyberte **HDInsight** > **Spark v HDInsight (Scala)**.

   b. V hello **nástroj pro sestavení** vyberte jednu z následujících, podle potřeby tooyour hello:

    * **Maven**, podpora Průvodce vytvoření projektu Scala
    * **SBT**, pro správu hello závislosti a sestavování pro projekt hello Scala

   ![Dialogové okno Nový projekt Hello](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. Vyberte **Další**.

3. V hello Další **nový projekt** dialogové okno pole, hello následující:

    ![Vytvoření IntelliJ Scala vlastnosti projektu](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    a. V hello **název projektu** zadejte název projektu.

    b. V hello **projektu umístění** zadejte umístění projektu.

    c. Další toohello **SDK projektu** rozevíracího seznamu vyberte **nový**, vyberte **JDK**, a pak zadejte hello složky JDK Java verze 1.7 nebo novější. Vyberte **Java 1.8** hello Spark 2.x clusteru, nebo vyberte **Java 1.7** hello Spark 1.x clusteru. Hello výchozí umístění je C:\Program Files\Java\jdk1.8.x_xxx.

    d. V hello **Spark verze** rozevíracího seznamu, Průvodce vytvořením projektu Scala integruje hello správnou verzi sady SDK Spark a Scala SDK. Pokud verze clusteru Spark hello je starší než 2.0, vyberte **Spark 1.x**. Jinak vyberte možnost **Spark2.x**. Tento příklad používá Spark 1.6.2 (Scala 2.10.5). Ujistěte se, že používáte úložiště hello označena Scala 2.10.x. Nepoužívejte hello úložiště označena Scala 2.11.x.

4. Vyberte **Finish** (Dokončit).

5. Pokud hello **projektu** zobrazení ještě není otevřené, stiskněte **Alt + 1** tooopen ho.

6. V **Project Exploreru**, rozbalte hello projektu a pak vyberte **src**.

7. Klikněte pravým tlačítkem na **src**, bod příliš**nový**a potom vyberte **Scala třída**.

8. V hello **název** zadejte název, v hello **druhu** vyberte **objekt**a potom vyberte **OK**.

    ![okno Vytvořit novou třídu Scala Hello](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. V souboru .scala hello vložte následující kód hello:

        import java.util.Random
        import org.apache.spark.{SparkConf, SparkContext}
        import org.apache.spark.SparkContext._

        /**
        * Usage: GroupByTest [numMappers] [numKVPairs] [valSize] [numReducers]
        */
        object GroupByTest {
            def main(args: Array[String]) {
                val sparkConf = new SparkConf().setAppName("GroupBy Test")
                var numMappers = 3
                var numKVPairs = 10
                var valSize = 10
                var numReducers = 2

                val sc = new SparkContext(sparkConf)

                val pairs1 = sc.parallelize(0 until numMappers, numMappers).flatMap { p =>
                val ranGen = new Random
                var arr1 = new Array[(Int, Array[Byte])](numKVPairs)
                for (i <- 0 until numKVPairs) {
                    val byteArr = new Array[Byte](valSize)
                    ranGen.nextBytes(byteArr)
                    arr1(i) = (ranGen.nextInt(Int.MaxValue), byteArr)
                }
                arr1
                }.cache
                // Enforce that everything has been calculated and in cache
                pairs1.count

                println(pairs1.groupByKey(numReducers).count)
            }
        }

10. Na hello **sestavení** nabídce vyberte možnost **sestavení projektu**. Ujistěte se, zda text hello kompilace byla úspěšně dokončena.


## <a name="link-toohello-hortonworks-sandbox"></a>Odkaz toohello Hortonworks karanténě

Chcete-li propojit tooa Hortonworks karanténě (emulátoru), musí mít existující aplikaci IntelliJ.

toolink tooan emulátoru, hello následující:

1. Hello otevřete projekt v IntelliJ.

2. Na hello **zobrazení** nabídce vyberte možnost **nástroje Windows**a potom vyberte **Azure Explorer**.

3. Rozbalte položku **Azure**, klikněte pravým tlačítkem na **HDInsight**a potom vyberte **odkaz emulátor**.

4. V hello **odkaz A nový emulátor** okno, zadejte heslo hello jste nakonfigurovali pro kořenový účet hello hello Hortonworks karanténě, zadejte hodnoty podobné toothose v hello následující snímek obrazovky a pak vyberte **OK** . 

   ![okno "Odkaz Nový emulátor" Hello](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. tooconfigure hello emulátoru, vyberte **Ano**.

Když se úspěšně připojí hello emulátoru, hello emulátoru (Hortonworks karanténě) je uvedena v uzlu HDInsight hello.

## <a name="submit-hello-spark-scala-application-toohello-hortonworks-sandbox"></a>Odeslání toohello aplikací Spark Scala hello Hortonworks karanténě

Po propojení IntelliJ IDEA toohello emulátoru, můžete odeslat projektu.

toosubmit tooan projektu emulátoru hello následující:

1. V **Project Exploreru**, klikněte pravým tlačítkem na projekt hello a pak vyberte **odeslání Spark aplikace tooHDInsight**.

2. Hello následující:

    a. V hello **cluster Spark (pouze Linux)** rozevíracího seznamu vyberte vaše místní Hortonworks karanténě.

    b. V hello **hlavní název třídy** pole, vyberte nebo zadejte název hlavní třídy hello. V tomto kurzu je název hello **GroupByTest**.

3. Vyberte **odeslání**. Hello úlohy odeslání protokolů se zobrazí v okně nástroje odeslání hello Spark.

## <a name="next-steps"></a>Další kroky

- jak toocreate aplikací Spark pro HDInsight pomocí nástroje HDInsight pro IntelliJ, přejděte příliš toolearn[pomocí nástrojů HDInsight v Azure nástrojů pro IntelliJ toocreate aplikací Spark pro cluster HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin.md).

- toowatch video o nástroje HDInsight pro IntelliJ, přejděte příliš[zavést nástroje HDInsight pro IntelliJ pro vývoj Spark](https://www.youtube.com/watch?v=YTZzYVgut6c).

- toolearn jak toodebug aplikací Spark pomocí hello toolkit vzdáleně v HDInsight pomocí SSH, přejděte příliš[vzdálené ladění aplikací Spark v clusteru HDInsight pomocí sady Azure Toolkit pro IntelliJ prostřednictvím SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).

- toolearn jak toodebug aplikací Spark pomocí hello toolkit vzdáleně v HDInsight prostřednictvím sítě VPN, přejděte příliš[pomocí nástrojů HDInsight v Azure nástrojů pro IntelliJ toodebug aplikací Spark vzdáleně na clusteru HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).

- jak toouse nástroje HDInsight pro Eclipse toocreate Spark aplikaci, přejděte příliš toolearn[pomocí nástrojů HDInsight v Azure nástrojů pro aplikací Spark toocreate Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md).

- toowatch video o nástroje HDInsight pro prostředí Eclipse, přejděte příliš[použijte nástroj HDInsight pro aplikací Spark toocreate Eclipse](https://mix.office.com/watch/1rau2mopb6fha).
