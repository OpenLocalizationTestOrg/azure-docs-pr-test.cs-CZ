---
title: Nainstalujte nebo aktualizujte Mono v HDInsight - Azure | Microsoft Docs
description: "Další informace o použití na konkrétní verzi Mono s clusterem HDInsight. Mono slouží ke spouštění aplikací .NET v clusterech HDInsight se systémem Linux."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: hdinsightactive
ms.openlocfilehash: fd284542e1de65f323f1e3a092689f847e025be6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="install-or-update-mono-on-hdinsight"></a><span data-ttu-id="40f37-104">Nainstalujte nebo aktualizujte Mono v HDInsight</span><span class="sxs-lookup"><span data-stu-id="40f37-104">Install or update Mono on HDInsight</span></span>

<span data-ttu-id="40f37-105">Naučte se instalovat na konkrétní verzi nástroje [Mono](https://www.mono-project.com) na HDInsight 3.4 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="40f37-105">Learn how to install a specific version of [Mono](https://www.mono-project.com) on HDInsight 3.4 or higher.</span></span>

<span data-ttu-id="40f37-106">Mono je nainstalován na HDInsight 3.4 a vyšší a slouží ke spouštění aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="40f37-106">Mono is installed on HDInsight 3.4 and higher, and is used to run .NET applications.</span></span> <span data-ttu-id="40f37-107">Informace o verzi Mono zahrnuté do každé verzi HDInsight, naleznete v části [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="40f37-107">For information on the version of Mono included with each HDInsight version, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="40f37-108">Chcete-li nainstalovat jinou verzi v clusteru, pomocí akce skriptu v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="40f37-108">To install a different version on your cluster, use the script action in this document.</span></span> 

## <a name="how-it-works"></a><span data-ttu-id="40f37-109">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="40f37-109">How it works</span></span>

<span data-ttu-id="40f37-110">Tento skript přijímá následující parametr:</span><span class="sxs-lookup"><span data-stu-id="40f37-110">This script accepts the following parameter:</span></span>

* <span data-ttu-id="40f37-111">__Číslo verze mono__: verze Mono k instalaci.</span><span class="sxs-lookup"><span data-stu-id="40f37-111">__Mono version number__: The version of Mono to install.</span></span> <span data-ttu-id="40f37-112">Verze musí být dostupný z [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).</span><span class="sxs-lookup"><span data-stu-id="40f37-112">The version must be available from [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).</span></span>

<span data-ttu-id="40f37-113">Skript nainstaluje Mono následujících balíčků:</span><span class="sxs-lookup"><span data-stu-id="40f37-113">The script installs the following Mono packages:</span></span>

* <span data-ttu-id="40f37-114">__dokončení mono__</span><span class="sxs-lookup"><span data-stu-id="40f37-114">__mono-complete__</span></span>

* <span data-ttu-id="40f37-115">__certifikační autority certifikáty mono__</span><span class="sxs-lookup"><span data-stu-id="40f37-115">__ca-certificates-mono__</span></span>

## <a name="the-script"></a><span data-ttu-id="40f37-116">Skript</span><span class="sxs-lookup"><span data-stu-id="40f37-116">The script</span></span>

<span data-ttu-id="40f37-117">__Skript umístění__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)</span><span class="sxs-lookup"><span data-stu-id="40f37-117">__Script location__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)</span></span>

<span data-ttu-id="40f37-118">__Požadavky na__:</span><span class="sxs-lookup"><span data-stu-id="40f37-118">__Requirements__:</span></span>

* <span data-ttu-id="40f37-119">Skript se musí použít na __hlavní uzly__ a __uzlů pracovního procesu__.</span><span class="sxs-lookup"><span data-stu-id="40f37-119">The script must be applied on the __head nodes__ and __worker nodes__.</span></span>

## <a name="to-use-the-script"></a><span data-ttu-id="40f37-120">Pomocí skriptu</span><span class="sxs-lookup"><span data-stu-id="40f37-120">To use the script</span></span>

<span data-ttu-id="40f37-121">Informace o tom, jak pomocí tohoto skriptu s HDInsight naleznete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="40f37-121">For information on how to use this script with HDInsight, see the [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.</span></span> <span data-ttu-id="40f37-122">Můžete použít skript prostřednictvím portálu Azure, Azure PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="40f37-122">You can use the script through the Azure portal, Azure PowerShell, or the Azure CLI.</span></span>

<span data-ttu-id="40f37-123">Během provádění akce dokumentu skriptu, použijte následující identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="40f37-123">While following the script action document, use the following URI:</span></span>

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

> [!NOTE]
> <span data-ttu-id="40f37-124">Při konfiguraci HDInsight pomocí tohoto skriptu, označí skript jako __trvalé__.</span><span class="sxs-lookup"><span data-stu-id="40f37-124">When configuring HDInsight with this script, mark the script as __Persisted__.</span></span> <span data-ttu-id="40f37-125">Toto nastavení umožňuje HDInsight použít skript k pracovním uzlům přidávají prostřednictvím škálování operace.</span><span class="sxs-lookup"><span data-stu-id="40f37-125">This setting allows HDInsight to apply the script to worker nodes added through scaling operations.</span></span>


## <a name="next-steps"></a><span data-ttu-id="40f37-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="40f37-126">Next steps</span></span>

<span data-ttu-id="40f37-127">Jste se naučili postup instalaci nebo upgrade na konkrétní verzi Mono v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="40f37-127">You have learned how to upgrade or install a specific version of Mono on HDInsight.</span></span> <span data-ttu-id="40f37-128">Další informace o používání aplikací .NET s Mono v HDInsight najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="40f37-128">For more information on using .NET applications with Mono on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="40f37-129">Použití rozhraní .NET pro streamování MapReduce v HDInsight</span><span class="sxs-lookup"><span data-stu-id="40f37-129">Use .NET for streaming MapReduce on HDInsight</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [<span data-ttu-id="40f37-130">Použití rozhraní .NET v Hive a Pig v HDInsight</span><span class="sxs-lookup"><span data-stu-id="40f37-130">Use .NET with Hive and Pig on HDInsight</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)
* [<span data-ttu-id="40f37-131">Vývoj řešení C# se Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="40f37-131">Develop C# solutions with Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="40f37-132">Migrace řešení .NET do HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="40f37-132">Migrate .NET solutions to Linux-based HDInsight</span></span>](hdinsight-hadoop-migrate-dotnet-to-linux.md)

<span data-ttu-id="40f37-133">Další informace o použití akce skriptu najdete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="40f37-133">For more information on using script actions, see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>