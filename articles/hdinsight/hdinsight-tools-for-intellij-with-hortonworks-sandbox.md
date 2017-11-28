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
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a><span data-ttu-id="ecfa3-104">Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="ecfa3-104">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>

<span data-ttu-id="ecfa3-105">Naučte se používat nástroje HDInsight pro IntelliJ k vývoji aplikací Apache Scala a poté otestujte aplikace na [Hortonworks karanténě](http://hortonworks.com/products/sandbox/) běží na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-105">Learn how to use HDInsight Tools for IntelliJ to develop Apache Scala applications and then test the applications on [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) running on your workstation.</span></span> 

<span data-ttu-id="ecfa3-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) je Java integrované vývojové prostředí (IDE) pro vývoj počítačového softwaru.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) is a Java-integrated development environment (IDE) for developing computer software.</span></span> <span data-ttu-id="ecfa3-107">Po vyvinuté a testování vaší aplikace na Hortonworks karanténě, můžete přesunout jejich [Azure HDInsight](hdinsight-hadoop-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-107">After you have developed and tested your applications on Hortonworks Sandbox, you can move them to [Azure HDInsight](hdinsight-hadoop-introduction.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ecfa3-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ecfa3-108">Prerequisites</span></span>

<span data-ttu-id="ecfa3-109">Než začnete tento kurz, musíte mít:</span><span class="sxs-lookup"><span data-stu-id="ecfa3-109">Before you begin this tutorial, you must have:</span></span>

- <span data-ttu-id="ecfa3-110">Hortonworks Data Platform (HDP) 2.4 na Hortonworks karanténě spuštěné v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-110">Hortonworks Data Platform (HDP) 2.4 on Hortonworks Sandbox running on your local environment.</span></span> <span data-ttu-id="ecfa3-111">Konfigurace softwaru HDP najdete v tématu [Začínáme v ekosystému Hadoop s Hadoop izolovaném prostoru na virtuálním počítači](hdinsight-hadoop-emulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-111">To configure HDP, see [Get started in the Hadoop ecosystem with a Hadoop sandbox on a virtual machine](hdinsight-hadoop-emulator-get-started.md).</span></span> 
    >[!NOTE]
    ><span data-ttu-id="ecfa3-112">Nástroje HDInsight pro IntelliJ byl testován pouze s HDP 2.4.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-112">HDInsight Tools for IntelliJ has been tested only with HDP 2.4.</span></span> <span data-ttu-id="ecfa3-113">Chcete-li získat HDP 2.4, rozbalte **Hortonworks karanténě archivu** na [Hortonworks karanténě stáhne lokality](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-113">To get HDP 2.4, expand **Hortonworks Sandbox Archive** on the [Hortonworks Sandbox downloads site](http://hortonworks.com/downloads/#sandbox).</span></span>

- <span data-ttu-id="ecfa3-114">[Java Developer Kit (JDK) verze 1,8 nebo novější](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-114">[Java Developer Kit (JDK) version 1.8 or later](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span> <span data-ttu-id="ecfa3-115">Sada nástrojů Azure vyžaduje JDK pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-115">JDK is required by the Azure Toolkit for IntelliJ.</span></span>

- <span data-ttu-id="ecfa3-116">[Edice community IntelliJ IDEA](https://www.jetbrains.com/idea/download) s [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) modulu plug-in a [nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij.md) modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-116">[IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) with the [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) plug-in and the [Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij.md) plug-in.</span></span> <span data-ttu-id="ecfa3-117">Nástroje HDInsight pro IntelliJ je k dispozici jako součást sady nástrojů Azure pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-117">HDInsight Tools for IntelliJ is available as a part of the Azure Toolkit for IntelliJ.</span></span> 

  <span data-ttu-id="ecfa3-118">Pokud chcete nainstalovat moduly plug-in, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ecfa3-118">To install the plug-ins, do the following:</span></span>

  1. <span data-ttu-id="ecfa3-119">Otevřete IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-119">Open IntelliJ IDEA.</span></span>
  2. <span data-ttu-id="ecfa3-120">Na **úvodní** obrazovku, vyberte **konfigurace**a potom vyberte **modulů plug-in**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-120">On the **Welcome** screen, select **Configure**, and then select **Plugins**.</span></span>
  3. <span data-ttu-id="ecfa3-121">Vyberte **JetBrains instalace modulu plug-in** v levém dolním rohu.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-121">Select **Install JetBrains plugin** in the lower-left corner.</span></span>
  4. <span data-ttu-id="ecfa3-122">Můžete vyhledat pomocí funkce vyhledávání **Scala**a potom vyberte **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-122">Use the search function to search for **Scala**, and then select **Install**.</span></span>
  5. <span data-ttu-id="ecfa3-123">Vyberte **restartujte IntelliJ IDEA** k dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-123">Select **Restart IntelliJ IDEA** to complete the installation.</span></span>
  6. <span data-ttu-id="ecfa3-124">Opakováním kroků 4 a 5 k instalaci **nástrojů Azure pro IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-124">Repeat step 4 and 5 to install the **Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="ecfa3-125">Další informace najdete v tématu [nainstalovat sadu Azure pro IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-125">For more information, see [Install the Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="create-a-spark-scala-application"></a><span data-ttu-id="ecfa3-126">Vytvoření aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="ecfa3-126">Create a Spark Scala application</span></span>

<span data-ttu-id="ecfa3-127">V této části vytvoříte projekt Scala ukázka pomocí IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-127">In this section, you create a sample Scala project by using IntelliJ IDEA.</span></span> <span data-ttu-id="ecfa3-128">V další části můžete propojit IntelliJ IDEA Hortonworks karanténě (emulátoru) před odesláním projektu.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-128">In the next section, you link IntelliJ IDEA to the Hortonworks Sandbox (emulator) before you submit the project.</span></span>

1. <span data-ttu-id="ecfa3-129">Otevřete IntelliJ IDEA z pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-129">Open IntelliJ IDEA from your workstation.</span></span> <span data-ttu-id="ecfa3-130">V **nový projekt** dialogové okno pole, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ecfa3-130">In the **New Project** dialog box, do the following:</span></span>

   <span data-ttu-id="ecfa3-131">a.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-131">a.</span></span> <span data-ttu-id="ecfa3-132">Vyberte **HDInsight** > **Spark v HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-132">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="ecfa3-133">b.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-133">b.</span></span> <span data-ttu-id="ecfa3-134">V **nástroj pro sestavení** vyberte některý z následujících akcí podle potřeby vašeho:</span><span class="sxs-lookup"><span data-stu-id="ecfa3-134">In the **Build tool** list, select either of the following, according to your need:</span></span>

    * <span data-ttu-id="ecfa3-135">**Maven**, podpora Průvodce vytvoření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="ecfa3-135">**Maven**, for Scala project-creation wizard support</span></span>
    * <span data-ttu-id="ecfa3-136">**SBT**, ke správě závislosti a vytváření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="ecfa3-136">**SBT**, for managing the dependencies and building for the Scala project</span></span>

   ![Dialogové okno Nový projekt](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. <span data-ttu-id="ecfa3-138">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-138">Select **Next**.</span></span>

3. <span data-ttu-id="ecfa3-139">V dalším **nový projekt** dialogové okno pole, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ecfa3-139">In the next **New Project** dialog box, do the following:</span></span>

    ![Vytvoření IntelliJ Scala vlastnosti projektu](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    <span data-ttu-id="ecfa3-141">a.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-141">a.</span></span> <span data-ttu-id="ecfa3-142">V **název projektu** zadejte název projektu.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-142">In the **Project name** box, enter a project name.</span></span>

    <span data-ttu-id="ecfa3-143">b.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-143">b.</span></span> <span data-ttu-id="ecfa3-144">V **projektu umístění** zadejte umístění projektu.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-144">In the **Project location** box, enter a project location.</span></span>

    <span data-ttu-id="ecfa3-145">c.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-145">c.</span></span> <span data-ttu-id="ecfa3-146">Vedle položky **SDK projektu** rozevíracího seznamu vyberte **nový**, vyberte **JDK**, a pak zadejte složku JDK Java verze 1.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-146">Next to the **Project SDK** drop-down list, select **New**, select **JDK**, and then specify the folder of Java JDK version 1.7 or later.</span></span> <span data-ttu-id="ecfa3-147">Vyberte **Java 1.8** pro Spark 2.x cluster nebo vyberte **Java 1.7** 1.x clusteru Spark.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-147">Select **Java 1.8** for the Spark 2.x cluster, or select **Java 1.7** for the Spark 1.x cluster.</span></span> <span data-ttu-id="ecfa3-148">Výchozí umístění je C:\Program Files\Java\jdk1.8.x_xxx.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-148">The default location is C:\Program Files\Java\jdk1.8.x_xxx.</span></span>

    <span data-ttu-id="ecfa3-149">d.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-149">d.</span></span> <span data-ttu-id="ecfa3-150">V **Spark verze** rozevíracího seznamu, Průvodce vytvořením projektu Scala integruje správnou verzi sady SDK Spark a Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-150">In the **Spark version** drop-down list, Scala project creation wizard integrates the proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="ecfa3-151">Pokud verze clusteru Spark je starší než 2.0, vyberte **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-151">If the Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="ecfa3-152">Jinak vyberte možnost **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-152">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="ecfa3-153">Tento příklad používá Spark 1.6.2 (Scala 2.10.5).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-153">This example uses Spark 1.6.2 (Scala 2.10.5).</span></span> <span data-ttu-id="ecfa3-154">Ujistěte se, že používáte úložiště označena Scala 2.10.x.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-154">Make sure that you are using the repository marked Scala 2.10.x.</span></span> <span data-ttu-id="ecfa3-155">Nepoužívejte úložiště označena Scala 2.11.x.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-155">Do not use the repository marked Scala 2.11.x.</span></span>

4. <span data-ttu-id="ecfa3-156">Vyberte **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-156">Select **Finish**.</span></span>

5. <span data-ttu-id="ecfa3-157">Pokud **projektu** zobrazení ještě není otevřené, stiskněte **Alt + 1** ho otevřete.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-157">If the **Project** view is not already open, press **Alt+1** to open it.</span></span>

6. <span data-ttu-id="ecfa3-158">V **Project Exploreru**, rozbalte projekt a potom vyberte **src**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-158">In **Project Explorer**, expand the project, and then select **src**.</span></span>

7. <span data-ttu-id="ecfa3-159">Klikněte pravým tlačítkem na **src**, přejděte na příkaz **nový**a potom vyberte **Scala třída**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-159">Right-click **src**, point to **New**, and then select **Scala class**.</span></span>

8. <span data-ttu-id="ecfa3-160">V **název** pole, zadejte název, **druhu** vyberte **objekt**a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-160">In the **Name** box, enter a name, in the **Kind** box, select **Object**, and then select **OK**.</span></span>

    ![Okno Vytvořit novou třídu Scala](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. <span data-ttu-id="ecfa3-162">V souboru .scala vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="ecfa3-162">In the .scala file, paste the following code:</span></span>

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

10. <span data-ttu-id="ecfa3-163">Na **sestavení** nabídce vyberte možnost **sestavení projektu**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-163">On the **Build** menu, select **Build project**.</span></span> <span data-ttu-id="ecfa3-164">Ujistěte se, zda kompilace byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-164">Make sure that the compilation has been completed successfully.</span></span>


## <a name="link-to-the-hortonworks-sandbox"></a><span data-ttu-id="ecfa3-165">Odkaz na Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="ecfa3-165">Link to the Hortonworks Sandbox</span></span>

<span data-ttu-id="ecfa3-166">Předtím, než můžete propojit Hortonworks karanténě (emulátoru), musí mít existující aplikaci IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-166">Before you can link to a Hortonworks Sandbox (emulator), you must have an existing IntelliJ application.</span></span>

<span data-ttu-id="ecfa3-167">Odkaz na emulátoru, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ecfa3-167">To link to an emulator, do the following:</span></span>

1. <span data-ttu-id="ecfa3-168">Otevřete projekt v IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-168">Open the project in IntelliJ.</span></span>

2. <span data-ttu-id="ecfa3-169">Na **zobrazení** nabídce vyberte možnost **nástroje Windows**a potom vyberte **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-169">On the **View** menu, select **Tools Windows**, and then select **Azure Explorer**.</span></span>

3. <span data-ttu-id="ecfa3-170">Rozbalte položku **Azure**, klikněte pravým tlačítkem na **HDInsight**a potom vyberte **odkaz emulátor**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-170">Expand **Azure**, right-click **HDInsight**, and then select **Link an Emulator**.</span></span>

4. <span data-ttu-id="ecfa3-171">V **odkaz A nový emulátor** okno, zadejte heslo, které jste nakonfigurovali pro kořenový účet Hortonworks karanténě, zadejte hodnoty, které jsou podobná nastavením na následujícím snímku obrazovky a pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-171">In the **Link A New Emulator** window, enter the password that you've configured for the root account of the Hortonworks Sandbox, enter values similar to those in the following screenshot, and then select **OK**.</span></span> 

   ![Okno "Odkaz Nový emulátor"](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. <span data-ttu-id="ecfa3-173">Chcete-li nakonfigurovat emulátoru, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-173">To configure the emulator, select **Yes**.</span></span>

<span data-ttu-id="ecfa3-174">Když je emulátor připojen úspěšně, je emulátor (Hortonworks karanténě) uvedené v uzlu HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-174">When the emulator is connected successfully, the emulator (Hortonworks Sandbox) is listed in the HDInsight node.</span></span>

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a><span data-ttu-id="ecfa3-175">Odesílání aplikací Spark Scala k izolovanému prostoru Hortonworks</span><span class="sxs-lookup"><span data-stu-id="ecfa3-175">Submit the Spark Scala application to the Hortonworks Sandbox</span></span>

<span data-ttu-id="ecfa3-176">Po propojení IntelliJ IDEA pro emulátor, můžete odeslat projektu.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-176">After you have linked IntelliJ IDEA to the emulator, you can submit your project.</span></span>

<span data-ttu-id="ecfa3-177">K odeslání projektu na emulátoru, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ecfa3-177">To submit a project to an emulator, do the following:</span></span>

1. <span data-ttu-id="ecfa3-178">V **Project Exploreru**, klikněte pravým tlačítkem na projekt a pak vyberte **aplikace odeslání Spark na HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-178">In **Project Explorer**, right-click the project, and then select **Submit Spark application to HDInsight**.</span></span>

2. <span data-ttu-id="ecfa3-179">Udělejte toto:</span><span class="sxs-lookup"><span data-stu-id="ecfa3-179">Do the following:</span></span>

    <span data-ttu-id="ecfa3-180">a.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-180">a.</span></span> <span data-ttu-id="ecfa3-181">V **cluster Spark (pouze Linux)** rozevíracího seznamu vyberte vaše místní Hortonworks karanténě.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-181">In the **Spark cluster (Linux only)** drop-down list, select your local Hortonworks Sandbox.</span></span>

    <span data-ttu-id="ecfa3-182">b.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-182">b.</span></span> <span data-ttu-id="ecfa3-183">V **hlavní název třídy** pole, vyberte nebo zadejte název hlavní třídy.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-183">In the **Main class name** box, choose or enter the main class name.</span></span> <span data-ttu-id="ecfa3-184">V tomto kurzu je název **GroupByTest**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-184">For this tutorial, the name is **GroupByTest**.</span></span>

3. <span data-ttu-id="ecfa3-185">Vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-185">Select **Submit**.</span></span> <span data-ttu-id="ecfa3-186">Protokoly odeslání úlohy se zobrazí v okně Nástroj odeslání Spark.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-186">The job submission logs are shown in the Spark submission tool window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ecfa3-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ecfa3-187">Next steps</span></span>

- <span data-ttu-id="ecfa3-188">Postup vytvoření aplikací Spark pro HDInsight pomocí nástroje HDInsight pro IntelliJ, přejděte na [pomocí nástrojů HDInsight v Azure nástrojů pro IntelliJ k vytvoření aplikací Spark pro cluster HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-188">To learn how to create Spark applications for HDInsight by using HDInsight Tools for IntelliJ, go to [Use HDInsight Tools in Azure Toolkit for IntelliJ to create Spark applications for HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>

- <span data-ttu-id="ecfa3-189">Pokud chcete přehrát video, nástroje HDInsight pro IntelliJ, přejděte na [zavést nástroje HDInsight pro IntelliJ pro vývoj Spark](https://www.youtube.com/watch?v=YTZzYVgut6c).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-189">To watch a video of HDInsight Tools for IntelliJ, go to [Introduce HDInsight Tools for IntelliJ for Spark Development](https://www.youtube.com/watch?v=YTZzYVgut6c).</span></span>

- <span data-ttu-id="ecfa3-190">Chcete-li se dozvědět, jak k ladění aplikací Spark vzdáleně pomocí sady nástrojů v HDInsight pomocí SSH, přejděte na [vzdálené ladění aplikací Spark v clusteru HDInsight pomocí sady Azure Toolkit pro IntelliJ prostřednictvím SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-190">To learn how to debug Spark applications by using the toolkit remotely on HDInsight through SSH, go to [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span></span>

- <span data-ttu-id="ecfa3-191">Chcete-li se dozvědět, jak k ladění aplikací Spark vzdáleně pomocí sady nástrojů v HDInsight prostřednictvím sítě VPN, přejděte na [pomocí nástrojů HDInsight v Azure nástrojů pro IntelliJ k ladění aplikací Spark vzdáleně na clusteru HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-191">To learn how to debug Spark applications by using the toolkit remotely on HDInsight through VPN, go to [Use HDInsight Tools in Azure Toolkit for IntelliJ to debug Spark applications remotely on HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span></span>

- <span data-ttu-id="ecfa3-192">Naučte se používat nástroje HDInsight pro Eclipse k vytvoření aplikací Spark, přejděte na [pomocí nástrojů HDInsight v Azure nástrojů pro Eclipse k vytvoření aplikací Spark](hdinsight-apache-spark-eclipse-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-192">To learn how to use HDInsight Tools for Eclipse to create a Spark application, go to [Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications](hdinsight-apache-spark-eclipse-tool-plugin.md).</span></span>

- <span data-ttu-id="ecfa3-193">Pokud chcete přehrát video, nástroje HDInsight pro Eclipse, přejděte na [použití nástroje HDInsight pro Eclipse k vytvoření aplikací Spark](https://mix.office.com/watch/1rau2mopb6fha).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-193">To watch a video of HDInsight Tools for Eclipse, go to [Use HDInsight Tool for Eclipse to create Spark applications](https://mix.office.com/watch/1rau2mopb6fha).</span></span>
