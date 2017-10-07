---
title: "Vytvoření - Azure clusteru knihovny Hive aaaAdd během HDInsight | Microsoft Docs"
description: "Zjistěte, jak knihovny Hive tooadd (souborů jar), tooan HDInsight clusteru při vytváření clusteru."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 2fd74b8d-c006-45c6-a9e2-72ff5d2d978a
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 2e028a07c3248205def0789af2c262a0774a8f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-hive-libraries-when-creating-your-hdinsight-cluster"></a><span data-ttu-id="923d0-103">Přidat vlastní knihovny Hive, při vytváření clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="923d0-103">Add custom Hive libraries when creating your HDInsight cluster</span></span>

<span data-ttu-id="923d0-104">Pokud máte knihovny, které často používáte s nástrojem Hive v HDInsight, tento dokument obsahuje informace o používání knihovny hello akce skriptu toopre zatížení při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="923d0-104">If you have libraries that you use frequently with Hive on HDInsight, this document contains information on using a Script Action toopre-load hello libraries during cluster creation.</span></span> <span data-ttu-id="923d0-105">Knihovny přidány pomocí hello kroky v tomto dokumentu jsou globálně dostupnou v podregistru – neexistuje žádná potřeba toouse [přidat JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) tooload je.</span><span class="sxs-lookup"><span data-stu-id="923d0-105">Libraries added using hello steps in this document are globally available in Hive - there is no need toouse [ADD JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) tooload them.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="923d0-106">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="923d0-106">How it works</span></span>

<span data-ttu-id="923d0-107">Při vytváření clusteru, Volitelně můžete zadat akce skriptu, který spouští skript na uzlech clusteru hello při jejich vytváření.</span><span class="sxs-lookup"><span data-stu-id="923d0-107">When creating a cluster, you can optionally specify a Script Action that runs a script on hello cluster nodes while they are being created.</span></span> <span data-ttu-id="923d0-108">Hello skript v tomto dokumentu přijímá jeden parametr, který je WASB umístění, které obsahuje toobe knihovny (uložené jako soubory jar) hello předem načtená.</span><span class="sxs-lookup"><span data-stu-id="923d0-108">hello script in this document accepts a single parameter, which is a WASB location that contains hello libraries (stored as jar files) toobe pre-loaded.</span></span>

