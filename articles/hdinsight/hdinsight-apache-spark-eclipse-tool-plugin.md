---
title: "sada Toolkit pro Eclipse – vytvoření Scala aplikací pro HDInsight Spark aaaAzure | Microsoft Docs"
description: "Použití nástrojů HDInsight v Azure nástrojů pro Eclipse toodevelop Spark aplikace napsané v jazyce Scala a odesílat je tooan clusteru HDInsight Spark, přímo z nich hello Eclipse IDE."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f6c79550-5803-4e13-b541-e86c4abb420b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.openlocfilehash: 3ab70857c1e81f591a1c7e29bc1706ec4899ff58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-eclipse-toocreate-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="6217e-103">Použití nástrojů Azure pro Eclipse toocreate aplikací Spark pro cluster služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="6217e-103">Use Azure Toolkit for Eclipse toocreate Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="6217e-104">Pomocí nástrojů HDInsight v Azure nástrojů pro Eclipse toodevelop Spark aplikace napsané v jazyce Scala a odesílat je cluster Azure HDInsight Spark tooan, přímo z hello Eclipse IDE.</span><span class="sxs-lookup"><span data-stu-id="6217e-104">Use HDInsight Tools in Azure Toolkit for Eclipse toodevelop Spark applications written in Scala and submit them tooan Azure HDInsight Spark cluster, directly from hello Eclipse IDE.</span></span> <span data-ttu-id="6217e-105">Můžete použít nástroje HDInsight hello modulu plug-in několika různými způsoby:</span><span class="sxs-lookup"><span data-stu-id="6217e-105">You can use hello HDInsight Tools plug-in in a few different ways:</span></span>

* <span data-ttu-id="6217e-106">toodevelop a odesílání aplikací Scala Spark na clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="6217e-106">toodevelop and submit a Scala Spark application on an HDInsight Spark cluster</span></span>
* <span data-ttu-id="6217e-107">tooaccess vaše prostředky clusteru Azure HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="6217e-107">tooaccess your Azure HDInsight Spark cluster resources</span></span>
* <span data-ttu-id="6217e-108">toodevelop a spustit místně na aplikaci Scala Spark</span><span class="sxs-lookup"><span data-stu-id="6217e-108">toodevelop and run a Scala Spark application locally</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6217e-109">Tento nástroj může být použité toocreate a odeslání aplikací pouze pro cluster služby HDInsight Spark na systému Linux.</span><span class="sxs-lookup"><span data-stu-id="6217e-109">This tool can be used toocreate and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="6217e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6217e-110">Prerequisites</span></span>

