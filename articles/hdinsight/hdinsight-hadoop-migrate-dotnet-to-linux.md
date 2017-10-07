---
title: "aaaUse .NET s MapReduce s Hadoop v HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse aplikace .NET pro streamování MapReduce na HDInsight se systémem Linux."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 5a4e6dc1b4dafa8cc40780e3371fa4b8ba3e3d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-toolinux-based-hdinsight"></a><span data-ttu-id="ed935-103">Migrace řešení .NET pro HDInsight se systémem Windows na základě tooLinux HDInsight</span><span class="sxs-lookup"><span data-stu-id="ed935-103">Migrate .NET solutions for Windows-based HDInsight tooLinux-based HDInsight</span></span>

<span data-ttu-id="ed935-104">Clustery HDInsight se systémem Linux použijte [Mono (https://mono-project.com)](https://mono-project.com) toorun aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="ed935-104">Linux-based HDInsight clusters use [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET applications.</span></span> <span data-ttu-id="ed935-105">Mono umožňuje toouse .NET součásti, jako jsou například aplikace MapReduce s HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="ed935-105">Mono allows you toouse .NET components such as MapReduce applications with Linux-based HDInsight.</span></span> <span data-ttu-id="ed935-106">V tomto dokumentu zjistěte, jak se vytváří toomigrate .NET řešení pro toowork clustery HDInsight se systémem Windows s Mono na HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="ed935-106">In this document, learn how toomigrate .NET solutions created for Windows-based HDInsight clusters toowork with Mono on Linux-based HDInsight.</span></span>

## <a name="mono-compatibility-with-net"></a><span data-ttu-id="ed935-107">Monofonní kompatibilitu s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="ed935-107">Mono compatibility with .NET</span></span>

<span data-ttu-id="ed935-108">Monofonní verze 4.2.1 je součástí HDInsight verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="ed935-108">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span> <span data-ttu-id="ed935-109">Další informace o verzi hello Mono zahrnuté do HDInsight naleznete v tématu [HDInsight verze součástí](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="ed935-109">For more information on hello version of Mono included with HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="ed935-110">tooinstall na konkrétní verzi Mono, najdete v části hello [instalace nebo aktualizace Mono](hdinsight-hadoop-install-mono.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ed935-110">tooinstall a specific version of Mono, see hello [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

<span data-ttu-id="ed935-111">Podrobné informace o kompatibilitě mezi Mono a rozhraní .NET najdete v tématu hello [Mono kompatibility (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ed935-111">For detailed information on compatibility between Mono and .NET, see hello [Mono compatibility (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ed935-112">je kompatibilní s Mono Hello SCP.NET framework.</span><span class="sxs-lookup"><span data-stu-id="ed935-112">hello SCP.NET framework is compatible with Mono.</span></span> <span data-ttu-id="ed935-113">Další informace o používání SCP.NET s Mono najdete v tématu [toodevelop C# topologií pomocí aplikace Visual Studio pro Apache Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="ed935-113">For more information on using SCP.NET with Mono, see [Use Visual Studio toodevelop C# topologies for Apache Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

## <a name="automated-portability-analysis"></a><span data-ttu-id="ed935-114">Analýza automatizované přenositelnost</span><span class="sxs-lookup"><span data-stu-id="ed935-114">Automated portability analysis</span></span>

<span data-ttu-id="ed935-115">Hello [.NET přenositelnost analyzátor](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) lze použít toogenerate sestavu problémům s kompatibilitou mezi aplikací a Mono.</span><span class="sxs-lookup"><span data-stu-id="ed935-115">hello [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) can be used toogenerate a report of incompatibilities between your application and Mono.</span></span> <span data-ttu-id="ed935-116">Použijte následující postup tooconfigure hello analyzátor toocheck hello vaší aplikace pro Mono přenositelnost:</span><span class="sxs-lookup"><span data-stu-id="ed935-116">Use hello following steps tooconfigure hello analyzer toocheck your application for Mono portability:</span></span>

1. <span data-ttu-id="ed935-117">Nainstalujte hello [.NET přenositelnost analyzátor](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer).</span><span class="sxs-lookup"><span data-stu-id="ed935-117">Install hello [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer).</span></span> <span data-ttu-id="ed935-118">Během instalace vyberte hello verzi sady Visual Studio toouse.</span><span class="sxs-lookup"><span data-stu-id="ed935-118">During installation, select hello version of Visual Studio toouse.</span></span>

2. <span data-ttu-id="ed935-119">Ze sady Visual Studio 2015, vyberte __analyzovat__ > __přenositelnost Analyzátor nastavení__a ujistěte se, že __4.5__ se změnami hello __Mono__  části.</span><span class="sxs-lookup"><span data-stu-id="ed935-119">From Visual Studio 2015, select __Analyze__ > __Portability Analyzer Settings__, and make sure that __4.5__ is checked in hello __Mono__ section.</span></span>

    ![4.5 změnami Mono části Nastavení analyzátor hello](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    <span data-ttu-id="ed935-121">Vyberte __OK__ toosave hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ed935-121">Select __OK__ toosave hello configuration.</span></span>

3. <span data-ttu-id="ed935-122">Vyberte __analyzovat__ > __analyzovat sestavení přenositelnost__.</span><span class="sxs-lookup"><span data-stu-id="ed935-122">Select __Analyze__ > __Analyze Assembly Portability__.</span></span> <span data-ttu-id="ed935-123">Vyberte hello sestavení, která obsahuje vaše řešení a pak vyberte __otevřete__ toobegin analýzy.</span><span class="sxs-lookup"><span data-stu-id="ed935-123">Select hello assembly that contains your solution, and then select __Open__ toobegin analysis.</span></span>

4. <span data-ttu-id="ed935-124">Po dokončení analýzy, vybrat __analyzovat__ > __zobrazit sestavy analýzy__.</span><span class="sxs-lookup"><span data-stu-id="ed935-124">Once analysis is complete, select __Analyze__ > __View analysis reports__.</span></span> <span data-ttu-id="ed935-125">V __výsledky analýzy přenositelnost__, vyberte __otevřete sestavu__ tooopen sestavy.</span><span class="sxs-lookup"><span data-stu-id="ed935-125">In __Portability Analysis Results__, select __Open report__ tooopen a report.</span></span>

    ![Dialogové okno výsledků analyzátoru přenositelnost](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]
> <span data-ttu-id="ed935-127">Analyzátor Hello nelze catch každých problém s vaším řešením.</span><span class="sxs-lookup"><span data-stu-id="ed935-127">hello analyzer cannot catch every problem with your solution.</span></span> <span data-ttu-id="ed935-128">Například soubor cestu `c:\temp\file.txt` se považuje za normální, protože Mono běží na systému Windows a hello cesta je platná v daném kontextu.</span><span class="sxs-lookup"><span data-stu-id="ed935-128">For example, a file path of `c:\temp\file.txt` is considered OK because Mono runs on Windows and hello path is valid in that context.</span></span> <span data-ttu-id="ed935-129">Ale hello cesta není platná na platformě Linux.</span><span class="sxs-lookup"><span data-stu-id="ed935-129">However, hello path is not valid on a Linux platform.</span></span>

## <a name="manual-portability-analysis"></a><span data-ttu-id="ed935-130">Analýza ruční přenositelnost</span><span class="sxs-lookup"><span data-stu-id="ed935-130">Manual portability analysis</span></span>

<span data-ttu-id="ed935-131">Proveďte ruční auditu kódu pomocí informací o hello v hello [přenositelnost aplikace (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ed935-131">Perform a manual audit of your code using hello information in hello [Application Portability (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) document.</span></span>

## <a name="modify-and-build"></a><span data-ttu-id="ed935-132">Upravit a sestavení</span><span class="sxs-lookup"><span data-stu-id="ed935-132">Modify and build</span></span>

<span data-ttu-id="ed935-133">Můžete pokračovat v sadě Visual Studio toobuild toouse řešení rozhraní .NET pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ed935-133">You can continue toouse Visual Studio toobuild your .NET solutions for HDInsight.</span></span> <span data-ttu-id="ed935-134">Ale můžete zajistit, aby byl tento projekt hello nakonfigurované toouse rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="ed935-134">However, you must ensure that hello project is configured toouse .NET Framework 4.5.</span></span>

## <a name="deploy-and-test"></a><span data-ttu-id="ed935-135">Nasazení a testování</span><span class="sxs-lookup"><span data-stu-id="ed935-135">Deploy and test</span></span>

<span data-ttu-id="ed935-136">Jakmile změníte řešení pomocí hello doporučení z hello analyzátor přenositelnost .NET nebo ruční analýzy, je nutné jej otestovat s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ed935-136">Once you have modified your solution using hello recommendations from hello .NET Portability Analyzer or from a manual analysis, you must test it with HDInsight.</span></span> <span data-ttu-id="ed935-137">Testování hello řešení na clusteru HDInsight se systémem Linux může odhalit jemně problémy, které vyžadují toobe opravena.</span><span class="sxs-lookup"><span data-stu-id="ed935-137">Testing hello solution on a Linux-based HDInsight cluster may reveal subtle problems that need toobe corrected.</span></span> <span data-ttu-id="ed935-138">Doporučujeme, abyste povolili další protokolování v aplikaci během testování.</span><span class="sxs-lookup"><span data-stu-id="ed935-138">We recommend that you enable additional logging in your application while testing it.</span></span>

<span data-ttu-id="ed935-139">Další informace o přístupu k protokoly najdete v tématu hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="ed935-139">For more information on accessing logs, see hello following documents:</span></span>

* [<span data-ttu-id="ed935-140">Přístup k protokolům aplikací YARN ve službě HDInsight s Linuxem</span><span class="sxs-lookup"><span data-stu-id="ed935-140">Access YARN application logs on Linux-based HDInsight</span></span>](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a><span data-ttu-id="ed935-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed935-141">Next steps</span></span>

* [<span data-ttu-id="ed935-142">Použití jazyka C# s MapReduce v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ed935-142">Use C# with MapReduce on HDInsight</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [<span data-ttu-id="ed935-143">Uživatelem definované funkce jazyka C# pomocí Hive a Pig</span><span class="sxs-lookup"><span data-stu-id="ed935-143">Use C# user defined functions with Hive and Pig</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [<span data-ttu-id="ed935-144">Vývoj C# topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ed935-144">Develop C# topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)