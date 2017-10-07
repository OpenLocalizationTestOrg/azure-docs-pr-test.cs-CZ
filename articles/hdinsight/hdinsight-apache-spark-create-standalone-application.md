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
# <a name="create-a-scala-maven-application-toorun-on-apache-spark-cluster-on-hdinsight"></a><span data-ttu-id="76f89-103">Vytvoření Scala Maven toorun aplikace na cluster Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="76f89-103">Create a Scala Maven application toorun on Apache Spark cluster on HDInsight</span></span>

<span data-ttu-id="76f89-104">Zjistěte, jak toocreate Spark aplikace napsané v jazyce Scala pomocí nástroje Maven s IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="76f89-104">Learn how toocreate a Spark application written in Scala using Maven with IntelliJ IDEA.</span></span> <span data-ttu-id="76f89-105">článek Hello používá Apache Maven hello sestavení systému a začíná existující archetype Maven pro Scala poskytované IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="76f89-105">hello article uses Apache Maven as hello build system and starts with an existing Maven archetype for Scala provided by IntelliJ IDEA.</span></span>  <span data-ttu-id="76f89-106">Vytvoření aplikace Scala v IntelliJ IDEA zahrnuje hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="76f89-106">Creating a Scala application in IntelliJ IDEA involves hello following steps:</span></span>

* <span data-ttu-id="76f89-107">Použijte Maven jako systém sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="76f89-107">Use Maven as hello build system.</span></span>
* <span data-ttu-id="76f89-108">Aktualizujte projektu objektu modelu (POM) souboru tooresolve Spark modulu závislosti.</span><span class="sxs-lookup"><span data-stu-id="76f89-108">Update Project Object Model (POM) file tooresolve Spark module dependencies.</span></span>
* <span data-ttu-id="76f89-109">Zápis v jazyce Scala vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="76f89-109">Write your application in Scala.</span></span>
* <span data-ttu-id="76f89-110">Generovat soubor jar, který může být odeslaná tooHDInsight clustery Spark.</span><span class="sxs-lookup"><span data-stu-id="76f89-110">Generate a jar file that can be submitted tooHDInsight Spark clusters.</span></span>
* <span data-ttu-id="76f89-111">Spusťte aplikaci hello na clusteru Spark pomocí Livy.</span><span class="sxs-lookup"><span data-stu-id="76f89-111">Run hello application on Spark cluster using Livy.</span></span>

