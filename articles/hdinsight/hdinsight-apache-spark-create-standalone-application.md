---
title: aaaCreate Scala aplikace toorun na clustery Spark - Azure HDInsight | Microsoft Docs
description: "Vytvoření Spark aplikace napsané v jazyce Scala s Apache Maven jako hello sestavení pro Scala poskytované IntelliJ IDEA systému a existující archetype Maven."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b2467a40-a340-4b80-bb00-f2c3339db57b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: b25291b60921021486f55d78b4832a070a54d163
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-scala-maven-application-toorun-on-apache-spark-cluster-on-hdinsight"></a>Vytvoření Scala Maven toorun aplikace na cluster Apache Spark v HDInsight

Zjistěte, jak toocreate Spark aplikace napsané v jazyce Scala pomocí nástroje Maven s IntelliJ IDEA. článek Hello používá Apache Maven hello sestavení systému a začíná existující archetype Maven pro Scala poskytované IntelliJ IDEA.  Vytvoření aplikace Scala v IntelliJ IDEA zahrnuje hello následující kroky:

* Použijte Maven jako systém sestavení hello.
* Aktualizujte projektu objektu modelu (POM) souboru tooresolve Spark modulu závislosti.
* Zápis v jazyce Scala vaší aplikace.
* Generovat soubor jar, který může být odeslaná tooHDInsight clustery Spark.
* Spusťte aplikaci hello na clusteru Spark pomocí Livy.

> [!NOTE]
> HDInsight také poskytuje IntelliJ IDEA modulu plug-in nástroje tooease hello procesu vytváření a odesílání aplikací tooan clusteru HDInsight Spark na systému Linux. Další informace najdete v tématu [pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark](hdinsight-apache-spark-intellij-tool-plugin.md).
> 
> 

## <a name="prerequisites"></a>Požadavky

* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Cluster Apache Spark v HDInsight. Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Java Development kit Oracle. Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Java IDE. Tento článek používá IntelliJ IDEA 15.0.1. Můžete nainstalovat z [zde](https://www.jetbrains.com/idea/download/).

## <a name="install-scala-plugin-for-intellij-idea"></a>Instalace modulu plug-in Scala pro IntelliJ IDEA
Pokud instalace IntelliJ IDEA není nebyl vyzván k povolení modulu plug-in Scala, spusťte IntelliJ IDEA a projít hello následující modul plug-in hello tooinstall kroky:

1. Spusťte IntelliJ IDEA a na úvodní obrazovce klikněte na **konfigurace** a pak klikněte na **modulů plug-in**.
   
    ![Povolení modulu plug-in scala](./media/hdinsight-apache-spark-create-standalone-application/enable-scala-plugin.png)
2. Hello na další obrazovce klikněte na tlačítko **JetBrains instalace modulu plug-in** z levého dolního rohu hello. V hello **procházet modulů plug-in JetBrains** dialogové okno otevře, vyhledejte Scala a pak klikněte na **nainstalovat**.
   
    ![Instalace modulu plug-in scala](./media/hdinsight-apache-spark-create-standalone-application/install-scala-plugin.png)
3. Po hello modul plug-in nainstaluje úspěšně, klikněte na tlačítko hello **tlačítko Restartovat IntelliJ IDEA** toorestart hello IDE.

## <a name="create-a-standalone-scala-project"></a>Vytvoření projektu samostatné Scala
1. Spusťte IntelliJ IDEA a vytvoření nového projektu. V hello projektu dialogové okno Nový, zkontrolujte hello následující možnosti a pak klikněte na **Další**.
   
    ![Vytvořte projekt Maven](./media/hdinsight-apache-spark-create-standalone-application/create-maven-project.png)
   
   * Vyberte **Maven** jako typ projektu hello.
   * Zadejte **projektu SDK**. Kliknutím na tlačítko Nová a přejděte toohello Java instalační adresář, obvykle `C:\Program Files\Java\jdk1.8.0_66`.
   * Vyberte hello **vytvořit z archetype** možnost.
   * Vyberte ze seznamu hello archetypes, **org.scala tools.archetypes:scala archetype jednoduchý**. Tato akce vytvoří hello správné adresářovou strukturu a stáhnout požadované hello výchozí závislosti toowrite Scala program.
2. Zadejte příslušné hodnoty pro **GroupId**, **ArtifactId**, a **verze**. Klikněte na **Další**.
3. V hello další dialogové okno, kde zadáte domovský adresář Maven a další nastavení uživatele, přijměte výchozí hodnoty hello a klikněte na **Další**.
4. V hello poslední dialogové okno, zadejte název projektu a umístění a pak klikněte na tlačítko **Dokončit**.
5. Odstranit hello **MySpec.Scala** souborů **src\test\scala\com\microsoft\spark\example**. Pro aplikace hello to nepotřebujete.
6. V případě potřeby přejmenujte hello výchozí zdroj a testovací soubory. V levém podokně hello v hello IntelliJ IDEA přejděte příliš**src\main\scala\com.microsoft.spark.example**. Klikněte pravým tlačítkem na **App.scala**, klikněte na tlačítko **Refaktorovat**, klikněte na tlačítko přejmenovat soubor a v dialogovém okně hello, zadejte nový název hello hello aplikace a klikněte **Refaktorovat**.
   
    ![Přejmenování souborů](./media/hdinsight-apache-spark-create-standalone-application/rename-scala-files.png)  
7. V následujících krocích hello aktualizujte hello pom.xml toodefine hello závislosti pro hello aplikací Spark Scala. Pro tyto závislosti toobe stáhli a vyřešit automaticky, že je nutné nakonfigurovat Maven odpovídajícím způsobem.
   
    ![Konfigurace Maven pro automatické stahování](./media/hdinsight-apache-spark-create-standalone-application/configure-maven.png)
   
   1. Z hello **soubor** nabídky, klikněte na tlačítko **nastavení**.
   2. V hello **nastavení** dialogové okno pole, přejděte příliš**sestavení, provádění nasazení** > **nástroje sestavení** > **Maven**  >  **Import**.
   3. Vyberte možnost hello příliš**automaticky importovat projekty Maven**.
   4. Klikněte na tlačítko **použít**a potom klikněte na **OK**.
8. Aktualizujte hello Scala zdrojového souboru tooinclude kódu aplikace. Otevřete a nahraďte hello existující ukázkový kód s hello následující kód a uložte změny hello. Tento kód čte hello data z hello HVAC.csv (k dispozici na všech clusterech HDInsight Spark), načte hello řádky, které mají ve sloupci šesté hello pouze jednu číslici a zapíše výstup hello příliš**/HVACOut** pod hello výchozí úložiště kontejner pro hello cluster.
   
        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO toowasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find hello rows which have only one digit in hello 7th column in hello CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
9. Aktualizujte hello pom.xml.
   
   1. V rámci `<project>\<properties>` přidat hello následující:
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. V rámci `<project>\<dependencies>` přidat hello následující:
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      Uložte změny toopom.xml.
10. Vytvoření souboru .jar hello. IntelliJ IDEA umožňuje vytváření JAR jako artefakt projektu. Proveďte následující kroky hello.
    
    1. Z hello **soubor** nabídky, klikněte na tlačítko **strukturu projektu**.
    2. V hello **strukturu projektu** dialogové okno, klikněte na tlačítko **artefakty** a pak klikněte na symbol plus – hello. Z rozbalovací dialogové hello, klikněte na tlačítko **JAR**a potom klikněte na **z modulů se závislostmi**.
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-1.png)
    3. V hello **vytvořit JAR z modulů** dialogové okno pole, klikněte na tlačítko se třemi tečkami hello (![třemi tečkami](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) proti hello **hlavní třídy**.
    4. V hello **vyberte třídu hlavní** dialogové okno, vyberte hello třídu, která se zobrazí ve výchozím nastavení a pak klikněte na tlačítko **OK**.
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-2.png)
    5. V hello **vytvořit JAR z modulů** dialogové okno zkontrolujte, zda tuto možnost hello příliš**extrahovat toohello cíl JAR** je vybrána a potom klikněte na **OK**. Tím se vytvoří jeden JAR s všechny závislosti.
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-3.png)
    6. Karta rozložení výstup Hello uvádí všechny hello JAR, které jsou součástí projekt Maven hello. Můžete vybrat a odstranění hello těch, které jsou na kterém hello Scala aplikace nemá žádné přímé závislostí. Pro aplikace hello tady vytváříme, můžete odebrat všechny ale hello naposledy (**SparkSimpleApp kompilace výstup**). Vyberte hello JAR toodelete a pak klikněte na hello **odstranit** ikonu.
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/delete-output-jars.png)
       
        Zajistěte, aby **sestavení na Ujistěte se,** je políčko zaškrtnuto, což zajistí, že jar hello se vytvoří pokaždé, když je vytvořené nebo aktualizované hello projektu. Klikněte na tlačítko **použít** a potom **OK**.
    7. V řádku nabídek hello, klikněte na **sestavení**a potom klikněte na **zkontrolujte projektu**. Můžete také kliknout na **sestavení artefaktů** toocreate hello jar. Hello jar výstup se vytvořil v rámci **\out\artifacts**.
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/output.png)

## <a name="run-hello-application-on-hello-spark-cluster"></a>Spuštění aplikace hello na clusteru Spark hello
aplikace hello toorun na hello clusteru, je nutné provést následující hello:

* **Kopírování hello aplikace jar toohello objektu blob úložiště Azure** přidruženého k hello clusteru. Můžete použít [ **AzCopy**](../storage/common/storage-use-azcopy.md), příkaz řádku nástroje pro toodo tak. Existuje mnoho dalších klientů i, které můžete použít tooupload data. Můžete najít další informace o nich v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).
* **Vzdáleně pomocí Livy toosubmit úlohu aplikace** toohello clusteru Spark. Clustery Spark v HDInsight zahrnuje Livy, který zveřejňuje REST koncové body tooremotely odeslání úlohy Spark. Další informace najdete v tématu [úlohy odeslání Spark vzdáleně pomocí Spark pomocí Livy clusterů v HDInsight](hdinsight-apache-spark-livy-rest-interface.md).

## <a name="next-step"></a>Další krok

V tomto článku jste se naučili jak toocreate k Spark scala aplikaci. ADVANCE toohello další článek toolearn jak toorun této aplikace na HDInsight Spark clusteru pomocí Livy.

> [!div class="nextstepaction"]
>[Vzdálené spouštění úloh na clusteru Sparku pomocí Livy](hdinsight-apache-spark-livy-rest-interface.md)

