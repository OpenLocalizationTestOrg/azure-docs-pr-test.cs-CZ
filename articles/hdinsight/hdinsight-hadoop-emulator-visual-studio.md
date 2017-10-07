---
title: "nástroje aaaData Lake pro Visual Studio s Hortonworks karanténě - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse hello nástroje Azure Data Lake pro Visual Studio s izolovaného prostoru Hortonworks hello spuštěné v místní virtuální počítač. Pomocí těchto nástrojů můžete vytvořit a spustit úlohy Hive a Pig na hello izolovaného prostoru a zobrazit výstup úlohy a historie."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e3434c45-95d1-4b96-ad4c-fb59870e2ff0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: 7a3406b28d87d953e9b5be387faa86268f47b935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-data-lake-tools-for-visual-studio-with-hello-hortonworks-sandbox"></a><span data-ttu-id="c1986-104">Použít hello nástroje Azure Data Lake pro Visual Studio s hello Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="c1986-104">Use hello Azure Data Lake tools for Visual Studio with hello Hortonworks Sandbox</span></span>

<span data-ttu-id="c1986-105">Azure Data Lake obsahuje nástroje pro práci s obecné clusterů systému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c1986-105">Azure Data Lake includes tools for working with generic Hadoop clusters.</span></span> <span data-ttu-id="c1986-106">Tento dokument obsahuje kroky hello potřeby hello nástrojů Data Lake hello toouse s Hortonworks karanténě běží na místním virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="c1986-106">This document provides hello steps needed toouse hello Data Lake tools with hello Hortonworks Sandbox running in a local virtual machine.</span></span>

<span data-ttu-id="c1986-107">Pomocí hello Hortonworks karanténě vám umožní toowork s Hadoop místně na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="c1986-107">Using hello Hortonworks Sandbox allows you toowork with Hadoop locally on your development environment.</span></span> <span data-ttu-id="c1986-108">Poté, co jste si vytvořili řešení a chcete toodeploy ji ve velkém měřítku, potom můžete přesunout tooan clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c1986-108">After you have developed a solution and want toodeploy it at scale, you can then move tooan HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1986-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c1986-109">Prerequisites</span></span>

