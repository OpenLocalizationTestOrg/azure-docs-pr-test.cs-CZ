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
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a><span data-ttu-id="1426b-104">Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="1426b-104">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>

<span data-ttu-id="1426b-105">Zjistěte, jak toouse nástroje HDInsight pro IntelliJ toodevelop Apache Scala aplikace a pak testování hello aplikace na [Hortonworks karanténě](http://hortonworks.com/products/sandbox/) běží na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="1426b-105">Learn how toouse HDInsight Tools for IntelliJ toodevelop Apache Scala applications and then test hello applications on [Hortonworks Sandbox](http://hortonworks.com/products/sandbox/) running on your workstation.</span></span> 

<span data-ttu-id="1426b-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) je Java integrované vývojové prostředí (IDE) pro vývoj počítačového softwaru.</span><span class="sxs-lookup"><span data-stu-id="1426b-106">[IntelliJ IDEA](https://www.jetbrains.com/idea/) is a Java-integrated development environment (IDE) for developing computer software.</span></span> <span data-ttu-id="1426b-107">Po vyvinuté a testování vaší aplikace na Hortonworks karanténě, můžete je přesunout příliš[Azure HDInsight](hdinsight-hadoop-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1426b-107">After you have developed and tested your applications on Hortonworks Sandbox, you can move them too[Azure HDInsight](hdinsight-hadoop-introduction.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1426b-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1426b-108">Prerequisites</span></span>

<span data-ttu-id="1426b-109">Než začnete tento kurz, musíte mít:</span><span class="sxs-lookup"><span data-stu-id="1426b-109">Before you begin this tutorial, you must have:</span></span>

- <span data-ttu-id="1426b-110">Hortonworks Data Platform (HDP) 2.4 na Hortonworks karanténě spuštěné v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1426b-110">Hortonworks Data Platform (HDP) 2.4 on Hortonworks Sandbox running on your local environment.</span></span> <span data-ttu-id="1426b-111">tooconfigure HDP, najdete v části [Začínáme v ekosystému Hadoop hello s Hadoop izolovaném prostoru na virtuálním počítači](hdinsight-hadoop-emulator-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1426b-111">tooconfigure HDP, see [Get started in hello Hadoop ecosystem with a Hadoop sandbox on a virtual machine](hdinsight-hadoop-emulator-get-started.md).</span></span> 
    >[!NOTE]
    ><span data-ttu-id="1426b-112">Nástroje HDInsight pro IntelliJ byl testován pouze s HDP 2.4.</span><span class="sxs-lookup"><span data-stu-id="1426b-112">HDInsight Tools for IntelliJ has been tested only with HDP 2.4.</span></span> <span data-ttu-id="1426b-113">Rozbalte tooget HDP 2.4 **Hortonworks karanténě archivu** na hello [Hortonworks karanténě stáhne lokality](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="1426b-113">tooget HDP 2.4, expand **Hortonworks Sandbox Archive** on hello [Hortonworks Sandbox downloads site](http://hortonworks.com/downloads/#sandbox).</span></span>

- <span data-ttu-id="1426b-114">[Java Developer Kit (JDK) verze 1,8 nebo novější](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="1426b-114">[Java Developer Kit (JDK) version 1.8 or later](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span> <span data-ttu-id="1426b-115">Je požadován JDK hello Azure Toolkit pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="1426b-115">JDK is required by hello Azure Toolkit for IntelliJ.</span></span>

- <span data-ttu-id="1426b-116">[Edice community IntelliJ IDEA](https://www.jetbrains.com/idea/download) s hello [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) modulu plug-in a hello [nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij.md) modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="1426b-116">[IntelliJ IDEA community edition](https://www.jetbrains.com/idea/download) with hello [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) plug-in and hello [Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij.md) plug-in.</span></span> <span data-ttu-id="1426b-117">Nástroje HDInsight pro IntelliJ je k dispozici jako součást hello nástrojů Azure pro IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="1426b-117">HDInsight Tools for IntelliJ is available as a part of hello Azure Toolkit for IntelliJ.</span></span> 

  <span data-ttu-id="1426b-118">tooinstall hello moduly plug-in, hello následující:</span><span class="sxs-lookup"><span data-stu-id="1426b-118">tooinstall hello plug-ins, do hello following:</span></span>

  1. <span data-ttu-id="1426b-119">Otevřete IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="1426b-119">Open IntelliJ IDEA.</span></span>
  2. <span data-ttu-id="1426b-120">Na hello **úvodní** obrazovku, vyberte **konfigurace**a potom vyberte **modulů plug-in**.</span><span class="sxs-lookup"><span data-stu-id="1426b-120">On hello **Welcome** screen, select **Configure**, and then select **Plugins**.</span></span>
  3. <span data-ttu-id="1426b-121">Vyberte **JetBrains instalace modulu plug-in** v levém dolním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="1426b-121">Select **Install JetBrains plugin** in hello lower-left corner.</span></span>
  4. <span data-ttu-id="1426b-122">Použít hello hledání funkce toosearch pro **Scala**a potom vyberte **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="1426b-122">Use hello search function toosearch for **Scala**, and then select **Install**.</span></span>
  5. <span data-ttu-id="1426b-123">Vyberte **restartujte IntelliJ IDEA** toocomplete hello instalace.</span><span class="sxs-lookup"><span data-stu-id="1426b-123">Select **Restart IntelliJ IDEA** toocomplete hello installation.</span></span>
  6. <span data-ttu-id="1426b-124">Opakováním kroků 4 a 5 tooinstall hello **nástrojů Azure pro IntelliJ**.</span><span class="sxs-lookup"><span data-stu-id="1426b-124">Repeat step 4 and 5 tooinstall hello **Azure Toolkit for IntelliJ**.</span></span> <span data-ttu-id="1426b-125">Další informace najdete v tématu [hello instalace nástrojů Azure pro IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="1426b-125">For more information, see [Install hello Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="create-a-spark-scala-application"></a><span data-ttu-id="1426b-126">Vytvoření aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="1426b-126">Create a Spark Scala application</span></span>

<span data-ttu-id="1426b-127">V této části vytvoříte projekt Scala ukázka pomocí IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="1426b-127">In this section, you create a sample Scala project by using IntelliJ IDEA.</span></span> <span data-ttu-id="1426b-128">V další části hello propojit IntelliJ IDEA toohello Hortonworks karanténě (emulátoru) před odesláním hello projektu.</span><span class="sxs-lookup"><span data-stu-id="1426b-128">In hello next section, you link IntelliJ IDEA toohello Hortonworks Sandbox (emulator) before you submit hello project.</span></span>

1. <span data-ttu-id="1426b-129">Otevřete IntelliJ IDEA z pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="1426b-129">Open IntelliJ IDEA from your workstation.</span></span> <span data-ttu-id="1426b-130">V hello **nový projekt** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="1426b-130">In hello **New Project** dialog box, do hello following:</span></span>

   <span data-ttu-id="1426b-131">a.</span><span class="sxs-lookup"><span data-stu-id="1426b-131">a.</span></span> <span data-ttu-id="1426b-132">Vyberte **HDInsight** > **Spark v HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="1426b-132">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="1426b-133">b.</span><span class="sxs-lookup"><span data-stu-id="1426b-133">b.</span></span> <span data-ttu-id="1426b-134">V hello **nástroj pro sestavení** vyberte jednu z následujících, podle potřeby tooyour hello:</span><span class="sxs-lookup"><span data-stu-id="1426b-134">In hello **Build tool** list, select either of hello following, according tooyour need:</span></span>

    * <span data-ttu-id="1426b-135">**Maven**, podpora Průvodce vytvoření projektu Scala</span><span class="sxs-lookup"><span data-stu-id="1426b-135">**Maven**, for Scala project-creation wizard support</span></span>
    * <span data-ttu-id="1426b-136">**SBT**, pro správu hello závislosti a sestavování pro projekt hello Scala</span><span class="sxs-lookup"><span data-stu-id="1426b-136">**SBT**, for managing hello dependencies and building for hello Scala project</span></span>

   ![Dialogové okno Nový projekt Hello](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. <span data-ttu-id="1426b-138">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="1426b-138">Select **Next**.</span></span>

3. <span data-ttu-id="1426b-139">V hello Další **nový projekt** dialogové okno pole, hello následující:</span><span class="sxs-lookup"><span data-stu-id="1426b-139">In hello next **New Project** dialog box, do hello following:</span></span>

    ![Vytvoření IntelliJ Scala vlastnosti projektu](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)

    <span data-ttu-id="1426b-141">a.</span><span class="sxs-lookup"><span data-stu-id="1426b-141">a.</span></span> <span data-ttu-id="1426b-142">V hello **název projektu** zadejte název projektu.</span><span class="sxs-lookup"><span data-stu-id="1426b-142">In hello **Project name** box, enter a project name.</span></span>

    <span data-ttu-id="1426b-143">b.</span><span class="sxs-lookup"><span data-stu-id="1426b-143">b.</span></span> <span data-ttu-id="1426b-144">V hello **projektu umístění** zadejte umístění projektu.</span><span class="sxs-lookup"><span data-stu-id="1426b-144">In hello **Project location** box, enter a project location.</span></span>

    <span data-ttu-id="1426b-145">c.</span><span class="sxs-lookup"><span data-stu-id="1426b-145">c.</span></span> <span data-ttu-id="1426b-146">Další toohello **SDK projektu** rozevíracího seznamu vyberte **nový**, vyberte **JDK**, a pak zadejte hello složky JDK Java verze 1.7 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1426b-146">Next toohello **Project SDK** drop-down list, select **New**, select **JDK**, and then specify hello folder of Java JDK version 1.7 or later.</span></span> <span data-ttu-id="1426b-147">Vyberte **Java 1.8** hello Spark 2.x clusteru, nebo vyberte **Java 1.7** hello Spark 1.x clusteru.</span><span class="sxs-lookup"><span data-stu-id="1426b-147">Select **Java 1.8** for hello Spark 2.x cluster, or select **Java 1.7** for hello Spark 1.x cluster.</span></span> <span data-ttu-id="1426b-148">Hello výchozí umístění je C:\Program Files\Java\jdk1.8.x_xxx.</span><span class="sxs-lookup"><span data-stu-id="1426b-148">hello default location is C:\Program Files\Java\jdk1.8.x_xxx.</span></span>

    <span data-ttu-id="1426b-149">d.</span><span class="sxs-lookup"><span data-stu-id="1426b-149">d.</span></span> <span data-ttu-id="1426b-150">V hello **Spark verze** rozevíracího seznamu, Průvodce vytvořením projektu Scala integruje hello správnou verzi sady SDK Spark a Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="1426b-150">In hello **Spark version** drop-down list, Scala project creation wizard integrates hello proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="1426b-151">Pokud verze clusteru Spark hello je starší než 2.0, vyberte **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="1426b-151">If hello Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="1426b-152">Jinak vyberte možnost **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="1426b-152">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="1426b-153">Tento příklad používá Spark 1.6.2 (Scala 2.10.5).</span><span class="sxs-lookup"><span data-stu-id="1426b-153">This example uses Spark 1.6.2 (Scala 2.10.5).</span></span> <span data-ttu-id="1426b-154">Ujistěte se, že používáte úložiště hello označena Scala 2.10.x.</span><span class="sxs-lookup"><span data-stu-id="1426b-154">Make sure that you are using hello repository marked Scala 2.10.x.</span></span> <span data-ttu-id="1426b-155">Nepoužívejte hello úložiště označena Scala 2.11.x.</span><span class="sxs-lookup"><span data-stu-id="1426b-155">Do not use hello repository marked Scala 2.11.x.</span></span>

4. <span data-ttu-id="1426b-156">Vyberte **Finish** (Dokončit).</span><span class="sxs-lookup"><span data-stu-id="1426b-156">Select **Finish**.</span></span>

5. <span data-ttu-id="1426b-157">Pokud hello **projektu** zobrazení ještě není otevřené, stiskněte **Alt + 1** tooopen ho.</span><span class="sxs-lookup"><span data-stu-id="1426b-157">If hello **Project** view is not already open, press **Alt+1** tooopen it.</span></span>

6. <span data-ttu-id="1426b-158">V **Project Exploreru**, rozbalte hello projektu a pak vyberte **src**.</span><span class="sxs-lookup"><span data-stu-id="1426b-158">In **Project Explorer**, expand hello project, and then select **src**.</span></span>

7. <span data-ttu-id="1426b-159">Klikněte pravým tlačítkem na **src**, bod příliš**nový**a potom vyberte **Scala třída**.</span><span class="sxs-lookup"><span data-stu-id="1426b-159">Right-click **src**, point too**New**, and then select **Scala class**.</span></span>

8. <span data-ttu-id="1426b-160">V hello **název** zadejte název, v hello **druhu** vyberte **objekt**a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="1426b-160">In hello **Name** box, enter a name, in hello **Kind** box, select **Object**, and then select **OK**.</span></span>

    ![okno Vytvořit novou třídu Scala Hello](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. <span data-ttu-id="1426b-162">V souboru .scala hello vložte následující kód hello:</span><span class="sxs-lookup"><span data-stu-id="1426b-162">In hello .scala file, paste hello following code:</span></span>

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

10. <span data-ttu-id="1426b-163">Na hello **sestavení** nabídce vyberte možnost **sestavení projektu**.</span><span class="sxs-lookup"><span data-stu-id="1426b-163">On hello **Build** menu, select **Build project**.</span></span> <span data-ttu-id="1426b-164">Ujistěte se, zda text hello kompilace byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="1426b-164">Make sure that hello compilation has been completed successfully.</span></span>


## <a name="link-toohello-hortonworks-sandbox"></a><span data-ttu-id="1426b-165">Odkaz toohello Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="1426b-165">Link toohello Hortonworks Sandbox</span></span>

<span data-ttu-id="1426b-166">Chcete-li propojit tooa Hortonworks karanténě (emulátoru), musí mít existující aplikaci IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="1426b-166">Before you can link tooa Hortonworks Sandbox (emulator), you must have an existing IntelliJ application.</span></span>

<span data-ttu-id="1426b-167">toolink tooan emulátoru, hello následující:</span><span class="sxs-lookup"><span data-stu-id="1426b-167">toolink tooan emulator, do hello following:</span></span>

1. <span data-ttu-id="1426b-168">Hello otevřete projekt v IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="1426b-168">Open hello project in IntelliJ.</span></span>

2. <span data-ttu-id="1426b-169">Na hello **zobrazení** nabídce vyberte možnost **nástroje Windows**a potom vyberte **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="1426b-169">On hello **View** menu, select **Tools Windows**, and then select **Azure Explorer**.</span></span>

3. <span data-ttu-id="1426b-170">Rozbalte položku **Azure**, klikněte pravým tlačítkem na **HDInsight**a potom vyberte **odkaz emulátor**.</span><span class="sxs-lookup"><span data-stu-id="1426b-170">Expand **Azure**, right-click **HDInsight**, and then select **Link an Emulator**.</span></span>

4. <span data-ttu-id="1426b-171">V hello **odkaz A nový emulátor** okno, zadejte heslo hello jste nakonfigurovali pro kořenový účet hello hello Hortonworks karanténě, zadejte hodnoty podobné toothose v hello následující snímek obrazovky a pak vyberte **OK** .</span><span class="sxs-lookup"><span data-stu-id="1426b-171">In hello **Link A New Emulator** window, enter hello password that you've configured for hello root account of hello Hortonworks Sandbox, enter values similar toothose in hello following screenshot, and then select **OK**.</span></span> 

   ![okno "Odkaz Nový emulátor" Hello](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. <span data-ttu-id="1426b-173">tooconfigure hello emulátoru, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="1426b-173">tooconfigure hello emulator, select **Yes**.</span></span>

<span data-ttu-id="1426b-174">Když se úspěšně připojí hello emulátoru, hello emulátoru (Hortonworks karanténě) je uvedena v uzlu HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="1426b-174">When hello emulator is connected successfully, hello emulator (Hortonworks Sandbox) is listed in hello HDInsight node.</span></span>

## <a name="submit-hello-spark-scala-application-toohello-hortonworks-sandbox"></a><span data-ttu-id="1426b-175">Odeslání toohello aplikací Spark Scala hello Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="1426b-175">Submit hello Spark Scala application toohello Hortonworks Sandbox</span></span>

<span data-ttu-id="1426b-176">Po propojení IntelliJ IDEA toohello emulátoru, můžete odeslat projektu.</span><span class="sxs-lookup"><span data-stu-id="1426b-176">After you have linked IntelliJ IDEA toohello emulator, you can submit your project.</span></span>

<span data-ttu-id="1426b-177">toosubmit tooan projektu emulátoru hello následující:</span><span class="sxs-lookup"><span data-stu-id="1426b-177">toosubmit a project tooan emulator, do hello following:</span></span>

1. <span data-ttu-id="1426b-178">V **Project Exploreru**, klikněte pravým tlačítkem na projekt hello a pak vyberte **odeslání Spark aplikace tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="1426b-178">In **Project Explorer**, right-click hello project, and then select **Submit Spark application tooHDInsight**.</span></span>

2. <span data-ttu-id="1426b-179">Hello následující:</span><span class="sxs-lookup"><span data-stu-id="1426b-179">Do hello following:</span></span>

    <span data-ttu-id="1426b-180">a.</span><span class="sxs-lookup"><span data-stu-id="1426b-180">a.</span></span> <span data-ttu-id="1426b-181">V hello **cluster Spark (pouze Linux)** rozevíracího seznamu vyberte vaše místní Hortonworks karanténě.</span><span class="sxs-lookup"><span data-stu-id="1426b-181">In hello **Spark cluster (Linux only)** drop-down list, select your local Hortonworks Sandbox.</span></span>

    <span data-ttu-id="1426b-182">b.</span><span class="sxs-lookup"><span data-stu-id="1426b-182">b.</span></span> <span data-ttu-id="1426b-183">V hello **hlavní název třídy** pole, vyberte nebo zadejte název hlavní třídy hello.</span><span class="sxs-lookup"><span data-stu-id="1426b-183">In hello **Main class name** box, choose or enter hello main class name.</span></span> <span data-ttu-id="1426b-184">V tomto kurzu je název hello **GroupByTest**.</span><span class="sxs-lookup"><span data-stu-id="1426b-184">For this tutorial, hello name is **GroupByTest**.</span></span>

3. <span data-ttu-id="1426b-185">Vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="1426b-185">Select **Submit**.</span></span> <span data-ttu-id="1426b-186">Hello úlohy odeslání protokolů se zobrazí v okně nástroje odeslání hello Spark.</span><span class="sxs-lookup"><span data-stu-id="1426b-186">hello job submission logs are shown in hello Spark submission tool window.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1426b-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1426b-187">Next steps</span></span>

- <span data-ttu-id="1426b-188">jak toocreate aplikací Spark pro HDInsight pomocí nástroje HDInsight pro IntelliJ, přejděte příliš toolearn[pomocí nástrojů HDInsight v Azure nástrojů pro IntelliJ toocreate aplikací Spark pro cluster HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="1426b-188">toolearn how toocreate Spark applications for HDInsight by using HDInsight Tools for IntelliJ, go too[Use HDInsight Tools in Azure Toolkit for IntelliJ toocreate Spark applications for HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>

- <span data-ttu-id="1426b-189">toowatch video o nástroje HDInsight pro IntelliJ, přejděte příliš[zavést nástroje HDInsight pro IntelliJ pro vývoj Spark](https://www.youtube.com/watch?v=YTZzYVgut6c).</span><span class="sxs-lookup"><span data-stu-id="1426b-189">toowatch a video of HDInsight Tools for IntelliJ, go too[Introduce HDInsight Tools for IntelliJ for Spark Development](https://www.youtube.com/watch?v=YTZzYVgut6c).</span></span>

- <span data-ttu-id="1426b-190">toolearn jak toodebug aplikací Spark pomocí hello toolkit vzdáleně v HDInsight pomocí SSH, přejděte příliš[vzdálené ladění aplikací Spark v clusteru HDInsight pomocí sady Azure Toolkit pro IntelliJ prostřednictvím SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span><span class="sxs-lookup"><span data-stu-id="1426b-190">toolearn how toodebug Spark applications by using hello toolkit remotely on HDInsight through SSH, go too[Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md).</span></span>

- <span data-ttu-id="1426b-191">toolearn jak toodebug aplikací Spark pomocí hello toolkit vzdáleně v HDInsight prostřednictvím sítě VPN, přejděte příliš[pomocí nástrojů HDInsight v Azure nástrojů pro IntelliJ toodebug aplikací Spark vzdáleně na clusteru HDInsight Spark Linux](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span><span class="sxs-lookup"><span data-stu-id="1426b-191">toolearn how toodebug Spark applications by using hello toolkit remotely on HDInsight through VPN, go too[Use HDInsight Tools in Azure Toolkit for IntelliJ toodebug Spark applications remotely on HDInsight Spark Linux cluster](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md).</span></span>

- <span data-ttu-id="1426b-192">jak toouse nástroje HDInsight pro Eclipse toocreate Spark aplikaci, přejděte příliš toolearn[pomocí nástrojů HDInsight v Azure nástrojů pro aplikací Spark toocreate Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="1426b-192">toolearn how toouse HDInsight Tools for Eclipse toocreate a Spark application, go too[Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications](hdinsight-apache-spark-eclipse-tool-plugin.md).</span></span>

- <span data-ttu-id="1426b-193">toowatch video o nástroje HDInsight pro prostředí Eclipse, přejděte příliš[použijte nástroj HDInsight pro aplikací Spark toocreate Eclipse](https://mix.office.com/watch/1rau2mopb6fha).</span><span class="sxs-lookup"><span data-stu-id="1426b-193">toowatch a video of HDInsight Tools for Eclipse, go too[Use HDInsight Tool for Eclipse toocreate Spark applications](https://mix.office.com/watch/1rau2mopb6fha).</span></span>
