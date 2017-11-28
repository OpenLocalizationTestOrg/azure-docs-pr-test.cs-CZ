---
title: "aaaCustomize Hadoop clusterů pro hello proces vědecké účely dat Team | Microsoft Docs"
description: "K dispozici ve vlastní clusterů systému Azure HDInsight Hadoop oblíbených modulů Python."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0c115dca-2565-4e7a-9536-6002af5c786a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: e192542dd39f71bccbb5163382b4050d0f12ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-hello-team-data-science-process"></a><span data-ttu-id="09d96-103">Přizpůsobení clusterů systému Azure HDInsight Hadoop pro hello procesu Team dat vědecké účely</span><span class="sxs-lookup"><span data-stu-id="09d96-103">Customize Azure HDInsight Hadoop clusters for hello Team Data Science Process</span></span>
<span data-ttu-id="09d96-104">Tento článek popisuje jak toocustomize HDInsight Hadoop clusteru nainstalováním Anaconda 64-bit (Python 2.7) na každém uzlu při zřízení clusteru hello jako služba HDInsight.</span><span class="sxs-lookup"><span data-stu-id="09d96-104">This article describes how toocustomize an HDInsight Hadoop cluster by installing 64-bit Anaconda (Python 2.7) on each node when hello cluster is provisioned as an HDInsight service.</span></span> <span data-ttu-id="09d96-105">Také ukazuje, jak tooaccess hello headnode toosubmit vlastní úlohy toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="09d96-105">It also shows how tooaccess hello headnode toosubmit custom jobs toohello cluster.</span></span> <span data-ttu-id="09d96-106">Toto vlastní nastavení díky mnoha oblíbených Python moduly, které jsou součástí Anaconda, pohodlně k dispozici pro použití v uživatele definovaných funkcí (UDF), které jsou navržené tooprocess záznamy Hive v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="09d96-106">This customization makes many popular Python modules, that are included in Anaconda, conveniently available for use in user defined functions (UDFs) that are designed tooprocess Hive records in hello cluster.</span></span> <span data-ttu-id="09d96-107">Pokyny o postupech hello používá v tomto scénáři najdete v tématu [jak dotazů Hive toosubmit](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="09d96-107">For instructions on hello procedures used in this scenario, see [How toosubmit Hive queries](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

<span data-ttu-id="09d96-108">Hello následující nabídky odkazy tootopics, které popisují, jak tooset až hello různé vědě prostředí datových používají v hello [tým datové vědy procesu (TDSP)](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="09d96-108">hello following menu links tootopics that describe how tooset up hello various data science environments used by hello [Team Data Science Process (TDSP)](data-science-process-overview.md).</span></span>

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

## <span data-ttu-id="09d96-109"><a name="customize"></a>Přizpůsobení clusteru Azure HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="09d96-109"><a name="customize"></a>Customize Azure HDInsight Hadoop Cluster</span></span>
<span data-ttu-id="09d96-110">toocreate vlastní cluster HDInsight Hadoop, začněte tím, že protokolování příliš[**portál Azure classic**](https://manage.windowsazure.com/), klikněte na tlačítko **nový** v hello zbývajících dolního rohu a pak vyberte datové služby -> HDINSIGHT -> **vytvořit vlastní** toobring až hello **podrobnosti o clusteru** okno.</span><span class="sxs-lookup"><span data-stu-id="09d96-110">toocreate a customized HDInsight Hadoop cluster, start by logging on too[**Azure classic portal**](https://manage.windowsazure.com/), click **New** at hello left bottom corner, and then select DATA SERVICES -> HDINSIGHT -> **CUSTOM CREATE** toobring up hello **Cluster Details** window.</span></span> 

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

<span data-ttu-id="09d96-112">Zadejte název hello toobe hello clusteru vytvořit na stránce konfigurace 1 a přijměte výchozí hodnoty pro hello další pole.</span><span class="sxs-lookup"><span data-stu-id="09d96-112">Input hello name of hello cluster toobe created on configuration page 1, and accept default values for hello other fields.</span></span> <span data-ttu-id="09d96-113">Klikněte na tlačítko hello šipku toogo toohello další konfigurační stránku.</span><span class="sxs-lookup"><span data-stu-id="09d96-113">Click hello arrow toogo toohello next configuration page.</span></span> 

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

<span data-ttu-id="09d96-115">Na stránce konfigurace 2, vstupní hello počet **datové uzly**, vyberte hello **oblasti nebo virtuální síť**a vyberte hello velikosti hello **hlavní uzel** a hello **Datový uzel**.</span><span class="sxs-lookup"><span data-stu-id="09d96-115">On configuration page 2, input hello number of **DATA NODES**, select hello **REGION/VIRTUAL NETWORK**, and select hello sizes of hello **HEAD NODE** and hello **DATA NODE**.</span></span> <span data-ttu-id="09d96-116">Klikněte na tlačítko hello šipku toogo toohello další konfigurační stránku.</span><span class="sxs-lookup"><span data-stu-id="09d96-116">Click hello arrow toogo toohello next configuration page.</span></span>

> [!NOTE]
> <span data-ttu-id="09d96-117">Hello **oblasti nebo virtuální síť** má toobe hello stejné jako hello oblast hello účet úložiště, který se bude používat pro hello clusteru HDInsight Hadoop toobe.</span><span class="sxs-lookup"><span data-stu-id="09d96-117">hello **REGION/VIRTUAL NETWORK** has toobe hello same as hello region of hello storage account that is going toobe used for hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="09d96-118">Hello čtvrté stránce konfigurace, jinak hodnota hello účet úložiště se nezobrazí na rozevírací seznam hello **název účtu**.</span><span class="sxs-lookup"><span data-stu-id="09d96-118">Otherwise, in hello fourth configuration page, hello storage account will not appear on hello dropdown list of **ACCOUNT NAME**.</span></span>
> 
> 

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

<span data-ttu-id="09d96-120">Na stránce konfigurace 3 zadejte uživatelské jméno a heslo pro hello clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="09d96-120">On configuration page 3, provide a user name and password for hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="09d96-121">**Nechcete** vyberte hello *Enter hello Metaúložiště Hive nebo Oozie*.</span><span class="sxs-lookup"><span data-stu-id="09d96-121">**Do not** select hello *Enter hello Hive/Oozie Metastore*.</span></span> <span data-ttu-id="09d96-122">Potom klikněte na hello šipku toogo toohello další konfigurační stránku.</span><span class="sxs-lookup"><span data-stu-id="09d96-122">Then, click hello arrow toogo toohello next configuration page.</span></span> 

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

<span data-ttu-id="09d96-124">Na stránce konfigurace 4 zadejte název účtu úložiště hello, hello výchozí kontejner hello clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="09d96-124">On configuration page 4, specify hello storage account name, hello default container of hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="09d96-125">Pokud vyberete *vytvořit výchozí kontejner* v hello **výchozí kontejner** rozevíracího seznamu, kontejner s hello stejný název jako hello clusteru bude vytvořena.</span><span class="sxs-lookup"><span data-stu-id="09d96-125">If you select *Create default container* in hello **DEFAULT CONTAINER** dropdown list, a container with hello same name as hello cluster will be created.</span></span> <span data-ttu-id="09d96-126">Klikněte na tlačítko hello šipku toogo toohello poslední stránku konfigurace.</span><span class="sxs-lookup"><span data-stu-id="09d96-126">Click hello arrow toogo toohello last configuration page.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

<span data-ttu-id="09d96-128">Na poslední hello **akcí skriptů** stránku konfigurace, klikněte na tlačítko **přidat akce skriptu** tlačítko a vyplníte pole textu hello hello následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="09d96-128">On hello final **Script Actions** configuration page, click **add script action** button, and fill hello text fields with hello following values.</span></span>

* <span data-ttu-id="09d96-129">**NÁZEV** -libovolný řetězec jako název hello této akce skriptu</span><span class="sxs-lookup"><span data-stu-id="09d96-129">**NAME** - any string as hello name of this script action</span></span>
* <span data-ttu-id="09d96-130">**Typ uzlu** – vyberte **všechny uzly**</span><span class="sxs-lookup"><span data-stu-id="09d96-130">**NODE TYPE** - select **All nodes**</span></span>
* <span data-ttu-id="09d96-131">**Identifikátor URI skriptu** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*</span><span class="sxs-lookup"><span data-stu-id="09d96-131">**SCRIPT URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*</span></span> 
  * <span data-ttu-id="09d96-132">*publicscripts* je veřejném kontejneru v účtu úložiště hello</span><span class="sxs-lookup"><span data-stu-id="09d96-132">*publicscripts* is a public container in hello storage account</span></span> 
  * <span data-ttu-id="09d96-133">*getgoing* používáme tooshare prostředí PowerShell skriptu soubory toofacilitate práci uživatelů v Azure</span><span class="sxs-lookup"><span data-stu-id="09d96-133">*getgoing* we use tooshare PowerShell script files toofacilitate users' work in Azure</span></span>
* <span data-ttu-id="09d96-134">**Parametry** -(ponechte prázdné)</span><span class="sxs-lookup"><span data-stu-id="09d96-134">**PARAMETERS** - (leave blank)</span></span>

<span data-ttu-id="09d96-135">Nakonec klikněte na hello zaškrtnutí toostart hello vytváření clusteru HDInsight Hadoop hello přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="09d96-135">Finally, click hello check mark toostart hello creation of hello customized HDInsight Hadoop cluster.</span></span> 

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <span data-ttu-id="09d96-137"><a name="headnode"></a>Přístup k hello uzlu Head clusteru Hadoop</span><span class="sxs-lookup"><span data-stu-id="09d96-137"><a name="headnode"></a> Access hello Head Node of Hadoop Cluster</span></span>
<span data-ttu-id="09d96-138">Než se dostanete k hlavnímu uzlu clusteru Hadoop hello hello prostřednictvím protokolu RDP, je nutné povolit clusteru Hadoop toohello vzdáleného přístupu v Azure.</span><span class="sxs-lookup"><span data-stu-id="09d96-138">You must enable remote access toohello Hadoop cluster in Azure before you can access hello head node of hello Hadoop cluster through RDP.</span></span> 

1. <span data-ttu-id="09d96-139">Přihlaste se toohello [ **portál Azure classic**](https://manage.windowsazure.com/), vyberte **HDInsight** na levé straně hello vyberte Hadoop cluster hello seznamu clusterů, klikněte na hello  **KONFIGURACE** a pak klikněte hello **povolit vzdálené** ikonu v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="09d96-139">Log in toohello [**Azure classic portal**](https://manage.windowsazure.com/), select **HDInsight** on hello left, select your Hadoop cluster from hello list of clusters, click hello **CONFIGURATION** tab, and then click hello **ENABLE REMOTE** icon at hello bottom of hello page.</span></span>
   
    ![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)
2. <span data-ttu-id="09d96-141">V hello **konfigurace vzdálené plochy** okno, zadejte hello uživatelské jméno a heslo pole a zvolte datum vypršení platnosti hello pro vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="09d96-141">In hello **Configure Remote Desktop** window, enter hello USER NAME and PASSWORD fields, and select hello expiration date for remote access.</span></span> <span data-ttu-id="09d96-142">Klikněte na tlačítko hello zaškrtnutí tooenable hello vzdáleného přístupu toohello hlavního uzlu v clusteru Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="09d96-142">Then click hello check mark tooenable hello remote access toohello head node of hello Hadoop cluster.</span></span>
   
    ![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> <span data-ttu-id="09d96-144">Hello uživatelské jméno a heslo pro vzdálený přístup hello nejsou hello uživatelské jméno a heslo, které používáte při vytváření clusteru Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="09d96-144">hello user name and password for hello remote access are not hello user name and password that you use when you created hello Hadoop cluster.</span></span> <span data-ttu-id="09d96-145">Toto je samostatnou sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="09d96-145">This is a separate set of credentials.</span></span> <span data-ttu-id="09d96-146">Datum vypršení platnosti hello hello vzdáleného přístupu má taky, toobe do 7 dnů od hello aktuální datum.</span><span class="sxs-lookup"><span data-stu-id="09d96-146">Also, hello expiration date of hello remote access has toobe within 7 days from hello current date.</span></span>
> 
> 

<span data-ttu-id="09d96-147">Po povolení vzdáleného přístupu klikněte na tlačítko **CONNECT** dole hello tooremote stránku hello do hlavního uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="09d96-147">After remote access is enabled, click **CONNECT** at hello bottom of hello page tooremote into hello head node.</span></span> <span data-ttu-id="09d96-148">Přihlášení toohello hlavního uzlu v clusteru Hadoop hello zadáním hello pověření pro uživatele hello vzdáleného přístupu, který jste dřív zadali.</span><span class="sxs-lookup"><span data-stu-id="09d96-148">You log on toohello head node of hello Hadoop cluster by entering hello credentials for hello remote access user that you specified earlier.</span></span>

![Vytvořit pracovní prostor](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

<span data-ttu-id="09d96-150">Hello další kroky v hello pokročilé analýzy procesu jsou namapované v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může obsahovat kroky, které přesun dat do HDInsight, pak zpracovat a ukázkové ho existuje v rámci přípravy učení se z dat hello pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="09d96-150">hello next steps in hello advanced analytics process are mapped in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) and may include steps that move data into HDInsight, then process and sample it there in preparation for learning from hello data with Azure Machine Learning.</span></span>

<span data-ttu-id="09d96-151">V tématu [jak dotazů Hive toosubmit](machine-learning-data-science-move-hive-tables.md#submit) pokyny, jak tooaccess hello moduly jazyka Python, které jsou součástí Anaconda z hlavního uzlu clusteru hello v uživatelsky definované funkce (UDF), které jsou používané tooprocess hello Hive záznamy uložené v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="09d96-151">See [How toosubmit Hive queries](machine-learning-data-science-move-hive-tables.md#submit) for instructions on how tooaccess hello Python modules that are included in Anaconda from hello head node of hello cluster in user-defined functions (UDFs) that are used tooprocess Hive records stored in hello cluster.</span></span>

