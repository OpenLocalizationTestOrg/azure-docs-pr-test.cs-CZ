---
title: "Nástroje data Lake pro Visual Studio s Hortonworks karanténě - Azure HDInsight | Microsoft Docs"
description: "Další informace o použití nástroje Azure Data Lake pro Visual Studio s Hortonworks izolovaný spuštěné v místní virtuální počítač. Pomocí těchto nástrojů můžete vytvořit a spustit úlohy Hive a Pig na izolovaného prostoru a zobrazit výstup úlohy a historie."
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
ms.openlocfilehash: 574ccaa8b2d9448a60ddf8adc7f92fa3683b1d61
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-the-azure-data-lake-tools-for-visual-studio-with-the-hortonworks-sandbox"></a><span data-ttu-id="e0870-104">Pomocí nástroje Azure Data Lake pro Visual Studio s Hortonworks karanténě</span><span class="sxs-lookup"><span data-stu-id="e0870-104">Use the Azure Data Lake tools for Visual Studio with the Hortonworks Sandbox</span></span>

<span data-ttu-id="e0870-105">Azure Data Lake obsahuje nástroje pro práci s obecné clusterů systému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="e0870-105">Azure Data Lake includes tools for working with generic Hadoop clusters.</span></span> <span data-ttu-id="e0870-106">Tento dokument obsahuje kroky potřebné k použití nástrojů Data Lake s Hortonworks karanténě běží na místním virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e0870-106">This document provides the steps needed to use the Data Lake tools with the Hortonworks Sandbox running in a local virtual machine.</span></span>

<span data-ttu-id="e0870-107">Pomocí Hortonworks karanténě umožňuje pracovat s Hadoop místně na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="e0870-107">Using the Hortonworks Sandbox allows you to work with Hadoop locally on your development environment.</span></span> <span data-ttu-id="e0870-108">Poté, co jste si vytvořili řešení a chcete nasadit ve velkém měřítku, můžete pak přesunout do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e0870-108">After you have developed a solution and want to deploy it at scale, you can then move to an HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0870-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e0870-109">Prerequisites</span></span>