<span data-ttu-id="923d0-109">Při vytváření clusteru hello skript vytvoří výčet hello soubory, kopíruje je toohello `/usr/lib/customhivelibs/` adresáře na head a pracovní uzly, potom se přidají toohello `hive.aux.jars.path` vlastnost hello `core-site.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="923d0-109">During cluster creation, hello script enumerates hello files, copies them toohello `/usr/lib/customhivelibs/` directory on head and worker nodes, then adds them toohello `hive.aux.jars.path` property in hello `core-site.xml` file.</span></span> <span data-ttu-id="923d0-110">Na clusterech se systémem Linux, aktualizuje také hello `hive-env.sh` soubor s hello umístění souborů hello.</span><span class="sxs-lookup"><span data-stu-id="923d0-110">On Linux-based clusters, it also updates hello `hive-env.sh` file with hello location of hello files.</span></span>

> [!NOTE]
> <span data-ttu-id="923d0-111">Pomocí akcí skriptů hello v tomto článku zpřístupní hello knihovny v hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="923d0-111">Using hello script actions in this article makes hello libraries available in hello following scenarios:</span></span>
>
> * <span data-ttu-id="923d0-112">**HDInsight se systémem Linux** – při použití hello klienta Hive, **WebHCat**, a **HiveServer2**.</span><span class="sxs-lookup"><span data-stu-id="923d0-112">**Linux-based HDInsight** - when using hello a Hive client, **WebHCat**, and **HiveServer2**.</span></span>
> * <span data-ttu-id="923d0-113">**HDInsight se systémem Windows** – Pokud pomocí klienta hello Hive a **WebHCat**.</span><span class="sxs-lookup"><span data-stu-id="923d0-113">**Windows-based HDInsight** - when using hello Hive client and **WebHCat**.</span></span>

## <a name="hello-script"></a><span data-ttu-id="923d0-114">skript Hello</span><span class="sxs-lookup"><span data-stu-id="923d0-114">hello script</span></span>

<span data-ttu-id="923d0-115">**Umístění skriptu**</span><span class="sxs-lookup"><span data-stu-id="923d0-115">**Script location**</span></span>

<span data-ttu-id="923d0-116">Pro **clusterech se systémem Linux**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)</span><span class="sxs-lookup"><span data-stu-id="923d0-116">For **Linux-based clusters**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)</span></span>

<span data-ttu-id="923d0-117">Pro **clusterech se systémem Windows**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)</span><span class="sxs-lookup"><span data-stu-id="923d0-117">For **Windows-based clusters**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="923d0-118">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="923d0-118">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="923d0-119">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="923d0-119">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="923d0-120">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="923d0-120">**Requirements**</span></span>

* <span data-ttu-id="923d0-121">skripty Hello musí být použité tooboth hello **hlavní uzly** a **uzlů pracovního procesu**.</span><span class="sxs-lookup"><span data-stu-id="923d0-121">hello scripts must be applied tooboth hello **Head nodes** and **Worker nodes**.</span></span>

* <span data-ttu-id="923d0-122">Hello JAR chcete tooinstall musí být uložen v úložišti objektů Blob Azure v **jediný kontejner**.</span><span class="sxs-lookup"><span data-stu-id="923d0-122">hello jars you wish tooinstall must be stored in Azure Blob Storage in a **single container**.</span></span>

* <span data-ttu-id="923d0-123">účet úložiště Hello obsahující hello knihovně souborů jar **musí** být během vytváření clusteru HDInsight propojené toohello.</span><span class="sxs-lookup"><span data-stu-id="923d0-123">hello storage account containing hello library of jar files **must** be linked toohello HDInsight cluster during creation.</span></span> <span data-ttu-id="923d0-124">Musí být buď hello výchozí účet úložiště, nebo účet přidané prostřednictvím __volitelné konfiguraci__.</span><span class="sxs-lookup"><span data-stu-id="923d0-124">It must either be hello default storage account, or an account added through __optional configuration__.</span></span>

* <span data-ttu-id="923d0-125">Hello WASB cesta toohello kontejneru musí být zadány jako parametr toohello akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="923d0-125">hello WASB path toohello container must be specified as a parameter toohello Script Action.</span></span> <span data-ttu-id="923d0-126">Například pokud hello JAR jsou uloženy v kontejneru nazvaném **knihovny** na účet úložiště s názvem **mystorage**, bude parametr hello  **wasb://libs@mystorage.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="923d0-126">For example, if hello jars are stored in a container named **libs** on a storage account named **mystorage**, hello parameter would be **wasb://libs@mystorage.blob.core.windows.net/**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="923d0-127">Tento dokument předpokládá, je již vytvoření účtu úložiště, kontejner objektů blob a tooit soubory nahrané hello.</span><span class="sxs-lookup"><span data-stu-id="923d0-127">This document assumes that you have already create a storage account, blob container, and uploaded hello files tooit.</span></span>
  >
  > <span data-ttu-id="923d0-128">Pokud jste dosud nevytvořili účet úložiště, můžete tak učinit pomocí hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="923d0-128">If you have not created a storage account, you can do so through hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="923d0-129">Potom můžete pomocí nástroje, jako [Azure Storage Explorer](http://storageexplorer.com/) toocreate kontejneru v účtu hello a nahrání souborů tooit.</span><span class="sxs-lookup"><span data-stu-id="923d0-129">You can then use a utility such as [Azure Storage Explorer](http://storageexplorer.com/) toocreate a container in hello account and upload files tooit.</span></span>

## <a name="create-a-cluster-using-hello-script"></a><span data-ttu-id="923d0-130">Vytvoření clusteru s podporou pomocí skriptu hello</span><span class="sxs-lookup"><span data-stu-id="923d0-130">Create a cluster using hello script</span></span>

> [!NOTE]
> <span data-ttu-id="923d0-131">Hello postupem vytvoření clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="923d0-131">hello following steps create a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="923d0-132">toocreate cluster systému Windows vyberte **Windows** jako hello clusteru operačního systému při vytváření clusteru hello a pomocí skriptu Windows (PowerShell) hello místo hello bash skriptu.</span><span class="sxs-lookup"><span data-stu-id="923d0-132">toocreate a Windows-based cluster, select **Windows** as hello cluster OS when creating hello cluster, and use hello Windows (PowerShell) script instead of hello bash script.</span></span>
>
> <span data-ttu-id="923d0-133">Můžete také použít Azure PowerShell nebo hello SDK rozhraní .NET HDInsight toocreate clusteru pomocí tohoto skriptu.</span><span class="sxs-lookup"><span data-stu-id="923d0-133">You can also use Azure PowerShell or hello HDInsight .NET SDK toocreate a cluster using this script.</span></span> <span data-ttu-id="923d0-134">Další informace o použití těchto metod najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="923d0-134">For more information on using these methods, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

1. <span data-ttu-id="923d0-135">Spuštění zřizování clusteru pomocí kroků hello v [zřizování clusterů HDInsight v Linuxu](hdinsight-hadoop-provision-linux-clusters.md), ale se nedokončí zřizování.</span><span class="sxs-lookup"><span data-stu-id="923d0-135">Start provisioning a cluster by using hello steps in [Provision HDInsight clusters on Linux](hdinsight-hadoop-provision-linux-clusters.md), but do not complete provisioning.</span></span>

2. <span data-ttu-id="923d0-136">Na hello **volitelné konfiguraci** vyberte **akcí skriptů**a zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="923d0-136">On hello **Optional Configuration** blade, select **Script Actions**, and provide hello following information:</span></span>

   * <span data-ttu-id="923d0-137">**NÁZEV**: Zadejte popisný název akce skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="923d0-137">**NAME**: Enter a friendly name for hello script action.</span></span>

   * <span data-ttu-id="923d0-138">**Identifikátor URI skriptu**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh</span><span class="sxs-lookup"><span data-stu-id="923d0-138">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh</span></span>

   * <span data-ttu-id="923d0-139">**HEAD**: zaškrtnete tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="923d0-139">**HEAD**: Check this option.</span></span>

   * <span data-ttu-id="923d0-140">**PRACOVNÍ**: zaškrtnete tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="923d0-140">**WORKER**: Check this option.</span></span>

   * <span data-ttu-id="923d0-141">**ZOOKEEPER**: nechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="923d0-141">**ZOOKEEPER**: Leave this blank.</span></span>

   * <span data-ttu-id="923d0-142">**Parametry**: Zadejte hello WASB adresu toohello úložiště a kontejneru účtu, který obsahuje kromě souborů JAR hello.</span><span class="sxs-lookup"><span data-stu-id="923d0-142">**PARAMETERS**: Enter hello WASB address toohello container and storage account that contains hello jars.</span></span> <span data-ttu-id="923d0-143">Například  **wasb://libs@mystorage.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="923d0-143">For example, **wasb://libs@mystorage.blob.core.windows.net/**.</span></span>

3. <span data-ttu-id="923d0-144">Na konci hello hello **akcí skriptů**, použijte hello **vyberte** tlačítko toosave hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="923d0-144">At hello bottom of hello **Script Actions**, use hello **Select** button toosave hello configuration.</span></span>

4. <span data-ttu-id="923d0-145">Na hello **volitelné konfiguraci** vyberte **propojených účtech Storage** a vyberte hello **přidejte klíč k úložišti** odkaz.</span><span class="sxs-lookup"><span data-stu-id="923d0-145">On hello **Optional Configuration** blade, select **Linked Storage Accounts** and select hello **Add a storage key** link.</span></span> <span data-ttu-id="923d0-146">Vyberte účet úložiště hello, který obsahuje kromě souborů JAR hello a potom pomocí hello **vyberte** nastavení toosave tlačítka a návratové hello **volitelné konfiguraci** okno.</span><span class="sxs-lookup"><span data-stu-id="923d0-146">Select hello storage account that contains hello jars, and then use hello **select** buttons toosave settings and return hello **Optional Configuration** blade.</span></span>

5. <span data-ttu-id="923d0-147">Použití hello **vyberte** tlačítko dole hello hello **volitelné konfiguraci** okno toosave hello volitelné konfiguraci informace.</span><span class="sxs-lookup"><span data-stu-id="923d0-147">Use hello **Select** button at hello bottom of hello **Optional Configuration** blade toosave hello optional configuration information.</span></span>

6. <span data-ttu-id="923d0-148">Pokračovat zřizování hello clusteru, jak je popsáno v [zřizování clusterů HDInsight v Linuxu](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="923d0-148">Continue provisioning hello cluster as described in [Provision HDInsight clusters on Linux](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="923d0-149">Po dokončení vytváření clusteru, budete moct toouse hello JAR přidány prostřednictvím tohoto skriptu z Hive, bez nutnosti toouse hello `ADD JAR` příkaz.</span><span class="sxs-lookup"><span data-stu-id="923d0-149">Once cluster creation finishes, you are able toouse hello jars added through this script from Hive without having toouse hello `ADD JAR` statement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="923d0-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="923d0-150">Next steps</span></span>

<span data-ttu-id="923d0-151">Další informace o práci s Hive naleznete v tématu [používání Hive s HDInsight](hdinsight-use-hive.md)</span><span class="sxs-lookup"><span data-stu-id="923d0-151">For more information on working with Hive, see [Use Hive with HDInsight](hdinsight-use-hive.md)</span></span>
