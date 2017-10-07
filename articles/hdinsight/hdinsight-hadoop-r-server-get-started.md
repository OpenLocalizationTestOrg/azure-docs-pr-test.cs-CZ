---
title: "aaaGet začít s R serverem v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate platformě Apache Spark v clusteru HDInsight, který zahrnuje R Server a odeslat skript v jazyce R na clusteru hello."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/14/2017
ms.author: bradsev
ms.openlocfilehash: f7e418bbac48eee080a4b4cfbb33e246324ea5c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-using-r-server-on-hdinsight"></a><span data-ttu-id="dc775-103">Začínáme používat R Server ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="dc775-103">Get started using R Server on HDInsight</span></span>

<span data-ttu-id="dc775-104">HDInsight zahrnuje serveru R toobe možnost integrovat do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dc775-104">HDInsight includes an R Server option toobe integrated into your HDInsight cluster.</span></span> <span data-ttu-id="dc775-105">Tato možnost umožňuje skripty R toouse Spark a MapReduce toorun distribuované výpočty.</span><span class="sxs-lookup"><span data-stu-id="dc775-105">This option allows R scripts toouse Spark and MapReduce toorun distributed computations.</span></span> <span data-ttu-id="dc775-106">V tomto dokumentu zjistíte, jak toocreate serveru R na clusteru HDInsight a poté spustit R skript, který ukazuje, jak pomocí Spark pro distribuovaných R výpočtů.</span><span class="sxs-lookup"><span data-stu-id="dc775-106">In this document, you learn how toocreate an R Server on HDInsight cluster and then run an R script that demonstrates using Spark for distributed R computations.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="dc775-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dc775-107">Prerequisites</span></span>

