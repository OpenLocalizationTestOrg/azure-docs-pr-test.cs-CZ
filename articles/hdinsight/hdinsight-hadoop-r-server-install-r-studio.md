---
title: aaaInstall Rstudia s R serverem v HDInsight - Azure | Microsoft Docs
description: Jak tooinstall Rstudia s R serverem v HDInsight.
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 918abb0d-8248-4bc5-98dc-089c0e007d49
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: b3a23021fcf99217e8f551f8b2e89bf1f1e5b967
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a><span data-ttu-id="7b507-103">Instalace Rstudia s R serverem v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7b507-103">Installing RStudio with R Server on HDInsight</span></span>

<span data-ttu-id="7b507-104">Tento článek popisuje, jak tooinstall hello community (zdarma) verze [Rstudia Server](https://www.rstudio.com/products/rstudio-server/) hello edge uzlu clusteru pomocí vlastního skriptu.</span><span class="sxs-lookup"><span data-stu-id="7b507-104">This article describes how tooinstall hello community (free) version of [RStudio Server](https://www.rstudio.com/products/rstudio-server/) on hello edge node of a cluster using a custom script.</span></span> <span data-ttu-id="7b507-105">Rstudia Server poskytuje IDE se založené na prohlížeči pro vzdálené klienty a se často používá v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="7b507-105">RStudio Server provides a browser-based IDE for use by remote clients and is widely used on Linux.</span></span> <span data-ttu-id="7b507-106">Existuje více integrované vývojové prostředí (integrovaného vývojového prostředí) k dispozici pro R dnes, včetně:</span><span class="sxs-lookup"><span data-stu-id="7b507-106">There are multiple integrated development environments (IDEs) available for R today, including:</span></span>

- <span data-ttu-id="7b507-107">Společnosti Microsoft [R Tools pro Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span><span class="sxs-lookup"><span data-stu-id="7b507-107">Microsoft’s  [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span></span> 
- [<span data-ttu-id="7b507-108">Rstudia serveru</span><span class="sxs-lookup"><span data-stu-id="7b507-108">RStudio Server</span></span>](https://www.rstudio.com/products/rstudio-server/) 
- <span data-ttu-id="7b507-109">Walware je na základě Eclipse [StatET](http://www.walware.de/goto/statet).</span><span class="sxs-lookup"><span data-stu-id="7b507-109">Walware’s Eclipse-based [StatET](http://www.walware.de/goto/statet).</span></span>

<span data-ttu-id="7b507-110">Výhodou Hello nainstalovat Rstudia Server hello hraniční uzel clusteru HDInsight je, že zajišťuje úplné prostředí IDE pro vývoj hello a spouštění skriptů R s R Server v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="7b507-110">hello advantage of installing RStudio Server on hello edge node of an HDInsight cluster is that it provides a full IDE experience for hello development and execution of R scripts with R Server on hello cluster.</span></span> <span data-ttu-id="7b507-111">Tato konfigurace může být výrazně zvýšit produktivitu, než výchozí použití hello R konzoly.</span><span class="sxs-lookup"><span data-stu-id="7b507-111">This configuration can be considerably more productive than default use of hello R Console.</span></span>

> [!NOTE]
> <span data-ttu-id="7b507-112">Hello postup popsaný v tomto článku je pouze v případě, že jste nevybrali tooinstall edice community Rstudia serveru při zřizování clusteru.</span><span class="sxs-lookup"><span data-stu-id="7b507-112">hello procedure described in this article is only relevant if you did not select tooinstall RStudio Server community edition when provisioning your cluster.</span></span> <span data-ttu-id="7b507-113">Pokud jste přidali při zřizování, pak tooaccess ho kliknutím na hello **řídicí panely serveru R** dlaždice v hello Azure portálu položka pro váš cluster a potom na hello **R Studio Server** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="7b507-113">If you added it during provisioning, then tooaccess it you click on hello **R Server Dashboards** tile in hello Azure portal entry for your cluster, then on hello **R Studio Server** tile.</span></span> 

<span data-ttu-id="7b507-114">Pokud dáváte přednost toouse hello komerčně licenci Pro verze Rstudia serveru, postupujte podle hello pokyny k instalaci z [Rstudia Server](https://www.rstudio.com/products/rstudio/download-server/).</span><span class="sxs-lookup"><span data-stu-id="7b507-114">If you prefer toouse hello commercially licensed Pro version of RStudio Server, you must follow hello installation instructions from [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).</span></span>

> [!NOTE]
> <span data-ttu-id="7b507-115">Pokud používáte cluster služby HDInsight, pro který byla nainstalována R pomocí hello [nainstalovat akce skriptu R](hdinsight-hadoop-r-scripts-linux.md), hello kroky v tomto dokumentu nebude fungovat správně, protože vyžadují serveru R na clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="7b507-115">If you are using an HDInsight cluster for which R was installed using hello [Install R Script Action](hdinsight-hadoop-r-scripts-linux.md), hello steps in this document will not work correctly as they require an R Server on hello HDInsight cluster.</span></span>
>
> 

## <a name="prerequisites"></a><span data-ttu-id="7b507-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7b507-116">Prerequisites</span></span>

* <span data-ttu-id="7b507-117">Cluster Azure HDInsight s nainstalovaným serverem R.</span><span class="sxs-lookup"><span data-stu-id="7b507-117">An Azure HDInsight cluster with R Server installed.</span></span> <span data-ttu-id="7b507-118">Pokyny najdete v tématu [začít pracovat s R Server v clusterech HDInsight](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7b507-118">For instructions, see [Get started with R Server on HDInsight clusters](hdinsight-hadoop-r-server-get-started.md).</span></span>
* <span data-ttu-id="7b507-119">Klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="7b507-119">An SSH client.</span></span> <span data-ttu-id="7b507-120">Pro distribuce operačních systémů Linux a Unix nebo Macintosh OS X hello `ssh` příkaz se poskytuje s hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="7b507-120">For Linux and Unix distributions or Macintosh OS X, hello `ssh` command is provided with hello operating system.</span></span> <span data-ttu-id="7b507-121">Pro systém Windows, doporučujeme, abyste [emulaci](http://www.redhat.com/services/custom/cygwin/) s hello [OpenSSH možnost](https://www.youtube.com/watch?v=CwYSvvGaiWU), nebo [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="7b507-121">For Windows, we recommend [Cygwin](http://www.redhat.com/services/custom/cygwin/) with hello [OpenSSH option](https://www.youtube.com/watch?v=CwYSvvGaiWU), or [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>  

## <a name="install-rstudio-on-hello-cluster-using-a-custom-script"></a><span data-ttu-id="7b507-122">Nainstalujte Rstudia hello clusteru pomocí vlastního skriptu</span><span class="sxs-lookup"><span data-stu-id="7b507-122">Install RStudio on hello cluster using a custom script</span></span>

<span data-ttu-id="7b507-123">Zde jsou kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7b507-123">Here are hello steps:</span></span>

1. <span data-ttu-id="7b507-124">Identifikujte hello hraniční uzel clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="7b507-124">Identify hello edge node of hello cluster.</span></span> <span data-ttu-id="7b507-125">Pro cluster služby HDInsight s R Server následuje hello zásady vytváření názvů pro hlavní uzel a hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="7b507-125">For an HDInsight cluster with R Server, following is hello naming convention for head node and edge node.</span></span>
   * <span data-ttu-id="7b507-126">Hlavní uzel`CLUSTERNAME-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="7b507-126">Head node `CLUSTERNAME-ssh.azurehdinsight.net`</span></span>
   * <span data-ttu-id="7b507-127">Hraniční uzel`CLUSTERNAME-ed-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="7b507-127">Edge node `CLUSTERNAME-ed-ssh.azurehdinsight.net`</span></span> 

2. <span data-ttu-id="7b507-128">SSH do hello hraniční uzel clusteru hello pomocí vzoru pro pojmenovávání hello zadané v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="7b507-128">SSH into hello edge node of hello cluster using hello naming pattern provided in step 1.</span></span> <span data-ttu-id="7b507-129">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="7b507-129">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="7b507-130">Jakmile se připojíte, stát uživatele root na hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="7b507-130">Once you are connected, become a root user on hello cluster.</span></span> <span data-ttu-id="7b507-131">V relaci SSH hello použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="7b507-131">In hello SSH session, use hello following command:</span></span>

        sudo su -

4. <span data-ttu-id="7b507-132">Stáhněte si hello vlastní skript tooinstall Rstudia.</span><span class="sxs-lookup"><span data-stu-id="7b507-132">Download hello custom script tooinstall RStudio.</span></span> <span data-ttu-id="7b507-133">Hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7b507-133">Use hello following command:</span></span>

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. <span data-ttu-id="7b507-134">Změna hello oprávnění u souboru hello vlastní skript a spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="7b507-134">Change hello permissions on hello custom script file and run hello script.</span></span> <span data-ttu-id="7b507-135">Hello použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="7b507-135">Use hello following commands:</span></span>

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. <span data-ttu-id="7b507-136">Pokud jste použili heslo k SSH při vytváření clusteru HDInsight s R Server, můžete tento krok přeskočit a přejít toohello Další.</span><span class="sxs-lookup"><span data-stu-id="7b507-136">If you used an SSH password while creating an HDInsight cluster with R Server, you can skip this step and proceed toohello next.</span></span> <span data-ttu-id="7b507-137">Pokud jste použili klíče SSH místo toocreate hello clusteru, musíte nastavit heslo pro vaše uživatele SSH.</span><span class="sxs-lookup"><span data-stu-id="7b507-137">If you used an SSH key instead toocreate hello cluster, you must set a password for your SSH user.</span></span> <span data-ttu-id="7b507-138">Při připojování tooRStudio musíte toto heslo.</span><span class="sxs-lookup"><span data-stu-id="7b507-138">You need this password when connecting tooRStudio.</span></span> <span data-ttu-id="7b507-139">Spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="7b507-139">Run hello following commands:</span></span>

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. <span data-ttu-id="7b507-140">Po zobrazení výzvy k **aktuální Kerberos heslo**, stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="7b507-140">When prompted for **Current Kerberos password**, press **ENTER**.</span></span>  <span data-ttu-id="7b507-141">Všimněte si, že je třeba nahradit `USERNAME` s uživatelem SSH pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7b507-141">Note that you must replace `USERNAME` with an SSH user for your HDInsight cluster.</span></span> <span data-ttu-id="7b507-142">Pokud je heslo úspěšně nastavená, měli byste vidět hello následující zprávou:</span><span class="sxs-lookup"><span data-stu-id="7b507-142">If your password is successfully set, you should see hello following message:</span></span>

        passwd: password updated successfully

    <span data-ttu-id="7b507-143">Ukončení relace SSH hello.</span><span class="sxs-lookup"><span data-stu-id="7b507-143">Exit hello SSH session.</span></span>

8. <span data-ttu-id="7b507-144">Vytvoření clusteru toohello tunelového propojení SSH pomocí mapování `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` v hello HDInsight clusteru toohello klientský počítač.</span><span class="sxs-lookup"><span data-stu-id="7b507-144">Create an SSH tunnel toohello cluster by mapping `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` on hello HDInsight cluster toohello client machine.</span></span> <span data-ttu-id="7b507-145">Tunelové propojení SSH je třeba vytvořit před otevřením novou relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7b507-145">You must create an SSH tunnel before opening a new browser session.</span></span>

   * <span data-ttu-id="7b507-146">U klienta Linux nebo klienta Windows s [emulaci](http://www.redhat.com/services/custom/cygwin/), otevřete relaci terminálu a použít hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7b507-146">On a Linux client or a Windows client with [Cygwin](http://www.redhat.com/services/custom/cygwin/), open a terminal session and use hello following command:</span></span>

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       <span data-ttu-id="7b507-147">Nahraďte **uživatelské jméno** s uživatelem SSH pro váš HDInsight cluster a nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7b507-147">Replace **USERNAME** with an SSH user for your HDInsight cluster, and replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>
       <span data-ttu-id="7b507-148">Můžete také použít klíč SSH, nikoli heslo přidáním `-i id_rsa_key`.</span><span class="sxs-lookup"><span data-stu-id="7b507-148">You can also use an SSH key rather than a password by adding `-i id_rsa_key`.</span></span>        
   * <span data-ttu-id="7b507-149">Pokud klient systému Windows pomocí klienta PuTTY pak</span><span class="sxs-lookup"><span data-stu-id="7b507-149">If using a Windows client with PuTTY then</span></span>

     1. <span data-ttu-id="7b507-150">Otevřete PuTTY a zadejte informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="7b507-150">Open PuTTY, and enter your connection information.</span></span>
     2. <span data-ttu-id="7b507-151">V hello **kategorie** levé části toohello hello dialogového okna, rozbalte položku **připojení**, rozbalte položku **SSH**a potom vyberte **tunely**.</span><span class="sxs-lookup"><span data-stu-id="7b507-151">In hello **Category** section toohello left of hello dialog, expand **Connection**, expand **SSH**, and then select **Tunnels**.</span></span>
     3. <span data-ttu-id="7b507-152">Zadejte následující informace na hello hello **možnosti řízení SSH port předávání** formuláře:</span><span class="sxs-lookup"><span data-stu-id="7b507-152">Provide hello following information on hello **Options controlling SSH port forwarding** form:</span></span>

        * <span data-ttu-id="7b507-153">**Zdrojový port** -hello na klienta hello chcete tooforward port.</span><span class="sxs-lookup"><span data-stu-id="7b507-153">**Source port** - hello port on hello client that you wish tooforward.</span></span> <span data-ttu-id="7b507-154">Například **8787**.</span><span class="sxs-lookup"><span data-stu-id="7b507-154">For example, **8787**.</span></span>
        * <span data-ttu-id="7b507-155">**Cílový** – hello cílového umístění, které musí být namapovaný toohello místní klientský počítač.</span><span class="sxs-lookup"><span data-stu-id="7b507-155">**Destination** - hello destination that must be mapped toohello local client machine.</span></span> <span data-ttu-id="7b507-156">Například **localhost:8787**.</span><span class="sxs-lookup"><span data-stu-id="7b507-156">For example, **localhost:8787**.</span></span>

            <span data-ttu-id="7b507-157">![Vytvoření tunelu SSH](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "vytvoření tunelu SSH")</span><span class="sxs-lookup"><span data-stu-id="7b507-157">![Create an SSH tunnel](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Create an SSH tunnel")</span></span>

     4. <span data-ttu-id="7b507-158">Klikněte na tlačítko **přidat** tooadd hello nastavení a potom klikněte na **otevřete** tooopen připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="7b507-158">Click **Add** tooadd hello settings, and then click **Open** tooopen an SSH connection.</span></span>
     5. <span data-ttu-id="7b507-159">Po zobrazení výzvy, přihlaste se toohello server tooestablish tunelu SSH relace a povolit hello.</span><span class="sxs-lookup"><span data-stu-id="7b507-159">When prompted, log in toohello server tooestablish an SSH session and enable hello tunnel.</span></span>

9. <span data-ttu-id="7b507-160">Otevřete webový prohlížeč a zadejte následující adresu URL na hello port, které jste zadali pro tunelové propojení hello hello:</span><span class="sxs-lookup"><span data-stu-id="7b507-160">Open a web browser and enter hello following URL based on hello port you entered for hello tunnel:</span></span>

        http://localhost:8787/ 

10. <span data-ttu-id="7b507-161">Jste výzvami tooenter hello SSH uživatelské jméno a heslo tooconnect toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="7b507-161">You are prompted tooenter hello SSH username and password tooconnect toohello cluster.</span></span> <span data-ttu-id="7b507-162">Pokud jste použili klíč SSH při vytváření clusteru hello, je nutné zadat heslo hello, kterou jste vytvořili v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="7b507-162">If you used an SSH key while creating hello cluster, you must enter hello password you created in step 5.</span></span>

    <span data-ttu-id="7b507-163">![Připojit tooR Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "vytvoření tunelu SSH")</span><span class="sxs-lookup"><span data-stu-id="7b507-163">![Connect tooR Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Create an SSH tunnel")</span></span>

11. <span data-ttu-id="7b507-164">tootest zda hello Rstudia instalace byla úspěšná, můžete spustit test skript, který spouští úlohy MapReduce a Spark na základě R na clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="7b507-164">tootest whether hello RStudio installation was successful, you can run a test script that executes R-based MapReduce and Spark jobs on hello cluster.</span></span> <span data-ttu-id="7b507-165">toodownload hello testovací skriptu toorun v Rstudia, toohello SSH konzole přejděte zpět a zadejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="7b507-165">toodownload hello test script toorun in RStudio, go back toohello SSH console and enter hello following commands:</span></span>

    *    <span data-ttu-id="7b507-166">Pokud jste vytvořili clusteru Hadoop s R, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="7b507-166">If you created a Hadoop cluster with R, use this command:</span></span>

            <span data-ttu-id="7b507-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span><span class="sxs-lookup"><span data-stu-id="7b507-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span></span>
    *    <span data-ttu-id="7b507-168">Když vytvoříte Spark cluster s R, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="7b507-168">If you created a Spark cluster with R, use this command:</span></span>

            <span data-ttu-id="7b507-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span><span class="sxs-lookup"><span data-stu-id="7b507-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span></span>

12. <span data-ttu-id="7b507-170">V Rstudia najdete v části hello testování skript, který jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="7b507-170">In RStudio, you see hello test script you downloaded.</span></span> <span data-ttu-id="7b507-171">Poklikejte na soubor tooopen hello, vyberte obsah hello hello souboru a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="7b507-171">Double-click hello file tooopen it, select hello contents of hello file, and then click **Run**.</span></span> <span data-ttu-id="7b507-172">Měli byste vidět výstup hello v hello **konzoly** podokně:</span><span class="sxs-lookup"><span data-stu-id="7b507-172">You should see hello output in hello **Console** pane:</span></span>

   <span data-ttu-id="7b507-173">![Testování instalace hello](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "otestovat instalaci hello")</span><span class="sxs-lookup"><span data-stu-id="7b507-173">![Test hello installation](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Test hello installation")</span></span>

<span data-ttu-id="7b507-174">Další možností by tootype `source(testhdi.r)` nebo `source(testhdi_spark.r)` tooexecute hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="7b507-174">Another option would be tootype `source(testhdi.r)` or `source(testhdi_spark.r)` tooexecute hello script.</span></span>

## <a name="see-also"></a><span data-ttu-id="7b507-175">Viz také</span><span class="sxs-lookup"><span data-stu-id="7b507-175">See also</span></span>

* [<span data-ttu-id="7b507-176">Výpočetní kontextu možnosti pro R Server v clusterech prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="7b507-176">Compute context options for R Server on HDInsight clusters</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)
* [<span data-ttu-id="7b507-177">Možnosti služby Azure Storage pro R Server ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="7b507-177">Azure Storage options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-storage.md)

