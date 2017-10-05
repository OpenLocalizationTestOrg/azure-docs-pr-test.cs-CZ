---
title: Nainstalujte Rstudia s R serverem v HDInsight - Azure | Microsoft Docs
description: Postup instalace Rstudia s R serverem v HDInsight.
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
ms.openlocfilehash: 416420d855505508735ebd8526e93efdb230ad53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a><span data-ttu-id="1d497-103">Instalace Rstudia s R serverem v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1d497-103">Installing RStudio with R Server on HDInsight</span></span>

<span data-ttu-id="1d497-104">Tento článek popisuje postup instalace community (zdarma) verze [Rstudia Server](https://www.rstudio.com/products/rstudio-server/) na hraničního uzlu clusteru pomocí vlastního skriptu.</span><span class="sxs-lookup"><span data-stu-id="1d497-104">This article describes how to install the community (free) version of [RStudio Server](https://www.rstudio.com/products/rstudio-server/) on the edge node of a cluster using a custom script.</span></span> <span data-ttu-id="1d497-105">Rstudia Server poskytuje IDE se založené na prohlížeči pro vzdálené klienty a se často používá v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="1d497-105">RStudio Server provides a browser-based IDE for use by remote clients and is widely used on Linux.</span></span> <span data-ttu-id="1d497-106">Existuje více integrované vývojové prostředí (integrovaného vývojového prostředí) k dispozici pro R dnes, včetně:</span><span class="sxs-lookup"><span data-stu-id="1d497-106">There are multiple integrated development environments (IDEs) available for R today, including:</span></span>

- <span data-ttu-id="1d497-107">Společnosti Microsoft [R Tools pro Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span><span class="sxs-lookup"><span data-stu-id="1d497-107">Microsoft’s  [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS)</span></span> 
- [<span data-ttu-id="1d497-108">Rstudia serveru</span><span class="sxs-lookup"><span data-stu-id="1d497-108">RStudio Server</span></span>](https://www.rstudio.com/products/rstudio-server/) 
- <span data-ttu-id="1d497-109">Walware je na základě Eclipse [StatET](http://www.walware.de/goto/statet).</span><span class="sxs-lookup"><span data-stu-id="1d497-109">Walware’s Eclipse-based [StatET](http://www.walware.de/goto/statet).</span></span>

<span data-ttu-id="1d497-110">Výhodou nainstalovat Server Rstudia hraničního uzlu clusteru HDInsight je, že zajišťuje úplné prostředí IDE pro vývoj a spouštění skriptů R s R Server v clusteru.</span><span class="sxs-lookup"><span data-stu-id="1d497-110">The advantage of installing RStudio Server on the edge node of an HDInsight cluster is that it provides a full IDE experience for the development and execution of R scripts with R Server on the cluster.</span></span> <span data-ttu-id="1d497-111">Tato konfigurace může být výrazně zvýšit produktivitu, než výchozí použití R konzoly.</span><span class="sxs-lookup"><span data-stu-id="1d497-111">This configuration can be considerably more productive than default use of the R Console.</span></span>

> [!NOTE]
> <span data-ttu-id="1d497-112">Postup popsaný v tomto článku je pouze v případě, že jste nevybrali pro instalaci edice community Rstudia serveru při zřizování clusteru.</span><span class="sxs-lookup"><span data-stu-id="1d497-112">The procedure described in this article is only relevant if you did not select to install RStudio Server community edition when provisioning your cluster.</span></span> <span data-ttu-id="1d497-113">Pokud jste přidali při zřizování, pak pro přístup k ní kliknete na **řídicí panely serveru R** v položce portál Azure pro váš cluster, klikněte na dlaždici **R Studio Server** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="1d497-113">If you added it during provisioning, then to access it you click on the **R Server Dashboards** tile in the Azure portal entry for your cluster, then on the **R Studio Server** tile.</span></span> 

<span data-ttu-id="1d497-114">Pokud byste radši chtěli použít komerčně licencovanou verzi Pro Rstudia serveru, postupujte podle pokynů pro instalaci z [Rstudia Server](https://www.rstudio.com/products/rstudio/download-server/).</span><span class="sxs-lookup"><span data-stu-id="1d497-114">If you prefer to use the commercially licensed Pro version of RStudio Server, you must follow the installation instructions from [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).</span></span>

> [!NOTE]
> <span data-ttu-id="1d497-115">Pokud používáte cluster služby HDInsight, pro který byla nainstalována pomocí R [nainstalovat akce skriptu R](hdinsight-hadoop-r-scripts-linux.md), kroky v tomto dokumentu nebude fungovat správně, protože vyžadují serveru R na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1d497-115">If you are using an HDInsight cluster for which R was installed using the [Install R Script Action](hdinsight-hadoop-r-scripts-linux.md), the steps in this document will not work correctly as they require an R Server on the HDInsight cluster.</span></span>
>
> 

## <a name="prerequisites"></a><span data-ttu-id="1d497-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1d497-116">Prerequisites</span></span>

* <span data-ttu-id="1d497-117">Cluster Azure HDInsight s nainstalovaným serverem R.</span><span class="sxs-lookup"><span data-stu-id="1d497-117">An Azure HDInsight cluster with R Server installed.</span></span> <span data-ttu-id="1d497-118">Pokyny najdete v tématu [začít pracovat s R Server v clusterech HDInsight](hdinsight-hadoop-r-server-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="1d497-118">For instructions, see [Get started with R Server on HDInsight clusters](hdinsight-hadoop-r-server-get-started.md).</span></span>
* <span data-ttu-id="1d497-119">Klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="1d497-119">An SSH client.</span></span> <span data-ttu-id="1d497-120">Pro distribuce operačních systémů Linux a Unix nebo OS X Macintosh `ssh` příkaz je součástí operačního systému.</span><span class="sxs-lookup"><span data-stu-id="1d497-120">For Linux and Unix distributions or Macintosh OS X, the `ssh` command is provided with the operating system.</span></span> <span data-ttu-id="1d497-121">Pro systém Windows, doporučujeme, abyste [emulaci](http://www.redhat.com/services/custom/cygwin/) s [OpenSSH možnost](https://www.youtube.com/watch?v=CwYSvvGaiWU), nebo [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="1d497-121">For Windows, we recommend [Cygwin](http://www.redhat.com/services/custom/cygwin/) with the [OpenSSH option](https://www.youtube.com/watch?v=CwYSvvGaiWU), or [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>  

## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a><span data-ttu-id="1d497-122">Nainstalujte Rstudia na cluster pomocí vlastního skriptu</span><span class="sxs-lookup"><span data-stu-id="1d497-122">Install RStudio on the cluster using a custom script</span></span>

<span data-ttu-id="1d497-123">Postup je následující:</span><span class="sxs-lookup"><span data-stu-id="1d497-123">Here are the steps:</span></span>

1. <span data-ttu-id="1d497-124">Identifikujte hraničního uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="1d497-124">Identify the edge node of the cluster.</span></span> <span data-ttu-id="1d497-125">Pro cluster služby HDInsight s R Server následuje zásady vytváření názvů pro hlavní uzel a hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="1d497-125">For an HDInsight cluster with R Server, following is the naming convention for head node and edge node.</span></span>
   * <span data-ttu-id="1d497-126">Hlavní uzel`CLUSTERNAME-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="1d497-126">Head node `CLUSTERNAME-ssh.azurehdinsight.net`</span></span>
   * <span data-ttu-id="1d497-127">Hraniční uzel`CLUSTERNAME-ed-ssh.azurehdinsight.net`</span><span class="sxs-lookup"><span data-stu-id="1d497-127">Edge node `CLUSTERNAME-ed-ssh.azurehdinsight.net`</span></span> 

2. <span data-ttu-id="1d497-128">SSH do hraničního uzlu clusteru pomocí vzoru pro pojmenovávání zadané v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="1d497-128">SSH into the edge node of the cluster using the naming pattern provided in step 1.</span></span> <span data-ttu-id="1d497-129">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="1d497-129">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="1d497-130">Jakmile se připojíte, stát kořenovou uživatel v clusteru.</span><span class="sxs-lookup"><span data-stu-id="1d497-130">Once you are connected, become a root user on the cluster.</span></span> <span data-ttu-id="1d497-131">V této relaci SSH použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1d497-131">In the SSH session, use the following command:</span></span>

        sudo su -

4. <span data-ttu-id="1d497-132">Stáhněte si vlastní skript k instalaci Rstudia.</span><span class="sxs-lookup"><span data-stu-id="1d497-132">Download the custom script to install RStudio.</span></span> <span data-ttu-id="1d497-133">Použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1d497-133">Use the following command:</span></span>

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. <span data-ttu-id="1d497-134">Změna oprávnění u souboru vlastní skript a spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="1d497-134">Change the permissions on the custom script file and run the script.</span></span> <span data-ttu-id="1d497-135">Použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1d497-135">Use the following commands:</span></span>

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. <span data-ttu-id="1d497-136">Pokud jste použili heslo k SSH při vytváření clusteru HDInsight s R Server, můžete tento krok přeskočit a přejít na další.</span><span class="sxs-lookup"><span data-stu-id="1d497-136">If you used an SSH password while creating an HDInsight cluster with R Server, you can skip this step and proceed to the next.</span></span> <span data-ttu-id="1d497-137">Pokud jste použili klíče SSH místo toho k vytvoření clusteru, musíte nastavit heslo pro vaše uživatele SSH.</span><span class="sxs-lookup"><span data-stu-id="1d497-137">If you used an SSH key instead to create the cluster, you must set a password for your SSH user.</span></span> <span data-ttu-id="1d497-138">Toto heslo je nutné při připojování k Rstudia.</span><span class="sxs-lookup"><span data-stu-id="1d497-138">You need this password when connecting to RStudio.</span></span> <span data-ttu-id="1d497-139">Spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1d497-139">Run the following commands:</span></span>

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. <span data-ttu-id="1d497-140">Po zobrazení výzvy k **aktuální Kerberos heslo**, stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="1d497-140">When prompted for **Current Kerberos password**, press **ENTER**.</span></span>  <span data-ttu-id="1d497-141">Všimněte si, že je třeba nahradit `USERNAME` s uživatelem SSH pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1d497-141">Note that you must replace `USERNAME` with an SSH user for your HDInsight cluster.</span></span> <span data-ttu-id="1d497-142">Pokud je heslo úspěšně nastavená, zobrazí se následující zpráva:</span><span class="sxs-lookup"><span data-stu-id="1d497-142">If your password is successfully set, you should see the following message:</span></span>

        passwd: password updated successfully

    <span data-ttu-id="1d497-143">Ukončení relace SSH.</span><span class="sxs-lookup"><span data-stu-id="1d497-143">Exit the SSH session.</span></span>

8. <span data-ttu-id="1d497-144">Vytvoření tunelu SSH do clusteru pomocí mapování `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` v clusteru HDInsight na klientský počítač.</span><span class="sxs-lookup"><span data-stu-id="1d497-144">Create an SSH tunnel to the cluster by mapping `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` on the HDInsight cluster to the client machine.</span></span> <span data-ttu-id="1d497-145">Tunelové propojení SSH je třeba vytvořit před otevřením novou relaci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1d497-145">You must create an SSH tunnel before opening a new browser session.</span></span>

   * <span data-ttu-id="1d497-146">U klienta Linux nebo klienta Windows s [emulaci](http://www.redhat.com/services/custom/cygwin/), otevřete relaci terminálu a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1d497-146">On a Linux client or a Windows client with [Cygwin](http://www.redhat.com/services/custom/cygwin/), open a terminal session and use the following command:</span></span>

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       <span data-ttu-id="1d497-147">Nahraďte **uživatelské jméno** s uživatelem SSH pro váš HDInsight cluster a nahraďte **CLUSTERNAME** s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1d497-147">Replace **USERNAME** with an SSH user for your HDInsight cluster, and replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>
       <span data-ttu-id="1d497-148">Můžete také použít klíč SSH, nikoli heslo přidáním `-i id_rsa_key`.</span><span class="sxs-lookup"><span data-stu-id="1d497-148">You can also use an SSH key rather than a password by adding `-i id_rsa_key`.</span></span>        
   * <span data-ttu-id="1d497-149">Pokud klient systému Windows pomocí klienta PuTTY pak</span><span class="sxs-lookup"><span data-stu-id="1d497-149">If using a Windows client with PuTTY then</span></span>

     1. <span data-ttu-id="1d497-150">Otevřete PuTTY a zadejte informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="1d497-150">Open PuTTY, and enter your connection information.</span></span>
     2. <span data-ttu-id="1d497-151">V **kategorie** nalevo dialogového okna, rozbalte položku **připojení**, rozbalte položku **SSH**a potom vyberte **tunely**.</span><span class="sxs-lookup"><span data-stu-id="1d497-151">In the **Category** section to the left of the dialog, expand **Connection**, expand **SSH**, and then select **Tunnels**.</span></span>
     3. <span data-ttu-id="1d497-152">Zadejte následující informace na **možnosti řízení SSH port předávání** formuláře:</span><span class="sxs-lookup"><span data-stu-id="1d497-152">Provide the following information on the **Options controlling SSH port forwarding** form:</span></span>

        * <span data-ttu-id="1d497-153">**Zdrojový port** – port na straně klienta, který chcete přesměrovat.</span><span class="sxs-lookup"><span data-stu-id="1d497-153">**Source port** - The port on the client that you wish to forward.</span></span> <span data-ttu-id="1d497-154">Například **8787**.</span><span class="sxs-lookup"><span data-stu-id="1d497-154">For example, **8787**.</span></span>
        * <span data-ttu-id="1d497-155">**Cílový** -na cíl, který musí být mapován na místním klientském počítači.</span><span class="sxs-lookup"><span data-stu-id="1d497-155">**Destination** - The destination that must be mapped to the local client machine.</span></span> <span data-ttu-id="1d497-156">Například **localhost:8787**.</span><span class="sxs-lookup"><span data-stu-id="1d497-156">For example, **localhost:8787**.</span></span>

            <span data-ttu-id="1d497-157">![Vytvoření tunelu SSH](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "vytvoření tunelu SSH")</span><span class="sxs-lookup"><span data-stu-id="1d497-157">![Create an SSH tunnel](./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Create an SSH tunnel")</span></span>

     4. <span data-ttu-id="1d497-158">Klikněte na tlačítko **přidat** přidejte nastavení a potom klikněte na **otevřete** otevřít připojení SSH.</span><span class="sxs-lookup"><span data-stu-id="1d497-158">Click **Add** to add the settings, and then click **Open** to open an SSH connection.</span></span>
     5. <span data-ttu-id="1d497-159">Pokud budete vyzváni, přihlaste se k serveru k zahájení relace SSH a povolte tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="1d497-159">When prompted, log in to the server to establish an SSH session and enable the tunnel.</span></span>

9. <span data-ttu-id="1d497-160">Otevřete webový prohlížeč a zadejte následující adresu URL na port, které jste zadali pro tunelové propojení:</span><span class="sxs-lookup"><span data-stu-id="1d497-160">Open a web browser and enter the following URL based on the port you entered for the tunnel:</span></span>

        http://localhost:8787/ 

10. <span data-ttu-id="1d497-161">Zobrazí se výzva k zadání SSH uživatelské jméno a heslo pro připojení ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="1d497-161">You are prompted to enter the SSH username and password to connect to the cluster.</span></span> <span data-ttu-id="1d497-162">Pokud jste použili klíč SSH při vytváření clusteru, je nutné zadat heslo, které jste vytvořili v kroku 5.</span><span class="sxs-lookup"><span data-stu-id="1d497-162">If you used an SSH key while creating the cluster, you must enter the password you created in step 5.</span></span>

    <span data-ttu-id="1d497-163">![Připojení k R Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "vytvoření tunelu SSH")</span><span class="sxs-lookup"><span data-stu-id="1d497-163">![Connect to R Studio](./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Create an SSH tunnel")</span></span>

11. <span data-ttu-id="1d497-164">K ověření, zda Rstudia instalace byla úspěšná, můžete spustit test skript, který spouští úlohy MapReduce a Spark na základě R na clusteru.</span><span class="sxs-lookup"><span data-stu-id="1d497-164">To test whether the RStudio installation was successful, you can run a test script that executes R-based MapReduce and Spark jobs on the cluster.</span></span> <span data-ttu-id="1d497-165">Chcete-li stáhnout zkušební skript spustit v Rstudia, přejděte zpět ke konzole SSH a zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1d497-165">To download the test script to run in RStudio, go back to the SSH console and enter the following commands:</span></span>

    *    <span data-ttu-id="1d497-166">Pokud jste vytvořili clusteru Hadoop s R, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="1d497-166">If you created a Hadoop cluster with R, use this command:</span></span>

            <span data-ttu-id="1d497-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span><span class="sxs-lookup"><span data-stu-id="1d497-167">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r</span></span>
    *    <span data-ttu-id="1d497-168">Když vytvoříte Spark cluster s R, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="1d497-168">If you created a Spark cluster with R, use this command:</span></span>

            <span data-ttu-id="1d497-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span><span class="sxs-lookup"><span data-stu-id="1d497-169">wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r</span></span>

12. <span data-ttu-id="1d497-170">V Rstudia najdete v části testovací skript, který jste stáhli.</span><span class="sxs-lookup"><span data-stu-id="1d497-170">In RStudio, you see the test script you downloaded.</span></span> <span data-ttu-id="1d497-171">Poklikáním soubor otevřete, vyberte obsah souboru a pak klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="1d497-171">Double-click the file to open it, select the contents of the file, and then click **Run**.</span></span> <span data-ttu-id="1d497-172">Měli byste vidět výstup v **konzoly** podokně:</span><span class="sxs-lookup"><span data-stu-id="1d497-172">You should see the output in the **Console** pane:</span></span>

   <span data-ttu-id="1d497-173">![Testování instalace](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "testování instalace")</span><span class="sxs-lookup"><span data-stu-id="1d497-173">![Test the installation](./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Test the installation")</span></span>

<span data-ttu-id="1d497-174">Další možností je `source(testhdi.r)` nebo `source(testhdi_spark.r)` se spustit skript.</span><span class="sxs-lookup"><span data-stu-id="1d497-174">Another option would be to type `source(testhdi.r)` or `source(testhdi_spark.r)` to execute the script.</span></span>

## <a name="see-also"></a><span data-ttu-id="1d497-175">Viz také</span><span class="sxs-lookup"><span data-stu-id="1d497-175">See also</span></span>

* [<span data-ttu-id="1d497-176">Výpočetní kontextu možnosti pro R Server v clusterech prostředí HDInsight</span><span class="sxs-lookup"><span data-stu-id="1d497-176">Compute context options for R Server on HDInsight clusters</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)
* [<span data-ttu-id="1d497-177">Možnosti služby Azure Storage pro R Server ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="1d497-177">Azure Storage options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-storage.md)