* <span data-ttu-id="dc775-108">**Předplatné Azure:** Než začnete tento kurz, musíte mít předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="dc775-108">**An Azure subscription**: Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="dc775-109">Článek přejděte toohello [bezplatná zkušební verze Microsoft Azure získat](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="dc775-109">Go toohello article [Get Microsoft Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) for more information.</span></span>
* <span data-ttu-id="dc775-110">**Klient Secure Shell (SSH)**: slouží klienta SSH tooremotely připojení clusteru HDInsight toohello a spusťte příkazy přímo na clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-110">**A Secure Shell (SSH) client**: An SSH client is used tooremotely connect toohello HDInsight cluster and run commands directly on hello cluster.</span></span> <span data-ttu-id="dc775-111">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="dc775-111">For more information, see [Use SSH with HDInsight.](hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>
* <span data-ttu-id="dc775-112">**(Volitelné) klíče SSH**: můžete zabezpečit hello SSH účet používaný tooconnect toohello clusteru pomocí hesla nebo veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="dc775-112">**SSH keys (optional)**: You can secure hello SSH account used tooconnect toohello cluster using either a password or a public key.</span></span> <span data-ttu-id="dc775-113">Pomocí hesla je jednodušší a umožňuje vám tooget spustit bez nutnosti toocreate pár veřejného a privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="dc775-113">Using a password is easier, and allows you tooget started without having toocreate a public/private key pair.</span></span> <span data-ttu-id="dc775-114">Použití klíče je ale bezpečnější.</span><span class="sxs-lookup"><span data-stu-id="dc775-114">However, using a key is more secure.</span></span>

> [!NOTE]
> <span data-ttu-id="dc775-115">Hello kroky v tomto dokumentu předpokládají, že používáte heslo.</span><span class="sxs-lookup"><span data-stu-id="dc775-115">hello steps in this document assume that you are using a password.</span></span>


## <a name="automated-cluster-creation"></a><span data-ttu-id="dc775-116">Automatizované vytváření clusterů</span><span class="sxs-lookup"><span data-stu-id="dc775-116">Automated cluster creation</span></span>

<span data-ttu-id="dc775-117">Můžete automatizovat vytváření hello serverů R HDInsight pomocí Azure Resource Manager šablony, hello SDK a také prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dc775-117">You can automate hello creation of HDInsight R Servers using Azure Resource Manager templates, hello SDK, and also PowerShell.</span></span>

* <span data-ttu-id="dc775-118">toocreate serveru R pomocí šablony Správa prostředků Azure, najdete v části [nasazení clusteru služby HDInsight serveru R](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).</span><span class="sxs-lookup"><span data-stu-id="dc775-118">toocreate an R Server using an Azure Resource Management template, see [Deploy an R server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).</span></span>
* <span data-ttu-id="dc775-119">toocreate na serveru R pomocí hello .NET SDK najdete v části [vytvořit clustery se systémem Linux v HDInsight pomocí hello .NET SDK.](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span><span class="sxs-lookup"><span data-stu-id="dc775-119">toocreate an R Server using hello .NET SDK, see [create Linux-based clusters in HDInsight using hello .NET SDK.](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)</span></span>
* <span data-ttu-id="dc775-120">toodeploy R Server pomocí prostředí powershell, najdete v článku hello na [vytváření R Server v HDInsight pomocí prostředí PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="dc775-120">toodeploy R Server using powershell, see hello article on [creating an R Server on HDInsight with PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).</span></span>


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-hello-cluster-using-hello-azure-portal"></a><span data-ttu-id="dc775-121">Vytvoření clusteru hello pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dc775-121">Create hello cluster using hello Azure portal</span></span>

1. <span data-ttu-id="dc775-122">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dc775-122">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="dc775-123">Vyberte **NOVÝ** -> **Inteligentní funkce a analýzy** -> **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="dc775-123">Select **NEW** -> **Intelligence + Analytics**, -> **HDInsight**.</span></span>

    ![Obrázek vytváření nového clusteru](./media/hdinsight-hadoop-r-server-get-started/newcluster.png)

3. <span data-ttu-id="dc775-125">V hello **rychle vytvořit** uživatele, zadejte název pro hello cluster v hello **název clusteru** pole.</span><span class="sxs-lookup"><span data-stu-id="dc775-125">In hello **Quick create** experience, enter a name for hello cluster in hello **Cluster Name** field.</span></span> <span data-ttu-id="dc775-126">Pokud máte víc předplatných Azure, použijte hello **předplatné** položka tooselect hello jeden chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="dc775-126">If you have multiple Azure subscriptions, use hello **Subscription** entry tooselect hello one you want toouse.</span></span>

    ![Výběr názvu clusteru a předplatného](./media/hdinsight-hadoop-r-server-get-started/clustername.png)

4. <span data-ttu-id="dc775-128">Vyberte **clusteru typ** tooopen hello **konfigurace clusteru** okno.</span><span class="sxs-lookup"><span data-stu-id="dc775-128">Select **Cluster type** tooopen hello **Cluster configuration** blade.</span></span> <span data-ttu-id="dc775-129">Na hello **konfigurace clusteru** okně vyberte hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="dc775-129">On hello **Cluster Configuration** blade, select hello following options:</span></span>

    * <span data-ttu-id="dc775-130">**Typ clusteru:** R Server</span><span class="sxs-lookup"><span data-stu-id="dc775-130">**Cluster Type**: R Server</span></span>
    * <span data-ttu-id="dc775-131">**Verze**: Vyberte hello verzi tooinstall R Server v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-131">**Version**: select hello version of R Server tooinstall on hello cluster.</span></span> <span data-ttu-id="dc775-132">Hello verzi aktuálně k dispozici je ***R Server 9.1 (HDI 3.6)***.</span><span class="sxs-lookup"><span data-stu-id="dc775-132">hello version currently available is ***R Server 9.1 (HDI 3.6)***.</span></span> <span data-ttu-id="dc775-133">Poznámky k verzi pro hello dostupné verze serveru R jsou k dispozici [zde](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).</span><span class="sxs-lookup"><span data-stu-id="dc775-133">Release notes for hello available versions of R Server are available [here](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).</span></span>
    * <span data-ttu-id="dc775-134">**R Studio community edition pro počítače s R Server**: Tento IDE založené na prohlížeči je nainstalována ve výchozím nastavení v uzlu edge hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-134">**R Studio community edition for R Server**: this browser-based IDE is installed by default on hello edge node.</span></span> <span data-ttu-id="dc775-135">Pokud si přejete toonot nainstalováno a potom zrušte zaškrtnutí políčka hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-135">If you would prefer toonot have it installed, then uncheck hello check box.</span></span> <span data-ttu-id="dc775-136">Pokud se rozhodnete toohave je nainstalovat, hello adresu URL pro přístup k hello přihlášení Rstudia Server se nachází v okně portálu aplikace pro váš cluster, je po vytvoření.</span><span class="sxs-lookup"><span data-stu-id="dc775-136">If you choose toohave it installed, hello URL for accessing hello RStudio Server login is found on a portal application blade for your cluster once it’s been created.</span></span>
    * <span data-ttu-id="dc775-137">Ponechte hello další možnosti hello výchozí hodnoty a použít hello **vyberte** toosave hello clusteru typ tlačítka.</span><span class="sxs-lookup"><span data-stu-id="dc775-137">Leave hello other options at hello default values and use hello **Select** button toosave hello cluster type.</span></span>

        ![Snímek obrazovky okna Typ clusteru](./media/hdinsight-hadoop-r-server-get-started/clustertypeconfig.png)

5. <span data-ttu-id="dc775-139">Zadejte **Uživatelské jméno přihlášení clusteru** a **Heslo přihlášení clusteru**.</span><span class="sxs-lookup"><span data-stu-id="dc775-139">Enter a **Cluster login username** and **Cluster login password**.</span></span>

    <span data-ttu-id="dc775-140">Zadejte **Uživatelské jméno SSH**.</span><span class="sxs-lookup"><span data-stu-id="dc775-140">Specify an **SSH Username**.</span></span> <span data-ttu-id="dc775-141">SSH je použité tooremotely toohello clusteru pomocí připojit **Secure Shell (SSH)** klienta.</span><span class="sxs-lookup"><span data-stu-id="dc775-141">SSH is used tooremotely connect toohello cluster using a **Secure Shell (SSH)** client.</span></span> <span data-ttu-id="dc775-142">Uživatel SSH hello můžete zadat v tomto dialogovém okně nebo po vytvoření clusteru hello (na kartě Konfigurace hello hello clusteru).</span><span class="sxs-lookup"><span data-stu-id="dc775-142">You can either specify hello SSH user in this dialog or after hello cluster has been created (in hello Configuration tab for hello cluster).</span></span> <span data-ttu-id="dc775-143">R Server je nakonfigurovaný tooexpect **uživatelské jméno SSH** z "remoteuser".</span><span class="sxs-lookup"><span data-stu-id="dc775-143">R Server is configured tooexpect an **SSH username** of “remoteuser”.</span></span>  <span data-ttu-id="dc775-144">**Pokud chcete použít jiné uživatelské jméno, je nutné provést další krok po vytvoření clusteru hello.**</span><span class="sxs-lookup"><span data-stu-id="dc775-144">**If you use a different username, you must perform an additional step after hello cluster is created.**</span></span>

    <span data-ttu-id="dc775-145">Ponechejte pole hello kontrola **použijte stejné heslo jako přihlašovací údaje clusteru** toouse **heslo** psaní hello ověřování Pokud dáváte přednost použití veřejného klíče.</span><span class="sxs-lookup"><span data-stu-id="dc775-145">Leave hello box checked for **Use same password as cluster login** toouse **PASSWORD** as hello authentication type unless you prefer use of a public key.</span></span>  <span data-ttu-id="dc775-146">Je nutné pár veřejného a privátního klíče tooaccess R Server v clusteru hello prostřednictvím vzdáleného klienta, jako například RTVS, Rstudia nebo jiné plochy IDE.</span><span class="sxs-lookup"><span data-stu-id="dc775-146">You need a public/private key pair tooaccess R Server on hello cluster via a remote client as, for example, RTVS, RStudio or another desktop IDE.</span></span> <span data-ttu-id="dc775-147">Pokud instalujete hello edice community Rstudia serveru, je nutné toochoose zadat heslo SSH.</span><span class="sxs-lookup"><span data-stu-id="dc775-147">If you install hello RStudio Server community edition, you need toochoose an SSH password.</span></span>     

    <span data-ttu-id="dc775-148">toocreate a používání páru veřejného a privátního klíče RSA, zrušte zaškrtnutí políčka **použijte stejné heslo jako přihlašovací údaje clusteru** a pak vyberte **veřejný klíč** a pokračovat následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="dc775-148">toocreate and use a public/private key pair, uncheck **Use same password as cluster login** and then select **PUBLIC KEY** and proceed as follows.</span></span> <span data-ttu-id="dc775-149">Tyto pokyny předpokládají, že máte nainstalované prostředí Cygwin, ve kterém je dostupný příkaz ssh-keygen nebo ekvivalentní příkaz.</span><span class="sxs-lookup"><span data-stu-id="dc775-149">These instructions assume that you have Cygwin with ssh-keygen or an equivalent installed.</span></span>

    * <span data-ttu-id="dc775-150">Generování páru veřejného a privátního klíče RSA z příkazového řádku hello na svém přenosném počítači:</span><span class="sxs-lookup"><span data-stu-id="dc775-150">Generate a public/private key pair from hello command prompt on your laptop:</span></span>

        <span data-ttu-id="dc775-151">ssh-keygen -t rsa -b 2048</span><span class="sxs-lookup"><span data-stu-id="dc775-151">ssh-keygen -t rsa -b 2048</span></span>

    * <span data-ttu-id="dc775-152">Postupujte podle výzvy tooname hello soubor klíče a potom zadejte přístupové heslo pro zvýšení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="dc775-152">Follow hello prompt tooname a key file and then enter a passphrase for added security.</span></span> <span data-ttu-id="dc775-153">Na obrazovce by měl vypadat podobně jako hello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="dc775-153">Your screen should look something like hello following image:</span></span>

        ![Příkazový řádek SSH v systému Windows](./media/hdinsight-hadoop-r-server-get-started/sshcmdline.png)

    * <span data-ttu-id="dc775-155">Tento příkaz vytvoří soubor privátního klíče a soubor veřejného klíče v části hello název < privátní klíč filename > .pub, například furiosa a furiosa.pub.</span><span class="sxs-lookup"><span data-stu-id="dc775-155">This command creates a private key file and a public key file under hello name <private-key-filename>.pub, for example furiosa and furiosa.pub.</span></span>

        ![Adresář SSH](./media/hdinsight-hadoop-r-server-get-started/dir.png)

    * <span data-ttu-id="dc775-157">Pak zadejte hello soubor veřejného klíče (&#42;. protokol pub) při přiřazování HDI clusteru přihlašovací údaje a nakonec potvrďte skupinu prostředků a oblasti a vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="dc775-157">Then specify hello public key file (&#42;.pub) when assigning HDI cluster credentials and finally confirm your resource group and region and select **Next**.</span></span>

        ![Okno Přihlašovací údaje](./media/hdinsight-hadoop-r-server-get-started/publickeyfile.png)  

   * <span data-ttu-id="dc775-159">Změna oprávnění u hello privátní keyfile na svém přenosném počítači:</span><span class="sxs-lookup"><span data-stu-id="dc775-159">Change permissions on hello private keyfile on your laptop:</span></span>

        <span data-ttu-id="dc775-160">chmod 600 <název_souboru_privátního_klíče></span><span class="sxs-lookup"><span data-stu-id="dc775-160">chmod 600 <private-key-filename></span></span>

   * <span data-ttu-id="dc775-161">Použijte hello soubor privátního klíče pomocí protokolu SSH pro vzdálené přihlášení:</span><span class="sxs-lookup"><span data-stu-id="dc775-161">Use hello private key file with SSH for remote login:</span></span>

        <span data-ttu-id="dc775-162">ssh –i <název_souboru_privátního_klíče> remoteuser@<hostname public ip></span><span class="sxs-lookup"><span data-stu-id="dc775-162">ssh –i <private-key-filename> remoteuser@<hostname public ip></span></span>

      <span data-ttu-id="dc775-163">Nebo jako součást definice hello vaší Hadoop Spark výpočetní kontext pro R Server hello klienta.</span><span class="sxs-lookup"><span data-stu-id="dc775-163">Or, as part hello definition of your Hadoop Spark compute context for R Server on hello client.</span></span> <span data-ttu-id="dc775-164">V tématu hello **pomocí Microsoft R Server jako klienta Hadoop** pododdílu v [vytvoření kontextu výpočetní pro Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).</span><span class="sxs-lookup"><span data-stu-id="dc775-164">See hello **Using Microsoft R Server as a Hadoop Client** subsection in [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark).</span></span>

6. <span data-ttu-id="dc775-165">Hello rychle vytvořit přechody, se kterými toohello **úložiště** okno tooselect hello účet úložiště toobe nastavení používá pro primárního umístění hello hello HDFS souboru systému používaného clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-165">hello quick create transitions you toohello **Storage** blade tooselect hello storage account settings toobe used for hello primary location of hello HDFS file system used by hello cluster.</span></span> <span data-ttu-id="dc775-166">Vyberte nový nebo existující účet služby Azure Storage nebo existující účet Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="dc775-166">Select either a new or existing Azure Storage account or an existing Data Lake Storage account.</span></span>

    - <span data-ttu-id="dc775-167">Pokud vyberete účet služby Azure Storage, stávající účet úložiště je vybrána výběrem **vyberte účet úložiště** a potom vybrat příslušný účet hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-167">If you select an Azure Storage account, an existing storage account is selected by choosing **Select a storage account** and then selecting hello relevant account.</span></span> <span data-ttu-id="dc775-168">Vytvořit nový účet pomocí hello **vytvořit nový** odkaz v hello **vyberte účet úložiště** části.</span><span class="sxs-lookup"><span data-stu-id="dc775-168">Create a new account using hello **Create New** link in hello **Select a storage account** section.</span></span>

      > [!NOTE]
      > <span data-ttu-id="dc775-169">Pokud vyberete **nový** musíte zadat název nového účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-169">If you select **New** you must enter a name for hello new storage account.</span></span> <span data-ttu-id="dc775-170">Pokud byla přijata hello název se zobrazí zelené zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="dc775-170">A green check appears if hello name is accepted.</span></span>

      <span data-ttu-id="dc775-171">Hello **výchozí kontejner** výchozí název toohello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="dc775-171">hello **Default Container** defaults toohello name of hello cluster.</span></span> <span data-ttu-id="dc775-172">Toto výchozí nastavení ponechte jako hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-172">Leave this default as hello value.</span></span>

      <span data-ttu-id="dc775-173">Pokud jste vybrali novou možnost účet úložiště výzva tooselect **umístění** je daný tooselect které oblasti toocreate hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="dc775-173">If a new storage account option was selected a prompt tooselect **Location** is given tooselect which region toocreate hello storage account.</span></span>  

         ![Okno zdroje dat](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > <span data-ttu-id="dc775-175">Výběr hello umístění pro zdroj dat výchozí hello také nastaví hello umístění hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dc775-175">Selecting hello location for hello default data source  also sets hello location of hello HDInsight cluster.</span></span> <span data-ttu-id="dc775-176">Hello clusteru a výchozí zdroj dat musí být v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="dc775-176">hello cluster and default data source must be in hello same region.</span></span>

    - <span data-ttu-id="dc775-177">Pokud chcete toouse existující Data Lake Store, pak vyberte hello toouse účtu ADLS úložiště clusteru a ten přidejte hello *přidat* identity tooyour clusteru tooallow přístup toohello úložiště.</span><span class="sxs-lookup"><span data-stu-id="dc775-177">If you want toouse an existing Data Lake Store, then select hello ADLS storage account toouse and add hello cluster *ADD* identity tooyour cluster tooallow access toohello store.</span></span> <span data-ttu-id="dc775-178">Další informace o tomto procesu najdete v tématu [Vytvoření clusteru HDInsight se službou Data Lake Store pomocí webu Azure Portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).</span><span class="sxs-lookup"><span data-stu-id="dc775-178">For more information on this process, see [Creating HDInsight cluster with Data Lake Store using Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).</span></span>

    <span data-ttu-id="dc775-179">Použití hello **vyberte** konfigurace zdroje dat hello toosave tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dc775-179">Use hello **Select** button toosave hello data source configuration.</span></span>


7. <span data-ttu-id="dc775-180">Hello **Souhrn** okno pak zobrazí toovalidate všechna nastavení.</span><span class="sxs-lookup"><span data-stu-id="dc775-180">hello **Summary** blade then displays toovalidate all your settings.</span></span> <span data-ttu-id="dc775-181">Zde můžete změnit vaše **velikost clusteru** toomodify hello počet serverů v clusteru a také zadat jakýkoli **skript akce** chcete toorun.</span><span class="sxs-lookup"><span data-stu-id="dc775-181">Here you can change your **Cluster size** toomodify hello number of servers in your cluster and also specify any **Script actions** you want toorun.</span></span> <span data-ttu-id="dc775-182">Pokud si nejste jisti, že je potřeba větší cluster, nechte hello počet uzlů pracovního procesu na výchozí hello `4`.</span><span class="sxs-lookup"><span data-stu-id="dc775-182">Unless you know that you need a larger cluster, leave hello number of worker nodes at hello default of `4`.</span></span> <span data-ttu-id="dc775-183">Hello odhadované náklady na hello clusteru se zobrazí v okně hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-183">hello estimated cost of hello cluster is shown within hello blade.</span></span>

    ![souhrn clusteru](./media/hdinsight-hadoop-r-server-get-started/clustersummary.png)

   > [!NOTE]
   > <span data-ttu-id="dc775-185">V případě potřeby můžete změnit velikost clusteru později prostřednictvím hello portálu (**clusteru** -> **nastavení** -> **škálování clusteru**) tooincrease nebo zmenšení hello počet uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="dc775-185">If needed, you can resize your cluster later through hello Portal (**Cluster** -> **Settings** -> **Scale Cluster**) tooincrease or decrease hello number of worker nodes.</span></span>  <span data-ttu-id="dc775-186">Tato změna velikosti může být užitečná pro volnoběhu dolů hello clusteru, když není používán, nebo pro přidání hello požadavků na kapacitu toomeet větší úloh.</span><span class="sxs-lookup"><span data-stu-id="dc775-186">This resizing can be useful for idling down hello cluster when not in use, or for adding capacity toomeet hello needs of larger tasks.</span></span>
   >
   >

   <span data-ttu-id="dc775-187">Některé faktory tookeep pamatujte při Změna velikosti vašeho clusteru, hello datové uzly a hello hraniční uzel patří:</span><span class="sxs-lookup"><span data-stu-id="dc775-187">Some factors tookeep in mind when sizing your cluster, hello data nodes, and hello edge node include:</span></span>  

   * <span data-ttu-id="dc775-188">Hello výkon distribuovaných R Server analýzy na Spark je přímo úměrná toohello počet uzlů pracovního procesu, když hello dat je velká.</span><span class="sxs-lookup"><span data-stu-id="dc775-188">hello performance of distributed R Server analyses on Spark is proportional toohello number of worker nodes when hello data is large.</span></span>  

   * <span data-ttu-id="dc775-189">výkon Hello R Server analýzy je lineární hello velikost dat probíhá analýza.</span><span class="sxs-lookup"><span data-stu-id="dc775-189">hello performance of R Server analyses is linear in hello size of data being analyzed.</span></span> <span data-ttu-id="dc775-190">Například:</span><span class="sxs-lookup"><span data-stu-id="dc775-190">For example:</span></span>  

     * <span data-ttu-id="dc775-191">Pro malé toomodest data je vhodné, když analyzovat v kontextu místního výpočetního uzlu edge hello výkonu.</span><span class="sxs-lookup"><span data-stu-id="dc775-191">For small toomodest data, performance is best when analyzed in a local compute context on hello edge node.</span></span>  <span data-ttu-id="dc775-192">Další informace o hello scénáře pod nimiž hello místní a Spark výpočetní kontexty nejlépe fungovat, najdete v části možnosti výpočetní kontext pro R Server v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dc775-192">For more information on hello scenarios under which hello local and Spark compute contexts work best, see  Compute context options for R Server on HDInsight.</span></span><br>
     * <span data-ttu-id="dc775-193">Pokud přihlášení toohello hraniční uzel a spustit R skript, pak všechny ale hello ScaleR rx – funkce provést <strong>místně</strong> na uzlu edge hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-193">If you log in toohello edge node and run your R script, then all but hello ScaleR rx-functions are executed <strong>locally</strong> on hello edge node.</span></span> <span data-ttu-id="dc775-194">Proto hello paměť a počet jader hello hraniční uzel by měly mít velikost odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="dc775-194">So hello memory and number of cores of hello edge node should be sized accordingly.</span></span> <span data-ttu-id="dc775-195">Hello totéž platí, pokud používáte R Server na HDI vzdálené výpočetní kontext z vašeho přenosného počítače.</span><span class="sxs-lookup"><span data-stu-id="dc775-195">hello same applies if you use R Server on HDI as a remote compute context from your laptop.</span></span>

     ![Okno s cenovými úrovněmi uzlů](./media/hdinsight-hadoop-r-server-get-started/pricingtier.png)

     <span data-ttu-id="dc775-197">Použití hello **vyberte** tlačítko toosave hello uzlu ceny konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dc775-197">Use hello **Select** button toosave hello node pricing configuration.</span></span>

   <span data-ttu-id="dc775-198">Je uvedený také odkaz na **stažení šablony a parametrů**.</span><span class="sxs-lookup"><span data-stu-id="dc775-198">There is also a link for **Download template and parameters**.</span></span> <span data-ttu-id="dc775-199">Klikněte na tento odkaz toodisplay skripty, které lze použít tooautomate hello vytvoření clusteru s konfigurací vybrané hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-199">Click on this link toodisplay scripts that can be used tooautomate hello creation of a cluster with hello selected configuration.</span></span> <span data-ttu-id="dc775-200">Tyto skripty jsou dostupné i z hello Azure portálu položka pro váš cluster po jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="dc775-200">These scripts are also available from hello Azure portal entry for your cluster once it has been created.</span></span>

   > [!NOTE]
   > <span data-ttu-id="dc775-201">Jak dlouho trvá nějakou dobu toobe hello clusteru vytvořený, obvykle přibližně 20 minut.</span><span class="sxs-lookup"><span data-stu-id="dc775-201">It takes some time for hello cluster toobe created, usually around 20 minutes.</span></span> <span data-ttu-id="dc775-202">Použít hello dlaždici na úvodní panel hello nebo hello **oznámení** položku na hello levé toocheck stránku hello v procesu vytváření hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-202">Use hello tile on hello Startboard, or hello **Notifications** entry on hello left of hello page toocheck on hello creation process.</span></span>
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-toorstudio-server"></a><span data-ttu-id="dc775-203">Připojit tooRStudio serveru</span><span class="sxs-lookup"><span data-stu-id="dc775-203">Connect tooRStudio Server</span></span>

<span data-ttu-id="dc775-204">Pokud jste vybrali tooinclude edice community Rstudia Server v instalaci, můžete přistupovat hello Rstudia přihlášení přes dvě různé metody.</span><span class="sxs-lookup"><span data-stu-id="dc775-204">If you’ve chosen tooinclude RStudio Server community edition in your installation, then you can access hello RStudio login via two different methods.</span></span>

1. <span data-ttu-id="dc775-205">Přejděte toohello následující adresu URL (kde **CLUSTERNAME** je název hello hello clusteru, které jste vytvořili):</span><span class="sxs-lookup"><span data-stu-id="dc775-205">Go toohello following URL (where **CLUSTERNAME** is hello name of hello cluster you've created):</span></span>

    <span data-ttu-id="dc775-206">https://**NÁZEV_CLUSTERU**.azurehdinsight.net/rstudio/</span><span class="sxs-lookup"><span data-stu-id="dc775-206">https://**CLUSTERNAME**.azurehdinsight.net/rstudio/</span></span>

2. <span data-ttu-id="dc775-207">Otevřete položku hello pro váš cluster v hello portál Azure, vyberte hello **řídicí panely serveru R** rychlé odkazu a potom vyberete hello **řídicí panel Studio R**:</span><span class="sxs-lookup"><span data-stu-id="dc775-207">Open hello entry for your cluster in hello Azure portal, select hello **R Server Dashboards** quick link and then selecting hello **R Studio Dashboard**:</span></span>

     ![Řídicí panel studio hello R přístup](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Řídicí panel studio hello R přístup](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > <span data-ttu-id="dc775-210">Bez ohledu na použité metodě hello hello při prvním přihlášení je třeba tooauthenticate dvakrát.</span><span class="sxs-lookup"><span data-stu-id="dc775-210">Regardless of hello method used, hello first time you log in you need tooauthenticate twice.</span></span>  <span data-ttu-id="dc775-211">Při prvním ověřování hello, zadejte hello *clusteru správce userid* a *heslo*.</span><span class="sxs-lookup"><span data-stu-id="dc775-211">At hello first authentication, provide hello *cluster Admin userid* and *password*.</span></span> <span data-ttu-id="dc775-212">Hello druhého řádku, zadejte hello *SSH userid* a *heslo*.</span><span class="sxs-lookup"><span data-stu-id="dc775-212">At hello second prompt, provide hello *SSH userid* and *password*.</span></span> <span data-ttu-id="dc775-213">Následné protokolu in vyžadují jenom hello *heslo SSH* a *userid*.</span><span class="sxs-lookup"><span data-stu-id="dc775-213">Subsequent log ins only require hello *SSH password* and *userid*.</span></span>

<a name="connect-to-edge-node"></a>
## <a name="connect-toohello-r-server-edge-node"></a><span data-ttu-id="dc775-214">Připojit okrajovému uzlu serveru R toohello</span><span class="sxs-lookup"><span data-stu-id="dc775-214">Connect toohello R Server edge node</span></span>

<span data-ttu-id="dc775-215">Připojte okrajovému uzlu serveru tooR hello clusteru HDInsight pomocí protokolu SSH pomocí příkazu hello:</span><span class="sxs-lookup"><span data-stu-id="dc775-215">Connect tooR Server edge node of hello HDInsight cluster using SSH with hello command:</span></span>

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> <span data-ttu-id="dc775-216">Můžete najít hello `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` adresu v hello portálu Azure, pak výběrem clusteru **všechna nastavení** -> **aplikace** -> **RServer**.</span><span class="sxs-lookup"><span data-stu-id="dc775-216">You can find hello `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` address in hello Azure portal by selecting your cluster then **All Settings** -> **Apps** -> **RServer**.</span></span> <span data-ttu-id="dc775-217">Zobrazí informace o koncovém SSH pro hello hraniční uzel hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-217">This displays hello SSH Endpoint information for hello edge node.</span></span>
>
> ![Obrázek hello SSH Endpoint pro hello hraniční uzel](./media/hdinsight-hadoop-r-server-get-started/sshendpoint.png)
>
>

<span data-ttu-id="dc775-219">Pokud jste použili toosecure hesla účtu uživatele SSH, jste výzvami tooenter ho.</span><span class="sxs-lookup"><span data-stu-id="dc775-219">If you used a password toosecure your SSH user account, you are prompted tooenter it.</span></span> <span data-ttu-id="dc775-220">Pokud jste použili veřejný klíč, můžete mít toouse hello `-i` parametr toospecify hello odpovídající soukromý klíč.</span><span class="sxs-lookup"><span data-stu-id="dc775-220">If you used a public key, you may have toouse hello `-i` parameter toospecify hello matching private key.</span></span> <span data-ttu-id="dc775-221">Například:</span><span class="sxs-lookup"><span data-stu-id="dc775-221">For example:</span></span>

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="dc775-222">Další informace najdete v tématu [tooHDInsight (Hadoop) pomocí protokolu SSH připojit](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="dc775-222">For more information, see [Connect tooHDInsight (Hadoop) using SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="dc775-223">Po připojení přijedete do výzva podobné následující toohello:</span><span class="sxs-lookup"><span data-stu-id="dc775-223">Once connected, you arrive at a prompt similar toohello following:</span></span>

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a><span data-ttu-id="dc775-224">Povolení několika souběžných uživatelů</span><span class="sxs-lookup"><span data-stu-id="dc775-224">Enable multiple concurrent users</span></span>

<span data-ttu-id="dc775-225">Přidáním více uživatelů pro hello hraniční uzel, na které hello Rstudia komunity běží verze můžete povolit více souběžných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="dc775-225">You can enable multiple concurrent users by adding more users for hello edge node on which hello RStudio community version runs.</span></span>

<span data-ttu-id="dc775-226">Při vytváření clusteru HDInsight musíte zadat dva uživatele – uživatele HTTP a uživatele SSH:</span><span class="sxs-lookup"><span data-stu-id="dc775-226">When you create an HDInsight cluster, you must provide two users, an HTTP user and an SSH user:</span></span>

![Souběžný uživatel 1](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-1.png)

- <span data-ttu-id="dc775-228">**Uživatelské jméno přihlášení clusteru**: uživatelé HTTP pro ověřování prostřednictvím brány hello HDInsight, který je použité tooprotect hello HDInsight clustery, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="dc775-228">**Cluster login username**: an HTTP user for authentication through hello HDInsight gateway that is used tooprotect hello HDInsight clusters you created.</span></span> <span data-ttu-id="dc775-229">Tento uživatel HTTP je použité tooaccess hello uživatelského rozhraní Ambari, uživatelském rozhraní YARN, jakož i ostatní součásti uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="dc775-229">This HTTP user is used tooaccess hello Ambari UI, YARN UI, as well as other UI components.</span></span>
- <span data-ttu-id="dc775-230">**Uživatelské jméno zabezpečené Shell (SSH)**: SSH uživatele tooaccess hello clusteru prostřednictvím zabezpečeného prostředí.</span><span class="sxs-lookup"><span data-stu-id="dc775-230">**Secure Shell (SSH) username**: an SSH user tooaccess hello cluster through secure shell.</span></span> <span data-ttu-id="dc775-231">Tento uživatel je uživatelem v systému Linux hello hlavních uzlech, pracovní uzly a uzly okraj hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-231">This user is a user in hello Linux system for all hello head nodes, worker nodes, and edge nodes.</span></span> <span data-ttu-id="dc775-232">Abyste mohli používat zabezpečený prostředí tooaccess hello uzly v clusteru s podporou vzdálené.</span><span class="sxs-lookup"><span data-stu-id="dc775-232">So you can use secure shell tooaccess any of hello nodes in a remote cluster.</span></span>

<span data-ttu-id="dc775-233">Hello R Studio serveru Community verze použitá v hello Microsoft R Server v clusteru HDInsight typ akceptuje pouze Linux uživatelské jméno a heslo jako mechanismus přihlášení.</span><span class="sxs-lookup"><span data-stu-id="dc775-233">hello R Studio Server Community version used in hello Microsoft R Server on HDInsight type cluster accepts only Linux username and password as a login mechanism.</span></span> <span data-ttu-id="dc775-234">Nepodporuje předávání tokenů.</span><span class="sxs-lookup"><span data-stu-id="dc775-234">It does not support passing tokens.</span></span> <span data-ttu-id="dc775-235">Pokud jste vytvořili nový cluster a chcete toouse R Studio tooaccess, je nutné toolog v dvakrát.</span><span class="sxs-lookup"><span data-stu-id="dc775-235">So if you have created a new cluster and want toouse R Studio tooaccess it, you need toolog in twice.</span></span>

- <span data-ttu-id="dc775-236">První přihlášení pomocí přihlašovacích údajů uživatele hello HTTP prostřednictvím hello HDInsight brány:</span><span class="sxs-lookup"><span data-stu-id="dc775-236">First log in using hello HTTP user credentials through hello HDInsight Gateway:</span></span> 

    ![Souběžný uživatel 2a](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2a.png)

- <span data-ttu-id="dc775-238">Pak použijte toolog přihlašovací údaje uživatele SSH hello tooRStudio:</span><span class="sxs-lookup"><span data-stu-id="dc775-238">Then use hello SSH user credentials toolog in tooRStudio:</span></span>
  
    ![Souběžný uživatel 2b](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2b.png)

<span data-ttu-id="dc775-240">V současné době je možné při zřizování clusteru HDInsight vytvořit pouze jeden uživatelský účet SSH.</span><span class="sxs-lookup"><span data-stu-id="dc775-240">Currently, only one SSH user account can be created when provisioning an HDInsight cluster.</span></span> <span data-ttu-id="dc775-241">Takže tooenable clusterů více uživatelů tooaccess Microsoft R serverem v HDInsight, potřebujeme toocreate další uživatele v hello systému Linux.</span><span class="sxs-lookup"><span data-stu-id="dc775-241">So tooenable multiple users tooaccess Microsoft R Server on HDInsight clusters, we need toocreate additional users in hello Linux system.</span></span>

<span data-ttu-id="dc775-242">Protože komunity Rstudia Server běží na clusteru hello hraniční uzel, existují zde několik kroků:</span><span class="sxs-lookup"><span data-stu-id="dc775-242">Because RStudio Server Community is running on hello cluster’s edge node, there are several steps here:</span></span>

1. <span data-ttu-id="dc775-243">Použít toolog uživatele SSH hello vytvořit v uzlu edge toohello</span><span class="sxs-lookup"><span data-stu-id="dc775-243">Use hello created SSH user toolog in toohello edge node</span></span>
2. <span data-ttu-id="dc775-244">Přidání dalších uživatelů Linuxu na hraničním uzlu</span><span class="sxs-lookup"><span data-stu-id="dc775-244">Add more Linux users in edge node</span></span>
3. <span data-ttu-id="dc775-245">Verze komunity Rstudia použití vytvořit uživateli hello</span><span class="sxs-lookup"><span data-stu-id="dc775-245">Use RStudio Community version with hello user created</span></span>

### <a name="step-1-use-hello-created-ssh-user-toolog-in-toohello-edge-node"></a><span data-ttu-id="dc775-246">Krok 1: Použití toolog uživatele SSH hello vytvořit v uzlu edge toohello</span><span class="sxs-lookup"><span data-stu-id="dc775-246">Step 1: Use hello created SSH user toolog in toohello edge node</span></span>

<span data-ttu-id="dc775-247">Stáhněte si nástroj žádné SSH (jako je například Putty) a použít hello existující SSH uživatele toolog v.</span><span class="sxs-lookup"><span data-stu-id="dc775-247">Download any SSH tool (such as Putty) and use hello existing SSH user toolog in.</span></span> <span data-ttu-id="dc775-248">Potom postupujte podle pokynů hello v [tooHDInsight (Hadoop) pomocí protokolu SSH připojit](hdinsight-hadoop-linux-use-ssh-unix.md) tooaccess hello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="dc775-248">Then follow hello instructions provided in [Connect tooHDInsight (Hadoop) using SSH](hdinsight-hadoop-linux-use-ssh-unix.md) tooaccess hello edge node.</span></span> <span data-ttu-id="dc775-249">Hello adresa edge uzlu serveru R na clusteru HDInsight: *clustername-ed-ssh.azurehdinsight.net*</span><span class="sxs-lookup"><span data-stu-id="dc775-249">hello edge node address for R Server on HDInsight cluster is: *clustername-ed-ssh.azurehdinsight.net*</span></span>


### <a name="step-2-add-more-linux-users-in-edge-node"></a><span data-ttu-id="dc775-250">Krok 2: Přidání dalších uživatelů Linuxu na hraničním uzlu</span><span class="sxs-lookup"><span data-stu-id="dc775-250">Step 2: Add more Linux users in edge node</span></span>

<span data-ttu-id="dc775-251">tooadd uživatele toohello hraniční uzel, spuštěním příkazů hello:</span><span class="sxs-lookup"><span data-stu-id="dc775-251">tooadd a user toohello edge node, execute hello commands:</span></span>

    sudo useradd yournewusername -m
    sudo passwd yourusername

<span data-ttu-id="dc775-252">Měli byste vidět hello položky vrátila následující:</span><span class="sxs-lookup"><span data-stu-id="dc775-252">You should see hello following items returned:</span></span> 

![Souběžný uživatel 3](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-3.png)

<span data-ttu-id="dc775-254">Po zobrazení výzvy k "aktuální heslo protokolu Kerberos:", stačí stisknout klávesu **Enter** tooignore ho.</span><span class="sxs-lookup"><span data-stu-id="dc775-254">When prompted for “Current Kerberos password:”, just press **Enter** tooignore it.</span></span> <span data-ttu-id="dc775-255">Hello `-m` možnost `useradd` příkaz znamená, že hello systému vytvoří domovské složky pro hello uživatele, který je vyžadován pro verze Rstudia komunity.</span><span class="sxs-lookup"><span data-stu-id="dc775-255">hello `-m` option in `useradd` command indicates that hello system will create a home folder for hello user, which is required for RStudio Community version.</span></span>


### <a name="step-3-use-rstudio-community-version-with-hello-user-created"></a><span data-ttu-id="dc775-256">Krok 3: Použití Rstudia komunity verze vytvořit uživateli hello</span><span class="sxs-lookup"><span data-stu-id="dc775-256">Step 3: Use RStudio Community version with hello user created</span></span>

<span data-ttu-id="dc775-257">Používejte toolog hello uživatelem vytvořené v tooRStudio:</span><span class="sxs-lookup"><span data-stu-id="dc775-257">Use hello user created toolog in tooRStudio:</span></span>

![Souběžný uživatel 4](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-4.png)

<span data-ttu-id="dc775-259">Všimněte si, že Rstudia označuje, že používáte hello nového uživatele (například tady *sshuser6*) toolog v clusteru hello:</span><span class="sxs-lookup"><span data-stu-id="dc775-259">Notice that RStudio indicates that you are using hello new user (here, for example, *sshuser6*) toolog in hello cluster:</span></span> 

![Souběžný uživatel 5](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-5.png)

<span data-ttu-id="dc775-261">Také můžete přihlásit pomocí přihlašovacích údajů původní hello (ve výchozím nastavení, je *sshuser*) z jiného okna prohlížeče současně.</span><span class="sxs-lookup"><span data-stu-id="dc775-261">You can also log in using hello original credentials (by default, it is *sshuser*) concurrently from another browser window.</span></span>

<span data-ttu-id="dc775-262">Můžete odeslat úlohu pomocí funkcí ScaleR.</span><span class="sxs-lookup"><span data-stu-id="dc775-262">You can submit a job using scaleR functions.</span></span> <span data-ttu-id="dc775-263">Tady je příklad hello příkazy používané toorun úlohy:</span><span class="sxs-lookup"><span data-stu-id="dc775-263">Here is an example of hello commands used toorun a job:</span></span>

    # Set hello HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data toohello tmp folder.
    remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

    # Set directory in bigDataDirRoot tooload hello data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create hello directory.
    rxHadoopMakeDir(inputDir)

    # Copy hello data from source tooinput.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define hello HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for hello airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all hello column names.
    varNames <- names(airlineColInfo)

    # Define hello text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define hello text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify hello formula toouse.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define hello Spark compute context.
    mySparkCluster <- RxSpark()

    # Set hello compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)


<span data-ttu-id="dc775-264">Všimněte si, že hello úlohy, odeslané jsou v různých uživatelská jména v uživatelském rozhraní YARN:</span><span class="sxs-lookup"><span data-stu-id="dc775-264">Notice that hello jobs submitted are under different user names in YARN UI:</span></span>

![Souběžný uživatel 6](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-6.png)

<span data-ttu-id="dc775-266">Všimněte si také že hello nově přidaní uživatelé nemají oprávnění kořenového v systému Linux, ale budou mít hello stejný přístup k souborům hello tooall v hello vzdálené HDFS a WASB úložiště.</span><span class="sxs-lookup"><span data-stu-id="dc775-266">Note also that hello newly added users do not have root privileges in Linux system, but they do have hello same access tooall hello files in hello remote HDFS and WASB storage.</span></span>


<a name="use-r-console"></a>
## <a name="use-hello-r-console"></a><span data-ttu-id="dc775-267">Pomocí konzoly hello R</span><span class="sxs-lookup"><span data-stu-id="dc775-267">Use hello R console</span></span>

1. <span data-ttu-id="dc775-268">Z relace SSH hello použijte následující příkaz toostart hello R konzoly hello:</span><span class="sxs-lookup"><span data-stu-id="dc775-268">From hello SSH session, use hello following command toostart hello R console:</span></span>  

        R

2. <span data-ttu-id="dc775-269">Měli byste vidět výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="dc775-269">You should see output similar toohello following:</span></span>
    
    <span data-ttu-id="dc775-270">R verze 3.2.2 (2015-08-14)--"Bezpečnost ještě efektivněji" Copyright (C) 2015 hello R Foundation pro statistické výpočty platformy: x86_64-pc-linux-gnu (64 bitů)</span><span class="sxs-lookup"><span data-stu-id="dc775-270">R version 3.2.2 (2015-08-14) -- "Fire Safety"  Copyright (C) 2015 hello R Foundation for Statistical Computing  Platform: x86_64-pc-linux-gnu (64-bit)</span></span>

    <span data-ttu-id="dc775-271">R je bezplatný software a nevztahuje se na něj ŽÁDNÁ ZÁRUKA.</span><span class="sxs-lookup"><span data-stu-id="dc775-271">R is free software and comes with ABSOLUTELY NO WARRANTY.</span></span>
    <span data-ttu-id="dc775-272">Jsou úvodní tooredistribute ho za určitých podmínek.</span><span class="sxs-lookup"><span data-stu-id="dc775-272">You are welcome tooredistribute it under certain conditions.</span></span>
    <span data-ttu-id="dc775-273">Podrobnosti o distribuci zobrazíte zadáním license() nebo licence().</span><span class="sxs-lookup"><span data-stu-id="dc775-273">Type 'license()' or 'licence()' for distribution details.</span></span>

    <span data-ttu-id="dc775-274">Podpora přirozeného jazyka, ale spuštění v anglickém národním prostředí</span><span class="sxs-lookup"><span data-stu-id="dc775-274">Natural language support but running in an English locale</span></span>

    <span data-ttu-id="dc775-275">R je projekt spolupráce s mnoha přispěvateli.</span><span class="sxs-lookup"><span data-stu-id="dc775-275">R is a collaborative project with many contributors.</span></span>
    <span data-ttu-id="dc775-276">Zadejte 'contributors()' Další informace a 'citation()' na tom, jak toocite R nebo R balíčky v publikacích.</span><span class="sxs-lookup"><span data-stu-id="dc775-276">Type 'contributors()' for more information and 'citation()' on how toocite R or R packages in publications.</span></span>

    <span data-ttu-id="dc775-277">Typ 'demo()' pro některé ukázky, 'help()' online nápovědu nebo 'help.start()' pro toohelp rozhraní prohlížeče HTML.</span><span class="sxs-lookup"><span data-stu-id="dc775-277">Type 'demo()' for some demos, 'help()' for on-line help, or 'help.start()' for an HTML browser interface toohelp.</span></span>
    <span data-ttu-id="dc775-278">Typ 'q()' tooquit R.</span><span class="sxs-lookup"><span data-stu-id="dc775-278">Type 'q()' tooquit R.</span></span>

    <span data-ttu-id="dc775-279">Microsoft R Server verze 8.0: vylepšená distribuce balíčků R Microsoftu – Copyright (C) 2016 Microsoft Corporation</span><span class="sxs-lookup"><span data-stu-id="dc775-279">Microsoft R Server version 8.0: an enhanced distribution of R  Microsoft packages Copyright (C) 2016 Microsoft Corporation</span></span>

    <span data-ttu-id="dc775-280">Pokud chcete zobrazit poznámky k verzi, zadejte readme().</span><span class="sxs-lookup"><span data-stu-id="dc775-280">Type 'readme()' for release notes.</span></span>
    >

3. <span data-ttu-id="dc775-281">Z hello `>` řádek, můžete zadat kód R.</span><span class="sxs-lookup"><span data-stu-id="dc775-281">From hello `>` prompt, you can enter R code.</span></span> <span data-ttu-id="dc775-282">R server obsahuje balíčky, které vám umožňují tooeasily interakci s Hadoop a spusťte distribuované výpočty.</span><span class="sxs-lookup"><span data-stu-id="dc775-282">R server includes packages that allow you tooeasily interact with Hadoop and run distributed computations.</span></span> <span data-ttu-id="dc775-283">Například použijte následující příkaz tooview hello kořenovém hello výchozí systém souborů pro hello HDInsight cluster hello:</span><span class="sxs-lookup"><span data-stu-id="dc775-283">For example, use hello following command tooview hello root of hello default file system for hello HDInsight cluster:</span></span>

    <span data-ttu-id="dc775-284">rxHadoopListFiles("/")</span><span class="sxs-lookup"><span data-stu-id="dc775-284">rxHadoopListFiles("/")</span></span>

4. <span data-ttu-id="dc775-285">Můžete také použít hello WASB styl adresování.</span><span class="sxs-lookup"><span data-stu-id="dc775-285">You can also use hello WASB style addressing.</span></span>

    <span data-ttu-id="dc775-286">rxHadoopListFiles("wasb:///")</span><span class="sxs-lookup"><span data-stu-id="dc775-286">rxHadoopListFiles("wasb:///")</span></span>


## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a><span data-ttu-id="dc775-287">Použití R Serveru ve službě HDInsight ze vzdálené instance Microsoft R Serveru nebo klienta Microsoft R Client</span><span class="sxs-lookup"><span data-stu-id="dc775-287">Using R Server on HDI from a remote instance of Microsoft R Server or Microsoft R Client</span></span>

<span data-ttu-id="dc775-288">Je možné tooset až přístup toohello HDI Hadoop Spark výpočetní kontextu ze vzdálené instance systému Microsoft R Server nebo klienta Microsoft R systémem stolní nebo přenosný.</span><span class="sxs-lookup"><span data-stu-id="dc775-288">It is possible tooset up access toohello HDI Hadoop Spark compute context from a remote instance of Microsoft R Server or Microsoft R Client running on a desktop or laptop.</span></span> <span data-ttu-id="dc775-289">V tématu **pomocí Microsoft R Server jako klienta Hadoop** část v hello [vytváření kontextu výpočetní pro Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="dc775-289">See **Using Microsoft R Server as a Hadoop Client** subsection in hello [Creating a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md).</span></span> <span data-ttu-id="dc775-290">toodo tedy potřebujete toospecify hello při definování hello RxSpark výpočetní kontext na svém přenosném počítači následující možnosti: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches a sshProfileScript.</span><span class="sxs-lookup"><span data-stu-id="dc775-290">toodo so, you need toospecify hello following options when defining hello RxSpark compute context on your laptop: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches, and sshProfileScript.</span></span> <span data-ttu-id="dc775-291">Například:</span><span class="sxs-lookup"><span data-stu-id="dc775-291">For example:</span></span>


    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )


## <a name="use-a-compute-context"></a><span data-ttu-id="dc775-292">Použití výpočetního kontextu</span><span class="sxs-lookup"><span data-stu-id="dc775-292">Use a compute context</span></span>

<span data-ttu-id="dc775-293">Výpočetní kontext umožňuje toocontrol zda výpočtu je provést lokálně na uzlu edge hello nebo rozdělené mezi hello uzly v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-293">A compute context allows you toocontrol whether computation is performed locally on hello edge node or distributed across hello nodes in hello HDInsight cluster.</span></span>

1. <span data-ttu-id="dc775-294">Z Rstudia serveru nebo konzoly hello R (v relaci SSH) použijte následující kód tooload příklad dat do hello výchozí úložiště pro HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="dc775-294">From RStudio Server or hello R console (in an SSH session), use hello following code tooload example data into hello default storage for HDInsight:</span></span>

        # Set hello HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data toohello tmp folder
        remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
        download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
        download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
        download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
        download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
        download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
        download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
        download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
        download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
        download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
        download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
        download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
        download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

        # Set directory in bigDataDirRoot tooload hello data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make hello directory
        rxHadoopMakeDir(inputDir)

        # Copy hello data from source tooinput
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. <span data-ttu-id="dc775-295">Dále umožňuje vytvořit některé informace data a definovat dvou zdrojů dat, aby jsme můžete pracovat s daty hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-295">Next, let's create some data info and define two data sources so that we can work with hello data.</span></span>

        # Define hello HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create info list for hello airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # get all hello column names
        varNames <- names(airlineColInfo)

        # Define hello text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define hello text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula toouse
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. <span data-ttu-id="dc775-296">Umožňuje spustit logistic regression nad hello dat pomocí kontextu místní výpočetní hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-296">Let's run a logistic regression over hello data using hello local compute context.</span></span>

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    <span data-ttu-id="dc775-297">Měli byste vidět výstup, který končí toohello podobné řádky následující:</span><span class="sxs-lookup"><span data-stu-id="dc775-297">You should see output that ends with lines similar toohello following:</span></span>

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                         Estimate Std. Error z value Pr(>|z|)
         (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
         ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
         ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
         ......
         DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
         DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
         DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
         DEST=BPT         Dropped    Dropped Dropped  Dropped

         ---

         Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

         Condition number of final variance-covariance matrix: 11904202
         Number of iterations: 7

4. <span data-ttu-id="dc775-298">Dále umožňuje spustit hello stejné logistic regression pomocí kontextu Spark hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-298">Next, let's run hello same logistic regression using hello Spark context.</span></span> <span data-ttu-id="dc775-299">kontext Spark Hello distribuuje hello zpracování přes všechny uzly pracovního procesu hello v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-299">hello Spark context distributes hello processing over all hello worker nodes in hello HDInsight cluster.</span></span>

        # Define hello Spark compute context
        mySparkCluster <- RxSpark()

        # Set hello compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        
        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > <span data-ttu-id="dc775-300">Můžete taky MapReduce toodistribute výpočetní mezi uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="dc775-300">You can also use MapReduce toodistribute computation across cluster nodes.</span></span> <span data-ttu-id="dc775-301">Další informace o výpočetním kontextu najdete v tématu [Možnosti výpočetního kontextu pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).</span><span class="sxs-lookup"><span data-stu-id="dc775-301">For more information on compute context, see [Compute context options for R Server on HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).</span></span>


## <a name="distribute-r-code-toomultiple-nodes"></a><span data-ttu-id="dc775-302">Distribuovat uzly toomultiple kód R</span><span class="sxs-lookup"><span data-stu-id="dc775-302">Distribute R code toomultiple nodes</span></span>

<span data-ttu-id="dc775-303">S R Server můžete snadno využít existující kód R a spusťte napříč několika uzly v clusteru hello pomocí `rxExec`.</span><span class="sxs-lookup"><span data-stu-id="dc775-303">With R Server, you can easily take existing R code and run it across multiple nodes in hello cluster by using `rxExec`.</span></span> <span data-ttu-id="dc775-304">Tato funkce je užitečná při uklízení parametrů nebo provádění simulací.</span><span class="sxs-lookup"><span data-stu-id="dc775-304">This function is useful when doing a parameter sweep or simulations.</span></span> <span data-ttu-id="dc775-305">Hello následující kód představuje příklad toouse `rxExec`:</span><span class="sxs-lookup"><span data-stu-id="dc775-305">hello following code is an example of how toouse `rxExec`:</span></span>

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

<span data-ttu-id="dc775-306">Pokud stále používáte hello Spark nebo kontextu MapReduce, tento příkaz vrátí hodnotu hello nodename pro uzly pracovního procesu hello tento kód hello `(Sys.info()["nodename"])` běží na.</span><span class="sxs-lookup"><span data-stu-id="dc775-306">If you are still using hello Spark or MapReduce context, this  command returns hello nodename value for hello worker nodes that hello code `(Sys.info()["nodename"])` is run on.</span></span> <span data-ttu-id="dc775-307">Například na cluster se čtyřmi uzly očekáváte, že tooreceive výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="dc775-307">For example, on a four node cluster, you expect tooreceive output similar toohello following:</span></span>

    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"


## <a name="accessing-data-in-hive-and-parquet"></a><span data-ttu-id="dc775-308">Přístup k datům v Hive a Parquet</span><span class="sxs-lookup"><span data-stu-id="dc775-308">Accessing Data in Hive and Parquet</span></span>

<span data-ttu-id="dc775-309">K dispozici v R Server 9.1 funkce umožňuje přímý přístup toodata v Hive a Parquet pro použití funkce ScaleR v kontextu výpočetní hello Spark.</span><span class="sxs-lookup"><span data-stu-id="dc775-309">A feature available in R Server 9.1 allows direct access toodata in Hive and Parquet for use by ScaleR functions in hello Spark compute context.</span></span> <span data-ttu-id="dc775-310">Tyto možnosti jsou k dispozici prostřednictvím nové ScaleR datového zdroje funkce volané RxHiveData a RxParquetData které fungovat prostřednictvím použití Spark SQL tooload dat přímo do DataFrame Spark pro analýzu ScaleR.</span><span class="sxs-lookup"><span data-stu-id="dc775-310">These capabilities are available through new ScaleR data source functions called RxHiveData and RxParquetData that work through use of Spark SQL tooload data directly into a Spark DataFrame for analysis by ScaleR.</span></span>  

<span data-ttu-id="dc775-311">Následující kód Hello poskytuje ukázkový kód při použití nových funkcí hello:</span><span class="sxs-lookup"><span data-stu-id="dc775-311">hello following code provides some sample code on use of hello new functions:</span></span>

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, cleanup, and close hello Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


<span data-ttu-id="dc775-312">Další informace o těchto nových funkcí naleznete v online nápovědě hello v R Server prostřednictvím použití hello `?RxHivedata` a `?RxParquetData` příkazy.</span><span class="sxs-lookup"><span data-stu-id="dc775-312">For additional info on use of these new functions see hello online help in R Server through use of hello `?RxHivedata` and `?RxParquetData` commands.</span></span>  


## <a name="install-additional-r-packages-on-hello-edge-node"></a><span data-ttu-id="dc775-313">Nainstalujte další balíčky R na uzlu edge hello</span><span class="sxs-lookup"><span data-stu-id="dc775-313">Install additional R packages on hello edge node</span></span>

<span data-ttu-id="dc775-314">Pokud chcete tooinstall další balíčky R na hello hraniční uzel, můžete použít `install.packages()` přímo z uvnitř hello konzoly R při připojené toohello okraj uzlu prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="dc775-314">If you would like tooinstall additional R packages on hello edge node, you can use `install.packages()` directly from within hello R console when connected toohello edge node through SSH.</span></span> <span data-ttu-id="dc775-315">Pokud potřebujete balíčky R tooinstall na uzly pracovního procesu hello hello clusteru, ale musí používat akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="dc775-315">However, if you need tooinstall R packages on hello worker nodes of hello cluster, you must use a Script Action.</span></span>

<span data-ttu-id="dc775-316">Akce skriptů jsou skripty Bash, které jsou používané toomake konfigurace změny toohello HDInsight clusteru nebo tooinstall další software, například další balíčky R.</span><span class="sxs-lookup"><span data-stu-id="dc775-316">Script Actions are Bash scripts that are used toomake configuration changes toohello HDInsight cluster or tooinstall additional software, such as additional R packages.</span></span> <span data-ttu-id="dc775-317">Další balíčky tooinstall pomocí akce skriptu, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dc775-317">tooinstall additional packages using a Script Action, use hello following steps:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc775-318">Pomocí skriptových akcí tooinstall další balíčky R dá použít jenom po vytvoření clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-318">Using Script Actions tooinstall additional R packages can only be used after hello cluster has been created.</span></span> <span data-ttu-id="dc775-319">Tento postup nepoužívejte při vytváření clusteru, protože skript hello používá tento údaj R Server úplně nainstalována a nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="dc775-319">Do not use this procedure during cluster creation, as hello script relies on R Server being completely installed and configured.</span></span>
>
>

1. <span data-ttu-id="dc775-320">Z hello [portál Azure](https://portal.azure.com), vyberte svůj Server R na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dc775-320">From hello [Azure portal](https://portal.azure.com), select your R Server on HDInsight cluster.</span></span>

2. <span data-ttu-id="dc775-321">Z hello **nastavení** vyberte **akcí skriptů** a potom **odeslání nové** toosubmit nové akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="dc775-321">From hello **Settings** blade, select **Script Actions** and then **Submit New** toosubmit a new Script Action.</span></span>

   ![Obrázek okna Akce skriptů](./media/hdinsight-hadoop-r-server-get-started/scriptaction.png)

3. <span data-ttu-id="dc775-323">Z hello **odeslat akci se skripty** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="dc775-323">From hello **Submit script action** blade, provide hello following information:</span></span>

   * <span data-ttu-id="dc775-324">**Název**: popisný název tooidentify tento skript</span><span class="sxs-lookup"><span data-stu-id="dc775-324">**Name**: A friendly name tooidentify this script</span></span>

   * <span data-ttu-id="dc775-325">**Identifikátor URI skriptu Bash:**`http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`</span><span class="sxs-lookup"><span data-stu-id="dc775-325">**Bash script URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`</span></span>

   * <span data-ttu-id="dc775-326">**Hlavní:** Tato položka by měla být **nezaškrtnutá**.</span><span class="sxs-lookup"><span data-stu-id="dc775-326">**Head**: This item should be **unchecked**</span></span>

   * <span data-ttu-id="dc775-327">**Pracovní:** Tato položka by měla být **zaškrtnutá**.</span><span class="sxs-lookup"><span data-stu-id="dc775-327">**Worker**: This item should be **checked**</span></span>

   * <span data-ttu-id="dc775-328">**Hraniční uzly:** Tato položka by měla být **nezaškrtnutá**.</span><span class="sxs-lookup"><span data-stu-id="dc775-328">**Edge nodes**: This item should be **unchecked**.</span></span>

   * <span data-ttu-id="dc775-329">**Zookeeper:** Tato položka by měla být **nezaškrtnutá**.</span><span class="sxs-lookup"><span data-stu-id="dc775-329">**Zookeeper**: This item should be **unchecked**</span></span>

   * <span data-ttu-id="dc775-330">**Parametry**: hello R balíčky toobe nainstalována.</span><span class="sxs-lookup"><span data-stu-id="dc775-330">**Parameters**: hello R packages toobe installed.</span></span> <span data-ttu-id="dc775-331">Například `bitops stringr arules`.</span><span class="sxs-lookup"><span data-stu-id="dc775-331">For example, `bitops stringr arules`</span></span>

   * <span data-ttu-id="dc775-332">**Zachovat tento skript...:** Tato položka by měla být **zaškrtnutá**.</span><span class="sxs-lookup"><span data-stu-id="dc775-332">**Persist this script...**: This item should be **Checked**</span></span>  

   > [!NOTE]
   > 1. <span data-ttu-id="dc775-333">Ve výchozím nastavení nejsou nainstalovány všechny balíčky R ze snímku úložiště Microsoft MRAN hello konzistentní s verzí hello R Server, který byl nainstalován.</span><span class="sxs-lookup"><span data-stu-id="dc775-333">By default, all R packages are installed from a snapshot of hello Microsoft MRAN repository consistent with hello version of R Server that has been installed.</span></span> <span data-ttu-id="dc775-334">Pokud chcete tooinstall novější verze balíčků, je některé riziko nekompatibilita.</span><span class="sxs-lookup"><span data-stu-id="dc775-334">If you want tooinstall newer versions of packages, then there is some risk of incompatibility.</span></span> <span data-ttu-id="dc775-335">Tento typ instalace je ale možné zadáním `useCRAN` jako hello hello balíčku první prvek seznamu, například `useCRAN bitops, stringr, arules`.</span><span class="sxs-lookup"><span data-stu-id="dc775-335">However this kind of install is possible by specifying `useCRAN` as hello first element of hello package list, for example `useCRAN bitops, stringr, arules`.</span></span>  
   > 2. <span data-ttu-id="dc775-336">Některé balíčky R vyžadují další linuxové systémové knihovny.</span><span class="sxs-lookup"><span data-stu-id="dc775-336">Some R packages require additional Linux system libraries.</span></span> <span data-ttu-id="dc775-337">Pro usnadnění práce jsme nainstalovali předem hello závislosti nutné hello nejvyšší 100 nejoblíbenější R balíčky.</span><span class="sxs-lookup"><span data-stu-id="dc775-337">For convenience, we have pre-installed hello dependencies needed by hello top 100 most popular R packages.</span></span> <span data-ttu-id="dc775-338">Ale pokud balíčky R hello, který nainstalujete vyžadují knihovny nad rámec těchto pak musíte stáhnout hello základní skriptu použít se zde a přidat kroky tooinstall hello systému knihovny.</span><span class="sxs-lookup"><span data-stu-id="dc775-338">However, if hello R package(s) you install require libraries beyond these then you must download hello base script used here and add steps tooinstall hello system libraries.</span></span> <span data-ttu-id="dc775-339">Je nutné nahrávání hello upravit skript tooa veřejné kontejner v úložišti Azure objektů blob a používat hello upravit skript tooinstall hello balíčky.</span><span class="sxs-lookup"><span data-stu-id="dc775-339">You must then upload hello modified script tooa public blob container in Azure storage and use hello modified script tooinstall hello packages.</span></span>
   >    <span data-ttu-id="dc775-340">Další informace o vývoji akcí skriptů najdete v tématu [Vývoj akcí skriptů](hdinsight-hadoop-script-actions-linux.md).</span><span class="sxs-lookup"><span data-stu-id="dc775-340">For more information on developing Script Actions, see [Script Action development](hdinsight-hadoop-script-actions-linux.md).</span></span>  
   >
   >

   ![Přidání akce skriptu](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. <span data-ttu-id="dc775-342">Vyberte **vytvořit** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="dc775-342">Select **Create** toorun hello script.</span></span> <span data-ttu-id="dc775-343">Po dokončení skriptu hello hello R balíčky jsou k dispozici na všechny uzly pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="dc775-343">Once hello script completes, hello R packages are available on all worker nodes.</span></span>


## <a name="using-microsoft-r-server-operationalization"></a><span data-ttu-id="dc775-344">Použití operacionalizace Microsoft R Serveru</span><span class="sxs-lookup"><span data-stu-id="dc775-344">Using Microsoft R Server Operationalization</span></span>

<span data-ttu-id="dc775-345">Po dokončení modelování vaše data můžete zprovoznit hello modelu toomake předpovědi.</span><span class="sxs-lookup"><span data-stu-id="dc775-345">When your data modeling is complete, you can operationalize hello model toomake predictions.</span></span> <span data-ttu-id="dc775-346">tooconfigure pro operationalization Microsoft R Server, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="dc775-346">tooconfigure for Microsoft R Server operationalization, perform hello following steps:</span></span>

<span data-ttu-id="dc775-347">První, ssh do uzlu Edge hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-347">First, ssh into hello Edge node.</span></span> <span data-ttu-id="dc775-348">Například:</span><span class="sxs-lookup"><span data-stu-id="dc775-348">For example,</span></span> 

    ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="dc775-349">Po použití ssh, změňte adresář pro příslušné verze hello a soubor dll dotnet sudo hello:</span><span class="sxs-lookup"><span data-stu-id="dc775-349">After using ssh, change directory for hello relevant version and sudo hello dotnet dll:</span></span> 

- <span data-ttu-id="dc775-350">Pro Microsoft R Server 9.1:</span><span class="sxs-lookup"><span data-stu-id="dc775-350">For Microsoft R Server 9.1:</span></span>

    <span data-ttu-id="dc775-351">cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0 sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll</span><span class="sxs-lookup"><span data-stu-id="dc775-351">cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll</span></span>

- <span data-ttu-id="dc775-352">Pro Microsoft R Server 9.0:</span><span class="sxs-lookup"><span data-stu-id="dc775-352">For Microsoft R Server 9.0:</span></span>

    <span data-ttu-id="dc775-353">cd /usr/lib64/microsoft-deployr/9.0.1 sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll</span><span class="sxs-lookup"><span data-stu-id="dc775-353">cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll</span></span>

<span data-ttu-id="dc775-354">Microsoft R Server operationalization tooconfigure s konfigurací jedné pole hello následující:</span><span class="sxs-lookup"><span data-stu-id="dc775-354">tooconfigure Microsoft R Server operationalization with a One-box configuration do hello following:</span></span>

1. <span data-ttu-id="dc775-355">Vyberte „Configure R Server for Operationalization“ (Konfigurovat R Server pro operacionalizaci).</span><span class="sxs-lookup"><span data-stu-id="dc775-355">Select “Configure R Server for Operationalization”</span></span>
2. <span data-ttu-id="dc775-356">Vyberte „A.</span><span class="sxs-lookup"><span data-stu-id="dc775-356">Select “A.</span></span> <span data-ttu-id="dc775-357">One-box (web + compute nodes)“ (Jednotná konfigurace webu a výpočetních uzlů).</span><span class="sxs-lookup"><span data-stu-id="dc775-357">One-box (web + compute nodes)”</span></span>
3. <span data-ttu-id="dc775-358">Zadejte heslo pro hello **správce** uživatele</span><span class="sxs-lookup"><span data-stu-id="dc775-358">Enter a password for hello **admin** user</span></span>

![jednotná konfigurace operacionalizace](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

<span data-ttu-id="dc775-360">Volitelně můžete provést diagnostické kontroly spuštěním diagnostického testu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dc775-360">As an optional step you can perform Diagnostic checks by running a diagnostic test as follows:</span></span>

1. <span data-ttu-id="dc775-361">Vyberte „6.</span><span class="sxs-lookup"><span data-stu-id="dc775-361">Select “6.</span></span> <span data-ttu-id="dc775-362">Run diagnostic tests“ (Spustit diagnostické testy).</span><span class="sxs-lookup"><span data-stu-id="dc775-362">Run diagnostic tests”</span></span>
2. <span data-ttu-id="dc775-363">Vyberte „A.</span><span class="sxs-lookup"><span data-stu-id="dc775-363">Select “A.</span></span> <span data-ttu-id="dc775-364">Test configuration“ (Testovat konfiguraci).</span><span class="sxs-lookup"><span data-stu-id="dc775-364">Test configuration”</span></span>
3. <span data-ttu-id="dc775-365">Zadejte uživatelské jméno admin a heslo, které jste zadali v předchozím kroku konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dc775-365">Enter Username = “admin” and password from previous configuration step</span></span>
4. <span data-ttu-id="dc775-366">Ověřte, že se zobrazí „Overall Health: pass“ (Celkový stav: v pořádku)</span><span class="sxs-lookup"><span data-stu-id="dc775-366">Confirm Overall Health = pass</span></span>
5. <span data-ttu-id="dc775-367">Ukončení hello nástroj pro správu</span><span class="sxs-lookup"><span data-stu-id="dc775-367">Exit hello Admin Utility</span></span>
6. <span data-ttu-id="dc775-368">Ukončete připojení SSH</span><span class="sxs-lookup"><span data-stu-id="dc775-368">Exit SSH</span></span>

![Diagnostika operacionalizace](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
><span data-ttu-id="dc775-370">**Dlouhá zpoždění při využívání webové služby ve Sparku**</span><span class="sxs-lookup"><span data-stu-id="dc775-370">**Long delays when consuming web service on Spark**</span></span>
>
><span data-ttu-id="dc775-371">Pokud dojde k prodlevám při pokusu o tooconsume webové služby vytvořené pomocí funkce mrsdeploy v kontextu výpočtů Spark, musíte tooadd některé chybějící složky.</span><span class="sxs-lookup"><span data-stu-id="dc775-371">If you encounter long delays when trying tooconsume a web service created with mrsdeploy functions in a Spark compute context, you may need tooadd some missing folders.</span></span> <span data-ttu-id="dc775-372">Hello aplikací Spark patří tooa uživatele volat '*rserve2*se vždy, když je volána z webové služby pomocí mrsdeploy funkcí.</span><span class="sxs-lookup"><span data-stu-id="dc775-372">hello Spark application belongs tooa user called '*rserve2*' whenever it is invoked from a web service using mrsdeploy functions.</span></span> <span data-ttu-id="dc775-373">toowork chcete tento problém vyřešit:</span><span class="sxs-lookup"><span data-stu-id="dc775-373">toowork around this issue:</span></span>

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


<span data-ttu-id="dc775-374">V této fázi hello konfigurace pro Operationalization byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="dc775-374">At this stage, hello configuration for Operationalization is complete.</span></span> <span data-ttu-id="dc775-375">Nyní můžete pomocí hello mrsdeploy balíček na vaše toohello tooconnect RClient Operationalization na hraniční uzel a začít používat jeho funkce, jako jsou [vzdálené spuštění](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) a [webové služby](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette).</span><span class="sxs-lookup"><span data-stu-id="dc775-375">Now you can use hello ‘mrsdeploy’ package on your RClient tooconnect toohello Operationalization on edge node and start using its features like [remote execution](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) and [web-services](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette).</span></span> <span data-ttu-id="dc775-376">V závislosti na tom, jestli váš cluster je nastavený na virtuální síť nebo ne může být nutné tooset až port dopředného tunelové propojení prostřednictvím přihlašování přes SSH.</span><span class="sxs-lookup"><span data-stu-id="dc775-376">Depending on whether your cluster is set up on a virtual network or not, you may need tooset up port forward tunneling through SSH login.</span></span> <span data-ttu-id="dc775-377">Hello následující oddíly popisují, jak tooset až toto tunelové propojení.</span><span class="sxs-lookup"><span data-stu-id="dc775-377">hello following sections explain how tooset up this tunnel.</span></span>

### <a name="rserver-cluster-on-virtual-network"></a><span data-ttu-id="dc775-378">Cluster R Serveru ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="dc775-378">RServer Cluster on virtual network</span></span>

<span data-ttu-id="dc775-379">Ujistěte se, že povolíte provoz přes port 12800 toohello hraniční uzel.</span><span class="sxs-lookup"><span data-stu-id="dc775-379">Make sure you allow traffic through port 12800 toohello edge node.</span></span> <span data-ttu-id="dc775-380">Tímto způsobem můžete hello hraniční uzel tooconnect toohello Operationalization funkce.</span><span class="sxs-lookup"><span data-stu-id="dc775-380">That way, you can use hello edge node tooconnect toohello Operationalization feature.</span></span>


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


<span data-ttu-id="dc775-381">Pokud hello `remoteLogin()` možné připojit toohello hraniční uzel, ale můžete SSH toohello hraniční uzel, a potom je nutné tooverify, zda text hello pravidlo tooallow přenosy na portu 12800 byla nastavena správně nebo není.</span><span class="sxs-lookup"><span data-stu-id="dc775-381">If hello `remoteLogin()` cannot connect toohello edge node, but you can SSH toohello edge node, then you need tooverify whether hello rule tooallow traffic on port 12800 has been set properly or not.</span></span> <span data-ttu-id="dc775-382">Pokud budete pokračovat tooface hello problém, můžete je vyřešit nastavením portu dopředného tunelové propojení prostřednictvím SSH.</span><span class="sxs-lookup"><span data-stu-id="dc775-382">If you continue tooface hello issue, you can work around it by setting up port forward tunneling through SSH.</span></span> <span data-ttu-id="dc775-383">Pokyny najdete v tématu hello následující části.</span><span class="sxs-lookup"><span data-stu-id="dc775-383">For instructions, see hello following section.</span></span>

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a><span data-ttu-id="dc775-384">Cluster R Serveru nastavený mimo virtuální síť</span><span class="sxs-lookup"><span data-stu-id="dc775-384">RServer Cluster not set up on virtual network</span></span>

<span data-ttu-id="dc775-385">Pokud váš cluster není nastavený ve virtuální síti nebo máte potíže s připojením přes virtuální síť, můžete použít přesměrování portu tunelovým propojením přes SSH:</span><span class="sxs-lookup"><span data-stu-id="dc775-385">If your cluster is not set up on vnet or if you are having troubles with connectivity through vnet, you can use SSH port forward tunneling:</span></span>

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

<span data-ttu-id="dc775-386">V PuTTY to také můžete nastavit.</span><span class="sxs-lookup"><span data-stu-id="dc775-386">On Putty, you can set it up as well.</span></span>

![připojení ssh v putty](./media/hdinsight-hadoop-r-server-get-started/putty.png)

<span data-ttu-id="dc775-388">Po relace SSH je aktivní, se předají hello provoz z vašeho počítače port 12800 toohello hraniční uzel port 12800 prostřednictvím relace SSH.</span><span class="sxs-lookup"><span data-stu-id="dc775-388">Once your SSH session is active, hello traffic from your machine’s port 12800 is forwarded toohello edge node’s port 12800 through SSH session.</span></span> <span data-ttu-id="dc775-389">Nezapomeňte v metodě `remoteLogin()` použít adresu `127.0.0.1:12800`.</span><span class="sxs-lookup"><span data-stu-id="dc775-389">Make sure you use `127.0.0.1:12800` in your `remoteLogin()` method.</span></span> <span data-ttu-id="dc775-390">Přihlásí toohello hraniční uzel operationalization prostřednictvím portu předávání.</span><span class="sxs-lookup"><span data-stu-id="dc775-390">This logs in toohello edge node’s operationalization through port forwarding.</span></span>


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="how-tooscale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a><span data-ttu-id="dc775-391">Jak tooscale Microsoft R Server Operationalization výpočetní uzly na HDInsight uzlů pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="dc775-391">How tooscale Microsoft R Server Operationalization compute nodes on HDInsight worker nodes</span></span>

### <a name="decommission-hello-worker-nodes"></a><span data-ttu-id="dc775-392">Vyřadit z provozu hello pracovní uzly</span><span class="sxs-lookup"><span data-stu-id="dc775-392">Decommission hello worker node(s)</span></span>

<span data-ttu-id="dc775-393">Microsoft R Server v současné době není spravován přes YARN.</span><span class="sxs-lookup"><span data-stu-id="dc775-393">Microsoft R Server is currently not managed through Yarn.</span></span> <span data-ttu-id="dc775-394">Pokud hello pracovní uzly nejsou vyřazení, hello Yarn Resource Manager nebude fungovat podle očekávání, protože nebude vědět hello prostředků se zabírá serverem hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-394">If hello worker nodes are not decommissioned, hello Yarn Resource Manager will not work as expected because it will not be aware of hello resources being taken up by hello server.</span></span> <span data-ttu-id="dc775-395">V pořadí tooavoid této situaci, doporučujeme před škálovat výpočetní uzly hello vyřazení z provozu hello uzlů pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="dc775-395">In order tooavoid this situation, we recommend decommissioning hello worker nodes before you scale out hello compute nodes.</span></span>

<span data-ttu-id="dc775-396">Kroky toodecommissioning pracovní uzly:</span><span class="sxs-lookup"><span data-stu-id="dc775-396">Steps toodecommissioning worker nodes:</span></span>

* <span data-ttu-id="dc775-397">Přihlaste se v konzole Ambari tooHDI clusteru a klikněte na kartě "hostitele"</span><span class="sxs-lookup"><span data-stu-id="dc775-397">Log in tooHDI cluster's Ambari console and click on "hosts" tab</span></span>
* <span data-ttu-id="dc775-398">Vyberte pracovní uzly (toobe vyřazena z provozu), klikněte na "Akce" > "Vybrané hostitele" > "Hostitele" > klikněte na "Zapnout ON režimu údržby".</span><span class="sxs-lookup"><span data-stu-id="dc775-398">Select worker nodes (toobe decommissioned), Click on "Actions" > "Selected Hosts" > "Hosts" > click on "Turn ON Maintenance Mode".</span></span> <span data-ttu-id="dc775-399">Například v hello následující obrázek jsme vybrali wn3 a wn4 toodecommission.</span><span class="sxs-lookup"><span data-stu-id="dc775-399">For example, in hello following image we have selected wn3 and wn4 toodecommission.</span></span>  

   ![vyřazení pracovních uzlů z provozu](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* <span data-ttu-id="dc775-401">Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **DataNodes** (Datové uzly) a klikněte na **Decommission** (Vyřadit z provozu).</span><span class="sxs-lookup"><span data-stu-id="dc775-401">Select **Actions** > **Selected Hosts** > **DataNodes** > click **Decommission**</span></span>
* <span data-ttu-id="dc775-402">Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **NodeManagers** (Správci uzlů) a klikněte na **Decommission** (Vyřadit z provozu).</span><span class="sxs-lookup"><span data-stu-id="dc775-402">Select **Actions** > **Selected Hosts** > **NodeManagers** > click **Decommission**</span></span>
* <span data-ttu-id="dc775-403">Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **DataNodes** (Datové uzly) a klikněte na **Stop** (Zastavit).</span><span class="sxs-lookup"><span data-stu-id="dc775-403">Select **Actions** > **Selected Hosts** > **DataNodes** > click **Stop**</span></span>
* <span data-ttu-id="dc775-404">Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **NodeManagers** (Správci uzlů) a klikněte na **Stop** (Zastavit).</span><span class="sxs-lookup"><span data-stu-id="dc775-404">Select **Actions** > **Selected Hosts** > **NodeManagers** > click on **Stop**</span></span>
* <span data-ttu-id="dc775-405">Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **Hosts** (Hostitelé) a klikněte na **Stop All Components** (Zastavit všechny komponenty).</span><span class="sxs-lookup"><span data-stu-id="dc775-405">Select **Actions** > **Selected Hosts** > **Hosts** > click **Stop All Components**</span></span>
* <span data-ttu-id="dc775-406">Zrušte výběr uzlů pracovního procesu hello a vyberte hlavních uzlech hello</span><span class="sxs-lookup"><span data-stu-id="dc775-406">Unselect hello worker nodes and select hello head nodes</span></span>
* <span data-ttu-id="dc775-407">Vyberte **Actions** (Akce) > **Selected Hosts** (Vybraní hostitelé) > **Hosts** (Hostitelé) > **Restart All Components** (Restartovat všechny komponenty).</span><span class="sxs-lookup"><span data-stu-id="dc775-407">Select **Actions** > **Selected Hosts** > "**Hosts** > **Restart All Components**</span></span>

### <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a><span data-ttu-id="dc775-408">Konfigurace výpočetních uzlů na všech vyřazených pracovních uzlech</span><span class="sxs-lookup"><span data-stu-id="dc775-408">Configure Compute nodes on each decommissioned worker node(s)</span></span>

1. <span data-ttu-id="dc775-409">Přihlaste se přes SSH do každého vyřazeného pracovního uzlu.</span><span class="sxs-lookup"><span data-stu-id="dc775-409">SSH into each decommissioned worker node.</span></span>
2. <span data-ttu-id="dc775-410">Spusťte nástroj pro správu pomocí příkazu `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.</span><span class="sxs-lookup"><span data-stu-id="dc775-410">Run admin utility using `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.</span></span>
3. <span data-ttu-id="dc775-411">Zadejte "1" tooselect možnost "Konfigurace R Server pro Operationalization".</span><span class="sxs-lookup"><span data-stu-id="dc775-411">Enter "1" tooselect option "Configure R Server for Operationalization".</span></span>
4. <span data-ttu-id="dc775-412">Zadejte možnost "c" tooselect "C.</span><span class="sxs-lookup"><span data-stu-id="dc775-412">Enter "c" tooselect option "C.</span></span> <span data-ttu-id="dc775-413">Compute node“ (Výpočetní uzel).</span><span class="sxs-lookup"><span data-stu-id="dc775-413">Compute node".</span></span> <span data-ttu-id="dc775-414">Tím se nakonfiguruje hello výpočetním uzlu na hello pracovního uzlu.</span><span class="sxs-lookup"><span data-stu-id="dc775-414">This configures hello compute node on hello worker node.</span></span>
5. <span data-ttu-id="dc775-415">Ukončení hello nástroj pro správu.</span><span class="sxs-lookup"><span data-stu-id="dc775-415">Exit hello Admin Utility.</span></span>

### <a name="add-compute-nodes-details-on-web-node"></a><span data-ttu-id="dc775-416">Přidání podrobností o výpočetních uzlech do webového uzlu</span><span class="sxs-lookup"><span data-stu-id="dc775-416">Add compute nodes details on Web Node</span></span>

<span data-ttu-id="dc775-417">Po byly nakonfigurovány všechny uzly pracovního procesu vyřazení toorun výpočetního uzlu, vraťte se na hello hraniční uzel a přidat vyřazení pracovní uzly IP adresy v konfiguraci hello Microsoft uzlu serveru R web na:</span><span class="sxs-lookup"><span data-stu-id="dc775-417">Once all decommissioned worker nodes have been configured toorun compute node, come back on hello edge node and add decommissioned worker nodes' IP addresses in hello Microsoft R Server web node's configuration:</span></span>

* <span data-ttu-id="dc775-418">SSH do uzlu edge hello.</span><span class="sxs-lookup"><span data-stu-id="dc775-418">SSH into hello edge node.</span></span>
* <span data-ttu-id="dc775-419">Spusťte `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="dc775-419">Run `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.</span></span>
* <span data-ttu-id="dc775-420">Vyhledejte část "Identifikátory URI" hello a přidejte IP pracovního uzlu a podrobnosti o portu.</span><span class="sxs-lookup"><span data-stu-id="dc775-420">Look for hello "URIs" section, and add worker node's IP and port details.</span></span>

    ![příkazový řádek vyřazení pracovních uzlů z provozu](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a><span data-ttu-id="dc775-422">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="dc775-422">Troubleshoot</span></span>

<span data-ttu-id="dc775-423">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="dc775-423">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>


## <a name="next-steps"></a><span data-ttu-id="dc775-424">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dc775-424">Next steps</span></span>

<span data-ttu-id="dc775-425">Nyní byste měli porozumět, jak toocreate nové clusteru HDInsight, která zahrnuje hello R Server a hello základy používání hello R konzoly z relace SSH.</span><span class="sxs-lookup"><span data-stu-id="dc775-425">Now you should understand how toocreate a new HDInsight cluster that includes hello R Server and hello basics of using hello R console from an SSH session.</span></span> <span data-ttu-id="dc775-426">Hello následující témata popisují jiné způsoby správy a práce s R serverem v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="dc775-426">hello following topics explain other ways of managing and working with R Server on HDInsight:</span></span>

* [<span data-ttu-id="dc775-427">Přidat Server Rstudia tooHDInsight (Pokud není nainstalován během vytváření clusteru)</span><span class="sxs-lookup"><span data-stu-id="dc775-427">Add RStudio Server tooHDInsight (if not installed during cluster creation)</span></span>](hdinsight-hadoop-r-server-install-r-studio.md)
* [<span data-ttu-id="dc775-428">Možnosti výpočetního kontextu pro R Server ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="dc775-428">Compute context options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)
* [<span data-ttu-id="dc775-429">Možnosti služby Azure Storage pro R Server ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="dc775-429">Azure Storage options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-storage.md)
