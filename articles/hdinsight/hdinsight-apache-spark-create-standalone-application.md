---
title: "Vytvoření aplikace Scala ke spuštění na clustery Spark - Azure HDInsight | Microsoft Docs"
description: "Vytvoření Spark aplikace napsané v jazyce Scala s Apache Maven jako systém sestavení a existující archetype Maven pro Scala poskytované IntelliJ IDEA."
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
ms.openlocfilehash: 95dba08744357f8800b05e3d4b892e3a363d5985
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-scala-maven-application-to-run-on-apache-spark-cluster-on-hdinsight"></a><span data-ttu-id="6f471-103">Vytvoření aplikace Scala Maven ke spuštění v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="6f471-103">Create a Scala Maven application to run on Apache Spark cluster on HDInsight</span></span>

<span data-ttu-id="6f471-104">Zjistěte, jak vytvořit aplikaci Spark napsané v jazyce Scala pomocí nástroje Maven s IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="6f471-104">Learn how to create a Spark application written in Scala using Maven with IntelliJ IDEA.</span></span> <span data-ttu-id="6f471-105">Článek používá Apache Maven jako systém sestavení a spuštění se existující archetype Maven Scala poskytované IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="6f471-105">The article uses Apache Maven as the build system and starts with an existing Maven archetype for Scala provided by IntelliJ IDEA.</span></span>  <span data-ttu-id="6f471-106">Vytvoření aplikace Scala v IntelliJ IDEA zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6f471-106">Creating a Scala application in IntelliJ IDEA involves the following steps:</span></span>

* <span data-ttu-id="6f471-107">Používání Maven jako systém sestavení.</span><span class="sxs-lookup"><span data-stu-id="6f471-107">Use Maven as the build system.</span></span>
* <span data-ttu-id="6f471-108">Aktualizujte soubor projektu objektu modelu (POM) o vyřešení závislostí modulu Spark.</span><span class="sxs-lookup"><span data-stu-id="6f471-108">Update Project Object Model (POM) file to resolve Spark module dependencies.</span></span>
* <span data-ttu-id="6f471-109">Zápis v jazyce Scala vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f471-109">Write your application in Scala.</span></span>
* <span data-ttu-id="6f471-110">Generovat soubor jar, který lze odeslat do clusterů HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="6f471-110">Generate a jar file that can be submitted to HDInsight Spark clusters.</span></span>
* <span data-ttu-id="6f471-111">Spuštění aplikace na clusteru Spark pomocí Livy.</span><span class="sxs-lookup"><span data-stu-id="6f471-111">Run the application on Spark cluster using Livy.</span></span>

