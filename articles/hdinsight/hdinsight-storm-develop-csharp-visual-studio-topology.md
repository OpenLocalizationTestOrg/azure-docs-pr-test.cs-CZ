---
title: "topologie Storm aaaApache sadou Visual Studio a C# – Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate topologie Storm v jazyce C#. Vytvořte topologii počtu jednoduché aplikace word v sadě Visual Studio pomocí nástroje hello Hadoop pro sadu Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 380d804f-a8c5-4b20-9762-593ec4da5a0d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: larryfr
ms.openlocfilehash: b3fb01a4dda144fd7fb4141e624e31e667f93753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-c-topologies-for-apache-storm-by-using-hello-data-lake-tools-for-visual-studio"></a><span data-ttu-id="97555-104">Vývoj topologie C# pro Apache Storm pomocí nástrojů hello Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="97555-104">Develop C# topologies for Apache Storm by using hello Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="97555-105">Zjistěte, jak toocreate topologie C# Storm pomocí hello Azure Data Lake (Hadoop) nástroje pro sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="97555-105">Learn how toocreate a C# Storm topology by using hello Azure Data Lake (Hadoop) tools for Visual Studio.</span></span> <span data-ttu-id="97555-106">Tento dokument vás provede procesem vytvoření projektu Storm v sadě Visual Studio, místní testování a nasazení zásady tooan Apache Storm v clusteru Azure HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="97555-106">This document walks through hello process of creating a Storm project in Visual Studio, testing it locally, and deploying it tooan Apache Storm on Azure HDInsight cluster.</span></span>

<span data-ttu-id="97555-107">Také zjistíte, jak toocreate hybridní topologie, které využívají součásti C# nebo Java.</span><span class="sxs-lookup"><span data-stu-id="97555-107">You also learn how toocreate hybrid topologies that use C# and Java components.</span></span>

> [!NOTE]
> <span data-ttu-id="97555-108">Při hello kroky v tomto dokumentu se spoléhají na vývojového prostředí systému Windows pomocí sady Visual Studio, může být zkompilovaný projekt hello odeslaná tooeither cluster Linux nebo HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="97555-108">While hello steps in this document rely on a Windows development environment with Visual Studio, hello compiled project can be submitted tooeither a Linux or Windows-based HDInsight cluster.</span></span> <span data-ttu-id="97555-109">Clustery se systémem Linux vytvořené po 28 říjnu 2016 se podporují jenom SCP.NET topologie.</span><span class="sxs-lookup"><span data-stu-id="97555-109">Only Linux-based clusters created after October 28, 2016, support SCP.NET topologies.</span></span>

<span data-ttu-id="97555-110">topologie toouse C# s clusterem se systémem Linux, je nutné aktualizovat hello balíček Microsoft.SCP.Net.SDK NuGet používá ve vašem projektu tooversion 0.10.0.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="97555-110">toouse a C# topology with a Linux-based cluster, you must update hello Microsoft.SCP.Net.SDK NuGet package used by your project tooversion 0.10.0.6 or later.</span></span> <span data-ttu-id="97555-111">Hello verzi balíčku hello se taky musí shodovat hello hlavní verzi Storm v HDInsight nainstalována.</span><span class="sxs-lookup"><span data-stu-id="97555-111">hello version of hello package must also match hello major version of Storm installed on HDInsight.</span></span>

| <span data-ttu-id="97555-112">HDInsight verze</span><span class="sxs-lookup"><span data-stu-id="97555-112">HDInsight version</span></span> | <span data-ttu-id="97555-113">Storm verze</span><span class="sxs-lookup"><span data-stu-id="97555-113">Storm version</span></span> | <span data-ttu-id="97555-114">SCP.NET verze</span><span class="sxs-lookup"><span data-stu-id="97555-114">SCP.NET version</span></span> | <span data-ttu-id="97555-115">Výchozí Mono verze</span><span class="sxs-lookup"><span data-stu-id="97555-115">Default Mono version</span></span> |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| <span data-ttu-id="97555-116">3.3</span><span class="sxs-lookup"><span data-stu-id="97555-116">3.3</span></span> |<span data-ttu-id="97555-117">0.10.x</span><span class="sxs-lookup"><span data-stu-id="97555-117">0.10.x</span></span> |<span data-ttu-id="97555-118">0.10.x.x</span><span class="sxs-lookup"><span data-stu-id="97555-118">0.10.x.x</span></span></br><span data-ttu-id="97555-119">(jenom na HDInsight se systémem Windows)</span><span class="sxs-lookup"><span data-stu-id="97555-119">(only on Windows-based HDInsight)</span></span> | <span data-ttu-id="97555-120">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="97555-120">NA</span></span> |
| <span data-ttu-id="97555-121">3.4</span><span class="sxs-lookup"><span data-stu-id="97555-121">3.4</span></span> | <span data-ttu-id="97555-122">0.10.0.x</span><span class="sxs-lookup"><span data-stu-id="97555-122">0.10.0.x</span></span> | <span data-ttu-id="97555-123">0.10.0.x</span><span class="sxs-lookup"><span data-stu-id="97555-123">0.10.0.x</span></span> | <span data-ttu-id="97555-124">3.2.8</span><span class="sxs-lookup"><span data-stu-id="97555-124">3.2.8</span></span> |
| <span data-ttu-id="97555-125">3,5</span><span class="sxs-lookup"><span data-stu-id="97555-125">3.5</span></span> | <span data-ttu-id="97555-126">1.0.2.x</span><span class="sxs-lookup"><span data-stu-id="97555-126">1.0.2.x</span></span> | <span data-ttu-id="97555-127">1.0.0.x</span><span class="sxs-lookup"><span data-stu-id="97555-127">1.0.0.x</span></span> | <span data-ttu-id="97555-128">4.2.1</span><span class="sxs-lookup"><span data-stu-id="97555-128">4.2.1</span></span> |
| <span data-ttu-id="97555-129">3.6</span><span class="sxs-lookup"><span data-stu-id="97555-129">3.6</span></span> | <span data-ttu-id="97555-130">1.1.0.x</span><span class="sxs-lookup"><span data-stu-id="97555-130">1.1.0.x</span></span> | <span data-ttu-id="97555-131">1.0.0.x</span><span class="sxs-lookup"><span data-stu-id="97555-131">1.0.0.x</span></span> | <span data-ttu-id="97555-132">4.2.8</span><span class="sxs-lookup"><span data-stu-id="97555-132">4.2.8</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="97555-133">C# topologií v clusterech se systémem Linux musí používat rozhraní .NET 4.5 a používat Mono toorun na clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="97555-133">C# topologies on Linux-based clusters must use .NET 4.5, and use Mono toorun on hello HDInsight cluster.</span></span> <span data-ttu-id="97555-134">Zkontrolujte [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) pro potenciální nekompatibility.</span><span class="sxs-lookup"><span data-stu-id="97555-134">Check [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) for potential incompatibilities.</span></span>

## <a name="install-visual-studio"></a><span data-ttu-id="97555-135">Instalace sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="97555-135">Install Visual Studio</span></span>

<span data-ttu-id="97555-136">Topologie C# s SCP.NET můžete vyvíjet pomocí jedné z následujících verzí sady Visual Studio hello:</span><span class="sxs-lookup"><span data-stu-id="97555-136">You can develop C# topologies with SCP.NET by using one of hello following versions of Visual Studio:</span></span>

