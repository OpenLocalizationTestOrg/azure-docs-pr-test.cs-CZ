---
title: "Zpracování událostí ze služby Event Hubs se Storm - Azure HDInsight | Microsoft Docs"
description: "Informace o zpracování dat z Azure Event Hubs s topologií C# Storm pomocí nástrojů HDInsight pro Visual Studio vytvořit v sadě Visual Studio."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 67f9d08c-eea0-401b-952b-db765655dad0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 4b6fd87b057d93175d3ef284238d77be3bdde61d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a><span data-ttu-id="61485-103">Zpracování událostí z Azure Event Hubs se Storm v HDInsight (C#)</span><span class="sxs-lookup"><span data-stu-id="61485-103">Process events from Azure Event Hubs with Storm on HDInsight (C#)</span></span>

<span data-ttu-id="61485-104">Naučte se pracovat s Azure Event Hubs z Apache Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="61485-104">Learn how to work with Azure Event Hubs from Apache Storm on HDInsight.</span></span> <span data-ttu-id="61485-105">Tento dokument používá ke čtení a zápisu dat z centra Evbent topologie C# Storm</span><span class="sxs-lookup"><span data-stu-id="61485-105">This document uses a C# Storm topology to read and write data from Evbent Hubs</span></span>

> [!NOTE]
> <span data-ttu-id="61485-106">Java verzi tohoto projektu, naleznete v části [zpracovat události z Azure Event Hubs se Storm v HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="61485-106">For a Java version of this project, see [Process events from Azure Event Hubs with Storm on HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span></span>

## <a name="scpnet"></a><span data-ttu-id="61485-107">SCP.NET</span><span class="sxs-lookup"><span data-stu-id="61485-107">SCP.NET</span></span>

<span data-ttu-id="61485-108">Kroky v tomto dokumentu používají SCP.NET balíčku NuGet, který usnadňuje vytvoření topologie C# a součásti pro použití s Storm v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="61485-108">The steps in this document use SCP.NET, a NuGet package that makes it easy to create C# topologies and components for use with Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61485-109">Pokud kroky v tomto dokumentu se spoléhají na vývojového prostředí systému Windows pomocí sady Visual Studio, zkompilovaný projekt lze odeslat do Storm v clusteru HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="61485-109">While the steps in this document rely on a Windows development environment with Visual Studio, the compiled project can be submitted to a Storm on HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="61485-110">Clustery se systémem Linux vytvořené po 28 říjnu 2016 se podporují jenom SCP.NET topologie.</span><span class="sxs-lookup"><span data-stu-id="61485-110">Only Linux-based clusters created after October 28, 2016, support SCP.NET topologies.</span></span>

<span data-ttu-id="61485-111">HDInsight 3.4 a větší použití Mono spouštění topologií C#.</span><span class="sxs-lookup"><span data-stu-id="61485-111">HDInsight 3.4 and greater use Mono to run C# topologies.</span></span> <span data-ttu-id="61485-112">V příkladu v tomto dokumentu pracuje s HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="61485-112">The example used in this document works with HDInsight 3.6.</span></span> <span data-ttu-id="61485-113">Pokud máte v plánu na vytváření řešení .NET pro HDInsight, podívejte se [Mono kompatibility](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu potenciální nekompatibility.</span><span class="sxs-lookup"><span data-stu-id="61485-113">If you plan on creating your own .NET solutions for HDInsight, check the [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document for potential incompatibilities.</span></span>

### <a name="cluster-versioning"></a><span data-ttu-id="61485-114">Správa verzí clusteru</span><span class="sxs-lookup"><span data-stu-id="61485-114">Cluster versioning</span></span>

<span data-ttu-id="61485-115">Balíček Microsoft.SCP.Net.SDK NuGet, které používáte pro svůj projekt se musí shodovat hlavní verzi Storm v HDInsight nainstalována.</span><span class="sxs-lookup"><span data-stu-id="61485-115">The Microsoft.SCP.Net.SDK NuGet package you use for your project must match the major version of Storm installed on HDInsight.</span></span> <span data-ttu-id="61485-116">HDInsight verze 3.5 a 3.6 používat Storm 1.x, musíte použít verzi 1.0.x.x SCP.NET s těchto clusterů.</span><span class="sxs-lookup"><span data-stu-id="61485-116">HDInsight versions 3.5 and 3.6 use Storm 1.x, so you must use SCP.NET version 1.0.x.x with these clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61485-117">V příkladu v tomto dokumentu očekává HDInsight 3.5 nebo 3,6 clusteru.</span><span class="sxs-lookup"><span data-stu-id="61485-117">The example in this document expects an HDInsight 3.5 or 3.6 cluster.</span></span>
>
> <span data-ttu-id="61485-118">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="61485-118">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="61485-119">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="61485-119">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="61485-120">Topologie C# musí být také .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="61485-120">C# topologies must also target .NET 4.5.</span></span>

## <a name="how-to-work-with-event-hubs"></a><span data-ttu-id="61485-121">Jak pracovat s Event Hubs</span><span class="sxs-lookup"><span data-stu-id="61485-121">How to work with Event Hubs</span></span>

<span data-ttu-id="61485-122">Společnost Microsoft poskytuje sadu komponent v jazyce Java, které lze použít ke komunikaci se službou Event Hubs od topologie Storm.</span><span class="sxs-lookup"><span data-stu-id="61485-122">Microsoft provides a set of Java components that can be used to communicate with Event Hubs from a Storm topology.</span></span> <span data-ttu-id="61485-123">Můžete najít soubor Java archivu (JAR), který obsahuje HDInsight 3.6 kompatibilní verze těchto součástí v [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="61485-123">You can find the Java archive (JAR) file that contains an HDInsight 3.6 compatible version of these components at [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61485-124">Při součásti jsou napsané v jazyce Java, je můžete snadno použít z topologie C#.</span><span class="sxs-lookup"><span data-stu-id="61485-124">While the components are written in Java, you can easily use them from a C# topology.</span></span>

<span data-ttu-id="61485-125">V tomto příkladu se používají následující součásti:</span><span class="sxs-lookup"><span data-stu-id="61485-125">The following components are used in this example:</span></span>

* <span data-ttu-id="61485-126">__EventHubSpout__: čte data ze služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="61485-126">__EventHubSpout__: Reads data from Event Hubs.</span></span>
* <span data-ttu-id="61485-127">__EventHubBolt__: zapisuje data do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="61485-127">__EventHubBolt__: Writes data to Event Hubs.</span></span>
* <span data-ttu-id="61485-128">__EventHubSpoutConfig__: slouží ke konfiguraci EventHubSpout.</span><span class="sxs-lookup"><span data-stu-id="61485-128">__EventHubSpoutConfig__: Used to configure EventHubSpout.</span></span>
* <span data-ttu-id="61485-129">__EventHubBoltConfig__: slouží ke konfiguraci EventHubBolt.</span><span class="sxs-lookup"><span data-stu-id="61485-129">__EventHubBoltConfig__: Used to configure EventHubBolt.</span></span>

### <a name="example-spout-usage"></a><span data-ttu-id="61485-130">Příklad použití funkcí spout</span><span class="sxs-lookup"><span data-stu-id="61485-130">Example spout usage</span></span>

<span data-ttu-id="61485-131">SCP.NET poskytuje metody pro přidání EventHubSpout do vaší topologie.</span><span class="sxs-lookup"><span data-stu-id="61485-131">SCP.NET provides methods for adding an EventHubSpout to your topology.</span></span> <span data-ttu-id="61485-132">Tyto metody usnadňují přidat spout než použití obecné metody pro přidání součásti Java.</span><span class="sxs-lookup"><span data-stu-id="61485-132">These methods make it easier to add a spout than using the generic methods for adding a Java component.</span></span> <span data-ttu-id="61485-133">Následující příklad ukazuje, jak vytvořit spout pomocí __SetEventHubSpout__ a **EventHubSpoutConfig** metody poskytované SCP.NET:</span><span class="sxs-lookup"><span data-stu-id="61485-133">The following example demonstrates how to create a spout by using the __SetEventHubSpout__ and **EventHubSpoutConfig** methods provided by SCP.NET:</span></span>

```csharp
 topologyBuilder.SetEventHubSpout(
    "EventHubSpout",
    new EventHubSpoutConfig(
        ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"],
        ConfigurationManager.AppSettings["EventHubSharedAccessKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        ConfigurationManager.AppSettings["EventHubEntityPath"],
        eventHubPartitions),
    eventHubPartitions);
```

<span data-ttu-id="61485-134">Předchozí příklad vytvoří novou funkcí spout součást s názvem __EventHubSpout__a nakonfiguruje ho ke komunikaci s centra událostí.</span><span class="sxs-lookup"><span data-stu-id="61485-134">The previous example creates a new spout component named __EventHubSpout__, and configures it to communicate with an event hub.</span></span> <span data-ttu-id="61485-135">Pomocný parametr paralelismus pro součást je nastavena na počet oddílů události rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="61485-135">The parallelism hint for the component is set to the number of partitions in the event hub.</span></span> <span data-ttu-id="61485-136">Toto nastavení umožňuje Storm k vytvoření instance součásti pro každý oddíl.</span><span class="sxs-lookup"><span data-stu-id="61485-136">This setting allows Storm to create an instance of the component for each partition.</span></span>

### <a name="example-bolt-usage"></a><span data-ttu-id="61485-137">Příklad použití bolt</span><span class="sxs-lookup"><span data-stu-id="61485-137">Example bolt usage</span></span>

<span data-ttu-id="61485-138">Použití **JavaComponmentConstructor** metodu pro vytvoření instance bolt.</span><span class="sxs-lookup"><span data-stu-id="61485-138">Use the **JavaComponmentConstructor** method to create an instance of the bolt.</span></span> <span data-ttu-id="61485-139">Následující příklad ukazuje, jak vytvořit a nakonfigurovat novou instanci třídy **EventHubBolt**:</span><span class="sxs-lookup"><span data-stu-id="61485-139">The following example demonstrates how to create and configure a new instance of the **EventHubBolt**:</span></span>

```csharp
// Java construcvtor for the Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set the bolt to subscribe to data from the spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]
> <span data-ttu-id="61485-140">Tento příklad používá Clojure výrazu předaného jako řetězec, místo použití **JavaComponentConstructor** vytvořit **EventHubBoltConfig**, jako v příkladu funkcí spout.</span><span class="sxs-lookup"><span data-stu-id="61485-140">This example uses a Clojure expression passed as a string, instead of using **JavaComponentConstructor** to create an **EventHubBoltConfig**, as the spout example did.</span></span> <span data-ttu-id="61485-141">Buď metoda funguje.</span><span class="sxs-lookup"><span data-stu-id="61485-141">Either method works.</span></span> <span data-ttu-id="61485-142">Použijte metodu, která funguje pro vás nejlepší.</span><span class="sxs-lookup"><span data-stu-id="61485-142">Use the method that feels best to you.</span></span>

## <a name="download-the-completed-project"></a><span data-ttu-id="61485-143">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="61485-143">Download the completed project</span></span>

<span data-ttu-id="61485-144">Můžete stáhnout úplnou verzi vytvořených v tomto kurzu z projektů [Githubu](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="61485-144">You can download a complete version of the project created in this tutorial from [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span> <span data-ttu-id="61485-145">Však stále musíte zadat nastavení konfigurace pomocí kroků v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="61485-145">However, you still need to provide configuration settings by following the steps in this tutorial.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="61485-146">Požadavky</span><span class="sxs-lookup"><span data-stu-id="61485-146">Prerequisites</span></span>

* <span data-ttu-id="61485-147">[Apache Storm v HDInsight clusteru verze 3.5 nebo 3.6](hdinsight-apache-storm-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="61485-147">An [Apache Storm on HDInsight cluster version 3.5 or 3.6](hdinsight-apache-storm-tutorial-get-started.md).</span></span>

    > [!WARNING]
    > <span data-ttu-id="61485-148">V příkladu v tomto dokumentu vyžaduje Storm v HDInsight verze 3.5 nebo 3.6.</span><span class="sxs-lookup"><span data-stu-id="61485-148">The example used in this document requires Storm on HDInsight version 3.5 or 3.6.</span></span> <span data-ttu-id="61485-149">To nefunguje se staršími verzemi HDInsight, kvůli nejnovější změny název třídy.</span><span class="sxs-lookup"><span data-stu-id="61485-149">This does not work with older versions of HDInsight, due to breaking class name changes.</span></span> <span data-ttu-id="61485-150">Verzi tohoto příkladu, který funguje s starší clustery, naleznete v části [Githubu](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span><span class="sxs-lookup"><span data-stu-id="61485-150">For a version of this example that works with older clusters, see [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span></span>

* <span data-ttu-id="61485-151">[Centra událostí Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="61485-151">An [Azure event hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

* <span data-ttu-id="61485-152">[Sady Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="61485-152">The [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>

* <span data-ttu-id="61485-153">[Nástroje HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="61485-153">The [HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

* <span data-ttu-id="61485-154">Java JDK 1.8 nebo později na vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="61485-154">Java JDK 1.8 or later on your development environment.</span></span> <span data-ttu-id="61485-155">Stahování JDK jsou k dispozici na [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="61485-155">JDK downloads are available from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>

  * <span data-ttu-id="61485-156">**JAVA_HOME** proměnnou prostředí musí odkazovat na adresář, který obsahuje Java.</span><span class="sxs-lookup"><span data-stu-id="61485-156">The **JAVA_HOME** environment variable must point to the directory that contains Java.</span></span>
  * <span data-ttu-id="61485-157">**%JAVA_HOME%/bin** adresář se musí nacházet v cestě.</span><span class="sxs-lookup"><span data-stu-id="61485-157">The **%JAVA_HOME%/bin** directory must be in the path.</span></span>

## <a name="download-the-event-hubs-components"></a><span data-ttu-id="61485-158">Stáhněte si komponenty služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="61485-158">Download the Event Hubs components</span></span>

<span data-ttu-id="61485-159">Stáhnout spout Event Hubs a funkce bolt z komponenty [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="61485-159">Download the Event Hubs spout and bolt component from [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

<span data-ttu-id="61485-160">Vytvořte adresář s názvem `eventhubspout`a uložte soubor do adresáře.</span><span class="sxs-lookup"><span data-stu-id="61485-160">Create a directory named `eventhubspout`, and save the file into the directory.</span></span>

## <a name="configure-event-hubs"></a><span data-ttu-id="61485-161">Konfigurace služby Event Hubs</span><span class="sxs-lookup"><span data-stu-id="61485-161">Configure Event Hubs</span></span>

<span data-ttu-id="61485-162">Event Hubs je zdroj dat pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="61485-162">Event Hubs is the data source for this example.</span></span> <span data-ttu-id="61485-163">Použijte informace v části "Vytvoření centra událostí" [Začínáme se službou Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="61485-163">Use the information in the "Create an event hub" section of [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

1. <span data-ttu-id="61485-164">Po vytvoření centra událostí, zobrazení **EventHub** okno ve službě Azure portálu a vyberte možnost **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="61485-164">After the event hub has been created, view the **EventHub** blade in the Azure portal, and select **Shared access policies**.</span></span> <span data-ttu-id="61485-165">Vyberte **+ přidat** přidat následující zásady:</span><span class="sxs-lookup"><span data-stu-id="61485-165">Select **+ Add** to add the following policies:</span></span>

   | <span data-ttu-id="61485-166">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="61485-166">Name</span></span> | <span data-ttu-id="61485-167">Oprávnění</span><span class="sxs-lookup"><span data-stu-id="61485-167">Permissions</span></span> |
   | --- | --- |
   | <span data-ttu-id="61485-168">Modul pro zápis</span><span class="sxs-lookup"><span data-stu-id="61485-168">writer</span></span> |<span data-ttu-id="61485-169">Odeslat</span><span class="sxs-lookup"><span data-stu-id="61485-169">Send</span></span> |
   | <span data-ttu-id="61485-170">Čtecí modul</span><span class="sxs-lookup"><span data-stu-id="61485-170">reader</span></span> |<span data-ttu-id="61485-171">Naslouchání</span><span class="sxs-lookup"><span data-stu-id="61485-171">Listen</span></span> |

    ![Okno zásady přístupu – snímek obrazovky sdílené složky](./media/hdinsight-storm-develop-csharp-event-hub-topology/sas.png)

2. <span data-ttu-id="61485-173">Vyberte **čtečky** a **zapisovače** zásady.</span><span class="sxs-lookup"><span data-stu-id="61485-173">Select the **reader** and **writer** policies.</span></span> <span data-ttu-id="61485-174">Zkopírujte a uložte hodnotu primárního klíče pro obě zásady, jako jsou tyto hodnoty použít později.</span><span class="sxs-lookup"><span data-stu-id="61485-174">Copy and save the primary key value for both policies, as these values are used later.</span></span>

## <a name="configure-the-eventhubwriter"></a><span data-ttu-id="61485-175">Konfigurace EventHubWriter</span><span class="sxs-lookup"><span data-stu-id="61485-175">Configure the EventHubWriter</span></span>

1. <span data-ttu-id="61485-176">Pokud jste ještě nenainstalovali nejnovější verzi nástroje HDInsight pro Visual Studio, najdete v části [Začínáme pomocí nástrojů HDInsight pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="61485-176">If you have not already installed the latest version of the HDInsight tools for Visual Studio, see [Get started using HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="61485-177">Stáhnout z [eventhub. storm hybridní](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="61485-177">Download the solution from [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span>

3. <span data-ttu-id="61485-178">V **EventHubWriter** projekt, otevřete **App.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="61485-178">In the **EventHubWriter** project, open the **App.config** file.</span></span> <span data-ttu-id="61485-179">Pomocí informací z centra událostí, které jste dříve nakonfigurovali vyplnit hodnotu pro následující klíče:</span><span class="sxs-lookup"><span data-stu-id="61485-179">Use the information from the event hub that you configured earlier to fill in the value for the following keys:</span></span>

   | <span data-ttu-id="61485-180">Klíč</span><span class="sxs-lookup"><span data-stu-id="61485-180">Key</span></span> | <span data-ttu-id="61485-181">Hodnota</span><span class="sxs-lookup"><span data-stu-id="61485-181">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="61485-182">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="61485-182">EventHubPolicyName</span></span> |<span data-ttu-id="61485-183">Zapisovač (Pokud jste použili jiný název pro zásady pomocí *odeslat* oprávnění, použijte místo toho.)</span><span class="sxs-lookup"><span data-stu-id="61485-183">writer (If you used a different name for the policy with *Send* permission, use it instead.)</span></span> |
   | <span data-ttu-id="61485-184">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="61485-184">EventHubPolicyKey</span></span> |<span data-ttu-id="61485-185">Klíč pro zásady zapisovače.</span><span class="sxs-lookup"><span data-stu-id="61485-185">The key for the writer policy.</span></span> |
   | <span data-ttu-id="61485-186">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="61485-186">EventHubNamespace</span></span> |<span data-ttu-id="61485-187">Obor názvů, který obsahuje Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="61485-187">The namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="61485-188">EventHubName</span><span class="sxs-lookup"><span data-stu-id="61485-188">EventHubName</span></span> |<span data-ttu-id="61485-189">Název centra událostí.</span><span class="sxs-lookup"><span data-stu-id="61485-189">Your event hub name.</span></span> |
   | <span data-ttu-id="61485-190">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="61485-190">EventHubPartitionCount</span></span> |<span data-ttu-id="61485-191">Počet oddílů v Centru událostí.</span><span class="sxs-lookup"><span data-stu-id="61485-191">The number of partitions in your event hub.</span></span> |

4. <span data-ttu-id="61485-192">Uložte a zavřete **App.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="61485-192">Save and close the **App.config** file.</span></span>

## <a name="configure-the-eventhubreader"></a><span data-ttu-id="61485-193">Konfigurace EventHubReader</span><span class="sxs-lookup"><span data-stu-id="61485-193">Configure the EventHubReader</span></span>

1. <span data-ttu-id="61485-194">Otevřete **EventHubReader** projektu.</span><span class="sxs-lookup"><span data-stu-id="61485-194">Open the **EventHubReader** project.</span></span>

2. <span data-ttu-id="61485-195">Otevřete **App.config** souboru **EventHubReader**.</span><span class="sxs-lookup"><span data-stu-id="61485-195">Open the **App.config** file for the **EventHubReader**.</span></span> <span data-ttu-id="61485-196">Pomocí informací z centra událostí, které jste dříve nakonfigurovali vyplnit hodnotu pro následující klíče:</span><span class="sxs-lookup"><span data-stu-id="61485-196">Use the information from the event hub that you configured earlier to fill in the value for the following keys:</span></span>

   | <span data-ttu-id="61485-197">Klíč</span><span class="sxs-lookup"><span data-stu-id="61485-197">Key</span></span> | <span data-ttu-id="61485-198">Hodnota</span><span class="sxs-lookup"><span data-stu-id="61485-198">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="61485-199">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="61485-199">EventHubPolicyName</span></span> |<span data-ttu-id="61485-200">Čtečka (Pokud jste použili jiný název pro zásady pomocí *naslouchání* oprávnění, použijte místo toho.)</span><span class="sxs-lookup"><span data-stu-id="61485-200">reader (If you used a different name for the policy with *listen* permission, use it instead.)</span></span> |
   | <span data-ttu-id="61485-201">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="61485-201">EventHubPolicyKey</span></span> |<span data-ttu-id="61485-202">Klíč pro zásady čtečky.</span><span class="sxs-lookup"><span data-stu-id="61485-202">The key for the reader policy.</span></span> |
   | <span data-ttu-id="61485-203">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="61485-203">EventHubNamespace</span></span> |<span data-ttu-id="61485-204">Obor názvů, který obsahuje Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="61485-204">The namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="61485-205">EventHubName</span><span class="sxs-lookup"><span data-stu-id="61485-205">EventHubName</span></span> |<span data-ttu-id="61485-206">Název centra událostí.</span><span class="sxs-lookup"><span data-stu-id="61485-206">Your event hub name.</span></span> |
   | <span data-ttu-id="61485-207">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="61485-207">EventHubPartitionCount</span></span> |<span data-ttu-id="61485-208">Počet oddílů v Centru událostí.</span><span class="sxs-lookup"><span data-stu-id="61485-208">The number of partitions in your event hub.</span></span> |

3. <span data-ttu-id="61485-209">Uložte a zavřete **App.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="61485-209">Save and close the **App.config** file.</span></span>

## <a name="deploy-the-topologies"></a><span data-ttu-id="61485-210">Nasazení topologie</span><span class="sxs-lookup"><span data-stu-id="61485-210">Deploy the topologies</span></span>

1. <span data-ttu-id="61485-211">Z **Průzkumníku řešení**, klikněte pravým tlačítkem myši **EventHubReader** projektu a vyberte **odeslání do Storm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="61485-211">From **Solution Explorer**, right-click the **EventHubReader** project, and select **Submit to Storm on HDInsight**.</span></span>

    ![Snímek obrazovky řešení Explorer, se odeslat do Storm v HDInsight zvýrazněná](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. <span data-ttu-id="61485-213">Na **odeslání topologie** dialogové okno, vyberte vaše **Storm Cluster**.</span><span class="sxs-lookup"><span data-stu-id="61485-213">On the **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="61485-214">Rozbalte položku **další konfigurace**, vyberte **cesty k souborům Java**, vyberte **...** a vyberte adresář, který obsahuje soubor JAR, který jste předtím stáhli.</span><span class="sxs-lookup"><span data-stu-id="61485-214">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select the directory that contains the JAR file that you downloaded earlier.</span></span> <span data-ttu-id="61485-215">Nakonec klikněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="61485-215">Finally, click **Submit**.</span></span>

    ![Dialogové okno snímek obrazovky odeslání topologie](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. <span data-ttu-id="61485-217">Při odeslání topologii **prohlížeč topologie Storm** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="61485-217">When the topology has been submitted, the **Storm Topologies Viewer** appears.</span></span> <span data-ttu-id="61485-218">Chcete-li zobrazit informace o topologii, vyberte **EventHubReader** topologie v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="61485-218">To view information about the topology, select the **EventHubReader** topology in the left pane.</span></span>

    ![Snímek obrazovky prohlížeč topologie Storm](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. <span data-ttu-id="61485-220">Z **Průzkumníku řešení**, klikněte pravým tlačítkem myši **EventHubWriter** projektu a vyberte **odeslání do Storm v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="61485-220">From **Solution Explorer**, right-click the **EventHubWriter** project, and select **Submit to Storm on HDInsight**.</span></span>

5. <span data-ttu-id="61485-221">Na **odeslání topologie** dialogové okno, vyberte vaše **Storm Cluster**.</span><span class="sxs-lookup"><span data-stu-id="61485-221">On the **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="61485-222">Rozbalte položku **další konfigurace**, vyberte **cesty k souborům Java**, vyberte **...** a vyberte adresář, který obsahuje soubor JAR jste předtím stáhli.</span><span class="sxs-lookup"><span data-stu-id="61485-222">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select the directory that contains the JAR file you downloaded earlier.</span></span> <span data-ttu-id="61485-223">Nakonec klikněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="61485-223">Finally, click **Submit**.</span></span>

6. <span data-ttu-id="61485-224">Při odeslání topologii aktualizaci seznamu topologie v **prohlížeč topologie Storm** ověřit, jestli obou topologií běží na clusteru.</span><span class="sxs-lookup"><span data-stu-id="61485-224">When the topology has been submitted, refresh the topology list in the **Storm Topologies Viewer** to verify that both topologies are running on the cluster.</span></span>

7. <span data-ttu-id="61485-225">V **prohlížeč topologie Storm**, vyberte **EventHubReader** topologie.</span><span class="sxs-lookup"><span data-stu-id="61485-225">In **Storm Topologies Viewer**, select the **EventHubReader** topology.</span></span>

8. <span data-ttu-id="61485-226">Chcete-li otevřít komponentu shrnutí bolt, dvakrát klikněte na **LogBolt** součásti v diagramu.</span><span class="sxs-lookup"><span data-stu-id="61485-226">To open the component summary for the bolt, double-click the **LogBolt** component in the diagram.</span></span>

9. <span data-ttu-id="61485-227">V **vykonavatelů** vyberte jeden z odkazů v **Port** sloupce.</span><span class="sxs-lookup"><span data-stu-id="61485-227">In the **Executors** section, select one of the links in the **Port** column.</span></span> <span data-ttu-id="61485-228">Zobrazí se informace, které jsou zapsané podle součásti.</span><span class="sxs-lookup"><span data-stu-id="61485-228">This displays information logged by the component.</span></span> <span data-ttu-id="61485-229">Zaznamenané informace je podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="61485-229">The logged information is similar to the following text:</span></span>

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-the-topologies"></a><span data-ttu-id="61485-230">Zastavit uvedené topologie</span><span class="sxs-lookup"><span data-stu-id="61485-230">Stop the topologies</span></span>

<span data-ttu-id="61485-231">Pokud chcete zastavit uvedené topologie, vyberte každý topologii **prohlížeč topologie Storm**, pak klikněte na tlačítko **Kill**.</span><span class="sxs-lookup"><span data-stu-id="61485-231">To stop the topologies, select each topology in the **Storm Topology Viewer**, then click **Kill**.</span></span>

![Snímek obrazovky systému Storm topologie prohlížeč, s tlačítkem Kill](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a><span data-ttu-id="61485-233">Odstranění clusteru</span><span class="sxs-lookup"><span data-stu-id="61485-233">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="61485-234">Další kroky</span><span class="sxs-lookup"><span data-stu-id="61485-234">Next steps</span></span>

<span data-ttu-id="61485-235">V tomto dokumentu jste se naučili, jak používat spout Java Event Hubs a funkce bolt od topologie C# pro práci s daty v Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="61485-235">In this document, you have learned how to use the Java Event Hubs spout and bolt from a C# topology to work with data in Azure Event Hubs.</span></span> <span data-ttu-id="61485-236">Další informace o vytváření topologie C#, naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="61485-236">To learn more about creating C# topologies, see the following:</span></span>

* [<span data-ttu-id="61485-237">Vývoj topologie C# pro Apache Storm v HDInsight pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61485-237">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="61485-238">Průvodce programováním spojovací bod služby</span><span class="sxs-lookup"><span data-stu-id="61485-238">SCP programming guide</span></span>](hdinsight-storm-scp-programming-guide.md)
* [<span data-ttu-id="61485-239">Příklad topologií pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="61485-239">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