> [!NOTE]
> <span data-ttu-id="6f471-112">HDInsight také poskytuje nástroj k usnadnění procesu vytváření a odesílání aplikací do clusteru HDInsight Spark v Linux na IntelliJ IDEA modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="6f471-112">HDInsight also provides an IntelliJ IDEA plugin tool to ease the process of creating and submitting applications to an HDInsight Spark cluster on Linux.</span></span> <span data-ttu-id="6f471-113">Další informace najdete v tématu [pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="6f471-113">For more information, see [Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark applications](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="6f471-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6f471-114">Prerequisites</span></span>

* <span data-ttu-id="6f471-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="6f471-115">An Azure subscription.</span></span> <span data-ttu-id="6f471-116">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="6f471-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="6f471-117">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6f471-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="6f471-118">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="6f471-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="6f471-119">Java Development kit Oracle.</span><span class="sxs-lookup"><span data-stu-id="6f471-119">Oracle Java Development kit.</span></span> <span data-ttu-id="6f471-120">Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="6f471-120">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="6f471-121">Java IDE.</span><span class="sxs-lookup"><span data-stu-id="6f471-121">A Java IDE.</span></span> <span data-ttu-id="6f471-122">Tento článek používá IntelliJ IDEA 15.0.1.</span><span class="sxs-lookup"><span data-stu-id="6f471-122">This article uses IntelliJ IDEA 15.0.1.</span></span> <span data-ttu-id="6f471-123">Můžete nainstalovat z [zde](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="6f471-123">You can install it from [here](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-scala-plugin-for-intellij-idea"></a><span data-ttu-id="6f471-124">Instalace modulu plug-in Scala pro IntelliJ IDEA</span><span class="sxs-lookup"><span data-stu-id="6f471-124">Install Scala plugin for IntelliJ IDEA</span></span>
<span data-ttu-id="6f471-125">Pokud instalace IntelliJ IDEA není nebyl vyzván k povolení modulu plug-in Scala, spusťte IntelliJ IDEA a projít následující kroky k instalaci modulu plug-in:</span><span class="sxs-lookup"><span data-stu-id="6f471-125">If IntelliJ IDEA installation did not not prompt for enabling Scala plugin, launch IntelliJ IDEA and go through the following steps to install the plugin:</span></span>

1. <span data-ttu-id="6f471-126">Spusťte IntelliJ IDEA a na úvodní obrazovce klikněte na **konfigurace** a pak klikněte na **modulů plug-in**.</span><span class="sxs-lookup"><span data-stu-id="6f471-126">Start IntelliJ IDEA and from welcome screen click **Configure** and then click **Plugins**.</span></span>
   
    ![Povolení modulu plug-in scala](./media/hdinsight-apache-spark-create-standalone-application/enable-scala-plugin.png)
2. <span data-ttu-id="6f471-128">Na další obrazovce klikněte na tlačítko **JetBrains instalace modulu plug-in** z levého dolního rohu.</span><span class="sxs-lookup"><span data-stu-id="6f471-128">In the next screen, click **Install JetBrains plugin** from the lower left corner.</span></span> <span data-ttu-id="6f471-129">V **procházet modulů plug-in JetBrains** dialogové okno otevře, vyhledejte Scala a pak klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="6f471-129">In the **Browse JetBrains Plugins** dialog box that opens, search for Scala and then click **Install**.</span></span>
   
    ![Instalace modulu plug-in scala](./media/hdinsight-apache-spark-create-standalone-application/install-scala-plugin.png)
3. <span data-ttu-id="6f471-131">Poté, co modul plug-in nainstaluje úspěšně, klikněte na tlačítko **restartujte IntelliJ IDEA tlačítko** restartování rozhraní IDE.</span><span class="sxs-lookup"><span data-stu-id="6f471-131">After the plugin installs successfully, click the **Restart IntelliJ IDEA button** to restart the IDE.</span></span>

## <a name="create-a-standalone-scala-project"></a><span data-ttu-id="6f471-132">Vytvoření projektu samostatné Scala</span><span class="sxs-lookup"><span data-stu-id="6f471-132">Create a standalone Scala project</span></span>
1. <span data-ttu-id="6f471-133">Spusťte IntelliJ IDEA a vytvoření nového projektu.</span><span class="sxs-lookup"><span data-stu-id="6f471-133">Launch IntelliJ IDEA and create a new project.</span></span> <span data-ttu-id="6f471-134">V dialogovém okně Nový projekt vyberte následující možnosti a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6f471-134">In the new project dialog box, make the following choices, and then click **Next**.</span></span>
   
    ![Vytvořte projekt Maven](./media/hdinsight-apache-spark-create-standalone-application/create-maven-project.png)
   
   * <span data-ttu-id="6f471-136">Vyberte **Maven** jako typ projektu.</span><span class="sxs-lookup"><span data-stu-id="6f471-136">Select **Maven** as the project type.</span></span>
   * <span data-ttu-id="6f471-137">Zadejte **projektu SDK**.</span><span class="sxs-lookup"><span data-stu-id="6f471-137">Specify a **Project SDK**.</span></span> <span data-ttu-id="6f471-138">Kliknutím na tlačítko Nová a přejděte do instalačního adresáře nástroje Java, obvykle `C:\Program Files\Java\jdk1.8.0_66`.</span><span class="sxs-lookup"><span data-stu-id="6f471-138">Click New and navigate to the Java installation directory, typically `C:\Program Files\Java\jdk1.8.0_66`.</span></span>
   * <span data-ttu-id="6f471-139">Vyberte **vytvořit z archetype** možnost.</span><span class="sxs-lookup"><span data-stu-id="6f471-139">Select the **Create from archetype** option.</span></span>
   * <span data-ttu-id="6f471-140">V seznamu archetypes vyberte **org.scala tools.archetypes:scala archetype jednoduchý**.</span><span class="sxs-lookup"><span data-stu-id="6f471-140">From the list of archetypes, select **org.scala-tools.archetypes:scala-archetype-simple**.</span></span> <span data-ttu-id="6f471-141">Tato akce vytvoří správné adresářovou strukturu a stáhnout požadované výchozí závislosti zápis Scala program.</span><span class="sxs-lookup"><span data-stu-id="6f471-141">This will create the right directory structure and download the required default dependencies to write Scala program.</span></span>
2. <span data-ttu-id="6f471-142">Zadejte příslušné hodnoty pro **GroupId**, **ArtifactId**, a **verze**.</span><span class="sxs-lookup"><span data-stu-id="6f471-142">Provide relevant values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="6f471-143">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="6f471-143">Click **Next**.</span></span>
3. <span data-ttu-id="6f471-144">V další dialogové okno, kde můžete zadat domovský adresář Maven a další nastavení uživatele, přijměte výchozí hodnoty a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="6f471-144">In the next dialog box, where you specify Maven home directory and other user settings, accept the defaults and click **Next**.</span></span>
4. <span data-ttu-id="6f471-145">V dialogovém okně poslední zadejte název projektu a umístění a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="6f471-145">In the last dialog box, specify a project name and location and then click **Finish**.</span></span>
5. <span data-ttu-id="6f471-146">Odstranit **MySpec.Scala** souborů **src\test\scala\com\microsoft\spark\example**.</span><span class="sxs-lookup"><span data-stu-id="6f471-146">Delete the **MySpec.Scala** file at **src\test\scala\com\microsoft\spark\example**.</span></span> <span data-ttu-id="6f471-147">Není to nutné pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6f471-147">You do not need this for the application.</span></span>
6. <span data-ttu-id="6f471-148">V případě potřeby přejmenujte výchozí zdroj a testovací soubory.</span><span class="sxs-lookup"><span data-stu-id="6f471-148">If required, rename the default source and test files.</span></span> <span data-ttu-id="6f471-149">V levém podokně IntelliJ IDEA, přejděte na **src\main\scala\com.microsoft.spark.example**.</span><span class="sxs-lookup"><span data-stu-id="6f471-149">From the left pane in the IntelliJ IDEA, navigate to **src\main\scala\com.microsoft.spark.example**.</span></span> <span data-ttu-id="6f471-150">Klikněte pravým tlačítkem na **App.scala**, klikněte na tlačítko **Refaktorovat**klikněte přejmenovat soubor a v dialogovém okně zadejte nový název pro aplikaci a pak na **Refaktorovat**.</span><span class="sxs-lookup"><span data-stu-id="6f471-150">Right-click **App.scala**, click **Refactor**, click Rename file, and in the dialog box, provide the new name for the application and then click **Refactor**.</span></span>
   
    ![Přejmenování souborů](./media/hdinsight-apache-spark-create-standalone-application/rename-scala-files.png)  
7. <span data-ttu-id="6f471-152">V dalších krocích aktualizujte pom.xml k definování závislostí u aplikací Spark Scala.</span><span class="sxs-lookup"><span data-stu-id="6f471-152">In the subsequent steps, you will update the pom.xml to define the dependencies for the Spark Scala application.</span></span> <span data-ttu-id="6f471-153">Tyto závislosti se stáhnou a automaticky vyřešen musíte nakonfigurovat Maven odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="6f471-153">For those dependencies to be downloaded and resolved automatically, you must configure Maven accordingly.</span></span>
   
    ![Konfigurace Maven pro automatické stahování](./media/hdinsight-apache-spark-create-standalone-application/configure-maven.png)
   
   1. <span data-ttu-id="6f471-155">Z **soubor** nabídky, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="6f471-155">From the **File** menu, click **Settings**.</span></span>
   2. <span data-ttu-id="6f471-156">V **nastavení** dialogové okno pole, přejděte na **sestavení, provádění nasazení** > **nástroje sestavení** > **Maven**  >  **Import**.</span><span class="sxs-lookup"><span data-stu-id="6f471-156">In the **Settings** dialog box, navigate to **Build, Execution, Deployment** > **Build Tools** > **Maven** > **Importing**.</span></span>
   3. <span data-ttu-id="6f471-157">Vyberte možnost **automaticky importovat projekty Maven**.</span><span class="sxs-lookup"><span data-stu-id="6f471-157">Select the option to **Import Maven projects automatically**.</span></span>
   4. <span data-ttu-id="6f471-158">Klikněte na tlačítko **použít**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f471-158">Click **Apply**, and then click **OK**.</span></span>
8. <span data-ttu-id="6f471-159">Aktualizujte Scala zdrojový soubor, který patří kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="6f471-159">Update the Scala source file to include your application code.</span></span> <span data-ttu-id="6f471-160">Otevřete existující ukázkového kódu nahraďte následujícím kódem a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="6f471-160">Open and replace the existing sample code with the following code and save the changes.</span></span> <span data-ttu-id="6f471-161">Tento kód čte data z HVAC.csv (k dispozici na všech clusterech HDInsight Spark), načte řádky, které mají ve sloupci šesté pouze jednu číslici a zapíše výstup do **/HVACOut** pod výchozí kontejner úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="6f471-161">This code reads the data from the HVAC.csv (available on all HDInsight Spark clusters), retrieves the rows that only have one digit in the sixth column, and writes the output to **/HVACOut** under the default storage container for the cluster.</span></span>
   
        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO to wasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows which have only one digit in the 7th column in the CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
9. <span data-ttu-id="6f471-162">Aktualizace souboru pom.xml.</span><span class="sxs-lookup"><span data-stu-id="6f471-162">Update the pom.xml.</span></span>
   
   1. <span data-ttu-id="6f471-163">V rámci `<project>\<properties>` přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="6f471-163">Within `<project>\<properties>` add the following:</span></span>
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. <span data-ttu-id="6f471-164">V rámci `<project>\<dependencies>` přidejte následující:</span><span class="sxs-lookup"><span data-stu-id="6f471-164">Within `<project>\<dependencies>` add the following:</span></span>
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      <span data-ttu-id="6f471-165">Uložte změny do pom.xml.</span><span class="sxs-lookup"><span data-stu-id="6f471-165">Save changes to pom.xml.</span></span>
10. <span data-ttu-id="6f471-166">Vytvoření souboru .jar.</span><span class="sxs-lookup"><span data-stu-id="6f471-166">Create the .jar file.</span></span> <span data-ttu-id="6f471-167">IntelliJ IDEA umožňuje vytváření JAR jako artefakt projektu.</span><span class="sxs-lookup"><span data-stu-id="6f471-167">IntelliJ IDEA enables creation of JAR as an artifact of a project.</span></span> <span data-ttu-id="6f471-168">Proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="6f471-168">Perform the following steps.</span></span>
    
    1. <span data-ttu-id="6f471-169">Z **soubor** nabídky, klikněte na tlačítko **strukturu projektu**.</span><span class="sxs-lookup"><span data-stu-id="6f471-169">From the **File** menu, click **Project Structure**.</span></span>
    2. <span data-ttu-id="6f471-170">V **strukturu projektu** dialogové okno, klikněte na tlačítko **artefakty** a pak klikněte na plus symbol.</span><span class="sxs-lookup"><span data-stu-id="6f471-170">In the **Project Structure** dialog box, click **Artifacts** and then click the plus symbol.</span></span> <span data-ttu-id="6f471-171">V dialogovém okně automaticky otevírané okno klikněte na **JAR**a potom klikněte na **z modulů se závislostmi**.</span><span class="sxs-lookup"><span data-stu-id="6f471-171">From the pop-up dialog box, click **JAR**, and then click **From modules with dependencies**.</span></span>
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-1.png)
    3. <span data-ttu-id="6f471-173">V **vytvořit JAR z modulů** dialogové okno pole, klikněte na tlačítko se třemi tečkami (![třemi tečkami](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) proti **hlavní třídy**.</span><span class="sxs-lookup"><span data-stu-id="6f471-173">In the **Create JAR from Modules** dialog box, click the ellipsis (![ellipsis](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) against the **Main Class**.</span></span>
    4. <span data-ttu-id="6f471-174">V **vyberte třídu hlavní** dialogovém okně vyberte třídu, která se zobrazí ve výchozím nastavení a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f471-174">In the **Select Main Class** dialog box, select the class that appears by default and then click **OK**.</span></span>
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-2.png)
    5. <span data-ttu-id="6f471-176">V **vytvořit JAR z modulů** dialogové okno pole, ujistěte se, že možnost **extrahovat k cíli JAR** je vybrána a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f471-176">In the **Create JAR from Modules** dialog box, make sure that the option to **extract to the target JAR** is selected, and then click **OK**.</span></span> <span data-ttu-id="6f471-177">Tím se vytvoří jeden JAR s všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="6f471-177">This creates a single JAR with all dependencies.</span></span>
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-3.png)
    6. <span data-ttu-id="6f471-179">Karta Výstup rozložení uvádí všechny JAR, které jsou součástí na projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="6f471-179">The output layout tab lists all the jars that are included as part of the Maven project.</span></span> <span data-ttu-id="6f471-180">Můžete vybrat a odstranit ty, na kterém aplikace Scala má žádné přímé závislost.</span><span class="sxs-lookup"><span data-stu-id="6f471-180">You can select and delete the ones on which the Scala application has no direct dependency.</span></span> <span data-ttu-id="6f471-181">Pro aplikaci tady vytváříme, můžete odebrat všechny uzly s výjimkou poslední (**SparkSimpleApp kompilace výstup**).</span><span class="sxs-lookup"><span data-stu-id="6f471-181">For the application we are creating here, you can remove all but the last one (**SparkSimpleApp compile output**).</span></span> <span data-ttu-id="6f471-182">Vyberte JAR odstranit a potom klikněte na **odstranit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6f471-182">Select the jars to delete and then click the **Delete** icon.</span></span>
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/delete-output-jars.png)
       
        <span data-ttu-id="6f471-184">Zajistěte, aby **sestavení na Ujistěte se,** je políčko zaškrtnuto, který zajistí, aby jar vytvořit pokaždé, když je vytvořené nebo aktualizované projektu.</span><span class="sxs-lookup"><span data-stu-id="6f471-184">Make sure **Build on make** box is selected, which ensures that the jar is created every time the project is built or updated.</span></span> <span data-ttu-id="6f471-185">Klikněte na tlačítko **použít** a potom **OK**.</span><span class="sxs-lookup"><span data-stu-id="6f471-185">Click **Apply** and then **OK**.</span></span>
    7. <span data-ttu-id="6f471-186">V řádku nabídek klikněte na **sestavení**a potom klikněte na **zkontrolujte projektu**.</span><span class="sxs-lookup"><span data-stu-id="6f471-186">From the menu bar, click **Build**, and then click **Make Project**.</span></span> <span data-ttu-id="6f471-187">Můžete také kliknout na **sestavení artefaktů** vytvořit jar.</span><span class="sxs-lookup"><span data-stu-id="6f471-187">You can also click **Build Artifacts** to create the jar.</span></span> <span data-ttu-id="6f471-188">Výstup jar se vytvořil v rámci **\out\artifacts**.</span><span class="sxs-lookup"><span data-stu-id="6f471-188">The output jar is created under **\out\artifacts**.</span></span>
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/output.png)

