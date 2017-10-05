---
title: "Přidání knihovny Hive během vytváření clusteru HDInsight - Azure | Microsoft Docs"
description: "Informace o postupu přidání knihovny Hive (soubory jar) do clusteru HDInsight při vytváření clusteru."
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
ms.openlocfilehash: 3412864384961e8820d6700c1bf22a4cae64ba4b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-custom-hive-libraries-when-creating-your-hdinsight-cluster"></a><span data-ttu-id="103f4-103">Přidat vlastní knihovny Hive, při vytváření clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="103f4-103">Add custom Hive libraries when creating your HDInsight cluster</span></span>

<span data-ttu-id="103f4-104">Pokud máte knihovny, které často používáte s nástrojem Hive v HDInsight, tento dokument obsahuje informace o použití akce skriptu pro předběžné načtení knihovny při vytváření clusteru.</span><span class="sxs-lookup"><span data-stu-id="103f4-104">If you have libraries that you use frequently with Hive on HDInsight, this document contains information on using a Script Action to pre-load the libraries during cluster creation.</span></span> <span data-ttu-id="103f4-105">Knihovny přidány, pomocí kroků v tomto dokumentu jsou globálně dostupnou v podregistru - není nutné používat [přidat JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) k jejich načtení.</span><span class="sxs-lookup"><span data-stu-id="103f4-105">Libraries added using the steps in this document are globally available in Hive - there is no need to use [ADD JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) to load them.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="103f4-106">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="103f4-106">How it works</span></span>

<span data-ttu-id="103f4-107">Při vytváření clusteru, Volitelně můžete zadat akce skriptu, který spouští skript na uzlech clusteru při jejich vytváření.</span><span class="sxs-lookup"><span data-stu-id="103f4-107">When creating a cluster, you can optionally specify a Script Action that runs a script on the cluster nodes while they are being created.</span></span> <span data-ttu-id="103f4-108">Skript v tomto dokumentu přijímá jeden parametr, který je WASB umístění, které obsahuje předem načíst knihovny (uložené jako soubory jar).</span><span class="sxs-lookup"><span data-stu-id="103f4-108">The script in this document accepts a single parameter, which is a WASB location that contains the libraries (stored as jar files) to be pre-loaded.</span></span>

