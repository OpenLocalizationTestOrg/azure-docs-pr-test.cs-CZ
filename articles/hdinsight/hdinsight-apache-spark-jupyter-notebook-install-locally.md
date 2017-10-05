---
title: "Instalace Jupyter místně a připojení ke clusteru Azure HDInsight Spark | Microsoft Docs"
description: "Naučte se nainstalovat Poznámkový blok Jupyter místně na váš počítač a připojte ho k cluster Apache Spark v Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48593bdf-4122-4f2e-a8ec-fdc009e47c16
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: fe9dcdb643aa6a8ee5d55738b7a446e4b0153986
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a><span data-ttu-id="40465-103">Do počítače nainstalovat Poznámkový blok Jupyter a připojte se k Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="40465-103">Install Jupyter notebook on your computer and connect to Apache Spark on HDInsight</span></span>

<span data-ttu-id="40465-104">V tomto článku a zjistěte, jak nainstalovat Poznámkový blok Jupyter, s vlastní PySpark (pro jazyk Python) a jádra Spark (pro Scala) s Spark magic a připojení ke clusteru HDInsight poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="40465-104">In this article you learn how to install Jupyter notebook, with the custom PySpark (for Python) and Spark (for Scala) kernels with Spark magic, and connect the notebook to an HDInsight cluster.</span></span> <span data-ttu-id="40465-105">Může být několik důvodů, proč nainstalovat Jupyter v místním počítači a může být také některé běžné problémy.</span><span class="sxs-lookup"><span data-stu-id="40465-105">There can be a number of reasons to install Jupyter on your local computer, and there can be some challenges as well.</span></span> <span data-ttu-id="40465-106">Další informace o to, najdete v části [Proč v mém počítači nainstalujte Jupyter](#why-should-i-install-jupyter-on-my-computer) na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="40465-106">For more on this, see the section [Why should I install Jupyter on my computer](#why-should-i-install-jupyter-on-my-computer) at the end of this article.</span></span>

<span data-ttu-id="40465-107">Účastnící se instalace Jupyter a Spark magic v počítači jsou tři klíčové kroky.</span><span class="sxs-lookup"><span data-stu-id="40465-107">There are three key steps involved in installing Jupyter and the Spark magic on your computer.</span></span>

* <span data-ttu-id="40465-108">Nainstalujte poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="40465-108">Install Jupyter notebook</span></span>
* <span data-ttu-id="40465-109">Instalace jádra PySpark a Spark pomocí Spark magic</span><span class="sxs-lookup"><span data-stu-id="40465-109">Install the PySpark and Spark kernels with the Spark magic</span></span>
* <span data-ttu-id="40465-110">Konfigurace magic Spark pro přístup ke clusteru Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="40465-110">Configure Spark magic to access Spark cluster on HDInsight</span></span>

<span data-ttu-id="40465-111">Další informace o vlastních jádrech a k dispozici pro poznámkové bloky Jupyter s clusterem HDInsight Spark magic najdete v tématu [jádra dostupná pro poznámkové bloky Jupyter s Apache Spark Linux clusterů v HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="40465-111">For more information about the custom kernels and the Spark magic available for Jupyter notebooks with HDInsight cluster, see [Kernels available for Jupyter notebooks with Apache Spark Linux clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40465-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="40465-112">Prerequisites</span></span>
<span data-ttu-id="40465-113">Nejsou zde uvedené předpoklady pro instalaci Jupyter.</span><span class="sxs-lookup"><span data-stu-id="40465-113">The prerequisites listed here are not for installing Jupyter.</span></span> <span data-ttu-id="40465-114">Jedná se o připojení poznámkového bloku Jupyter do clusteru HDInsight, po instalaci poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="40465-114">These are for connecting the Jupyter notebook to an HDInsight cluster once the notebook is installed.</span></span>

* <span data-ttu-id="40465-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="40465-115">An Azure subscription.</span></span> <span data-ttu-id="40465-116">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="40465-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="40465-117">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="40465-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="40465-118">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="40465-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="install-jupyter-notebook-on-your-computer"></a><span data-ttu-id="40465-119">Do počítače nainstalovat poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="40465-119">Install Jupyter notebook on your computer</span></span>

<span data-ttu-id="40465-120">Python je třeba nainstalovat před instalací poznámkové bloky Jupyter.</span><span class="sxs-lookup"><span data-stu-id="40465-120">You  must install Python before you can install Jupyter notebooks.</span></span> <span data-ttu-id="40465-121">Python a Jupyter jsou k dispozici jako součást [Anaconda distribuční](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="40465-121">Both Python and Jupyter are available as part of the [Anaconda distribution](https://www.continuum.io/downloads).</span></span> <span data-ttu-id="40465-122">Při instalaci Anaconda instalujete distribuční jazyka Python.</span><span class="sxs-lookup"><span data-stu-id="40465-122">When you install Anaconda, you install a distribution of Python.</span></span> <span data-ttu-id="40465-123">Po instalaci Anaconda přidáte Jupyter instalaci spuštěním příslušnými příkazy.</span><span class="sxs-lookup"><span data-stu-id="40465-123">Once Anaconda is installed, you add the Jupyter installation by running appropriate commands.</span></span>

1. <span data-ttu-id="40465-124">Stažení [instalační program Anaconda](https://www.continuum.io/downloads) pro vaše platformy a spusťte instalaci.</span><span class="sxs-lookup"><span data-stu-id="40465-124">Download the [Anaconda installer](https://www.continuum.io/downloads) for your platform and run the setup.</span></span> <span data-ttu-id="40465-125">Při spuštění Průvodce instalací, ujistěte se, že vyberete možnost Přidat Anaconda do vaše proměnná PATH.</span><span class="sxs-lookup"><span data-stu-id="40465-125">While running the setup wizard, make sure you select the option to add Anaconda to your PATH variable.</span></span>
2. <span data-ttu-id="40465-126">Spusťte následující příkaz pro instalaci Jupyter.</span><span class="sxs-lookup"><span data-stu-id="40465-126">Run the following command to install Jupyter.</span></span>

        conda install jupyter

    <span data-ttu-id="40465-127">Další informace o instalaci Jupyter najdete v tématu [Jupyter instalaci pomocí Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span><span class="sxs-lookup"><span data-stu-id="40465-127">For more information on installing Jupyter, see [Installing Jupyter using Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span></span>

## <a name="install-the-kernels-and-spark-magic"></a><span data-ttu-id="40465-128">Instalace jádra a Spark magic</span><span class="sxs-lookup"><span data-stu-id="40465-128">Install the kernels and Spark magic</span></span>

<span data-ttu-id="40465-129">Pokyny k instalaci Spark magic jádra PySpark a Spark, postupujte podle pokynů instalace [sparkmagic dokumentace](https://github.com/jupyter-incubator/sparkmagic#installation) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="40465-129">For instructions on how to install the Spark magic, the PySpark and Spark kernels, follow the installation instructions in the [sparkmagic documentation](https://github.com/jupyter-incubator/sparkmagic#installation) on GitHub.</span></span> <span data-ttu-id="40465-130">Prvním krokem v dokumentaci magic Spark se žádostí o instalaci Spark magic.</span><span class="sxs-lookup"><span data-stu-id="40465-130">The first step in the Spark magic documentation asks you to install Spark magic.</span></span> <span data-ttu-id="40465-131">Nahraďte tento první krok v odkazu pomocí následujících příkazů, v závislosti na verzi clusteru HDInsight se připojí k.</span><span class="sxs-lookup"><span data-stu-id="40465-131">Replace that first step in the link with the following commands, depending on the version of the HDInsight cluster you will connect to.</span></span> <span data-ttu-id="40465-132">Potom postupujte podle zbývajících kroků v dokumentaci magic Spark.</span><span class="sxs-lookup"><span data-stu-id="40465-132">After that, follow the remaining steps in the Spark magic documentation.</span></span> <span data-ttu-id="40465-133">Pokud chcete nainstalovat jádrech jiné, je třeba provést krok 3 v části Spark magic instalační pokyny.</span><span class="sxs-lookup"><span data-stu-id="40465-133">If you want to install the different kernels, you must perform Step 3 in the Spark magic installation instructions section.</span></span>

* <span data-ttu-id="40465-134">Pro clustery v3.4 nainstalujte sparkmagic 0.2.3 spuštěním`pip install sparkmagic==0.2.3`</span><span class="sxs-lookup"><span data-stu-id="40465-134">For clusters v3.4, install sparkmagic 0.2.3 by executing `pip install sparkmagic==0.2.3`</span></span>

* <span data-ttu-id="40465-135">Pro clustery v3.5 a v3.6 nainstalujte sparkmagic 0.11.2 spuštěním`pip install sparkmagic==0.11.2`</span><span class="sxs-lookup"><span data-stu-id="40465-135">For clusters v3.5 and v3.6, install sparkmagic 0.11.2 by executing `pip install sparkmagic==0.11.2`</span></span>

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a><span data-ttu-id="40465-136">Konfigurace magic Spark se připojit ke clusteru HDInsight Spark</span><span class="sxs-lookup"><span data-stu-id="40465-136">Configure Spark magic to connect to HDInsight Spark cluster</span></span>

<span data-ttu-id="40465-137">V této části nakonfigurujete magic Spark, který jste dříve nainstalovali k připojení ke clusteru Apache Spark, musí již jste vytvořili v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="40465-137">In this section you configure the Spark magic that you installed earlier to connect to an Apache Spark cluster that you must have already created in Azure HDInsight.</span></span>

1. <span data-ttu-id="40465-138">Informace o konfiguraci Jupyter je obvykle uložen v domovském adresáři uživatele.</span><span class="sxs-lookup"><span data-stu-id="40465-138">The Jupyter configuration information is typically stored in the users home directory.</span></span> <span data-ttu-id="40465-139">Chcete-li vyhledat domovského adresáře na jakékoli platformě operačního systému, zadejte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="40465-139">To locate your home directory on any OS platform, type the following commands.</span></span>

    <span data-ttu-id="40465-140">Spusťte prostředí Python.</span><span class="sxs-lookup"><span data-stu-id="40465-140">Start the Python shell.</span></span> <span data-ttu-id="40465-141">V okně příkazového řádku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="40465-141">On a command window, type the following:</span></span>

        python

    <span data-ttu-id="40465-142">V prostředí Python zadejte následující příkaz a zjistěte, k domovskému adresáři.</span><span class="sxs-lookup"><span data-stu-id="40465-142">On the Python shell, enter the following command to find out the home directory.</span></span>

        import os
        print(os.path.expanduser('~'))

2. <span data-ttu-id="40465-143">Přejděte do domovského adresáře a vytvořte složku s názvem **.sparkmagic** Pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="40465-143">Navigate to the home directory and create a folder called **.sparkmagic** if it does not already exist.</span></span>
3. <span data-ttu-id="40465-144">Ve složce, vytvořte soubor s názvem **config.json** a přidejte následující fragment kódu JSON je uvnitř.</span><span class="sxs-lookup"><span data-stu-id="40465-144">Within the folder, create a file called **config.json** and add the following JSON snippet inside it.</span></span>

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. <span data-ttu-id="40465-145">SUBSTITUTE **{USERNAME}**, **{CLUSTERDNSNAME}**, a **{BASE64ENCODEDPASSWORD}** s příslušnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="40465-145">Substitute **{USERNAME}**, **{CLUSTERDNSNAME}**, and **{BASE64ENCODEDPASSWORD}** with appropriate values.</span></span> <span data-ttu-id="40465-146">Můžete použít několik nástrojů programovacího jazyka oblíbených nebo online ke generování hesla kódováním base64 pro vlastní heslo.</span><span class="sxs-lookup"><span data-stu-id="40465-146">You can use a number of utilities in your favorite programming language or online to generate a base64 encoded password for your actual password.</span></span>

5. <span data-ttu-id="40465-147">Nakonfigurovat správné nastavení prezenčního signálu v `config.json`.</span><span class="sxs-lookup"><span data-stu-id="40465-147">Configure the right Heartbeat settings in `config.json`.</span></span> <span data-ttu-id="40465-148">Měli byste přidat tato nastavení na stejné úrovni jako `kernel_python_credentials` a `kernel_scala_credentials` fragmenty vaší přidaných výše.</span><span class="sxs-lookup"><span data-stu-id="40465-148">You should add these settings at the same level as the `kernel_python_credentials` and `kernel_scala_credentials` snippets your added earlier.</span></span> <span data-ttu-id="40465-149">Příklad jak a kde se mají přidat nastavení prezenčního signálu, najdete [ukázka config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span><span class="sxs-lookup"><span data-stu-id="40465-149">For an example on how and where to add the heartbeat settings, see this [sample config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span></span>

    * <span data-ttu-id="40465-150">Pro `sparkmagic 0.2.3` (clusterů v3.4), zahrnují:</span><span class="sxs-lookup"><span data-stu-id="40465-150">For `sparkmagic 0.2.3` (clusters v3.4), include:</span></span>

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * <span data-ttu-id="40465-151">Pro `sparkmagic 0.11.2` (v3.5 clustery a v3.6), zahrnují:</span><span class="sxs-lookup"><span data-stu-id="40465-151">For `sparkmagic 0.11.2` (clusters v3.5 and v3.6), include:</span></span>

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    ><span data-ttu-id="40465-152">Ujistěte se, že nejsou neuniknou relací jsou odesílány prezenčních signálů.</span><span class="sxs-lookup"><span data-stu-id="40465-152">Heartbeats are sent to ensure that sessions are not leaked.</span></span> <span data-ttu-id="40465-153">Když počítač přejde do režimu spánku nebo je vypnutý, není odeslat prezenční signál, výsledkem relace se vyčistit.</span><span class="sxs-lookup"><span data-stu-id="40465-153">When a computer goes to sleep or is shut down, the heartbeat is not sent, resulting in the session being cleaned up.</span></span> <span data-ttu-id="40465-154">Pro clustery v3.4, pokud chcete zakázat toto chování můžete nastavit konfigurace Livy `livy.server.interactive.heartbeat.timeout` k `0` z uživatelského rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="40465-154">For clusters v3.4, if you wish to disable this behavior, you can set the Livy config `livy.server.interactive.heartbeat.timeout` to `0` from the Ambari UI.</span></span> <span data-ttu-id="40465-155">Pro clustery v3.5 Pokud není nastaveno výše, 3.5 Konfigurace relace nebudou odstraněna.</span><span class="sxs-lookup"><span data-stu-id="40465-155">For clusters v3.5, if you do not set the 3.5 configuration above, the session will not be deleted.</span></span>

6. <span data-ttu-id="40465-156">Spusťte Jupyter.</span><span class="sxs-lookup"><span data-stu-id="40465-156">Start Jupyter.</span></span> <span data-ttu-id="40465-157">Použijte následující příkaz z příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="40465-157">Use the following command from the command prompt.</span></span>

        jupyter notebook

7. <span data-ttu-id="40465-158">Ověřte, zda se můžete připojit ke clusteru pomocí poznámkového bloku Jupyter a, které můžete použít k dispozici Spark magic s jádrech.</span><span class="sxs-lookup"><span data-stu-id="40465-158">Verify that you can connect to the cluster using the Jupyter notebook and that you can use the Spark magic available with the kernels.</span></span> <span data-ttu-id="40465-159">Proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="40465-159">Perform the following steps.</span></span>

    <span data-ttu-id="40465-160">a.</span><span class="sxs-lookup"><span data-stu-id="40465-160">a.</span></span> <span data-ttu-id="40465-161">Vytvořte nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="40465-161">Create a new notebook.</span></span> <span data-ttu-id="40465-162">V pravém rohu, klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="40465-162">From the right-hand corner, click **New**.</span></span> <span data-ttu-id="40465-163">Měli byste vidět jádra výchozí **Python2** a dvě nové jádra, které nainstalujete, **PySpark** a **Spark**.</span><span class="sxs-lookup"><span data-stu-id="40465-163">You should see the default kernel **Python2** and the two new kernels that you install, **PySpark** and **Spark**.</span></span> <span data-ttu-id="40465-164">Klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="40465-164">Click **PySpark**.</span></span>

    <span data-ttu-id="40465-165">![Jádra v Poznámkový blok Jupyter](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "jádra v poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="40465-165">![Kernels in Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Kernels in Jupyter notebook")</span></span>

    <span data-ttu-id="40465-166">b.</span><span class="sxs-lookup"><span data-stu-id="40465-166">b.</span></span> <span data-ttu-id="40465-167">Spusťte následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="40465-167">Run the following code snippet.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    <span data-ttu-id="40465-168">Pokud se můžete úspěšně načíst výstup, je otestovat připojení ke clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="40465-168">If you can successfully retrieve the output, your connection to the HDInsight cluster is tested.</span></span>

    >[!TIP]
    ><span data-ttu-id="40465-169">Pokud chcete aktualizovat konfiguraci Poznámkový blok pro připojení do jiného clusteru, aktualizujte config.json nové sady hodnot, jak je znázorněno v kroku 3 výše.</span><span class="sxs-lookup"><span data-stu-id="40465-169">If you want to update the notebook configuration to connect to a different cluster, update the config.json with the new set of values, as shown in Step 3 above.</span></span>

## <a name="why-should-i-install-jupyter-on-my-computer"></a><span data-ttu-id="40465-170">Proč nainstalovat Jupyter v mém počítači?</span><span class="sxs-lookup"><span data-stu-id="40465-170">Why should I install Jupyter on my computer?</span></span>
<span data-ttu-id="40465-171">Může být z mnoha důvodů, proč můžete chtít nainstalovat Jupyter na váš počítač a připojte ho k cluster Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="40465-171">There can be a number of reasons why you might want to install Jupyter on your computer and then connect it to a Spark cluster on HDInsight.</span></span>

* <span data-ttu-id="40465-172">I když poznámkové bloky Jupyter jsou již k dispozici pro cluster Spark v Azure HDInsight, instalace Jupyter ve vašem počítači získáte možnost vytvořit poznámkové bloky místně, aplikaci otestovat s spuštěného clusteru a potom odeslat poznámkové bloky do clusteru.</span><span class="sxs-lookup"><span data-stu-id="40465-172">Even though Jupyter notebooks are already available on the Spark cluster in Azure HDInsight, installing Jupyter on your computer provides you the option to create your notebooks locally, test your application against a running cluster, and then upload the notebooks to the cluster.</span></span> <span data-ttu-id="40465-173">Pokud chcete nahrát poznámkových bloků do clusteru, můžete buď odešlete pomocí poznámkového bloku Jupyter, který běží nebo clusteru nebo uložíte do složky /HdiNotebooks v účtu úložiště, který je přidružen ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="40465-173">To upload the notebooks to the cluster, you can either upload them using the Jupyter notebook that is running or the cluster, or save them to the /HdiNotebooks folder in the storage account associated with the cluster.</span></span> <span data-ttu-id="40465-174">Další informace o tom, jak jsou poznámkových bloků uložené v clusteru najdete v tématu [úložiště poznámkové bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span><span class="sxs-lookup"><span data-stu-id="40465-174">For more information on how notebooks are stored on the cluster, see [Where are Jupyter notebooks stored](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span></span>
* <span data-ttu-id="40465-175">Díky poznámkových bloků, která je k dispozici místně, můžete připojit k jiné clustery Spark podle požadavků vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="40465-175">With the notebooks available locally, you can connect to different Spark clusters based on your application requirement.</span></span>
* <span data-ttu-id="40465-176">GitHub můžete použít k implementaci systému správy zdrojů a mají Správa verzí pro poznámkových bloků.</span><span class="sxs-lookup"><span data-stu-id="40465-176">You can use GitHub to implement a source control system and have version control for the notebooks.</span></span> <span data-ttu-id="40465-177">Můžete taky nechat spolupráce prostředí, kde můžete pracovat více uživatelů se stejným poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="40465-177">You can also have a collaborative environment where multiple users can work with the same notebook.</span></span>
* <span data-ttu-id="40465-178">Můžete pracovat s poznámkových bloků místně bez nutnosti i cluster s podporou.</span><span class="sxs-lookup"><span data-stu-id="40465-178">You can work with notebooks locally without even having a cluster up.</span></span> <span data-ttu-id="40465-179">Potřebujete jenom clusteru k testování poznámkové bloky proti, nechcete spravovat ručně poznámkové bloky nebo vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="40465-179">You only need a cluster to test your notebooks against, not to manually manage your notebooks or a development environment.</span></span>
* <span data-ttu-id="40465-180">Může být snadněji nakonfigurovat vlastní místní vývojové prostředí, než je konfigurace instalace Jupyter v clusteru.</span><span class="sxs-lookup"><span data-stu-id="40465-180">It may be easier to configure your own local development environment than it is to configure the Jupyter installation on the cluster.</span></span>  <span data-ttu-id="40465-181">Můžete využít výhod všech software, který jste nainstalovali místně bez konfigurace jeden nebo více vzdálené clusterů.</span><span class="sxs-lookup"><span data-stu-id="40465-181">You can take advantage of all the software you have installed locally without configuring one or more remote clusters.</span></span>

> [!WARNING]
> <span data-ttu-id="40465-182">S Jupyter nainstalovaná v místním počítači můžete více uživatelů ve stejném clusteru Spark spustit stejné poznámkového bloku, ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="40465-182">With Jupyter installed on your local computer, multiple users can run the same notebook on the same Spark cluster at the same time.</span></span> <span data-ttu-id="40465-183">V takovém případě se vytvoří více Livy relací.</span><span class="sxs-lookup"><span data-stu-id="40465-183">In such a situation, multiple Livy sessions are created.</span></span> <span data-ttu-id="40465-184">Pokud narazíte na problém a chcete ladit, bude, že složité úlohy sledování relace které Livy patří do které uživatel.</span><span class="sxs-lookup"><span data-stu-id="40465-184">If you run into an issue and want to debug that, it will be a complex task to track which Livy session belongs to which user.</span></span>
>
>

## <span data-ttu-id="40465-185"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="40465-185"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="40465-186">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="40465-186">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="40465-187">Scénáře</span><span class="sxs-lookup"><span data-stu-id="40465-187">Scenarios</span></span>
* [<span data-ttu-id="40465-188">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="40465-188">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="40465-189">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="40465-189">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="40465-190">Spark s Machine Learning: Používejte Spark v HDInsight k předpovědím výsledků kontrol potravin</span><span class="sxs-lookup"><span data-stu-id="40465-190">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="40465-191">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="40465-191">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="40465-192">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="40465-192">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="40465-193">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="40465-193">Create and run applications</span></span>
* [<span data-ttu-id="40465-194">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="40465-194">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="40465-195">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="40465-195">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="40465-196">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="40465-196">Tools and extensions</span></span>
* [<span data-ttu-id="40465-197">Modul plug-in nástroje HDInsight pro IntelliJ IDEA pro vytvoření a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="40465-197">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="40465-198">Použití modulu plug-in nástroje HDInsight pro IntelliJ IDEA pro vzdálené ladění aplikací Spark</span><span class="sxs-lookup"><span data-stu-id="40465-198">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="40465-199">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="40465-199">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="40465-200">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="40465-200">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="40465-201">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="40465-201">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a><span data-ttu-id="40465-202">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="40465-202">Manage resources</span></span>
* [<span data-ttu-id="40465-203">Správa prostředků v clusteru Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="40465-203">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="40465-204">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="40465-204">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