## <a name="run-the-application-on-the-spark-cluster"></a><span data-ttu-id="6f471-190">Spuštění aplikace na clusteru Spark</span><span class="sxs-lookup"><span data-stu-id="6f471-190">Run the application on the Spark cluster</span></span>
<span data-ttu-id="6f471-191">Ke spuštění aplikace v clusteru, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="6f471-191">To run the application on the cluster, you must do the following:</span></span>

* <span data-ttu-id="6f471-192">**Zkopírujte jar aplikace do objektu blob úložiště Azure** přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="6f471-192">**Copy the application jar to the Azure storage blob** associated with the cluster.</span></span> <span data-ttu-id="6f471-193">Můžete použít [ **AzCopy**](../storage/common/storage-use-azcopy.md), nástroj příkazového řádku, tak.</span><span class="sxs-lookup"><span data-stu-id="6f471-193">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command line utility, to do so.</span></span> <span data-ttu-id="6f471-194">Existuje mnoho také ostatní klienty, který můžete použít k nahrání dat.</span><span class="sxs-lookup"><span data-stu-id="6f471-194">There are a lot of other clients as well that you can use to upload data.</span></span> <span data-ttu-id="6f471-195">Můžete najít další informace o nich v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="6f471-195">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>
* <span data-ttu-id="6f471-196">**Odeslat úlohu aplikaci vzdáleně pomocí Livy** do clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="6f471-196">**Use Livy to submit an application job remotely** to the Spark cluster.</span></span> <span data-ttu-id="6f471-197">Clustery Spark v HDInsight zahrnuje Livy, který zveřejňuje koncové body REST pro vzdálené odesílání úloh Spark.</span><span class="sxs-lookup"><span data-stu-id="6f471-197">Spark clusters on HDInsight includes Livy that exposes REST endpoints to remotely submit Spark jobs.</span></span> <span data-ttu-id="6f471-198">Další informace najdete v tématu [úlohy odeslání Spark vzdáleně pomocí Spark pomocí Livy clusterů v HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="6f471-198">For more information, see [Submit Spark jobs remotely using Livy with Spark clusters on HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="6f471-199">Další krok</span><span class="sxs-lookup"><span data-stu-id="6f471-199">Next step</span></span>

<span data-ttu-id="6f471-200">V tomto článku jste zjistili, jak vytvořit aplikaci Spark scala.</span><span class="sxs-lookup"><span data-stu-id="6f471-200">In this article you learned how to create a Spark scala application.</span></span> <span data-ttu-id="6f471-201">Přechodu na další článku se dozvíte, jak ke spuštění této aplikace v clusteru HDInsight Spark pomocí Livy.</span><span class="sxs-lookup"><span data-stu-id="6f471-201">Advance to the next article to learn how to run this application on an HDInsight Spark cluster using Livy.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="6f471-202">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="6f471-202">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

