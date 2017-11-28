---
title: aaaInstall nebo aktualizovat Mono v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak toouse na konkrétní verzi Mono s clusterem HDInsight. Mono je použité toorun aplikací .NET na clustery HDInsight se systémem Linux."
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
ms.openlocfilehash: 1e8a8aaeff231c93a9e232379448517b326da965
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-or-update-mono-on-hdinsight"></a><span data-ttu-id="1d6e5-104">Nainstalujte nebo aktualizujte Mono v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1d6e5-104">Install or update Mono on HDInsight</span></span>

<span data-ttu-id="1d6e5-105">Zjistěte, jak tooinstall na konkrétní verzi nástroje [Mono](https://www.mono-project.com) na HDInsight 3.4 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="1d6e5-105">Learn how tooinstall a specific version of [Mono](https://www.mono-project.com) on HDInsight 3.4 or higher.</span></span>

<span data-ttu-id="1d6e5-106">Mono je nainstalován na HDInsight 3.4 a vyšší a je použité toorun aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="1d6e5-106">Mono is installed on HDInsight 3.4 and higher, and is used toorun .NET applications.</span></span> <span data-ttu-id="1d6e5-107">Informace o verzi hello Mono zahrnuté do každé verzi HDInsight, naleznete v části [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="1d6e5-107">For information on hello version of Mono included with each HDInsight version, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="1d6e5-108">tooinstall jinou verzi v clusteru, použijte akci skriptu hello v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1d6e5-108">tooinstall a different version on your cluster, use hello script action in this document.</span></span> 

## <a name="how-it-works"></a><span data-ttu-id="1d6e5-109">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="1d6e5-109">How it works</span></span>

<span data-ttu-id="1d6e5-110">Tento skript přijímá hello následující parametr:</span><span class="sxs-lookup"><span data-stu-id="1d6e5-110">This script accepts hello following parameter:</span></span>

* <span data-ttu-id="1d6e5-111">__Číslo verze mono__: hello verzi Mono tooinstall.</span><span class="sxs-lookup"><span data-stu-id="1d6e5-111">__Mono version number__: hello version of Mono tooinstall.</span></span> <span data-ttu-id="1d6e5-112">Hello verze musí být dostupný z [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).</span><span class="sxs-lookup"><span data-stu-id="1d6e5-112">hello version must be available from [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).</span></span>

<span data-ttu-id="1d6e5-113">skript Hello nainstaluje hello následující Mono balíčky:</span><span class="sxs-lookup"><span data-stu-id="1d6e5-113">hello script installs hello following Mono packages:</span></span>

* <span data-ttu-id="1d6e5-114">__dokončení mono__</span><span class="sxs-lookup"><span data-stu-id="1d6e5-114">__mono-complete__</span></span>

* <span data-ttu-id="1d6e5-115">__certifikační autority certifikáty mono__</span><span class="sxs-lookup"><span data-stu-id="1d6e5-115">__ca-certificates-mono__</span></span>

## <a name="hello-script"></a><span data-ttu-id="1d6e5-116">skript Hello</span><span class="sxs-lookup"><span data-stu-id="1d6e5-116">hello script</span></span>

<span data-ttu-id="1d6e5-117">__Skript umístění__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)</span><span class="sxs-lookup"><span data-stu-id="1d6e5-117">__Script location__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)</span></span>

<span data-ttu-id="1d6e5-118">__Požadavky na__:</span><span class="sxs-lookup"><span data-stu-id="1d6e5-118">__Requirements__:</span></span>

* <span data-ttu-id="1d6e5-119">skript Hello se musí použít na hello __hlavní uzly__ a __uzlů pracovního procesu__.</span><span class="sxs-lookup"><span data-stu-id="1d6e5-119">hello script must be applied on hello __head nodes__ and __worker nodes__.</span></span>

## <a name="toouse-hello-script"></a><span data-ttu-id="1d6e5-120">toouse hello skriptu</span><span class="sxs-lookup"><span data-stu-id="1d6e5-120">toouse hello script</span></span>

<span data-ttu-id="1d6e5-121">Informace o tom, jak toouse tento skript s HDInsight, najdete v části hello [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1d6e5-121">For information on how toouse this script with HDInsight, see hello [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.</span></span> <span data-ttu-id="1d6e5-122">Můžete použít skript hello prostřednictvím hello portál Azure, Azure PowerShell nebo hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1d6e5-122">You can use hello script through hello Azure portal, Azure PowerShell, or hello Azure CLI.</span></span>

<span data-ttu-id="1d6e5-123">Při následující hello dokumentu akce skriptu, použijte následující identifikátor URI hello:</span><span class="sxs-lookup"><span data-stu-id="1d6e5-123">While following hello script action document, use hello following URI:</span></span>

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

> [!NOTE]
> <span data-ttu-id="1d6e5-124">Při konfiguraci HDInsight pomocí tohoto skriptu, označte hello skriptu jako __trvalé__.</span><span class="sxs-lookup"><span data-stu-id="1d6e5-124">When configuring HDInsight with this script, mark hello script as __Persisted__.</span></span> <span data-ttu-id="1d6e5-125">Toto nastavení umožňuje HDInsight tooapply hello skriptu tooworker uzly přidávají prostřednictvím škálování operace.</span><span class="sxs-lookup"><span data-stu-id="1d6e5-125">This setting allows HDInsight tooapply hello script tooworker nodes added through scaling operations.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1d6e5-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d6e5-126">Next steps</span></span>

<span data-ttu-id="1d6e5-127">Jste se naučili, jak tooupgrade nebo nainstalujte na konkrétní verzi Mono v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1d6e5-127">You have learned how tooupgrade or install a specific version of Mono on HDInsight.</span></span> <span data-ttu-id="1d6e5-128">Další informace o používání aplikací .NET s Mono v HDInsight naleznete v tématu hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="1d6e5-128">For more information on using .NET applications with Mono on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="1d6e5-129">Použití rozhraní .NET pro streamování MapReduce v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1d6e5-129">Use .NET for streaming MapReduce on HDInsight</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [<span data-ttu-id="1d6e5-130">Použití rozhraní .NET v Hive a Pig v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1d6e5-130">Use .NET with Hive and Pig on HDInsight</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)
* [<span data-ttu-id="1d6e5-131">Vývoj řešení C# se Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="1d6e5-131">Develop C# solutions with Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="1d6e5-132">Migrace řešení pro rozhraní .NET HDInsight se systémem tooLinux</span><span class="sxs-lookup"><span data-stu-id="1d6e5-132">Migrate .NET solutions tooLinux-based HDInsight</span></span>](hdinsight-hadoop-migrate-dotnet-to-linux.md)

<span data-ttu-id="1d6e5-133">Další informace o použití akce skriptu najdete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="1d6e5-133">For more information on using script actions, see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>