<span data-ttu-id="103f4-109">Při vytváření clusteru, skript vytvoří výčet soubory, kopíruje je do `/usr/lib/customhivelibs/` přidá je do adresáře na head a pracovní uzly, pak `hive.aux.jars.path` vlastnost v `core-site.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="103f4-109">During cluster creation, the script enumerates the files, copies them to the `/usr/lib/customhivelibs/` directory on head and worker nodes, then adds them to the `hive.aux.jars.path` property in the `core-site.xml` file.</span></span> <span data-ttu-id="103f4-110">Na clusterech se systémem Linux, aktualizuje také `hive-env.sh` soubor s umístění souborů.</span><span class="sxs-lookup"><span data-stu-id="103f4-110">On Linux-based clusters, it also updates the `hive-env.sh` file with the location of the files.</span></span>

> [!NOTE]
> <span data-ttu-id="103f4-111">Pomocí akcí skriptů v tomto článku zpřístupní v knihovnách v následujících scénářích:</span><span class="sxs-lookup"><span data-stu-id="103f4-111">Using the script actions in this article makes the libraries available in the following scenarios:</span></span>
>
> * <span data-ttu-id="103f4-112">**HDInsight se systémem Linux** – při použití Hive klienta, **WebHCat**, a **HiveServer2**.</span><span class="sxs-lookup"><span data-stu-id="103f4-112">**Linux-based HDInsight** - when using the a Hive client, **WebHCat**, and **HiveServer2**.</span></span>
> * <span data-ttu-id="103f4-113">**HDInsight se systémem Windows** – při použití Hive klienta a **WebHCat**.</span><span class="sxs-lookup"><span data-stu-id="103f4-113">**Windows-based HDInsight** - when using the Hive client and **WebHCat**.</span></span>

## <a name="the-script"></a><span data-ttu-id="103f4-114">Skript</span><span class="sxs-lookup"><span data-stu-id="103f4-114">The script</span></span>

<span data-ttu-id="103f4-115">**Umístění skriptu**</span><span class="sxs-lookup"><span data-stu-id="103f4-115">**Script location**</span></span>

<span data-ttu-id="103f4-116">Pro **clusterech se systémem Linux**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)</span><span class="sxs-lookup"><span data-stu-id="103f4-116">For **Linux-based clusters**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)</span></span>

<span data-ttu-id="103f4-117">Pro **clusterech se systémem Windows**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)</span><span class="sxs-lookup"><span data-stu-id="103f4-117">For **Windows-based clusters**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="103f4-118">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="103f4-118">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="103f4-119">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="103f4-119">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="103f4-120">**Požadavky**</span><span class="sxs-lookup"><span data-stu-id="103f4-120">**Requirements**</span></span>

* <span data-ttu-id="103f4-121">Skripty se musí použít pro obě **hlavní uzly** a **uzlů pracovního procesu**.</span><span class="sxs-lookup"><span data-stu-id="103f4-121">The scripts must be applied to both the **Head nodes** and **Worker nodes**.</span></span>

* <span data-ttu-id="103f4-122">JAR chcete nainstalovat, musí být uložen v úložišti objektů Blob Azure v **jediný kontejner**.</span><span class="sxs-lookup"><span data-stu-id="103f4-122">The jars you wish to install must be stored in Azure Blob Storage in a **single container**.</span></span>

* <span data-ttu-id="103f4-123">Účet úložiště obsahuje knihovnu souborů jar **musí** propojit během vytváření clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="103f4-123">The storage account containing the library of jar files **must** be linked to the HDInsight cluster during creation.</span></span> <span data-ttu-id="103f4-124">Musí být buď výchozí účet úložiště, nebo účet přidané prostřednictvím __volitelné konfiguraci__.</span><span class="sxs-lookup"><span data-stu-id="103f4-124">It must either be the default storage account, or an account added through __optional configuration__.</span></span>

* <span data-ttu-id="103f4-125">WASB cesta ke kontejneru musí být zadána jako parametr pro akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="103f4-125">The WASB path to the container must be specified as a parameter to the Script Action.</span></span> <span data-ttu-id="103f4-126">Například, pokud kromě souborů JAR jsou uloženy v kontejneru nazvaném **knihovny** na účet úložiště s názvem **mystorage**, bude parametr  **wasb://libs@mystorage.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="103f4-126">For example, if the jars are stored in a container named **libs** on a storage account named **mystorage**, the parameter would be **wasb://libs@mystorage.blob.core.windows.net/**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="103f4-127">Tento dokument předpokládá, že jste již vytvoření účtu úložiště, kontejner objektů blob a odeslat soubory do ní.</span><span class="sxs-lookup"><span data-stu-id="103f4-127">This document assumes that you have already create a storage account, blob container, and uploaded the files to it.</span></span>
  >
  > <span data-ttu-id="103f4-128">Pokud jste nevytvořili účet úložiště, můžete to udělat tak prostřednictvím [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="103f4-128">If you have not created a storage account, you can do so through the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="103f4-129">Potom můžete pomocí nástroje, jako [Azure Storage Explorer](http://storageexplorer.com/) vytvořit kontejner v účtu a odeslat soubory do ní.</span><span class="sxs-lookup"><span data-stu-id="103f4-129">You can then use a utility such as [Azure Storage Explorer](http://storageexplorer.com/) to create a container in the account and upload files to it.</span></span>

## <a name="create-a-cluster-using-the-script"></a><span data-ttu-id="103f4-130">Vytvoření clusteru s podporou pomocí skriptu</span><span class="sxs-lookup"><span data-stu-id="103f4-130">Create a cluster using the script</span></span>

> [!NOTE]
> <span data-ttu-id="103f4-131">Následující postup vytvoření clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="103f4-131">The following steps create a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="103f4-132">K vytvoření clusteru se systémem Windows, vyberte **Windows** jako cluster operačního systému při vytváření clusteru a použití skriptu systému Windows (PowerShell) namísto skriptů bash.</span><span class="sxs-lookup"><span data-stu-id="103f4-132">To create a Windows-based cluster, select **Windows** as the cluster OS when creating the cluster, and use the Windows (PowerShell) script instead of the bash script.</span></span>
>
> <span data-ttu-id="103f4-133">Prostředí Azure PowerShell nebo sady SDK rozhraní .NET HDInsight můžete také použít k vytvoření clusteru pomocí tohoto skriptu.</span><span class="sxs-lookup"><span data-stu-id="103f4-133">You can also use Azure PowerShell or the HDInsight .NET SDK to create a cluster using this script.</span></span> <span data-ttu-id="103f4-134">Další informace o použití těchto metod najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="103f4-134">For more information on using these methods, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

1. <span data-ttu-id="103f4-135">Spuštění zřizování clusteru pomocí kroků v [zřizování clusterů HDInsight v Linuxu](hdinsight-hadoop-provision-linux-clusters.md), ale se nedokončí zřizování.</span><span class="sxs-lookup"><span data-stu-id="103f4-135">Start provisioning a cluster by using the steps in [Provision HDInsight clusters on Linux](hdinsight-hadoop-provision-linux-clusters.md), but do not complete provisioning.</span></span>

2. <span data-ttu-id="103f4-136">Na **volitelné konfiguraci** vyberte **akcí skriptů**a zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="103f4-136">On the **Optional Configuration** blade, select **Script Actions**, and provide the following information:</span></span>

   * <span data-ttu-id="103f4-137">**NÁZEV**: Zadejte popisný název akce skriptu.</span><span class="sxs-lookup"><span data-stu-id="103f4-137">**NAME**: Enter a friendly name for the script action.</span></span>

   * <span data-ttu-id="103f4-138">**Identifikátor URI skriptu**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh</span><span class="sxs-lookup"><span data-stu-id="103f4-138">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh</span></span>

   * <span data-ttu-id="103f4-139">**HEAD**: zaškrtnete tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="103f4-139">**HEAD**: Check this option.</span></span>

   * <span data-ttu-id="103f4-140">**PRACOVNÍ**: zaškrtnete tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="103f4-140">**WORKER**: Check this option.</span></span>

   * <span data-ttu-id="103f4-141">**ZOOKEEPER**: nechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="103f4-141">**ZOOKEEPER**: Leave this blank.</span></span>

   * <span data-ttu-id="103f4-142">**Parametry**: Zadejte adresu WASB účtu úložiště a kontejneru, který obsahuje JAR.</span><span class="sxs-lookup"><span data-stu-id="103f4-142">**PARAMETERS**: Enter the WASB address to the container and storage account that contains the jars.</span></span> <span data-ttu-id="103f4-143">Například  **wasb://libs@mystorage.blob.core.windows.net/** .</span><span class="sxs-lookup"><span data-stu-id="103f4-143">For example, **wasb://libs@mystorage.blob.core.windows.net/**.</span></span>

3. <span data-ttu-id="103f4-144">V dolní části **akcí skriptů**, použijte **vyberte** tlačítko Uložit konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="103f4-144">At the bottom of the **Script Actions**, use the **Select** button to save the configuration.</span></span>

4. <span data-ttu-id="103f4-145">Na **volitelné konfiguraci** vyberte **propojených účtech Storage** a vyberte **přidejte klíč k úložišti** odkaz.</span><span class="sxs-lookup"><span data-stu-id="103f4-145">On the **Optional Configuration** blade, select **Linked Storage Accounts** and select the **Add a storage key** link.</span></span> <span data-ttu-id="103f4-146">Vyberte účet úložiště, který obsahuje JAR a potom pomocí **vyberte** tlačítka pro uložení nastavení a návrat **volitelné konfiguraci** okno.</span><span class="sxs-lookup"><span data-stu-id="103f4-146">Select the storage account that contains the jars, and then use the **select** buttons to save settings and return the **Optional Configuration** blade.</span></span>

5. <span data-ttu-id="103f4-147">Použití **vyberte** tlačítko v dolní části **volitelné konfiguraci** okno a uložte informace o volitelné konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="103f4-147">Use the **Select** button at the bottom of the **Optional Configuration** blade to save the optional configuration information.</span></span>

6. <span data-ttu-id="103f4-148">Pokračovat zřizování clusteru, jak je popsáno v [zřizování clusterů HDInsight v Linuxu](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="103f4-148">Continue provisioning the cluster as described in [Provision HDInsight clusters on Linux](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="103f4-149">Po dokončení vytváření clusteru, budete moci použít JAR přidány prostřednictvím tohoto skriptu z Hive, bez nutnosti použití `ADD JAR` příkaz.</span><span class="sxs-lookup"><span data-stu-id="103f4-149">Once cluster creation finishes, you are able to use the jars added through this script from Hive without having to use the `ADD JAR` statement.</span></span>

## <a name="next-steps"></a><span data-ttu-id="103f4-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="103f4-150">Next steps</span></span>

<span data-ttu-id="103f4-151">Další informace o práci s Hive naleznete v tématu [používání Hive s HDInsight](hdinsight-use-hive.md)</span><span class="sxs-lookup"><span data-stu-id="103f4-151">For more information on working with Hive, see [Use Hive with HDInsight](hdinsight-use-hive.md)</span></span>
