---
title: "Topologií Apache Storm sadou Visual Studio a C# – Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak k vytváření topologie Storm v jazyce C#. Vytvořte topologii počtu jednoduché aplikace word v sadě Visual Studio pomocí nástroje Hadoop pro sadu Visual Studio."
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
ms.openlocfilehash: 3ee89b6644ba395e0a6c28ecc2c082c2f7393ac8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="develop-c-topologies-for-apache-storm-by-using-the-data-lake-tools-for-visual-studio"></a><span data-ttu-id="57900-104">Vývoj topologie C# pro Apache Storm pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="57900-104">Develop C# topologies for Apache Storm by using the Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="57900-105">Naučte se vytvářet topologie C# Storm pomocí nástrojů Azure Data Lake (Hadoop) pro Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="57900-105">Learn how to create a C# Storm topology by using the Azure Data Lake (Hadoop) tools for Visual Studio.</span></span> <span data-ttu-id="57900-106">Tento dokument vás provede procesem vytvoření projektu Storm v sadě Visual Studio, místní testování a nasazení do Apache Storm v clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="57900-106">This document walks through the process of creating a Storm project in Visual Studio, testing it locally, and deploying it to an Apache Storm on Azure HDInsight cluster.</span></span>

<span data-ttu-id="57900-107">Můžete také Naučte se vytvářet hybridní topologie, které používají C# a komponent v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="57900-107">You also learn how to create hybrid topologies that use C# and Java components.</span></span>

> [!NOTE]
> <span data-ttu-id="57900-108">Pokud kroky v tomto dokumentu se spoléhají na vývojového prostředí systému Windows pomocí sady Visual Studio, zkompilovaný projekt lze odeslat do clusteru Linux nebo HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="57900-108">While the steps in this document rely on a Windows development environment with Visual Studio, the compiled project can be submitted to either a Linux or Windows-based HDInsight cluster.</span></span> <span data-ttu-id="57900-109">Clustery se systémem Linux vytvořené po 28 říjnu 2016 se podporují jenom SCP.NET topologie.</span><span class="sxs-lookup"><span data-stu-id="57900-109">Only Linux-based clusters created after October 28, 2016, support SCP.NET topologies.</span></span>

<span data-ttu-id="57900-110">Pokud chcete používat topologie C# s clusterem se systémem Linux, je třeba aktualizovat balíček Microsoft.SCP.Net.SDK NuGet používá váš projekt na verzi 0.10.0.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="57900-110">To use a C# topology with a Linux-based cluster, you must update the Microsoft.SCP.Net.SDK NuGet package used by your project to version 0.10.0.6 or later.</span></span> <span data-ttu-id="57900-111">Verze balíčku se zároveň musí shodovat s hlavní verzí Stormu nainstalovanou ve službě HDInsight.</span><span class="sxs-lookup"><span data-stu-id="57900-111">The version of the package must also match the major version of Storm installed on HDInsight.</span></span>

| <span data-ttu-id="57900-112">HDInsight verze</span><span class="sxs-lookup"><span data-stu-id="57900-112">HDInsight version</span></span> | <span data-ttu-id="57900-113">Storm verze</span><span class="sxs-lookup"><span data-stu-id="57900-113">Storm version</span></span> | <span data-ttu-id="57900-114">SCP.NET verze</span><span class="sxs-lookup"><span data-stu-id="57900-114">SCP.NET version</span></span> | <span data-ttu-id="57900-115">Výchozí Mono verze</span><span class="sxs-lookup"><span data-stu-id="57900-115">Default Mono version</span></span> |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| <span data-ttu-id="57900-116">3.3</span><span class="sxs-lookup"><span data-stu-id="57900-116">3.3</span></span> |<span data-ttu-id="57900-117">0.10.x</span><span class="sxs-lookup"><span data-stu-id="57900-117">0.10.x</span></span> |<span data-ttu-id="57900-118">0.10.x.x</span><span class="sxs-lookup"><span data-stu-id="57900-118">0.10.x.x</span></span></br><span data-ttu-id="57900-119">(jenom na HDInsight se systémem Windows)</span><span class="sxs-lookup"><span data-stu-id="57900-119">(only on Windows-based HDInsight)</span></span> | <span data-ttu-id="57900-120">Není k dispozici</span><span class="sxs-lookup"><span data-stu-id="57900-120">NA</span></span> |
| <span data-ttu-id="57900-121">3.4</span><span class="sxs-lookup"><span data-stu-id="57900-121">3.4</span></span> | <span data-ttu-id="57900-122">0.10.0.x</span><span class="sxs-lookup"><span data-stu-id="57900-122">0.10.0.x</span></span> | <span data-ttu-id="57900-123">0.10.0.x</span><span class="sxs-lookup"><span data-stu-id="57900-123">0.10.0.x</span></span> | <span data-ttu-id="57900-124">3.2.8</span><span class="sxs-lookup"><span data-stu-id="57900-124">3.2.8</span></span> |
| <span data-ttu-id="57900-125">3,5</span><span class="sxs-lookup"><span data-stu-id="57900-125">3.5</span></span> | <span data-ttu-id="57900-126">1.0.2.x</span><span class="sxs-lookup"><span data-stu-id="57900-126">1.0.2.x</span></span> | <span data-ttu-id="57900-127">1.0.0.x</span><span class="sxs-lookup"><span data-stu-id="57900-127">1.0.0.x</span></span> | <span data-ttu-id="57900-128">4.2.1</span><span class="sxs-lookup"><span data-stu-id="57900-128">4.2.1</span></span> |
| <span data-ttu-id="57900-129">3.6</span><span class="sxs-lookup"><span data-stu-id="57900-129">3.6</span></span> | <span data-ttu-id="57900-130">1.1.0.x</span><span class="sxs-lookup"><span data-stu-id="57900-130">1.1.0.x</span></span> | <span data-ttu-id="57900-131">1.0.0.x</span><span class="sxs-lookup"><span data-stu-id="57900-131">1.0.0.x</span></span> | <span data-ttu-id="57900-132">4.2.8</span><span class="sxs-lookup"><span data-stu-id="57900-132">4.2.8</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="57900-133">Topologie jazyka C# v clusterech založených na Linuxu musí používat technologii .NET 4.5. a pro spuštění v clusteru HDInsight musí používat Mono.</span><span class="sxs-lookup"><span data-stu-id="57900-133">C# topologies on Linux-based clusters must use .NET 4.5, and use Mono to run on the HDInsight cluster.</span></span> <span data-ttu-id="57900-134">Zkontrolujte [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) pro potenciální nekompatibility.</span><span class="sxs-lookup"><span data-stu-id="57900-134">Check [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) for potential incompatibilities.</span></span>

## <a name="install-visual-studio"></a><span data-ttu-id="57900-135">Instalace sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="57900-135">Install Visual Studio</span></span>

<span data-ttu-id="57900-136">Pomocí jedné z následujících verzí sady Visual Studio můžete vyvíjet topologie C# s SCP.NET:</span><span class="sxs-lookup"><span data-stu-id="57900-136">You can develop C# topologies with SCP.NET by using one of the following versions of Visual Studio:</span></span>