* <span data-ttu-id="e0870-110">Hortonworks karanténě spuštěných ve virtuálním počítači na vývojového prostředí.</span><span class="sxs-lookup"><span data-stu-id="e0870-110">The Hortonworks Sandbox, running in a virtual machine on your development environment.</span></span> <span data-ttu-id="e0870-111">Tento dokument byl zapsán a testovány s izolovaného prostoru spuštěné v Oracle VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="e0870-111">This document was written and tested with the sandbox running in Oracle VirtualBox.</span></span> <span data-ttu-id="e0870-112">Informace o nastavení izolovaný prostor najdete v tématu [začít pracovat s Hortonworks karanténě.](hdinsight-hadoop-emulator-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="e0870-112">For information on setting up the sandbox, see the [Get started with the Hortonworks sandbox.](hdinsight-hadoop-emulator-get-started.md)</span></span> <span data-ttu-id="e0870-113">dokument.</span><span class="sxs-lookup"><span data-stu-id="e0870-113">document.</span></span>

* <span data-ttu-id="e0870-114">Visual Studio 2013, Visual Studio 2015 nebo Visual Studio 2017 (všechny edice).</span><span class="sxs-lookup"><span data-stu-id="e0870-114">Visual Studio 2013, Visual Studio 2015, or Visual Studio 2017 (any edition).</span></span>

* <span data-ttu-id="e0870-115">[Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="e0870-115">The [Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1 or later.</span></span>

* <span data-ttu-id="e0870-116">[Nástroje Azure Data Lake pro Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="e0870-116">The [Azure Data Lake tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

## <a name="configure-passwords-for-the-sandbox"></a><span data-ttu-id="e0870-117">Konfigurace hesla pro izolovaný prostor</span><span class="sxs-lookup"><span data-stu-id="e0870-117">Configure passwords for the sandbox</span></span>

<span data-ttu-id="e0870-118">Ujistěte se, zda je spuštěna Hortonworks karanténě.</span><span class="sxs-lookup"><span data-stu-id="e0870-118">Make sure that the Hortonworks Sandbox is running.</span></span> <span data-ttu-id="e0870-119">Potom postupujte podle kroků v [Začínáme v Hortonworks karanténě](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e0870-119">Then follow the steps in the [Get started in the Hortonworks Sandbox](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) document.</span></span> <span data-ttu-id="e0870-120">Tyto kroky konfigurace heslo SSH `root` účet a Ambari `admin` účtu.</span><span class="sxs-lookup"><span data-stu-id="e0870-120">These steps configure the password for the SSH `root` account, and the Ambari `admin` account.</span></span> <span data-ttu-id="e0870-121">Tato hesla se používají při připojování k izolovanému prostoru ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0870-121">These passwords are used when you connect to the sandbox from Visual Studio.</span></span>

## <a name="connect-the-tools-to-the-sandbox"></a><span data-ttu-id="e0870-122">Nástroje pro připojení k izolovanému prostoru</span><span class="sxs-lookup"><span data-stu-id="e0870-122">Connect the tools to the sandbox</span></span>

1. <span data-ttu-id="e0870-123">Otevřete Visual Studio, vyberte **zobrazení**a potom vyberte **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="e0870-123">Open Visual Studio, select **View**, and then select **Server Explorer**.</span></span>

2. <span data-ttu-id="e0870-124">Z **Průzkumníka serveru**, klikněte pravým tlačítkem myši **HDInsight** položku a potom vyberte **připojit k emulátoru HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="e0870-124">From **Server Explorer**, right-click the **HDInsight** entry, and then select **Connect to HDInsight Emulator**.</span></span>

    ![Snímek obrazovky Server Explorer, se připojit k emulátoru HDInsight zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. <span data-ttu-id="e0870-126">Z **připojit k emulátoru HDInsight** dialogové okno pole, zadejte heslo, které jste nakonfigurovali pro Ambari.</span><span class="sxs-lookup"><span data-stu-id="e0870-126">From the **Connect to HDInsight Emulator** dialog box, enter the password that you configured for Ambari.</span></span>

    ![Snímek obrazovky dialogového okna, se zvýrazněnou textového pole hesla](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    <span data-ttu-id="e0870-128">Vyberte **Další** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="e0870-128">Select **Next** to continue.</span></span>

4. <span data-ttu-id="e0870-129">Použití **heslo** pole k zadání hesla, které jste nakonfigurovali pro `root` účtu.</span><span class="sxs-lookup"><span data-stu-id="e0870-129">Use the **Password** field to enter the password you configured for the `root` account.</span></span> <span data-ttu-id="e0870-130">Nechte ostatní pole na výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e0870-130">Leave the other fields at the default value.</span></span>

    ![Snímek obrazovky dialogového okna, se zvýrazněnou textového pole hesla](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    <span data-ttu-id="e0870-132">Vyberte **Další** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="e0870-132">Select **Next** to continue.</span></span>

5. <span data-ttu-id="e0870-133">Počkejte, než pro ověření služby ukončíte.</span><span class="sxs-lookup"><span data-stu-id="e0870-133">Wait for validation of the services to finish.</span></span> <span data-ttu-id="e0870-134">V některých případech může ověření nezdaří a zobrazí výzvu k aktualizaci konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e0870-134">In some cases, validation may fail and prompt you to update the configuration.</span></span> <span data-ttu-id="e0870-135">Pokud ověření selže, vyberte **aktualizace**a počkejte, konfigurace a ověření pro službu, kterou chcete dokončit.</span><span class="sxs-lookup"><span data-stu-id="e0870-135">If validation fails, select **Update**, and wait for the configuration and verification for the service to finish.</span></span>

    ![Snímek obrazovky dialogového okna, pomocí tlačítka Aktualizovat zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > <span data-ttu-id="e0870-137">Proces aktualizace používá Ambari ke změně konfigurace Hortonworks karanténě k očekávané pomocí nástroje Data Lake pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0870-137">The update process uses Ambari to modify the Hortonworks Sandbox configuration to what is expected by the Data Lake tools for Visual Studio.</span></span>

6. <span data-ttu-id="e0870-138">Po dokončení ověření vyberte **Dokončit** a dokončete konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="e0870-138">After validation has finished, select **Finish** to complete configuration.</span></span>
    <span data-ttu-id="e0870-139">![Snímek obrazovky dialogového okna, s tlačítkem Dokončit](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span><span class="sxs-lookup"><span data-stu-id="e0870-139">![Screenshot of dialog box, with Finish button highlighted](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span></span>

     >[!NOTE]
     > <span data-ttu-id="e0870-140">V závislosti na rychlosti vašeho vývojového prostředí a množství paměti přidělené virtuálnímu počítači se může trvat několik minut, konfigurace a ověřte, že services.</span><span class="sxs-lookup"><span data-stu-id="e0870-140">Depending on the speed of your development environment, and the amount of memory allocated to the virtual machine, it can take several minutes to configure and validate the services.</span></span>

<span data-ttu-id="e0870-141">Po následujícím postupem, nyní máte **místní cluster HDInsight** položky v Průzkumníku serveru pod **HDInsight** části.</span><span class="sxs-lookup"><span data-stu-id="e0870-141">After following these steps, you now have an **HDInsight local cluster** entry in Server Explorer, under the **HDInsight** section.</span></span>

## <a name="write-a-hive-query"></a><span data-ttu-id="e0870-142">Zápis dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="e0870-142">Write a Hive query</span></span>

<span data-ttu-id="e0870-143">Hive poskytuje podobné jazyku SQL dotazovací jazyk (HiveQL) pro práci s strukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="e0870-143">Hive provides a SQL-like query language (HiveQL) for working with structured data.</span></span> <span data-ttu-id="e0870-144">Použijte následující postup se dozvíte, jak ke spouštění dotazů na vyžádání na místním clusteru.</span><span class="sxs-lookup"><span data-stu-id="e0870-144">Use the following steps to learn how to run on-demand queries against the local cluster.</span></span>

1. <span data-ttu-id="e0870-145">V **Průzkumníka serveru**, pravým tlačítkem na položku pro místní cluster, který jste přidali dříve a pak vyberte **napsat dotaz Hive**.</span><span class="sxs-lookup"><span data-stu-id="e0870-145">In **Server Explorer**, right-click the entry for the local cluster that you added previously, and then select **Write a Hive Query**.</span></span>

    ![Snímek obrazovky z Průzkumníku serveru při zápisu zvýrazněná dotaz Hive](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    <span data-ttu-id="e0870-147">Zobrazí se nové okno dotazu.</span><span class="sxs-lookup"><span data-stu-id="e0870-147">A new query window appears.</span></span> <span data-ttu-id="e0870-148">Zde můžete rychle zapisování a odesílání dotazů na místní cluster.</span><span class="sxs-lookup"><span data-stu-id="e0870-148">Here you can quickly write and submit a query to the local cluster.</span></span>

2. <span data-ttu-id="e0870-149">V novém okně dotazů zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e0870-149">In the new query window, enter the following command:</span></span>

        select count(*) from sample_08;

    <span data-ttu-id="e0870-150">Pokud chcete spustit dotaz, vyberte **odeslání** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="e0870-150">To run the query, select **Submit** at the top of the window.</span></span> <span data-ttu-id="e0870-151">Nechte ostatní hodnoty (**Batch** a název serveru) na výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e0870-151">Leave the other values (**Batch** and server name) at the default values.</span></span>

    ![Snímek obrazovky okna dotazu se zvýrazněnou tlačítko pro odeslání](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    <span data-ttu-id="e0870-153">Můžete také použít v rozevírací nabídce vedle položky **odeslání** vyberte **Upřesnit**.</span><span class="sxs-lookup"><span data-stu-id="e0870-153">You can also use the drop-down menu next to **Submit** to select **Advanced**.</span></span> <span data-ttu-id="e0870-154">Rozšířené možnosti umožňují poskytují další možnosti při odeslání úlohy.</span><span class="sxs-lookup"><span data-stu-id="e0870-154">Advanced options allow you to provide additional options when you submit the job.</span></span>

    ![Dialogové okno snímek obrazovky odeslat skript](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. <span data-ttu-id="e0870-156">Po odeslání dotazu, zobrazí se stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="e0870-156">After you submit the query, the job status appears.</span></span> <span data-ttu-id="e0870-157">Stav úlohy zobrazí informace o úloze, jako je zpracován Hadoop.</span><span class="sxs-lookup"><span data-stu-id="e0870-157">The job status displays information about the job as it is processed by Hadoop.</span></span> <span data-ttu-id="e0870-158">**Stav úlohy** obsahuje informace o stavu úlohy.</span><span class="sxs-lookup"><span data-stu-id="e0870-158">**Job State** provides the status of the job.</span></span> <span data-ttu-id="e0870-159">Stav se aktualizuje pravidelně nebo na ikonu aktualizace můžete ručně obnovit stav.</span><span class="sxs-lookup"><span data-stu-id="e0870-159">The state is updated periodically, or you can use the refresh icon to refresh the state manually.</span></span>

    ![Snímek obrazovky zobrazení úloh dialogové okno, se zvýrazněnou stav úlohy](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    <span data-ttu-id="e0870-161">Po **stav úlohy** změny **dokončeno**, zobrazí se přesměruje Acyklické grafu (DAG).</span><span class="sxs-lookup"><span data-stu-id="e0870-161">After the **Job State** changes to **Finished**, a Directed Acyclic Graph (DAG) is displayed.</span></span> <span data-ttu-id="e0870-162">Tento diagram znázorňuje provádění cestu, která byla dáno Tez při zpracování dotazu Hive.</span><span class="sxs-lookup"><span data-stu-id="e0870-162">This diagram describes the execution path that was determined by Tez when processing the Hive query.</span></span> <span data-ttu-id="e0870-163">Tez je výchozí modul provádění pro Hive na místním clusteru.</span><span class="sxs-lookup"><span data-stu-id="e0870-163">Tez is the default execution engine for Hive on the local cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e0870-164">Tez je také výchozí, pokud používáte clustery HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="e0870-164">Tez is also the default when you are using Linux-based HDInsight clusters.</span></span> <span data-ttu-id="e0870-165">Není výchozí na HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="e0870-165">It is not the default on Windows-based HDInsight.</span></span> <span data-ttu-id="e0870-166">Pro použití existuje, je nutné přidat na řádku `set hive.execution.engine = tez;` na začátek dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="e0870-166">To use it there, you must add the line `set hive.execution.engine = tez;` to the beginning of your Hive query.</span></span>

    <span data-ttu-id="e0870-167">Použití **výstup úlohy** odkaz k zobrazení výstupu.</span><span class="sxs-lookup"><span data-stu-id="e0870-167">Use the **Job Output** link to view the output.</span></span> <span data-ttu-id="e0870-168">V takovém případě je 823, počet řádků v tabulce sample_08.</span><span class="sxs-lookup"><span data-stu-id="e0870-168">In this case, it is 823, the number of rows in the sample_08 table.</span></span> <span data-ttu-id="e0870-169">Diagnostické informace o úloze můžete zobrazit pomocí **protokol úlohy** a **stáhnout protokolu YARN** odkazy.</span><span class="sxs-lookup"><span data-stu-id="e0870-169">You can view diagnostics information about the job by using the **Job Log** and **Download YARN Log** links.</span></span>

4. <span data-ttu-id="e0870-170">Můžete také spouštět úlohy Hive interaktivně změnou **Batch** do **interaktivní**.</span><span class="sxs-lookup"><span data-stu-id="e0870-170">You can also run Hive jobs interactively by changing the **Batch** field to **Interactive**.</span></span> <span data-ttu-id="e0870-171">Potom vyberte **Execute**.</span><span class="sxs-lookup"><span data-stu-id="e0870-171">Then select **Execute**.</span></span>

    ![Snímek obrazovky interaktivní i pro spouštění tlačítka zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    <span data-ttu-id="e0870-173">Interaktivní dotaz datové proudy výstup protokolu vygenerovaných během zpracování **HiveServer2 výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="e0870-173">An interactive query streams the output log generated during processing to the **HiveServer2 Output** window.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e0870-174">Informace jsou stejné, která je dostupná na **protokol úlohy** odkaz po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="e0870-174">The information is the same that is available from the **Job Log** link after a job has finished.</span></span>

    ![Snímek obrazovky výstupu protokolu](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a><span data-ttu-id="e0870-176">Vytvoření projektu Hive</span><span class="sxs-lookup"><span data-stu-id="e0870-176">Create a Hive project</span></span>

<span data-ttu-id="e0870-177">Můžete také vytvořit projekt, který obsahuje několik skriptů Hive.</span><span class="sxs-lookup"><span data-stu-id="e0870-177">You can also create a project that contains multiple Hive scripts.</span></span> <span data-ttu-id="e0870-178">Projekt použijte, pokud mají související skripty nebo chcete uložit skripty v systému správy verzí.</span><span class="sxs-lookup"><span data-stu-id="e0870-178">Use a project when you have related scripts or want to store scripts in a version control system.</span></span>

1. <span data-ttu-id="e0870-179">V sadě Visual Studio, vyberte **soubor**, **nový**a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e0870-179">In Visual Studio, select **File**, **New**, and then **Project**.</span></span>

2. <span data-ttu-id="e0870-180">Ze seznamu projekty, rozbalte položku **šablony**, rozbalte položku **Azure Data Lake**a potom vyberte **HIVE (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="e0870-180">From the list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **HIVE (HDInsight)**.</span></span> <span data-ttu-id="e0870-181">V seznamu šablon vyberte **Hive ukázka**.</span><span class="sxs-lookup"><span data-stu-id="e0870-181">From the list of templates, select **Hive Sample**.</span></span> <span data-ttu-id="e0870-182">Zadejte název a umístění a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0870-182">Enter a name and location, and then select **OK**.</span></span>

    ![Zvýrazněná okno snímek obrazovky nový projekt, s Azure Data Lake, HIVE, Hive ukázka a OK](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

<span data-ttu-id="e0870-184">**Hive ukázka** projekt obsahuje dva skripty **WebLogAnalysis.hql** a **SensorDataAnalysis.hql**.</span><span class="sxs-lookup"><span data-stu-id="e0870-184">The **Hive Sample** project contains two scripts, **WebLogAnalysis.hql** and **SensorDataAnalysis.hql**.</span></span> <span data-ttu-id="e0870-185">Tyto skripty můžete odeslat pomocí stejné **odeslání** tlačítka v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="e0870-185">You can submit these scripts by using the same **Submit** button at the top of the window.</span></span>

## <a name="create-a-pig-project"></a><span data-ttu-id="e0870-186">Vytvořit projekt Pig</span><span class="sxs-lookup"><span data-stu-id="e0870-186">Create a Pig project</span></span>

<span data-ttu-id="e0870-187">Zatímco Hive poskytuje podobné jazyku SQL jazyk pro práci s strukturovaných dat, Pig funguje tak, že provádění transformací na data.</span><span class="sxs-lookup"><span data-stu-id="e0870-187">While Hive provides a SQL-like language for working with structured data, Pig works by performing transformations on data.</span></span> <span data-ttu-id="e0870-188">Pig poskytuje jazyk (Pig Latin), který umožňuje vyvíjet kanálu transformací.</span><span class="sxs-lookup"><span data-stu-id="e0870-188">Pig provides a language (Pig Latin) that allows you to develop a pipeline of transformations.</span></span> <span data-ttu-id="e0870-189">Pig s místním clusteru, postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="e0870-189">To use Pig with the local cluster, follow these steps:</span></span>

1. <span data-ttu-id="e0870-190">Otevřete Visual Studio a vyberte **soubor**, **nový**a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e0870-190">Open Visual Studio, and select **File**, **New**, and then **Project**.</span></span> <span data-ttu-id="e0870-191">Ze seznamu projekty, rozbalte položku **šablony**, rozbalte položku **Azure Data Lake**a potom vyberte **Pig (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="e0870-191">From the list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **Pig (HDInsight)**.</span></span> <span data-ttu-id="e0870-192">V seznamu šablon vyberte **Pig aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e0870-192">From the list of templates, select **Pig Application**.</span></span> <span data-ttu-id="e0870-193">Zadejte název, umístění a potom vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0870-193">Enter a name, location, and then select **OK**.</span></span>

    ![Zvýrazněná okno snímek obrazovky nový projekt, s Azure Data Lake, Pig, Pig aplikace a OK](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. <span data-ttu-id="e0870-195">Zadejte následující text jako obsah **script.pig** soubor, který byl vytvořen tento projekt.</span><span class="sxs-lookup"><span data-stu-id="e0870-195">Enter the following text as the contents of the **script.pig** file that was created with this project.</span></span>

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

    <span data-ttu-id="e0870-196">Při Pig používá jiný jazyk než Hive, jak spouštět úlohy je konzistentní mezi oba jazyky prostřednictvím **odeslání** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e0870-196">While Pig uses a different language than Hive, how you run the jobs is consistent between both languages, through the **Submit** button.</span></span> <span data-ttu-id="e0870-197">Výběr rozevíracího seznamu vedle položky **odeslání** zobrazí dialogové okno rozšířených pro Pig.</span><span class="sxs-lookup"><span data-stu-id="e0870-197">Selecting the drop-down beside **Submit** displays an advanced submit dialog box for Pig.</span></span>

    ![Dialogové okno snímek obrazovky odeslat skript](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. <span data-ttu-id="e0870-199">Také se zobrazí stav úlohy a výstup, stejně jako dotaz Hive.</span><span class="sxs-lookup"><span data-stu-id="e0870-199">The job status and output is also displayed, the same as a Hive query.</span></span>

    ![Snímek obrazovky dokončené úlohy Pig](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a><span data-ttu-id="e0870-201">Zobrazit úlohy</span><span class="sxs-lookup"><span data-stu-id="e0870-201">View jobs</span></span>

<span data-ttu-id="e0870-202">Nástroje data Lake taky umožňují snadno zobrazit informace o úlohách, které byly spuštěny v systému Hadoop.</span><span class="sxs-lookup"><span data-stu-id="e0870-202">Data Lake tools also allow you to easily view information about jobs that have been run on Hadoop.</span></span> <span data-ttu-id="e0870-203">Pomocí následujících kroků můžete zobrazit úlohy, které byly spuštěny v místním clusteru.</span><span class="sxs-lookup"><span data-stu-id="e0870-203">Use the following steps to see the jobs that have been run on the local cluster.</span></span>

1. <span data-ttu-id="e0870-204">Z **Průzkumníka serveru**, klikněte pravým tlačítkem na místní cluster a potom vyberte **zobrazit úlohy**.</span><span class="sxs-lookup"><span data-stu-id="e0870-204">From **Server Explorer**, right-click the local cluster, and then select **View Jobs**.</span></span> <span data-ttu-id="e0870-205">Zobrazí se seznam úloh, které byly odeslány do clusteru.</span><span class="sxs-lookup"><span data-stu-id="e0870-205">A list of jobs that have been submitted to the cluster is displayed.</span></span>

    ![Snímek obrazovky Server Explorer, se zvýrazněnou zobrazit úlohy](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. <span data-ttu-id="e0870-207">Seznam úloh vyberte jednu zobrazíte podrobnosti úlohy.</span><span class="sxs-lookup"><span data-stu-id="e0870-207">From the list of jobs, select one to view the job details.</span></span>

    ![Snímek obrazovky úlohy prohlížeče, s jedním z úlohy zvýrazněná](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    <span data-ttu-id="e0870-209">Informace zobrazené je podobná co se zobrazí po spuštění dotazu Hive nebo Pig, včetně odkazů na Zobrazit výstup a protokolovat informace.</span><span class="sxs-lookup"><span data-stu-id="e0870-209">The information displayed is similar to what you see after running a Hive or Pig query, including links to view the output and log information.</span></span>

3. <span data-ttu-id="e0870-210">Můžete také upravit a znovu odeslat úlohu z tohoto umístění.</span><span class="sxs-lookup"><span data-stu-id="e0870-210">You can also modify and resubmit the job from here.</span></span>

## <a name="view-hive-databases"></a><span data-ttu-id="e0870-211">Zobrazení Hive databáze</span><span class="sxs-lookup"><span data-stu-id="e0870-211">View Hive databases</span></span>

1. <span data-ttu-id="e0870-212">V **Průzkumníka serveru**, rozbalte **místní cluster HDInsight** položku a potom rozbalte **databáze Hive**.</span><span class="sxs-lookup"><span data-stu-id="e0870-212">In **Server Explorer**, expand the **HDInsight local cluster** entry, and then expand **Hive Databases**.</span></span> <span data-ttu-id="e0870-213">**Výchozí** a **xademo** se zobrazí databáze v místním clusteru.</span><span class="sxs-lookup"><span data-stu-id="e0870-213">The **Default** and **xademo** databases on the local cluster are displayed.</span></span> <span data-ttu-id="e0870-214">Rozšíření databáze zobrazuje tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="e0870-214">Expanding a database shows the tables within the database.</span></span>

    ![Snímek obrazovky z Průzkumníka serveru s databází rozšířit](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. <span data-ttu-id="e0870-216">Rozšiřování tabulka obsahuje sloupec pro tuto tabulku.</span><span class="sxs-lookup"><span data-stu-id="e0870-216">Expanding a table displays the columns for that table.</span></span> <span data-ttu-id="e0870-217">Pokud chcete rychle zobrazit data, klikněte pravým tlačítkem na tabulku a vyberte **zobrazit prvních 100 řádků**.</span><span class="sxs-lookup"><span data-stu-id="e0870-217">To quickly view the data, right-click a table, and select **View Top 100 Rows**.</span></span>

    ![Snímek obrazovky z Průzkumníku serveru s Tabulka rozbalit a zobrazit prvních 100 řádků vybrané](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a><span data-ttu-id="e0870-219">Vlastnosti databáze a tabulky</span><span class="sxs-lookup"><span data-stu-id="e0870-219">Database and table properties</span></span>

<span data-ttu-id="e0870-220">Můžete zobrazit vlastnosti databázi nebo tabulku.</span><span class="sxs-lookup"><span data-stu-id="e0870-220">You can view the properties of a database or table.</span></span> <span data-ttu-id="e0870-221">Výběr **vlastnosti** zobrazí podrobnosti pro vybranou položku v okně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e0870-221">Selecting **Properties** displays details for the selected item in the properties window.</span></span> <span data-ttu-id="e0870-222">Například najdete informace zobrazené na následujícím snímku obrazovky:</span><span class="sxs-lookup"><span data-stu-id="e0870-222">For example, see the information shown in the following screenshot:</span></span>

![Snímek obrazovky Vlastnosti – okno](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a><span data-ttu-id="e0870-224">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="e0870-224">Create a table</span></span>

<span data-ttu-id="e0870-225">Chcete-li vytvořit tabulku, klikněte pravým tlačítkem na databázi a pak vyberte **Create Table**.</span><span class="sxs-lookup"><span data-stu-id="e0870-225">To create a table, right-click a database, and then select **Create Table**.</span></span>

![Snímek obrazovky Server Explorer, se zvýrazněnou Create Table](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

<span data-ttu-id="e0870-227">Potom můžete vytvořit v tabulce pomocí formuláře.</span><span class="sxs-lookup"><span data-stu-id="e0870-227">You can then create the table using a form.</span></span> <span data-ttu-id="e0870-228">V dolní části následující snímek obrazovky uvidíte nezpracovaná HiveQL, který se používá k vytvoření tabulky.</span><span class="sxs-lookup"><span data-stu-id="e0870-228">At the bottom of the following screenshot, you can see the raw HiveQL that is used to create the table.</span></span>

![Snímek obrazovky formulář, který slouží k vytvoření tabulky](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a><span data-ttu-id="e0870-230">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0870-230">Next steps</span></span>

* [<span data-ttu-id="e0870-231">Učení LAN Hortonworks izolovaného prostoru</span><span class="sxs-lookup"><span data-stu-id="e0870-231">Learning the ropes of the Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="e0870-232">Hadoop kurz – Začínáme s HDP</span><span class="sxs-lookup"><span data-stu-id="e0870-232">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