* <span data-ttu-id="97555-137">Visual Studio 2012 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="97555-137">Visual Studio 2012 with [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>

* <span data-ttu-id="97555-138">Visual Studio 2013 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=44921) nebo [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span><span class="sxs-lookup"><span data-stu-id="97555-138">Visual Studio 2013 with [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span></span>

* <span data-ttu-id="97555-139">Visual Studio 2015 nebo [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)</span><span class="sxs-lookup"><span data-stu-id="97555-139">Visual Studio 2015 or [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)</span></span>

* <span data-ttu-id="97555-140">Visual Studio 2017 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="97555-140">Visual Studio 2017 (any edition)</span></span>

## <a name="install-data-lake-tools-for-visual-studio"></a><span data-ttu-id="97555-141">Nástroje pro instalaci Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="97555-141">Install Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="97555-142">tooinstall nástroje Data Lake pro Visual Studio, postupujte podle kroků hello v [Začínáme pomocí nástrojů Data Lake pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="97555-142">tooinstall Data Lake tools for Visual Studio, follow hello steps in [Get started using Data Lake tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

## <a name="install-java"></a><span data-ttu-id="97555-143">Instalace Javy</span><span class="sxs-lookup"><span data-stu-id="97555-143">Install Java</span></span>

<span data-ttu-id="97555-144">Při odesílání topologie Storm ze sady Visual Studio SCP.NET generuje soubor zip, který obsahuje hello topologie a závislosti.</span><span class="sxs-lookup"><span data-stu-id="97555-144">When you submit a Storm topology from Visual Studio, SCP.NET generates a zip file that contains hello topology and dependencies.</span></span> <span data-ttu-id="97555-145">Java je použité toocreate tyto zip souborů, protože používá formátu, který je více kompatibilní s clustery se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="97555-145">Java is used toocreate these zip files, because it uses a format that is more compatible with Linux-based clusters.</span></span>

1. <span data-ttu-id="97555-146">Nainstalujte hello Java Developer Kit (JDK) 7 nebo novější na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="97555-146">Install hello Java Developer Kit (JDK) 7 or later on your development environment.</span></span> <span data-ttu-id="97555-147">Můžete získat hello Oracle JDK z [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="97555-147">You can get hello Oracle JDK from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span> <span data-ttu-id="97555-148">Můžete také použít [jiných distribuce Java](http://openjdk.java.net/).</span><span class="sxs-lookup"><span data-stu-id="97555-148">You can also use [other Java distributions](http://openjdk.java.net/).</span></span>

2. <span data-ttu-id="97555-149">Hello `JAVA_HOME` prostředí proměnné musí bodu toohello adresář, který obsahuje Java.</span><span class="sxs-lookup"><span data-stu-id="97555-149">hello `JAVA_HOME` environment variable must point toohello directory that contains Java.</span></span>

3. <span data-ttu-id="97555-150">Hello `PATH` proměnnou prostředí musí obsahovat hello `%JAVA_HOME%\bin` adresáře.</span><span class="sxs-lookup"><span data-stu-id="97555-150">hello `PATH` environment variable must include hello `%JAVA_HOME%\bin` directory.</span></span>

<span data-ttu-id="97555-151">Můžete použít následující C# konzole aplikace tooverify správně nainstalována Java a hello JDK hello:</span><span class="sxs-lookup"><span data-stu-id="97555-151">You can use hello following C# console application tooverify that Java and hello JDK are correctly installed:</span></span>

```csharp
using System;
using System.IO;
namespace ConsoleApplication2
{
   class Program
   {
       static void Main(string[] args)
       {
           string javaHome = Environment.GetEnvironmentVariable(“JAVA_HOME”);
           if (!string.IsNullOrEmpty(javaHome))
           {
               string jarExe = Path.Combine(javaHome + @”\bin”, “jar.exe”);
               if (File.Exists(jarExe))
               {
                   Console.WriteLine(“JAVA Is Installed properly”);
                    return;
               }
               else
               {
                   Console.WriteLine(“A valid JAVA JDK is not found. Looks like JRE is installed instead of JDK.”);
               }
           }
           else
           {
             Console.WriteLine(“A valid JAVA JDK is not found. JAVA_HOME environment variable is not set.”);
           }
       }  
   }
}
```

## <a name="storm-templates"></a><span data-ttu-id="97555-152">Šablony Storm</span><span class="sxs-lookup"><span data-stu-id="97555-152">Storm templates</span></span>

<span data-ttu-id="97555-153">Hello nástroje Data Lake pro Visual Studio poskytují hello následující šablony:</span><span class="sxs-lookup"><span data-stu-id="97555-153">hello Data Lake tools for Visual Studio provide hello following templates:</span></span>

| <span data-ttu-id="97555-154">Typ projektu</span><span class="sxs-lookup"><span data-stu-id="97555-154">Project type</span></span> | <span data-ttu-id="97555-155">Demonstruje</span><span class="sxs-lookup"><span data-stu-id="97555-155">Demonstrates</span></span> |
| --- | --- |
| <span data-ttu-id="97555-156">Aplikace Storm</span><span class="sxs-lookup"><span data-stu-id="97555-156">Storm Application</span></span> |<span data-ttu-id="97555-157">Prázdný projekt topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="97555-157">An empty Storm topology project.</span></span> |
| <span data-ttu-id="97555-158">Ukázka zapisovače Azure SQL Storm</span><span class="sxs-lookup"><span data-stu-id="97555-158">Storm Azure SQL Writer Sample</span></span> |<span data-ttu-id="97555-159">Jak toowrite tooAzure databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="97555-159">How toowrite tooAzure SQL Database.</span></span> |
| <span data-ttu-id="97555-160">Ukázka čtečky Azure Cosmos DB Storm</span><span class="sxs-lookup"><span data-stu-id="97555-160">Storm Azure Cosmos DB Reader Sample</span></span> |<span data-ttu-id="97555-161">Jak tooread z Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="97555-161">How tooread from Azure Cosmos DB.</span></span> |
| <span data-ttu-id="97555-162">Ukázka zapisovače Azure Cosmos DB Storm</span><span class="sxs-lookup"><span data-stu-id="97555-162">Storm Azure Cosmos DB Writer Sample</span></span> |<span data-ttu-id="97555-163">Jak toowrite tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="97555-163">How toowrite tooAzure Cosmos DB.</span></span> |
| <span data-ttu-id="97555-164">Ukázka EventHub čtečky Storm</span><span class="sxs-lookup"><span data-stu-id="97555-164">Storm EventHub Reader Sample</span></span> |<span data-ttu-id="97555-165">Jak tooread z Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="97555-165">How tooread from Azure Event Hubs.</span></span> |
| <span data-ttu-id="97555-166">Ukázka EventHub zapisovače Storm</span><span class="sxs-lookup"><span data-stu-id="97555-166">Storm EventHub Writer Sample</span></span> |<span data-ttu-id="97555-167">Jak toowrite tooAzure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="97555-167">How toowrite tooAzure Event Hubs.</span></span> |
| <span data-ttu-id="97555-168">Ukázka HBase čtečky Storm</span><span class="sxs-lookup"><span data-stu-id="97555-168">Storm HBase Reader Sample</span></span> |<span data-ttu-id="97555-169">Jak tooread z HBase v HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="97555-169">How tooread from HBase on HDInsight clusters.</span></span> |
| <span data-ttu-id="97555-170">Ukázka HBase zapisovače Storm</span><span class="sxs-lookup"><span data-stu-id="97555-170">Storm HBase Writer Sample</span></span> |<span data-ttu-id="97555-171">Jak toowrite tooHBase v HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="97555-171">How toowrite tooHBase on HDInsight clusters.</span></span> |
| <span data-ttu-id="97555-172">Ukázka hybridní Storm</span><span class="sxs-lookup"><span data-stu-id="97555-172">Storm Hybrid Sample</span></span> |<span data-ttu-id="97555-173">Jak toouse součásti Java.</span><span class="sxs-lookup"><span data-stu-id="97555-173">How toouse a Java component.</span></span> |
| <span data-ttu-id="97555-174">Ukázka Storm</span><span class="sxs-lookup"><span data-stu-id="97555-174">Storm Sample</span></span> |<span data-ttu-id="97555-175">Počet topologii základní aplikace word.</span><span class="sxs-lookup"><span data-stu-id="97555-175">A basic word count topology.</span></span> |

> [!WARNING]
> <span data-ttu-id="97555-176">Ne všechny šablony, bude fungovat s HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="97555-176">Not all templates will work with Linux-based HDInsight.</span></span> <span data-ttu-id="97555-177">Balíčky Nuget použité šablony hello nemusí být kompatibilní s Mono.</span><span class="sxs-lookup"><span data-stu-id="97555-177">Nuget packages used by hello templates may not be compatible with Mono.</span></span> <span data-ttu-id="97555-178">Zkontrolujte hello [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu a použít hello [.NET přenositelnost analyzátor](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) tooidentify potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="97555-178">Check hello [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document and use hello [.NET Portability Analyzer](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) tooidentify potential problems.</span></span>

<span data-ttu-id="97555-179">V hello kroky v tomto dokumentu můžete použít hello základní aplikace Storm projektu typu toocreate topologii.</span><span class="sxs-lookup"><span data-stu-id="97555-179">In hello steps in this document, you use hello basic Storm Application project type toocreate a topology.</span></span>

### <a name="hbase-templates-notes"></a><span data-ttu-id="97555-180">Poznámky k šablony HBase</span><span class="sxs-lookup"><span data-stu-id="97555-180">HBase templates notes</span></span>

<span data-ttu-id="97555-181">Hello čtečky HBase a zapisovače šablony pomocí hello HBase REST API, není hello HBase Java API, toocommunicate s HBase v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="97555-181">hello HBase reader and writer templates use hello HBase REST API, not hello HBase Java API, toocommunicate with an HBase on HDInsight cluster.</span></span>

### <a name="eventhub-templates-notes"></a><span data-ttu-id="97555-182">Poznámky k EventHub šablony</span><span class="sxs-lookup"><span data-stu-id="97555-182">EventHub templates notes</span></span>

> [!IMPORTANT]
> <span data-ttu-id="97555-183">Hello založené na jazyce Java EventHub spout součásti, které jsou součástí hello EventHub čtečky šablony nemusí pracovat se Storm v HDInsight verze 3.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="97555-183">hello Java-based EventHub spout component included with hello EventHub Reader template may not work with Storm on HDInsight version 3.5 or later.</span></span> <span data-ttu-id="97555-184">Aktualizovanou verzi této součásti je k dispozici na [Githubu](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).</span><span class="sxs-lookup"><span data-stu-id="97555-184">An updated version of this component is available at [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).</span></span>

<span data-ttu-id="97555-185">Ukázkové topologie, který používá tato součást a spolupracuje s Storm v HDInsight 3.5, najdete v části [Githubu](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="97555-185">For an example topology that uses this component and works with Storm on HDInsight 3.5, see [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span>

## <a name="create-a-c-topology"></a><span data-ttu-id="97555-186">Vytvoření topologie C#</span><span class="sxs-lookup"><span data-stu-id="97555-186">Create a C# topology</span></span>

1. <span data-ttu-id="97555-187">Otevřete Visual Studio, vyberte **soubor** > **nový**a potom vyberte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="97555-187">Open Visual Studio, select **File** > **New**, and then select **Project**.</span></span>

2. <span data-ttu-id="97555-188">Z hello **nový projekt** okně rozbalte **nainstalovaná** > **šablony**a vyberte **Azure Data Lake**.</span><span class="sxs-lookup"><span data-stu-id="97555-188">From hello **New Project** window, expand **Installed** > **Templates**, and select **Azure Data Lake**.</span></span> <span data-ttu-id="97555-189">Hello seznam šablon, vyberte **aplikace Storm**.</span><span class="sxs-lookup"><span data-stu-id="97555-189">From hello list of templates, select **Storm Application**.</span></span> <span data-ttu-id="97555-190">V hello dolní části obrazovky hello, zadejte **WordCount** jako název hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="97555-190">At hello bottom of hello screen, enter **WordCount** as hello name of hello application.</span></span>

    ![Snímek obrazovky nový projekt – okno](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

3. <span data-ttu-id="97555-192">Po vytvoření projektu hello byste měli mít hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="97555-192">After you have created hello project, you should have hello following files:</span></span>

   * <span data-ttu-id="97555-193">**Program.cs**: Tento soubor definuje hello topologie pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="97555-193">**Program.cs**: This file defines hello topology for your project.</span></span> <span data-ttu-id="97555-194">Ve výchozím nastavení se vytvoří výchozí topologie, která se skládá z jedné spout a jeden bolt.</span><span class="sxs-lookup"><span data-stu-id="97555-194">A default topology that consists of one spout and one bolt is created by default.</span></span>

   * <span data-ttu-id="97555-195">**Spout.cs**: Příklad funkcí spout, který vysílá náhodných čísel.</span><span class="sxs-lookup"><span data-stu-id="97555-195">**Spout.cs**: An example spout that emits random numbers.</span></span>

   * <span data-ttu-id="97555-196">**Bolt.cs**: Příklad funkcí bolt, který uchovává počet čísla vysílaných hello spout.</span><span class="sxs-lookup"><span data-stu-id="97555-196">**Bolt.cs**: An example bolt that keeps a count of numbers emitted by hello spout.</span></span>

     <span data-ttu-id="97555-197">Při vytváření projektu hello NuGet stahování hello nejnovější [SCP.NET balíček](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).</span><span class="sxs-lookup"><span data-stu-id="97555-197">When you create hello project, NuGet downloads hello latest [SCP.NET package](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).</span></span>

     [!INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-hello-spout"></a><span data-ttu-id="97555-198">Implementace hello spout</span><span class="sxs-lookup"><span data-stu-id="97555-198">Implement hello spout</span></span>

1. <span data-ttu-id="97555-199">Otevřete **Spout.cs**.</span><span class="sxs-lookup"><span data-stu-id="97555-199">Open **Spout.cs**.</span></span> <span data-ttu-id="97555-200">Funkcích spouts jsou použité tooread data v topologii z externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="97555-200">Spouts are used tooread data in a topology from an external source.</span></span> <span data-ttu-id="97555-201">Hello hlavní komponenty pro funkcí spout jsou:</span><span class="sxs-lookup"><span data-stu-id="97555-201">hello main components for a spout are:</span></span>

   * <span data-ttu-id="97555-202">**NextTuple**: volány Storm, pokud hello spout je povoleno tooemit nové řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="97555-202">**NextTuple**: Called by Storm when hello spout is allowed tooemit new tuples.</span></span>

   * <span data-ttu-id="97555-203">**Potvrzení** (pouze pro transakční topologie): zpracovává potvrzení iniciovaná jiné komponenty v hello topologie pro odeslaný hello spout řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="97555-203">**Ack** (transactional topology only): Handles acknowledgements initiated by other components in hello topology for tuples sent from hello spout.</span></span> <span data-ttu-id="97555-204">To v úvahu řazené kolekce členů umožňuje hello spout vědět, že byl úspěšně zpracován podřízené součásti.</span><span class="sxs-lookup"><span data-stu-id="97555-204">Acknowledging a tuple lets hello spout know that it was processed successfully by downstream components.</span></span>

   * <span data-ttu-id="97555-205">**Selhání** (pouze pro transakční topologie): zpracovává řazené kolekce členů, které jsou selhání zpracování ostatní součásti v topologii hello.</span><span class="sxs-lookup"><span data-stu-id="97555-205">**Fail** (transactional topology only): Handles tuples that are fail-processing other components in hello topology.</span></span> <span data-ttu-id="97555-206">Implementace selhání metoda vám umožní toore-emitování hello řazené kolekce členů, takže může být znovu zpracována.</span><span class="sxs-lookup"><span data-stu-id="97555-206">Implementing a Fail method allows you toore-emit hello tuple so that it can be processed again.</span></span>

2. <span data-ttu-id="97555-207">Nahraďte obsah hello hello **Spout** se hello následující text.</span><span class="sxs-lookup"><span data-stu-id="97555-207">Replace hello contents of hello **Spout** class with hello following text.</span></span> <span data-ttu-id="97555-208">Tato spout náhodně vysílá věty do topologie hello.</span><span class="sxs-lookup"><span data-stu-id="97555-208">This spout randomly emits a sentence into hello topology.</span></span>

    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "hello cow jumped over hello moon",
        "an apple a day keeps hello doctor away",
        "four score and seven years ago",
        "snow white and hello seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set hello instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // hello schema for hello default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of hello spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // hello sentence toobe emitted
        string sentence;

        // Get a random sentence
        sentence = sentences[r.Next(0, sentences.Length - 1)];
        Context.Logger.Info("Emit: {0}", sentence);
        // Emit it
        this.ctx.Emit(new Values(sentence));

        Context.Logger.Info("NextTuple exit");
    }

    public void Ack(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }
    ```

### <a name="implement-hello-bolts"></a><span data-ttu-id="97555-209">Implementace funkce bolts hello</span><span class="sxs-lookup"><span data-stu-id="97555-209">Implement hello bolts</span></span>

1. <span data-ttu-id="97555-210">Odstraňte existující hello **Bolt.cs** souboru z projektu hello.</span><span class="sxs-lookup"><span data-stu-id="97555-210">Delete hello existing **Bolt.cs** file from hello project.</span></span>

2. <span data-ttu-id="97555-211">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="97555-211">In **Solution Explorer**, right-click hello project, and select **Add** > **New item**.</span></span> <span data-ttu-id="97555-212">Hello seznamu, vyberte **Storm Bolt**a zadejte **Splitter.cs** jako název hello.</span><span class="sxs-lookup"><span data-stu-id="97555-212">From hello list, select **Storm Bolt**, and enter **Splitter.cs** as hello name.</span></span> <span data-ttu-id="97555-213">Opakujte tento proces toocreate s názvem druhý bolt **Counter.cs**.</span><span class="sxs-lookup"><span data-stu-id="97555-213">Repeat this process toocreate a second bolt named **Counter.cs**.</span></span>

   * <span data-ttu-id="97555-214">**Splitter.cs**: implementuje funkce bolt, které rozdělí věty do jednotlivých slov a vydá nové proud slova.</span><span class="sxs-lookup"><span data-stu-id="97555-214">**Splitter.cs**: Implements a bolt that splits sentences into individual words, and emits a new stream of words.</span></span>

   * <span data-ttu-id="97555-215">**Counter.cs**: implementuje funkce bolt, které počty jednotlivých slov a vysílá nového datového proudu slova a hello počtu pro každou aplikaci word.</span><span class="sxs-lookup"><span data-stu-id="97555-215">**Counter.cs**: Implements a bolt that counts each word, and emits a new stream of words and hello count for each word.</span></span>

     > [!NOTE]
     > <span data-ttu-id="97555-216">Tyto funkce bolts číst a zapisovat toostreams, ale můžete použít také bolt toocommunicate s zdroje jako databázi nebo službě.</span><span class="sxs-lookup"><span data-stu-id="97555-216">These bolts read and write toostreams, but you can also use a bolt toocommunicate with sources such as a database or service.</span></span>

3. <span data-ttu-id="97555-217">Otevřete **Splitter.cs**.</span><span class="sxs-lookup"><span data-stu-id="97555-217">Open **Splitter.cs**.</span></span> <span data-ttu-id="97555-218">Ve výchozím nastavení obsahuje pouze jednu metodu: **Execute**.</span><span class="sxs-lookup"><span data-stu-id="97555-218">It has only one method by default: **Execute**.</span></span> <span data-ttu-id="97555-219">Hello Execute metoda je volána, když hello bolt obdrží řazené kolekce členů pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="97555-219">hello Execute method is called when hello bolt receives a tuple for processing.</span></span> <span data-ttu-id="97555-220">Zde si můžete přečíst zpracovat příchozí řazených kolekcí členů a vydávání odchozích řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="97555-220">Here, you can read and process incoming tuples, and emit outbound tuples.</span></span>

4. <span data-ttu-id="97555-221">Nahraďte obsah hello hello **rozdělovače** se hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="97555-221">Replace hello contents of hello **Splitter** class with hello following code:</span></span>

    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (hello sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (hello word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of hello bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello sentence from hello tuple
        string sentence = tuple.GetString(0);
        // Split at space characters
        foreach (string word in sentence.Split(' '))
        {
            Context.Logger.Info("Emit: {0}", word);
            //Emit each word
            this.ctx.Emit(new Values(word));
        }

        Context.Logger.Info("Execute exit");
    }
    ```

5. <span data-ttu-id="97555-222">Otevřete **Counter.cs**a nahraďte obsah třída hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="97555-222">Open **Counter.cs**, and replace hello class contents with hello following:</span></span>

    ```csharp
    private Context ctx;

    // Dictionary for holding words and counts
    private Dictionary<string, int> counts = new Dictionary<string, int>();

    // Constructor
    public Counter(Context ctx)
    {
        Context.Logger.Info("Counter constructor called");
        // Set instance context
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string field - hello word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - hello word and hello word count
        outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance
    public static Counter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Counter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello word from hello tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for hello word in hello dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment hello count
        count++;
        // Update hello count in hello dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit hello word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```

### <a name="define-hello-topology"></a><span data-ttu-id="97555-223">Definovat topologii hello</span><span class="sxs-lookup"><span data-stu-id="97555-223">Define hello topology</span></span>

<span data-ttu-id="97555-224">Funkcích spouts a funkce bolts jsou uspořádány v grafu, který definuje, jak hello data proudí mezi součástmi.</span><span class="sxs-lookup"><span data-stu-id="97555-224">Spouts and bolts are arranged in a graph, which defines how hello data flows between components.</span></span> <span data-ttu-id="97555-225">Pro tuto topologii hello grafu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="97555-225">For this topology, hello graph is as follows:</span></span>

![Diagram jak jsou uspořádány součásti](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

<span data-ttu-id="97555-227">Věty jsou nevydává hello spout a jsou distribuované tooinstances bolt rozdělovače hello.</span><span class="sxs-lookup"><span data-stu-id="97555-227">Sentences are emitted from hello spout, and are distributed tooinstances of hello Splitter bolt.</span></span> <span data-ttu-id="97555-228">bolt rozdělovače Hello dělí do slova, které jsou distribuované toohello čítač bolt hello věty.</span><span class="sxs-lookup"><span data-stu-id="97555-228">hello Splitter bolt breaks hello sentences into words, which are distributed toohello Counter bolt.</span></span>

<span data-ttu-id="97555-229">Vzhledem k tomu, že počet slov v instanci čítače hello trvá místně, chceme toomake jistotu, že určitá slova toku toohello stejnou instanci bolt čítače.</span><span class="sxs-lookup"><span data-stu-id="97555-229">Because word count is held locally in hello Counter instance, we want toomake sure that specific words flow toohello same Counter bolt instance.</span></span> <span data-ttu-id="97555-230">Každá instance uchovává informace o konkrétní slova.</span><span class="sxs-lookup"><span data-stu-id="97555-230">Each instance keeps track of specific words.</span></span> <span data-ttu-id="97555-231">Vzhledem k tomu, že hello rozdělovače bolt udržuje bez stavu, skutečně nezávisle na tom, kterou instanci hello rozdělovače obdrží které věty.</span><span class="sxs-lookup"><span data-stu-id="97555-231">Since hello Splitter bolt maintains no state, it really doesn't matter which instance of hello splitter receives which sentence.</span></span>

<span data-ttu-id="97555-232">Otevřete **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="97555-232">Open **Program.cs**.</span></span> <span data-ttu-id="97555-233">je důležité metoda Hello **GetTopologyBuilder**, což je použité toodefine hello topologie, která je odeslána tooStorm.</span><span class="sxs-lookup"><span data-stu-id="97555-233">hello important method is **GetTopologyBuilder**, which is used toodefine hello topology that is submitted tooStorm.</span></span> <span data-ttu-id="97555-234">Nahraďte obsah hello **GetTopologyBuilder** s hello následující kód tooimplement hello topologie popsali výš:</span><span class="sxs-lookup"><span data-stu-id="97555-234">Replace hello contents of **GetTopologyBuilder** with hello following code tooimplement hello topology described previously:</span></span>

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add hello spout toohello topology.
// Name hello component 'sentences'
// Name hello field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add hello splitter bolt toohello topology.
// Name hello component 'splitter'
// Name hello field that is emitted 'word'
// Use suffleGrouping toodistribute incoming tuples
//   from hello 'sentences' spout across instances
//   of hello splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add hello counter bolt toohello topology.
// Name hello component 'counter'
// Name hello fields that are emitted 'word' and 'count'
// Use fieldsGrouping tooensure that tuples are routed
//   toocounter instances based on hello contents of field
//   position 0 (hello word). This could also have been
//   List<string>(){"word"}.
//   This ensures that hello word 'jumped', for example, will always
//   go toohello same instance
topologyBuilder.SetBolt(
    "counter",
    Counter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
    },
    1).fieldsGrouping("splitter", new List<int>() { 0 });

// Add topology config
topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
{
    {"topology.kryo.register","[\"[B\"]"}
});

return topologyBuilder;
```

## <a name="submit-hello-topology"></a><span data-ttu-id="97555-235">Odeslání hello topologie</span><span class="sxs-lookup"><span data-stu-id="97555-235">Submit hello topology</span></span>

1. <span data-ttu-id="97555-236">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **odeslání tooStorm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="97555-236">In **Solution Explorer**, right-click hello project, and select **Submit tooStorm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="97555-237">Pokud se zobrazí výzva, zadejte přihlašovací údaje hello vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="97555-237">If prompted, enter hello credentials for your Azure subscription.</span></span> <span data-ttu-id="97555-238">Pokud máte více než jedno předplatné, přihlaste se toohello, která obsahuje váš cluster Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="97555-238">If you have more than one subscription, sign in toohello one that contains your Storm on HDInsight cluster.</span></span>

2. <span data-ttu-id="97555-239">Vyberte váš cluster Storm v HDInsight z hello **Storm Cluster** rozevíracího seznamu a potom vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="97555-239">Select your Storm on HDInsight cluster from hello **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="97555-240">Můžete sledovat, pokud je pomocí hello úspěšné odeslání hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="97555-240">You can monitor if hello submission is successful by using hello **Output** window.</span></span>

3. <span data-ttu-id="97555-241">Když hello topologie byl úspěšně odeslán, hello **topologie Storm** pro hello clusteru by se měla objevit.</span><span class="sxs-lookup"><span data-stu-id="97555-241">When hello topology has been successfully submitted, hello **Storm Topologies** for hello cluster should appear.</span></span> <span data-ttu-id="97555-242">Vyberte hello **WordCount** topologie z hello seznamu tooview informace o hello spuštěná topologie.</span><span class="sxs-lookup"><span data-stu-id="97555-242">Select hello **WordCount** topology from hello list tooview information about hello running topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="97555-243">Můžete také zobrazit **topologie Storm** z **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="97555-243">You can also view **Storm Topologies** from **Server Explorer**.</span></span> <span data-ttu-id="97555-244">Rozbalte položku **Azure** > **HDInsight**, klikněte pravým tlačítkem myši cluster Storm v HDInsight a pak vyberte **topologie Storm zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="97555-244">Expand **Azure** > **HDInsight**, right-click a Storm on HDInsight cluster, and then select **View Storm Topologies**.</span></span>

    <span data-ttu-id="97555-245">tooview informace o komponentách hello v topologii hello, dvakrát klikněte na součást hello v diagramu hello.</span><span class="sxs-lookup"><span data-stu-id="97555-245">tooview information about hello components in hello topology, double-click hello component in hello diagram.</span></span>

4. <span data-ttu-id="97555-246">Z hello **souhrn topologie** zobrazit, klikněte na tlačítko **Kill** toostop hello topologie.</span><span class="sxs-lookup"><span data-stu-id="97555-246">From hello **Topology Summary** view, click **Kill** toostop hello topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="97555-247">Topologie Storm pokračovat toorun, dokud se deaktivovala nebo odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="97555-247">Storm topologies continue toorun until they are deactivated, or hello cluster is deleted.</span></span>

## <a name="transactional-topology"></a><span data-ttu-id="97555-248">Transakční topologie</span><span class="sxs-lookup"><span data-stu-id="97555-248">Transactional topology</span></span>

<span data-ttu-id="97555-249">předchozí topologie Hello je netransakční.</span><span class="sxs-lookup"><span data-stu-id="97555-249">hello previous topology is non-transactional.</span></span> <span data-ttu-id="97555-250">Hello součásti v topologii hello neimplementuje funkci tooreplaying zprávy.</span><span class="sxs-lookup"><span data-stu-id="97555-250">hello components in hello topology do not implement functionality tooreplaying messages.</span></span> <span data-ttu-id="97555-251">Příklad transakční topologie vytvořte projekt a vyberte **Storm ukázka** jako typ projektu hello.</span><span class="sxs-lookup"><span data-stu-id="97555-251">For an example of a transactional topology, create a project and select **Storm Sample** as hello project type.</span></span>

<span data-ttu-id="97555-252">Transakční topologie implementovat hello následující toosupport opakování dat:</span><span class="sxs-lookup"><span data-stu-id="97555-252">Transactional topologies implement hello following toosupport replay of data:</span></span>

* <span data-ttu-id="97555-253">**Ukládání do mezipaměti metadat**: hello spout musí ukládat metadata o datech hello vygenerované, tak, aby hello dat můžete načíst a vygenerované znovu, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="97555-253">**Metadata caching**: hello spout must store metadata about hello data emitted, so that hello data can be retrieved and emitted again if a failure occurs.</span></span> <span data-ttu-id="97555-254">Protože vysílaných hello ukázková data hello je malá, je hello nezpracovaná data pro každý záznam uloženy ve slovníku pro opakování.</span><span class="sxs-lookup"><span data-stu-id="97555-254">Because hello data emitted by hello sample is small, hello raw data for each tuple is stored in a dictionary for replay.</span></span>

* <span data-ttu-id="97555-255">**Potvrzení**: každý bolt v topologii hello můžete volat `this.ctx.Ack(tuple)` tooacknowledge, že je úspěšně zpracovala řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="97555-255">**Ack**: Each bolt in hello topology can call `this.ctx.Ack(tuple)` tooacknowledge that it has successfully processed a tuple.</span></span> <span data-ttu-id="97555-256">Pokud všechny funkce bolts acked hello řazené kolekce členů, hello `Ack` je volána metoda hello spout.</span><span class="sxs-lookup"><span data-stu-id="97555-256">When all bolts have acked hello tuple, hello `Ack` method of hello spout is invoked.</span></span> <span data-ttu-id="97555-257">Hello `Ack` metoda umožňuje hello spout tooremove data, která se ukládá do mezipaměti pro opakování.</span><span class="sxs-lookup"><span data-stu-id="97555-257">hello `Ack` method allows hello spout tooremove data that was cached for replay.</span></span>

* <span data-ttu-id="97555-258">**Selhání**: můžete volat každý bolt `this.ctx.Fail(tuple)` tooindicate toto zpracování došlo k chybě na řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="97555-258">**Fail**: Each bolt can call `this.ctx.Fail(tuple)` tooindicate that processing has failed for a tuple.</span></span> <span data-ttu-id="97555-259">selhání Hello rozšíří toohello `Fail` metoda hello spout, kde můžete s použitím přehrány hello řazené kolekce členů v mezipaměti metadat.</span><span class="sxs-lookup"><span data-stu-id="97555-259">hello failure propagates toohello `Fail` method of hello spout, where hello tuple can be replayed by using cached metadata.</span></span>

* <span data-ttu-id="97555-260">**Pořadí ID**: při generování řazené kolekce členů, můžete zadat ID jedinečný pořadí.</span><span class="sxs-lookup"><span data-stu-id="97555-260">**Sequence ID**: When emitting a tuple, a unique sequence ID can be specified.</span></span> <span data-ttu-id="97555-261">Tato hodnota identifikuje hello řazené kolekce členů pro zpracování opětovného přehrání (Ack a selhání).</span><span class="sxs-lookup"><span data-stu-id="97555-261">This value identifies hello tuple for replay (Ack and Fail) processing.</span></span> <span data-ttu-id="97555-262">Například hello spout v hello **Storm ukázka** projektu používá následující hello při generování dat:</span><span class="sxs-lookup"><span data-stu-id="97555-262">For example, hello spout in hello **Storm Sample** project uses hello following when emitting data:</span></span>

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    <span data-ttu-id="97555-263">Tento kód vysílá řazené kolekce členů obsahující větu toohello výchozí datový proud, se hodnota ID pořadí hello obsažené v **lastSeqId**.</span><span class="sxs-lookup"><span data-stu-id="97555-263">This code emits a tuple that contains a sentence toohello default stream, with hello sequence ID value contained in **lastSeqId**.</span></span> <span data-ttu-id="97555-264">V tomto příkladu **lastSeqId** se zvýší při každé řazené kolekce členů vygenerované.</span><span class="sxs-lookup"><span data-stu-id="97555-264">For this example, **lastSeqId** is incremented for every tuple emitted.</span></span>

<span data-ttu-id="97555-265">Jak je předvedeno v hello **Storm ukázka** projektu, zda je součást transakční lze nastavit v době běhu v závislosti na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="97555-265">As demonstrated in hello **Storm Sample** project, whether a component is transactional can be set at runtime, based on configuration.</span></span>

## <a name="hybrid-topology-with-c-and-java"></a><span data-ttu-id="97555-266">Hybridní topologie s C# a Java</span><span class="sxs-lookup"><span data-stu-id="97555-266">Hybrid topology with C# and Java</span></span>

<span data-ttu-id="97555-267">Můžete taky nástrojů Data Lake pro Visual Studio toocreate hybridní topologie, kde jsou některé součásti C# a jiné jsou Java.</span><span class="sxs-lookup"><span data-stu-id="97555-267">You can also use Data Lake tools for Visual Studio toocreate hybrid topologies, where some components are C# and others are Java.</span></span>

<span data-ttu-id="97555-268">Příklad hybridní topologie, vytvořte projekt a vyberte **Storm hybridní ukázka**.</span><span class="sxs-lookup"><span data-stu-id="97555-268">For an example of a hybrid topology, create a project and select **Storm Hybrid Sample**.</span></span> <span data-ttu-id="97555-269">Tento typ ukázka ukazuje hello následující koncepty:</span><span class="sxs-lookup"><span data-stu-id="97555-269">This sample type demonstrates hello following concepts:</span></span>

* <span data-ttu-id="97555-270">**Java spout** a **C# bolt**: definovaná v **HybridTopology_javaSpout_csharpBolt**.</span><span class="sxs-lookup"><span data-stu-id="97555-270">**Java spout** and **C# bolt**: Defined in **HybridTopology_javaSpout_csharpBolt**.</span></span>

    * <span data-ttu-id="97555-271">Transakční verze je definována v **HybridTopologyTx_javaSpout_csharpBolt**.</span><span class="sxs-lookup"><span data-stu-id="97555-271">A transactional version is defined in **HybridTopologyTx_javaSpout_csharpBolt**.</span></span>

* <span data-ttu-id="97555-272">**C# spout** a **Java bolt**: definovaná v **HybridTopology_csharpSpout_javaBolt**.</span><span class="sxs-lookup"><span data-stu-id="97555-272">**C# spout** and **Java bolt**: Defined in **HybridTopology_csharpSpout_javaBolt**.</span></span>

    * <span data-ttu-id="97555-273">Transakční verze je definována v **HybridTopologyTx_csharpSpout_javaBolt**.</span><span class="sxs-lookup"><span data-stu-id="97555-273">A transactional version is defined in **HybridTopologyTx_csharpSpout_javaBolt**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="97555-274">Tato verze také ukazuje, jak toouse Clojure kód z textového souboru jako součást Java.</span><span class="sxs-lookup"><span data-stu-id="97555-274">This version also demonstrates how toouse Clojure code from a text file as a Java component.</span></span>


<span data-ttu-id="97555-275">topologie hello tooswitch, který se používá při odeslání projektu hello jednoduše přesunout hello `[Active(true)]` topologie toohello příkaz toouse, chcete před odesláním ji toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="97555-275">tooswitch hello topology that is used when hello project is submitted, simply move hello `[Active(true)]` statement toohello topology you want toouse, before submitting it toohello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="97555-276">Všechny soubory hello Java, které jsou požadovány jsou uvedeny jako součást tohoto projektu v hello **JavaDependency** složky.</span><span class="sxs-lookup"><span data-stu-id="97555-276">All hello Java files that are required are provided as part of this project in hello **JavaDependency** folder.</span></span>

<span data-ttu-id="97555-277">Při vytváření a odesílání hybridní topologie, zvažte následující hello:</span><span class="sxs-lookup"><span data-stu-id="97555-277">Consider hello following when you are creating and submitting a hybrid topology:</span></span>

* <span data-ttu-id="97555-278">Je nutné použít **JavaComponentConstructor** toocreate instanci hello třída jazyka Java pro funkcích spout nebo bolt.</span><span class="sxs-lookup"><span data-stu-id="97555-278">You must use **JavaComponentConstructor** toocreate an instance of hello Java class for a spout or bolt.</span></span>

* <span data-ttu-id="97555-279">Měli byste použít **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** tooserialize data do nebo z komponent v jazyce Java z prostředí Java objekty tooJSON.</span><span class="sxs-lookup"><span data-stu-id="97555-279">You should use **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** tooserialize data into or out of Java components from Java objects tooJSON.</span></span>

* <span data-ttu-id="97555-280">Při odesílání hello topologie toohello serveru, je nutné použít hello **další konfigurace** možnost toospecify hello **cesty k souborům Java**.</span><span class="sxs-lookup"><span data-stu-id="97555-280">When submitting hello topology toohello server, you must use hello **Additional configurations** option toospecify hello **Java File paths**.</span></span> <span data-ttu-id="97555-281">Zadaná cesta Hello by měl být hello adresář, který obsahuje hello JAR soubory, které obsahují třídy Java.</span><span class="sxs-lookup"><span data-stu-id="97555-281">hello path specified should be hello directory that contains hello JAR files that contain your Java classes.</span></span>

### <a name="azure-event-hubs"></a><span data-ttu-id="97555-282">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="97555-282">Azure Event Hubs</span></span>

<span data-ttu-id="97555-283">Verze SCP.NET 0.9.4.203 zavádí nové třídy a metody speciálně pro práci s funkcí spout centra událostí hello (spout Java, který čte ze služby Event Hubs).</span><span class="sxs-lookup"><span data-stu-id="97555-283">SCP.NET version 0.9.4.203 introduces a new class and method specifically for working with hello Event Hub spout (a Java spout that reads from Event Hubs).</span></span> <span data-ttu-id="97555-284">Když vytvoříte topologii, která se používá spout centra událostí, použijte hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="97555-284">When you create a topology that uses an Event Hub spout, use hello following methods:</span></span>

* <span data-ttu-id="97555-285">**EventHubSpoutConfig** – třída: vytvoří objekt, který obsahuje hello konfiguraci pro součást spout hello.</span><span class="sxs-lookup"><span data-stu-id="97555-285">**EventHubSpoutConfig** class: Creates an object that contains hello configuration for hello spout component.</span></span>

* <span data-ttu-id="97555-286">**TopologyBuilder.SetEventHubSpout** metoda: Přidá hello Event Hub spout součást toohello topologie.</span><span class="sxs-lookup"><span data-stu-id="97555-286">**TopologyBuilder.SetEventHubSpout** method: Adds hello Event Hub spout component toohello topology.</span></span>

> [!NOTE]
> <span data-ttu-id="97555-287">Stále je nutné použít hello **CustomizedInteropJSONSerializer** tooserialize data vytvořená pomocí funkcí spout hello.</span><span class="sxs-lookup"><span data-stu-id="97555-287">You must still use hello **CustomizedInteropJSONSerializer** tooserialize data produced by hello spout.</span></span>

## <span data-ttu-id="97555-288"><a id="configurationmanager"></a>Použití ConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="97555-288"><a id="configurationmanager"></a>Use ConfigurationManager</span></span>

<span data-ttu-id="97555-289">Nepoužívejte **ConfigurationManager** tooretrieve konfigurační hodnoty z funkce bolt a spout součásti.</span><span class="sxs-lookup"><span data-stu-id="97555-289">Don't use **ConfigurationManager** tooretrieve configuration values from bolt and spout components.</span></span> <span data-ttu-id="97555-290">Díky tomu může způsobit výjimku ukazatele null.</span><span class="sxs-lookup"><span data-stu-id="97555-290">Doing so can cause a null pointer exception.</span></span> <span data-ttu-id="97555-291">Místo toho hello konfigurace pro váš projekt je předán do topologie Storm hello jako dvojice klíč a hodnotu v kontextu topologie hello.</span><span class="sxs-lookup"><span data-stu-id="97555-291">Instead, hello configuration for your project is passed into hello Storm topology as a key and value pair in hello topology context.</span></span> <span data-ttu-id="97555-292">Jednotlivé komponenty, které jsou závislé na hodnoty konfigurace musíte je znovu načíst z kontextu hello během inicializace.</span><span class="sxs-lookup"><span data-stu-id="97555-292">Each component that relies on configuration values must retrieve them from hello context during initialization.</span></span>

<span data-ttu-id="97555-293">Hello následující kód ukazuje, jak tooretrieve tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="97555-293">hello following code demonstrates how tooretrieve these values:</span></span>

```csharp
public class MyComponent : ISCPBolt
{
    // toohold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of hello context for this component instance
        this.ctx = ctx;
        // If it exists, load hello configuration for hello component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve hello value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

<span data-ttu-id="97555-294">Pokud používáte `Get` metoda tooreturn na instanci příslušné součásti, ujistěte se, že předává obou hello `Context` a `Dictionary<string, Object>` konstruktor toohello parametry.</span><span class="sxs-lookup"><span data-stu-id="97555-294">If you use a `Get` method tooreturn an instance of your component, you must ensure that it passes both hello `Context` and `Dictionary<string, Object>` parameters toohello constructor.</span></span> <span data-ttu-id="97555-295">Hello následující příklad je základní `Get` metoda, která správně předává tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="97555-295">hello following example is a basic `Get` method that properly passes these values:</span></span>

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-tooupdate-scpnet"></a><span data-ttu-id="97555-296">Jak tooupdate SCP.NET</span><span class="sxs-lookup"><span data-stu-id="97555-296">How tooupdate SCP.NET</span></span>

<span data-ttu-id="97555-297">Poslední verze SCP.NET podporovat upgrade balíčku prostřednictvím balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="97555-297">Recent releases of SCP.NET support package upgrade through NuGet.</span></span> <span data-ttu-id="97555-298">Když je k dispozici nové aktualizace, zobrazí se oznámení o upgradu.</span><span class="sxs-lookup"><span data-stu-id="97555-298">When a new update is available, you receive an upgrade notification.</span></span> <span data-ttu-id="97555-299">Kontrola toomanually pro upgrade, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="97555-299">toomanually check for an upgrade, follow these steps:</span></span>

1. <span data-ttu-id="97555-300">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="97555-300">In **Solution Explorer**, right-click hello project, and select **Manage NuGet Packages**.</span></span>

2. <span data-ttu-id="97555-301">Správce balíčků hello, vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="97555-301">From hello package manager, select **Updates**.</span></span> <span data-ttu-id="97555-302">Pokud je k dispozici aktualizace, je uvedena.</span><span class="sxs-lookup"><span data-stu-id="97555-302">If an update is available, it is listed.</span></span> <span data-ttu-id="97555-303">Klikněte na tlačítko **aktualizace** pro balíček tooinstall hello ho.</span><span class="sxs-lookup"><span data-stu-id="97555-303">Click **Update** for hello package tooinstall it.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="97555-304">Pokud váš projekt byl vytvořen z předchozích verzí SCP.NET, který NuGet nepoužili, je třeba provést následující kroky tooupdate tooa novější verze hello:</span><span class="sxs-lookup"><span data-stu-id="97555-304">If your project was created with an earlier version of SCP.NET that did not use NuGet, you must perform hello following steps tooupdate tooa newer version:</span></span>
>
> 1. <span data-ttu-id="97555-305">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="97555-305">In **Solution Explorer**, right-click hello project, and select **Manage NuGet Packages**.</span></span>
> 2. <span data-ttu-id="97555-306">Pomocí hello **vyhledávání** pole, vyhledejte a pak přidejte, **Microsoft.SCP.Net.SDK** toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="97555-306">Using hello **Search** field, search for, and then add, **Microsoft.SCP.Net.SDK** toohello project.</span></span>

## <a name="troubleshoot-common-issues-with-topologies"></a><span data-ttu-id="97555-307">Řešení běžných problémů s topologie</span><span class="sxs-lookup"><span data-stu-id="97555-307">Troubleshoot common issues with topologies</span></span>

### <a name="null-pointer-exceptions"></a><span data-ttu-id="97555-308">Výjimky ukazatele Null.</span><span class="sxs-lookup"><span data-stu-id="97555-308">Null pointer exceptions</span></span>

<span data-ttu-id="97555-309">Při použití topologie C# s clusterem HDInsight se systémem Linux, funkce bolt a spout součásti, které používají **ConfigurationManager** tooread nastavení konfigurace v době běhu může vrátit výjimky ukazatele null.</span><span class="sxs-lookup"><span data-stu-id="97555-309">When you are using a C# topology with a Linux-based HDInsight cluster, bolt and spout components that use **ConfigurationManager** tooread configuration settings at runtime may return null pointer exceptions.</span></span>

<span data-ttu-id="97555-310">Hello konfigurace pro váš projekt je předán do topologie Storm hello jako dvojice klíč a hodnotu v kontextu topologie hello.</span><span class="sxs-lookup"><span data-stu-id="97555-310">hello configuration for your project is passed into hello Storm topology as a key and value pair in hello topology context.</span></span> <span data-ttu-id="97555-311">Mohou být načteny z hello objektu slovník, který je předán tooyour komponenty, při jejich inicializaci.</span><span class="sxs-lookup"><span data-stu-id="97555-311">It can be retrieved from hello dictionary object that is passed tooyour components when they are initialized.</span></span>

<span data-ttu-id="97555-312">Další informace najdete v tématu hello [ConfigurationManager](#configurationmanager) část tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="97555-312">For more information, see hello [ConfigurationManager](#configurationmanager) section of this document.</span></span>

### <a name="systemtypeloadexception"></a><span data-ttu-id="97555-313">System.TypeLoadException</span><span class="sxs-lookup"><span data-stu-id="97555-313">System.TypeLoadException</span></span>

<span data-ttu-id="97555-314">Při použití topologie C# s clusterem HDInsight se systémem Linux, se můžete setkat hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="97555-314">When you are using a C# topology with a Linux-based HDInsight cluster, you may encounter hello following error:</span></span>

    System.TypeLoadException: Failure has occurred while loading a type.

<span data-ttu-id="97555-315">K této chybě dojde, pokud použijete binární soubor, který není kompatibilní s verzí rozhraní .NET, která podporuje Mono hello.</span><span class="sxs-lookup"><span data-stu-id="97555-315">This error occurs when you use a binary that is not compatible with hello version of .NET that Mono supports.</span></span>

<span data-ttu-id="97555-316">Pro clustery HDInsight se systémem Linux Ujistěte se, že váš projekt používá binární soubory zkompilovaném pro rozhraní .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="97555-316">For Linux-based HDInsight clusters, make sure that your project uses binaries compiled for .NET 4.5.</span></span>

### <a name="test-a-topology-locally"></a><span data-ttu-id="97555-317">Testování topologii místně</span><span class="sxs-lookup"><span data-stu-id="97555-317">Test a topology locally</span></span>

<span data-ttu-id="97555-318">I když je snadno toodeploy cluster tooa topologie, v některých případech může být nutné tootest topologii místně.</span><span class="sxs-lookup"><span data-stu-id="97555-318">Although it is easy toodeploy a topology tooa cluster, in some cases, you may need tootest a topology locally.</span></span> <span data-ttu-id="97555-319">Použijte následující postup toorun hello a testování ukázkové topologie hello v tomto kurzu místně ve vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="97555-319">Use hello following steps toorun and test hello example topology in this tutorial locally in your development environment.</span></span>

> [!WARNING]
> <span data-ttu-id="97555-320">Místní testování funguje výhradně u basic, C# – pouze topologie.</span><span class="sxs-lookup"><span data-stu-id="97555-320">Local testing only works for basic, C#-only topologies.</span></span> <span data-ttu-id="97555-321">Nemůžete použít místní testování pro hybridní topologie nebo topologie, které používají víc datových proudů.</span><span class="sxs-lookup"><span data-stu-id="97555-321">You cannot use local testing for hybrid topologies or topologies that use multiple streams.</span></span>

1. <span data-ttu-id="97555-322">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="97555-322">In **Solution Explorer**, right-click hello project, and select **Properties**.</span></span> <span data-ttu-id="97555-323">V okně Vlastnosti projektu hello změnit hello **výstupní typ** příliš**konzolové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="97555-323">In hello project properties, change hello **Output type** too**Console Application**.</span></span>

    ![Snímek obrazovky vlastností projektu, se zvýrazněným typem výstupu](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

   > [!NOTE]
   > <span data-ttu-id="97555-325">Mějte na paměti, toochange hello **výstupní typ** zpět příliš**knihovny tříd** před nasazením hello topologie tooa clusteru.</span><span class="sxs-lookup"><span data-stu-id="97555-325">Remember toochange hello **Output type** back too**Class Library** before you deploy hello topology tooa cluster.</span></span>

2. <span data-ttu-id="97555-326">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a pak vyberte **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="97555-326">In **Solution Explorer**, right-click hello project, and then select **Add** > **New Item**.</span></span> <span data-ttu-id="97555-327">Vyberte **třída**a zadejte **LocalTest.cs** jako název třídy hello.</span><span class="sxs-lookup"><span data-stu-id="97555-327">Select **Class**, and enter **LocalTest.cs** as hello class name.</span></span> <span data-ttu-id="97555-328">Nakonec klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="97555-328">Finally, click **Add**.</span></span>

3. <span data-ttu-id="97555-329">Otevřete **LocalTest.cs**a přidejte následující hello **pomocí** příkaz v horní části hello:</span><span class="sxs-lookup"><span data-stu-id="97555-329">Open **LocalTest.cs**, and add hello following **using** statement at hello top:</span></span>

    ```csharp
    using Microsoft.SCP;
    ```

4. <span data-ttu-id="97555-330">Použití hello následující kód jako obsah hello hello **LocalTest** třídy:</span><span class="sxs-lookup"><span data-stu-id="97555-330">Use hello following code as hello contents of hello **LocalTest** class:</span></span>

    ```csharp
    // Drives hello topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test hello spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of hello spout, using hello local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext toopersist hello data stream toofile
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test hello splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set hello data stream toohello data created by hello spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test hello counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set hello data stream toohello data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    <span data-ttu-id="97555-331">Trvat chvíli tooread prostřednictvím komentáře kódu hello.</span><span class="sxs-lookup"><span data-stu-id="97555-331">Take a moment tooread through hello code comments.</span></span> <span data-ttu-id="97555-332">Tento kód používá **LocalContext** toorun hello součásti v hello vývojového prostředí a přetrvává hello datový proud mezi komponenty tootext soubory na místní disk hello.</span><span class="sxs-lookup"><span data-stu-id="97555-332">This code uses **LocalContext** toorun hello components in hello development environment, and it persists hello data stream between components tootext files on hello local drive.</span></span>

1. <span data-ttu-id="97555-333">Otevřete **Program.cs**a přidejte následující toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="97555-333">Open **Program.cs**, and add hello following toohello **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize hello runtime
    SCPRuntime.Initialize();

    //If we are not running under hello local context, throw an error
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
    {
        throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
    }
    // Create test instance
    LocalTest tests = new LocalTest();
    // Run tests
    tests.RunTestCase();
    Console.WriteLine("Tests finished");
    Console.ReadKey();
    ```

2. <span data-ttu-id="97555-334">Uložte změny hello a pak klikněte na tlačítko **F5** nebo vyberte **ladění** > **spustit ladění** toostart hello projektu.</span><span class="sxs-lookup"><span data-stu-id="97555-334">Save hello changes, and then click **F5** or select **Debug** > **Start Debugging** toostart hello project.</span></span> <span data-ttu-id="97555-335">Okno konzoly by měla zobrazovat a stavu protokolu jako průběhu testů hello.</span><span class="sxs-lookup"><span data-stu-id="97555-335">A console window should appear, and log status as hello tests progress.</span></span> <span data-ttu-id="97555-336">Když **dokončení testů** se zobrazí, stiskněte klávesu žádné klíče tooclose hello okno.</span><span class="sxs-lookup"><span data-stu-id="97555-336">When **Tests finished** appears, press any key tooclose hello window.</span></span>

3. <span data-ttu-id="97555-337">Použití **Průzkumníka Windows** toolocate hello adresář, který obsahuje projektu.</span><span class="sxs-lookup"><span data-stu-id="97555-337">Use **Windows Explorer** toolocate hello directory that contains your project.</span></span> <span data-ttu-id="97555-338">Příklad: **C:\Users\<vaše_uživatelské_jméno > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**.</span><span class="sxs-lookup"><span data-stu-id="97555-338">For example: **C:\Users\<your_user_name>\Documents\Visual Studio 2013\Projects\WordCount\WordCount**.</span></span> <span data-ttu-id="97555-339">V tomto adresáři otevřete **Bin**a potom klikněte na **ladění**.</span><span class="sxs-lookup"><span data-stu-id="97555-339">In this directory, open **Bin**, and then click **Debug**.</span></span> <span data-ttu-id="97555-340">Měli byste vidět hello textové soubory, které byly vytvořeny při hello testy byly spuštěny: sentences.txt, counter.txt a splitter.txt.</span><span class="sxs-lookup"><span data-stu-id="97555-340">You should see hello text files that were produced when hello tests ran: sentences.txt, counter.txt, and splitter.txt.</span></span> <span data-ttu-id="97555-341">Otevřete každý textový soubor a zkontrolujte hello data.</span><span class="sxs-lookup"><span data-stu-id="97555-341">Open each text file and inspect hello data.</span></span>

   > [!NOTE]
   > <span data-ttu-id="97555-342">Řetězec dat trvá jako pole desetinných míst v těchto souborech.</span><span class="sxs-lookup"><span data-stu-id="97555-342">String data persists as an array of decimal values in these files.</span></span> <span data-ttu-id="97555-343">Například \[[97,103,111]] v hello **splitter.txt** soubor je slovo hello *a*.</span><span class="sxs-lookup"><span data-stu-id="97555-343">For example, \[[97,103,111]] in hello **splitter.txt** file is hello word *and*.</span></span>

> [!NOTE]
> <span data-ttu-id="97555-344">Se, zda text hello tooset **typ projektu** zpět příliš**knihovny tříd** před nasazením tooa Storm v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="97555-344">Be sure tooset hello **Project type** back too**Class Library** before deploying tooa Storm on HDInsight cluster.</span></span>

### <a name="log-information"></a><span data-ttu-id="97555-345">Informace o protokolu</span><span class="sxs-lookup"><span data-stu-id="97555-345">Log information</span></span>

<span data-ttu-id="97555-346">Můžete snadno protokolovat informace ze součásti vaší topologie pomocí `Context.Logger`.</span><span class="sxs-lookup"><span data-stu-id="97555-346">You can easily log information from your topology components by using `Context.Logger`.</span></span> <span data-ttu-id="97555-347">Například následující hello vytvoří položku informační protokolu:</span><span class="sxs-lookup"><span data-stu-id="97555-347">For example, hello following creates an informational log entry:</span></span>

```csharp
Context.Logger.Info("Component started");
```

<span data-ttu-id="97555-348">Zaznamenané informace lze zobrazit z hello **protokol služby Hadoop**, který se nachází v **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="97555-348">Logged information can be viewed from hello **Hadoop Service Log**, which is found in **Server Explorer**.</span></span> <span data-ttu-id="97555-349">Rozbalte položku hello pro váš cluster Storm v HDInsight a pak rozbalte **protokol služby Hadoop**.</span><span class="sxs-lookup"><span data-stu-id="97555-349">Expand hello entry for your Storm on HDInsight cluster, and then expand **Hadoop Service Log**.</span></span> <span data-ttu-id="97555-350">Nakonec vyberte tooview souboru protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="97555-350">Finally, select hello log file tooview.</span></span>

> [!NOTE]
> <span data-ttu-id="97555-351">Hello protokoly jsou uloženy v hello účtu úložiště Azure, který je používán clusteru.</span><span class="sxs-lookup"><span data-stu-id="97555-351">hello logs are stored in hello Azure storage account that is used by your cluster.</span></span> <span data-ttu-id="97555-352">protokoly hello tooview v sadě Visual Studio, musíte se přihlásit toohello předplatné Azure, který vlastní účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="97555-352">tooview hello logs in Visual Studio, you must sign in toohello Azure subscription that owns hello storage account.</span></span>

### <a name="view-error-information"></a><span data-ttu-id="97555-353">Informace o chybě zobrazení</span><span class="sxs-lookup"><span data-stu-id="97555-353">View error information</span></span>

<span data-ttu-id="97555-354">tooview chybách, ke kterým došlo v topologii spuštěné, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="97555-354">tooview errors that have occurred in a running topology, use hello following steps:</span></span>

1. <span data-ttu-id="97555-355">Z **Průzkumníka serveru**, klikněte pravým tlačítkem na hello Storm v clusteru HDInsight a vyberte **topologie Storm zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="97555-355">From **Server Explorer**, right-click hello Storm on HDInsight cluster, and select **View Storm topologies**.</span></span>

2. <span data-ttu-id="97555-356">Pro hello **Spout** a **Bolts**, hello **poslední chyba** sloupec obsahuje informace o poslední chybě hello.</span><span class="sxs-lookup"><span data-stu-id="97555-356">For hello **Spout** and **Bolts**, hello **Last Error** column contains information on hello last error.</span></span>

3. <span data-ttu-id="97555-357">Vyberte hello **Spout Id** nebo **Bolt Id** pro hello komponenty, která obsahuje chybu uvedené.</span><span class="sxs-lookup"><span data-stu-id="97555-357">Select hello **Spout Id** or **Bolt Id** for hello component that has an error listed.</span></span> <span data-ttu-id="97555-358">Na stránce s podrobnostmi o hello, který je zobrazený, další chyba je uvedena informace ve hello **chyby** oddíl hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="97555-358">On hello details page that is displayed, additional error information is listed in hello **Errors** section at hello bottom of hello page.</span></span>

4. <span data-ttu-id="97555-359">tooobtain Další informace, vyberte **Port** z hello **vykonavatelů** oddílu hello stránky, toosee hello Storm pracovní protokolu hello posledních několik minut.</span><span class="sxs-lookup"><span data-stu-id="97555-359">tooobtain more information, select a **Port** from hello **Executors** section of hello page, toosee hello Storm worker log for hello last few minutes.</span></span>

### <a name="errors-submitting-topologies"></a><span data-ttu-id="97555-360">Chyby odesílání topologie</span><span class="sxs-lookup"><span data-stu-id="97555-360">Errors submitting topologies</span></span>

<span data-ttu-id="97555-361">Pokud narazíte na chyby odesílání topologie tooHDInsight, můžete najít protokoly pro hello serverové komponenty, které zpracovávají topologie odeslání na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="97555-361">If you encounter errors submitting a topology tooHDInsight, you can find logs for hello server-side components that handle topology submission on your HDInsight cluster.</span></span> <span data-ttu-id="97555-362">tooretrieve tyto protokoly, hello použijte následující příkaz z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="97555-362">tooretrieve these logs, use hello following command from a command line:</span></span>

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

<span data-ttu-id="97555-363">Nahraďte __sshuser__ s hello SSH uživatelský účet pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="97555-363">Replace __sshuser__ with hello SSH user account for hello cluster.</span></span> <span data-ttu-id="97555-364">Nahraďte __clustername__ s názvem hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="97555-364">Replace __clustername__ with hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="97555-365">Další informace o používání `scp` a `ssh` s HDInsight, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="97555-365">For more information on using `scp` and `ssh` with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="97555-366">Odesílání může selhat z několika důvodů:</span><span class="sxs-lookup"><span data-stu-id="97555-366">Submissions can fail for multiple reasons:</span></span>

* <span data-ttu-id="97555-367">JDK není nainstalována nebo není v cestě hello.</span><span class="sxs-lookup"><span data-stu-id="97555-367">JDK is not installed or is not in hello path.</span></span>
* <span data-ttu-id="97555-368">Požadované závislosti Java nejsou součástí hello odeslání.</span><span class="sxs-lookup"><span data-stu-id="97555-368">Required Java dependencies are not included in hello submission.</span></span>
* <span data-ttu-id="97555-369">Nekompatibilní závislosti.</span><span class="sxs-lookup"><span data-stu-id="97555-369">Incompatible dependencies.</span></span>
* <span data-ttu-id="97555-370">Duplicitní názvy topologie.</span><span class="sxs-lookup"><span data-stu-id="97555-370">Duplicate topology names.</span></span>

<span data-ttu-id="97555-371">Pokud hello `hdinsight-scpwebapi.out` protokol obsahuje `FileNotFoundException`, může to být způsobeno hello následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="97555-371">If hello `hdinsight-scpwebapi.out` log contains a `FileNotFoundException`, this might be caused by hello following conditions:</span></span>

* <span data-ttu-id="97555-372">Hello JDK není v cestě hello na hello vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="97555-372">hello JDK is not in hello path on hello development environment.</span></span> <span data-ttu-id="97555-373">Ověřte, že hello JDK je nainstalován v hello vývojového prostředí a že `%JAVA_HOME%/bin` se nachází na cestě hello.</span><span class="sxs-lookup"><span data-stu-id="97555-373">Verify that hello JDK is installed in hello development environment, and that `%JAVA_HOME%/bin` is in hello path.</span></span>
* <span data-ttu-id="97555-374">Chybí závislost Java.</span><span class="sxs-lookup"><span data-stu-id="97555-374">You are missing a Java dependency.</span></span> <span data-ttu-id="97555-375">Ujistěte se, že všechny požadované .jar soubory jsou včetně jako součást hello odeslání.</span><span class="sxs-lookup"><span data-stu-id="97555-375">Make sure you are including any required .jar files as part of hello submission.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97555-376">Další kroky</span><span class="sxs-lookup"><span data-stu-id="97555-376">Next steps</span></span>

<span data-ttu-id="97555-377">Příklad zpracování dat ze služby Event Hubs naleznete v části [zpracovat události z Azure Event Hubs se Storm v HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="97555-377">For an example of processing data from Event Hubs, see [Process events from Azure Event Hubs with Storm on HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).</span></span>

<span data-ttu-id="97555-378">Příklad topologie C#, která rozdělí datový proud do různých datových proudů, naleznete v části [C# Storm příklad](https://github.com/Blackmist/csharp-storm-example).</span><span class="sxs-lookup"><span data-stu-id="97555-378">For an example of a C# topology that splits stream data into multiple streams, see [C# Storm example](https://github.com/Blackmist/csharp-storm-example).</span></span>

<span data-ttu-id="97555-379">Další informace o vytváření topologie C#, toodiscover naleznete v [Githubu](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).</span><span class="sxs-lookup"><span data-stu-id="97555-379">toodiscover more information about creating C# topologies, see [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).</span></span>

<span data-ttu-id="97555-380">Další způsoby toowork s HDInsight a další Storm v HDInsight ukázky najdete v tématu hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="97555-380">For more ways toowork with HDInsight and more Storm on HDInsight samples, see hello following documents:</span></span>

<span data-ttu-id="97555-381">**Microsoft SCP.NET**</span><span class="sxs-lookup"><span data-stu-id="97555-381">**Microsoft SCP.NET**</span></span>

* [<span data-ttu-id="97555-382">Průvodce programováním spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="97555-382">SCP programming guide</span></span>](hdinsight-storm-scp-programming-guide.md)

<span data-ttu-id="97555-383">**Apache Storm v HDInsight**</span><span class="sxs-lookup"><span data-stu-id="97555-383">**Apache Storm on HDInsight**</span></span>

* [<span data-ttu-id="97555-384">Nasazení a monitorování topologie s Apache Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="97555-384">Deploy and monitor topologies with Apache Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology.md)
* [<span data-ttu-id="97555-385">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="97555-385">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

<span data-ttu-id="97555-386">**Apache Hadoop v HDInsight**</span><span class="sxs-lookup"><span data-stu-id="97555-386">**Apache Hadoop on HDInsight**</span></span>

* [<span data-ttu-id="97555-387">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="97555-387">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="97555-388">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="97555-388">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="97555-389">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="97555-389">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="97555-390">**Apache HBase v HDInsight**</span><span class="sxs-lookup"><span data-stu-id="97555-390">**Apache HBase on HDInsight**</span></span>

* [<span data-ttu-id="97555-391">Začínáme s HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="97555-391">Getting started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started.md)