* <span data-ttu-id="57900-137">Visual Studio 2012 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="57900-137">Visual Studio 2012 with [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>

* <span data-ttu-id="57900-138">Visual Studio 2013 s [aktualizací 4](http://www.microsoft.com/download/details.aspx?id=44921) nebo [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span><span class="sxs-lookup"><span data-stu-id="57900-138">Visual Studio 2013 with [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span></span>

* <span data-ttu-id="57900-139">Visual Studio 2015 nebo [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)</span><span class="sxs-lookup"><span data-stu-id="57900-139">Visual Studio 2015 or [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)</span></span>

* <span data-ttu-id="57900-140">Visual Studio 2017 (všechny edice)</span><span class="sxs-lookup"><span data-stu-id="57900-140">Visual Studio 2017 (any edition)</span></span>

## <a name="install-data-lake-tools-for-visual-studio"></a><span data-ttu-id="57900-141">Nástroje pro instalaci Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="57900-141">Install Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="57900-142">Instalace nástrojů Data Lake pro Visual Studio, postupujte podle kroků v [Začínáme pomocí nástrojů Data Lake pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="57900-142">To install Data Lake tools for Visual Studio, follow the steps in [Get started using Data Lake tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

## <a name="install-java"></a><span data-ttu-id="57900-143">Instalace Javy</span><span class="sxs-lookup"><span data-stu-id="57900-143">Install Java</span></span>

<span data-ttu-id="57900-144">Při odesílání topologie Storm ze sady Visual Studio SCP.NET generuje soubor zip, který obsahuje topologie a závislosti.</span><span class="sxs-lookup"><span data-stu-id="57900-144">When you submit a Storm topology from Visual Studio, SCP.NET generates a zip file that contains the topology and dependencies.</span></span> <span data-ttu-id="57900-145">Java se používá k vytvoření tyto soubory zip, protože používá formátu, který je více kompatibilní s clustery se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="57900-145">Java is used to create these zip files, because it uses a format that is more compatible with Linux-based clusters.</span></span>

1. <span data-ttu-id="57900-146">Nainstalujte Java Developer Kit (JDK) 7 nebo novější na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="57900-146">Install the Java Developer Kit (JDK) 7 or later on your development environment.</span></span> <span data-ttu-id="57900-147">Můžete získat JDK Oracle z [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="57900-147">You can get the Oracle JDK from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span> <span data-ttu-id="57900-148">Můžete také použít [jiných distribuce Java](http://openjdk.java.net/).</span><span class="sxs-lookup"><span data-stu-id="57900-148">You can also use [other Java distributions](http://openjdk.java.net/).</span></span>

2. <span data-ttu-id="57900-149">`JAVA_HOME` Proměnnou prostředí musí odkazovat na adresář, který obsahuje Java.</span><span class="sxs-lookup"><span data-stu-id="57900-149">The `JAVA_HOME` environment variable must point to the directory that contains Java.</span></span>

3. <span data-ttu-id="57900-150">`PATH` Musí obsahovat proměnné prostředí `%JAVA_HOME%\bin` adresáře.</span><span class="sxs-lookup"><span data-stu-id="57900-150">The `PATH` environment variable must include the `%JAVA_HOME%\bin` directory.</span></span>

<span data-ttu-id="57900-151">Chcete-li ověřit, zda jsou správně nainstalován Java a sadu JDK, můžete použít následující konzolovou aplikaci C#:</span><span class="sxs-lookup"><span data-stu-id="57900-151">You can use the following C# console application to verify that Java and the JDK are correctly installed:</span></span>

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

## <a name="storm-templates"></a><span data-ttu-id="57900-152">Šablony Storm</span><span class="sxs-lookup"><span data-stu-id="57900-152">Storm templates</span></span>

<span data-ttu-id="57900-153">Nástroje Data Lake pro Visual Studio poskytují následujících šablon:</span><span class="sxs-lookup"><span data-stu-id="57900-153">The Data Lake tools for Visual Studio provide the following templates:</span></span>

| <span data-ttu-id="57900-154">Typ projektu</span><span class="sxs-lookup"><span data-stu-id="57900-154">Project type</span></span> | <span data-ttu-id="57900-155">Demonstruje</span><span class="sxs-lookup"><span data-stu-id="57900-155">Demonstrates</span></span> |
| --- | --- |
| <span data-ttu-id="57900-156">Aplikace Storm</span><span class="sxs-lookup"><span data-stu-id="57900-156">Storm Application</span></span> |<span data-ttu-id="57900-157">Prázdný projekt topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="57900-157">An empty Storm topology project.</span></span> |
| <span data-ttu-id="57900-158">Ukázka zapisovače Azure SQL Storm</span><span class="sxs-lookup"><span data-stu-id="57900-158">Storm Azure SQL Writer Sample</span></span> |<span data-ttu-id="57900-159">Jak napsat do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="57900-159">How to write to Azure SQL Database.</span></span> |
| <span data-ttu-id="57900-160">Ukázka čtečky Azure Cosmos DB Storm</span><span class="sxs-lookup"><span data-stu-id="57900-160">Storm Azure Cosmos DB Reader Sample</span></span> |<span data-ttu-id="57900-161">Jak číst z Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="57900-161">How to read from Azure Cosmos DB.</span></span> |
| <span data-ttu-id="57900-162">Ukázka zapisovače Azure Cosmos DB Storm</span><span class="sxs-lookup"><span data-stu-id="57900-162">Storm Azure Cosmos DB Writer Sample</span></span> |<span data-ttu-id="57900-163">Postup zápisu do databáze Azure Cosmos.</span><span class="sxs-lookup"><span data-stu-id="57900-163">How to write to Azure Cosmos DB.</span></span> |
| <span data-ttu-id="57900-164">Ukázka EventHub čtečky Storm</span><span class="sxs-lookup"><span data-stu-id="57900-164">Storm EventHub Reader Sample</span></span> |<span data-ttu-id="57900-165">Jak číst z Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="57900-165">How to read from Azure Event Hubs.</span></span> |
| <span data-ttu-id="57900-166">Ukázka EventHub zapisovače Storm</span><span class="sxs-lookup"><span data-stu-id="57900-166">Storm EventHub Writer Sample</span></span> |<span data-ttu-id="57900-167">Jak zapsat do služby Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="57900-167">How to write to Azure Event Hubs.</span></span> |
| <span data-ttu-id="57900-168">Ukázka HBase čtečky Storm</span><span class="sxs-lookup"><span data-stu-id="57900-168">Storm HBase Reader Sample</span></span> |<span data-ttu-id="57900-169">Jak číst z HBase v HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="57900-169">How to read from HBase on HDInsight clusters.</span></span> |
| <span data-ttu-id="57900-170">Ukázka HBase zapisovače Storm</span><span class="sxs-lookup"><span data-stu-id="57900-170">Storm HBase Writer Sample</span></span> |<span data-ttu-id="57900-171">Postup zápisu HBase v HDInsight clustery.</span><span class="sxs-lookup"><span data-stu-id="57900-171">How to write to HBase on HDInsight clusters.</span></span> |
| <span data-ttu-id="57900-172">Ukázka hybridní Storm</span><span class="sxs-lookup"><span data-stu-id="57900-172">Storm Hybrid Sample</span></span> |<span data-ttu-id="57900-173">Postup používání komponent prostředí Java.</span><span class="sxs-lookup"><span data-stu-id="57900-173">How to use a Java component.</span></span> |
| <span data-ttu-id="57900-174">Ukázka Storm</span><span class="sxs-lookup"><span data-stu-id="57900-174">Storm Sample</span></span> |<span data-ttu-id="57900-175">Počet topologii základní aplikace word.</span><span class="sxs-lookup"><span data-stu-id="57900-175">A basic word count topology.</span></span> |

> [!WARNING]
> <span data-ttu-id="57900-176">Ne všechny šablony, bude fungovat s HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="57900-176">Not all templates will work with Linux-based HDInsight.</span></span> <span data-ttu-id="57900-177">Balíčky Nuget, které jsou používané šablony nemusí být kompatibilní s Mono.</span><span class="sxs-lookup"><span data-stu-id="57900-177">Nuget packages used by the templates may not be compatible with Mono.</span></span> <span data-ttu-id="57900-178">Zkontrolujte [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu a použít [.NET přenositelnost analyzátor](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) zjistit potenciální problémy.</span><span class="sxs-lookup"><span data-stu-id="57900-178">Check the [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document and use the [.NET Portability Analyzer](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) to identify potential problems.</span></span>

<span data-ttu-id="57900-179">Kroky v tomto dokumentu použijete k vytvoření topologii základní aplikace Storm typ projektu.</span><span class="sxs-lookup"><span data-stu-id="57900-179">In the steps in this document, you use the basic Storm Application project type to create a topology.</span></span>

### <a name="hbase-templates-notes"></a><span data-ttu-id="57900-180">Poznámky k šablony HBase</span><span class="sxs-lookup"><span data-stu-id="57900-180">HBase templates notes</span></span>

<span data-ttu-id="57900-181">Šablony čtení a zápis HBase REST API HBase, není rozhraní API HBase Java, používají ke komunikaci s HBase v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="57900-181">The HBase reader and writer templates use the HBase REST API, not the HBase Java API, to communicate with an HBase on HDInsight cluster.</span></span>

### <a name="eventhub-templates-notes"></a><span data-ttu-id="57900-182">Poznámky k EventHub šablony</span><span class="sxs-lookup"><span data-stu-id="57900-182">EventHub templates notes</span></span>

> [!IMPORTANT]
> <span data-ttu-id="57900-183">Komponentu spout založené na jazyce Java EventHub součástí šablony EventHub čtečky nemusí pracovat se Storm v HDInsight verze 3.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="57900-183">The Java-based EventHub spout component included with the EventHub Reader template may not work with Storm on HDInsight version 3.5 or later.</span></span> <span data-ttu-id="57900-184">Aktualizovanou verzi této součásti je k dispozici na [Githubu](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).</span><span class="sxs-lookup"><span data-stu-id="57900-184">An updated version of this component is available at [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).</span></span>

<span data-ttu-id="57900-185">Ukázkové topologie, který používá tato součást a spolupracuje s Storm v HDInsight 3.5, najdete v části [Githubu](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="57900-185">For an example topology that uses this component and works with Storm on HDInsight 3.5, see [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span>

## <a name="create-a-c-topology"></a><span data-ttu-id="57900-186">Vytvoření topologie C#</span><span class="sxs-lookup"><span data-stu-id="57900-186">Create a C# topology</span></span>

1. <span data-ttu-id="57900-187">Otevřete Visual Studio, vyberte **soubor** > **nový**a potom vyberte **projektu**.</span><span class="sxs-lookup"><span data-stu-id="57900-187">Open Visual Studio, select **File** > **New**, and then select **Project**.</span></span>

2. <span data-ttu-id="57900-188">Z **nový projekt** okně rozbalte **nainstalovaná** > **šablony**a vyberte **Azure Data Lake**.</span><span class="sxs-lookup"><span data-stu-id="57900-188">From the **New Project** window, expand **Installed** > **Templates**, and select **Azure Data Lake**.</span></span> <span data-ttu-id="57900-189">V seznamu šablon vyberte **aplikace Storm**.</span><span class="sxs-lookup"><span data-stu-id="57900-189">From the list of templates, select **Storm Application**.</span></span> <span data-ttu-id="57900-190">V dolní části obrazovky, zadejte **WordCount** jako název aplikace.</span><span class="sxs-lookup"><span data-stu-id="57900-190">At the bottom of the screen, enter **WordCount** as the name of the application.</span></span>

    ![Snímek obrazovky nový projekt – okno](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

3. <span data-ttu-id="57900-192">Po vytvoření projektu, musí mít následující soubory:</span><span class="sxs-lookup"><span data-stu-id="57900-192">After you have created the project, you should have the following files:</span></span>

   * <span data-ttu-id="57900-193">**Program.cs**: Tento soubor definuje topologie pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="57900-193">**Program.cs**: This file defines the topology for your project.</span></span> <span data-ttu-id="57900-194">Ve výchozím nastavení se vytvoří výchozí topologie, která se skládá z jedné spout a jeden bolt.</span><span class="sxs-lookup"><span data-stu-id="57900-194">A default topology that consists of one spout and one bolt is created by default.</span></span>

   * <span data-ttu-id="57900-195">**Spout.cs**: Příklad funkcí spout, který vysílá náhodných čísel.</span><span class="sxs-lookup"><span data-stu-id="57900-195">**Spout.cs**: An example spout that emits random numbers.</span></span>

   * <span data-ttu-id="57900-196">**Bolt.cs**: Příklad funkcí bolt, který uchovává počet čísla vysílaných funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="57900-196">**Bolt.cs**: An example bolt that keeps a count of numbers emitted by the spout.</span></span>

     <span data-ttu-id="57900-197">Když vytvoříte projekt, NuGet stáhne nejnovější [SCP.NET balíček](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).</span><span class="sxs-lookup"><span data-stu-id="57900-197">When you create the project, NuGet downloads the latest [SCP.NET package](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).</span></span>

     [!INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-the-spout"></a><span data-ttu-id="57900-198">Implementace funkcí spout</span><span class="sxs-lookup"><span data-stu-id="57900-198">Implement the spout</span></span>

1. <span data-ttu-id="57900-199">Otevřete **Spout.cs**.</span><span class="sxs-lookup"><span data-stu-id="57900-199">Open **Spout.cs**.</span></span> <span data-ttu-id="57900-200">Funkcích spouts slouží ke čtení z externího zdroje dat v topologii.</span><span class="sxs-lookup"><span data-stu-id="57900-200">Spouts are used to read data in a topology from an external source.</span></span> <span data-ttu-id="57900-201">Hlavní komponenty pro funkcí spout jsou:</span><span class="sxs-lookup"><span data-stu-id="57900-201">The main components for a spout are:</span></span>

   * <span data-ttu-id="57900-202">**NextTuple**: volány Storm, pokud je povoleno spout emitování nové řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="57900-202">**NextTuple**: Called by Storm when the spout is allowed to emit new tuples.</span></span>

   * <span data-ttu-id="57900-203">**Potvrzení** (pouze pro transakční topologie): zpracovává potvrzení iniciovaná ostatní součásti v topologii pro odeslaný spout řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="57900-203">**Ack** (transactional topology only): Handles acknowledgements initiated by other components in the topology for tuples sent from the spout.</span></span> <span data-ttu-id="57900-204">To v úvahu řazené kolekce členů umožňuje spout vědět, že byl úspěšně zpracován podřízené součásti.</span><span class="sxs-lookup"><span data-stu-id="57900-204">Acknowledging a tuple lets the spout know that it was processed successfully by downstream components.</span></span>

   * <span data-ttu-id="57900-205">**Selhání** (pouze pro transakční topologie): zpracovává řazené kolekce členů, které jsou selhání zpracování ostatní součásti v topologii.</span><span class="sxs-lookup"><span data-stu-id="57900-205">**Fail** (transactional topology only): Handles tuples that are fail-processing other components in the topology.</span></span> <span data-ttu-id="57900-206">Implementace metody selhání umožňuje znovu emitování řazenou kolekci členů, takže může být znovu zpracována.</span><span class="sxs-lookup"><span data-stu-id="57900-206">Implementing a Fail method allows you to re-emit the tuple so that it can be processed again.</span></span>

2. <span data-ttu-id="57900-207">Nahraďte obsah **Spout** třída tímto textem.</span><span class="sxs-lookup"><span data-stu-id="57900-207">Replace the contents of the **Spout** class with the following text.</span></span> <span data-ttu-id="57900-208">Tato spout náhodně vysílá věty do topologie.</span><span class="sxs-lookup"><span data-stu-id="57900-208">This spout randomly emits a sentence into the topology.</span></span>

    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "the cow jumped over the moon",
        "an apple a day keeps the doctor away",
        "four score and seven years ago",
        "snow white and the seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set the instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // The schema for the default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of the spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // The sentence to be emitted
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

### <a name="implement-the-bolts"></a><span data-ttu-id="57900-209">Implementace funkce bolts</span><span class="sxs-lookup"><span data-stu-id="57900-209">Implement the bolts</span></span>

1. <span data-ttu-id="57900-210">Odstraňte existující **Bolt.cs** souboru z projektu.</span><span class="sxs-lookup"><span data-stu-id="57900-210">Delete the existing **Bolt.cs** file from the project.</span></span>

2. <span data-ttu-id="57900-211">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="57900-211">In **Solution Explorer**, right-click the project, and select **Add** > **New item**.</span></span> <span data-ttu-id="57900-212">V seznamu vyberte **Storm Bolt**a zadejte **Splitter.cs** jako název.</span><span class="sxs-lookup"><span data-stu-id="57900-212">From the list, select **Storm Bolt**, and enter **Splitter.cs** as the name.</span></span> <span data-ttu-id="57900-213">Opakujte tento postup vytvoření druhého bolt s názvem **Counter.cs**.</span><span class="sxs-lookup"><span data-stu-id="57900-213">Repeat this process to create a second bolt named **Counter.cs**.</span></span>

   * <span data-ttu-id="57900-214">**Splitter.cs**: implementuje funkce bolt, které rozdělí věty do jednotlivých slov a vydá nové proud slova.</span><span class="sxs-lookup"><span data-stu-id="57900-214">**Splitter.cs**: Implements a bolt that splits sentences into individual words, and emits a new stream of words.</span></span>

   * <span data-ttu-id="57900-215">**Counter.cs**: implementuje funkce bolt, které počty jednotlivých slov a vysílá nového datového proudu slova a počty v jednotlivých slov.</span><span class="sxs-lookup"><span data-stu-id="57900-215">**Counter.cs**: Implements a bolt that counts each word, and emits a new stream of words and the count for each word.</span></span>

     > [!NOTE]
     > <span data-ttu-id="57900-216">Tyto funkce bolts čtení a zápis do datových proudů, ale můžete také použít na funkce bolt ke komunikaci s zdroje jako databázi nebo službě.</span><span class="sxs-lookup"><span data-stu-id="57900-216">These bolts read and write to streams, but you can also use a bolt to communicate with sources such as a database or service.</span></span>

3. <span data-ttu-id="57900-217">Otevřete **Splitter.cs**.</span><span class="sxs-lookup"><span data-stu-id="57900-217">Open **Splitter.cs**.</span></span> <span data-ttu-id="57900-218">Ve výchozím nastavení obsahuje pouze jednu metodu: **Execute**.</span><span class="sxs-lookup"><span data-stu-id="57900-218">It has only one method by default: **Execute**.</span></span> <span data-ttu-id="57900-219">Metoda Execute je volána, když obdrží bolt řazené kolekce členů pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="57900-219">The Execute method is called when the bolt receives a tuple for processing.</span></span> <span data-ttu-id="57900-220">Zde si můžete přečíst zpracovat příchozí řazených kolekcí členů a vydávání odchozích řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="57900-220">Here, you can read and process incoming tuples, and emit outbound tuples.</span></span>

4. <span data-ttu-id="57900-221">Nahraďte obsah **rozdělovače** třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="57900-221">Replace the contents of the **Splitter** class with the following code:</span></span>

    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (the sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (the word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of the bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get the sentence from the tuple
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

5. <span data-ttu-id="57900-222">Otevřete **Counter.cs**a nahraďte jeho obsah třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="57900-222">Open **Counter.cs**, and replace the class contents with the following:</span></span>

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
        // A tuple containing a string field - the word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - the word and the word count
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

        // Get the word from the tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for the word in the dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment the count
        count++;
        // Update the count in the dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit the word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```

### <a name="define-the-topology"></a><span data-ttu-id="57900-223">Definovat topologii</span><span class="sxs-lookup"><span data-stu-id="57900-223">Define the topology</span></span>

<span data-ttu-id="57900-224">Funkcích spouts a funkce bolts jsou uspořádány v grafu, který definuje tok dat mezi součástmi.</span><span class="sxs-lookup"><span data-stu-id="57900-224">Spouts and bolts are arranged in a graph, which defines how the data flows between components.</span></span> <span data-ttu-id="57900-225">Pro tuto topologii grafu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="57900-225">For this topology, the graph is as follows:</span></span>

![Diagram jak jsou uspořádány součásti](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

<span data-ttu-id="57900-227">Věty jsou nevydává spout a jsou distribuovány do instance rozdělovače bolt.</span><span class="sxs-lookup"><span data-stu-id="57900-227">Sentences are emitted from the spout, and are distributed to instances of the Splitter bolt.</span></span> <span data-ttu-id="57900-228">Bolt rozdělovače dělí věty do slova, které jsou distribuovány do bolt čítače.</span><span class="sxs-lookup"><span data-stu-id="57900-228">The Splitter bolt breaks the sentences into words, which are distributed to the Counter bolt.</span></span>

<span data-ttu-id="57900-229">Vzhledem k tomu, že počet slov trvá místně v instanci čítače, chceme, abyste měli jistotu, že určitá slova toku na stejnou instanci bolt čítače.</span><span class="sxs-lookup"><span data-stu-id="57900-229">Because word count is held locally in the Counter instance, we want to make sure that specific words flow to the same Counter bolt instance.</span></span> <span data-ttu-id="57900-230">Každá instance uchovává informace o konkrétní slova.</span><span class="sxs-lookup"><span data-stu-id="57900-230">Each instance keeps track of specific words.</span></span> <span data-ttu-id="57900-231">Vzhledem k tomu, že bolt rozdělovače udržuje bez stavu, skutečně nezávisle na tom, kterou instanci systému rozdělovače obdrží které věty.</span><span class="sxs-lookup"><span data-stu-id="57900-231">Since the Splitter bolt maintains no state, it really doesn't matter which instance of the splitter receives which sentence.</span></span>

<span data-ttu-id="57900-232">Otevřete **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="57900-232">Open **Program.cs**.</span></span> <span data-ttu-id="57900-233">Je důležité metoda **GetTopologyBuilder**, který se používá k definování topologie, které je odeslána do Storm.</span><span class="sxs-lookup"><span data-stu-id="57900-233">The important method is **GetTopologyBuilder**, which is used to define the topology that is submitted to Storm.</span></span> <span data-ttu-id="57900-234">Nahraďte obsah **GetTopologyBuilder** implementovat topologii popsané následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="57900-234">Replace the contents of **GetTopologyBuilder** with the following code to implement the topology described previously:</span></span>

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add the spout to the topology.
// Name the component 'sentences'
// Name the field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add the splitter bolt to the topology.
// Name the component 'splitter'
// Name the field that is emitted 'word'
// Use suffleGrouping to distribute incoming tuples
//   from the 'sentences' spout across instances
//   of the splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add the counter bolt to the topology.
// Name the component 'counter'
// Name the fields that are emitted 'word' and 'count'
// Use fieldsGrouping to ensure that tuples are routed
//   to counter instances based on the contents of field
//   position 0 (the word). This could also have been
//   List<string>(){"word"}.
//   This ensures that the word 'jumped', for example, will always
//   go to the same instance
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

## <a name="submit-the-topology"></a><span data-ttu-id="57900-235">Odeslání topologie</span><span class="sxs-lookup"><span data-stu-id="57900-235">Submit the topology</span></span>

1. <span data-ttu-id="57900-236">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **odeslání do Storm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="57900-236">In **Solution Explorer**, right-click the project, and select **Submit to Storm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="57900-237">Pokud se zobrazí výzva, zadejte přihlašovací údaje pro vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="57900-237">If prompted, enter the credentials for your Azure subscription.</span></span> <span data-ttu-id="57900-238">Pokud máte více než jedno předplatné, přihlaste se k ta, která obsahuje váš cluster Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="57900-238">If you have more than one subscription, sign in to the one that contains your Storm on HDInsight cluster.</span></span>

2. <span data-ttu-id="57900-239">Vyberte váš cluster Storm v HDInsight z **Storm Cluster** rozevíracího seznamu a potom vyberte **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="57900-239">Select your Storm on HDInsight cluster from the **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="57900-240">Můžete sledovat, pokud je úspěšné odesílání pomocí **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="57900-240">You can monitor if the submission is successful by using the **Output** window.</span></span>

3. <span data-ttu-id="57900-241">Když topologii byl úspěšně odeslán, **topologie Storm** pro cluster by se měla objevit.</span><span class="sxs-lookup"><span data-stu-id="57900-241">When the topology has been successfully submitted, the **Storm Topologies** for the cluster should appear.</span></span> <span data-ttu-id="57900-242">Vyberte **WordCount** topologie ze seznamu zobrazíte informace o spuštěné topologie.</span><span class="sxs-lookup"><span data-stu-id="57900-242">Select the **WordCount** topology from the list to view information about the running topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="57900-243">Můžete také zobrazit **topologie Storm** z **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="57900-243">You can also view **Storm Topologies** from **Server Explorer**.</span></span> <span data-ttu-id="57900-244">Rozbalte položku **Azure** > **HDInsight**, klikněte pravým tlačítkem myši cluster Storm v HDInsight a pak vyberte **topologie Storm zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="57900-244">Expand **Azure** > **HDInsight**, right-click a Storm on HDInsight cluster, and then select **View Storm Topologies**.</span></span>

    <span data-ttu-id="57900-245">Chcete-li zobrazit informace o komponentách v topologii, dvakrát klikněte na komponentu v diagramu.</span><span class="sxs-lookup"><span data-stu-id="57900-245">To view information about the components in the topology, double-click the component in the diagram.</span></span>

4. <span data-ttu-id="57900-246">Z **souhrn topologie** zobrazit, klikněte na tlačítko **Kill** k zastavení topologie.</span><span class="sxs-lookup"><span data-stu-id="57900-246">From the **Topology Summary** view, click **Kill** to stop the topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="57900-247">Topologie Storm i nadále spustit, dokud se deaktivovala nebo odstranění clusteru.</span><span class="sxs-lookup"><span data-stu-id="57900-247">Storm topologies continue to run until they are deactivated, or the cluster is deleted.</span></span>

## <a name="transactional-topology"></a><span data-ttu-id="57900-248">Transakční topologie</span><span class="sxs-lookup"><span data-stu-id="57900-248">Transactional topology</span></span>

<span data-ttu-id="57900-249">Předchozí topologie je netransakční.</span><span class="sxs-lookup"><span data-stu-id="57900-249">The previous topology is non-transactional.</span></span> <span data-ttu-id="57900-250">Součásti v topologii neimplementuje funkci k přehrání zprávy.</span><span class="sxs-lookup"><span data-stu-id="57900-250">The components in the topology do not implement functionality to replaying messages.</span></span> <span data-ttu-id="57900-251">Příklad transakční topologie vytvořte projekt a vyberte **Storm ukázka** jako typ projektu.</span><span class="sxs-lookup"><span data-stu-id="57900-251">For an example of a transactional topology, create a project and select **Storm Sample** as the project type.</span></span>

<span data-ttu-id="57900-252">Transakční topologie implementovat následující pro podporu opětovného přehrání dat:</span><span class="sxs-lookup"><span data-stu-id="57900-252">Transactional topologies implement the following to support replay of data:</span></span>

* <span data-ttu-id="57900-253">**Ukládání do mezipaměti metadat**: spout musí ukládat metadata o datech vygenerované, takže data můžete načíst a vygenerované znovu, pokud dojde k selhání.</span><span class="sxs-lookup"><span data-stu-id="57900-253">**Metadata caching**: The spout must store metadata about the data emitted, so that the data can be retrieved and emitted again if a failure occurs.</span></span> <span data-ttu-id="57900-254">Protože je v datech vysílaných ukázku malé, nezpracovaná data pro každý záznam je uloženy ve slovníku pro opakování.</span><span class="sxs-lookup"><span data-stu-id="57900-254">Because the data emitted by the sample is small, the raw data for each tuple is stored in a dictionary for replay.</span></span>

* <span data-ttu-id="57900-255">**Potvrzení**: každý bolt v topologii můžete volat `this.ctx.Ack(tuple)` aby vzali na vědomí, že je úspěšně zpracovala řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="57900-255">**Ack**: Each bolt in the topology can call `this.ctx.Ack(tuple)` to acknowledge that it has successfully processed a tuple.</span></span> <span data-ttu-id="57900-256">Pokud všechny funkce bolts acked řazené kolekce členů, `Ack` zavolání metody funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="57900-256">When all bolts have acked the tuple, the `Ack` method of the spout is invoked.</span></span> <span data-ttu-id="57900-257">`Ack` Metoda umožňuje spout odebrat data, která se ukládá do mezipaměti pro opakování.</span><span class="sxs-lookup"><span data-stu-id="57900-257">The `Ack` method allows the spout to remove data that was cached for replay.</span></span>

* <span data-ttu-id="57900-258">**Selhání**: můžete volat každý bolt `this.ctx.Fail(tuple)` k označení, že zpracování se nezdařilo pro řazené kolekce členů.</span><span class="sxs-lookup"><span data-stu-id="57900-258">**Fail**: Each bolt can call `this.ctx.Fail(tuple)` to indicate that processing has failed for a tuple.</span></span> <span data-ttu-id="57900-259">Selhání rozšíří do `Fail` metoda spout, kde můžete s použitím přehrány řazenou kolekci členů do mezipaměti metadat.</span><span class="sxs-lookup"><span data-stu-id="57900-259">The failure propagates to the `Fail` method of the spout, where the tuple can be replayed by using cached metadata.</span></span>

* <span data-ttu-id="57900-260">**Pořadí ID**: při generování řazené kolekce členů, můžete zadat ID jedinečný pořadí.</span><span class="sxs-lookup"><span data-stu-id="57900-260">**Sequence ID**: When emitting a tuple, a unique sequence ID can be specified.</span></span> <span data-ttu-id="57900-261">Tato hodnota určuje řazenou kolekci členů pro zpracování opětovného přehrání (Ack a selhání).</span><span class="sxs-lookup"><span data-stu-id="57900-261">This value identifies the tuple for replay (Ack and Fail) processing.</span></span> <span data-ttu-id="57900-262">Například spout v **Storm ukázka** projektu používá následující při generování dat:</span><span class="sxs-lookup"><span data-stu-id="57900-262">For example, the spout in the **Storm Sample** project uses the following when emitting data:</span></span>

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    <span data-ttu-id="57900-263">Tento kód vysílá řazené kolekce členů, který obsahuje věty do datového proudu výchozí hodnotou pořadí ID obsažené v **lastSeqId**.</span><span class="sxs-lookup"><span data-stu-id="57900-263">This code emits a tuple that contains a sentence to the default stream, with the sequence ID value contained in **lastSeqId**.</span></span> <span data-ttu-id="57900-264">V tomto příkladu **lastSeqId** se zvýší při každé řazené kolekce členů vygenerované.</span><span class="sxs-lookup"><span data-stu-id="57900-264">For this example, **lastSeqId** is incremented for every tuple emitted.</span></span>

<span data-ttu-id="57900-265">Jak je předvedeno v **Storm ukázka** projektu, zda je součást transakční lze nastavit v době běhu v závislosti na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="57900-265">As demonstrated in the **Storm Sample** project, whether a component is transactional can be set at runtime, based on configuration.</span></span>

## <a name="hybrid-topology-with-c-and-java"></a><span data-ttu-id="57900-266">Hybridní topologie s C# a Java</span><span class="sxs-lookup"><span data-stu-id="57900-266">Hybrid topology with C# and Java</span></span>

<span data-ttu-id="57900-267">Nástroje Data Lake pro Visual Studio můžete také vytvářet hybridní topologie, kde jsou některé součásti C# a jiné jsou Java.</span><span class="sxs-lookup"><span data-stu-id="57900-267">You can also use Data Lake tools for Visual Studio to create hybrid topologies, where some components are C# and others are Java.</span></span>

<span data-ttu-id="57900-268">Příklad hybridní topologie, vytvořte projekt a vyberte **Storm hybridní ukázka**.</span><span class="sxs-lookup"><span data-stu-id="57900-268">For an example of a hybrid topology, create a project and select **Storm Hybrid Sample**.</span></span> <span data-ttu-id="57900-269">Tento typ ukázka ukazuje následující koncepty:</span><span class="sxs-lookup"><span data-stu-id="57900-269">This sample type demonstrates the following concepts:</span></span>

* <span data-ttu-id="57900-270">**Java spout** a **C# bolt**: definovaná v **HybridTopology_javaSpout_csharpBolt**.</span><span class="sxs-lookup"><span data-stu-id="57900-270">**Java spout** and **C# bolt**: Defined in **HybridTopology_javaSpout_csharpBolt**.</span></span>

    * <span data-ttu-id="57900-271">Transakční verze je definována v **HybridTopologyTx_javaSpout_csharpBolt**.</span><span class="sxs-lookup"><span data-stu-id="57900-271">A transactional version is defined in **HybridTopologyTx_javaSpout_csharpBolt**.</span></span>

* <span data-ttu-id="57900-272">**C# spout** a **Java bolt**: definovaná v **HybridTopology_csharpSpout_javaBolt**.</span><span class="sxs-lookup"><span data-stu-id="57900-272">**C# spout** and **Java bolt**: Defined in **HybridTopology_csharpSpout_javaBolt**.</span></span>

    * <span data-ttu-id="57900-273">Transakční verze je definována v **HybridTopologyTx_csharpSpout_javaBolt**.</span><span class="sxs-lookup"><span data-stu-id="57900-273">A transactional version is defined in **HybridTopologyTx_csharpSpout_javaBolt**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="57900-274">Tato verze také ukazuje, jak použít Clojure kód z textového souboru jako součást Java.</span><span class="sxs-lookup"><span data-stu-id="57900-274">This version also demonstrates how to use Clojure code from a text file as a Java component.</span></span>


<span data-ttu-id="57900-275">Chcete-li přepnout topologie, která se používá při odeslání projektu, jednoduše přesunout `[Active(true)]` příkaz do topologie, kterou chcete použít, před odesláním do clusteru.</span><span class="sxs-lookup"><span data-stu-id="57900-275">To switch the topology that is used when the project is submitted, simply move the `[Active(true)]` statement to the topology you want to use, before submitting it to the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="57900-276">Všechny soubory Java, které jsou požadovány jsou uvedeny jako součást tohoto projektu v **JavaDependency** složky.</span><span class="sxs-lookup"><span data-stu-id="57900-276">All the Java files that are required are provided as part of this project in the **JavaDependency** folder.</span></span>

<span data-ttu-id="57900-277">Při vytváření a odesílání hybridní topologie, zvažte následující:</span><span class="sxs-lookup"><span data-stu-id="57900-277">Consider the following when you are creating and submitting a hybrid topology:</span></span>

* <span data-ttu-id="57900-278">Je nutné použít **JavaComponentConstructor** k vytvoření instance třídy Java pro funkcích spout nebo funkce bolt.</span><span class="sxs-lookup"><span data-stu-id="57900-278">You must use **JavaComponentConstructor** to create an instance of the Java class for a spout or bolt.</span></span>

* <span data-ttu-id="57900-279">Měli byste použít **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** k serializaci dat do nebo z komponent v jazyce Java z objekty Java do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="57900-279">You should use **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** to serialize data into or out of Java components from Java objects to JSON.</span></span>

* <span data-ttu-id="57900-280">Při odesílání topologie do serveru, je nutné použít **další konfigurace** možnost zadat **cesty k souborům Java**.</span><span class="sxs-lookup"><span data-stu-id="57900-280">When submitting the topology to the server, you must use the **Additional configurations** option to specify the **Java File paths**.</span></span> <span data-ttu-id="57900-281">Zadaná cesta musí být adresář, který obsahuje soubory JAR obsahující dané třídy jazyka Java.</span><span class="sxs-lookup"><span data-stu-id="57900-281">The path specified should be the directory that contains the JAR files that contain your Java classes.</span></span>

### <a name="azure-event-hubs"></a><span data-ttu-id="57900-282">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="57900-282">Azure Event Hubs</span></span>

<span data-ttu-id="57900-283">Verze SCP.NET 0.9.4.203 zavádí nové třídy a metody speciálně pro práci s funkcí spout centra událostí (spout Java, který čte ze služby Event Hubs).</span><span class="sxs-lookup"><span data-stu-id="57900-283">SCP.NET version 0.9.4.203 introduces a new class and method specifically for working with the Event Hub spout (a Java spout that reads from Event Hubs).</span></span> <span data-ttu-id="57900-284">Když vytvoříte topologii, která se používá spout centra událostí, použijte následující metody:</span><span class="sxs-lookup"><span data-stu-id="57900-284">When you create a topology that uses an Event Hub spout, use the following methods:</span></span>

* <span data-ttu-id="57900-285">**EventHubSpoutConfig** – třída: vytvoří objekt, který obsahuje konfiguraci pro součást funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="57900-285">**EventHubSpoutConfig** class: Creates an object that contains the configuration for the spout component.</span></span>

* <span data-ttu-id="57900-286">**TopologyBuilder.SetEventHubSpout** metoda: přidá komponentu spout centra událostí do topologie.</span><span class="sxs-lookup"><span data-stu-id="57900-286">**TopologyBuilder.SetEventHubSpout** method: Adds the Event Hub spout component to the topology.</span></span>

> [!NOTE]
> <span data-ttu-id="57900-287">Je nutné použít **CustomizedInteropJSONSerializer** k serializaci dat vytvářených funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="57900-287">You must still use the **CustomizedInteropJSONSerializer** to serialize data produced by the spout.</span></span>

## <span data-ttu-id="57900-288"><a id="configurationmanager"></a>Použití ConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="57900-288"><a id="configurationmanager"></a>Use ConfigurationManager</span></span>

<span data-ttu-id="57900-289">Nepoužívejte **ConfigurationManager** k načtení hodnoty konfigurace z bolt a spout součásti.</span><span class="sxs-lookup"><span data-stu-id="57900-289">Don't use **ConfigurationManager** to retrieve configuration values from bolt and spout components.</span></span> <span data-ttu-id="57900-290">Díky tomu může způsobit výjimku ukazatele null.</span><span class="sxs-lookup"><span data-stu-id="57900-290">Doing so can cause a null pointer exception.</span></span> <span data-ttu-id="57900-291">Místo toho konfigurace pro váš projekt je předán do topologie Storm jako dvojice klíč a hodnotu v kontextu topologie.</span><span class="sxs-lookup"><span data-stu-id="57900-291">Instead, the configuration for your project is passed into the Storm topology as a key and value pair in the topology context.</span></span> <span data-ttu-id="57900-292">Jednotlivé komponenty, které jsou závislé na hodnoty konfigurace musíte je znovu načíst z kontextu během inicializace.</span><span class="sxs-lookup"><span data-stu-id="57900-292">Each component that relies on configuration values must retrieve them from the context during initialization.</span></span>

<span data-ttu-id="57900-293">Následující kód ukazuje, jak se načtení těchto hodnot:</span><span class="sxs-lookup"><span data-stu-id="57900-293">The following code demonstrates how to retrieve these values:</span></span>

```csharp
public class MyComponent : ISCPBolt
{
    // To hold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of the context for this component instance
        this.ctx = ctx;
        // If it exists, load the configuration for the component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve the value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

<span data-ttu-id="57900-294">Pokud používáte `Get` metoda vrátí instanci komponenty, ujistěte se, že předá obě `Context` a `Dictionary<string, Object>` parametry konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="57900-294">If you use a `Get` method to return an instance of your component, you must ensure that it passes both the `Context` and `Dictionary<string, Object>` parameters to the constructor.</span></span> <span data-ttu-id="57900-295">Následující příklad je základní `Get` metoda, která správně předává tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="57900-295">The following example is a basic `Get` method that properly passes these values:</span></span>

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-to-update-scpnet"></a><span data-ttu-id="57900-296">Postup aktualizace SCP.NET</span><span class="sxs-lookup"><span data-stu-id="57900-296">How to update SCP.NET</span></span>

<span data-ttu-id="57900-297">Poslední verze SCP.NET podporovat upgrade balíčku prostřednictvím balíčku NuGet.</span><span class="sxs-lookup"><span data-stu-id="57900-297">Recent releases of SCP.NET support package upgrade through NuGet.</span></span> <span data-ttu-id="57900-298">Když je k dispozici nové aktualizace, zobrazí se oznámení o upgradu.</span><span class="sxs-lookup"><span data-stu-id="57900-298">When a new update is available, you receive an upgrade notification.</span></span> <span data-ttu-id="57900-299">Ručně zkontrolujte upgrade, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="57900-299">To manually check for an upgrade, follow these steps:</span></span>

1. <span data-ttu-id="57900-300">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="57900-300">In **Solution Explorer**, right-click the project, and select **Manage NuGet Packages**.</span></span>

2. <span data-ttu-id="57900-301">Správce balíčků, vyberte **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="57900-301">From the package manager, select **Updates**.</span></span> <span data-ttu-id="57900-302">Pokud je k dispozici aktualizace, je uvedena.</span><span class="sxs-lookup"><span data-stu-id="57900-302">If an update is available, it is listed.</span></span> <span data-ttu-id="57900-303">Klikněte na tlačítko **aktualizace** pro balíček k její instalaci.</span><span class="sxs-lookup"><span data-stu-id="57900-303">Click **Update** for the package to install it.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="57900-304">Pokud projekt byla vytvořena pomocí dřívější verzi SCP.NET, který NuGet nepoužili, musíte provést následující kroky k aktualizaci na novější verzi:</span><span class="sxs-lookup"><span data-stu-id="57900-304">If your project was created with an earlier version of SCP.NET that did not use NuGet, you must perform the following steps to update to a newer version:</span></span>
>
> 1. <span data-ttu-id="57900-305">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="57900-305">In **Solution Explorer**, right-click the project, and select **Manage NuGet Packages**.</span></span>
> 2. <span data-ttu-id="57900-306">Pomocí **vyhledávání** pole, vyhledejte a pak přidejte, **Microsoft.SCP.Net.SDK** do projektu.</span><span class="sxs-lookup"><span data-stu-id="57900-306">Using the **Search** field, search for, and then add, **Microsoft.SCP.Net.SDK** to the project.</span></span>

## <a name="troubleshoot-common-issues-with-topologies"></a><span data-ttu-id="57900-307">Řešení běžných problémů s topologie</span><span class="sxs-lookup"><span data-stu-id="57900-307">Troubleshoot common issues with topologies</span></span>

### <a name="null-pointer-exceptions"></a><span data-ttu-id="57900-308">Výjimky ukazatele Null.</span><span class="sxs-lookup"><span data-stu-id="57900-308">Null pointer exceptions</span></span>

<span data-ttu-id="57900-309">Při použití topologie C# s clusterem HDInsight se systémem Linux, funkce bolt a spout součásti, které používají **ConfigurationManager** číst nastavení konfigurace v době běhu může vrátit výjimky ukazatele null.</span><span class="sxs-lookup"><span data-stu-id="57900-309">When you are using a C# topology with a Linux-based HDInsight cluster, bolt and spout components that use **ConfigurationManager** to read configuration settings at runtime may return null pointer exceptions.</span></span>

<span data-ttu-id="57900-310">Konfigurace pro váš projekt je předán do topologie Storm jako dvojice klíč a hodnotu v kontextu topologie.</span><span class="sxs-lookup"><span data-stu-id="57900-310">The configuration for your project is passed into the Storm topology as a key and value pair in the topology context.</span></span> <span data-ttu-id="57900-311">Se dá načíst z objektu slovník, který je předán u součástí při jejich inicializaci.</span><span class="sxs-lookup"><span data-stu-id="57900-311">It can be retrieved from the dictionary object that is passed to your components when they are initialized.</span></span>

<span data-ttu-id="57900-312">Další informace najdete v tématu [ConfigurationManager](#configurationmanager) část tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="57900-312">For more information, see the [ConfigurationManager](#configurationmanager) section of this document.</span></span>

### <a name="systemtypeloadexception"></a><span data-ttu-id="57900-313">System.TypeLoadException</span><span class="sxs-lookup"><span data-stu-id="57900-313">System.TypeLoadException</span></span>

<span data-ttu-id="57900-314">Při použití topologie C# s clusterem HDInsight se systémem Linux, může dojít k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="57900-314">When you are using a C# topology with a Linux-based HDInsight cluster, you may encounter the following error:</span></span>

    System.TypeLoadException: Failure has occurred while loading a type.

<span data-ttu-id="57900-315">K této chybě dojde, pokud použijete binární soubor, který není kompatibilní s verzí rozhraní .NET, která podporuje Mono.</span><span class="sxs-lookup"><span data-stu-id="57900-315">This error occurs when you use a binary that is not compatible with the version of .NET that Mono supports.</span></span>

<span data-ttu-id="57900-316">Pro clustery HDInsight se systémem Linux Ujistěte se, že váš projekt používá binární soubory zkompilovaném pro rozhraní .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="57900-316">For Linux-based HDInsight clusters, make sure that your project uses binaries compiled for .NET 4.5.</span></span>

### <a name="test-a-topology-locally"></a><span data-ttu-id="57900-317">Testování topologii místně</span><span class="sxs-lookup"><span data-stu-id="57900-317">Test a topology locally</span></span>

<span data-ttu-id="57900-318">I když je snadno nasadit topologii do clusteru, v některých případech může je potřeba provést testování topologii místně.</span><span class="sxs-lookup"><span data-stu-id="57900-318">Although it is easy to deploy a topology to a cluster, in some cases, you may need to test a topology locally.</span></span> <span data-ttu-id="57900-319">Pomocí následujících kroků spustit a otestovat topologie příklad v tomto kurzu místně ve vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="57900-319">Use the following steps to run and test the example topology in this tutorial locally in your development environment.</span></span>

> [!WARNING]
> <span data-ttu-id="57900-320">Místní testování funguje výhradně u basic, C# – pouze topologie.</span><span class="sxs-lookup"><span data-stu-id="57900-320">Local testing only works for basic, C#-only topologies.</span></span> <span data-ttu-id="57900-321">Nemůžete použít místní testování pro hybridní topologie nebo topologie, které používají víc datových proudů.</span><span class="sxs-lookup"><span data-stu-id="57900-321">You cannot use local testing for hybrid topologies or topologies that use multiple streams.</span></span>

1. <span data-ttu-id="57900-322">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="57900-322">In **Solution Explorer**, right-click the project, and select **Properties**.</span></span> <span data-ttu-id="57900-323">Ve vlastnostech projektu změnit **výstupní typ** k **konzolové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="57900-323">In the project properties, change the **Output type** to **Console Application**.</span></span>

    ![Snímek obrazovky vlastností projektu, se zvýrazněným typem výstupu](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

   > [!NOTE]
   > <span data-ttu-id="57900-325">Mějte na paměti, chcete-li změnit **výstupní typ** zpět na **knihovny tříd** před nasazením topologie do clusteru.</span><span class="sxs-lookup"><span data-stu-id="57900-325">Remember to change the **Output type** back to **Class Library** before you deploy the topology to a cluster.</span></span>

2. <span data-ttu-id="57900-326">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak vyberte **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="57900-326">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span> <span data-ttu-id="57900-327">Vyberte **třída**a zadejte **LocalTest.cs** jako název třídy.</span><span class="sxs-lookup"><span data-stu-id="57900-327">Select **Class**, and enter **LocalTest.cs** as the class name.</span></span> <span data-ttu-id="57900-328">Nakonec klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="57900-328">Finally, click **Add**.</span></span>

3. <span data-ttu-id="57900-329">Otevřete **LocalTest.cs**a přidejte následující **pomocí** příkaz v horní části:</span><span class="sxs-lookup"><span data-stu-id="57900-329">Open **LocalTest.cs**, and add the following **using** statement at the top:</span></span>

    ```csharp
    using Microsoft.SCP;
    ```

4. <span data-ttu-id="57900-330">Použít následující kód jako obsah **LocalTest** třídy:</span><span class="sxs-lookup"><span data-stu-id="57900-330">Use the following code as the contents of the **LocalTest** class:</span></span>

    ```csharp
    // Drives the topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test the spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of the spout, using the local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext to persist the data stream to file
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test the splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set the data stream to the data created by the spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test the counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set the data stream to the data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    <span data-ttu-id="57900-331">Pozorně si přečíst komentáře kódu.</span><span class="sxs-lookup"><span data-stu-id="57900-331">Take a moment to read through the code comments.</span></span> <span data-ttu-id="57900-332">Tento kód používá **LocalContext** spuštění součásti v vývojového prostředí a přetrvává datový proud mezi součástmi k textovým souborům na místní disk.</span><span class="sxs-lookup"><span data-stu-id="57900-332">This code uses **LocalContext** to run the components in the development environment, and it persists the data stream between components to text files on the local drive.</span></span>

1. <span data-ttu-id="57900-333">Otevřete **Program.cs**a přidejte následující **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="57900-333">Open **Program.cs**, and add the following to the **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize the runtime
    SCPRuntime.Initialize();

    //If we are not running under the local context, throw an error
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

2. <span data-ttu-id="57900-334">Uložte změny a pak klikněte na tlačítko **F5** nebo vyberte **ladění** > **spustit ladění** a spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="57900-334">Save the changes, and then click **F5** or select **Debug** > **Start Debugging** to start the project.</span></span> <span data-ttu-id="57900-335">Okno konzoly by měla zobrazovat a stavu protokolu jako průběhu testů.</span><span class="sxs-lookup"><span data-stu-id="57900-335">A console window should appear, and log status as the tests progress.</span></span> <span data-ttu-id="57900-336">Když **dokončení testů** se zobrazí, stisknutím libovolné klávesy zavřete toto okno.</span><span class="sxs-lookup"><span data-stu-id="57900-336">When **Tests finished** appears, press any key to close the window.</span></span>

3. <span data-ttu-id="57900-337">Použití **Průzkumníka Windows** vyhledejte adresář, který obsahuje projektu.</span><span class="sxs-lookup"><span data-stu-id="57900-337">Use **Windows Explorer** to locate the directory that contains your project.</span></span> <span data-ttu-id="57900-338">Příklad: **C:\Users\<vaše_uživatelské_jméno > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**.</span><span class="sxs-lookup"><span data-stu-id="57900-338">For example: **C:\Users\<your_user_name>\Documents\Visual Studio 2013\Projects\WordCount\WordCount**.</span></span> <span data-ttu-id="57900-339">V tomto adresáři otevřete **Bin**a potom klikněte na **ladění**.</span><span class="sxs-lookup"><span data-stu-id="57900-339">In this directory, open **Bin**, and then click **Debug**.</span></span> <span data-ttu-id="57900-340">Textové soubory, které byly vytvořeny při testy došlo měli vidět: sentences.txt, counter.txt a splitter.txt.</span><span class="sxs-lookup"><span data-stu-id="57900-340">You should see the text files that were produced when the tests ran: sentences.txt, counter.txt, and splitter.txt.</span></span> <span data-ttu-id="57900-341">Otevřete každý textový soubor a kontrolovat data.</span><span class="sxs-lookup"><span data-stu-id="57900-341">Open each text file and inspect the data.</span></span>

   > [!NOTE]
   > <span data-ttu-id="57900-342">Řetězec dat trvá jako pole desetinných míst v těchto souborech.</span><span class="sxs-lookup"><span data-stu-id="57900-342">String data persists as an array of decimal values in these files.</span></span> <span data-ttu-id="57900-343">Například \[[97,103,111]] v **splitter.txt** soubor je slovo *a*.</span><span class="sxs-lookup"><span data-stu-id="57900-343">For example, \[[97,103,111]] in the **splitter.txt** file is the word *and*.</span></span>

> [!NOTE]
> <span data-ttu-id="57900-344">Nastavte **typ projektu** zpět na **knihovny tříd** před nasazením Storm v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="57900-344">Be sure to set the **Project type** back to **Class Library** before deploying to a Storm on HDInsight cluster.</span></span>

### <a name="log-information"></a><span data-ttu-id="57900-345">Informace o protokolu</span><span class="sxs-lookup"><span data-stu-id="57900-345">Log information</span></span>

<span data-ttu-id="57900-346">Můžete snadno protokolovat informace ze součásti vaší topologie pomocí `Context.Logger`.</span><span class="sxs-lookup"><span data-stu-id="57900-346">You can easily log information from your topology components by using `Context.Logger`.</span></span> <span data-ttu-id="57900-347">Následující příklad vytvoří položku informační protokolu:</span><span class="sxs-lookup"><span data-stu-id="57900-347">For example, the following creates an informational log entry:</span></span>

```csharp
Context.Logger.Info("Component started");
```

<span data-ttu-id="57900-348">Zaznamenané informace lze zobrazit z **protokol služby Hadoop**, který se nachází v **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="57900-348">Logged information can be viewed from the **Hadoop Service Log**, which is found in **Server Explorer**.</span></span> <span data-ttu-id="57900-349">Rozbalte položku pro váš cluster Storm v HDInsight a pak rozbalte **protokol služby Hadoop**.</span><span class="sxs-lookup"><span data-stu-id="57900-349">Expand the entry for your Storm on HDInsight cluster, and then expand **Hadoop Service Log**.</span></span> <span data-ttu-id="57900-350">Nakonec vyberte soubor protokolu, chcete-li zobrazit.</span><span class="sxs-lookup"><span data-stu-id="57900-350">Finally, select the log file to view.</span></span>

> [!NOTE]
> <span data-ttu-id="57900-351">Protokoly jsou uloženy v účtu úložiště Azure, který je používán clusteru.</span><span class="sxs-lookup"><span data-stu-id="57900-351">The logs are stored in the Azure storage account that is used by your cluster.</span></span> <span data-ttu-id="57900-352">K zobrazení protokolů v sadě Visual Studio, musíte se přihlásit k předplatnému Azure, který vlastní účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="57900-352">To view the logs in Visual Studio, you must sign in to the Azure subscription that owns the storage account.</span></span>

### <a name="view-error-information"></a><span data-ttu-id="57900-353">Informace o chybě zobrazení</span><span class="sxs-lookup"><span data-stu-id="57900-353">View error information</span></span>

<span data-ttu-id="57900-354">Chcete-li zobrazit chyby, ke kterým došlo v spuštěné topologie, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="57900-354">To view errors that have occurred in a running topology, use the following steps:</span></span>

1. <span data-ttu-id="57900-355">Z **Průzkumníka serveru**, klikněte pravým tlačítkem myši cluster Storm v HDInsight a vyberte **topologie Storm zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="57900-355">From **Server Explorer**, right-click the Storm on HDInsight cluster, and select **View Storm topologies**.</span></span>

2. <span data-ttu-id="57900-356">Pro **Spout** a **Bolts**, **poslední chyba** sloupec obsahuje informace o poslední chybě.</span><span class="sxs-lookup"><span data-stu-id="57900-356">For the **Spout** and **Bolts**, the **Last Error** column contains information on the last error.</span></span>

3. <span data-ttu-id="57900-357">Vyberte **Spout Id** nebo **Bolt Id** pro součást, která obsahuje chybu uvedené.</span><span class="sxs-lookup"><span data-stu-id="57900-357">Select the **Spout Id** or **Bolt Id** for the component that has an error listed.</span></span> <span data-ttu-id="57900-358">Na stránce podrobností, který je zobrazený, další chyba je uvedena informace ve **chyby** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="57900-358">On the details page that is displayed, additional error information is listed in the **Errors** section at the bottom of the page.</span></span>

4. <span data-ttu-id="57900-359">Chcete-li získat další informace, vyberte **Port** z **vykonavatelů** části stránky, naleznete v protokolu Storm pracovního procesu v posledních několika minutách.</span><span class="sxs-lookup"><span data-stu-id="57900-359">To obtain more information, select a **Port** from the **Executors** section of the page, to see the Storm worker log for the last few minutes.</span></span>

### <a name="errors-submitting-topologies"></a><span data-ttu-id="57900-360">Chyby odesílání topologie</span><span class="sxs-lookup"><span data-stu-id="57900-360">Errors submitting topologies</span></span>

<span data-ttu-id="57900-361">Pokud narazíte na chyby odesílání topologie do HDInsight, můžete nějakého najít protokoly pro serverové komponenty, které zpracovávají topologie odeslání na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="57900-361">If you encounter errors submitting a topology to HDInsight, you can find logs for the server-side components that handle topology submission on your HDInsight cluster.</span></span> <span data-ttu-id="57900-362">Chcete-li získat tyto protokoly, použijte následující příkaz z příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="57900-362">To retrieve these logs, use the following command from a command line:</span></span>

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

<span data-ttu-id="57900-363">Nahraďte __sshuser__ pomocí uživatelského účtu SSH pro cluster.</span><span class="sxs-lookup"><span data-stu-id="57900-363">Replace __sshuser__ with the SSH user account for the cluster.</span></span> <span data-ttu-id="57900-364">Nahraďte __clustername__ s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="57900-364">Replace __clustername__ with the name of the HDInsight cluster.</span></span> <span data-ttu-id="57900-365">Další informace o používání `scp` a `ssh` s HDInsight, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="57900-365">For more information on using `scp` and `ssh` with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="57900-366">Odesílání může selhat z několika důvodů:</span><span class="sxs-lookup"><span data-stu-id="57900-366">Submissions can fail for multiple reasons:</span></span>

* <span data-ttu-id="57900-367">JDK není nainstalována nebo není v cestě.</span><span class="sxs-lookup"><span data-stu-id="57900-367">JDK is not installed or is not in the path.</span></span>
* <span data-ttu-id="57900-368">Požadované závislosti Java nejsou v odesílání zahrnuté.</span><span class="sxs-lookup"><span data-stu-id="57900-368">Required Java dependencies are not included in the submission.</span></span>
* <span data-ttu-id="57900-369">Nekompatibilní závislosti.</span><span class="sxs-lookup"><span data-stu-id="57900-369">Incompatible dependencies.</span></span>
* <span data-ttu-id="57900-370">Duplicitní názvy topologie.</span><span class="sxs-lookup"><span data-stu-id="57900-370">Duplicate topology names.</span></span>

<span data-ttu-id="57900-371">Pokud `hdinsight-scpwebapi.out` protokol obsahuje `FileNotFoundException`, může to být způsobeno následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="57900-371">If the `hdinsight-scpwebapi.out` log contains a `FileNotFoundException`, this might be caused by the following conditions:</span></span>

* <span data-ttu-id="57900-372">Sadu JDK není v cestě na vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="57900-372">The JDK is not in the path on the development environment.</span></span> <span data-ttu-id="57900-373">Ověřte, zda je nainstalován sadu JDK v vývojového prostředí a že `%JAVA_HOME%/bin` v cestě.</span><span class="sxs-lookup"><span data-stu-id="57900-373">Verify that the JDK is installed in the development environment, and that `%JAVA_HOME%/bin` is in the path.</span></span>
* <span data-ttu-id="57900-374">Chybí závislost Java.</span><span class="sxs-lookup"><span data-stu-id="57900-374">You are missing a Java dependency.</span></span> <span data-ttu-id="57900-375">Ujistěte se, že všechny požadované .jar soubory jsou včetně jako součást odesílání.</span><span class="sxs-lookup"><span data-stu-id="57900-375">Make sure you are including any required .jar files as part of the submission.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57900-376">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57900-376">Next steps</span></span>

<span data-ttu-id="57900-377">Příklad zpracování dat ze služby Event Hubs naleznete v části [zpracovat události z Azure Event Hubs se Storm v HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="57900-377">For an example of processing data from Event Hubs, see [Process events from Azure Event Hubs with Storm on HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).</span></span>

<span data-ttu-id="57900-378">Příklad topologie C#, která rozdělí datový proud do různých datových proudů, naleznete v části [C# Storm příklad](https://github.com/Blackmist/csharp-storm-example).</span><span class="sxs-lookup"><span data-stu-id="57900-378">For an example of a C# topology that splits stream data into multiple streams, see [C# Storm example](https://github.com/Blackmist/csharp-storm-example).</span></span>

<span data-ttu-id="57900-379">Chcete-li zjistit další informace o vytváření topologie C#, přečtěte si téma [Githubu](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).</span><span class="sxs-lookup"><span data-stu-id="57900-379">To discover more information about creating C# topologies, see [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).</span></span>

<span data-ttu-id="57900-380">Pro další způsoby, jak pracovat s HDInsight a další Storm v HDInsight ukázky najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="57900-380">For more ways to work with HDInsight and more Storm on HDInsight samples, see the following documents:</span></span>

<span data-ttu-id="57900-381">**Microsoft SCP.NET**</span><span class="sxs-lookup"><span data-stu-id="57900-381">**Microsoft SCP.NET**</span></span>

* [<span data-ttu-id="57900-382">Průvodce programováním spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="57900-382">SCP programming guide</span></span>](hdinsight-storm-scp-programming-guide.md)

<span data-ttu-id="57900-383">**Apache Storm v HDInsight**</span><span class="sxs-lookup"><span data-stu-id="57900-383">**Apache Storm on HDInsight**</span></span>

* [<span data-ttu-id="57900-384">Nasazení a monitorování topologie s Apache Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="57900-384">Deploy and monitor topologies with Apache Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology.md)
* [<span data-ttu-id="57900-385">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="57900-385">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

<span data-ttu-id="57900-386">**Apache Hadoop v HDInsight**</span><span class="sxs-lookup"><span data-stu-id="57900-386">**Apache Hadoop on HDInsight**</span></span>

* [<span data-ttu-id="57900-387">Použijte Hive s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="57900-387">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="57900-388">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="57900-388">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="57900-389">Používání nástroje MapReduce s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="57900-389">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="57900-390">**Apache HBase v HDInsight**</span><span class="sxs-lookup"><span data-stu-id="57900-390">**Apache HBase on HDInsight**</span></span>

* [<span data-ttu-id="57900-391">Začínáme s HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="57900-391">Getting started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started.md)