> [!NOTE]
> <span data-ttu-id="76f89-112">HDInsight také poskytuje IntelliJ IDEA modulu plug-in nástroje tooease hello procesu vytváření a odesílání aplikací tooan clusteru HDInsight Spark na systému Linux.</span><span class="sxs-lookup"><span data-stu-id="76f89-112">HDInsight also provides an IntelliJ IDEA plugin tool tooease hello process of creating and submitting applications tooan HDInsight Spark cluster on Linux.</span></span> <span data-ttu-id="76f89-113">Další informace najdete v tématu [pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="76f89-113">For more information, see [Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark applications](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="76f89-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="76f89-114">Prerequisites</span></span>

* <span data-ttu-id="76f89-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="76f89-115">An Azure subscription.</span></span> <span data-ttu-id="76f89-116">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="76f89-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="76f89-117">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="76f89-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="76f89-118">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="76f89-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="76f89-119">Java Development kit Oracle.</span><span class="sxs-lookup"><span data-stu-id="76f89-119">Oracle Java Development kit.</span></span> <span data-ttu-id="76f89-120">Můžete nainstalovat z [zde](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="76f89-120">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="76f89-121">Java IDE.</span><span class="sxs-lookup"><span data-stu-id="76f89-121">A Java IDE.</span></span> <span data-ttu-id="76f89-122">Tento článek používá IntelliJ IDEA 15.0.1.</span><span class="sxs-lookup"><span data-stu-id="76f89-122">This article uses IntelliJ IDEA 15.0.1.</span></span> <span data-ttu-id="76f89-123">Můžete nainstalovat z [zde](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="76f89-123">You can install it from [here](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-scala-plugin-for-intellij-idea"></a><span data-ttu-id="76f89-124">Instalace modulu plug-in Scala pro IntelliJ IDEA</span><span class="sxs-lookup"><span data-stu-id="76f89-124">Install Scala plugin for IntelliJ IDEA</span></span>
<span data-ttu-id="76f89-125">Pokud instalace IntelliJ IDEA není nebyl vyzván k povolení modulu plug-in Scala, spusťte IntelliJ IDEA a projít hello následující modul plug-in hello tooinstall kroky:</span><span class="sxs-lookup"><span data-stu-id="76f89-125">If IntelliJ IDEA installation did not not prompt for enabling Scala plugin, launch IntelliJ IDEA and go through hello following steps tooinstall hello plugin:</span></span>

1. <span data-ttu-id="76f89-126">Spusťte IntelliJ IDEA a na úvodní obrazovce klikněte na **konfigurace** a pak klikněte na **modulů plug-in**.</span><span class="sxs-lookup"><span data-stu-id="76f89-126">Start IntelliJ IDEA and from welcome screen click **Configure** and then click **Plugins**.</span></span>
   
    ![Povolení modulu plug-in scala](./media/hdinsight-apache-spark-create-standalone-application/enable-scala-plugin.png)
2. <span data-ttu-id="76f89-128">Hello na další obrazovce klikněte na tlačítko **JetBrains instalace modulu plug-in** z levého dolního rohu hello.</span><span class="sxs-lookup"><span data-stu-id="76f89-128">In hello next screen, click **Install JetBrains plugin** from hello lower left corner.</span></span> <span data-ttu-id="76f89-129">V hello **procházet modulů plug-in JetBrains** dialogové okno otevře, vyhledejte Scala a pak klikněte na **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="76f89-129">In hello **Browse JetBrains Plugins** dialog box that opens, search for Scala and then click **Install**.</span></span>
   
    ![Instalace modulu plug-in scala](./media/hdinsight-apache-spark-create-standalone-application/install-scala-plugin.png)
3. <span data-ttu-id="76f89-131">Po hello modul plug-in nainstaluje úspěšně, klikněte na tlačítko hello **tlačítko Restartovat IntelliJ IDEA** toorestart hello IDE.</span><span class="sxs-lookup"><span data-stu-id="76f89-131">After hello plugin installs successfully, click hello **Restart IntelliJ IDEA button** toorestart hello IDE.</span></span>

## <a name="create-a-standalone-scala-project"></a><span data-ttu-id="76f89-132">Vytvoření projektu samostatné Scala</span><span class="sxs-lookup"><span data-stu-id="76f89-132">Create a standalone Scala project</span></span>
1. <span data-ttu-id="76f89-133">Spusťte IntelliJ IDEA a vytvoření nového projektu.</span><span class="sxs-lookup"><span data-stu-id="76f89-133">Launch IntelliJ IDEA and create a new project.</span></span> <span data-ttu-id="76f89-134">V hello projektu dialogové okno Nový, zkontrolujte hello následující možnosti a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="76f89-134">In hello new project dialog box, make hello following choices, and then click **Next**.</span></span>
   
    ![Vytvořte projekt Maven](./media/hdinsight-apache-spark-create-standalone-application/create-maven-project.png)
   
   * <span data-ttu-id="76f89-136">Vyberte **Maven** jako typ projektu hello.</span><span class="sxs-lookup"><span data-stu-id="76f89-136">Select **Maven** as hello project type.</span></span>
   * <span data-ttu-id="76f89-137">Zadejte **projektu SDK**.</span><span class="sxs-lookup"><span data-stu-id="76f89-137">Specify a **Project SDK**.</span></span> <span data-ttu-id="76f89-138">Kliknutím na tlačítko Nová a přejděte toohello Java instalační adresář, obvykle `C:\Program Files\Java\jdk1.8.0_66`.</span><span class="sxs-lookup"><span data-stu-id="76f89-138">Click New and navigate toohello Java installation directory, typically `C:\Program Files\Java\jdk1.8.0_66`.</span></span>
   * <span data-ttu-id="76f89-139">Vyberte hello **vytvořit z archetype** možnost.</span><span class="sxs-lookup"><span data-stu-id="76f89-139">Select hello **Create from archetype** option.</span></span>
   * <span data-ttu-id="76f89-140">Vyberte ze seznamu hello archetypes, **org.scala tools.archetypes:scala archetype jednoduchý**.</span><span class="sxs-lookup"><span data-stu-id="76f89-140">From hello list of archetypes, select **org.scala-tools.archetypes:scala-archetype-simple**.</span></span> <span data-ttu-id="76f89-141">Tato akce vytvoří hello správné adresářovou strukturu a stáhnout požadované hello výchozí závislosti toowrite Scala program.</span><span class="sxs-lookup"><span data-stu-id="76f89-141">This will create hello right directory structure and download hello required default dependencies toowrite Scala program.</span></span>
2. <span data-ttu-id="76f89-142">Zadejte příslušné hodnoty pro **GroupId**, **ArtifactId**, a **verze**.</span><span class="sxs-lookup"><span data-stu-id="76f89-142">Provide relevant values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="76f89-143">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="76f89-143">Click **Next**.</span></span>
3. <span data-ttu-id="76f89-144">V hello další dialogové okno, kde zadáte domovský adresář Maven a další nastavení uživatele, přijměte výchozí hodnoty hello a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="76f89-144">In hello next dialog box, where you specify Maven home directory and other user settings, accept hello defaults and click **Next**.</span></span>
4. <span data-ttu-id="76f89-145">V hello poslední dialogové okno, zadejte název projektu a umístění a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="76f89-145">In hello last dialog box, specify a project name and location and then click **Finish**.</span></span>
5. <span data-ttu-id="76f89-146">Odstranit hello **MySpec.Scala** souborů **src\test\scala\com\microsoft\spark\example**.</span><span class="sxs-lookup"><span data-stu-id="76f89-146">Delete hello **MySpec.Scala** file at **src\test\scala\com\microsoft\spark\example**.</span></span> <span data-ttu-id="76f89-147">Pro aplikace hello to nepotřebujete.</span><span class="sxs-lookup"><span data-stu-id="76f89-147">You do not need this for hello application.</span></span>
6. <span data-ttu-id="76f89-148">V případě potřeby přejmenujte hello výchozí zdroj a testovací soubory.</span><span class="sxs-lookup"><span data-stu-id="76f89-148">If required, rename hello default source and test files.</span></span> <span data-ttu-id="76f89-149">V levém podokně hello v hello IntelliJ IDEA přejděte příliš**src\main\scala\com.microsoft.spark.example**.</span><span class="sxs-lookup"><span data-stu-id="76f89-149">From hello left pane in hello IntelliJ IDEA, navigate too**src\main\scala\com.microsoft.spark.example**.</span></span> <span data-ttu-id="76f89-150">Klikněte pravým tlačítkem na **App.scala**, klikněte na tlačítko **Refaktorovat**, klikněte na tlačítko přejmenovat soubor a v dialogovém okně hello, zadejte nový název hello hello aplikace a klikněte **Refaktorovat**.</span><span class="sxs-lookup"><span data-stu-id="76f89-150">Right-click **App.scala**, click **Refactor**, click Rename file, and in hello dialog box, provide hello new name for hello application and then click **Refactor**.</span></span>
   
    ![Přejmenování souborů](./media/hdinsight-apache-spark-create-standalone-application/rename-scala-files.png)  
7. <span data-ttu-id="76f89-152">V následujících krocích hello aktualizujte hello pom.xml toodefine hello závislosti pro hello aplikací Spark Scala.</span><span class="sxs-lookup"><span data-stu-id="76f89-152">In hello subsequent steps, you will update hello pom.xml toodefine hello dependencies for hello Spark Scala application.</span></span> <span data-ttu-id="76f89-153">Pro tyto závislosti toobe stáhli a vyřešit automaticky, že je nutné nakonfigurovat Maven odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="76f89-153">For those dependencies toobe downloaded and resolved automatically, you must configure Maven accordingly.</span></span>
   
    ![Konfigurace Maven pro automatické stahování](./media/hdinsight-apache-spark-create-standalone-application/configure-maven.png)
   
   1. <span data-ttu-id="76f89-155">Z hello **soubor** nabídky, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="76f89-155">From hello **File** menu, click **Settings**.</span></span>
   2. <span data-ttu-id="76f89-156">V hello **nastavení** dialogové okno pole, přejděte příliš**sestavení, provádění nasazení** > **nástroje sestavení** > **Maven**  >  **Import**.</span><span class="sxs-lookup"><span data-stu-id="76f89-156">In hello **Settings** dialog box, navigate too**Build, Execution, Deployment** > **Build Tools** > **Maven** > **Importing**.</span></span>
   3. <span data-ttu-id="76f89-157">Vyberte možnost hello příliš**automaticky importovat projekty Maven**.</span><span class="sxs-lookup"><span data-stu-id="76f89-157">Select hello option too**Import Maven projects automatically**.</span></span>
   4. <span data-ttu-id="76f89-158">Klikněte na tlačítko **použít**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="76f89-158">Click **Apply**, and then click **OK**.</span></span>
8. <span data-ttu-id="76f89-159">Aktualizujte hello Scala zdrojového souboru tooinclude kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="76f89-159">Update hello Scala source file tooinclude your application code.</span></span> <span data-ttu-id="76f89-160">Otevřete a nahraďte hello existující ukázkový kód s hello následující kód a uložte změny hello.</span><span class="sxs-lookup"><span data-stu-id="76f89-160">Open and replace hello existing sample code with hello following code and save hello changes.</span></span> <span data-ttu-id="76f89-161">Tento kód čte hello data z hello HVAC.csv (k dispozici na všech clusterech HDInsight Spark), načte hello řádky, které mají ve sloupci šesté hello pouze jednu číslici a zapíše výstup hello příliš**/HVACOut** pod hello výchozí úložiště kontejner pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="76f89-161">This code reads hello data from hello HVAC.csv (available on all HDInsight Spark clusters), retrieves hello rows that only have one digit in hello sixth column, and writes hello output too**/HVACOut** under hello default storage container for hello cluster.</span></span>
   
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
9. <span data-ttu-id="76f89-162">Aktualizujte hello pom.xml.</span><span class="sxs-lookup"><span data-stu-id="76f89-162">Update hello pom.xml.</span></span>
   
   1. <span data-ttu-id="76f89-163">V rámci `<project>\<properties>` přidat hello následující:</span><span class="sxs-lookup"><span data-stu-id="76f89-163">Within `<project>\<properties>` add hello following:</span></span>
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. <span data-ttu-id="76f89-164">V rámci `<project>\<dependencies>` přidat hello následující:</span><span class="sxs-lookup"><span data-stu-id="76f89-164">Within `<project>\<dependencies>` add hello following:</span></span>
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      <span data-ttu-id="76f89-165">Uložte změny toopom.xml.</span><span class="sxs-lookup"><span data-stu-id="76f89-165">Save changes toopom.xml.</span></span>
10. <span data-ttu-id="76f89-166">Vytvoření souboru .jar hello.</span><span class="sxs-lookup"><span data-stu-id="76f89-166">Create hello .jar file.</span></span> <span data-ttu-id="76f89-167">IntelliJ IDEA umožňuje vytváření JAR jako artefakt projektu.</span><span class="sxs-lookup"><span data-stu-id="76f89-167">IntelliJ IDEA enables creation of JAR as an artifact of a project.</span></span> <span data-ttu-id="76f89-168">Proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="76f89-168">Perform hello following steps.</span></span>
    
    1. <span data-ttu-id="76f89-169">Z hello **soubor** nabídky, klikněte na tlačítko **strukturu projektu**.</span><span class="sxs-lookup"><span data-stu-id="76f89-169">From hello **File** menu, click **Project Structure**.</span></span>
    2. <span data-ttu-id="76f89-170">V hello **strukturu projektu** dialogové okno, klikněte na tlačítko **artefakty** a pak klikněte na symbol plus – hello.</span><span class="sxs-lookup"><span data-stu-id="76f89-170">In hello **Project Structure** dialog box, click **Artifacts** and then click hello plus symbol.</span></span> <span data-ttu-id="76f89-171">Z rozbalovací dialogové hello, klikněte na tlačítko **JAR**a potom klikněte na **z modulů se závislostmi**.</span><span class="sxs-lookup"><span data-stu-id="76f89-171">From hello pop-up dialog box, click **JAR**, and then click **From modules with dependencies**.</span></span>
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-1.png)
    3. <span data-ttu-id="76f89-173">V hello **vytvořit JAR z modulů** dialogové okno pole, klikněte na tlačítko se třemi tečkami hello (![třemi tečkami](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) proti hello **hlavní třídy**.</span><span class="sxs-lookup"><span data-stu-id="76f89-173">In hello **Create JAR from Modules** dialog box, click hello ellipsis (![ellipsis](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) against hello **Main Class**.</span></span>
    4. <span data-ttu-id="76f89-174">V hello **vyberte třídu hlavní** dialogové okno, vyberte hello třídu, která se zobrazí ve výchozím nastavení a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="76f89-174">In hello **Select Main Class** dialog box, select hello class that appears by default and then click **OK**.</span></span>
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-2.png)
    5. <span data-ttu-id="76f89-176">V hello **vytvořit JAR z modulů** dialogové okno zkontrolujte, zda tuto možnost hello příliš**extrahovat toohello cíl JAR** je vybrána a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="76f89-176">In hello **Create JAR from Modules** dialog box, make sure that hello option too**extract toohello target JAR** is selected, and then click **OK**.</span></span> <span data-ttu-id="76f89-177">Tím se vytvoří jeden JAR s všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="76f89-177">This creates a single JAR with all dependencies.</span></span>
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-3.png)
    6. <span data-ttu-id="76f89-179">Karta rozložení výstup Hello uvádí všechny hello JAR, které jsou součástí projekt Maven hello.</span><span class="sxs-lookup"><span data-stu-id="76f89-179">hello output layout tab lists all hello jars that are included as part of hello Maven project.</span></span> <span data-ttu-id="76f89-180">Můžete vybrat a odstranění hello těch, které jsou na kterém hello Scala aplikace nemá žádné přímé závislostí.</span><span class="sxs-lookup"><span data-stu-id="76f89-180">You can select and delete hello ones on which hello Scala application has no direct dependency.</span></span> <span data-ttu-id="76f89-181">Pro aplikace hello tady vytváříme, můžete odebrat všechny ale hello naposledy (**SparkSimpleApp kompilace výstup**).</span><span class="sxs-lookup"><span data-stu-id="76f89-181">For hello application we are creating here, you can remove all but hello last one (**SparkSimpleApp compile output**).</span></span> <span data-ttu-id="76f89-182">Vyberte hello JAR toodelete a pak klikněte na hello **odstranit** ikonu.</span><span class="sxs-lookup"><span data-stu-id="76f89-182">Select hello jars toodelete and then click hello **Delete** icon.</span></span>
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/delete-output-jars.png)
       
        <span data-ttu-id="76f89-184">Zajistěte, aby **sestavení na Ujistěte se,** je políčko zaškrtnuto, což zajistí, že jar hello se vytvoří pokaždé, když je vytvořené nebo aktualizované hello projektu.</span><span class="sxs-lookup"><span data-stu-id="76f89-184">Make sure **Build on make** box is selected, which ensures that hello jar is created every time hello project is built or updated.</span></span> <span data-ttu-id="76f89-185">Klikněte na tlačítko **použít** a potom **OK**.</span><span class="sxs-lookup"><span data-stu-id="76f89-185">Click **Apply** and then **OK**.</span></span>
    7. <span data-ttu-id="76f89-186">V řádku nabídek hello, klikněte na **sestavení**a potom klikněte na **zkontrolujte projektu**.</span><span class="sxs-lookup"><span data-stu-id="76f89-186">From hello menu bar, click **Build**, and then click **Make Project**.</span></span> <span data-ttu-id="76f89-187">Můžete také kliknout na **sestavení artefaktů** toocreate hello jar.</span><span class="sxs-lookup"><span data-stu-id="76f89-187">You can also click **Build Artifacts** toocreate hello jar.</span></span> <span data-ttu-id="76f89-188">Hello jar výstup se vytvořil v rámci **\out\artifacts**.</span><span class="sxs-lookup"><span data-stu-id="76f89-188">hello output jar is created under **\out\artifacts**.</span></span>
       
        ![Vytvoření JAR](./media/hdinsight-apache-spark-create-standalone-application/output.png)

## <a name="run-hello-application-on-hello-spark-cluster"></a><span data-ttu-id="76f89-190">Spuštění aplikace hello na clusteru Spark hello</span><span class="sxs-lookup"><span data-stu-id="76f89-190">Run hello application on hello Spark cluster</span></span>
<span data-ttu-id="76f89-191">aplikace hello toorun na hello clusteru, je nutné provést následující hello:</span><span class="sxs-lookup"><span data-stu-id="76f89-191">toorun hello application on hello cluster, you must do hello following:</span></span>

* <span data-ttu-id="76f89-192">**Kopírování hello aplikace jar toohello objektu blob úložiště Azure** přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="76f89-192">**Copy hello application jar toohello Azure storage blob** associated with hello cluster.</span></span> <span data-ttu-id="76f89-193">Můžete použít [ **AzCopy**](../storage/common/storage-use-azcopy.md), příkaz řádku nástroje pro toodo tak.</span><span class="sxs-lookup"><span data-stu-id="76f89-193">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command line utility, toodo so.</span></span> <span data-ttu-id="76f89-194">Existuje mnoho dalších klientů i, které můžete použít tooupload data.</span><span class="sxs-lookup"><span data-stu-id="76f89-194">There are a lot of other clients as well that you can use tooupload data.</span></span> <span data-ttu-id="76f89-195">Můžete najít další informace o nich v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="76f89-195">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>
* <span data-ttu-id="76f89-196">**Vzdáleně pomocí Livy toosubmit úlohu aplikace** toohello clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="76f89-196">**Use Livy toosubmit an application job remotely** toohello Spark cluster.</span></span> <span data-ttu-id="76f89-197">Clustery Spark v HDInsight zahrnuje Livy, který zveřejňuje REST koncové body tooremotely odeslání úlohy Spark.</span><span class="sxs-lookup"><span data-stu-id="76f89-197">Spark clusters on HDInsight includes Livy that exposes REST endpoints tooremotely submit Spark jobs.</span></span> <span data-ttu-id="76f89-198">Další informace najdete v tématu [úlohy odeslání Spark vzdáleně pomocí Spark pomocí Livy clusterů v HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="76f89-198">For more information, see [Submit Spark jobs remotely using Livy with Spark clusters on HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="76f89-199">Další krok</span><span class="sxs-lookup"><span data-stu-id="76f89-199">Next step</span></span>

<span data-ttu-id="76f89-200">V tomto článku jste se naučili jak toocreate k Spark scala aplikaci.</span><span class="sxs-lookup"><span data-stu-id="76f89-200">In this article you learned how toocreate a Spark scala application.</span></span> <span data-ttu-id="76f89-201">ADVANCE toohello další článek toolearn jak toorun této aplikace na HDInsight Spark clusteru pomocí Livy.</span><span class="sxs-lookup"><span data-stu-id="76f89-201">Advance toohello next article toolearn how toorun this application on an HDInsight Spark cluster using Livy.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="76f89-202">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="76f89-202">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

