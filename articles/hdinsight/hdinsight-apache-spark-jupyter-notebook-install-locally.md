---
title: "aaaInstall Jupyter místně & připojení clusteru Azure HDInsight Spark tooan | Microsoft Docs"
description: "Zjistěte, jak poznámkového bloku Jupyter tooinstall místně na vašem počítači a připojte ho tooan cluster Apache Spark v Azure HDInsight."
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
ms.openlocfilehash: 95c052110b84b677fd23048597af9511365cacfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-tooapache-spark-on-hdinsight"></a><span data-ttu-id="d380c-103">Do počítače nainstalovat Poznámkový blok Jupyter a připojte tooApache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d380c-103">Install Jupyter notebook on your computer and connect tooApache Spark on HDInsight</span></span>

<span data-ttu-id="d380c-104">V tomto článku zjistíte, jak poznámkového bloku Jupyter tooinstall, s hello vlastní PySpark (pro jazyk Python) a Spark (pro Scala) jader s Spark magic a připojit cluster HDInsight tooan hello poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="d380c-104">In this article you learn how tooinstall Jupyter notebook, with hello custom PySpark (for Python) and Spark (for Scala) kernels with Spark magic, and connect hello notebook tooan HDInsight cluster.</span></span> <span data-ttu-id="d380c-105">Může být několik důvodů tooinstall Jupyter v místním počítači a může být také některé běžné problémy.</span><span class="sxs-lookup"><span data-stu-id="d380c-105">There can be a number of reasons tooinstall Jupyter on your local computer, and there can be some challenges as well.</span></span> <span data-ttu-id="d380c-106">Další informace v této části hello [Proč v mém počítači nainstalujte Jupyter](#why-should-i-install-jupyter-on-my-computer) na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="d380c-106">For more on this, see hello section [Why should I install Jupyter on my computer](#why-should-i-install-jupyter-on-my-computer) at hello end of this article.</span></span>

<span data-ttu-id="d380c-107">Účastnící se instalace Jupyter a hello Spark magic v počítači jsou tři klíčové kroky.</span><span class="sxs-lookup"><span data-stu-id="d380c-107">There are three key steps involved in installing Jupyter and hello Spark magic on your computer.</span></span>

* <span data-ttu-id="d380c-108">Nainstalujte poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="d380c-108">Install Jupyter notebook</span></span>
* <span data-ttu-id="d380c-109">Instalace s hello Spark magic hello PySpark a Spark jádra</span><span class="sxs-lookup"><span data-stu-id="d380c-109">Install hello PySpark and Spark kernels with hello Spark magic</span></span>
* <span data-ttu-id="d380c-110">Konfigurace Spark magic tooaccess Spark cluster v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d380c-110">Configure Spark magic tooaccess Spark cluster on HDInsight</span></span>

<span data-ttu-id="d380c-111">Další informace o hello vlastní jádra a magic Spark hello k dispozici pro poznámkové bloky Jupyter s clusterem HDInsight najdete v tématu [jádra dostupná pro poznámkové bloky Jupyter s Apache Spark Linux clusterů v HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="d380c-111">For more information about hello custom kernels and hello Spark magic available for Jupyter notebooks with HDInsight cluster, see [Kernels available for Jupyter notebooks with Apache Spark Linux clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d380c-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d380c-112">Prerequisites</span></span>
<span data-ttu-id="d380c-113">Zde uvedené požadavky Hello nejsou pro instalaci Jupyter.</span><span class="sxs-lookup"><span data-stu-id="d380c-113">hello prerequisites listed here are not for installing Jupyter.</span></span> <span data-ttu-id="d380c-114">Toto jsou pro připojování hello Jupyter poznámkového bloku tooan HDInsight cluster po instalaci hello Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="d380c-114">These are for connecting hello Jupyter notebook tooan HDInsight cluster once hello notebook is installed.</span></span>

* <span data-ttu-id="d380c-115">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="d380c-115">An Azure subscription.</span></span> <span data-ttu-id="d380c-116">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="d380c-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="d380c-117">Cluster Apache Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d380c-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="d380c-118">Pokyny najdete v tématu [clusterů vytvořit Apache Spark v Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="d380c-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="install-jupyter-notebook-on-your-computer"></a><span data-ttu-id="d380c-119">Do počítače nainstalovat poznámkového bloku Jupyter</span><span class="sxs-lookup"><span data-stu-id="d380c-119">Install Jupyter notebook on your computer</span></span>

<span data-ttu-id="d380c-120">Python je třeba nainstalovat před instalací poznámkové bloky Jupyter.</span><span class="sxs-lookup"><span data-stu-id="d380c-120">You  must install Python before you can install Jupyter notebooks.</span></span> <span data-ttu-id="d380c-121">Python a Jupyter jsou k dispozici jako součást hello [Anaconda distribuční](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="d380c-121">Both Python and Jupyter are available as part of hello [Anaconda distribution](https://www.continuum.io/downloads).</span></span> <span data-ttu-id="d380c-122">Při instalaci Anaconda instalujete distribuční jazyka Python.</span><span class="sxs-lookup"><span data-stu-id="d380c-122">When you install Anaconda, you install a distribution of Python.</span></span> <span data-ttu-id="d380c-123">Po instalaci Anaconda přidáte hello Jupyter instalace spuštěním příslušnými příkazy.</span><span class="sxs-lookup"><span data-stu-id="d380c-123">Once Anaconda is installed, you add hello Jupyter installation by running appropriate commands.</span></span>

1. <span data-ttu-id="d380c-124">Stáhnout hello [instalační program Anaconda](https://www.continuum.io/downloads) pro vaše platformy a instalační program spusťte hello.</span><span class="sxs-lookup"><span data-stu-id="d380c-124">Download hello [Anaconda installer](https://www.continuum.io/downloads) for your platform and run hello setup.</span></span> <span data-ttu-id="d380c-125">Při spuštění Průvodce instalací hello Ujistěte se, že jste vybrali hello možnost tooadd Anaconda tooyour proměnné PATH.</span><span class="sxs-lookup"><span data-stu-id="d380c-125">While running hello setup wizard, make sure you select hello option tooadd Anaconda tooyour PATH variable.</span></span>
2. <span data-ttu-id="d380c-126">Spusťte následující příkaz tooinstall Jupyter hello.</span><span class="sxs-lookup"><span data-stu-id="d380c-126">Run hello following command tooinstall Jupyter.</span></span>

        conda install jupyter

    <span data-ttu-id="d380c-127">Další informace o instalaci Jupyter najdete v tématu [Jupyter instalaci pomocí Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span><span class="sxs-lookup"><span data-stu-id="d380c-127">For more information on installing Jupyter, see [Installing Jupyter using Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).</span></span>

## <a name="install-hello-kernels-and-spark-magic"></a><span data-ttu-id="d380c-128">Instalace jádra hello a Spark magic</span><span class="sxs-lookup"><span data-stu-id="d380c-128">Install hello kernels and Spark magic</span></span>

<span data-ttu-id="d380c-129">Pokyny, jak tooinstall hello Spark magic, hello PySpark a Spark jádra podle pokynů instalace hello v hello [sparkmagic dokumentace](https://github.com/jupyter-incubator/sparkmagic#installation) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="d380c-129">For instructions on how tooinstall hello Spark magic, hello PySpark and Spark kernels, follow hello installation instructions in hello [sparkmagic documentation](https://github.com/jupyter-incubator/sparkmagic#installation) on GitHub.</span></span> <span data-ttu-id="d380c-130">Hello prvním krokem při hello Spark magic dokumentace žádostí tooinstall Spark magic.</span><span class="sxs-lookup"><span data-stu-id="d380c-130">hello first step in hello Spark magic documentation asks you tooinstall Spark magic.</span></span> <span data-ttu-id="d380c-131">Nahraďte hello následující příkazy, v závislosti na verzi hello hello clusteru HDInsight, které se připojí k této prvním krokem při hello odkaz.</span><span class="sxs-lookup"><span data-stu-id="d380c-131">Replace that first step in hello link with hello following commands, depending on hello version of hello HDInsight cluster you will connect to.</span></span> <span data-ttu-id="d380c-132">Potom postupujte podle hello zbývající kroky v dokumentaci magic hello Spark.</span><span class="sxs-lookup"><span data-stu-id="d380c-132">After that, follow hello remaining steps in hello Spark magic documentation.</span></span> <span data-ttu-id="d380c-133">Pokud chcete tooinstall hello různých jádra, je nutné provést v hello Spark magic instalační pokyny části kroku 3.</span><span class="sxs-lookup"><span data-stu-id="d380c-133">If you want tooinstall hello different kernels, you must perform Step 3 in hello Spark magic installation instructions section.</span></span>

* <span data-ttu-id="d380c-134">Pro clustery v3.4 nainstalujte sparkmagic 0.2.3 spuštěním`pip install sparkmagic==0.2.3`</span><span class="sxs-lookup"><span data-stu-id="d380c-134">For clusters v3.4, install sparkmagic 0.2.3 by executing `pip install sparkmagic==0.2.3`</span></span>

* <span data-ttu-id="d380c-135">Pro clustery v3.5 a v3.6 nainstalujte sparkmagic 0.11.2 spuštěním`pip install sparkmagic==0.11.2`</span><span class="sxs-lookup"><span data-stu-id="d380c-135">For clusters v3.5 and v3.6, install sparkmagic 0.11.2 by executing `pip install sparkmagic==0.11.2`</span></span>

## <a name="configure-spark-magic-tooconnect-toohdinsight-spark-cluster"></a><span data-ttu-id="d380c-136">Konfigurace clusteru Spark tooHDInsight magic tooconnect Spark</span><span class="sxs-lookup"><span data-stu-id="d380c-136">Configure Spark magic tooconnect tooHDInsight Spark cluster</span></span>

<span data-ttu-id="d380c-137">V této části nakonfigurujete magic hello Spark, který jste nainstalovali starší cluster Apache Spark tooan tooconnect, který musí již jste vytvořili v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d380c-137">In this section you configure hello Spark magic that you installed earlier tooconnect tooan Apache Spark cluster that you must have already created in Azure HDInsight.</span></span>

1. <span data-ttu-id="d380c-138">informace o konfiguraci Jupyter Hello je obvykle uložen ve hello uživatelé domovský adresář.</span><span class="sxs-lookup"><span data-stu-id="d380c-138">hello Jupyter configuration information is typically stored in hello users home directory.</span></span> <span data-ttu-id="d380c-139">toolocate domovského adresáře na jakékoli platformě operačního systému, typ hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="d380c-139">toolocate your home directory on any OS platform, type hello following commands.</span></span>

    <span data-ttu-id="d380c-140">Spusťte prostředí Python hello.</span><span class="sxs-lookup"><span data-stu-id="d380c-140">Start hello Python shell.</span></span> <span data-ttu-id="d380c-141">V okně příkazového řádku zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="d380c-141">On a command window, type hello following:</span></span>

        python

    <span data-ttu-id="d380c-142">Na hello prostředí Python zadejte následující příkaz toofind na domovský adresář hello hello.</span><span class="sxs-lookup"><span data-stu-id="d380c-142">On hello Python shell, enter hello following command toofind out hello home directory.</span></span>

        import os
        print(os.path.expanduser('~'))

2. <span data-ttu-id="d380c-143">Přejděte toohello domovský adresář a vytvořte složku s názvem **.sparkmagic** Pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="d380c-143">Navigate toohello home directory and create a folder called **.sparkmagic** if it does not already exist.</span></span>
3. <span data-ttu-id="d380c-144">Ve složce hello, vytvořte soubor s názvem **config.json** a přidejte následující fragment kódu JSON je uvnitř hello.</span><span class="sxs-lookup"><span data-stu-id="d380c-144">Within hello folder, create a file called **config.json** and add hello following JSON snippet inside it.</span></span>

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

4. <span data-ttu-id="d380c-145">SUBSTITUTE **{USERNAME}**, **{CLUSTERDNSNAME}**, a **{BASE64ENCODEDPASSWORD}** s příslušnými hodnotami.</span><span class="sxs-lookup"><span data-stu-id="d380c-145">Substitute **{USERNAME}**, **{CLUSTERDNSNAME}**, and **{BASE64ENCODEDPASSWORD}** with appropriate values.</span></span> <span data-ttu-id="d380c-146">Můžete použít několik nástrojů v oblíbených programovací jazyk nebo online toogenerate s kódováním base64 zakódované heslo pro váš vlastní heslo.</span><span class="sxs-lookup"><span data-stu-id="d380c-146">You can use a number of utilities in your favorite programming language or online toogenerate a base64 encoded password for your actual password.</span></span>

5. <span data-ttu-id="d380c-147">Konfigurace hello správné nastavení prezenčního signálu v `config.json`.</span><span class="sxs-lookup"><span data-stu-id="d380c-147">Configure hello right Heartbeat settings in `config.json`.</span></span> <span data-ttu-id="d380c-148">Měli byste přidat tato nastavení na stejné úrovni jako hello hello `kernel_python_credentials` a `kernel_scala_credentials` fragmenty vaší přidaných výše.</span><span class="sxs-lookup"><span data-stu-id="d380c-148">You should add these settings at hello same level as hello `kernel_python_credentials` and `kernel_scala_credentials` snippets your added earlier.</span></span> <span data-ttu-id="d380c-149">Příklad na tom, jak a kde tooadd hello nastavení prezenčního signálu najdete [ukázka config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span><span class="sxs-lookup"><span data-stu-id="d380c-149">For an example on how and where tooadd hello heartbeat settings, see this [sample config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).</span></span>

    * <span data-ttu-id="d380c-150">Pro `sparkmagic 0.2.3` (clusterů v3.4), zahrnují:</span><span class="sxs-lookup"><span data-stu-id="d380c-150">For `sparkmagic 0.2.3` (clusters v3.4), include:</span></span>

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * <span data-ttu-id="d380c-151">Pro `sparkmagic 0.11.2` (v3.5 clustery a v3.6), zahrnují:</span><span class="sxs-lookup"><span data-stu-id="d380c-151">For `sparkmagic 0.11.2` (clusters v3.5 and v3.6), include:</span></span>

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    ><span data-ttu-id="d380c-152">Prezenční signály odešlou tooensure nejsou úniku relací.</span><span class="sxs-lookup"><span data-stu-id="d380c-152">Heartbeats are sent tooensure that sessions are not leaked.</span></span> <span data-ttu-id="d380c-153">Pokud počítač přejde toosleep nebo je vypnutý, není odeslán hello prezenčního signálu, výsledkem hello relace se vyčistit.</span><span class="sxs-lookup"><span data-stu-id="d380c-153">When a computer goes toosleep or is shut down, hello heartbeat is not sent, resulting in hello session being cleaned up.</span></span> <span data-ttu-id="d380c-154">Pro clustery v3.4, pokud chcete toodisable toto chování můžete nastavit hello Livy konfigurace `livy.server.interactive.heartbeat.timeout` příliš`0` z hello uživatelského rozhraní Ambari.</span><span class="sxs-lookup"><span data-stu-id="d380c-154">For clusters v3.4, if you wish toodisable this behavior, you can set hello Livy config `livy.server.interactive.heartbeat.timeout` too`0` from hello Ambari UI.</span></span> <span data-ttu-id="d380c-155">Pro clustery v3.5 Pokud není nastavena konfigurace hello 3.5 výše, hello relace nebudou odstraněna.</span><span class="sxs-lookup"><span data-stu-id="d380c-155">For clusters v3.5, if you do not set hello 3.5 configuration above, hello session will not be deleted.</span></span>

6. <span data-ttu-id="d380c-156">Spusťte Jupyter.</span><span class="sxs-lookup"><span data-stu-id="d380c-156">Start Jupyter.</span></span> <span data-ttu-id="d380c-157">Použijte následující příkaz z příkazového řádku hello hello.</span><span class="sxs-lookup"><span data-stu-id="d380c-157">Use hello following command from hello command prompt.</span></span>

        jupyter notebook

7. <span data-ttu-id="d380c-158">Ověřte, zda se můžete připojit toohello clusteru pomocí poznámkového bloku Jupyter hello a, které můžete použít k dispozici magic Spark hello s hello jádra.</span><span class="sxs-lookup"><span data-stu-id="d380c-158">Verify that you can connect toohello cluster using hello Jupyter notebook and that you can use hello Spark magic available with hello kernels.</span></span> <span data-ttu-id="d380c-159">Proveďte následující kroky hello.</span><span class="sxs-lookup"><span data-stu-id="d380c-159">Perform hello following steps.</span></span>

    <span data-ttu-id="d380c-160">a.</span><span class="sxs-lookup"><span data-stu-id="d380c-160">a.</span></span> <span data-ttu-id="d380c-161">Vytvořte nový poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="d380c-161">Create a new notebook.</span></span> <span data-ttu-id="d380c-162">V pravém rohu hello, klikněte na **nový**.</span><span class="sxs-lookup"><span data-stu-id="d380c-162">From hello right-hand corner, click **New**.</span></span> <span data-ttu-id="d380c-163">Měli byste vidět hello výchozí jádra **Python2** a hello dvě nové jádra, které nainstalujete, **PySpark** a **Spark**.</span><span class="sxs-lookup"><span data-stu-id="d380c-163">You should see hello default kernel **Python2** and hello two new kernels that you install, **PySpark** and **Spark**.</span></span> <span data-ttu-id="d380c-164">Klikněte na tlačítko **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="d380c-164">Click **PySpark**.</span></span>

    <span data-ttu-id="d380c-165">![Jádra v Poznámkový blok Jupyter](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "jádra v poznámkového bloku Jupyter")</span><span class="sxs-lookup"><span data-stu-id="d380c-165">![Kernels in Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Kernels in Jupyter notebook")</span></span>

    <span data-ttu-id="d380c-166">b.</span><span class="sxs-lookup"><span data-stu-id="d380c-166">b.</span></span> <span data-ttu-id="d380c-167">Spusťte hello následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="d380c-167">Run hello following code snippet.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    <span data-ttu-id="d380c-168">Pokud se můžete úspěšně načíst výstup hello, je testován clusteru HDInsight toohello připojení.</span><span class="sxs-lookup"><span data-stu-id="d380c-168">If you can successfully retrieve hello output, your connection toohello HDInsight cluster is tested.</span></span>

    >[!TIP]
    ><span data-ttu-id="d380c-169">Pokud chcete tooupdate hello poznámkového bloku konfigurace tooconnect tooa jiného clusteru, aktualizujte hello config.json hello nové sady hodnot, jak je znázorněno v kroku 3 výše.</span><span class="sxs-lookup"><span data-stu-id="d380c-169">If you want tooupdate hello notebook configuration tooconnect tooa different cluster, update hello config.json with hello new set of values, as shown in Step 3 above.</span></span>

## <a name="why-should-i-install-jupyter-on-my-computer"></a><span data-ttu-id="d380c-170">Proč nainstalovat Jupyter v mém počítači?</span><span class="sxs-lookup"><span data-stu-id="d380c-170">Why should I install Jupyter on my computer?</span></span>
<span data-ttu-id="d380c-171">Může být číslo z důvodů, proč má tooinstall Jupyter v počítači a připojte jej clusteru tooa Spark v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d380c-171">There can be a number of reasons why you might want tooinstall Jupyter on your computer and then connect it tooa Spark cluster on HDInsight.</span></span>

* <span data-ttu-id="d380c-172">I když poznámkové bloky Jupyter jsou již k dispozici na hello cluster Spark v Azure HDInsight, instalaci do vašeho počítače Jupyter poskytuje hello možnost toocreate poznámkové bloky místně, aplikaci otestovat s spuštěného clusteru a potom odeslat hello cluster toohello poznámkových bloků.</span><span class="sxs-lookup"><span data-stu-id="d380c-172">Even though Jupyter notebooks are already available on hello Spark cluster in Azure HDInsight, installing Jupyter on your computer provides you hello option toocreate your notebooks locally, test your application against a running cluster, and then upload hello notebooks toohello cluster.</span></span> <span data-ttu-id="d380c-173">tooupload hello poznámkových bloků toohello clusteru, můžete buď je načíst pomocí poznámkového bloku Jupyter hello, který běží nebo hello clusteru, nebo je uložíte toohello /HdiNotebooks složek v účtu úložiště hello přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d380c-173">tooupload hello notebooks toohello cluster, you can either upload them using hello Jupyter notebook that is running or hello cluster, or save them toohello /HdiNotebooks folder in hello storage account associated with hello cluster.</span></span> <span data-ttu-id="d380c-174">Další informace o ukládání poznámkových bloků v hello clusteru najdete v tématu [úložiště poznámkové bloky Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span><span class="sxs-lookup"><span data-stu-id="d380c-174">For more information on how notebooks are stored on hello cluster, see [Where are Jupyter notebooks stored](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?</span></span>
* <span data-ttu-id="d380c-175">Díky poznámkových bloků hello k dispozici místně, můžete připojit clustery Spark toodifferent podle požadavků vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d380c-175">With hello notebooks available locally, you can connect toodifferent Spark clusters based on your application requirement.</span></span>
* <span data-ttu-id="d380c-176">Můžete použít Githubu tooimplement systému správy zdrojů a mají Správa verzí pro poznámkové bloky hello.</span><span class="sxs-lookup"><span data-stu-id="d380c-176">You can use GitHub tooimplement a source control system and have version control for hello notebooks.</span></span> <span data-ttu-id="d380c-177">Můžete mít také možnost spolupráce při prostředí, kde více mohou uživatelé pracovat s hello stejné poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="d380c-177">You can also have a collaborative environment where multiple users can work with hello same notebook.</span></span>
* <span data-ttu-id="d380c-178">Můžete pracovat s poznámkových bloků místně bez nutnosti i cluster s podporou.</span><span class="sxs-lookup"><span data-stu-id="d380c-178">You can work with notebooks locally without even having a cluster up.</span></span> <span data-ttu-id="d380c-179">Potřebujete jenom tootest clusteru poznámkové bloky proti, není toomanually spravovat poznámkové bloky nebo vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="d380c-179">You only need a cluster tootest your notebooks against, not toomanually manage your notebooks or a development environment.</span></span>
* <span data-ttu-id="d380c-180">Ho může být snazší tooconfigure vlastní místní vývojové prostředí než je tooconfigure hello Jupyter instalace v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="d380c-180">It may be easier tooconfigure your own local development environment than it is tooconfigure hello Jupyter installation on hello cluster.</span></span>  <span data-ttu-id="d380c-181">Můžete využít výhod všech hello softwaru, které jste nainstalovali místně bez konfigurace minimálně jeden vzdálený cluster.</span><span class="sxs-lookup"><span data-stu-id="d380c-181">You can take advantage of all hello software you have installed locally without configuring one or more remote clusters.</span></span>

> [!WARNING]
> <span data-ttu-id="d380c-182">S Jupyter nainstalovaná v místním počítači, můžete spustit více uživatelů hello stejné poznámkového bloku na clusteru stejné Spark v hello hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="d380c-182">With Jupyter installed on your local computer, multiple users can run hello same notebook on hello same Spark cluster at hello same time.</span></span> <span data-ttu-id="d380c-183">V takovém případě se vytvoří více Livy relací.</span><span class="sxs-lookup"><span data-stu-id="d380c-183">In such a situation, multiple Livy sessions are created.</span></span> <span data-ttu-id="d380c-184">Pokud narazíte na problém a chcete, aby mohla být složité úlohy tootrack, které Livy relace patří toowhich uživatele toodebug.</span><span class="sxs-lookup"><span data-stu-id="d380c-184">If you run into an issue and want toodebug that, it will be a complex task tootrack which Livy session belongs toowhich user.</span></span>
>
>

## <span data-ttu-id="d380c-185"><a name="seealso"></a>Viz také</span><span class="sxs-lookup"><span data-stu-id="d380c-185"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="d380c-186">Přehled: Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="d380c-186">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="d380c-187">Scénáře</span><span class="sxs-lookup"><span data-stu-id="d380c-187">Scenarios</span></span>
* [<span data-ttu-id="d380c-188">Spark s BI: Provádějte interaktivní analýzy dat pomocí Sparku v HDInsight pomocí nástrojů BI</span><span class="sxs-lookup"><span data-stu-id="d380c-188">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="d380c-189">Spark s Machine Learning: Používejte Spark v HDInsight pro analýzu teploty v budově pomocí dat HVAC</span><span class="sxs-lookup"><span data-stu-id="d380c-189">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="d380c-190">Spark s Machine Learning: používejte Spark v výsledků kontroly potravin toopredict HDInsight</span><span class="sxs-lookup"><span data-stu-id="d380c-190">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="d380c-191">Datové proudy Spark: Používejte Spark v HDInsight pro sestavení aplikací datových proudů v reálném čase</span><span class="sxs-lookup"><span data-stu-id="d380c-191">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="d380c-192">Analýza protokolu webu pomocí Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d380c-192">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="d380c-193">Vytvoření a spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="d380c-193">Create and run applications</span></span>
* [<span data-ttu-id="d380c-194">Vytvoření samostatné aplikace pomocí Scala</span><span class="sxs-lookup"><span data-stu-id="d380c-194">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="d380c-195">Vzdálené spouštění úloh na clusteru Sparku pomocí Livy</span><span class="sxs-lookup"><span data-stu-id="d380c-195">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="d380c-196">Nástroje a rozšíření</span><span class="sxs-lookup"><span data-stu-id="d380c-196">Tools and extensions</span></span>
* [<span data-ttu-id="d380c-197">Pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toocreate a odesílání aplikací Spark Scala</span><span class="sxs-lookup"><span data-stu-id="d380c-197">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="d380c-198">Vzdáleně pomocí modulu plug-in nástroje HDInsight pro IntelliJ IDEA toodebug Spark aplikace</span><span class="sxs-lookup"><span data-stu-id="d380c-198">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="d380c-199">Použití poznámkových bloků Zeppelin s clusterem Sparku v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d380c-199">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="d380c-200">Jádra dostupná pro poznámkový blok Jupyter v clusteru Sparku pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="d380c-200">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="d380c-201">Použití externích balíčků s poznámkovými bloky Jupyter</span><span class="sxs-lookup"><span data-stu-id="d380c-201">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a><span data-ttu-id="d380c-202">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="d380c-202">Manage resources</span></span>
* [<span data-ttu-id="d380c-203">Správa prostředků hello cluster Apache Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="d380c-203">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="d380c-204">Sledování a ladění úloh spuštěných v clusteru Apache Spark v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d380c-204">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
