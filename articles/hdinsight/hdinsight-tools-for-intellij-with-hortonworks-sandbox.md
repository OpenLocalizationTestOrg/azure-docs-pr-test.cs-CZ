---
title: "Použijte Azure sadu nástrojů pro IntelliJ s Hortonworks karanténě | Microsoft Docs"
description: "Naučte se používat nástroje HDInsight pro IntelliJ s Hortonworks karanténě v Azure nástrojů."
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
ms.openlocfilehash: c49f185db5a035f70a711bf309b973182d94a2b0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě

Naučte se používat nástroje HDInsight pro IntelliJ k vývoji aplikací Apache Scala a poté otestujte aplikace na [Hortonworks karanténě](http://hortonworks.com/products/sandbox/) běží na pracovní stanici. 

[IntelliJ IDEA](https://www.jetbrains.com/idea/) je Java integrované vývojové prostředí (IDE) pro vývoj počítačového softwaru. Po vyvinuté a testování vaší aplikace na Hortonworks karanténě, můžete přesunout jejich [Azure HDInsight](hdinsight-hadoop-introduction.md).

## <a name="prerequisites"></a>Požadavky

Než začnete tento kurz, musíte mít:

- Hortonworks Data Platform (HDP) 2.4 na Hortonworks karanténě spuštěné v místním prostředí. Konfigurace softwaru HDP najdete v tématu [Začínáme v ekosystému Hadoop s Hadoop izolovaném prostoru na virtuálním počítači](hdinsight-hadoop-emulator-get-started.md). 
    >[!NOTE]
    >Nástroje HDInsight pro IntelliJ byl testován pouze s HDP 2.4. Chcete-li získat HDP 2.4, rozbalte **Hortonworks karanténě archivu** na [Hortonworks karanténě stáhne lokality](http://hortonworks.com/downloads/#sandbox).

- [Java Developer Kit (JDK) verze 1,8 nebo novější](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). Sada nástrojů Azure vyžaduje JDK pro IntelliJ.

- [Edice community IntelliJ IDEA](https://www.jetbrains.com/idea/download) s [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) modulu plug-in a [nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij.md) modulu plug-in. Nástroje HDInsight pro IntelliJ je k dispozici jako součást sady nástrojů Azure pro IntelliJ. 

  Pokud chcete nainstalovat moduly plug-in, postupujte takto:

  1. Otevřete IntelliJ IDEA.
  2. Na **úvodní** obrazovku, vyberte **konfigurace**a potom vyberte **modulů plug-in**.
  3. Vyberte **JetBrains instalace modulu plug-in** v levém dolním rohu.
  4. Můžete vyhledat pomocí funkce vyhledávání **Scala**a potom vyberte **nainstalovat**.
  5. Vyberte **restartujte IntelliJ IDEA** k dokončení instalace.
  6. Opakováním kroků 4 a 5 k instalaci **nástrojů Azure pro IntelliJ**. Další informace najdete v tématu [nainstalovat sadu Azure pro IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="create-a-spark-scala-application"></a>Vytvoření aplikací Spark Scala

V této části vytvoříte projekt Scala ukázka pomocí IntelliJ IDEA. V další části můžete propojit IntelliJ IDEA Hortonworks karanténě (emulátoru) před odesláním projektu.

1. Otevřete IntelliJ IDEA z pracovní stanice. V **nový projekt** dialogové okno pole, postupujte takto:

   a. Vyberte **HDInsight** > **Spark v HDInsight (Scala)**.

   b. V **nástroj pro sestavení** vyberte některý z následujících akcí podle potřeby vašeho:

    * **Maven**, podpora Průvodce vytvoření projektu Scala
    * **SBT**, ke správě závislosti a vytváření projektu Scala

   ![Dialogové okno Nový projekt](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. Vyberte **Další**.

3. V dalším **nový projekt** dialogové okno pole, postupujte takto:

    ![Vytvoření IntelliJ Scala vlastnosti projektu](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    a. V **název projektu** zadejte název projektu.

    b. V **projektu umístění** zadejte umístění projektu.

    c. Vedle položky **SDK projektu** rozevíracího seznamu vyberte **nový**, vyberte **JDK**, a pak zadejte složku JDK Java verze 1.7 nebo novější. Vyberte **Java 1.8** pro Spark 2.x cluster nebo vyberte **Java 1.7** 1.x clusteru Spark. Výchozí umístění je C:\Program Files\Java\jdk1.8.x_xxx.

    d. V **Spark verze** rozevíracího seznamu, Průvodce vytvořením projektu Scala integruje správnou verzi sady SDK Spark a Scala SDK. Pokud verze clusteru Spark je starší než 2.0, vyberte **Spark 1.x**. Jinak vyberte možnost **Spark2.x**. Tento příklad používá Spark 1.6.2 (Scala 2.10.5). Ujistěte se, že používáte úložiště označena Scala 2.10.x. Nepoužívejte úložiště označena Scala 2.11.x.

4. Vyberte **Finish** (Dokončit).

5. Pokud **projektu** zobrazení ještě není otevřené, stiskněte **Alt + 1** ho otevřete.

6. V **Project Exploreru**, rozbalte projekt a potom vyberte **src**.

7. Klikněte pravým tlačítkem na **src**, přejděte na příkaz **nový**a potom vyberte **Scala třída**.

8. V **název** pole, zadejte název, **druhu** vyberte **objekt**a potom vyberte **OK**.

    ![Okno Vytvořit novou třídu Scala](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. V souboru .scala vložte následující kód:

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

10. Na **sestavení** nabídce vyberte možnost **sestavení projektu**. Ujistěte se, zda kompilace byla úspěšně dokončena.


## <a name="link-to-the-hortonworks-sandbox"></a>Odkaz na Hortonworks karanténě

Předtím, než můžete propojit Hortonworks karanténě (emulátoru), musí mít existující aplikaci IntelliJ.

Odkaz na emulátoru, postupujte takto:

1. Otevřete projekt v IntelliJ.

2. Na **zobrazení** nabídce vyberte možnost **nástroje Windows**a potom vyberte **Azure Explorer**.

3. Rozbalte položku **Azure**, klikněte pravým tlačítkem na **HDInsight**a potom vyberte **odkaz emulátor**.

4. V **odkaz A nový emulátor** okno, zadejte heslo, které jste nakonfigurovali pro kořenový účet Hortonworks karanténě, zadejte hodnoty, které jsou podobná nastavením na následujícím snímku obrazovky a pak vyberte **OK**. 

   ![Okno "Odkaz Nový emulátor"](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. Chcete-li nakonfigurovat emulátoru, vyberte **Ano**.

Když je emulátor připojen úspěšně, je emulátor (Hortonworks karanténě) uvedené v uzlu HDInsight.

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a>Odesílání aplikací Spark Scala k izolovanému prostoru Hortonworks

Po propojení IntelliJ IDEA pro emulátor, můžete odeslat projektu.

K odeslání projektu na emulátoru, postupujte takto:

1. V **Project Exploreru**, klikněte pravým tlačítkem na projekt a pak vyberte **aplikace odeslání Spark na HDInsight**.

2. Udělejte toto:

    a. V **cluster Spark (pouze Linux)** rozevíracího seznamu vyberte vaše místní Hortonworks karanténě.

    b. V **hlavní název třídy** pole, vyberte nebo zadejte název hlavní třídy. V tomto kurzu je název **GroupByTest**.

3. Vyberte **odeslání**. Protokoly odeslání úlohy se zobrazí v okně Nástroj odeslání Spark.

## <a name="next-steps"></a>Další kroky

- Postup vytvoření aplikací Spark pro HDInsight pomocí nástroje HDInsight pro IntelliJ, přejděte na [pomocí nástrojů HDInsight v Azure nástrojů pro IntelliJ k vytvoření aplikací Spark pro cluster HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin.md).

- Pokud chcete přehrát video, nástroje HDInsight pro IntelliJ, přejděte na [zavést nástroje HDInsight pro IntelliJ pro vývoj Spark](https://www.youtube.com/watch?v=YTZzYVgut6c).

- Chcete-li se dozvědět, jak k ladění aplikací Spark vzdáleně pomocí sady nástrojů v HDInsight pomocí SSH, přejděte na [vzdálené ladění aplikací Spark v clusteru HDInsight pomocí sady Azure Toolkit pro IntelliJ prostřednictvím SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).

- Chcete-li se dozvědět, jak k ladění aplikací Spark vzdáleně pomocí sady nástrojů v HDInsight prostřednictvím sítě VPN, přejděte na [pomocí nástrojů HDInsight v Azure nástrojů pro IntelliJ k ladění aplikací Spark vzdáleně na clusteru HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).

- Naučte se používat nástroje HDInsight pro Eclipse k vytvoření aplikací Spark, přejděte na [pomocí nástrojů HDInsight v Azure nástrojů pro Eclipse k vytvoření aplikací Spark](hdinsight-apache-spark-eclipse-tool-plugin.md).

- Pokud chcete přehrát video, nástroje HDInsight pro Eclipse, přejděte na [použití nástroje HDInsight pro Eclipse k vytvoření aplikací Spark](https://mix.office.com/watch/1rau2mopb6fha).