* <span data-ttu-id="c1986-110">Hello Hortonworks karanténě, běží na virtuálním počítači na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="c1986-110">hello Hortonworks Sandbox, running in a virtual machine on your development environment.</span></span> <span data-ttu-id="c1986-111">Tento dokument byl zapsán a testovány s izolovaného prostoru hello spuštěné v Oracle VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="c1986-111">This document was written and tested with hello sandbox running in Oracle VirtualBox.</span></span> <span data-ttu-id="c1986-112">Informace o nastavení izolovaného prostoru hello najdete v tématu hello [začít pracovat s Hortonworks karanténě hello.](hdinsight-hadoop-emulator-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="c1986-112">For information on setting up hello sandbox, see hello [Get started with hello Hortonworks sandbox.](hdinsight-hadoop-emulator-get-started.md)</span></span> <span data-ttu-id="c1986-113">dokument.</span><span class="sxs-lookup"><span data-stu-id="c1986-113">document.</span></span>

* <span data-ttu-id="c1986-114">Visual Studio 2013, Visual Studio 2015 nebo Visual Studio 2017 (všechny edice).</span><span class="sxs-lookup"><span data-stu-id="c1986-114">Visual Studio 2013, Visual Studio 2015, or Visual Studio 2017 (any edition).</span></span>

* <span data-ttu-id="c1986-115">Hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c1986-115">hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1 or later.</span></span>

* <span data-ttu-id="c1986-116">Hello [nástroje Azure Data Lake pro Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="c1986-116">hello [Azure Data Lake tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

## <a name="configure-passwords-for-hello-sandbox"></a><span data-ttu-id="c1986-117">Konfigurace hesel pro izolovaný prostor hello</span><span class="sxs-lookup"><span data-stu-id="c1986-117">Configure passwords for hello sandbox</span></span>

<span data-ttu-id="c1986-118">Ujistěte se, že hello Hortonworks karanténě běží.</span><span class="sxs-lookup"><span data-stu-id="c1986-118">Make sure that hello Hortonworks Sandbox is running.</span></span> <span data-ttu-id="c1986-119">Potom postupujte podle kroků hello v hello [Začínáme v hello Hortonworks karanténě](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="c1986-119">Then follow hello steps in hello [Get started in hello Hortonworks Sandbox](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) document.</span></span> <span data-ttu-id="c1986-120">Tyto kroky konfigurace hello heslo pro hello SSH `root` účtu a hello Ambari `admin` účtu.</span><span class="sxs-lookup"><span data-stu-id="c1986-120">These steps configure hello password for hello SSH `root` account, and hello Ambari `admin` account.</span></span> <span data-ttu-id="c1986-121">Tato hesla se používají při připojení izolovaného prostoru toohello ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1986-121">These passwords are used when you connect toohello sandbox from Visual Studio.</span></span>

## <a name="connect-hello-tools-toohello-sandbox"></a><span data-ttu-id="c1986-122">Připojit hello nástroje toohello izolovaného prostoru</span><span class="sxs-lookup"><span data-stu-id="c1986-122">Connect hello tools toohello sandbox</span></span>

1. <span data-ttu-id="c1986-123">Otevřete Visual Studio, vyberte **zobrazení**a potom vyberte **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="c1986-123">Open Visual Studio, select **View**, and then select **Server Explorer**.</span></span>

2. <span data-ttu-id="c1986-124">Z **Průzkumníka serveru**, klikněte pravým tlačítkem na hello **HDInsight** položku a potom vyberte **připojit tooHDInsight emulátoru**.</span><span class="sxs-lookup"><span data-stu-id="c1986-124">From **Server Explorer**, right-click hello **HDInsight** entry, and then select **Connect tooHDInsight Emulator**.</span></span>

    ![Snímek obrazovky z Průzkumníku serveru s Connect tooHDInsight emulátoru zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. <span data-ttu-id="c1986-126">Z hello **připojit tooHDInsight emulátoru** dialogovém okně zadejte hello heslo, které jste nakonfigurovali pro Ambari.</span><span class="sxs-lookup"><span data-stu-id="c1986-126">From hello **Connect tooHDInsight Emulator** dialog box, enter hello password that you configured for Ambari.</span></span>

    ![Snímek obrazovky dialogového okna, se zvýrazněnou textového pole hesla](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    <span data-ttu-id="c1986-128">Vyberte **Další** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="c1986-128">Select **Next** toocontinue.</span></span>

4. <span data-ttu-id="c1986-129">Použití hello **heslo** pole tooenter hello heslo jste nakonfigurovali pro hello `root` účtu.</span><span class="sxs-lookup"><span data-stu-id="c1986-129">Use hello **Password** field tooenter hello password you configured for hello `root` account.</span></span> <span data-ttu-id="c1986-130">Ostatní pole na výchozí hodnotu hello zůstanou hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-130">Leave hello other fields at hello default value.</span></span>

    ![Snímek obrazovky dialogového okna, se zvýrazněnou textového pole hesla](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    <span data-ttu-id="c1986-132">Vyberte **Další** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="c1986-132">Select **Next** toocontinue.</span></span>

5. <span data-ttu-id="c1986-133">Počkejte, než pro ověření služby toofinish hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-133">Wait for validation of hello services toofinish.</span></span> <span data-ttu-id="c1986-134">V některých případech může ověření nezdaří a vyzvat vás tooupdate hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c1986-134">In some cases, validation may fail and prompt you tooupdate hello configuration.</span></span> <span data-ttu-id="c1986-135">Pokud ověření selže, vyberte **aktualizace**a počkat na hello konfigurace a ověření pro služby toofinish hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-135">If validation fails, select **Update**, and wait for hello configuration and verification for hello service toofinish.</span></span>

    ![Snímek obrazovky dialogového okna, pomocí tlačítka Aktualizovat zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > <span data-ttu-id="c1986-137">proces aktualizace Hello používá hello toomodify Ambari toowhat Hortonworks karanténě konfigurace je očekáváno hello nástrojů Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1986-137">hello update process uses Ambari toomodify hello Hortonworks Sandbox configuration toowhat is expected by hello Data Lake tools for Visual Studio.</span></span>

6. <span data-ttu-id="c1986-138">Po dokončení ověření vyberte **Dokončit** toocomplete konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c1986-138">After validation has finished, select **Finish** toocomplete configuration.</span></span>
    <span data-ttu-id="c1986-139">![Snímek obrazovky dialogového okna, s tlačítkem Dokončit](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span><span class="sxs-lookup"><span data-stu-id="c1986-139">![Screenshot of dialog box, with Finish button highlighted](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span></span>

     >[!NOTE]
     > <span data-ttu-id="c1986-140">V závislosti na rychlosti hello vývojového prostředí a hello množství paměti přidělené toohello virtuálního počítače může trvat několik minut tooconfigure a ověřit hello služby.</span><span class="sxs-lookup"><span data-stu-id="c1986-140">Depending on hello speed of your development environment, and hello amount of memory allocated toohello virtual machine, it can take several minutes tooconfigure and validate hello services.</span></span>

<span data-ttu-id="c1986-141">Po následujícím postupem, nyní máte **místní cluster HDInsight** položky v Průzkumníku serveru pod hello **HDInsight** části.</span><span class="sxs-lookup"><span data-stu-id="c1986-141">After following these steps, you now have an **HDInsight local cluster** entry in Server Explorer, under hello **HDInsight** section.</span></span>

## <a name="write-a-hive-query"></a><span data-ttu-id="c1986-142">Zápis dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="c1986-142">Write a Hive query</span></span>

<span data-ttu-id="c1986-143">Hive poskytuje podobné jazyku SQL dotazovací jazyk (HiveQL) pro práci s strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="c1986-143">Hive provides a SQL-like query language (HiveQL) for working with structured data.</span></span> <span data-ttu-id="c1986-144">Pomocí následujících kroků toolearn jak toorun na vyžádání dotazuje na místní cluster hello hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-144">Use hello following steps toolearn how toorun on-demand queries against hello local cluster.</span></span>

1. <span data-ttu-id="c1986-145">V **Průzkumníka serveru**, klikněte pravým tlačítkem na položku hello pro hello místní cluster, který jste přidali dříve a pak vyberte **napsat dotaz Hive**.</span><span class="sxs-lookup"><span data-stu-id="c1986-145">In **Server Explorer**, right-click hello entry for hello local cluster that you added previously, and then select **Write a Hive Query**.</span></span>

    ![Snímek obrazovky z Průzkumníku serveru při zápisu zvýrazněná dotaz Hive](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    <span data-ttu-id="c1986-147">Zobrazí se nové okno dotazu.</span><span class="sxs-lookup"><span data-stu-id="c1986-147">A new query window appears.</span></span> <span data-ttu-id="c1986-148">Zde můžete rychle zapisování a odesílání dotazů toohello místní cluster.</span><span class="sxs-lookup"><span data-stu-id="c1986-148">Here you can quickly write and submit a query toohello local cluster.</span></span>

2. <span data-ttu-id="c1986-149">V novém okně dotazu hello zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c1986-149">In hello new query window, enter hello following command:</span></span>

        select count(*) from sample_08;

    <span data-ttu-id="c1986-150">toorun hello dotaz, vyberte **odeslání** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-150">toorun hello query, select **Submit** at hello top of hello window.</span></span> <span data-ttu-id="c1986-151">Nechte hello ostatní hodnoty (**Batch** a název serveru) v hello výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c1986-151">Leave hello other values (**Batch** and server name) at hello default values.</span></span>

    ![Snímek obrazovky okna dotazu s tlačítkem odeslání hello](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    <span data-ttu-id="c1986-153">Můžete také použít hello rozevírací nabídce vedle příliš**odeslání** tooselect **Upřesnit**.</span><span class="sxs-lookup"><span data-stu-id="c1986-153">You can also use hello drop-down menu next too**Submit** tooselect **Advanced**.</span></span> <span data-ttu-id="c1986-154">Rozšířené možnosti povolit další možnosti tooprovide při odesílání úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-154">Advanced options allow you tooprovide additional options when you submit hello job.</span></span>

    ![Dialogové okno snímek obrazovky odeslat skript](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. <span data-ttu-id="c1986-156">Po odeslání hello dotazu, zobrazí se stav úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-156">After you submit hello query, hello job status appears.</span></span> <span data-ttu-id="c1986-157">Stav úlohy Hello zobrazí informace o hello úlohy, jako je zpracován Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c1986-157">hello job status displays information about hello job as it is processed by Hadoop.</span></span> <span data-ttu-id="c1986-158">**Stav úlohy** poskytuje hello stav úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-158">**Job State** provides hello status of hello job.</span></span> <span data-ttu-id="c1986-159">Stav Hello je pravidelně aktualizována, nebo můžete použít hello obnovit ikonu toorefresh hello stav ručně.</span><span class="sxs-lookup"><span data-stu-id="c1986-159">hello state is updated periodically, or you can use hello refresh icon toorefresh hello state manually.</span></span>

    ![Snímek obrazovky zobrazení úloh dialogové okno, se zvýrazněnou stav úlohy](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    <span data-ttu-id="c1986-161">Po hello **stav úlohy** změní příliš**dokončeno**, zobrazí se přesměruje Acyklické grafu (DAG).</span><span class="sxs-lookup"><span data-stu-id="c1986-161">After hello **Job State** changes too**Finished**, a Directed Acyclic Graph (DAG) is displayed.</span></span> <span data-ttu-id="c1986-162">Tento diagram znázorňuje hello provádění cestu, která byla dáno Tez při zpracování dotazu Hive hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-162">This diagram describes hello execution path that was determined by Tez when processing hello Hive query.</span></span> <span data-ttu-id="c1986-163">Tez je hello výchozí spouštěcí modul pro Hive na místní cluster hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-163">Tez is hello default execution engine for Hive on hello local cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c1986-164">Tez je také výchozí hello, pokud používáte clustery HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="c1986-164">Tez is also hello default when you are using Linux-based HDInsight clusters.</span></span> <span data-ttu-id="c1986-165">Není výchozí hello na HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="c1986-165">It is not hello default on Windows-based HDInsight.</span></span> <span data-ttu-id="c1986-166">toouse ho existuje, je nutné přidat hello řádku `set hive.execution.engine = tez;` toohello začátku dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="c1986-166">toouse it there, you must add hello line `set hive.execution.engine = tez;` toohello beginning of your Hive query.</span></span>

    <span data-ttu-id="c1986-167">Použití hello **výstup úlohy** odkaz tooview hello výstup.</span><span class="sxs-lookup"><span data-stu-id="c1986-167">Use hello **Job Output** link tooview hello output.</span></span> <span data-ttu-id="c1986-168">V takovém případě je 823 hello počet řádků v tabulce sample_08 hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-168">In this case, it is 823, hello number of rows in hello sample_08 table.</span></span> <span data-ttu-id="c1986-169">Diagnostické informace o úloze hello můžete zobrazit pomocí hello **protokol úlohy** a **stáhnout protokolu YARN** odkazy.</span><span class="sxs-lookup"><span data-stu-id="c1986-169">You can view diagnostics information about hello job by using hello **Job Log** and **Download YARN Log** links.</span></span>

4. <span data-ttu-id="c1986-170">Můžete také spouštět úlohy Hive interaktivně změnou hello **Batch** pole příliš**interaktivní**.</span><span class="sxs-lookup"><span data-stu-id="c1986-170">You can also run Hive jobs interactively by changing hello **Batch** field too**Interactive**.</span></span> <span data-ttu-id="c1986-171">Potom vyberte **Execute**.</span><span class="sxs-lookup"><span data-stu-id="c1986-171">Then select **Execute**.</span></span>

    ![Snímek obrazovky interaktivní i pro spouštění tlačítka zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    <span data-ttu-id="c1986-173">Interaktivní dotaz datové proudy hello výstup protokolu vygenerovaných během zpracování toohello **HiveServer2 výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="c1986-173">An interactive query streams hello output log generated during processing toohello **HiveServer2 Output** window.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c1986-174">Hello informace je hello stejné, která je dostupná na hello **protokol úlohy** odkaz po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="c1986-174">hello information is hello same that is available from hello **Job Log** link after a job has finished.</span></span>

    ![Snímek obrazovky výstupu protokolu](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a><span data-ttu-id="c1986-176">Vytvoření projektu Hive</span><span class="sxs-lookup"><span data-stu-id="c1986-176">Create a Hive project</span></span>

<span data-ttu-id="c1986-177">Můžete také vytvořit projekt, který obsahuje několik skriptů Hive.</span><span class="sxs-lookup"><span data-stu-id="c1986-177">You can also create a project that contains multiple Hive scripts.</span></span> <span data-ttu-id="c1986-178">Projekt použijte, pokud mají související skripty nebo má toostore skripty v systému správy verzí.</span><span class="sxs-lookup"><span data-stu-id="c1986-178">Use a project when you have related scripts or want toostore scripts in a version control system.</span></span>

1. <span data-ttu-id="c1986-179">V sadě Visual Studio, vyberte **soubor**, **nový**a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="c1986-179">In Visual Studio, select **File**, **New**, and then **Project**.</span></span>

2. <span data-ttu-id="c1986-180">Ze seznamu hello projektů, rozbalte položku **šablony**, rozbalte položku **Azure Data Lake**a potom vyberte **HIVE (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="c1986-180">From hello list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **HIVE (HDInsight)**.</span></span> <span data-ttu-id="c1986-181">Hello seznam šablon, vyberte **Hive ukázka**.</span><span class="sxs-lookup"><span data-stu-id="c1986-181">From hello list of templates, select **Hive Sample**.</span></span> <span data-ttu-id="c1986-182">Zadejte název a umístění a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1986-182">Enter a name and location, and then select **OK**.</span></span>

    ![Zvýrazněná okno snímek obrazovky nový projekt, s Azure Data Lake, HIVE, Hive ukázka a OK](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

<span data-ttu-id="c1986-184">Hello **Hive ukázka** projekt obsahuje dva skripty **WebLogAnalysis.hql** a **SensorDataAnalysis.hql**.</span><span class="sxs-lookup"><span data-stu-id="c1986-184">hello **Hive Sample** project contains two scripts, **WebLogAnalysis.hql** and **SensorDataAnalysis.hql**.</span></span> <span data-ttu-id="c1986-185">Můžete odeslat tyto skripty pomocí hello stejné **odeslání** tlačítko hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-185">You can submit these scripts by using hello same **Submit** button at hello top of hello window.</span></span>

## <a name="create-a-pig-project"></a><span data-ttu-id="c1986-186">Vytvořit projekt Pig</span><span class="sxs-lookup"><span data-stu-id="c1986-186">Create a Pig project</span></span>

<span data-ttu-id="c1986-187">Zatímco Hive poskytuje podobné jazyku SQL jazyk pro práci s strukturovaných dat, Pig funguje tak, že provádění transformací na data.</span><span class="sxs-lookup"><span data-stu-id="c1986-187">While Hive provides a SQL-like language for working with structured data, Pig works by performing transformations on data.</span></span> <span data-ttu-id="c1986-188">Pig poskytuje jazyk (Pig Latin), který vám umožní toodevelop kanálu transformací.</span><span class="sxs-lookup"><span data-stu-id="c1986-188">Pig provides a language (Pig Latin) that allows you toodevelop a pipeline of transformations.</span></span> <span data-ttu-id="c1986-189">toouse Pig s hello místní cluster, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="c1986-189">toouse Pig with hello local cluster, follow these steps:</span></span>

1. <span data-ttu-id="c1986-190">Otevřete Visual Studio a vyberte **soubor**, **nový**a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="c1986-190">Open Visual Studio, and select **File**, **New**, and then **Project**.</span></span> <span data-ttu-id="c1986-191">Ze seznamu hello projektů, rozbalte položku **šablony**, rozbalte položku **Azure Data Lake**a potom vyberte **Pig (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="c1986-191">From hello list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **Pig (HDInsight)**.</span></span> <span data-ttu-id="c1986-192">Hello seznam šablon, vyberte **Pig aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c1986-192">From hello list of templates, select **Pig Application**.</span></span> <span data-ttu-id="c1986-193">Zadejte název, umístění a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1986-193">Enter a name, location, and then select **OK**.</span></span>

    ![Zvýrazněná okno snímek obrazovky nový projekt, s Azure Data Lake, Pig, Pig aplikace a OK](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. <span data-ttu-id="c1986-195">Zadejte následující text jako hello obsah hello hello **script.pig** soubor, který byl vytvořen tento projekt.</span><span class="sxs-lookup"><span data-stu-id="c1986-195">Enter hello following text as hello contents of hello **script.pig** file that was created with this project.</span></span>

        a = LOAD '/demo/data/Website/Website-Logs' AS (
            log_id:int,
            ip_address:chararray,
            date:chararray,
            time:chararray,
            landing_page:chararray,
            source:chararray);
        b = FILTER a BY (log_id > 100);
        c = GROUP b BY ip_address;
        DUMP c;

    <span data-ttu-id="c1986-196">Při Pig používá jiný jazyk než Hive, jak spouštět úlohy hello je konzistentní mezi oba jazyky, prostřednictvím hello **odeslání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c1986-196">While Pig uses a different language than Hive, how you run hello jobs is consistent between both languages, through hello **Submit** button.</span></span> <span data-ttu-id="c1986-197">Výběr hello rozevíracího seznamu vedle položky **odeslání** zobrazí dialogové okno rozšířených pro Pig.</span><span class="sxs-lookup"><span data-stu-id="c1986-197">Selecting hello drop-down beside **Submit** displays an advanced submit dialog box for Pig.</span></span>

    ![Dialogové okno snímek obrazovky odeslat skript](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. <span data-ttu-id="c1986-199">Stav úlohy Hello a výstup se také zobrazí, hello stejné jako dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="c1986-199">hello job status and output is also displayed, hello same as a Hive query.</span></span>

    ![Snímek obrazovky dokončené úlohy Pig](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a><span data-ttu-id="c1986-201">Zobrazit úlohy</span><span class="sxs-lookup"><span data-stu-id="c1986-201">View jobs</span></span>

<span data-ttu-id="c1986-202">Nástroje data Lake také umožní tooeasily zobrazit informace o úlohách, které byly spuštěny v systému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c1986-202">Data Lake tools also allow you tooeasily view information about jobs that have been run on Hadoop.</span></span> <span data-ttu-id="c1986-203">Pomocí následujících kroků toosee hello úlohy, které byly spuštěny na místní cluster hello hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-203">Use hello following steps toosee hello jobs that have been run on hello local cluster.</span></span>

1. <span data-ttu-id="c1986-204">Z **Průzkumníka serveru**, klikněte pravým tlačítkem na místní cluster hello a pak vyberte **zobrazit úlohy**.</span><span class="sxs-lookup"><span data-stu-id="c1986-204">From **Server Explorer**, right-click hello local cluster, and then select **View Jobs**.</span></span> <span data-ttu-id="c1986-205">Zobrazí se seznam úloh, které byly odeslaná toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="c1986-205">A list of jobs that have been submitted toohello cluster is displayed.</span></span>

    ![Snímek obrazovky Server Explorer, se zvýrazněnou zobrazit úlohy](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. <span data-ttu-id="c1986-207">Hello seznam úloh vyberte jednu podrobnosti úlohy tooview hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-207">From hello list of jobs, select one tooview hello job details.</span></span>

    ![Snímek obrazovky úlohy prohlížeče, s jedním z úlohy hello zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    <span data-ttu-id="c1986-209">zobrazené informace jsou Hello je podobné toowhat, které se zobrazí po spuštění dotazu Hive nebo Pig, včetně informací odkazy tooview hello výstup a protokolu.</span><span class="sxs-lookup"><span data-stu-id="c1986-209">hello information displayed is similar toowhat you see after running a Hive or Pig query, including links tooview hello output and log information.</span></span>

3. <span data-ttu-id="c1986-210">Můžete také upravit a znovu odeslat úlohu hello z tohoto umístění.</span><span class="sxs-lookup"><span data-stu-id="c1986-210">You can also modify and resubmit hello job from here.</span></span>

## <a name="view-hive-databases"></a><span data-ttu-id="c1986-211">Zobrazení Hive databáze</span><span class="sxs-lookup"><span data-stu-id="c1986-211">View Hive databases</span></span>

1. <span data-ttu-id="c1986-212">V **Průzkumníka serveru**, rozbalte položku hello **místní cluster HDInsight** položku a potom rozbalte **databáze Hive**.</span><span class="sxs-lookup"><span data-stu-id="c1986-212">In **Server Explorer**, expand hello **HDInsight local cluster** entry, and then expand **Hive Databases**.</span></span> <span data-ttu-id="c1986-213">Hello **výchozí** a **xademo** databáze na místní cluster hello jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="c1986-213">hello **Default** and **xademo** databases on hello local cluster are displayed.</span></span> <span data-ttu-id="c1986-214">Rozšíření databáze zobrazuje hello tabulky v rámci hello databáze.</span><span class="sxs-lookup"><span data-stu-id="c1986-214">Expanding a database shows hello tables within hello database.</span></span>

    ![Snímek obrazovky z Průzkumníka serveru s databází rozšířit](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. <span data-ttu-id="c1986-216">Rozbalení tabulku zobrazí hello sloupce pro tuto tabulku.</span><span class="sxs-lookup"><span data-stu-id="c1986-216">Expanding a table displays hello columns for that table.</span></span> <span data-ttu-id="c1986-217">data hello tooquickly zobrazení, klikněte pravým tlačítkem na tabulku a vyberte **zobrazit prvních 100 řádků**.</span><span class="sxs-lookup"><span data-stu-id="c1986-217">tooquickly view hello data, right-click a table, and select **View Top 100 Rows**.</span></span>

    ![Snímek obrazovky z Průzkumníku serveru s Tabulka rozbalit a zobrazit prvních 100 řádků vybrané](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a><span data-ttu-id="c1986-219">Vlastnosti databáze a tabulky</span><span class="sxs-lookup"><span data-stu-id="c1986-219">Database and table properties</span></span>

<span data-ttu-id="c1986-220">Můžete zobrazit vlastnosti hello databázi nebo tabulku.</span><span class="sxs-lookup"><span data-stu-id="c1986-220">You can view hello properties of a database or table.</span></span> <span data-ttu-id="c1986-221">Výběr **vlastnosti** zobrazuje podrobnosti o hello vybrané položky v okně Vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="c1986-221">Selecting **Properties** displays details for hello selected item in hello properties window.</span></span> <span data-ttu-id="c1986-222">Například najdete hello informace zobrazené v hello následující snímek obrazovky:</span><span class="sxs-lookup"><span data-stu-id="c1986-222">For example, see hello information shown in hello following screenshot:</span></span>

![Snímek obrazovky Vlastnosti – okno](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a><span data-ttu-id="c1986-224">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="c1986-224">Create a table</span></span>

<span data-ttu-id="c1986-225">toocreate tabulku, klikněte pravým tlačítkem na databázi a potom vyberte **Create Table**.</span><span class="sxs-lookup"><span data-stu-id="c1986-225">toocreate a table, right-click a database, and then select **Create Table**.</span></span>

![Snímek obrazovky Server Explorer, se zvýrazněnou Create Table](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

<span data-ttu-id="c1986-227">Potom můžete vytvořit hello tabulky pomocí formuláře.</span><span class="sxs-lookup"><span data-stu-id="c1986-227">You can then create hello table using a form.</span></span> <span data-ttu-id="c1986-228">Dole hello hello následující snímek obrazovky, uvidíte hello nezpracovaná HiveQL, který je použité toocreate hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="c1986-228">At hello bottom of hello following screenshot, you can see hello raw HiveQL that is used toocreate hello table.</span></span>

![Snímek obrazovky formuláře hello používá toocreate tabulku](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a><span data-ttu-id="c1986-230">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c1986-230">Next steps</span></span>

* [<span data-ttu-id="c1986-231">Learning hello LAN Dobrý den Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="c1986-231">Learning hello ropes of hello Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="c1986-232">Hadoop kurz – Začínáme s HDP</span><span class="sxs-lookup"><span data-stu-id="c1986-232">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
