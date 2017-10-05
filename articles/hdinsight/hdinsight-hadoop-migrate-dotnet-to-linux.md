---
title: "MapReduce s Hadoop v HDInsight se systémem Linux - Azure pomocí rozhraní .NET | Microsoft Docs"
description: "Další informace o použití aplikace .NET pro streamování MapReduce na HDInsight se systémem Linux."
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
ms.openlocfilehash: 6ad188fb752474ff5c7d8a3fb9d609eefe8c7a9a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-to-linux-based-hdinsight"></a><span data-ttu-id="7c0bc-103">Migrace řešení .NET pro HDInsight se systémem Windows do HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="7c0bc-103">Migrate .NET solutions for Windows-based HDInsight to Linux-based HDInsight</span></span>

<span data-ttu-id="7c0bc-104">Clustery HDInsight se systémem Linux použijte [Mono (https://mono-project.com)](https://mono-project.com) ke spouštění aplikací .NET.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-104">Linux-based HDInsight clusters use [Mono (https://mono-project.com)](https://mono-project.com) to run .NET applications.</span></span> <span data-ttu-id="7c0bc-105">Mono umožňuje použití rozhraní .NET komponent, jako jsou například aplikace MapReduce s HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-105">Mono allows you to use .NET components such as MapReduce applications with Linux-based HDInsight.</span></span> <span data-ttu-id="7c0bc-106">V tomto dokumentu zjistěte, jak migrovat .NET řešení vytvořená pro clustery HDInsight se systémem Windows pro práci s Mono na HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-106">In this document, learn how to migrate .NET solutions created for Windows-based HDInsight clusters to work with Mono on Linux-based HDInsight.</span></span>

## <a name="mono-compatibility-with-net"></a><span data-ttu-id="7c0bc-107">Monofonní kompatibilitu s rozhraním .NET</span><span class="sxs-lookup"><span data-stu-id="7c0bc-107">Mono compatibility with .NET</span></span>

<span data-ttu-id="7c0bc-108">Monofonní verze 4.2.1 je součástí HDInsight verze 3.5.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-108">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span> <span data-ttu-id="7c0bc-109">Další informace o verzi Mono zahrnuté do HDInsight naleznete v tématu [HDInsight verze součástí](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="7c0bc-109">For more information on the version of Mono included with HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="7c0bc-110">K instalaci na konkrétní verzi Mono naleznete [instalovat nebo aktualizovat Mono](hdinsight-hadoop-install-mono.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-110">To install a specific version of Mono, see the [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

<span data-ttu-id="7c0bc-111">Podrobné informace o kompatibilitě mezi Mono a rozhraní .NET najdete v tématu [Mono kompatibility (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-111">For detailed information on compatibility between Mono and .NET, see the [Mono compatibility (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7c0bc-112">Rozhraní framework SCP.NET je kompatibilní s Mono.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-112">The SCP.NET framework is compatible with Mono.</span></span> <span data-ttu-id="7c0bc-113">Další informace o používání SCP.NET s Mono najdete v tématu [pomocí Visual Studio pro vývoj topologií C# pro Apache Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="7c0bc-113">For more information on using SCP.NET with Mono, see [Use Visual Studio to develop C# topologies for Apache Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

## <a name="automated-portability-analysis"></a><span data-ttu-id="7c0bc-114">Analýza automatizované přenositelnost</span><span class="sxs-lookup"><span data-stu-id="7c0bc-114">Automated portability analysis</span></span>

<span data-ttu-id="7c0bc-115">[.NET přenositelnost analyzátor](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) lze použít k vygenerování sestavy problémům s kompatibilitou mezi aplikací a Mono.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-115">The [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) can be used to generate a report of incompatibilities between your application and Mono.</span></span> <span data-ttu-id="7c0bc-116">Pomocí následujících kroků nakonfigurujte analyzátor zkontrolujte vaši aplikaci pro Mono přenositelnost:</span><span class="sxs-lookup"><span data-stu-id="7c0bc-116">Use the following steps to configure the analyzer to check your application for Mono portability:</span></span>

1. <span data-ttu-id="7c0bc-117">Nainstalujte [.NET přenositelnost analyzátor](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer).</span><span class="sxs-lookup"><span data-stu-id="7c0bc-117">Install the [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer).</span></span> <span data-ttu-id="7c0bc-118">Během instalace vyberte verzi sady Visual Studio používat.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-118">During installation, select the version of Visual Studio to use.</span></span>

2. <span data-ttu-id="7c0bc-119">Ze sady Visual Studio 2015, vyberte __analyzovat__ > __přenositelnost Analyzátor nastavení__a ujistěte se, že __4.5__ se změnami __Mono__ části.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-119">From Visual Studio 2015, select __Analyze__ > __Portability Analyzer Settings__, and make sure that __4.5__ is checked in the __Mono__ section.</span></span>

    ![4.5 změnami Mono části pro Analyzátor nastavení](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    <span data-ttu-id="7c0bc-121">Vyberte __OK__ konfiguraci uložíte.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-121">Select __OK__ to save the configuration.</span></span>

3. <span data-ttu-id="7c0bc-122">Vyberte __analyzovat__ > __analyzovat sestavení přenositelnost__.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-122">Select __Analyze__ > __Analyze Assembly Portability__.</span></span> <span data-ttu-id="7c0bc-123">Vyberte sestavení, které obsahuje řešení a pak vyberte __otevřete__ zahájíte analýzu.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-123">Select the assembly that contains your solution, and then select __Open__ to begin analysis.</span></span>

4. <span data-ttu-id="7c0bc-124">Po dokončení analýzy, vybrat __analyzovat__ > __zobrazit sestavy analýzy__.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-124">Once analysis is complete, select __Analyze__ > __View analysis reports__.</span></span> <span data-ttu-id="7c0bc-125">V __výsledky analýzy přenositelnost__, vyberte __otevřete sestavu__ otevřete sestavu.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-125">In __Portability Analysis Results__, select __Open report__ to open a report.</span></span>

    ![Dialogové okno výsledků analyzátoru přenositelnost](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]
> <span data-ttu-id="7c0bc-127">Analyzátoru nelze catch každých problém s vaším řešením.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-127">The analyzer cannot catch every problem with your solution.</span></span> <span data-ttu-id="7c0bc-128">Například soubor cestu `c:\temp\file.txt` se považuje za normální, protože Mono běží na systému Windows a je cesta platná v daném kontextu.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-128">For example, a file path of `c:\temp\file.txt` is considered OK because Mono runs on Windows and the path is valid in that context.</span></span> <span data-ttu-id="7c0bc-129">Cesta však není platný na platformě Linux.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-129">However, the path is not valid on a Linux platform.</span></span>

## <a name="manual-portability-analysis"></a><span data-ttu-id="7c0bc-130">Analýza ruční přenositelnost</span><span class="sxs-lookup"><span data-stu-id="7c0bc-130">Manual portability analysis</span></span>

<span data-ttu-id="7c0bc-131">Proveďte ruční auditu podle informací uvedených v kódu [přenositelnost aplikace (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-131">Perform a manual audit of your code using the information in the [Application Portability (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) document.</span></span>

## <a name="modify-and-build"></a><span data-ttu-id="7c0bc-132">Upravit a sestavení</span><span class="sxs-lookup"><span data-stu-id="7c0bc-132">Modify and build</span></span>

<span data-ttu-id="7c0bc-133">Můžete pokračovat pomocí sady Visual Studio pro vývoj řešení .NET pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-133">You can continue to use Visual Studio to build your .NET solutions for HDInsight.</span></span> <span data-ttu-id="7c0bc-134">Však musí zajistit, aby se projekt konfigurovány k použití rozhraní .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-134">However, you must ensure that the project is configured to use .NET Framework 4.5.</span></span>

## <a name="deploy-and-test"></a><span data-ttu-id="7c0bc-135">Nasazení a testování</span><span class="sxs-lookup"><span data-stu-id="7c0bc-135">Deploy and test</span></span>

<span data-ttu-id="7c0bc-136">Jakmile změníte řešení pomocí doporučení z analyzátor přenositelnost .NET nebo ruční analýzy, je nutné jej otestovat s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-136">Once you have modified your solution using the recommendations from the .NET Portability Analyzer or from a manual analysis, you must test it with HDInsight.</span></span> <span data-ttu-id="7c0bc-137">Testování řešení na clusteru HDInsight se systémem Linux může odhalit jemně problémy, které je třeba opravit.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-137">Testing the solution on a Linux-based HDInsight cluster may reveal subtle problems that need to be corrected.</span></span> <span data-ttu-id="7c0bc-138">Doporučujeme, abyste povolili další protokolování v aplikaci během testování.</span><span class="sxs-lookup"><span data-stu-id="7c0bc-138">We recommend that you enable additional logging in your application while testing it.</span></span>

<span data-ttu-id="7c0bc-139">Další informace o přístupu k protokolů najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="7c0bc-139">For more information on accessing logs, see the following documents:</span></span>

* [<span data-ttu-id="7c0bc-140">Přístup k protokolům aplikací YARN ve službě HDInsight s Linuxem</span><span class="sxs-lookup"><span data-stu-id="7c0bc-140">Access YARN application logs on Linux-based HDInsight</span></span>](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a><span data-ttu-id="7c0bc-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7c0bc-141">Next steps</span></span>

* [<span data-ttu-id="7c0bc-142">Použití jazyka C# s MapReduce v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7c0bc-142">Use C# with MapReduce on HDInsight</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [<span data-ttu-id="7c0bc-143">Uživatelem definované funkce jazyka C# pomocí Hive a Pig</span><span class="sxs-lookup"><span data-stu-id="7c0bc-143">Use C# user defined functions with Hive and Pig</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [<span data-ttu-id="7c0bc-144">Vývoj C# topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7c0bc-144">Develop C# topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)