* <span data-ttu-id="6217e-111">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6217e-111">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="6217e-112">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="6217e-112">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="6217e-113">Oracle Java Development Kit verze 8, která se používá k hello Eclipse IDE runtime.</span><span class="sxs-lookup"><span data-stu-id="6217e-113">Oracle Java Development Kit version 8, which is used for hello Eclipse IDE runtime.</span></span> <span data-ttu-id="6217e-114">Můžete ji stáhnout z hello [Oracle webu](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="6217e-114">You can download it from hello [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="6217e-115">Integrované vývojové prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6217e-115">Eclipse IDE.</span></span> <span data-ttu-id="6217e-116">Tento článek používá Neónová Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6217e-116">This article uses Eclipse Neon.</span></span> <span data-ttu-id="6217e-117">Můžete ji nainstalovat ze hello [Eclipse webu](https://www.eclipse.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6217e-117">You can install it from hello [Eclipse website](https://www.eclipse.org/downloads/).</span></span>   
* <span data-ttu-id="6217e-118">Spark SDK.</span><span class="sxs-lookup"><span data-stu-id="6217e-118">Spark SDK.</span></span> <span data-ttu-id="6217e-119">Si můžete stáhnout z [Githubu](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="6217e-119">You can download it from [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).</span></span>


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a><span data-ttu-id="6217e-120">Instalace nástrojů HDInsight v Azure nástrojů Eclipse a modulů plug-in Scala</span><span class="sxs-lookup"><span data-stu-id="6217e-120">Install HDInsight Tools in Azure Toolkit for Eclipse and Scala Plugin</span></span>
### <a name="install-hdinsight-tools"></a><span data-ttu-id="6217e-121">Instalace nástrojů HDInsight</span><span class="sxs-lookup"><span data-stu-id="6217e-121">Install HDInsight Tools</span></span>
<span data-ttu-id="6217e-122">Nástroje HDInsight pro Eclipse je k dispozici jako součást nástrojů Azure pro prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6217e-122">HDInsight Tools for Eclipse is available as part of Azure Toolkit for Eclipse.</span></span> <span data-ttu-id="6217e-123">Pokyny k instalaci naleznete v tématu [instalace nástrojů Azure pro Eclipse](../azure-toolkit-for-eclipse-installation.md).</span><span class="sxs-lookup"><span data-stu-id="6217e-123">For installation instructions, see [Installing Azure Toolkit for Eclipse](../azure-toolkit-for-eclipse-installation.md).</span></span>
### <a name="install-scala-plugin"></a><span data-ttu-id="6217e-124">Instalace modulu plug-in Scala</span><span class="sxs-lookup"><span data-stu-id="6217e-124">Install Scala Plugin</span></span>
<span data-ttu-id="6217e-125">Při otevření hello Intellij hello nástroje HDInsight automaticky zjistí, zda jste nainstalovali modul plug-in Scala nebo ne.</span><span class="sxs-lookup"><span data-stu-id="6217e-125">When you open hello Intellij, hello HDInsight Tools auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="6217e-126">Klikněte na tlačítko **OK** toocontinue a postupujte podle hello tooinstall pokyny podle hello Eclipse Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6217e-126">Click **OK** toocontinue and follow hello instructions tooinstall by hello Eclipse Marketplace.</span></span>

 ![Modul plug-in Scala automatické instalace](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-tooyour-azure-subscription"></a><span data-ttu-id="6217e-128">Přihlaste se tooyour předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="6217e-128">Sign in tooyour Azure subscription</span></span>
1. <span data-ttu-id="6217e-129">Spusťte hello Eclipse IDE a otevřete Průzkumník Azure.</span><span class="sxs-lookup"><span data-stu-id="6217e-129">Start hello Eclipse IDE and open Azure Explorer.</span></span> <span data-ttu-id="6217e-130">Na hello **okno** nabídky, klikněte na tlačítko **zobrazit zobrazení**a potom klikněte na **jiných**.</span><span class="sxs-lookup"><span data-stu-id="6217e-130">On hello **Window** menu, click **Show View**, and then click **Other**.</span></span> <span data-ttu-id="6217e-131">V poli hello dialogové okno, které se otevře, rozbalte položku **Azure**, klikněte na tlačítko **Azure Explorer**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6217e-131">In hello dialog box that opens, expand **Azure**, click **Azure Explorer**, and then click **OK**.</span></span>

    ![Zobrazit dialogové okno zobrazení](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. <span data-ttu-id="6217e-133">Klikněte pravým tlačítkem na hello **Azure** uzel a pak klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="6217e-133">Right-click hello **Azure** node, and then click **Sign in**.</span></span>
3. <span data-ttu-id="6217e-134">V hello **přihlásit k Azure** dialogové okno pole, vyberte metodu ověřování hello, klikněte na **přihlášení**a zadejte přihlašovací údaje Azure.</span><span class="sxs-lookup"><span data-stu-id="6217e-134">In hello **Azure Sign In** dialog box, choose hello authentication method, click **Sign in**, and enter your Azure credentials.</span></span>
   
    ![Azure přihlašovací dialogové okno](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. <span data-ttu-id="6217e-136">Poté, co jste přihlášení, hello **vyberte odběry** zobrazí dialogové okno pole všechny hello předplatná Azure přidružená pověření hello.</span><span class="sxs-lookup"><span data-stu-id="6217e-136">After you're signed in, hello **Select Subscriptions** dialog box lists all hello Azure subscriptions associated with hello credentials.</span></span> <span data-ttu-id="6217e-137">Klikněte na tlačítko **vyberte** dialogové okno tooclose hello.</span><span class="sxs-lookup"><span data-stu-id="6217e-137">Click **Select** tooclose hello dialog box.</span></span>

    ![Odběry dialogové okno Vybrat](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. <span data-ttu-id="6217e-139">Na hello **Azure Explorer** rozbalte **HDInsight** toosee hello clusterů HDInsight Spark v rámci svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="6217e-139">On hello **Azure Explorer** tab, expand **HDInsight** toosee hello HDInsight Spark clusters under your subscription.</span></span>
   
    ![Clustery HDInsight Spark v Azure Explorer](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. <span data-ttu-id="6217e-141">Můžete dále rozšířit název uzlu toosee hello prostředky clusteru (například účty úložiště) přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6217e-141">You can further expand a cluster name node toosee hello resources (for example, storage accounts) associated with hello cluster.</span></span>
   
    ![Rozšiřování prostředky toosee název clusteru](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="6217e-143">Nastavení projektu pro cluster služby HDInsight Spark Spark Scala</span><span class="sxs-lookup"><span data-stu-id="6217e-143">Set up a Spark Scala project for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="6217e-144">V pracovním prostoru hello Eclipse IDE, klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="6217e-144">In hello Eclipse IDE workspace, click **File**, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="6217e-145">V Průvodci hello nového projektu, rozbalte položku **HDInsight**, vyberte **Spark v HDInsight (Scala)**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="6217e-145">In hello New Project wizard, expand **HDInsight**, select **Spark on HDInsight (Scala)**, and then click **Next**.</span></span>

    ![Výběr hello Spark na HDInsight (Scala) projektu](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. <span data-ttu-id="6217e-147">Hello Scala projektu vytvoření průvodce automaticky zjistí, zda jste nainstalovali modul plug-in Scala nebo ne.</span><span class="sxs-lookup"><span data-stu-id="6217e-147">hello Scala project creation wizard auto detects whether you installed Scala plugin or not.</span></span> <span data-ttu-id="6217e-148">Klikněte na tlačítko **OK** toocontinue stahování hello Scala modulů plug-in a potom postupujte podle hello pokyny toorestart Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6217e-148">Click **OK** toocontinue downloading hello Scala plugin, then follow hello instructions toorestart Eclipse.</span></span>

    ![Kontrola scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. <span data-ttu-id="6217e-150">V hello **nový projekt Scala HDInsight** dialogové okno, zadejte následující hodnoty hello a pak klikněte na tlačítko **Další**:</span><span class="sxs-lookup"><span data-stu-id="6217e-150">In hello **New HDInsight Scala Project** dialog box, provide hello following values, and then click **Next**:</span></span>
   * <span data-ttu-id="6217e-151">Zadejte název projektu hello.</span><span class="sxs-lookup"><span data-stu-id="6217e-151">Enter a name for hello project.</span></span>
   * <span data-ttu-id="6217e-152">V hello **prostředí JRE** oblasti, ujistěte se, že **používání spuštění prostředí JRE** je nastaven příliš**JavaSE 1.7** nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6217e-152">In hello **JRE** area, make sure that **Use an execution environment JRE** is set too**JavaSE-1.7** or later.</span></span>
   * <span data-ttu-id="6217e-153">Ujistěte se, že Spark SDK je nastavení toohello umístění, kam jste stáhli hello SDK.</span><span class="sxs-lookup"><span data-stu-id="6217e-153">Make sure that Spark SDK is set toohello location where you downloaded hello SDK.</span></span> <span data-ttu-id="6217e-154">Hello umístění stahování toohello odkaz je součástí hello [požadavky](#prerequisites) výše v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="6217e-154">hello link toohello download location is included in hello [prerequisites](#prerequisites) earlier in this article.</span></span> <span data-ttu-id="6217e-155">Můžete také stáhnout hello SDK z hello odkaz zahrnuté v dialogovém okně hello.</span><span class="sxs-lookup"><span data-stu-id="6217e-155">You can also download hello SDK from hello link included in hello dialog box.</span></span>

    ![Dialogové okno Nový projekt HDInsight Scala](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  <span data-ttu-id="6217e-157">V hello další dialogové okno, klikněte na tlačítko hello **knihovny** kartě a zachovat výchozí nastavení hello a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="6217e-157">In hello next dialog box, click hello **Libraries** tab and keep hello defaults, and then click **Finish**.</span></span> 
   
    ![Karta knihovny](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a><span data-ttu-id="6217e-159">Vytvořit aplikaci pro cluster služby HDInsight Spark Scala</span><span class="sxs-lookup"><span data-stu-id="6217e-159">Create a Scala application for an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="6217e-160">V hello Eclipse IDE, z Průzkumníka balíčku, rozbalte hello projekt, který jste předtím vytvořili, klikněte pravým tlačítkem na **src**, bod příliš**nový**a potom klikněte na **jiných**.</span><span class="sxs-lookup"><span data-stu-id="6217e-160">In hello Eclipse IDE, from Package Explorer, expand hello project that you created earlier, right-click **src**, point too**New**, and then click **Other**.</span></span>
2. <span data-ttu-id="6217e-161">V hello **vyberte Průvodce** dialogové okno, rozbalte seznam **Scala průvodců**, klikněte na tlačítko **Scala objekt**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="6217e-161">In hello **Select a wizard** dialog box, expand **Scala Wizards**, click **Scala Object**, and then click **Next**.</span></span>
   
    ![Vyberte dialogové okno Průvodce](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. <span data-ttu-id="6217e-163">V hello **vytvořit nový soubor** dialogové okno, zadejte název pro objekt hello a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="6217e-163">In hello **Create New File** dialog box, enter a name for hello object, and then click **Finish**.</span></span>
   
    ![Vytvořit nový soubor – dialogové okno](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. <span data-ttu-id="6217e-165">Vložte následující kód v textovém editoru hello hello:</span><span class="sxs-lookup"><span data-stu-id="6217e-165">Paste hello following code in hello text editor:</span></span>
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find hello rows that have only one digit in hello seventh column in hello CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
5. <span data-ttu-id="6217e-166">Spuštění aplikace hello na clusteru HDInsight Spark:</span><span class="sxs-lookup"><span data-stu-id="6217e-166">Run hello application on an HDInsight Spark cluster:</span></span>
   
   1. <span data-ttu-id="6217e-167">V Průzkumníku balíčku, klikněte pravým tlačítkem na název projektu hello a potom vyberte **odesílání aplikací Spark tooHDInsight**.</span><span class="sxs-lookup"><span data-stu-id="6217e-167">From Package Explorer, right-click hello project name, and then select **Submit Spark Application tooHDInsight**.</span></span>        
   2. <span data-ttu-id="6217e-168">V hello **Spark odeslání** dialogové okno, zadejte následující hodnoty hello a pak klikněte na tlačítko **odeslání**:</span><span class="sxs-lookup"><span data-stu-id="6217e-168">In hello **Spark Submission** dialog box, provide hello following values, and then click **Submit**:</span></span>
      
      * <span data-ttu-id="6217e-169">Pro **název clusteru**, vyberte hello clusteru HDInsight Spark, na kterém chcete toorun vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6217e-169">For **Cluster Name**, select hello HDInsight Spark cluster on which you want toorun your application.</span></span>
      * <span data-ttu-id="6217e-170">Vyberte artefakt z projektu Eclipse hello nebo vyberte jednu z pevného disku.</span><span class="sxs-lookup"><span data-stu-id="6217e-170">Select an artifact from hello Eclipse project, or select one from a hard drive.</span></span> <span data-ttu-id="6217e-171">Výchozí hodnota Hello závisí na hello položku, kterou klikněte pravým tlačítkem z Průzkumníka balíčku.</span><span class="sxs-lookup"><span data-stu-id="6217e-171">hello default value depends on hello item you right-click from package explorer.</span></span>
      * <span data-ttu-id="6217e-172">V hello **hlavní název třídy** rozevírací seznam, odeslání Průvodce zobrazí všechny názvy objektů z vybraného projektu.</span><span class="sxs-lookup"><span data-stu-id="6217e-172">In hello **Main class name** dropdownlist, submission wizard displays all object names from your selected project.</span></span> <span data-ttu-id="6217e-173">Vyberte nebo zadejte jednu, které chcete toorun.</span><span class="sxs-lookup"><span data-stu-id="6217e-173">Select or input one that you want toorun.</span></span> <span data-ttu-id="6217e-174">Pokud vyberete artefaktů z pevného disku, je nutné zadat název hlavní třídy sami podle.</span><span class="sxs-lookup"><span data-stu-id="6217e-174">If you select artifact from hard disk, you need input main class name by yourself.</span></span> 
      * <span data-ttu-id="6217e-175">Protože kód aplikace hello v tomto příkladu nevyžaduje žádných argumentů příkazového řádku ani odkazovat JAR nebo soubory, můžete ponechat hello zbývající textová pole prázdná.</span><span class="sxs-lookup"><span data-stu-id="6217e-175">Because hello application code in this example does not require any command-line arguments or reference JARs or files, you can leave hello remaining text boxes empty.</span></span>
        
       ![Dialogové okno Spark odeslání](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. <span data-ttu-id="6217e-177">Hello **Spark odeslání** karta by se měl spustit zobrazování průběhu hello.</span><span class="sxs-lookup"><span data-stu-id="6217e-177">hello **Spark Submission** tab should start displaying hello progress.</span></span> <span data-ttu-id="6217e-178">Hello aplikaci můžete ukončit kliknutím na tlačítko hello red v hello **Spark odeslání** okno.</span><span class="sxs-lookup"><span data-stu-id="6217e-178">You can stop hello application by clicking hello red button in hello **Spark Submission** window.</span></span> <span data-ttu-id="6217e-179">Můžete také zobrazit hello protokoly pro tuto konkrétní aplikaci spustit kliknutím na ikonu zeměkouli hello (označen hello modrá značka obrázku hello).</span><span class="sxs-lookup"><span data-stu-id="6217e-179">You can also view hello logs for this specific application run by clicking hello globe icon (denoted by hello blue box in hello image).</span></span>
      
       ![Okno Spark odeslání](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a><span data-ttu-id="6217e-181">Přístup a spravovat clustery HDInsight Spark pomocí nástrojů HDInsight v Azure nástrojů pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="6217e-181">Access and manage HDInsight Spark clusters by using HDInsight Tools in Azure Toolkit for Eclipse</span></span>
<span data-ttu-id="6217e-182">Můžete provádět různé operace pomocí nástroje HDInsight, včetně přístupu k výstup úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="6217e-182">You can perform various operations by using HDInsight Tools, including accessing hello job output.</span></span>

### <a name="access-hello-job-view"></a><span data-ttu-id="6217e-183">Zobrazení úloh hello přístup</span><span class="sxs-lookup"><span data-stu-id="6217e-183">Access hello job view</span></span>
1. <span data-ttu-id="6217e-184">V Průzkumníku Azure rozbalte **HDInsight**, rozbalte název clusteru Spark hello a pak klikněte na tlačítko **úlohy**.</span><span class="sxs-lookup"><span data-stu-id="6217e-184">In Azure Explorer, expand **HDInsight**, expand hello Spark cluster name, and then click **Jobs**.</span></span> 

    ![Úlohy zobrazení uzlu](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="6217e-186">Klikněte na hello **úlohy** uzlu.</span><span class="sxs-lookup"><span data-stu-id="6217e-186">Click on hello **Jobs** node.</span></span> <span data-ttu-id="6217e-187">Nástroje HDInsight Hello automaticky zjistí zda jste nainstalovali modul plug-in clipse hello E (fx) nebo ne.</span><span class="sxs-lookup"><span data-stu-id="6217e-187">hello HDInsight Tools auto-detects whether you installed hello E(fx)clipse plugin or not.</span></span> <span data-ttu-id="6217e-188">Klikněte na tlačítko **OK** toocontinue a postupujte podle pokynů hello tooinstall hello Eclipse Marketplace a restartování prostředí Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6217e-188">Click **OK** toocontinue and follow hello instructions tooinstall hello Eclipse Marketplace and restart Eclipse.</span></span>

    ![Nainstalujte clipse E (fx)](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. <span data-ttu-id="6217e-190">Otevřete hello zobrazení úloh z hello **úlohy** uzlu.</span><span class="sxs-lookup"><span data-stu-id="6217e-190">Open hello Job View from hello **Jobs** node.</span></span> <span data-ttu-id="6217e-191">V pravém podokně hello hello **zobrazení úloh Spark** karta zobrazuje všechny hello aplikace, které byly spuštěny v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="6217e-191">In hello right pane, hello **Spark Job View** tab displays all hello applications that were run on hello cluster.</span></span> <span data-ttu-id="6217e-192">Klikněte na název hello hello aplikace, pro které chcete toosee další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6217e-192">Click hello name of hello application for which you want toosee more details.</span></span>

    ![Podrobnosti o aplikaci](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. <span data-ttu-id="6217e-194">Pokud jste při přechodu myší na graf úlohy hello, zobrazuje základní informace o spuštěné úlohy.</span><span class="sxs-lookup"><span data-stu-id="6217e-194">If you hover on hello job graph, it displays basic running job info.</span></span> <span data-ttu-id="6217e-195">Kliknutím na graf úlohy hello zobrazuje graf fázích hello a informací, které generuje každých úlohy.</span><span class="sxs-lookup"><span data-stu-id="6217e-195">Clicking on hello job graph shows hello stages graph and info that every job generates.</span></span>

    ![Fáze podrobnosti úlohy](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. <span data-ttu-id="6217e-197">Často používané protokoly, včetně ovladačů Stderr, ovladač Stdout a informací o adresáři, jsou uvedeny v hello **protokolu** kartě.</span><span class="sxs-lookup"><span data-stu-id="6217e-197">Frequently used logs, including Driver Stderr, Driver Stdout, and Directory Info, are listed in hello **Log** tab.</span></span>

    ![Podrobnosti protokolu](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. <span data-ttu-id="6217e-199">Můžete také otevřít hello Spark historie uživatelského rozhraní a hello YARN uživatelského rozhraní (na úrovni aplikace hello) kliknutím na příslušné hypertextový odkaz hello hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="6217e-199">You can also open hello Spark history UI and hello YARN UI (at hello application level) by clicking hello respective hyperlink at hello top of hello window.</span></span>

### <a name="access-hello-storage-container-for-hello-cluster"></a><span data-ttu-id="6217e-200">Kontejner úložiště hello přístup pro hello cluster</span><span class="sxs-lookup"><span data-stu-id="6217e-200">Access hello storage container for hello cluster</span></span>
1. <span data-ttu-id="6217e-201">V Průzkumníku Azure rozbalte hello **HDInsight** kořenový uzel toosee seznam clustery HDInsight Spark, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6217e-201">In Azure Explorer, expand hello **HDInsight** root node toosee a list of HDInsight Spark clusters that are available.</span></span>
2. <span data-ttu-id="6217e-202">Rozbalte hello clusteru název účtu úložiště toosee hello a hello výchozí kontejner úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="6217e-202">Expand hello cluster name toosee hello storage account and hello default storage container for hello cluster.</span></span>
   
    ![Kontejner úložiště účet a výchozí úložiště](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. <span data-ttu-id="6217e-204">Klikněte na název kontejneru úložiště hello přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6217e-204">Click hello storage container name associated with hello cluster.</span></span> <span data-ttu-id="6217e-205">V pravém podokně hello, klikněte dvakrát na hello **HVACOut** složky.</span><span class="sxs-lookup"><span data-stu-id="6217e-205">In hello right pane, double-click hello **HVACOut** folder.</span></span> <span data-ttu-id="6217e-206">Otevřete ho hello **část -** soubory výstup hello toosee aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6217e-206">Open one of hello **part-** files toosee hello output of hello application.</span></span>

### <a name="access-hello-spark-history-server"></a><span data-ttu-id="6217e-207">Přístup k serveru historie Spark hello</span><span class="sxs-lookup"><span data-stu-id="6217e-207">Access hello Spark history server</span></span>
1. <span data-ttu-id="6217e-208">V Průzkumníku Azure, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete uživatelské rozhraní historie Spark**.</span><span class="sxs-lookup"><span data-stu-id="6217e-208">In Azure Explorer, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> <span data-ttu-id="6217e-209">Když se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6217e-209">When you're prompted, enter hello admin credentials for hello cluster.</span></span> <span data-ttu-id="6217e-210">Musí být zadána tyto při zřizování clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="6217e-210">You must have specified these while provisioning hello cluster.</span></span>
2. <span data-ttu-id="6217e-211">V hello Spark historie řídicího panelu serveru použijte toolook název aplikace hello hello aplikace právě dokončila spuštění.</span><span class="sxs-lookup"><span data-stu-id="6217e-211">In hello Spark history server dashboard, you use hello application name toolook for hello application that you just finished running.</span></span> <span data-ttu-id="6217e-212">V předchozích kód hello, nastavte název aplikace hello pomocí `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="6217e-212">In hello preceding code, you set hello application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="6217e-213">Proto se název aplikace Spark **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="6217e-213">Hence, your Spark application name was **MyClusterApp**.</span></span>

### <a name="start-hello-ambari-portal"></a><span data-ttu-id="6217e-214">Spustit portál Ambari hello</span><span class="sxs-lookup"><span data-stu-id="6217e-214">Start hello Ambari portal</span></span>
1. <span data-ttu-id="6217e-215">V Průzkumníku Azure, klikněte pravým tlačítkem na název clusteru Spark a pak vyberte **otevřete portál pro správu clusteru (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="6217e-215">In Azure Explorer, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 
2. <span data-ttu-id="6217e-216">Když se zobrazí výzva, zadejte přihlašovací údaje správce hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6217e-216">When you're prompted, enter hello admin credentials for hello cluster.</span></span> <span data-ttu-id="6217e-217">Musí být zadána tyto při zřizování clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="6217e-217">You must have specified these while provisioning hello cluster.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="6217e-218">Spravovat předplatná Azure</span><span class="sxs-lookup"><span data-stu-id="6217e-218">Manage Azure subscriptions</span></span>
<span data-ttu-id="6217e-219">Ve výchozím nastavení seznam nástrojů HDInsight v Azure nástrojů pro Eclipse clustery Spark hello ze všech předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="6217e-219">By default, HDInsight Tools in Azure Toolkit for Eclipse lists hello Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="6217e-220">V případě potřeby můžete zadat hello odběry, pro které chcete tooaccess hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="6217e-220">If necessary, you can specify hello subscriptions for which you want tooaccess hello cluster.</span></span> 

1. <span data-ttu-id="6217e-221">V Průzkumníku Azure, klikněte pravým tlačítkem na hello **Azure** kořenový uzel a potom klikněte na **Spravovat odběry**.</span><span class="sxs-lookup"><span data-stu-id="6217e-221">In Azure Explorer, right-click hello **Azure** root node, and then click **Manage Subscriptions**.</span></span> 
2. <span data-ttu-id="6217e-222">V dialogovém okně hello, zrušte zaškrtnutí políček hello hello předplatného, nemáte má tooaccess a pak klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="6217e-222">In hello dialog box, clear hello check boxes for hello subscription that you don't want tooaccess, and then click **Close**.</span></span> <span data-ttu-id="6217e-223">Můžete také kliknout na **Odhlásit** Pokud chcete toosign mimo vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="6217e-223">You can also click **Sign Out** if you want toosign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="6217e-224">Místní spuštění aplikace Spark Scala</span><span class="sxs-lookup"><span data-stu-id="6217e-224">Run a Spark Scala application locally</span></span>
<span data-ttu-id="6217e-225">Můžete použít nástroje HDInsight v Azure nástrojů aplikací Spark Scala toorun Eclipse místně na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="6217e-225">You can use HDInsight Tools in Azure Toolkit for Eclipse toorun Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="6217e-226">Obvykle se tyto aplikace nepotřebují přístup k prostředkům toocluster například kontejner úložiště a můžete spustit a otestovat je místně.</span><span class="sxs-lookup"><span data-stu-id="6217e-226">Typically, these applications don't need access toocluster resources such as a storage container, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="6217e-227">Požadavek</span><span class="sxs-lookup"><span data-stu-id="6217e-227">Prerequisite</span></span>
<span data-ttu-id="6217e-228">Když spouštíte na počítači se systémem Windows hello místních aplikací Spark Scala, jak je popsáno v může získat výjimku [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="6217e-228">While you're running hello local Spark Scala application on a Windows computer, you might get an exception as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="6217e-229">K této výjimce, protože **WinUtils.exe** chybí v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="6217e-229">This exception occurs because **WinUtils.exe** is missing in Windows.</span></span> 

<span data-ttu-id="6217e-230">tooresolve tato chyba, je nutné [stáhnout hello spustitelné](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa umístění jako **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="6217e-230">tooresolve this error, you must [download hello executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa location like **C:\WinUtils\bin**.</span></span> <span data-ttu-id="6217e-231">Pak musíte přidat proměnnou prostředí hello **HADOOP_HOME** a nastavte hodnotu hello hello proměnné příliš**C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="6217e-231">You must then add hello environment variable **HADOOP_HOME** and set hello value of hello variable too**C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="6217e-232">Spustit místních aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="6217e-232">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="6217e-233">Spusťte Eclipse a vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="6217e-233">Start Eclipse and create a project.</span></span> <span data-ttu-id="6217e-234">V hello **nový projekt** dialogové okno, ujistěte se, hello následující možnosti a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6217e-234">In hello **New Project** dialog box, make hello following choices, and then click **Next**.</span></span>
   
   * <span data-ttu-id="6217e-235">V levém podokně hello vyberte **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="6217e-235">In hello left pane, select **HDInsight**.</span></span>
   * <span data-ttu-id="6217e-236">V pravém podokně hello vyberte **Spark v ukázkové spustit HDInsight místní (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="6217e-236">In hello right pane, select **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    ![Dialogové okno Nový projekt](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. <span data-ttu-id="6217e-238">hello podrobností projektu tooprovide hello, postupujte podle kroků 3 až 6 z výše uvedené části [nastavení projektu Spark Scala pro cluster služby HDInsight Spark](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span><span class="sxs-lookup"><span data-stu-id="6217e-238">tooprovide hello project details, follow steps 3 through 6 from hello earlier section [Set up a Spark Scala project for an HDInsight Spark cluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).</span></span>
3. <span data-ttu-id="6217e-239">Ukázkový kód přidá šablona Hello (**LogQuery**) v části hello **src** složky, kterou můžete spustit místně v počítači.</span><span class="sxs-lookup"><span data-stu-id="6217e-239">hello template adds a sample code (**LogQuery**) under hello **src** folder that you can run locally on your computer.</span></span>
   
    ![Umístění LogQuery](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. <span data-ttu-id="6217e-241">Klikněte pravým tlačítkem na hello **LogQuery** aplikace, bod příliš**spustit jako**a potom klikněte na **aplikace: 1 Scala**.</span><span class="sxs-lookup"><span data-stu-id="6217e-241">Right-click hello **LogQuery** application, point too**Run As**, and then click **1 Scala Application**.</span></span> <span data-ttu-id="6217e-242">Zobrazí se výstup takto v hello **konzoly** kartě dolnímu hello:</span><span class="sxs-lookup"><span data-stu-id="6217e-242">You will see an output like this in hello **Console** tab at hello bottom:</span></span>
   
   ![Místní aplikace Spark, výsledek spuštění](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a><span data-ttu-id="6217e-244">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="6217e-244">FAQ</span></span>
<span data-ttu-id="6217e-245">Zvolte toosubmit aplikaci tooAzure Data Lake Store, **interaktivní** režimu během procesu hello Azure přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6217e-245">toosubmit an application tooAzure Data Lake Store, choose **Interactive** mode during hello Azure sign-in process.</span></span> <span data-ttu-id="6217e-246">Pokud vyberete **automatizovaná** režimu, můžete dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="6217e-246">If you select **Automated** mode, you can get an error.</span></span>

![interaktivní přihlášení](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

<span data-ttu-id="6217e-248">Nyní jsme ho vyřešil.</span><span class="sxs-lookup"><span data-stu-id="6217e-248">Now, we resolved it.</span></span> <span data-ttu-id="6217e-249">Můžete použít Azure Data Lake clusteru toosubmit vaší aplikace pomocí libovolné metody přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6217e-249">You can choose an Azure Data Lake Cluster toosubmit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="6217e-250">Názory a známé problémy</span><span class="sxs-lookup"><span data-stu-id="6217e-250">Feedback and known issues</span></span>
<span data-ttu-id="6217e-251">V současné době Spark výstupů zobrazení přímo není podporováno.</span><span class="sxs-lookup"><span data-stu-id="6217e-251">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="6217e-252">Pokud máte jakékoli návrhy či názory, nebo pokud se vyskytnou potíže při používání tohoto nástroje, myslíte, že volné toosend nám e-mail na hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="6217e-252">If you have any suggestions or feedback, or if you encounter any problems when using this tool, feel free toosend us an email at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="6217e-253"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="6217e-253"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="6217e-254">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="6217e-254">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="6217e-255">Scénáře</span><span class="sxs-lookup"><span data-stu-id="6217e-255">Scenarios</span></span>
* [<span data-ttu-id="6217e-256">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="6217e-256">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="6217e-257">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="6217e-257">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="6217e-258">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="6217e-258">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="6217e-259">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="6217e-259">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="6217e-260">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="6217e-260">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="6217e-261">Vytváření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="6217e-261">Creating and running applications</span></span>
* [<span data-ttu-id="6217e-262">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="6217e-262">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="6217e-263">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="6217e-263">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="6217e-264">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="6217e-264">Tools and extensions</span></span>
* [<span data-ttu-id="6217e-265">Použijte sadu nástrojů Azure pro IntelliJ toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="6217e-265">Use Azure Toolkit for IntelliJ toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="6217e-266">Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně prostřednictvím sítě VPN</span><span class="sxs-lookup"><span data-stu-id="6217e-266">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="6217e-267">Použití nástrojů Azure pro IntelliJ toodebug aplikací Spark vzdáleně přes SSH</span><span class="sxs-lookup"><span data-stu-id="6217e-267">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="6217e-268">Použití nástroje HDInsight pro IntelliJ s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="6217e-268">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="6217e-269">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="6217e-269">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="6217e-270">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="6217e-270">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="6217e-271">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="6217e-271">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="6217e-272">Do počítače nainstalovat Jupyter a připojte tooan clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="6217e-272">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="6217e-273">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="6217e-273">Managing resources</span></span>
* [<span data-ttu-id="6217e-274">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="6217e-274">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="6217e-275">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="6217e-275">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

